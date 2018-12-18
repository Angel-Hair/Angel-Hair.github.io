---
title: 一些Pandas DataFrame下数据操作的整理
categories:
 - 数据分析
 - 代码整理
tags:
 - Pandas
 - DataFrame
 - 数据操作
 - Python
---

# 一些Pandas DataFrame下数据操作的整理

## 前言

自从入了Pandas这个坑后，各种的麻烦的问题层出不穷，解决方法积累地越来越多，很明显，我这个失忆患者已经记不住了_(:зゝ∠)_最近做了几篇分析文章，就想着把一些比较有用的数据操作记录下来，以后方便查。

## 分类统计

### 分类统计次数并排序

```python
list_1 = []
list_2 = []
for index, row in test_data.iterrows():
    if row["position_number"] < 40 and row["position_number"] != 0:
#         print(row["classification"], row["position_number"])
        list = re.split('[,、 ]', row["classification"])
        for one in list:
            if one != '' and one not in '其他|不限':
#                 print(one, row["position_number"])
                list_1.append(one)
                list_2.append(row["position_number"])
text_df = pd.DataFrame(list_2, index=list_1, columns=['招聘人数'])
```

### 当某列所含多个值同时对应其他列值时进行统计（正则）

```python
show_list = test_data.groupby(by='address').count().sort_values('classification')
```

### 当某列含有多个未分离值而需要统计

PS:*注意这里用到了collections库*

```python
import collections

cnt_all = collections.Counter()
for one in test_data['classification']:
    list = re.split('[,、 ]', one)
    for word in list:
        if(word not in '其他|不限'):
            cnt_all[word] += 1
financing_cont_all = pd.DataFrame(cnt_all.most_common(20), columns=['行业分类','统计次数'])
```

### 提取特定值的所有行

```python
guangdong = test_data.loc[test_data['address']=='广东']
```

### 按照对应index排序（可以去掉不需要的index）

```python
next_index_1 = ['天使轮','A轮','B轮','C轮','D轮及以上','上市公司','不需要融资','未融资']
next_financing = show_financing.reindex(next_index_1)
```

## 修改

### 匹配多个字符串并修改为某个字符串（正则）

```python
test_data['address'].replace('.*四川.*|.*乐山.*|.*内江.*|.*凉山彝族自治州.*|.*南充.*|.*成都.*|.*宜宾.*|.*遂宁.*|.*巴中.*|.*资阳.*|.*达州.*|.*绵阳.*|.*泸州.*|.*德阳.*|.*眉山.*|.*广安.*|.*自贡.*', '四川', regex=True, inplace = True)
```

### 将多个值修改为对应值

以下结果为把`test_data`的“number”行中的'15-50 人'修改为'少于 15 人'以及把'15-50人'修改为'少于15人'。

```python
test_data['number']=test_data['number'].replace(['15-50 人','少于 15 人'],['15-50人','少于15人'])
```

### 删去特定值的对应行

```python
next_df=text_df[~text_df['招聘人数'].isin([0])]
```

### 修改数据精确度

以下结果为小数点后两位

```python
show_positio_mean = show_positio_mean.round(2)
```
