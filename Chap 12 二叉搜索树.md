# Chap 12 二叉搜索树

标签（空格分隔）： 算法导论

---

#12.1 什么是二叉搜索树
##本节内容概括
介绍了什么是二叉搜索树，并且介绍了其中序遍历：
```c
INORDER_TREE(x)
if x!=NIL
    INORDER_TREE(x.left)
    print x
    INORDER_TREE(x.right)
```
中序遍历的渐进时间为$\Theta(n)$，为搜索一遍树的时间

##练习

###12.1-2
二叉搜索树要求左孩子小于根结点而右孩子大于根节点
最小堆要求孩子大于根节点
不可以，如果可以的话，就可以在$\Theta(n)$时间内完成排序，在基于比较的排序中这是不可能的
###12.1-5
二叉搜索树可以使用决策树模型来做。

#12.2 查询二叉搜索树

对于二叉搜索树的静态操作，有如下四种操作，其中需要注意后继操作，当当前结点有右节点时，就是返回右子树的最小值，当当前结点没有有节点时，就是向上搜索，返回第一个将原结点作为左子树的根节点

```c
TREE_SEARCH(x,k)
if x.key = k
    return x
else if k<x.key
    TREE_SEARCH(x.left,k)
else
    TREE_SEARCH(x.right,k)
```

```c
INTERTIVE_TREE_SEARCH(x,k)
while x!=NIL && x.key = !k
    if k<x.key
        x = x.left
    else
        x = x.right
```

```c
MAXIMUM_TREE(x)
while x.left != NIL
    x = x.left
return x
```


```c
SUCCESSOR(x)
if x.right != NIL
    return MINIMUM_TREE(x.right)
y=x.p
while y!=NIL && y.right = x
    x=y
    y=x.p
return y
```
##练习
###12.2-1
c和e，他们都不是二叉搜索树：
因为912>911
299<347

###12.2-4
反例：
        9
7          10
1  100   4   11
查找4，显然不对
###12.2-5
它的后继如果有左孩子，那么就不是最小的结点
它的前驱如果有右孩子，那么就不会是最大的结点

#12.3 插入和删除

二叉搜索树的插入比较简单，但是删除较为复杂

```c
TREE_INSERT(T,z)
y=NIL
x=T.root
while x!=NIL
    y=x
    if x.key <= z.key
        x=x,right
    else
        x=x,left
z.p=y
if y = NIL
    T.root = z
else if z.key < y.key
    y.left = z
else
    y.right = z
```

删除的本质是用该结点的某个子树结点去替换该结点
这个策略分为3种情况：

1. 没有左孩子，用右孩子替换原节点
2. 只有左孩子，用左孩子替换原节点
3. 有左孩子和右孩子，分为两种情况，本质上就是用后继去替换它：
    a. 后继无是原结点右孩子
    b. 后继不是原结点右孩子

在这里定义一个过程来简化代码结构：用一颗子树替换一棵子树并成为其双亲的孩子结点
```c
TRANSPLANT(T,u,v)//用v来替换u
if u.p = NIL
    T.root = v
else 
    if u.p.left=u
        u.p.left = v
    else
        u.p.right = v
if u!=NIL
    v.p = u.p
```
```c
TREE_DELETE(T,z)
if z.left = NIL
    TRANSPLANT(T,z,z.right)
else if z.right = NIL
    TRANSPLANT(T,z,z.left)
else
    y=TREE_MINIMUM(z.right)
    if y != z.right
        TRANSPLANT(T,y,y.right)
        y.right = z.right
        y.right.p = y
    TRANSPLANT(T,z,y)
    y.left = z.left
    y.left.p = y
        TRANSPLANT(T,y,)
```

渐进时间：$O(h)$

##练习
###12.3-2
因为INSERT过程和查找过程是一样的，但是查找需要和本身的结点做比较
###12.3-5
本题在中文版书籍上翻译错误











