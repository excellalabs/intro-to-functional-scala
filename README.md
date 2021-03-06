# intro-to-functional-scala

[Curriculum Outline](README-outline.md)

Intro to Functional Scala outline:

- [0.1 Key Concepts](README-outline.md)
- 1.0 Functional Programming
  - [1.1 Why Functional and Scala?](#section-11---why-functional-and-scala)
  - [1.2 Getting Started with Functional](#section-12---getting-started-with-functional)
  - 1.3 Functional Data Structures
  - 1.4 Exceptions without Errors
  - 1.5 Handling State
- 2.0 Scala
  - 2.1 Intro
  - [2.1.1 sbt](readme.md#section-211---using-sbt)
  - 2.2 Using Collections
  - 2.3 Composing functions
  - 2.4 Object Orientation
  - 2.4.2 Polymorphism and types
  - [2.5 Pattern matching](README.md#section-25---pattern-matching)
  - [2.6 Type Classes](README.md#section-26---type-classes)
  - [2.7 Concurrency](README.md#section-27---concurrency)
  - 2.8 Testing - Scalatest and Specs
- 3.0 Play
  - With React
  - With Slick
  - With SSE
  - With Web Sockets
  - With DI

## Chapter 1

### Section 1.1 - Why functional and Scala

What is functional programming & Scala & why use them?

Functional programming treats computation as the evaluation of functions and **avoids changing state and mutable data**. 

It's declarative, using expressions over statements. The **output of a function depends only on the input**, always yielding the same results if called multiple times with the same input (**pure function**). This is in contrast to functions that depend on local or global state, which could have different results each time called **depending on current state** (lacking **referential transparency**), and can have **side effects** which are changes in state that don't depend on the inputs. Thus functional programming can make **much easier to understand and predict the behavior** of an application. Functional programming can be done in other languages such as   Java, but Scala makes it significantly easier.

- Idempotence - a property of a function that says you can apply it many times and get the same result
- Uniform access principle - object.field and object.method have the same syntax (i.e. vs Java where it's object.field and object.method()). Scala best practice says you shouldn't care if dealing with an Option or collection, because all of them can be treated equally in most situations using common higher order functions like `.map, .foreach, .flatMap, .filter, .isEmpty, etc`.

**What makes Scala functional?** [Every function is a value](https://docs.scala-lang.org/tour/unified-types.html) with support for anonymous functions, higher-order functions, allows [functions to be nested](https://docs.scala-lang.org/tour/nested-functions.html), and supports currying. Functions are first-class values in Scala. There are [case classes](https://docs.scala-lang.org/tour/case-classes.html) and built-in support for [pattern matching](https://docs.scala-lang.org/tour/pattern-matching.html) model algebraic types. Singleton objects provide a convenient way to group functions that aren't members of a class. Lends itself to concurrency, mapping, reducing, applying lambda functions.

[Anonymous functions](https://dzone.com/articles/scala-higher-order-and-anonymous-functions) - using a function where it's declared as opposed to declaring it somewhere else first

[Higher-order functions](https://docs.scala-lang.org/tour/higher-order-functions.html) - take other functions as parameters or return a function.  `map` is a common example. Functions passed as arguments are `callback functions`. Works not only on lists but other types of containers like futures.

Closures - functions that carry around their scope; lets you ship code around, i.e. around nodes in a cluster

[Currying](https://docs.scala-lang.org/tour/multiple-parameter-lists.html) - when a method is called with a fewer number of parameter lists than it defined, yielding a function taking the missing parameter lists as its arguments

[Pattern matching](https://docs.scala-lang.org/tour/pattern-matching.html) - a mechanism for checking a value against a pattern. A match can deconstruct the value into parts. More powerful than `switch` and can be used in place of if/else collections.

  * [Exercises](https://www.scala-exercises.org/std_lib/pattern_matching)

[Sequence Comprehensions](https://docs.scala-lang.org/tour/for-comprehensions.html) - generally creating a list based on existing lists, but any datatype with datatype with withFilter, map, and flatMap (with the proper types) can be used.

**Scala is also object-oriented**. [Every value is an object](https://docs.scala-lang.org/tour/unified-types.html) (i.e. functions represented by objects are called function values). Types and behavior of objects are described by classes and traits. Classes are extended by subclassing & a mixin-based composition mechanism as a clean replacement for multiple inheritance. Some say Scala does OO better than Java.

[Traits](https://docs.scala-lang.org/tour/traits.html) - for sharing interfaces and fields betweemn classes (like Java 8 interfaces). Can be extended by classes and objects but cannot be instantiated so have no parameters. Especially useful as generic types and with abstract methods.

[Case classes](https://docs.scala-lang.org/tour/case-classes.html) - like regular classes that have an apply method by default that takes care of object construction, and are compared by structure, not reference. They are good for modeling immutable data.

[Mixin-based composition](https://docs.scala-lang.org/tour/mixin-class-composition.html) - traits used

**Scala is statically typed** and has the following features:

* generic classes
* variance annotations
* upper and lower type bounds,
* inner classes and abstract types as object members
* compound types
* explicitly typed self references
* implicit parameters and conversions
* polymorphic methods

**Extensibility in Scala**

It's easy to add new language constructs in the form of libraries which makes creating DSLs easy.

* [Implicit classes](http://docs.scala-lang.org/overviews/core/implicit-classes.html) - allows adding extension methods to existing types
* [String interpolations](https://docs.scala-lang.org/overviews/core/string-interpolation.html)

### Section 1.2 - Getting started with Functional

* [Get up and running with SBT CLI and an IDE](http://docs.scala-lang.org/getting-started.html)

### Section 1.3 - Functional Data Structures

[Functional Data Structures](https://www.scala-exercises.org/fp_in_scala/functional_data_structures)

### Section 1.4 - Exceptions without Errors

Some functions might fail (i.e. g(2,0), divide by 0). We have no "exceptions" in functional programming (an exception is not a function). How do we solve it?

Solution: Let's allow functions to return two kind of things: instead of having g : Real,Real -> Real (function from two reals into a real), let's allow g : Real,Real -> Real | Nothing (function from two reals into (real or nothing)).

But functions should (to be simpler) return only one thing.

Solution: let's create a new type of data to be returned, a "boxing type" that encloses maybe a real or be simply nothing. Hence, we can have g : Real,Real -> Maybe Real. OK, but ...

What happens now to f(g(x,y))? f is not ready to consume a Maybe Real. And, we don't want to change every function we could connect with g to consume a Maybe Real.

Solution: let's have a special function to "connect"/"compose"/"link" functions. That way, we can, behind the scenes, adapt the output of one function to feed the following one.

In our case:  g >>= f (connect/compose g to f). We want >>= to get g's output, inspect it and, in case it is Nothing just don't call f and return Nothing; or on the contrary, extract the boxed Real and feed f with it. (This algorithm is just the implementation of >>= for the Maybe type). Also note that >>= must be written only once per "boxing type" (different box, different adapting algorithm).

Many other problems arise which can be solved using this same pattern: 1. Use a "box" to codify/store different meanings/values, and have functions like g that return those "boxed values". 2. Have a composer/linker g >>= f to help connecting g's output to f's input, so we don't have to change any f at all.

Remarkable problems that can be solved using this technique are:

having a global state that every function in the sequence of functions ("the program") can share: solution StateMonad.

We don't like "impure functions": functions that yield different output for same input. Therefore, let's mark those functions, making them to return a tagged/boxed value: IO monad.

*FROM: [Why do we need monads?](https://stackoverflow.com/questions/28139259/why-do-we-need-monads/28139260#28139260)

### Section 1.5 - Handling state

## Chapter 2 - Scala

### Section 2.1 - Intro

### Section 2.9 - Using SBT

#### What is SBT?

    Simple Build Tool is a modernized build tool. It is a general purpose build tool that can be used for building any JVM project, but it is written in scala and it has features built out specifically for scala use.
    
##### quick terminology 

    - everything in sbt is either a task or a setting. tasks are things scala does, settings are values you are defining. tasks are scala functions and sbt builds a dependency graph to determine how to execute them.
    
#### How do I use it?

##### Bootstrap a new project (this is recommended standard for scala projects using sbt)
sbt new scala/scala-seed.g8 (creates new scala project from minimal scala template https://www.scala-sbt.org/1.0/docs/sbt-new-and-Templates.html)

this will create a standard directory structure for a minimal scala project, with 
```
build.sbt
src/main/scala/example
src/test/scala/example
project/Dependencies.scala
project/build.properties
```
your build.sbt in here already has scalatest added as a managed dependency.

##### Jump into the sbt shell

- inside of an existing sbt project directory you use _sbt_ or _sbt console_ command and it will load up the sbt shell in interactive mode. This is like a scala REPL but you have access to all project source files and dependencies, you can import them.

##### Use it as a build console (here are common commands ripped mostly from twitter scala school

Common Commands
* run - 
* actions – show actions available for this project
* update – downloads dependencies
* compile – compiles source
* test – runs tests
* package – creates a publishable jar file
* publish-local – installs the built jar in your local ivy cache
* publish – pushes your jar to a remote repo (if configured)
Moar Commands
* test-failed – run any specs that failed
* test-quick – run any specs that failed and/or had dependencies updated
* clean-cache – remove all sorts of sbt cached stuff. Like clean for sbt
* clean-lib – remove everything in lib_managed

##### Dependency Management

- A nice thing about build.sbt is that it is just more scala, so your build configuration is just more of the language you are likely coding in.
- Every entry in build.sbt boils down to a key value pair where the key is defined in Scala.Keys
- SBT supports managed and unmanaged dependencies.

##### Managed Dependencies
(some portions of this taken directly from https://alvinalexander.com/scala/sbt-how-to-manage-project-dependencies-in-scala)

* managed dependencies are dependencies that sbt helps you pull from either the default Maven2 repo or another repo that you have set up a resolver for (more on that later).

* most larger dependencies you pull in you are going to want to treat as managed dependencies, because they will likely have libraries they in turn depend on, which have libraries that they depend on... (transitive dependencies). sbt with the help of ivy sorts all that out for you. 

*adding to your build.sbt* 
the template for adding a managed dependency is:

```
libraryDependencies += groupID % artifactID % revision % configuration
```
* configuration is optional, allows you to profile which dependencies you want to bring in depending on how you are running your scala code.

* SBT defines a minimal DSL for declaring dependencies which is being used in the above template: 

```
+= Appends to the key’s value. The build.sbt file works with settings defined as key/value pairs. In the examples shown, libraryDependencies is a key, and it’s shown with several different values.
% A method used to construct an Ivy Module ID from the strings you supply.
%% When used after the groupID, it automatically adds your project’s Scala version (such as _2.10) to the end of the artifact name.
```

* If you haven't used ivy before, you can check where it is keeping your managed dependencies by looking at 

##### Unmanaged Dependencies
--leaving this to be fleshed out later, cool but we probably won't use them much--

#### Cool Things

    * triggered actions, when you preface a scala task with a tilde e.g. sbt run ~test, it will watch your project source files and perform that action when there is a change, so you can get a quick feedback loop as you develop.
    * sbt uses a lazy dependency management system, sbt doesn't tell ivy to go get dependencies if the configuration hasn't changed unless you force it to with sbt update, https://www.scala-sbt.org/1.x/docs/Dependency-Management-Flow.html 
    * the dependency graph that sbt uses for tasks means that sbt determines which tasks it can run in parallel and which need to be run sequentially, optimizing for a very fast task execution
    
#### helpful links
      
   https://github.com/shekhargulati/52-technologies-in-2016/blob/master/02-sbt/README.md - sbt missing manual, good overview of how to think about sbt and what sbt is doing. 
   https://twitter.github.io/scala_school/sbt.html - the twitter scala code school sbt page. this section pulled a lot from twitter's page, it has an easy to follow tutorial that will get you performing most of the main functions of sbt fast.
   https://www.scala-sbt.org/release/docs/Basic-Def-Examples.html - a set of helpful build.sbt recipes to tweak configuaration settings.
   
#### Other thigns that we could add to SBT seciton
- adding resolvers for other repositories
- multi project builds

### Section 2.2 - Using collections

Sequence comprehensions - syntatic construct for creating a list based on existing lists. `For comprehensions` can enumerate and filter.

### Section 2.4 - Composing functions

### Section 2.3 - [Polymorphism and types](https://docs.scala-lang.org/tour/unified-types.html)

### Section 2.4 - Object orientation

### Section 2.5 - Pattern matching

For those not familiar with pattern matching, think of it as an eloquent, more advanced/useful switch statement. Pattern matching can be used to match more than just primitives. Pattern matching can match on custom types and can even extract from tuples or case classes. In a basic example below, we created the case class "Player", which consists of two fields: a number, and the player name. We created an instance of that class and matched on cases where the player's number is 20 and print the name.

```
case class Player(Number: Int, Name: String)
val runningback = Player(20, "Barry Sanders")
runningback match {
  case Player(20, name) =>
    println(name + " is number 20!")
}
```


Pattern matching can also be a valuable tool when combined with for comprehensions. Below, we created a tuple of two integers, match on it, and yield the sum.


```
val x = (32, 15)

val (a,b) = x

for((a,b) <- Some(x)) yield a+b // The result would be Some(47)
```

This is a basic introduction of Pattern Matching in Scala. For more information, check out the official scala-lang documentation below.

[https://docs.scala-lang.org/tour/pattern-matching.html](https://docs.scala-lang.org/tour/pattern-matching.html)

### Section 2.6 - Type Classes

Type Classes are constructs that allow us to add ad-hoc polymorphism. Type Classes are not a native construct and are not obviously recongnizable but you might have worked with them.

A Type Class is a group of classes that satisfy a contract provided by a trait. Type Classes allow us to add functionality to an existing class without modifying it. Type Classes in scala are composed of three parts
* The type class
* The instances for the various types
* The interface methods that are exposed to the outside world

Consider an ADT that represents JSON structures.



```scala
scala> sealed trait Json
defined trait Json

scala> final case class JsObject(get: Map[String, Json]) extends Json
defined class JsObject

scala> final case class JsString(get: String) extends Json
defined class JsString

scala> final case class JsNumber(get: Double) extends Json
defined class JsNumber

scala> case object JsNull extends Json
defined object JsNull
```
```//Type Class```
```scala
scala> trait JsonWriter[A] {
     |   def write(value: A): Json
     | }
defined trait JsonWriter
```

The type class instances provide implementations for the types we want to add functionality to. Say we want to be able to serialize a case class representing a person.

```//the instance```
```scala
scala> final case class Person(name: String, address: String)
defined class Person

scala> object JsonWriterInstances {
     |   implicit val stringWriter: JsonWriter[String] = new JsonWriter[String] {
     |     def write(value: String): Json = JsString(value)
     |   }
     | 
     |   implicit val personWriter: JsonWriter[Person] = new JsonWriter[Person] {
     |     def write(value: Person): Json =
     |       JsObject(Map("name" -> JsString(value.name), "address" -> JsString(value.address)))
     |   }
     | }
defined object JsonWriterInstances
```
```//Finally the interface methods```
```scala
scala> object JsonInterface {
     | 	 implicit class JsonWriterOps[A](value: A) {
     |       def toJson(implicit w: JsonWriter[A]): Json = w.write(value)
     |     }
     |   }
defined object JsonInterface
```

```//Putting it all together```
```scala
scala> import JsonWriterInstances._
import JsonWriterInstances._

scala> import JsonInterface._
import JsonInterface._

scala> Person("Homer", "742 Evergreen Terrace").toJson
res0: Json = JsObject(Map(name -> JsString(Homer), address -> JsString(742 Evergreen Terrace)))
```

### Section 2.7 - Concurrency

Scala's approach makes it much easier to reason about concurrency and write well-behaving programs than lower-level APIs you often see in other languages. The two cornerstones to concurrency in Scala are `Futures` and `Actors`. 

#### Futures and Promises

##### Futures

A future is a placeholder object for a value that may not exist yet becuase of an async operation that hasn't yet completed. Threads cooperate, via a future. The Future will eventually contain either the result or an exception in its monad.

* Callbacks populate the future with the actual value when it's ready. 
* The execution to get said value happens in an `ExecutionContext` - similar to an `Executor`, it can execute computations in a new, pooled or the current (discouraged) thread. 
* Futures are completed when they get the value back from the computation, whether that's the expected value or an exception thrown from it, and that result is immuatable. They are generally for async operations but can block when necessary.

`Future[T]` is a type which denotes future objects, whereas `Future.apply` is a method which creates and schedules an asynchronous computation, and then returns a future object which will be completed with the result of that computation.

Futures provide a great at interface for client (main thread), but less good for what producer of a result could do. Own implementation needed. Scala, when creating task, an additional abstraction provides mediation b/t client and server (like Java 8 completeable futures, influenced by Scala's attempt to create their own).

EXAMPLE: Let’s assume that we want to use a hypothetical API of some popular stock service to get stock quotes and buy if a good deal. We will open a new connection and send a request to obtain a list:

It is made async with a future, and the following lines handle the results, however this way is not ideal which we will explore next:

```scala
import scala.concurrent._
import ExecutionContext.Implicits.global

val connection = ...

val rateQuote = Future {
  connection.getCurrentValue(USD)
}

rateQuote onSuccess { case quote =>
  val purchase = Future {
    if (isProfitable(quote)) connection.buy(amount, quote)
    else throw new Exception("not profitable")
  }

  purchase onSuccess {
    case _ => println("Purchased " + amount + " USD")
  }
  case Failure(t) => println("An error has occured: " + t.getMessage)
}
```

* Importing `scala.concurrent` brings in `Future` and the 2nd import brings in the default execution context.

##### Mapping Futures

The problems with this approach are 
* nested callbacks are required when doing a subsequent action in the onComplete of the first
* operation & result is limited within that scope. 

For these reasons, futures provide **combinators** such as `map`, which allow a more straightforward composition. `map` will produce a new future with the value mapped from the original when it's completed (not dissimilar to mapping collections).

The above can be done using `map` to eliminate the nesting and one onSuccess callback:

```scala
val rateQuote = Future {
  connection.getCurrentValue(USD)
}

val purchase = rateQuote map { quote =>
  if (isProfitable(quote)) connection.buy(amount, quote)
  else throw new Exception("not profitable")
}

purchase onSuccess {
  case _ => println("Purchased " + amount + " USD")
}
```

##### Combinators on Futures

Exceptions: 
* If the mapping function throws an exception the future is completed with that exception
* If the original future fails with an exception then the returned future also contains the same exception 

This exception propagating semantics is present in the rest of the combinators, as well. They are designed to work with `for-comprehensions` also, so futures contain `flatMap`, `filter` and `foreach` combinators.

`flatMap` is not typically used outside of `for-comprehensions`, and `filter` creates a new future with the original's value only if it satisfies some predicate, else failing with a `NoSuchElementException`.

Usually you'd want to use a `for-comprehension` over a `flatMap` because it **reads cleaner**:

```scala
val acceptable: Future[Boolean] = for {
  heatedWater <- heatWater(Water(25))
  okay <- temperatureOkay(heatedWater)
} yield okay
```

If you have multiple computations you can create the Future outside of the for-comprehension. However, this is just like nested flatMap calls, which will run sequentially:

```scala
def prepareCappuccinoSequentially(): Future[Cappuccino] = {
  for {
    ground <- grind("arabica beans")
    water <- heatWater(Water(20))
    foam <- frothMilk("milk")
    espresso <- brew(ground, water)
  } yield combine(espresso, foam)
}
```

To run in parallel, instantiate all your independent Futures before the for-comprehension:

```scala
def prepareCappuccino(): Future[Cappuccino] = {
  val groundCoffee = grind("arabica beans")
  val heatedWater = heatWater(Water(20))
  val frothedMilk = frothMilk("milk")
  for {
    ground <- groundCoffee
    water <- heatedWater
    foam <- frothedMilk
    espresso <- brew(ground, water)
  } yield combine(espresso, foam)
}
```

It is non-deterministic which what order the 3 futures will run, though it is guaranteed brew will run last.

##### Exception combinators

Combinators to handle future exceptions include `recover`, which maps the exception to a new future's value. `recoverWith` is available and is similar to how `map` relates to `flatMap`. 

`fallbackTo` will try to populate a new future with the value of a fallback function given the first threw an exception. For example:

```scala
val usdQuote = Future {
  connection.getCurrentValue(USD)
} map {
  usd => "Value: " + usd + "$"
}
val chfQuote = Future {
  connection.getCurrentValue(CHF)
} map {
  chf => "Value: " + chf + "CHF"
}

val anyQuote = usdQuote fallbackTo chfQuote

anyQuote onSuccess { println(_) }
```

`andThen` can be used just for side effect purposes, and can be chained and executed in order, essentially creating a new future as a copy of the original.

Projections can be used to enable for-comprehensions on a result returned as an exception.

Combinators are purely functional - they each return a new future which is related to the one it was derived form.

##### Promises

A promise is similar to a future but is writable (CompletableFuture), so can be kept by the issuer explicitly as it has a public setter. It's a way for other parts of the program to put a value in the Future. 

Working with Promises is more flexible. Can register multiple callbacks. Object between producer and consumer.

Where Future provides an interface exclusively for querying, Promise is a companion type that allows you to complete a Future by putting a value into it exactly once. It cannot be changed after it completes.

A Promise instance is always linked to exactly one Future. There is obviously no way to complete a Future other than through a Promise – the apply method on Future is just a nice helper function that shields you from this.

When talking about promises that may be fulfilled or not, an obvious example domain is that of politicians, elections, campaign pledges, and legislative periods.

Suppose the politicians that then got elected into office promised their voters a tax cut. This can be represented as a Promise[TaxCut], which you can create by calling the apply method on the Promise companion object, like so:

```scala
import concurrent.Promise
case class TaxCut(reduction: Int)
// either give the type as a type parameter to the factory method:
val taxcut = Promise[TaxCut]()
// or give the compiler a hint by specifying the type of your val:
val taxcut2: Promise[TaxCut] = Promise()
```

Once you have created a Promise, you can get the Future belonging to it by calling the future method on the Promise instance:

```scala
val taxcutF: Future[TaxCut] = taxcut.future
```

The returned Future might not be the same object as the Promise, but calling the future method of a Promise multiple times will definitely always return the same object to make sure the one-to-one relationship between a Promise and its Future is preserved.

**Completing a promise**

In Scala, you can complete a Promise either with a success or a failure:

```taxcut.success(TaxCut(20))```

Once you have done this, 
* that Promise instance is no longer writable, and future attempts to do so will lead to an exception
* completing your Promise leads to the completion of the associated Future
* Any success or completion handlers on that future will now be called, or the mapping functions if you are mapping it
* Usually, the completion of the Promise and the processing of the completed Future will not happen in the same thread. It’s more likely that you create your Promise, start computing its value in another thread and immediately return the uncompleted Future to the caller.

**Failure**

If that happens, you can still complete your Promise instance gracefully, by calling its failure method and passing it an exception:

```scala
case class LameExcuse(msg: String) extends Exception(msg)
object Government {
  def redeemCampaignPledge(): Future[TaxCut] = {
       val p = Promise[TaxCut]()
       Future {
         println("Starting the new legislative period.")
         Thread.sleep(2000)
         p.failure(LameExcuse("global economy crisis"))
         println("We didn't fulfill our promises, but surely they'll understand.")
       }
       p.future
     }
}
```

* If you already have a Try, you can also complete a Promise by calling its complete method

##### Conclusion

* You should design your application with concurrency from the ground up, 
  * your functions in all your application layers are asynchronous 
  * return futures

A likely use case these days is that of developing a web application. If you are using a modern Scala web framework, it will allow you to return your responses as something like a Future[Response] instead of blocking and then returning your finished Response. This is important since it allows your web server to handle a huge number of open connections with a relatively low number of threads. By always giving your web server Future[Response], you maximize the utilization of the web server’s dedicated thread pool.

A service in your application might make multiple calls to your database layer and/or some external web service, receiving multiple futures, and then compose them to return a new Future

3 different cases to consider:

* Non-blocking IO
* Blocking IO
* Long running computations - no IO, CPU-bound. They shouldn't be executed in the web server thread either so turn into Futures and thus in a separate ExecutionContext.

More reading: [Scala Docs - Promises & Futures](https://docs.scala-lang.org/overviews/core/futures.html)

##### EXERCISE

// 1. Import that scala.concurrent package
import scala.concurrent.Future

// 2. execute async
val f1 = Future { Thread.sleep(1000); 50 } 

// Who is going to provide the execution context?
import scala.concurrent.ExecutionContext.Implicits.global

// 3. Try failed line again

// Imported Implicits class. Let's compiler find a value that fits a particular need.
// Futures need an ExecutionContext, if one exists the compiler can find it.
// Notice it not being there didn't call a compiler error, showing the flexibility of implicits. USE SPARINGLY, beginners error to overuse them - can create very hard to maintain code that is hard to see why it works

// 4. A Future is like Option or Try, it may hold a result or an exception

f1.foreach(n => println(n))

val f2 = Future { Thread.sleep(1000); 1/0 }

// Nothing good in here, nothing returned thus no blocking
f1.foreach(n => println(n))

// The exception in the Future
f2.value

// 5. Future gives us 3 states - succeeded, failed, hasn't finished in a non-blocking & functional way

// Convert into Future that worked but has Throwable
val ff = f2.failed 

// prints message from exception
ff.foreach(n => println(n))

// returns another future
val f2 = f1.map(n => n * 2) 

// 6. When f1 finishes, run this on result, a chain. 

val f3 = f1.filter(n => n > 20)

// Map & filter are good for piping
// Often used with for comprehensions by using with a Promise.

### Section 2.8 - Testing: Scalatest and Specs

## Chapter 3 - Play MVC Framework

### Section 3.1 - Play Hello World

### Section 3.2 - Play with React

### Section 3.3 - Play with Slick Evolutions

### Section 3.4 - Play with SSE

### Section 3.5 - Play with Websockets

### Compile-time DI

[https://github.com/playframework/play-scala-rest-api-example](https://github.com/playframework/play-scala-rest-api-example)

