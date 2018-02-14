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

