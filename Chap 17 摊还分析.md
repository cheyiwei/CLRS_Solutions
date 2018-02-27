# Chap 17 摊还分析

标签（空格分隔）： 算法导论

---

#17.1 聚合分析
##本节内容概括
```c
MULTIPOP(S,k)
while not STACK_EMPTY(S) && k>0
    POP(S)
    k--
```
有n个PUSH,POP,MULTIPOP的操作序列通过聚合分析，每个操作的渐进时间为$O(1)$
```c
INCREMENT(A)
i=1
while i<=A.length && A[i]=1
    A[i]=0
    i++
if i<=A.length
    A[i]=1
```
计数器，A[1]翻转n次，那么A[2]翻转$\frac n2$次，$\ldots$，A[i]翻转$\frac {1}{2}^{i-1} * n$次

全部相加，可知每个操作的摊还代价为$\Theta(1)$

##练习
###17.1-1
不是
###17.1-2
```c
DECREMENT(A)
k=A.length
i=1
while i<=A.length && A[i]=0
    A[i]=1
    i++
if i<=A.length
    A[i] = 0
```
加入这个操作后，当这n个操作序列为:
$DECREMENT+INCREMENT+DECREMENT+INCREMENT+...$即可

#17.2 核算法

##本节概念概括

介绍了核算法，使用核算法有点像充值扣费的感觉

##练习

###17.2-2

对第i个操作，如果是2的幂次，我们设其摊还代价为$2^{i+1}-2^{i-1}+1$

#势能法

##本节内容概括

势函数就是将数据结构映射为实数的函数！

##练习
###17.3-1
$\Phi^{'}(D_i)=\Phi (D_i)-\Phi (D_0)$即可
###17.3-3
![image_1c2ucqhm5q6u5v3sj311c9f649.png-39.8kB][1]


  [1]: http://static.zybuluo.com/cheyiwei/mkz8k5uq99nfu6y4gz8z7tzt/image_1c2ucqhm5q6u5v3sj311c9f649.png


  
  
  
  
  
  
  
  
  
  
  
  
  