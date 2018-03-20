## Key Concepts

Scala:

- Functions & Evaluation
  - Functional vs. imperative
  - expressions, evaluation, conditionals, functional loops, functions, tuples, tail recursion
- Higher Order Functions
  - functions as first class values, 
  - Scala syntax and how it's formally defined
  - methods, classes, data abstraction thru design of data structures
- Data abstraction
  - traits
  - classes in hierarchies; hierarchy of standard Scala types; polymorphism
  - organize classes and traits into packages
- Types and Pattern Matching
  - functions vs objects (functions are objects)
  - Scala type system details - generics, subtyping, variance, etc.
  - Lists data structure
  - Pattern matching
- Abstractions for concurrency (i.e. futures, semaphores, actors)

Play:

  - Slick functional relational mapper (FRM)
    - Work with data almost as if using Scala collections
    - Full control over when database access happens and what data is transferred
    - Can use SQL directly
    - Database actions are executed asynchronously so good for reactive apps based on Play & Akka
  - Anom
  - EBean
  - Comet
  - WebSocket
  - Kalium cryptography
  - compile time DI

## Learning Steps

### 1: Preliminary Concepts & Scala Foundation

1. JVM necessities: how the JVM works, garbage collection, commands, tools, whats new in the JDK, etc.
1. [Learn in 1 Video](https://www.youtube.com/watch?v=DzFt0YkZo8M)
1. [Fundamentals First (video)](https://www.youtube.com/watch?v=ugHsIj60VfQ)
1. Scala for the Impatient? (book)
1. [Scala exercises](https://www.scala-exercises.org/scala_tutorial/terms_and_types)

### Play 

1. [Play web framework tutorials](https://www.playframework.com/documentation/2.6.x/Tutorials) - including for Slick, JPA, Anom, EBean, Comet, WebSocket, Kalium cryptography, compile time DI
- [Play Hello World tutorial](https://www.playframework.com/documentation/1.3.0-RC1/firstapp) 
  - [Play starter example](https://github.com/playframework/play-scala-starter-example)
1. [Creaeting forms with Play](http://pedrorijo.com/blog/play-forms/#getting-started)

### 2: In-depth Learning

- Coursera Course
- [Get the most out of Scala with Functional Programming in Scala](https://www.manning.com/books/functional-programming-in-scala)

## Other Resources

### Examples 

- [Play, Scala,  REST API Example](https://github.com/playframework/play-scala-rest-api-example)
- [Play, Scala, Slick example](https://github.com/playframework/play-scala-slick-example)

### Other Key Resources

- Martin Odersky's Programming in Scala
  - [Scala By Example (PDF)](http://www.scala-lang.org/docu/files/ScalaByExample.pdf)
