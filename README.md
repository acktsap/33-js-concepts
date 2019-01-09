<h1 align="center">
<br>
  <a href="https://github.com/leonardomso/33"><img src="https://i.imgur.com/dsHmk6H.jpg" alt="33 Concepts Every JS Developer Should Know" width=200"></a>
  <br>
    <br>
  33 Concepts Every JavaScript Developer Should Know
  <br><br>
</h1>

<p align="center">
  <a href="http://makeapullrequest.com">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square" alt="PRs Welcome">
  </a>
  <a href="https://opensource.org/licenses/MIT">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square" alt="License MIT">
  </a>
</p>

## Introduction

This repository was created with the intention of my own summary of [33-js-concepts](https://github.com/leonardomso/33-js-concepts) which is based on an article written by [Stephen Curtis](https://twitter.com/stephenthecurt).

---

## Table of Contents

1. **[Call Stack](#1-call-stack)**
2. **[Primitive Types](#2-primitive-types)**
3. **[Value Types and Reference Types](#3-value-types-and-reference-types)**
4. **[Implicit, Explicit, Nominal, Structuring and Duck Typing](#4-implicit-explicit-nominal-structuring-and-duck-typing)**
5. **[== vs === vs typeof](#5--vs--vs-typeof)**
6. **[Function Scope, Block Scope and Lexical Scope](#6-function-scope-block-scope-and-lexical-scope)**
7. **[Expression vs Statement](#7-expression-vs-statement)**
8. **[IIFE, Modules and Namespaces](#8-iife-modules-and-namespaces)**
9. **[Message Queue and Event Loop](#9-message-queue-and-event-loop)**
10. **[setTimeout, setInterval and requestAnimationFrame](#10-settimeout-setinterval-and-requestanimationframe)**
11. **[JavaScript Engines](#11-javascript-engines)**
12. **[Bitwise Operators, Type Arrays and Array Buffers](#12-bitwise-operators-type-arrays-and-array-buffers)**
13. **[DOM and Layout Trees](#13-dom-and-layout-trees)**
14. **[Factories and Classes](#14-factories-and-classes)**
15. **[this, call, apply and bind](#15-this-call-apply-and-bind)**
16. **[new, Constructor, instanceof and Instances](#16-new-constructor-instanceof-and-instances)**
17. **[Prototype Inheritance and Prototype Chain](#17-prototype-inheritance-and-prototype-chain)**
18. **[Object.create and Object.assign](#18-objectcreate-and-objectassign)**
19. **[map, reduce, filter](#19-map-reduce-filter)**
20. **[Pure Functions, Side Effects and State Mutation](#20-pure-functions-side-effects-and-state-mutation)**
21. **[Closures](#21-closures)**
22. **[High Order Functions](#22-high-order-functions)**
23. **[Recursion](#23-recursion)**
24. **[Collections and Generators](#24-collections-and-generators)**
25. **[Promises](#25-promises)**
26. **[async/await](#26-asyncawait)**
27. **[Data Structures](#27-data-structures)**
28. **[Expensive Operation and Big O Notation](#28-expensive-operation-and-big-o-notation)**
29. **[Algorithms](#29-algorithms)**
30. **[Inheritance, Polymorphism and Code Reuse](#30-inheritance-polymorphism-and-code-reuse)**
31. **[Design Patterns](#31-design-patterns)**
32. **[Partial Applications, Currying, Compose and Pipe](#32-partial-applications-currying-compose-and-pipe)**
33. **[Clean Code](#33-clean-code)**


---

## 1. Call Stack

### Articles

* 📜 [Understanding the JavaScript Call Stack — Charles Freeborn](https://medium.freecodecamp.org/understanding-the-javascript-call-stack-861e41ae61d4)

### Summary

Just an normal LIFO(Last In First Out) function call stack. If you already took a programming lauguage class, it's obvious. Just a call stack frame.

```js
function firstFunction(){
 console.log("Hello from firstFunction");
}
function secondFunction(){
 firstFunction();
 console.log("The end from secondFunction");
}

// prints
// "Hello from firstFunction"
// "The end from secondFunction"
secondFunction();

/*
stack
==============
     ...
     ...
firstFunction
secondFunction
==============
*/
```

---

## 2. Primitive Types

### Articles

* 📜 [Primitive Types — Flow](https://flow.org/en/docs/types/primitives/)
* 📜 [(Not) Everything in JavaScript is an Object - Daniel Li](http://blog.brew.com.hk/not-everything-in-javascript-is-an-object/)

### Summary

* boolean
* null
* undefined
* number
* string
* symbol (new in ECMAScript 2015)

```js
// literals
true                 // boolean
"hello"              // string
3                    // number
3.14                 // number
null                 // null
undefined            // undefined

// wrappers
new Boolean(false)   // Boolean
new String("hello")  // String
new Number(3)        // Number
new Number(3.14)     // Number
Symbol("foo")        // Symbol
```

All others are object type including `function` and `array`.

```js
// function
const foo = function () {}
foo instanceof Object // true
```

Primitive types have no methods so they are immutable. But if you try to use method of primitive types, js engine automatically convert primitive type into its wrapper which is called **auto-boxing**

```js
const pet = new String("cat")
pet.length   // 3
"cat".length // 3

const foo = 42
foo.bar = "baz" // create an new object and assign to it
foo.bar         // undefined since the "baz" is assigned to temp wrapper object
```

**[⬆ Back to Top](#table-of-contents)**

---

## 3. Value Types and Reference Types

### Articles

* 📜 [Grasp “By Value” and “By Reference” in JavaScript — Léna Faure](https://hackernoon.com/grasp-by-value-and-by-reference-in-javascript-7ed75efa1293)


### Summary

Primitive Data Types are passed By Value and Objects are passed By Reference. That's it.

* Primitive Data Types : deep copy
* Objects              : shadow copy

```js
// primitive type
var firstName = "A";
var secondName = firstName;
firstName = "B";
console.log(firstName);  // "B"
console.log(secondName); // "A"

// object
var myName = {
  firstName: "A"
};
var identity = myName;
myName.firstName = "B";
console.log(myName.firstName); // "B"
console.log(identity.firstName); // "B"

// object
function myNameIs(aName){
  aName.firstName = "Carla";
}
var myName = {};
myNameIs(myName);
console.log(myName); // Object {firstName: "Carla"}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 4. Implicit, Explicit, Nominal, Structuring and Duck Typing

### Articles

* 📜 [What you need to know about Javascript's Implicit Coercion — Promise Tochi](https://dev.to/promhize/what-you-need-to-know-about-javascripts-implicit-coercion-e23)

### Summary

You know that it should be avoided. Use explicit coercion instead.

#### Non-numeric values in numeric expressions

String : for `-, *, /, %` calling `Number` wrapper function

```js
3 * "3" // 3 * 3
3 * Number("3") // 3 * 3
Number("5") // 5

Number("1.") // 1
Number("1.34") // 1.34
Number("0") // 0
Number("012") // 12

Number("1,") // NaN
Number("1+1") // NaN
Number("1a") // NaN
Number("text") // NaN
```

String : for `+` convert number to string by calling `String` wrapper function

```js
// concatenation
1 + "2" // "12"
1 + "js" // "1js"

// addition
1 + 2 // 3
1 + 2 + 1 // 4

// addition, then concatenation
1 + 2 + "1" // "31"
(1 + 2) + "1" // "31"

// concatenation all through
1 + "2" + 1 // "121"
(1 + "2") + 1 // "121"
```

Object : call `toString` and convert it to number by calling `Number` wrapper

```js
// default toString method
const foo = {}
foo.toString() // [object Object]

// overrided toString method
const baz = {
  toString: () => "I'm object baz"
}
baz + "!" // "I'm object baz!"


// toString method used as numeric expression with number return
const foo = {
  toString: () => 4
}
2 * foo // 8
2 / foo // 0.5
2 + foo // 6
"four" + foo // "four4"

// toString method used as numeric expression with string return
const baz = {
  toString: () => "four"
}
2 * baz // NaN
2 + baz // 2four

// toString method used as numeric expression with number convertable string return
const bar = {
  toString: () => "2"
}
2 + bar // "22"
2 * bar // 4
```

Object : call `valueOf` if its defined

```js
const bar = {
  toString: () => 2, // ignored
  valueOf: () => 5
}

"sa" + bar // "sa5"
3 * bar // 15
2 + bar // 7
```

Arrays : also object, call `toString` method

```js
// default toString method of array
[1,2,3].toString() // "1,2,3"
[1,2,3].join() // "1,2,3"
[].toString() // ""
[].join() // ""

// toString method with concat
"me" + [1,2,3] // "me1,2,3"
4 + [1,2,3] // "41,2,3"

// toString method with numeric expression1
4 * [] // 0

// similar to
4 * Number([].toString())
4 * Number("")
4 * 0

// toString method with numeric expression2
4 / [2] // 2

// similar to
4 / Number([2].toString())
4 / Number("2")
4 / 2

// toString method with numeric expression3
4 * [1,2,3] // NaN
```

`True` is 1, `False` and `""` are 0

```js
Number(true) // 1
Number(false) // 0
Number("") // 0

4 + true // 5
3 * false // 0
3 * "" // 0
3 + "" // "3", in '+' string concat is first
```

#### Falsy and Truthy

Falsy values

* false
* 0
* null
* undefined
* ""
* NaN
* -0

Everything else is truthy

```js

// falsy
if (-1)
if (null)
if (undefined)
if ("")

// truthy
if (-1)
if ("0")
if ({})

const counter = 2
if (counter) // true
```

#### NaN (Not a Number)

NaN is a special numeric value that is not equal to itself

```js
NaN === NaN // false

const notANumber = 3 * "a" // NaN
notANumber == notANumber // false
notANumber === notANumber // false
```

To check `NaN`

```js
// classical
if (notANumber !== notANumber)

// ECMAScript 6 introduced
Number.isNaN(NaN) // true
Number.isNaN("name") // false

// avoid global NaN since it try to convert Number first
isNaN("name") // true since Number("name") returns NaN
isNaN("1") // false since Number("1") returns NaN
```

**[⬆ Back to Top](#table-of-contents)**

---

## 5. == vs === vs typeof

### Articles

 * 📜 [JavaScript Double Equals vs. Triple Equals — Brandon Morelli](https://codeburst.io/javascript-double-equals-vs-triple-equals-61d4ce5a121a)
 * 📜 [What is the difference between =, ==, and === in JS? — Codecademy](https://www.codecademy.com/en/forum_questions/558ea4f5e39efed371000508)
 * 📜 [Should I use === or == equality comparison operator in JavaScript? — Panu Pitkamaki](https://bytearcher.com/articles/equality-comparison-operator-javascript/)
 * 📜 [== vs === JavaScript: Double Equals and Coercion — AJ Meyghani](https://www.codementor.io/javascript/tutorial/double-equals-and-coercion-in-javascript)
 * 📜 [Why Use the Triple-Equals Operator in JavaScript? — Louis Lazaris](https://www.impressivewebs.com/why-use-triple-equals-javascipt/)
 * 📜 [What is the difference between == and === in JavaScript? — Craig Buckler](https://www.oreilly.com/learning/what-is-the-difference-between-and-in-javascript)
 * 📜 [Why javascript's typeof always return "object"? — Stack Overflow](https://stackoverflow.com/questions/3787901/why-javascripts-typeof-always-return-object)
 * 📜 [Checking Types in Javascript — Toby Ho](http://tobyho.com/2011/01/28/checking-types-in-javascript/)
 * 📜 [How to better check data types in JavaScript — Webbjocke](https://webbjocke.com/javascript-check-data-types/)
 * 📜 [Checking for the Absence of a Value in JavaScript — Tomer Aberbach](https://tomeraberba.ch/html/post/checking-for-the-absence-of-a-value-in-javascript.html)

### Videos

 * 🎥 [JavaScript - The typeof operator — Java Brains](https://www.youtube.com/watch?v=ol_su88I3kw)
 * 🎥 [Javascript typeof operator — DevDelight](https://www.youtube.com/watch?v=qPYhTPt_SbQ)

**[⬆ Back to Top](#table-of-contents)**

---

## 6. Function Scope, Block Scope and Lexical Scope

### Articles

 * 📜 [You Don't Know JS: Scope & Closures [Book] — Kyle Simpson](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md)
 * 📜 [JavaScript Functions — Understanding The Basics — Brandon Morelli](https://codeburst.io/javascript-functions-understanding-the-basics-207dbf42ed99)
 * 📜 [The battle between Function Scope and Block Scope — Marius Herring](http://www.deadcoderising.com/2017-04-11-es6-var-let-and-const-the-battle-between-function-scope-and-block-scope/)
 * 📜 [Emulating Block Scope in JavaScript — Josh Clanton](http://adripofjavascript.com/blog/drips/emulating-block-scope-in-javascript.html)
 * 📜 [The Difference Between Function and Block Scope in JavaScript — Joseph Cardillo](https://medium.com/@josephcardillo/the-difference-between-function-and-block-scope-in-javascript-4296b2322abe)
 * 📜 [Function Scopes and Block Scopes in JavaScript — Samer Buna](https://edgecoders.com/function-scopes-and-block-scopes-in-javascript-25bbd7f293d7)
 * 📜 [Understanding Scope and Context in JavaScript | Ryan Morr](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)
 * 📜 [JavaScript Scope and Closures — Zell Liew](https://css-tricks.com/javascript-scope-closures/)
 * 📜 [Understanding Scope in JavaScript — Wissam Abirached](https://developer.telerik.com/topics/web-development/understanding-scope-in-javascript/)
 * 📜 [Speaking JavaScript - Variables: Scopes, Environments, and Closures — Dr. Axel Rauschmayer](http://speakingjs.com/es5/ch16.html)
 * 📜 [Understanding Scope in JavaScript ― Hammad Ahmed](https://scotch.io/tutorials/understanding-scope-in-javascript)

### Videos

 * 🎥 [What Makes Javascript Weird ... and Awesome pt. 4 — LearnCode.academy](https://www.youtube.com/watch?v=SBwoFkRjZvE)
 * 🎥 [Variable Scope in JavaScript — Kirupa Chinnathambi](https://www.youtube.com/watch?v=dhp57T3p760)
 * 🎥 [JavaScript Block Scope and Function Scope — mmtuts](https://www.youtube.com/watch?v=aK_nuUAdr8E)
 * 🎥 [What the Heck is Lexical Scope? — NWCalvank](https://www.youtube.com/watch?v=GhNA0r10MmA)

**[⬆ Back to Top](#table-of-contents)**

---

## 7. Expression vs Statement

### Articles

 * 📜 [All you need to know about Javascript's Expressions, Statements and Expression Statements — Promise Tochi](https://dev.to/promhize/javascript-in-depth-all-you-need-to-know-about-expressions-statements-and-expression-statements-5k2)
 * 📜 [Function Expressions vs Function Declarations — Paul Wilkins](https://www.sitepoint.com/function-expressions-vs-declarations/)
 * 📜 [JavaScript Function — Declaration vs Expression — Ravi Roshan](https://medium.com/@raviroshan.talk/javascript-function-declaration-vs-expression-f5873b8c7b38)
 * 📜 [Function Declarations vs. Function Expressions — Mandeep Singh](https://medium.com/@mandeep1012/function-declarations-vs-function-expressions-b43646042052)
 * 📜 [Function Declarations vs. Function Expressions — Anguls Croll](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)

### Videos

 * 🎥 [Expressions vs. Statements in JavaScript — Hexlet](https://www.youtube.com/watch?v=WVyCrI1cHi8)
 * 🎥 [JavaScript - Expression vs. Statement — WebTunings](https://www.youtube.com/watch?v=3jDpNGJkupA)
 * 🎥 [Function Statements and Function Expressions — Codeacademy](https://www.youtube.com/watch?v=oB5rH_9bqAI)

**[⬆ Back to Top](#table-of-contents)**

---

## 8. IIFE, Modules and Namespaces

### Articles

 * 📜 [Mastering Immediately-Invoked Function Expressions ― Chandra Gundamaraju](https://medium.com/@vvkchandra/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6)
 * 📜 [Do ES6 Modules make the case of IIFEs obsolete?](https://hashnode.com/post/do-es6-modules-make-the-case-of-iifes-obsolete-civ96wet80scqgc538un20es0)
 * 📜 [A 10 minute primer to JavaScript modules, module formats, module loaders and module bundlers ― Jurgen Van de Moere](https://www.jvandemo.com/a-10-minute-primer-to-javascript-modules-module-formats-module-loaders-and-module-bundlers/)
 * 📜 [Modules ― Exploring JS](http://exploringjs.com/es6/ch_modules.html)
 * 📜 [ES modules: A cartoon deep-dive — Lin Clark](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)
 * 📜 [Understanding ES6 Modules — Craig Buckler](https://www.sitepoint.com/understanding-es6-modules/)
 * 📜 [An overview of ES6 Modules in JavaScript — Brent Graham](https://blog.cloud66.com/an-overview-of-es6-modules-in-javascript/)
 * 📜 [ES6 Modules in Depth — Nicolás Bevacqua](https://ponyfoo.com/articles/es6-modules-in-depth)
 * 📜 [ES6 modules, Node.js and the Michael Jackson Solution — Alberto Gimeno](https://medium.com/dailyjs/es6-modules-node-js-and-the-michael-jackson-solution-828dc244b8b)
 * 📜 [JavaScript Modules: A Beginner’s Guide — Preethi Kasireddy](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc)

### Videos

 * 🎥 [Immediately Invoked Function Expression - Beau teaches JavaScript — freeCodeCamp](https://www.youtube.com/watch?v=3cbiZV4H22c)
 * 🎥 [Understanding JavaScript IIFE](https://www.youtube.com/watch?v=I5EntfMeIIQ)
 * 🎥 [JavaScript Modules: ES6 Import and Export — Kyle Robinson](https://www.youtube.com/watch?v=_3oSWwapPKQ)
 * 🎥 [ES6 - Modules — Ryan Christiani](https://www.youtube.com/watch?v=aQr2bV1BPyE)
 * 🎥 [ES6 Modules in the Real World — Sam Thorogood](https://www.youtube.com/watch?v=fIP4pjAqCtQ)
 * 🎥 [ES6 Modules — TempleCoding](https://www.youtube.com/watch?v=5P04OK6KlXA)

**[⬆ Back to Top](#table-of-contents)**

---

## 9. Message Queue and Event Loop

### Articles

 * 📜 [JavaScript Event Loop Explained — Anoop Raveendran](https://medium.com/front-end-hacking/javascript-event-loop-explained-4cd26af121d4)
 * 📜 [The JavaScript Event Loop: Explained — Erin Sweson-Healey](https://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/)
 * 📜 [What is the Event Loop in Javascript — WP Tutor.io](https://www.wptutor.io/web/js/javascript-event-loop)
 * 📜 [Understanding JS: The Event Loop — Alexander Kondov](https://hackernoon.com/understanding-js-the-event-loop-959beae3ac40)
 * 📜 [Understanding the JavaScript Event Loop — Ashish Gupta](https://www.zeolearn.com/magazine/understanding-the-javascript-event-loop)
 * 📜 [Event Loop in Javascript — Manjula Dube](https://code.likeagirl.io/what-the-heck-is-event-loop-1e414fccef49)
 * 📜 [The JavaScript Event Loop — Flavio Copes](https://flaviocopes.com/javascript-event-loop/)
 * 📜 [How JavaScript works: Event loop — Alexander Zlatkov](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)
 * 📜 [Tasks, microtasks, queues and schedules — Jake Archibald](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
 * 📜 [Visualising the JavaScript Event Loop with a Pizza Restaurant analogy — Priyansh Jain](https://dev.to/presto412/visualising-the-javascript-event-loop-with-a-pizza-restaurant-analogy-47a8)

### Videos

 * 🎥 [What the heck is the event loop anyway? | JSConf EU — Philip Roberts](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
 * 🎥 [JavaScript Event Loop — ComScience Simplified](https://www.youtube.com/watch?v=XzXIMZMN9k4)
 * 🎥 [I'm stuck in an Event Loop — Philip Roberts](https://www.youtube.com/watch?v=6MXRNXXgP_0)
 * 🎥 [In The Loop - Jake Archibald | JSConf.Asia 2018](https://www.youtube.com/watch?v=cCOL7MC4Pl0)
 * 🎥 [Desmitificando el Event Loop (Spanish)](https://www.youtube.com/watch?v=Eqq2Rb7LzYE)


**[⬆ Back to Top](#table-of-contents)**

---

## 10. setTimeout, setInterval and requestAnimationFrame

### Articles

 * 📜 [setTimeout and setInterval — JavaScript.Info](https://javascript.info/settimeout-setinterval)
 * 📜 [Why not to use setInterval — Akanksha Sharma](https://dev.to/akanksha_9560/why-not-to-use-setinterval--2na9)
 * 📜 [setTimeout VS setInterval — Develoger](https://develoger.com/settimeout-vs-setinterval-cff85142555b)
 * 📜 [Using requestAnimationFrame — Chris Coyier](https://css-tricks.com/using-requestanimationframe/)
 * 📜 [Understanding JavaScript's requestAnimationFrame() — JavaScript Kit](http://www.javascriptkit.com/javatutors/requestanimationframe.shtml)
 * 📜 [Handling time intervals in JavaScript - Amit Merchant](https://www.amitmerchant.com/Handling-Time-Intervals-In-Javascript/)

### Videos

 * 🎥 [Javascript: How setTimeout and setInterval works — Coding Blocks India](https://www.youtube.com/watch?v=6bPKyl8WYWI)
 * 🎥 [setTimeout and setInterval in JavaScript — techsith](https://www.youtube.com/watch?v=TbCgGWe8LN8)
 * 🎥 [JavaScript Timers — Steve Griffith](https://www.youtube.com/watch?v=0VVJSvlUgtg)
 * 🎥 [JavaScript setTimeout, setInterval & clearInterval — DoingITeasyChannel](https://www.youtube.com/watch?v=BVALvvy5bZY)
 * 🎥 [JavaScript setTimeOut and setInterval Explained — Theodore Anderson](https://www.youtube.com/watch?v=mVKfrWCOB60)

**[⬆ Back to Top](#table-of-contents)**

---

## 11. JavaScript Engines

### Articles

* 📜 [JavaScript Engines — Jen Looper](http://www.softwaremag.com/javascript-engines/)
* 📜 [JavaScript essentials: why you should know how the engine works - Rainer Hahnekamp](https://medium.freecodecamp.org/javascript-essentials-why-you-should-know-how-the-engine-works-c2cc0d321553)

### Summary

Just a javascript engine capable of running javascript code. Implementation of ECMAScript standards.

Modern js engines combine

* Fast application startup of interpreter
* Fast execution of compiler

To do this, js engines flag frequently used code and pass them to a compiler.

#### Inline Caching (IC)

The engine keep an shape of objects for effective memory lookup which is called object shape or 'type'. For two object to share same type, both object must have exactly the same properties in the same order.

```js
// which is faster since all the objects have the same shape
(() => {
  const han = {firstname: "Han", lastname: "Solo"};
  const luke = {firstname: "Luke", lastname: "Skywalker"};
  const yoda = {firstname: "", lastname: "Yoda"};
  const people = [
    han, luke, yoda
  ];
  const getName = (person) => person.lastname;
  console.time("engine");
  for(var i = 0; i < 1000 * 1000 * 1000; i++) {
    getName(people[i & 2]);
  }
  console.timeEnd("engine");
})();

// which is slower then previous one since all the objects are in different shape
(() => {
  const han = {
    firstname: "Han", lastname: "Solo",
    spacecraft: "Falcon"
  };
  const luke = {
    firstname: "Luke", lastname: "Skywalker",
    job: "Jedi"
  };
  const yoda = {lastname: "Yoda"};
  const people = [
    han, luke, yoda
  ];
  const getName = (person) => person.lastname;
  console.time("engine");
  for(var i = 0; i < 1000 * 1000 * 1000; i++) {
    getName(people[i & 2]);
  }
  console.timeEnd("engine");
})();

// here is a solution (make them in the same shape)
(() => {
  const han = {
    firstname: "Han", lastname: "Solo",
    job: "",
    spacecraft: "Falcon"
  };
  const luke = {
    firstname: "Luke", lastname: "Skywalker",
    job: "Jedi",
    spacecraft: ""
   };
  const yoda = {
    firstname: "Luke", lastname: "Yoda",
    job: "",
    spacecraft: ""
  };
  const people = [
    han, luke, yoda
  ];
  const getName = (person) => person.lastname;
  console.time("engine");
  for(var i = 0; i < 1000 * 1000 * 1000; i++) {
    getName(people[i & 2]);
  }
  console.timeEnd("engine");
})();
```

**[⬆ Back to Top](#table-of-contents)**

---

## 12. Bitwise Operators, Type Arrays and Array Buffers

### Articles

* 📜 [Using JavaScript’s Bitwise Operators in Real Life — ian m](https://codeburst.io/using-javascript-bitwise-operators-in-real-life-f551a731ff5)
* 📜 [JavaScript Bitwise Operators — w3resource](https://www.w3resource.com/javascript/operators/bitwise-operator.php)

### Summary

* AND `a & b`         : Both left and right operands are ones.
* OR `a | b`          : Either left or right operand is one.
* XOR `a ^ b`         : Not both left and right operand are one.
* NOT `~a`            : Flips the bits of its operand.
* Left shift `a << b` : Shifts b bits to the left, fill in zeros from the right.
* Sign-propagating right shift `a >> b` : Shifts b bits to the right, discarding bits shifted off, and keep the sign bit.
* Zero-fill right shift `a >>> b` : Shifts b bits to the right, discarding bits shifted off, and fill in zeros from the left.

```js
// an objects with multiple properties
// How to check foo2 == true && foo4 == true; What about other combination?
// We need 2**4 combination...
const hasFoo2andFoo4 = {
  foo1: false,
  foo2: true,
  foo3: false,
  foo4: true
}

// solution
const HAS_FOO1 = 1;       // 0001
const HAS_FOO2 = 1 << 1;  // 0010
const HAS_FOO3 = 1 << 2;  // 0100
const HAS_FOO4 = 1 << 3;  // 1000
let myBitNumber = 0;
if (myObject['foo1'] === true)
  myBitNumber = myBitNumber | HAS_FOO1;
if (myObject['foo2'] === true)
  myBitNumber = myBitNumber | HAS_FOO2;
if (myObject['foo3'] === true)
  myBitNumber = myBitNumber | HAS_FOO3;
if (myObject['foo4'] === true)
  myBitNumber = myBitNumber | HAS_FOO4;
console.log(myBitNumber.toString(2)); // 1010

// usage
if (myBitNumber & (HAS_FOO1 | HAS_FOO2)) {
  // True!
}
if (myBitNumber & (HAS_FOO1 | HAS_FOO3)) {
  // False
}
```

**[⬆ Back to Top](#table-of-contents)**

---

## 13. DOM and Layout Trees

### Articles

 * 📜 [What’s the Document Object Model, and why you should know how to use it — Leonardo Maldonado](https://medium.freecodecamp.org/whats-the-document-object-model-and-why-you-should-know-how-to-use-it-1a2d0bc5429d)
 * 📜 [Traversing the DOM with JavaScript — Zell Liew](https://zellwk.com/blog/dom-traversals/)

### Summary

The DOM (Document Object Model) : represents how HTML and XML documents are read by the browser. Javascript can interact with DOM node.

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Entendendo o DOM (Document Object Model)</title>
</head>
<body>
  <div class="container">
    <h1><time>00:00:00</time></h1>
    <button id="start">Start</button>
    <button id="stop">Stop</button>
    <button id="reset">Reset</button>
  </div>
</body>
</html>

<script>
// Methods
var myStart = document.getElementById('start');
var myContainer = document.getElementsByClassName('container');
var buttons = document.getElementsByTagName('button'); // returns button elements
var resetButton = document.querySelector('#reset'); // returns first element with 'reset'
var noneButton = document.querySelector('#none'); // returns null
var myButtons = document.querySelectorAll('#reset'); // returns elements with 'reset'

// Events
myStart.addEventListener('click', function(event) {
  alert('Start clicked!');
}, false);
resetButton.addEventListener('click', function(event) {
  console.log("Reset selected");
}, false);

// DOM Traversing
var container = document.querySelector('.container');
var getContainerChilds = container.childNodes; // children of .container
var getFirstChild = container.firstChild; // first child of .container
var getName = container.nodeName; // get node name, here 'div'
var getValue = container.nodeValue; // get text and comments of node, here null

// Element traversing, recommanded
var getParent = container.parentNode;
var getValue = container.firstElementChild;
var getValue = container.lastElementChild;
</script>
```

[see also](https://www.w3schools.com/jsref/prop_node_nodetype.asp)

**[⬆ Back to Top](#table-of-contents)**

---

## 14. Factories and Classes

### Articles

* 📜 [How To Use Classes in JavaScript — Tania Rascia](https://www.digitalocean.com/community/tutorials/understanding-classes-in-javascript)
* 📜 [Javascript Classes — Under The Hood — Majid](https://medium.com/tech-tajawal/javascript-classes-under-the-hood-6b26d2667677)
* 📜 [Class vs Factory function: exploring the way forward — Cristi Salcescu](https://medium.freecodecamp.org/class-vs-factory-function-exploring-the-way-forward-73258b6a8d15)

### Summary

#### ECMAScript 2015 class basic

Every object in JavaScript has a hidden internal property called [[Prototype]] that can be used to extend object properties and methods.

```js
// A class is also a function, just a 'syntactical sugar' over prototypes.
const x = function() {}
const y = class {}

// returns 'ƒ () { [native code] }'
Object.getPrototypeOf(x);
Object.getPrototypeOf(y);

// Initialize a constructor from a function
const withFunction = new x();

// Output
// x {}
// constructor: ƒ ()
console.log(constructorFromFunction);

// Initialize a constructor from a class
const withClass = new y();

// Output
// y {}
// constructor: class
console.log(constructorFromClass);
```

#### Defining ECMAScript 2015 class

```js
// with function, constructor is starting with upper letter by convention
function Hero(name, level) {
  this.name = name;
  this.level = level;
}
Hero.prototype.greet = function() {
  return `${this.name} says hello.`;
}

// with class keyword
class Hero {
  constructor(name, level) {
    this.name = name;
    this.level = level;
  }
  greet() {
    return `${this.name} says hello.`;
  }
}

// create an instance
const hero1 = new Hero('Varg', 1);
```

#### ECMAScript 2015 class inheritance

```js
// with function
function Mage(name, level, spell) {
  // Chain constructor with call
  Hero.call(this, name, level);

  // Add a new property
  this.spell = spell;
}

// with class and extends keyword
class Mage extends Hero {
  constructor(name, level, spell) {
    // Chain constructor with super
    super(name, level);

    // Add a new property
    this.spell = spell;
  }
}

// create an instance
const hero2 = new Mage('Lejon', 2, 'Magic Missile');
```

#### Static methods / Getters / Setters

```js
class Vehicle {
  constructor(model, color) {
    // use _ to prevent name collision with getter and setter
    this._model = model;
    this.color = color;
  }

  static getColor(v) {
    return v.color;
  }

  get model() {
    return this._model;
  }

  set model(value) {
    this._model = value;
  }
}

let car = new Vehicle("Honda", "Purple");

console.log(Vehicle.getColor(car)); // static methods, get "purple"
console.log(car.model); // getter, get "Honda"
car.model = "Boat"; // setter called
console.log(car.model); // getter, get "Boat"
```

#### Factory function

```js
// with class keyword, private members are not encapsulated
class TodoModel {
  constructor(){
    this.todos = [];
  }

  addToPrivateList(){
    console.log("addToPrivateList");
  }
  add(val) { this.todos.push(val); }
  get() { return this.todos; }
  reload(){ this.todos = []; }
}
var todoModel = new TodoModel();
console.log(todoModel.todos);     // []
todoModel.addToPrivateList();     // addToPrivateList

// with factory method, private members are encapsulated
function TodoModel(){
  var todos = [];

  function addToPrivateList(){
    console.log("addToPrivateList");
  }

  function add(val) { todos.push(val); }
  function get() { return todos; }
  function reload(){ todos = []; }

  // todos is only accessable by add, get, reload
  return Object.freeze({
      add,
      get,
      reload
  });
}
var todoModel = TodoModel();
console.log(todoModel.todos);     // undefined
todoModel.addToPrivateList();     // taskModel.addToPrivateList is not a function
```

**[⬆ Back to Top](#table-of-contents)**

---

## 15. this, call, apply and bind

### Articles

* 📜 [WTF is this - Understanding the this keyword, call, apply, and bind in JavaScript — Tyler McGinnis](https://tylermcginnis.com/this-keyword-call-apply-bind-javascript/)

### Summary

Binding of `this`

#### Implicit Binding

```js
const user = {
  name: 'Tyler',
  age: 27,
  greet() {
    alert(`Hello, my name is ${this.name}`)
  }
}

user.greet(); // Tyler
```

#### Explicit Binding

```js
function greet (l1, l2, l3) {
  alert(
    `Hello, my name is ${this.name} and I know ${l1}, ${l2}, and ${l3}`
  )
}
const user = {
  name: 'Tyler',
  age: 27,
}
const languages = ['JavaScript', 'Ruby', 'Python']

// binding with 'call', 'apply', 'bind' keyword
// All alerts "Hello, my name is Tyler and I know JavaScript, Ruby, and Python"
greet.call(user, languages[0], languages[1], languages[2]);
greet.apply(user, languages);
const newFn = greet.bind(user, languages[0], languages[1], languages[2]);
newFn();
```

#### new Binding

```js
function User (name, age) {
  this.name = name
  this.age = age
}

// binding with 'new' keyword
const me = new User('Tyler', 27)
```

#### Lexical Binding

```js
const user = {
  name: 'Tyler',
  age: 27,
  languages: ['JavaScript', 'Ruby', 'Python'],
  greet() {
    const hello = `Hello, my name is ${this.name} and I know`
    const langs = this.languages.reduce(function (str, lang, i) {
      // here, this is referencing nothing
      // to resolve it, use '.bind(this)'
      if (i === this.languages.length - 1) {
        return `${str} and ${lang}.`
      }
      return `${str} ${lang},`
    }.bind(this), "")

    alert(hello + langs)
  }
}

const user = {
  name: 'Tyler',
  age: 27,
  languages: ['JavaScript', 'Ruby', 'Python'],
  greet() {
    const hello = `Hello, my name is ${this.name} and I know`
    const langs = this.languages.reduce((str, lang, i) => {
      // here, this is lexically binded
      if (i === this.languages.length - 1) {
        return `${str} and ${lang}.`
      }
      return `${str} ${lang},`
    }, "")

    alert(hello + langs)
  }
}
```

#### window Binding

```js
function sayAge () {
  console.log(`My age is ${this.age}`)
}
const user = {
  name: 'Tyler',
  age: 27
}

sayAge() // My age is undefined
window.age = 27
sayAge() // My age is 27, automatically binded with window object
```

```js
'use strict'

...

// in case of strict mode, it's not binded with window object
sayAge() // TypeError: Cannot read property 'age' of undefined
```

**[⬆ Back to Top](#table-of-contents)**

---

## 16. new, Constructor, instanceof and Instances

### Articles

* 📜 [Use Constructor Functions — Openclassrooms](https://openclassrooms.com/en/courses/3523231-learn-to-code-with-javascript/4379006-use-constructor-functions)
* 📜 [Beyond `typeof` and `instanceof`: simplifying dynamic type checks — Dr. Axel Rauschmayer](http://2ality.com/2017/08/type-right.html)

### Summary


#### Constructor function

```js
// with init
var Pastry = {
  // initialize the pastry
  init: function (type, flavor) {
    this.type = type;
    this.flavor = flavor;
  },
}

// with constructor function
function Pastry(type, flavor) {
  this.type = type;
  this.flavor = flavor;
}

// create in classic way
const withClassic = Object.create(Pastry);
withClassic.init("A", "a");

// create in es6 way
const withEs6 = new Pastry("A", "a");
```

#### typeof vs instanceof

```js
// typeof checks if a value is an element of a primitive type
if (typeof value === 'string')

// instanceof checks if a value is an instance of a class or a constructor function
if (value instanceof Map)
if (value.constructor instanceof Map)
if (value.constructor.name instanceof Map)

// use instanceof on primitive type
class PrimitiveNumber {
  static [Symbol.hasInstance](x) {
    return typeof x === 'number';
  }
}
console.log(123 instanceof PrimitiveNumber); // true
```

**[⬆ Back to Top](#table-of-contents)**

---

## 17. Prototype Inheritance and Prototype Chain

### Articles

 * 📜 [Javascript : Prototype vs Class — Valentin PARSY](https://medium.com/@parsyval/javascript-prototype-vs-class-a7015d5473b)
 * 📜 [JavaScript engine fundamentals: optimizing prototypes — Mathias Bynens](https://mathiasbynens.be/notes/prototypes)
 * 📜 [JavaScript Prototype — NC Patro](https://codeburst.io/javascript-prototype-cb29d82b8809)
 * 📜 [Prototype in Javascript — Sandeep Ranjan](https://www.codementor.io/sandeepranjan2007/prototype-in-javascipt-knbve0lqo)
 * 📜 [Prototypes in JavaScript — Rupesh Mishra](https://hackernoon.com/prototypes-in-javascript-5bba2990e04b)
 * 📜 [Prototype in JavaScript: it’s quirky, but here’s how it works — Pranav Jindal](https://medium.freecodecamp.org/prototype-in-js-busted-5547ec68872)
 * 📜 [Inheritance and the prototype chain — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
 * 📜 [Understanding JavaScript: Prototype and Inheritance — Alexander Kondov](https://hackernoon.com/understanding-javascript-prototype-and-inheritance-d55a9a23bde2)
 * 📜 [Prototypal Inheritance — JavaScript.Info](https://javascript.info/prototype-inheritance)
 * 📜 [How To Work with Prototypes and Inheritance in JavaScript — Tania Rascia](https://www.digitalocean.com/community/tutorials/understanding-prototypes-and-inheritance-in-javascript)
 * 📜 [Master JavaScript Prototypes & Inheritance — Arnav Aggarwal](https://codeburst.io/master-javascript-prototypes-inheritance-d0a9a5a75c4e)
 * 📜 [You Don't Know JS [Book] Chapter 5: Prototypes — Kyle Simpson](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch5.md)
 * 📜 [JavaScript’s Prototypal Inheritance Explained Using CSS — Nash Vail](https://medium.freecodecamp.org/understanding-prototypal-inheritance-in-javascript-with-css-93b2fcda75e4)
 * 📜 [Prototypal Inheritance in JavaScript — Jannis Redmann](https://gist.github.com/derhuerst/a585c4916b1c361cc6f0)
 * 📜 [Classical and Prototypical Inheritance in JavaScript — Danny Cornelisse](http://www.competa.com/blog/classical-prototypical-inheritance-javascript/)
 * 📜 [Demystifying ES6 Classes And Prototypal Inheritance ― Neo Ighodaro](https://scotch.io/tutorials/demystifying-es6-classes-and-prototypal-inheritance)
 * 📜 [Intro To Prototypal Inheritance — Dharani Jayakanthan](https://dev.to/danny/intro-to-prototypal-inheritance---js-9di)
 * 📜 [Classes in JavaScript - Explained — Daniel Li](http://blog.brew.com.hk/classes-in-javascript-explained/)
 * 📜 [You Don't Know JS: this & Object Prototypes — Kyle Simpson](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch4.md)

 ### Videos

 * 🎥 [Javascript Prototype Inheritance — Avelx](https://www.youtube.com/watch?v=sOrtAjyk4lQ)
 * 🎥 [JavaScript Prototype Inheritance Explained pt. I — techsith](https://www.youtube.com/watch?v=7oNWNlMrkpc)
 * 🎥 [JavaScript Prototype Inheritance Explained pt. II — techsith](https://www.youtube.com/watch?v=uIlj6_z_wL8)
 * 🎥 [JavaScript Prototype Inheritance Explained — Kyle Robinson](https://www.youtube.com/watch?v=qMO-LTOrJaE)
 * 🎥 [Advanced Javascript - Prototypal Inheritance In 1 Minute](https://www.youtube.com/watch?v=G6l5CHl67HQ)
 * 🎥 [An Overview Of Classical Javascript Classes and Prototypal Inheritance — Pentacode](https://www.youtube.com/watch?v=phwzuiJJPpQ)
 * 🎥 [Object Oriented JavaScript - Prototype — The Net Ninja](https://www.youtube.com/watch?v=4jb4AYEyhRc)
 * 🎥 [Prototype in JavaScript — kudvenkat](https://www.youtube.com/watch?v=2rkEbcptR64)
 * 🎥 [JavaScript Using Prototypes — O'Reilly](https://www.youtube.com/watch?v=oCwCcNvaXAQ)
 * 🎥 [A Beginner's Guide to Javascript's Prototype — Tyler Mcginnis](https://www.youtube.com/watch?v=XskMWBXNbp0)
 * 🎥 [Prototypes in Javascript - p5.js Tutorial — The Coding Train](https://www.youtube.com/watch?v=hS_WqkyUah8)


**[⬆ Back to Top](#table-of-contents)**

---

## 18. Object.create and Object.assign

### Articles

 * 📜 [Object.create() — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
 * 📜 [Object.create in JavaScript — Rupesh Mishra](https://hackernoon.com/object-create-in-javascript-fa8674df6ed2)
 * 📜 [Object.create(): the New Way to Create Objects in JavaScript — Rob Gravelle](https://www.htmlgoodies.com/beyond/javascript/object.create-the-new-way-to-create-objects-in-javascript.html)
 * 📜 [Basic Inheritance with Object.create — Joshua Clanton](http://adripofjavascript.com/blog/drips/basic-inheritance-with-object-create.html)
 * 📜 [Object.create() In JavaScript — GeeksforGeeks](https://www.geeksforgeeks.org/object-create-javascript/)
 * 📜 [Understanding the difference between Object.create() and the new operator — Jonathan Voxland](https://medium.com/@jonathanvox01/understanding-the-difference-between-object-create-and-the-new-operator-b2a2f4749358)
 * 📜 [JavaScript Object Creation: Patterns and Best Practices — Jeff Mott](https://www.sitepoint.com/javascript-object-creation-patterns-best-practises/)
 * 📜 [Dealing With Objects in JavaScript With Object.assign, Object.keys and hasOwnProperty](https://alligator.io/js/dealing-with-objects/)
 * 📜 [Copying Objects in JavaScript ― Orinami Olatunji](https://scotch.io/bar-talk/copying-objects-in-javascript)
 * 📜 [Object.assign() — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
 * 📜 [JavaScript: Object.assign() — Thiago S. Adriano](https://codeburst.io/javascript-object-assign-bc9696dcbb6e)
 * 📜 [How to deep clone a JavaScript Object — Flavio Copes](https://flaviocopes.com/how-to-clone-javascript-object/)

 ### Videos

 * 🎥 [Object.assign() explained — Aaron Writes Code](https://www.youtube.com/watch?v=aw7NfYhR5rc)
 * 🎥 [Object.assign() Method — techsith](https://www.youtube.com/watch?v=9Ky4X6inpi4)

**[⬆ Back to Top](#table-of-contents)**

---

## 19. map, reduce, filter

### Articles

 * 📜 [JavaScript Functional Programming — map, filter and reduce — Bojan Gvozderac](https://medium.com/jsguru/javascript-functional-programming-map-filter-and-reduce-846ff9ba492d)
 * 📜 [Learn map, filter and reduce in Javascript — João Miguel Cunha](https://medium.com/@joomiguelcunha/learn-map-filter-and-reduce-in-javascript-ea59009593c4)
 * 📜 [JavaScript’s Map, Reduce, and Filter — Dan Martensen](https://danmartensen.svbtle.com/javascripts-map-reduce-and-filter)
 * 📜 [How to Use Map, Filter, & Reduce in JavaScript — Peleke Sengstacke](https://code.tutsplus.com/tutorials/how-to-use-map-filter-reduce-in-javascript--cms-26209)
 * 📜 [JavaScript — Learn to Chain Map, Filter, and Reduce — Brandon Morelli](https://codeburst.io/javascript-learn-to-chain-map-filter-and-reduce-acd2d0562cd4)
 * 📜 [Javascript data structure with map, reduce, filter and ES6 — Deepak Gupta](https://codeburst.io/write-beautiful-javascript-with-%CE%BB-fp-es6-350cd64ab5bf)
 * 📜 [Understanding map, filter and reduce in Javascript — Luuk Gruijs](https://hackernoon.com/understanding-map-filter-and-reduce-in-javascript-5df1c7eee464)
 * 📜 [Functional Programming in JS: map, filter, reduce (Pt. 5) — Omer Goldberg](https://hackernoon.com/functional-programming-in-js-map-filter-reduce-pt-5-308a205fdd5f)
 * 📜 [JavaScript: Map, Filter, Reduce — William S. Vincent](https://wsvincent.com/functional-javascript-map-filter-reduce/)
 * 📜 [Arrow Functions: Fat and Concise Syntax in JavaScript — Kyle Pennell](https://www.sitepoint.com/es6-arrow-functions-new-fat-concise-syntax-javascript/)
 * 📜 [JavaScript: Arrow Functions for Beginners — Brandon Morelli](https://codeburst.io/javascript-arrow-functions-for-beginners-926947fc0cdc)
 * 📜 [When (and why) you should use ES6 arrow functions — and when you shouldn’t — Cynthia Lee](https://medium.freecodecamp.org/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26)
 * 📜 [JavaScript — Learn & Understand Arrow Functions — Brandon Morelli](https://codeburst.io/javascript-learn-understand-arrow-functions-fe2083533946)
 * 📜 [(JavaScript )=> Arrow functions — sigu](https://medium.com/podiihq/javascript-arrow-functions-27d4c3334b83)
 * 📜 [A possibility to use Async/Await for filter(), find(), forEach(), map() and reduce() methods in Array — Ruwan Geeganage](https://www.linkedin.com/pulse/possibility-use-asyncawait-filter-find-foreach-map-reduce-geeganage/)
 * 📜 [Javascript.reduce() — Paul Anderson](https://medium.com/@panderson.dev/javascript-reduce-79aab078da23)
 * 📜 [Why you should replace forEach with map and filter in JavaScript — Roope Hakulinen](https://gofore.com/en/why-you-should-replace-foreach/)
 * 📜 [Simplify your JavaScript – Use .map(), .reduce(), and .filter() — Etienne Talbot](https://medium.com/poka-techblog/simplify-your-javascript-use-map-reduce-and-filter-bd02c593cc2d)
 * 📜 [JavaScript’s Reduce Method Explained By Going On a Diet — Kevin Kononenko](https://blog.codeanalogies.com/2018/07/24/javascripts-reduce-method-explained-by-going-on-a-diet/)

 ### Videos

 * 🎥 [Map, Filter and Reduce — Lydia Hallie](https://www.youtube.com/watch?v=UXiYii0Y7Nw)
 * 🎥 [Functional JavaScript: Map, forEach, Reduce, Filter — Theodore Anderson](https://www.youtube.com/watch?v=vytzLlY_wmU)
 * 🎥 [JavaScript Array superpowers: Map, Filter, Reduce (part I) — Michael Rosata](https://www.youtube.com/watch?v=qTeeVd8hOFY)
 * 🎥 [JavaScript Array superpowers: Map, Filter, Reduce (part 2) — Michael Rosata](https://www.youtube.com/watch?v=gIm9xLYudL0)
 * 🎥 [JavaScript Higher Order Functions - Filter, Map, Sort & Reduce — Epicop](https://www.youtube.com/watch?v=zYBeEPxNSbw)
 * 🎥 [[Array Methods 2/3] .filter + .map + .reduce — CodeWithNick](https://www.youtube.com/watch?v=4qWlqD0yYTU)
 * 🎥 [Arrow functions in JavaScript - What, Why and How — Fun Fun Function](https://www.youtube.com/watch?v=6sQDTgOqh-I)
 * 🎥 [Learning Functional Programming with JavaScript — Anjana Vakil - JSUnconf](https://www.youtube.com/watch?v=e-5obm1G_FY&t=1521s)
 * 🎥 [Map - Parte 2 JavaScript - Fun Fun Function](https://www.youtube.com/watch?v=bCqtb-Z5YGQ&t=17s)
 * 🎥 [Reduce basics - Part 3 of FP in JavaScript - Fun Fun Function](https://www.youtube.com/watch?v=Wl98eZpkp-c)
 * 🎥 [Reduce Advanced - Part 4 of FP in JavaScript - Fun Fun Function](https://www.youtube.com/watch?v=1DMolJ2FrNY&t=621s)

**[⬆ Back to Top](#table-of-contents)**

---

## 20. Pure Functions, Side Effects and State Mutation

### Articles

 * 📜 [Javascript and Functional Programming — Pure Functions — Omer Goldberg](https://hackernoon.com/javascript-and-functional-programming-pt-3-pure-functions-d572bb52e21c)
 * 📜 [Master the JavaScript Interview: What is a Pure Function? — Eric Elliott](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)
 * 📜 [JavaScript: What Are Pure Functions And Why Use Them? — James Jeffery](https://medium.com/@jamesjefferyuk/javascript-what-are-pure-functions-4d4d5392d49c)
 * 📜 [Pure functions in JavaScript — @nicoespeon](http://www.nicoespeon.com/en/2015/01/pure-functions-javascript/)
 * 📜 [Functional Programming: Pure Functions — Arne Brasseur](https://www.sitepoint.com/functional-programming-pure-functions/)
 * 📜 [Pure Functions In Javascript — Krunal](https://appdividend.com/2017/04/10/pure-functions-in-javascript/)
 * 📜 [Making your JavaScript Pure — Jack Franklin](https://alistapart.com/article/making-your-javascript-pure)
 * 📜 [To mutate, or not to mutate, in JavaScript](https://slemgrim.com/mutate-or-not-to-mutate/)
 * 📜 [Arrays, Objects and Mutations — Federico Knüssel](https://medium.com/@fknussel/arrays-objects-and-mutations-6b23348b54aa)
 * 📜 [The State of Immutability — Maciej Sikora](https://medium.com/dailyjs/the-state-of-immutability-169d2cd11310)
 * 📜 [How to deal with dirty side effects in your pure functional JavaScript — James Sinclair](https://jrsinclair.com/articles/2018/how-to-deal-with-dirty-side-effects-in-your-pure-functional-javascript/)
 * 📜 [Preventing Side Effects in JavaScript — David Walsh](https://davidwalsh.name/preventing-sideeffects-javascript)
 * 📜 [Wielding Pure Functions in JavaScript and Function Composition — Peleke Sengstacke
](https://scotch.io/tutorials/wielding-pure-functions-in-javascript-and-function-composition)
 * 📜 [JavaScript: Pure Functions — William S. Vincent](https://wsvincent.com/javascript-pure-functions/)
 * 📜 [Functional programming paradigms in modern JavaScript: Pure functions — Alexander Kondov](https://hackernoon.com/functional-programming-paradigms-in-modern-javascript-pure-functions-797d9abbee1)

 ### Videos

 * 🎥 [Pure Functions — Hexlet](https://www.youtube.com/watch?v=dZ41D6LDSBg)
 * 🎥 [Pure Functions - Functional Programming in JavaScript — Paul McBride](https://www.youtube.com/watch?v=Jh_Uzqzz_wM)
 * 🎥 [JavaScript Pure Functions — Seth Alexander](https://www.youtube.com/watch?v=frT3H-eBmPc)
 * 🎥 [JavaScript Pure vs Impure Functions Explained — Theodore Anderson](https://www.youtube.com/watch?v=AHbRVJzpB54)
 * 🎥 [Pure Functions - Programação Funcional: Parte 1 - Fun Fun Function](https://www.youtube.com/watch?v=BMUiFMZr7vk)

**[⬆ Back to Top](#table-of-contents)**

---

## 21. Closures

### Articles

 * 📜 [Closures — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
 * 📜 [I never understood JavaScript closures — Olivier De Meulder](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)
 * 📜 [Closure — JavaScript.Info](https://javascript.info/closure)
 * 📜 [Understand JavaScript Closures With Ease — Richard Bovell](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
 * 📜 [Understanding JavaScript Closures — Codesmith](https://codeburst.io/understanding-javascript-closures-da6aab330302)
 * 📜 [Understand Closures in JavaScript — Brandon Morelli](https://codeburst.io/understand-closures-in-javascript-d07852fa51e7)
 * 📜 [A simple guide to help you understand closures in JavaScript — Prashant Ram](https://medium.freecodecamp.org/javascript-closures-simplified-d0d23fa06ba4)
 * 📜 [Understanding JavaScript Closures: A Practical Approach — Paul Upendo](https://scotch.io/tutorials/understanding-javascript-closures-a-practical-approach)
 * 📜 [Understanding JavaScript: Closures — Alexander Kondov](https://hackernoon.com/understanding-javascript-closures-4188edf5ea1b)
 * 📜 [How to use JavaScript closures with confidence — Léna Faure](https://hackernoon.com/how-to-use-javascript-closures-with-confidence-85cd1f841a6b)
 * 📜 [JavaScript closures by example — tyler](https://howchoo.com/g/mge2mji2mtq/javascript-closures-by-example)
 * 📜 [JavaScript — Closures and Scope — Alex Aitken](https://codeburst.io/javascript-closures-and-scope-3784c75b9290)
 * 📜 [Discover the power of closures in JavaScript — Cristi Salcescu](https://medium.freecodecamp.org/discover-the-power-of-closures-in-javascript-5c472a7765d7)
 * 📜 [Simplified JavaScript: Getting Started with Closures — Code Like A Girl](https://code.likeagirl.io/simplified-javascript-getting-started-with-closures-f40f65317d00)
 * 📜 [The Ultimate Guide to Hoisting, Scopes, and Closures in JavaScript — Tyler McGinnis](https://tylermcginnis.com/ultimate-guide-to-execution-contexts-hoisting-scopes-and-closures-in-javascript/)
 * 📜 [Getting Closure — RealLifeJS](http://reallifejs.com/the-meat/getting-closure/)
 * 📜 [Understanding Closures in JavaScript — Sukhjinder Arora](https://blog.bitsrc.io/a-beginners-guide-to-closures-in-javascript-97d372284dda)

 ### Videos

 * 🎥 [Javascript Closure — techsith](https://www.youtube.com/watch?v=71AtaJpJHw0)
 * 🎥 [Closures — Fun Fun Function](https://www.youtube.com/watch?v=CQqwU2Ixu-U)
 * 🎥 [Closures in JavaScript — techsith](https://www.youtube.com/watch?v=-xqJo5VRP4A)
 * 🎥 [JavaScript Closures 101: What is a closure? — JavaScript Tutorials](https://www.youtube.com/watch?v=yiEeiMN2Khs)
 * 🎥 [Closures — freeCodeCamp](https://www.youtube.com/watch?v=1JsJx1x35c0)
 * 🎥 [JavaScript Closures — CodeWorkr](https://www.youtube.com/watch?v=-rLrGAXK8WE)

**[⬆ Back to Top](#table-of-contents)**

---

## 22. High Order Functions

### Articles

 * 📜 [Higher-Order Functions — Eloquent JavaScript [Book]](https://eloquentjavascript.net/05_higher_order.html)
 * 📜 [Higher-Order Functions in JavaScript — M. David Green](https://www.sitepoint.com/higher-order-functions-javascript/)
 * 📜 [Higher Order Functions: Using Filter, Map and Reduce for More Maintainable Code — Guido Schmitz](https://medium.freecodecamp.org/higher-order-functions-in-javascript-d9101f9cf528)
 * 📜 [First-class and Higher Order Functions: Effective Functional JavaScript — Hugo Di Francesco](https://hackernoon.com/effective-functional-javascript-first-class-and-higher-order-functions-713fde8df50a)
 * 📜 [Higher Order Functions in JavaScript — John Hannah](https://www.lullabot.com/articles/higher-order-functions-in-javascript)
 * 📜 [Higher-order Functions — Richard Bovell](http://javascriptissexy.com/tag/higher-order-functions/)
 * 📜 [Higher Order Functions in JavaScript — Zsolt Nagy](http://www.zsoltnagy.eu/higher-order-functions-in-javascript/)
 * 📜 [Fun With Higher Order Functions In JavaScript — Derick](https://derickbailey.com/2015/10/21/fun-with-higher-order-functions-in-javascript/)
 * 📜 [Just a reminder on how to use high order functions — Pedro Filho](https://github.com/pedroapfilho/high-order-functions)
 * 📜 [Understanding Higher-Order Functions in JavaScript — Sukhjinder Arora](https://blog.bitsrc.io/understanding-higher-order-functions-in-javascript-75461803bad)

 ### Videos

 * 🎥 [JavaScript Higher Order Functions & Arrays — Traversy Media](https://www.youtube.com/watch?v=rRgD1yVwIvE)
 * 🎥 [Higher Order Functions — Fun Fun Function](https://www.youtube.com/watch?v=BMUiFMZr7vk)
 * 🎥 [Higher Order Functions in Javascript — Raja Yogan](https://www.youtube.com/watch?v=dTlpYnmBW9I)
 * 🎥 [Higher Order Iterators in JavaScript — Fun Fun Function](https://www.youtube.com/watch?v=GYRMNp1SKXA)
 * 🎥 [Higher Order Functions in JavaScript — The Coding Train](https://www.youtube.com/watch?v=H4awPsyugS0)

**[⬆ Back to Top](#table-of-contents)**

---

## 23. Recursion

### Articles

 * 📜 [Recursion in JavaScript — Kevin Ennis](https://medium.freecodecamp.org/recursion-in-javascript-1608032c7a1f)
 * 📜 [Understanding Recursion in JavaScript — Zak Frisch](https://medium.com/@zfrisch/understanding-recursion-in-javascript-992e96449e03)
 * 📜 [Learn and Understand Recursion in JavaScript — Brandon Morelli](https://codeburst.io/learn-and-understand-recursion-in-javascript-b588218e87ea)
 * 📜 [Recursion in Functional JavaScript — M. David Green](https://www.sitepoint.com/recursion-functional-javascript/)
 * 📜 [Programming with JS: Recursion — Alexander Kondov](https://hackernoon.com/programming-with-js-recursion-31371e2bf808)
 * 📜 [Anonymous Recursion in JavaScript — simo](https://dev.to/simov/anonymous-recursion-in-javascript)
 * 📜 [Recursion, iteration and tail calls in JS — loverajoel](http://www.jstips.co/en/javascript/recursion-iteration-and-tail-calls-in-js/)
 * 📜 [Understanding Recursion in JavaScript with Confidence — Jay](https://www.thecodingdelight.com/understanding-recursion-javascript/)

 ### Videos

 * 🎥 [Recursion In JavaScript — techsith](https://www.youtube.com/watch?v=VtG0WAUvq2w)
 * 🎥 [Recursion — Fun Fun Function](https://www.youtube.com/watch?v=k7-N8R0-KY4)
 * 🎥 [Recursion and Recursive Functions — Hexlet](https://www.youtube.com/watch?v=vLhHyGTkjCs)
 * 🎥 [Recursion: Recursion() — JS Monthly — Lucas da Costa](https://www.youtube.com/watch?v=kGXVsd8pBLw)
 * 🎥 [Recursive Function in JavaScript — kudvenkat](https://www.youtube.com/watch?v=uyjsR9eNTIw)
 * 🎥 [What on Earth is Recursion? — Computerphile](https://www.youtube.com/watch?v=Mv9NEXX1VHc)
 * 🎥 [Javascript Tutorial 34: Introduction To Recursion — codedamn](https://www.youtube.com/watch?v=9NO5dXSlbv8)
 * 🎥 [Recursion, Iteration, and JavaScript: A Love Story | JSHeroes 2018 — Anjana Vakil](https://www.youtube.com/watch?v=FmiQr4nfoPQ)

**[⬆ Back to Top](#table-of-contents)**

---

## 24. Collections and Generators

### Articles

 * 📜 [ES6 In Depth: Collections — Jason Orendorff](https://hacks.mozilla.org/2015/06/es6-in-depth-collections/)
 * 📜 [ES6 Collections: Using Map, Set, WeakMap, WeakSet — Kyle Pennell](https://www.sitepoint.com/es6-collections-map-set-weakmap-weakset/)
 * 📜 [ES6 WeakMaps, Sets, and WeakSets in Depth — Nicolás Bevacqua](https://ponyfoo.com/articles/es6-weakmaps-sets-and-weaksets-in-depth)
 * 📜 [Introduction to Sets in JavaScript — Alligator.io](https://alligator.io/js/sets-introduction/)
 * 📜 [Introduction to Maps in JavaScript — Alligator.io](https://alligator.io/js/maps-introduction/)
 * 📜 [Map, Set, WeakMap and WeakSet — JavaScript.Info](https://javascript.info/map-set-weakmap-weakset)
 * 📜 [Maps in ES6 - A Quick Guide — Ben Mildren](https://dev.to/mildrenben/maps-in-es6---a-quick-guide-35pk)
 * 📜 [ES6 — Set vs Array — What and when? — Maya Shavin](https://medium.com/front-end-hacking/es6-set-vs-array-what-and-when-efc055655e1a)
 * 📜 [ES6 — Map vs Object — What and when? — Maya Shavin](https://medium.com/front-end-hacking/es6-map-vs-object-what-and-when-b80621932373)
 * 📜 [ES6: Working with Sets in JavaScript — Dead Code Rising](http://www.deadcoderising.com/es6-working-with-sets-in-javascript/)
 * 📜 [Array vs Set vs Map vs Object — Real-time use cases in Javascript (ES6/ES7) — Rajesh Babu](https://codeburst.io/array-vs-set-vs-map-vs-object-real-time-use-cases-in-javascript-es6-47ee3295329b)
 * 📜 [How to create an array of unique values in JavaScript using Sets — Claire Parker-Jones](https://dev.to/claireparker/how-to-create-an-array-of-unique-values-in-javascript-using-sets-5dg6)
 * 📜 [What You Should Know About ES6 Maps — Just Chris](https://hackernoon.com/what-you-should-know-about-es6-maps-dc66af6b9a1e)
 * 📜 [ES6 Maps in Depth — Nicolás Bevacqua](https://ponyfoo.com/articles/es6-maps-in-depth)
 * 📜 [Generator — MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
* 📜 [What are JavaScript Generators and how to use them — Vladislav Stepanov](https://codeburst.io/what-are-javascript-generators-and-how-to-use-them-c6f2713fd12e)
* 📜 [Understanding JavaScript Generators With Examples  — Arfat Salman](https://codeburst.io/understanding-generators-in-es6-javascript-with-examples-6728834016d5)
* 📜 [The Basics of ES6 Generators — Kyle Simpson](https://davidwalsh.name/es6-generators)



 ### Videos

 * 🎥 [JavaScript ES6 / ES2015 Set, Map, WeakSet and WeakMap — Traversy Media](https://www.youtube.com/watch?v=ycohYSx5h9w)
 * 🎥 [The Differences between ES6 Maps and Sets — Steve Griffith](https://www.youtube.com/watch?v=m4abICrldQI)
 * 🎥 [Javascript Generators - THEY CHANGE EVERYTHING - ES6 Generators Harmony Generators — LearnCode.academy](https://www.youtube.com/watch?v=QO07THdLWQo)

**[⬆ Back to Top](#table-of-contents)**

---

## 25. Promises

### Articles

 * 📜 [Promise — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
 * 📜 [JavaScript Promises for Dummies ― Jecelyn Yeen](https://scotch.io/tutorials/javascript-promises-for-dummies)
 * 📜 [Understanding promises in JavaScript — Gokul N K](https://hackernoon.com/understanding-promises-in-javascript-13d99df067c1)
 * 📜 [Master the JavaScript Interview: What is a Promise? — Eric Elliott](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)
 * 📜 [An Overview of JavaScript Promises — Sandeep Panda](https://www.sitepoint.com/overview-javascript-promises/)
 * 📜 [How to use Promises in JavaScript — Prashant Ram](https://medium.freecodecamp.org/promises-in-javascript-explained-277b98850de)
 * 📜 [Implementing Promises In JavaScript — Maciej Cieslar](https://medium.freecodecamp.org/how-to-implement-promises-in-javascript-1ce2680a7f51)
 * 📜 [JavaScript: Promises explained with simple real life analogies — Shruti Kapoor](https://codeburst.io/javascript-promises-explained-with-simple-real-life-analogies-dd6908092138)
 * 📜 [Promises for Asynchronous Programming — Exploring JS](http://exploringjs.com/es6/ch_promises.html)
 * 📜 [JavaScript Promises Explained By Gambling At A Casino — Kevin Kononenko](https://blog.codeanalogies.com/2018/08/26/javascript-promises-explained-by-gambling-at-a-casino/)
 * 📜 [ES6 Promises: Patterns and Anti-Patterns — Bobby Brennan](https://medium.com/datafire-io/es6-promises-patterns-and-anti-patterns-bbb21a5d0918)
 * 📜 [A Simple Guide to ES6 Promises — Brandon Morelli](https://codeburst.io/a-simple-guide-to-es6-promises-d71bacd2e13a)
 * 📜 [The ES6 Promises — Manoj Singh Negi](https://codeburst.io/the-es6-promises-87a979ab27e4)
 * 📜 [ES6 Promises in Depth — Nicolás Bevacqua](https://ponyfoo.com/articles/es6-promises-in-depth)
 * 📜 [Playing with Javascript Promises: A Comprehensive Approach — Rajesh Babu](https://codeburst.io/playing-with-javascript-promises-a-comprehensive-approach-25ab752c78c3)

 ### Videos

 * 🎥 [Let's Learn ES6 - Promises — Ryan Christiani](https://www.youtube.com/watch?v=vQ3MoXnKfuQ)
 * 🎥 [JavaScript ES6 / ES2015 Promises — Traversy Media](https://www.youtube.com/watch?v=XJEHuBZQ5dU)
 * 🎥 [Promises — Fun Fun Function](https://www.youtube.com/watch?v=2d7s3spWAzo)
 * 🎥 [Error Handling Promises in JavaScript — Fun Fun Function](https://www.youtube.com/watch?v=f8IgdnYIwOU)
 * 🎥 [Promises Part 1 - Topics of JavaScript/ES6 — The Coding Train](https://www.youtube.com/watch?v=QO4NXhWo_NM)

**[⬆ Back to Top](#table-of-contents)**

---

## 26. async/await

### Articles

 * 📜 [async/await — JavaScript.Info](https://javascript.info/async-await)
 * 📜 [Understanding async/await in Javascript — Gokul N K](https://hackernoon.com/understanding-async-await-in-javascript-1d81bb079b2c)
 * 📜 [Asynchronous Programming — Eloquent JavaScript](https://eloquentjavascript.net/11_async.html)
 * 📜 [Exploring Async/Await Functions in JavaScript — Alligator.io](https://alligator.io/js/async-functions/)
 * 📜 [Asynchronous Javascript using async/await — Joy Warugu](https://scotch.io/tutorials/asynchronous-javascript-using-async-await)
 * 📜 [Modern Asynchronous JavaScript with async/await — Flavio Copes](https://flaviocopes.com/javascript-async-await/)
 * 📜 [Asynchronous JavaScript: From Callback Hell to Async and Await — Demir Selmanovic](https://www.toptal.com/javascript/asynchronous-javascript-async-await-tutorial)
 * 📜 [Javascript — ES8 Introducing async/await Functions — Ben Garrison](https://medium.com/@_bengarrison/javascript-es8-introducing-async-await-functions-7a471ec7de8a)
 * 📜 [How to escape async/await hell — Aditya Agarwal](https://medium.freecodecamp.org/avoiding-the-async-await-hell-c77a0fb71c4c)
 * 📜 [Understanding JavaScript’s async await — Nicolás Bevacqua](https://ponyfoo.com/articles/understanding-javascript-async-await)
 * 📜 [JavaScript Async/Await: Serial, Parallel and Complex Flow — TechBrij](https://techbrij.com/javascript-async-await-parallel-sequence)
 * 📜 [Asynchronous Programming — Exploring JS](http://exploringjs.com/es6/ch_async.html)
 * 📜 [From JavaScript Promises to Async/Await: why bother? — Chris Nwamba](https://blog.pusher.com/promises-async-await/)
 * 📜 [Flow Control in Modern JS: Callbacks to Promises to Async/Await — Craig Buckler](https://www.sitepoint.com/flow-control-callbacks-promises-async-await/)
 * 📜 [JavaScript: Promises and Why Async/Await Wins the Battle — Nick Parsons](https://dzone.com/articles/javascript-promises-and-why-asyncawait-wins-the-ba)

 ### Videos

 * 🎥 [Async + Await — Wes Bos](https://www.youtube.com/watch?v=9YkUCxvaLEk)
 * 🎥 [Asynchrony: Under the Hood — Shelley Vohr](https://www.youtube.com/watch?v=SrNQS8J67zc)
 * 🎥 [async/await in JavaScript - What, Why and How — Fun Fun Function](https://www.youtube.com/watch?v=568g8hxJJp4&index=3&list=PL0zVEGEvSaeHJppaRLrqjeTPnCH6)
 * 🎥 [async/await Part 1 - Topics of JavaScript/ES8 — The Coding Train](https://www.youtube.com/watch?v=XO77Fib9tSI&index=3&list=PLRqwX-V7Uu6bKLPQvPRNNE65kBL62mVfx)
 * 🎥 [async/await Part 2 - Topics of JavaScript/ES8 — The Coding Train](https://www.youtube.com/watch?v=chavThlNz3s&index=4&list=PLRqwX-V7Uu6bKLPQvPRNNE65kBL62mVfx)

**[⬆ Back to Top](#table-of-contents)**

---

## 27. Data Structures

### Articles

 * 📜 [Data Structures in JavaScript — Thon Ly](https://medium.com/siliconwat/data-structures-in-javascript-1b9aed0ea17c)
 * 📜 [Algorithms and Data Structures in JavaScript — Oleksii Trekhleb](https://itnext.io/algorithms-and-data-structures-in-javascript-a71548f902cb)
 * 📜 [Data Structures: Objects and Arrays ― Chris Nwamba](https://scotch.io/courses/10-need-to-know-javascript-concepts/data-structures-objects-and-arrays)
 * 📜 [Data structures in JavaScript — Benoit Vallon](http://blog.benoitvallon.com/data-structures-in-javascript/data-structures-in-javascript/)
 * 📜 [Playing with Data Structures in Javascript — Anish K.](https://blog.cloudboost.io/playing-with-data-structures-in-javascript-stack-a55ebe50f29d)
 * 📜 [The Little Guide of Queue in JavaScript — Germán Cutraro](https://hackernoon.com/the-little-guide-of-queue-in-javascript-4f67e79260d9)
 * 📜 [All algorithms writing with JavaScript in the book 'Algorithms Fourth Edition'](https://github.com/barretlee/algorithms)
 * 📜 [Collection of classic computer science paradigms in JavaScript](https://github.com/nzakas/computer-science-in-javascript)
 * 📜 [All the things you didn't know you wanted to know about data structures](https://github.com/jamiebuilds/itsy-bitsy-data-structures)

 ### Videos

 * 🎥 [Algorithms in JavaScript — Seth Koch](https://www.youtube.com/watch?v=PylQlISSH8U&list=PLujX4CIdBGCa-65N3uN8CDbUMrYsHBrz-)
 * 🎥 [Algorithms In Javascript | Ace Your Interview — Eduonix Learning Solutions](https://www.youtube.com/watch?v=H_EBPZgiAas&list=PLDmvslp_VR0zYUSth_8O69p4_cmvZEgLa)
 * 🎥 [Data Structures and Algorithms in JavaScript — freeCodeCamp](https://www.youtube.com/watch?v=Gj5qBheGOEo&list=PLWKjhJtqVAbkso-IbgiiP48n-O-JQA9PJ)
 * 🎥 [Learning JavaScript Data Structures and Algorithms: Sorting — Packt Video](https://www.youtube.com/watch?v=Ymh_AurrMbA)

**[⬆ Back to Top](#table-of-contents)**

---

## 28. Expensive Operation and Big O Notation

### Articles

 * 📜 [Big O Notation in Javascript — César Antón Dorantes](https://medium.com/cesars-tech-insights/big-o-notation-javascript-25c79f50b19b)
 * 📜 [Time Complexity/Big O Notation — Tim Roberts](https://medium.com/javascript-scene/time-complexity-big-o-notation-1a4310c3ee4b)
 * 📜 [Big O in JavaScript — Gabriela Medina](https://medium.com/@gmedina229/big-o-in-javascript-36ff67766051)
 * 📜 [Big O Search Algorithms in JavaScript — Bradley Braithwaite](http://www.bradoncode.com/blog/2012/04/big-o-algorithm-examples-in-javascript.html)
 * 📜 [Time Complexity Analysis in JavaScript — Jennifer Bland](https://www.jenniferbland.com/time-complexity-analysis-in-javascript/)
 * 📜 [Algorithms in plain English: time complexity and Big-O Notation — Michael Olorunnisola](https://medium.freecodecamp.org/time-is-complex-but-priceless-f0abd015063c)

### Videos

 * 🎥 [JavaScript: Intro to Big O Notation and Function Runtime — Eric Traub](https://www.youtube.com/watch?v=HgA5VOFan5E)
 * 🎥 [Essential Big O for JavaScript Developers — Dave Smith](https://www.youtube.com/watch?v=KatlvCFHPRo)
 * 🎥 [Big O Notation - Time Complexity Analysis — WebTunings](https://www.youtube.com/watch?v=ALl86xJiTD8)

**[⬆ Back to Top](#table-of-contents)**

---

## 29. Algorithms

### Articles

 * 📜 [Data Structures and Algorithms using ES6](https://github.com/Crizstian/data-structure-and-algorithms-with-ES6)
 * 📜 [Algorithms and data structures implemented in JavaScript with explanations and links to further readings](https://github.com/trekhleb/javascript-algorithms)
 * 📜 [JS: Interview Algorithm](http://www.thatjsdude.com/interview/js1.html)
 * 📜 [Algorithms in JavaScript — Thon Ly](https://medium.com/siliconwat/algorithms-in-javascript-b0bed68f4038)
 * 📜 [JavaScript Objects, Square Brackets and Algorithms — Dmitri Grabov](https://medium.freecodecamp.org/javascript-objects-square-brackets-and-algorithms-e9a2916dc158)
 * 📜 [Atwood's Law applied to CS101 - Classic algorithms and data structures implemented in JavaScript](https://github.com/felipernb/algorithms.js)
 * 📜 [Data Structures and Algorithms library in JavaScript](https://github.com/yangshun/lago)
 * 📜 [Collection of computer science algorithms and data structures written in JavaScript](https://github.com/idosela/algorithms-in-javascript)

**[⬆ Back to Top](#table-of-contents)**

---

## 30. Inheritance, Polymorphism and Code Reuse

### Articles

 * 📜 [Class inheritance, super — JavaScript.Info](https://javascript.info/class-inheritance)
 * 📜 [Inheritance in JavaScript — MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance)
 * 📜 [Inheritance in JavaScript — Rupesh Mishra](https://hackernoon.com/inheritance-in-javascript-21d2b82ffa6f)
 * 📜 [Simple Inheritance with JavaScript — David Catuhe](https://www.sitepoint.com/simple-inheritance-javascript/)
 * 📜 [JavaScript — Inheritance, delegation patterns and Object linking — NC Patro](https://codeburst.io/javascript-inheritance-25fe61ab9f85)
 * 📜 [Object Oriented JavaScript: Polymorphism with examples — Knoldus Blogs](https://blog.knoldus.com/object-oriented-javascript-polymorphism-with-examples/)
 * 📜 [Program Like Proteus — A beginner’s guide to polymorphism in Javascript — Sam Galson](https://medium.com/yld-engineering-blog/program-like-proteus-a-beginners-guide-to-polymorphism-in-javascript-867bea7c8be2)
 * 📜 [Object-oriented JavaScript: A Deep Dive into ES6 Classes — Jeff Mott](https://www.sitepoint.com/object-oriented-javascript-deep-dive-es6-classes/)

  ### Videos

 * 🎥 [Inheritance in JavaScript — kudvenkat](https://www.youtube.com/watch?v=yXlFR81tDBM)
 * 🎥 [JavaScript ES6 Classes and Inheritance — Traversy Media](https://www.youtube.com/watch?v=RBLIm5LMrmc)
 * 🎥 [Polymorphism in JavaScript — kudvenkat](https://www.youtube.com/watch?v=zdovG9cuEBA)

**[⬆ Back to Top](#table-of-contents)**

---

## 31. Design Patterns

### Articles

 * 📜 [4 JavaScript Design Patterns You Should Know — Devan Patel](https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know)
 * 📜 [JavaScript Design Patterns – Beginner's Guide to Mobile Web Development — Soumyajit Pathak](https://medium.com/beginners-guide-to-mobile-web-development/javascript-design-patterns-25f0faaaa15)
 * 📜 [JavaScript Design Patterns — Akash Pal](https://medium.com/front-end-hacking/javascript-design-patterns-ed9d4c144c81)
 * 📜 [Javascript Design Patterns: What They Are & How To Use Them — Patrick Simpson](https://seesparkbox.com/foundry/javascript_design_patterns)
 * 📜 [JavaScript Design Patterns: Understanding Design Patterns in JavaScript - Sukhjinder Arora](https://blog.bitsrc.io/understanding-design-patterns-in-javascript-13345223f2dd)
 * 📜 [All the 23 (GoF) design patterns implemented in Javascript — Felipe Beline](https://github.com/fbeline/Design-Patterns-JS)
 * 📜 [Learning JavaScript Design Patterns — Addy Osmani ](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)


  ### Videos

 * 🎥 [JavaScript Design Patterns — Udacity](https://www.udacity.com/course/javascript-design-patterns--ud989)
 * 🎥 [JavaScript Patterns for 2017 — Scott Allen](https://www.youtube.com/watch?v=hO7mzO83N1Q)

 **[⬆ Back to Top](#table-of-contents)**

---

## 32. Partial Applications, Currying, Compose and Pipe

### Articles

 * 📜 [Use function composition in JavaScript — Rémi](https://www.codementor.io/michelre/use-function-composition-in-javascript-gkmxos5mj)
 * 📜 [Currying in JavaScript ES6 — Adam Bene](https://blog.benestudio.co/currying-in-javascript-es6-540d2ad09400)
 * 📜 [Composition and Currying Elegance in JavaScript — Pragyan Das](https://medium.com/@pragyan88/writing-middleware-composition-and-currying-elegance-in-javascript-8b15c98a541b)
 * 📜 [Functional JavaScript: Function Composition For Every Day Use — Joel Thoms](https://hackernoon.com/javascript-functional-composition-for-every-day-use-22421ef65a10)
 * 📜 [Functional Composition: compose() and pipe() — Anton Paras](https://medium.com/@acparas/what-i-learned-today-july-2-2017-ab9a46dbf85f)
 * 📜 [Why The Hipsters Compose Everything: Functional Composing In JavaScript — A. Sharif](http://busypeoples.github.io/post/functional-composing-javascript/)
 * 📜 [A Gentle Introduction to Functional JavaScript pt III: Functions for making functions — James Sinclair](https://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-functions/)
 * 📜 [Curry And Compose (why you should be using something like ramda in your code) — jsanchesleao](https://jsleao.wordpress.com/2015/02/22/curry-and-compose-why-you-should-be-using-something-like-ramda-in-your-code/)
 * 📜 [Function Composition in JavaScript with Pipe — Andy Van Slaars](https://vanslaars.io/post/create-pipe-function/)
 * 📜 [Practical Functional JavaScript with Ramda — Andrew D'Amelio, Yuri Takhteyev](https://developer.telerik.com/featured/practical-functional-javascript-ramda/)
 * 📜 [The beauty in Partial Application, Currying, and Function Composition — Joel Thoms](https://hackernoon.com/the-beauty-in-partial-application-currying-and-function-composition-d885bdf0d574)
 * 📜 [Curry or Partial Application? — Eric Elliott](https://medium.com/javascript-scene/curry-or-partial-application-8150044c78b8)
 * 📜 [Partial Application in JavaScript — Ben Alman](http://benalman.com/news/2012/09/partial-application-in-javascript/)
 * 📜 [Partial Application of Functions — Functional Reactive Ninja](https://hackernoon.com/partial-application-of-functions-dbe7d9b80760)
 * 📜 [Currying vs Partial Application — Deepak Gupta](https://codeburst.io/javascript-currying-vs-partial-application-4db5b2442be8)
 * 📜 [Partial Application in ECMAScript 2015 — Ragan Wald](http://raganwald.com/2015/04/01/partial-application.html)
 * 📜 [Functional Composition in Javascript — Joe Cortopassi](https://joecortopassi.com/articles/functional-composition-in-javascript/)
 * 📜 [So You Want to be a Functional Programmer pt. I — Charles Scalfani](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
 * 📜 [So You Want to be a Functional Programmer pt. II — Charles Scalfani](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a)
 * 📜 [So You Want to be a Functional Programmer pt. III — Charles Scalfani](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-3-1b0fd14eb1a7)
 * 📜 [So You Want to be a Functional Programmer pt. IV — Charles Scalfani](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-4-18fbe3ea9e49)
 * 📜 [So You Want to be a Functional Programmer pt. V — Charles Scalfani](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-5-c70adc9cf56a)
 * 📜 [Functional-Light JavaScript Chapter 3: Managing Function Inputs — Kyle Simpson](https://github.com/getify/Functional-Light-JS/blob/master/manuscript/ch3.md)
 * 📜 [An introduction to the basic principles of Functional Programming — TK](https://medium.freecodecamp.org/an-introduction-to-the-basic-principles-of-functional-programming-a2c2a15c84)
 * 📜 [Concepts of Functional Programming in Javascript — TK](https://medium.com/the-renaissance-developer/concepts-of-functional-programming-in-javascript-6bc84220d2aa)
 * 📜 [An Introduction to Functional Programming Style in JavaScript — JavaScript Teacher](https://medium.freecodecamp.org/an-introduction-to-functional-programming-style-in-javascript-71fcc050f064)

  ### Videos

 * 🎥 [Compose vs Pipe: Functional Programming in JavaScript — Chyld Studios](https://www.youtube.com/watch?v=Wl2ejJOqHUU)
 * 🎥 [JavaScript Functional Programing: Compose — Theodore Anderson](https://www.youtube.com/watch?v=jigHxo9YR30)
 * 🎥 [Function Composition - Functional JavaScript — NWCalvank](https://www.youtube.com/watch?v=mth5WpEc4Qs)
 * 🎥 [JavaScript Function Composition Explained — Theodore Anderson](https://www.youtube.com/watch?v=Uam37AlzPYw)
 * 🎥 [Let's code with function composition — Fun Fun Function](https://www.youtube.com/watch?v=VGB9HbL1GHk)
 * 🎥 [Partial Application vs. Currying — NWCalvank](https://www.youtube.com/watch?v=DzLkRsUN2vE)
 * 🎥 [JavaScript Partial Application — Theodore Anderson](https://www.youtube.com/watch?v=jkebgHEcvac)

**[⬆ Back to Top](#table-of-contents)**

---

## 33. Clean Code

### Articles

 * 📜 [Clean Code concepts adapted for JavaScript — Ryan McDermott](https://github.com/ryanmcdermott/clean-code-javascript)
 * 📜 [JavaScript Clean Coding Best Practices — András Tóth](https://blog.risingstack.com/javascript-clean-coding-best-practices-node-js-at-scale/)
 * 📜 [Function parameters in JavaScript Clean Code — Kevin Peters](https://medium.com/@kevin_peters/function-parameters-in-javascript-clean-code-4caac109159b)
 * 📜 [Clean Code JavaScript — Sarah Drasner](https://css-tricks.com/clean-code-javascript/)
 * 📜 [Keeping your code clean — Samuel James](https://codeburst.io/keeping-your-code-clean-d30bcffd1a10)
 * 📜 [Best Practices for Using Modern JavaScript Syntax — M. David Green](https://www.sitepoint.com/modern-javascript-best-practices/)

### Videos
*  🎥 [JavaScript Pro Tips - Code This, NOT That](https://www.youtube.com/watch?v=Mus_vwhTCq0)

 **[⬆ Back to Top](#table-of-contents)**
