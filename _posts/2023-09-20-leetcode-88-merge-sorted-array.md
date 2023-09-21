---
layout: post
title: Leetcode 88 - Merge Sorted Array
categories: [Leetcode]
date: 2023-09-20 15:23 +0800
---

My first approach to this problem was to assign pointers to the beginning of each array and compare the current value of each array to see where the current value of the second array should be inserted in the first array. It worked, but gave 46ms runtime, which I thought could be improved. 

Hence, the second approach was to use the same logic, but this time moving the pointers in a descending order. Since the first array has enough space for the elements in the second array, we can compare the last value in the first array with the last value in the second array and place the larger one in the last slot of the first array. I made sure to cover the cases where there are leftover elements in the second array. Here is my newly implemented code : 

```python
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        num1_pointer = m-1
        num2_pointer = n-1

        current_pointer = m + n - 1

        while num2_pointer >= 0 :
            if nums1[num1_pointer] > nums2[num2_pointer]:
                nums1[current_pointer] = nums1[num1_pointer]
                num1_pointer -= 1
            else :
                nums1[current_pointer] = nums2[num2_pointer]
                num2_pointer -= 1
        
            current_pointer -= 1 

            if num1_pointer < 0 :
                nums1[:current_pointer+1] = nums2[:num2_pointer+1] #fill nums1 with leftover nums2 elements
                break
```

After writing this code, I realized that it is more time-effective to use m, n directly. Here is the code from NeetCode , it is much more concise. I will take this as the learning point to write more concise lines of code. There is no difference in runtime though.

```python
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    last = m + n -1

    while m > 0 and n > 0 :
        if nums1[m-1] > nums2[n-1]:
            nums1[last] = nums1[m-1]
            m -= 1
        else :
            nums1[last] = nums2[n-1]
            n -= 1
        last -= 1
    
    while n > 0 : # fill nums1 with leftover nums2 elements
        nums1[last] = nums2[n-1]
        n, last = n - 1, last - 1
```