---
postUuid: d4aefaf5-3611-4585-b460-6e09abcdc247
title: Learn to program - JavaScript
slug: learn-to-program-javascript
tags:
  - Learn To Program
  - JavaScript
  - JS
categories:
  - Frontend
---

To get a working program in any programming language, the `syntax` of the language should be known. This are all the rules that are used to write a working program.

## Keywords

In JavaScript there are reserved keywords. A couple of examples are `var`, `let`, `const`, `function`, `return` ... These words cannot be used as variable or function names.

```js
const let = 3;
// A variable name cannot be a reserved keyword, this will result in an error.
```

## Statements

A JavaScript program exists out of a list of statements.

```js
var x, y, solution; // Statement 1
x = 6; // Statement 2
y = 9; // Statement 3
solution = x + y; // Statement 4
```

## Semicolon

A semicolon is used in a lot of programming languages to separate statements. In JavaScript, this is optional.

```js
var x, y, solution;
x = 6;
y = 9;
solution = x + y;
```

In JavaScript the use of a semicolon is optional, unless there are multiple statements on the same line.

## White space

JavaScript does not take spaces, new lines ... into account.

```
var x,y,solution;x=6;y=9;solution=x+y;
```

Is identical to:

```js
var x, y, solution;
x = 6;
y = 9;
solution = x + y;
```

For better readability, it is recommended to make use of white space.

## Declaration

```js
var a, b, solution;
```

Declaring a variable, creates a new variable. Declared variables do not yet contain a value. It simply creates a variable, which later can be assigned a value.

## Assigning

```js
var a, b, solution; // Declaration of variables a, b and solution
a = 6; // Assigning the value 6 to variable a
b = 9; // Assigning the value 9 to variable b
```

It is also possible to combine the declaration and the assignment of a variable. This is called initialization.

```js
var a = 6,
  b = 9,
  solution;
```

## Operators

JavaScript uses mathmatical operators `+`, `-`, `*`, `/` to perform operations on variables.

JavaScript uses an assignment operator `=` to assign a value to variables.

```js
var solution = 6 + 9;
// Mathmetical operator + is used to add 6 and 9 togheter
// Assignment opertor `=` is used to assign the result (the value 15) to the variable solution
```

Mathmetical operators use the priority rules of mathematics.

| operator | explanation                                             |
| :------- | :------------------------------------------------------ |
| ()       | Everything between parenthesis has the highest priority |
| \*\*     | Power                                                   |
| \* and / | Multiplication and division (same priority)             |
| + and -  | Addition and subtraction (same priority)                |

```
3 * 2 + 5 = 11
3 * (2 + 5) = 3 * 7 = 21
```

## Expressions

Expressions are a combination of values, variables and operators. The evaluation of an expression results in a value.

Calculating the value, is called evaluating.

The expression `6 + 9` evaluates to `15`.

```js
var x = 6;
x + 9; // The expression x + 9 evaluates to 15
```

```js
var x = 6;
x = x + 9;
// The expression x = x + 9 evaluates to 15, the result is assigned to x
```

## Comments

Comments in JavaScript are not executed. Comments on one line can be written with `//`. Using `/*` and `*/` it is possible to write comments on multiple lines`

```js
var x = 6; // This is commentary and will be ignored.
// var y = 9; This whole line is in commentary, thus it will not create a variable y.

/*
This is all commentary.
Also this line is commentary.
*/
```

## Case sensitive

Names of variables are case sensitive. This means `firstname` and `Firstname` are different variables. A variable name must begin with a letter, `$` or `_`

```js
var firstname, firstName;
firstname = "Bart";
firstname = "Barry";
```

## Variables

In JavaScript there are three ways to create variables.
Er zijn in JavaScript drie manieren om een variabele te maken. `var`, `let` en `const`. In the early days of JavaScript, `var` was the only way to create variables. The difference between the three options becomes clear with some examples.

A good rule to use is to not use `var`. Start with `const`. Imagine a variable needs to be reassigned, then `const` should be replaced by `let`. To avoid issues, always declare a variable before using it.

### var

```js
var num = 13; // Declaration of variable num, assigning the value 13
var num; // Redeclaration of variable num
console.log(num); // 13
```

console.log(); will print the (evaluated) value of the expression in between `(` and `)` to the console.

### let

```js
let num = 13; // Declaration of variable num, assigning the value 13
let num; // This results in an error, because the variable cannot be redeclared
```

```js
let num = 13; // Declaration of variable num, assigning the value 13
num = 14; // It is possible to reassign a variable declared with `let`
console.log(num); // 14
```

### const

```js
const num = 13; // Declaration of variable num, assigning the value 13
const num; // This results in an error, because the variable cannot be redeclared
```

```js
const num = 13; // Declaration of variable num, assigning the value 13
num = 14; // This results in an error, because the variable cannot be reassigned
```

## Operators

### Assignment

| operator | description                     | example                       |
| :------- | :------------------------------ | :---------------------------- |
| =        | assigning a value to a variable | const a = 3; const b = a + 4; |

### Mathmetical

| operator | description    | example       |
| :------- | :------------- | :------------ |
| +        | addition       | 6 + 9 = 15    |
| -        | subtraction    | 15 - 9 = 6    |
| \*       | multiplication | 5 \* 2 = 10   |
| \*\*     | power          | 5 \*\* 2 = 25 |
| /        | division       | 10 / 2 = 5    |
| %        | modulus        | 11 % 2 = 1    |

<br/>

### Comparison

| operator | description                       |
| :------- | :-------------------------------- |
| ==       | equal to                          |
| ===      | equal value and equal type        |
| !=       | not equal to                      |
| !==      | not equal value or not equal type |
| >        | greater than                      |
| <        | smaller than                      |
| >=       | greater than or equal to          |
| <=       | smaller than or equal to          |
| ? :      | ternary operator                  |

<br/>

## Datatypes

Datatypes make sure that programming languages know what to do with an expression.

### Primitive datatypes

- `string`
  - A collection of characters enclosed in single or double quotes.
  - e.g. `"Hello World"`, `'Hello World'`
- `number`
  - A `number` is a number, with or without a decimal point.
  - e.g. `69`, `3.141592653`
- `boolean`
  - e.g. `true`, `false`
- `undefined`
  - When a variable is declared but not assigned a value, the variable has the type `undefined`.
  - e.g. `let x;`, the type of the variable `x` is `undefined`

### Complex datatypes

- `object`
- `function`

### Function

A function has the type `function`.

### Object

An object `{name: 'Sylvia'}`, an array `['Sylvia', 'Bart', 'John']` and `null` have the type `object`.

### typeof

With `typeof` you can check the type of an expression.

```js
typeof "Sylvia"; // string
typeof "Barry"; // string
typeof 69; // number
typeof 3.141592653; // number
typeof true; // boolean
typeof false; // boolean
typeof x; // undefined
typeof { name: "Sylvia" }; // object
typeof [1, 2, 3]; // object
typeof null; // object
typeof function som() {}; // function
```

In JavaScript datatypes of a variable are dynamic, a variable can have multiple datatypes assigned to it.

```js
let x; // x has the type undefined
x = 69; // x has the type number
x = "Sylvia"; // x has the type string
```

When `+` is used on two `numbers`, the numbers will be added.
When `+` is used on two `strings`, the strings will be concatenated.
When `+` is used on a `number` and `string`, then the number will be converted to a `string` and the result will be concatenated.

Expressions are evaluated from left to right.

```js
1 + 2; // 3
1 + "2"; // "12"
"Bart" + " " + "Duisters"; // "Bart Duisters"
1 + "Bart"; // "1Bart"
1 + 2 + "Bart"; // "3Bart"
```

## Functions

A function is a block of code that can be called at a later moment. When a function is called, the code will be executed.

```js
function functionname(parameter1, paramater2) {
  /*
   * parameter1 and parameter 2 are variables,
   * only available in the code block of the function { and }
   */
  const result = parameter1 - parameter2; // An example of an operator (subtraction)
  return result; // Returning a result
}
```

A function is defined by the keyword `function`, followed by the name of the function. In between the parentheses `(` and `)` you can specify the parameters/arguments of the function. These are the values that are passed to the function when it is called. The `parameters` are handled as local variables inside the function code block, between `{` and `}`. The code block of the function is also called the `body` of the function.

### Calling

Imagine there is a function to print "Hello World" in the console.

```js
function helloWorld() {
  console.log("Hello World!");
}
```

This can be called with:

```js
helloWorld(); // This will print "Hello World!" to the console
```

Imagine there is a function to add two numbers.

```js
function sum(a, b) {
  return a + b;
}
```

When JavaScript is executed and a `return` is evaluated, the function will stop executing and return the value.

This can be called with:

```js
sum(1, 2); // 3
sum(6, 9); // 15
sum(9, "Bart"); // "9Bart"
```

## Scope

Scope is the area of a program where variables are accessible.

Some examples will help to understand the scope of variables.

```js
let x = 4; // Can be accessed anywhere in the program, the scope is global

function scopeExample(parameter) {
  // The parameter is only accessible in the function scopeExample, the scope is local
  let y = 3;
  // The variable y is only accessible in the function scopeExample, the scope is local
  console.log("x: ", x); // "x: 4"
  console.log("y: ", y); // "y: 3"
  console.log("x + y: ", x + y); // "x + y: 7"
  console.log(parameter); // [value of parameter]
}

scopeExample("Bart");
// "x: 4"
// "y: 3"
// "x + y: 7"
// "Bart"

console.log(x); // 4
console.log(y); // undefined
console.log(parameter); // undefined
```

## Keywords

In JavaScript there are reserved keywords. Some examples are `var`, `let`, `const`, `function`, `return` ... These words cannot be used as the name of a variable or a function.

```js
const let = 3;
// a variable name cannot be a reserved keyword, this will result in an error
```

## Conditional statements

With conditional statements it is possible to execute different code depending on the `condition` that is fulfilled.

JavaScript has four conditional statements:

`if`, `else`, `else if`, `switch`

This becomes clear with some examples.

### if

```js
const x = 4;
const y = 3;

/* 
  The if-statement can be read as:
  "If the condition is 'x > y' evaluates to true, execute the code in the block (between '{' and '}')."
*/
if (x > y) {
  // The code in the block is executed becaues x > y -> 4 > 3 evaluates to 'true'
  console.log("x is greater than y");
}
```

An `if` statement exists out of the keyword `if`, a condition that is evaluated, and a block of code that is executed if the condition is fulfilled.

### else

An `else` statement is always coupled with an `if` statement. When the condition of the `if` statement is not fulfilled, the `else` statement is executed.

```js
const x = 3;
const y = 4;

// x > y -> 3 > 4 -> false
if (x > y) {
  // The code in the block of the if-statement is not executed
  console.log("x is greater than y");
} else {
  // The code in the block of the else-statement is executed
  console.log("x is smaller than y");
}
```

### else if

An `else if` statement is always coupled with an `if` statement. When the condition of the `if` statement is not fulfilled, the `else if` statement is evaluated.

```js
const x = 100;

if (x <= 10) {
  // The code in the block of the if-statement is not executed
  console.log("x is smaller or equal to 10");
} else if (x <= 100) {
  // The code in the block of the else-if-statement is executed, because x <= 100 evaluates to true
  console.log("x is greater than 10, but smaller or equal to 100");
} else {
  // The code in the block of the else-statement is not executed
  console.log("x is greater than 100");
}
```

### switch

A `switch` statement is used to execute a block of code depending on the value of a variable.

A code block of a switch case contains the keyword `case` followed by the value that is evaluated. The code block is defined after a colon `:`, the code block ends with the keyword `break`.

```js
const name = "Bart";

switch (name) {
  case "Sylvia":
    console.log("The name is Sylvia");
    break;
  case "Bart":
    // This code block is executed
    console.log("The name is Bart");
    // When 'break' is reached, the code will go from HERE*
    break;
  case "Barry":
    console.log("De naam is Barry");
    break;
}
// to HERE*
```

<br/>
It is possible to provide a `default` case, this is executed when no other case is fulfilled.

```js
const name = "Mark";

switch (name) {
  case "Sylvia":
    console.log("The name is Sylvia");
    break;
  case "Barry":
    console.log("The name is Barry");
    break;
  default:
    // This code block is executed
    console.log("The name does not match any case");
    break;
}
```

<br/>
It is possible to place multiple `case` statements on the same code block.

```js
const today = "tuesday";

switch (vandaag) {
  case "monday":
  case "wednesday":
    console.log("Work + teaching the second year students");
    break;
  case "tuesday":
  case "thursday":
    // This code block is executed
    console.log("Work + teaching the first year students");
    break;
  case "friday":
    console.log("Work");
    break;
  case "saturday":
  case "sunday":
    console.log("Preparing next class");
    break;
}
```

## Iterative statements

Iteration is a synonym for repetition. With an iterative statement it is possible to execute a block of code multiple times.

JavaScript has five iterative statements:
`for`, `for in`, `for of`, `while` and `do while`

Also called `loops`.

We will take a look at the `for loop`, `while loop` and `do while loop`.

### while

The code in the code block will be executed as long as the condition is fulfilled.

```js
let i = 0;

// Before the first iteration starts, the expression (i < 3) is evaluated.
// Because i has the value 0, the condition evaluates to true.
// The code block of the while loop is executed.
while (i < 3) {
  console.log("Loop: ", i); // *
  // * First iteration will print "Loop: 0"
  // * Second iteration will print "Loop: 1"
  // * Third iteration will print "Loop: 2"
  i++; // **
  // ** First iteration will increment i to 1, then 1 < 3 will be evaluated
  // ** Second iteration will increment i to 2, then 2 < 3 will be evaluated
  // ** Third iteration will increment i to 3, then 3 < 3 will be evaluated
}
// After the third iteration, the code block is not executed anymore, because 3 < 3 evaluates to false. JavaScript will stop executing the while loop.
```

### do while

The code in the code block will be executed, after the first iteration the first evaluation of the expression is done.

```js
let i = 0;

// The code block of the do while loop is executed.
do {
  console.log("Loop: ", i); // *
  // * First iteration will print "Loop: 0"
  // * Second iteration will print "Loop: 1"
  // * Third iteration will print "Loop: 2"
  i++; // **
  // ** First iteration will increment i to 1, then 1 < 3 will be evaluated
  // ** Second iteration will increment i to 2, then 2 < 3 will be evaluated
  // ** Third iteration will increment i to 3, then 3 < 3 will be evaluated
} while (i < 3);
// After the third iteration, the code block is not executed anymore, because 3 < 3 evaluates to false. JavaScript will stop executing the do while loop.
```

The code above gives the same result as the example of the while loop. Yet there is an important difference. The example below shows the difference between the while and the do while loop.

```js
let i = 2;

while (i < 1) {
  console.log("While loop");
  i++;
}

console.log("i is now: ", i);

do {
  console.log("Do while loop");
  i++;
} while (i < 1);

console.log("i is now: ", i);
/*
* The following output is printed:
i is now: 2
Do while loop
i is now: 3
* Note that the while loop is not executed at all.
* The do while IS executed.
*/
```

### for

As long as an expression evaluates to true, the code block is executed.

```js
// There are three statements in the for loop
// statement 1: initialization of variable i with keyword let and value 0
// statement 2: comparison of i with 3, this condition will be evaluated before the code block is executed every iteration
// statement 3: increment of i by 1, this happens after every iteration
for (let i = 0; i < 3; i++) {
  console.log("Loop: ", i); // *
  // * First iteration will print "Loop: 0"
  // * Second iteration will print "Loop: 1"
  // * Third iteration will print "Loop: 2"
}
```

## Array

An array is a way to store a list of values.

```js
const fibonacciNumbers = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34];
```

The Fibonacci numbers is a sequence of numbers where each number is the sum of the two preceding numbers.

The first number i 0, the second number is 1, the third number is the sum of the first number with the second number. The fourth number is the sum of the second number with the third number. And so on.

The variable `fibonacciNumbers` contains the first 10 Fibonacci numbers.

The values of an array can be accessed by index. An index starts at 0. The first number has index 0, the fifth number has index 4. The tenth number has index 9.

To know the number of the sixth number:

```js
console.log(fibonacciReeks[5]); // This will print the number 5
```

Assigning a value to an array is possible by using the index. To add the eleventh number:

```js
const fibonacciReeks = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34];
fibonacciReeks[10] = 55;
```

Note that the array is changed, even though the variable is declared using the keyword `const`.

For now it suffices to remember:

- A variable declared with the keyword `const` cannot be assigned a new value.
- The array itself is the value. It will not be possbile to assign a new array.
- The values in an array, change the array, but it is still the same array that got assigned to the variable.

## Object

An object is a way to store complex data.
As an example, a car exists out of different parts and has certain properties.

An object that resembles a car.

```js
{
  brand: "Opel",
  type: "Manta 400",
  amountOfWheels: 4,
  amountOfDoors: 2,
  hp: 114
}
```

An object that resembles a person.

```js
{
  firstName: "Bart",
  lastName: "Duisters",
  dateOfBirth: "13-08-1991",
}
```

An object can, similar to an array, be looked at as a value that can be assigned to a variable. Similar to an array, the object itself is the value that gets assigned to the variable. Changing the properties and values of properties, changes the object, but it is still the same object that got assigned to the variable.

```js
const person = {
  firstName: "Bart",
  lastName: "Duisters",
  datOfBirth: "13-08-1991",
};

// Changing of the value of an existing property
person.firstName = "Mark";

// Assigning extra property to the object
person.age = 29;
```

The object, with the changes, will look like this:

```js
{
  firstName: "Mark",
  lastName: "Duisters",
  dateOfBirth: "13-08-1991",
  age: 29,
}
```

Because JavaScript is a dynamic language, it is possible to assign all types of values to an object its properties.

```js
const person = {
  firstName: "Bart",
  lastName: "Duisters",
  dateOfBirth: "13-08-1991",
  fibonacciNumbers: [0, 1, 1, 2, 3, 5, 8, 13],
};

console.log(person.fibonacciNumbers[4]);
// This will print the number 4
```

A `function` is also a type. It is thus also possible to assign a function as value of a property of an object.

```js
const person = {
  firstName: "Bart",
  lastName: "Duisters",
  dateOfBirth: "13-08-1991",
  fibonacciNumbers: [0, 1, 1, 2, 3, 5, 8, 13],
  sayHello: function () {
    console.log("Hello");
  },
};

person.sayHello(); // This will print "Hello" in the console
```

```js
const person = {
  sayHello: function () {
    console.log("Hello");
  },
  sayBye: () => {
    console.log("Bye");
  },
};
```

New terminology:

- `Anonymous function`: A function that is not named.
- `Arrow function`: A `=>` behind the function parentheses instead of the `function` keyword.
