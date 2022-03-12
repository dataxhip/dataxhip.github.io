# Scala for the impatient

Scala is statically typed, it means it gives you the error before compiling. 
It is very scalable
Marting odersky at EPFL

Very libray oriented
Work with JVM
More flexible than Java in term of feature adding

Expand your mind with the functional aspect

Very complex language!!!

https://scala-lang.org/api/3.x/



# Data Parallel to Disstributed Data Parallelism

When the abstract is made for a compute node and when data is in a jar, we could apply  map operation to it  and then do some processing in parallel
* split the data
* Workers/threads independently operates on the data shards in parallel.
* Combine when done (if necessary)

```scala
val res= jar.map(jellyBean => doSomething(jellyBean))
```

Scala's parallel Collection is a collection abstraction over shared memory data-parallel execution.

For the distribution part, data is splitted over several nodes. And Nodes independently operates on the data shards in parallel.
The abstraction is maintained.

Spark implement a distributed data parallel model called RDD, for Resilient Distributed Datasets.


# Latency
partial faillure : nodes can crash
Latency : certain operations have much higher latency than others due network communication

It is better to work in memory than from disk. 
network communication is the slowest.

Many strategy to handle latency in Spark Cluster.
Functional Programming
==> Keep all data immutable and in memory. Hence all operation on data are just functional transformations, like regular scala collections.
==> Fault tolerance is achieved by replaying function transformation over original dataset.

# RDDs
Using RDDs in Spark is like normal Scala sequential/parallel collections, with the added knowledge that your data is distributed.

The operations are quite the same : map, filter, reduce, count, ...

Let's say given an RDD, we want to search a mention (EPFL) and count the number of pages it appears.

```Java
val result = encyclopedia.filter(page => page.contains("EPFL")).count()
```

Classic wordcount with RDD

```Java
val rdd = spark.textFile("hdfs://...")

val count = rdd.flatMap(line => line.split(" "))
                .map(word => (word, 1))
                .reduceByKey(_ + _)

```

There are two ways to create an RDD : 
* Transformaing an existing RDD : just like Map on a List return a new List, many Higher Order functions defined on RDD return a new RDD.
* From a SparkContext(or SparkSession): use parallelize (convert a local Scala collection to an RDD) or textFile (to read from local or HDFS and return an RDD of String) from a SparkSession


# RDDs: Transformations and Actions 

## Transformers, returns new collection as a result (not single values). examples: map, filter, flatMap, groupBy

```Java
map(f : A => B): Transversable[B] 
```

## Accessors, returns single values as results (Not collections). Examples: reduce, fold, aggregate
```Java
reduce (op: (A,A)=> A):A 
```

And similaryb as Scala transformations and accessors on collections, Spark define Transformations and Actions on RDDs

## Transformations, return new RDDs as results.
They are LAZY, their result RDD is not immediately computed
*map, flatMap, filter *
## Actions, compute a result based on an RDD, and either returned or saved to an external storage (e.g HDFS)
They are EAGER, their result is immediately computed.
*collect, count, take, reduce, foreach*

### Lazyness/eagerness is how spark limit network communication while using the programming model


# REDUCTION

Reduction, such as fold, reduce and aggregate from Scala Sequential Collections.
==> Walking through a collection and combine neighboring elements of the collecion together to produce a single combined result.
```Java
case class Taco(kind:String, price:Double)

val tacoOrder = List(
	Taco("Carnitas", 2.34),
	Taco("Corn", 3.50),
	Taco("Barbacoa", 2.50),
	Taco("Chicken", 4.25)
	)

val cost=tacoOrder.foldLeft(0.0)((sum, taco) => sum + taco.price)

```
```fold and foldLeft```, foldLeft is not parallizable, do not exist in Spark.
Aggregate, fold and reduce some three reduction opertations.
Aggregate is often preferred because it helps in the reduction level to only work on desired elements.


# Paired RDDs
Often working with distributed data, it's useful to organize data into key-value pairs.

In Spark, distributed key-value  pairs are "Pair RDDs"

Why is it USEFUL? 
Pair RDDs allow you to act on each key in parallel or regroup data across network.

Pair RDDs have additionnal specialized method for working with data associated with keys. 
RDDs parametrized by a pair are Pair RDDs.
``RDD[(k, v)]`` are special Spark RDDs, They are manipulated differently.
They are known as Key-Value pairs in Spark.

When an RDD is created with a pair as its element type, Spark automatically adds a number of extra useful additional methods (extension methods) for such pairs.
Some of the most important extension  methods for RDDs containing pairs are : 
```Java
def groupByKey(): RDD[(K, Iterable[V])]

def reduceByKey(func : (V, V) => V ) : RDD[(K, V)]

def join[W](other: RDD[(K, W)]):  RDD[(K, (V, W) )]

```

How to create a Pair RDD?
It is created often from already-existing non-pair RDDs, for example by using map operation on RDDs.

```Java
val rdd: RDD[Wikipedia]= ...

// has a type : org.apache.park.rdd.RDD[(String, String)]
// We  can create a Pair RDD using map operation

val pairRdd = rdd.map( page => (page.title, page.text))

```

Once created, you can use transformations specific to key-value pairs such as reduceByKey, groupByKey and join.

## Pair RDDs Operations
Important operations are defined opn Pair RDDs (but not available on regular RDDs)

#### Transformations
groupByKey, reduceByKey, mapValues, keys, join, leftOuterJoin, rightOuterJoin


Recall groupBy from Scala  collections
`def groupBy[K](f : A => K) : Map[K, Traversable[A]] ` 

=> Breaks up a collection into two or more collections according to function that is passed to it. Results of the function is the key, the collection of results that return that key when the function is applied to it. Returns a Map mapping computed keys to colection of computed keys of corresponding values.

For example let's group the list below into child, senior and adult

```Java
val ages = List(2, 52, 44, 23, 17, 14, 12, 82, 51, 64)

val grouped = ages.groupBy{
	age => if (age> 18 && age < 65) "adult"
	       else if (age < 18) "child"
	       else "senior" 
	   }
```
==> grouping by values based on some discriminating function

If we come back now to groupByKey in Spark,

```Java
def groupByKey(): RDD[(K, Iterable[V])]
```

Example 
```Java
// case class that shows data type
case class Event(organizer:String, name:String, budget:Int)

val eventRdd = sc.parallelize(...).map( event.organizer => event.budget )
// keys are organizer and values budget
val groupedRdd = eventRdd.groupByKey()
// But no result since groupByKey is a transformation operation, not an action
// to  print, we need to do

groupedRdd.collect().foreach(println)

// the result is ( organizers) and a combined buffer containing a list of budget.
// There is not operation
```

ReduceByKey is a transformation operation that goes further that groupByKey. 
Conceptually, it can be thought as a combination of a groupByKey and reduce-ing on all the values per key.
It's more efficient though, than using each separately.

```Java
def reduceByKey(func : (V, V) => V ) : RDD[(K, V)]

```
Example

```Java
// case class that shows data type
case class Event(organizer:String, name:String, budget:Int)

val eventRdd = sc.parallelize(...).map( event.organizer => event.budget )
// keys are organizer and values budget
val reducedRdd = eventRdd.reduceByKey(_+_)
// But no result since groupByKey is a transformation operation, not an action
// to  print, we need to do

reducedRdd.collect().foreach(println)

// the result is ( organizers) and a combined buffer containing a list of budget.
// There is not operation
```


mapValues can be thought of as short hand for ` rdd.map{ case (x, y) : (x, func(y))} `.
That is, it applies a function to only the values in a Pair RDD.

```Java
def mapValues[U] (f : V => U ) : RDD[(K, U)]

```

countByKey, simply counts the numnber of elements per key in a Pair RDD, returning a normal Scala Map. Remember, it is an Action!!! (mapping keys to counts).

```Java
def countByKey(): Map[K : Long]
``` 

Example of countByKey on top of mapValues. after mapValues doing operations  on values only and returning values. In this process, it will map 1 to every value.
This operation is to compute the average Budget per Event Organizer.

```Java
// calculate a pair ( as key's value) containing (budget and #events)
val intermediate= eventRdd.mapValues(b => (b, 1)) // map 1 to each value, but we still have keys b => (b, 1) is (k,v) => (v, 1)). Now we have one key and two values
                          .reduceByKey((v1, v2) => (v1._1 + v2._1,  v1._2 + v2._2)) //, we sum values with values  and ones with ones ??? // RDD[(String , (Int, Int))]

 val avgBudgets = intermediate.mapValues {
 	case (budget, numberOfEvent) => budget / numberOfEvents
 }

// to print

 avgBudgets.collect().foreach(println)

```

### Pair RDD  Transformation : keys
keys (def keys: RDD[K]) Returns an RDD with the keys of each tuple.
Note : this method is a transformation and this returns an RDD because the number of keys in a Pair RDD may be unbounded.
It is possible for every value to have a unique key, and thus is may  not be possible to collect all keys at one node.

Example: we can count the number of visitors to a website using the keys transformation.

```Java
case classe Visitor(ip:String, timestamp: String, duration:String)
val visits: RDD[Visitor] = sc.textFile(...)
                             .map(v => (v.ip, v.duration))

val nimberOfvisists = visits.keys.distinct().count() // count is the  action

```


# Join RDD

There are another sort of transformation on Pair RDDs. They are used to combine multiple datasets. They are one of the most commonly-used operations on Pair RDDs.


There are two kinds of joins: Inner Join and Outer Join (leftOuterJoin and rightOuterJoin).

The main difference is what happen to the key when the two RDDs containing different keys.
The difference between inner and outer joins is what happens to customers whose Ids don't exist in both RDDs.























