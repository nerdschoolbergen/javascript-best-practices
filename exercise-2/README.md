# Exercise 2 - Equality

You will learn about:
 - Equality
 - Truthy/falsy values

## Required software and tools for this exercise
- [Atom](https://atom.io/)
- [Node.js](https://nodejs.org)

## 1.1 - Equality

:book: We often need to compare values when programming in JavaScript. JavaScript provides two types of value-comparison operations:
- Loose equality using `==` (also called "double equals")
- Strict equality using `===` (also called "triple equals")

These operators both takes one operand on the left and right side and returns a boolean.

## 1.1.1 - Loose equality

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

:exclamation: If the types are identical, for example a string and a string, the loose equality operator (`==`) just performs the equality comparison. However, if the types are _not identical, it converts both values to a common type _before_ doing the equality comparison.

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

|`a`|`b`|
|---|---|
|`''`| `0`|
|`0`|`''`|
|`0`|`'0'`|
|`false`|`'false'`|
|`false`|`'0'`|
|`false`|`undefined`|
|`false`|`null`|
|`null`|`undefined`|
|`'\t\r\n'`|`0`|
|`[]`|`null`|

# 1.1.2 - Strict equality

:book: We can perform a strict equality comparison using `===`. The strict equality operator has two conditions. In order to match, operands need to:
- be of the _same type_
- have the same value

:pencil2: Modify the `areEqual` function to use `===` unstead of `==`.

:question: What do you think the output of this code will be after replacing the comparison operator?

:pencil2: Try running the code again using Node.js.

:book: Because we are trying to compare a number (`0`) and a boolean (`false`), the strict equality comparison returns `false`.

:exclamation: **Best practice #3:** Always use strict equality instead of loose equality to prevent unintended behaviour

## 1.1.3 - Linting rules for equality

:book: To help you remember to use the correct way of testing for equality, we can set up ESLint to enforce strict mode using the [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq) rule.

:pencil2: Add the `eqeqeq`-rule to `\exercise-2\.eslintrc.json`. It should look like this:

```json
{
    "rules": {
      "semi": "error",
      "brace-style": "error",
      "eqeqeq": "error"
    }
}
```

:exclamation: Remember the trailing commas at the end of the third and fourth line.

:pencil2: Open `exercise-2.js` and add the following code:

```JavaScript
var x = 1;
var y = '0';
var areTheyEqual = x == y;
```

The linter should alert you that you have an error related to the `eqeqeq` rule in your code on line three.

:pencil2: Try fixing the code to get rid of the error.

## 2.1 - Truthy/falsy values

> :book: From MDN: In JavaScript, a _truthy_ value is a value that is considered true when evaluated in a Boolean context. All values are truthy unless they are defined as _falsy_ (i.e., except for `false`, `0`, `""`, `null`, `undefined`, and `NaN`).

Consider the following code:
```JavaScript
var x = 1;
if(x) {
  console.log('Value exists');
}
else {
  console.log('Value does not exist');
}
```

### [Go to exercise 3 ==>](../exercise-3/README.md)
