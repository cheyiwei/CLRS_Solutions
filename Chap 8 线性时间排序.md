# Chap 8 线性时间排序

标签（空格分隔）： 算法导论

---
#8.1 排序算法的下界
##本节内容概括
基于比较的排序算法，其元素的比较过程可以用一棵判定树来表示
通过判定树可以证明任何基于比较的排序算法时间的渐进下界都为$\Omega(n\log n)$
##练习
#8.2 计数排序
##本节内容概括
计数排序是一个稳定的排序算法，其运行渐进时间为线性$\Theta(n+k)$
```c
COUNTING_SORT(A,B,k)
let C[0...k] be a new arrays
for i=1 to A.length
    C[A[i]]++
for i=0 to k
    C[i] = 0
for i=1 to k
    C[i] += C[i-1]
for i=A.length downto 1
    B[C[A[i]]] = A[i]
    C[A[i]]--
```

##练习
###8.2-3
正确的，但是不稳定了
###8.2-4
```
COUNTING(A,a,b)
let C[0...k] be a new arrays
for i=1 to A.length
    C[A[i]]++
for i=0 to k
    C[i] = 0
sum = 0
for i=a to b
    sum += C[i]
```
#8.3 基数排序

##本节内容概括

在本节中放入引理8.4，初看时难以理解，是中文版的翻译有一点问题，在这里的b和r的位数都是bit位，这样就易于理解了
```c
RADIX_SORT(A,d)
for i to d
    use a stable sort arrat A on digit i
```

##练习
###8.3-4
将所有的整数当作以n为基进行排序，d = 3
这样时间复杂度为$O(3(n+n))$


#8.4 桶排序

##本节内容概括
```c
BUCKET_SORT(A)
n=A.length
let B[0...n-1] be a new arrays
for i=0 to n-1
    make B[i] be an empty list
for i=1 to A.length
    insert A[i] to B[n*A[i]]
for i=0 to n-1
    使用插入排序对list B[i] 排序
contatenate the lists B[0],B[1],..,B[n-1] in order
```

##8.4-4
需要使每个桶的大小尽可能相等
$n$个桶，对应的$d_i$的大小应为：
$(0,\frac {\sqrt 1} n),(\frac {\sqrt 1} n,\frac {\sqrt 2} n)$
共$n$个桶
所以在算法中将第7行改为
```c
inser A[i] into B[n*n*A[i]]
```




