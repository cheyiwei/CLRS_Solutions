# Chap 19 斐波那契堆

标签（空格分隔）： 算法导论

---

#19.1 斐波那契堆结构
##本节内容概括
一个斐波那契堆是一系列具有最小堆序的有根树的集合
介绍了斐波那契堆的基本结构和它的势函数的求法
#19.2 可合并堆操作

##本节内容概括
```c
FIB_HEAP_INSERT(H,x)
x.degree = 0
x.p =NIL
x.child = NIL
x.mark = FALSE
if H.min =NIL
    create a root by x
    H.min=x
else insert x into list
    if H.min.key>x.key
        H.min = x
H.n++
```

```c
FIB_HEAP_UNION(H_1,H_2)
H=MAKE_FIB_HEAP()
H.min = H_1.min
将两个斐波那契堆的根链表连接起来
if H_1=NIL || (H_2!=NIL && H_2.min.key < H.min.key)
    H.min=H_2.min
H.n=H_1.n+H_2.n
return H
```

抽取最小结点：
```c
FIB_HEAP_EXTRACT_MIN(H)
z=H.min
if z!=NIL
    for each chile x of z
        add z to the root list of H
        x.p=NIL
    remove z from the root list of H
    if z.right=NIL
        H.min=NIL
    else
        H.min=z.right
        CONSOLIDATE(H)
    H.n=H.n-1
return z
```


```c
CONSOLIDATE(H)
let A[0..D(H)] be a new arrays
for i=0 to D(H)
    D[i]=NIL
for each node w in the root list of H
    x=w
    d=x.degree
    while A[d]!=NIL
        y=A[d]
        if x.ket>y.key
            exchange x and y
        FIB_HEAP_LINK(H,y,x)
        A[d] = NIL
        d++
    A[d] = x
H.min=NIL
for i =  0 to D[n]
    if D[i]!=NIL
        if H.min=NIL
            create a root by D[i]
            H.min=D[i]
        else
            add D[i] into heap
            if D[i].key < H.min.key
                H.min=D[i]
```

#19.3 关键字减值和删除一个结点

```c
```
在这里需要注意斐波那契堆的结点DECREASE需要有级联操作即可