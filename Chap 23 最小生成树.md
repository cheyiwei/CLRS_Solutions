# Chap 23 最小生成树
标签（空格分隔）： 算法导论

---

#23.1 最小生成树的形成

本节介绍了我们生成最小生成树的基本思想：不断加入一条safe边

就是将最小割边不断加入到集合中

##练习
###23.1-4
三角形三条边权值相等
###23.1-5
利用反证法可轻易证明

#Kruskal算法和Prim算法
```c
MST_KRUSKAL(G,w)
A=NULL
for each v in G
    MAKE_SET(v)
按照边的权重排序
for each edge(u,v) in G.E
    if FIND_SET(u) != FIND_SET(v)
        A=A+{edge(u,v)}
        UNION(u,v)
return A
```
PRIME算法是选择一个最小的割边加入，Kruskal算法是每次加入一个权值最小的
```c
MST_PRIM(G,w,r)
for each v in G.V
    u:key = infty
    u:pi = NIL
r:key = 0
Q=G.V
while Q!=NULL
    u=EXTRACT_MIN(Q)
    for each v in Adj[u]
        if v in Q && w(u,v) < v.key
            v.pi = u
            v.key = w(u,v)
```
上述两个算法渐进时间都是$O(E\lg V)$

##练习


