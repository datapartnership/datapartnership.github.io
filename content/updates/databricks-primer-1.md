---
date: 2020-11-03T00:00:00Z
title: "A light primer to Spark / Databricks"
type: ""
url: "light-primer-to-spark-databricks-1"
---


Databricks is a powerful platform for using Spark, a powerful data technology. 

If you're reading this, you're likely a Python or R developer who begins their Spark journey to process large datasets. 

In this post, I try to provide a very general overview of the things that confused me when using these tools.  I hope to help you avoid costly mistakes, saving you some time and some money.

If I missed something or got something wrong, I would love a [tweet from you](https://twitter.com/mrmaksimize) letting me know. I would much prefer that over you cursing my name under your breath for the next several months.   

![](https://i.imgur.com/fOfA1je.png)


## TLDR:
* Spark is a framework that enables large data to be processed by parallelizing workloads across multiple machines, or clusters.
* Databricks is a service that makes it easy to work with Spark (like Github for Git)
* There are two types of commands in Spark: **Transformations** and **Actions**; **Transformations** do not run on the entire dataset, but are queued up and executed when you run an **action**.
* If you use Pandas in Spark, you lose all the power of Spark parallelization.
* Use Spark Dataframes, not Pandas dataframes or RDDs.
* The name of the game in Spark is to:
    * **Keep data and workloads spread evenly across nodes.**
    * **Ensure that data size distributed to any given node does not exceed that node's RAM capacity.**
* Databricks runs on top of Azure or AWS. That means you pay for Databricks AND for Azure or AWS compute. Here's how to think about cost:
    * **How long are my nodes going to be turned on:**
    * **How long are my nodes going to be "crunching data":**
    * **How much storage am I going to use, and what are the costs for reading and writing from that storage:** 
    


## Spark vs. Databricks.
Let's clear up the difference between Spark and Databricks. Spark is a distributed computing framework for working with large datasets. Databricks hosts that technology, making it easier to use (they also contribute heavily to the Spark open-source project). 

Think of Git and Github: You can use Git with or without Github, but Github makes a few things a lot easier.  

   
## Spark Overview
A cluster is a configuration of machines (or nodes) that work together to accomplish a parallelization task.

Spark is a distributed computing platform. When you run a job in Spark, the driver node in your cluster decides the best way to distribute data across the worker nodes based on the operation and the data you are operating on. Each node (the driver node and the worker nodes) are separate machines (or virtual machines) with a specific configuration (# CPU cores, RAM, and runtime version).

![Cluster](https://i.imgur.com/IGe5bJd.png)

Generally, Spark is pretty good at deciding how to spread work across nodes, but you have the option to assume more control if you need to.  The standard abstraction level in Spark is a Spark Dataframe.  If you use a SparkDF, you're letting Spark make most decisions about how the job executes.

If you run into performance issues, you will likely want to see if there is data skew (when variable amounts of work are getting assigned to different nodes) and potentially repartition.  

As a last resort, you may need to drop to a less abstract version of a SparkDF, a SparkRDD (Resilient Distributed Dataset - drop that at a dinner party!).  With an RDD, you can be much explicit about how your analysis works.

To keep this brief, I am explicitly skipping partitioning and RDDs.  But I may write about these later.

If you remember nothing else from this article so far, remember this: The name of the game in Spark is to:

* Keep data and workloads spread evenly across nodes.
* Ensure that data size distributed to any given node does not exceed that node's RAM capacity.

## Interacting with Spark
In Python or R, you write some code in a notebook cell, you run it, and the data gets assigned to a variable. 
In Spark, there are two types of commands: **transformations** and **actions**.  

![Transformations v Actions](https://i.imgur.com/qvkTq9h.png)
*[Source: Databricks Visual API](https://training.databricks.com/visualapi.pdf)*

**Transformations** change the data in some way, like a filter, sort, or groupBy. When Spark evaluates **Transformations**, it won't execute the computation.  

When you run an **Action** command, Spark will evaluate all the queued up **transformations** before the **action** call. Then it will generate a DAG (Directed Acyclic Graph) that has the most efficient computation path. Finally, Spark will execute the DAG.  **Actions** commands include sum, display, top, and many others.

For example, say you want to perform a filter, a groupBy, then calculate a sum for each group.  In Python, each command would run independently, taking time, and using compute resources.  In Spark, all the commands would run once, in the most efficient way possible.

For great visual representations of available transformations and actions, check out the [Visual API](https://training.databricks.com/visualapi.pdf)

## The Spark API
![Spark Layout](https://2s7gjr373w3x22jf92z99mgm5w-wpengine.netdna-ssl.com/wp-content/uploads/2019/03/spark_graphic.png)
Scala, Python, and R have libraries for interacting with the Spark Engine. It's surprisingly easy to switch languages since the API wrappers for each language are consistent. 

In Databricks, you can set the language at the notebook level:

![Set language at Notebook Level](https://i.imgur.com/CK2jRs6.png)

or use flags within the cell to enable language evaluation just for that cell:

![Language flag](https://i.imgur.com/0qg2xeY.png)

Just remember that to get Spark's full power, you need to **use a Spark DataFrame**, not the dataframe of the language you're using (pandas DataFrame in Python, or and R Dataframe).

A bit more detail on this below:

### Can I Use Pandas?
In Databricks, this is legitimate to do:
```python
import pandas as pd
df = pd.read_csv("/path/to/file")
```
This will read the file into a Pandas dataframe.
This will **not** get you a **Spark Dataframe**. 

![Pandas](https://3qeqpr26caki16dnhd19sv6by6v-wpengine.netdna-ssl.com/wp-content/uploads/2014/06/pandas-for-data-analysis.jpg)

*sad panda*

Spark does not parallelize Pandas dataframes.  It only parallelizes Spark Dataframes.  That means your Pandas dataframe will only run on a single node of your cluster - the driver.  The rest of the nodes will sit there, twiddling their thumbs. So if you're using Pandas, no matter how many nodes you have, you will always be constrained by the driver node's memory / CPU.  

To make full use of Spark, create a Spark Dataframe:
```python
df = spark.read.format('csv').load("/path/to/file")
```

Spark will know how to distribute it across the nodes of the cluster.  However, what you gain in performance, you lose in syntactic sugar that Pandas provides. 

You will likely stumble into this command as you're Googling around: 
```python
pandasDf = sparkDf.toPandas()
```
This command does indeed convert a Spark Dataframe into Pandas one. However, expect this to take a while for a large dataset. 
The driver will have to pull all the data partitions back from the cluster nodes back to the single driver node.  If that data size exceeds the driver's RAM capacity, the command will crash (likely after several hours).



You can still have a Panda-like interface if you use [the Koalas library](https://koalas.readthedocs.io/en/latest/getting_started/10min.html).  However, I can't vouch for it since I haven't used it yet. 





### What about SQL?
In Spark, you can interchangeably use the Spark API and SQL to perform transformations and actions on Spark Dataframes.

For example,

this:
```python
result = df.filter(df.state == "IL").show()
```
this:

```python
df.createOrReplaceTempView("myDF")
 
result = spark.sql("SELECT * FROM myDF WHERE myDF.state = "IL")
```

and finally this:
```python
%python
df.createOrReplaceTempView("myDF")
```
```sql
%sql
SELECT * FROM myDF WHERE myDF.state = "IL"
```

Are effectively the same.  The single exception is, you cannot assign the last example to a variable.

As you get into using Spark / Databricks, you will likely find that each approach is useful in its way.

## RDDs and UDFs
As you're reading Spark or Databricks documentation, you will run into frequent mentions of RDDs and UDFs. Let's have a quick look at what they are, so you can speak the language.

#### RDD
An RDD, or Resilient Distributed Dataset, is a core abstraction level in Spark.  It is a fault-tolerant collection of elements that Spark can operate on in parallel across clusters.  In the beginning, you will likely not work with RDDs, because Spark Dataframes provide a higher-level abstraction that makes it easier to work with data in Spark.

#### UDF
UDFs are User Defined Functions in Spark.  In python, when you create a function and pass it to `df.apply()` - it's a similar idea.  UDFs make things convenient, but Spark does not know how to optimize them.  In the beginning, it's best not to write your own.  

## Installing Libraries
The experience of installing libraries brings us to the first significant divergence of Spark and Databricks.  If you are running Spark in a docker container, installing libraries is just a regular `pip install`.  

Databricks, on the other hand, has many [libraries preinstalled already](https://docs.databricks.com/libraries/index.html). Before installing something, it's a good idea to try to `import` it and see if you get an error.  If you do, head over to Clusters, Libraries, and install what you need. Just make sure your cluster is on.

![](https://i.imgur.com/xHSkXIQ.png)

## Storage

Storage was another thing in Databricks that took a bit of time to understand.

### DBFS
The Databricks File System or DBFS provides a way to interact with files stored in Databricks.  The file system itself, however, is an abstraction.  DBFS encompasses files you manually uploaded to Databricks (usually stored at `/`, `/user`, `/FileStore`), mounts (`/mnt`), as well as other things.

DBFS makes things very convenient. You can mount an S3 Bucket at `/mnt/S3_BucketName`, and an Azure Data Lake at `/mnt/ADLS_NAME`, and mix data from these two sources seamlessly in your analysis. 

```python
# Read Data
df = spark.read.format("csv").load("dbfs:/mnt/S3_BucketName/file.csv")

# Do some stuff
...

# Write Data
spark.write.format("delta").save("dbfs:/mnt/ADLS_NAME/output_delta_lake")
```

Keep in mind that anything you store outside of `/mnt/YOUR_MOUNT_X` will live on Databricks instances.  After a quick Google search, I couldn't figure out how much it costs. But suffice it to say, I ran up a bit of a bill not knowing that, so I suggest avoiding it.

### Hive Metastore
![](https://i.imgur.com/neA1Y1D.png)
"Hive Metastore" is about as much of a cool buzzword as it gets, and it lives in the "Data" tab in Databricks.  This is a nice *relational-if-you-want-it* database that Databricks maintains to make life easier and use SQL.  

To put a Spark Dataframe into the Metastore:
```python
df.createOrReplaceTempView(tableName)
```

The Metastore has several layers of "persistence".  You can create temporary tables just for the session duration, tables available to all users, or tables available to only one user.

This could be another blog post, so we'll leave it here for such a thing.


### The Delta Lake
The Hive Metastore doesn't have much of a relationship to DBFS, except in a Delta Lake. Delta Lake, created by Databricks, is a data format heavily based on Parquet.  Mounting Delta Lake files from DBFS to the Hive Metastore will make Databricks automatically keep the two in sync.  So when you change data in the Hive Metastore or write new data to Delta files, its counterpart will update accordingly.

Delta is also versioned, keeping a granular record of every data change.  This lets you write SQL or Spark to "time travel" across your data, using a version or timestamp.  This has helped me recover from a dumb mistake many times.

The [Databricks' documentation](https://docs.databricks.com/delta/delta-batch.html) is pretty good here, so I'll let you read it if interested.



## Pricing
![](https://i.imgur.com/7It4KqH.png)

Databricks pricing is complicated. 

If you look at the Databricks Pricing page for [Azure](https://databricks.com/product/azure-pricing) or [AWS](https://databricks.com/product/aws-pricing), you pay a certain amount of cents per Databricks Unit (DBU).

Except... what's a DBU? And why are we looking at Azure and AWS? Aren't we using Databricks?

Databricks' core value offering is to provide managed Spark and interactive notebooks on top of cloud infrastructure. Databricks does NOT offer the cloud infrastructure itself.  In this instance, by cloud infrastructure, I explicitly mean Storage and Compute.  

That job is outsourced to AWS or Azure - as it should - but they still need to make money, so they charge you for cloud infrastructure as well. Again...storage and compute. 

Any given Databricks analysis cost is going to consist of: 

* **How long are my nodes going to be turned on:** *(This is the price you pay for AWS for keeping your nodes running)*
* **How long are my nodes going to be "crunching data":** *(This is the price you pay to Databricks (DBUs) that get counted when a computation is running.)*
* **How much storage am I going to use, and what are the costs for reading and writing from that storage:** *(This is the price you pay to AWS for using S3. Ideally, if you're using EC2 instances and you're in the same region, this should be 0)*

Knowing this, the most reasonable way to optimize costs is to work on a tiny subset of your data, using a small cluster with the least nodes while you're developing your code. Then, scale up to more / larger nodes as you begin processing your full dataset. 

This is a good rule of thumb, and cost/performance optimization here can get pretty tricky (maybe tricky enough for another blog post), so I will leave it here for now.  


## Wrapping up
Hopefully this intro overview has been helpful. Databricks and Spark is a cool technology and we're excited to see what you do with it!
If I missed something or got something wrong, I would love a [tweet from you](https://twitter.com/mrmaksimize) letting me know.

If you liked this article, please share it and tag [@DevDataPship](https://twitter.com/devdatapship) on Twitter!



