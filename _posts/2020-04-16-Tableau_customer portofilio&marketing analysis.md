Tableau_customer portofilio&marketing analysis
#Tableau用户画像及营销分析



项目概述

  本项目的目的是充分挖掘客户的存款需求，通过运用MySQL，Python (相关性分析部分）和Tableau (数据可视化部分）建立客户画像，为精准营销提供数据化的建议，从而改善客户服务体验和开展银行产品销售业务。[数据集](https://www.kaggle.com/janiobachmann/bank-marketing-dataset) 为此，本报告从如下几个维度来分析：
  
### 客户画像

 1）**人口属性信息**：年龄； 
 
 2）**信用属性信息**：职业、婚姻状况、银行存款余额、负债（住房贷款和个人贷款）、学历等； 
 
 3）**消费特征信息**：参与银行营销活动的次数、参与通话时长、是否有定期存款。
 
### 相关性分析
分析影响客户定期存款业务的特征的相关性，找出**强相关信息**：

 1）**矩阵相关性分析**
 
 2）银行存款与负债相关
 
 3）营销活动相关


### 总结与营销建议

# 一、客户画像

![](https://kiranli.github.io/images/bank1_t26.png)

* **Notes:** 总体上，这个样本中的有定期存款与无定期存款客户比例相当，数据有数值型变量和字符串变量，数值变量采用矩阵相关性分析，其余的进行特性分析。


## 1.1 人口属性信息

![](https://kiranli.github.io/images/bank1_t12.png)

![](https://kiranli.github.io/images/bank1_t13.png)

![](https://kiranli.github.io/images/bank1_t17.png)

* **Notes:** 20岁-60岁的群体会更趋于拥有定期存款。

## 1.2 信用属性信息
## 1.2.1 职业
![](https://kiranli.github.io/images/bank1_t15.png)

![](https://kiranli.github.io/images/bank1_t19.png)

**Notes：**

* 管理人员是最多的职业类型，其次是技术人员和蓝领；
* 学生和退休者更趋于拥有定期存款，因为其有定期存款的比例明显高于无定期存款的占比。

## 1.2.2 学历

![](https://kiranli.github.io/images/bank1_t10.png)

![](https://kiranli.github.io/images/bank1_t20.png)

* **Notes**: 受教育程度越高，越趋于拥有定期存款，其中高中教育用户占比最高，其次是高等大学教育用户。

## 1.2.3 婚姻状态

![](https://kiranli.github.io/images/bank1_t6.png)

![](https://kiranli.github.io/images/bank1_t22.png)

* **Notes:** 已婚用户更倾向定期存款，定期存款产品用户中超过半数为已婚用户。



## 1.2.4 银行余额

![](https://kiranli.github.io/images/bank1_t21.png)

* **Notes:** 定期存款产品用户平均银行余额是1800，其中近80%的用户是低存款额客户，即存款额小于3000的。

## 1.2.5 负债

![](https://kiranli.github.io/images/bank1_t24.png)

* **Notes:** 没有住房贷款和个人贷款的客户，明显更倾向于定期存款。



## 1.3 消费特征信息

![](https://kiranli.github.io/images/bank1_t25.png)

* **Notes:**无违约记录的客户更倾向于定期存款。

# 二、相关性分析
##  2.1 通话时长相关

![](https://kiranli.github.io/images/bank1_p2.png)

![](https://kiranli.github.io/images/bank1_t30.png)

**Notes:**
 
*  由矩阵图中，可以看出这几个值当中与业务相关系数最大的是duration通话时间；
*  duration的值主要集中在0~800之间，通话时间的均值为372s，以此为界，可看出通话时间高于均值的，开办定期业务的比例约为70%，低于均值的，仅约为30%；
*  随着通话时间越长，用户开办业务的成功率越高，因为聊的越久，表明兴趣越大。
## 2.2 银行余额相关

![](https://kiranli.github.io/images/bank1_t28.png) 

![](https://kiranli.github.io/images/bank1_t29.png)

![](https://kiranli.github.io/images/bank1_p3.png)

**Notes:**
 
*  有违约记录的人员盈余明显偏低，表明他们的经济状况确实不太好；
*  经济状况较好的职业有，退休人员、管理层、自雇和技术人员，其中管理人员和技术人员是存款最高的人；
*  不同教育程度的人员的银行存款情况没有明显的偏差，四种存款分类下的中值都差不多，并不像想象中的，高等教育者有更高的存款；
*  婚姻状态与银行存款没有什么相关性，因为无论离婚者、单身者、结婚者，在各个存款区间的分布都比较相似，普遍分布在0~5k内；
*  有无住房贷款和个人贷款会直接影响银行存款，没有住房贷款和个人贷款的用户有更多的存款，但矩阵图看出负债与定期存款是弱相关。


## 2.3 营销活动相关

![](https://kiranli.github.io/images/bank1_t31.png)

 **Notes:** 

* 从营销结果来看，20岁以下或60岁以上的群体似乎更容易营销成功，因此可以考虑将他们作为重点的营销对象;
* 学生及退休人员更加可能有定期存款，同时在营销方面取得成功；
* 蓝领、企业家、服务者、技术员不容易推销成功。

![](https://kiranli.github.io/images/bank1_t11.png)

 **Notes:** 
 
*  营销活动主要集中在8月、5月、2月；
*  尽管8月份的营销客户数很多，但是营销结果却存在大量未知的结果, 需与营销部门沟通，了解原因；
*  实际营销活动较为成功的月份为5月、2月，也很有可能是因为营销的次数比较多导致的成功案例增多，而由于8月的营销结果存在大量未知数据因此无法具体分析。

# 三、 总结建议
## 3.1 定期存款产品用户画像

* 年龄：20岁-60岁的群体会更趋于拥有定期存款；
* 职业：管理人员，技术人员和蓝领是占比最多的职业，而学生和退休者更趋于拥有定期存款；
* 学历：受教育程度高的用户更倾向拥有定期存款，其中高中教育用户占比最高，其次是高等大学教育用户；
* 银行余额：约80%的定期存款产品用户是低存款额客户，即存款额小于3000的；
* 负债：没有住房贷款和个人贷款的客户，明显更倾向于定期存款；
* 消费特征：无违约记录的客户更倾向于定期存款；


## 3.1 营销建议

* **加强营销话术培训**：通话时长偏好：与定期存款相关性最高的是通话时长，随着通话时间越长，用户开办业务的成功率越高，通话时间高于均值372s的，办理定期业务的比例约为70%，低于均值的，仅约为30%；
* **营销对象**：20岁以下或60岁以上的群体更容易营销成功，蓝领、企业家、服务者、技术员不容易推销成功；
* **营销时间**：营销活动成功率较高的集中在8月、5月、2月，但需要更多数据验证。


# 四、数据处理

## 客户画像分析部分-MySQL
```mysql
#新增一列序号列 
ALTER TABLE bank ADD id INT(4) NOT NULL
    PRIMARY KEY AUTO_INCREMENT FIRST;
		
#查看数据总数 
select count(*) from bank;

#概览数据 
desc bank; 

#查看异常值
select min(age),max(age),avg(age),
min(balance),max(balance),avg(balance),
min(duration),max(duration),avg(duration) from bank;

# 有定期存款客户的年龄分层并计算比重
select case when age <20 then '<20' 
when age >=20 and age <30 then '[20,30)' 
when age >=30 and age <40 then '[30,40)' 
when age >=40 and age <60 then '[40,60)'
ELSE '>60' END 
AS 'age_group',
count(*), 
concat(round((count(age)/(select count(*) from bank))*100,2),'%') AS 'PER'  
from bank
where deposit = 'yes'
group by case when age <20 then '<20' 
when age >=20 and age <30 then '[20,30)' 
when age >=30 and age <40 then '[30,40)' 
when age >=40 and age <60 then '[40,60)'
ELSE '>60' END 
order by count(*) desc;

# 有定期存款客户的银行余额分层并计算比重
select case when balance <0 then 'negative' 
when balance>=0 and balance <3000 then 'low' 
when balance>=3000 and balance<10000 then 'median' 
ELSE 'high' END 
AS 'balance_group',
count(*), 
concat(round((count(balance)/(select count(*) from bank))*100,2),'%') AS 'PER'  
from bank
where deposit = 'yes'
group by case when balance <0 then 'negative' 
when balance>=0 and balance <3000 then 'low' 
when balance>=3000 and balance<10000 then 'median' 
ELSE 'high' END 
order by count(*) desc;

# 有定期存款客户的职业比重
select job, count(*), 
concat(round((count(job)/(select count(*) from bank))*100,2),'%') AS 'PER'  
from bank
where deposit = 'yes'
group by job 
order by count(*) desc;

# 有定期存款客户的婚姻状况比重
select marital, count(*), 
concat(round((count(marital)/(select count(*) from bank))*100,2),'%') AS 'PER'  
from bank
where deposit = 'yes'
group by marital 
order by count(*) desc;

# 有定期存款客户的学历比重
select education, count(*), 
concat(round((count(education)/(select count(*) from bank))*100,2),'%') AS 'PER'  
from bank
where deposit = 'yes'
group by education 
order by count(*) desc;

# 营销活动结果的分布 
select poutcome, count(*), 
concat(round((count(poutcome)/(select count(*) from bank))*100,2),'%') AS 'PER'  
from bank
group by poutcome
order by count(*) desc;
```
## 矩阵相关性分析部分-Python

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
plt.rcParams['font.sans-serif']=['SimHei'] #用来正常显示中文标签
plt.rcParams['axes.unicode_minus']=False #用来正常显示负号
bank=pd.read_csv('/Users/hui2046/Downloads/SQL+Python/bank.csv')
bank.info()
bank.head()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 11162 entries, 0 to 11161
    Data columns (total 17 columns):
    age          11162 non-null int64
    job          11162 non-null object
    marital      11162 non-null object
    education    11162 non-null object
    default      11162 non-null object
    balance      11162 non-null int64
    housing      11162 non-null object
    loan         11162 non-null object
    contact      11162 non-null object
    day          11162 non-null int64
    month        11162 non-null object
    duration     11162 non-null int64
    campaign     11162 non-null int64
    pdays        11162 non-null int64
    previous     11162 non-null int64
    poutcome     11162 non-null object
    deposit      11162 non-null object
    dtypes: int64(7), object(10)
    memory usage: 1.4+ MB





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>balance</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>day</th>
      <th>month</th>
      <th>duration</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>deposit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>59</td>
      <td>admin.</td>
      <td>married</td>
      <td>secondary</td>
      <td>no</td>
      <td>2343</td>
      <td>yes</td>
      <td>no</td>
      <td>unknown</td>
      <td>5</td>
      <td>may</td>
      <td>1042</td>
      <td>1</td>
      <td>-1</td>
      <td>0</td>
      <td>unknown</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>1</td>
      <td>56</td>
      <td>admin.</td>
      <td>married</td>
      <td>secondary</td>
      <td>no</td>
      <td>45</td>
      <td>no</td>
      <td>no</td>
      <td>unknown</td>
      <td>5</td>
      <td>may</td>
      <td>1467</td>
      <td>1</td>
      <td>-1</td>
      <td>0</td>
      <td>unknown</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>2</td>
      <td>41</td>
      <td>technician</td>
      <td>married</td>
      <td>secondary</td>
      <td>no</td>
      <td>1270</td>
      <td>yes</td>
      <td>no</td>
      <td>unknown</td>
      <td>5</td>
      <td>may</td>
      <td>1389</td>
      <td>1</td>
      <td>-1</td>
      <td>0</td>
      <td>unknown</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>3</td>
      <td>55</td>
      <td>services</td>
      <td>married</td>
      <td>secondary</td>
      <td>no</td>
      <td>2476</td>
      <td>yes</td>
      <td>no</td>
      <td>unknown</td>
      <td>5</td>
      <td>may</td>
      <td>579</td>
      <td>1</td>
      <td>-1</td>
      <td>0</td>
      <td>unknown</td>
      <td>yes</td>
    </tr>
    <tr>
      <td>4</td>
      <td>54</td>
      <td>admin.</td>
      <td>married</td>
      <td>tertiary</td>
      <td>no</td>
      <td>184</td>
      <td>no</td>
      <td>no</td>
      <td>unknown</td>
      <td>5</td>
      <td>may</td>
      <td>673</td>
      <td>2</td>
      <td>-1</td>
      <td>0</td>
      <td>unknown</td>
      <td>yes</td>
    </tr>
  </tbody>
</table>
</div>




```python
bank.hist(bins=20,figsize=(14,10))
```


![](https://kiranli.github.io/images/bank1_p5.png)



```python
from sklearn.preprocessing import StandardScaler, OneHotEncoder, LabelEncoder
data5=bank
data5['deposit']=LabelEncoder().fit_transform(data5['deposit'])
#把deposit转化为数值变量
corrmat=data5.corr()
plt.figure(figsize=(15,10))
sns.heatmap(corrmat,annot=True,cmap=sns.diverging_palette(250, 150, as_cmap=True))
ax = sns.heatmap(...)
bottom, top = ax.get_ylim()
ax.set_ylim(bottom + 0.5, top - 0.5)
```





![](https://kiranli.github.io/images/bank1_p6.png)



```python
data6=bank[['deposit','housing','loan']]
data6['deposit']=LabelEncoder().fit_transform(data5['deposit'])
data6['housing']=LabelEncoder().fit_transform(data6['housing'])
data6['loan']=LabelEncoder().fit_transform(data6['loan'])
corrmat=data6.corr()
plt.figure(figsize=(13,8))
sns.heatmap(corrmat,annot=True,cmap=sns.diverging_palette(250, 150, as_cmap=True))
```

![](https://kiranli.github.io/images/bank1_p7.png)



```python

```


