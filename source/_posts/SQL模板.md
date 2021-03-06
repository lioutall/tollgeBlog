---
title: SQL模板
date: 2018-05-27 17:49:20
tags:
---

# sql-engine
简单的sql模板引擎, 非常小, 基本不需要引入外部包(你改几行代码就可以不引入任何外部包),首次sql加载时需要语法运算,后续调用基本无性能开销.[地址](https://github.com/lioutall/sql-engine)    
主要功能如下:

#### sql模板
```
select *
from tab
where a = #a#
:if b='bb' ::
and b = #b#
:else
and b != #b#
if:
```
java代码
```
Map<String, Object> params = new HashMap<>();
params.put("a", "a");
params.put("b", "bbb");
SqlTemplate.generateSQL("test.testIf", params)
```
#### 输出:
> sql:  select * from tab where a = ? and b != ?   
params:  [a, bbb]


## 语法
### IF
> if的条件规则后面会讲
```
:if a==1 ::
  mix
  if:
```
### IF_ELSE
```
:if a==1 ::
  mix
  :else
  mix
  if:
```
### FOR
```
:for a:as ::
  mix
  for
```
### FOR_WITH
> b是字符串, 看test样例
```
:for a:as :with b ::
  mix
  for:
```
### SUB
> 子模板, 看test样例
```
:sub test.testSub sub:
```
## IF条件
IF条件引擎是用的开源的[jexparser](https://gitee.com/drinkjava2/jexparser)   
它只有3个类, 直接copy到工程里了

目前支持:
```
>  <  =  >=  <=  
+  -  *  /  
or  and  not  
'  ( )  ?  0~9 . 
equals  equalsIgnoreCase  contains  containsIgnoreCase  
startWith  startWithIgnoreCase  endWith  endWithIgnoreCase
```
在它基础上新增了一个关键字: isNotNull, 用法见test样例

### 使用
sql模板目录: mapper   
sql模板文件后缀: .md  (是的,就是markdown)   
模板文件构成:
```
## testIf      ->   sql的key
> 注释:测试If   -> 注释
```sql         -> sql模板内容

```sub         -> 子模板,只能被sql模板引用

```
SqlTemplate类初始化时加载所有sql模板, 并保存为 xxx.testIf, xxx是文件名, testIf是sql的key.
你只需要调用:
```
SqlTemplate.generateSQL("xxx.testIf", params)
```

### 其他
有时候解析不出来, 可以试试在#a#之类前后加空格.
如果还解决不了, 把无法解析的地方提取到子模板里去,比如例子里面的:
```
## dateformat
'yyyy-mm-dd hh24:mi:ss'
```
因为使用了:(冒号)作为语法符号, 导致没办法对日期格式解析. 暂时不想替换掉冒号, 有洁癖的哥们可以自行修改成其他.

