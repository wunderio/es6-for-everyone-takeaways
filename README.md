
# Finding from ES6 For Everyone

## Topic 1: New Variables

#### lesson 01 - var Scoping Refresher	

* `var` is function scoped and can be re-declarable in the same scope
* `let`/`const` are block scoped
* see http://2ality.com/2015/02/es6-scoping.html


#### lesson 02 - let VS const

* `let` & `const` can be re-declarable only in a different block (even a sub-block to one with an existing same declaration, which can produce logic error hard to track)
* `const`: _binding_ of thing to name can't be changed after declaration.
* `Object.freeze();` / `Object.isFrozen();` makes/checks an object _shallowly_(!) immutable. See:
  * https://goo.gl/IeQwbN
  * https://goo.gl/P8Poji
  * https://goo.gl/NMHtbJ


#### lesson 03 - let and const in the Real World

* Instantly-Invoked-Function-Expression (IIFE): `(function() {})();`
* In `for(CLAUSE) { … }`, the block scope {} is already extended to the `CLAUSE`

#### lesson 04 - Temporal Dead Zone

* `var`-declarations are implicitely moved to the top of the function scope they're defined in, and thus exist and can be accessed in that scope _before_ their explicit declaration.
* `let`/`const`-declarartions are not implicitely moved and do thus not exist before their explicit declaration.


#### lesson 05 - Is var Dead? What should I use?

Personal pick as best practice for variable declaration in _newly_ written code:

* use `const` by default
* only use `let` if rebinding is needed
* (`var` shouldn’t be used in ES6)

see http://wesbos.com/is-var-dead



## Topic 2: Arrow Functions.


#### lesson 06 - Arrow Functions Introduction

An arrow function is a compact (ideally one-line and in-line) notation of an anonymous function.

* `const someArray = [item1, item2, item3]`
* `someArray.map(someFunction)`
* `(arg1, arg2) => {…}` equals `function(arg1, arg2) {…}`
* `singleArg => {…}` equals `function(singleArg) {…}`
* `() => {…}` equals `function() {…}`
* Implicit return: `singleArg => singleArg+1` equals `function(singleArg) { return singleArg+1; }`
* Named arrow function: `const funcName = singleArg => singleArg+1;` 
* `=>` is called "fat arrow"


#### lesson 07 - More Arrow Function Examples

* Implicitly return an object literal: `singleArg => ({ obj: SingleArg })`
* On function names: http://2ality.com/2015/09/function-names-es6.html
* Console methods
  * `console.table(v);` - renders nice table view of structure parameter `v`.
  * `console.trace();` - outputs a call stack trace.
  * `console.clear();` - clear console.
  * `console.dir();` - less cluttering on first level of structure output?
  * filter: `Array.filter(booleanFunction);` e.g. `[32,5,678].filter(num => num > 100)`


#### lesson 08 - Arrow Functions and `this`

* In a regular function, `this` is defined/rebound by the caller (and thus points e.g. to `window` for many functions or a rebound value e.g. for an event callback).
* In an arrow function, `this` is _not_ rebound in an arrow function, but is inherited from the function's execution context (i.e. _parent_ scope)!
* =>
  * use a regular function, if you need `this` to be rebound/different from its value in the calling scope.
  * use an arrow function, if you need `this` to stay the same as in the calling scope. 
* `console.log(this)` names and shows the DOM element if `this` is a DOM element. For use of e.g. jQuery objects, you have to console.log the underlying DOM element instead of the jQuery object!


#### 09 - Default Function Arguments

* In ES6, arguments can be defined with a default parameters:
  ```
  function f(a, b=1, c=2) { return a*b + c; }
  f(1); // =3
  f(2); // =4
  f(2, 2); //=6
  f(2, 2, 1); //=5
  f(1, undefined, 3); //=4
  ```
* optional arguments:
  ```
  function f(a, b=null, c=null) { ... }
  ```

#### 10 - When NOT to use an Arrow Function

* Don't use an arrow function, when you need `this`to be rebound
  * event handler callback
  * object methods
  * need access to the ```arguments``` array




# Other findings

* ES6 support in JS engines: http://kangax.github.io/compat-table/es6
* Scope / scope chain / execution context:
  * see
    * http://www.digital-web.com/articles/scope_in_javascript
    * http://2ality.com/2015/02/es6-scoping.html
  * Evaluating a function establishes a distinct execution context that appends its local scope to the scope chain it was defined within.
  * JavaScript resolves identifiers within a particular context by climbing up the scope chain, moving locally to globally.
  
* `this` in functions (see http://stackoverflow.com/a/3127440 / http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it /  https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/this#The_bind_method)
  * is not rebound in arrow functions but inherited from the arrow function's execution context / calling scope 
  * is rebound explicitely via the `thisArg` argument when called via one of the following 8 special cases (see http://stackoverflow.com/a/3127440 and http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals):
    * Function.prototype.apply( thisArg, argArray )
    * Function.prototype.call( thisArg [ , arg1 [ , arg2, ... ] ] )
    * Function.prototype.bind( thisArg [ , arg1 [ , arg2, ... ] ] )
    * Array.prototype.every( callbackfn [ , thisArg ] )
    * Array.prototype.some( callbackfn [ , thisArg ] )
    * Array.prototype.forEach( callbackfn [ , thisArg ] )
    * Array.prototype.map( callbackfn [ , thisArg ] )
    * Array.prototype.filter( callbackfn [ , thisArg ] )
  * is bound to the object the function has been part of when being called
  * certain library's functions (e.g. jQuery's) manipulate the value of `this`. 
  * can be manually bound via ES5's `f.bind(t)`, which returns a cloned instance of function f, whose `this` is permanently bound to `t`.
  * see http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it
* functions can be instantiated as an object:
  ```
  function f(x) {
      this.z = function(y) { return y + this.somedata }
      this.somedata = x;
  }

  let test = new f(5);
  console.log( test.z(2) ); // "7"
  ```
* Object property key definitions 
  ```
  obj = { 
      m1() {}, // 'm1'
      m2: function () {}, // 'm2'
      ['m' + '3']: function () {}, // 'm3' - computed property key
      func, // 'func' - property value shorthand
  };```
* classes
  * http://exploringjs.com/es6/ch_classes.html#sec_species-pattern
* ammending an object => adds a method/property only to that one object instance:
  ```
  class Test { }
  t.newFunc1 = function () { };
  ```
  vs. ammending an object's class (via the object's __proto__ property or the class's prototype) => adds the new method/property to all instances of that class:
  ```
  class Test { }
  t.__proto__.newFunc2 = function () { };
  Test.prototype.newFunc3 = function () { };
  ```
* Arrays: New ES6 methods of Array. or any extended class like MyArray.
  * `.isArray()`
  * `.keys()`
  * `.values()`
  * `.entries()`
  * `.forEeach(function(item, index, array) {})`
  * `.from(source, mapFunction?, thisArg?)` - Creates a new Array instance from an array-like or iterable object, via optional mapFunction.
  * `.filter(item => item>10 ? true : false)`
  * `.find(booleanFilterFunc(element, index, array))` - returns the first array element for which the callback predicate returns true 
  * `.push(item)`
  * `.pop()`
  * `.shift()`
  * `.unshift(item)`
  * `.indexOf(value)`
  * `.slice()` - creates a shallow copy
  * `.fill(value, startIndex=0, endIndex=max)` - fills array elements (all or in the optionally specified range) with value, e.g. `new Array(3).fill(7)`
  * `.map(item => ({ 'text': item.text })`
  * `.reduce(reduceCallback, startValue)`
     * `startValue` is sometimes essential to specify explicitely, even if 0.
     * reduceCallback is of signature `reduceCallback(accumulator, currentValue, currentIndex?, array?) { ... }`
     * accumulator can also be an array, e.g. to count occurances to specific item types in rthe array.
     * See https://goo.gl/IQJliK
  * See
    * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array
    * http://2ality.com/2014/05/es6-array-methods.html
* String
  * `.startWidth()`
  * `.endsWith()`
  * `.includes()`
  * `.match(regExp)` e.h. with regExp = `/searcHWord/i` to do an case-insensitive search.
  * `*1` converts a string to number, so `+` is an addition and not a concatenation 
  
# Resources

* https://mathiasbynens.be/notes#javascript
* http://adripofjavascript.com
* http://2ality.com/2015/09/function-names-es6.html
* http://exploringjs.com/es6.html* 
