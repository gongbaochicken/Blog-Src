title: Delete Node in a Linked List
date: 2015-07-24 00:27:46
tags:
  - 刷题总结
categories:
  - 刷题&面试
---
<strong>Question:</strong>

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.
<!--more-->

### Solution:
The tricky thing is that you can't trace back in a singly linkded list which lacks "prev" link to node of the previous node. Therefore, we hava to come up with a backup solution to achieve the same result without using "previous" links.

![](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150721-1.jpg)

The basic idea is that the current node can copy its next node's value, then delete or jump over the next node. At this time, the returning linked list is the same as we want. The solutions in Java and C++ are pretty similar.

### Code in C++

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        ListNode* next = node->next;
        node->val = next->val;
        node->next = next->next;
        delete next;
    }
};
```


### Code in Java
Java solution is very simple, because you don't need to free the node pointer.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```
