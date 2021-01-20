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
* a
