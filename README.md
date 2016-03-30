# What's that called?

What's the name of that operator/symbol/syntax/thing, for Scala.


### General

| Operator       | "Official"                                  | "Colloquial"  
|----------------|---------------------------------------------|---------------
| `==`           | equals                                      |               
| `>>=`          | [monad bind](#monad-bind)                   | flatMap
| `>>`           | [monad sequence](#monad-sequence)           | followedBy
| `|@|`          | [applicative builder](#applicative-builder) | Cinnabon, Home Alone aka the Macaulay Culkin, Scream, Admiral Ackbar
| `::`           | [cons](#cons)                               |
| `_*`           | vararg expansion                            |


### SBT Keys

| Operator | What??  
|----------|-------------------------
| `:=`     | [assign](#sbt-key-assign)
| `+=`     | [append](#sbt-key-appends)
| `++=`    | [append](#sbt-key-appends)
| `~=`     | [transform](#sbt-key-transform)
| `<<=`    | [compute](#sbt-key-compute)
| `<++=`   | [compute values then append](#sbt-key-compute-then-append)

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

#### <a id="monad-bind"/> `>>=` monad bind

Using Cats:
```scala
import cats.std.all._
import cats.syntax.flatMap._

List(1, 2, 3) >>= { (x: Int) => List(x, x + 1) }
// > res0: List[Int] = List(1, 2, 2, 3, 3, 4)
```

#### <a id="monad-sequence"/> `>>=` monad sequence

This is very similar to a monad bind, except the result of the
first action is discarded.

Using Cats:
```scala
import cats.std.all._
import cats.syntax.flatMap._

List(1, 2, 3) >> { List(2, 2) }
// > res0: List[Int] = List(2, 2, 2, 2, 2, 2)
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

#### <a id="sbt-key-compute"/> `<<=` compute new key value

```scala
name <<= (name, organization, version) { (n, o, v) => "project " + n + " from " + o + " version " + v }
```

#### <a id="sbt-key-compute-then-append"/> `<++=` compute values then append to key

```scala
// val watchSources: TaskKey[Seq[File]] = // ...
watchSources in ConfigGlobal <++= unmanagedSources
```
This is the same thing as:
```scala
watchSources in ConfigGlobal ++= unmanagedSources.value
```
