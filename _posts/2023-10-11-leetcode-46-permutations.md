---
layout: post
title: Leetcode 46 - Permutations
category: [Leetcode]
date: 2023-10-11 23:55 +0700
---

This was one of the most difficult questions, I will be coming back to this again. For now, I will leave the easiest solution I have found in the discussion section. 

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtrack(path):
            if len(path) == len(nums):
                ans.append(path[:])
                return 

            for num in nums:
                if num not in path:
                    path.append(num)
                    backtrack(path)
                    path.pop()

        backtrack([])
```
I personally liked the idea of swap as well :

```python
def backtrack(i) :
    if i >= len(nums):
        result.append(nums[:])
    
    for j in range(i, len(nums)):
        nums[i], nums[j] = nums[j], nums[i]
        backtrack(i+1)
        nums[i], nums[j] = nums[j], nums[i]
```