# Chap 22 基本的图算法

标签（空格分隔）： 算法导论

---

#22.1 图的表示

##练习
###22.1-1
遍历图的邻接链表即可
出度：$O(|E|+|V|)$
入度：$O(|E|+|V|)$
###22.1-2
完全二叉树：
$$
         \begin{matrix}
        ~ & 1 & 2& 3& 4& 5& 6& 7\\
        1 & 0 & 1& 1& 0& 0& 0& 0\\
        2 & 1 & 0& 0& 1& 1& 0& 0\\
        3 & 1 & 0& 0& 0& 0& 1& 1\\
        4 & 0 & 1& 0& 0& 0& 0& 0\\
        5 & 0 & 1& 0& 0& 0& 0& 0\\
        6 & 0 & 0& 1& 0& 0& 0& 0\\
        7 & 0 & 0& 1& 0& 0& 0& 0\\
        \end{matrix}
$$

###22.1-3

对于邻接矩阵，只需要$O(V^2)$即可
对于邻接链表，需要$O(V+E)$,遍历每个结点的所有边即可

###22.1-4
这个算法简单来讲就是遍历一遍所在边即可
```c
for each v in G.V
    i=j=0;
    visted[V]={0};
    while(v!=NULL){
        visted[v.key]=true;
        while(visted[v.next.key]==true){
            v.next = v.next.next;
        }
        v=v.next;
    }
```
时间复杂度为$O(V+E)$

###22.1-5

对于邻接矩阵来讲
需要的时间为$O(V^3)$
对于邻接链表来说
需要的时间为$O(V+E*(E))$
每个边最多遍历新的边

###22.1-6

通用汇点：
从Matrix[1,1]开始寻找，如果遇到1向下走，遇到0向右走，一直走到最后一行或者最后一列停下，检查当前位置是否是通用汇点

通用汇点的所在的行均为0，所在的列除汇点本身均为1

###22.1-7
$对于A=BB^T中的元素a_{ij}\\
当i==j时\\
就是B的第i行乘以B^T的第i列，所以此时a_{ii}的值为与i所有相连的边的条数\\
当i!=j时\\
就是B的第i行乘以B^T的第i列,即B的第j行，所以此时a_{ij}的值为-（i，j）相连的边数的绝对值
$



#22.2 广度优先搜索
##本章内容概括
```c
BFS(G,s)
for each v in G - {s}
    v.color = WHITE
    v.d = infty
    v.pi = NIL
s.color = GRAY
s.d = 0
s.pi = NIL
ENQUEUE(Q,s)
while Q != NULL
    s = DEQUEUE(Q)
    for each v in G.adj[s]
        if v.color = WHITE
            v.color = GRAY
            v.d = s.d+1
            v.pi = s
    s.color = BLACK
```
```c
PRINT_PATH(G,s,v)
if s=v
    print s
else if v.pi = NULL
    print there is no way from s to v
else
    PRINT_PATH(G,s,v.pi)
    print v
```
##练习
###22.2-1
node|pi|d
--|--|--
3|NULL|0
6|3|1
5|3|1
4|5|2
2|4|3
1|NULL|$\infty$
###22.2-2
node|pi|d
--|--|--
u|NULL|0
y|u|1
t|u|1
x|u|1
w|t|2
s|w|3
r|s|4
v|s|5
###22.2-3

证明略，显而易见的是，除了第18行，没有其余的行使用到了BLACK颜色

###22.2-4

如果改为邻接矩阵的话
遍历所有边的时间将为$O(V^2)$，所以总时间为$O(V+V^2)$

###22.2-5

广度优先搜索和次序无关，但和起始点有关

###22.2-6

###22.2-7
给每个结点增加一个属性，定为 good（好人）或者 bad（坏人）。

链表中的结点的属性必与该链表头结点属性相反。按照这个规则遍历每个链表，第一个链表表头结点属性设为 good，若在遍历过程，发现新定义的属性与先前定义的属性冲突，则不可以划分；反之，则可以划分，并且属性为 good 和 bad 的结点就是相应的两类。

###22.2-8
根据树的直径的性质：树的直径的路径的两个端点之一必定是以任一结点为源结点的最大路径的末结点。所以运行两次 BFS 搜索，第一次随机选择一个结点作为源结点；第二次选择第一次搜索结果中路径最大值的结点作为源结点，第二次 BFS 搜索得出的最大路径即为树的直径。时间复杂度为O(V+E)。



#22.3 深度优先搜索
```
DFS(G)
for each v in G
    v.color = WHITE
    v.pi = NULL
time = 0
for each v in G
    if v.color = WHITE
        DFS_VISIT(G,v)
        
DFS_VISIT(G,v)
time++
v.d=time
v.color = GRAY
for each u in Adj[v]
    if u.color = WHITE
        u.pi=v
        DFS_VISIT(G,u)
time++
v.f=time
v.color = BLACK
```

树边、前向边、后向边、横向边
#22.4 拓扑排序
```c
TOPOLOGICAL_SORT(G)
call DFS(G) to computing finsh time v.f for each vertex
as each vertex is finshed,insert it onto the front of a linked list
return the linked list
```

拓扑排序的运行时间和DFS是一样的$\Theta(V+E)$

##练习
###22.4-3
利用DFS来做，发现一条边被标记为访问状态，即返回FALSE
或者边数>=|V|直接报FALSE
否则报TRUE
在这里边数一定小于顶点数
#22.5 强连通分量
两次DFS
第一次正常，第二次完成时间递减顺序访问