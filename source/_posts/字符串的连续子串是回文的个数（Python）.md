title: 字符串的连续子串是回文的个数（Python）
author: weizhao
date: 2019-10-21 21:37:01
tags:
---
# 字符串的连续子串是回文的个数
* 首先编写一个函数用来判断字符串是否是回文：
~~~
def isPalindrome(mystr):
    l = len(mystr)
    i = 0
    count = 0
    while i<(l/2):
        if(mystr[i]==mystr[l-i-1]):
            count += 1
            i += 1
        else:
            count = 0
            break
    if(count==0):
        return 0
    else:
        return 1
~~~
* 找出字符串所有的连续子串并分别判断是否为回文：
~~~
def palindNum(mystr):
    sub_str = []
    ls = len(mystr)
    count = 0
    for i in range(1,ls):#i控制步长
        for j in range(ls-i+1):#j控制位置
            sub_str.append(mystr[j:j+i])
    for i in sub_str:
        count += isPalindrome(i)
    return count
~~~