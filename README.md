# A quick overview of ECMAScript 6
Summary of the most important new features of ES6 and how they differ from ES5.

This short guide is written for colleagues and pair-programming buddies to use as a reference while working through code that was written in or refactored using new ES6 syntax/functionalities.

## `let` and `const`
The first thing you'll notice when looking through ES6 code, is its lack of `var`s.
This is because `var` has essentially (save for some rare edge cases) been replaced by `let` and `const`.

### 'let'
We can use the `let` keyword to create _block scoping_ in locations where we weren't able to do so before.

With `var` the value changes inside the `if` statement:
```javascript
var x = 10;

if(x) {
  var x = 4;
}
```
