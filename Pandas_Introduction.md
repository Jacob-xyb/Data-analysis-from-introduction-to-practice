```python
import pandas as pd
```

# Read and Write File

## set_option()

> 读取文件前的简单设置

- 描述

    - set_option() 函数用于对文本行列对齐。

- 用法
    - pd.set_option('display.unicode.east_asian_width', True) -- 列名对齐
    - pd.set_option('display.max_rows', 1000)  -- 修改默认输出最大行数
    - pd.set_option('display.max_columns', 1000)  -- 修改默认输出最大列数

- 示例

1.**解决输出列名不对齐**

没有设置前内容(显然结果歪歪扭扭)


```python
df1_1_1 = pd.read_excel(r'data\0301.xlsx')
print(df1_1_1.head())
```

       排名    球员    球队 进球（点球）  出场次数  出场时间  射门  射正
    0   1   瓦尔迪   莱斯特  17(3)    20  1800  49  29
    1   2    英斯  南安普敦     14    22  1537  57  26
    2   3  奥巴梅扬   阿森纳  14(1)    22  1945  55  22
    3   4  拉什福德    曼联  14(5)    22  1881  74  34
    4   5  亚伯拉罕   切尔西     13    21  1673  66  29
    

设置后就对齐了


```python
pd.set_option('display.unicode.east_asian_width', True)
```


```python
df1_1_2 = pd.read_excel(r'data\0301.xlsx')
print(df1_1_2.head())
```

       排名      球员      球队 进球（点球）  出场次数  出场时间  射门  射正
    0     1    瓦尔迪    莱斯特        17(3)        20      1800    49    29
    1     2      英斯  南安普敦           14        22      1537    57    26
    2     3  奥巴梅扬    阿森纳        14(1)        22      1945    55    22
    3     4  拉什福德      曼联        14(5)        22      1881    74    34
    4     5  亚伯拉罕    切尔西           13        21      1673    66    29
    

## read_excel()


- 描述

    - read_excel() 函数导入.xls或.xlsx。

- 语法
```python
pd.read_excel(io,sheet_name=0,header=0,names=None,indexcol=None,usecols=None)
```

- 参数
    - io: 字符串，xls或xlsx文件路径或类文件对象.
    - sheet_name: 获取工作表，默认值为0.
        - sheet_name=[0,1,'Sheet3'] 第一个，第二个和名为'Sheet3'的Sheet页.
    - header: 指定作为列名的行，默认值为0.数据为除列名以外的数据，设置header=None(此时列索引为数字).
    - names: 默认值为None,要使用的列名列表.
    - index_col: 指定列为索引列，默认值为None，索引0是DataFrame对象的行标签。
    - usecols: int,list,字符串，默认为None.
        - None: 解析所有列.
        - int: 解析最后一列.  ```# 已失效!!!  # 必须是[int]```
        - list: 解析列号和列表的列。
        - str: "A:E" or "A,C,E:F"     # 范围包括前后

- 示例


```python
excel_path = r'data/0312.xlsx'
```

1.常规导入
- 只需要输入path，就可以读取excel


```python
df1_2_1 = pd.read_excel(excel_path)
print(df1_2_1.head())
```

      买家会员名  买家实际支付金额 收货人姓名            宝贝标题
    0      mrhy1             41.86     周某某      零基础学Python
    1      mrhy2             41.86     杨某某      零基础学Python
    2      mrhy3             48.86     刘某某      零基础学Python
    3      mrhy4             48.86     张某某      零基础学Python
    4      mrhy5             48.86     赵某某  C#项目开发实战入门
    

2.sheet_name
- None、字符串、整数、字符串列表或整数列表，默认值为0.
    - None：获取所有的工作表。


```python
'''sheet_name=None'''
df1_2_2 = pd.read_excel(excel_path, sheet_name=None)
# print(df1_2_2) # 获取所有工作表后生成的是dict对象
print(len(df1_2_2))
print(df1_2_2.keys())  # 可以看到excel所有工作表的名称
print('\n',df1_2_2['明日'].head())
```

    3
    dict_keys(['明日', '莫寒', '白桦'])
    
       买家会员名  买家实际支付金额 收货人姓名            宝贝标题
    0      mrhy1             41.86     周某某      零基础学Python
    1      mrhy2             41.86     杨某某      零基础学Python
    2      mrhy3             48.86     刘某某      零基础学Python
    3      mrhy4             48.86     张某某      零基础学Python
    4      mrhy5             48.86     赵某某  C#项目开发实战入门
    


```python
'''sheet_name混合使用'''
df1_2_3 = pd.read_excel(excel_path, sheet_name=[0,'白桦'])
# print(df1_2_3)  # 此时也是生成的dict对象，但是不同点在于key值跟随着我们的sheet_name内容的
print(df1_2_3.keys())
print('\n',df1_2_3[0].head())
```

    dict_keys([0, '白桦'])
    
       买家会员名  买家实际支付金额 收货人姓名            宝贝标题
    0      mrhy1             41.86     周某某      零基础学Python
    1      mrhy2             41.86     杨某某      零基础学Python
    2      mrhy3             48.86     刘某某      零基础学Python
    3      mrhy4             48.86     张某某      零基础学Python
    4      mrhy5             48.86     赵某某  C#项目开发实战入门
    

3.header
- 指定作为列名的行，默认值为0.数据为除列名以外的数据，设置header=None(此时列索引为数字).


```python
'''原文本'''
df1_2_4 = pd.read_excel(excel_path)
print(df1_2_4.head())
```

      买家会员名  买家实际支付金额 收货人姓名            宝贝标题
    0      mrhy1             41.86     周某某      零基础学Python
    1      mrhy2             41.86     杨某某      零基础学Python
    2      mrhy3             48.86     刘某某      零基础学Python
    3      mrhy4             48.86     张某某      零基础学Python
    4      mrhy5             48.86     赵某某  C#项目开发实战入门
    


```python
'''header=None'''
df1_2_5 = pd.read_excel(excel_path, header=None)
print(df1_2_5.head())
# 会发现第一行也变成了数据，列索引为[0,1,2,3...]
```

                0                 1           2               3
    0  买家会员名  买家实际支付金额  收货人姓名        宝贝标题
    1       mrhy1             41.86      周某某  零基础学Python
    2       mrhy2             41.86      杨某某  零基础学Python
    3       mrhy3             48.86      刘某某  零基础学Python
    4       mrhy4             48.86      张某某  零基础学Python
    


```python
'''header=1'''
df1_2_6 = pd.read_excel(excel_path, header=1)
print(df1_2_6.head())
# header=number时，前面行的数据就被抹去了。
```

       mrhy1  41.86  周某某      零基础学Python
    0  mrhy2  41.86  杨某某      零基础学Python
    1  mrhy3  48.86  刘某某      零基础学Python
    2  mrhy4  48.86  张某某      零基础学Python
    3  mrhy5  48.86  赵某某  C#项目开发实战入门
    4  mrhy6  48.86  李某某  C#项目开发实战入门
    


```python
'''header=2'''
df1_2_7 = pd.read_excel(excel_path, header=2)
print(df1_2_7.head())
# header=number时，前面行的数据就被抹去了。
```

       mrhy2   41.86  杨某某      零基础学Python
    0  mrhy3   48.86  刘某某      零基础学Python
    1  mrhy4   48.86  张某某      零基础学Python
    2  mrhy5   48.86  赵某某  C#项目开发实战入门
    3  mrhy6   48.86  李某某  C#项目开发实战入门
    4  mrhy7  104.72  张某某  C语言精彩编程200例
    

4.names
- 默认值为None,要使用的列名列表
    - 不做说明时，names会覆盖excel第0行


```python
df1_2_8 = pd.read_excel(excel_path, names=[0,1,2,3])
print(df1_2_8.head())
```

           0      1       2                   3
    0  mrhy1  41.86  周某某      零基础学Python
    1  mrhy2  41.86  杨某某      零基础学Python
    2  mrhy3  48.86  刘某某      零基础学Python
    3  mrhy4  48.86  张某某      零基础学Python
    4  mrhy5  48.86  赵某某  C#项目开发实战入门
    

- header=None 就不会覆盖了


```python
df1_2_9 = pd.read_excel(excel_path, names=[0,1,2,3], header=None)
print(df1_2_9.head())
```

                0                 1           2               3
    0  买家会员名  买家实际支付金额  收货人姓名        宝贝标题
    1       mrhy1             41.86      周某某  零基础学Python
    2       mrhy2             41.86      杨某某  零基础学Python
    3       mrhy3             48.86      刘某某  零基础学Python
    4       mrhy4             48.86      张某某  零基础学Python
    

- 发现的确是先执行header的结果，再执行names
    - header=2,前两行的都消失了，然后names替换第三行


```python
df1_2_9 = pd.read_excel(excel_path, names=[0,1,2,3], header=2)
print(df1_2_9.head())
```

           0       1       2                   3
    0  mrhy3   48.86  刘某某      零基础学Python
    1  mrhy4   48.86  张某某      零基础学Python
    2  mrhy5   48.86  赵某某  C#项目开发实战入门
    3  mrhy6   48.86  李某某  C#项目开发实战入门
    4  mrhy7  104.72  张某某  C语言精彩编程200例
    

5.index_col
- 指定列为索引列，默认值为None，索引0是DataFrame对象的行标签。

- 列索引就变化了，但是形式变的奇怪起来


```python
df1_2_10 = pd.read_excel(excel_path, index_col=0)
print(df1_2_10.head())
```

                买家实际支付金额 收货人姓名            宝贝标题
    买家会员名                                                 
    mrhy1                  41.86     周某某      零基础学Python
    mrhy2                  41.86     杨某某      零基础学Python
    mrhy3                  48.86     刘某某      零基础学Python
    mrhy4                  48.86     张某某      零基础学Python
    mrhy5                  48.86     赵某某  C#项目开发实战入门
    

- 与header不同的是，index_col不会丢失前面的列


```python
df1_2_11 = pd.read_excel(excel_path, index_col=1)
print(df1_2_11.head())
```

                     买家会员名 收货人姓名            宝贝标题
    买家实际支付金额                                          
    41.86                 mrhy1     周某某      零基础学Python
    41.86                 mrhy2     杨某某      零基础学Python
    48.86                 mrhy3     刘某某      零基础学Python
    48.86                 mrhy4     张某某      零基础学Python
    48.86                 mrhy5     赵某某  C#项目开发实战入门
    

- 可以看出是默认的先header后index_col，不推荐使用index_col，格式异常混乱


```python
df1_2_12 = pd.read_excel(excel_path, index_col=0, header=1)
print(df1_2_12.head())
```

           41.86  周某某      零基础学Python
    mrhy1                                   
    mrhy2  41.86  杨某某      零基础学Python
    mrhy3  48.86  刘某某      零基础学Python
    mrhy4  48.86  张某某      零基础学Python
    mrhy5  48.86  赵某某  C#项目开发实战入门
    mrhy6  48.86  李某某  C#项目开发实战入门
    

6.usecols
- usecols: int,list,字符串，默认为None.
    - None: 解析所有列.
    - int: 解析最后一列.  ```# 已失效!!!  # 必须是[int]```
    - list: 解析列号和列表的列。
    - str: "A:E" or "A,C,E:F"     # 范围包括前后

```# 已失效!!!  # 必须是[int]```


```python
# df1_2_13 = pd.read_excel(excel_path, usecols=2)
# ValueError: Passing an integer for `usecols` is no longer supported.  Please pass in a list of int from 0 to `usecols` inclusive instead.
```


```python
df1_2_13 = pd.read_excel(excel_path, usecols=[2])
print(df1_2_13.head())
```

      收货人姓名
    0     周某某
    1     杨某某
    2     刘某某
    3     张某某
    4     赵某某
    

- 字符串也是要放在列表里才能读出来


```python
# df1_2_14 = pd.read_excel(excel_path, usecols='收货人姓名')
# print(df1_2_14.head())
#  ValueError: Invalid column name: 收货人姓名
```


```python
df1_2_14 = pd.read_excel(excel_path, usecols=['收货人姓名'])
print(df1_2_14.head())
```

      收货人姓名
    0     周某某
    1     杨某某
    2     刘某某
    3     张某某
    4     赵某某
    

- 字符串的用法比较单一，均是报错


```python
# df1_2_18 = pd.read_excel(excel_path, usecols=['买家会员名'，'收货人姓名'])
# # SyntaxError: invalid character in identifier
# df1_2_18 = pd.read_excel(excel_path, usecols=['买家会员名': '收货人姓名'])
# # SyntaxError: invalid syntax
# df1_2_18 = pd.read_excel(excel_path, usecols=[['买家会员名']: ['收货人姓名']])
# # SyntaxError: invalid syntax
# print(df1_2_18.head())
```


      File "<ipython-input-38-47825b072021>", line 5
        df1_2_18 = pd.read_excel(excel_path, usecols=[['买家会员名']: ['收货人姓名']])
                                                               ^
    SyntaxError: invalid syntax
    


- <font color=darkred>还可以用Excel列字母来选取</font>


```python
df1_2_15 = pd.read_excel(excel_path, usecols='A')
print(df1_2_15.head())
```

      买家会员名
    0      mrhy1
    1      mrhy2
    2      mrhy3
    3      mrhy4
    4      mrhy5
    


```python
df1_2_16 = pd.read_excel(excel_path, usecols='A,C')
print(df1_2_16.head())
```

      买家会员名 收货人姓名
    0      mrhy1     周某某
    1      mrhy2     杨某某
    2      mrhy3     刘某某
    3      mrhy4     张某某
    4      mrhy5     赵某某
    


```python
df1_2_17 = pd.read_excel(excel_path, usecols='A:C,D')
print(df1_2_17.head())
```

      买家会员名  买家实际支付金额 收货人姓名            宝贝标题
    0      mrhy1             41.86     周某某      零基础学Python
    1      mrhy2             41.86     杨某某      零基础学Python
    2      mrhy3             48.86     刘某某      零基础学Python
    3      mrhy4             48.86     张某某      零基础学Python
    4      mrhy5             48.86     赵某某  C#项目开发实战入门
    


```python
# df1_2_18 = pd.read_excel(excel_path, usecols=['A'])
# print(df1_2_18.head())
# ValueError: Usecols do not match columns, columns expected but not found: ['A']
'''字符串是列字母专用，列表则可以输入字符串和数字'''
```


```python
# df1_2_18 = pd.read_excel(excel_path, usecols=[0, '收货人姓名'])
# print(df1_2_18.head())
# ValueError: 'usecols' must either be list-like of all strings, all unicode, all integers or a callable.
'''同时列表里int和str是不能混用的'''
```

## read_csv()


- 描述

    - read_csv() 函数导入.csv。

- 语法
```python
pandas.read_csv(filepath_or_buffer,encoding=None)
```

- 参数
    - filepath_or_buffer: 字符串，文件路径，也可以是URL链接.
    - encoding: 字符串，默认值为None，文件的编码格式.
    - header:指定作为列名的行，默认值为0.数据为除列名以外的数据，设置header=None(此时列索引为数字).
    - names: 默认值为None,要使用的列名列表.
    - index_col: 指定列为索引列，默认值为None，索引0是DataFrame对象的行标签。

- 示例


```python
csv_path = r'data/0316.csv'
```

1.常规导入
- 只需要输入path，就可以读取csv

- 注意

python常用的编码格式是UTF-8和GBK格式，默认编码格式为UTF-8。

导入.csv文件时，需要通过encoding参数指定编码格式。

当我们将Excel文件另存为.csv文件时，默认编码格式为GBK。
因此导入.scv文件时，需要保持编码格式保持一致。


```python
df1_3_1 = pd.read_csv(csv_path, encoding='gbk')  # 导入csv文件，并指定编码格式
print(df1_3_1.head(), "\n", "="*50)  # 输出前5条数据
```

      买家会员名  买家实际支付金额 收货人姓名            宝贝标题    订单付款时间 
    0      mrhy1             41.86     周某某      零基础学Python   2018/5/16 9:41
    1      mrhy2             41.86     杨某某      零基础学Python   2018/5/9 15:31
    2      mrhy3             48.86     刘某某      零基础学Python  2018/5/25 15:21
    3      mrhy4             48.86     张某某      零基础学Python  2018/5/25 15:21
    4      mrhy5             48.86     赵某某  C#项目开发实战入门  2018/5/25 15:21 
     ==================================================
    

2. 其余操作与excel基本一致

## # read_txt

- 同样使用csv方法，但是要指定`sep`参数，如：`/t`

- 描述

    - read_csv() 函数导入.txt。

- 语法
```python
pandas.read_csv(filepath_or_buffer,encoding=None)
```

- 参数
    - filepath_or_buffer: 字符串，文件路径，也可以是URL链接.
    - encoding: 字符串，默认值为None，文件的编码格式.
    - header:指定作为列名的行，默认值为0.数据为除列名以外的数据，设置header=None(此时列索引为数字).
    - names: 默认值为None,要使用的列名列表.
    - index_col: 指定列为索引列，默认值为None，索引0是DataFrame对象的行标签。


```python
txt_path = r'data/0317.txt'
```


```python
df1_4_1 = pd.read_csv(txt_path, sep='\t', encoding='gbk')
print(df1_4_1.head())
```

      买家会员名  买家实际支付金额 收货人姓名            宝贝标题    订单付款时间 
    0      mrhy1             41.86     周某某      零基础学Python   2018/5/16 9:41
    1      mrhy2             41.86     杨某某      零基础学Python   2018/5/9 15:31
    2      mrhy3             48.86     刘某某      零基础学Python  2018/5/25 15:21
    3      mrhy4             48.86     张某某      零基础学Python  2018/5/25 15:21
    4      mrhy5             48.86     赵某某  C#项目开发实战入门  2018/5/25 15:21
    

## read_html()

- 语法
```python
pd.read_html()
```
- 参数

    - io:字符串，文件路径，也可以是URL链接。网址不接受https，可尝试去掉https中的s再尝试。
- 说明
    - 使用read_html方法前，首先要确定网页表格是否为table标签。例如，下列NBA网页中右键该网页中的表格，在弹出的菜单中选择"检查元素"
        查看是否含有表格标签`<table>···</table>`的字样。


```python
df_1_5_1 = pd.DataFrame()
url_list = ['http://www.espn.com/nba/salaries/_/seasontype/4']
for i in range(2, 13):
    url = 'http://www.espn.com/nba/salaries/_/page/%s/seasontype/4' % i
    url_list.append(url)
# 遍历网页中的table读取网页表格数据
for url in url_list:
    df_1_5_1 = df_1_5_1.append(pd.read_html(url), ignore_index=True)
# 列表解析：遍历dataframe第3列，以子字符串$开头
df_1_5_1 = df_1_5_1[[x.startswith('$') for x in df_1_5_1[3]]]
print(df_1_5_1.head())
df_1_5_1.to_csv('data/0318.csv', header=['RK', 'NAME', 'TEAM', 'SALARY'], index=False)
```

       0                      1                      2            3
    1  1      Stephen Curry, PG  Golden State Warriors  $43,006,362
    2  2  Russell Westbrook, PG     Washington Wizards  $41,358,814
    3  3         Chris Paul, PG           Phoenix Suns  $41,358,814
    4  4          John Wall, PG        Houston Rockets  $41,254,920
    5  5       James Harden, SG        Houston Rockets  $41,254,920
    

# Series

> 带标签的一维同构数组

- 语法
```python
s = pd.Series(data,index,name,dtype)
```
- 参数

    - data: 表示数据，支持python字典、多维数组、标量值（即只有大小没有方向的量）。
    - index: 表示行标签（索引）
    - name: 列名
    - dtype: 数据类型
    - 返回值：Series对象

## object

1.简单创建一个Series对象
> 可以是一维列表，也可以是一个数


```python
s2_01 = pd.Series([88, 60, 75])
s2_02 = pd.Series(5)
print(s2_01,'\n',s2_02)
```

    0    88
    1    60
    2    75
    dtype: int64 
     0    5
    dtype: int64
    

> 混合类型和多维列表都是可以的


```python
s2_03 = pd.Series([88, '字符串', 75])
s2_04 = pd.Series([88, '字符串', [75, 80]])
print(s2_03)
print(s2_04)
```

    0        88
    1    字符串
    2        75
    dtype: object
    0          88
    1      字符串
    2    [75, 80]
    dtype: object
    

2.index

> 当data参数是多维数组时，index长度必须与data长度一致。
如果没有指定index参数，将自动创建数值型索引（从0~data的数据长度减1）


```python
# 默认的index是0,1,2..
s2_05 = pd.Series([88, 60, 75], index=[1, 2, 3])
s2_06 = pd.Series([88, 60, 75], index=['明日同学', '高同学', '七月流火'])
print(s2_05)
print(s2_06)
```

    1    88
    2    60
    3    75
    dtype: int64
    明日同学    88
    高同学      60
    七月流火    75
    dtype: int64
    

3.name
> Series中也是可以设置列名的


```python
s2_07 = pd.Series([88, 60, 75], name='语文')  # Series也是可以有列名的
print(s2_07)
```

    0    88
    1    60
    2    75
    Name: 语文, dtype: int64
    

## index

1.用位置进行索引
- 位置索引是从0开始，[0]是Series的第一个数


```python
s_index_1 = pd.Series([88, 60, 75, 99])
```

> 对某个值进行索引


```python
print(s_index_1[1])  # 通过一个标签索引获取索引值
```

    60
    

>- 注意：Series对象不能使用[-1]定位索引。


```python
print(s_index_1[-1])  # KeyError: -1
```

> 对多个值进行索引


```python
print(s_index_1[0:2])  # 通过标签位置切片获取索引值
```

    0    88
    1    60
    dtype: int64
    

>- 注意：但是Series对象切片可以使用[：-1]定位索引。


```python
print(s_index_1[0:-1])  # 通过标签位置切片获取索引值
```

    0    88
    1    60
    2    75
    dtype: int64
    


```python
print(s_index_1[0:-2])  # 通过标签位置切片获取索引值
```

    0    88
    1    60
    dtype: int64
    


```python
print(s_index_1[-1:])  # 还可以通过这样的方式获取Series最后一个值
```

    3    99
    dtype: int64
    

2.用标签进行索引
- 标签索引方法与位置索引方法类似，用```[ ]```表示
- 注意：index的数据类型时字符串，多个标签索引值则用```[[ ]]```表示


```python
s_index_2 = pd.Series([88, 60, 75], index=['小明', '小高', '小亮'])
```


```python
print(s_index_2['小明'])
```

    88
    


```python
print(s_index_2[['小明', '小亮']])
```

    小明    88
    小亮    75
    dtype: int64
    

> 同样可以使用切片，但是注意，标签的切片区间是```[ ]```闭区间
>
> 位置的切片区间是```[ )```右开区间


```python
print(s_index_2['小明':'小亮'])
```

    小明    88
    小高    60
    小亮    75
    dtype: int64
    


```python
print(s_index_2[0:2])
```

    小明    88
    小高    60
    dtype: int64
    

> 还有其他小操作


```python
print(s_index_2['小明':])
```

    小明    88
    小高    60
    小亮    75
    dtype: int64
    


```python
print(s_index_2['小明':'小亮':2])
```

    小明    88
    小亮    75
    dtype: int64
    

## #CRUD

1.单个位置重新赋值


```python
s_CRUD_1 = pd.Series([88, 60, 75, 99])
s_CRUD_1[0] = 100
print(s_CRUD_1)
```

    0    100
    1     60
    2     75
    3     99
    dtype: int64
    

> 计算赋值


```python
s_CRUD_4 = pd.Series([88, 60, 75, 99])
s_CRUD_4[0] += 1
print(s_CRUD_4)
```

    0    89
    1    60
    2    75
    3    99
    dtype: int64
    

> 计算赋值:但是注意！类型已经无法改变了。```int```还是```int```
>
> 但是可以提前定义数据类型```dtype```


```python
s_CRUD_5 = pd.Series([88, 60, 75, 99])
s_CRUD_5[0] += 1.5
print(s_CRUD_5)
```

    0    89
    1    60
    2    75
    3    99
    dtype: int64
    


```python
s_CRUD_6 = pd.Series([88, 60, 75, 99])
s_CRUD_6[0] = s_CRUD_6[0]*1.1
print(s_CRUD_6)
s_CRUD_6 = pd.Series([88, 60, 75, 99],dtype=float)
s_CRUD_6[0] = s_CRUD_6[0]*1.1
print(s_CRUD_6)
```

    0    96
    1    60
    2    75
    3    99
    dtype: int64
    0    96.8
    1    60.0
    2    75.0
    3    99.0
    dtype: float64
    

2.多个位置重新赋值


```python
s_CRUD_2 = pd.Series([88, 60, 75, 99])
s_CRUD_2[0:2] = 100
print(s_CRUD_2)
```

    0    100
    1    100
    2     75
    3     99
    dtype: int64
    


```python
s_CRUD_3 = pd.Series([88, 60, 75, 99])
s_CRUD_3[0:2] = [100, 90]
print(s_CRUD_3)
```

    0    100
    1     90
    2     75
    3     99
    dtype: int64
    

> 多个位置也可直接参与运算赋值


```python
s_CRUD_3 = pd.Series([88, 60, 75, 99])
s_CRUD_3[0:2] += 1
print(s_CRUD_3)
```

    0    89
    1    61
    2    75
    3    99
    dtype: int64
    

## #获取索引和值


```python
s2_08 = pd.Series([88, 60, 75])
print(s2_08.index)  # 输出为：RangeIndex(start=0, stop=3, step=1)
print(s2_08.values)  
```

    RangeIndex(start=0, stop=3, step=1)
    [88 60 75]
    


```python
s2_09 = pd.Series([88, 60, 75], index=['明日同学', '高同学', '七月流火'])
print(s2_09.index)  # 输出为：Index(['明日同学', '高同学', '七月流火'], dtype='object')
print(s2_09.values)  # 返回一个numpy.ndarray
print(type(s2_09.index))
print(type(s2_09.values))
```

    Index(['明日同学', '高同学', '七月流火'], dtype='object')
    [88 60 75]
    <class 'pandas.core.indexes.base.Index'>
    <class 'numpy.ndarray'>
    


```python
s2_08 = pd.Series([88, 60, 75])
print(s2_08.values[0])  
```

    88
    

# DataFrame

> 由多种类型的列组成的二维表数据结构，类似于Excel、SQL或Series对象构成的字典。

- 语法
```python
pd.DataFrame(data,index,columns,dtype,copy)
```
- 参数

    - data: 表示数据，支持ndarray数组、Series对象、列表、字典等。
    - index: 表示行标签（索引）
    - columns： 列标签（索引）
    - dtype: 数据类型
    - copy： 复制数据
    - 返回值：DataFrame对象

## #数据类型对应表

|Pandas数据类型|Python数据类型|
| :--- | :--- |
|object|str|
|int64|int|
|float64|float|
|bool|bool|
|datetime64|datetime64[ns]|
|tinmedelta[ns]|NA|
|category|NA|

## object

1.二维数据构建

- 需要注意的是，用二维数据构建```DataFrame```时，```data```的维数等于```DataFrame```的行数


```python
data3_1 = [[10, 20, 30], [40, 50, 60], [70, 80, 90]]
df3_1 = pd.DataFrame(data3_1,columns = ['语文', '数学', '英语'])
print(df3_1)
```

       语文  数学  英语
    0    10    20    30
    1    40    50    60
    2    70    80    90
    


```python
df3_1.transpose()  # 行列转换
print(df3_1)  # 没有变化说明transpose()没有改变自身
print(df3_1.transpose())
```

       语文  数学  英语
    0    10    20    30
    1    40    50    60
    2    70    80    90
           0   1   2
    语文  10  40  70
    数学  20  50  80
    英语  30  60  90
    

2. 字典创建

注意：
通过字典创建时，字典中的value值只能是一维数组或单个的简单数据类型，
- 如果是数组，则要求所有的数组长度一致；
- 如果是单个数据，则每行都需要添加相同数据。


```python
df3_2 = pd.DataFrame({
    '语文': [110, 105, 99],
    '数学': [105, 88, 115],
    '英语': [109, 120, 130],
    '班级': '高一7班'},
    index=[0, 1, 2])
print(df3_2)
```

       语文  数学  英语     班级
    0   110   105   109  高一7班
    1   105    88   120  高一7班
    2    99   115   130  高一7班
    

## #遍历


```python
data = [[110, 105, 99], [105, 88, 115], [109, 120, 130]]
index = [0, 1, 2]
columns = ['语文', '数学', '英语']
df3_3 = pd.DataFrame(data=data, index=index, columns=columns)
# df = pd.DataFrame(data=data, index=index)  # 列名不设置时，默认为RangeIndex
print(df3_3)
print(df3_3.columns)  # 并不是列表，但是可以遍历
print(df3_3.columns[0])  # 带有部分列表的功能
print(len(df3_3.columns))  # 带有部分列表的功能
```

        语文   数学   英语
    0  110  105   99
    1  105   88  115
    2  109  120  130
    Index(['语文', '数学', '英语'], dtype='object')
    语文
    3
    

遍历列


```python
# 遍历DataFrame表格数据的每一列
for col in df3_3.columns:
    # print(col)  # col为一个个列名
    series = df3_3[col]  # 将df一列变为Series
    print(series)
```

    0    110
    1    105
    2    109
    Name: 语文, dtype: int64
    0    105
    1     88
    2    120
    Name: 数学, dtype: int64
    0     99
    1    115
    2    130
    Name: 英语, dtype: int64
    

# 数据抽取


```python
import pandas as pd
pd.set_option('display.unicode.east_asian_width', True)
```

## 抽取行列

DataFrame对象中的loc属性和iloc属性都可以抽取数据，区别如下：
- loc属性：以列名(columns)和行名（index）作为参数，__当只有一个参数时，默认是行名，__即抽取整行数据，包括所有列，如df.loc['A']

- iloc属性：以行和列位置索引（即0,1，2，···）作为参数，0表示第一行，1表示第二行，以此类推。当只有一个参数时，默认是行索引，如df.iloc[0]

loc是指location的意思，iloc中的i是指integer。这两者的区别如下：  
loc：works on labels in the index.  
iloc：works on the positions in the index (so it only takes integers).

1. 抽取行列数据


```python
data = [[110, 105, 99], [105, 88, 115], [109, 120, 130], [112, 115]]
name = ['明日', '七月流火', '高袁圆', '二月二']
columns = ['语文', '数学', '英语']
df4_1 = pd.DataFrame(data=data, index=name, columns=columns)
print(df4_1)
```

              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
    二月二     112   115    NaN
    

### 抽取行数据

loc抽取一行数据


```python
print(df4_1.loc["明日"])
```

    语文    110.0
    数学    105.0
    英语     99.0
    Name: 明日, dtype: float64
    

loc抽取多行数据
- 行名之间用`,`隔开,且是`[[]]`的形式


```python
print(df4_1.loc[["明日","高袁圆"]])
```

            语文  数学   英语
    明日     110   105   99.0
    高袁圆   109   120  130.0
    

loc抽取连续多行数据
- 连续多行不需要`[[]]`的形式


```python
print(df4_1.loc["明日":"高袁圆"])
print(df4_1.loc[:"高袁圆"])
print(df4_1.loc[:"高袁圆":2])
```

              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
            语文  数学   英语
    明日     110   105   99.0
    高袁圆   109   120  130.0
    

iloc与loc同理
- loc列名是`[]`区间，iloc序号是`[)`区间。


```python
print(df4_1.iloc[0])
print(df4_1.iloc[0:3])
print(df4_1.iloc[:3])
print(df4_1.iloc[:3:2])
```

    语文    110.0
    数学    105.0
    英语     99.0
    Name: 明日, dtype: float64
              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
            语文  数学   英语
    明日     110   105   99.0
    高袁圆   109   120  130.0
    

### 抽取列数据

__1. 直接使用列名__


```python
print(df4_1)
```

              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
    二月二     112   115    NaN
    


```python
print(df4_1["数学"])
```

    明日        105
    七月流火     88
    高袁圆      120
    二月二      115
    Name: 数学, dtype: int64
    


```python
print(df4_1[["数学","语文"]])  # 前后关系可以不用对应上
```

              数学  语文
    明日       105   110
    七月流火    88   105
    高袁圆     120   109
    二月二     115   112
    

直接使用列名是不能进行连续提取的，会报错。


```python
# print(df4_1["语文":"英语"])
```

__2.使用loc和iloc__

loc


```python
print(df4_1.loc[:,"语文"])
```

    明日        110
    七月流火    105
    高袁圆      109
    二月二      112
    Name: 语文, dtype: int64
    


```python
print(df4_1.loc[:,["语文","英语"]])
```

              语文   英语
    明日       110   99.0
    七月流火   105  115.0
    高袁圆     109  130.0
    二月二     112    NaN
    


```python
print(df4_1.loc[:,"语文":])
```

              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
    二月二     112   115    NaN
    

iloc
- 与loc方法类似


```python
print(df4_1.iloc[:,0])
```

    明日        110
    七月流火    105
    高袁圆      109
    二月二      112
    Name: 语文, dtype: int64
    

__3. 指定行列__


```python
print(df4_1.loc["七月流火","语文"])
```

    105
    


```python
print(df4_1.loc["七月流火","语文":])
```

    语文    105.0
    数学     88.0
    英语    115.0
    Name: 七月流火, dtype: float64
    


```python
print(df4_1.iloc[1,0])
```

    105
    


```python
print(df4_1.iloc[1,0:])
```

    语文    105.0
    数学     88.0
    英语    115.0
    Name: 七月流火, dtype: float64
    


```python
print(df4_1.iloc[1,0::2])
```

    语文    105.0
    英语    115.0
    Name: 七月流火, dtype: float64
    

## 按条件抽取
(1)取其中的一个元素，如.iax[x,x]  
(2)基于位置查询，如.iloc[2,1]  
(3)基于行列名称的查询，如.loc[x]

- '''常用的条件判断(& | ~ ：与或非)'''


```python
import pandas as pd
pd.set_option('display.unicode.east_asian_width', True)
data = [[110, 105, 99], [105, 88, 115], [109, 120, 130], [112, 115]]
name = ['明日', '七月流火', '高袁圆', '二月二']
columns = ['语文', '数学', '英语']
df4_2 = pd.DataFrame(data=data, index=name, columns=columns)
```


```python
print(df4_2)
```

              语文  数学   英语
    明日       110   105   99.0
    七月流火   105    88  115.0
    高袁圆     109   120  130.0
    二月二     112   115    NaN
    

__loc输出满足条件的行__


```python
print(df4_2.loc[(df4_2["语文"] > 105) & (df4_2["数学"] > 88)])
# print(df4_2[(df4_2["语文"] > 105) & (df4_2["数学"] > 88)])  # 结果一样
```

            语文  数学   英语
    明日     110   105   99.0
    高袁圆   109   120  130.0
    二月二   112   115    NaN
    

：剖析过程

相当于对`["语文"]`这一列进行了判断，返回一个bool型的DataFrame


```python
print(df4_2["语文"] > 105)
# test
# print(df4_2["语文"] > 105 & df4_2["数学"] > 88)
# ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() or a.all().
print((df4_2["语文"] > 105) & (df4_2["数学"] > 88))
print(df4_2[["语文","数学"]] > 105)
```

    明日         True
    七月流火    False
    高袁圆       True
    二月二       True
    Name: 语文, dtype: bool
    明日         True
    七月流火    False
    高袁圆       True
    二月二       True
    dtype: bool
               语文   数学
    明日       True  False
    七月流火  False  False
    高袁圆     True   True
    二月二     True   True
    

结论


```python
judge = (df4_2["语文"] > 105) & (df4_2["数学"] > 88)
print(df4_2[judge])
```

            语文  数学   英语
    明日     110   105   99.0
    高袁圆   109   120  130.0
    二月二   112   115    NaN
    

加入与或非：`& | ~`
- 单独讲讲`~`的用法


```python
judge = ~((df4_2["语文"] > 105) & (df4_2["数学"] > 88))
print(df4_2[judge])
```

              语文  数学   英语
    七月流火   105    88  115.0
    
