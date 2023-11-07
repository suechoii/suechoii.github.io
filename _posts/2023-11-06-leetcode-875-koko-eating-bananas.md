---
layout: post
title: Leetcode 875 - Koko eating Bananas
category: [Leetcode]
date: 2023-11-06 17:37 +0800 
---

```python
def minEatingSpeed(self, piles: List[int], h: int) -> int:

    def feasible(eatingSpeed) -> bool : 
        return sum((pile-1) // eatingSpeed + 1 for pile in piles) <= h

    left, right = 1, max(piles) #smallest possible k is 1, largest possible k is max(piles)

    while left < right : 
        mid = left + (right - left) // 2
        if feasible(mid) : 
            right = mid
        else :
            left = mid + 1
    
    return left 
```
