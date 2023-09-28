---
layout: post
title: Leetcode 130 - Surrounded Regions
category: [Leetcode]
date: 2023-09-29 01:34 +0800
---

Since I solved this question a day after 200. Number of islands, I naturally thought of solving it using the similar DFS approach. One trick here, was that the "connected component" shouldn't be on the border. Hence, I divided my solution into two parts : a) Check for connected "O" components on the border, then mark them as visited. b) Iterate through the board and search for "O"s - then replace them with "X" if not visited. 

Time complexity is O(mn). One minor improvement that could be made would be less use of memory.

Here is my code :
```python
def solve(self, board: List[List[str]]) -> None:
    """
    Do not return anything, modify board in-place instead.
    """
    m, n = len(board), len(board[0])

    visited = [[False] * n for _ in range(m)]

    def dfs(i,j) :
        if i >= 0 and j >= 0 and i < m and j < n and board[i][j] == "O" and not visited[i][j]: #O shouldn't be on the edge
            visited[i][j] = True
            dfs(i+1, j)
            dfs(i-1, j)
            dfs(i, j+1)
            dfs(i, j-1)

    for r in range(m):
        for c in range(n):
            if (r == 0 or c == 0 or r == m-1 or c == n-1) and board[r][c] == "O":
                dfs(r,c) #mark all regions connected to borderline "O" as visited

    for r in range(1,m-1):
        for c in range(1,n-1):
            if board[r][c] == "O" and not visited[r][c] :
                board[r][c] = "X"
```

