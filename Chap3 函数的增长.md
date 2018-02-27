# Chap3 函数的增长

标签（空格分隔）： 算法导论

---

#3.1 渐进符号
##本节内容概括

本节介绍了五个记号
$\Theta,O,\Omega,o,\omega$
分别代表渐进紧确界，渐进上界，渐进下界，非渐进上界，非渐进下界，和它们有关的具体定义可以查阅算法导论

>定理3.1 对任意的两个函数$f(n),g(n)$，$f(n)=\Theta(g(n))\quad iff\quad f(n)=O(g(n))\quad and \quad f(n)=\Omega(g(n))$ 

实数的很多性质可以迁移到这里，但是三分性不行，因为不是每两个函数都是渐进可比的


##练习
###3.1-1
$f(n)+g(n)=min(f(n),g(n))+max(f(n),g(n))$
取$c_1=\frac 12 \quad c_2 = 1$，命题可证

###3.1-2

只需要注意到$n-|a|\leq n+a\leq  n+|a|$
所以，取$n\geq 2|a|\quad c_1 = \frac 12 \quad c_2 = \frac 32$
命题即证

###3.1-3

因为$O$是渐进上界

###3.1-4

前一个成立，后一个不成立

###3.1-5

按照定义叙述一遍即可

注意在证明时$n_0 = \max({n_{01},n_{02}})$

###3.1-6

证明易，略

###3.1-7

非渐进上界和非渐进下界无交集

###3.1-8

扩充定义即可，略

#标准记号和常用函数

##本节内容概括

本节中提到了向上取整和向下取整，注意下面的公式
$\lceil \frac{\lceil \frac xa \rceil}{b}\rceil=\lceil \frac{x}{ab} \rceil$,$\lfloor \frac{\lfloor \frac xa \rfloor}{b}\rfloor=\lfloor \frac{x}{ab} \rfloor$

补充定理：
$f(x)$是任何单调上升函数，且$f(x)$在整数点才可能取到整数值，则
1) $\lfloor f(x) \rfloor = \lfloor f(\lfloor x\rfloor) \rfloor$
2) $\lceil f(x) \rceil = \lceil f(\lceil x\rceil) \rceil$

上面定理的证明可以用到反证法，我觉得这个的证明很有意思：
我们以1）为例进行证明
$x> \lfloor x \rfloor \implies f(x) > f(\lfloor x \rfloor)$
假设$\lfloor f(x) \rfloor != \lfloor f(\lfloor x\rfloor) \rfloor$
那么必定有在$\lfloor x \rfloor \text{和} x$之间存在不止一个整数，因此假设不成立，命题可证

注意指数中的一些公式有关$e^x$的部分，我们在微积分课程中已经学习过
$1+x\leq e^x \leq 1+x+x^2$
$\lim_{n \to +\infty}{(1+\frac xn)^n}=e^x$

对于阶乘
正如习题中要求证明的
$n!=o(n^n)$
$n!=\omega(2^n)$
$lg(n!)=\Theta (n\log n)$
前两个十分好证明，第三个需要利用斯特林公式，简易证法1暂时未想出

##练习
###3.2-1
证明：
由函数单调性的定义可以得出此结论
###3.2-2
证明：
$t = a^{\log_b{c}} \implies \frac{\lg t}{\lg a} = \frac{\lg c}{\lg b}\implies \frac{\lg t}{\lg c} = \frac{\lg a}{\lg b}\implies t = c^{\log_b{a}}$
命题可证
###3.2-3
证明易，见上文，略
###3.2-4
证明多项式有界就是证明$lg{(f(n))}=O(lg n)$
$\lg{\lg{\lceil (n) \rceil}!} = \Theta(\lg{\lceil (n) \rceil}\lg\lg {\lceil (n) \rceil})=\omega(\lg n)$
所以不是多项式有界
对于第二个式子，可证明它是多项式无界
在这里用到了公式3.19
















