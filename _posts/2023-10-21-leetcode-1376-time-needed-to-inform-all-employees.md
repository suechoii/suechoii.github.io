---
layout: post
title: Leetcode 1376 - Time Needed To Inform All Employees
category: [Leetcode]
date: 2023-10-21 21:25 +0800
---

Solved this with BFS, with time complexity of O(n)
```python
def numOfMinutes(self, n: int, headID: int, manager: List[int], informTime: List[int]) -> int:
        # create an adjacency list (manager -> employees) - hashmap
        adj = defaultdict(list)   
        for i in range(n) : 
            adj[manager[i]].append(i)

        # BFS 
        q = deque([(headID, informTime[headID])])
        res = 0
        while q : 
            i, time = q.popleft()
            res = max(res,time)
            for employee in adj[i]:
                q.append((employee, time + informTime[employee]))
            
        return res



```