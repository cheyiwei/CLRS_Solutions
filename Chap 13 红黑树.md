# Chap 13 红黑树

标签（空格分隔）： 算法导论

---

# 13.1 红黑树的性质
##本节内容概括
红黑树是满足以下性质的二叉搜索树：

1. 结点要么是红色，要么是黑色
2. 根节点是黑色
3. 叶结点（NIL）是黑色的
4. 如果一个结点是红色的，则它的两个孩子都是黑色的
5. 从根节点到每个叶子结点的黑色结点个数相等

n个结点的红黑树的高度至多为$2\lg ({n+1})$

##练习
###13.1-3
所得到的还是一棵红黑树
#13.2 旋转

左旋：
```c
LEFT_ROTATE(T,x)
y=x.right
y.p=x.p
if x.p = NIL
    T.root = y
else if x.p.left = x
    x.p.left = y
else
    x.p.right = y
x.p = y
x.right = y.left
y.left = x
```
右旋是相反的操作，注意左旋只能以右孩子不为NIL时进行，右旋只能在左孩子不为NIL时进行

##练习
###13.2-3
a的深度会增加1
b的深度不变
c的深度减少1

#13.3 插入

红黑树的插入过程需要对结点的颜色进行调整，其中
首先以该结点的父结点是祖父结点的右结点还是做节点来一个划分，总共六种情况，但是两两对称，因此，以其为左孩子为例，分为三种情况

1. 其叔结点是红色，那么改变其父结点和叔结点及其祖父结点的颜色即可，并将问题上移两层
2. 其叔结点是黑色，该结点是父亲的右孩子，此时需要以其父结点为中心进行左旋(最后指向父结点)，然后进入3的操作
3. 其叔结点是红色，该结点是父亲的左孩子，此时需要以其祖父结点为中心进行右旋，在此过程中需要改变祖父结点颜色为RED，父结点颜色为BLACK

算法如下：
```c
RB_INSERT_FIXUP(T,x)
while x.p.color = RED
    if x.p = x.p.p.left
        y=x.p.p.right
        if y.color = RED  //case 1
            y.color = BLACK
            x.p.color = BLACK
            y.p.color = RED
            x=x.p.p
        else
            if x=x.p.right //case 2
                x=x.p
                LEFT_ROTATE(T,x)
            x.p.color = BLACK//case 3
            x.p.p.color = RED
            RIGHT_ROTATE(T,x.p.p)
    else(same as then clause with right and left exchange)
T.root = BLACK
```

###13.3-4

在算法执行过程中，如果需要修改T.nil结点，那么根节点一定需要是红色，又因为根节点没有兄弟结点，当在case 1 修改根节点颜色后，算法会终止，并将根节点重新置为黑色，当在case3中赋予根节点红色时，又会立即右旋，此时根节点还是黑色，所以根节点是黑色，也就是书上的循环不变式2，如果z.p是根节点，那么一定是黑色
命题可证，不会出现z.p为根节点且为红色的情况

#13.4 删除

##本节内容概括
```c
RB_TRANSPLANT(T,u,v)
if u.p=T.nil
    T.root = v
else if u.p.left = u
    u.p.left = v
else u.p.right = v
v.p=u.p
```
```c
RB_DELETE(T,z)
y=z
y_originaled_color = y.color
if z.left = T.nil
    x=z.right
    RB_TRANSPLANT(T,z,z.right)
else if z.right = T.nil
    x = z.left
    RB_TRANSPLANT(T,z,z.left)
else
    y=TREE_MINIMUM(T,z.right)
    x=y.right
    if y.p=z
        x.p = y
    else
        RB_TRANSPLANT(T,y,y.right)
        y.right = z.right
        y.right.p = y
    RB_TRANSPLANT(T,z,y)
    y.left = z.left
    y.left.p=y
    y.color = z.color
if y_originaled_color = BLACK //会影响到红黑树的性质
    RB_DELETE_FIXUP(T,x)
```
插入有三种情况，删除有四种情况：
在这里要注意对双重黑的理解，我们的目标就是消除双重黑
情况一：x的兄弟结点w是红色，改变w和x.p，并以x.p进行一次左旋
情况二：x的兄弟结点w是黑色,且w的两个子结点都是黑色的，将w变为红色，x上移一层
情况三：x的兄弟结点w是黑色,且w的两个子结点左红右黑，w和w.left颜色点到进行右旋
情况四：x的兄弟结点w是黑色,且w的两个子结点右孩子是红色的，w和w.right颜色颠倒进行左旋，x设置为根
```c
RB_DELETE_FIXUP(T,x)
while x != T.root && x.color = BLACK
    if x=x.p.left
        w=x.p.right
        if w.color = RED//case 1
            w.color = BLACK
            w.p.color = RED
            LEFT_ROTATION(T,x.p)
            w=x.p.right
        if w.left.color = BLACK && w.right.color = BLACK//case 2
            w.color = RED
            x=x.p
        else 
            if w.right.color = BLACK   //case 3
                w.left.color = BLACK
                w.color = RED
                RIGHT_ROTATION(T,w)
                w=x.p.right
            w.color = RED
            w.right.color = BLACK
            x.p.color = BLACK
            LEFT_ROTATION(T,x.p)
            x=T.root
    else(same as then clause with right and left exchange)
x.color = BLACK
```

##练习
###13.4-2
如果x和x.p都是红色的。算法执行后会将x置为黑，而根据原本这是一棵红黑树可知，x的兄弟结点也是黑色，那么性质4即可恢复。
###13.4-5
这确实是不变的，注意**x本身自带一层黑高度**






