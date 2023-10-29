---
layout: post
title: Leetcode 122 - Best Time to Buy and Sell Stock II
category: [Leetcode]
date: 2023-09-20 23:27 +0800
---

This was rather simple, but only after I realized that this is a greedy problem with profit made whenever the price is less than the consequent. This has a time complexity of O(n) :

```python
def maxProfit(self, prices: List[int]) -> int:
        prices_sum  = 0

        for i in range(1,len(prices)) : 
            if prices[i] > prices[i-1] :
                prices_sum += (prices[i] - prices[i-1])
            
        return prices_sum
```