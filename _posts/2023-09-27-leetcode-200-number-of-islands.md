---
layout: post
title: Leetcode 200 - Number of Islands
category: [Leetcode]
date: 2023-09-27 22:56 +0800
---

I used BFS to approach this problem, but was unable to pass two testcases. 

Here is my original code :
```python
def numIslands(self, grid: List[List[str]]) -> int:

    visited = []
    numberIslands = 0

    m , n = len(grid), len(grid[0])

    def bfs(r,c) :
        q = deque()
        q.append((r,c))
        visited.append((r,c))

        while q : 
            now = q.popleft()
            directions = [[1,0], [0,1], [-1,0], [0, -1]]
            for dx,dy in directions : 
                r, c = now[0] + dx, now[1] + dy 
                if r >= 0 and c >= 0 and r < m and c < n : # 좌표 유효성 검사
                    if grid[r][c] == "1" and (r,c) not in visited:
                        visited.append((r,c))
                        q.append((r,c))



    for r in range(m) :
        for c in range(n) :
            if grid[r][c] == "1" and (r,c) not in visited :
                bfs(r,c)
                numberIslands += 1
    
    return numberIslands
```
Time limit exceeded, so I tried changing visited to a list of booleans :

```python
visited = [[False] * n for _ in range(m)]
...
if grid[r][c] == "1" and not visited[r][c]:
                            visited[r][c] = True
                            q.append((r,c))
```