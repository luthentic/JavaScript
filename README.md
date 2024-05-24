
<p align="center">
  <img src = "https://miro.medium.com/v2/resize:fit:1200/1*LyZcwuLWv2FArOumCxobpA.png" width=800 height=500>
</p>

## 🚩 Table of Contents

- [Var](#var)
- [Const](#const)
- [Function Scope vs Block Scope](#function-scope-vs-block-scope)
- [Primitive data types](primitive-data-types)
- [Prototype](#prototype)
- [DOM](#dom)
- [CSSOM](#cssom)

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

```js

```

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

