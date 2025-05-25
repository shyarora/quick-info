# Node.js

## Introduction

Node.js is an open-source, cross-platform JavaScript runtime environment that executes JavaScript code outside a web browser. Node.js lets developers use JavaScript to write command-line tools and for server-side scripting—running scripts server-side to produce dynamic web page content before the page is sent to the user's web browser.

## Installation

### macOS

```bash
# Using Homebrew
brew install node

# Using NVM (recommended for managing multiple Node versions)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
nvm install node  # Latest version
nvm install --lts  # LTS version
```

### Linux

```bash
# Using apt (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Using NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
nvm install node
```

### Windows

1. Download the installer from the [official Node.js website](https://nodejs.org/)
2. Run the installer and follow the prompts
3. Verify installation by running `node -v` in Command Prompt

## Core Concepts

### Module System

#### CommonJS (traditional Node.js)

```javascript
// Importing modules
const fs = require("fs");
const { promisify } = require("util");

// Exporting
module.exports = myFunction;
// or
module.exports = { func1, func2 };
```

#### ES Modules (newer standard)

```javascript
// package.json: { "type": "module" }

// Importing
import fs from "fs";
import { promisify } from "util";

// Exporting
export default myFunction;
// or
export { func1, func2 };
```

### Event Loop

Node.js operates on a single-threaded event loop model:

```javascript
console.log("First");

setTimeout(() => {
  console.log("Third - after timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Second - from microtask queue");
});

console.log("Fourth");

// Output: First, Fourth, Second, Third
```

## File System Operations

### Synchronous

```javascript
const fs = require("fs");

// Read file
const data = fs.readFileSync("file.txt", "utf8");
console.log(data);

// Write file
fs.writeFileSync("output.txt", "Hello Node.js");
```

### Asynchronous with Callbacks

```javascript
const fs = require("fs");

fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});

fs.writeFile("output.txt", "Hello Node.js", (err) => {
  if (err) {
    console.error(err);
  }
});
```

### Promises with fs/promises

```javascript
const fs = require("fs/promises");

async function readAndWriteFiles() {
  try {
    const data = await fs.readFile("file.txt", "utf8");
    console.log(data);
    await fs.writeFile("output.txt", "Hello Node.js");
  } catch (err) {
    console.error(err);
  }
}

readAndWriteFiles();
```

## HTTP Server

### Basic HTTP Server

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader("Content-Type", "text/plain");
  res.end("Hello World\n");
});

server.listen(3000, "127.0.0.1", () => {
  console.log("Server running at http://127.0.0.1:3000/");
});
```

### Using Express.js

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

## Working with JSON

```javascript
// Parse JSON
const jsonString = '{"name":"John", "age":30}';
const obj = JSON.parse(jsonString);
console.log(obj.name); // John

// Convert to JSON
const person = { name: "John", age: 30 };
const json = JSON.stringify(person);
console.log(json); // {"name":"John","age":30}
```

## Asynchronous Programming

### Callbacks

```javascript
function fetchData(callback) {
  setTimeout(() => {
    callback(null, "Data received");
  }, 1000);
}

fetchData((err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

### Promises

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Data received");
    }, 1000);
  });
}

fetchData()
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```

### Async/Await

```javascript
async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}

getData();
```

## Error Handling

### Try/Catch

```javascript
try {
  const data = JSON.parse("Invalid JSON");
} catch (err) {
  console.error("Error parsing JSON:", err.message);
}
```

### Promises

```javascript
fetchData()
  .then((data) => processData(data))
  .catch((err) => console.error("Error processing data:", err.message));
```

### Async/Await

```javascript
async function processDataAsync() {
  try {
    const data = await fetchData();
    return processData(data);
  } catch (err) {
    console.error("Error:", err.message);
    throw err; // Re-throw or handle
  }
}
```

### Error-First Callbacks

```javascript
function readConfig(path, callback) {
  fs.readFile(path, "utf8", (err, data) => {
    if (err) {
      callback(err);
      return;
    }

    try {
      const config = JSON.parse(data);
      callback(null, config);
    } catch (parseErr) {
      callback(parseErr);
    }
  });
}
```

## Debugging

### Using console

```javascript
console.log("Debug info");
console.error("Error message");
console.warn("Warning message");
console.time("Timer");
// Code to measure
console.timeEnd("Timer");
console.table([
  { name: "John", age: 30 },
  { name: "Jane", age: 25 },
]);
```

### Node Inspector

```bash
# Run node with inspector
node --inspect app.js

# Break on first line
node --inspect-brk app.js
```

### Using debugger statement

```javascript
function complexCalculation(x, y) {
  debugger; // Code will pause here when run with inspector
  return x * y + x / y;
}
```

## NPM (Node Package Manager)

### Package Management

```bash
# Initialize a new project
npm init

# Install dependencies
npm install express
npm i lodash

# Dev dependencies
npm install --save-dev jest

# Global packages
npm install -g nodemon

# Install specific version
npm install express@4.17.1

# Update dependencies
npm update
```

### package.json

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My Node.js app",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.17.1",
    "mongoose": "^6.0.13"
  },
  "devDependencies": {
    "jest": "^27.4.5",
    "nodemon": "^2.0.15"
  }
}
```

## Popular Frameworks & Libraries

### Web Frameworks

- **Express**: Minimal and flexible web framework
- **Koa**: Next-generation framework by Express team
- **Fastify**: High-performance web framework
- **NestJS**: Progressive framework for building efficient, scalable apps
- **Hapi**: Rich framework for building applications and services

### Database Integration

- **Mongoose**: MongoDB object modeling
- **Sequelize**: ORM for SQL databases
- **TypeORM**: ORM for TypeScript
- **Prisma**: Modern database toolkit
- **Knex**: SQL query builder

### Testing

- **Jest**: JavaScript testing framework
- **Mocha**: Feature-rich test framework
- **Chai**: Assertion library
- **Supertest**: HTTP assertions library

### Utilities

- **Lodash**: Utility library for working with arrays, objects, etc.
- **Moment/date-fns**: Date manipulation libraries
- **Axios**: Promise-based HTTP client
- **dotenv**: Environment variable management

## Best Practices

1. **Use Asynchronous Code**: Avoid blocking the event loop
2. **Error Handling**: Always handle errors and promise rejections
3. **Environment Variables**: Use for configuration and secrets
4. **Validation**: Validate user input and API requests
5. **Security**: Use security best practices (helmet, rate limiting, etc.)
6. **Logging**: Implement proper logging (winston, pino)
7. **Testing**: Write tests for your code
8. **Project Structure**: Organize code logically by feature or component

## Advanced Topics

### Streams

```javascript
const fs = require("fs");

const readStream = fs.createReadStream("largefile.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);

readStream.on("data", (chunk) => {
  console.log(`Received ${chunk.length} bytes of data`);
});

readStream.on("end", () => {
  console.log("Finished reading file");
});
```

### Worker Threads

```javascript
const { Worker, isMainThread, parentPort } = require("worker_threads");

if (isMainThread) {
  const worker = new Worker(__filename);
  worker.on("message", (message) => {
    console.log(`Received: ${message}`);
  });
  worker.postMessage("Hello from main thread");
} else {
  parentPort.on("message", (message) => {
    console.log(`Worker received: ${message}`);
    parentPort.postMessage("Hello from worker");
  });
}
```

### Child Processes

```javascript
const { exec, spawn } = require("child_process");

// Using exec
exec("ls -la", (err, stdout, stderr) => {
  if (err) {
    console.error(`Error: ${err.message}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
});

// Using spawn
const ls = spawn("ls", ["-la"]);

ls.stdout.on("data", (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on("data", (data) => {
  console.error(`stderr: ${data}`);
});

ls.on("close", (code) => {
  console.log(`child process exited with code ${code}`);
});
```

## Further Learning Resources

- [Official Node.js Documentation](https://nodejs.org/en/docs/)
- [MDN Web Docs - Node.js](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs)
- [Node.js Design Patterns](https://www.nodejsdesignpatterns.com/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [Free Node.js Courses on freeCodeCamp](https://www.freecodecamp.org/)
