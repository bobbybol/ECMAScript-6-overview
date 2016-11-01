# A quick overview of ECMAScript 6
Summary of the most important new features of ES6 and how they differ from ES5.

This short guide is written for colleagues and pair-programming buddies to use as a reference while working through code that was written in or refactored using new ES6 syntax/functionalities.

This list is by no means exhaustive, but should cover all the features to get you up and running with modern JavaScript frameworks such as **vue.js**, **ReactJS**, and **Aurelia**.

## let and const
The first thing you'll notice when looking through ES6 code, is its lack of `var`s.
This is because `var` has essentially (save for some rare edge cases) been replaced by `let` and `const`.

### The `let` keyword:
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

Benefits of `let` are apparent in the `for` loop. Consider the following loop where we simply want to add 45 `div`s to a `section`, and give them a click handler that alerts the index number of each `div`:
```javascript
for(var i=0; i<45; i++) {
  var div = document.createElement('div');
  div.onclick = function() {
    alert("you clicked on a box #" + i);
  }
  document.getElementsByTagName('section')[0].appendChild(div);
}
```
The above will log out \#45 for each and every `div` that's clicked. Simply replacing `var` for `let` in the `for` loop like so: 
```javascript
for(let i=0; i<45; i++) {
  var div = document.createElement('div');
  div.onclick = function() {
    alert("you clicked on a box #" + i);
  }
  document.getElementsByTagName('section')[0].appendChild(div);
}
```
..will eliminate this problem, because now the `i` used in the click handler is the index number local to the `for` loop and therefore has the value of its current iteration, rather than the final value 45 as an `i` value hoisted to the outside of the `for` loop.

### The `const` keyword:
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

## (fat) Arrow Functions
Arrow functions have an abbreviated syntax for working with functions:
```javascript
// use example of MPJ
```

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


## Object.assign();
