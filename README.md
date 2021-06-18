# programming-interview

## 1. Strings 

### Palindrome 
A palindrome string is a string that does not change when its characters are reversed. For example `abcba` is a palindrome but `abcbad` is not. 
Checking if a string is a palindome is straightforward: iterate over all characters and compare `A[i]` with `A[n-1-i]` which takes `O(n)` time. 

#### Problem 1 
Given a string `A`, find the length of the longest prefix of `A` that is a palindome. 

A brutal force solution iterates over all possible prefixes of `A` and check if each prefix is a palindome. This takes `O(n^2)` time. There is a data structure that can solve this problem in `O(n)` time using [**LPS (longest prefix-suffix) array**](https://www.youtube.com/watch?v=tWDUjkMv6Lc). In particular, given an array `B`, `LPS` of `B` has the same length of `B` where 
> `LPS[i]` is the length of the longest proper prefix of `B[0:i+1]` that is also a proper suffix. For example, if `B = 'abca'`, then `LPS = [0,0,0,1]`. Constructing LPS takes `O(n)`. 

```
def LPS(A): 
    """
    Dynamic programming 
    
    Key idea: given j = lps[i-1], how to compute lps[i]? 
    """
    lps = [0] * len(A) 
    for i in range(1, len(A)): 
        j = lps[i-1]
        while A[i] != A[j] and j > 0:
            j = lps[j-1] 
        if A[i] == A[j]: 
            lps[i] = j + 1 
            j = j + 1 
        else: 
            lps[i] = 0

    return lps 


A = 'abca'
lps = LPS(A) 
print(lps)
```


Now, applying the idea of LPS to the problem 1 above as follows: 

  * `B = A + '$' + A[::-1] # use the symmetry of palindome`
  *  Construct LPS of `B`
  *  `LPS[-1]` is the length of the longest prefix of `A` that is a palindome


An application of the problem 1 above is that problem of finding the minimal number of characters inserted in the beginning of a string to make it a palindrome, as presented [here](https://www.interviewbit.com/problems/minimum-characters-required-to-make-a-string-palindromic/).

## 2. Graph/tree 

### Binary lifting: 
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

## 3. Dynamic Programming 

### Knapsack problem

Knapsack problem is a combinatoric optimization that can be solved by dynamic programming. 
Given n items, each has its own weights and values. Which subset of elements to be put into a knapsack such that the total value is maximized while the total weight does not exceed a certain value? 

A similar question is which subset of elements to be put into a knapsack such that the number of elements is minimized while the total value is equal to a certain value. 
