---
layout: post
title: Leetcode 93 - Restore IP Addresses
category: [Leetcode]
date: 2023-10-13 16:57 +0800
---
My initial approach had a poor runtime so I looked for ways to improve the runtime. Here is my initial code : 

I used current variable to temporary store the "sub" ip-addresses, and created two branches each time - one branch including the current sub ip-address, and one branch which doesn't. 

```python
def restoreIpAddresses(self, s: str) -> List[str]:
    output = []

    def backtrack(i, current, ip) :
        if len(ip) == 4 and len("".join(ip)) == len(s):
            output.append(".".join(ip))
        
        if len(current) > 3 or i >= len(s) :
            return 
        
    
        current += s[i]
        
        if 0 <= int(current) <= 255 :
            backtrack(i+1, "", ip + [str(int(current))])
            backtrack(i+1, current, ip)

    backtrack(0, "", [])
    return output
```
After exploring the question further, I realized that the current variable is unneccessary. This updated solution has a time complexity of O(3^5).


```python
def restoreIpAddresses(self, s: str) -> List[str]:
    output = []

    def backtrack(i, ip) :
        if len(ip) == 4 and i == len(s):
            output.append(".".join(ip))
            return
        
        if len(ip) > 4: 
            return
        
        for j in range(i, min(i+3, len(s))) :
            if int(s[i:j+1]) < 256 and (i == j or s[i] != "0"): #check fo any leading zeros.
                backtrack(j+1, ip + [s[i:j+1]])

    backtrack(0, [])
    return output
```

