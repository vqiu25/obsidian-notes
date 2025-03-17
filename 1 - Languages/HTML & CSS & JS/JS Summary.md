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

# Retrieving & Setting Values of HTML Elements in JS 🚀

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

* Calls a function on each element using a closure on each element. Does not persist in the original array.

```js
let numbers = [1, 2, 3];

function logNumber(num) {
    console.log(num);
}

numbers.forEach(logNumber);
```

## 🗺 map()

* Creates a **new array** by applying a function on every element

```js
let numbers = [1, 2, 3];

function square(num) {
    return num * num;
}

let squared = numbers.map(square);

console.log(squared); // [1, 4, 9]
```

## 🚰 filter()

* Returns elements that match a condition.

```js
let numbers = [1, 2, 3, 4, 5];

function isEven(num) {
    return num % 2 === 0;
}

let evenNumbers = numbers.filter(isEven);

console.log(evenNumbers); // [2, 4]
```

## ♻ reduce()

* Used to combine all elements of an array into a single value by applying a function.
* **~={red}Example=~**: reduce() starts with 0 as the accumulator, adds 1 to it (returning 1), then adds 2 to 1 (returning 3), then 3 to 3 (returning 6), continuing until all elements are combined into a single value.

```js
let numbers = [1, 2, 3, 4, 5];

function add(acc, num) {
    return acc + num;
}

let sum = numbers.reduce(add, 0);

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

## 📖 Spread operator & 🗄 Rest parameters 

**~={purple}Spread Operator=~**
* The **spread operator (...)** is used to **expand** arrays, objects, or function arguments.

```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

let mergedArr = [...arr1, ...arr2];
let notMergedArr= [arr1, arr2];

console.log(mergedArr); // [1, 2, 3, 4, 5, 6]
console.log(notMergedArr); // [[1, 2, 3], [4, 5, 6]]
```

**~={purple}Rest Parameters=~**
* The **rest parameter (...)** allows functions to **accept multiple arguments** as an array.

```js
// This is because ... converts (1, 2, 3, 4) into [1, 2, 3, 4]
function sum(...numbers) {
    function add(acc, num) {
        return acc + num;
    }
    
    return numbers.reduce(add, 0);
}

console.log(sum(1, 2, 3, 4)); // Output: 10
```

# Functions & Scope 📞

## Functions 📞

```js
function isEven(num) {
	if (num % 2 == 0) return true;
	else return false;
}
```

## Callbacks 🤙

* A function that is passed as an argument to another function. Typically used for database operations, network operations and asynchronous tasks. Callbacks ensure that code executes only after a specific task is completed, such as fetching data from a database or reading a file.

If `hello()` takes super long to run, `goodbye()` maybe print first.

```js
hello();
goodbye();
```

Thus, to resolve this, we pass the `goodbye()` function to the `hello()` function, and have `hello()` call `goodbye()` when it's finished

```js
hello(goodbye);

function hello(callback) {
	console.log("Hello");
	callback();
}

function goodbye() {
	console.log("Bye!");
}
```

## Function Expressions 🐣

Allows us to define functions as variables. `Variable = Function Expression`

```js
const hello = function() {
	console.log("Hello");
}
```

Where we can invoke the function by:

```js
hello();
```

---

We can also replace functions that take closures as input, with a function expression:

```js
const numbers = [1, 2, 3, 4, 5, 6];
const squares = numbers.map(square);

console.log(squares);

function square(element) {
    return Math.pow(element, 2);
}
```

Where we can replace `square` with the function itself (note that we did optionally remove the name from the function):

```js
const numbers = [1, 2, 3, 4, 5, 6];
const squares = numbers.map(function (element) {
    return Math.pow(element, 2);
});

console.log(squares);
```

---

A function expression can also return a function.

```js
var createCounter = function(n) {
    let currentCount = n - 1;
    return function() {
        currentCount++;
        return currentCount;
    };
};

const counter = createCounter(5); // Initializes currentCount to 4
console.log(counter()); // Outputs 5 (increments from 4)
console.log(counter()); // Outputs 6 (increments from 5)
console.log(counter()); // Outputs 7 (increments from 6)
```

* ~={purple}**Initialisation runs once**=~: currentCount = n - 1 is set when createCounter(n) is called.
* **~={purple}Returns a function=~**: This function (a closure) retains access to currentCount.
* **~={purple}Subsequent calls=~**: Only execute the returned function, modifying and returning currentCount.
* **~={purple}State persists=~**: currentCount is not reset; it updates with each call.

## Arrow Functions 🎯

* A concise way to write function expressions good for simple functions that you use only once 
* **~={red}Template Example=~**: (parameters) => some code

```js
const hello = function() {
	console.log("Hello");
}

// Can be changed to 

const hello = () => { 
	console.log("Hello");
}

// Or

const hello = () => console.log("Hello");

```

**~={red}Example=~**:

```js
const numbers = [1, 2, 3, 4, 5, 6];
const squares = numbers.map(function (element) {
    return Math.pow(element, 2);
});

console.log(squares);
```

We can remove the word `function` and put an => after the `(parmaeter)`, and remove the word `return`:

```js
const numbers = [1, 2, 3, 4, 5, 6];
const squares = numbers.map((element) => {
    Math.pow(element, 2);
});

console.log(squares);
```

# JavaScript Objects & Classes 🏭

## JavaScript Objects 🧍

* A collection of related properties and/or methods
* `object = { key: value, key: function() }`
* `this` is compulsory to access an object's variables within the object

```js
const person = {
    name: "Alice",
    age: 25,
    greet: function() {
        return `Hello, I'm ${this.name}`;
    }
};

console.log(person.name); // Access property → "Alice"
console.log(person.greet()); // Call method → "Hello, I'm Alice"
```

## Constructors 🛠

* Is a function that creates an object. Used to create multiple objects with the same structure.

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        return `Hello, I'm ${this.name}`;
    };
}

const alice = new Person("Alice", 25);
console.log(alice.name); // "Alice"
console.log(alice.greet()); // "Hello, I'm Alice"
```

## Classes 🏭

* Provides a more structured and cleaner way to work with objects compared to traditional constructor functions i.e. static keyword, encapsulation, inheritance.
* We instantiate a class object using the `new` keyword

```js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        return `Hello, I'm ${this.name}`;
    }
}

const alice = new Person("Alice", 25);
console.log(alice.greet()); // "Hello, I'm Alice"
```

## Static Keyword ⚡

* `static ` keyword  defines properties or methods that belong to a class itself rather than the objects created from that class (class owns anything static, not the objects)
	*  Call with `ClassName.property` or `ClassName.method()`.
	* Instances **cannot** access static members.

```js
class MathUtil {
    static PI = 3.14159;

    static add(a, b) {
        return a + b;
    }
}

console.log(MathUtil.PI); // 3.14159
console.log(MathUtil.add(5, 3)); // 8
```

## Inheritance 🐇

* Allows a new class to inherit properties and methods from an existing class (parent -› child) which can help with code reusability
* We achieve this using `extends`

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        return `${this.name} makes a noise.`;
    }
}

class Dog extends Animal {
	fetch () {
		return `Fetch ball.`;
	}
	
	// We can override parent functions by simply defining the function
	// here again
}

const dog = new Dog("Buddy");
console.log(dog.speak()); // "Buddy makes a noise."
```

## Super Keyword 🦸‍♂️

* `super` keyword is used in classes to call the constructor or access the properties and methods of a parent (superclass)
* `super(args)` **forwards arguments** to the parent constructor. We do this because `super()` must be called first in a subclass before using `this`

```js
class Animal {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

class Rabbit extends Animal {
    constructor(name, age, runSpeed) {
        super(name, age);
        this.runSpeed = runSpeed;
    }
}

class Fish extends Animal {
    constructor(name, age, swimSpeed) {
        super(name, age); 
        this.swimSpeed = swimSpeed;
    }
}
```

We can also run methods from the super class in a child class:

```js
class Rabbit extends Animal {
    constructor(name, age, runSpeed) {
        super(name, age);
        this.runSpeed = runSpeed;
    }

	jump() {
		super.parentMethod();
	}
}
```

## Getters & Setters 📐

```js
class Person {
    #name;  // Private field
    #age;

    constructor(name, age) {
        this.#name = name;
        this.#age = age;
    }

    // Getter for name
    getName() {
        return this.#name;
    }

    // Setter for name
    setName(newName) {
        if (typeof newName === 'string' && newName.trim() !== '') {
            this.#name = newName;
        } else {
            console.error("Invalid name!");
        }
    }
}

const p = new Person("Alice", 25);

console.log(p.getName()); // ✅ Alice

p.setName("Bob"); 
console.log(p.getName()); // ✅ Bob

p.setName("");  // ❌ Invalid name!
console.log(p.getName()); // ✅ Bob (remains unchanged)
```

## Destructuring 💥

### 1️⃣ Array Destructuring 📦

Extract values from an array into variables.

```js
const numbers = [1, 2, 3];
const [a, b, c] = numbers;
  

console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

Supports default values

```js
const [x, y, z = 10] = [5, 6]; 
console.log(z); // 10
```

Skipping elements

```js
const [first] = [10, 20, 30]; 
const [, , third] = [10, 20, 30]; 
const [, second] = [10, 20, 30]; 
console.log(second); // 20
```

### 2️⃣ Object Destructuring 🏗️

Extract properties from an object into variables. Also supports default values and skipping elements

```js
const person = { name: "Alice", age: 25 };
const { name, age } = person;

console.log(name); // "Alice"
console.log(age);  // 25
```

### **3️⃣** Rest and Spread with Destructuring 🌟

Rest (...) collects remaining elements into an array or object. It must be placed at the **~={red}end=~**.

```js
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); //
console.log(rest); // [2, 3, 4]
```

Spread (...) expands an array or object into individual elements. If a property appears multiple times, the last occurrence wins.

```js
const person = { name: "Alice", age: 25 };
const person2 = { ...person, age: 30 };
console.log(person2); // { name: "Alice", age: 30 }
```

### 4️⃣ Function Parameter Destructuring 🎯

In essence, allows us to pass in an object to a function, and have the method signature break the object down

```js
function displayPerson({ firstName, age, job = "Unknown" }) {
    console.log(`Name: ${firstName}`);
    console.log(`Age: ${age}`);
    console.log(`Job: ${job}`);
}

const person = { 
	firstName: "SpongeBob", 
	lastName: "SquarePants", 
	age: 30 
};

displayPerson(person);
```

## Nested Objects 📫

Allows for objects in objects.

```js
const person = {
    name: "Alice",
    address: {
        city: "New York",
        zip: "10001"
    }
};
console.log(person.address.city); // "New York"
```

# ES6 Module and Error Handling

## ES6 Modules 🚢

An external file that contains reusable code that can be imported into other JavaScript files Can contain variables, classes, functions ... and more!

In our HTML file, we need to specify the type of our "main" JS file to be of type `"module`, to support ES6 modueles:

```html
<script type="module" src="script.js"></script>
```

Let's say we had `Person.js`:
* First let's add `export` to the front of them

```js
export class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        return `Hello, I'm ${this.name}`;
    }
}
```

To use this class in `script.js`:

```js
import { Person } from './Person.js';

const user = new Person("Alice", 25);
console.log(user.greet()); // "Hello, I'm Alice"
```

## Error Handling ⚠

* An `Error` is an Object that is created to represent a problem that occurs. 
* Occurs often with user input or when establishing a connection.

```js
try {
    console.log("Trying...");
} catch (error) {
    console.error(error);
} finally {
    console.log("This always runs.");
}
```

We can throw an error by:

```js
throw new Error("Must be 18+");
```

# Closures 🔒

## Closures 🔒

A function defined inside of another function, the inner function has access to the variables and scope of the outer function. This allows for private variables and state maintenance.

```js
// Function that creates a counter using closures
function createCounter(){
    // `count` is a local variable within `createCounter`
    let count = 0;

    // Inner function `increment` has access to `count`
    function increment(){
        count++; // Increments the `count` variable
        console.log(`Count increased to ${count}`);
    }

    // Another inner function that accesses `count`
    function getCount(){
        return count; // Returns the current count
    }

    // Returns an object containing references to both functions
    return { increment, getCount };
}

// Creates a new counter instance
const counter = createCounter();

// Calls the `increment` function multiple times
counter.increment(); // Count increased to 1
counter.increment(); // Count increased to 2
counter.increment(); // Count increased to 3
counter.increment(); // Count increased to 4

// Retrieves and logs the current count
console.log(`The current count is ${counter.getCount()}`); // Output: The current count is 4
```

## setTimeout() ⏰

Executes a function **after a delay** (in milliseconds).

**~={red}Example One=~**:
* ✅ This **executes** the function after **2 seconds** (2000ms).
* 🚀 The timeout **is not canceled**, so "Hello after 2 seconds!" will print.*

```js
setTimeout(() => {
    console.log("Hello after 2 seconds!");
}, 2000);
```

**~={red}Example Two=~**:
* ✅ setTimeout() **returns an ID**, which we store in timeoutId.
* ✅ clearTimeout(timeoutId) **prevents execution** before 3 seconds pass.
* 🛑 "This won't run" **never appears** because the timeout was canceled.

```js
const timeoutId = setTimeout(() => console.log("This won't run"), 3000);
clearTimeout(timeoutId); // Cancels the timeout
```

# Working with the DOM 🌳

## What is the DOM? 🌳

![[Drawing 2025-03-16 22.54.31.excalidraw | center | 350]]

✅ **~={purple}What is the DOM?=~**
The Document Object Model (DOM) represents a web page as a tree-like structure, where each HTML element is a node.

✅ **~={purple}How is it Created?=~**
When a browser loads an HTML document, it parses it and constructs the DOM tree.

✅ **~={purple}Why is it Important?=~**
JavaScript can interact with the DOM to dynamically modify content, structure, and styles.

✅ **~={purple}document = DOM=~**
In JavaScript, document **represents the entire DOM**. `document` is an **object** that allows access to and manipulation of the web page.

## Element Selectors 📑

### 1️⃣ document.getElementById(id)

✅ Selects a single element by its id.
✅ Returns an element or null if not found.

```html
<h1 id="main-title">Hello, World!</h1>
```

```css
const title = document.getElementById("main-title");
console.log(title.textContent); // "Hello, World!"
```

### 2️⃣ document.getElementsByClassName(class)

✅ Selects **multiple elements** by class.
✅ Returns an **HTMLCollection** (live list of elements). This can be converted to an Array if necessary.

```html
<p class="info">Paragraph 1</p>
<p class="info">Paragraph 2</p>
```

```css
const paragraphs = document.getElementsByClassName("info");
console.log(paragraphs[0].textContent); // "Paragraph 1"
console.log(paragraphs[1].textContent); // "Paragraph 2"
```

### 3️⃣ document.getElementsByTagName(tag)

✅ Selects **multiple elements** by tag name.
✅ Returns an **HTMLCollection** (live).

```html
<div>First Div</div>
<div>Second Div</div>
```

```css
const divs = document.getElementsByTagName("div");
console.log(divs[0].textContent); // "First Div"
console.log(divs[1].textContent); // "Second Div"
```

### 4️⃣ document.querySelector(selector)

✅ Selects the first matching element using CSS selectors.
✅ Returns an element or null if not found.

```html
<p class="info">First</p>
<p class="info">Second</p>
```

```js
const firstParagraph = document.querySelector(".info");
console.log(firstParagraph.textContent); // "First"
```

### 5️⃣ document.querySelectorAll(selector)

✅ Selects all matching elements using CSS selectors.
✅ Returns a NodeList (static list of elements).

```html
<ul>
    <li class="item">Item 1</li>
    <li class="item">Item 2</li>
</ul>
```

```js
const items = document.querySelectorAll(".item");
console.log(items[0].textContent); // "Item 1"
console.log(items[1].textContent); // "Item 2"
```

## DOM Navigation 🧭

The process of navigating through the structure of an HTML document using JavaScript.



## Add & Change HTML 🛠️

## Mouse Events 🖱

## Key Events ⌨

## Hide/Show HTML 🖼

## NodeLists 📃

## classList 🧾


# Asynchronous JavaScript 💤

* Allows multiple operations to be performed concurrently without waiting Doesn't block the execution flow and allows the program to continue (I/O operations, network requests, fetching data) 
* **~={purple}Handled with=~**: Callbacks, Promises, Async/Await

## Callback Hell 🔥


## Promises 🤞


## Async/Await ⏳



# Handling Data & APIs 📄

## JSON Files 📄

## Fetching Data from an API ↩️