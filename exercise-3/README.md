# Exercise 3 - Hoisting and scope

You will learn about:
 - Variables and hoisting
 - Functions and hoisting
 - Global variables and scope

## Required software and tools for this exercise
- [Atom](https://atom.io/)
- [Node.js](https://nodejs.org)

## 1.0 - Variables and hoisting

:book: Let's have a look at how defining and initialising variables work in JavaScript.

Consider the following code:
```JavaScript
console.log(myVariable);
```

:question: What do you think the output of this code will be?

:pencil2: Try adding the code to `exercise-3.js` and run it in Node.js.

:book: Why did this happen? Because the `myVariable` variable i undefined, the code throws an `ReferenceError`.

:pencil2: Let's try adding a `myVariable` variable definition at the top:

```JavaScript
var myVariable;
console.log(myVariable);
```

:question: What do you think happens now?

:pencil2: Try modifying the code in `exercise-3.js` and run it in Node.js

:book: The code now runs, and outputs `undefined` because the `myVariable` variable has no value.

:pencil2: Now try assigning a value to the variable:

```JavaScript
console.log(myVariable);
var myVariable = 'Hello Nerdschool!';
```

:question: How do you think the code behaves now?

:pencil2: Try modifying the code in `exercise-3.js` and run it in Node.js

:exclamation: Why does it still behave the same as before we moved the variable declaration? Because JavaScript "hoists" or lifts all variable _creations_ to the top of the current scope before running the code, the `myVariable` variable will be created and set to `undefined` at the top:
```JavaScript
var myVariable;
console.log(myVariable);
var myVariable = 10;
```

Next we are going to look at why this can cause problems when dealing with functions.

## 2.0 - Functions and hoisting

## 3.0 - Global variables and scope
