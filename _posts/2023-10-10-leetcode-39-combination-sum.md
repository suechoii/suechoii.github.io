---
layout: post
title: Leetcode 39 - Combination Sum
category: [Leetcode]
date: 2023-10-10 16:37 +0800
---

As my previous post was on backtracking, I tried solving a similar question. Here is my initial code with the backtracking approach : 

```python
def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []

    def backtrack(current, targ) : 
        if targ == 0 and len(current) != 0:
            current.sort()
            if current not in result : 
                result.append(current)
            return
        elif targ < 0 :
            current = []
            return
        print(current)

        for candidate in candidates : 
            backtrack(current + [candidate], targ-candidate)

    backtrack([], target)

    return result
```

I realized this is quite inefficient as it checks for duplicates each time. Hence modified the code . Below is a diagram from Neetcode with explains the two recursion calls each time. 

```python
def backtrack(i, current, total) : 
    if total == target:
        result.append(current)
        return
    elif total > target or i >= len(candidates):
        return
    
    backtrack(i, current + [candidates[i]], total + candidates[i])
    backtrack(i+1, current, total)

backtrack(0, [], 0)
```
![tree diagram](/assets/img/tree.png)