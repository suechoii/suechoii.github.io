---
layout: post
title: Leetcode 90 - Subsets II
category: [Leetcode]
date: 2023-11-06 17:01 +0800
---

Here is my initial backtracking approach : 
```python
def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
    res = []
    nums = sorted(nums)

    def backtrack(i, curr) : 
        if curr not in res : 
            res.append(curr)

        if i >= len(nums):
            return
    
        backtrack(i+1, curr + [nums[i]]) 
        backtrack(i+1, curr) 

    backtrack(0, [])

    return(res)
```
However, this had poor running time, and modifying the backtracking function to below improved it - checking for duplicates in a more efficient way :

```python
  def backtrack(i, curr) : 
    if i == len(nums) :
        res.append(curr[::]) #copy
        return
    
    #All subsets that include nums[i]
    curr.append(nums[i])
    backtrack(i+1, curr)
    curr.pop()

    #All subsets that don't include nums[i]
    while i + 1 < len(nums) and nums[i] == nums[i+1] : #duplicates
        i+=1 
    backtrack(i+1, curr)
```

This is my final backtracking function : 
```python
def backtrack(i, curr) : 
    if i == len(nums):
        res.append(curr)
        return

    backtrack(i+1, curr + [nums[i]]) 
    while i + 1 < len(nums) and nums[i] == nums[i+1] : # check duplicates
        i += 1
    backtrack(i+1, curr) 
```
