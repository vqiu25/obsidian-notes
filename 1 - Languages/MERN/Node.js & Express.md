# Node.js & Express Fundamentals

## Node.js vs Express

🟢 **~={purple}Node.js=~**

* A JavaScript runtime that allows you to run JavaScript outside the browser (e.g., on servers).
* Used for building backend services or tools like Webpack, Babel, etc.
* It enables server-side programming using JavaScript.
=> **~={red}the tool that runs JavaScript on your computer/server instead of just in a browser. Basically, we create a local network on our device that will host the JS files written with Express=~**

🟣 **~={purple}Express=~**

* A Node.js package (framework) that simplifies creating backend APIs and web servers.
* Lets developers define routes (e.g., /users, /posts) and handle HTTP methods like GET, POST, etc.
=> **~={red}Express give you extra functions to make writing HTTP requests easier=~** 
How to install:

```js
npm install express
```

How to run:

```
# In one terminal tab (backend)
npm run dev        # runs Express with nodemon

# In another terminal tab (frontend)
cd client
npm start          # runs React dev server
```

## Basic Server Setup

* Import Express using: `import express from 'express';`
* Create an app instance: `const app = express();`
* Set the port: `const port = process.env.PORT || 3000;`

```js
import express from 'express';
const app = express();
const port = process.env.PORT || 3000;

// HTTP Request Methods

// Starts the app and listens for incoming connections on the 
// specified port.
app.listen(port, () => {
  console.log(`Server listening on port ${port}`);
});
```

## `req` and `res`

* `req`: This is the HTTP request coming from the client
* `res`: This is the HTTP response you send back to the client.

| **~={purple}Property=~** | **~={purple}Description=~**        | **~={purple}Example Usag=~e**                                     |
| ------------------------ | ---------------------------------- | ----------------------------------------------------------------- |
| req.params               | Route parameters from the URL path | req.params.id → /user/:id → /user/5 = '5'                         |
| req.query                | Query string parameters (after ?)  | req.query.search → /search?search=apple = 'apple'                 |
| req.body                 | Data sent in POST/PUT requests     | req.body.name → { name: 'Alice' } _(requires_ _express.json()__)_ |
| req.method               | HTTP method used                   | 'GET', 'POST', etc. → req.method                                  |
| req.url                  | Full request URL path              | req.url → /products?sort=asc                                      |

| **~={purple}Method=~** | **~={purple}Description=~**       | **~={purple}Example Usage=~**     |
| ---------------------- | --------------------------------- | --------------------------------- |
| res.send()             | Sends plain text or HTML response | res.send('Hello World!')          |
| res.json()             | Sends a JSON response             | res.json({ success: true })       |
| res.status()           | Sets HTTP status code             | res.status(404).send('Not Found') |
| res.redirect()         | Redirects to another URL          | res.redirect('/login')            |

### GET

* Used to retrieve data from the server.
* When a client makes a GET request to `/api/items`, the given function will be called

```js
app.get('/api/items', (req, res) => {
  res.send('Fetching all items');
});
```

### POST

* Used to send data to the server (e.g., creating a new item).

```js
app.post('/api/items', (req, res) => {
  res.send('Creating a new item');
});
```

### PUT

* Used to update an existing item (usually the full object).

```js
app.put('/api/items/:id', (req, res) => {
  const itemId = req.params.id;
  res.send(`Updating item with ID ${itemId}`);
});
```

### PATCH

* Used to partially update an item.

```js
app.patch('/api/items/:id', (req, res) => {
  const itemId = req.params.id;
  res.send(`Partially updating item with ID ${itemId}`);
});
```

### DELETE

* Used to delete an item.

```js
app.delete('/api/items/:id', (req, res) => {
  const itemId = req.params.id;
  res.send(`Deleting item with ID ${itemId}`);
});
```

# Serving Data

## JSON

* JavaScript objects can be converted to JSON easily using `res.json()`

```js
const todos = [
  { text: 'Do stuff', completed: false },
];

app.get('/todos', (req, res) => {
  res.json(todos);
});
```

## Static Files

* Sometimes you want to serve static assets (e.g., images, HTML, CSS, JS).
* Place these files inside a folder (commonly named public).
* This will make all files in the /public folder accessible via URL:

```js
import path from 'path';
app.use(express.static(path.join(__dirname, 'public')));
```

# Routers

## Routing & Folder Structure

`express.Router()` is a built-in middleware in Express.js used to **create modular, mountable route handlers**.

**~={red}Example=~**

Given the following folder structure:

```
project/
│
├── app.js
├── routes/
│   └── userRoutes.js
```

## 1️⃣ 🛠️ Creating a Router `app.js`

1. Import express and initialise the app
2. Import any routers defined in any other files
3. Tell the app to use all routes from that file under the /users path. In other words, all routes within `useRoutes` start from `/users`

```js
const express = require('express');
const app = express();

// For each external file, require it here
const userRoutes = require('./routes/userRoutes');

// The userRoutes file will have start from the /users URL
app.use('/users', userRoutes);
```

## 2️⃣ Mounting the Router `userRoutes.js`

1. Import `express` and create a `router`
2. Define routes. There routes will be appended onto the route defined in `app.js`

```js
const express = require('express');
const router = express.Router();

// Route 1: /users/
router.get('/', (req, res) => {
  res.send('User root');
});

// Route 2: /users/profile
router.get('/profile', (req, res) => {
  res.send('User profile');
});

module.exports = router;
```

## Folder Structure

![[Pasted image 20250325102700.png]]

# Axios

> [!Quote] Information
> We need to be able to consume web APIs - both our own and external APIs - from our frontend code. The fetch() API is standard in all modern browsers, and allows us to make arbitrary HTTP requests, and process any responses we receive. Axios is a popular third-party library which provides a similar (but in my opinion, friendlier) interface compared to fetch().

```js
npm install axios
```

## What is Axios?

* Axios is a **promise-based HTTP client** for making HTTP requests from Node.js or the browser.
* Automatically transforms JSON data

**~={purple}Template=~**

```js
axios({
  method: 'get' || 'post' || 'put' || 'delete',
  url: 'https://api.example.com/endpoint',
  headers: {
    'Content-Type': 'application/json', // Optional, unless sending JSON
    // 'Authorization': 'Bearer token' // For authenticated routes
  },
  data: {
    // Payload (for POST, PUT)
  }
})
.then(res => {
  console.log(res.data); // Handle successful response
})
.catch(err => {
  console.error(err); // Handle error
});
```

**~={purple}Default Behaviour=~**

**~={green}Default Headers=~**

* Axios can receive JSON or plain text
* Axios sends JSON data by default

**~={green}Status Code Handling=~**

* Axios treats any non-2xx responses as an error

```js
axios.get('/endpoint')
  .then(res => console.log(res.data))
  .catch(err => {
    if (err.response) {
      console.log('Error:', err.response.status, err.response.data);
    } else {
      console.log('Network Error');
    }
  });
```

## Sample HTTP Requests

**~={purple}GET=~**

```js
async function fetchData() {
  try {
    const res = await axios.get('https://api.example.com/data');
    console.log(res.data);
  } catch (err) {
    console.error('GET Error:', err);
  }
}
```

**~={purple}POST=~**

```js
async function createUser() {
  try {
    const res = await axios.post('https://api.example.com/users', {
      name: 'Alice',
      age: 30
    });
    console.log('Created user:', res.data);
  } catch (err) {
    console.error('POST Error:', err);
  }
}
```

```js
async function deleteUser(id) {
  try {
    const res = await axios.delete(`https://api.example.com/users/${id}`);
    console.log('Deleted:', res.data);
  } catch (err) {
    console.error('DELETE Error:', err);
  }
}
```

# Consuming Express Endpoints from Frontend

**~={red}Example=~**

Creates a reusable hook for **fetching data via GET** requests with Axios, while also exposing a loading state.

---

**🔹 ~={purple}Function Definition=~**

```js
export default function useGet(url, initialState = null)
```

* Exports a custom hook named useGet.
* Accepts two arguments:
	* url: The API endpoint to fetch data from.
	* initialState: Optional — the default value for data (defaults to null).

**🔹 ~={purple}State Variables=~**

```js
const [data, setData] = useState(initialState);
const [isLoading, setLoading] = useState(false);
```

* data: Holds the API response once fetched.
* isLoading: Tracks if the request is currently in progress.
* Both use useState() to manage state internally.

**🔹 ~={purple}useEffect Hook=~**

```js
useEffect(() => {
  async function fetchData() {
    setLoading(true);
    const response = await axios.get(url);
    setData(response.data);
    setLoading(false);
  }
  fetchData();
}, []);
```

~={green}**What this does=~:**
* Runs **once on mount** ([] means no dependencies).
* Defines an async function called fetchData():
	* Sets loading to true before fetching.
	* Performs a GET request with axios.get(url).
	* Stores the result (response.data) in data.
	* Sets loading back to false once done.
* Calls fetchData() immediately.

**🔹 ~={purple}Return Value=~**

```
return { data, isLoading };
```

* Exposes data and isLoading to the component using this hook.
* You can use the above hook like:

```js
const { data, isLoading } = useGet('/api/stuff');
```

