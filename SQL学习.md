<center>SQL语法学习</center>

[TOC]



# 一、单表查询

1. 查询订购日期在1996年7月1日至1996年7月15日之间的订单的订购日期、订单ID、客户ID和雇员ID等字段的值

```sql
SELECT 订购日期,订单ID,客户ID,雇员ID FROM `订单` 
WHERE 订购日期 BETWEEN '1996-07-01' AND '1996-07-15'
```



![image-20210103120259439](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103120259439.png)



2. 查询供应商的ID、公司名称、地区、城市和电话字段的值。条件是“地区等于华北”并且“联系人头衔等于销售代表”

```sql
SELECT 供应商ID,公司名称,地区,城市,电话 FROM `供应商`
WHERE 地区 = '华北' AND 联系人职务 = '销售代表'
```



![image-20210103121046011](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103121046011.png)



3. 查询供应商的ID、公司名称、地区、城市和电话字段的值。其中的一些供应商位于华东或华南地区，另外一些供应商所在的城市是天津

```sql
SELECT 供应商ID,公司名称,地区,城市,电话 FROM `供应商`
WHERE (地区='华东' OR 地区='华南')
OR (城市 = '天津')
```



![image-20210103121608653](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103121608653.png)



4. 查询位于“华东”或“华南”地区的供应商的ID、公司名称、地区、城市和电话字段的值

```sql
SELECT 供应商ID,公司名称,地区,城市,电话 FROM `供应商`
WHERE (地区='华东' OR 地区='华南')
```



![image-20210103122145806](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103122145806.png)









# 二、多表查询



1. 查询订购日期在1996年7月1日至1996年7月15日之间的订单的订购日期、订单ID、相应订单的客户公司名称、负责订单的雇员的姓氏和名字等字段的值，并将查询结果按雇员的“姓氏”和“名字”字段的升序排列，“姓氏”和“名字”值相同的记录按“订单 ID”的降序排列

```sql
SELECT 订购日期, 订单ID, 公司名称, 姓氏, 名字
FROM 订单, 客户, 雇员
WHERE 订单.`雇员ID` = 雇员.`雇员ID`
AND 订单.`客户ID` = 客户.`客户ID`
AND 订单.订购日期 BETWEEN '1996-07-01' AND '1996-07-15'
ORDER BY convert(姓氏 using gbk)ASC, convert(名字 using gbk)ASC, convert(订单ID using gbk)DESC 
```





![image-20210103171959067](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103171959067.png)



2. 查询“10248”和“10254”号订单的订单ID、运货商的公司名称、订单上所订购的产品的名称

```sql
SELECT 订单.订单ID, 公司名称, 产品名称
FROM 订单
LEFT JOIN 运货商 ON 订单.`运货商` = 运货商.`运货商ID`
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
WHERE 订单.订单ID IN (10248, 10254)
```



![image-20210103183559821](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103183559821.png)



3. 查询“10248”和“10254”号订单的订单ID、订单上所订购的产品的名称、数量、单价和折扣

```sql
SELECT 订单.订单ID, 产品名称, 单位数量, 订单明细.单价, 折扣
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
WHERE 订单.订单ID IN (10248, 10254)
```



![image-20210103184901495](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103184901495.png)





4. 查询“10248”和“10254”号订单的订单ID、订单上所订购的产品的名称及其销售金额

```sql
SELECT 订单.订单ID, 产品名称, ROUND(订单明细.数量 * 订单明细.单价 * (1 - 折扣),2) AS 销售金额
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
WHERE 订单.订单ID IN (10248, 10254)
```



![image-20210103190800521](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103190800521.png)

# 三、综合查询

1. 查询所有运货商的公司名称和电话

```sql
SELECT 公司名称, 电话 
FROM 运货商
```



![image-20210103192333806](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103192333806.png)





2. 查询所有客户的公司名称、电话、传真、地址、联系人姓名和联系人头衔

```sql
SELECT 公司名称, 电话, 传真, 地址, 联系人姓名, 联系人职务 
FROM 客户
```



![image-20210103192701662](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103192701662.png)





3. 查询单价介于10至30元的所有产品的产品ID、产品名称和库存量

```sql
SELECT 产品ID, 产品名称, 库存量
FROM 产品
WHERE 单价 BETWEEN 10 AND 30
```



![image-20210103193238126](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103193238126.png)





4. 查询单价大于20元的所有产品的产品名称、单价以及供应商的公司名称、电话

```sql
SELECT 产品名称, 单价, 公司名称, 电话
FROM 产品
LEFT JOIN 供应商 ON 产品.`供应商ID` = 供应商.`供应商ID`
WHERE 单价 > 20
```

![image-20210103194353469](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103194353469.png)



5. 查询上海和北京的客户在1996年订购的所有订单的订单ID、所订购的产品名称和数量

```sql
SELECT 订单.订单ID, 产品名称, 数量
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 客户 ON 订单.`客户ID` = 客户.`客户ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
WHERE YEAR(订单.订购日期) = '1996'
AND 客户.`城市` IN ('上海', '北京')
```



![image-20210103200137200](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103200137200.png)



6. 查询华北客户的每份订单的订单ID、产品名称和销售金额

```sql
SELECT 订单.订单ID, 产品名称, ROUND(订单明细.单价 * 订单明细.数量 * (1 - 折扣),2) AS 销售金额
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 客户 ON 订单.`客户ID` = 客户.`客户ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
WHERE 客户.`地区` = '华北'
```

![image-20210103201210491](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103201210491.png)





7. 按运货商公司名称，统计1997年由各个运货商承运的订单的总数量

```sql
SELECT 运货商.公司名称, COUNT(订单.运货商) AS 总数量
FROM 订单
LEFT JOIN 运货商 ON 订单.`运货商` = 运货商.运货商ID
WHERE YEAR(订单.发货日期) = '1997'
GROUP BY 公司名称
```



![image-20210103202659426](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103202659426.png)



8. 统计1997年上半年的每份订单上所订购的产品的总数量

```sql
SELECT 订单.订单ID, SUM(订单明细.`数量`) AS 总数量（上半年）
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
WHERE 订单.订购日期 BETWEEN '1997-01-01' AND '1997-07-01'
GROUP BY 订单.订单ID
```



![image-20210103203924161](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103203924161.png)





9. 统计各类产品的平均价格

```sql
SELECT 产品.`类别ID`, ROUND(AVG(产品.`单价`),2) AS 平均价格
FROM 产品
GROUP BY 产品.类别ID
```

![image-20210103204722590](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103204722590.png)



10. 统计各地区客户的总数量

```sql
SELECT 地区, COUNT(客户.地区) AS 客户总数量
FROM 客户
GROUP BY 客户.`地区`
```

![image-20210103205315218](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210103205315218.png)





# 四、总练习



1. 找出供应商名称，所在城市

```sql
SELECT 公司名称 AS 供应商名称, 城市 AS 所在城市
FROM 供应商
```

![image-20210104104646135](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104104646135.png)



2. 找出华北地区能够供应海鲜的所有供应商列表。

```sql
SELECT 公司名称 AS 供应商列表
FROM 产品
LEFT JOIN 供应商 ON 供应商.`供应商ID` = 产品.`供应商ID`
LEFT JOIN 类别 ON 类别.`类别ID` = 产品.`类别ID`
WHERE 类别.类别ID = 8
AND 地区 = '华北'
```

![image-20210104110004962](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104110004962.png)



3.  找出订单销售额前五的订单是经由哪家运货商运送的。

```sql
SELECT 运货商.`公司名称`, ROUND((订单明细.单价 * 订单明细.数量 * (1 - 折扣)), 2) AS 销售额
FROM 订单明细
LEFT JOIN 订单 ON 订单明细.`订单ID` = 订单.`订单ID`
LEFT JOIN 运货商 ON 订单.`运货商` = 运货商.`运货商ID`
ORDER BY 销售额 DESC
LIMIT 5
```

![image-20210104112431177](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104112431177.png)



4. 找出按箱包装的产品名称

```sql
SELECT 产品名称
FROM 产品
WHERE 单位数量 LIKE '%箱%'
```

![image-20210104113022679](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104113022679.png)



5. 找出重庆的供应商能够供应的所有产品列表

```sql
SELECT 产品名称 
FROM 产品
LEFT JOIN 供应商 ON 产品.`供应商ID` = 供应商.`供应商ID`
WHERE 供应商.`城市` = '重庆'
```

![image-20210104113606118](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104113606118.png)





6. 找出雇员郑建杰所有的订单并根据订单销售额排序

```sql
SELECT 订单明细.`订单ID`, ROUND((订单明细.单价 * 数量 * (1 - 折扣)), 2) AS 销售额
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 雇员 ON 订单.`雇员ID` = 雇员.`雇员ID`
WHERE 雇员.`姓氏` = '郑'
ORDER BY 销售额 DESC
```

![image-20210104114444166](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104114444166.png)



7. 建立产品与订单的关联

```sql
SELECT * 
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
```

![image-20210104125447342](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104125447342.png)



8.  找出订单10284的所有产品以及订单金额，运货商

```sql
SELECT 产品名称, ROUND((订单明细.`单价` * 数量 * (1 - `折扣`)),2) AS 订单金额, 公司名称
FROM 订单明细
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
LEFT JOIN 订单 ON 订单明细.`订单ID` = 订单.`订单ID`
LEFT JOIN 运货商 ON 订单.`运货商` = 运货商.`运货商ID`
```

![image-20210104130058278](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104130058278.png)



9.  计算销量前10位的订单明细，结果集返回订单ID，订单日期，公司名称，发货日期，销售额，并排序

```sql
SELECT 订单.订单ID, 订购日期, 公司名称, 发货日期, ROUND((订单明细.单价 * 数量 * (1 - 折扣)), 2) AS 销售额
FROM  订单
LEFT JOIN 运货商 ON 订单.`运货商` = 运货商.`运货商ID`
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
ORDER BY 销售额 DESC
LIMIT 10
```

![image-20210104141054278](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210104141054278.png)



10.  按年度统计销售额

```sql
SELECT YEAR(订单.订购日期) AS 年度, SUM(ROUND((订单明细.单价 * 数量 * (1 - 折扣)),2)) AS 销售额
FROM 订单明细
LEFT JOIN 订单 ON 订单.`订单ID` = 订单明细.`订单ID`
GROUP BY 年度
```

![image-20210105111159553](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105111159553.png)





11. 查询供应商中能够供应的产品样数最多的供应商

```sql
SELECT 公司名称 AS 供应商, COUNT(产品名称) AS 产品样数
FROM 产品
LEFT JOIN 供应商 ON 产品.`供应商ID` = 供应商.`供应商ID`
GROUP BY 供应商
ORDER BY 产品样数 DESC
LIMIT 1
```

![image-20210105112605021](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105112605021.png)





12.查询产品类别中包含的产品数量最多的类别

```sql
SELECT 类别名称, COUNT(产品.类别ID) AS 数量
FROM 类别,产品
WHERE 类别.`类别ID` = 产品.`类别ID`
GROUP BY 类别名称
ORDER BY 数量 DESC
LIMIT 1
```

![image-20210105113031071](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105113031071.png)





13. 找出所有的订单中经由哪家运货商运货次数最多

```sql
SELECT 公司名称 AS 运货商, COUNT(运货商) AS 运货次数
FROM 订单, 运货商
WHERE 订单.`运货商` = 运货商.`运货商ID`
GROUP BY 运货商
ORDER BY 运货次数 DESC
LIMIT 1
```

![image-20210105132336632](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105132336632.png)



14. 按类别，产品分组，统计销售额

```sql
SELECT 类别.类别名称, 产品.产品名称, SUM(ROUND((订单明细.单价 * 数量 * (1 - 折扣)),2)) AS 销售额
FROM 订单明细
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
LEFT JOIN 类别 ON 产品.`类别ID` = 类别.`类别ID`
GROUP BY 类别名称, 产品名称
```

![image-20210105133355949](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105133355949.png)



15. 查询海鲜类别最大的一笔订单

```sql
SELECT 订单ID, 数量, 产品.产品名称, ROUND((订单明细.单价 * 数量 * (1 - 折扣)),2) AS 销售额
FROM 订单明细
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
LEFT JOIN 类别 ON 产品.`类别ID` = 类别.`类别ID`
WHERE 类别.`类别名称` = '海鲜'
ORDER BY 销售额 DESC
LIMIT 1
```

![image-20210105134319537](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105134319537.png)





16. 按季度统计销售量

```sql
SELECT SUBSTRING(订单.订购日期, 1, 4) 年度, QUARTER(订单.订购日期) 季度, SUM(订单明细.数量) 订购数量     
FROM 订单
LEFT JOIN 订单明细 ON 订单明细.订单ID = 订单.订单ID     
GROUP BY 年度,季度     
ORDER BY 年度,季度
```

![image-20210105153910204](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105153910204.png)



17. 查出订单总额超出5000的所有订单，客户名称，客户所在地区

```sql
SELECT 订单.订单ID, 订单.客户ID, 地区, SUM(ROUND((订单明细.`单价` * `数量` * (1 - 折扣)),2)) AS 总额
FROM 订单
LEFT JOIN 订单明细 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 客户 ON 订单.`客户ID` = 客户.`客户ID`
GROUP BY 订单ID
HAVING 总额 > 5000
ORDER BY 总额 DESC
```

![image-20210105160805390](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105160805390.png)



18. 查询哪些产品的年度销售额低于2000

```sql
SELECT YEAR(订单.订购日期) AS 年度,产品.产品名称, SUM(ROUND((订单明细.单价 * 数量 * (1 - 折扣)),2)) AS 销售额
FROM 订单明细
LEFT JOIN 订单 ON 订单.`订单ID` = 订单明细.`订单ID`
LEFT JOIN 产品 ON 订单明细.`产品ID` = 产品.`产品ID`
GROUP BY 年度, 产品名称
HAVING 销售额 < 2000
ORDER BY 年度
```

![image-20210105162123574](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105162123574.png)



19. 查询所有订单ID开头为102的订单

```sql
SELECT *
FROM 订单
WHERE 订单ID LIKE '102%'
```

![image-20210105162341880](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105162341880.png)



20. 查询所有“中硕贸易”，“学仁贸易”，“正人资源”，“中通”客户的订单，（要求使用in函数）

```sql
SELECT *
FROM 订单, 客户
WHERE 订单.`客户ID` = 客户.`客户ID`
AND 客户.`公司名称` IN ('中硕贸易', '学仁贸易', '正人资源', '中通')
```

![image-20210105163401237](C:\Users\acer\AppData\Roaming\Typora\typora-user-images\image-20210105163401237.png)



21. 