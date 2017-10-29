# Exercise 3 - Hoisting and scope

You will learn about:
 - Variables and hoisting
 - Function scope and hoisting
 - Global variables and scope

## Required software and tools for this exercise
- [Atom](https://atom.io/)
- [Node.js](https://nodejs.org)

## 1.0 - Variables and hoisting

## 1.0.1 - Defining variables

:book: Let's have a look at how defining variables work in JavaScript.

Consider the following code:
```JavaScript
console.log(myVariable);
```

:question: What do you think the output of this code will be?

:pencil2: Try adding the code to `exercise-3.js` and run it in Node.js.

:book: Because the `myVariable` variable i undefined, the code throws an `ReferenceError`.

:pencil2: Let's try adding a `myVariable` variable definition at the top:

```JavaScript
var myVariable;
console.log(myVariable);
```

:question: What do you think happens now?

:pencil2: Try modifying the code in `exercise-3.js` and run it in Node.js

:book: The code now runs, and outputs `undefined` because the `myVariable` variable has no value.

## 1.0.2 - Variable hoisting

:pencil2: Let's try moving the `myVariable` definition to the buttom:
```JavaScript
console.log(myVariable);
var myVariable;
```

:question: Will the code still run?

:pencil2: Try modifying the code in `exercise-3.js` and run it in Node.js

:exclamation: Why does it still behave the same as before we moved the variable declaration? Because JavaScript "hoists" or lifts all variable _creations_ to the top of the current scope before running the code, the `myVariable` variable will be created and set to `undefined` at the top:
```JavaScript
var myVariable; // hoisted variable, created By JavaScript
console.log(myVariable);
var myVariable; // this line has no effect
```

:pencil2: Let't try assigning a value to the variable:

```JavaScript
console.log(myVariable);
var myVariable = 'Hello Nerdschool!';
```

:question: How do you think the code behaves now?

:pencil2: Try modifying the code in `exercise-3.js` and run it in Node.js

:exclamation: Why does the code still behave the same? Because the _value_ assignment of the `myVariable` variable will _not_ be hoisted by JavaScript. In effect, the code now looks like this:

```JavaScript
var myVariable; // hoisted variable, created by JavaScript
console.log(myVariable);
var myVariable = 'Hello Nerdschool';
```

Next we are going to look at why this can cause problems when dealing with functions.

## 2.0 - Functions and hoisting

### 2.1.0 - Function scope

:book: In the last section we looked at how JavaScript hoists variable _creation_ to the top of the scope _where it is defined_.

:book: Each time you create a function in JavaScript, a _scope_ is also created.

Consider the following code that prints two different variables, one declared inside the function and one outside:
```JavaScript
var myMessage = 'A message';

function printStuff() {
  var anotherMessage = 'Testing 123';
  console.log(anotherMessage);
  console.log(myMessage);
}
printStuff();
```

:pencil2: Remove all code from `exercise-3.js` from the previous task,  add the code above, then run it in Node.js.

:book: The code outputs both variables because the `printStuff()` function has access to all variables created in the _same scope_ or the scope _outside the function_. This also applies when you nest functions inside functions.

:book: Let's try changing the code a bit by adding a `console.log()` statement to the bottom:

```JavaScript
var myMessage = 'A message';

function printStuff() {
  var anotherMessage = 'Testing 123';
  console.log(anotherMessage);
  console.log(myMessage);
}
printStuff();
console.log(anotherMessage); // add this line
```

:question: What do you think will happen if we run the code now?

:pencil2: Modify the code in `exercise-3.js` and run it in Node.js.

:book: Because the `anotherMessage` variable is declared _inside_ the `printStuff()` function, we cannot access it from the outside like we are trying to in the last line. JavaScript will therefore throw a `ReferenceError` when trying to access the `anotherMessage` variable in the wrong scope.

### 2.2.0 - Function scope and hoisting

:book: As we saw in the last task, we can access variables created _outside_ a function.

Let's try changing the code a bit by assigning a value to `myMessage` inside the function and logging out the value of `myMessage` in the last line. Remember to remove the second `console.log()` statement inside the function as well:
```JavaScript
var myMessage = 'A message';

function printStuff() {
  myMessage = 'Testing 123'; // changed
  console.log(myMessage);
}
printStuff();
console.log(myMessage); // changed
```

:question: What do you think will happen if we run the code now?

:pencil2: Modify the code in `exercise-3.js` and run it in Node.js. (Remember to cross-check that the code is exacly the same)

:book: Because we can access variables created _outside_ a function, the `myMessage` variable will now be assigned a new value inside the `printStuff()` function. The output will therefore be "Testing 123" (twice).

## 3.0 - Global variables and scope
