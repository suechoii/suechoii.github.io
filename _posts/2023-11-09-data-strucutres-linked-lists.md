---
layout: post
title: Data Structures - Linked Lists
category: [Data Structures]
date: 2023-11-09 16:59 +0800
---

## Linked List (연결 리스트)

- In Linked Lists, each element is known as a <u> node </u>. 
- Each node has <u> data </u> and a <u> pointer </u>.
- The node at the start of the list is known as the <u> head node </u>, wehre the node at the end is known as the <u> tail node </u>.
- The node right in front of the current referenced node is called the <u> predecessor node </u> , and the node right behind of it is called the <u> successor node </u>.
- Linked lists are dynamic , we **don't** have to define its size . However, it takes up **more memory** due to the address pointer

**Types of Linked List**
1. Singly-linked list : Each node points to the next node and the last node points to null 
![diagram](/assets/img/SinglyLinkedList.png)

2. Doubly-linked list : Each node has two pointers, prev and next, where prev points to the previous node and next points to the next node. The last node's next pointer points to null
![diagram](/assets/img/DoublyLinkedList.png)

3. Circular-singly-linked list : Each node points to the next node and the last node points back to the first node . No distinct beginning or end, and there's no node which next pointer points to null
![diagram](/assets/img/CircularSinglyLinkedList.png)

4. Circular-doubly-linked list : A doubly linked list, where the last node points back to the first node by the next pointer, and the first node points to the last node by the previous pointer. 
![diagram](/assets/img/CircularDoublyLinkedList.png)

**Time Complexity**
Access : O(n)
Search : O(n)
Insert : O(1)
Remove : O(1)
