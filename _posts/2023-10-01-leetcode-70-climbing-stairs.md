---
layout: post
title: Leetcode 70 - Climbing Stairs
category: [Leetcode]
date: 2023-10-01 17:49 +0800
---

I tried solving this using DP. If observed closely, it can be said that this is similar to a Fibonacci sequence. For example, if you want to find out the answer to n = 3, we first can find out the number of ways to reach the 3rd step from i) the 3rd step, and ii) the 2nd step, which are both 1. To find out the number of ways to reach from the 1st step, we can add i) and ii) together, which sums up to 2. Hence the final answer would be the sum of 1 and 2 = 3. Likewise, this can be repeated for any n, until we reach the 0th step. Here is my code using DP : 

```python
def climbStairs(self, n: int) -> int:

    D = [0 for _ in range(n+1)]

    D[0], D[1] = 1 , 1
    
    for i in range(2,n+1): 
        D[i] = D[i-1] + D[i-2]

    return D[n]
```
Eventhough I am happy with the time complexity of O(n), I realized that there is a way to improve the space complexity from the current O(n) to O(1), by not using an array. Here is my modified code : 

```python 
def climbStairs(self, n: int) -> int:
    i1 , i2  = 1, 1
    for _ in range(2, n+1):
        current = i1 + i2
        i2 = i1
        i1 = current
    return i1
```