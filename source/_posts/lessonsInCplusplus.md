---
title: Lessons I learned in C++ job
date: 2017-03-08 23:08:34
tags:
  - C++
  - SAP HANA
categories:
  - C++
---
As a C++ software developer, I work with our team to deal with a very large scale database project, SAP HANA. HANA is a column-based, in-memory computing database, which is the dominant super star product of SAP. We serve hundreds of companies around the world.

The following tips for C++ codes is the notes that I learned from my current work as an entry-level C++ engineer.
<!--more-->
1. Use parameters with default value if we can, instead of adding a new overload function, which cause verbose.
2. When you meet the compile errors "discards qualifiers", it's talking about const or volatile.
3. If the order does not matter, and basically we only need it for pure lookup-retrieval, use unordered_map to replace map. Since the the unordered map has O(1) lookup performance due to its implementation of hash function. However, when the map is frequently insert/remove, I think that will depends.
4. Be careful about double/triple for loops in the industry code, these codes can be optimized in complexity in some cases.
5. Keyword static means that this method can be called on the class itself, no instance of that class necessary.
7. It is is crucial to know which kind of lock is needed, when there is any necessity to add lock. Ask yourself, is what you need a read lock or write lock? a shared lock or an exclusive lock? This is not only functionality concern, but also some performance concern.
8. Use const iterator for representing end().
9. Consider of using friend class if you want to call the protected methods of certain class.
10. Use inline function in header, if the implementation is really simple.
11. Have a good habit to define types.
```c++
typedef ltt::pair<const char *, const char *> SuggestionListComponentNamePair;
```
12. Use reference to avoid unnecessary copy.
13. Use size_t when return size(), dont' use int.
14. Don't use Linked List, use vector instead. "Vector is ALWAYS better than list.", said by Bjarne Stroustrup. I think the main difference is the find insert point(vector uses binary search, linked list uses linear search) and find delete point.

(--continue)
