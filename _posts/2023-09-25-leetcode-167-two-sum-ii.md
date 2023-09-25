---
layout: post
title: Leetcode 167 - Two Sum II
category: [Leetcode]
date: 2023-09-25 09:29 +0800
---

This question confused me a little. I initially thought of using two pointers, one starting from index 0, and the other starting from the last index. Then I changed my thoughts, thinking that we might miss out a combination this way. However, after coming up with brute-force code, I realized that the elements are always first being removed from the last element in the list, in descending order, given that the list is sorted in ascending order. Hence I realized we do not have to do it the brute force way (with time complexity of O(n^2)), and stick with my initial strategy (with time complexity of O(n)).

Here is my code : 
```python
def twoSum(self, numbers: List[int], target: int) -> List[int]:
        
    pointer1 = 0
    pointer2 = len(numbers) - 1

    while pointer1 < pointer2 :
        if numbers[pointer1] + numbers[pointer2] == target :
            return [pointer1+1, pointer2+1]
        
        if numbers[pointer1] + numbers[pointer2] > target:
            pointer2 -=1
        else : 
            pointer1 +=1
```
