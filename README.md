
<p align="center">
  <img src = "https://miro.medium.com/v2/resize:fit:1200/1*LyZcwuLWv2FArOumCxobpA.png" width=800 height=500>
</p>

## 🚩 Table of Contents

- [Var](#var)
- [Const](#const)
- [Function Scope vs Block Scope](#function-scope-vs-block-scope)
- [Primitive data types](primitive-data-types)
- [Prototype](#prototype)
- [Generator](#generator)
- [IIFE](#IIFE)
- [Argument Object](#argument-object)
- [Strict Mode](#strict-mode)

--- 

## Var:

let is in a child scope of var, not the same scope:

```js
var a = 1;
{
  let a = 2;
}
```

Only a variable's declaration is hoisted, not its initialization. The initialization happens only when the assignment statement is reached. Until then the variable remains undefined (but declared):

```js
function doSomething() {
  console.log(bar); // undefined
  var bar = 111;
  console.log(bar); // 111
}
```

---






## Const:

object keys are not protected, so the following statement is executed without problem.

### Object:
```js
MY_OBJECT.key = "otherValue";
```

### Array:
```js
MY_ARRAY.push("A"); // ["A"]
```

---








## Function Scope vs Block Scope:


```js
var character4 = 
function foo() {
    if(true) {
        var character1 = "Robin"      //function scope
        let character2 = "Ted"        //block scope
        const character3 = "Barney"   //block scope
    }
    console.log(character1)  //Robin
    console.log(character2)  //not defined
    console.log(character3). //not defined
}
```

---












## 7 primitive data types:

Primitives have no methods but still behave as if they do. When properties are accessed on primitives, JavaScript auto-boxes the value into a wrapper object and accesses the property on that object instead. For example, "foo".includes("f") implicitly creates a String wrapper object and calls String.prototype.includes() on that object. This auto-boxing behavior is not observable in JavaScript code but is a good mental model of various behaviors — for example, why "mutating" primitives does not work (because str.foo = 1 is not assigning to the property foo of str itself, but to an ephemeral wrapper object).

- string

The String() function is a more reliable way of converting values to strings than calling the toString() method of the value, as the former works when used on null and undefined.


```js
// You cannot access properties on null or undefined

const nullVar = null;
nullVar.toString(); // TypeError: Cannot read properties of null
String(nullVar); // "null"

const undefinedVar = undefined;
undefinedVar.toString(); // TypeError: Cannot read properties of undefined
String(undefinedVar); // "undefined"

```

String literals (denoted by double or single quotes) and strings returned from String calls in a non-constructor context (that is, called without using the new keyword) are primitive strings. In contexts where a method is to be invoked on a primitive string or a property lookup occurs, JavaScript will automatically wrap the string primitive and call the method or perform the property lookup on the wrapper object instead.


```js
const strPrim = "foo"; // A literal is a string primitive
const strPrim2 = String(1); // Coerced into the string primitive "1"
const strPrim3 = String(true); // Coerced into the string primitive "true"
const strObj = new String(strPrim); // String with new returns a string wrapper object.

console.log(typeof strPrim); // "string"
console.log(typeof strPrim2); // "string"
console.log(typeof strPrim3); // "string"
console.log(typeof strObj); // "object"
```



String primitives and String objects also give different results when using eval(). Primitives passed to eval are treated as source code; String objects are treated as all other objects are, by returning the object.

```js
const s1 = "2 + 2"; // creates a string primitive
const s2 = new String("2 + 2"); // creates a String object
console.log(eval(s1)); // returns the number 4
console.log(eval(s2)); // returns the string "2 + 2"
```


A String object can always be converted to its primitive counterpart with the valueOf() method

```js
console.log(eval(s2.valueOf())); // returns the number 4
```

- number

Number() - Creates a new Number value.

When Number is called as a constructor (with new), it creates a Number object, which is not a primitive. For example, typeof new Number(42) === "object", and new Number(42) !== 42 (although new Number(42) == 42)


- bigint

BigInt values represent numeric values which are too large to be represented by the number primitive.

- boolean

- undefined

undefined is a property of the global object. That is, it is a variable in global scope.

In all non-legacy browsers, undefined is a non-configurable, non-writable property. Even when this is not the case, avoid overriding it.


- symbol

Symbol is a built-in object whose constructor returns a symbol primitive — also called a Symbol value or just a Symbol — that's guaranteed to be unique. Symbols are often used to add unique property keys to an object that won't collide with keys any other code might add to the object, and which are hidden from any mechanisms other code will typically use to access the object. That enables a form of weak encapsulation, or a weak form of information hiding


- null

Difference between null and undefined
When checking for null or undefined, beware of the differences between equality (==) and identity (===) operators, as the former performs type-conversion.

```js
typeof null; // "object" (not "null" for legacy reasons)
typeof undefined; // "undefined"
null === undefined; // false
null == undefined; // true
null === null; // true
null == null; // true
!null; // true
Number.isNaN(1 + null); // false
Number.isNaN(1 + undefined); // true
```



---








## Prototype:

```js
// A constructor function
function Box(value) {
  this.value = value;
}

// Properties all boxes created from the Box() constructor
// will have
Box.prototype.getValue = function () {
  return this.value;
};

const boxes = [new Box(1), new Box(2), new Box(3)];
```

The above constructor function can be rewritten in classes as:


```js
class Box {
  constructor(value) {
    this.value = value;
  }

  // Methods are created on Box.prototype
  getValue() {
    return this.value;
  }
}
```

**Warning: ** There is one misfeature that used to be prevalent — extending Object.prototype or one of the other built-in prototypes. An example of this misfeature is, defining Array.prototype.myMethod = function () {...} and then using myMethod on all array instances.

This misfeature is called **monkey patching**. Doing monkey patching risks forward compatibility, because if the language adds this method in the future but with a different signature, your code will break. It has led to incidents like the SmooshGate, and can be a great nuisance for the language to advance since JavaScript tries to "not break the web".

The only good reason for extending a built-in prototype is to backport the features of newer JavaScript engines, like Array.prototype.forEach.

---


## Map:

Maps can be merged, maintaining key uniqueness:

```js
onst first = new Map([
  [1, "one"],
  [2, "two"],
  [3, "three"],
]);

const second = new Map([
  [1, "uno"],
  [2, "dos"],
]);

// Merge two maps. The last repeated key wins.
// Spread syntax essentially converts a Map to an Array
const merged = new Map([...first, ...second]);

console.log(merged.get(1)); // uno
console.log(merged.get(2)); // dos
console.log(merged.get(3)); // three
```

```js
const first = new Map([
  [1, "one"],
  [2, "two"],
  [3, "three"],
]);

const second = new Map([
  [1, "uno"],
  [2, "dos"],
]);

// Merge maps with an array. The last repeated key wins.
const merged = new Map([...first, ...second, [1, "eins"]]);

console.log(merged.get(1)); // eins
console.log(merged.get(2)); // dos
console.log(merged.get(3)); // three
```
---










## Generator:

### What Are Generators?
- Generators are special functions that can be paused and resumed.
- They allow you to yield multiple values one after another, on-demand.
- Created using the function* syntax (generator function).
### How Generators Work:
- When called, a generator function doesn’t execute its code immediately.
- Instead, it returns a “generator object” that manages execution.
- The main method of a generator is next(), which runs until the nearest yield statement.
- The result of next() includes value (yielded value) and done (true if finished).

```js
function* generateSequence() {
    yield 1;
    yield 2;
    return 3;
}
let generator = generateSequence();
let one = generator.next(); // { value: 1, done: false }
let two = generator.next(); // { value: 2, done: false }
let three = generator.next(); // { value: 3, done: true }
```

### Iterating with Generators:
- Generators are iterable, making them great for loops.
- Use for..of to loop over their values.
- Note that the last value (when done: true) is ignored.

```js
function* generateSequence() {
    yield 1;
    yield 2;
    return 3;
}

let generator = generateSequence();

for (let value of generator) {
    console.log(value); // Outputs: 1, then 2
}
```

## IIFE:

```js
const makeWithdraw = (balance) =>
  ((copyBalance) => {
    let balance = copyBalance; // This variable is private
    const doBadThings = () => {
      console.log("I will do bad things with your money");
    };
    doBadThings();
    return {
      withdraw(amount) {
        if (balance >= amount) {
          balance -= amount;
          return balance;
        }
        return "Insufficient money";
      },
    };
  })(balance);

const firstAccount = makeWithdraw(100); // "I will do bad things with your money"
console.log(firstAccount.balance); // undefined
console.log(firstAccount.withdraw(20)); // 80
console.log(firstAccount.withdraw(30)); // 50
console.log(firstAccount.doBadThings); // undefined; this method is private
const secondAccount = makeWithdraw(20); // "I will do bad things with your money"
console.log(secondAccount.withdraw(30)); // "Insufficient money"
console.log(secondAccount.withdraw(20)); // 0
```

### IIFE and Private Scope:
- The outer function (copyBalance) => { ... } is an IIFE.
- It immediately executes and returns an object with a withdraw method.
- Inside this IIFE, the balance variable is scoped to the function, making it private (not accessible from outside).
### Accessing the withdraw Method:
- Despite the private scope, the withdraw method is accessible from outside the IIFE.
- Why? Because the returned object (with the withdraw method) is assigned to firstAccount.
- The withdraw method is part of that object, so it’s accessible via firstAccount.withdraw.
### Private Variables vs. Public Methods:
- The withdraw method is not private; it’s part of the public interface.
- However, the balance variable is private because it’s scoped within the IIFE.
- The doBadThings function is also private and inaccessible from outside.

---



## Argument Object:
Non-strict functions that only have simple parameters (that is, no rest, default, or destructured parameters) will sync the new value of parameters with the arguments object, and vice versa

```js
function func(a) {
  arguments[0] = 99; // updating arguments[0] also updates a
  console.log(a);
}
func(10); // 99

function func2(a) {
  a = 99; // updating a also updates arguments[0]
  console.log(arguments[0]);
}
func2(10); // 99
```

Non-strict functions that are passed rest, default, or destructured parameters will not sync new values assigned to parameters in the function body with the arguments object. Instead, the arguments object in non-strict functions with complex parameters will always reflect the values passed to the function when the function was called.

```js
function funcWithDefault(a = 55) {
  arguments[0] = 99; // updating arguments[0] does not also update a
  console.log(a);
}
funcWithDefault(10); // 10

function funcWithDefault2(a = 55) {
  a = 99; // updating a does not also update arguments[0]
  console.log(arguments[0]);
}
funcWithDefault2(10); // 10

// An untracked default parameter
function funcWithDefault3(a = 55) {
  console.log(arguments[0]);
  console.log(arguments.length);
}
funcWithDefault3(); // undefined; 0
```



---












## Strict Mode:

### Strict mode for modules:
The entire contents of JavaScript modules are automatically in strict mode, with no statement needed to initiate it.

```js
function myStrictFunction() {
  // because this is a module, I'm strict by default
}
export default myStrictFunction;
```

### Strict mode for classes:
All parts of a class's body are strict mode code, including both class declarations and class expressions.

```js
class C1 {
  // All code here is evaluated in strict mode
  test() {
    delete Object.prototype;
  }
}
new C1().test(); // TypeError, because test() is in strict mode

const C2 = class {
  // All code here is evaluated in strict mode
};

// Code here may not be in strict mode
delete Object.prototype; // Will not throw error
```


