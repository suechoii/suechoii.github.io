---
layout: post
title: Leetcode 42 - Trapping Rain Water
category: [Leetcode]
date: 2023-11-08 23:53 +0800
---

First approach was to make a list of all the minimum values, using the concept of min(maxL, maxR). However this took a long time as we had to find maxL and maxR for each height : 

```python
 def trap(self, height: List[int]) -> int:

    minimums = [0 for _ in range(len(height))]

    for i in range(1,len(height)-1) :
        minimums[i] = min(max(height[:i]), max(height[i+1:]))
    
    res = 0

    for i, h in enumerate(height):
        water = minimums[i] - h
        if water > 0 :
            res += water
    
    return res 
```

Hence I modfiied the logic and made use of two pointers so that it can be completed in O(n) time and to get rid of extra space allocated for the minimum values => O(1) space complexity :

```python
def trap(self, height: List[int]) -> int:

    # Use of two pointers 
    l, r = 0, len(height)-1
    maxL , maxR = height[l], height[r]
    total = 0

    # Compare maxL and maxR values
    while l <= r: 
        if maxL < maxR :
            maxL = max(height[l], maxL) #no need to check > 0
            total += maxL - height[l]
            l += 1
        else :
            maxR = max(height[r], maxR)
            total += maxR - height[r]
            r -= 1

    return total

```