title: Python数据结构之链表
author: weizhao
date: 2019-10-17 20:34:00
tags:
---
# 链表的基本元素有：  
* 节点：每个节点包含两个部分，左边部分称为值域，用来存放数据；右边部分称为指针域，用来存放指针指向下一个元素。 
* head节点：指向链表的第一个节点。 
* tail节点：指向链表的最后一个节点。 
* None值:链表的最后一个节点的指针域为None值
节点类的定义如下:
~~~
class Node:
    def __init__(self,val=None,next=None):
    	self.val = val #值域
        self.next = next #指针域
~~~
# 链表的定义方法一：
可以先定义一个一个的节点，然后再把节点的关系表示出来
~~~
node1 = Node(1)
node2 = Node(2)
node3 = Node(3)
node1.next = node2
node2.next = node3
~~~
 
# 链表的定义方法二：定义链表类及其基本操作     
~~~
class LinkedList(object):
    def __init__(self,head = None):
        self.head = head
    def __len__(self):#计算链表长度
        curr = self.head
        counter = 0
        while curr is not None:
            counter +=1
            curr = curr.next
        return counter
    def printNode(self):#打印链表元素
        node = self.head
        while node is not None:
            print(node.val)
            node = node.next
        return
    def insertToFront(self,data):#从前面插入节点
        if data is not None:
            node = Node(data,self.head) #生成一个节点，值域为data,指针指向头结点。
            self.head = node #将插入的节点设置为头结点
            return node
    def append(self,data):#从后面添加节点
        if data is not None:
            node = Node(data)
            if self.head is None: #如果原链表为空则插入的节点为投机诶单
                self.head = node
                return node
            curr_node = self.head
            while curr_node.next is not None:#如果原链表不为空则找到链表的最后一个节点，让最后一个节点指向新加入的节点。
                curr_node = curr_node.next
            curr_node.next = node
            return self.head
    def delete(self,data):#删除数值为data的节点，判断当前节点的下一个节点的值是否等于要删除的值，如果是则将当前节点的指针指向当前结点下一个节点的下一个节点。
        if self.head is None:
            return
        if self.head.val == data:
            self.head = self.head.next
            return
        curr_node = self.head
        while curr_node.next is not None:
            if curr_node.next.val == data:
                curr_node.next = curr_node.next.next
                return
            curr_node = curr_node.next
    def deleteRepeat(self):#删除链表中重复的元素，初始化一个空的列表用来存放已经出现过的链表元素，查看当前节点的下一个节点的值是否已经出现在列表中如果出现则将其删除。
        uniq = []
        if self.head is None:
            return
        curr_node = self.head
        uniq.append(self.head.val)
        while curr_node.next is not None:
            if curr_node.next.val in uniq:
                curr_node.next = curr_node.next.next
            else:
                uniq.append(curr_node.next.val)
                curr_node = curr_node.next
       	return 
~~~
## 测试：
~~~
tmp = LinkedList()
tmp.append(1)
tmp.append(2)
tmp.insertToFront(1)
tmp.append(3)
tmp.append(4)
tmp.append(5)
tmp.deletere()
tmp.printNode()
~~~
## 输出为：
~~~
1
2
3
4
5
~~~