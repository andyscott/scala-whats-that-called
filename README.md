# What's that called?

What's the name of that operator/symbol/syntax/thing, for Scala.

| Operator       | "Official"          | "Colloquial"  
|----------------|---------------------|---------------
| `==`           | equals              |               
| [`>>=`](#bind) | [bind](#bind)       | flatMap
| `|@|`          | applicative builder | Cinnabon, Macaulay Culkin, home alone, scream, Admiral Ackbar
| `::`           | [cons](#cons)       |
| `_*`           | vararg expansion    |


## <a id="cons"/> `::` cons

```scala
val list1 = List(1, 2, 3)
// > list1: List[Int] = List(1, 2, 3)
val list2 = 1 :: 2 :: 3 :: Nil
// > list2: List[Int] = List(1, 2, 3)
```

## <a id="bind"/> `>>=` bind

```scala
import cats.std.all._
import cats.syntax.flatMap._

List(1, 2, 3) >>= { (x: Int) => List(x, x + 1) }
// > res0: List[Int] = List(1, 2, 2, 3, 3, 4)
```
