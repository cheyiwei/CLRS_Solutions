# Chap 24 单源最短路径

标签（空格分隔）： 算法导论

---

#序言
单源最短路径问题：
```c
INITIALIZE_SINGLE_SOURCE(G,s)
for v in G
    v.d=infty
    v.pi=NIL
s.d=0
s.pi=NIL
```
松弛操作
```c
RELAX(u,v,w)
if v.d>u.d+w(u,v)
    v.d = w(u,v)
    v.pi = u
```
#24.1 Bellman-Ford 算法

边的权值可以为负，该算法对边使用松弛操作来渐进的降低从s到每个结点v的最短路径
```c
Bellman_Ford(G,s,w)
INITIALIZE_SINGLE_SOURCE(G,s)
for i = 1 to |G.V| - 1
    for each edge(u,v) in G.E
        REALEX(u,v,w)
for each edge(u,v) in G.E
    if v.d > u.d+w(u,v)
        return FALSE
return TRUE
```
运行时间$O(VE)$

##练习
###24.1-1 
图略
###24.1-3
记录下s.d，只要s.d不变即可

#24.3 Dijkstra 算法
```c
Dijkstra(G,w,s)
INITILAIZE_SINGLE_SOURCE(G,s)
S=NULL
Q=G.V
while Q!= NULL
    u = EXTRACT_MIN(Q)
    S=S+u
    for each v in Adj[u]
        RELAX(u,v,w)
```



