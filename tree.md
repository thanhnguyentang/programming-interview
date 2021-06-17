## Binary lifting: 
dp[i][j]: the 2^j-th parent of node i in a bidirectional graph 

LCA: Lowest Common Ancestor 

```
lca(u,v, log, lev, dp):
  if u > v:
    u,v = v,u
    for j in range(log, -1,-1):
      if (lev[v] - 2**j) >= lev[u]: 
        v = dp[v][j]
        
    if v == u:
      return v 
    for j in range(log,-1,-1):
      if dp[u][j] != dp[v][j]:
        u = dp[u][j] 
        v = dp[v][j] 
    return dp[u][0]
```
