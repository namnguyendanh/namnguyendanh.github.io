
1.x - RDD framework
2.x - DataSets / Dataframe
    

### Important Spark Tutorials and Blog
[Important spark links - D-Zone](https://dzone.com/articles/the-complete-apache-spark-collection-tutorials-and)

## re-partition vs coalesce 
```text
Keep in mind that repartitioning your data is a fairly expensive operation. 
Spark also has an optimized version of repartition() called coalesce() that allows avoiding data movement,
 but only if you are decreasing the number of RDD partitions.
```
ex:

```text

It avoids a full shuffle. If it's known that the number is decreasing then the executor can safely keep data on the minimum number of partitions, only moving the data off the extra nodes, onto the nodes that we kept.

So, it would go something like this:

Node 1 = 1,2,3
Node 2 = 4,5,6
Node 3 = 7,8,9
Node 4 = 10,11,12
Then coalesce down to 2 partitions:

Node 1 = 1,2,3 + (10,11,12)
Node 3 = 7,8,9 + (4,5,6)
Notice that Node 1 and Node 3 did not require its original data to move.
```

### Difference between coalesce and repartition

coalesce uses existing partitions to minimize the amount of data that's shuffled. repartition creates new partitions
and does a full shuffle. coalesce results in partitions with different amounts of data (sometimes partitions that
have much different sizes) and repartition results in roughly equal sized partitions.

### Why MSCK REPAIR is an expensive operation

`MSCK REPAIR TABLE` can be a costly operation, because it needs to scan the table's sub-tree in the file system (the S3 bucket).
 Multiple levels of partitioning can make it more costly, as it needs to traverse additional sub-directories.
 Assuming all potential combinations of partition values occur in the data set, this can turn into a combinatorial explosion.

If you are adding new partitions to an existing table, then you may find that it's more efficient to run `ALTER TABLE ADD PARTITION`
commands for the individual new partitions. This avoids the need to scan the table's entire sub-tree in the file system.
It is less convenient than simply running `MSCK REPAIR TABLE`, but sometimes the optimization is worth it. A viable strategy
is often to use `MSCK REPAIR TABLE` for an initial import, and then use `ALTER TABLE ADD PARTITION` for ongoing maintenance
as new data gets added into the table.

If it's really not feasible to use `ALTER TABLE ADD PARTITION` to manage the partitions directly,
then the execution time might be unavoidable. Reducing the number of partitions might reduce execution time,
because it won't need to traverse as many directories in the file system. Of course, then the partitioning is different,
which might impact query execution time, so it's a trade-off.

## Spark transformation is lazy and it's advantages

For transformations, Spark adds them to a DAG of computation and only when driver requests some data, does this DAG actually gets executed.

One advantage of this is that Spark can make many optimization decisions after it had a chance to look at the DAG in entirety.
This would not be possible if it executed everything as soon as it got it.

For example -- if you executed every transformation eagerly, what does that mean?
Well, it means you will have to materialize that many intermediate datasets in memory.
This is evidently not efficient -- for one, it will increase your GC costs.
(Because you're really not interested in those intermediate results as such.
Those are just convnient abstractions for you while writing the program.) 
So, what you do instead is -- you tell Spark what is the eventual answer you're interested and it figures out best way to get there.

## Spark Transformation vs Action

[Spark Transformation vs Action](https://medium.com/@aristo_alex/how-apache-sparks-transformations-and-action-works-ceb0d03b00d0)


## Spark architecture 
```text
Cluster
    - Driver 
    - Executor (Memory / Disk) (Cores / Task Slots)
    - Actions -> (One or more transformations (1 or more) -> Job->(Stages 1 or more)->(Tasks 1 or more)
    - Only task interacts with H/W.(tasks in same stage performs same transformation but on different data.
    - If a different operation / transformation needs to be performed it needs to be in a different stage.
    - 1 Task == 1 partition == 1 slot == 1 core.
```

### Spark partitions

- Firstly, spark partition is not the same as hive partition
- spark partition can be divided into 3
    1. Input (Map Phase) - controlled by parallelism and `sql.files.maxPartitionBytes`
    2. Shuffle - controlled by `sql.shuffle.partitions` 
    3. Output (Reduce) - Coalesce(n), Repartition(n) or something like `df.write.option(maxRecordsPerFile, N)`
- Rule of thumb for determing shuffle partitions required for a job.

```text
Output target sixe for shuffle : ~200mb
partition count for shuffle would be : input size to the shuffle stage / output target size 
ex:
shuffle input stage : 210GB

shuffle partitions = 2100000MB/200MB = 1050 
so the shuffle partition should be 1050 partitions. 
but if there are 2000 cores avaialble then we should use 2000 as the shuffle partition.

Optimize further by using the ceiling of core count 

so total partitions for shuffle = Math.ceil(( Input size to shuffle / target size) / total cores) * total cores
By this way you ensure that you are always using all the cores effectively.

```
- After the shuffle stage if there are too many other transformation before eventually writing it out it's better
to create some sort of stage barrier. By this way we can reduce the total imbalance on the output file size.
- use `df.localCheckpoint().partition(n).write()...` for creating local stage barrier.


### Advance Optimization
- Look for balance in the jobs.
- `df.cache = df.persist(MEMORY_AND_DISK)` remember that persist actually uses the same internal memory that is 
used by other operations like shuffle and join. So persist with care.
- Use persist when you find commanlity or find something repetitive. Find out the memory fraction that is required for spark in terms of heap and non heap.
- Also unpersist whatever you had persisted so that the memory is freed up for other using the same cluster...
- How dbio cache helps minimize data scans.


### JoinOptimization
- BroadCast Join are the fastest but be careful when using it. It's used when one side of the join is actually less
than `sql.auto.autoBroadCastJoinThreshold` default (10 m)
Risk associated with BCJ is 
    1. What if there is not enough driver memory
    2. What if DF size is > `driver.maxResultSize`
    3. What is DF size is > single executor available memory size.
    
Make sure you have validation functions to make sure that all these are caught.

- Persistence vs Broadcast 
    1. Assume, you have 12 GB dataframe and you persist them in 6 partitions across 3 executors. so 4gb in each executor.
    2. Now in a Broadcast join you will have the same thing as step 1 which will be collected and sent to the driver
    which will in turn send it to the executors i.e 3 executors will be broadcasted 12 Gb . so total 36 + 12 Gb from step
    1 since that is not yet Garbage collected making it 48GB for Broadcast join.
    
Note: So whenever the BCJ is deserialized and row compressed they take more space than what we initially assumption so account for that.
`data1.join(broadcast(data2), data1.id == data2.id)`

skew data example with salting fix:
```python
df.groupByKey("city","state").agg(f(x)).orderBy(col, ascending=False)
salt = random(0,spark.cong.get(shuffle.partitions) - 1)
df.withColumn("salt", lit(salt))
  .groupByKey("city","state", "salt")
  .agg(f(x))
  .drop("salt")
  .orderBy(col, ascending=False)
```

- Try to use `null safe equality` for nulls in the datasets. use `isolated salting` for avoiding nulls. In this 
you basically only apply high salting to columns with `null`.

- You should use `Not exists` instead on `Not in` in SQL.

- Range Join optimization?? Not explained check reference.

- Don't use distinct instead use `approxCountDistinct()` 5% margin of error.
- `dropDuplicates before JOIN or groupBy` . `dropDuplicates` is an alias.
 
If you can use an explode and `sql.functions()` instead of doing `map` or `flatmap` that is usually better.
If you are using primitives in UDF's it's usually not vectorized and is not gonna perform that well.
Try to use `sql.function` wherever possible. use pandas UDF's or Arrow UDF's if not available in function

**Summary**
- Utilize Data Skipping / Partition Pruning to narrow down (Lazy Loading)
- Maximize your hardware
- Right size spark partitions
- balance
- optimized joins
- minimize data movements
- minimize repetitions
- only use vectorized UDF's


### What are accumulator variables??

### Why stage barriers improve the read performance. 

[What is tungsten's project](https://databricks.com/glossary/tungsten)

### Fork join pool / Fork join task support

```python
solve(problem):
    if problem is small enough:
        solve problem directly (sequential algorithm)
    else:
        for part in subdivide(problem)
            fork subtask to solve(part)
        join all subtasks spawned in previous loop
        return combined results
```
[Java implementation for fork join](https://www.baeldung.com/java-fork-join)


### Fair and FIFO task scheduling what does it do how does it help.


Question : Say that we have a partitioned data set and there is code which runs a series of SQL 
  spark SQL queries all of them operating over the columns rather than the rows of the data
  set there is and one of these queries is actually doing an aggregate operation over each of the each of the columns           
  even though this is quick the other queries in the in this process are gonna take a lot of time okay so my
  question here is in this situation should I be looking into scheduling of these tasks itself and second
  thing is like when you're processing columns rather than rows should we be like looking at some sort
  of like vertical partitioning rather than like the just repartition of spark which offers.

[Someone else's notes on Daniel lecture](https://jilongliao.com/2019/07/15/Spark-Performance/)

##Memory Tuning for spark
[Basic memory configuration](https://medium.com/analytics-vidhya/apache-spark-memory-management-49682ded3d42)

```text
JVM Heap memory
    - Spark Memory (0.75 of heap)
        1. Executor Memory : It's mainly used to store temporary data in the calculation process of Shuffle, Join, Sort, Aggregation, etc. 
        2. Storage Memory : It's mainly used to store Spark cache data, such as RDD cache, Broadcast variable, Unroll data, and so on.
    - User Memory ( It's mainly used to store the data needed for RDD conversion operations, such as the information for RDD dependency. (~0.25 MB)
    - Reserved Memory (Reserved for system and spark's internal objects)(300 MB)
```


##CI-CD for spark
[DataBricks CI-CD](https://databricks.com/blog/2017/10/30/continuous-integration-continuous-delivery-databricks.html)
[DataBricks Video](https://databricks.com/blog/2017/10/30/continuous-integration-continuous-delivery-databricks.html)
[At Metacog](https://databricks.com/blog/2016/04/06/continuous-integration-and-delivery-of-apache-spark-applications-at-metacog.html)
[Using AWS and Github actions](https://medium.com/alterway/building-a-ci-cd-pipeline-for-a-spark-project-using-github-actions-sbt-and-aws-s3-part-1-c7d43658832d)

### Spark SQL joins & performance tuning
[Join strategies Broadcast Hash Join, Shuffle Hash Join, Shuffle Sort Merge, Iterative Broadcast Join](https://towardsdatascience.com/strategies-of-spark-join-c0e7b4572bcf)

*Troubleshooting shuffle / uneven sharding - Some task are taking a lot of time. speculative task's are triggered.*

**ShuffleHashJoin** (Impt)
 - The join keys don’t need to be sortable.
 - Supported for all join types except full outer joins.
 
Problems with shuffle joins usually are 
 - it’s an expensive join in a way that involves both shuffling and hashing(Hash Join as explained above). Maintaining a hash table requires memory and computation.
 - uneven sharding and limited parallelism for instance if we perform a query on US census and try to (inner) join on states
 what is going to US DF is very big and state is going to be very small so the job the bottlenecked on only a certain
 state where the data points are lot and significantly less resource will be used for smaller states.
 
 ```python
    join_df = sqlContext.sql("Select * FROM people_in_the_US JOIN states ON people_in_the_US.states = states");
```
 ```python
    sqlContext.sql("Select * FROM people_in_cali  LEFT JOIN all_people_in_world ON people_in_cali.id = all_people_in_world.id");
```

**BroadcastHashJoin** (Impt)
 - If one dataframe is small and could be fit in memory we simply put it in memory and send it to all the worker nodes.
 - It doesn't require you to do any shuffle as the operations are performed on worker nodes.
 - Use explain command to see which JOIN was selected by sql catalyst.
 - Spark deploys this join strategy when the size of one of the join relations is less than the threshold values(default 10 M). The spark property which defines this threshold is spark.sql.autoBroadcastJoinThreshold(configurable).
 
**SortMergeJoin**

 
**CartesianJoin**
Not much explained here.

**One to Many Joins**
One to many joins - parquet took care of it?

**Theta Joins**
Join is based on a condition - Full cartesian join and then do the condition.
Generate bucket to match condition and match the buckets?? not clear about this as well.


### JOIN
`join(self, other, on=None, how=None)`

how=left,right,inner,outer
ex : `empDF.join(deptDF,empDF.emp_dept_id ==  deptDF.dept_id, "inner")`


## Tackling Data Skewness in Spark
- [Iterative Broadcaast Join optimization](https://github.com/godatadriven/iterative-broadcast-join)
- [Data Skewness](https://bigdatacraziness.wordpress.com/2018/01/05/oh-my-god-is-my-data-skewed/)
- [Salting code in Python to reduce data skew](https://datarus.wordpress.com/2015/05/04/fighting-the-skew-in-spark/)
- [Trouble shooting data skewness](https://dzone.com/articles/why-your-spark-apps-are-slow-or-failing-part-ii-da)


## Tackling memory issues in spark
- [Troubleshooting memory issues in spark](https://dzone.com/articles/common-reasons-your-spark-applications-are-slow-or)

## Spark Optimization
- [Spark Core - Proper Optimization](https://www.youtube.com/watch?v=daXEp4HmS-E&ab_channel=Databricks) (TODO)


## GroupBy vs ReduceBy
[Stack Overflow - differences](https://medium.com/@sderosiaux/governing-data-with-kafka-avro-ecfb665f266c)
```text

Group by Key

sparkContext.textFile("s3://../..")
			.flatMap(lambda line: line.split())
			.map(lambda word: (word,1))
			.groupByKey()
			map(lambda (w,counts) : (w, sum(counts)))


Reduce By Key

spaarkContext.textFile("s3://../..")
			.flatMap(lambda line: line.split())
			.map(lambda word: (word,1))
			reduceByKey(lambda a,b : a+b)


Reduce by key will perform better because it will combine the results in the node before sending it over.
whereas GBK all the records will have to be moved to
its respective partitions in appropriate nodes.


We can not always apply ReduceByKey as ReduceByKey requires combining all the values into another value 
with the exact same type.

aggregateByKey, foldByKey, combineByKey
```

## Spark Cluster Management
[Deep Dive](https://dzone.com/articles/deep-dive-into-spark-cluster-management)


## Ideal Paritition count 
The recommended number of partitions is around 3 or 4 times the number of CPUs in the cluster so that the work gets distributed more evenly among the CPUs.

### What is synthetic keys in hive? What are they used for?


### Create a data frame with schema attached.
```sparksql
spark.createDataFrame(vals, schema=schema)
```

### Defining schema in spark 

```sparksql
from pyspark.sql.types import
schema = StructType([StructField("author", StringType(), False),  StructField("title", StringType(), False),  StructField("pages", IntegerType(), False)])
```

### Create a SparkSession
```sparksql
   spark = (SparkSession
     .builder
     .appName("Example-3_6")
     .getOrCreate())
```
### Read data in DF
```sparksql
file = "/databricks-datasets/learning-spark-v2/flights/summary-data/orc/*"
df = spark.read.format("orc").option("path", file).load()
df.show(10, False)
```
### Read binary file in DF
```sparksql
path = "/databricks-datasets/learning-spark-v2/cctvVideos/train_images/"
binary_files_df = (spark.read.format("binaryFile")
  .option("pathGlobFilter", "*.jpg")
  .load(path))
binary_files_df.show(5)
```
### write to parquet
```sparksql
df.write.parquet(...)
```

### condition when-otherwise on column
```sparksql
df_nextdate = df_nextdate.withColumn('next_timestamp', when(df_nextdate.next_timestamp.isNull(),
                                   to_timestamp(lit("2038-01-19 03:14:07")))
                  .otherwise(to_timestamp(df_nextdate.next_timestamp)))
```

### Regex extract
```sparksql
df_gmsd = df_gmsd.withColumn('market_legacyname',
                             regexp_extract(concat_ws(',', 'attributes'), 'Market:([^,]+)',1))
```


### Row Number
```sparksql
df_lastrec = df_lastrec.withColumn('rnk', row_number()
                   .over(Window
                         .partitionBy(
                             "zipcode")
                         .orderBy(desc("update_id"))))
```

### Lead function
```sparksql
df_final = df_final.withColumn('end_date', lead('eventtimestamp_utc' , 1)
                           .over(Window
                                 .partitionBy('zipcode','propertytype')
                                 .orderBy('zipcode', 'propertytype', 'eventtimestamp_utc', desc('status'), 'ruleId')))
```
### Lag function
```sparksql
df_final = df_final.withColumn('md5val_prev', lag('md5val', 1, 0)
                  .over(Window
                        .partitionBy('zipcode', 'propertytype')
                        .orderBy('zipcode', 'propertytype','eventtimestamp_utc', 'status', 'ruleId')))
```





















































