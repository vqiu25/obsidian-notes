# Printing and Alerts

```js
console.log("Hello World");
window.alert("Hello World");
```

# HTML <-> JS/TS

Use `document.getElementById("id")` to select an HTML element:

```html
<p id="helloText">Hello</p>
```

```js
document.getElementById("helloText").textContent = "Updated Text";
```

```ts
const myP = document.getElementById("myP") as HTMLParagraphElement;
if (myP) {
    myP.textContent = "Updated Text";
}
```

# Retrieving & Setting Values in JS 🚀

🔹 **Retrieving** Values

| **Method**            | **Example**                                             |
| --------------------- | ------------------------------------------------------- |
| .value                | document.getElementById("myInput").value;               |
| .textContent          | document.getElementById("myDiv").textContent;           |
| .innerHTML            | document.getElementById("myDiv").innerHTML;             |
| .getAttribute("attr") | document.getElementById("myImage").getAttribute("src"); |


**🔹 Setting Values**

| **Method**                   | **Example**                                                             |
| ---------------------------- | ----------------------------------------------------------------------- |
| .value = "text"              | document.getElementById("myInput").value = "New Value";                 |
| .textContent = "text"        | document.getElementById("myDiv").textContent = "Updated Text";          |
| .innerHTML = "<b>text</b>"   | document.getElementById("myDiv").innerHTML = "<b>Bold Text</b>";        |
| .setAttribute("attr", value) | document.getElementById("myImage").setAttribute("src", "newImage.png"); |

* `.value`: is used for text fields, text areas, and other form elements. Works with `<input>, <textarea>, <select>`
* `.textContent`: Used to get or set only the text inside an element, ignoring any HTML tags. Works with `<h1>, <h2>, <p>, <div>, <span>`
* `.innerHTML`: Used to get or set the HTML content inside an element, allowing for HTML tags.
* `.setAttribute`: Used to add or modify an attribute of an element dynamically.

# JS/TS Fundamentals 🌐

## 📦 Variables

TypeScript supports **typed variables** using `let`, `const`, and `var` (though `var` is discouraged).

```typescript
let username: string = "John"; // String type
let age: number = 25; // Number type
let isActive: boolean = true; // Boolean type
```

* **~={purple}let=~** → Block-scoped, mutable. 
* **~={purple}const=~** → Block-scoped, immutable.
* **~={purple}var=~** → Function-scoped (not recommended).

## ➕ Arithmetic Operators

TypeScript supports basic arithmetic operations just like JavaScript.

```ts
let a: number = 10;
let b: number = 5;

console.log(a + b); // ➕ Addition (15)
console.log(a - b); // ➖ Subtraction (5)
console.log(a * b); // ✖ Multiplication (50)
console.log(a / b); // ➗ Division (2)
console.log(a % b); // 🔢 Modulus (0)
console.log(a ** b); // 💪 Exponentiation (10^5 = 100000)
```


## 🖊️ Handling User Input in TypeScript 

1️⃣ **HTML Form with Input and Button**

First, create an **HTML textbox** and a **button** for user input.

![[Pasted image 20250315154319.png | center | 200]]

```html
<body>
    <label for="myText">Enter Username:</label>
    <input type="text" id="myText" placeholder="Type here..." />
    <button id="mySubmit">Submit</button>
</body>
```

2️⃣ **JavaScript**

```js
document.getElementById("mySubmit").onclick = function() {
    let username = document.getElementById("myText").value;
    // Prints out the username to console
    console.log(username);
};
```

### 💱 Type Conversion

**~={purple}String to Number=~**

```ts
let strNum: string = "123";
let num: number = Number(strNum); // ✅ Safe conversion
```

**~={purple}Number to String=~**

```ts
let numValue: number = 50;
let strValue: string = numValue.toString();
```

### 🧮 Math object 

```js
console.log(Math.round(4.7));    // 5
console.log(Math.pow(2, 3));     // 8
console.log(Math.random());      // Random float (0-1)
console.log(Math.max(10, 20, 5)); // 20
console.log(Math.sin(Math.PI / 2)); // 1
```

# Control Flow & Conditional Statements 🚦

## 🤔 If Statements

```js
let age = 18;
if (age >= 18) {
    console.log("Meow");
} else {
    console.log("Woof");
}
```

## ✅ Checked Property

* **~={purple}Purpose=~**: Used to check if a **checkbox or radio button** is selected.

```js
let checkbox = document.getElementById("myCheckbox");

if (checkbox.checked) {
    console.log("Checkbox is checked.");
} else {
    console.log("Checkbox is not checked.");
}
```

## ❗ Logical Operators

| **Operator** | **Meaning** |
| ------------ | ----------- |
| **&&**       | AND         |
| \|\|         | OR          |
| **!**        | NOT         |

## 🟰 Strict Equality

* **~={purple}loose=~** → Converts types before comparing.
* **~={purple}strict=~** → No type conversion, **preferred** in JavaScript.

```js
console.log(5 == "5");  // true (loose equality)
console.log(5 === "5"); // false (strict equality)
```

# Loops & Iteration 🔁

## 🔄 While Loops

```js
let count = 0;

while (count < 3) {
    console.log("Count:", count);
    count++;
}
```

## 🔂 For Loops

```js
for (let i = 0; i < 3; i++) {
    console.log("Iteration:", i);
}
```

## ➿ forEach()

* Calls a function on each element

```js
let numbers = [1, 2, 3];

numbers.forEach(num => {
    console.log(num);
});
```

## 🗺 map()

* Creates a **new array** by applying a function on every element

```js
let numbers = [1, 2, 3];
let squared = numbers.map(num => num * num);

console.log(squared); // [1, 4, 9]
```

## 🚰 filter()

* Returns elements that match a condition.

```js
let numbers = [1, 2, 3, 4, 5];
let evenNumbers = numbers.filter(num => num % 2 === 0);

console.log(evenNumbers); // [2, 4]
```

## ♻ reduce()

* Used to combine all elements of an array into a single value by applying a function.

```js
let numbers = [1, 2, 3, 4, 5];
let sum = numbers.reduce((acc, num) => acc + num, 0);

console.log(sum); // 15
```

| **Step**              | **acc (accumulator)** | **num (current value)** | **Result** |
| --------------------- | --------------------- | ----------------------- | ---------- |
| Start (initial value) | 0                     | -                       | -          |
| 1st iteration         | 0 + 1                 | 1                       | 1          |
| 2nd iteration         | 1 + 2                 | 2                       | 3          |
| 3rd iteration         | 3 + 3                 | 3                       | 6          |
| 4th iteration         | 6 + 4                 | 4                       | 10         |
| 5th iteration         | 10 + 5                | 5                       | 15         |

# Functions & Scope 📞

## Functions 📞

```js
function isEven(num) {
	if (num % 2 == 0) return true;
	else return false;
}
```


• Function Expressions 🐣

• Arrow Functions 🎯

• Callbacks 🤙

# JavaScript Objects & Classes 🏭

• JavaScript Objects 🧍

• The this Keyword 👈

• Constructors 🛠

• Classes 🏭

• Static Keyword ⚡

• Inheritance 🐇

• Super Keyword 🦸‍♂️

• Getters & Setters 📐

• Destructuring 💥

• Nested Objects 📫

• Arrays of Objects 🍎

# Closures & Asynchronous JavaScript 🔒

• Closures 🔒

• setTimeout() ⏰

• Callback Hell 🔥

• Promises 🤞

• Async/Await ⏳
# Working with the DOM 🌳

• What is the DOM? 🌳

• Element Selectors 📑

• DOM Navigation 🧭

• Add & Change HTML 🛠️

• Mouse Events 🖱

• Key Events ⌨

• Hide/Show HTML 🖼

• NodeLists 📃

• classList 🧾
# Handling Data & APIs 📄

• JSON Files 📄

• Fetching Data from an API ↩️