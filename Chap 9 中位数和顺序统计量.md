# Chap 9 中位数和顺序统计量

标签（空格分隔）： 算法导论

---

#9.1 最小值和最大值
##本节内容概括
```c
MINIMUM(A)
min = A[1]
for i = 2 to A.length
    if A[i] < A[min]
        min = i
return min
```
上述算法的时间复杂度为线性，根据两两配对查找的方法，最多需要$\frac{3n}{2}$即可查找出最大值和最小值
##练习
###9.1-1
![image_1c2rfq5er1ckv194a1pmt5583tj9.png-166.8kB][1]


  [1]: http://static.zybuluo.com/cheyiwei/5pjpojgvg0umi0pidonge79n/image_1c2rfq5er1ckv194a1pmt5583tj9.png
  
  
#9.2 期望为线性时间的算法
本质上是用到了PARTITION过程
```c
RANDOMZED_SELECT(A,p,r,i)
if p==r
    return A[p]
q=RANDOMZED_PARTITION(A,p,r)
k=q-p+1
if k==i
    return A[q]
else if i<k
    return RANDOMZED_SELECT(A,p,q-1,i)
else
    return RANDOMZED_SELECT(A,q+1,r,i-k)
```
##练习
###9.2-3
```c
RANDOMZED_SELECT(A,p,r,i)
if p==r
    return A[p]
do
    q=RANDOMZED_PARTITION(A,p,r)
    k=q-p+1
    if k==i
        return A[q]
    else if i<k
        r = q-1
    else
        p=q+1
        i=i-k
while(p!=r)
return A[p]
```
#9.3 最坏时间为线性的选择算法
##练习
###9.3-5
```c
SELECT(A,p,r,i)
x=BLACK(A,p,r)
if i==(p+r)/2
    return A[x]
else
    q = PARTITION(A,p,r,x)
    if i < (p+r)/2
        return SELECT(A,p,q-1,i)
    else
        return SELECT(A,q+1,p,i-(q-p+1))
```
###9.3-7
```c
a=SELECT(A,1,n,n/2)
for i = 1 to n
    T[i] = A[i] - A[a]
k = SELECT(T,1,n-1,k)
j=0
for i = 1 to n
if T[i]<=T[k]
    S[j++]=A[i]
```
 