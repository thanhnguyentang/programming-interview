

# Palindrome 
A palindrome string is a string that does not change when its characters are reversed. For example `abcba` is a palindrome but `abcbad` is not. 
Checking if a string is a palindome is straightforward: iterate over all characters and compare `A[i]` with `A[n-1-i]` which takes `O(n)` time. 

## Problem 1 
Given a string `A`, find the lenght of the longest prefix of `A` that is a palindome. 

A brutal force solution iterates over all possible prefixes of `A` and check if each prefix is a palindome. This takes `O(n^2)` time. There is a data structure that can solve this problem in `O(n)` time using [**LPS (longest prefix-suffix) array**](https://www.youtube.com/watch?v=tWDUjkMv6Lc). In particular, given an array `B`, `LPS` of `B` has the same length of `B` where 
> `LPS[i]` is the length of the longest proper prefix of `B[0:i+1]` that is also a proper suffix. For example, if `B = 'abca'`, then `LPS = [0,0,0,1]`. Constructing LPS takes `O(n)`. 

Now, applying the idea of LPS to the problem 1 above as follows: 

  * `B = A + '$' + A[::-1] # use the symmetry of palindome`
  *  Construct LPS of `B`
  *  `LPS[-1]` is the length of the longest prefix of `A` that is a palindome


An application of the problem 1 above is that problem of finding the minimal number of characters inserted in the beginning of a string to make it a palindrome, as presented [here](https://www.interviewbit.com/problems/minimum-characters-required-to-make-a-string-palindromic/).
