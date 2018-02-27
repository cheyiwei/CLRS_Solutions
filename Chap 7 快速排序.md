# Chap 7 快速排序

标签（空格分隔）： 算法导论

---

#7.1 快速排序概述
##本节内容概括

注意，快排和Merge都是划分，其中快排是先PARTITION后划分，MERGE是先划分后合并
```c
QUICKSORT(A,p,r)
if p<r
    q=PARTITION(A,p,r)
    QUICKSORT(A,p,q-1)
    QUICKSORT(A,p,q+1)
```
```c
PARTITION(A,p,r)
x = A[r]
i = p - 1
for j = p to r - 1
    if A[j] <= x
        i++
        swap(A[j],A[i])
swap(A[i+1],A[r])
return i+1
```
注意PARTITION过程是利用i来进行划分的

```c
//A是数组,p,r为上下标
PARETITION(A,p,r)
for i = p to r
    if(A[i]%2==0)
        temp = A[i]
        break
if(i==r)
    return   //第一个偶数为最后一个元素或者没有偶数，结束
else
    temp = A[i] //这是原数组中第一个偶数
while(i!=r) 
    A[i]=A[i+1]
    i++     
A[r]=temp   //使得第一个偶数位于数组末尾，其余元素相对位置不变
i = p - 1   //下面为改制Partition过程
for j = p to r - 1
    if A[j] % 2 == 1
        i++
        swap(A[j],A[i])
swap(A[i+1],A[r])
return i+1//返回第一个偶数所在的坐标位置
```

##练习
###7.1-1
略
###7.1-2
q = r
```c
if i + 1 == r
    return (p+r)/2
```
###7.1-4
将PARTITION中的大于等于改成小于等于
#7.2&&7.3 快速排序的性能和随机化版本

快速排序的性能和划分的情况有关

随机化：
```c
RANDOMIZED_PARTITION(A,p,r)
i = RANDOM(p,r)
exchange(A[i],A[r])
PARTITION(A,p,r)
```
##练习
###7.2-2
$\Theta(n^2)$
###7.2-5
这个根据树的层数来证明即可，每层的代价都为线性的$cn$
















