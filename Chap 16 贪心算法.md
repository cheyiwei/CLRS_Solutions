# Chap 16 贪心算法

标签（空格分隔）： 算法导论

---

#16.1 活动选择问题

贪心算法求解该问题，本质是求紧跟在当前结束问题之后的一个最早结束问题，在输入之前已经按照最早结束时间排好序了，设置一个虚拟的a[0]活动，其中f[0]=0，有：
```c
RECURSIVE_ACTIVITY_SELECTOR(s,f,k,n)
m=k+1
while m<=n && s[m]<f[k]
    m=m+1
if m<=n
    return {a_m}+RECURSIVE_ACTIVITY_SELECTOR(s,f,m,n)
else
    return NULL
```
迭代的贪心算法
```c
GREED_ACTIVITY_SELECTOR(s,f)
n=s.length
A={a_1}
k=1
for m = 2 to n
    if s[m] >= f[k]
        A=A+{a_m}
        k=m
return A
```
##练习

###16.1-1
```c
ACTIVITY_SELECTOR(s,f)
let c[1...n-1,1....n],k[1...n,1...n] be new arrays
for i=1 to n-1
    c[i][i] = 0
for l = 1 to n
    for i=0 to n-l+1
        j=i+l-1
        c[i][j] = 0
        while s[m] < i
            m++
        while s[m]>=i && f[m] <= j
            if c[i][j] < c[i][s[m]] + c[f[m]][j] + 1
                c[i][j] = c[i][s[m]] + c[f[m]][j] + 1
                k[i][j] = m
            m++
return c and k
```

###16.1-3

反例略

#16.2 贪心算法原理

##本节内容概括
介绍了贪心问题和动态规划问题的不同，“贪心选择性质”
##练习
###16.2-2
###16.2-3
先拿轻的，再拿重的即可
###16.2-5
首先排序，取最小的设置单位长度，从原点集中去掉后重复本过程即可

#16.3 赫夫曼编码
##本节内容概括

```c
HUFFUMAN(C)
n=|C|
Q=C
for i = 1 to n - 1
    allocate a new node z
    z.left = x = EXTRACT_MIN(Q)
    z.right = y = EXTRACT_MIN(Q)
    z.freq = x.freq + y.freq
    INSERT(Q,z)
return EXTRECT_MIN(Q)
```
当使用最小堆
时间复杂度：$\Theta(n\log n)$
当使用van Emda Boas 时，时间复杂度：$\Theta(n\lg \lg n)$

将赫夫曼编码扩充到k叉树，主要方法：
在字符集中增添若干个频率为0的结点，使得$(n-1)\mod (k-1) = 0$
```c
HUFFUMAN(C,k)//生成k叉树
n=|C|
Q=C
for i= (n-1) mod (k-1) to k-1
    INSERT(Q,0)
for i = 1 to (n-1)/(k-1)
    allocate z be a new node
    z.freq = 0
    for j = 1 to k
        z.child[j]=EXTRACT_MIN(Q)
        z.freq = z.child[j].freq
    INSERT(Q,z)
return EXTRACT_MIN(Q）
```
##练习
###16.3-2
按照哈夫曼算法的构造，一定是一棵满的二叉树
###16.3-3
$a:0000000$
$b:0000001$
$c:000001$
$d:00001$
$e:0001$
$f:001$
$g:01$
$h:1$

###16.3-5
利用反证法即可










