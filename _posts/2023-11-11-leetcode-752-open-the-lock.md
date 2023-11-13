---
layout: post
title: Leetcode 752 - Open the Lock
category: [Leetcode]
date: 2023-11-11 22:11 +0800
---

The initial passcode is 0000 so there are eight possible combinations after one turn : [1000, 9000, 0100, 0900, 0010, 0090, 0001, 0009]. We want to find the **shortest path**, hence we need a <u>hashset</u> to store the previously created lock passwords. This is basically an application of Breadth-First Search (BFS). Since we don't want to visit the deadends, it would be wise to initialize the visit hashset with the list of deadends first. 

```python
def openLock(self, deadends: List[str], target: str) -> int:
    # if there happen to be "0000" in deadends :
    if "0000" in deadends:
        return -1


    # get all 8 combinations
    def getChild(password) -> List[str]:
        children = []
        for i, num in enumerate(password) :
            newDigit1, newDigit2 = (int(num) + 1) % 10, (int(num) - 1 + 10) % 10
            child1, child2 = password[:i] + str(newDigit1) + password[i+1:] , password[:i] + str(newDigit2) + password[i+1:]
            children.append(child1)
            children.append(child2)
        return children

    
    # bfs
    q = deque([("0000",0)])
    visited = set(deadends) # to ensure the deadends are not visited

    while q :
        password, moves = q.popleft()
        if password == target :
            return moves 
        for child in getChild(password) :
            if child not in visited : 
                q.append((child, moves + 1))
                visited.add(child)

    return -1 
```