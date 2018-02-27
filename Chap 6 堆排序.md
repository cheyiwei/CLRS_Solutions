# Chap 6 堆排序

标签（空格分隔）： 算法导论

---

#其他排序算法
简单选择排序：
```c
Selection_sort(A)
for i = 1 to A.length - 1
    min = i
    for j = i + 1 to A.length
        if A[j]<A[min]
            min = j
    if min != i
        swap(A[i],A[min])
```

简单选择排序是一个就地排序，而且它不稳定

冒泡排序：
```c
BUBBLE_SORT(A)
for i = 1 to A.length - 1
    for j = A.length downto i + 1
        if A[j] < A[j-1]
            swap(A[j],A[j-1])
```
冒泡排序，稳定

希尔排序：缩小增量的插入排序
```c
SHELL_SORT(A,D)//D为增量集合
i=1
for i = 1 to D.length
    SHELL_PASS(A,D[i])

SHELL_PASS(A,d)
for i = d+1 to A.length
    if A[i] < A[i-d]
        A[0] = A[i]
        j = i-d
        while j>0 && A[0] < A[j]
            A[j+d] = A[j]
            j -= d
        A[j+d] = A[0]
return A            
```
希尔排序实际的插入效率较直接插入排序快
#6.1 堆
##本节概念概括
介绍了堆的一些概念和三个函数，注意下面三个函数对应的求堆结点的方法对应的根节点从1开始
```c
PARENT(i)
    return i/2
LEFT(i)
    return 2*i
RIGHT(i)
    return 2*i + 1
```

##练习

###6.1-1
最多：$2^{n+1}-1$
最少：$2^{n}$

###6.1-2
假设高度为$i$，则元素个数最多为$2^{i+1}-1$,最少为$2^i$
另$2^i\leq n\leq 2^{i+1}-1$
命题可证
###6.1-3
因为最大堆的性质
$A[PARENT[i]]\geq A[i]$
因此命题易证
###6.1-4
在某片叶子上
###6.1-5
是的，排好序的数组就是一个最小堆
###6.1-6
23
17      14
6 13    10 1
2 7 12

不是一个最大堆，因为7>6,但是7是6的RIGHT

###6.1-7
根据堆的性质，下标为$\lfloor n/2 \rfloor + 1 $没有孩子结点
命题可证

#6.2 维护堆的性质
维护大根堆的性质，主要思想：除了大根堆的根节点外，以LEFT和RIGHT为根的结点都满足大根堆的性质
算法：
```c
MAX_HEAPIFY(A,i)
left = LEFT(i)
right = RIGHT(i)
if left<=A.size && A[left] > A[i]
    largest = left
else largest = i
if right<=A.size && A[right] > A[largest]
    largest = right
if largest != i
    swap(A.[largest],A[i])
    MAX_HEAPIFY(A,largest)
```
可以用如下的递归式来刻画这个的最坏情况：
$T(n)=T(2n/3)+\Theta(1)$
根据主定理，它的解为$\Theta(\lg n)$

##练习
###6.2-1
图略
最后得到的数组为 ${27,17,10,16,13,9,1,5,7,12,4,8,3,0}$
###6.2-2
```c
MIN_HEAPIFY(A,i)
left = LEFT(i)
right = RIGHT(i)
if left<=A.size && A[left] < A[i]
    min = left
else min = i
if right<=A.size && A[right] < A[min]
    min = right
if min != i
    swap(A.[min],A[i])
    MIN_HEAPIFY(A,min)
```
最大堆和最小堆的运行时间一样

###6.2-3
将不会对原有的大根堆做变动
###6.2-4
将不会对原有的大根堆做变动
###6.2-5
```c
MAX_HEAPIFY(A,i)
largest = i
do
    i = largest
    left = LEFT(i)
    right = RIGHT(i)
    if left<=A.size && A[left] > A[i]
        largest = left
    if right<=A.size && A[right] > A[largest]
        largest = right
    if largest != i
        swap(A.[largest],A[i])
while(largest != i)
```
###6.2-6
根节点是所插入的所有值中的最小值即可，这就是最大根堆调整中的最小值
#6.3 建堆
##本节内容概括

构建最大堆，时间复杂度为线性

```c
BUILD_MAX_HEAP(A)
for i = A.length/2 downto 1
    MAX_HEAPFIY(A,i)
```

##练习
###6.3-1 
图略
###6.3-2
因为MAX_HEAPFIY要求以当前需要以当前调整结点两个孩子为根的两个堆都是最大堆
###6.3-3
利用数学归纳法证明：
高度为$0$时，至多有$1$个高度为$0$的结点，结论成立
![image_1c2r1eihod4017mb4gp1i3de1um.png-25.7kB][1]


  [1]: http://static.zybuluo.com/cheyiwei/iz9sipyz0dh7lcgkh0q2a8aw/image_1c2r1eihod4017mb4gp1i3de1um.png
#6.4 堆排序算法
##本节内容概括
算法：
```c
HEAPSORT(A)
BUILD_MAX_HEAP(A)
for i = A.length downto 2
    swap(A[1],A[A.length])
    A.length--
    MAX_HEAPIFY(A,1)
```
时间复杂度：$\Theta(n\log n)$

##练习
###6.4-1
图略
###6.4-2
循环开始
循环过程
循环终止
###6.4-3
升序：$\Theta(n\log n)$
降序：$\Theta(n\log n)$

#6.5 优先队列
##本节内容概括

优先队列函数实现
```c
MAXMUM(S)
return S[1]
```
```c
EXTRACT_MAX(S)
if S.length < 1
    error "heap underflow"
key = S[1]
S[1] = S[S.length]
S.length -= 1
MAX_HEAPFIY(S,1)
return key
```
```c
INCREASE_KEY(S,i,k)
if S[i] > k
    error "new key is amaller than current key" 
else
    S[i] = k
    while(i>1 && S[PARENT[i]]<S[i])
        swap(S[PARENT[i]],S[i])
        i = PARENT[i]
```
```c
INSERT(S,x)
S.length++
S.[S.length] = -infty
INCREASE_KEY(S,S.length,k)
```
##练习
###6.5-4
这是为了保证在INCREASE_KEY过程中不出错
###6.5-6
```c
INCREASE_KEY(S,i,k)
if S[i] > k
    error "new key is amaller than current key" 
else
    S[i] = k
    while(i>1 && S[PARENT[i]]<S[i])
        S[i] = S[PARENT[i]]
        i = PARENT[i]
    S[i] = k
```
###6.5-8
```c
HEAP_DELETE(S,i)
swap(S[i],S[S.length])
S.length--
MAX_HEAPFIY(S,i)
```



```c
void print(int now,int large,char c){//打印沙漏 
	int i=0;//打印上半部分
	for(i=0;i<(large-now)/2;i++){
		printf(" ");
	}
	for(i=0;i<now;i++){
		printf("%c",c);
	}
	printf("\n");
	if(now >= 3){
		print(now-2,large,c);//递归调用
	}else{
		return;
	}
	for(i=0;i<(large-now)/2;i++){//打印下半部分
		printf(" ");
	}
	for(i=0;i<now;i++){
		printf("%c",c);
	}
	printf("\n");
}
```















