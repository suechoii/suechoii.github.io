---
layout: post
title: Leetcode 787 - Cheapest Flights Within K Stops
category: [Leetcode]
date: 2023-11-15 23:25 +0800
---

I'll be solving this with Bellman-Ford (BFS), with time complexity of O(E \* k). BFS of k + 1 layers is needed because k is the number of stops (nodes).
