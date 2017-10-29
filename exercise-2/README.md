# Exercise 2 - Syntax continued

You will learn about:
 - Equality
 - Variables

## Required software and tools for this exercise
- [Atom](https://atom.io/)
- [Node.js](https://nodejs.org)

## 1.1 - Equality

:book: We often need to compare values when programming in JavaScript. JavaScript provides two types of value-comparison operations:
- Loose equality using `==` (also called "double equals")
- Strict equality using `===` (also called "tripe equals")

These operators both takes one operand on the left and right side and returns a boolean.

Consider the following code:
```JavaScript
var a = 1;
var b = '1';

function areEqual(x, y) {
  return x == y;
}

if(areEqual(a, b)) {
  console.log('a and b are equal!');
} else {
  console.log('a and b are not equal :(');
}
```

:question: What do you think the output of this code will be?

:book: The answer is that it will print `a and b are equal!`. To understand this we need to look at the loose equality operator (`==`) does.

Before comparing the two values on each side of the operator, it looks at the data types.

:bulb: JavaScript has seven data types:
- Boolean (`true`, `false`)
- Null (`null`)
- Undefined (`undefined`)
- Number (`3.14`, `1`)
- String (`"Nerdschool rocks"`)
- Object (`{a:1}`, `["apples", "pears", "oranges"]` )
- Symbol (ES6, created via the `Symbol()` function)

:exclamation: If the types are identical, for example a string and a string, the loose equality operator (`==`) just performs the equality comparison. However, if the types are _not identitcal_, it converts both values to a common type _before_ doing the equality comparison.

:book: In the code above, we compare a string and a number (`1` and `"1"`). In this case the second operand (`"1"`) is converted to a number (`1`). `1` equals `1`, so the result is `true`.

This behaviour can have some some potentially unintended effects.

Let's pretend we are going to compare a number and a boolean:
```JavaScript
var a = 0;
var b = false;

function areEqual(x, y) {
  return x == y;
}

if(areEqual(a, b)) {
  console.log('a and b are equal!');
} else {
  console.log('a and b are not equal :(');
}
```

:question: What do you think the output of this code will be?

:pencil2: Try typing it into the `exercise-2.js` file and run it in using Node.js.

:pencil2: Try setting `a` and `b` to the following and see what the output is. Does the result equal what you'd expect?

| A | B |
|---|---|
|'' | 0 |
|0  |'' |
|0  |'0'|
|false   |'false'   |
|false   |'0'   |
|false   |undefined   |
|false   |null   |
|null   |undefined   |
|'\t\r\n'   | 0  |
