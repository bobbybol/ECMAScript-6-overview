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



## Object.assign();
