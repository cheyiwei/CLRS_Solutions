# Chap 14 数据结构的扩张

标签（空格分隔）： 算法导论

---

#14.1 动态顺序统计
##本节内容概括
在红黑树的结点中增加一个域 size

$x.size = x.left.size+x.right.size+1$

```c
OS_SELECT(x,i)
r = x.left.size + 1
if r = i
    return x
else if i<r
    return OS_SELECT(x.left,i)
else
    return OS_SELECT(x.right,i-r)

```

```c
OS_RANK(T,x)
r=x.left.size+1
y=x
while y != T.root
    if y=y.p.right
        r+= y.p.left.size+1
    y=y.p
return r
```
上面两个算法的渐进时间都是$O(\lg n)$

对子树规模的维护：包括插入删除，维护都只需要$O(\lg n)$的时间

##练习
14.1-3
```c
OS_SELECT(x,i)
r=x.left.size+1
while(i!=r)
    if i<r
        x=x.left
    else
        x=x.right
        i=i-r
    r=x.left.size+1
return i
```


#14.2 如何扩张数据结构

红黑树中，如果一个结点的某个属性仅依赖于x,x.left,x.right，那么就是可扩张的
###14.2-2
都不行
#14.3 区间树
```c
INTERVAL_SEARCH(T,i)
x=T.root
while x != T.nil && i does not overlap x.int
    if x.left != T.nil && x.left.int.max >= x.int.low
        x=x.left
    else
        x=x.right
return x
```
##练习
###14.3-2
将大于等于改为大于
###14.3-4
```c
ALL_SEARCH(x,i)
if x != NULL && x.int overlap x.int
    print x
if x.left != NIL && x.left.max>=i.low
    ALL_SEARCH(x.left,i)
else x.right != NIL && x.right.max>=i.low
    ALL_SEARCH(x.right,i)
```








