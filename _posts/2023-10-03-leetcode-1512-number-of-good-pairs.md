---
layout: post
title: Leetcode 1512 - Number of Good Pairs
category: [Leetcode]
date: 2023-10-03 19:12 +0800
---

This was an easy question but I wanted to note down a mistake I have made when first attempting this question. I did not think of simply using the fact that the number of possible pairs can be calculated by simply continuously adding the occurrences of the current number up till, but not including, the current count. 

I initally tried to use the combinations method, which took a rather long runtime :
```python
def numIdenticalPairs(self, nums: List[int]) -> int:

    dictionary = defaultdict(list)
    numPairs = 0

    for index, num in enumerate(nums) : 
        dictionary[num].append(index) 
    
    for lst in dictionary.values() : 
        numPairs += math.comb(len(lst),2)
    
    return numPairs
```

Hence I modified my code and resulted in a much better runtime .
```python
dictionary = defaultdict(int)
numPairs = 0

for num in nums : 
    numPairs += dictionary[num]
    dictionary[num] += 1
    
return numPairs
```