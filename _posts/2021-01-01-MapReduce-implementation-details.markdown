---
layout: post
category: distributed systems
title: MapReduce Implementation  Details 
data: 2021-01-01
---
There are 2 systems working together in MapReduce programming model :
1. MapReduce : acts as a library which users use for their large computations.
2. Google file system (GFS) : Splits the input file into 16-64 MB blocks and stores several copies (typically 3) of each block on different machines. Reduce workers stores the output on GFS

<br>
<br>

### <b> Implementation as in paper (production system) </b>

<img src="/assets/img/mapReduceMe.jpg">

[MapReduce Paper] [MRP] describes the implementation as follows (along with some details I filled in) :
1. The MapReduce library in the user program first
splits the input files into M pieces of typically 16
megabytes to 64 megabytes (MB) per piece (controllable by the user via an optional parameter). It then starts up many copies of the program on a cluster of machines.

    <i>The splitting of file in blocks is done by GFS  </i>
2. One of the copies of the program is special – <span style="color:green">the
master</span>. The rest are <span style="color:green">workers </span>that are assigned work
by the master. There are M map tasks and R reduce
tasks to assign. The master picks idle workers and
assigns each one a map task or a reduce task.

    <i>Ideally, M
and R should be much larger than the number of worker
machines. Having each worker perform many different
tasks improves dynamic load balancing, and also speeds
up recovery when a worker fails: the many map tasks
it has completed can be spread out across all the other
worker machines. Google often perform MapReduce computations with M = 200, 000 and
R = 5, 000, using 2,000 worker machines.</i>
3. A worker who is assigned a map task reads the
contents of the corresponding input split. It parses
key/value pairs out of the input data and passes each
pair to the user-defined Map function. The intermediate key/value pairs produced by the Map function
are buffered in memory.

    <i> Master knows about the location of input file blocks, and taking this knowledge into account tries to assign a map task on a machine that contains a replica of the corresponding input data. Failing that, it attempts to schedule
a map task near a replica of that task’s input data (e.g., on
a worker machine that is on the same network switch as
the machine containing the data). </i>
4. Periodically, the buffered pairs are written to <span style="color:green">local
disk</span>, partitioned into R regions by the partitioning
function. The locations of these buffered pairs on
the local disk are passed back to the master, who
is responsible for forwarding these locations to the
reduce workers.

    <i>Reduce invocations are distributed by partitioning the intermediate key
space into R pieces using a partitioning function (e.g.,
hash(key) mod R). The number of partitions (R) and
the partitioning function are specified by the user. </i>
5. When a reduce worker is notified by the master
about these locations, it uses remote procedure calls
to read the buffered data from the local disks of the
map workers. When a reduce worker has read all intermediate data, it <span style="color:green">sorts </span>it by the intermediate keys
so that all occurrences of the same key are grouped
together. The sorting is needed because typically
many different keys map to the same reduce task. If
the amount of intermediate data is too large to fit in
memory, an external sort is used.
6. The reduce worker iterates over the sorted intermediate data and for each unique intermediate key encountered, it passes the key and the corresponding
set of intermediate values to the user’s Reduce function. The output of the Reduce function is appended
to a final output file for this reduce partition.

7. When all map tasks and reduce tasks have been
completed, the master wakes up the user program.
At this point, the MapReduce call in the user program returns back to the user code.

    <i> After successful completion, the output of the mapreduce execution is available in the R output files (one per
reduce task, with file names as specified by the user).<span style="color:green">
Typically, users do not need to combine these R output
files into one file </span>– they often pass these files as input to
another MapReduce call, or use them from another distributed application that is able to deal with input that is
partitioned into multiple files.</i>

Job of master :
- keeps track of where data is stored and provides the workerts location of this datworkerts location of this data 
- Keeps track of what task is done, who is assigned what task


### <b> Implementation as a project in 6.824 </b>

Now the above implementation is a production implementation but we want to implement it a learning project without losing the spirit and fun of implementing this system, 

<b>So how do we do it ? </b>

For implementing MapReduce as a a project we will use [MIT 6.824 course Lab 1][Lab1]. But why not do it from scratch one may ask, the reason to do so is that [Prof. Robert Morris ] [Prof] provides us with tests, a middleware with which we can run this distributed system on our local machines, a broad outline code whose details we need to fill in. Tests are a core part of any software and more so in distributed systems, if we start from scratch then we first have to write our tests that cover all the corner cases for this system. And to implement tests one needs to have knowledge about  the tools (programming language, test architecture, RPC) and enough practical operating systems knowledge to implement the distributed system on one machine (I think we can also use containers to achieve the same on single machine). So if the goal is to implement distributed systems then we should use this Lab as a given and build upon it. 

<i>Before starting the lab I was worried that there would be much spoon feeding that it would no longer be fun to implement, I would not learn about the difficulties in implementating distributed systems and would not get experience in writing medium sized software. But all this worry was out of water when I saw the code for lab, because only the broad outline is provided and all the details and software designs are in the hands of implementor, you can see this in the starting code I posted below. </i>


<b>What would all the differences be from the implementation described in paper ?</b    >
1. We don't have GFS and not enough money to buy and test on real machines so the major difference is that all the code is going to be implemented in local machine 



[6.824]: https://pdos.csail.mit.edu/6.824/labs/lab-mr.html
[Lab1]: https://pdos.csail.mit.edu/6.824/labs/lab-mr.html

[MRP]: https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf