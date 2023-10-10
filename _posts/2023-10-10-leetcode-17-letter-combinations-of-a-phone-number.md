---
layout: post
title: Leetcode 17 - Letter Combinations of a Phone Number
category: [Leetcode]
date: 2023-10-10 15:12 +0800
---

I used the backtrack method to solve this problem, with a time complexity of O(4^n), where n is the length of digits, since there are maximum 4 recursive calls for each digit. 

```python
def letterCombinations(self, digits: str) -> List[str]:
    digitToChar = {
        "2" : "abc",
        "3" : "def",
        "4" : "ghi",
        "5" : "jkl",
        "6" : "mno",
        "7" : "pqrs",
        "8" : "tuv",
        "9" : "wxyz"
    }

    output = []

    #backtracking
    def backtrack(i, currentStr) :
        if len(currentStr) == len(digits) :
            output.append(currentStr)
            return
        
        for char in digitToChar[digits[i]]:
            backtrack(i + 1, currentStr + char)
    
    if digits: # check if digits is not empty 
        backtrack(0,"")
    return output
```