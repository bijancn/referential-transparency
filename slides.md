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

1. Hands-on Examples and Utilities

1. HTTP Proxy as Case Study for Exception Handling

1. Outlook and Summary

---
Disclaimer and further reading
------------------------------------------------------------------------
A lot of this talk is directly from <mark>the red book</mark>

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

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F2efd68921433fa90e959722834d11223.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=130 width=370></iframe>
<br>
vs
<br>
<br>
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Ff3b4dfe9f0b05c955d6dc65cd9a29f0d.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=130 width=570></iframe>

---
Motivation
------------------------------------------------------------------------
Or what is `x` in this expression?

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F6a71a0131926b679dd3ae1b95b1deec7.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=110 width=570></iframe>
<div>
<mark>Equational reasoning</mark> would suggest
`   0 = 1`<br><br>

<div id="left">
  <img src="images/y-u-program-like-this.jpg" height="250">
</div><!-- .element: class="fragment" -->
<div id="right">
  *(Because it works like that on the lowest level)*

<div>
  <mark>Is there not a cleaner way?</mark>

  *(Yes there is)*
</div><!-- .element: class="fragment" -->
</div><!-- .element: class="fragment" -->
</div><!-- .element: class="fragment" -->

---
What is referential transparency (RT)?
------------------------------------------------------------------------
<br>
An expression `e` is <mark>referentially transparent</mark> if, for all programs
`p`, all occurrences of `e` in `p` can be replaced by the result of evaluating
`e` without affecting the meaning of `p`.
<br>
<br>

We will refer to this also as the <mark>substitution model</mark>.
<br>
<br>

A function `f` is <mark>pure</mark> if the expression `f(x)` is referentially
transparent for all referentially transparent `x`.

---
An example for the substitution model
------------------------------------------------------------------------
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F195b05b06f507f6c9e39389ff72e2240.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=170 width=770></iframe><br>
Let's try to substitute
<div>
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fe937a868ec7dcdafe615e97f24f16399.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=150 width=770></iframe><br>
⚡⚡⚡
</div><!-- .element: class="fragment" -->

*(Normally you don't have to substitute to see if code is RT or not but
formalizing it allows to proof it.)*
<!-- .element: class="fragment" -->

---
What isn't RT?
------------------------------------------------------------------------
- <mark>Modifying</mark> a variable
- <mark>Modifying</mark> a data structure in place
- <mark>Setting</mark> a field on an object
- <mark>Throwing</mark> an exception or halting with an error
- `Printing` to the console or reading user input
- `Reading` from or writing to a file
- `Drawing` on the screen

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
- generalize
- parallelize
- reason about locally
- much less prone to bugs
<br><br>

<mark>Functional programming (FP)</mark> is the programming paradigm
that builds on the <mark>core idea of RT</mark>

---
What else does that mean for us in practice?
------------------------------------------------------------------------
RT enables <mark>equational reasoning</mark> about programs <br>
through the substitution model ⇨

<div>
You can <mark>refactor confidently</mark> and relentlessly!

<mark>Combined with a strongly typed language</mark>, the signature of a pure
function already tells you often what it does
</div> <!-- .element: class="fragment" -->

---
Bringing it to practice
------------------------------------------------------------------------

---
A coffee shop with side effects
------------------------------------------------------------------------
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fef32b11b397b98c3b427043c013fba66.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=240 width=770></iframe>
<div>
Think about <mark>how to test this</mark>:<br>
We don't want to charge actual money every time

Think about <mark>how to avoid multiple calls</mark> for two coffees
</div> <!-- .element: class="fragment" -->
<div>
What happens when we have <mark>no connection?</mark>

The type signature pretends `Coffee` creation and <br>
payment *never fails* ⇨ <mark>can't be RT</mark>
</div> <!-- .element: class="fragment" -->

---
A coffee shop without side effects
------------------------------------------------------------------------
We can <mark>separate concerns</mark> (creating the coffee doesn't have to be coupled to
calling the credit card company) and return `Charge`s instead of relying on side
effects

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F36c4f7ab1ffd5aad9a47dcfa5b0dbda6.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=250 width=770></iframe><br>

<div>
Enjoy how <mark>trivial</mark> it is <mark>to test</mark> this <mark>without mocking</mark>
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F43391b7c6b85ecb83df2b408bd38a3b7.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=250 width=770></iframe><br>
</div> <!-- .element: class="fragment" -->

---
Extra benefits
------------------------------------------------------------------------
<mark>Pushing side effects to the boundary</mark> <br>
<mark>makes the core more composable</mark>

E.g. it allows to write `buyCoffees` that combines the charges to the
same `CreditCard` reusing `buyCoffee`:

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fb2551034a6458e928ca9e9a1744ade50.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=210 width=800></iframe>
<div>
We already used two extremely useful high-level List methods above: `map` (~
*homomorphism*) and <br>
`reduce` (similar to `fold` and ~ *catamorphism*).
</div> <!-- .element: class="fragment" -->

---
Mapping instead of looping
------------------------------------------------------------------------
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fa387cbdd838c35a04cb1b7be55710df7.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=300 width=500></iframe>

<div style="font-size:3em">
🙈
</div>

---
Mapping instead of looping
------------------------------------------------------------------------
`map` is *the way* to transform values of one category to another
<br><br>

<div id="left">
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F5182658bf2983c418576368ad37696a8.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=170 width=370></iframe>
<br>
`F` can be `List` or `Array` or `Option` or `Future` or anything for which we
can define `map`
</div>
<div id="right">
Or graphically
<img src="images/functor.jpg" height="250">
</div>

---
Mapping instead of looping
------------------------------------------------------------------------
<br><br>
<div id="left">
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F081b0482fd6287eb42ea8308f6d7dc80.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=150 width=400></iframe>
<br>
<br>
No off-by-one errors,<br>
run-away loop or <br>
`IndexOutOfBoundsException` <mark>possible</mark>
</div>
<div id="right">
<img src="images/add-three.png" height="250">
<br>
<br>
</div>
<div style="font-size:3em">
👌
😎
</div>

---
Folding instead of looping
------------------------------------------------------------------------
<div id="left">
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fa397f5d2ab7399c9a7f8c10cecb63ae8.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=320 width=800></iframe>

<div style="font-size:3em">
🙈
</div>
</div>
<div id="right">
<img src="images/doge-code.png" height="500">
</div> <!-- .element: class="fragment" -->

---
Folding instead of looping
------------------------------------------------------------------------
`fold` is the generic way to go from one data structure to a summary value

<div id="left">
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F327129af054fbfc80bfb5db9ebebd73d.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=220 width=500></iframe>
`A` and `B` can also be the same
</div>
<div id="right">
<img src="images/left-fold-transformation.png" height="300">
</div>

---
Folding instead of looping
------------------------------------------------------------------------
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F599717764575b8df5f312b3139799786.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=240 width=500></iframe>

No more loop mechanics or mutable state involved 
👌

<div>
Still not really expressive. 
There might be more cases where we <br>
<mark>check predicates to reduce</mark>:
<div id="left">
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F4fc90db522c4b12a8f69ba3c5683e390.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=220 width=500></iframe>
</div>
<div id="right">
<div style="font-size:2em">
😎
</div>
</div>
</div> <!-- .element: class="fragment" -->

---
Folding instead of looping
------------------------------------------------------------------------
`Foldables` have a range of similar semantics that can be implemented when a
datatype has `fold` <br>(omitting `(fa: F[A])`)
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F537da894e118c04e9a24c532786ad8d0.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=300 width=500></iframe>

---
In other words
------------------------------------------------------------------------
<img src="images/MapFilterReduce.jpg" height="500">


---
How to do exception handling without exceptions
------------------------------------------------------------------------

---
Let's create an HTTP proxy
------------------------------------------------------------------------
<div id="left" style="width: 45%">
Bob 👨‍🎓
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F58e5500ffcb7b89f69fad9b10c22797b.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=150 width=500></iframe>
Easy, right? Let's party!<br> 🍺 🏖️
</div>
<div id="right" style="width: 55%">
Alice 👩‍💻

<div id="code-bad">
loadNumberOfItems("foo.bar") shouldBe a [Int]
<br>
// InvalidUrlException
<br>
loadNumberOfItems("http://google.com") shouldBe a [Int]
<br>
// InvalidPayloadException
<br>
loadNumberOfItems("http://actual.server.endpoint.com/number") shouldBe a [Int]
<br>
// ConnectionException
<br>
// ParsingException
<br>
// ClassCastException
</div>
💥 😠 💥
</div> <!-- .element: class="fragment" -->

---
Back to the drawing board
------------------------------------------------------------------------
- Represent <mark>failures</mark> or <mark>partiality</mark> with
    <mark>ordinary values</mark>
- The return <mark>type</mark> should indicate what can go wrong, so the compiler can
    remind us to handle all cases
- Think `C` <mark>return codes</mark> on steroids
- Trying to use <mark>sentinal values</mark> like `NaN` might not be an option, allow
    silent propagation and result in nongeneric boilerplate

---
Either or Option
------------------------------------------------------------------------
There are two common, minimal wrappers <br>to indicate <mark>possible failures</mark>:
<br>
<br>

`Option` is like a list with at most one element
- `Option` is either `None` or `Some(x)`
- `map` on an Option adds the next computation step when there is
    `Some`thing
- `map` allows us <mark>lift</mark> any function that goes `A => B` to a
  function `Option[A] => Option[B]`. Thus we don't have to entangle arguments
  with `Option`
<br>
<br>
<br>

`Either` is like `Option` but instead of `None` it allows to add
<mark>what went wrong</mark>

---
Let's create an HTTP proxy
------------------------------------------------------------------------
Bob 👨‍🎓 😩
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F7127b21eaf06280d617d24173dd0e8a7.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=150 width=500></iframe>

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fe93151bac653cb3b0306dd515952d699.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=350 width=800></iframe>

---
Let's create an HTTP proxy
------------------------------------------------------------------------
👨‍🎓  👩‍💻
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F29e4d76c3987f5713a430ebf6f7948fc.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=250 width=800></iframe>
<div id="code-good">
loadNumberOfItems(&quot;foo.bar&quot;).unsafeRunSync
<br>
&nbsp;&nbsp;shouldBe Left(InvalidUrl) 
<br>
loadNumberOfItems(&quot;http://google.com&quot;).unsafeRunSync
<br>
&nbsp;&nbsp;shouldBe Left(InvalidJson)
<br>
loadNumberOfItems(&quot;http://actual.server.endpoint.com/number&quot;).unsafeRunSync
<br>
&nbsp;&nbsp;shouldBe Right(42)
</div>

😎 🍾 🥂

---
Related topics we left out
------------------------------------------------------------------------
- <mark>Applicatives & Monads</mark>
- <mark>Semigroups</mark>
- <mark>Strictness & Laziness</mark>
- <mark>Encapsulating any side effects</mark> in RT types (`IO`)
<br>
<br>

- How <mark>typeclasses</mark> and <mark>ad-hoc polymorphism</mark> works in Scala
- <mark>Variance</mark> (Covariant, Contravariant and Invariant)

---
Summary
------------------------------------------------------------------------
- <mark>Referential transparency (RT)</mark> is the basis <br>
  for <mark>functional programming</mark>
- RT is by definition the only way to allow for<br>
  <mark>relentless refactoring</mark>
- RT forces you to make any assumptions <mark>explicit</mark>
- FP allows you to express anything in an RT way
- You can make your code more transparent in any language...

  ...but it's simpler in some then others 😎
  
Note:
- Referential transparency makes <mark>parallel execution</mark> far easier
- All <mark>side-effects can be wrapped</mark> in typed objects
