# Shuffling

Shuffles happen when we move data from one node to another.
It can be an enormous hit, it means Spark has to send data from one node to another.
Hence Latency !!!!?

```Java
val pairs = sc.parallelize(List( (1, "one"), (2, "two"), (3, "three") ))

pairs.groupByKey() // A shuffledRDD is made 
```

To avoid shuffling or at least the amount of suffling, we need to use reduceByKey instead.
The reduceByKey is transformation like groupByKey. And the reduce is applied at the low level, before the shuffle.


# Partitioning

The data within an RDD is split into several partitions.

* Partitions never span multiple machines, i.e, tuples in the same partition are guaranteed to be on the same machine.

* Each machine in the cluster contains one or more partitions.

* The number of partitions to be used is configurable. By default, it is equal to the total number of cores on all executors nodes.


There two kinds of partitions available in Spark : Hash partitioning and Range partitioning.

For RDD, it is better to use Range Partitioning.

You can create your own partition by invoking partionBy => to create an RDD with a specified partitioner.

Example
```Java
val pairs = purchasesRdd.map( p => (p.customerId, p.price) )
// instantiate the partionner
val tunedPartioner = new RangePartioner (8, pairs)

val partioned = pairs.partitionBy(tunedPartioner).persist() // better persit to not partition again.

``` 

It requires : 
*  number of partitions
* Pair RDD with ordered keys

Here a list of operations on Pair RDDs that hold to (and propagate) a partioner:
```Java
cogroup, groupWith, join, leftOuterJoin, rightOuterJoin, groupByKey, reduceByKey, foldByKey, combineByKey, partitionBy, sort, 
mapValues //if parent has a partitioner
flatMapValues //if parent has a partitioner
filter  //if parent has a partitioner
```
All other operations will produce a result without a partioner. 

map and flatMap will induce the RDD to lose its partition. Since it change the keys produce new keys.
Better do it before the partiton. 
Better use mapValues. We only work on values and keep keys.



# Structured Data: SQL, DataFrames, and Datasets

With our newfound understanding of the cost of data movement in a Spark job, and some experience optimizing jobs for data locality last week, this week we'll focus on how we can more easily achieve similar optimizations. Can structured data help us? We'll look at Spark SQL and its powerful optimizer which uses structure to apply impressive optimizations. We'll move on to cover DataFrames and Datasets, which give us a way to mix RDDs with the powerful automatic optimizations behind Spark SQL.



DataFrame us Spark SQL's Core abstraction. Like a table in Relational Databases.

DataFrames are, conceptually, RDDs full of records with known schema.


DataFrames are untyped. It means, the scala compiler doesn't check the types in its schema.
DataFrames contain rows which can contain any schema.
To create a DF, we either create it using row data, by reading from the SparkSession, or creating it from an RDD.

```Java
val tupleRDD= ...

val tupleDF = tupleRDD.toDF("id", "name", "city", "country")


```

