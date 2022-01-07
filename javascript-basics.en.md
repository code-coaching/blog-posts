---
title: JavaScript - Basis
slug: javascript-basis
tags:
  - JavaScript
  - JS
categories:
  - Frontend
---

De belangrijkste concepten uit `Leren Programmeren - JavaScript` worden herhaald.

## Variabelen

- `var`: Sleutelwoord om een variabele aan te maken, verouderd.
- `let`: Sleutelwoord om een variabele aan te maken, deze kan op een later moment een nieuwe waarde toegekend krijgen.
- `const`: Sleutelwoord om een variabele aan te maken, de variabele moet meteen een waarde toegekend krijgen en kan daarna niet opnieuw een waarde toegekend krijgen.

## Datatypes

### Primitieve datatypes

- `string`
  - bv. `"Dit is een string."`, `'Dit is ook een string.'`
- `number`
  - bv. `69`, `3.141592653`
- `boolean`
  - bv. `true`, `false`
- `undefined`
  - Wanneer een variabele nog geen waarde toegekend heeft gekregen, is het type `undefined`.

### Complexe datatypes

- `object`
- `function`

## Function

```js
/* Functie aanmaken */
function zegHallo(naam) {
  // Codeblok van de functie tussen { en }
  console.log("Hallo " + naam + "!");
  console.log(`Hallo ${naam}!`);
}

/*
 * Anonieme functie toekennen aan een variabele.
 * Gebruik maken van 'arrow notation'.
 */
const zegDoei = (naam) => {
  console.log(`Doei ${naam}!`);
};

/* Functie aanroepen met een parameter */
zegHallo("Bart"); // Print 'Hallo Bart!'

/* Functie aanroepen met een parameter */
zegDoei("Bart"); // Print 'Doei Bart!'
```

## Conditionele statements

Depending on whether a certain `condition` is met, code is executed.

### if

```js
const condition = true;

if (condition) {
  // This code block is executed
  console.log("The condition is true");
}
```

### if - else

```js
const condition = false;

if (condition) {
  // This code block is NOT executed
  console.log("The condition is true");
} else {
  // This code block will be executed
  console.log("The condition is false");
}
```

### if - else if - else

```js
const condition = 300;

if (condition < 100) {
  // This code block is NOT executed
  console.log("The condition is less than 100");
} else if (condition < 200) {
  // This code block will NOT be executed
  console.log("The condition is less than 200");
} else {
  // This code block will be executed
  console.log("The condition is equal to or greater than 200");
}
```

### switch

```js
const name = "John Duck";

switch (name) {
  case "Mark":
    // This code block is NOT executed
    console.log("Hello Mark!");
    break; // Stop executing the switch
  case "Bart":
  case "Barry":
    // This code block is NOT executed
    // Two cases, both execute this code block
    console.log("Hello Bart or Barry!");
    break;
  case "John Duck":
    // This code block is executed
    console.log("Hello John Duck!");
    break;
  default:
    // This code block is NOT executed
    console.log("Hello stranger!");
  // No break needed, the switch stops executing at this point anyway
}
```

## Iterative statements

### for

```js
for (let i = 0; i < 10; i++) {
  // this code block will be executed 10 times
  // the first loop i has value 0
  // the tenth loop i has value 9
  console.log(`This is loop ${i}`);
}
```

### while

```js
let i = 0;
while (i < 10) {
  // this code block will be executed 10 times
  // the first loop i has value 0
  // the tenth loop i has value 9
  console.log(`This is loop ${i}`);
  i++;
}
```

### do while

The difference between the `while` and `do while` is that a `do while` is **always executed once** and then the statement is evaluated to see if it should be executed again.

```js
let i = 0;
do {
  // This code block is executed 10 times
  // the first loop i has value 0
  // the tenth loop i has value 9
  console.log(`This is loop ${i}`);
  i++;
} while (i < 10);
```

## Objects

Almost everything is an object in JavaScript. So it is important to understand objects through and through.

```js
const person = {
  firstName: "John",
  lastName: "Duck",
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
};

const person2 = {
  firstName: "Bart",
  lastName: "Duisters",
  fullName: () => {
    return `${this.firstName} ${this.lastName}`;
  },
};

console.log(person.fullName()); // This prints "John Duck"
console.log(person2.fullName()); // This prints "undefined undefined" - IMPORTANT!
```

The variables `person` and `person2` above have both been assigned an object. Both objects have three properties: `firstName`, `lastName` and `fullName`.

There is no difference in the `return` statement of both functions, they both return a concatenated (merged) string.

There **is** a difference in assigning an anonymous function `function () {}` and assigning an anonymous arrow function `() => {}`.
The arrow function **does not know what `this` is**. So a normal anonymous function must be used to ensure that `this` is known.

## this

```js
const person = {
  firstName: "Bart",
  lastName: "Duisters",
  fullName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};
```

In the above example, `this` is used. `this` refers to `this object`. In the above example, `this` refers to the variable `person`, so it could also be written like this:

```js
const person = {
  firstName: "Bart",
  lastName: "Duisters",
  fullName: function () {
    return `${person.firstName} ${person.lastName}`;
  },
};
```

## class

A class can be thought of as a template to create an object.

Take the example below, where three objects are created. Each object represents a person. Each object has two properties, namely `firstName` and `lastName`. Each object has one method, namely `fullName`.

For each new person, each property needs to be retyped and each method must be retyped `fullName`.

Suppose each person gets a new property, called `nickname`. Then in each object, the new property will have to be added separately.

If ten objects are created that represent a person, this means that a property has to be added ten times.

```js
const person = {
  firstName: "John",
  lastName: "Duck",
  fullName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};

const person2 = {
  firstName: "Bart",
  lastName: "Duisters",
  fullName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};

const person3 = {
  firstName: "Mark",
  lastName: "Duisters",
  fullName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};
```

To make sure that for each person there is no need to create a new object with all the properties, a `class` can be created.

```js
class Person {
  firstName = "Anonymous";
  lastName;

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const person = new Person();
person.firstName = "John";
person.lastName = "Duck";

const person2 = new Person();
person2.firstName = "Bart";
person2.lastName = "Germans";

console.log(person.fullName());
console.log(person2.fullName());
```

Properties defined within the code block of a `class`, are defined without `const` or `let`.
Functions (also called `methods`) defined within the code block of a `class`, are defined without the keyword `function`.

What can be derived from the code above, is that `new Person()`, returns an object, with the properties `firstName`, `lastName` and the method `fullName`.
The `new` keyword creates a new instance of the `class` called `Person`. Each instance of the `class` `Person` gets by default a property `firstName` with the default value `'Anonymous'`, a property `lastName` with no default value (i.e., the value is `undefined`) and a method `fullName()`.

After assigning an instance of a `class` to a variable, the properties and methods can be addressed as if it were an object (because it is an object) using the dot notation. This is done, for example, to assign values to `person.firstName` and `person.lastName`.

### constructor

To make sure that you do not always have to create an instance first and then assign a value to the properties one by one. A special method can be used, the `constructor` method. This is a method that is called when an instance of an object is created.

```js
class Person {
  firstName;
  lastName;

  constructor() {
    /*
     * Code in this code block is executed at the moment
     * that 'new Person()' is called.
     */
    console.log("I am being executed");
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const person = new Person(); // The constructor method is executed
// 'I am being executed' is printed

const person2 = new Person(); // The constructor method is executed
// 'I am being executed' is printed
```

Parameters can also be passed to this special method.

```js
class Person {
  firstName;
  lastName;

  constructor(parameter1, parameter2) {
    this.firstName = parameter1;
    this.lastName = parameter2;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const person = new Person("Bart", "Duisters");
console.log(person.fullName()); // Bart Duisters
```

### static

Sometimes it is interesting not to have to create an instance of a class and and still be able to call the methods. For this we use an example with a class called 'Utils'.

```js
let a = 3;
let b = 4;
let largestNumber;

if (a > b) {
  largestNumber = a;
} else {
  largestNumber = b;
}
console.log(`The largest number is: ${a}`);

a = 4;
b = 5;

if (a > b) {
  largestNumber = a;
} else {
  largestNumber = b;
}
console.log(`The largest number is: ${b}`);
```

The same thing is done twice, so this can be put into a function.

```js
function largestNumber(a, b) {
  if (a > b) {
    return a;
  }
  return b;
}

let a = 3;
let b = 4;
let largestNumber;

console.log(`The largest number is: ${largestNumber(a, b)}`);

a = 4;
b = 5;

console.log(`The largest number is: ${largestNumber(a, b)}`);
```

Right now, there is only one JavaScript file. But imagine that in a new file the same logic had to be used again to determine what the largest number is, then the same function would have to be written again.

Instead of always writing code/logic at the time it is necessary to know which number is greater, this can be put into a class as a method. Because this has to be used often, it is interesting to add this as a static method. This ensures that no instance of the class needs to be created to use the method.

```js
// Use a little imagination, Utils is in a separate JavaScript file.
class Utils {
  // New keyword: static
  // This allows the function to be called via: Utils.largestNumber(a, b);
  static largestNumber(a, b) {
    if (a > b) {
      return a;
    }
    return b;
  }
}

// Again use imagination, this is the second JavaScript file.
const a = 3,
  b = 5;
console.log(`The largest number is: ${Utils.largestNumber(a, b)}`);

// Again use imagination, this is the third JavaScript file.
const x = 4,
  y = 6;
console.log(`The largest number is: ${Utils.largestNumber(x, y)}`);
```

### extends & super

Imagine there is code in which different classes are provided to create different animals.

```js
class Lion {
  name;
  age;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  eat() {
    return `${this.name} is eating.`;
  }

  sleep() {
    return `${this.name} is sleeping.`;
  }

  hunt() {
    return `${this.name} is hunting.`;
  }
}

class Dog {
  name;
  age;
  petName;

  constructor(name, age, petName) {
    this.name = name;
    this.age = age;
    this.petName = petName;
  }

  eat() {
    return `${this.name} is eating.`;
  }

  sleep() {
    return `${this.name} is sleeping.`;
  }
}

class Elephant {
  name;
  age;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  eat() {
    return `${this.name} is eating.`;
  }

  sleep() {
    return `${this.name} is sleeping.`;
  }

  eat() {
    return `${this.eat()} And continues to eat.`;
  }
}

const lion = new Lion("Lion", 13);
const dog = new Dog("Samson", 8);
const elephant = new Elephant("Dumbo", 9);

console.log(lion.eat()); // Lion is eating.
console.log(lion.hunt()); // Lion cub is hunting.
console.log(lion.sleep()); // Lion cub is sleeping.

console.log(dog.eat()); // Samson is eating.
console.log(dog.sleep()); // Samson is sleeping.

console.log(elephant.eat()); // Dumbo is eating.
console.log(elephant.eat()); // Dumbo is eating. And keeps eating.
console.log(elephant.sleep()); // Dumbo is sleeping.
```

Each class has two properties that are similar: `name` and `age`.

Each class has two functions that are similar: `eat()` and `sleep()`.

As a developer, it is not nice to have to type the same property again for each class and to have to type the same function again.

Something has been provided so that it only needs to be typed once, the keyword `extends` and the use of `super`.

New terms: `super class` and `sub class`.

A class that `extends` from another class is a sub class (also called `child`). The class from which `extends` is a super class (also called `parent`).

In English, `extends` can be phrased by ` inheritance`.

The code below does exactly the same thing:

```js
class Animal {
  name;
  age;

  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  eat() {
    return `${this.name} is eating.`;
  }

  sleep() {
    return `${this.name} is sleeping.`;
  }
}

class Lion extends Animal {
  constructor(name, age) {
    super(name, age);
  }

  hunt() {
    return `${this.name} is hunting.`;
  }
}

class Dog extends Animal {
  petName;

  constructor(name, age, petName) {
    super(name, age);
    this.petName = petName;
  }
}

class Elephant extends Animal {
  constructor(name, age) {
    super(name, age);
  }

  eatLots() {
    return `${super.eat()} And continues to eat.`;
  }
}

const lion = new Lion("Lion", 13);
const dog = new Dog("Samson", 8);
const elephant = new Elephant("Dumbo", 9);

console.log(lion.eat()); // Lion is eating.
console.log(lion.hunt()); // Lion cub is hunting.
console.log(lion.sleep()); // Lion cub is sleeping.

console.log(dog.eat()); // Samson is eating.
console.log(dog.sleep()); // Samson is sleeping.

console.log(elephant.eat()); // Dumbo is eating.
console.log(elephant.eatLots()); // Dumbo is eating. And keeps eating.
console.log(elephant.sleep()); // Dumbo is sleeping.
```

### Access modifiers

In other programming languages it is possible to make use of `access modifiers`. In other words, it is possible to restrict access to certain properties and methods.

Usually there is a keyword `public` and a keyword `private` that is used to determine whether a property/method is available everywhere (public) or only within an instance of the class (private).

Suppose these keywords existed in JavaScript, it would look like this:

```js
// NO VALID JAVASCRIPT CODE
class Person {
  private pinCode;
  private password;
  public name;
  public age;

  constructor(pin, password, name, age) {
    this.pinCode = pin;
    this.password = password;
    this.name = name;
    this.age = age;
  }

  private getPin() {
    return this.pinCode;
  }

  public getPinCode() {
    return this.pinCode;
  }
}

const person = new Person(1111, 1234, 'John Duck', 29);
console.log(person.name); // "John Duck"
console.log(person.age); // 29
console.log(person.pinCode); // This would not print anything, pinCode is private
console.log(person.password); // This would not print anything, password is private

console.log(person.getPin()); // This would not print anything, getPin() is private
console.log(person.getPinCode()); // 1111
/*
* The property pinCode is private, and thus only available within the code block of the class Person.
* The method getPinCode() is public, so it can be called on the instance.
* Because getPinCode() is inside the code block of the class, it can access the private properties/methods.
*/
```

In JavaScript, the keywords `public` and `private` do not exist. So the code would look like this:

```js
class Person {
  pinCode;
  password;
  name;
  age;

  constructor(pin, password, name, age) {
    this.pinCode = pin;
    this.password = password;
    this.name = name;
    this.age = age;
  }

  getPin() {
    return this.pinCode;
  }

  getPinCode() {
    return this.pinCode;
  }
}

const person = new Person(1111, 1234, "John Duck", 29);
console.log(person.name); // "John Duck"
console.log(person.age); // 29
console.log(person.pinCode); // 1111
console.log(person.password); // 1234

console.log(person.getPin()); // 1111
console.log(person.getPinCode()); // 1111
```

Everything is `public` by default, all properties and methods can be called on the instance of a class.

However, there is a convention used among developers to still indicate that something is actually `private`. By adding a `_` (underscore) in front of the name of the property/method, it is indicated that a property/method is actually `private`.

```js
class Person {
  _pinCode;
  _password;
  name;
  age;

  constructor(pin, password, name, age) {
    this._pinCode = pin;
    this._password = password;
    this.name = name;
    this.age = age;
  }

  _getPin() {
    return this._pinCode;
  }

  getPinCode() {
    return this._pinCode;
  }
}

const person = new Person(1111, 1234, "John Duck", 29);
console.log(person.name); // "John Duck"
console.log(person.age); // 29
console.log(person._pinCode); // 1111
console.log(person._password); // 1234

console.log(person._getPin()); // 1111
console.log(person.getPinCode()); // 1111
```

The properties/methods with an underscore in front are still public, so these can be called. But, as a developer, you know that this is actually not the intention. This way, it is still possible to have some kind of `access modifiers` within JavaScript.

### get & set

The keyword `get` and the keyword `set` can be used to create `getters` and `setters`. These are methods that work slightly different.

Suppose there is a class Student:

```js
class Student {
  _firstName;
  _lastName;

  constructor(firstName, lastName) {
    this._firstName = firstName;
    this._lastName = lastName;
  }

  getFirstName() {
    return this._firstName;
  }

  setFirstName(FirstName) {
    this._firstName = firstName;
  }

  getLastName() {
    return this._lastName;
  }

  setLastName(lastName) {
    this._lastName = lastName;
  }

  getName() {
    if (this._firstName && this._lastName) {
      return `${this._firstName} ${this._lastName}`;
    } else if (this._firstName) {
      return this._firstName;
    } else {
      return this._last name;
    }
  }
}

const student = new Student("John", "Duck");

console.log(student.getFirstName()); // "John"
console.log(student.getLastName()); // "Duck"
student.setFirstName("Jane");
console.log(student.getName()); // "Jane Duck"
```

This would work and it is perfectly valid JavaScript. But it would be nice if the first name, last name and full name could be requested as properties.

For this, `get` and `set` can be used.

```js
class Student {
  _firstName;
  _lastName;

  constructor(firstName, lastName) {
    this._firstName = firstName;
    this._lastName = lastName;
  }

  get firstName() {
    return this._firstName;
  }

  set firstName(firstName) {
    this._firstName = firstName;
  }

  get lastName() {
    return this._lastName;
  }

  set lastName(lastName) {
    this.lastName = lastName;
  }

  get name() {
    if (this._firstName && this._lastName) {
      return `${this._firstName} ${this._lastName}`;
    } else if (this._firstName) {
      return this._firstName;
    } else {
      return this._lastName;
    }
  }
}

const student = new Student("John", "Duck");

console.log(student.firstName); // "John"
console.log(student.lastName); // "Duck"
student.firstName = "Jane";
console.log(student.name); // "Jane Duck"
```

The advantage of using `getters` and `setters`, is that it can be used as if they were properties. But in fact they are methods, so additional logic can be performed. An example of this is `get name()`. On the instance it is called as if it were a property `student.name`, but behind it the code block of the `getter method` is executed.

## Built-in objects

There are many different built-in objects. These objects make use of a class.

It is not necessary to know literally every function and every property by heart. To know what is possible with an object that is globally present within JavaScript, documentation can be consulted.

An example is the Array class. The [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) can be found on a website maintained by Mozilla (the makers of Firefox and Thunderbird).
