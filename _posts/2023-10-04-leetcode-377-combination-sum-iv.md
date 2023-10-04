---
layout: post
title: Leetcode 377 - Combination Sum IV
category: [Leetcode]
date: 2023-10-04 18:27 +0800
---

This was a rather easy dp question - solved it with a time complexity of O(n * target) and linear space complexity of O(target).

Here is my code : 
```python
def combinationSum4(self, nums: List[int], target: int) -> int:

    dp = defaultdict(int)
    dp[0] = 1

    for i in range(1, target + 1):
        for n in nums : 
            if i - n >= 0 : 
                dp[i] += dp[i-n]
    
    return dp[target]
```
