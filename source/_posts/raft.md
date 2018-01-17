---
title: Distributed System之Raft
date: 2015-11-05 18:55:43
tags:
  - Distributed System
  - Theory
categories:
  - Distributed System
---

Hi, I am preparing tech interviews these days, and I am reviewing Raft project I did last semester. I gave a short presentation and summary to the interviewers, which was a good way to check if I have a good understanding of the algorithm. And after that, I just want to write a summary by writing this blog.
![Raft](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20151005-5.jpg)
<!--more-->

Before we talk about Raft, we need to talk a little history about Paxos(1980s), a distributed consensus algorithm. The author of Paxos didn't take the average IQ of people into account, when he declares that Paxos makes things simple! Oppps. The complexity of Paxos has strongly restricted the development, until Raft showed up.

Raft was proposed by Diego and John (Stanford), and this algorithm really makes everything understandable. I will say, it is vivid.

There are 2 kinds of servers in this situations: Naming Server and Storage Server. Naming Server is responsible for sending information between servers, but not process information(Storage Servers do). If there is only one naming server and it dies, every one dies. This is called "Single Point Failure". So the naming servers are usually redundant. For there servers, there are consensus issues.

For simplicity, we define three role/status of server: Leader, Follower, Candidate. There is only one valid leader at a time. And the whole process contains two parts: Leader Election and Log Replication.

## Leader Election
Leader will periodically send heartbeat to all followers. If the followers can hear the heartbeat from the leader, then they just stick to the status quo, being followers. Once they can't receive the heartbeat. Then leader election begins. The amount of time a follower waits until becoming a candidate is the election timeout. The period of sending heartbeat is called heartbeat timeout. In our project, election timeout is set to at least 5 to 10 times heartbeat interval to account for variance in leader replication.

We can describe the process in the following way:

![Leader Election1](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20151005-3.jpg)

First, the ambitious follower turns into a candidate, adds its term by 1, and sends vote request. Here we have 3 situations:

A. More than 1/2 servers give the vote, and this candidate wins the leader election.

B. Another server wins the election and becomes the new leader, this candidate will become a follower.

C. No one wins, so they will wait (sleep for a while) to increase their term and re-elect again.

During the leader election, the candidate may receive vote request from other servers. If the other server has a higher term, then this candidate will give its vote and becomes a follower, otherwise it refuses the request. (one case in situtation B/A)

![Leader Election1](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20151005-2.jpg)

If two candidates has the same votes(which means either of them has a majority votes), they will wait to increase their term and re-elect again. (Situation C)

We can use randomize the waiting/Sleeping time to reduce the possibility of the occurrence of Situation C.

We should take care the term. Term changes doesn't mean that leader changes; but leader changes must cause term increases. For me, I think term is much like a Dynasty (in Chinese we say "Nian Hao") for a particular king.

Reviewing Question:

How does follower determine the candidate is worthy to vote? Compare the Term, bigger term wins. What if the follower and the candidate has the same term? Then compare the index, the bigger index wins.

Also, we need a FLAG to remember the server has voted for the election or not. In order to have only one leader, everyone has only one chance to vote in every term.

## Log Replication

What is in a Log entry? Things might be different in various industrious situations. In general, one log entry at least has index, term, and certain instructions.

Leader is responsible for receiving request from the clients. When the leader receives the requests, it will write the log, and sends its log to other servers. When the log is regarded as safely committed, the leader replies the client: blablabla, we did what you need us to do or store.

How to make the log safely committed? We have 2-phase commit.

* First, the leader should send appending request to others, and collects the votes.
* Once the leader collects joint consensus(votes > 1/2), the leader will transfer to new configuration, commit log, and declare to clients and other servers. Other server will append the commit log.


The record of the committing is persistent and consistent. There might be some conflicts between logs(Leader dies and recovers as follower later), so the leader will force the follower to clone the committed log to solve this problem. Therefore, a qualified leader should have all committed logs.

When a new leader is elected, the leader will check the logs in a descending index order, in case of disturbance and loss of necessary logs.

## Other implement details
In our project, we use Java to implement the algorithm, use a thread to represent every independent server, and use RPC to send communication information(request, vote, logs) between threads.

During the learning process, the animation below is very helpful for me to understand the whole process.

<a>http://thesecretlivesofdata.com/raft/</a>
