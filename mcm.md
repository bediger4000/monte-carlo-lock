<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML Basic 1.0//EN" "file:///home/bediger/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en-US">
<head>
<meta name="google-site-verification" content="ixC4wtdetOlWNosRSwl6CFgRutGwfbq_9r9z534uRkY" />
<title>The Mystery of the Monte Carlo Lock</title>
<script language="javascript" type="text/javascript">//<![CDATA[
function machine1(x) {
	var y = 'x';

	if (x.length < 2) throw "Too short";

	switch (x.charAt(0)) {
	case '2':
		y = x.substr(1);
		break;
	case '3':
		var z = machine1(x.substr(1));
		y = z + '2' + z;
		break;
	default:
		throw "No rule";
		break;
	}

	return y;
}

function machine2(x) {
	var y = 'x';
	var z;

	if (x.length < 2) throw "Too short";

	switch (x.charAt(0)) {
	case '2':
		y = x.substr(1);
		break;
	case '3':
		z = machine2(x.substr(1));
		y = z + '2' + z;
		break;
	case '4':
		z = machine2(x.substr(1));
		var l = z.length;
		y = '';
		z = z.split("");
		for (var i = 0; i < l; ++i)
			y = z[i].concat(y);
		break;
	case '5':
		z = machine2(x.substr(1));
		y = z + z;
		break;
	default:
		throw "No rule";
		break;
	}

	return y;
}

function machine3(x) {
	var y = 'x';
	var z;

	if (x.length < 2) throw "Too short";

	switch (x.charAt(0)) {
	case '2':  // 2x2 -> x
		var lastchar = x.substr(x.length-1);
		if (lastchar == '2')
			y = x.slice(1, x.length - 1);
		else
			throw "No rule for 2x";
		break;
	case '4':
		z = machine3(x.substr(1));
		var l = z.length;
		y = '';
		z = z.split("");
		for (var i = 0; i < l; ++i)
			y = z[i].concat(y);
		break;
	case '5':
		z = machine3(x.substr(1));
		y = z + z;
		break;
	case '6':
		z = machine3(x.substr(1));
		y = '2' +z;
		break;
	default:
		throw "No rule for " + x.charAt(0);;
		break;
	}

	return y;
}

function compute() {
	try {
		var x = machine1(document.f1.txt1.value);
		document.f1.txt2.value = x;
	}
	catch (e) {
		document.f1.txt2.value = e;
	}
}

function clearInputs() {
	document.f1.txt2.value = "";
	document.f1.txt1.value = "";
}
function swap() {
	document.f1.txt1.value = document.f1.txt2.value;
	document.f1.txt2.value = '';
}
function clearInputs2() {
	document.f2.txt2.value = "";
	document.f2.txt1.value = "";
}
function swap2() {
	document.f2.txt1.value = document.f2.txt2.value;
	document.f2.txt2.value = '';
}
function compute2() {
	try {
		var x = machine2(document.f2.txt1.value);
		document.f2.txt2.value = x;
	}
	catch (e) {
		document.f2.txt2.value = e;
	}
}
function compute3() {
	try {
		var x = machine3(document.mcnm.nin.value);
		document.mcnm.nout.value = x;
	}
	catch (e) {
		document.mcnm.nout.value = e;
	}
}
function clearInputs3() {
	document.mcnm.nout.value = "";
	document.mcnm.nin.value = "";
}
function swap3() {
	document.mcnm.nin.value = document.mcnm.nout.value;
	document.mcnm.nout.value = '';
}
function rev() {
	var inv = document.scratch.txt1.value;
	var l = inv.length;
	var outv = '';
	inv = inv.split("");
	for (var i = 0; i < l; ++i)
			outv = inv[i].concat(outv);
	document.scratch.txt1.value = outv;
}
function assoc() {
	var inv = document.scratch.txt1.value;
	var outv = inv + '2' + inv;
	document.scratch.txt1.value = outv;
}
function repeat() {
	var inv = document.scratch.txt1.value;
	document.scratch.txt1.value = inv + inv;
}
function quot() {
	document.scratch.txt1.value = '2' + document.scratch.txt1.value;
}

function mclock_machine(x) {
	var y = 'WRONG';
	var z;

	if (x.length < 2) throw "Too short";

	switch (x.charAt(0)) {

	case 'Q':  // QxQ -> x
		var lastchar = x.substr(x.length-1);
		if (lastchar == 'Q')
			y = x.slice(1, x.length - 1);
		else
			throw "No rule for Qx";
		break;

	case 'V':
		z = mclock_machine(x.substr(1));
		var l = z.length;
		y = '';
		z = z.split("");
		for (var i = 0; i < l; ++i)
			y = z[i].concat(y);
		break;

	case 'R':
		z = mclock_machine(x.substr(1));
		y = z + z;
		break;

	case 'L':
		z = mclock_machine(x.substr(1));
		y = 'Q' +z;
		break;

	default:
		throw "No rule for " + x.charAt(0);;
		break;
	}

	return y;
}

function computemcl() {
	try {
		var x = mclock_machine(document.mclock.txt1.value);
		document.mclock.txt2.value = x;
	}
	catch (e) {
		document.mclock.txt2.value = e;
	}
}

function clearMclInputs() {
	document.mclock.txt2.value = "";
	document.mclock.txt1.value = "";
}

function klooster_machine(x) {
	var y = 'x';
	var z;

	if (x.length < 2) throw "Too short";

	switch (x.charAt(0)) {
	case '2':  // 2x2 -> x
		var lastchar = x.substr(x.length-1);
		if (lastchar == '2')
			y = x.slice(1, x.length - 1);
		else
			throw "No rule for 2x";
		break;
	case '5':
		z = klooster_machine(x.substr(1));
		var l = z.length;
		y = '';
		z = z.split("");
		for (var i = 0; i < l; ++i)
			y = z[i].concat(y);
		break;
	case '9':
		z = klooster_machine(x.substr(1));
		y = z + z;
		break;
	case '1':
		z = klooster_machine(x.substr(1));
		y = '2' +z;
		break;
	default:
		throw "No rule for " + x.charAt(0);;
		break;
	}

	return y;
}

function computeklooster() {
	try {
		document.klooster.txt2.value
			= klooster_machine(document.klooster.txt1.value);
	}
	catch (e) {
		document.klooster.txt2.value = e;
	}
}

function clearklooster() {
	document.klooster.txt2.value = "";
	document.klooster.txt1.value = "";
}
//]]>
</script>
</head>
<body>
<h1>The Mystery of the Monte Carlo Lock</h1>
<p><a href="mailto:bediger@stratigery.com">Bruce Ediger</a></p>
<p><em>February, 2013</em></p>
<p>
This web page implements some of the various formal systems in Raymond
Smullyan's neglected classic,
<a href="http://www.amazon.com/The-Lady-Tiger-Other-Puzzles/dp/048647027X/">The Lady or The Tiger</a>.
Lots of <a href="http://archive.ite.journal.informs.org/Vol3No3/ChlondToase/">web pages exist explaining the titular
	"Lady or Tiger" problems</a>
in the book, but
<a href="http://heras-gilsanz.com/manuel/smullyan-machines.html">few have bothered to emulate</a>
the Monte Carlo Lock, or any of McCulloch's "logic machines".
</p>
<hr/>
<table border="1">
<tr><td>
<h2>A Curious Number Machine</h2>
<form name="f1" action='nothing'>
<table border="0">
	<tr><td>Input:</td><td><input type="text" name="txt1" /></td></tr>
	<tr><td>Output:</td><td><input type="text" name="txt2" /></td></tr>
</table>
<input type="button" name="btn1" value="Compute" onclick="compute()"/>
<input type="button" name="btn1a" value="Input to scratch" onclick="document.scratch.txt1.value = document.f1.txt1.value;"/>
<input type="button" name="btn1b" value="Scratch to input" onclick="document.f1.txt1.value = document.scratch.txt1.value;"/>
<br/>
<input type="button" name="btn2" value="Clear" onclick="clearInputs()"/>
<br/>
<input type="button" name="btn3" value="Output to input" onclick="swap()"/>
</form>
</td>
<td>
<p>
This JavaScript program emulates McCulloch's Machine as Smullyan
describes in Chapter 9, <em>A Curious Number Machine</em>
</p>
<ol>
	<li>For any number <strong>X</strong>, <strong>2X</strong> produces <strong>X</strong></li>
	<li>For any numbers <strong>X</strong> and <strong>Y</strong>,
	if <strong>X</strong> produces <strong>Y</strong>,
	<strong>3X</strong> produces the associate of <strong>Y</strong></li>
</ol>
<p>
The <em>associate</em> of any number <strong>X</strong> is <strong>X2X</strong>.
Smullyan uses "number" to mean "string of symbols", where a symbol is
one of '1', '2', '3'... '9'.  This emulated machine is a bit more forgiving
of improper inputs than the machine Smullyan describes, as it will allow
'0' (zero) and any single US ASCII letter as a symbol.
</p>
</td>
</tr>
</table>
<hr/>
<table border="1">
<tr><td>
<h2>Craig's Law</h2>
<form name="f2" action='nothing'>
<table border="0">
	<tr><td>Input:</td><td><input type="text" name="txt1" /></td></tr>
	<tr><td>Output:</td><td><input type="text" name="txt2" /></td></tr>
</table>
<input type="button" name="btn1" value="Compute" onclick="compute2()"/>
<input type="button" name="btn1a" value="Input to scratch" onclick="document.scratch.txt1.value = document.f2.txt1.value;"/>
<input type="button" name="btn1b" value="Scratch to input" onclick="document.f2.txt1.value = document.scratch.txt1.value;"/>
<br/>
<input type="button" name="btn2" value="Clear" onclick="clearInputs2()"/>
<br/>
<input type="button" name="btn3" value="Output to input" onclick="swap2()"/>
<br/>
</form>
</td>
<td>
<p>
This JavaScript program emulates McCulloch's Machine with
two additional rules of behavior, as in Chapter 10, <em>Craig's Law</em>
</p>
<ol>
	<li>For any number <strong>X</strong>, <strong>2X</strong> produces <strong>X</strong></li>
	<li>For any numbers <strong>X</strong> and <strong>Y</strong>,
	if <strong>X</strong> produces <strong>Y</strong>,
	<strong>3X</strong> produces the associate of <strong>Y</strong>, the number <strong>Y2Y</strong></li>
	<li>For any numbers <strong>X</strong> and <strong>Y</strong>,
	if <strong>X</strong> produces <strong>Y</strong>,
	<strong>4X</strong> produces the reverse of <strong>Y</strong></li>
	<li>For any numbers <strong>X</strong> and <strong>Y</strong>,
	if <strong>X</strong> produces <strong>Y</strong>,
	<strong>5X</strong> produces the repeat of <strong>Y</strong></li>
</ol>
The <em>reverse of a number</em> is the lexical reverse of the
symbols making up the number.  The number 789 has reverse 987.
<p>
</p>
<p>
The <em>repeat of a number</em> is the number (which is a string)
concatenated with itself: the repeat of 789 is 789789.
</p>
</td>
</tr>
</table>
<hr/>
<h2>Scratch Space</h2>
<form name="scratch" action='nothing'>
	<table border='1'>
	<tr><td>Scratch:</td><td><input type="text" name="txt1" size='75'/></td></tr>
	<tr>
		<td colspan='2' align='center'>
		<input type="button" name="quote" value="quote" onclick="quot();" />
		<input type="button" name="reverse" value="reverse" onclick="rev();" />
		<input type="button" name="associate" value="assoc" onclick="assoc();" />
		<input type="button" name="rpt" value="repeat" onclick="repeat();" />
		<input type="button" name="clr" value="clear" onclick="document.scratch.txt1.value = '';" />
		</td>
	</tr>
	</table>
</form>
<p>
Scratch space exists to help you check your answers. Suppose you want to
solve chapter 10, problem 9, a number that produces the associate of its
own repeat.  In the "Craig's Law" input field, you enter your candidate.
Click "Input to scratch" in the "Craig's Law" box, then click
"repeat" and "assoc" buttons in the "Scratch Space" box to see the
associate of the repeat of your candidate number.
</p>
<p>
The "Scratch Space" tool can also work the other way: use its buttons to
build up a number, then draw that number into an "Input" field before
computing what your number produces.
</p>
<hr/>
<br/>
<hr/>
<table border="0">
<tr>
<td>
<h2>McCulloch's New Machine</h2>
<form name="mcnm" action='nothing'>
<table border="0">
	<tr><td>Input:</td><td><input type="text" name="nin" /></td></tr>
	<tr><td>Output:</td><td><input type="text" name="nout" /></td></tr>
</table>
<input type="button" name="fbtn1" value="Compute" onclick="compute3()"/>
<br/>
<input type="button" name="fbtn2" value="Clear" onclick="clearInputs3()"/>
<br/>
<input type="button" name="fbtn3" value="Output to input" onclick="swap3()"/>
<br/>
</form>
<p>
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
<br/>
<p><em>I renounce all rights - do with this as you will.</em></p>
</body>
</html>
