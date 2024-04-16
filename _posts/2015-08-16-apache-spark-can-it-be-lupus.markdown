---
layout: single
title: "Apache Spark - Can it be lupus?"
date: 2015-08-16 00:50:31 -0700
comments: false
tags:
  - engineering
  - big data
---

<p>
Hmm...so you're trying to diagnose problems in your Spark Job and can't seem to make progress? I've been there.
Working with Spark is similar to working with a Machine Learning algorithm with respect to the number of (hyper) parameters.
Tuning them properly can be really hard initially. But, if you understand the semantics behind each of them, use it properly, you'll see it's true potential. I can't stress on how much of an improvement it is over the standard Map Reduce paradigm.</p>

<br />
I've played with Spark a lot over the last month. Here are some of the things I've learned that may help you cure your ailing Spark Job.
<br />
<!--more-->

Start by reading these:

- <a href="http://spark.apache.org/docs/latest/tuning.html" target="_blank">Tuning Spark</a> (You may want to change to the Spark Version currently deployed)
- <a href="http://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-1/" target="_blank">Tune your Spark Jobs Part 1</a>
- <a href="http://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-2/" target="_blank">Tune your Spark Jobs Part 2</a>

<br />
<hr />
<h3>Not all Spark programs are created equal</h3>

Spark has a lot of parameters that you can play with. Each can have quite an impact on the running time of your spark job. The four parameters that are probably the most critical are, <code> --num-executors, --driver-memory, --executor-memory and --executor-cores</code>. Just increasing the num-executors does not mean your job will run faster. When you persist/cache any RDD, that is shared by all tasks running in an executor. So in most cases, increasing the parallelism (executor-cores) will have more effect as they can access the same cache. However, make sure you change executor-memory accordingly. The executor memory is shared among all the tasks. So if you have 16g and you can run 3 tasks in parallel, then each task will get around 5G. If you notice that only a fraction of your RDD is getting cached, you need to increase the amount of partitions it can access. Increasing the number of executors and reducing the executor-cores might help with this.

<br />
<hr />
<h3> Things to be aware of when you use variables.</h3>
If you're passing values via command line arguments to your Spark jobs, you need to be extra careful. The values that you pass are only accessible to the driver class. If you're using Flags with default values, then the executors will receive the default values and not the ones you passed! For mandatory flags, it will just pass null. To avoid such cases, you can broadcast these flags and access them using the broadcast variables.
<br />
<hr />
<h3>Kryo. Always.</h3>
<p>
Always always use Kryo. You can check the difference between Kryo and Java Serialization <a href="https://github.com/eishay/jvm-serializers/wiki" target="_blank">here</a>. When caching, it will use less far less space and will still be much faster. Seriously, don't use java serialization. </p>
<br />
<hr />
<h3>StackOverflow problems? Cut the DAG.</h3>
<p>If you have long DAG of RDD objects, it needs to be serialized as part of the task creation. Serializing this leads to stack overflow error. It's generally common with algorithms where you perform iterations on the same RDD (eg. ALS). To fix these errors, you need to enable check-pointing (sparkContext.setCheckpointDir) and do a rdd.count() to cut the DAG.
</p>
<br />
<hr />
<h3>Java heap space/OOM issues. </h3>
<p>
There are many variations of these errors. Typical ones are "Out of Java heap Space", "OOM errors". But, there are ones that are more subtle like "Failed without being ACK'd", "Futures timed out after [n seconds]".  These generally happen when the threads are busy with GC and couldn't send the heartbeat message to the driver.  A good way to debug these are by going through the GC logs. <br />
<pre> spark.executor.extraJavaOptions=-verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamps </pre>
<br />
You can tweak your configuration based on the behavior seen by the gc logs. If there are lot of Full GC cycles happening, then you tuning config parameters like "spark.storage.memoryFraction" may help. For the timeout errors, changing the following defaults: <pre>spark.akka.frameSize=120</pre><pre>spark.rpc.askTimeout=120</pre></p>
<br />
<hr />
<h3>Everyday I'm shuffling.</h3>
When doing operations like joins, intersection, cartesian, etc. consider partitioning your RDD using a Hash Partitioner. In particular, if you're joining a large RDD with a relatively smaller one, consider partitioning the larger RDD at the outset. This will ensure that only the elements of the smaller RDD are shuffled. </p>
<br />
<hr />
<h3>Broadcast it! </h3>
<p>Broadcasting data can make your jobs really really fast. It obviously helps knowing what data size you're dealing with. Assume you're taking a cartesian between two RDDS, one of size 300k, other 100k. The resultant RDD would be 90 x 10<sup>9</sup>. That's 90 billion pairs. The network shuffle required here will most likely kill your Spark job. However, if you broadcast the 100K RDD, creating the cartesian takes less than 5 minutes as it involves a simple flatMap operation over each element in it's own partition. </p>
<br />
<hr />
<h3>Prefer Integers over Strings</h3>
<p>Prefer using Integers over Strings and large objects since it affects serialization/shuffle times. If you need to use a large object simply as a key, or in cases where your algorithm demands integers, we can use zipWithIndex to create a object -> integer (with the reverse mapping) RDD and cache it. If it's small enough, just create a BiMap and broadcast it. </p>
<br />
<hr />
<h3>Be Careful with Enums!</h3>
When you use transformations like reduceByKey,countByKey,etc. the internal implementation of Spark uses hashCode to do the calculations. Now,
Java Enums do not have consistent hash code across different VMs. Now, since the enum keys will be distributed across different executors, even same keys may have different hash keys! This will lead to unexpected results. If you collect this result, the keys will be sent to the driver, and the now the keys which had different hash codes will get a consistent hash and will override each other. A simple way to avoid this is using the name/toString method for the keys.
<p>
Spark is really really powerful. Use it properly, and you can solve most of your problems.
Remember, it's never lupus!
</p>
<img src="/assets/images/its-not-lupus.jpg" >
