---
layout: post
title: Leetcode 5 - Longest Palindromic Substring
category: [Leetcode]
date: 2023-10-05 16:16 +0800
---

I figured a way to solve this by iterating the string and checking the current character's left and ride side by using a while loop to see if its left and right character are the same each loop. Hence the time complexity is O(n^2). Here is my code, but I am trying to find a way to simplify it : 

```python
def longestPalindrome(self, s: str) -> str:
    result = ""
    longestLength = 0 

    # check whether s is of even or odd index

    for i in range(len(s)):
        l, r = i, i #odd 
        while l >= 0 and r < len(s) and s[l] == s[r] :
            if (r - l + 1) > longestLength :
                result = s[l:r+1]
                longestLength = r - l + 1
            
            r += 1 
            l -= 1
        
        l, r = i, i + 1 #even
        while l >= 0 and r < len(s) and s[l] == s[r] :
            if (r - l + 1) > longestLength :
                result = s[l:r+1]
                longestLength = r - l + 1
            
            r += 1 
            l -= 1
    
    return result
```