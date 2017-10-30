# Exercise 4 - Patterns

You will learn about:
 - Lexical scope and this
 - How to avoid polluting the global scope

## Required software and tools for this exercise
- [Atom](https://atom.io/)
- [Node.js](https://nodejs.org)

## 1.0 - Lexical scoping and closures

Let's start with defining what _lexical scope_ means.

> Lexical scoping (sometimes known as static scoping ) is a convention used with many programming languages that sets the scope (range of functionality) of a variable so that it may only be called (referenced) from within the block of code in which it is defined.

However, we're not going to talk much about how closures or lexical scoping works on a theoretical level. In this exercise we want to explain how `this` works in JavaScript.

Consider the following code:

```js
function createGreeter() {
  this.greeting = "hello";
}
```

At this point you might ask
- What is `this` set to in this example?
- Where does `this` come from?

In object-oriented languages such as Java or C#, `this` refers to the scope of the current `class` we're in. The `class`, therefore, is the _lexical scope_ in that case. In the newest version of JavaScript we also have classes, but for now, let's just focus on functions.

In JavaScript, you can be in two _contexts_:

1. Global context (outside of any function)
2. Function context (inside a function)

When in _Global context_, `this` refers to the `global` object. If you're in a browser, it will refer to the `window` object.

> We (and resources on the web) might refer to the `window` and the `global` objects in JavaScript. You might question where these come from. These are built-in, global variables in the language. The `window` object is typically only available in a browser context (when JavaScript runs in a web browser). It won't be available in a _Node_ context, since Node does not run in a browser. The `global` object is always available anywhere in JavaScript.

When in _Function context_, `this` refers to the `global` object. If you're in a browser, it will refer to the `window` object.

```js
this.wat = 'huh?';

function funkyBusiness() {
  return this.wat; // "huh?"
}
```

:pencil2: Assign a variable to the `global` object, for example a string containing a message.  
:pencil2: Below the assignment you did above, create a function where you log `this.YOUR_VARIABLE`

For example (please write it out yourself):

```js
global.hi = 'hallaien';
function greet() {
  console.log(this.hi);
}
greet();
```

:pencil2: Run the code and you should see your message printed to the console. This confirms that `this` inside a function and `global` is the same object.  

(This is weird and scary, and a good rule of thumb in JavaScript is that whenever the language does not know what to do with a value you declare, it will put it on the global scope and go about its business. A lot of the "magic" in JavaScript and it's "It Just Works" functionality is also the same thing - it just puts your mess on the global scope.  While this is helpful (it doesn't crash), it is a source of a lot of misunderstandings and bad practices. We always want to avoid the global scope if possible).

## 1.1 - Strict mode

You probably noticed that the description of _Global context_ and _Function context_ above was identical. Well, that changes when we use _Strict Mode_.

As described earlier, _Strict mode_ is something we can enable to make the language stricter and safer.

When we enable strict mode in a function, the function's `this` context changes.

:pencil2: Enable strict mode in the function you made.  
:pencil2: Run the code again.  

You should now see the error `TypeError: Cannot read property 'YOUR_VARIABLE' of undefined` in the console. So what happened?

In strict mode, the value of `this` inside a function is whatever it was set to before entering the function. So because we didn't assign `this` to be anything specific, it is `undefined` when we enter the function. Therefore, `global` is no longer equal to `this` when strict mode is enabled, and reading the variable you set on `global` failed.

### 1.1.0 `call()`

So to make `this` not be undefined, we must take a look at the built-in prototype functions `call` and `apply`. For simplicity, let's just use the `call` function today.

All functions in JavaScript inherits certain other functions from the `Function.prototype` function in the language. This might seem confusing, but for now just try to acknowledge that functions can have functions in JavaScript. Yes, it's weird.

So if we look at the [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) function, the first argument will be the function's `this` context, and the other parameters will be the function's arguments.

For example:

```js
function greet(name) {
  console.log(this.hi + ' ' + name);
}
greet.call({ hi: 'hei' }, 'eirik');
```
(Yes `greet` is a function which can be invoked as `greet()` but it also has other functions we can invoke by treating it like an object, such as `greet.call()`, `greet.apply()`, and `greet.bind()`. What can we say, JavaScript is weird...)

- We pass a new object as the first parameter to `call`. This will be the `this` context while in the `greet` function.
- The other parameter to `call` will be the `name` argument in the `greet` function.

:pencil2: Modify your code to use `call` and make `this` work as expected in your code.  
:pencil2: Play around with this funkyness a bit and make sure you (somewhat) understand what's going on here.

> You can read more about `call` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call).

### 1.1.2 `bind()`

Like `call`, `bind` exists to let us set `this` on a function. However, `bind` will set the `this` context without actually invoking the function like `call` does. With bind we say "remember to use this object as your `this` context whenever I call you later on". With call we say "Invoke this function right now using this object as your `this` context".

```js
function greet(name) {
  console.log(this.hi + ' ' + name);
}
greet = greet.bind({ hi: 'helloooo' });
// ... other code and stuff
greet('eirik');
```

:pencil2: Make a new function and use `bind` to set it's `this` context.  

### 1.1.3 Accessing `this` in sub-functions

An often occurring problem is accessing `this` which lives outside the current function.

Consider the following code:

```js
function greet(name) {
  this.smiley = '=D';
  function sayHello() {
    console.log(this.hi + ' ' + name + ' ' + this.smiley);
  }
  sayHello();
}
greet.call({ hi: 'hi' }, 'eirik');
```

Executing this code returns `undefined eirik undefined`. What gives?

Well, as mentioned earlier, in JavaScript, functions is the lexical scope to which `this` is bound. In other words, the `greet` function has a different `this` context than the `sayHello` function. So when we try to access `this` inside the `sayHello` function, the `hi` and `smiley` values are not set, because they exists on the outer `this`. A common workaround for this is to assign `this` to another variable:

```js
function greet(name) {
  this.smiley = '=D';

  var that = this;

  function sayHello() {
    console.log(that.hi + ' ' + name + ' ' + that.smiley);
  }
  sayHello();
}
greet.call({ hi: 'hi' }, 'eirik');
```

This correctly prints `hi eirik =D`. Note that we changed `sayHello` to use `that.hi` and `that.smiley` instead of `this`. Other common words are `self` and `me`.

This workaround is very common and an accepted solution by the community. We do however want to always use the same "alias" throughout the codebase, and ESLint can help us enfore this with its `consistent-this` rule.

:pencil2: Open `.eslintrc.json` and add `"consistent-this": ["error", "that"]` to the rules. Feel free to use another alias if you want.  

## 1.3.0 Best practices summary for `this`

- Avoid global scope.
- _Use strict_ in functions.
- Specify a "strict function"'s `this` context by using `call`, `apply`, or `bind`.
- Use lint rules to avoid multiple aliases for `this` access.

## 1.4.0 Classes

We're not going to dive deep into JavaScript classes here, but we want to just touch on the basics so you can see they're really nothing special, and that scoping (and `this`) for a class is just like what we have already discussed above.

A `class` in JavaScript is just syntax sugar for functions.

The following class:

```js
class Foo {
  bar() {
    return 'bar';
  }
}
```

Is the same as writing:

```js
var Foo = function() {
}

Foo.prototype.bar = function() {
  return 'bar';
}
```

So you see, it's just functions under the hood. A class containing functions is just compiled into an outer function wrapping other inner functions. The same rules regarding `this` etc applies here just like anywhere else in the language. What classes gives you, though, is a lot less boilerplate to write, easier code to read and maintain, and managing `this` with `call` is handled for you behind the scenes. There is nothing a class can do that we couldn't do before in the language (including inheritance/`extends`), it just gives us a nicer wrapping around it, which is nice, and very much welcomed.

## 1.5.0 Lambdas

The newer versions of the language introduced _lambdas_ or _arrow functions_, which you might recognize from many other languages.

```js
// Single-line lambda function that takes no arguments and returns 'hello'
var greet = () => 'hello';

// Single-line lambda function that takes to arguments and returns a string
var greet = (greeting, name) => greeting + ' ' + name;

// Multi-line lambda that takes no arguments, returns a new function that takes a 'name' argument, that returns a string
var greet = () => {
  var message = 'hallaien';
  return (name) => message + ' ' + name;
};
```

In most cases you can just think _function_ whenever you hear _lambda_.

Where regular functions and lambdas/arrow functions differ, though, is how they handle `this`.

As mentioned several times now, a _function_ is the natural scope/encapsulation/closure for `this` in JavaScript, and we looked at how inner functions define their own `this` which is not the same as the enclosing/outer function's `this` context.

Well, lambdas _do not have their own `this` scope_. Instead, they inherit the parent's `this` context.

```js
function greet() {
  this.hi = 'hallaien';
  return (name) => this.hi + ' ' + name; // Using this.hi inside the lambda function works fine
}
```

(Playing with lambdas require a more complex set-up/build chain than our workshop is aimed for so no try-lambdas-yourself in this workshop, unfortunately.)

In many ways, this makes lambdas easier to reason about and "more lightweight". In modern code bases we typically prefer lambdas wherever we can, since `this` is rarely used outside of classes. If you however need the "full functionality" of a function, then use a good old `function`.

## 2.0 - Global scope pollution

:book: As we learned about in the previous section, it is good practice to avoid "polluting" the global scope, as you never know what function or variable declared in other scripts (yours or others) you are potentially overwriting. All scripts loaded in the browser the same global scope.

To work around this issue we can create a new scope that works as a "wrapper" around all the code in our script.

### 2.1.0 - IIFEs

:book: Before we get into what an "IIFE" is, we need to go over some JavaScript function basics. There are two ways of creating a function in JavaScript:
- Via function declarations
- Via Function expressions

Function declarations you know already (the "normal" way):
```JavaScript
function myFunc() {
  // do stuff
}
myFunc();
```

The other way is via Function expressions by using the `function` keyword:
```JavaScript
var myFunc = function() {
  // do stuff
};
myFunc();
```

:book: The last one is a bit weird, because we are assigning the function to a variable. Functions in JavaScript are like any other object, and because of this you can assign them to variables or even properties of objects:

```JavaScript
var myObj = {
  myFunc: function() {
    // do stuff
  }
}
myObj.myFunc();
```

:book: Expressions in JavaScript are any valid unit of code that resolves to a value.

For example:
```JavaScript
3 + 4; // This is an expression even if it is not assigned to a variable
3 + (4 * 4); // This is an example of using the grouping operator
console.log((4/3)+2) // The expression resolved to a velue before it is logged
```

:book: The `function` keyword we used to create functions earlier can be used to define a function _inside_ an expression:

```JavaScript
(function() {
  // do stuff
});
```

:book: If we want to _execute_ the function, we add a `()` at the end:
```JavaScript
(function() {
    // do stuff
})();
```

:book: The function inside the expression will now be invoked immediately after it is parsed. It's an _Immediately Invoked Function Expression_, or IIFE for short.

:pencil2: Try adding the code above to `exercise-4.js`, add a `console.log()` statement inside the IIFE and run the code.

:book: The `console.log()` statment should have printed something out to the console.

:pencil2: Try modifying the code a bit by declaring a varible inside the IIFE and try to access it outside:

```JavaScript
(function() {
    var myPrivateVar = 1;
})();
console.log(myPrivateVar);
```

:pencil2: Try running the code. Observe that the code throws an `ReferenceError` because the `myPrivateVar` is not defined in the global scope outside the IIFE (where it is being accessed).

This is because the IIFE creates a new scope inside it, where you can declare all the variables you want without worrying about polluting the global scope.

:exclamation: **Best practice:** Use IIFEs around your code to prevent polluting the global scope.

### 2.2.0 - ECMAScript 6 modules

> :bulb: This feature is only just beginning to be implemented in browsers natively at this time. It is implemented in many transpilers, such as TypeScript and Babel, and bundlers such as Rollup) and Webpack.

:book: ECMAScript 6 has a feature called modules, which helps you structure code in an efficient way.

A module...
- ...is a JavaScript file that that exposes one or more functions via the `export` keyword
- ...can import other modules via the `import` keyword in order to call other modules
- ...has strict mode enabled by default
- ...creates a new function scope automatically

An example:

```JavaScript
// HelloWorld.js
var helloWorld = function() {
 return "Hello World!";
}
export default HelloWorld;
```

Importing the module:
```JavaScript
// Main.js
import helloWorld from './HelloWorld.js';
consolel.log(helloWorld());
```

:exclamation: Node.js does not currently support this syntax yet. Starting with Node.js version 8.5.0 support is available behind an [experimental flag](http://2ality.com/2017/09/native-esm-node.html). ES modules will available by default in Node.js 10.

### 2.3.0 - ECMAScript 6 block scope with let/const

:book: Another feature in ECMASCript 6 is the `let` and `const` keyword for declaring variables.

#### 2.3.1.0 - let

As mentioned in exercise 3, the `let` keyword creates variables scoped to the current _block_, not function scope or global scope.

:pencil2: Try adding the following code to `exercise-4.js`:
```JavaScript
(function() {
  if(true) {
    let x = 1;
  }
  console.log(x);
})();
```

ESLint will complain about the `let` keyword. This is because we need to configure ES6 support in `.eslintrc.json`.

:pencil2: Insert the `"parserOptions"` key at the top of `excercise-4\.eslintrc.json`:
```json
{
  "parserOptions": {
    "ecmaVersion": 6
  },
  "rules": {
    "semi": "error",
    "brace-style": "error",
    "eqeqeq": "error",
    "vars-on-top": "error",
    "strict": ["error", "global"]
  }
}
```

:pencil2: Try running the code.

:book: The code throws an `ReferenceError` because we defined the `x` variable the block scope of an `if` statement.

Here is a simpler version:
```JavaScript
(function() {
  {
    let x = 1;
  }
  console.log(x);
})();
```

:question: Is the IIFE really need now that we can scope variables to blocks instead?

:pencil2: Try simplifying the code by removing the IIFE, and see if it still behaves in the same way (not modifying the global scope).

:book: As we mentioned about in exercise 3, the `const` keyword let's you define variables that cannot be _reassigned_. It also behaves the same as `let` when it comes to scoping.

**Best practice:**
- If you are using ES6 (or newer) and you are not using modules, use `let` and `const` for encapsulation instead of IIFEs.  

### [Go to feedback ==>](../feedback)
