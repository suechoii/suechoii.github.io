---
layout: post
title: Leetcode 2466 - Count Ways To Build Good Strings
category: [Leetcode]
date: 2023-10-14 19:52 +0800
---

There were two approaches to solve this problem - 1) Backtracking , 2) Dynamic Programming

1) 
```python 
def countGoodStrings(self, low: int, high: int, zero: int, one: int) -> int:

    dp = defaultdict(int)
    mod = 10**9 + 7

    def backtrack(currentLength) :

        if currentLength > high:
            return 0
        if currentLength in dp:
            return dp[currentLength]
        if currentLength >= low :
            dp[currentLength] = 1
        dp[currentLength] += backtrack(currentLength + zero) + backtrack(currentLength + one)

        return dp[currentLength] % mod
    
    return backtrack(0)
```
2)
```python
dp = defaultdict(int)
dp[0] = 1
mod = 10**9 + 7

for i in range(1, high+1) :
    dp[i] = (dp[i-one] + dp[i-zero]) % mod

return sum(dp[i] for i in range(low, high+1)) % mod
```

