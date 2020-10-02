# The Mystery of the Monte Carlo Lock

[Bruce Ediger](mailto:bediger@stratigery.com)

This web page implements some of the various formal systems in Raymond
Smullyan's neglected classic,
[The Lady or The Tiger](http://www.amazon.com/The-Lady-Tiger-Other-Puzzles/dp/048647027X/)

Lots of [web pages](http://archive.ite.journal.informs.org/Vol3No3/ChlondToase/)
exist explaining the titular "Lady or Tiger" problems
in the book,
but [few have bothered to emulate](fttp://heras-gilsanz.com/manuel/smullyan-machines.html)
the Monte Carlo Lock, or any of McCulloch's "logic machines".

[Run Smullyan's machines](mcm.html?raw=true) right now.

## A Curious Number Machine

This JavaScript program emulates McCulloch's Machine as Smullyan
describes in Chapter 9, _A Curious Number Machine_

* For any number **X**, **2X** produces **X**
* For any numbers **X** and **Y**,
* if **X** produces **Y**, **3X** produces the associate of **Y**

The **associate** of any number **X** is **X2X**.

Smullyan uses "number" to mean "string of symbols",
where a symbol is one of '1', '2', '3'... '9'.
This emulated machine is a bit more forgiving of improper inputs than the machine Smullyan describes,
as it will allow '0' (zero) and any single US ASCII letter as a symbol.

## Craig's Law

This JavaScript program emulates McCulloch's Machine with
two additional rules of behavior, as in Chapter 10, _Craig's Law_

* For any number **X**, **2X** produces **X**
* For any numbers **X** and **Y**,
if **X** produces **Y**,
**3X** produces the associate of **Y**, the number **Y2Y**
* For any numbers **X** and **Y**,
if **X** produces **Y**,
**4X** produces the reverse of **Y**
* For any numbers **X** and **Y**,
if **X** produces **Y**,
**5X** produces the repeat of **Y**

The _reverse of a number_ is the lexical reverse of the symbols making up the number.
The number 789 has reverse 987.

The _repeat of a number_ is the number (which is a string)
concatenated with itself: the repeat of 789 is 789789.

## McCulloch's New Machine

Smullyan describes McCulloch's New Machine in Chapter 13, _The Key_
<!-- Self-producing term: 4564245642 -->

* **M-I**: For any number **X**, **2X2** produces **X**
* **M-II**: If **X** produces **Y**, **6X** produces **2Y**
* **M-III**: If **X** produces **Y**, **4X** produces the reverse of **Y**
* **M-IV**: If **X** produces **Y**, **5X** produces **YY**

## Monte Carlo Lock

After spending many chapters on "logic machines",
Smullyan suddenly switches up the syntax.
The Monte Carlo Lock is clearly a formal system akin to the "logic machines",
but the syntax differs, including quoting differently.

### Martin Farkus notes on the lock

* _Property Q_: For any combination **x**, 
the combination **QxQ** is specially related to **x**.

* _Property L_: If **x** is specially related to **y**, 
then **Lx** is specially related to **Qy**.

* _Property V_ (the reversal property: If **x** is specially related to **y**, 
then **Vx** is specially related to the reverse of **y**.

* _Property R_ (the repetition property: If **x** is specially related to **y**, 
then **Rx** is specially related to **yy**.

* _Property Sp_: If **x** is specially related to **y**, 
then if **x** jams the lock, **y** is neutral,
and if **x** is neutral, then **y** jams the lock.

My emulator does not implement _Property Sp_ - it gives no indication of
whether the input combination unlocks or jams the lock or is neutral.
The Monte Carlo Lock must be
an intricate piece of machinery indeed, if it actually does all of _Property Sp_.

That's OK, as _Property Sp_ exits only to motivate Inspector Craig,
the hero of the story.

## Marnix Klooster's Mysterious Monte Carlo Lock

Marnix Klooster wrote a very nice
[derivation](http://home.solcon.nl/mklooster/calc/tlott-8-and-13.html)
of a combination that unlocks the Monte Carlo lock.
His notation is a bit different,
so I include a version of the Monte Carlo lock emulator in Klooster's notation.

Klooster uses <kbd>[3x] = y2y</kbd> to mean the same thing
that Smullyan means when he writes
"if x produces y, 3x produces the associate of y".

* (A)  [2x2] = x
* (B)  [1x] = 2[x]
* (C)  [5x] = <[x]> where &lt;x&gt; means reverse of x
* (D)  [9x] = [x][x]

<!--[More classes of combinations](http://www.100balls.com/Primrose%20Lodge/Playtime/puzzle_32_solution.htm)
that unlock the lock. -->

## Programming Notes

I implemented each of the machines or "locks" as a pretty simple
JavaScript program. They all run in your browser, locally.
Because the only acceptable numbers have a quote character or
digit in them ('2' or 'Q'), the recursion terminates readily.
Either the recursion encounters a quote, or it encounters a digit
that has no rule (not a '2', '3', '4', '5' or '6').

Initially, I found Smullyan's terminology a bit confusing.
He phrased the recursion rules as "If X produces Y, then
MX produces something". Once I realized that you recurse
on the remaining characters in the string until you get
to a quote character, I found it simple to write the
JavaScript implementations.

## Somewhat theoretical considerations

The logical machines and locks comprise simple,
recursive programs,
with a stop condition on a quoted sub-string.
All operations (repeat, reverse, associate)
happen on the way back "up" the recursion.
Does this mean that a [Simple Recursive Function](https://github.com/bediger4000/simple-recursive-functions)
(in the mathematical logic sense) could compute the output of the
various "locks" or "machines" on this page?

Also, is there another, more common name, for the formal
systems that Smullyan describes as "logical machines" in
_The Lady or The Tiger_?
They remind me of [concatenative combinators](https://suhr.github.io/papers/calg.html)
but the correspondence isn't exact.

Mathoverflow features
[a technical explanation](http://mathoverflow.net/questions/13972/shortest-key-for-the-monte-carlo-lock-of-smullyan)
of solutions to the Monte Carlo Lock.
