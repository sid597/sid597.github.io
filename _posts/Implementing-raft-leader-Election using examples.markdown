---
layout: post
category: distributed systems
title: Implementing raft Leader Election using example 
data: 2021-01-01
---


There is a cluster with odd number of servers, one of them is a leader
for some term and others are its followers. There is a timer attached to
every follower and if one them times out then, that follower conducts 
an election and if successful becomes leader for that term.

<i> I assume the leader has read the paper atleast once, and played with the 
raft visualisation on raft homwpage.

First time I read the paper I thought I am following what the author is saying, 
things are making sense but things only make sense while reading the paper. Once I got down to implementing my mind was blank, I could not figure out how to start implementing, how to ask for votes and decide majority of votes is granted, when to reset the election timer for some follower, how to transition between states, what data different states keep.  I implemeted leader election first time and after writing a couple hundred lines of code I myself no longer what was happening and had to delete the whole code and start from scratch.

Implementation also needs to handle RPC request which might come from previous term Leader and if decisions are taken based on this old request then it can cause problems. 

Another issue I faced was what to make concurrent and what not to for example do I keep track of number of votes received in a thread or not. 

Then there is debugging when for some case the library is producing wrong result, like not selecting any leader for some term, timeouts not happening when expected. If there is some error in implementation or worse forgot some detail to implement from paper it becomes hard to debug, somethimes it happens with buggy implementation that while testing code will return correct result most of the time but for a few times it will not. And these types of bugs are dangerous if proper testing is not done. 

These were all the issues implementing whats given in the paper, I am not talking about all the concurrency related bugs like races, deadlocks which arise when there are many threads running. 

<b> How to make the implementation easier ?</b>

When writing concurrent code I think it is best to reason deep and hard about what is it we are trying to do insted of depending on the tests in concurrent code because there might be cases which might occur and one did not think about. There should be proof of correctness it would be a disaster to assume code is doing the right thing when it is not and debugging it using tests might not even expose it until some condiditon hits which was not hit during testing.

On hindsight I think the best way to go about implementing would be first wirting down all the type of corner cases one can think of and then write down what you think the library would do and arrive at the correct outcome. This is what I will be doing in this article, first will post a situation and then write in detail what I think the library would do. Writing log is the best option to detect bugs so they should be detailed
</i>


3 Servers : A, B, C


Tell some scenerios that can occur and then I will write what would happen.

1. All servers are started for the first time, assume no RPC will be lost and would be replied withing a reasonable time also no server will crash ?

 - All servers (A, B, C) will set their state as <i>Follower</i>, initialize data needed by follower, assign themselves some timeout between a range, create a thread that will check if they have timed out. 

<i> Now let's say the timeout for A, B, C are 202ms, 293ms, 282ms respectively, so server <i>A</i> will timeout</i>


 <b>Steps server <i>A</i> will take :</b>
 <i> Remember a thread checking for timeout is running in background and <i>A</i> can also receive a RequestVote RPC from some other server</i>

 - <i>A</i> will start a new election, it will increase its current term, set its current state to <i>Candidate</i>, vote for itself for this term and reset its timeout.

- Will initialize a variable to keep track of how many votes are granted

 - Will send RequestVote RPC which will ask for vote from other servers, fields in the RPC will include the server Id and term for which it is asking vote, reply to this RPC will contain a bool that tells if vote was granted or not and term of the server from whom vote was requested.

 - If vote is granted by some server then variable keeping track of votes is increased and checked if the total votes received is greater than or equal to majority of servers. And if total votes >= majority of servers then we make this server <i> Leader </i>.
  
  - After becoming <i> Leader </i> resets the election timer and sends Heartbeat to all the other servers. This will tell other servers that a new leader is elected.
  - One of Leader's job is to send Heartbeat to its followers after some fixed time, so the Leader will check when was the last Heartbeat sent  and if it is greater than let's say 50ms then a new Heartbeat is sent to followers.

 - If vote is not granted then server sets it current Term to the term received in reply and becomes <i>Follower </i>


 <b>Steps server <i>B</i> and <i>C</i>will take :</b>

<i>Let's assume both these servers replied to RequestVote RPC send by A before they timed out</i>

- They will receive RequestVote RPC from A
  - First will check if the term of candidate asking for vote is greater than, less than or equal to the current term of self.
  - If the candidate;s term is lesser than self's term then will reply false.
  - Else will reply true, reset current election timer.

What while in Candidate state a server receives RequestVote RPC from some other server ?
