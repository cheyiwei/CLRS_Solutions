# Chap2 算法基础

标签（空格分隔）： 算法导论

---

#2.1 插入排序
##小节内容概括
```c
INSERTION_SORT
for i = 2 to n
    key = A[i]
    j = i - 1
    while(j>0 && A[j] > key)
        A[j+1]=A[j]
        j--
    A[j+1]=key
```
>在上面的算法复写中，我在第5行忽略了j>0这个条件，在第8行将j+1误认为是j

在本章中还介绍了循环不变式的概念，需要注意的是，循环不变式需要注意以下三个特点：在循环开始阶段为真，若在某次循环开始前为真，则在循环结束后为真，终止条件为我们提供了证明算法正确性的工具

##练习

###2.1-1
图略
###2.1-2
```c
//按照升序重写
INSERTION_SORT_UPFIX
for i = 2 to n
    key = A[i]
    j = i - 1;
    while(j > 0 && A[j] < A[j+1])
        A[j+1]=A[j]
        j--
    A[j+1]=key
```
###2.1-3
注意本题中文版翻译有误，以下为英文原版
![image_1c2nmgmh0ad6t9uo0pg7v1gg19.png-43.7kB][1]
  [1]: http://static.zybuluo.com/cheyiwei/drxfxjty9g5kmj7xfn1tddle/image_1c2nmgmh0ad6t9uo0pg7v1gg19.png
```c
SEARCH(A,v)
for i = 1 to A.length
    if v == A[i]
        return i
return NIL
```
循环不变体 : A[1...i - 1]中不存在和v值相等的元素

初始：i = 1,A[1,0]中没有元素，自然不存在和v值相等的元素

注：在下面证明中的i指的是每次循环结束后的i
假设每次迭代开始阶段，A[1...i - 2]中不存在和v值相等的元素，那么在循环中如果A[i - 1]=v，则中断循环，返回i，否则结束循环，此时A[1...i - 1]中不存在和v值相等的元素

在循环终止时，如果i<=A.length,那么返回i值满足条件A[i]=v，若i>A.length，那么就是i=A,length - 1说明A[1...i - 1]，即A[1...A.length]不存在v值,返回NIL，算法正确。

###2.1-4
INPUT: $A[1...n],B[1...n],\forall i \in [1..n],A[i] = 0 and A[i] = 1,B[i]=0 and B[i]=1$
OUTPUT: $C[1...n+1],C是AB两个二进制数的和,\forall i \in [1..n],C[i] = 0 and C[i] = 1$
```c
ADD_BIT(A,B)
let C[1...A.length + 1] be new arrays
for i = A.length to 1
    C[i+1] += A[i] + B[i]
    while(C[i+1] > 2)
        C[i+1]-=2
        C[i]+=1
```
#分析算法
##小节内容概括
分析了插入排序的时间复杂度
告诉我们本书的算法分析是建立在RAM模型上的
表示最坏情况往往和平均情况的增长量级一样
##习题
###2.2-1
$\Theta(n^3)$
###2.2-2
```c
SELECT_SORT(A)
for i = 1 to A.length
    min = i
    for j = i+1 to A.length
        if(A[j]<A[min])
            min = j
    swap(A[min],A[i])
```
循环不变式：A[1...i-1]中的元素均大于A[i-1...A.length]中的元素

最好情况和最坏情况运行时间都是$\Theta(n^2)$

###2.2-3
平均需要查找$\frac n2$次最坏情况下需要查找$n$次
$\Theta(n)$
###2.2-4
增加一些常数时间可以运行完的代码，减少循环的可能性，覆盖一些特例

#设计算法
##小节内容概括
###分治排序
```c
/*A[p..q],A[q+1...r]*/
MERGE(A,p,q,r)
let B[1..q - p+2] and C[1...r-q+1] be new arrays
/*初始化B和C*/
for i = p to q
    B[i-p+1]=A[i]
B[i-p+1]=inf
for i = q + 1 to r
    C[i-q] = A[i]
C[i-q]=inf
/*MERGE过程*/
i=j=k=1
while(i<=r-p+1)
    if(B[j]<C[k])
        A[i] = B[j]
        j++
        i++
    else
        A[i] = C[k]
        k++
        i++
        
/*下面为排序算法*/
MERGE_SORT(A,p,q,r)
if p<q
    mid = (p+q)/2
    MERGE_SORT(A,p,mid,r)
    MERGE_SORT(A,p,mid+1,r)
    MERGE(A,p,q,r)
```

在本节中，利用递归树给出了时间复杂度$\Theta(nlog{n})$
在第四章中可以通过主定理来判断
##练习
###2.3-2
```c
MERGE(A,p,q,r)
let B[1..q - p+1] and C[1...r-q] be new arrays
/*初始化B和C*/
for i = p to q
    B[i-p+1]=A[i]
for i = q + 1 to r
    C[i-q] = A[i]
/*MERGE过程*/
i=j=k=1
while(j <= B.length && k <= C.length )
    if(B[j]<C[k])
        A[i] = B[j]
        j++
        i++
    else
        A[i] = C[k]
        k++
        i++
while(k <= C.length)
    A[i] = C[k]
        k++
        i++
while(j <= B.length)
    A[i] = B[j]
        j++
        i++
```
###2.3-4
$$
        T(n) =
        \begin{cases}
        \Theta(1),  & \text{if $n = 1$} \\
        T(n-1)+\Theta(n), & \text{else}
        \end{cases}
$$
###2.3-5
```c
BINARY_SEARCH(A,p,q,v)
if p <= q
    mid = (p+q)/2
    if(A[mid]==v)
        return mid
    else if(A[mid]>v)
        return BINARY_SEARCH(A,p,mid-1,v)
    else
        return BINARY_SEARCH(A,mid+1,q,v)
return NIL
```
递归式为：
$$
        T(n) =
        \begin{cases}
        \Theta(1),  & \text{if $p-q+1 = 1$} \\
        T(n/2)+\Theta(1), & \text{else}
        \end{cases}
$$

画递归树，从直观上可以看出，时间复杂度为$\Theta(\log{n})$

###2.3-6

我认为不能，因为while循环执行的功能不只是查找，还有将所有大于key的元素右移一位来给key空出位置

###2.3-7

基本思想，使用一次排序，然后n次查找，时间复杂度$\Theta(n\log{n})$

```c
FIND_SUM(S,x)
MERGE_SORT(S,1,S.length)
for i = 1 to S.length
    if((key=BINARY_SEARCH(S,1,S.length,x-S[i]))!=NIL)
        return key
return NIL
```









  
  
  
  
  
  
