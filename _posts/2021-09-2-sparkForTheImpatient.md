```scala
spark
```


    Intitializing Scala interpreter ...



    Spark Web UI available at http://kindis-mbp:4040
    SparkContext available as 'sc' (version = 3.1.2, master = local[*], app id = local-1631110935759)
    SparkSession available as 'spark'






    res0: org.apache.spark.sql.SparkSession = org.apache.spark.sql.SparkSession@3544bed4




Scala is multiparadigme programing language and object oriented

It is statically typed and is a scalable language (as its name : SCALA)

We can use it as script or to develop applications. It runs in JVM



```scala
val ki="BALDE"
// everything is object in scala
```




    ki: String = BALDE





```scala
//val // declare object as immutable
val a =1*3 
```




    a: Int = 3





```scala
// you can not reasign a value to a since immuatble
a=4

```


    <console>:27: error: reassignment to val

           a=4

            ^

    



```scala
// even when mutable , it is typed
var c=2
```




    c: Int = 2





```scala
c="Kindi"
```


    <console>:26: error: type mismatch;

     found   : String("Kindi")

     required: Int

           c="Kindi"

             ^

    



```scala
// therefore type is not mutable
```


```scala
// everything is class thus object in scala
// no primitive like in java
// semicolumns are optional
// many types : Int, Double, Bytes, Char, Short, Long, Float
```


```scala
1.to(10)
```




    res3: scala.collection.immutable.Range.Inclusive = Range 1 to 10





```scala
"Hello".intersect("World")
```




    res5: String = lo





```scala
// can store big numbers
val x:BigInt=1234567789
```




    x: BigInt = 1234567789





```scala
x*x*x
```




    res8: scala.math.BigInt = 1881675909969356511604390069





```scala
1 to 20
for (i <- 1 to 20) print(i)
```

    1234567891011121314151617181920


```scala
val res=1.to(10)
```




    res: scala.collection.immutable.Range.Inclusive = Range 1 to 10





```scala
// no ++ and --
// but += or -=
```


```scala
// Methods and Functions
```


```scala
import scala.math._
```




    import scala.math._





```scala
sqrt(25)
```




    res14: Double = 5.0





```scala
// function don't operate on object
```


```scala
// But method operate on object
```


```scala
"Hello"(3)
```




    res19: Char = l





```scala
"Hello".apply(4)
```




    res21: Char = o





```scala
for (i <- 1 to 10) print(i)
```

    12345678910


```scala
for ( i <- 1 to 3; j <- 1 to 3 if i!=j) print((10*i +j) + " " )
```

    12 13 21 23 31 32 


```scala
for (i <- 1 to 10) yield i % 3
// with yield we can use it in another object
```




    res26: scala.collection.immutable.IndexedSeq[Int] = Vector(1, 2, 0, 1, 2, 0, 1, 2, 0, 1)





```scala
// FUNCTION
def abs(x:Double) = if (x>=0) x else -x
```




    abs: (x: Double)Double





```scala
// return type is inferred unless the function is recursive
// def fact(n:Int) : Int = if (n<=0) 1 else n*fact(n-1)
// the funct return int
// procedure not have equal
// Named argu usefull
// default argument
// decorator??
// variable number of arg indicated with * 
```


```scala
def sum(args: Int*) = {
     var res=0
     for (arg <- args) res+=arg
     res
     }
```




    sum: (args: Int*)Int





```scala
sum(1,20,45,47 )
```




    res29: Int = 113





```scala
// to check for vowels in a String
def isVowel(ch: Char)=ch=='a' || ch=='i' || ch=='e' || ch=='i' || ch=='o' || ch =='u'
```




    isVowel: (ch: Char)Boolean





```scala
ef isVowel2(ch: Char) = "aeiou".contains(ch)
```


    <console>:32: error: not found: value ef

           ef isVowel2(ch: Char) = "aeiou".contains(ch)

           ^

    <console>:32: error: not found: value ch

           ef isVowel2(ch: Char) = "aeiou".contains(ch)

                       ^

    <console>:32: error: not found: value ch

           ef isVowel2(ch: Char) = "aeiou".contains(ch)

                                                    ^

    



```scala
def vowels(s :String)={for (i <- s) yield i }
```




    vowels: (s: String)String





```scala
def vowels(s: String)={
         var res ="" 
         for (i <- s){
             if (isVowel(i)) res += i }
         res
     }
```




    vowels: (s: String)String





```scala
vowels("Kindi")
```




    res33: String = ii





```scala
def vowels2(s: String)={ 
         for (i <- s if isVowel(i)) yield i
     }
```




    vowels2: (s: String)String





```scala
vowels("Nikaragua")
```




    res35: String = iaaua





```scala
vowels2("Nikaragua")
```




    res34: String = iaaua





```scala
// ARRAYS and ARRAYS BUFFERS
```


```scala
// how to collect stuff
// collecct integer
```


```scala
val nums = new Array[Int](10)
```




    nums: Array[Int] = Array(0, 0, 0, 0, 0, 0, 0, 0, 0, 0)





```scala
// ten integer , all initialized
// without new
val a = Array("Hello", "World")
```




    a: Array[String] = Array(Hello, World)





```scala
a.length
```




    res39: Int = 2





```scala
// the type is infered
// use parenthesis to access element of arrays
a(0)
```




    res40: String = Hello





```scala
// a reassignment
a(0)="Kindi"
```


```scala
a
```




    res44: Array[String] = Array(Kindi, World)





```scala
for (i <- a) println(i)
```

    Kindi
    World



```scala
// printr index
for (i <- 0 until a.length) println(i)
```

    0
    1



```scala
 val b = new Array[Int](10)
```




    b: Array[Int] = Array(0, 0, 0, 0, 0, 0, 0, 0, 0, 0)





```scala
// put some values in the array
for (i <- 0 until b.length) b(i)=i*i
```


```scala
for (i <- b) print((i)+ " ")
```

    0 1 4 9 16 25 36 49 64 81 


```scala
// array buffer like array list inJava
import scala.collection.mutable.ArrayBuffer
```




    import scala.collection.mutable.ArrayBuffer





```scala
val c = new ArrayBuffer[Int]
```




    c: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer()





```scala
// to add new elements use +=
```


```scala
c
```




    res53: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer()





```scala
c+=1
```




    res54: c.type = ArrayBuffer(1)





```scala
// To add a whole array, we use this
c ++= Array(8, 13, 21)
```




    res57: c.type = ArrayBuffer(1, 8, 13, 21, 8, 13, 21)





```scala
for (i <- c) print((i)+ " ")
```

    1 8 13 21 8 13 21 


```scala
// TRNSFORM ARRAY
// like mutate them?
// Transform
val k = new Array[Int](5)
```




    k: Array[Int] = Array(0, 0, 0, 0, 0)





```scala
for ( i <- 1 until k.length -1) k(i)+=i*2
```


```scala
k
```




    res64: Array[Int] = Array(0, 2, 4, 6, 0)





```scala
// create a new variable from it
```


```scala
val res= for (i <- k ) yield 2*i
```




    res: Array[Int] = Array(0, 4, 8, 12, 0)





```scala
// use guard
```


```scala
val res= for (i <- k  if i % 2 ==0 ) yield 2*i
```




    res: Array[Int] = Array(0, 4, 8, 12, 0)





```scala
// op on array
k.sum
```




    res67: Int = 12





```scala
k.max
```




    res68: Int = 6





```scala
k.sorted
```




    res69: Array[Int] = Array(0, 0, 2, 4, 6)





```scala
// give a sorted answer not the original transf.
// toString like in java

```


```scala

```


```scala
// MAP and TUPLE
val scores = Map ("Alice" -> 10, "Bob" -> 3, "Cindy" -> 8 )
```




    scores: scala.collection.immutable.Map[String,Int] = Map(Alice -> 10, Bob -> 3, Cindy -> 8)





```scala
// immutable collection  by  nature
// use this scala.collection.mutable.Map for mutable ones
val scores2 =scala.collection.mutable.Map ( "Cindy" -> 8 )
```




    scores2: scala.collection.mutable.Map[String,Int] = Map(Cindy -> 8)





```scala
// key value pair

scores("Bob")
scores("Alice")
```




    res72: Int = 10





```scala
scores.getOrElse("Fred")
```


    <console>:35: error: not enough arguments for method getOrElse: (key: String, default: => V1)V1.

    Unspecified value parameter default.

           scores.getOrElse("Fred")

                           ^

    



```scala
// create it and give it a value at the same time
scores.getOrElse("Fred", 0)
```




    res77: Int = 0





```scala
scores.getOrElse("Fred", 20)
```




    res74: Int = 20





```scala
scores
```




    res75: scala.collection.immutable.Map[String,Int] = Map(Alice -> 10, Bob -> 3, Cindy -> 8)





```scala
// if mutable you can add some to it
scores2("Kindi")=39
```


```scala
scores2
```




    res81: scala.collection.mutable.Map[String,Int] = Map(Kindi -> 39, Cindy -> 8)





```scala
// you can and remove using 
scores2+=("BALDE" ->39)
```




    res83: scores2.type = Map(Kindi -> 39, BALDE -> 39, Cindy -> 8)





```scala
// you can and remove using 
scores2-=("Cindy")
```




    res85: scores2.type = Map(Kindi -> 39, BALDE -> 39)





```scala
// interate on maps
for ((k,v) <- scores) println(k + " has Score " + v)
```

    Alice has Score 10
    Bob has Score 3
    Cindy has Score 8



```scala
// for pattern matching and loop
// but use for / yield to get a new map
for ((k,v) <- scores) yield (k, v)
// you should get the output in a new var
```




    res92: scala.collection.immutable.Map[String,Int] = Map(Alice -> 10, Bob -> 3, Cindy -> 8)





```scala
// like in java you can get and value collections
```


```scala
scores.keySet
```




    res94: scala.collection.immutable.Set[String] = Set(Alice, Bob, Cindy)





```scala
scores.values
```




    res90: Iterable[Int] = MapLike.DefaultValuesIterable(10, 3, 8)





```scala
// TUPLE
// aggregate values of different type
// aggregate values of different types
// unlike Array tha aggregate values or elmenes of the same type
```


```scala
val t = (1,3, 3.14, "Fred")
```




    t: (Int, Int, Double, String) = (1,3,3.14,Fred)





```scala
// lookup component
t._1
```




    res97: Int = 1





```scala
t._4
```




    res99: String = Fred





```scala
 // Tuple position start with 1
 // Better to use patter matching
```


```scala
// when assigning
val ( _ , second, third, fourth) =t
```




    second: Int = 3
    third: Double = 3.14
    fourth: String = Fred





```scala
val (sth , second, third, fourth) =t
```




    sth: Int = 1
    second: Int = 3
    third: Double = 3.14
    fourth: String = Fred





```scala
// first class citizen
// just like a number
// what can you do with a function? call it, store it, give it to another function

// call

// fun(num) 

// how to give a function to another function

// Array(3.14, 1,34, 2).map(fun) 

// the function is applied to every elment of the array

// you don't need to give each function a name: y =x*2  and not val factor =2; y=x*factor 


// Array(3.14, 1,34, 2).map((x:Double => 3 * x))

// You can use anomimous function

// Array(3.14, 1,34, 2).map(((x:Double) => 3 * x))

// you can store it in a variable

// def triple(x:Double) = 3*x

// Work with Higher order function

// function  eaten a function

// function produce or consume a function
```


```scala
def valeuAtOneQuater(f : (Double) => Double ) = f(0.25)
```




    valeuAtOneQuater: (f: Double => Double)Double





```scala
valeuAtOneQuater(ceil _)
```




    res102: Double = 1.0





```scala
valeuAtOneQuater(sqrt _)
```




    res103: Double = 0.5





```scala
// interesting
// Now function that produce a function
def mulBy(factor: Double) = (x: Double) => factor *x
```




    mulBy: (factor: Double)Double => Double





```scala
 mulBy(3)
```




    res104: Double => Double = $Lambda$2474/0x0000000800f21840@16d4a8a9





```scala
mulBy(3)(4)
```




    res105: Double = 12.0





```scala
//mulBy(3)  returns (x:Double) => 3 * x
// we can produce function with any multiplier
```


```scala
val quintuple = mulBy(5)
```




    quintuple: Double => Double = $Lambda$2474/0x0000000800f21840@53855e34





```scala
quintuple(20)
```




    res107: Double = 100.0





```scala
// scala can deduce type
```


```scala
// valueAtOneQuater((x) => 3*x)
// valueAtOneQuater(x => 3*x)
// valueAtOneQuater( 3* _)

// MAP FILTER REDUCE
// (1 to 9).map(0.1 * _) map a function to each element

// map transform the values

// Filter retains the element that fulfill a predicate
// (1 to 9).filter(_ % 2 ==0)  

// reduceLeft applied a binary function, going from left to right
// (1 to 9).reduceLeft(_  * _)

// map and filter are the same loop yield
// (1 to 9).filter(_ % 2 ==0).map(0.1 * _) is the same as
// for (x <- 1 to 9 if n % 2 ==0) yield 0.1 * n

// concept of closuers
// in the mulBy(3) function 3 is the closure fucntion. It is captured by the function and get stored 

// currying == turning a function that takes two arguments into function that takes one argument
// that function returns a fucntion that consumes the second argument

// def mul(x: Int, y:Int) = x * y and the current version
// def mulOneAtATime(x: Int) = (y: Int) => x * y

//mulOneAtATime(3) is the function y => 3 * y
// mulOneAtATime(3) (14) ==< 42
// mulOneAtATime(3) (14) ==< 42
// mulOneAtATime(3) (14) // 42
```


```scala

```


```scala

```


```scala

```


```scala

```
