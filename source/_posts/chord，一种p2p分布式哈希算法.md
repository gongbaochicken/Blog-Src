---
title: Distributed System之Chord，一种P2P分布式哈希算法
date: 2016-01-22 00:27:46
tags:
  - Distributed System
  - Theory
categories:
  - Distributed System
---

这里的Chord不是音乐里面的和弦，而是一个解决P2P分布式查找协议，或者说算法，由MIT的实验室设计的。其项目地址为：
><strong>http://pdos.csail.mit.edu/chord/ </strong>

在我上学期的课程：研究生分布式系统中，第二个作业就是要求根据Paper，使用C++或者Java实现Chord。在此跟大家一起分享一下。
<!--more-->
## Paper

根据Chord的原理，只要把每个数据项与一个key关联起来，并把这一对key/data项存储在该key被映射到的节点上，P2P网络中的数据定位问题就容易得到解决。在Chord中，可以很好地实现节点加入系统和离开系统，在系统持续变化的时候仍然可以应答查询。理论分析、仿真和实验结果表明Chord是可扩展的（scalable），每个节点的通信开销与维护的状态与Chord节点数目成对数关系。

Chord协议给出了一个有效的原型：给定一个key，它决定存储这个key的value的节点，并且运行很有效。在一个N个节点的稳定状态的网络中，每个节点仅维护到大约个其他节点的路由信息。由于节点离开和加入引起的对路由信息的刷新仅需要O（log2N）个消息。

Chord主要特点总结如下：<br>

*  **负载均衡**：Chord以分布式哈希函数的行为在每个节点上平均散布keys，这可以提供一定程度的自然负载平衡。

*  **分散化**：Chord是一个完全分布式的。没有节点比其他的节点更重要。这提高了健壮性，也使Chord 更适于松散组织性的peer-to-peer的应用。

* **可扩展性**：Chord查询的花费是随着日志中的节点数目而增长的，所以大的系统也是行得通的。实现这一比例不需要调整参数。

* **有效性**：Chord自动地调整它的内在的表，去反映新加入的节点和错误的节点，确保禁止在基础网络上的主要错误，节点负责一个key能够被找到。

## 实现
The pseudocode to find the successor node of an id is given below:

```
 // ask node n to find the successor of id
 n.find_successor(id)
   //Yes, that should be a closing square
   bracket to match the opening parenthesis.

 //It is a half closed interval.
   if (id in (n, successor] )
     return successor;
   else
     // forward the query around the circle
     n0 = closest_preceding_node(id);
     return n0.find_successor(id);

 // search the local table for the highest predecessor of id
 n.closest_preceding_node(id)
   for i = m downto 1
     if (finger[i]\in(n,id))
       return finger[i];
   return n;
```


The pseudocode to stabilize the chord ring/circle after node joins and departures is as follows:

```
// create a new Chord ring.
 n.create()
   predecessor = nil;
   successor = n;

 // join a Chord ring containing node n'.
 n.join(n')
   predecessor = nil;
   successor = n'.find_successor(n);

 // called periodically. n asks the successor
 // about its predecessor, verifies if n's immediate
 // successor is consistent, and tells the successor about n
 n.stabilize()
   x = successor.predecessor;
   if (x\in(n, successor))
     successor = x;
   successor.notify(n);

 // n' thinks it might be our predecessor.
 n.notify(n')
   if (predecessor is nil or n'\in(predecessor, n))
     predecessor = n';

 // called periodically. refreshes finger table entries.
 // next stores the index of the finger to fix
 n.fix_fingers()
   next = next + 1;
   if (next > m)
     next = 1;
   finger[next] = find_successor(n+2^{next-1});

 // called periodically. checks whether predecessor has failed.
 n.check_predecessor()
   if (predecessor has failed)
     predecessor = nil;
```

## Rerenfence:
>[1] Ion Stoica, Robert Morris, David Karger, M. Frans Kaashoek, and Hari Balakrishnan. Chord: A scalable peer-to-peer lookup service for internet applications. In Proc. ACM SIGCOMM 2001, August 2001. An early version appeared as LCS TR-819 available at <br>
> <strong>http : //www.pdos.lcs.mit.edu/chord/papers.</strong>
