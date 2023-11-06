---
layout: post
title: Leetcode 76 - Maximum Window Substring
category: [Leetcode]
date: 2023-11-04 23:01 +0800
---

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        #base case when t = ""
        if t == "" : return ""
        #use of hashmaps - count of all alphabets of t in s is greater than or equal to the count of all alphabets in t 
        window_hashmap, t_hashmap = defaultdict(int), defaultdict(int)

        for c in t :
            t_hashmap[c] += 1
        
        have, need = 0, len(t)
        res, minimum = [-1,-1], float("infinity")

        l = 0

        #expand sliding window to the right
        for r in range(len(s)) : 
            c = s[r]
            if c in t_hashmap : #only if in t 
                window_hashmap[c] += 1
                if window_hashmap[c] <= t_hashmap[c] : #if there are duplicate letters
                    have += 1 

            while have == need : 
                # if less than minimum
                if (r - l + 1) < minimum : 
                    res = [l, r]
                    minimum = r - l + 1
                # pop left of window to see if there is smaller substring
                if s[l] in t_hashmap:
                    window_hashmap[s[l]] -= 1 
                    if window_hashmap[s[l]] < t_hashmap[s[l]] :
                        have -= 1
                l += 1

        return s[res[0]:res[1]+1] if minimum != float("infinity") else ""
```