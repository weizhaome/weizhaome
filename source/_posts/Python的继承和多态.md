title: Python的继承和多态
author: weizhao
date: 2019-10-05 21:27:32
tags:
---
# Python类的继承 
定义一个父类Person:  

	class Person(object):
    	def __init__(self,name,sex):
        	self.name = name
            self.sex = sex
        def print_title(self):
        	if self.sex == "male":
            	print("man")
            else self.sex == "female":
            	print("woman")
定义一个Child类继承父类Person:

	class Child(Person):
    	pass
新建子类和父类的对象：
	
	May = Child("May","female")
    Peter = Person("Peter","male") print(May.name,May.sex,Peter.name,Peter.sex)#子类将继承父类的方法和属性
    May.print_title()
    Peter.print_title()
## Python两个判断继承的函数：isinstance()和issubclass()  
isinstance()用于检查实例的类型,issubclass()用于检查类继承：

	isinstance(May,Child) == True
    issubclass(Child,Person)==True
# Python类的多态  
子类可以重写父类的方法，如果子类在创建时重新父类的方法则父类的方法被覆盖，如果不重写则可以直接使用父类的方法。
	
    class Child(Person):#重写print_title()方法               
    	def print_title(self):
        	if self.sex == "male":
            	print("boy")
        	elif self.sex == "female":
            	print("girl")
# Python使用下划线作为变量前缀和后缀制定特殊方法/变量  
* __foo被看作是类”私有的“，类外和子类都不能访问，不能用‘from module import *’导入，只能通过类对象访问。
* _foo 被看作是类”受保护的“，只有类对象和子类对象自己能访问，不能通过‘from module import *’导入。
* \__foo__ 代表Python里特殊方法专用标识。

# Python的构造函数\__new\__()和初始化函数\__init\__()  
* “\__new\__(cls,*args,\**kargs)“方法在Python中是真正的构造方法（创建并返回实例），是在类实例化对象时第一个调用的方法，将返回实例对象，通过这个方法可以产生一个”cls”对应的实例对象，所以说”\__ new\__(cls,*args,\**kargs)”方法一定要有返回

* 对于”\__ init\__ (self,*args,\**kwags)“方法，是一个初始化的方法，“self”代表由类产生出来的实例对象，” \__init\__(self,*args,\**kwags)”将对这个对象进行相应的初始化操作。