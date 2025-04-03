# ✅ React Fundamentals

## Toolchains

> [!QUOTE] Title
> Toolchains streamline environment setup process

### Vite

* A fast, powerful, modern toolkit for creating JavaScript-based frontends in a variety of frameworks
	* **~={purple}Rapid Development Server=~**: Instant startup and quick hot-module replacement (HMR) for a fast development experience.
	* **~={purple}Automatic Dependency Resolution=~**: Efficiently resolves and pre-bundles dependencies for optimal performance.
	* **~={purple}Built-in JSX and TypeScript Support=~**: Automatically transforms JSX and compiles TypeScript without extra configuration.
	* **~={purple}CSS and Styling Support=~*o*: Includes built-in handling for CSS, CSS Modules, PostCSS, and preprocessors like Sass.

```js
npm create vite@latest
```

```js
// Installs all required dependencies listed in your newly created 
// project’s package.json.
npm install

// Starts a local development server that serves your React app,
// providing hot module replacement
npm run dev
```

## Creating Components

* **~={green}Functional components=~** are thus-called as they are written as a single JavaScript function, which may optionally take a single argument for its properties
* Components return `JSX` Code

```jsx
function Component() {
	return (
		<div>
			<p> "Hello World" </p>
		</div>
	);
}
// Exports the App function so it can be used outside this file (with 
// a corresponding import).
export default Component 
```

```jsx
const Component = () =>
	<div>
		<p> "Hello World" </p>
	</div>

export default Component
```

We can import this component to another file by:
* We can only return a single component, but we can wrap multiple components into one using the fragment syntax `<> </>`

```jsx
import Component from './Component.jsx'

const App = () => 
	<>
		<Component/>
		<Component/>
	</>
```

## JSX

* **~={red}What is JSX=~**: JSX is a JavaScript syntax extension that looks similar to HTML and supports inline JavaScript expressions, with slightly different syntax.
	* Can embed any JavaScript expression within `{ }`
	* Attribute naming uses camelCase (`className`, `onClick`).

```jsx
function Welcome(props) {
  return <h1 className="greeting">Hello, {props.name}</h1>;
}
```

# 🧩 Components & Props

## Properties

* Properties allow parent components to pass configuration down to children. These are read-only.

**~={purple}Passing in Properties to Components=~**

```jsx
<Component firstName = "Cinnamon", age = {5}/>
```

**~={purple}Receiving Properties=~**

```jsx
const Component = (props) =>
	<div>
		<p> "Hello {props.firstName}" </p>
	</div>
```

We can also use ~={green}**object destructuring**=~ to avoid having to write `props`

```jsx
const Component = ({ firstName, age }) => 
  <div>
    <p>Hello {firstName}</p>
  </div>
```

## Conditional Rendering

* Allows you to control what gets rendered in your application based on certain conditions — to show, hide, or change components.

**~={red}Example=~**: Render a component if true, otherwise don't render

```jsx
const UserProfile = (props) => (
  <div>
    <h2>User Profile</h2>
    {props.name ? <p><strong>Name:</strong> {props.name}</p> : null}
    {/* Render age if exists, otherwise show fallback */}
    {props.age ? (
      <p><strong>Age:</strong> {props.age}</p>
    ) : (
      <p><em>No age provided.</em></p>
    )}
  </div>
);
```

* We can't use if-statements inside JSX, but we can use the ternary operator
	* **~={green}Truthy=~**: `true`, everything else
	* **~={red}Falsey=~**: `""`, `0`, `null`, `NaN`, `undefined`, `false`

## Iteration

* We cannot write loops in JSX, but we can output arrays within `{}`, and can use `.map()` to iterate through it, applying a function to each element
* Each element in the array needs a unique key

```jsx
const ItemList = (props) =>
    <div>
      <h2>Shopping Items</h2>
      {props.items.map((item, index) => (
        <p key={index}>Item: {item}</p>
      ))}
    </div>
```

# 🎨 Styling in React

## CSS

* CSS properties are written in camelCase

To use CSS in a React Component, we do:

```jsx
import `./styles.css`
```

## CSS Modules

* `Component.jsx` is paired with `Component.module.css`

Where in the `JSX` file, we:

```jsx
import styles from `./Component.module.css`;
```

`styles` is effectively an object, where we can do:

* Which will apply the `.article` part of the CSS file to our `<div>`
* In CSS modules, all styles should ideally be defined using class selectors (e.g., `.article`) rather than element selectors (e.g., `div`) or ID selectors (`#id`). This ensures styles remain scoped to the module, preventing unintended global overrides and making the component more reusable and maintainable.

```jsx
<div className = {styles.article}>
</div>
```

# 🔄 React Context API

> [!QUOTE] Information
> React Context lets you share data globally across components without manually passing props through every level of the component tree. Think of it as **global state or configuration** that any component can access.

| ~={purple}Step=~        | ~={purple}Step=~                               |
| --------------- | ----------------------------------------- |
| createContext() | Creates a context object                  |
| <Provider>      | Wraps children and provides value         |
| useContext()    | Reads the context value inside components |
## How to use React Context

1️⃣ **~={purple}Create the Context=~**

```jsx
import { createContext } from 'react';

const ThemeContext = createContext(); // optional: pass default value
```

2️⃣ **~={purple}Provide the Context=~**

* At the very top level, wrap our app or a subtree in a provider

```jsx
<ThemeContext.Provider value={{ theme: 'dark', user: { name: 'Alice' }}}>
  <App />
</ThemeContext.Provider>
```

3️⃣ **~={purple}Consume the Context=~**

* At any level below the where the context was provided, we can now access the value set in the provider

```jsx
import { useContext } from 'react';

const { theme, user } = useContext(ThemeContext);
console.log(user.name); // "Alice"
```

**~={red}Example with State=~**

1️⃣ **~={purple}Create the Context and State (ThemeContext.js)=~**

* This file **creates the context** and **wraps the app with a provider** that holds state.

```jsx
import { createContext, useState } from 'react';

// 1. Create the context
export const ThemeContext = createContext();

// 2. Create a provider component
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light'); // holds state (light/dark)

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children} {/* exposes context to children */}
    </ThemeContext.Provider>
  );
}
```

2️⃣ **~={purple}Provide the Context (App.jsx) =~**

* This is your root component. It wraps everything inside the ThemeProvider.

```jsx
import React from 'react';
import { ThemeProvider } from './ThemeContext';
import Layout from './Layout';

function App() {
  return (
    <ThemeProvider>
      <Layout />
    </ThemeProvider>
  );
}

export default App;
```

3️⃣ **~={purple}Consume the Context (Layout.jsx)=~**

* This component reads the context and updates the theme using a toggle button.

```jsx
import React, { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function Layout() {
  const { theme, setTheme } = useContext(ThemeContext);

  const toggleTheme = () => {
    // We're passing in a function, where the current state value
    // will be set as `prev`, and the function will be ran
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <div className={theme}>
      <h1>Current theme: {theme}</h1>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}

export default Layout;
```

# 🌐 React Router v6

> [!QUOTE] Information
> React Router is one of the most popular libraries assisting with _client-side routing_ in our React applications. Client-side routing - which gives the illusion of navigating to different areas of our application via the URL path without actually making additional server requests - is a key feature in many single-page applications.

This package adds React components and hooks (BrowserRouter, Link, NavLink, Routes, Route, useLocation, and useNavigate, amongst others) which:
* Correctly allow the generation of hyperlinks (normal hyperlinks are purely server side (causes a refresh))
* Define routes
* Abstract away the challenges working with the History API

```jsx
npm install react-router-dom
```

## Setting it Up

Wrap your root component with `<BrowserRouter>` to enable routing across your app:

* Any component rendered within <App /> (and any of their children) can safely use `react-router-dom` features like `<Routes>`, `useNavigate`, `useParams`, etc. No additional wrapping is needed.

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

## Defining Routes

* Use `<Routes>` to define the route tree and `<Route>` to match a specific path.
	* element is the component rendered when the path matches the URL
	* `path="*"` is a wildcard (used for 404 fallback).

```jsx
<Routes>
  <Route path="/page1" element={<p>Page One</p>} />
  <Route path="/page2" element={<p>Page Two</p>} />
  <Route path="*" element={<p>404 Not Found!</p>} />
</Routes>
```

## Rendering Components on every Page

* Any JSX **outside** `<Routes>` (but inside `<BrowserRouter>`) will render on **every page**.
* Great for headers, navbars, or footers that remain visible across all routes.

```jsx
<BrowserRouter>
  // Renders everywhere
  <header><h1>My Website</h1></header>
  // Renders dependent on URL
  <Routes> ... </Routes>
</BrowserRouter>
```

## Navigation with `<Link>`

* Use `<Link>` to navigate between routes **~={red}without full-page reload=~** (triggers client side routing)
* This replaces the traditional hyperlinking using `<a>`
* In essence, to a reach a route defined at URL `"/x"`, we use a link that links to the same URL `"/x"`

```jsx
<Link to="/page1">Page One</Link>
<Link to="/page2">Page Two</Link>
```

**~={red}Example=~**

```jsx
import React from 'react';
import { Routes, Route, Link } from 'react-router-dom';

function Home() {
  return <h2>Welcome to the Home Page</h2>;
}

function About() {
  return <h2>About Us</h2>;
}

function NotFound() {
  return <h2>404 - Page Not Found</h2>;
}

export default function App() {
  return (
    <div>
      <header>
        <h1>My Website</h1>
        <nav>
          <Link to="/">Home</Link> |{" "}
          <Link to="/about">About</Link>
        </nav>
      </header>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </div>
  );
}
```

| ~={purple}URL=~       | ~={purple}URL=~        |
| ------------- | ------------------ |
| /             | Home Page          |
| /about        | About Page         |
| Anything else | 404 Not Found Page |

## Navigation with `<NavLink>`

* Is an extension of `<Link>`, where it automatically adds styling or class names when its target route matches the current URL.

```jsx
// Apply the highlight name to the className (we can associate this with
// CSS), when the link's isActive = true
<NavLink
  to="/about"
  className={({ isActive }) => isActive ? "highlight" : ""}
>
  About
</NavLink>
```

## Navigation with `<Navigate />`

* `<Navigate />` is a React Router component used to redirect users to another route automatically, without needing a hook or user interaction. Used for automatically redirection.

Use within a `<Route>`:

* This will make it so if the URL is `/`, it will automatically redirected you to `/home`

```jsx
<Routes>
  <Route path="/" element={<Navigate to="/home" />} />
  <Route path="/home" element={<Home />} />
</Routes>
```

Use of `replace`:

* Rather then pushing to the URL, it will replace the the entire URL with `/login`

```jsx
<Navigate to="/login" replace />
```

## Absolute vs Relative Paths

**~={purple}Absolute=~**

* Start with `/`
* Always refer to the root of your app, no matter where they are used
* You use them when you want to go to a specific route, from anywhere in your app

```jsx
<Link to="/settings">Settings</Link>
// Goes to:
// http://localhost:3000/settings
```

**~={purple}Relative Paths=~**

* Does not start with `/`
* Relative to the current route you're in
* Useful when you want to navigate deeper within the current section

```jsx
// Let's say you're at http://localhost:3000/account

<Link to="profile">Profile</Link>
//Goes to
// http://localhost:3000/account/profile
```

## Nested Routes

* Nested routes allow you to render components inside other components, based on the URL.

```jsx
<Routes>
  <Route path="/dashboard" element={<Dashboard />}>
    <Route path="profile" element={<Profile />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```

Where if a user travels to `/dashboard/profile` or `/dashboard/settings`, in the `Dashboard.jsx` component, will contain the tag `<outlet>`, this is where either the:
* `Profile.jsx` or
* `Settings.jsx`
Will get embedded:

![[Drawing 2025-03-22 23.04.31.excalidraw | center | 400]]

# 🎣 React Hooks

## What are React Hooks

* Enable functional components to manage state and side effects
	* **~={purple}State=~**: Manages data within the component that changes over time, like user input or a counter.
	* **~={purple}Side Effect=~**: Interacts with things outside the component, such as fetching data, modifying the DOM, or setting up event listeners.

## useState()

### Overview

> [!QUOTE] Information
> * A component's properties should be considered immutable - they don't change once set.
> * Components may maintain local state, which can change. Should this change, the component will re-render itself.

**~={purple}Importing=~**

```jsx
import { useState } from `react`;
```

**~={purple}Sample=~**

* `useState(false)`: This value is the initial value for the state (in this case, the Boolean value "false"). It's only used the first time this is rendered.
* `likedState`: This will be an array with two elements:
	* `liked`: Index [0] will be the value itself
	* `setLiked`: Index [1] will be a function we can call to change the value of `liked`. Every time we call this function, the entire component will re-render.

```jsx
const likedState = useState(false);
const liked = likedState[0];
const settiked = likedState [1];
```

### Examples

* Array dereferencing is employed to make code more readable

```jsx
const LikeButton = () => {
  const [liked, setLiked] = useState(false);

  // A button that displays "unlike" or "like"
  return (
    <button onClick={() => setLiked(!liked)}>
      {liked ? 'Unlike' : 'Like'}
    </button>
  );
};

export default LikeButton;
```

* `onClick={() => setLiked(!liked)}`: 
	* `onClick={...}`: This is the event handler for the button click. We pass a function to handle what happens when the button is clicked. We run the JS expression within `{}`
	* `() => setLiked(!liked)`: This is an **arrow function**
		* `()`: These are our function parameters. We’re not passing any in, because setLiked() doesn’t need any inputs — it just uses the liked variable from the outer scope.
		* `setLiked(!liked)`: Is our function body

## useEffect()

## Overview

> [!QUOTE] Information
> This hook allows us to execute code _after_ each time a component is rendered. Further, we can execute additional code to "clean up" after a previous `useEffect()` execution, and we can introduce additional arguments which modify how often `useEffect()` triggers.

**~={purple}Importing=~**

```jsx
import { useEffect } from `react`;
```

**~={purple}Sample=~**

* The function inside `useEffect()` **runs after the component renders**.
* The **~={green}dependency array=~** controls when the effect runs:
	* [] → only once when component mounts.
	* [someVar] → runs when someVar changes.
* The **~={green}returned function=~** is optional, but used for cleanup (e.g., clearing timers, unsubscribing).

```jsx
useEffect(() => {
  // ✅ Code to run when the component mounts or dependencies change
  // e.g., fetching data, setting a timer, subscribing to something

  // Optional: create a resource (like a timer or subscription)
  const resource = doSomething();

  // 🔁 Cleanup function (runs before re-run or unmount)
  return () => {
    // Clean up the resource (e.g., clear timer, unsubscribe)
    cleanup(resource);
  };
}, [/* dependencies here */]);
```

## Examples

```jsx
import React, { useState, useEffect } from 'react';

const Timer = () => {
  const [seconds, setSeconds] = useState(0);
  const [isActive, setActive] = useState(false);

  useEffect(() => {
    let interval = null;

    // If active, wait 1 second then increment 'seconds'
    if (isActive) {
      interval = setTimeout(() => {
        setSeconds(seconds + 1);
      }, 1000);
    }
	
	// Clear timeout before effect re-runs or component unmounts
    return () => clearTimeout(interval);
    
  }, [seconds, isActive]); 
  // Re-run effect when 'seconds' or 'isActive' changes

  const toggleTimer = () => setActive(!isActive);
  const resetTimer = () => {
    setActive(false);
    setSeconds(0);
  };

  return (
    <div>
      <h1>Timer: {seconds} second{seconds !== 1 ? 's' : ''}</h1>
      <button onClick={toggleTimer}>
        {isActive ? 'Pause' : 'Start'}
      </button>
      <button onClick={resetTimer}>Reset</button>
    </div>
  );
};

export default Timer;
```

## useRef()

> [!QUOTE] Information
>
> This hook gives you a way to persist **mutable values** across renders **without triggering a re-render**. It’s commonly used to store references to **DOM elements**, timers, or any value that you want to keep around but not cause updates to the UI.

**~={purple}Importing=~**

```jsx
import { useRef } from 'react';
```

**~={purple}Sample=~**

* `useRef()` returns a ref object with a `.current` property.
* Updating `.current` does not cause a re-render.

```jsx
const myRef = useRef(initialValue); // usually null for DOM, or any default value

// Reading or updating the value
console.log(myRef.current);
myRef.current = newValue;
```

| Feature            | `useState()` Approach                                                                                   | `useRef()` Approach                                               |
| ------------------ | ------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **~={green}Pro=~** | Other UI elements using the same stateful value will be updated immediately as the user types           | No unnecessary re-renders – better performance                    |
| **~={red}Con=~**   | If we don’t need the instant update functionality, it causes unnecessary re-renders (hurts performance) | Can’t have other UI elements update immediately as the user types |

# 🎣 React Router Hooks
## useNavigate()

> [!QUOTE] Information
>
> Sometimes, you don’t want to wait for a user to click a `<Link>` — you want to navigate automatically based on an event (like form submission, button click, successful login, etc).

**~={purple}Importing=~**

```jsx
import { useNavigate } from 'react-router-dom';
```

**~={purple}Sample=~**

* When the button is clicked, the app will navigate to `/dashboard `automatically — no `<Link>` needed.

```jsx
// So long as we import this, as long as the <Route> '/dashboard'
// is defined somewhere, we can access it
import { useNavigate } from 'react-router-dom';

function LoginButton() {
  const navigate = useNavigate();

  function handleLogin() {
    // (e.g. auth logic...)
    navigate('/dashboard');
  }

  return <button onClick={handleLogin}>Log In</button>;
}
```

Additional Options Exist:

```jsx
navigate('/path');         // normal push (adds to history stack)
navigate('/path', { replace: true });  // replaces current entry (like a redirect)
navigate(-1);              // go back
navigate(1);               // go forward
```

## useParams()

> [!QUOTE] Information
>
> Path params are dynamic parts of the URL — useful when your route needs to change based on an ID, a username, etc

**~={purple}Importing=~**

```jsx
import { useParams } from 'react-router-dom';
```

**~={purple}Sample=~**

With the following route:
* `:userId` is a placeholder
* Matches any path like /users/123 or /users/cinnamoroll

```jsx
<Route path="/users/:userId" element={<UserProfile />} />
```

Use the `useParams()` hook:

```jsx
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { userId } = useParams(); 
  // grabs the value from the URL (:userId) as a string

  return <h2>Viewing profile for user: {userId}</h2>;
}
```

| ~={purple}URL=~     | ~={purple}URL=~ | ~={purple}URL=~ |
| ----------- | ----------------- | ---------------- |
| /users/42   | /users/:userId    | "42"             |
| /users/alex | /users/:userId    | "alex"           |

# Summary

| ~={purple}Approach=~          | ~={purple}Approach=~                                    | ~={purple}Approach=~                    |
| ----------------------------- | ------------------------------------------------------- | --------------------------------------- |
| 🔗 `<Link />` / `<NavLink />` | You want users to **click** to navigate                 | Menus, sidebars, buttons                |
| 🔀 `useNavigate()` hook       | You want to **navigate in response to logic or events** | Form submission, login redirects        |
| 🚧 `<Navigate />`             | You want to **redirect inside JSX immediately**         | Protected routes, route-level redirects |

# Storing Data in Local Browser Storage

* React state is transient (temporary)
  → It’s lost when the page reloads or the browser is closed.
* This is fine for short-term interactions, but sometimes we need **persistence**.

| ~={purple}Use Case=~                                              | ~={purple}Use Case=~ |
| ----------------------------------------------------------------- | -------------------- |
| Save data for **only this browser (i.e. what was added to cart)** | ✅ Yes                |
| Save data across **all user devices**                             | ❌ No – use backend   |

**~={purple}How to Store in Local Storage using React Hooks=~**

> [!Quote] Information
> React state resets on page refresh. If you want to **persist state across reloads**, you can store it in the browser’s **localStorage**. This custom hook wraps that logic so you can use it just like useState.

1️⃣ **~={purple}Define the Hook=~**

* Makes the logic reusable across your app
* Takes a key (storage key) and an optional initialValue

```jsx
export function useLocalStorage(key, initialValue = null) {
```

2️⃣ **~={purple}Load Saved Value from Local Storage=~**

* Runs only once on initial state setup
* It tries to:
	* Load the saved data from localStorage
	* Parse it from JSON
	* Fallback to initialValue if not found or if there is an error

```jsx
const [value, setValue] = useState(() => {
  try {
    const data = window.localStorage.getItem(key);
    return data ? JSON.parse(data) : initialValue;
  } catch {
    return initialValue;
  }
});
```

3️⃣ **~={purple}Save Updated Value to Local Storage=~**

* Every time the value changes, it’s saved back to localStorage
* This ensures persistence across reloads

```jsx
useEffect(() => {
  window.localStorage.setItem(key, JSON.stringify(value));
}, [value, setValue]);
```

4️⃣ **~={purple}Return the Value and Setter Function=~**

```jsx
return [value, setValue];
```

**~={red}Example Usage=~**

```jsx
function Counter() {
  // The JSON key which we are storing our data in will be "counter"
  const [count, setCount] = useLocalStorage('counter', 0);

  return (
    <button onClick={() => setCount(count + 1)}>
      The current value is: {count}
    </button>
  );
}
```

| ~={purple}Feature=~          | ~={purple}Feature=~                                   |
| -------------------- | ------------------------------------------------- |
| useLocalStorage hook | Combines React state with localStorage            |
| First load           | Reads saved data if it exists, else uses fallback |
| useEffect inside     | Saves data on every update                        |
| Usage                | Just like useState()                              |