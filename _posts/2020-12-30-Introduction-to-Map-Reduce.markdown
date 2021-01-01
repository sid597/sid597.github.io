---
layout: post
category: distributed systems
title: Introduction to MapReduce
data: 2021-01-01
---

MapReduce is a programming model and an associated implementation for processing and generating large data sets. Users specify a map function that processes a key/value pair to generate a set of intermediate key/value pairs, and a reduce function that merges all intermediate values associated with the same intermediate key. Many real world tasks are expressible in this model a typical MapReduce computation processes many terabytes of data on thousands of machines.

The authors and many others at Google have implemented hundreds of special-purpose
computations that process large amounts of raw data, such as crawled documents, web request logs, etc., to compute various kinds of derived data,Most such computations are conceptually straightforward. <span style="color:green">However, the input data is usually
large and the computations have to be distributed across
hundreds or thousands of machines in order to finish in
a reasonable amount of time.</span> The issues of how to parallelize the computation, distribute the data, and handle failures conspire to obscure the original simple computation with large amounts of complex code to deal with
these issues.

As a reaction to this complexity, we designed a new
abstraction that allows us to express the simple computations we were trying to perform but hides the messy details of parallelization, fault-tolerance, data distribution and load balancing in a library.

<b> Programming Model</b> 

The computation takes a set of input key/value pairs, and
produces a set of output key/value pairs. The user of
the MapReduce library expresses the computation as two
functions: Map and Reduce.

Map, written by the user, takes an input pair and produces a set of intermediate key/value pairs. The MapReduce library groups together all intermediate values associated with the same intermediate key <i>I</i> and passes them
to the Reduce function.

The Reduce function, also written by the user, accepts
an intermediate key  <i>I</i> and a set of values for that key. It
merges together these values to form a possibly smaller
set of values. Typically just zero or one output value is
produced per Reduce invocation. The intermediate values are supplied to the user’s reduce function via an iterator. This allows us to handle lists of values that are too
large to fit in memory.

<img src="{{site.baseurl}}/assets/img/mapReduce.png">

<i> All of the above is from MapReduce paper for further reading, consult the [MapReduce Paper] [MRP]. </i>

Continue to [MapReduce Implementation details]({%link _posts/2021-01-01-MapReduce-implementation-details.markdown %})



[MRP]: https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf

