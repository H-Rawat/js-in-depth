# How JS works?

Execution Context -> Everything in JS happens inside the execution context. This has 2 components: variable environment & thread of execution

#### Variable Environment

- also known as memory where variables and functions are stored as key and value pairs. For eg -> a: 5, fn: {...}

#### Thread of execution

- this means that code is executed one line at a time

**Javascript is a synchronous single-threaded language**

- this means that it can only go to the next line of code once the current line has been executed.

# How JS code is executed?

```
var n = 2;
function square(num) {
  var ans = num * num;
  return ans;
}

var square2 = square(n);
var square4 = square(4);
```

#### Phase 1: Memory Creation

- variables and function are stored in memory
- n: undefined
- square: {...}
- square2: undefined
- square4: undefined

#### Phase 2: Code Execution

- n: 2
- when the square function is called, it again runs the **Execution Context**
  - Memory Allocations: num: undefined, ans: undefined
  - Code Execution: num: 2, ans: 4
  - square2: 4
  - after the return statement, the Execution Context will be deleted
- same thing happens for square4, square4: 16
- JS is done executing the code, so now the whole global execution context will be deleted

## CALL STACK IN JS

- at the bottom of the call stack, global execution context exists
- newly created execution context will be inserted above global EC inside the stack
- once that newly created EC has been executed, it will be popped off the stack
- also known as : Execution context stack, program stack, runtime stack, control stack & machine stack

# Hoisting in JS

```
console.log(x);
console.log(getName);

var x = 7;
function getName() {
  console.log('Hey');
}
```

- here first log statement will print `undefined` and the other log statement will print the whole function, this is because before executing the program JS first allocates memeory for all the variables and functions where variables are set to `undefined` and functions to the code inside the body of the function.

# Window

- whenever a JS program runs, window object is created.
- at the global context, `this === window` return true

# Undefined vs Not Defined

- undefined is used as a placeholder in most cases
- not defined means that it hasn't been allocated any space in the memory

Note: `a = undefined`, avoid assigning undefined to any variable

# Lexical Environment

- Lexical environment is the local memory along with the lexical environment of its parent

```
function a() {
  var b = 10;
  c();

  function c() {
  }
}

a();
console.log(b);
```

- c function is lexically inside a function
- lexical environment is something which holds the local memory and also the reference to the lexical environment of its parent

Note: Scope is where you can access a specific variable or function in code

# Let and Const

```
let a = 10;
var b = 1;
console.log(a)
```

- here b is allocated memory and attached to the global object
- a is also allocated memory but stored in a different memory space that we can't access
- a can not be declared again

#### Temporal Dead Zone

- Time frame from the point a variable was hoisted to the point when it was initialized is known as temporal dead zone
- If you try to access the variable in this temporal dead zone, it will throw out a reference error
- happens with let and const

#### Const

- behaves just like `let` in case of hoisting
- in case of `const`, you will have to decalre and initialize the variable in the same line

# Difference btw SyntaxError, TypeError & ReferenceError

TypeError :

```
const b = 1000;
b = 1;
```

SyntaxError :

```
const b;
```

ReferenceError :

```
console.log(a);
let a = 1;
```

---

- Always prefer using `const` first and then `let`, avoid using `var`
- To avoid temporal dead zone, try declaring and initializing variables on the top of you file or scope

# Block Scope

- variable or functions that we can access inside a block(made up using curly braces) are called block scope variables or functions
- let and const are block scoped variables

# Shadowing

```
var a = 1;

{
  var a = 22;
}
```

- here `a` present inside the block shadows the variable `a` outside the block

#### In case of let and const

```
let a = 1;
const b = 2;

{
  let a = 11;
  const b = 22;
  console.log(a); // 11
  console.log(b); // 22
}

console.log(a); // 1
console.log(b); // 2
```

- inside the block, `a` and `b` shadow the `a` and `b` outside the block

# Illegal Shadowing

```
let a = 1;

{
  var a = 11;
}
```

- here it will throw an error because in a particular scope `let` can not be re-declared

- var is function scoped so the below code is fine

```
let a = 1;

function x() {
  var a = 2;
}
```

**Arrow functions scope is same as that of a normal function**

# Closures

- function along with its lexical scope forms a closure

```
function x() {
  var a = 7;
  function y() {
    console.log(a);
  }
  return y;
}

var z = x();
console.log(z);
z();
```

- this will print `7`, here when `y` was returned, not only the function was returned but the closure which had function and its lexical scope. That's why it still has the reference to a, and it prints that.

Uses of closure -

- module design pattern
- currying
- functions like `once`
- memoize
- maintaining state in async world
- setTimeouts
- Iterators

# SetTimeout

```
function x() {
  for (var i = 1; i <= 5; i++) {
    setTimeout(function() {
      console.log(i);
    }, 3000);
  }

  console.log('Namaste');
}

x();
```

- here the function is stored somewhere and it also remembers the reference to the variable `i`
- while JS is waiting to run the function after 3 seconds, it is also running the loop simultaneously, which is why it prints `6` after 3 seconds
- for each copy of the function, they refer to the same `i`, so it prints 6 everytime

To avoid this behaviour you can use let instead of var.

- You can also achieve this like this using var

```
function close(x) {
  setTimeout(function() {
    console.log(x);
  }, x * 1000);
}
close(i);
```

- here it will create a new copy of `i` everytime setTimeout is called.

# More on closures

```
function counter() {
  var count = 0;
  return function increment() {
    count++;
    console.log(count);
  }
}

var counter1 = counter();
counter1(); // prints 1
counter1(); // prints 2

var counter2 = counter();
counter2(); // prints 1
```

### Constructor function

```
function Counter() {
  var count = 0;

  this.increment = function() {
    count++;
    console.log(count);
  }

  this.decrement = function() {
    count--;
    console.log(count);
  }
}

var counter1 = new Counter();

counter1.increment();
counter1.decrement();
```

### Disadvantages of Closure

- overconsumption of memory is possible because in some cases variables are not garbage collected until the complete program expires

Garbage collector -> program in the JS engine which free up the memory by removing the unutilized variables

# Function Statement / Function Declaration

```
function a() {
  console.log('fakjfa');
}
```

# Function Expression

```
var b = function() {
 console.log('jfadkjs');
}
```

Difference between function Statement and Expression is that in statement the hoisting works which means that i will not get the undefined error.

# Anonymous Function

```
var a = function() {

}
```

# Named function expression

```
var b = function xyz() {
  ...
}

xyz(); // this will throw an error
```

# First class function

- the ability to pass functions as arguments to another functions & return functions from a function & assigning functions to variables is called first class functions
- functions are first class citizens means the same thing
