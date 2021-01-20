# Spark and Hadoop MapReduce

## The Five Key Differences of Apache Spark vs Hadoop MapReduce
1. Apache Spark is potentially 100 times faster than Hadoop MapReduce.
2. Apache Spark utilizes RAM and isn’t tied to Hadoop’s two-stage paradigm.
3. Apache Spark works well for smaller data sets that can all fit into a server's RAM.
4. Hadoop is more cost effective processing massive data sets.
5. Apache Spark is now more popular that Hadoop MapReduce.

For years, Hadoop was the undisputed champion of big data—until Spark came along.

Since its initial release in 2014, Apache Spark has been setting the world of big data on fire. With Spark's convenient APIs and promised speeds up to 100 times faster than Hadoop MapReduce, some analysts believe that Spark has signaled the arrival of a new era in big data.

How can Spark, an open-source data processing framework, crunch all this information so fast? The secret is that Spark runs in-memory on the cluster, and it isn’t tied to Hadoop’s MapReduce two-stage paradigm. This makes repeated access to the same data much faster.

Spark can run as a standalone application or on top of Hadoop YARN, where it can read data directly from HDFS. Dozens of major tech companies such as Yahoo, Intel, Baidu, Yelp, and Zillow are already using Spark as part of their technology stacks.

While Spark seems like it's bound to replace Hadoop MapReduce, you shouldn't count out MapReduce just yet. In this post we’ll compare the two platforms and see if Spark truly comes out on top.

## Table of Contents
* What is Apache Spark?
* What is Hadoop MapReduce?
* The Differences Between Spark and MapReduce
* Performance
* Ease of Use
* Cost
* Compatibility
* Data Processing
* Failure Tolerance
* Security
* Summary

## What is Apache Spark?
In its own words, Apache Spark is "a unified analytics engine for large-scale data processing." Spark is maintained by the non-profit Apache Software Foundation, which has released hundreds of open-source software projects. More than 1200 developers have contributed to Spark since the project's inception.

Originally developed at UC Berkeley's AMPLab, Spark was first released as an open-source project in 2010. Spark uses the Hadoop MapReduce distributed computing framework as its foundation. Spark was intended to improve on several aspects of the MapReduce project, such as performance and ease of use, while preserving many of MapReduce's benefits.

Spark includes a core data processing engine, as well as libraries for SQL, machine learning, and stream processing. With APIs for Java, Scala, Python, and R, Spark enjoys a wide appeal among developers—earning it the reputation of the "Swiss army knife" of big data processing.

## What is Hadoop MapReduce?
Hadoop MapReduce describes itself as "a software framework for easily writing applications which process vast amounts of data (multi-terabyte data-sets) in-parallel on large clusters (thousands of nodes) of commodity hardware in a reliable, fault-tolerant manner."

The MapReduce paradigm consists of two sequential tasks: Map and Reduce (hence the name). Map filters and sorts data while converting it into key-value pairs. Reduce then takes this input and reduces its size by performing some kind of summary operation over the dataset.

MapReduce can drastically speed up big data tasks by breaking down large datasets and processing them in parallel. The MapReduce paradigm was first proposed in 2004 by Google employees Jeff Dean and Sanjay Ghemawat; it was later incorporated into Apache's Hadoop framework for distributed processing.

## The Differences Between Spark and MapReduce
The main differences between Apache Spark and Hadoop MapReduce are:

* Performance
* Ease of use
* Data processing
* Security
However, there are also a few similarities between Spark and MapReduce—not surprising, since Spark uses MapReduce as its foundation. The points of similarity between Spark and MapReduce include:

* Cost
* Compatibility
* Failure tolerance
Below, we'll go into more detail about the differences between Spark and MapReduce (and the similarities) in each section.

## Spark vs MapReduce: Performance
Apache Spark processes data in random access memory (RAM), while Hadoop MapReduce persists data back to the disk after a map or reduce action. In theory, then, Spark should outperform Hadoop MapReduce.

Nonetheless, Spark needs a lot of memory. Much like standard databases, Spark loads a process into memory and keeps it there until further notice for the sake of caching. If you run Spark on Hadoop YARN with other resource-demanding services, or if the data is too big to fit entirely into memory, then Spark could suffer major performance degradations.

MapReduce, on the other hand, kills its processes as soon as a job is done, so it can easily run alongside other services with minor performance differences.

Spark has the upper hand for iterative computations that need to pass over the same data many times. But when it comes to one-pass ETL-like jobs—for example, data transformation or data integration—then that's exactly what MapReduce was designed for.

Bottom line: Spark performs better when all the data fits in memory, especially on dedicated clusters. Hadoop MapReduce is designed for data that doesn’t fit in memory, and can run well alongside other services.
