# Chap 15 线性规划

标签（空格分隔）： 算法导论

---

在开始这一节之前，需要知道最优化问题，以及我们求解的以下问题需要具有最优子结构，即问题的最优解由全局的最优解组成，而每个子问题可以独立求解

#15.1 钢条切割

##本节内容概括

钢条切割问题的本质可以用如下描述：
    $r_n=\max{(p_i,p_1+r_{n-1},...,p_{n-1}+r_1)}$
即：
$r_n=\max_{1\leq i\leq n}{(p_i+r_{n-i})}$

由此可以写出算法伪代码如下：
```c
CUT_ROD(p,n)
if n=0
    return 0
else
    q=-infty
    for i = 1 to n
        q=max(q,p[i]+CUT_ROD(p,n-i))
return q
```
注意这是自顶向下递归算法，实际在计算机运行过程中还是自底向上计算

将其转化为动态规划问题就是增加一个备忘机制，记录所有计算过的长度小于n的最大收益，如下：

自顶向下：
```c
MEMOIZED_CUT_ROD(p,n)
for i = 0 to n
    r[i] = -infty
return MEMOIZED_CUT_ROD_AUX(p,n,r)

MEMOIZED_CUT_ROD_AUX(p,n,r)
if(r[n]>0)
    return r[n]
else if(n=0)
    return 0
else
    for i = 1 to n
        q=max(q,p[i]+MEMOIZED_CUT_ROD_AUX(p,n-i,r))
r[n]=q
return q
```
自底向上就比较简单了
```c
BOTTOM_UP_ROD(p,n)
let r[0...n] be a new arrays
r[0]=0
for i=1 to n
    q=-infty
    for j=1 to i
        q=max(q,p[j]+r[i-j])
    r[i]=q
return r[n]
```

注意在这里对q的理解，按照围殴自己的理解是在所有已经比较过的切割方案中最大的那个方案代表的收益

两种方法的时间复杂度为：$\Theta(n^2)$

除此之外，还向我们介绍了子问题图这个有趣的分析动态规划问题运行时间的方法
```c
EXTEND_BOTTOMUP_CUT_ROD(p,n)
let r[0..n],s[0..n] be a new arrays
r[0]=s[0]=0
for i = 1 to n
    q=-infty
    for j = 1 to i
        if q <p[j]+r[i-j]
            q = p[j]+r[i-j]
            s[i] = j
    r[i]=q
return r and s
```

```c
PRINT_CUT_ROD_SOLUTION(s,n)
while(n>0)
    print(s[n])
    n=n - s[n]
```

##练习
###15.1-1
$T(0)=1,T(1)=2,T(2)=4,T(3)=8$
$\text{假设$T(n)=2^n$,则有$T(n+1)=1+2^{n+1}-1=2^{n+1}$}$
###15.1-2
反例
i|0|1|2|3|4
--|--|--|--|--|
P[i]|0|1|10|16|4
如果按照贪心算法，那么对于n-4，最终收益将会是17
###15.1-3
```c
CUT_ROD_FIXUP(p,n)
let r[0...n] be a new arrays
r[0]=0
for i = 1 to n
    q = -infty
    for j = 1 to i
        if q < p[j]+r[i-j]-c
            q = p[j]+r[i-j]-c
    r[i]=q
return r[n]
```
###15.1-4
```c
MEMOIZED_CUT_ROD(p,n)
for i = 0 to n
    r[i] = -infty
return MEMOIZED_CUT_ROD_AUX(p,n,r)

MEMOIZED_CUT_ROD_AUX(p,n,r)
if(r[n]>0)
    return r[n]
else if(n=0)
    return 0
else
    for i = 1 to n
        if q < p[i]+MEMOIZED_CUT_ROD_AUX(p,n-i,r)
            s[n] = i
            q = p[i] + MEMOIZED_CUT_ROD_AUX(p,n-i,r)
r[n]=q
return r[n]
```
###15.1-5

```c
FABNAC(n)
let F[0...n] be a new arrays
F[0] = 0
F[1] = 1
if(n>1)
    for i=2 to n
        F[i]= F[i-1] +F[i-2]
return F[n]
```
#矩阵链乘法
##本节内容概括
自底向上的求解矩阵连乘问题，，注意矩阵链乘问题是按照矩阵链的长度自底向上来填表项的：
```c
MARTIX_CHAIN_ORDER(p)
let m[1...n,1...n] and s[1...n-1,2...n] be new arrays
for i=1 to n
    m[i][i] = 0
for l=2 to n
    for i = 1 to n-l+1
        j = i+l-1
        m[i][j]=infty
        for k=i to j-1
            if m[i][j] > m[i][k] + m[k+1][j] + p[i]p[k]p[j]
                m[i][j] = m[i][k] + m[k+1][j] + p[i]p[k]p[j]
                s[i][j] = k
return m and s
```
注：我与书上的算法不同之处是书上引入了一个变量q暂存乘式的值，在这里我为了书写方便省略了这个过程

运行时间：$\Theta(n^3)$

递归求解矩阵链乘问题：
```c
MATRIX_CHAIN_ORDER(A,s,i,j)
if i=j
    return 0
else
    q = infty
    for m=i to j
        q=min(q,MATRIX_CHAIN_MULTIPLY(A,s,i,m)+MATRIX_CHAIN_MULTIPLY(A,s,m+1,j)+p[i-1]p[m]p[j])
return q
```

```c
PRINT_OPTIMAL_PARENS(s,i,j)
if i==j
    print A[i]
else
    print (
    PRINT_OPTIMAL_PARENS(s,i,s[i][j])
    PRINT_OPTIMAL_PARENS(s,s[i][j]+1,j)
    print )
```
#练习
##15.2-1

图略
##15.2-2
```c
MATRIX_CHAIN_MULTIPLY(A,s,i,j)
if i=j
    return A[i]
return MATRIX_CHAIN_MULTIPLY(A,s,i,s[i][j])*MATRIX_CHAIN_MULTIPLY(A,s,s[i][j]+1,j)
```

##15.2-5
对于每层循环，i层执行$n-l+1$次，内层执行$j-i=l-1$次，每次引用两次$m[i][j]$,对其求和即可

#15.3 动态规划原理
##本节内容概括
可以寻求用动态规划来解决问题，需要满足：最优子结构和子问题重叠

重叠子问题就是：
两个子问题如果不共享资源，它们就是独立的
重叠是指两个子问题实际上是一个子问题，只是作为不同问题的子问题出现的

通常有两种实现，一种是待备忘的

##练习
###15.3-2
子问题不重叠
###15.3-4
课本图：15-5的例子已经说明贪心算法不可行了

#15.4 最长公共子序列
##本节内容概括
本节讲述了最长公共子序列LCS问题的求解方法
```c
LCS_LENGTH(X,Y)
m=X.length
n=Y.length
let s[0...m,0...n],b[1...m,1...n] be a new arrays
for i=0 to n
    s[0][i]=0
for i=0 to m
    s[i][0]=0
for i=0 to m
    for j=0 to n
        if x[i]=y[j]
            s[i][j] = s[i-1][j-1] + 1
            b[i][j] = 斜上方箭头
        else if s[i-1][j] > s[i][j-1]
            s[i][j] = s[i-1][j]
            b[i][j] = 向上箭头
        else
            s[i][j] = s[i][j]
            b[i][j] = 向右箭头
return s and b
```
```c
PRINT_LCS(b,X,i,j)
if i=0 or j=0
    return
if b[i][j] = 斜上方箭头
    PRINT_LCS(b,X,i-1,j-1)
    print X[i]
else if b[i][j] = 向上箭头
    PRINT_LCS(b,X,i-1,j)
else
    PRINT_LCS(b,X,i,j-1)
```

时间复杂度：$O(mn)$

##练习
###15.4-1
LCS:100110
###15.4-4
可以动态的使用两行表示当前行和上一行
还可以申请两个变量存储i-1,j和i,j-1,这样就会节省空间开支

#15.5 最优二叉搜索树
##本节内容概括
```c
OPTIMAL_BST(p,q,n)
let e[1...n+1,0...n],w[1....n+1,0...n],root[1...n,1...n] be new arrays
for i=1 to n+1
    e[i][i-1]=q[i-1]
    w[i][i-1]=q[i-1]
for l=1 to n
    for i=1 to n-l+1
        j=i+l-1
        e[i][j] = infty
        w[i][j] = w[i][j-1] + p[j] + q[j]
        for r = 1 to j
            if e[i][j] > e[i][r-1] + e[r+1][j] + w[i][j]
                e[i][j] = e[i][r-1] + e[r+1][j] + w[i][j]
                root[i][j] = r
return e and w and root    
```

##练习
###15.5-3
会多出$O(n)$求w的操作，但不会影响总体的渐进时间









