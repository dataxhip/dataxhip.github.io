# Datasets

DataFrames are typed!
DataFrames are actually DataSets of Rows

``type DataFrame =  DataSets[Row]``

What is a DataSet?

* A typed distributed collections of data
* DataSet API unifies the DataFrame API and thne RDD API. Mix and Match!
* DataSets require structured/semi-structured data. Schemas and Encoders are the core part of DataSets.


==> We get more type information on DataSets than on DataFrames, and we get more optimization on DataSets than on RDDs.

Example: calculate the average home price per zip code with DS. Assuming listingsDS is of typoe DataSets[Listing]
```Java
linstingDS.groupByKey(l => l.zip)            // like RDD grouypBy
          .agg(avg($"price")).as[Double]     // DataFrame operators
```

* We can use relational DF operations on DS
* DS add more typed operations that can be used as well
* DS let us use higher-order functions like map, flatMap, filter like with RDD

## Creating DataSets

### From DataFrame
we just the ``toDS``convenience method
```Java
myDF.toDS 
```
But it requires the import of ``spark.implicits._ ``

### From a file like JSON for instance
```Java
val myDS = spark.read.json("people.json").as[Person]
```
Given that we already have a `case class Person` well defined for the Schema.

### From RDD
we just use the ``toDS`` for convenience method

```Java
myRDD.toDS 
```
But it requires the import of ``spark.implicits._ ``

### From common Scala types
Just use the toDS for convenience method
```Java
List("Kindi", "Mara", "Massaka").toDS // import spark.impplicts._
```

## Transformations on DataSets

The DS API includes both typed and untyped transformations

* Typed Transformations are typed variants of many dataFrame transformations + additional transformations such as RDD-like higher order functions (map, filter, ...)

* UnTyped Transformations are those on DataFrames

These APIs are integrated. You can call a map on a DataFrame and get back a DataSet, for example

** Not every operation from RDDs are available on DataSets, and not all operations look 100% the same on DataSets as they did on RDDs**

```Java
val df = List((1, "one"), (2, "two"), (3, "three"), (4, "four")).toDF //dataframe

val res = df.map(row => row(0).asInstanceOf[Int] +1 )  // dataset
```
List of typed Transformations on DataSets

```
map, flatMap, filter, distinct
```
### Grouped Operations on DataSets
* Calling `groupByKey` operation on a dataset returns a `KeyValueGroupedDataSet `

* `KeyValueGroupedDataSet `  contains a number of aggregation operations which return DataSets

* ``reduceGroups`` 
* ``agg``
* ``mapGroups``
* ``flatMapGroups``


`reduceByKey ` is missing 


# Datasets from the Chapter 11 of the Spark Definitive Guide Book

To know first that DataFrames are DataSets of type `Row` that are available across Spark's different languages.
With DataFrames, the schema is not inferred, we only manipulate `Row`s. But with DataSets, even though we can still work as DataFrames (because under the hood, DataFrames are just DataSets of type),
we can create a schema object (using the case class for instance).

`DataSets`are strictly JVM language feature that work only with Scala and Java.
With `DataSets`, we can define the object that each row in the dataset will consist of. It is done using the `case class object`, that define our data's schema. In Java, we define Java Vean.
`DataSet` is the typed set of API in Spark.

## How it works?
When we have case class object that define the schema for our DataSet, an ``Encoder`` directs Spark to generate code at runtime to serialize the object (the case class Person for example, with name and age as resp.String and Int) into binary structure. for DataFrames, this binary structure is `Row` (hence DataFrames are DataSets of type Row).


## Creating DataSet

```Java
case class Flight(DEST_COUNTRY_NAME: String,
                  ORIGIN_COUNTRY_NAME: String, count: BigInt)
```
* Immuatable
* Parameters are by default `val`s
* Decomposable through pattern matching
* Easy to use and manipulate

This case class defines the schema of our DataSet.
When we read our data, it is DataFrame, and then by applying the .as[] to the DataFrame, we cast it to DataSet.

```Java
val flightsDF = spark.read
  .parquet("/data/flight-data/parquet/2010-summary.parquet/") // This is a DataFrame

val flights = flightsDF.as[Flight] // Casted DataFrame to DataSet using .as[case class]
```


## Actions

Even though we are using the power of DataSets, what is imortant to know is that we can apply action operations like `collect, take, count` to whether we are using Datasets or DataFrames.
```Java
flights.show(3)

+-----------------+-------------------+-----+
|DEST_COUNTRY_NAME|ORIGIN_COUNTRY_NAME|count|
+-----------------+-------------------+-----+
|    United States|            Romania|    1|
|    United States|            Ireland|  264|
+-----------------+-------------------+-----+

flights.first.DEST_COUNTRY_NAME // United States

```

## Transformations 

In addition to those transformations with DataFrames, Datasets allow us to specify more complex and strongly typed transformations than we could perform on DataFrames alone because we manipulate raw Java Virtual Machine (JVM) types. 


#### Filtering
```Java
def originIsDestination(flight_row: Flight): Boolean = {
  return flight_row.ORIGIN_COUNTRY_NAME == flight_row.DEST_COUNTRY_NAME
}
```

We can now pass this function into the filter method specifying that for each row. It should verify that this function returns true and in the process will filter our Dataset down accordingly

```Java
flights.filter(flight_row => originIsDestination(flight_row)).first()

// Flight = Flight(United States,United States,348113)
```

#### Mapping

Filtering is a simple transformation, but sometimes you need to map one value to another value.
The simplest example is manipulating our Dataset such that we extract one value from each row. This is effectively performing a DataFrame like select on our Dataset. Let’s extract the destination

```Java
val destinations = flights.map(f => f.DEST_COUNTRY_NAME)
```

Notice that we end up with a Dataset of type String. That is because Spark already knows the JVM type that this result should return and allows us to benefit from compile-time checking if, for some reason, it is invalid.

```Java
// We can collect this and get back an array of strings on the driver:

val localDestinations = destinations.take(5)
```

#### Joins

Joins, as we covered earlier, apply just the same as they did for DataFrames. However Datasets also provide a more sophisticated method, the ``joinWith`` method.

joinWith is roughly equal to a co-group (in RDD terminology) and you basically end up with two nested Datasets inside of one.

```Java
//Let’s create a fake flight metadata dataset to demonstrate joinWith:

case class FlightMetadata(count: BigInt, randomData: BigInt)

val flightsMeta = spark.range(500).map(x => (x, scala.util.Random.nextLong)) // the fake data
  .withColumnRenamed("_1", "count").withColumnRenamed("_2", "randomData") // naming columns
  .as[FlightMetadata] // casting as DataSet

```

Now the Join

```Java
val flights2 = flights
  .joinWith(flightsMeta, flights.col("count") === flightsMeta.col("count"))
// querying as DataSet with complex type
flights2.selectExpr("_1.DEST_COUNTRY_NAME")

```

Of course, a “regular” join would work quite well, too, although you’ll notice in this case that we end up with a DataFrame (and thus lose our JVM type information).

```Java
val flights2 = flights.join(flightsMeta, Seq("count"))
```

#### Grouping and Aggregations

Grouping and aggregations follow the same fundamental standards that we saw in the previous aggregation chapter, so ``groupBy rollup and cube`` still apply, but these return ``DataFrames instead of Datasets`` (you lose type information):

```Java
flights.groupBy("DEST_COUNTRY_NAME").count()
```
This often is not too big of a deal, but if you want to keep type information around there are other groupings and aggregations that you can perform. An excellent example is the ``groupByKey`` method.

This function, however, doesn’t accept a specific column name but rather a function. This makes it possible for you to specify more sophisticated grouping functions that are much more akin to something like this:

```Java
flights.groupByKey(x => x.DEST_COUNTRY_NAME).count()
```


After we perform a grouping with a key on a Dataset, we can operate on the Key Value Dataset with functions that will manipulate the groupings as raw objects:

```Java
def grpSum(countryName:String, values: Iterator[Flight]) = {
  values.dropWhile(_.count < 5).map(x => (countryName, x))
}

flights.groupByKey(x => x.DEST_COUNTRY_NAME).flatMapGroups(grpSum).show(5)

+--------+--------------------+
|      _1|                  _2|
+--------+--------------------+
|Anguilla|[Anguilla,United ...|
|Paraguay|[Paraguay,United ...|
|  Russia|[Russia,United St...|
| Senegal|[Senegal,United S...|
|  Sweden|[Sweden,United St...|
+--------+--------------------+

// More
def grpSum2(f:Flight):Integer = {
  1
}

flights.groupByKey(x => x.DEST_COUNTRY_NAME).mapValues(grpSum2).count().take(5)



```

We can even create new manipulations and define how groups should be reduced:

```Java
def sum2(left:Flight, right:Flight) = {
  Flight(left.DEST_COUNTRY_NAME, null, left.count + right.count)
}
flights.groupByKey(x => x.DEST_COUNTRY_NAME).reduceGroups((l, r) => sum2(l, r))
  .take(5)
  
```










