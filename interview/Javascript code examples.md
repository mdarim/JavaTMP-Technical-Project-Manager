Here are **code examples for the most important JavaScript features you should know for interviews**, with explanations and runnable examples. These cover core language features (including ES6+), asynchronous behavior, closures, scope, and more — all frequently asked in interviews. ([Codinginterview.com][1])

---

## ✅ 1) **Variable Declarations (`var`, `let`, `const`)**

Modern JavaScript uses `let` and `const` for block-scoped variables. ([إيجبلي][2])

```js
// var — function scope
function testVar() {
  var x = 1;
  if (true) {
    var x = 2; // same variable!
    console.log(x); // 2
  }
  console.log(x); // 2
}

// let — block scope
function testLet() {
  let y = 1;
  if (true) {
    let y = 2; // different variable
    console.log(y); // 2
  }
  console.log(y); // 1
}

testVar();
testLet();

// const — block scope + no reassignment
const pi = 3.14;
// pi = 3.15; // Error
```

---

## ✅ 2) **Template Literals (ES6)**

Use backticks `` ` `` for easier string interpolation and multi-line strings. ([Codinginterview.com][1])

```js
const name = "Alice";
const age = 25;
console.log(`Name: ${name}, Age: ${age}`); 
// Name: Alice, Age: 25
```

---

## ✅ 3) **Destructuring (ES6)**

Extract values from arrays/objects easily. ([Foundit][3])

```js
// Array destructuring
const [a, b] = [10, 20];
console.log(a, b); // 10 20

// Object destructuring
const user = { name: "Bob", city: "NY" };
const { name, city } = user;
console.log(name, city); // Bob NY
```

---

## ✅ 4) **Spread & Rest Operators**

Spread expands, rest gathers arguments. ([Codinginterview.com][1])

```js
// Spread (arrays)
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];
console.log(arr2); // [1,2,3,4]

// Rest (function params)
function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
console.log(sum(1,2,3)); // 6
```

---

## ✅ 5) **Arrow Functions & Lexical `this`**

Shorter function syntax; doesn’t re-bind `this`. ([Skilr][4])

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2,4,6]

// Arrow has lexical this
const obj = {
  value: 10,
  show: function() {
    setTimeout(() => {
      console.log(this.value); // 10
    }, 100);
  }
};
obj.show();
```

---

## ✅ 6) **Closures**

A closure *remembers* variables from its containing scope even after the outer function runs. ([DEV Community][5])

```js
function makeCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

---

## ✅ 7) **Immediately Invoked Function Expression (IIFE)**

Creates a new scope immediately — useful for encapsulation. ([ويكيبيديا][6])

```js
(function () {
  let msg = "IIFE running!";
  console.log(msg);
})();
```

---

## ✅ 8) **Promises & Async/Await**

Essential for asynchronous code. ([إيجبلي][2])

```js
// Using Promise
fetch("https://jsonplaceholder.typicode.com/todos/1")
  .then(res => res.json())
  .then(data => console.log("Promise:", data));

// Using async/await
async function getTodo() {
  const res = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  const data = await res.json();
  console.log("Async/Await:", data);
}
getTodo();
```

---

## ✅ 9) **Event Loop & setTimeout Example**

Demonstrates JavaScript’s asynchronous behavior. ([Zero To Mastery][7])

```js
console.log("Start");

setTimeout(() => {
  console.log("Inside Timeout");
}, 0);

console.log("End");

// Console:
// Start
// End
// Inside Timeout
```

---

## ✅ 10) **Array Methods: `map`, `filter`, `reduce`**

Common functional operations in JavaScript. ([DEV Community][8])

```js
const nums = [1, 2, 3, 4];

// map
console.log(nums.map(n => n * 2)); // [2,4,6,8]

// filter
console.log(nums.filter(n => n % 2 === 0)); // [2,4]

// reduce
console.log(nums.reduce((sum, n) => sum + n, 0)); // 10
```

---

## ✅ 11) **`this` Keyword Rules**

Understanding how `this` behaves in different contexts (object methods vs standalone functions) is critical. ([Skilr][4])

```js
const person = {
  name: "Sam",
  greet: function () {
    console.log(this.name);
  }
};

person.greet(); // Sam

const fn = person.greet;
fn(); // undefined (in strict mode)
```

---

## 🧠 Tips for Interviews

📍 Explain **why** a feature is useful (e.g., closures help encapsulate state).
📍 Show differences between older and modern patterns (like ES5 vs ES6+).
📍 Use real code you can run or paste into the console.

---

If you want, I can also give you a **set of interview exercises with expected outputs** based on these features — perfect for hands-on practice. Just ask!

[1]: https://www.codinginterview.com/guide/javascript-coding-interview-questions-and-answers/?utm_source=chatgpt.com "JavaScript Coding Interview Questions​ and Answers (2025)"
[2]: https://www.ejable.com/tech-corner/tech-interviews/javascript-interview-questions/?utm_source=chatgpt.com "JavaScript Interview Questions | EJable"
[3]: https://www.monster.co.th/career-advice/javascript-es6-features-for-interviews/?utm_source=chatgpt.com "JavaScript ES6+ Features You Must Know for Interviews in 2025"
[4]: https://www.skilr.com/blog/top-50-javascript-interview-questions-and-answers/?utm_source=chatgpt.com "Top 50 Javascript Interview Questions and Answers - Skilr Blog"
[5]: https://dev.to/jagroop2001/20-important-javascript-concepts-for-your-next-interview-2000?utm_source=chatgpt.com "20 Important JavaScript Concepts for Your Next Interview 🚀 - DEV Community"
[6]: https://en.wikipedia.org/wiki/Immediately_invoked_function_expression?utm_source=chatgpt.com "Immediately invoked function expression"
[7]: https://zerotomastery.io/blog/frontend-javascript-interview-questions/?utm_source=chatgpt.com "Top 30 JavaScript Interview Questions + Answers (with Code Snippets) | Zero To Mastery"
[8]: https://dev.to/frontendengineer/top-10-javascript-interview-questions-and-answers-you-should-know-junior-and-senior-3943?utm_source=chatgpt.com "JavaScript Interview Questions and Answers you should know - Junior and Senior - DEV Community"


Here are **hands-on JavaScript practice problems with code examples and expected outputs** — perfect for interview preparation. These cover common interview patterns like string/array manipulation, logic challenges, and real coding tasks you might be asked to solve live. ([Medium][1])

---

## 🧪 **1) Reverse a String**

**Problem:** Write a function to reverse a string.

```js
function reverseString(str) {
  return str.split('').reverse().join('');
}

console.log(reverseString("Hello")); // Output: "olleH"
```

✔ Useful for practicing string/array manipulation. ([DEV Community][2])

---

## 🧠 **2) Check for Palindrome**

**Problem:** Check if a string is a palindrome (reads same backward).

```js
function isPalindrome(str) {
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  return cleaned === cleaned.split('').reverse().join('');
}

console.log(isPalindrome("121")); // Output: true
console.log(isPalindrome("hello")); // Output: false
```

✔ Tests string processing and logic. ([DEV Community][2])

---

## 🧮 **3) Fibonacci Sequence**

**Problem:** Generate first `n` Fibonacci numbers.

```js
function fibonacci(n) {
  const result = [0, 1];
  for (let i = 2; i < n; i++) {
    result.push(result[i - 1] + result[i - 2]);
  }
  return result.slice(0, n);
}

console.log(fibonacci(6)); // Output: [0, 1, 1, 2, 3, 5]
```

✔ Classic sequence challenge. ([DEV Community][2])

---

## 🧮 **4) Factorial**

**Problem:** Find factorial of a number.

```js
function factorial(n) {
  if (n === 0) return 1;
  let result = 1;
  for (let i = 1; i <= n; i++) result *= i;
  return result;
}

console.log(factorial(5)); // Output: 120
```

✔ Tests loops and basic math logic. ([DEV Community][2])

---

## 🔎 **5) Remove Falsy Values from Array**

**Problem:** Filter out falsy values (false, 0, "", null, undefined, NaN).

```js
function removeFalsy(arr) {
  return arr.filter(Boolean);
}

console.log(removeFalsy([0, 5, false, 6, '', 7])); // Output: [5, 6, 7]
```

✔ Uses built-in filter with truthiness. ([GeeksforGeeks][3])

---

## 🔢 **6) Find Maximum Number in Array**

**Problem:** Get the largest element in an array.

```js
function maxNumber(arr) {
  return Math.max(...arr);
}

console.log(maxNumber([10, 5, 100, 20])); // Output: 100
```

✔ Shows use of spread operator for simplicity. ([DevWhirl][4])

---

## 🧩 **7) Remove Duplicates in Array**

**Problem:** Return only unique values.

```js
function removeDuplicates(arr) {
  return [...new Set(arr)];
}

console.log(removeDuplicates([1, 2, 3, 2, 4, 5, 3]));
// Output: [1, 2, 3, 4, 5]
```

✔ Uses `Set` and spread. ([DevWhirl][4])

---

## 🔄 **8) Merge Two Sorted Arrays**

**Problem:** Merge two sorted arrays into a single sorted array without duplicates.

```js
function mergeSortedArrays(a, b) {
  let i = 0, j = 0, result = [];
  while (i < a.length && j < b.length) {
    if (a[i] < b[j]) {
      if (!result.includes(a[i])) result.push(a[i]);
      i++;
    } else {
      if (!result.includes(b[j])) result.push(b[j]);
      j++;
    }
  }
  return result
    .concat(a.slice(i), b.slice(j))
    .filter((v, idx, self) => self.indexOf(v) === idx);
}

console.log(mergeSortedArrays([1,2,4],[2,3,4,6]));
// Output: [1,2,3,4,6]
```

✔ Good for two-pointer technique. ([Medium][1])

---

## 📊 **9) Count Vowels and Consonants**

**Problem:** Count vowels & consonants in a string.

```js
function countLetters(str) {
  const vowels = str.match(/[aeiou]/gi) || [];
  const consonants = str.match(/[bcdfghjklmnpqrstvwxyz]/gi) || [];
  return {
    vowels: vowels.length,
    consonants: consonants.length
  };
}

console.log(countLetters("Ganesh205"));
// Output: { vowels: 2, consonants: 4 }
```

✔ Regex + logic practice. ([DEV Community][2])

---

## 🧪 **10) Find Common Elements Between Arrays**

**Problem:** Return common elements (including duplicates).

```js
function commonElements(a, b) {
  const res = [];
  let map = {};
  a.forEach(x => map[x] = (map[x] || 0) + 1);
  b.forEach(x => {
    if (map[x] > 0) {
      res.push(x);
      map[x]--;
    }
  });
  return res;
}

console.log(commonElements([1, 2, 6, 2],[4,1,2,2]));
// Output: [2, 2]
```

✔ Object map approach for duplicates. ([Medium][1])

---

## 📌 **Bonus: Output Prediction Challenge**

**Problem:** What’s the output?

```js
let x = { a: 1 };
let y = Object.assign({}, x);
console.log(x === y);
```

📌 **Output:** `false`
Because `Object.assign()` creates a *new object*, so `x` and `y` don’t reference the same object. ([GeeksforGeeks][5])

---

## ✨ **Tips for Coding Interviews**

✅ **Explain your approach** before writing code.
✅ **Run test cases by hand** to verify logic.
✅ Use **clear variable names** and comment tricky sections.
✅ Practice **edge cases** like empty inputs or unusual values. ([rustcodeweb.com][6])

---

Would you like a **printable PDF** or **interactive runnable code sandbox** for these problems? (I can generate code you can copy/paste into tools like CodePen or JSFiddle.)

[1]: https://riya-rj-jain.medium.com/javascript-interview-coding-questions-2355b36f7682?utm_source=chatgpt.com "JavaScript Interview Coding Questions | by Riya Jain | Medium"
[2]: https://dev.to/koolkamalkishor/40-essential-javascript-coding-challenges-for-interviews-and-practice-4d17?utm_source=chatgpt.com "40+ Essential JavaScript Coding Challenges for Interviews and Practice - DEV Community"
[3]: https://www.geeksforgeeks.org/javascript-coding-questions-and-answers/?utm_source=chatgpt.com "JavaScript Coding Questions and Answers - GeeksforGeeks"
[4]: https://devwhirl.com/blog/javascript-interview-questions?utm_source=chatgpt.com "JavaScript Interview Questions and Answers - [2025]"
[5]: https://www.geeksforgeeks.org/javascript-output-based-interview-questions/?utm_source=chatgpt.com "JavaScript Output Based Interview Questions - GeeksforGeeks"
[6]: https://www.rustcodeweb.com/2025/02/practical-javascript-interview-questions-with-solutions-and-code-examples.html?utm_source=chatgpt.com "Practical JavaScript Interview Questions with Solutions and Code Examples | RUSTCODE"
