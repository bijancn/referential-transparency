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

2. HTTP Proxy - case study

3. Bash scripting

4. Outlook - "but how do I do X?"

---
Disclaimer and further reading
------------------------------------------------------------------------
Most of this talk is directly from <mark>the red book</mark>

Code examples will be in Scala but <mark>ideas apply in any language</mark>

---
What is referential transparency (RT)?
------------------------------------------------------------------------
An expression *e* is <mark>referentially transparent</mark> if, for all programs
*p*, all occurrences of *e* in *p* can be replaced by the result of evaluating
*e* without affecting the meaning of *p*.

A function *f* is <mark>pure</mark> if the expression *f(x)* is referentially
transparent for all referentially transparent *x*.

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
Hint: Usually the idea is to <mark>push side effects to the boundary</mark> of
your program and then handle them there <mark>explicitly</mark>
</div> <!-- .element: class="fragment" -->

Note:

---
Benefits of RT
------------------------------------------------------------------------
- <mark>Pure</mark> functions are easier to test, reuse, parallelize, generalize and reason about
- They are also much less prone to bugs

<mark>Functional programming (FP)</mark> is the programming paradigm that allows
you to build solid programs <mark>without side effects</mark>

---
What does that mean for us?
------------------------------------------------------------------------
RT enables equational reasoning about programs ⇨
- You can refactor confidently and relentlessly!
- You can easily reason about code <br> as it doesn't change some state you have to think about

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
