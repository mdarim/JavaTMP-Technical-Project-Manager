Here are the **most important TypeScript features you should know for interviews**, along with **clear code examples** you can explain or run during a coding interview. These focus on what makes TypeScript powerful compared to plain JavaScript (static typing, interfaces, generics, enums, type guards, etc.). ([Zero To Mastery][1])

---

## ✅ **1. Static Typing (Basic Type Annotations)**

TypeScript adds *types* to JavaScript, catching errors at compile time (before running the code). ([DEV Community][2])

```ts
let username: string = "Alice";
let age: number = 30;
let isAdmin: boolean = false;

function greet(name: string): string {
  return `Hello, ${name}!`;
}

console.log(greet(username));
```

This prevents mistakes like assigning a number to a string variable.

---

## ✅ **2. Interfaces (Define Object Shapes)**

Interfaces enforce consistent structure for data — great for modeling objects and API responses. ([GitHub][3])

```ts
interface User {
  id: number;
  name: string;
  email?: string; // optional
}

function printUser(u: User) {
  console.log(`${u.id}: ${u.name}`);
}

printUser({ id: 1, name: "Alice" });
```

Optional fields (`?`) make some properties non-required.

---

## ✅ **3. Enums (Named Constants)**

Enums make managing sets of related constants easier and safer than raw strings or numbers. ([GitHub][3])

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}

let move: Direction = Direction.Up;
console.log(move); // 0
```

String enums are also possible for clearer values.

---

## ✅ **4. Generics (Reusable, Safe Functions & Types)**

Generics let you write functions and types that work across many types while staying type-safe. ([Zero To Mastery][1])

```ts
function identity<T>(value: T): T {
  return value;
}

let str = identity<string>("Hello");
let num = identity<number>(123);

console.log(str, num);
```

Generics help build reusable utilities without losing type safety.

---

## ✅ **5. Type Inference**

TypeScript often *infers* types automatically, reducing annotation noise. ([DEV Community][2])

```ts
let count = 10; // inferred as number
function add(a: number, b = 2) {
  return a + b; // return type inferred as number
}
```

Use annotations when inference isn’t clear or for public APIs.

---

## ✅ **6. Type Guards & Narrowing**

Type guards let TypeScript *narrow* a variable’s union type safely at runtime. ([howik.com][4])

```ts
function padLeft(value: string, padding: string | number) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + value;
  } else {
    return padding + value;
  }
}

console.log(padLeft("Text", 4));
```

This avoids unsafe property access on union types.

---

## ✅ **7. Union & Intersection Types**

Unions allow a variable to be *one of several types*; intersections combine types. ([DEV Community][2])

```ts
type ID = number | string;

function printId(id: ID) {
  console.log("ID:", id);
}

type Admin = {
  role: string;
};

type Employee = {
  name: string;
};

type AdminEmployee = Admin & Employee;

const ae: AdminEmployee = { role: "Manager", name: "Bob" };
```

Unions + intersections help build flexible type shapes.

---

## ✅ **8. Modules (Import/Export)**

Modules organize code into reusable pieces — essential for real apps. ([Zero To Mastery][1])

```ts
// math.ts
export function add(x: number, y: number): number {
  return x + y;
}

// app.ts
import { add } from "./math";
console.log(add(4, 5));
```

Modules help separate concerns and improve maintainability.

---

## ✅ **9. Nullable Types & Strict Null Checks**

With strict null checks, TypeScript catches issues with `null` or `undefined`. ([Zero To Mastery][1])

```ts
let x: number | null = null;

function printLength(s: string | null) {
  if (s !== null) {
    console.log(s.length);
  }
}
```

Union types clarify whether a value *can* be null/undefined.

---

## ✅ **10. Async/Await with Promise Types**

TypeScript supports modern asynchronous patterns with typed promises. ([howik.com][4])

```ts
async function fetchData(): Promise<string> {
  let promise = new Promise<string>((resolve) =>
    setTimeout(() => resolve("Fetched!"), 1000)
  );
  let result = await promise;
  return result;
}

fetchData().then(console.log);
```

Types help catch errors in async code early.

---

## 🧠 **Quick Interview Summary**

| Feature       | Key Benefit                          |
|---------------|--------------------------------------|
| Static Typing | Catches bugs at compile time         |
| Interfaces    | Define consistent data shapes        |
| Generics      | Create reusable, type-safe utilities |
| Enums         | Named constant sets                  |
| Type Guards   | Safe narrowing of union types        |
| Modules       | Scalable code organization           |
| Async/Await   | Typed asynchronous code              |

TypeScript’s type system and tooling (better autocompletion, refactoring support) make large codebases safer and easier to maintain than plain JavaScript. ([DEV Community][2])

---

If you want, I can provide a **set of hands-on interview tasks** with expected outputs that use these features!

[1]: https://zerotomastery.io/blog/typescript-interview-questions/?utm_source=chatgpt.com "TypeScript Interview Questions + Answers (With Code Examples) | Zero To Mastery"
[2]: https://dev.to/jessie_chen/interview-essentials-8-common-typescript-interview-questions-4jki?utm_source=chatgpt.com "〖Interview Essentials〗8 Common TypeScript Interview Questions - DEV Community"
[3]: https://github.com/Devinterview-io/typescript-interview-questions?utm_source=chatgpt.com "GitHub - Devinterview-io/typescript-interview-questions: 🟣 Typescript interview questions and answers to help you prepare for your next technical interview in 2025."
[4]: https://howik.com/typescript-interview-questions?utm_source=chatgpt.com "Mastering TypeScript Interview Questions: A 2025 Guide - Howik"

Here are **hands-on TypeScript interview coding tasks** — with **problem descriptions, code you can write, and expected outputs** — that are great for practice before a live or online interview. These tasks focus on key TypeScript features like **types, generics, utility types, classes, and real practical logic**. Many are commonly asked or helpful for interview prep. ([LinkedIn][1])

---

## 🧪 **1) Fix a Logical Error in a Typed Function (Basic)**

**Task:** The following function is meant to calculate a factorial, but it returns an incorrect result because of a wrong loop condition. Fix it.

```ts
function factorial(n: number): number {
  let result = 1;
  for (let i = n; i >= 0; i--) {
    result *= i;
  }
  return result;
}

console.log(factorial(5));
```

✅ **Issue:** The loop runs down to `0`, making result `0` every time.
✅ **Fix:**

```ts
function factorial(n: number): number {
  let result = 1;
  for (let i = n; i > 0; i--) {
    result *= i;
  }
  return result;
}

console.log(factorial(5)); // Expected: 120
```

✔ This tests understanding of **types + logic flow** in TypeScript. ([CoderPad][2])

---

## 🧠 **2) Implement a Generic Queue Class (Medium)**

**Task:** Write a **generic Queue<T>** class with methods to enqueue, dequeue, and get size — supporting any type.

```ts
class Queue<T> {
  private storage: T[] = [];

  enqueue(item: T): void {
    this.storage.push(item);
  }

  dequeue(): T | undefined {
    return this.storage.shift();
  }

  get size(): number {
    return this.storage.length;
  }
}

// Example usage
let numberQueue = new Queue<number>();
numberQueue.enqueue(1);
numberQueue.enqueue(2);
console.log(numberQueue.size);   // Expected: 2
console.log(numberQueue.dequeue()); // Expected: 1
console.log(numberQueue.size);   // Expected: 1
```

✔ This covers **generics, classes, and type safety** — a classic interview task. ([hackerrank.com][3])

---

## 📦 **3) Build Custom Utility Types (Advanced)**

**Task:** Recreate some built-in TypeScript utility types *from scratch* — a frequent interview challenge. ([LinkedIn][1])

### 💡 a) Partial<T> — make all properties optional

```ts
type MyPartial<T> = { [P in keyof T]?: T[P] };

interface User {
  id: number;
  name: string;
}

const u1: MyPartial<User> = { name: "Alice" };
```

✅ Expected: works like built-in `Partial<T>` — all keys optional.

---

### 💡 b) Required<T> — make all properties required

```ts
type MyRequired<T> = { [P in keyof T]-?: T[P] };

let user: MyRequired<User> = { id: 1, name: "Bob" };
```

✔ Should behave like built-in `Required<T>`.

---

### 💡 c) Pick<T, K> — pick a subset of properties

```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};

type NameOnly = MyPick<User, "name">; 
// expected: { name: string }
```

✔ Demonstrates mastery of **mapped types and constraints**. ([LinkedIn][1])

---

## 🧩 **4) Parse a Query String into an Object (Practical)**

**Task:** Write a function that takes a URL query string and returns an object with parsed key/value pairs.

```ts
// Input: "name=John&age=30&city=NewYork"
function parseQueryString(query: string): { [key: string]: string } {
  const result: { [key: string]: string } = {};
  query.split("&").forEach(pair => {
    const [key, value] = pair.split("=");
    result[key] = value;
  });
  return result;
}

console.log(parseQueryString("name=John&age=30&city=NewYork"));
// Expected: { name: "John", age: "30", city: "NewYork" }
```

✔ This tests **string manipulation + typing for dynamic objects** in TS. ([Placement Preparation][4])

---

## 🔀 **5) Merge Two Sorted Arrays (Array Logic)**

**Task:** Merge two sorted arrays into one sorted array **without duplicates**.

```ts
function mergeSortedArrays(arr1: number[], arr2: number[]): number[] {
  const result: number[] = [];
  let i = 0, j = 0;

  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) {
      if (!result.includes(arr1[i])) result.push(arr1[i]);
      i++;
    } else {
      if (!result.includes(arr2[j])) result.push(arr2[j]);
      j++;
    }
  }
  return [...result, ...arr1.slice(i), ...arr2.slice(j)].filter((v, idx, self) => self.indexOf(v) === idx);
}

console.log(mergeSortedArrays([1,2,4],[2,3,4,6]));
// Expected: [1,2,3,4,6]
```

✔ Great for interviews that combine **logic + types + array handling**. ([Placement Preparation][4])

---

## 🧠 **6) Type-Constrained Generic Function**

**Task:** Write a generic function constrained to objects with an `id` property. Return the same object.

```ts
interface HasId { id: string }

function logId<T extends HasId>(obj: T): T {
  console.log(obj.id);
  return obj;
}

const item = logId({ id: "123", name: "Book" });
console.log(item.name); // Expected: "Book"
```

✔ Shows understanding of **generic constraints** and safe type inference. ([credmark][5])

---

## 🧪 **7) Function Overloading (Conditional Behavior)**

**Task:** Create a function that behaves differently based on type of argument — classic interview prompt. ([Interview Zen][6])

```ts
function process(input: string): string;
function process(input: string[]): number;
function process(input: any): any {
  if (typeof input === "string") {
    return input.toUpperCase();
  } else {
    return input.length;
  }
}

console.log(process("TypeScript"));                   // Expected: "TYPESCRIPT"
console.log(process(["T", "S", "JS"]));               // Expected: 3
```

✔ Demonstrates **function overloading + type narrowing**.

---

## 📌 **Interview Tips for These Tasks**

✔ Always **explain your types** as part of your answer — interviewers want clarity on why you chose them.
✔ Check edge cases (empty arrays, empty query strings).
✔ Explain **type safety benefits** while writing code — a key TypeScript interview point. ([geeksforgeeks.org][7])

---

If you want, I can also provide **unit tests (Jest)** for these tasks so you can run and verify behavior automatically!

[1]: https://www.linkedin.com/pulse/typescript-interview-tasks-implementing-built-in-types-ruchevskii-0wxrf?utm_source=chatgpt.com "TypeScript Interview Tasks: Implementing Built-in Utility Types"
[2]: https://coderpad.io/interview-questions/typescript-interview-questions/?utm_source=chatgpt.com "25+ TypeScript Interview Questions For Tech Interviews - CoderPad"
[3]: https://www.hackerrank.com/blog/typescript-interview-questions-developers-should-know/?utm_source=chatgpt.com "TypeScript Interview Questions to Know - HackerRank Blog"
[4]: https://www.placementpreparation.io/programming-exercises/typescript/advanced/?utm_source=chatgpt.com "Top 30 TypeScript Practice Exercises for Advanced with Solutions"
[5]: https://credmark.ai/practice/top-typescript-programming-interview-questions-and-answers?utm_source=chatgpt.com "TypeScript Programming interview questions and answers - credmark"
[6]: https://www.interviewzen.com/blog/top-7-typescript-interview-questions/?utm_source=chatgpt.com "7 TypeScript Interview Questions Every Hiring Manager Should Ask - Interview Zen"
[7]: https://www.geeksforgeeks.org/typescript-interview-questions/?utm_source=chatgpt.com "TypeScript Interview Questions and Answers - GeeksforGeeks"

