---
layout: post
title: Leetcode 787 - Cheapest Flights Within K Stops
category: [Leetcode]
date: 2023-11-15 23:25 +0800
---

I'll be solving this with Bellman-Ford (BFS), with time complexity of O(E \* k). BFS of k + 1 layers is needed because k is the number of stops (nodes).

```python
def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:

        #use of bellman-ford
        prices = [float("inf")] * n
        prices[src] = 0 # starting point

        for i in range(k + 1) :
            tempPrices = prices.copy()
            for s, d, p in flights : # update path up to ith node
                if prices[s] == float("inf") :
                    continue
                if prices[s] + p < tempPrices[d]:
                    tempPrices[d] = prices[s] + p
            prices = tempPrices


        return -1 if prices[dst] == float("inf") else prices[dst]
```
