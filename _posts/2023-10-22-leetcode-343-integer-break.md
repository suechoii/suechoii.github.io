---
layout: post
title: Leetcode 343 - Integer Break
cateogry: [Leetcode]
date: 2023-10-22 16:09 +0800
---

```python 
dp = { 1 : 1 }

for num in range(2, n + 1) :
    dp[num] = 0 if num == n else num
    for i in range(1, num) : 
        val = dp[i] * dp[num - i]
        dp[num] = max(dp[num], val)

return dp[n]

```