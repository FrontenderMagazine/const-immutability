This seems to be a very common misconception that just won’t die. I keep
running into it in blog posts, Twitter discussions, and even books. Here’s my 
attempt at setting things straight.

## `const` creates an immutable *binding*  {#immutable-bindings}

ES6 `const` does **not** indicate that a value is ‘constant’ or immutable. A 
`const` value can definitely change. The following is perfectly valid ES6 code
that does not throw an exception:

    const foo = {};<br>foo.bar = 42;<br>console.log(foo.bar);<br>// → 42

The only thing that’s immutable here is the *binding*. `const` assigns a value
(`{}`) to a variable name (`foo`), and guarantees that no rebinding will happen
. Using an[assignment operator][1] or a [unary][2] or [postfix][3] `--` or `++`
`const` variable throws a `TypeError` exception:

    const foo = 27;<br>// Any of the following uncommented lines throws an exception.<br>// Assignment operators:<br>foo = 42;<br>foo *= 42;<br>foo /= 42;<br>foo %= 42;<br>foo += 42;<br>foo -= 42;<br>foo <<= 0b101010;<br>foo >>= 0b101010;<br>foo >>>= 0b101010;<br>foo &= 0b101010;<br>foo ^= 0b101010;<br>foo |= 0b101010;<br>// Unary `--` and `++`:<br>--foo;<br>++foo;<br>// Postfix `--` and `++`:<br>foo--;<br>foo++;

ES6 `const` has nothing to do with immutability of values.

## So, how to make a value immutable? {#immutable-values}

Primitive values, i.e. numbers, strings, booleans, symbols, `null`, or 
`undefined`, are always immutable.

    var foo = 27;<br>foo.bar = 42;<br>console.log(foo.bar);<br>// → `undefined`

To make an object immutable, use [`Object.freeze()`][4]. It has been around
since ES5 and is widely available nowadays.

    const foo = Object.freeze({<br>	'bar': 27<br>});<br>foo.bar = 42; // throws a TypeError exception in strict mode;<br>              // silently fails in sloppy mode<br>console.log(foo.bar);<br>// → 27

Note that `Object.freeze()` is shallow: object values within a frozen object (i
.e. nested objects) can still be mutated.
[The MDN entry on `Object.freeze()`][4] provides an example `deepFreeze()`
implementation that can be used to make objects fully immutable.

## `const` vs. `let`  {#const-vs-let}

The only difference between `const` and `let` is that `const` makes the
contract that no rebinding will happen.

Everything I wrote here so far are facts. What follows is entirely subjective,
but bear with me.

Given the above, `const` makes code easier to read: within its scope, a `const`
`let` there is no such guarantee. As a result, it makes sense to use `let` and
`const` as follows in your ES6 code:

*   use `const` by default
*   only use `let` if rebinding is needed
*   (`var` shouldn’t be used in ES6)

Do you agree? Why (not)? I’m especially interested in hearing from developers
who prefer`let` over `const` (i.e. even for variables that are never rebound).
If you’re using`let` without rebinding, why are you using `let` in the first
place? Is it because of the
“`const` is for constants” misunderstanding, or is there another reason? Let
me know in the comments!<section>

<legend>Comment on “ES6 `const` is not about immutability”</legend> 
Name *** 

Email *** 

Website 

Your input will be parsed as Markdown.

Spammer? (Enter ‘no’) *** </section>

 [1]: https://tc39.github.io/ecma262/#sec-assignment-operators
 [2]: https://tc39.github.io/ecma262/#sec-unary-operators
 [3]: https://tc39.github.io/ecma262/#sec-postfix-increment-operator

 [4]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze