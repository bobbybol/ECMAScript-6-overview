# A quick overview of ECMAScript 6
Summary of the most important new features of ES6 and how they differ from ES5.

This short guide is written for colleagues and pair-programming buddies to use as a reference while working through code that was written in or refactored using new ES6 syntax/functionalities.

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
The above will log out /#45 for each and every `div` that's clicked. Simply replacing `var` for `let` in the `for` loop like so: 
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
