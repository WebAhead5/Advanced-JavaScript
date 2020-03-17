# Advanced JavaScript

## 1. Value vs. reference types in JavaScript
Value types or primitives are independent variables. And they can be any of the following types:
1. Numbers
2. Strings
3. Booleans
4. Symbols
5. Undefineds
6. Nulls
Reference types (or Objects):
1. Objects
2. Functions
3. Arrays 

```javascript=
//Value types code example
let x=10;
let y=x;
x=20;

//x and y are independent variables. for the outcome will be; x=20 and y =10;

//Reference code example
let x = { value: 10};
let y=x;
x.value= 20;

// the outcomes: x=20 and y=20
```

Value type is stored within a variable. however, object is stored somewhere in memory and the address
of that memory location is stored inside the variable. So, primitives are copied by their value and 
objects are copied by their reference. Hint when a reference is modified, the value of any object pointing 
to that reference will also get modified.

```javascript=
//the aim for the following code is to increase the number by one.

//Value types 
let number=10;
function increase(number){
    number++;
}
increase(number);
console.log(number);
// this code will return the same number we started with, that is 10; see variables scope!

//Reference types 
let numberObj={value: 10};
function increase(numberObj){
    numberObj.value++;
}
increase(numberObj);
console.log(numberObj);
// the outcome: 11;
```



---

## 2. == vs. ===

### Introduction

In this section, I'll be talking you through the differences between the == and === operators, and why neither work when comparing reference types, such as arrays, objects and functions. 

In Javascript, we have couple of options for checking equality:

* == (Double equals operator): Known as the equality or abstract comparison operator
* === (Triple equals operator): Known as the identity or strict comparison operator

As we can see, both operators can be used to check for equality of different variables:

```javascript
var foo = 13;
var bar = 13;

console.log(foo ==  bar); // true
console.log(foo === bar); // also true
```

### The Difference between == and ===

The difference between == and === is that:

* == converts the variable values to the same type before performing comparison. This is called type coercion.
* === does not do any type conversion (coercion) and returns true only if both values and types are identical for the two variables being compared.

```javascript
var one = 1;
var one_string = "1";  // note: this is string

console.log(one ==  one_string); // true. See below for explanation.
console.log(one === one_string); // false. See below for explanation.
```

* console.log(one == one_string) returns true because both variables, one and one_string contain the same value even though they have different types: one is of type Number whereas one_string is String. But since the == operator does type coercion, the result is true.
* Line 8: console.log(one === one_string) returns false because the types of variables are different.
 
### Which should you use as your default?

You might think that === would have a slower performance in your code (since it requires the additional job of coercion), but in most cases, === runs quicker than ==. In any case, the performance is actually negligible - to the point of being irrelevant. 

It turns out it is better practice to use === as the default equality checking operator. This will avoid confusion in cases such as:
* "1" == true //returns true
* "" == 0     //returns true

Most linters (software that checks your code for errors) also don't like to see the ==, and will flag them for checking.

### Inequality Operators
== and === have their counterparts when it comes to checking for inequality:
* !=: Converts values if variables are different types before checking for inequality
* !==: Checks both type and value for the two variables being compared

They behave as you'd expect, but it's worth noting that we drop an '=' character when we check for inequality:

```javascript
var one = 1;
var one_string = "1";  // note: this is a string

console.log(one != one_string); // false
console.log(one !== one_string);// true. Types are different
```

### Arrays and Objects

The previous examples explored the use of the equality operators on primitive types, but what if we want to check reference types, like arrays or objects?

It turns out we can't:

```javascript
var a1 = [1,2,3,4,5]
var a2 = [1,2,3,4,5]

console.log(a1 ==  a2); // false
console.log(a1 === a2); // false
```

As you can see, the two arrays 'a1' and 'a2' both have identical contents, but each variable points to different objects in memory. This applies to objects and presumably other reference types - ie, functions.



ECMAScript6 instruced a new method for comparing values:
* [Object.is()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

Object.is() makes a few things less confusing, such as:

```javascript
console.log(NaN === NaN); // false
console.log(Object.is(NaN, NaN));// true
```

But it still doesn't allow you to compare arrays, or any other reference type.


### In Summary (TLDR)
* == and === are known as 'equality operators'. They both check for value, but only === checks for type.
* Use === in normal circumstances. Only use == where you are looking for equal values of different types.
* The operators don't work for reference types. For these you'll need a more complicated method of comparing values - eg, loop through each item of two arrays comparing items.

<details><summary>Further Reading</summary>
<p>



### Glossary of Terms
#### Linter
lint, or a linter, is a tool that analyzes source code to flag programming errors, bugs, stylistic errors, and suspicious constructs. The term originates from a Unix utility (aka, a program) that examined C language source code.
#### Primitive Types
These six types are considered to be primitives. A primitive is not an object and has no methods of its own. All primitives are immutable.
* Boolean — true or false
* Null — no value
* Undefined — a declared variable but hasn’t been given a value
* Number — integers, floats, etc
* String — an array of characters i.e words
* Symbol — a unique value that's not equal to any other value
#### Reference Types
Javascript has 3 data types that are passed by reference: Array, Function, and Object. These are all technically Objects.

### Things I Haven't Worked Out Yet

* DESCRIBE COERCION
* WHY DO WE DROP THE EQUALS SIGN?
* WHY IS === QUICKER THAN ==
* WHAT'S THE MOST EFFICIENT WAY TO COMPARE ARRAYS, OBJECTS, FUNCTIONS?

</p>
</details>

---

# Scope
## Definition
The scope is the accessibility of variables, functions, or objects in some particular part of your code.

The scope of a variable or a function is the part of the code in which it is available.


An example of a bug some of you surely faced at some point cased of these so-called "scopes":
```javascript
    function someFunction() {
        let someString = "Hello World!"; // local scope variable (bear with me)
    }
    console.log(someString); //logs "undefined"
```

## Why should we care to learn it?
* They provide some level of security to your code, i.e are only used when they are really needed.
* It solves the naming problem when you have variables with the same name but in different scopes, so reduce namespace collision.

## Scope Types
### In JavaScript there are three main types of scopes
1. ***Global Scope***
    * There is only one Global scope throughout a JavaScript document.
    * When you start writing JavaScript in a document, you are already in the Global scope.
    * variables diclared outside functions are considered global scope variables.
    * global scope variables are accessable within the entire document file (incliding inner functions). 
    
2. ***Local Scope***

     variables defines with a ***let*** or ***const*** keywords within a block or a function.
     these variables are only knows to their containing block/function. 
    

    
    the local scope incude the following definitions:
    * **Function Scope**
parameters and variables defined in a function are visible everywhere within the function, but are not visible outside of the function.
 (variables having the same name can be used in different functions. This is because those variables are bound to their respective functions).
    
    * **Block Scope**
Block scope is defined with curly braces. It is separated by { and }.
which includes block statements such as *if* and *switch* conditions or *for* and while loops.
 They can only be accessed in the block in which they are defined. Example:

      ```javascript
      let x = 1;
      { 
      let x = 2;
      }
      console.log(x); //1
      ```

         In contrast, the var declaration has no block scope:

        ```javascript
        var x = 1;
        { 
          var x = 2;
        }
        console.log(x); //2
        ```
    
3. ***Module scope***
 In modules, a variable declared outside any function is hidden and not available to other modules unless it is explicitly exported.

    
    Exporting makes a function or object available to other modules. In the next example, I export a function from the sequence.js module file:

    ```javascript
    export { sequence, toList, take };
    ```
    Importing makes a function or object, from other modules, available to the current module.

    ```javascript    
    import { sequence, toList, toList } from "./sequence";
    ```
    
    In a way, we can imagine a module as a self-executing function that takes the import data as inputs and returns the export data.



# Closure
## Introduction
An important part of working with functions, with JavaScript, is understanding the topic known as closures. Closures touch upon the area where functions and 
variable scope intersect. We can have functions that contain functions inside them. In the following example, we have our youSayGoodBye function that contains 
an alert and another function called andISayHello

```javascript
function youSayGoodBye() {

    alert("Good Bye!");

    function andISayHello() {
        alert("Hello!");
    }

    return andISayHello;
}
```
The interesting part is what the youSayGoodBye function returns when it gets called. It returns the andISayHello function.

## Examples

The moment the following line of code runs, all of the code inside your youSayGoodBye function will get run as well. This means, you will see a dialog that says Good Bye
```javascript
var something = youSayGoodBye();
```
As part of running to completion, the andISayHello function will be created and then returned as well. At this point, our something variable only has eyes for one thing, 
and that thing is the andISayHello function. The youSayGoodBye outer function, from the something variable's point of view, simply goes away. Because the something variable now points to a function, you can invoke this function by just calling it using the open and close parentheses like you normally would:

```javascript
var something = youSayGoodBye();
something();
```
When you do this, the returned inner function (aka andISayHello) will execute. Just like before, you will see a dialog appear, but this dialog will say Hello! - which is 
what the alert inside this function specified.

## When the Inner Functions Aren't Self-Contained
In many real scenarios, very rarely will we run into a case like this. We will often have variables and data that are shared between the outer function and the inner function. To highlight this, take a look at the following:
```javascript
function stopWatch() {
    var startTime = Date.now();

    function getDelay() {
        var elapsedTime = Date.now() - startTime;
        alert(elapsedTime);
    }
    return getDelay;
}
```
This example shows a very simple way of measuring the time it takes to do something. This example shows a very simple way of measuring the time it takes to do something. Inside the stopWatch function, we have a startTime variable that is set to the value of Date.now(). We also have an inner function called getDelay.The getDelay function displays a dialog containing the difference in time between a new call to Date.now() and the startTime variable declared earlier.Getting back to the outer stopWatch function, the last thing that happens is that it returns the getDelay function before exiting. 
The full markup and code for this example looks as follows:
```htmlembedded=
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>Closures</title>

  <style>

  </style>
</head>

<body>
  <script>
    function stopWatch() {
      var startTime = Date.now();

      function getDelay() {
        var elapsedTime = Date.now() - startTime;
        alert(elapsedTime);
      }

      return getDelay;
    }

    var timer = stopWatch();

    // do something that takes some time
    for (var i = 0; i < 1000000; i++) {
      var foo = Math.random() * 10000;
    }

    // invoke the returned function
    timer();
  </script>
</body>

</html>
```
If you run this example, we'll see a dialog displaying the number of milliseconds it took between your timer variable getting initialized, your for loop running to completion, and the timer variable getting invoked as a function.

video reference: https://www.youtube.com/watch?v=rBBwrBRoOOY&feature=emb_title





# 5. Clean code concepts

Here are some examples I've stolen from [this website](https://github.com/ryanmcdermott/clean-code-javascript/edit/master/README.md) on what is meant by clean code. To keep this short I've limited it to code that deals with variables:

### Use meaningful and pronounceable variable names

**Bad:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

### Use the same vocabulary for the same type of variable

**Bad:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**

```javascript
getUser();
```


### Use searchable names

We will read more code than we will ever write. It's important that the code we do write is readable and searchable. By _not_ naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable. Tools like [buddy.js](https://github.com/danielstjules/buddy.js) and [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) can help identify unnamed constants.

**Bad:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**Good:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86_400_000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```


### Use explanatory variables

**Bad:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

### Avoid Mental Mapping

Explicit is better than implicit.

**Bad:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

### Don't add unneeded context

If your class/object name tells you something, don't repeat that in your
variable name.

**Bad:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**Good:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

