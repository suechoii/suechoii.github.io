---
layout: post
title: Leetcode 3 - Longest Substring Without Repeating Characters
category: [Leetcode]
date: 2023-10-04 19:30 +0800
---

I solved this by checking whether the character is in the current substring each time, and slicing the substring to start from the first index of the duplicate character. 

This is my initial code : 
```python
def lengthOfLongestSubstring(self, s: str) -> int:

    length, currentLongest = 0, 0
    currentSubstring = ""

    for char in s :
        if char not in currentSubstring :
            currentSubstring += char
        else : 
            # continue from : last found index + 1
            lastFoundIndex = currentSubstring.index(char) 
            currentSubstring = currentSubstring[lastFoundIndex+1:] + char
        
        currentLongest = max(currentLongest, len(currentSubstring))
    
    return currentLongest
```

Then I realized duplicates can be easily found by using a set or a dictionary, and the use of pointers to create the sliding window. This would also have a time complexity of O(n) and a space complexity of O(n).

Using set : 
```python
def lengthOfLongestSubstring(self, s: str) -> int:
    length, currentLongest = 0, 0
    currentSubstring = set()
    l, r = 0, 0

    for r in range(len(s)) :
        while s[r] in currentSubstring : 
            currentSubstring.remove(s[l])
            l += 1
        
        currentSubstring.add(s[r])
        currentLongest = max(currentLongest, r - l + 1)
    
    return currentLongest
```

Using dictionary :
```python
def lengthOfLongestSubstring(self, s: str) -> int:
    length, currentLongest = 0, 0
    currentSubstring = {}
    l, r = 0, 0

    for r in range(len(s)) :
        if s[r] in currentSubstring and currentSubstring[s[r]] >= l: 
            l = currentSubstring[s[r]] + 1
        
        currentSubstring[s[r]] = r
        currentLongest = max(currentLongest, r - l + 1)
    
    return currentLongest
```