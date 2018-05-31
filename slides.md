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

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F6a71a0131926b679dd3ae1b95b1deec7.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=100 width=570></iframe>
<div>
<mark>Equational reasoning</mark> would suggest
`   0 = 1`

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
An expression `e` is <mark>referentially transparent</mark> if, for all programs
`p`, all occurrences of `e` in `p` can be replaced by the result of evaluating
`e` without affecting the meaning of `p`.

We will refer to this also as the <mark>substitution model</mark>.

A function `f` is <mark>pure</mark> if the expression `f(x)` is referentially
transparent for all referentially transparent `x`.

---
An example for the substitution model
------------------------------------------------------------------------
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F195b05b06f507f6c9e39389ff72e2240.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=170 width=770></iframe><br>
Let's substitute
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
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fef32b11b397b98c3b427043c013fba66.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=230 width=770></iframe><br>
Think about <mark>how to test this</mark>:<br>
We don't want to charge actual money every time

Think about <mark>how to avoid multiple calls</mark> for two coffees

What happens when we have <mark>no connection?</mark>

The type signature pretends `Coffee` creation and <br>
payment *never fails* ⇨ <mark>can't be RT</mark>

---
A coffee shop without side effects
------------------------------------------------------------------------
We can <mark>separate concerns</mark> (creating the coffee doesn't have to be coupled to
calling the credit card company) and return `Charge`s instead of relying on side
effects

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F36c4f7ab1ffd5aad9a47dcfa5b0dbda6.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=230 width=770></iframe><br>

Enjoy how <mark>trivial</mark> it is <mark>to test</mark> this <mark>without mocking</mark>
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F43391b7c6b85ecb83df2b408bd38a3b7.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=230 width=770></iframe><br>

---
Extra benefits
------------------------------------------------------------------------
This allows to write `buyCoffees` that combines the charges to the
same `CreditCard` reusing `buyCoffee`:

<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2Fb2551034a6458e928ca9e9a1744ade50.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=220 width=800></iframe>

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

<div id="left">
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F5182658bf2983c418576368ad37696a8.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=170 width=370></iframe>
<br>
`F` can be `List` or `Array` or `Option` or `Future` or anything for which we
can define `map` in an RT way
</div>
<div id="right">
Or graphically
<img src="images/functor.jpg" height="250">
</div>

---
Mapping instead of looping
------------------------------------------------------------------------
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
<iframe src="data:text/html;charset=utf-8,%3Cbody%3E%3Cscript%20src%3D%22https%3A%2F%2Fgist.github.com%2Fbijancn%2F599717764575b8df5f312b3139799786.js%22%3E%3C%2Fscript%3E%3C%2Fbody%3E" height=220 width=500></iframe>

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
Related topics we left out
------------------------------------------------------------------------
- How <mark>typeclasses</mark> and <mark>ad-hoc polymorphism</mark> works in Scala
- <mark>Applicatives & Monads</mark>
- <mark>Strictness & Laziness</mark>
- <mark>Encapsulating any side effects</mark> in RT types (`IO`)
- <mark>Variance</mark> (Covariant, Contravariant and Invariant)
- <mark>Semigroups</mark>

---
Summary
------------------------------------------------------------------------
- <mark>Referential transparency (RT)</mark> is the basis <br>
  for <mark>functional programming</mark>
- RT is by definition the only way to allow for<br>
  <mark>relentless refactoring</mark>
- RT forces you to make any assumptions <mark>explicit</mark>
- You can make your code more transparent in any language...

  ...but it's simpler in some then others 😎
  
Note:
- Referential transparency makes <mark>parallel execution</mark> far easier
- All <mark>side-effects can be wrapped</mark> in typed objects
