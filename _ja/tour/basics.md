---
layout: tour
title: 基本
language: ja

discourse: true

partof: scala-tour

num: 2
next-page: unified-types
previous-page: tour-of-scala

redirect_from: "/tutorials/tour/basics.html"
---

このページでは、Scalaの基本を取り扱う。

## Scalaをブラウザで試してみる

ScalaFiddleを利用することでブラウザ上でScalaを実行できる。

1. [https://scalafiddle.io](https://scalafiddle.io)を開く。
2. 左側のパネルに`println("Hello, world!")`を貼り付ける。
3. "Run"ボタンを押すと、右側のパネルに出力が表示される。

このサイトを使えば、簡単にセットアップせずScalaのコードの一部を試すことができる。

このドキュメントの多くのコードの例はScalaFiddleで開発されている。
そのため、サンプルコード内のRunボタンをクリックするだけで、そのまま簡単にコードを試すことができる。

## 式

式は計算可能な文です。

```
1 + 1
```
`println`を使うと、式の結果を出力できる。

{% scalafiddle %}
```tut
println(1) // 1
println(1 + 1) // 2
println("Hello!") // Hello!
println("Hello," + " world!") // Hello, world!
```
{% endscalafiddle %}

### 値

`val`キーワードを利用すれば、式の結果に名前を付けることができる。

```tut
val x = 1 + 1
println(x) // 2
```

ここでいうところの `x` のように、名前をつけられた結果は 値 と呼ばれる。
値を参照した場合、その値は再計算されない。

値は再代入できない。

```tut:fail
x = 3 // この記述はコンパイルされません。
```

値の型は推測可能だが、このように型を明示的に宣言することもできる。

```tut
val x: Int = 1 + 1
```

型定義では`Int` は識別子`x`の後にくることに注意しよう。そして`:`も必要となる。 

### 変数

再代入ができることを除けば、変数は値と似ている。
`var`キーワードを使えば、変数は定義できる。

```tut
var x = 1 + 1
x = 3 // "x"は"var"キーワードで宣言されているので、これはコンパイルされる。
println(x * x) // 9
```

値と同様に、型を宣言したければ、明示的に型を宣言できる。

```tut
var x: Int = 1 + 1
```


## ブロック

`{}`で囲むめば式をまとめることができる。これをブロックと呼ぶ。

ブロックの最後の式の結果はブロック全体の結果にもなる。

```tut
println({
  val x = 1 + 1
  x + 1
}) // 3
```

## 関数

関数は引数を受け取る式です。  
ここでは与えられた数値に1を足す無名関数（すなわち名前が無い関数）を宣言している。

```tut
(x: Int) => x + 1
```
`=>` の左側は引数のリストです。右側は引数を含む式です。

関数には名前をつけることもできる。

{% scalafiddle %}
```tut
val addOne = (x: Int) => x + 1
println(addOne(1)) // 2
```
{% endscalafiddle %}

関数は複数の引数をとることもできる。

{% scalafiddle %}
```tut
val add = (x: Int, y: Int) => x + y
println(add(1, 2)) // 3
```
{% endscalafiddle %}

また引数を取らないこともありえる。

```tut
val getTheAnswer = () => 42
println(getTheAnswer()) // 42
```

## メソッド

メソッドは関数と見た目、振る舞いがとても似ているが、それらには違いがいくつかある。

メソッドは `def` キーワードで定義される。 `def` の後ろには名前、引数リスト、戻り値の型、処理の内容が続く。

{% scalafiddle %}
```tut
def add(x: Int, y: Int): Int = x + y
println(add(1, 2)) // 3
```
{% endscalafiddle %}

戻り値の型は引数リストとコロンの「後」に宣言することに注意してください。`: Int`

メソッドは複数のパラメーターリストを受け取ることができる。

{% scalafiddle %}
```tut
def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
println(addThenMultiply(1, 2)(3)) // 9
```
{% endscalafiddle %}

また、パラメーターリストを一切受け取らないこともある。

```tut
def name: String = System.getProperty("user.name")
println("Hello, " + name + "!")
```
メソッドと関数には他にも違いがあるが、今のところは同じようなものと考えて大丈夫です。

メソッドは複数行の式も持つことができる。
```tut
def getSquareString(input: Double): String = {
  val square = input * input
  square.toString
}
```
メソッド本体にある最後の式はメソッドの戻り値になる。(Scalaには`return`キーワードはあるが、めったに使わない。)

## クラス

`class` キーワードとその後ろに名前、コンストラクタ引数を続けるとクラスを定義できる。

```tut
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit =
    println(prefix + name + suffix)
}
```
`greet` メソッドの戻り値の型は`Unit`です。`Unit`は戻り値として意味がないことを示す。
それはJavaやC言語の`void`と似たような使われ方をする。（`void`との違いは、全てのScalaの式は値を持つ必要があるため、
実はUnit型のシングルトンな値があり、()と書く。その値には情報はありません。）

`new` キーワードを使えば、クラスのインスタンスを生成できる。

```tut
val greeter = new Greeter("Hello, ", "!")
greeter.greet("Scala developer") // Hello, Scala developer!
```

クラスについては[後で](classes.html)詳しく取り扱う。

## ケースクラス

Scalaには"ケース"クラスという特別な種類のクラスがある。デフォルトでケースクラスは不変であり、値で比較される。
`case class` キーワードを利用して、ケースクラスを定義できる。

```tut
case class Point(x: Int, y: Int)
```
ケースクラスは、`new` キーワードなしでインスタンス化できる。

```tut
val point = Point(1, 2)
val anotherPoint = Point(1, 2)
val yetAnotherPoint = Point(2, 2)
```

ケースクラスは値で比較される。

```tut
if (point == anotherPoint) {
  println(point + " and " + anotherPoint + " are the same.")
} else {
  println(point + " and " + anotherPoint + " are different.")
} // Point(1,2) と Point(1,2) は同じです。

if (point == yetAnotherPoint) {
  println(point + " and " + yetAnotherPoint + " are the same.")
} else {
  println(point + " and " + yetAnotherPoint + " are different.")
} // Point(1,2) と Point(2,2) は異なる。
```

ケースクラスについて紹介すべきことはたくさんあり、あなたはケースクラスが大好きになると確信している！
それらについては[後で](case-classes.html)詳しく取り扱う。

## オブジェクト

オブジェクトはそれ自体が定義である単一のインスタンスです。そのクラスのシングルトンと考えることもできる。

`object`キーワードを利用してオブジェクトを定義できる。

```tut
object IdFactory {
  private var counter = 0
  def create(): Int = {
    counter += 1
    counter
  }
}
```

名前を参照してオブジェクトにアクセスできる。

```tut
val newId: Int = IdFactory.create()
println(newId) // 1
val newerId: Int = IdFactory.create()
println(newerId) // 2
```

オブジェクトについては [後で](singleton-objects.html)詳しく取り扱う。

## トレイト

トレイトはいくつかのフィールドとメソッドを含む型です。複数のトレイトを結合することもできる。

`trait`キーワードでトレイトを定義できる。

```tut
trait Greeter {
  def greet(name: String): Unit
}
```

トレイトはデフォルトの実装を持つこともできる。

{% scalafiddle %}
```tut
trait Greeter {
  def greet(name: String): Unit =
    println("Hello, " + name + "!")
}
```

`extends`キーワードでトレイトを継承することも、`override` キーワードで実装をオーバーライドすることもできる。

```tut
class DefaultGreeter extends Greeter

class CustomizableGreeter(prefix: String, postfix: String) extends Greeter {
  override def greet(name: String): Unit = {
    println(prefix + name + postfix)
  }
}

val greeter = new DefaultGreeter()
greeter.greet("Scala developer") // Hello, Scala developer!

val customGreeter = new CustomizableGreeter("How are you, ", "?")
customGreeter.greet("Scala developer") // How are you, Scala developer?
```
{% endscalafiddle %}

ここでは、`DefaultGreeter`は一つのトレイトだけを継承しているが、複数のトレイトを継承することもできる。

トレイトについては [後で](traits.html)詳しく取り扱う。

## メインメソッド

メインメソッドはプログラムの始点になる。Javaバーチャルマシーンは`main`と名付けられたメインメソッドが必要で、
それは文字列の配列を一つ引数として受け取る。

オブジェクトを使い、以下のようにメインメソッドを定義できる。

```tut
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```
