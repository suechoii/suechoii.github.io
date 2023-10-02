---
layout: post
title: Leetcode 2038 - Remove Colored Pieces if Both Neighbors are the Same Color
category: [Leetcode]
date: 2023-10-03 01:16 +0800
---

When I first attempted this question, time limit exceeded for some test cases. Here is my initial code : 

```python
def winnerOfGame(self, colors: str) -> bool:
    isAlice, isWinner = True, False
    aliceString, bobString = "AAA", "BBB"
    while not isWinner : 
        if isAlice and aliceString in colors: 
            indexToRemove = colors.index(aliceString) + 1
            isAlice = False
        elif not isAlice and bobString in colors :
            indexToRemove = colors.index(bobString) + 1 
            isAlice = True
        else : 
            if isAlice : 
                return False 
            else :
                return True 
        colors = colors[:indexToRemove] + colors[indexToRemove +1 :]
```
When I re-read through my code , I realized that I do not need to actually remove the color from the list, I can simply count the occurrences of "AAA" and "BBB". My initial approach was inefficient as it had to check for "AAA" or "BBB" each turn, hence the reason for exceeding the time limit.

Hence I modified my code to have a time complexity of O(n) and a space complexity of O(1).
Here is my modified code : 
```python
def winnerOfGame(self, colors: str) -> bool:

    alice, bob = 0, 0 

    for i in range(1, len(colors)) : 
        if colors[i-1:i+2] == "AAA" :
            alice += 1
        elif colors[i-1:i+2] == "BBB" :
            bob += 1
    
    return alice > bob 
```
This way, I was able to pass all test cases, however, I was not quite satisfied with the runtime. I thought this might be due to the if statements checking a substring instead of a character, hence further modified my code :

```python
for i in range(1, len(colors)-1) : 
    if colors[i-1] == colors[i] == colors[i+1] : 
        if colors[i] == "A" :
            alice += 1
        else :
            bob += 1
```

This did improve the runtime by 100ms, which made me reflect on my coding practices. 