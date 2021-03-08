title: Python数据结构之栈
author: weizhao
date: 2019-10-17 21:25:13
tags:
---
# 基础知识 
栈是一种先进后出的数据结构，可以看成是一种特殊的链表，每次添加的元素都将成为头节点，每次取元素时都先取头结点。
# 栈的基本操作包括：
~~~
class Node(object):
    def __init__(self,data,next = None):
        self.data = data
        self.next = next
class Stack(object):
    def __init__(self,top=None):
        self.top = top
    def push(self,data):
        self.top = Node(data,self.top)#将元素插入到头结点的前面并成为头结点
    def pop(self):#将元素的头结点取出
        if self.top is None:
            return None
        data = self.top.data
        self.top = self.top.next
        return data
    def peek(self):#查看栈顶元素，原来的栈不变
        return self.top.data if self.top is None else None
    def printStack(self):#打印栈元素
        while self.top:
            print(self.pop())
        return 
    def isEmpty(self):#判断栈是否为空
        if self.top is None:
            return True
        else:
            return False
if __name__=="__main__":
    stack = Stack()
    stack.push(1)
    stack.push(2)
    stack.printStack()
~~~
输出：
~~~
2
1
~~~
# 逆转字符串
~~~
def revString(mystr):
	stack = Stack()
    for i in mystr:
    	stack.push(i)
    revStr = ''
    while not stack.isEmpty():
    	revStr += stack.pop()
    return revStr
~~~