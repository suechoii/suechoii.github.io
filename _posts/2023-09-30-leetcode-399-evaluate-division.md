---
layout: post
title: Leetcode 399 - Evaluate Division
category: [Leetcode]
date: 2023-09-30 23:53 +0800
---
This took me a long time to solve, due to two reasons : 1) struggled creating a graph with weights , 2) was unfamiliar with defaultdict , initially tried to create the adjacency list with an empty dictionary . I would definitely like to revisit this question after solving a few more graph problems. 


```python
def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
    
    adjacencyList = collections.defaultdict(list)

    # create a graph with key as src, and value as [dest, value] pair
    for i, eq in enumerate(equations) :
        start, end = eq
        adjacencyList[start].append([end, values[i]])
        adjacencyList[end].append([start, 1/values[i]])
    
    def bfs(src, dst) :
        q = deque()
        visited = set()

        # if not found in adjacency list, immediately return -1 
        if src not in adjacencyList :
            return - 1
        
        q.append([src, 1]) # default value as 1
        visited.add(src)

        while q :
            current, weight = q.popleft()
            if current == dst : 
                return weight
            # iterate through neighbors and update their weight
            for neighbor, neighborWeight in adjacencyList[current] :    
                if neighbor not in visited : 
                    q.append([neighbor, weight * neighborWeight])
                    visited.add(neighbor)
        
        return -1

    output = []
    for query in queries : 
        output.append(bfs(query[0],query[1]))
    
    return output
```
