# intro-to-functional-scala

## Chapter 1 - Functional programming

### Section 1.1 - Why functional programming?

### Section 1.2 - Getting started

### Section 1.3 - Data Structures

### Section 1.4 - Exceptions without Errors

### Section 1.5 - Handling state

## Chapter 2 - Scala

### Section 2. 1 - Intro

### Section 2. 2 - Using collections

### Section 2. 3 - Pattern matching

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

### Section 2. 4 - Composing functions

### Section 2. 5 - Polymorphism and types

### Section 2.6  - Type Classes

Type Classes are  constructs that allow us to add ad-hoc polymorphism. Type Classes are not a native construct and are not obviously recongnizable but you might have worked with them.

A Type Class is a group of classes that satisfy a contract provided by a trait. Type Classes allow us to add functionality to an existing class without modifying it. Type Classes in scala are composed of three parts
*The type class
*The instances for the various types
*The interface methods that are exposed to the outside world

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
     | 	implicit val stringWriter: JsonWriter[String] = new JsonWriter[String] {
     | 	def write(value: String): Json = JsString(value)
     | 	}
     | 
     | 	implicit val personWriter: JsonWriter[Person] = new JsonWriter[Person] {
     | 		def write(value: Person): Json = 
     | 		JsObject(Map("name" -> JsString(value.name), "address" -> JsString(value.address)))
     | 	}
```
```//Finally the interface methods```
```scala
     | object JsonInterface {
     | 	implicit class JsonWriterOps[A](value: A) {
     | 	def toJson(implicit w: JsonWriter[A]): Json = w.write()
     | 	}
     | }
```

`Putting it all together`
```scala
     | import JsonWriterInstances._
     | import JsonInterface._
     | 
     | Person("Homer", "742 Evergreen Terrace").toJson
```





### Section 2. 6 - Concurrency

### Section 2. 7 - Testing: Scalatest and Specs

### Section 2. 8 - Using SBT

## Chapter 3 - Play MVC Framework

### Section 3.1 - Play Hello World

### Section 3.2 - Play with React

### Section 3.3 - Play with Slick Evolutions

### Section 3.4 - Play with SSE

### Section 3.5 - Play with Websockets

### Compile-time DI

[https://github.com/playframework/play-scala-rest-api-example](https://github.com/playframework/play-scala-rest-api-example)

