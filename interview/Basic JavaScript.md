## JavaScript Introduction

JavaScript is the programming language of the web.
It can calculate, manipulate and validate data.
It can update and change both HTML and CSS.

```JS
// JavaScript Can Change HTML Content
document.getElementById('demo').innerHTML = "Hello JavaScript";
// JavaScript Can Change HTML Attribute Values
document.getElementById('myImage').src='pic_bulbon.gif';
// JavaScript Can Change HTML Styles (CSS)
document.getElementById("demo").style.fontSize = "35px";
document.getElementById("demo").style.display = "none";
document.getElementById("demo").style.display = "block";
```

### JavaScript Display Possibilities

```script
document.getElementById("demo").innerHTML = "<h2>Hello World</h2>";
document.getElementById("demo").innerText = "Hello World";
document.write(5 + 6);
window.alert(5 + 6);
console.log(5 + 6);
window.print();

```

## JavaScript Syntax

The let and const keywords create variables.

An identifier is the name you give to a variable.

Rules for identifiers:

* Must start with a letter, _, or $
* Can contain digits after the first character
* Cannot be a reserved keyword (let, const, if, etc.)
* Are case-sensitive

## JavaScript Variables

Declaring a Variable Using `let`.
Declaring a Variable Using `const`.

## JavaScript Operators

The `&&=` Operator. The Logical AND assignment operator is used between two values.
If the first value is true, the second value is assigned.

The `||=` Operator.  The Logical OR assignment operator is used between two values.
If the first value is false, the second value is assigned.

The `??=` Operator.  The Nullish coalescing assignment operator is used between two values.
If the first value is undefined or null, the second value is assigned.

The Spread (...) Operator. The `...` operator splits iterables into individual elements.

## JavaScript Conditionals

```js
if (condition1) {
  // code to execute if condition1 is true
} else if (condition2) {
  // code to execute if the condition1 is false and condition2 is true
} else {
  // code to execute if the condition1 is false and condition2 is false
}

switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
  default:
    // code block
}

condition ? expression1 : expression2;

let text = (age < 18) ? "Minor" : "Adult";

switch (new Date().getDay()) {
  case 0:
    day = "Sunday";
    break;
  case 1:
    day = "Monday";
    break;
  case 2:
     day = "Tuesday";
    break;
  case 3:
    day = "Wednesday";
    break;
  case 4:
    day = "Thursday";
    break;
  case 5:
    day = "Friday";
    break;
  case 6:
    day = "Saturday";
}

switch (new Date().getDay()) {
  case 6:
    text = "Today is Saturday";
    break;
  case 0:
    text = "Today is Sunday";
    break;
  default:
    text = "Looking forward to the Weekend";
}

switch (new Date().getDay()) {
  default:
    text = "Looking forward to the Weekend";
    break;
  case 6:
    text = "Today is Saturday";
    break;
  case 0:
    text = "Today is Sunday";
}

Boolean(10 > 9)

let x = Boolean(false);
let y = new Boolean(false);

(x == y) returns true
(y === y) returns false

```

## JavaScript Loops

```Javascript
for (let i = 0; i < cars.length; i++) {
  text += cars[i] + "<br>";
}
for (let i = 0; i < 5; i++) {
  text += "The number is " + i + "<br>";
}
while (i < 10) {
  text += "The number is " + i;
  i++;
}
do {
  text += "The number is " + i;
  i++;
}
while (i < 10);

const cars = ["BMW", "Volvo", "Saab", "Ford"];
let len = cars.length;

let text = "";
for (let i = 0; i < len; i++) {
  text += cars[i];
}

const cars = ["BMW", "Volvo", "Saab", "Ford"];
let i = 0;
let text = "";

while (cars[i]) {
  text += cars[i];
  i++;
}

for (let i = 0; i < 10; i++) {
  if (i === 3) { break; }
  text += "The number is " + i + "<br>";
}

let text = "";

loop1: for (let j = 1; j < 5; j++) {
  loop2: for (let i = 1; i < 5; i++) {
    if (i === 3) { break loop1; }
    text += i;
   }
}

const cars = ["BMW", "Volvo", "Saab", "Ford"];
list: {
  text += cars[0] + "<br>";
  text += cars[1] + "<br>";
  break list;
  text += cars[2] + "<br>";
  text += cars[3] + "<br>";
}

for (let i = 1; i < 10; i++) {
  if (i === 3) { continue; }
  text += "The number is " + i + "<br>";
}

let text = "";

loop1: for (let j = 1; j < 5; j++) {
  loop2: for (let i = 1; i < 5; i++) {
    if (i === 3) { continue loop1; }
    text += i;
   }
}

let text = "";

loop1: for (let j = 1; j < 5; j++) {
  loop2: for (let i = 1; i < 5; i++) {
    if (i === 3) { continue loop2; }
    text += i;
   }
}
```

## JavaScript Strings

```javascript
let text = `Hello World!`;
let text = `He's often called "Johnny"`;
let text =
`The quick
brown fox
jumps over
the lazy dog`;

let firstName = "John";
let lastName = "Doe";

let text = `Welcome ${firstName}, ${lastName}!`;

// Template Strings allow interpolation of expressions in strings:
let price = 10;
let VAT = 0.25;
let total = `Total: ${(price * (1 + VAT)).toFixed(2)}`;

let header = "Template Strings";
let tags = ["template strings", "javascript", "es6"];
let html = `<h2>${header}</h2><ul>`;
for (const x of tags) {
  html += `<li>${x}</li>`;
}
html += `</ul>`;

let text = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
let length = text.length;

let text = "HELLO WORLD";
let char = text.charAt(0);

// The method returns a UTF-16 code (an integer between 0 and 65535).
let text = "HELLO WORLD";
let char = text.charCodeAt(0);

const name = "W3Schools";
let letter = name.at(2);

const name = "W3Schools";
let letter = name[2];

let text1 = "Hello";
let text2 = "World";
let text3 = text1.concat(" ", text2);

text = "Hello" + " " + "World!";
text = "Hello".concat(" ", "World!");

// Slice out a portion of a string from position 7 to position 13:
let text = "Apple, Banana, Kiwi";
let part = text.slice(7, 13);

// If you omit the second parameter, the method will slice out the rest of the string:
let text = "Apple, Banana, Kiwi";
let part = text.slice(7);

// If a parameter is negative, the position is counted from the end of the string:
let text = "Apple, Banana, Kiwi";
let part = text.slice(-12);

// This example slices out a portion of a string from position -12 to position -6:
let text = "Apple, Banana, Kiwi";
let part = text.slice(-12, -6);

// The difference is that start and end values less than 0 are treated as 0 in substring().
let str = "Apple, Banana, Kiwi";
let part = str.substring(7, 13);

let text1 = "Hello World!";
let text2 = text1.toUpperCase();
let text2 = text1.toLowerCase();  // text2 is text1 converted to lower

let text = "Please visit Microsoft!";
let newText = text.replace("Microsoft", "W3Schools");

// To replace case insensitive, use a regular expression with an /i flag (insensitive):
let text = "Please visit Microsoft!";
let newText = text.replace(/MICROSOFT/i, "W3Schools");

// To replace all matches, use a regular expression with a /g flag (global match):
let text = "Please visit Microsoft and Microsoft!";
let newText = text.replace(/Microsoft/g, "W3Schools");

// The replaceAll() method allows you to specify a regular expression instead of a string to be replaced.
text = text.replaceAll("Cats","Dogs");
text = text.replaceAll("cats","dogs");
text = text.replaceAll(/Cats/g,"Dogs");
text = text.replaceAll(/cats/g,"dogs");

text.split(",")    // Split on commas
text.split(" ")    // Split on spaces
text.split("|")    // Split on pipe
text.split("")

// Both methods accept a second parameter as the starting position for the search:
let text = "Please locate where 'locate' occurs!";
let index = text.indexOf("locate");
let index = text.lastIndexOf("locate");
let index = text.lastIndexOf("John");
text.search("locate");
text.search(/locate/);
text.match("ain");
text.match(/ain/);
text.match(/ain/g);
text.match(/ain/gi);
const iterator = text.matchAll("Cats");
const iterator = text.matchAll(/Cats/g);
const iterator = text.matchAll(/Cats/gi);

// The includes() method returns true if a string contains a specified value. Otherwise it returns false.
let text = "Hello world, welcome to the universe.";
text.includes("world");
text.includes("world", 12);

let text = "Hello world, welcome to the universe.";
text.startsWith("Hello");
let text = "Hello world, welcome to the universe.";
text.startsWith("world", 5)

let text = "John Doe";
text.endsWith("Doe");

let text = "Hello world, welcome to the universe.";
text.endsWith("world", 11);

```

## JavaScript Numbers

```javascript
let x = 3.14;    // A number with decimals
let y = 3;       // A number without decimals
let x = 123e5;    // 12300000
let y = 123e-5;   // 0.00123
let x = 999999999999999;   // x will be 999999999999999
let y = 9999999999999999;  // y will be 10000000000000000
let x = 0.2 + 0.1;
let x = (0.2 * 10 + 0.1 * 10) / 10;

let x = 100 / "Apple";
isNaN(x);

// NaN is a JavaScript reserved word indicating that a number is not a legal number.
// Trying to do arithmetic with a non-numeric string will result in NaN (Not a Number).
// You can use the global JavaScript function isNaN() to find out if a value is a not a number.

// Infinity (or -Infinity) is the value JavaScript will return if you calculate a number outside the largest possible number.
let myNumber = 2;
// Execute until Infinity
while (myNumber != Infinity) {
  myNumber = myNumber * myNumber;
}

// Infinity is a number: typeof Infinity returns number.

let x = 0xFF;

// By default, JavaScript displays numbers as base 10 decimals. 
// But you can use the toString() method to output numbers from base 2 to base 36.
let myNumber = 32;
myNumber.toString(32);
myNumber.toString(16);
myNumber.toString(12);
myNumber.toString(10);
myNumber.toString(8);
myNumber.toString(2);

let x = 123;
let y = new Number(123);
let x = 123;
let y = new Number(123);

let x = 9007199254740995n;
let y = 9007199254740995n;
let z = x * y;

let a = 5n; // 0101
let b = 3n; // 0011
let x = (a & b); // 1n (0001)
let y = (a | b); // 7n (0111)
let z = (a ^ b); // 6n (0110)
let n = (~a);    // -6n

let big = 10n; // binary: 1010
let x = (big << 2n); // 40n (101000)
let y = (big >> 1n); // 5n (0101)
```

## JavaScript Functions

```javascript
let myFunction = function(a, b) {return a * b}
let myFunction = (a, b) => a * b;

function sum(...args) {
  let sum = 0;
  for (let arg of args) sum += arg;
  return sum;
}
let x = sum(4, 9, 16, 25, 29, 100, 66, 77);

x = findMax(1, 123, 500, 115, 44, 88);
function findMax() {
  let max = -Infinity;
  for (let i = 0; i < arguments.length; i++) {
    if (arguments[i] > max) {
      max = arguments[i];
    }
  }
  return max;
}

x = sumAll(1, 123, 500, 115, 44, 88);
function sumAll() {
  let sum = 0;
  for (let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}

const x = function (a, b) {return a * b};
let z = x(4, 3);

let hello = () => {
  return "Hello World!";
}

let hello = () => "Hello World!";

let hello = (val) => "Hello " + val;
let hello = val => "Hello " + val;

// Only this will work
let myFunction = (x, y) => { return x * y };
```

## JavaScript Objects

```JS
const car = {type:"Fiat", model:"500", color:"white"};

// Create an Object
const person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};

// Create an Object
const person = {};
// Add Properties
person.firstName = "John";
person.lastName = "Doe";
person.age = 50;
person.eyeColor = "blue";

// Create an Object
const person = new Object({
  firstName: "John",
  lastName: "Doe",
  age: 50,
  eyeColor: "blue"
});

person.lastName;
person["lastName"];

const person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};

// To create an object type we use an object constructor function.
function Person(first, last, age, eye) {
  this.firstName = first;
  this.lastName = last;
  this.age = age;
  this.eyeColor = eye;
}

const myFather = new Person("John", "Doe", 50, "blue");
const myMother = new Person("Sally", "Rally", 48, "green");
const mySister = new Person("Anna", "Rally", 18, "green");
const mySelf = new Person("Johnny", "Rally", 22, "green");

// The delete keyword deletes both the value of the property and the property itself.
// After deletion, the property cannot be used before it is added back again.

// Create an Object
const person = {
  name: "John",
  age: 30,
  city: "New York"
};

// Build a Text
let text = "";
for (let x in person) {
  text += person[x] + " ";
};

const fruits = {Bananas:300, Oranges:200, Apples:500};
let text = "";
for (let [fruit, value] of Object.entries(fruits)) {
  text += fruit + ": " + value + "<br>";
}

// Stringify Object
let text = JSON.stringify(person);
```

## JavaScript Scope

## JavaScript Date

## JavaScript Arrays





