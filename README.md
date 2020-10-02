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

[Run Smullyan's machine](mcm.html?raw=true) right now.

** A Curious Number Machine

This JavaScript program emulates McCulloch's Machine as Smullyan
describes in Chapter 9, _A Curious Number Machine_

* For any number *X*, *2X* produces *X*
* For any numbers *X* and *Y*,
* if *X* produces *Y*, *3X* produces the associate of *Y*

The _associate_ of any number *X* is *X2X*.

Smullyan uses "number" to mean "string of symbols",
where a symbol is one of '1', '2', '3'... '9'.
This emulated machine is a bit more forgiving of improper inputs than the machine Smullyan describes,
as it will allow '0' (zero) and any single US ASCII letter as a symbol.

## Craig's Law

This JavaScript program emulates McCulloch's Machine with
two additional rules of behavior, as in Chapter 10, _Craig's Law_

* For any number *X*, *2X* produces *X*
* For any numbers *X* and *Y*,
if *X* produces *Y*,
*3X* produces the associate of *Y*, the number *Y2Y*
* For any numbers *X* and *Y*,
if *X* produces *Y*,
*4X* produces the reverse of *Y*
* For any numbers *X* and *Y*,
if *X* produces *Y*,
*5X* produces the repeat of *Y*

The _reverse of a number_ is the lexical reverse of the symbols making up the number.
The number 789 has reverse 987.

The <em>repeat of a number</em> is the number (which is a string)
concatenated with itself: the repeat of 789 is 789789.

## McCulloch's New Machine

Smullyan describes McCulloch's New Machine in Chapter 13, <em>The Key</em>
<!-- Self-producing term: 4564245642 -->
</p>
<table border="0">
	<tr><td><strong>M-I</strong></td><td>For any number <strong>X</strong>, <strong>2X2</strong> produces <strong>X</strong>.</td></tr>
	<tr><td><strong>M-II</strong></td><td>If <strong>X</strong> produces <strong>Y</strong>, <strong>6X</strong> produces <strong>2Y</strong>.</td></tr>
	<tr><td><strong>M-III</strong></td><td>If <strong>X</strong> produces <strong>Y</strong>, <strong>4X</strong> produces the reverse of <strong>Y</strong>.</td></tr>
	<tr><td><strong>M-IV</strong></td><td>If <strong>X</strong> produces <strong>Y</strong>, <strong>5X</strong> produces <strong>YY</strong>.</td></tr>
</table>
</td>
<td valign="top">
<h2>Monte Carlo Lock</h2>
<form name="mclock" action='nothing'>
<table border="0">
	<tr><td>Input:</td><td><input type="text" name="txt1" /></td></tr>
	<tr><td>Output:</td><td><input type="text" name="txt2" /></td></tr>
</table>
<input type="button" name="btn1" value="Compute" onclick="computemcl()"/>
<br/>
<input type="button" name="btn2" value="Clear" onclick="clearMclInputs()"/>
</form>
<h4>Martin Farkus notes on the lock</h4>
<ul>
	<li><em>Property Q</em>: For any combination <strong>x</strong>, 
	the combination <strong>QxQ</strong> is specially related to <strong>x</strong>.
	</li>
	<li><em>Property L</em>: If <strong>x</strong> is specially related to <strong>y</strong>, 
	then <strong>Lx</strong> is specially related to <strong>Qy</strong>.
	</li>
	<li><em>Property V</em> (the reversal property: If <strong>x</strong> is specially related to <strong>y</strong>, 
	then <strong>Vx</strong> is specially related to the reverse of <strong>y</strong>.
	</li>
	<li><em>Property R</em> (the repetition property: If <strong>x</strong> is specially related to <strong>y</strong>, 
	then <strong>Rx</strong> is specially related to <strong>yy</strong>.
	</li>
	<li><em>Property Sp</em>: If <strong>x</strong> is specially related to <strong>y</strong>, 
	then if <strong>x</strong> jams the lock, <strong>y</strong> is neutral,
	and if <strong>x</strong> is neutral, then <strong>y</strong> jams the lock.
	</li>
</ul>
<p>
My emulator does not implement <em>Property Sp</em> - it gives no indication of
whether the input combination unlocks or jams the lock or is neutral.
The Monte Carlo Lock must be
an intricate piece of machinery indeed, if it actually does all of <em>Property Sp</em>.
</p>
</td>
</tr>
<tr>
	<td>
		<h2>Marnix Klooster's Mysterious Monte Carlo Lock</h2>
<p>
Marnix Klooster wrote a very nice
<a href="http://home.solcon.nl/mklooster/calc/tlott-8-and-13.html">derivation</a>
of a combination that unlocks the Monte Carlo lock.  His notation is a bit different,
so I include a version of the Monte Carlo lock emulator in Klooster's notation.
</p>
<form name="klooster" action='nothing'>
<table border="0">
	<tr><td>Input:</td><td><input type="text" name="txt1" /></td></tr>
	<tr><td>Output:</td><td><input type="text" name="txt2" /></td></tr>
</table>
<input type="button" name="btn1" value="Compute" onclick="computeklooster()"/>
<br/>
<input type="button" name="btn2" value="Clear" onclick="clearklooster()"/>
</form>
		<p>
		Klooster uses <kbd>[3x] = y2y</kbd> to mean the same thing
		that Smullyan means when he writes "if x produces y, 3x produces the associate of y".
		</p>
		<ul>
		<li>(A)  [2x2] = x</li>
		<li>(B)  [1x] = 2[x]</li>
		<li>(C)  [5x] = &lt;[x]&gt;&nbsp;&nbsp;<em>&lt;x&gt; means reverse of x</em></li>
		<li>(D)  [9x] = [x][x]</li>
		</ul>
		<p>
		<a href="http://www.100balls.com/Primrose%20Lodge/Playtime/puzzle_32_solution.htm">More classes of combinations</a>
		that unlock the lock.
		</p>
	</td>
	<td valign="top">
<h2>Programming Notes</h2>
<h4>Implementation</h4>
<p>
I implemented each of the machines or "locks" as a pretty simple
JavaScript program. They all run in your browser, locally.
Because the only acceptable numbers have a quote character or
digit in them ('2' or 'Q'), the recursion terminates readily.
Either the recursion encounters a quote, or it encounters a digit
that has no rule (not a '2', '3', '4', '5' or '6').
</p>
<p>
Initially, I found Smullyan's terminology a bit confusing.
He phrased the recursion rules as "If X produces Y, then
MX produces something". Once I realized that you recurse
on the remaining characters in the string until you get
to a quote character, I found it simple to write the
JavaScript implementations.
</p>
<h4>Somewhat theoretical considerations</h4>
<p>
The logical machines and locks comprise simple, recursive programs,
with a stop condition on a quoted sub-string.  All operations
(repeat, reverse, associate) happens
on the way back "up" the recursion.
Does this mean that a <a href="/srf.html">Simple Recursive Function</a>
(in the mathematical logic sense) could compute the output of the
various "locks" or "machines" on this page?
</p>
<p>
Mathoverflow features <a href="http://mathoverflow.net/questions/13972/shortest-key-for-the-monte-carlo-lock-of-smullyan">a technical explanation</a>
of solutions to the Monte Carlo Lock.
</p>
	</td>
</tr>
</table>
