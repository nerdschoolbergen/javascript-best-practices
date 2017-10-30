# Exercise 3 - Hoisting and scope

You will learn about:
 - Variables and hoisting
 - Function scope and hoisting
 - Strict mode

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

:exclamation: Why does it still behave the same as before we moved the variable declaration? Because JavaScript "**hoists**" or lifts all variable _creations_ to the top of the current scope before running the code. The `myVariable` variable will be created at the top of the current scope, and set to `undefined`. This is called _variable hoisting_ or just _hoisting_.
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

#### 2.2.0.1 - Modifying variables outside a function from the inside

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

##### 2.2.0.2 - Variable hoisting inside a function

:book: Let's try modifying the code from the last task yet again:

```JavaScript
var myMessage = 'A message';

function printStuff() {
  myMessage = 'Testing 123';
  console.log(myMessage);
  var myMessage; // added
}
printStuff();
console.log(myMessage);
```

:question: What do you think the output of the code will be?

:pencil2: Modify the code in `exercise-3.js` and run it in Node.js. (Remember to cross-check that the code is exacly the same)

:book: The code now outputs "Testing 123" and then "A message". Hoisting will move the `var myMessage` declaration inside the function to the top of it's scope. The result will be:

```JavaScript
var myMessage = 'A message';

function printStuff() {
  var myMessage; // Created by JavaScript
  myMessage = 'Testing 123';
  console.log(myMessage);
}
printStuff();
console.log(myMessage);
```

:exclamation: We now have two different variable declarations with the same name, each in its own scope. When the `printStuff()` function tries to assign a new value to `myMessage` outside the function, it will instead just set the value of its own `myMessage` variable that hoisting created.

:book: In larger functions/code bases it's easy to forget that a variable name has been used before outside a function. If you also end up creating variables close to where you need them in the code (instead of at the top of each scope), it's very easy to fall into the "hoisting trap". This creates potential conflicts and confusion.

:exclamation: **Best practice:** Always put variable declarations at the top of the scope where you declare them to avoid accidental hoisting. Assignment can be done at a later time. Never use variables before defining them.

Best practice #4 example:
```JavaScript
var a;
var b;

function myFunc(input) {
  var x;
  var y;
  if(input > 1) {
    x = input * 2;
    y = input / 2;
  } else {
    x = input * 4;
    y = input / 4;
  }
  return x + y;
}

a = 1;
b = myFunc(a);
console.log(b);
```

## 2.3.0 - Linting rules to avoid accidental hoisting

:book: To avoid accidental variable hoisting we can configure ESLint to enforce this via these two rules:
-  [`no-use-before-define`](https://eslint.org/docs/rules/no-use-before-define)
- [`vars-on-top`](https://eslint.org/docs/rules/vars-on-top)

- The `no-use-before-define` rule will warn when it encounters a reference to a variable that has not yet been declared.
- The `vars-on-top` rule will warn when it encounters a variable declaration that is not on top of its scope.

:pencil2: Try these rules out by adding them to the `exercise-3\.eslintrc.json` file:

```json
{
    "rules": {
      "semi": "error",
      "brace-style": "error",
      "eqeqeq": "error",
      "no-use-before-define": "error",
      "vars-on-top": "error"
    }
}
```

:pencil2: Remove all code from `exercise-3.js` and add the following:

```JavaScript
a = 1;
var a;

myFunc(a);

function myFunc(input) {
  var x = input;
  if(input > 0) {
    var y = input;
  }
  return x + y;
}
```

:pencil2: Try modifying the code to get rid of all linting errors.

## 2.4.0 - `let` and `const`

ECMAScript 6 has a few new keywords for variable declaration: `let` and `const`.

`let` variables is simply just like `var` except there's _no hoisting at all_ and the scope is at block level (new in ES6). More on this in exercise 4.

`const` is pretty much exactly what you think: define a variable as a constant, and therefore unchangeable. Since JavaScript can be a pretty volatile language that allows most values to be overwritten at any time, `const` has been warmly welcomed by the community and the rule in many modern code bases is to always use `const` for everything (also function declarations using function expressions), unless you explicitly need to mutate (change) a value after it has been defined. Typically we prefer to create a new variable (using `const`) instead of mutating the existing one.

**Best practice for ES6 variable use:**
- Use `const` by default.
- Use `let` if you have to reassign a variable.
- `let` is the new `var`, except in cases where you really want a variable to be in the function scope.

## 3.0 - Strict mode

## 3.1.0 - What the heck is Strict mode?

[Strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) is a switch we can enable to force a, well, stricter version of the language. It will turn several warnings into errors and forbid you from doing certain bad practices that the language allows by default. It was introduced as a way to make the language more predictable and robust.

Strict mode is opt-in, meaning we must enable it ourselves when we want to enforce it.

- Strict mode is used on functions or entire scripts.
- When enabled in scripts, the `'use strict'` statement must come before any other statement.
- When enabled in functions, the `'use strict'` statement must be the first line in the function

The below script has strict mode enabled:

```js
'use strict';

// ... other code
```

The below function has strict mode enabled:

```js
function foo() {
  'use strict';

  // ... other code
}
```

> JavaScript _modules_ introduced in the newest version of the language is strict by default so we don't have to enable it explicitly.

:pencil2: Let's look at a code example related to the code from previous tasks. Remove all code from `exercise-3.js` and add the following:

```JavaScript
function print(input) {
  stringToPrint = input; // here we implicitly create a global var
  console.log(stringToPrint);
}

print('Hi Nerdschool');
console.log(stringToPrint);
```

:pencil2: Run the code in Node.js.

:book: The code runs normally without throwing an error when you implicitly create the `stringToPrint` global variable inside the function.

:pencil2: Try adding `use strict;`:

```JavaScript
`use strict`;
function print(input) {
  stringToPrint = input; // here we implicitly create a global var
  console.log(stringToPrint);
}

print('Hi Nerdschool');
console.log(stringToPrint);
```

:pencil2: Run the code in Node.js.

:book: The code now throws an `ReferenceError` when accessing a variable that has not been defined.


:exclamation: **Best practice:** Use strict mode to avoid hoisting problems

:bulb: There are way to many details about strict mode to cover here, but if you have trouble sleeping one night, you can read more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

## 3.2.0 - Linting rules for strict mode

We can force the use of Strict mode in ESLint by adding the [`strict`](https://eslint.org/docs/rules/strict) rule to `exercise-3\.eslintrc.json`:

```json
{
    "rules": {
      "semi": "error",
      "brace-style": "error",
      "eqeqeq": "error",
      "no-use-before-define": "error",
      "vars-on-top": "error",
      "strict": ["error", "global"]
    }
}
```

:pencil2: Try removing the `use strict;` directive from  `exercise-3.js` and see what happens.

### [Go to exercise 4 ==>](../exercise-4/README.md)
