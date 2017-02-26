title: Python 学习第一课
date: 2016-11-05 11:54:5
mathjax: true
categories: Python
---

今天开始学习python，用的教程主要是网上的mooc，为了更好地梳理自己的学习思路，特将一些比较重要的点记录下来。

# 寻找第n个默尼森数
寻找第n个默尼森数。
代码格式如下：
```
def prime(num):  
...
def monisen(no):  
… …  
return xxx
print monisen(input())#此处不需要自己输入，只要写这样一条语句即可，主要完成monisen()函数
# print(monisen(int(input()))) in Python 3.x（5分）
```

题目内容：找第n个默尼森数。P是素数且M也是素数，并且满足等式$M=2^P-1$，则称M为默尼森数。例如，$P=5，M=2^P-1=31$，5和31都是素数，因此31是默尼森数。
输入格式:用input()函数输入，注意如果Python 3中此函数的返回类型
输出格式：int类型
输入样例：4
输出样例：127
时间限制：500ms内存限制：32000kb

<!--more-->

## 解决方法
 思路是通过枚举所有的p（即素数），来寻找第n个默尼森数。初始时刻，设定一定长度的数字列表来寻找素数，然后在素数表中找默尼森数，当找到一个默尼森数，就看看当前的默尼森数是否是第n个。如果在当前素数表中未找到第n个默尼森数，则需要增加素数，继续查找。也就是通过递归的方法，直到找到第 n个默尼森数为止。
## 代码
``` python
# -*- coding: utf-8 -*-
"""
Created on Sat Oct 01 18:32:41 2016

@author: dukeyu
"""

from math import sqrt 


# 缓存默尼森数 
cacheMonisen = [] 

# 缓存素数 
cachePrime = [] 

# 素数范围，搜索更多的素数 
minNumber = 2 
maxNumber = 100

def isPrime(x): 
    '''判断是不是素数''' 
    k = int(sqrt(x)) 
    for i in range(2, k + 1): 
        if x % i == 0: 
            return False 
    return True 
def addCachePrime(): 
    '''增加素数缓存''' 
    for i in range(minNumber, maxNumber): 
        if isPrime(i) and (i not in cachePrime): 
            cachePrime.append(i) 
def monisen(no):
    '''求解第N个默尼森数'''
    
    # 初始化素数缓存 
    addCachePrime()
    
    if no <= len(cacheMonisen): 
        return cacheMonisen[no - 1]
        
    for i in cachePrime: 
        # M和P均为素数 
        p = i
        m = pow(2, p) - 1
        if isPrime(p) and isPrime(m):
            cacheMonisen.append(m) 
        if no == len(cacheMonisen): 
            break 
        
    if no == len(cacheMonisen):
        return cacheMonisen[-1]
        
    else: 
        # 素数范围不足，须增加素数，继续查找 
        global minNumber, maxNumber
        minNumber, maxNumber = maxNumber, maxNumber * 2 
        return monisen(no) 
print monisen(input())  
```
# Python中元组，列表，字典的区别
Python中,有3种内建的数据结构:列表、元组和字典。
##1.列表

list是处理一组有序项目的数据结构，即你可以在一个列表中存储一个序列的项目。列表中的项目。列表中的项目应该包括在方括号中，这样python就知道你是在指明一个列表。一旦你创建了一个列表，你就可以添加，删除，或者是搜索列表中的项目。由于你可以增加或删除项目，我们说列表是可变的数据类型，即这种类型是可以被改变的，并且列表是可以嵌套的。
实例：
```
    #coding=utf-8
    animalslist=['fox','tiger','rabbit','snake']
    print "I don't like these",len(animalslist),'animals...'
    
    for items in animalslist:
         print items
    
    print "\n操作后"  
```
 
- 对列表的操作,添加,删除，排序
```
animalslist.append('pig')
del animalslist[0]
animalslist.sort()
for i in range(0,len(animalslist)):
    print animalslist[i]
```
结果：
```
I don't like these 4 animals...
fox tiger rabbit snake
操作后
pig rabbit snake tiger
```

##2.元组

元祖和列表十分相似，不过元组是不可变的。即你不能修改元组。元组通过圆括号中用逗号分隔的项目定义。元组通常用在使语句或用户定义的函数能够安全的采用一组值的时候，即被使用的元组的值不会改变。元组可以嵌套。
```
>>> zoo=('wolf','elephant','penguin')
>>> zoo.count('penguin')
1
>>> zoo.index('penguin')
2
>>> zoo.append('pig')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'tuple' object has no attribute 'append'
>>> del zoo[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object doesn't support item deletion
```
##3 字典
字典类似于你通过联系人名称查找地址和联系人详细情况的地址簿，即，我们把键（名字）和值（详细情况）联系在一起。注意，键必须是唯一的，就像如果有两个人恰巧同名的话，你无法找到正确的信息。
     键值对在字典中以这样的方式标记：
     ``d = {key1 : value1, key2 : value2`` }。注意它们的键/值对用冒号分割，而各个对用逗号分割，所有这些都包括在花括号中。另外，记住字典中的键/值对是没有顺序的。如果你想要一个特定的顺 序，那么你应该在使用前自己对它们排序。
实例：
```
#coding=utf-8
dict1={'zhang':'张家辉','wang':'王宝强','li':'李冰冰','zhao':'赵薇'}
#字典的操作，添加，删除，打印
dict1['huang']='黄家驹'
del dict1['zhao']
for firstname,name in dict1.items():
    print firstname,name
```
结果：
```
li 李冰冰
wang 王宝强
huang 黄家驹
zhang 张家辉
```
# 参考文献
1. http://blog.csdn.net/yuyu391/article/details/52718860
2. http://blog.csdn.net/yasi_xi/article/details/38384047
