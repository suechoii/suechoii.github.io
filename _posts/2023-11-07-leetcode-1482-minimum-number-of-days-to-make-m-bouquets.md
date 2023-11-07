---
layout: post
title: Leetcode 1482 - Minimum Number of Days to Make m Bouquets
category: [Leetcode]
date: 2023-11-07 13:43 +0800
---
```python
def minDays(self, bloomDay: List[int], m: int, k: int) -> int:

    def feasible(days) -> bool : 
        flowers, bouquets = 0, 0
        for bloom in bloomDay : 
            if bloom  > days : 
                flowers = 0
            else :
                flowers += 1
                if flowers == k : 
                    bouquets += 1
                    if bouquets == m: break
                    flowers = 0 
        
        return bouquets == m 

    if len(bloomDay) < m * k:
        return -1

    left, right = 1, max(bloomDay)

    while left < right :
        mid = left + (right - left) // 2
        if feasible(mid) :
            right = mid
        else :
            left = mid + 1
    
    return left 
```