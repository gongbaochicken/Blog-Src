---
title: Palindrome LinkedList
date: 2017-03-07 20:46:48
tags:  
	- 刷题总结
categories:
	- 刷题&面试
---
Given a singly linked list, determine if it is a palindrome.
<strong>Follow up: Could you do it in O(n) time and O(1) space?</strong>
<!--more-->

### Solution in C++:
The basic idea to solve this problem is to break this solution into 3 steps:

* Step 1: Find the middle point of the whole linked list.
* Step 2: Reverse the second part of the linked list and do it.
* Step 3: Move and compare the pointer from head and the pointer from the middle until end.

The fans of Linus may start shouting: "Talk is cheap, show me the code!" Well, the code is simple:

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
    bool isPalindrome(ListNode* head) {
        if(head == NULL || head->next == NULL) return true;

        ListNode *fast = head->next, *slow = head;
        while(fast != NULL && fast->next != NULL ){
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode *laterList = reverseList(slow->next);
        slow->next = NULL;
        while(laterList && head){
            if(laterList->val != head->val){
                return false;
            }else{
                laterList = laterList->next;
                head = head->next;
            }
        }
        return true;
    }

    ListNode* reverseList(ListNode* head) {
        if(head == NULL || head->next == NULL){
            return head;
        }
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        while(head != NULL && head->next != NULL){
            ListNode* next = head->next->next;
            head->next->next = dummy->next;
            dummy->next = head->next;
            head->next = next;
        }
        ListNode* key = dummy->next;
        delete dummy;
        return key;
    }
};
```

If you use
```c++
ListNode *fast = head, *slow = head;
```
Then the
```c++
ListNode *laterList = reverseList(slow->next);
```

should be modified as
```c++
ListNode *laterList = reverseList(slow);
```
