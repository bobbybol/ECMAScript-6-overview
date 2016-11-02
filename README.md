# A quick overview of ECMAScript 6
Summary of the most important new features of ES6 and how they differ from ES5.

This short guide is written for colleagues and pair-programming buddies to use as a reference while working through code that was written in or refactored using new ES6 syntax/functionalities.

This list is by no means exhaustive, but should cover all the features to get you up and running with modern JavaScript frameworks such as **vue.js**, **ReactJS**, and **Aurelia**.

## Table of Contents
  1. [let and const](#let-and-const)
    + [The `let` keyword](#the-let-keyword)
    + [The `const` keyword](#the-const-keyword)
  1. [Template strings](#template-strings)
  1. [Spread operators](#spread-operators)
  1. [Default function parameters](#default-function-parameters)
  1. [Enhanced object literals](#enhanced-object-literals)
  1. [(fat) Arrow Functions](#fat-arrow-functions)
    + [Lexical `this`](#lexical-this)
  1. [Destructuring assignment](#destructuring-assignment)
    + [Destructuring arrays](#destructuring-arrays)
    + [Destructuring objects](#destructuring-objects)
  1. [ES6 Modules](#es6-modules)
  1. [ES6 Class Syntax](#es6-class-syntax)
    + [Class inheritance](#class-inheritance)
  1. [Object.assign()](#objectassign)

## let and const
The first thing you'll notice when looking through ES6 code, is its lack of `var`s.
This is because `var` has essentially (save for some rare edge cases) been replaced by `let` and `const`.

### The `let` keyword
We can use the `let` keyword to create _block scoping_ in locations where we weren't able to do so before.

With `var` the value changes inside the `if` statement:
```javascript
var x = 10;

if(x) {
  var x = 4;
}

console.log(x) //logs 4
```

But with `let` inside the `if` block, the value of x outside of the `if` statement stays the same:
```javascript
var x = 10;

if(x) {
  let x = 4;
}

console.log(x) //logs 10
```
The globally scoped x is not changed by the scope local to the `if` statement. Before ES6, there was no local scope for anything other than `function`s. Also note that if we console.log x from within the `if` statement, x will log out to be 4.

Benefits of `let` are apparent in the `for` loop. Consider the following loop where we simply want to add 42 `div`s to a `section`, and give them a click handler that alerts the index number of each `div`:
```javascript
for(var i=0; i<42; i++) {
  var div = document.createElement('div');
  div.onclick = function() {
    alert("you clicked on a box #" + i);
  }
  document.getElementsByTagName('section')[0].appendChild(div);
}
```
The above will log out \#42 for each and every `div` that's clicked. Simply replacing `var` for `let` in the `for` loop like so: 
```javascript
for(let i=0; i<42; i++) {
  var div = document.createElement('div');
  div.onclick = function() {
    alert("you clicked on a box #" + i);
  }
  document.getElementsByTagName('section')[0].appendChild(div);
}
```
..will eliminate this problem, because now the `i` used in the click handler is the index number local to the `for` loop and therefore has the value of its current iteration, rather than the final value 42 as an `i` value hoisted to the outside of the `for` loop.

### The `const` keyword
The `const` keyword is short for 'constant' (surprise!) and allows us to set variables that cannot be _reassigned_.
```javascript
const x = 1;

x = 2
// TypeError: Assignment to constant variable
```

But `const` **_does not_** make a value _immutable_.
```javascript
const x = {
  feline: atiger
};

x.feline = alion;
console.log(x); // Logs { feline: alion }

x = {
  feline: apuma
};
// TypeError: Assignment to constant variable
```
We see we can _change_ object property values and even add new properties, but we cannot reassign `x`.

**[Back to top](#table-of-contents)**

## Template strings
Anyone who has used an JavaScript framework has seen a way to use variables directly in their markup (and often bind them to the model in a MV\*-type fashion). The new ES6 template strings allow us to do the same in JavaScript strings using backticks `` `your string with a ${variable}.` ``.

The old way to use variables in strings was to concatenate them:
```javascript
function greet(firstName) {
  console.log("Hello " + firstName);
}

greet(Raoul); //Logs Hello Raoul
```

In ES6 however we can use variables directly _in_ our strings using backticks:
```javascript
function greet(firstName) {
  console.log(`Hello ${firstName}`);
}

greet(Raoul); //Logs Hello Raoul
```

**[Back to top](#table-of-contents)**

## Spread operators
The spread operator can do a lot of powerful things with just three little dots...
It can turn elements of an array:
- into arguments of a function call
- into elements of an array literal

For example, if we just put a the variable name of one array into another array, in ES5 this doesn't put the actual elements of the first array into the second:
```javascript
const felines = ["Alion", "Atiger", "Acat"];
const canines = ["Awolf", "Afox", "Adog"];

const animals = ["Awhale", "Agiraffe", felines, "Asnake", canines, "Adragon"];
console.log(animals); //logs ["Awhale", "Agiraffe", Array[3], "Asnake", Array[3], "Adragon"];
```

But with the spread operater, we can 'inject' the elements of the first array directly into next:
```javascript
const felines = ["Alion", "Atiger", "Acat"];
const canines = ["Awolf", "Afox", "Adog"];

const animals = ["Awhale", "Agiraffe", ...felines, "Asnake", ...canines, "Adragon"];
console.log(animals); //logs ["Awhale", "Agiraffe", "Alion", "Atiger", "Acat", "Asnake", "Awolf", "Afox", "Adog", "Adragon"];
```

**[Back to top](#table-of-contents)**

## Default function parameters
In ES6, we can give a function 'default' parameters which will be used if the parameters are not specified in the function call.

A basic example:
```javascript
function add(x=5, y=7) {
  console.log(x + y);
}

add();    //logs 12 (5+7)
add(3);   //logs 10 (3+7)
add(2,6); //logs 8  (2+6)
```

**[Back to top](#table-of-contents)**

## Enhanced object literals
Object literals can now be declared in a shorter fashion:
```javascript
// the old way
var dairo = {
  // a property
  position: "programmer",
  
  // a property where name and value are the same
  fast: fast,
  
  // a method
  attack: function(enemy) {
    console.log("Scorpion kick the " + enemy);
  }
};

// the ES6 way
let dairo = {
  // a property
  position: "programmer",
  
  // a property where name and value are the same
  fast,
  
  // a method
  attack (enemy) {
    console.log("Scorpion kick the " + enemy);
  }
};
```

**[Back to top](#table-of-contents)**

## (fat) Arrow Functions
Arrow functions have an abbreviated syntax for working with functions:
```javascript
const myArray = [1, 2, 3];

// ES5 way of writing functions
const squares = myArray.map(function (x) {
  return x * x;
});

// Shorter with an arrow function when replacing 'function' keyword for '=>':
const squares = myArray.map((x) => {
  return x * x;
});

// Even shorter, because if there's only one argument it doesn't need braces:
const squares = myArray.map(x => {
  return x * x;
});

// Even shorter, because if your return code is one statement, you can omit return and enclosing brackets
const squares = myArray.map(x => x * x);
```
Beautiful! And very useful for functional programming, to have short simple functions that are easy to reason about and can be chained together.

### Lexical `this`
Fat arrow functions have a more intuitive handling of 'current' object context.

In previous versions of JavaScript, you had to use some tricks to keep a correct reference to `this` when using it inside a nested method or function, because using `this` within a nested method or function would automatically refer to that method or function:
```javascript
var alioner = {
  firstName: "Julian",
  secretNerdHobbies: ["read the Dark Tower series", "camp in CoD4", "hum songs from cartoons"],
  printHobbies: function() {
    this.secretNerdHobbies.forEach(function(hobby) {
      var toPrint = this.firstName + " likes to " + hobby;
      console.log(toPrint);
    });
  }
};

alioner.printHobbies(); 
// logs: 
// undefined likes to read the Dark Tower series
// undefined likes to camp in CoD4
// undefined likes to hum songs from cartoons
```
We would usually fix the above code with a `var self = this;` trick right inside the methode declaration, but arrow functions save us the trouble:
```javascript
let alioner = {
  firstName: "Julian",
  secretNerdHobbies: ["read the Dark Tower series", "camp in CoD4", "hum songs from cartoons"],
  printHobbies() {
    this.secretNerdHobbies.forEach(hobby => {
      let toPrint = this.firstName + " likes to " + hobby;
      console.log(toPrint);
    });
  }
};

alioner.printHobbies(); 
// logs: 
// Julian likes to read the Dark Tower series
// Julian likes to camp in CoD4
// Julian likes to hum songs from cartoons
```

**[Back to top](#table-of-contents)**

## Destructuring assignment
Destructuring assignment gives us an easy way to extract data from arrays and objects and assign them to variables.

### Destructuring arrays
In ES5, the way to assign elements of an array to a variable had to be done one by one, separate from the array declaration.
```javascript
var bannerHeroes = ["Michel", "Carlos", "Laurent", "Marijn", "Rodney"];

// if we want banner heroes that suck at 'Gang Beasts', we can get them like so:
var firstToBeThrownOfTheBuilding   = bannerHeroes[0]; //Michel
var secondToBeThrownOfTheBuilding  = bannerHeroes[3]; //Marijn
```

In ES6, we can map array elements to variables directly while declaring the array:
```javascript
[firstToBeThrownOfTheBuilding, , ,secondToBeThrownOfTheBuilding, ] = ["Michel", "Carlos", "Laurent", "Marijn", "Rodney"];

// if we want banner heroes that suck at 'Gang Beasts', we can get them like so:
console.log(firstToBeThrownOfTheBuilding); //Michel
console.log(secondToBeThrownOfTheBuilding); //Marijn
```
Note above that you don't have to name every value in the array, you can just skip them by not entering any name.

### Destructuring objects
In ES5 we can get to an objects properties by referring to both the object and the property name
```javascript
var sandwich = {
  title: "The Julian",
  price: 42,
  description: "Disgustingly tasty",
  ingredients: ['bread', 'peanut butter', 'sausage', 'cucumber', 'sambal']
}

console.log(sandwich.title); //logs "The Julian"
console.log(sandwich.price); //logs 42
```

In ES6 we can simply declare an empty object, with variable names that can be immediately accessed from the object's parent scope.
```javascript
let {title, price} = {
  title: "The Julian",
  price: 42,
  description: "Disgustingly tasty",
  ingredients: ['bread', 'peanut butter', 'sausage', 'cucumber', 'sambal']
}

console.log(title); //logs "The Julian"
console.log(price); //logs 42
```

The real use for destructuring assignment however becomes apparent when we use it as predefined variables we want to create when passing an object to a function:
```javascript
let vacation = {
  destination: "the moon",
  travelers: 2,
  activity: "moonwalking",
  cost: 20000
};

function vacationMarketing({destination, activity}) {
  return `Come to ${destination} and do some ${activity}!`;
}

console.log(vacationMarketing(vacation)); //logs "Come to the moon and do some moonwalking!"
```
See how the function parses the object that's passed to it and automatically maps values of the 'chosen' properties to variables of the same name.

**[Back to top](#table-of-contents)**

## ES6 Modules 
Modules are an extremely important feature in any programming language, which JavaScript sadly was lacking until now. We did have libraries such as CommonJS and AMD, but nothing native. That changes with ES6 modules, with which you can `export` functions/objects/classes and `import` them somewhere else.

To show a basic setup let's imagine a **utility.js** file that _exports_ functionality, and a **app.js** file that _imports_ these functions.

**lib/utility.js**
```javascript
function generateRandom() {
  return Math.random();
}

function sum(a, b) {
  return a + b;
}

export { generateRandom, sum }
```

The `generateRondom()` and `sum()` functions are not visible outside their own module unless they are explicitly _imported_, like we do in our app:

**app.js**
```javascript
import { generateRandom, sum } from 'lib/utility';

console.log(generateRandom()); //logs a random number
console.log(sum(39, 3)); //logs 42
```

Note that one the first line, we import blocks of code from a module: _this module is simple a reference to the module file._ After importing, the functions `generateRondom()` and `sum()` are available in our app without any restriction.

### Extra features

**Renaming exports**
We can rename the exported functions.
```javascript
export {generateRandom as random, sum as doSum}
```

**Importing complete utility**
We can also import a complete utility as an object and access exported values ar properties.
```javascript
import 'utility' as utils;

console.log(utils.sum(28, 14)); //logs 42
```

**Default Exports**
We can export a single value from a module and make that the default export.
Below we export a single object with the functions as methods:
```javascript
// lib/utility.js
var utils = {
  generateRandom: function() {
    return Math.random();    
  },
  sum: function(a, b) {
    return a + b;
  }
};
export default utils;

// app.js
import utils from 'lib/utility';
console.log(utils.sum(23, 19)); //logs 42
```

**[Back to top](#table-of-contents)**

## ES6 Class Syntax
ES6 offers us a new way of making classes.
**Note that the following is _just_ syntactic sugar, JavaScript _has no_ classes!**

I personally avoid using 'classes' and 'inheritance', favoring 'OLOO' and 'composition', but it is very probable we will encounter classes in JavaScript examples and documentation, so we at least need to understand the syntax:
```javascript
class Alioner {
  // properties specific to the future instance go in the constructor function
  constructor(position, geekLevel) {
    this.position   = position;
    this.geekLevel  = geekLevel;
  }

  // methods that are the same for every instance are declared outside of constructor function
  describeYourself() {
    console.log(`I am a ${this.position} with a geek level of ${this.geekLevel}.`);
  }
}

var Marijn = new Alioner("animator", 2000);
var Dennis = new Alioner("designer", -5);

Marijn.describeYourself(); //logs "I am a animator with a geek level of 2000.
Dennis.describeYourself(); //logs "I am a designer with a geek level of -5.
```
The nice thing about the new `class` syntax is that we can easily channel what properties/methods are re-created with each instance, and which properties/methods are delegated to the instance's prototype. This means that (if used correctly), the creation of objects is automatically geared for optimal performance.

### Class inheritance
Once we have a class, we can inherit from it and create another class. This is a process called 'extending' the class.
If we use the previous example with the 'Alioner' class:
```javascript
// original class
class Alioner {
  constructor(position, geekLevel) {
    this.position   = position;
    this.geekLevel  = geekLevel;
  }
  describeYourself() {
    console.log(`I am a ${this.position} with a geek level of ${this.geekLevel}.`);
  }
}

// new extended class
class AlionManager extends Alioner {
  constructor(favoriteSandwich) {
    // super works on inherited properties
    super("project manager", 1000000);
    
    // ..and we can also add new properties
    this.favoriteSandwich = favoriteSandwich;
  }
  // ..and add new methods
  whatAreYouEating() {
    console.log(`I am eating a sandwich I invented myself called ${this.favoriteSandwich}.`);
  }
}

var Julian = new AlionManager("the Julian");

Julian.describeYourself(); //logs "I am a project manager with a geek level of 1000000.
Julian.whatAreYouEating(); //logs "I am eating a sandwich I invented myself called the Julian."
```

**[Back to top](#table-of-contents)**

## Object.assign()
ES6 brought some syntactic sugar to those who favor classes and inheritance, but luckily for us there's also some genuinely awesome sugar dubbed `Object.assign()`. Together with the ES5 `Object.create()`, we can easily do OLOO style programming using prototypal delegation and never _ever_ have to look at classes again.

Object.assign() by itself is real simple - it takes a target object as first argument, and then as many source objects as you'd like to assign their properties to the target object
```javascript
  // Object.assign(target, ...sources);  
  
  let obj1 = { expertise1: "Display Advertising" };
  let obj2 = { expertise2: "Design & Development" };
  let obj3 = { expertise3: "Motion" };
  
  let alion = Object.assign({}, obj1, obj2, obj3, { expertise4: "Raoul's Café" });
  
  // 'alion' is now an object like so: 
  // Object {
  //   expertise1: "Display Advertising",
  //   expertise2: "Design & Development",
  //   expertise3: "Motion", 
  //   expertise4: "Raoul's Café"
  // } 
```
To show the true benefits of Object.assign() in combination with Object.create() would go beyond the scope of this article. However, I'm putting together another guide that focues solely on the creation of objects in JavaScript, which can be found here: [Prototypal Delegation in JavaScript](https://github.com/bobbybol/prototypal-delegation-in-javascript).

**[Back to top](#table-of-contents)**
