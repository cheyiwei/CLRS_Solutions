# Chap 4 分治策略

标签（空格分隔）： 算法导论

---

#4.1 最大子数组问题
##本节内容概括
最大子数组代码
```c
FIND_MAX_CROSSING_SUBARRAY(A,low,mid,high)
sum=0
max_left = mid
left_sum= - inf
for i = mid downto low
    sum += A[i]
    if(sum>left_sum)
        left_sum = sum
        max_left = i
sum=0
max_right = mid
right_sum = -inf
for i = mid + 1 to high
    sum += A[i]
    if(sum>right_sum)
        right_sum = sum
        max_right = i
return(max_left,max_right,left_sum+right_sum)

FIND_MAXIMUM_SUBARRAY(A,low,high)
if high == low
    return (low,high,A[low])
else
    mid = (high+low)/2
    (left1,right1,sum1)=FIND_MAXIMUM_SUBARRAY(A,low,mid)
    (left2,right2,sum2)=FIND_MAXIMUM_SUBARRAY(A,mid+1,high)
    (left3,right3,sum3)=FIND_MAX_CROSSING_SUBARRAY(A,low,mid,high)
    if sum1 >sum2 && sum1>sum3
        return(left1,right1,sum1)
    else if sum2>sum3
        return(left2,right2,sum2)
    else
        return(left3,right3,sum3)
```
注：我默写的代码和书上代码的区别是我在FIND_MAX_CROSSING_SUBARRAY中对max_right和max_left做了初始化，后来想想，其实不用初始化，因为即使只有两个元素，max_left也会被赋值，此时low=mid，而high=mid+1,max_riht也会被赋值，这个细节令人赞叹

递归式如下：
$$
        T(n) =
        \begin{cases}
        T(n/2),  & \text{if $n>1$} \\
        \Theta(n), & \text{if $n=1$}
        \end{cases}
$$


##练习
###4.1-1
会返回A中最小元素的下标（作为low和high），其值作为sum
###4.1-2
暴力求解如下
```c
FIND_MAXIMUM_SUBARRAY(A,low,high)
max_left = low
max_right = low
max_sum = A[low]
for i = low to high
    sum = 0
    for j = i to high
        sum += A[j]
        if(sum > max_sum)
            max_left = i
            max_right = j
            max_sum = sum
return (max_left,max_right,A[low])
```
###4.1-3
源代码很好实现，略
实验结果如下：
![image_1c2oh7re81at9r4u3ou1k651n9r9.png-72.9kB][1]


  [1]: http://static.zybuluo.com/cheyiwei/1vquhub5iuaa58ebz2jtf2y1/image_1c2oh7re81at9r4u3ou1k651n9r9.png
  
  
  
###4.1-4
在代码中加上判断，如果sum为负，返回0

###4.1-5
```c
FIND_MAXIMUM_SUBARRAY(A,low,high)
max_right=max_left = low
max_sum = A[low]
sum = 0
cur_low = 1
for i = low to high
    sum += A[i]
    if(sum < 0)
        cur_low = i + 1
        sum = 0
    if(sum > max_sum)
        max_sum = sum
        max_right = i
        max_low = cur_low
return(max_low,max_right,max_sum)
```

注意：在这里cur_low和sum的设置十分巧妙

#4.2 矩阵乘法的Strassen算法
##本节内容概括
这个算法被誉为20世纪最经典的十大算法之一

矩阵乘法（普通），这里是方阵：
```c
SQUARE_MATRIX_MULTIPLY(A,B)
n=A.rows
let C[1...n,1...n] be a new arrays
for i = 1 to n
    for j = 1 to n
        C[i][j]=0
        for k = 0 to n
            C[i][j]+=A[i][k]*B[k][j]
return C
```
矩阵乘法，非方阵
```c
n=A.rows
c=A.columns
s=B.columns
let C[1...n,1...s] be a new array
for i = 1 to n
    for j = 1 to s
        C[i][j]=0
        for k = 0 to c
            C[i][j]+=A[i][k]*B[k][j]
return C
```
非方阵可以扩展为方阵，而且本节算法实现对象也是方阵

通过对上述方阵分析，时间复杂度为$\Theta(n^3)$

为了解决这个复杂度太大的问题，我们首先来看传统的分治策略：
核心思想为：将A,B,C分成4个$(n/2 * n/2)$的矩阵，然后利用分块矩阵的乘法来做，需要进行8次乘法和4次加法，具体代码见书上，如果是采取复制的方法来划分矩阵，那么每次划分需要$\Theta(n^2)$，伪代码略，递归式如下：
$$
        T(n) =
        \begin{cases}
        8T(n/2) + \Theta(n^2),  & \text{if $n>1$} \\
        1, & \text{if $n=1$}
        \end{cases}
$$
通过主方法可知，时间复杂度并未得到优化，依旧为$\Theta(n^3)$

Strassen方法十分巧妙，它的的递归式为
$$
        T(n) =
        \begin{cases}
        7T(n/2) + \Theta(n^2),  & \text{if $n>1$} \\
        1, & \text{if $n=1$}
        \end{cases}
$$
通过主方法可知，时间复杂度为$\Theta(n^{\log 7})$
##练习
###4.2-1
计算略
###4.2-2
```c
STRASSEN_MATRIX_MULTIPLY(A,B)
n=A.rows
let C[1...n,1...n] be a new arrays
let S[1...10][1...n/2,1...n/2] be a new arrays
let P[1...7][1...n/2,1...n/2] be a new arrays
if n==1
    c[1][1]=a[1][1]*b[1][1]
else
    partition A,B and C //分割ABC
    S[1] = B_12 - B_22
    S[2] = A_11 + A_12
    S[3] = A_21 + A_22
    S[4] = B_21 - B_11
    S[5] = A_11 + A_22
    S[6] = B_11 + B_22
    S[7] = A_12 - A_22
    S[8] = B_21 + B_22
    S[9] = A_11 - A_21
    S[10]= B_11 + B_12
    P[1] = STRASSEN_MATRIX_MULTIPLY(A_11,S[1])
    P[2] = STRASSEN_MATRIX_MULTIPLY(B_22,S[2])
    P[3] = STRASSEN_MATRIX_MULTIPLY(B_11,S[3])
    P[4] = STRASSEN_MATRIX_MULTIPLY(A_22,S[4])
    P[5] = STRASSEN_MATRIX_MULTIPLY(S[6],S[5])
    P[6] = STRASSEN_MATRIX_MULTIPLY(S[7],S[8])
    P[7] = STRASSEN_MATRIX_MULTIPLY(S[9],S[10])
    C_11 = P[5]+P[4]-P[2]+P[6]
    C_12 = P[1]+P[2]
    C_21 = P[3]+P[4]
    C_22 = P[5]+P[1]-P[3]-P[7]
return C
```
###4.2-3
在运行Strassen算法之前，先将A和B扩展为方阵，然后再进行求解，扩展为方阵的时间复杂度为$\Theta(n^2)$，所以命题可证
###4.2-5

#4.3 用代入法求解递归式
##本节内容概括
代入法需要做的
1）猜测一个界
2）利用数学归纳法证明他

需要注意的是，改变变量（变量替换）和选择一个更强的边界条件有时候更易证明
##练习
###4.3-1
证明：假设对所有$m<n$，特别是$m=n-1$，都有$T(m)\leq cm^2$
将其带入递归式，可证，其中要求$c\leq \frac 12$

###4.3-2
证明：假设对所有$m<n$，特别是$m=\frac 12$，都有$T(m)\leq c\lg n$
将其带入递归式，可证，其中要求$c\leq 1$

###4.3-3
和书上的证明过程几乎一样，除了将小于等于改为大于等于外，注意此时的$c\geq 1$

###4.3-4
在这里假设小于$cn\lg n + d$即可，最后$d>=1,c>=d+1$
###4.3-5
同上，证明略
###4,3-6
证明略，注意在证明过程中可以在法律允许的范围内尽情放缩
###4.3-7
证明略，注意添加的低阶量为$dn$
#4.4 用递归树方法求解递归式
##本节内容概括
本节介绍了通过递归树来求解递归式的一种方法
##练习
###4.4-1
总共有$\lg n$层，每层的代价为$(\frac{3}{2})^i*n^2$,最后一层的代价为$\Theta(3^{\log_2^n})=\Theta(n^{\log_2^3})$
最后算得的渐进上界为$O(n^{\log_2^3})$
###4.4-2
总共有$\lg n$层，每层的代价为$(\frac{1}{2})^i*n^2$,最后一层的代价为$\Theta(1)$
最后算得的渐进上界为$O(n^2)$
###4.4-3
本题递归树的层数为$\infty$，因为$T(4)=4*T(2+2)+4$
可见，最后结果并不收敛
###4.4-4
共$n$层，每层代价为$2^i$，最后一层代价为$\Theta(2^n)$
最终的渐进上界为$O(2^n)$
###4.4-5
共$n$层，可以把它当作一个满的递归树来求解，其中每层的代价为$\leq (\frac{3}{2})^i*n$
最终的渐进上界为$O(n(\frac{3}{2})^n)$
###4.4-6
可以把它当作一个满的递归树来求解
共$\log_{\frac{3}{2}}n $层，每层的代价为$cn$
最终的渐进上界为$O(n*\log_{\frac{3}{2}}n)$
最短的路径有$\log_3n $层，所以渐进下界为
$\Omega(n\log_3n)$,自然也是$\Omega(n\lg n)$
###4.4-7
第一层$n$，第二层$2n-4 \le 4 \lfloor n/2 \rfloor \le 2n$，最长$\lg n$层，最短$\lg n - 1$层，所以总时间
可证为：$O(n^2),\Omega(n^2)$
###4.4-8
每一层的代价都为$cn$
层数为$n/a$
所以总的时间代价为$O(n^2)$
###4.4-9
类似于4.4-7，每层都是$cn$，最多$\log_{\min (\frac1{\alpha}, \frac1{1-\alpha})} n$层，最少$\log_{\max (\frac1{\alpha}, \frac1{1-\alpha})} n$层，所以
$$T(n) \le cn log_{\min (\frac1{\alpha}, \frac1{1-\alpha})} n = O(n \lg n)$$
$$T(n) \ge cn log_{\max (\frac1{\alpha}, \frac1{1-\alpha})} n = \Omega(n \lg n)$$
综上$T(n) = \Theta (n \lg n)$

#4.5用主方法求解递归式
##本节内容概括
主方法提供了一种菜谱式的方法（直接调用公式）
>对于形如 $T(n)=aT(n/b)+f(n)$的递归式，其中$a\geq 1,b> 1 $是常数，有如下定理
1. 若$f(n)=O(n^{\log_a {b-\epsilon}}),\epsilon>0$,则$T(n)=\Theta(n^{\log_b a})$
2. 若$f(n)=\Theta(n^{\log_a {b}})$,则$T(n)=\Theta(n^{\log_b a}\log n)$
3. 若$f(n)=\Omega(n^{\log_a {b+\epsilon}}),\epsilon>0$,且$af(n/b)\leq cf(n),c<1$,则$T(n)=\Theta(f(n))$
##练习
###4.5-1
1. $T(n)=2T(n/4)+1$
$a=2,b=4,\log_ba=\frac 12$
$T(n)=\Theta(n^{\frac 12})$
2. $T(n)=2T(n/4)+\sqrt n$
$a=2,b=4,\log_ba=\frac 12$
$T(n)=\Theta(n^{\frac 12}\log n)$
3. $T(n)=2T(n/4)+n$
$a=2,b=4,\log_ba=\frac 12$
$2*n/4 n \leq 1/2 n , c\leq \frac 12$
$T(n)=\Theta(n)$
4. $T(n)=2T(n/4)+n^2$
$a=2,b=4,\log_ba=\frac 12$
$2/16*n^2 n \leq 1/8 n^2 , c\leq \frac 18$
$T(n)=\Theta(n^2)$
###4.5-2
<li>如果$\log_4 a =2 \ \Rightarrow \ k=16$，那么满足第2种情况，$T(n)=\Theta (n^2 \lg n)=o(n^{\lg 7})$</li>
<li>如果$\log_4 a \lt 2 \ \Rightarrow \ k \lt 16$，那么满足第1种情况，$T(n)= \Theta (n^2)=o(n^{\lg 7})$</li>
<li>如果$\log_4 a \gt 2 \ \Rightarrow \ k \gt 16$，那么满足第3种情况，这时$\log_4 a \lt \lg 7 \Rightarrow a \lt 4^{\lg 7} = 49$
所以 $a$的最大值为48
###4.5-3

$a=1,b=2,\log ba=0$
所以$\Theta(\lg n)$
###4.5-4
不可以，但是可以用递归树来求解

  
  
  
  
  
  
  