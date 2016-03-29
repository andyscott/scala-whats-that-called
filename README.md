# What's that called?

What's the name of that operator/symbol/syntax/thing, for Scala.


### General

| Operator       | "Official"                                  | "Colloquial"  
|----------------|---------------------------------------------|---------------
| `==`           | equals                                      |               
| [`>>=`](#bind) | [bind](#bind)                               | flatMap
| `|@|`          | [applicative builder](#applicative-builder) | Cinnabon, Macaulay Culkin, home alone, scream, Admiral Ackbar
| `::`           | [cons](#cons)                               |
| `_*`           | vararg expansion                            |


### SBT Keys

| Operator                     | What??  
|------------------------------|-------------------------
| [`:=`](#sbt-key-assign)      | assign
| [`+=`](#sbt-key-appends)     | append
| [`++=`](#sbt-key-appends)    | append
| [`~=`](#sbt-key-transform)   | transform
| [`<<=`](#sbt-key-compute)    | compute

--
--

### General Usages

#### <a id="applicative-builder"/> `|@|` applicative builder

Using Cats:
```scala
import cats._
import cats.std.option._
import cats.syntax.cartesian._

(Option(1) |@| Option(2)) map (_ + _)
// > res4: Option[Int] = Some(3)
```

#### <a id="cons"/> `::` cons

In standard Scala:
```scala
val list1 = List(1, 2, 3)
// > list1: List[Int] = List(1, 2, 3)
val list2 = 1 :: 2 :: 3 :: Nil
// > list2: List[Int] = List(1, 2, 3)
```

#### <a id="bind"/> `>>=` bind

Using Cats:
```scala
import cats.std.all._
import cats.syntax.flatMap._

List(1, 2, 3) >>= { (x: Int) => List(x, x + 1) }
// > res0: List[Int] = List(1, 2, 2, 3, 3, 4)
```

--
--

### SBT Key Usages

#### <a id="sbt-key-assign"/> `:=` assign key value

```scala
name := "fooproject"
```

The assignment operator can also be used to compute or transform values using the `.value` macro.

```scala
organization := name.value
// or
organization := { "hello" + name.value }
```

#### <a id="sbt-key-appends"/> `+=` / `++=` append key value

```scala
// appending one item to a key
sourceDirectories in Compile += new File("source")
// appending several items to a key
sourceDirectories in Compile ++= Seq(file("sources1"), file("sources2"))
```
#### <a id="sbt-key-transform"/> `~=` transform key value

```scala
name ~= { _.toUpperCase }
```

#### <a id="sbt-key-compute"/> `<<=` compute key value

```scala
name <<= (name, organization, version) { (n, o, v) => "project " + n + " from " + o + " version " + v }
```
