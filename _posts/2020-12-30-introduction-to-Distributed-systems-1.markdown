---
layout: post
category: distributed systems
title: Introduction to Distributed Systems 1
data: 2020-12-30
---

Why are distributed systems hard ? 
1. Concurrency: Lots of parts and all the parts execute concurrently. The interaction between them needs to be precise and Correct, then there are some time dependent stuff.
2. Partial Failures: Apart from the multiple pieces we have network and combination of all these causes some unusual failure patterns. 
Part of system can fail and have to deal with that and make the best out of it.
3. Performance: If I buy X no of machines how will I get X times the performance. And getting the worth out these machines careful design and implementation is required.

types of failures
Partitions
SPlit brain
CAP theorem
How to increase Consistency
How to increase Availability
Implementing Distributed systems
Logs


<b>From [MIT 6.824 Course][6.824]</b> 

Distributed Systems is about infrastructure upon which applications are made.

Infrastructure

* Storage: A well defined and useful abstraction
* Communication : As a tool 
* Computation: Map reduce

Dream is to build an interface that hides the distributed nature of the systems want the user to think they are talking to a single system

but are really extramly high performant fault tolerant distributed systems 

Topics that show up when talking about the above things

*Implementation tools :
RPC: Abstractions that hides the dact that we are communication over a unreliavle networj
Threads: Structuring concurrent tasks
Locks: cOncurrency control

* High level goal of distributes systems
2X computers => 2X throughput

 *Fault tolerance 
 with 1000 computers with 1yr life then after some time we have 3 computers broken evry day
 Need some way to mask this behaviour because there are alwaus failures

 What it means to be dault tolerance ?
 availability : Continue operation on face of failures
 Recoverability : Stops when hits failure

NV Storage 
replication 

*Consistency
why an issue ? Because there are more than one replucated data floeting around may get stale data
KV server 
 Strong and weak consistency
 uncorelated falures
 


[6.824]: https://www.youtube.com/watch?v=cQP8WApzIQQ&list=PLrw6a1wE39_tb2fErI4-WkMbsvGQk9_UB