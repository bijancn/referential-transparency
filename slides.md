<div style="font-size:smaller">
Referential Transparency
========================================================================

<div>
For simple and robust code
-----

<img src="images/functional.png" height="250">

June 8, 2018

Dr. **Bijan** Chokoufe Nejad
</div> <!-- .element: class="fragment" -->

Note:
- Welcome everyone
- Say who you are

</div>

---
Outline
------------------------------------------------------------------------
1. Motivation and Definition

1. Hands-on examples

1. HTTP Proxy - case study

1. Outlook - "but how do I do X?"

---
Disclaimer and further reading
------------------------------------------------------------------------
Most of this talk is directly from <mark>the red book</mark>

("Functional Programming in Scala")

<div id="left">
<img src="images/javascript-the-good-parts.jpg" height="250">
</div>
<div id="right">
<img src="images/scala-the-good-parts.jpg" height="250">
</div>

<br><br>
Code examples will be in Scala but <mark>ideas apply in any language</mark>

Note:
<img src="images/red-book.jpg" height="170">

---
Motivation
------------------------------------------------------------------------
What is `x`, `r1` and `r2` in these examples?

```scala
val x = "Hello"
val r1 = x + " World"
val r2 = x + " World"
```
vs
```scala
val x = new java.lang.StringBuilder("Hello")
val r1 = x.append(" World").toString
val r2 = x.append(" World").toString
```

---
Motivation
------------------------------------------------------------------------
Or what is `x` in this expression?

```scala
var x = 42
x = x + 1
```

<div>
<mark>Equational reasoning</mark> would suggest
```scala
0 = 1
```
???

<mark>
Why do we program like this?
</mark>
</div><!-- .element: class="fragment" -->
<div>
*(Because it works like that on the lowest level)*

<mark>Is there not a cleaner way?</mark>
</div><!-- .element: class="fragment" -->
<div>
*(Yes there is)*
</div><!-- .element: class="fragment" -->

---
What is referential transparency (RT)?
------------------------------------------------------------------------
An expression `e` is <mark>referentially transparent</mark> if, for all programs
`p`, all occurrences of `e` in `p` can be replaced by the result of evaluating
`e` without affecting the meaning of `p`.

We will refer to this also as the <mark>substitution model</mark>.

A function `f` is <mark>pure</mark> if the expression `f(x)` is referentially
transparent for all referentially transparent `x`.

---
An example for the substitution model
------------------------------------------------------------------------
```scala
val x = new java.lang.StringBuilder("Hello")
val r1 = x.append(" World").toString
// 'Hello World'
val r2 = x.append(" World").toString
// 'Hello World World'
```
Let's substitute
<div>
```scala
val r1 = (new java.lang.StringBuilder("Hello")).append(" World").toString
// 'Hello World'
val r2 = (new java.lang.StringBuilder("Hello")).append(" World").toString
// 'Hello World'
```
⚡⚡⚡
</div><!-- .element: class="fragment" -->

*(Normally you don't have to substitute to see if code is RT or not but
formalizing it allows to proof it.)*
<!-- .element: class="fragment" -->

---
What isn't RT?
------------------------------------------------------------------------
- Modifying a variable
- Modifying a data structure in place
- Setting a field on an object
- Throwing an exception or halting with an error
- Printing to the console or reading user input
- Reading from or writing to a file
- Drawing on the screen

Does that mean we can't interact with the real world?

How do we do anything useful?

<div>
**Spoiler Alert**: The idea is to <mark>push side effects to the
boundary</mark> of your program and then handle them there
<mark>explicitly</mark>
</div> <!-- .element: class="fragment" -->

Note:

---
Benefits of RT
------------------------------------------------------------------------
<mark>Pure functions</mark> are
- easier to test
- reuse
- compose
- parallelize
- generalize
- reason about
- much less prone to bugs

<mark>Functional programming (FP)</mark> is the programming paradigm that builds
on the core idea of RT

---
What does that mean for us in practice?
------------------------------------------------------------------------
RT enables <mark>equational reasoning</mark> about programs <br>
through the substitution model ⇨
- You can <mark>refactor confidently</mark> and relentlessly!
- You can easily <mark>reason about code locally</mark> <br>
  as it doesn't change something you cannot see
- Vice versa, you can <mark>reuse code without fearing</mark> to break something

<mark>Combined with a strongly typed language</mark>, the signature of a pure
function already tells you often what it does

---
Bringing it to practice
------------------------------------------------------------------------

---
A coffee shop with side effects
------------------------------------------------------------------------
```scala
object Cafe {
  //@JavaPeople: We don't need semicolons or the return keyword
  def buyCoffee(creditCard: CreditCard): Coffee = {
    val cup = new Coffee()         // new coffee is created
    creditCard.charge(cup.price)   // side effecting call to outside
    cup                            // cup is returned
  }
}
```
Think about how to test this

We don't want to charge actual money every time

Think about how to avoid multiple calls for two coffees

---
A coffee shop without side effects
------------------------------------------------------------------------
We can <mark>separate concerns</mark> (creating the coffee doesn't have to be coupled to
calling the credit card company) and return `Charge`s instead of relying on side
effects

```scala
object Cafe {
  def buyCoffee(creditCard: CreditCard): (Coffee, Charge) = {
    val cup = new Coffee()                // new coffee is created
    (cup, Charge(creditCart, cup.price))  // cup and charge is returned
  }
}

// Can be created without new keyword
case class Charge(creditCard: CreditCard, amount: Double)
```

Enjoy how <mark>trivial</mark> it is <mark>to test</mark> this <mark>without mocking</mark>
```scala
Cafe.buyCoffee(creditCard) shouldBe (coffee, charge)
```

---
Extra benefits
------------------------------------------------------------------------
This allows to write `buyCoffees` that combines the charges to the
same `CreditCard` reusing `buyCoffee`:

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fb2551034a6458e928ca9e9a1744ade50.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=220 width=800></iframe>

<div>
We already used two extremely useful high-level List methods above: `map` (a.k.a.
*homomorphism*) and <br>
`reduce` (similar to `fold` and a.k.a. *catamorphism*).
</div> <!-- .element: class="fragment" -->

---
How to do a loop without a loop
------------------------------------------------------------------------

---
How to do exception handling without exceptions
------------------------------------------------------------------------


---
Summary
------------------------------------------------------------------------

- Referential transparency is the basis for functional programming
- Referential transparency is by definition the only way to make
  <mark>relentless refactoring</mark> possible
- All <mark>side-effects can be wrapped</mark> in typed objects
- Referential transparency forces you to make any assumptions <mark>explicit</mark>
- You can make your code more transparent in any language...

  ...but it's simpler in some then others
  
Note:
- Referential transparency makes <mark>parallel execution</mark> far easier
