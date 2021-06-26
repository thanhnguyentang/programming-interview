# programming-interview

## 1. Strings 

### Palindrome 
A palindrome string is a string that does not change when its characters are reversed. For example `abcba` is a palindrome but `abcbad` is not. 
Checking if a string is a palindome is straightforward: iterate over all characters and compare `A[i]` with `A[n-1-i]` which takes `O(n)` time. 

* **Example**: Given a string `A`, find the length of the longest prefix of `A` that is a palindome. 

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


An application of the example above is that problem of finding the minimal number of characters inserted in the beginning of a string to make it a palindrome, as presented [here](https://www.interviewbit.com/problems/minimum-characters-required-to-make-a-string-palindromic/).

### Longest common substring (LCS)

The **Longest common substring** (LCS) uses Dynamic Programming to find the longest common substrings of two strings. 
Let `dp[i][j]` be the LCS of `A[:i+1]` and `B[:j+1]`, the recursion for `dp` is: 
```
if A[i] == B[j]:
    dp[i][j] = dp[i-1][j-1] + 1 
else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

## 2. Graph/tree 

### Binary lifting: 
Given a graph/tree, **Binary Lifting** is a dynamic programming approach that stores the `2^j-th` ancesstor of node `i` into `dp[i][j]`.
**Lowest Common Ancestor** (LCA) is a very useful algorithm that leverages the idea of binary lifting. We will demonstrate LCA in the following. 
Let `lev[i]` be the level of node `i`. Here we use the convention that the root has the lowest level.

Given two nodes `u` and `v`. If they are not at the same level, we move up the higher-level node to its ancesstor of the same level as the other node. 
This moving-up works as follows: iterate over all the level range in an ascending order to find the highest number of levels (in the powers of `2`) we could move up the higher-level node to a position that is just below the level of the other node. When we keep iterate the level futher in an ascending order, it guarantees to move up the higher-level node to the same level as the other node. 

When two nodes have the samle level, check if they are identical. If so, the initial lower-level node is the LCA. If not, we move up these nodes until 
they reach the lowest level on which they are different. The idea for this moving-up is similar to that in the previous paragraph. After that, just move up one level and we get the LCA. 

```
lca(u,v, log, lev, dp):
    if lev[u] >= lev[v]:
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
### Depth-First Search (DFS)  

DFS searchs all the ways down the depth before moving to the next branch. It uses `stack` to implement the bread-first direction. 
```
def dfs(G): 
    stack = []
    stack.append(root) 
    while len(stack):
        node = stack.pop() 
        visit(node) 
        for u in node.children:
            stack.append(node) 
```

How to store the path from the root to a target node in a tree/graph? We can use DFS. An modification of DFS for this is that 
`stack` is a collection of `[node, path]` where `path` stores the path from the root to the parent of `node`. An example: [Path To Given Node](https://www.interviewbit.com/old/problems/path-to-given-node/) problem in InterviewBit. 

### Trie 
The name `trie` is from ReTRIeval. A Trie, a.k.a. **prefix tree**, is a tree data structure that comes up a lot in programming interview. Each node of a Trie is a character and each path down the trie represents a word. Tries are good for **quick prefix lookups**

A trie can check if a string is a valid prefix of a word in `O(k)` time where `k` is the lenght of the string.

* **Example**: ([Maximum Xor between two arrays](https://www.interviewbit.com/old/problems/xor-between-two-arrays/)) Given two integer array `A` and `B`, pick one element from each array such that their xor is maximum.

## 3. Dynamic Programming 

### Knapsack problem

Knapsack problem is a combinatoric optimization that can be solved by dynamic programming. 
Given n items, each has its own weights and values. Which subset of elements to be put into a knapsack such that the total value is maximized while the total weight does not exceed a certain value? 

A similar question is which subset of elements to be put into a knapsack such that the number of elements is minimized while the total value is equal to a certain value. 

## 4. Bit Manipulation 

* Left shift: `a << i` is equivalent to `a 2^i` 
* Right shift: `a >> i` is equivalent to `a // 2^i` 
* Negative numbers in a `N`-bit representation: `1` followed by the bit representation of `2^(N-1) - k`. For example, for `N=4`, `-2 = 1110` as `6 = 110`. 
* Create `mask = 0...0 1 1 ... 1` with `i` `1s` at the tail: `mask = (1 << i) - 1`
* Create `mask = 11...10...0` with `i` `0s` at the tail: `mask = -1 << i` 
* Create `mask = 11...1011..1` with `i` `1s` at the tail: `mask = ~(1 << i)`
* `x ^ 1 = ~x`
* `x ^ 0 = x` 
