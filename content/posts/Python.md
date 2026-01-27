---
title: "Python数据分析"
subtitle: ""
date: '2025-01-14T10:55:00+08:00'
lastmod: '2026-01-27T11:01:00+08:00'
draft: false
categories: ['学习']
tags: ['Python','Numpy','Pandas','命令']
---



### 一、 numpy

1. 下标提取

   ```python
   # x为开始位置,y为结束位置,z为步长
   a[x:y:z]
   # 逗号前为行,后为行
   a[x:y:z,x1:y1:z1]
   # 取出满足x>3并且x<10的元素
   a[(x > 3) & (x < 10)]
   ```
   
2. 反转数组

   ```python
   z = z[::-1]
   ```

3. 改变数组形状

   ```python
   reshape(3,3)
   ```

4. 找出不为0的数，返回下标

   ```python
   np.nonzero([1,2,0,0,4,0])
   ```

5. 随机生成3\*3\*3的矩阵

   ```python
   np.random.random((3,3,3))
   ```

6. 提取主对角线上面/下面斜线的元素

   ```python
   # -1:主对角线下面,1:主对角线上面,0:主对角线
   np.diag(x, k=-1/1/0)
   ```

7. 创建一个只包含主对角线元素的n*m矩阵(其他元素为0)

   ```python
   # 负数:主对角线向下偏移,0:主对角线,正数:主对角线向上偏移
   np.eye(n,m,k=负数/0/正数)
   ```

8. 创建一个矩阵（基于另外一个矩阵重复reps次）

   ```python
   # 重复矩阵A(reps=数字:单纯重复次数,reps=(y,x):重复行y次,列x次)
   np.tile(A,reps)
   ```

9. 矩阵相乘

   ```python
   np.dot(A,B)
   ```

10. 获取日期

    ```python
    #(今天,昨天,明天)
    yesterday = np.datetime64('today') - np.timedelta64(1)
    today     = np.datetime64('today')
    tomorrow  = np.datetime64('today') + np.timedelta64(1)
    #某一个月所有的日期(np.arange('2016-07', '2016-08', dtype='datetime64[D]'))
    np.arange('某个月','下个月',dtype='datetime64[D]')
    ```

11. 随机生成均匀分布的矩阵

    ```python
    np.random.uniform(low,high,size)
    ```

12. 生成不包含x,y的在x到y的z个数的线性数组

    ```python
    np.linspace(x,y,z,endpoint=False)[1:]
    ```

13. 快速计算小数组求和

    ```python
    Z = np.arange(10)
    np.add.reduce(Z)
    ```

14. 矩阵只读

    ```python
    Z.flags.writeable = False
    ```

15. 将矩阵的最大值改成x

    ```python
    Z[Z.argmax()] = x
    ```

16. 找出矩阵中最接近x的值

    ```python
    Z.flat[np.abs(Z - x).argmin()]
    ```

17. 找到在矩阵中最接近给定值的值

     ```python
     Z.flat[np.abs(Z - 给定值).argmin()]
     ```

### 二、pandas

1. 提取某一列含有nan的行

   ```python
   df[df['col_name'].isnull()]
   ```

2. 统计某一列不同元素的个数

   ```python
   df['col_name'].value_counts()
   ```

3. 将某一列元素值对应更改(映射更改某列)

   ```python
   df['col_name'].map({'old_value1': new_value1, 'old_value2': new_value2})
   ```

4. 某一列元素值直接替换

   ```python
   df['col_name'].replace('old_value', 'new_value')
   ```

5. 生成数据透视表

   ```python
   df.pivot_table(index='col_name1', columns='col_name2', values='col_name3', aggfunc='method_name')
   ```

6. 找到某一列最大的几个元素

   ```python
   df['col_name'].nlargest(num)
   ```

7. 拆分某一列(按照字符串拆分)

   ```python
   t = df['col_name'].str.split('',expand=True)
   t.columns = ['col_name1','col_',...]
   ```

8. 删除某一列

   ```python
   del df['col_name']
   ```

9. 获取不连续位置数据集

   ```python
   df[['col_name1','col_name2'...]]
   ```

10. 获取内层索引数据

    ```python
    df[:,'inner_index_name']
    ```
