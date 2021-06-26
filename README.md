# programming-interview

# A. Data Structures 

## 1. Arrays 
@TODO: FILL IN

## 2. Strings

### Longest Prefix-Suffix (LPS)
First, let us consider the following basic problem: 

* **Example 1**: Given a string `A`, find the length of the longest proper prefix of `A` that is also a proper suffix. Note that the proper prefix and the proper suffix must not overlap. 

A brutal force solution would iterate over all possible proper prefixes of `A` and check if each of the proper prefix is also a proper suffice. Though this brutal force solution takes `O(n)`, there is a dynamic programming solution that is very helpful for many other string problems. This solution is called [`LPS`](https://www.youtube.com/watch?v=tWDUjkMv6Lc). 

Given a string `A`, we can construct a data structure `LPS` of length `n` in `O(n)` time where `LPS[i]` is the length of the longest proper prefix of `A[:i+1]` that is also a proper suffix (and the proper prefix and the proper suffix do not overlap). For example, if `A = 'abca'`, then `LPS = [0,0,0,1]`. 
The idea for `LPS` is based on dynamic programming where the key question is that given `j = LPS[i-1]`, how to compute `LPS[i]`? 

```
def LPS(A): 
    """    
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

* **Example 2 (Palindrome)**: A palindrome is a string that does not change when its characters are reversed. For example `abcba` is a palindrome but `abcbad` is not. Given a string `A`, find the length of the longest prefix of `A` that is a palindome. 

A brutal force solution iterates over all possible prefixes of `A` and check if each prefix is a palindome. This takes `O(n^2)` time. Using `LPS`, we can reduce to `O(n)` time. The key ideas are as follows

  *  Let `B = A + '$' + A[::-1]`. By the symmetry of palindome, the length of the longest prefix of `A` that is also a palindome must be the length of the longest proper prefix of `B` that is also a proper suffice. 
  *  Then, just solve `LPS` for `B` and return `LPS[-1]`

* **Example 3 ([Make A String Palindrome](https://www.interviewbit.com/problems/minimum-characters-required-to-make-a-string-palindromic/))**: Given an string `A`. The only operation allowed is to insert characters in the beginning of the string. Find how many minimum characters are needed to be inserted to make the string a palindrome string.

The key idea to solve the problem above is that a string `A` can be decomposed into two parts where the first part is the longest prefix of `A` that is also a palindrome. Thus, by the symmetry of palindrom, to make `A` a palindrome, we only need to insert the reversed version of the second part of `A` (in the beginning of `A`). 

### Longest Common Substring (LCS)
Another useful data structure for strings is the **Longest common substring** (LCS), the longest common substrings (not necessarily consecutive characters) of two strings. This `LCS` data structure can be constructed using Dynamic Programming. In particular, let `dp[i][j]` be the LCS of `A[:i+1]` and `B[:j+1]`, how to compute `dp[i][j]` given the previous values in the DP table? The key idea is that if `A[i] = B[j]`, this character must be counted in the longest common substring; otherwise, there are two possible cases: (i) `A[i]` is not included in the longest common substring, (ii) `B[j]` is not included in the longest common substring. Thus these operations are translated in the following recursion:
```
if A[i] == B[j]:
    dp[i][j] = dp[i-1][j-1] + 1 
else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

## 3. Linked Lists 

## 4. Stacks and Queues 

### Priority Queues 

## 5. Trees and Graphs

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

## Min-Heaps 

A min-heap is a *complete* binary tree (all the levels of the tree except for possibly the last one are totally fullfiled; if the last level is not complete, it is filled from the left to right). The value of each node in a min-heap does not exceed the value of any of its descendant. Thus, the minimum value is at the root of a min-heap. An example of a min-heap is as follows. 

```
       1
     /   \
    2     3 
   / \   
  4   5   

```

Two key operations in a min-heap: `insert`, `extract_min`

# B. Algorithms

## 1. Big-O 
@TODO: FILL IN

## 2. Dynamic Programming 

### Knapsack problem

Knapsack problem is a combinatoric optimization that can be solved by dynamic programming. 
Given n items, each has its own weights and values. Which subset of elements to be put into a knapsack such that the total value is maximized while the total weight does not exceed a certain value? 

A similar question is which subset of elements to be put into a knapsack such that the number of elements is minimized while the total value is equal to a certain value. 

## 3. Bit Manipulation 

* Left shift: `a << i` is equivalent to `a 2^i` 
* Right shift: `a >> i` is equivalent to `a // 2^i` 
* Negative numbers in a `N`-bit representation: `1` followed by the bit representation of `2^(N-1) - k`. For example, for `N=4`, `-2 = 1110` as `6 = 110`. 
* Create `mask = 0...0 1 1 ... 1` with `i` `1s` at the tail: `mask = (1 << i) - 1`
* Create `mask = 11...10...0` with `i` `0s` at the tail: `mask = -1 << i` 
* Create `mask = 11...1011..1` with `i` `1s` at the tail: `mask = ~(1 << i)`
* `x ^ 1 = ~x`
* `x ^ 0 = x` 

## 4. Dijkstra's Algorithm 
