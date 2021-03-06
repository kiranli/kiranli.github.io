MySQL_customer portfolio analysis

#MySQL用户画像建立

# 一、什么是用户画像
用户画像、即用户标签化，通过收集用户的社会属性、消费习惯、偏好特征等各个维度的数据，对用户或者产品特征属性进行刻画，并对这些特征进行分析、统计以挖掘潜在的信息，从而抽象出一个用户的信息全貌。用户画像可看做是企业应用大数据的根基，是定向广告投放于个性化推荐的前置条件，为数据驱动运营奠定了基础。

用户画像的核心目的是了解用户，画像是真实用户的虚拟代表，是建立在一系列真实数据之上的目标用户模型。

# 二、用户画像模型及应用场景

1、基于统计类的标签：例如用户的年龄、性别、所在城市、近7天活跃时长、近7天活跃天数等可以从用户的注册数据、访问数据、消费类数据中统计得出。改标签构成了用户画像的基础。

2、基于规则类标签：该类标签基于用户行为及确定的规则产生。例如对平台上“消费活跃用户”的定义为近30天交易次数不少于2次的用户。

3、基于挖掘类的标签：该类标签通过数据挖掘产生，对于用户的某些属性或者行为进行预测。

## 1.用户属性标签表

```mysql

DROP TABLE user_profile_basic_informatin_01;
CREATE TABLE user_profile_basic_informatin_01
AS
SELECT email,mobile ,orderno,memberid,MIN(DATE(CreateDataTime)) AS first_order_time,MAX(DATE(CreateDataTime)) AS last_order_time,
DATEDIFF(NOW(),MIN(CreateDataTime)) AS first_order_ago,DATEDIFF(NOW(),MAX(CreateDataTime)) AS last_order_ago,
MAX(OrderAmount) AS max_oeder_amt,MIN(OrderAmount) AS min_oeder_amt,SUM(OrderAmount) AS sum_order_amount,AVG(OrderAmount) AS avg_order_amount,
COUNT(memberid) AS sum_order_cnt,fromip,gender,DATE(createdon) AS createdon
FROM user_profile_basic_informatin #用户基础信息表
GROUP BY memberid
```

## 2.用户行为标签表
Step1：从用户订单表中抽取用户选择产品的属性带来的标签，同时记录用户的行为次数、行为日期等数据。通过检测用户行为，为用户打上各类标签。

Step2：根据一段时间内在用户身上打上各种标签的数量和、时效性、行为次数、标签权重（TF-IDF计算），利用用户标签权重推导规则，计算用户身上每个标签的的权重。

Step3：计算各标签对应属性的参考值，与预定阈值进行比较，判断是否包含该种属性。

Step4：根据所确定的用户各维度属性，完成用户画像。

**应用TF-IDF计算标签权重 01**

```mysql

DROP TABLE tag_weight_of_tfidf_01;
CREATE TABLE tag_weight_of_tfidf_01
AS
SELECT t1.memberid,t1.tag_id,
t1.tag_name,t1.weight_m_p,
t2.weight_m_s
FROM
(SELECT memberid,tag_id,tag_name,COUNT(tag_name) AS weight_m_p
FROM persona_user_tag_relation_public_real_01
GROUP BY memberid,tag_name,tag_id) t1
INNER JOIN
(SELECT memberid,tag_id,COUNT(tag_id) AS weight_m_s
FROM persona_user_tag_relation_public_real_01 #用户订单行为表
GROUP BY memberid,tag_id) AS t2
ON t1.memberid=t2.memberid AND t1.tag_id=t2.tag_id
```

**应用TF-IDF计算标签权重 02**

```mysql

DROP TABLE tag_weight_of_tfidf_02;
CREATE TABLE tag_weight_of_tfidf_02
AS
SELECT t1.tag_name,
t1.weight_w_p,-- 每个标签在全体标签中共有多少个
t1.weight_w_s,-- s所有标签的总个数,
t2.weight_w_s_01
FROM(
SELECT tag_name,SUM(weight_m_p) AS weight_w_p,
29561 AS weight_w_s
FROM tag_weight_of_tfidf_01
GROUP BY tag_name) t1
CROSS JOIN
(SELECT SUM(weight_m_p) AS weight_w_s_01
FROM tag_weight_of_tfidf_01)t2

SELECT * FROM tag_weight_of_tfidf_02
```
**if-idf计算每个用户身上标签权重**

```mysql

DROP TABLE IF EXISTS tag_weight_of_tfidf_03;
CREATE TABLE tag_weight_of_tfidf_03
AS
SELECT t1.memberid,t1.tag_name,
(t1.weight_m_p/weight_m_s)(t2.weight_w_s/t2.weight_w_p) AS ratio
FROM tag_weight_of_tfidf_01 t1
LEFT JOIN tag_weight_of_tfidf_02 t2
ON t1.tag_name=t2.tag_name
GROUP BY t1.memberid,t1.tag_name,
(t1.weight_m_p/weight_m_s)(t2.weight_w_s/t2.weight_w_p)

SELECT * FROM tag_weight_of_tfidf_03
```

**计算用户标签权重**

```mysql

DROP TABLE person_user_tag_relation_weight;
CREATE TABLE person_user_tag_relation_weight
AS
SELECT t1.memberid,
t2.tag_name AS tag_name,t1.tag_id,
t1.cnt,
t1.dateid,t1.cnt*t2.ratio AS act_weight
FROM persona_user_tag_relation_public_real_01 t1
INNER JOIN tag_weight_of_tfidf_03 t2
ON t1.memberid=t2.memberid AND t1.tag_name=t2.tag_name

SELECT * FROM person_user_tag_relation_weight ORDER BY memberid
```

**计算用户行为画像权重**

```mysql

DROP TABLE person_user_tag_relation_weight_01;
CREATE TABLE person_user_tag_relation_weight_01
AS
SELECT t1.memberid,
t2.tag_name AS tag_name,t1.tag_id,
t1.cnt,
t1.dateid,t1.cnt*t2.ratio AS act_weight
FROM persona_user_tag_relation_public_real_01 t1
INNER JOIN tag_weight_of_tfidf_03 t2
ON t1.memberid=t2.memberid AND t1.tag_name=t2.tag_name
```

## 3. 用户偏好标签表：
Step1：从用户行为标签表中抽取用户的数据，计算用户对每个标签的偏好程度（即权重）。对各标签的权重做历史加总后，按从高到低的顺序做排序，排名靠前的即为用户最关心的标签。

Step2：应用词频共现矩阵，计算标签两两之间的相关性。

Step3：将Step1与Step2中创建的临时表通过标签相关联，以Step1中的的表为基础，找到与用户最关心标签关联性最大的标签（来自Step2），即为用户偏好同类标签。抽取topN标签即为用户推荐标签。

**建立用户偏好画像表**

```mysql

DROP TABLE user_prefer_peasona_tag_01;
CREATE TABLE user_prefer_peasona_tag_01
AS
SELECT memberid , tag_name,cnt,dateid
FROM persona_user_tag_relation_public_real_01

where dataid>=date_sub(from_unixtime(unix_timestamp(),‘yyyy-mm-dd’,31)and dataid<=date_sub(from_unixtime(unix_timestamp(),‘yyyy-mm-dd’,1)
GROUP BY memberid,tag_name,cnt,dateid
```

**计算每个标签对应的人数**

```mysql

DROP TABLE user_prefer_peasona_tag_02;
CREATE TABLE user_prefer_peasona_tag_02
AS
SELECT tag_name,COUNT(DISTINCT memberid) AS user_num
FROM user_prefer_peasona_tag_01
GROUP BY tag_name

SELECT * FROM user_prefer_peasona_tag_02
```

**计算每个标签共同关注人数的同现矩阵**

```mysql

DROP TABLE user_prefer_peasona_tag_03;
CREATE TABLE user_prefer_peasona_tag_03
AS
SELECT tag_name_1,
tag_name_2,
t.num
FROM(
SELECT t1.tag_name tag_name_1,t2.tag_name tag_name_2,
COUNT(DISTINCT t1.memberid) AS num
FROM user_prefer_peasona_tag_01 t1
CROSS JOIN user_prefer_peasona_tag_01 t2
WHERE t1.memberid=t2.memberid #同一用户
GROUP BY t1.tag_name,t2.tag_name)t
```
**余弦相似矩阵，计算两两标签间的相似性**

```mysql

DROP TABLE user_prefer_peasona_tag_04;
CREATE TABLE user_prefer_peasona_tag_04
AS
SELECT t1.tag_name_1,#第一个标签名字
t2.user_num_1,#第一个标签人数
t1.tag_name_2,#第二个标签名字
t3.user_num_2,#第二个标签人数
t1.num,
(t1.num/SQRT(t2.user_num_1t3.user_num_2)) AS POWER
FROM user_prefer_peasona_tag_03 t1
LEFT JOIN (SELECT tag_name ,user_num AS user_num_1 FROM user_prefer_peasona_tag_02) AS t2
ON t1.tag_name_1=t2.tag_name
LEFT JOIN
(SELECT tag_name,user_num AS user_num_2 FROM user_prefer_peasona_tag_02) t3
ON t1.tag_name_2=t3.tag_name
GROUP BY
t1.tag_name_1,#第一个标签名字
t2.user_num_1,#第一个标签人数
t1.tag_name_2,#第二个标签名字
t3.user_num_2,#第二个标签人数
t1.num,
(t1.num/SQRT(t2.user_num_1t3.user_num_2))
```

**对每个用户的历史权重加总**

```mysql

DROP TABLE user_prefer_peasona_tag_05;
CREATE TABLE user_prefer_peasona_tag_05
AS
SELECT memberid,tag_name,SUM(act_weight) AS weight
FROM person_user_tag_relation_weight
GROUP BY memberid,tag_name
```

**计算推荐给用户的相关标签**

```mysql

DROP TABLE user_prefer_peasona_tag;
CREATE TABLE user_prefer_peasona_tag
AS
SELECT t.memberid,
t.tag_name,
t.recommend
FROM(
SELECT t1.memberid,
t2.tag_name_2 AS tag_name,#推荐的标签
SUM(t1.weight*t2.power) AS recommend FROM user_prefer_peasona_tag_05 t1
LEFT JOIN
user_prefer_peasona_tag_04 t2
ON t1.tag_name=t2.tag_name_1
GROUP BY t1.memberid,
t2.tag_name_2)t
ORDER BY recommend DESC
```

## 4. 用户群体偏好标签表

Step1：先将用户按照性别、会员年限等人群属性进行划分（不同的应用场景有不同的划分方式），从用户属性表中抽取相应的逻辑做用户属性归类，然后将每个用户通过用户id关联到用户标签表上。

Step2：应用TF-IDF算法计算用户标签表中没类人群中各标签的的权重值ratio。
Step3：取出没类人群中的topN标签，即得到目标群体的偏好标签。

**用户群体画像:构建用户性别会员年限表**

```mysql

DROP TABLE person_groups_temp_age;
CREATE TABLE person_groups_temp_age
SELECT memberid,
CASE
WHEN gender=‘M’ THEN ‘男’
WHEN gender =‘F’ THEN ‘女’
ELSE ‘其他’ END
AS gender,
CASE
WHEN DATEDIFF(NOW(),createdon)/365>=0 AND DATEDIFF(NOW(),createdon)/365<=0.5 THEN ‘v1’
WHEN DATEDIFF(NOW(),createdon)/365>0.5 AND DATEDIFF(NOW(),createdon)/365<=1.5 THEN ‘v2’
WHEN DATEDIFF(NOW(),createdon)/365>1.5 AND DATEDIFF(NOW(),createdon)/365<=3 THEN ‘v3’
WHEN DATEDIFF(NOW(),createdon)/365>3 THEN ‘v4’
ELSE ‘其他’
END AS member_years
FROM user_profile_basic_informatin
GROUP BY memberid
```

**通过会员年限找到对应的权重**

```mysql

DROP TABLE tmp_person_groups_prefer_01;
CREATE TABLE tmp_person_groups_prefer_01
AS
SELECT t1.memberid,t1.tag_name,t1.tag_id,t1.act_weight,t2.gender,t2.member_years
FROM person_user_tag_relation_weight_01 t1
INNER JOIN person_groups_temp_age t2
ON t1.memberid=t2.memberid

SELECT * FROM tmp_person_groups_prefer_01
```

**使用tf-idf计算不同人群偏好**

```mysql

DROP TABLE tmp_person_groups_man_prefer_sum;
CREATE TABLE tmp_person_groups_man_prefer_sum
AS
SELECT t1.tag_name,t1.weight_w_p,t2.weight_w_s_01
FROM
(SELECT tag_name,tag_id,SUM(act_weight) AS weight_w_p
FROM tmp_person_groups_prefer_01 GROUP BY tag_name) t1
INNER JOIN
(SELECT tag_id,SUM(act_weight) AS weight_w_s_01 FROM
tmp_person_groups_prefer_01
GROUP BY tag_id)t2
ON t1.tag_id=t2.tag_id
```
