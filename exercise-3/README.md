# Exercise 3 - Hoisting and scope

You will learn about:
 - Variables and hoisting
 - Functions and hoisting
 - Global variables and scope

## Required software and tools for this exercise
- [Atom](https://atom.io/)
- [Node.js](https://nodejs.org)

## 1.0 - Variables and hoisting

:book: Let's have a look at how declaring and initialising variables work in JavaScript.

Consider the following code:
```JavaScript
var myVariable;
console.log(myVariable);
```

:question: What do you think the output of this code will be?

:pencil2: Try adding the code to `exercise-3.js` and run it in Node.js.

:book: Why did this happen? Because the `myVariable` variable is uninitalised, the code returns `undefined`.

:pencil2: Try moving the variable declaration to the bottom like this:

```JavaScript
console.log(myVariable);
var myVariable;
```

:question: How do you think the code behaves now?

:pencil2: Try modifying the code in `exercise-3.js` and run it in Node.js

:book: Why does it still behave the same as before we moved the variable declaration? Because JavaScript "hoists" or lifts all variables to the top of its scope before running the code, the code will behave in the same way as before.

## 2.0 - Functions and hoisting

## 3.0 - Global variables and scope
