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

<b>Job of master : </b>
- Keeps track of state of worker : idle, in-progress or completed
- Identity of all workers 
- Stores the location of intermediate files along with their sizes.

<b> Fault Tolerance : Worker Failure </b>

- Master pings the workers periodically, if no response is recived from worker in a certain amount of time that worker is marked as failed. <span style="color:green"> Any completed map task or any map or reduce task in progress are reset back to their initial <i> idle </i> state.</span>

- Completed <span style="color:green"> map tasks are re-executed </span>because their output is stored on the local disk of failed machine and is therefore inaccessible. Completed reduce tasks need not be recomputed because they are stored in GFS and that makes it available.

- When a map task is executed first by worker A and
then later executed by worker B (because A failed), all
workers executing reduce tasks are notified of the reexecution. Any reduce task that has not already read the
data from worker A will read the data from worker B.


<b> Fault Tolerance : Master Failure </b>


It is easy to make the master write periodic checkpoints
of the master data structures described above. If the master task dies, a new copy can be started from the last
checkpointed state. However, given that there is only a
single master, its <span style="color:green">failure is unlikely; therefore our current implementation aborts the MapReduce computation
if the master fails. </span>


### <b> Implementation as a project in 6.824 </b>

Now the above implementation is a production implementation but we want to implement it a learning project without losing the spirit and fun of implementing this system, 

<b>So how do we do it ? </b>

For implementing MapReduce as a a project we will use [<u>MIT 6.824 course Lab 1</u>][Lab1]. But <span style="color:green">why </span>not do it from scratch one may ask, the reason to do so is that [Prof. Robert Morris ] [Prof] provides us with <span style="color:green">tests, a middleware </span>with which we can run this distributed system on our local machines, a <span style="color:green">broad outline code </span>whose details we need to fill in. Tests are a core part of any software and more so in distributed systems, if we start from scratch then we first have to write our tests that cover all the corner cases for this system. And to implement tests one needs to have knowledge about  the tools (programming language, test architecture, RPC) and enough practical operating systems knowledge to implement the distributed system on one machine (I think we can also use containers to achieve the same on single machine). So if the goal is to implement distributed systems then we should use this Lab as a given and build upon it. 

<i>Before starting the lab I was worried that there would be much spoon feeding that it would no longer be fun to implement, I would not learn about the difficulties in implementating distributed systems and would not get experience in writing medium sized software. But all this worry was out of water when I saw the code for lab, because only the broad outline is provided and all the details and software designs are in the hands of implementer, you can see this in the starting code I posted below. </i>


<b>What will the differences be from the implementation described in paper ?</b>

 We don't have GFS and not enough money to buy real machines so the difference due to these designs are :
 -  All the data is read from and written to local disk.

 -  Master and Workers are just processes, I think we can also use containers to get the same result.

 - Not using heartbeat mechanism, will use 10 seconds timeout (for this lab) and if no response is received from a worker in progress we reassign.

    <i>It is easy to implement heartbeat from our side, we will do it in raft  implementation.</i>
- No concurrent job scheduling i.e cannot submit multiple mapReduce jobs and then the MapReduce library schedules these jobs to a set of available machines within the cluster.

- Master selection process and how master picks idle workers is not detailed in paper. So in labs who master is, is decided by middleware and workers call master for a job and based on if all map tasks are finished or not master assigns map task or reduce task to the asking worker. 

- The naming convention for machines might be different i.e how master knows which machines to communicate and how worker knows who the master is. In lab Master and worker implementation is in same package so workers call master object for job and then master assigns some id to these workers which is remembered by both the master and the worker.

- Using a garbage collected language GO insted of of systems language like rust, C or C++ for performance.
   

### <b>Practical Implementation </b>

Here's what the outline code looks like :


- <b>Communication between Master and Worker:</b>

    First let's figure out how to communicate between master and workers. We do this by using the RPC Package provided by GO there are other RPC frameworks like grpc which is useful if different parts of your system are written in different languages. But here it is much easier to use GO's RPC package.

    <b>So how does it work ?  </b>

    You can read my other article where I (just out of curosity) tried to understand and wrote about how are RPC's implemented. But we don't necessary need to know how is it implemented we are the consumer's of this tech so we care about how to use it.

    <b>How to use GO's RPC package ?</b>


    Refer to [RPC package documentation][RPC] it defines exactly how to use RPC in GO and it has everything you will need. In summary here's what you need  :

    On Server side (side on which we call a function):

    - Create an object with method satisfying the following criterion :
        - Need exported methods(method starting with capital letter) of an object with exported type.
        - Method needs to have 2 exported arguments where second's argument type is pointer. First argument contains the arguments provided by the caller and the second argument represents reply parameters to the caller.
        - Method has a return type of error
        <br>
        <br>

        ```
        func (t *T) MethodName(argType T1, replyType *T2) error 
        ```

        - The method's return value, if non-nil, is passed back as a string that the client sees as if created by errors.New. If an error is returned, the reply parameter will not be sent back to the client.
    - Register the object to rpc package hence making this object visible as a service.
    - Start a thread that listens for calls on some port to this object.

    <br>
    On Caller side (side from which we call RPC on an object from other side):
    - We need to do 2 tasks : Establish a connection to server side and invoke a [NewClient][newclient] (NewClient returns a new Client to handle requests to the set of services at the other end of connection) 
    The convenience function Dial (DialHTTP) performs both steps for a raw network connection (an HTTP connection).

        ```
        client, err := rpc.DialHTTP("tcp", serverAddress + ":1234")
        ```

    - The resulting Client object has two methods, Call and Go, that specify the service and method to call, a pointer containing the arguments, and a pointer to receive the result parameters.

        ```
        args := &server.Args{7,8}
        var reply int
        err = client.Call("Arith.Multiply", args, &reply)
        ```

    I was not very comfortable with RPCs after reading the documentation once so I learned it by playing around with the RPC package to get a better understanding how to do and what to do.
    
- <b>Implementing Master </b>

    - Assign new task to worker based on if all the map tasks are done or not.

      For map task master gives the filename and mapFunction, mapWorker opens the file from localDisk and read. The intermediate file names by mapWorker has format ```mr-inter-X.tmp``` where ```X``` represents the hash for some key.
      
       For reduce task master gives a number and reduceFunction, number represents which files reduce has to read from. 

    - Reassign tasks that are taking more than 10s. What this means from the implementation point is that based on the task, add the task back to stack which keeps track of pending tasks so it can be reassigned when some worker asks for another job.

    - For all the completed tasks make their corresponding output file permanent. Workers write to files and name them .tmp then returns these
    file names back to master, master checks if the worker finished within time
    if so it make the files permanent.

    
    
 
- <b>Implementing Worker </b>
    - Ask master for a new job, if already in midst of a job wait for 1 second for current task to finish and try again.
    - If map job is assigned then,
      - create all the intermediary files
      - pass the filename and contents of that file to mapFunction which will return a list of key value pairs. 
      - for each key value pair, pass the key through a hash function and based on the value received write this key value pair in JSON format. 

    - If reduce job is assigned then
      - from all the corresponding files read their content(you would first have to decode the JSON) in a list, 
      - sort it (if the list too big to fit then we can use external sort but this is not the case with this lab),
      - pass this sorted list to reduce function and write the returned result to an output file. 

    - After a job is finished worker tells the master it finished the job.

    <br>
- <b> Concurrency Issues </b>
    Beware of <span style="color:green">deadlocks and race conditions</span> you are going to use multiple threads/goroutines. If you find yourself in one of these situation, general suggestion is to use logs with good format (use unique id or something to see which worker/master is logging) to narrow down the issue.
    
    In case of deadlocks see where all are you using locks and see if the locks are released before returning or sending RPC. Deadlocks can also arise there are 2 threads and each one is wating for lock acquired by the other thread while holding locks.

    In case of race condition use <span style="color:green">GO's race detector </span>although this <span style="color:green">does not gurantee </span>race free code. Race in code can be detected if that code is executed. So if there is some code with race condition but is unreacheable by compiler during execution that will not be detected. There is race in code if same resource is accessed from multiple threads/routines, the solution is to use locks. You can also look into other GO concurrency primitives like channels, condition variables, wait groups etc. but I don't think they will be needed in this implementation. You can use them when implementing Raft. 


<br>

<b> My Experience </b>

<i>I will leave the code implementation to reader, there is not much to say about how to go about implementing master or worker. This lab is not hard to implement or debug if you know the behaviour of master and worker, the thing that made this good programming exercise for me was not knowing about most of the things before starting this lab and learning them all while implementing. For example: I did not have experience with concurrent programing language or concept (also did not know the difference between concurrent, parallel programming), all the concepts related to distributed systems. Also while reading the paper things seems like yeah I understand, its obvious then watching the lectures makes this feeling stronger but when I sat down implementing I found some gaps. I think that is due to my lack of experience in implementing after reading just docs. So all in all for me this was an excellent learning experience, I did not write good code (I am seeing it now and it does not look idiomatic) there were some workarounds which should not be there. I will not amend them but keep them as a token of how far I have come (although not that far) :) </i>
<br>



 
[6.824]: https://pdos.csail.mit.edu/6.824/labs/lab-mr.html
[Lab1]: https://pdos.csail.mit.edu/6.824/labs/lab-mr.html
[Prof]: https://en.wikipedia.org/wiki/Robert_Tappan_Morris
[MRP]: https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf
[RPC]: https://golang.org/pkg/net/rpc/
[newclient]: https://golang.org/pkg/net/rpc/#NewClient