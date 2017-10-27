# Exercise 1 - Syntax
This exercise will focus on JavaScript syntax.

You will learn about:
 - Semicolons and Automatic Semicolon Insertion (ASI)
 - Linting
 - Curly braces

## Required software and tools for this exercise
- [Google Chrome]()
- A code editor -  [Atom](https://atom.io/) or [VS Code](https://code.visualstudio.com/)

## 1.1 - Semicolons

You may have heard that "semicolons are optional in JavaScript", or that "semicolons are required". Well, both statements are true in some cases.

For example, the following code is perfectly valid JavaScript:

```JavaScript
var x = 1
var y = 2
console.log(x + y)
```

So why does this work? JavaScript has a built in feature called _Automatic Semicolon Insertion (ASI)_. ASI inserts, as the name implies, semicolons in your code automatically before it is run. This means semicolons are technically optional in JavaScript in many cases. However, there are some cases where not using semicolons can lead to problems. So in order to fully understand how this feature works, we will go through the rules.

There are three ASI rules:

### 1.1.1 - ASI Rule #1

Rule #1 from the JavaScript spesification states:
> When, as the program is parsed from left to right, a token (called the offending token) is encountered that is not allowed by any production of the grammar, then a semicolon is automatically inserted before the offending token if one or more of the following conditions is true:
>  - The offending token is separated from the previous token by at least one LineTerminator.
>  - The offending token is }.

What does this mean? Let's break down the terms:
- **Parsing**: The process of converting your code into a _syntax tree_ before compiling it.
- **Token**: A token is a piece of code, for example `var`, `x`, `=`, and so on
- **Offending token**: A piece of code that cannot be logically placed after the previous token
- **Grammar**: The syntax rules that JavaScript follows
- **LineTerminator**: A line break in the code

Let's name each of the conditions of the first rule a and b:

#### ASI Rule 1.a

The first condition of ASI Rule #1 states:
> The offending token is separated from the previous token by at least one LineTerminator.

So, in what cases will this be true? Consider the following code:

```JavaScript
var x = 1
var y = 2
if(x){console.log(x)}
console.log(y)
```

If we read this code from the left to the right, the last token before line 1 ends is `1`.
The next token is on line 2, hence it is separated from the last token by a "LineTerminator", or a line break. The next token is `var`, which is not valid grammar if you place it after the previous token: `1 var`.

Because of this first condition, a semicolon will therefore be inserted at the end of the first line and the second line (because `2 if` is not valid syntax):

```JavaScript
var x = 1;
var y = 2;
if(x>0){y=3}
console.log('hello world')
```

#### ASI Rule 1.b

The second condition of ASI Rule #1 states:
> The offending token is }.

Let's look at the code from after applying rule 1.a:
```JavaScript
var x = 1;
var y = 2;
if(x>0){y=3}
console.log('hello world')
```

Next, the parser reads line 3 from left to right, and encounters a closing curly brace (`}`) after variable assignment `y=3`. `y=3}` is not valid syntax, and the result
is that a semicolon is inserted before the so called _offending token_ (closing curly brace):

```JavaScript
var x = 1;
var y = 2;
if(x>0){y=3;}
console.log('hello world')
```

### 1.1.2 ASI Rule #2

Rule #2 from the JavaScript spesification states:
> When, as the program is parsed from left to right, the end of the input stream of tokens is encountered and the parser is unable to parse the input token stream as a single complete ECMAScript Program, then a semicolon is automatically inserted at the end of the input stream.

Again, let's break down the terms:
- **input stream**: A sequence of text characters representing the code
- **ECMAScript Program**: A piece JavaScript code can be compiled and run

When reading the last line from left to right, the parser encounters the end of the line and the end of the script. A semicolon is therefore inserted at the end of the _input stream_, or after the closing paren in `console.log('hello world')`:
```JavaScript
var x = 1;
var y = 2;
if(x>0){y=3;}
console.log('hello world');
```

### 1.1.3 ASI Rule #3


:pencil2: Open `exercise1.html` both in Chrome and Atom. Make sure changes to the HTML file are reflected in the browser.

[ECMAScript 5.1 spec - ASI rules](http://www.ecma-international.org/ecma-262/5.1/#sec-7.9)

### [Go to exercise 2 ==>](../exercise-2/README.md)
