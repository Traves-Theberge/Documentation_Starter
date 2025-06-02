TITLE: Adding One-Time Event Listener with emitter.once (JS)
DESCRIPTION: Adds a listener function that is invoked at most once for the specified event. After the first time the event is triggered, the listener is removed. The method returns the emitter instance for chaining.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_36

LANGUAGE: javascript
CODE:
```
server.once('connection', (stream) => {
  console.log('Ah, we have our first user!');
});
```

----------------------------------------

TITLE: Creating a Node-API Thread-Safe Function (C)
DESCRIPTION: This function creates a thread-safe JavaScript function object that can be called from any thread. It takes parameters for the JavaScript function, async resource tracking, queue size, initial thread count, finalization callbacks, context data, and a custom JavaScript call callback. It returns a `napi_threadsafe_function` handle.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_205

LANGUAGE: C
CODE:
```
NAPI_EXTERN napi_status
napi_create_threadsafe_function(napi_env env,
                                napi_value func,
                                napi_value async_resource,
                                napi_value async_resource_name,
                                size_t max_queue_size,
                                size_t initial_thread_count,
                                void* thread_finalize_data,
                                napi_finalize thread_finalize_cb,
                                void* context,
                                napi_threadsafe_function_call_js call_js_cb,
                                napi_threadsafe_function* result);
```

----------------------------------------

TITLE: Installing ansi-styles (Shell)
DESCRIPTION: Provides the command line instruction to install the ansi-styles package using the npm package manager. This command fetches the package from the npm registry and adds it as a dependency to the current project.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/ansi-styles/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install ansi-styles
```

----------------------------------------

TITLE: Basic Undici Stream GET Request (JavaScript)
DESCRIPTION: Shows how to use `undici.stream` to efficiently handle a GET request by directly piping the response body into a writable stream provided by a factory function. It sets up a server, client, makes a stream request, collects the streamed data in a buffer, and logs the collected response body.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/Dispatcher.md#_snippet_13

LANGUAGE: javascript
CODE:
```
import { createServer } from 'http';
import { Client } from 'undici';
import { once } from 'events';
import { Writable } from 'stream';

const server = createServer((request, response) => {
  response.end('Hello, World!');
}).listen();

await once(server, 'listening');

const client = new Client(`http://localhost:${server.address().port}`);

const bufs = [];

try {
  await client.stream({
    path: '/',
    method: 'GET',
    opaque: { bufs }
  }, ({ statusCode, headers, opaque: { bufs } }) => {
    console.log(`response received ${statusCode}`);
    console.log('headers', headers);
    return new Writable({
      write (chunk, encoding, callback) {
        bufs.push(chunk);
        callback();
      }
    });
  });

  console.log(Buffer.concat(bufs).toString('utf-8'));

  client.close();
  server.close();
} catch (error) {
  console.error(error);
}
```

----------------------------------------

TITLE: Configuring Undici MockAgent for Success (JavaScript)
DESCRIPTION: Illustrates setting up Undici's MockAgent for testing. It sets the mock agent as the global dispatcher, creates a mock pool for a specific origin, and defines an interceptor that matches a POST request to '/bank-transfer' including specific headers and body content, returning a 200 success response.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/best-practices/mocking-request.md#_snippet_1

LANGUAGE: javascript
CODE:
```
// index.test.mjs
import { strict as assert } from 'assert'
import { MockAgent, setGlobalDispatcher, } from 'undici'
import { bankTransfer } from './bank.mjs'

const mockAgent = new MockAgent();

setGlobalDispatcher(mockAgent);

// Provide the base url to the request
const mockPool = mockAgent.get('http://localhost:3000');

// intercept the request
mockPool.intercept({
  path: '/bank-transfer',
  method: 'POST',
  headers: {
    'X-TOKEN-SECRET': 'SuperSecretToken',
  },
  body: JSON.stringify({
    recipient: '1234567890',
    amount: '100'
  })
}).reply(200, {
  message: 'transaction processed'
})

const success = await bankTransfer('1234567890', '100')

assert.deepEqual(success, { message: 'transaction processed' })
```

----------------------------------------

TITLE: Implementing File ReadStream Class - Node.js Streams - JavaScript
DESCRIPTION: This comprehensive snippet demonstrates creating a custom Readable stream, `ReadStream`, that reads data from a file using the Node.js `fs` module. It extends `Readable`, implements `_construct` to asynchronously open the file and set up the file descriptor, implements `_read` to read chunks of data from the file descriptor and push them into the stream's buffer, and implements `_destroy` to ensure the file descriptor is closed when the stream is destroyed. It showcases handling asynchronous operations and error propagation within a custom readable stream.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_85

LANGUAGE: javascript
CODE:
```
const { Readable } = require('node:stream');
const fs = require('node:fs');

class ReadStream extends Readable {
  constructor(filename) {
    super();
    this.filename = filename;
    this.fd = null;
  }
  _construct(callback) {
    fs.open(this.filename, (err, fd) => {
      if (err) {
        callback(err);
      } else {
        this.fd = fd;
        callback();
      }
    });
  }
  _read(n) {
    const buf = Buffer.alloc(n);
    fs.read(this.fd, buf, 0, n, null, (err, bytesRead) => {
      if (err) {
        this.destroy(err);
      } else {
        this.push(bytesRead > 0 ? buf.slice(0, bytesRead) : null);
      }
    });
  }
  _destroy(err, callback) {
    if (this.fd) {
      fs.close(this.fd, (er) => callback(er || err));
    } else {
      callback(err);
    }
  }
}
```

----------------------------------------

TITLE: Importing Modules and Files using require() in Node.js (JavaScript)
DESCRIPTION: Provides practical examples of using the `require()` function to import various types of resources: local JavaScript files via relative paths, JSON data files, and built-in Node.js modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
// Importing a local module with a path relative to the `__dirname` or current
// working directory. (On Windows, this would resolve to .\path\myLocalModule.)
const myLocalModule = require('./path/myLocalModule');

// Importing a JSON file:
const jsonData = require('./path/filename.json');

// Importing a module from node_modules or Node.js built-in module:
const crypto = require('node:crypto');
```

----------------------------------------

TITLE: Resolving Multiple Path Segments from CWD with Node.js path.resolve
DESCRIPTION: This snippet illustrates how `path.resolve()` combines multiple relative path segments (`wwwroot`, `static_files/png/`, `../gif/image.gif`) and resolves them against the current working directory (CWD) if no initial absolute path is provided. It shows how '..' navigates up one directory level.
SOURCE: https://github.com/asana/node/blob/main/doc/api/path.md#_snippet_6

LANGUAGE: javascript
CODE:
```
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
```

----------------------------------------

TITLE: Manually Migrating Buffer(non-number) to Buffer.from JavaScript
DESCRIPTION: Provides a manual migration pattern for creating a Buffer from a non-numeric argument (string, array, etc.), replacing the deprecated new Buffer() with Buffer.from(). Includes a safeguard for older Node.js versions lacking Buffer.from and emphasizes the necessary type check for unsanitized input due to security risks.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
var buf;\nif (Buffer.from && Buffer.from !== Uint8Array.from) {\n  buf = Buffer.from(notNumber, encoding);\n} else {\n  if (typeof notNumber === 'number')\n    throw new Error('The \"size\" argument must be of type number.');\n  buf = new Buffer(notNumber, encoding);\n}
```

----------------------------------------

TITLE: Checking Path Existence using fs.existsSync
DESCRIPTION: Illustrates how to synchronously check if a given path exists using `fs.existsSync`. It returns a boolean value (`true` if it exists, `false` otherwise) and prints a message based on the result.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_77

LANGUAGE: javascript
CODE:
```
import { existsSync } from 'node:fs';

if (existsSync('/etc/passwd'))
  console.log('The path exists.');
```

----------------------------------------

TITLE: Checking Node.js and npm Versions (Shell)
DESCRIPTION: These commands are used in a terminal or command prompt to display the currently installed versions of Node.js and npm. They are useful for verifying installation and ensuring the correct versions are being used. Requires Node.js and npm binaries to be in the system's PATH.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/install.md#_snippet_0

LANGUAGE: Shell
CODE:
```
node -v
npm -v
```

----------------------------------------

TITLE: Extending an Object using Object.assign in JavaScript
DESCRIPTION: The `util._extend()` API is deprecated as it's an unmaintained legacy feature. The standard and recommended way to merge properties from a source object into a target object is using `Object.assign()`. This mutates the target object and returns it.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
target = Object.assign(target, source)
```

----------------------------------------

TITLE: Setting Debugger Breakpoint in JavaScript
DESCRIPTION: This standard JavaScript statement is used to programmatically pause execution at the exact line where it appears, invoking any attached debugger and allowing inspection of the runtime environment, variables, and the current call stack.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/inspector/debugger/wasm-source-expected.txt#_snippet_0

LANGUAGE: javascript
CODE:
```
debugger;
```

----------------------------------------

TITLE: Creating and Aborting AbortController (JavaScript)
DESCRIPTION: This snippet demonstrates the basic usage of the AbortController and its associated AbortSignal in Node.js. It shows how to create an instance, attach an event listener for the 'abort' event using the signal property, trigger the cancellation using the abort() method, and check the aborted status via the signal's aborted property. This pattern is used to signal cancellation in promise-based APIs.
SOURCE: https://github.com/asana/node/blob/main/doc/api/globals.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const ac = new AbortController();

ac.signal.addEventListener('abort', () => console.log('Aborted!'),
                           { once: true });

ac.abort();

console.log(ac.signal.aborted);  // Prints true
```

----------------------------------------

TITLE: Setting Default Module Type to ES Modules (JSON)
DESCRIPTION: Defines the "type" field in package.json with the value "module". This setting causes Node.js to interpret all .js files within this package and its subdirectories as ES modules by default, unless overridden by a closer package.json file or the file extension (.cjs or .mjs).
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_34

LANGUAGE: json
CODE:
```
{
  "type": "module"
}
```

----------------------------------------

TITLE: Creating a TLS Echo Server in Node.js
DESCRIPTION: This snippet demonstrates setting up a basic TLS server using Node.js's `tls` module. It reads server certificate and key files, optionally requests a client certificate and validates it against a CA, listens on a specified port, and echoes received data back to the connecting client. It requires PEM-encoded certificate and key files ('server-key.pem', 'server-cert.pem', 'client-cert.pem') to exist in the same directory.
SOURCE: https://github.com/asana/node/blob/main/doc/api/tls.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
const tls = require('node:tls');
const fs = require('node:fs');

const options = {
  key: fs.readFileSync('server-key.pem'),
  cert: fs.readFileSync('server-cert.pem'),

  // This is necessary only if using client certificate authentication.
  requestCert: true,

  // This is necessary only if the client uses a self-signed certificate.
  ca: [ fs.readFileSync('client-cert.pem') ],
};

const server = tls.createServer(options, (socket) => {
  console.log('server connected',
              socket.authorized ? 'authorized' : 'unauthorized');
  socket.write('welcome!\n');
  socket.setEncoding('utf8');
  socket.pipe(socket);
});
server.listen(8000, () => {
  console.log('server bound');
});
```

----------------------------------------

TITLE: Demonstrating Unsafe and Safe Array Iteration in JavaScript
DESCRIPTION: This snippet set shows the potentially unsafe `for-of` loop pattern, the safer traditional `for` loop, and an example demonstrating how user-land modification of `Array.prototype[Symbol.iterator]` can break the `for-of` loop but not the `for` loop.
SOURCE: https://github.com/asana/node/blob/main/doc/contributing/primordials.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
for (const item of array) {
  console.log(item);
}
```

LANGUAGE: JavaScript
CODE:
```
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```

LANGUAGE: JavaScript
CODE:
```
// User-land
Array.prototype[Symbol.iterator] = () => ({
  next: () => ({ done: true }),
});

// Core
let forOfLoopBlockExecuted = false;
let forLoopBlockExecuted = false;
const array = [1, 2, 3];
for (const item of array) {
  forOfLoopBlockExecuted = true;
}
for (let i = 0; i < array.length; i++) {
  forLoopBlockExecuted = true;
}
console.log(forOfLoopBlockExecuted); // false
console.log(forLoopBlockExecuted); // true
```

----------------------------------------

TITLE: Importing fs Module [MJS]
DESCRIPTION: Imports the callback and synchronous APIs of the Node.js file system module using ES module syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import * as fs from 'node:fs';
```

----------------------------------------

TITLE: Defining Basic Node.js Conditional Exports in package.json
DESCRIPTION: Demonstrates using the `"import"` and `"require"` conditions within the `"exports"` field of `package.json`. This allows the package to provide different entry points or versions of modules depending on how they are consumed (ES module `import` vs. CommonJS `require()`). Also includes `"type": "module"` indicating the package's default type.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_8

LANGUAGE: JSON
CODE:
```
// package.json
{
  "exports": {
    "import": "./index-module.js",
    "require": "./index-require.cjs"
  },
  "type": "module"
}
```

----------------------------------------

TITLE: Asserting Thrown Error Message in JavaScript
DESCRIPTION: Demonstrates the preferred way to use `assert.throws` in JavaScript tests by providing a regular expression that matches the full expected error message thrown by a function, rather than just a partial match.
SOURCE: https://github.com/asana/node/blob/main/doc/contributing/writing-tests.md#_snippet_10

LANGUAGE: javascript
CODE:
```
assert.throws(
  () => {
    throw new Error('Wrong value');
  },
  /^Error: Wrong value$/, // Instead of something like /Wrong value/
);
```

----------------------------------------

TITLE: AsyncLocalStorage HTTP Request Logging (MJS)
DESCRIPTION: This comprehensive example illustrates how to use `AsyncLocalStorage` within an HTTP server to associate a unique request ID with logging messages throughout the request's lifecycle, even across asynchronous calls like `setImmediate`. It creates an `AsyncLocalStorage` instance, assigns an ID using `run()`, retrieves the ID with `getStore()`, and logs messages tied to that ID.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import http from 'node:http';
import { AsyncLocalStorage } from 'node:async_hooks';

const asyncLocalStorage = new AsyncLocalStorage();

function logWithId(msg) {
  const id = asyncLocalStorage.getStore();
  console.log(`${id !== undefined ? id : '-'}:`, msg);
}

let idSeq = 0;
http.createServer((req, res) => {
  asyncLocalStorage.run(idSeq++, () => {
    logWithId('start');
    // Imagine any chain of async operations here
    setImmediate(() => {
      logWithId('finish');
      res.end();
    });
  });
}).listen(8080);

http.get('http://localhost:8080');
http.get('http://localhost:8080');
// Prints:
//   0: start
//   1: start
//   0: finish
//   1: finish
```

----------------------------------------

TITLE: Extending Readable Stream Class - Node.js Streams - JavaScript
DESCRIPTION: This snippet provides the basic structure for creating a custom Readable stream by extending the `stream.Readable` class in Node.js. It shows the constructor calling the parent `super(options)` constructor, which is required when extending Node.js stream classes. Further stream logic, such as implementing the `_read` method, would be added within this class.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_81

LANGUAGE: javascript
CODE:
```
const { Readable } = require('node:stream');

class MyReadable extends Readable {
  constructor(options) {
    // Calls the stream.Readable(options) constructor.
    super(options);
    // ...
  }
}
```

----------------------------------------

TITLE: Setting HTTP Status Code (Node.js JS)
DESCRIPTION: Explains how to set the HTTP status code (e.g., 404) for the response when using implicit headers by assigning a number to the `response.statusCode` property. This value is sent when headers are flushed.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_34

LANGUAGE: js
CODE:
```
response.statusCode = 404;
```

----------------------------------------

TITLE: Handling 'error' Events (ESM)
DESCRIPTION: Demonstrates the best practice of registering a listener for the 'error' event using `on()` to catch emitted errors gracefully, preventing the Node.js process from crashing.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_14

LANGUAGE: javascript
CODE:
```
import { EventEmitter } from 'node:events';
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('error', (err) => {
  console.error('whoops! there was an error');
});
myEmitter.emit('error', new Error('whoops!'));
// Prints: whoops! there was an error
```

----------------------------------------

TITLE: Correctly Ordering Async FS Operations with Promises (Node.js MJS)
DESCRIPTION: Demonstrates the correct approach to sequence asynchronous file system operations (`rename`, `stat`) using the promise-based API (`node:fs/promises`) and `async/await`. This ensures operations complete in the desired order, preventing race conditions. Requires the `node:fs/promises` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_87

LANGUAGE: mjs
CODE:
```
import { rename, stat } from 'node:fs/promises';

const oldPath = '/tmp/hello';
const newPath = '/tmp/world';

try {
  await rename(oldPath, newPath);
  const stats = await stat(newPath);
  console.log(`stats: ${JSON.stringify(stats)}`);
} catch (error) {
  console.error('there was an error:', error.message);
}
```

----------------------------------------

TITLE: Creating and Accessing Error Message in Node.js JavaScript
DESCRIPTION: Demonstrates how to create a new `Error` object with a message string passed to the constructor and access that message using the `error.message` property. This shows a basic usage of the `Error` constructor and property.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_8

LANGUAGE: javascript
CODE:
```
const err = new Error('The message');
console.error(err.message);
// Prints: The message
```

----------------------------------------

TITLE: Printing to Stdout using console.log in Node.js
DESCRIPTION: Demonstrates printing a variable to standard output using `console.log` with and without `printf`-style substitution. It shows how `%d` is used as a placeholder for a number, leveraging `util.format()` internally.
SOURCE: https://github.com/asana/node/blob/main/doc/api/console.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const count = 5;
console.log('count: %d', count);
// Prints: count: 5, to stdout
console.log('count:', count);
// Prints: count: 5, to stdout
```

----------------------------------------

TITLE: Exporting Function using ES Modules in Node.js
DESCRIPTION: This snippet defines a simple JavaScript function `addTwo` that takes a number and returns the number plus 2. It then uses the `export` statement to make this function available for use in other modules, demonstrating the standard ES module export syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
function addTwo(num) {
  return num + 2;
}

export { addTwo };
```

----------------------------------------

TITLE: Executing npm install command - Bash
DESCRIPTION: Provides the basic command-line syntax for using the `npm install` command to install packages, optionally specifying package names. It also lists common aliases that can be used instead of `install`. This command requires npm to be installed and configured.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-install.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm install [<package-spec> ...]

aliases: add, i, in, ins, inst, insta, instal, isnt, isnta, isntal, isntall
```

----------------------------------------

TITLE: Copying File using Promises - Node.js MJS
DESCRIPTION: This snippet demonstrates how to use `fsPromises.copyFile` from the `node:fs/promises` module to copy a file asynchronously. It shows a basic copy operation and another attempt using `constants.COPYFILE_EXCL` to prevent overwriting an existing destination, handling potential errors with a `try...catch` block.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_19

LANGUAGE: mjs
CODE:
```
import { copyFile, constants } from 'node:fs/promises';

try {
  await copyFile('source.txt', 'destination.txt');
  console.log('source.txt was copied to destination.txt');
} catch {
  console.error('The file could not be copied');
}

// By using COPYFILE_EXCL, the operation will fail if destination.txt exists.
try {
  await copyFile('source.txt', 'destination.txt', constants.COPYFILE_EXCL);
  console.log('source.txt was copied to destination.txt');
} catch {
  console.error('The file could not be copied');
}
```

----------------------------------------

TITLE: Constructing WHATWG URL with Template Literal
DESCRIPTION: Demonstrates creating a WHATWG `URL` object using the constructor with a template literal string composed of component parts.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
const pathname = '/a/b/c';\nconst search = '?d=e';\nconst hash = '#fgh';\nconst myURL = new URL(`https://example.org\${pathname}\${search}\${hash}`);
```

----------------------------------------

TITLE: Installing find-up Package
DESCRIPTION: Use npm to install the find-up package as a project dependency.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/find-up/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
$ npm install find-up
```

----------------------------------------

TITLE: Handling Readline 'line' Event in Node.js
DESCRIPTION: Demonstrates how to attach an event listener to the `'line'` event of a `readline.Interface` instance. This event fires whenever the user enters a line of input (typically by pressing Enter). The listener receives the entered line as a string argument.
SOURCE: https://github.com/asana/node/blob/main/doc/api/readline.md#_snippet_6

LANGUAGE: javascript
CODE:
```
rl.on('line', (input) => {
  console.log(`Received: ${input}`);
});
```

----------------------------------------

TITLE: Signing and Verifying Data (Update Method, mjs)
DESCRIPTION: Demonstrates using the Node.js `crypto` module's `Sign` and `Verify` classes with the `update()` method to generate and verify signatures. It generates an RSA key pair, updates the `Sign` instance with data, generates the signature, updates the `Verify` instance with the same data, and verifies the signature using the public key. Requires `node:crypto` and uses ES Module syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_21

LANGUAGE: javascript
CODE:
```
const {
  generateKeyPairSync,
  createSign,
  createVerify,
} = await import('node:crypto');

const { privateKey, publicKey } = generateKeyPairSync('rsa', {
  modulusLength: 2048,
});

const sign = createSign('SHA256');
sign.update('some data to sign');
sign.end();
const signature = sign.sign(privateKey);

const verify = createVerify('SHA256');
verify.update('some data to sign');
verify.end();
console.log(verify.verify(publicKey, signature));
// Prints: true
```

----------------------------------------

TITLE: Passing Arguments and 'this' to Listeners (CJS)
DESCRIPTION: Illustrates how arguments passed to `emit()` are forwarded to listener functions. It also shows that, for standard function expressions, the `this` keyword inside the listener refers to the EventEmitter instance using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_3

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('event', function(a, b) {
  console.log(a, b, this, this === myEmitter);
  // Prints:
  //   a b MyEmitter {
  //     _events: [Object: null prototype] { event: [Function (anonymous)] },
  //     _eventsCount: 1,
  //     _maxListeners: undefined,
  //     [Symbol(kCapture)]: false
  //   } true
});
myEmitter.emit('event', 'a', 'b');
```

----------------------------------------

TITLE: Deleting File with fs/promises [MJS]
DESCRIPTION: Demonstrates asynchronously deleting a file using the promise-based `unlink` function from `node:fs/promises` with ES module syntax and `async`/`await`, handling potential errors with `try...catch`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_4

LANGUAGE: javascript
CODE:
```
import { unlink } from 'node:fs/promises';

try {
  await unlink('/tmp/hello');
  console.log('successfully deleted /tmp/hello');
} catch (error) {
  console.error('there was an error:', error.message);
}
```

----------------------------------------

TITLE: Checking if a value is an Object in JavaScript
DESCRIPTION: The `util.isObject()` API is deprecated. To check if a value is an object (and not null), combine a truthiness check for `arg` with the `typeof` operator. This correctly identifies objects while excluding primitives and null.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
arg && typeof arg === 'object'
```

----------------------------------------

TITLE: Running - npm start Command - Bash
DESCRIPTION: Shows the basic syntax for executing the `npm start` command from the terminal, allowing optional arguments to be passed to the script defined in `package.json`. This is the primary way to invoke the start script associated with a package.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-start.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm start [-- <args>]
```

----------------------------------------

TITLE: Measuring Performance with PerformanceObserver and Marks/Measures in JavaScript
DESCRIPTION: This snippet demonstrates how to use `PerformanceObserver` to track 'measure' performance entries and the `performance` object's `mark` and `measure` methods to record timing between points. It sets up an observer, creates initial measures and marks, performs a simulated process, creates more marks/measures, and clears marks in the observer callback. Requires the `node:perf_hooks` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/perf_hooks.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const { PerformanceObserver, performance } = require('node:perf_hooks');

const obs = new PerformanceObserver((items) => {
  console.log(items.getEntries()[0].duration);
  performance.clearMarks();
});
obs.observe({ type: 'measure' });
performance.measure('Start to Now');

performance.mark('A');
doSomeLongRunningProcess(() => {
  performance.measure('A to Now', 'A');

  performance.mark('B');
  performance.measure('A to B', 'A', 'B');
});
```

----------------------------------------

TITLE: Parsing Arguments with Tokens in Node.js (MJS)
DESCRIPTION: This example demonstrates how to use `util.parseArgs` with the `tokens: true` option to get detailed information about parsed arguments. It shows how to iterate through the tokens to implement custom behavior, specifically handling negated options like `--no-color` and ensuring the last argument wins when options are repeated.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_23

LANGUAGE: mjs
CODE:
```
import { parseArgs } from 'node:util';

const options = {
  'color': { type: 'boolean' },
  'no-color': { type: 'boolean' },
  'logfile': { type: 'string' },
  'no-logfile': { type: 'boolean' },
};
const { values, tokens } = parseArgs({ options, tokens: true });

// Reprocess the option tokens and overwrite the returned values.
tokens
  .filter((token) => token.kind === 'option')
  .forEach((token) => {
    if (token.name.startsWith('no-')) {
      // Store foo:false for --no-foo
      const positiveName = token.name.slice(3);
      values[positiveName] = false;
      delete values[token.name];
    } else {
      // Resave value so last one wins if both --foo and --no-foo.
      values[token.name] = token.value ?? true;
    }
  });

const color = values.color;
const logfile = values.logfile ?? 'default.log';

console.log({ logfile, color });
```

----------------------------------------

TITLE: Creating Buffer Instances - ESM
DESCRIPTION: Demonstrates various methods for creating Buffer instances using ES Module import syntax, including allocating zero-filled, pre-filled, uninitialized, or pre-populated Buffers from arrays or strings.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

// Creates a zero-filled Buffer of length 10.
const buf1 = Buffer.alloc(10);

// Creates a Buffer of length 10,
// filled with bytes which all have the value `1`.
const buf2 = Buffer.alloc(10, 1);

// Creates an uninitialized buffer of length 10.
// This is faster than calling Buffer.alloc() but the returned
// Buffer instance might contain old data that needs to be
// overwritten using fill(), write(), or other functions that fill the Buffer's
// contents.
const buf3 = Buffer.allocUnsafe(10);

// Creates a Buffer containing the bytes [1, 2, 3].
const buf4 = Buffer.from([1, 2, 3]);

// Creates a Buffer containing the bytes [1, 1, 1, 1] – the entries
// are all truncated using `(value & 255)` to fit into the range 0–255.
const buf5 = Buffer.from([257, 257.5, -255, '1']);

// Creates a Buffer containing the UTF-8-encoded bytes for the string 'tést':
// [0x74, 0xc3, 0xa9, 0x73, 0x74] (in hexadecimal notation)
// [116, 195, 169, 115, 116] (in decimal notation)
const buf6 = Buffer.from('tést');

// Creates a Buffer containing the Latin-1 bytes [0x74, 0xe9, 0x73, 0x74].
const buf7 = Buffer.from('tést', 'latin1');
```

----------------------------------------

TITLE: Encrypting Data with AES-CBC in JavaScript
DESCRIPTION: This asynchronous function `aesEncrypt` encrypts a plaintext string using AES-CBC. It generates a new AES key and a random Initialization Vector (IV), encodes the plaintext using `TextEncoder`, and performs encryption using `subtle.encrypt`. It returns the key, IV, and ciphertext. Requires a `generateAesKey` function (assumed from a previous example).
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_6

LANGUAGE: javascript
CODE:
```
const crypto = globalThis.crypto;

async function aesEncrypt(plaintext) {
  const ec = new TextEncoder();
  const key = await generateAesKey();
  const iv = crypto.getRandomValues(new Uint8Array(16));

  const ciphertext = await crypto.subtle.encrypt({
    name: 'AES-CBC',
    iv,
  }, key, ec.encode(plaintext));

  return {
    key,
    iv,
    ciphertext,
  };
}
```

----------------------------------------

TITLE: Closing FileHandle with fs/promises [MJS]
DESCRIPTION: Demonstrates opening a file asynchronously using `fs/promises.open` and ensuring the returned `FileHandle` is closed using `filehandle.close()` within a `try...finally` block to prevent resource leaks.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_10

LANGUAGE: javascript
CODE:
```
import { open } from 'node:fs/promises';

let filehandle;
try {
  filehandle = await open('thefile.txt', 'r');
} finally {
  await filehandle?.close();
}
```

----------------------------------------

TITLE: Closing Standard Node-API Handle Scope in C
DESCRIPTION: Closes the specified handle scope, invalidating all handles associated with it. Scopes must be closed in the reverse order they were opened. This function is safe to call even if there is a pending JavaScript exception. It requires the Node-API environment and the handle for the scope to be closed. It returns napi_ok.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_46

LANGUAGE: C
CODE:
```
NAPI_EXTERN napi_status napi_close_handle_scope(napi_env env,
                                                napi_handle_scope scope);
```

----------------------------------------

TITLE: Implementing Async Task Offloading with Worker Threads - JavaScript
DESCRIPTION: Shows how to create a worker thread from the main thread to perform an asynchronous task like parsing a script. It utilizes `workerData` to pass the script content and `parentPort` for communication, resolving a Promise when the worker finishes or rejects on error. Note that for efficiency, a worker pool is recommended for real applications.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const {
  Worker, isMainThread, parentPort, workerData,
} = require('node:worker_threads');

if (isMainThread) {
  module.exports = function parseJSAsync(script) {
    return new Promise((resolve, reject) => {
      const worker = new Worker(__filename, {
        workerData: script,
      });
      worker.on('message', resolve);
      worker.on('error', reject);
      worker.on('exit', (code) => {
        if (code !== 0)
          reject(new Error(`Worker stopped with exit code ${code}`));
      });
    });
  };
} else {
  const { parse } = require('some-js-parsing-library');
  const script = workerData;
  parentPort.postMessage(parse(script));
}
```

----------------------------------------

TITLE: Default 'error' Event Behavior (CJS)
DESCRIPTION: Shows the critical default behavior when an 'error' event is emitted on an EventEmitter without any registered listeners: the error is thrown, leading to a crash and process exit using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_13

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('whoops!'));
// Throws and crashes Node.js
```

----------------------------------------

TITLE: Specifying MIT or Apache-2.0 License in package.json (Modern)
DESCRIPTION: Modern method for specifying multiple licenses using an SPDX expression. Replaces older object/array formats.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_9

LANGUAGE: JSON
CODE:
```
{
  "license": "(MIT OR Apache-2.0)"
}
```

----------------------------------------

TITLE: Using stream.pipeline (Promises API) for File Compression (ESM)
DESCRIPTION: Shows how to use the Promise-based `stream.pipeline` function with ES Module import syntax. This snippet performs the same file compression task as the CommonJS example, piping data from a readable stream through a gzip stream to a writable stream.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { pipeline } from 'node:stream/promises';
import { createReadStream, createWriteStream } from 'node:fs';
import { createGzip } from 'node:zlib';

await pipeline(
  createReadStream('archive.tar'),
  createGzip(),
  createWriteStream('archive.tar.gz'),
);
console.log('Pipeline succeeded.');
```

----------------------------------------

TITLE: Mocking Undici Request to Reply with Error - JavaScript
DESCRIPTION: Illustrates how to configure an `undici` mock to respond with a specific error instead of a successful response. It sets up a mock that throws an `Error('kaboom')` when a GET request to `/foo` is intercepted. The snippet includes a try...catch block to demonstrate handling the mocked error.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/MockPool.md#_snippet_7

LANGUAGE: javascript
CODE:
```
import { MockAgent, setGlobalDispatcher, request } from 'undici'

const mockAgent = new MockAgent()
setGlobalDispatcher(mockAgent)

const mockPool = mockAgent.get('http://localhost:3000')

mockPool.intercept({
  path: '/foo',
  method: 'GET'
}).replyWithError(new Error('kaboom'))

try {
  await request('http://localhost:3000/foo', {
    method: 'GET'
  })
} catch (error) {
  console.error(error) // Error: kaboom
}
```

----------------------------------------

TITLE: Accessing and Setting WHATWG url.hash
DESCRIPTION: Shows how to get the fragment portion of a URL using the `hash` property and how to modify it, observing that invalid characters are percent-encoded.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
const myURL = new URL('https://example.org/foo#bar');\nconsole.log(myURL.hash);\n// Prints #bar\n\nmyURL.hash = 'baz';\nconsole.log(myURL.href);\n// Prints https://example.org/foo#baz
```

----------------------------------------

TITLE: Emitting Events and Triggering Multiple Listeners in Node.js EventEmitter (MJS)
DESCRIPTION: This snippet demonstrates in ECMAScript Modules how the `emit` method synchronously calls multiple listeners registered for the same event. It shows listeners receiving arguments and the order of execution. It also uses `listeners()` to show the registered functions. Requires the `node:events` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_28

LANGUAGE: mjs
CODE:
```
import { EventEmitter } from 'node:events';
const myEmitter = new EventEmitter();

// First listener
myEmitter.on('event', function firstListener() {
  console.log('Helloooo! first listener');
});
// Second listener
myEmitter.on('event', function secondListener(arg1, arg2) {
  console.log(`event with parameters ${arg1}, ${arg2} in second listener`);
});
// Third listener
myEmitter.on('event', function thirdListener(...args) {
  const parameters = args.join(', ');
  console.log(`event with parameters ${parameters} in third listener`);
});

console.log(myEmitter.listeners('event'));

myEmitter.emit('event', 1, 2, 3, 4, 5);

// Prints:
// [
//   [Function: firstListener],
//   [Function: secondListener],
//   [Function: thirdListener]
// ]
// Helloooo! first listener
// event with parameters 1, 2 in second listener
// event with parameters 1, 2, 3, 4, 5 in third listener
```

----------------------------------------

TITLE: Installing Chalk NPM Package
DESCRIPTION: This snippet shows the standard command line instruction to install the Chalk library using npm. It's required to add Chalk as a project dependency before using it in a Node.js application.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/@babel/highlight/node_modules/chalk/readme.md#_snippet_0

LANGUAGE: console
CODE:
```
$ npm install chalk
```

----------------------------------------

TITLE: Adding Authorization Header with Interceptor (JavaScript)
DESCRIPTION: This snippet demonstrates creating a basic interceptor function that adds an 'Authorization' header with a bearer token to the outgoing request options. It shows how to return a new dispatch function that modifies options before calling the original dispatch, and how to configure this interceptor when creating a client instance.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/DispatchInterceptor.md#_snippet_0

LANGUAGE: javascript
CODE:
```
'use strict'

const insertHeaderInterceptor = dispatch => {
  return function InterceptedDispatch(opts, handler){
    opts.headers.push('Authorization', 'Bearer [Some token]')
    return dispatch(opts, handler)
  }
}

const client = new Client('https://localhost:3000', {
  interceptors: { Client: [insertHeaderInterceptor] }
})

```

----------------------------------------

TITLE: Enabling Timer and Date Mocking with Initial Time in Node.js Test (MJS)
DESCRIPTION: Enables mocking for `Date` using the `mock.timers.enable()` function and sets the initial mocked time to a specific number of milliseconds (1000). Requires the `node:test` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_38

LANGUAGE: javascript
CODE:
```
import { mock } from 'node:test';
mock.timers.enable({ apis: ['Date'], now: 1000 });
```

----------------------------------------

TITLE: Encrypting File using Cipher Stream Pipeline (CJS)
DESCRIPTION: Demonstrates encrypting the contents of a file and writing the output to another file using Node.js streams and the `crypto.Cipher` class in a CommonJS pipeline. It uses `fs.createReadStream`, `fs.createWriteStream`, and `stream.pipeline` to connect the input, cipher, and output streams.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_3

LANGUAGE: javascript
CODE:
```
const {
  createReadStream,
  createWriteStream,
} = require('node:fs');

const {
  pipeline,
} = require('node:stream');

const {
  scrypt,
  randomFill,
  createCipheriv,
} = require('node:crypto');

const algorithm = 'aes-192-cbc';
const password = 'Password used to generate key';

// First, we'll generate the key. The key length is dependent on the algorithm.
// In this case for aes192, it is 24 bytes (192 bits).
scrypt(password, 'salt', 24, (err, key) => {
  if (err) throw err;
  // Then, we'll generate a random initialization vector
  randomFill(new Uint8Array(16), (err, iv) => {
    if (err) throw err;

    const cipher = createCipheriv(algorithm, key, iv);

    const input = createReadStream('test.js');
    const output = createWriteStream('test.enc');

    pipeline(input, cipher, output, (err) => {
      if (err) throw err;
    });
  });
});
```

----------------------------------------

TITLE: Declaring Peer Dependencies - JSON
DESCRIPTION: Demonstrates how to use `peerDependencies` to specify compatibility with a host tool or library without making it a direct dependency. This is common for plugins and ensures the user has a compatible version of the host package installed alongside your package.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_33

LANGUAGE: json
CODE:
```
{
  "name": "tea-latte",
  "version": "1.3.5",
  "peerDependencies": {
    "tea": "2.x"
  }
}
```

----------------------------------------

TITLE: Promisifying Callback Function with util.promisify (async/await)
DESCRIPTION: This example demonstrates using a function promisified with `util.promisify` within an `async function`. The Promise returned by the promisified function (`stat`) is consumed using the `await` keyword, allowing for cleaner asynchronous code that resembles synchronous execution.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_27

LANGUAGE: js
CODE:
```
const util = require('node:util');
const fs = require('node:fs');

const stat = util.promisify(fs.stat);

async function callStat() {
  const stats = await stat('.');
  console.log(`This directory is owned by ${stats.uid}`);
}

callStat();
```

----------------------------------------

TITLE: Using child_process.exec with util.promisify and async/await in Node.js
DESCRIPTION: Shows how to convert the callback-based `child_process.exec` into a Promise-based function using `util.promisify`, allowing it to be used with `async/await` for cleaner asynchronous code. The promise resolves with an object containing `stdout` and `stderr` on success or rejects with an error.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
const util = require('node:util');
const exec = util.promisify(require('node:child_process').exec);

async function lsExample() {
  const { stdout, stderr } = await exec('ls');
  console.log('stdout:', stdout);
  console.error('stderr:', stderr);
}

lsExample();
```

----------------------------------------

TITLE: Parsing Multipart Form Data with Busboy Node.js
DESCRIPTION: This snippet demonstrates how to create a basic Node.js HTTP server that uses @fastify/busboy to parse incoming POST requests with multipart form data. It shows how to listen for 'file' and 'field' events to handle file uploads and standard form fields, logging information about each part as it is processed.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/node_modules/@fastify/busboy/README.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const http = require('node:http');
const { inspect } = require('node:util');
const Busboy = require('busboy');

http.createServer((req, res) => {
  if (req.method === 'POST') {
    const busboy = new Busboy({ headers: req.headers });
    busboy.on('file', (fieldname, file, filename, encoding, mimetype) => {
      console.log(`File [${fieldname}]: filename: ${filename}, encoding: ${encoding}, mimetype: ${mimetype}`);
      file.on('data', data => {
        console.log(`File [${fieldname}] got ${data.length} bytes`);
      });
      file.on('end', () => {
        console.log(`File [${fieldname}] Finished`);
      });
    });
    busboy.on('field', (fieldname, val, fieldnameTruncated, valTruncated, encoding, mimetype) => {
      console.log(`Field [${fieldname}]: value: ${inspect(val)}`);
    });
    busboy.on('finish', () => {
      console.log('Done parsing form!');
      res.writeHead(303, { Connection: 'close', Location: '/' });
      res.end();
    });
    req.pipe(busboy);
  } else if (req.method === 'GET') {
    res.writeHead(200, { Connection: 'close' });
    res.end(`<html><head></head><body>\n               <form method="POST" enctype="multipart/form-data">\n                <input type="text" name="textfield"><br>\n                <input type="file" name="filefield"><br>\n                <input type="submit">\n              </form>\n            </body></html>`);
  }
}).listen(8000, () => {
  console.log('Listening for requests');
});

// Example output, using http://nodejs.org/images/ryan-speaker.jpg as the file:
//
// Listening for requests
// File [filefield]: filename: ryan-speaker.jpg, encoding: binary
// File [filefield] got 11971 bytes
// Field [textfield]: value: 'testing! :-)'
// File [filefield] Finished
// Done parsing form!

```

----------------------------------------

TITLE: Defining Node.js Exports/Imports Patterns in package.json
DESCRIPTION: Shows how to define subpath patterns using `*` in the `exports` and `imports` fields of `package.json`. The `*` acts as a wildcard for matching and replacement, allowing a single rule to map multiple source files to their corresponding exposed paths. This helps avoid listing every single subpath explicitly.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_4

LANGUAGE: JSON
CODE:
```
// ./node_modules/es-module-package/package.json
{
  "exports": {
    "./features/*.js": "./src/features/*.js"
  },
  "imports": {
    "#internal/*.js": "./src/internal/*.js"
  }
}
```

----------------------------------------

TITLE: Differentiating module.exports and exports (Node.js)
DESCRIPTION: Shows the difference between assigning properties to `module.exports` (which affects the module's export value) and reassigning the local `exports` variable (which breaks the reference to `module.exports` and does not affect the module's export value).
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
module.exports.hello = true; // Exported from require of module
exports = { hello: false };  // Not exported, only available in the module
```

----------------------------------------

TITLE: npx and npm exec Usage Examples Bash
DESCRIPTION: Provides practical command examples demonstrating various ways to use npx and npm exec. Examples include running a locally available package command, executing a specific binary from a package using --package, and running a multi-command shell script via the -c option.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npx.md#_snippet_2

LANGUAGE: bash
CODE:
```
$ npm exec -- tap --bail test/foo.js
$ npx tap --bail test/foo.js
```

LANGUAGE: bash
CODE:
```
$ npm exec --package=foo -- bar --bar-argument
# ~ or ~
$ npx --package=foo bar --bar-argument
```

LANGUAGE: bash
CODE:
```
$ npm x -c 'eslint && say "hooray, lint passed"'
$ npx -c 'eslint && say "hooray, lint passed"'
```

----------------------------------------

TITLE: Unsafe File Write After Access Check using fs.access (Node.js)
DESCRIPTION: This example demonstrates a deprecated pattern of using `fs.access` to check if a file exists before attempting to write to it with `fs.open`. This approach is unsafe because it creates a race condition where the file's state can change between the `access` check and the `open` call.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_31

LANGUAGE: mjs
CODE:
```
import { access, open, close } from 'node:fs';

access('myfile', (err) => {
  if (!err) {
    console.error('myfile already exists');
    return;
  }

  open('myfile', 'wx', (err, fd) => {
    if (err) throw err;

    try {
      writeMyData(fd);
    } finally {
      close(fd, (err) => {
        if (err) throw err;
      });
    }
  });
});
```

----------------------------------------

TITLE: Streaming FileHandle Data (mjs)
DESCRIPTION: This snippet demonstrates how to use the `readableWebStream()` method of a Node.js `FileHandle` to obtain a Web-compatible `ReadableStream`. It shows how to asynchronously iterate over the stream's chunks using `for await...of` and logs each chunk. Note that the `FileHandle` must be closed manually after the stream is consumed.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
import {
  open,
} from 'node:fs/promises';

const file = await open('./some/file/to/read');

for await (const chunk of file.readableWebStream())
  console.log(chunk);

await file.close();
```

----------------------------------------

TITLE: Example package.json test script configuration - JSON
DESCRIPTION: Provides a JSON example illustrating how to define a `test` script within the `scripts` property of a `package.json` file. This script specifies the actual command (e.g., `node test.js`) that will be executed when `npm test` is run in the project's root directory.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-test.md#_snippet_1

LANGUAGE: json
CODE:
```
{
  "scripts": {
    "test": "node test.js"
  }
}
```

----------------------------------------

TITLE: Performing Basic GET Request with Undici Client in Node.js
DESCRIPTION: Demonstrates how to make a simple GET request using the `undici` `Client.request` method with async/await. It sets up a basic Node.js HTTP server that responds with a fixed string. The client sends a GET request to the server and asynchronously waits for the response data, which includes the status code, headers, body, and trailers. It then processes the response body by piping it to `console.log`. Requires `undici`, `http`, and `events`.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/Dispatcher.md#_snippet_9

LANGUAGE: javascript
CODE:
```
import { createServer } from 'http'
import { Client } from 'undici'
import { once } from 'events'

const server = createServer((request, response) => {
  response.end('Hello, World!')
}).listen()

await once(server, 'listening')

const client = new Client(`http://localhost:${server.address().port}`)

try {
  const { body, headers, statusCode, trailers } = await client.request({
    path: '/',
    method: 'GET'
  })
  console.log(`response received ${statusCode}`)
  console.log('headers', headers)
  body.setEncoding('utf8')
  body.on('data', console.log)
  body.on('end', () => {
    console.log('trailers', trailers)
  })

  client.close()
  server.close()
} catch (error) {
  console.error(error)
}
```

----------------------------------------

TITLE: Using timersPromises.scheduler.wait (ESM)
DESCRIPTION: Shows how to use the experimental `scheduler.wait` function, which is part of the Scheduling APIs draft. It demonstrates awaiting a specified delay using `await` in an ESM context.
SOURCE: https://github.com/asana/node/blob/main/doc/api/timers.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
import { scheduler } from 'node:timers/promises';

await scheduler.wait(1000); // Wait one second before continuing
```

----------------------------------------

TITLE: Using Multiple Require Flags Node.js Shell
DESCRIPTION: Demonstrates how to use the Node.js `--require` (or `-r`) command-line flag multiple times to pre-load several modules before executing the main script. Each specified module will be required in the order specified, which is useful for setting up global variables or monkey-patching before the main program logic runs.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cli.md#_snippet_17

LANGUAGE: Shell
CODE:
```
node --require "./a.js" --require "./b.js"
```

----------------------------------------

TITLE: Creating HTTP Server with Request Listener (CJS, Node.js)
DESCRIPTION: Demonstrates creating a simple Node.js HTTP server using the `http.createServer()` method with a CJS `require()` statement and a request listener function provided directly as an argument. The server listens on port 8000 and responds to all requests with a JSON object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_50

LANGUAGE: JavaScript
CODE:
```
const http = require('node:http');

// Create a local server to receive data from
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({
    data: 'Hello World!',
  }));
});

server.listen(8000);
```

----------------------------------------

TITLE: Importing Node.js Net Module
DESCRIPTION: This snippet shows the standard way to import the built-in Node.js `net` module using the `require` function with the `node:` prefix.
SOURCE: https://github.com/asana/node/blob/main/doc/api/net.md#_snippet_0

LANGUAGE: js
CODE:
```
const net = require('node:net');
```

----------------------------------------

TITLE: Basic HTTPS GET Request with https.request Node.js
DESCRIPTION: This snippet demonstrates how to make a basic HTTPS GET request to a secure web server using the `https.request()` method. It includes setting request options, handling the response event to log status and headers, processing incoming data chunks, and handling potential request errors. Finally, `req.end()` is called to finalize and send the request.
SOURCE: https://github.com/asana/node/blob/main/doc/api/https.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
const https = require('node:https');

const options = {
  hostname: 'encrypted.google.com',
  port: 443,
  path: '/',
  method: 'GET',
};

const req = https.request(options, (res) => {
  console.log('statusCode:', res.statusCode);
  console.log('headers:', res.headers);

  res.on('data', (d) => {
    process.stdout.write(d);
  });
});

req.on('error', (e) => {
  console.error(e);
});
req.end();
```

----------------------------------------

TITLE: Deriving Key with Async PBKDF2 (ESM)
DESCRIPTION: This snippet shows how to use the asynchronous `crypto.pbkdf2` function in Node.js with ES modules. It imports the function, specifies the password, salt, iteration count (100,000), desired key length (64 bytes), and digest algorithm (SHA-512). It provides a callback to handle the result or error, logging the derived key as a hexadecimal string. Requires Node.js version supporting `node:crypto` import.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_39

LANGUAGE: JavaScript
CODE:
```
const {
  pbkdf2,
} = await import('node:crypto');

pbkdf2('secret', 'salt', 100000, 64, 'sha512', (err, derivedKey) => {
  if (err) throw err;
  console.log(derivedKey.toString('hex'));  // '3745e48...08d59ae'
});
```

----------------------------------------

TITLE: Output of require.main from Node.js Entry Script (console)
DESCRIPTION: Displays the typical console output when printing `require.main` from an entry script. It shows the structure of the `Module` object, including properties like id, path, filename, loaded status, children, and paths.
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_15

LANGUAGE: console
CODE:
```
Module {
  id: '.',
  path: '/absolute/path/to',
  exports: {},
  filename: '/absolute/path/to/entry.js',
  loaded: false,
  children: [],
  paths:
   [ '/absolute/path/to/node_modules',
     '/absolute/path/node_modules',
     '/absolute/node_modules',
     '/node_modules' ] }
```

----------------------------------------

TITLE: Example Creating ESM Library Bash
DESCRIPTION: Provides steps to create a new directory, navigate into it, and then use `npm init` with the `esm` initializer (`create-esm`) and the `--yes` flag to quickly generate an `esm`-compatible package.json.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-init.md#_snippet_5

LANGUAGE: bash
CODE:
```
$ mkdir my-esm-lib && cd my-esm-lib
$ npm init esm --yes
```

----------------------------------------

TITLE: Aborting Asynchronous File Read Node.js
DESCRIPTION: Demonstrates how to use an `AbortSignal` with the `fs.readFile` options to cancel an ongoing asynchronous file read operation, allowing for resource management and responsive applications.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_59

LANGUAGE: mjs
CODE:
```
import { readFile } from 'node:fs';

const controller = new AbortController();
const signal = controller.signal;
readFile(fileInfo[0].name, { signal }, (err, buf) => {
  // ...
});
// When you want to abort the request
controller.abort();
```

----------------------------------------

TITLE: Example: Uninstalling a Package in Bash
DESCRIPTION: Demonstrates how to uninstall a specific package (`sax`) using the default behavior. This command will remove the package from `node_modules` and also update the project's `package.json` and lock files (`npm-shrinkwrap.json` or `package-lock.json`).
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-uninstall.md#_snippet_1

LANGUAGE: bash
CODE:
```
npm uninstall sax
```

----------------------------------------

TITLE: Creating Secure Node.js HTTP/2 Server (Core API)
DESCRIPTION: This example sets up a basic secure HTTP/2 server using `http2.createSecureServer`. It requires SSL certificate and key files to enable encrypted communication, listens for incoming streams, responds with HTML content, and starts the server on port 8443.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http2.md#_snippet_3

LANGUAGE: javascript
CODE:
```
const http2 = require('node:http2');
const fs = require('node:fs');

const server = http2.createSecureServer({
  key: fs.readFileSync('localhost-privkey.pem'),
  cert: fs.readFileSync('localhost-cert.pem'),
});
server.on('error', (err) => console.error(err));

server.on('stream', (stream, headers) => {
  // stream is a Duplex
  stream.respond({
    'content-type': 'text/html; charset=utf-8',
    ':status': 200,
  });
  stream.end('<h1>Hello World</h1>');
});

server.listen(8443);
```

----------------------------------------

TITLE: Using and Displaying Error Cause (JavaScript)
DESCRIPTION: Illustrates the `error.cause` property, which enables chaining errors by providing the underlying error that led to the current one. The example demonstrates how to set the `cause` via the constructor options and how Node.js's `util.inspect` displays the chained error.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
const cause = new Error('The remote HTTP server responded with a 500 status');
const symptom = new Error('The message failed to send', { cause });

console.log(symptom);
// Prints:
//   Error: The message failed to send
//       at REPL2:1:17
//       at Script.runInThisContext (node:vm:130:12)
//       ... 7 lines matching cause stack trace ...
//       at [_line] [as _line] (node:internal/readline/interface:886:18) {
//     [cause]: Error: The remote HTTP server responded with a 500 status
//         at REPL1:1:15
//         at Script.runInThisContext (node:vm:130:12)
//         at REPLServer.defaultEval (node:repl:574:29)
//         at bound (node:domain:426:15)
//         at REPLServer.runBound [as eval] (node:domain:437:12)
//         at REPLServer.onLine (node:repl:902:10)
//         at REPLServer.emit (node:events:549:35)
//         at REPLServer.emit (node:domain:482:12)
//         at [_onLine] [as _onLine] (node:internal/readline/interface:425:12)
//         at [_line] [as _line] (node:internal/readline/interface:886:18)
}
```

----------------------------------------

TITLE: Composing Async Iterables/Functions into Streams with stream.compose (Node.js)
DESCRIPTION: This snippet shows how `stream.compose` can convert various asynchronous constructs (AsyncIterable, AsyncGeneratorFunction, AsyncFunction) into Node.js stream components (readable, transform, writable) and then compose them together. It demonstrates chaining these composed streams and waiting for the final completion using `stream/promises.finished`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_61

LANGUAGE: javascript
CODE:
```
import { compose } from 'node:stream';
import { finished } from 'node:stream/promises';

// Convert AsyncIterable into readable Duplex.
const s1 = compose(async function*() {
  yield 'Hello';
  yield 'World';
}());

// Convert AsyncGenerator into transform Duplex.
const s2 = compose(async function*(source) {
  for await (const chunk of source) {
    yield String(chunk).toUpperCase();
  }
});

let res = '';

// Convert AsyncFunction into writable Duplex.
const s3 = compose(async function(source) {
  for await (const chunk of source) {
    res += chunk;
  }
});

await finished(compose(s1, s2, s3));

console.log(res); // prints 'HELLOWORLD'
```

----------------------------------------

TITLE: Correctly Ordering Async FS Operations with Promises (Node.js CJS)
DESCRIPTION: Demonstrates the correct approach to sequence asynchronous file system operations (`rename`, `stat`) using the promise-based API (`node:fs/promises`) and `async/await` within a CommonJS module using an immediately-invoked async function expression (IIFE). Ensures ordered execution. Requires the `node:fs/promises` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_88

LANGUAGE: cjs
CODE:
```
const { rename, stat } = require('node:fs/promises');

(async function(oldPath, newPath) {
  try {
    await rename(oldPath, newPath);
    const stats = await stat(newPath);
    console.log(`stats: ${JSON.stringify(stats)}`);
  } catch (error) {
    console.error('there was an error:', error.message);
  }
})('/tmp/hello', '/tmp/world');
```

----------------------------------------

TITLE: Scan and Fix Vulnerabilities - Bash
DESCRIPTION: Provides the basic command to scan the project for vulnerabilities and automatically install compatible updates to vulnerable dependencies. This is the most common use case for fixing found issues.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-audit.md#_snippet_4

LANGUAGE: Bash
CODE:
```
$ npm audit fix
```

----------------------------------------

TITLE: Creating & Reading ReadableStream (CJS)
DESCRIPTION: This snippet creates a ReadableStream that pushes performance.now() every second using setInterval from node:timers/promises. It then consumes the stream data using a for await...of loop wrapped in an async IIFE. Requires Node.js 16.5.0+ for the Web Streams API and Node.js 16.0.0+ for timers/promises.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_1

LANGUAGE: cjs
CODE:
```
const {
  ReadableStream,
} = require('node:stream/web');

const {
  setInterval: every,
} = require('node:timers/promises');

const {
  performance,
} = require('node:perf_hooks');

const SECOND = 1000;

const stream = new ReadableStream({
  async start(controller) {
    for await (const _ of every(SECOND))
      controller.enqueue(performance.now());
  },
});

(async () => {
  for await (const value of stream)
    console.log(value);
})();
```

----------------------------------------

TITLE: Writing Direct File Open Node.js (RECOMMENDED)
DESCRIPTION: This is the recommended approach for writing to a file without checking for existence first. It attempts to open the file using the 'wx' flag (write, exclusive) and handles the 'EEXIST' error directly if the file already exists, avoiding race conditions.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_44

LANGUAGE: javascript
CODE:
```
import { open, close } from 'node:fs';
open('myfile', 'wx', (err, fd) => {
  if (err) {
    if (err.code === 'EEXIST') {
      console.error('myfile already exists');
      return;
    }

    throw err;
  }

  try {
    writeMyData(fd);
  } finally {
    close(fd, (err) => {
      if (err) throw err;
    });
  }
});
```

----------------------------------------

TITLE: Using Dev Dependencies and Prepare Script - JSON
DESCRIPTION: Illustrates the use of `devDependencies` for packages needed during development or build processes (like compilers) but not by consumers of the package. It also shows the `prepare` script, which runs before publishing and in dev mode, ensuring build artifacts are ready.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_32

LANGUAGE: json
CODE:
```
{
  "name": "ethopia-waza",
  "description": "a delightfully fruity coffee varietal",
  "version": "1.2.3",
  "devDependencies": {
    "coffee-script": "~1.6.3"
  },
  "scripts": {
    "prepare": "coffee -o lib/ -c src/waza.coffee"
  },
  "main": "lib/waza.js"
}
```

----------------------------------------

TITLE: Making HTTP POST Request with Undici (JavaScript)
DESCRIPTION: Defines a simple asynchronous function `bankTransfer` that utilizes Undici's `request` method to send a POST request to a specified URL. The request includes custom headers and a JSON body containing recipient and amount details. It expects a JSON response.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/best-practices/mocking-request.md#_snippet_0

LANGUAGE: javascript
CODE:
```
// bank.mjs
import { request } from 'undici'

export async function bankTransfer(recipient, amount) {
  const { body } = await request('http://localhost:3000/bank-transfer',
    {
      method: 'POST',
      headers: {
        'X-TOKEN-SECRET': 'SuperSecretToken',
      },
      body: JSON.stringify({
        recipient,
        amount
      })
    }
  )
  return await body.json()
}
```

----------------------------------------

TITLE: Using Arrow Functions as Listeners and 'this' (CJS)
DESCRIPTION: Shows that using ES6 arrow functions as listeners causes the `this` keyword inside the listener to retain its surrounding context (often the global object or an empty object in CJS top-level), rather than referencing the EventEmitter instance using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_5

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('event', (a, b) => {
  console.log(a, b, this);
  // Prints: a b {}
});
myEmitter.emit('event', 'a', 'b');
```

----------------------------------------

TITLE: Mocking Node.js Function with Temporary Implementation using times Option
DESCRIPTION: This snippet demonstrates creating a mock function using `t.mock.fn()` and providing a temporary implementation (`addTwo`) that is used only for a specified number of times (`times: 2`) before automatically reverting to the original function's behavior (`addOne`).
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_34

LANGUAGE: javascript
CODE:
```
test('mocks a counting function', (t) => {
  let cnt = 0;

  function addOne() {
    cnt++;
    return cnt;
  }

  function addTwo() {
    cnt += 2;
    return cnt;
  }

  const fn = t.mock.fn(addOne, addTwo, { times: 2 });

  assert.strictEqual(fn(), 2);
  assert.strictEqual(fn(), 4);
  assert.strictEqual(fn(), 5);
  assert.strictEqual(fn(), 6);
});
```

----------------------------------------

TITLE: Writing Data to Writable Stream: Javascript
DESCRIPTION: Illustrates the basic pattern for writing data to a Node.js Writable stream using the `write()` method one or more times, followed by the `end()` method to signal that no more data will be written and the stream should be closed.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_12

LANGUAGE: javascript
CODE:
```
const myStream = getWritableStreamSomehow();
myStream.write('some data');
myStream.write('some more data');
myStream.end('done writing data');
```

----------------------------------------

TITLE: Creating HTTP Server with Request Listener (ESM, Node.js)
DESCRIPTION: Demonstrates creating a simple Node.js HTTP server using the `http.createServer()` method with an ESM `import` statement and a request listener function provided directly as an argument. The server listens on port 8000 and responds to all requests with a JSON object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_49

LANGUAGE: JavaScript
CODE:
```
import http from 'node:http';

// Create a local server to receive data from
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({
    data: 'Hello World!',
  }));
});

server.listen(8000);
```

----------------------------------------

TITLE: Installing glob npm Package
DESCRIPTION: Provides the command to install the `glob` library using the npm package manager. This is the first step to use the library in a Node.js project. Requires npm installed.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/glob/README.md#_snippet_0

LANGUAGE: shell
CODE:
```
npm i glob
```

----------------------------------------

TITLE: Specifying ISC License in package.json (Modern)
DESCRIPTION: Modern method for specifying the ISC license using its SPDX identifier. Replaces older object/array formats.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_8

LANGUAGE: JSON
CODE:
```
{
  "license": "ISC"
}
```

----------------------------------------

TITLE: Using Record<K, T> Utility Type in TypeScript
DESCRIPTION: This snippet illustrates the `Record<K, T>` utility type, which constructs a type with a set of properties `K` (a union type or literal keys) mapped to values of type `T`. It defines a `MemberPosition` union type and an `Employee` interface, then creates a `team` object typed as `Record<MemberPosition, Employee[]>`, ensuring keys for 'intern', 'developer', and 'tech-lead' are present and mapped to `Employee[]`. A commented-out example shows the type error when a key is missing.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/type-fest/readme.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
// Positions of employees in our company.
	type MemberPosition = 'intern' | 'developer' | 'tech-lead';

	// Interface describing properties of a single employee.
	interface Employee {
			firstName: string;
			lastName: string;
			yearsOfExperience: number;
	}

	// Create an object that has all possible `MemberPosition` values set as keys.
	// Those keys will store a collection of Employees of the same position.
	const team: Record<MemberPosition, Employee[]> = {
			intern: [],
			developer: [],
			'tech-lead': [],
	};

	// Our team has decided to help John with his dream of becoming Software Developer.
	team.intern.push({
		firstName: 'John',
		lastName: 'Doe',
		yearsOfExperience: 0
	});

	// `Record` forces you to initialize all of the property keys.
	// TypeScript Error: "tech-lead" property is missing
	const teamEmpty: Record<MemberPosition, null> = {
			intern: null,
			developer: null,
	};
```

----------------------------------------

TITLE: Checking if a value is an Error in JavaScript
DESCRIPTION: The `util.isError()` API is deprecated. A more robust check involves using `Object.prototype.toString` to get the internal class property or the `instanceof` operator to handle different types of errors. This ensures compatibility across various error types.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
Object.prototype.toString(arg) === '[object Error]' || arg instanceof Error
```

----------------------------------------

TITLE: Implementing Inheritance with ES6 Class Extends in Node.js
DESCRIPTION: Provides a modern alternative to `util.inherits` using ES6 `class` syntax with the `extends` keyword. It demonstrates how to create a class `MyStream` that inherits from `EventEmitter`, achieving the same event-emitting functionality in a more standard JavaScript pattern.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_8

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');

class MyStream extends EventEmitter {
  write(data) {
    this.emit('data', data);
  }
}

const stream = new MyStream();

stream.on('data', (data) => {
  console.log(`Received data: "${data}"`);
});
stream.write('With ES6');
```

----------------------------------------

TITLE: Importing CommonJS Default Export (JavaScript)
DESCRIPTION: Shows how to import the `module.exports` value from a CommonJS module into an ES module using both explicit `{ default as name }` syntax and the standard `import name from` syntax, confirming they are equivalent.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_10

LANGUAGE: javascript
CODE:
```
import { default as cjs } from 'cjs';

// The following import statement is "syntax sugar" (equivalent but sweeter)
// for `{ default as cjsSugar }` in the above import statement:
import cjsSugar from 'cjs';

console.log(cjs);
console.log(cjs === cjsSugar);
// Prints:
//   <module.exports>
//   true
```

----------------------------------------

TITLE: Installing Package - npm - Shell
DESCRIPTION: This command line snippet provides the standard method for installing the `libnpmdiff` package. Users should run this command in their project's terminal to add `libnpmdiff` as a dependency, making it available for use in their Node.js code. It requires npm to be installed and accessible in the environment.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/libnpmdiff/README.md#_snippet_2

LANGUAGE: shell
CODE:
```
$ npm install libnpmdiff
```

----------------------------------------

TITLE: Replacing createSecurePair with TLSSocket - JavaScript
DESCRIPTION: This snippet shows the recommended modern approach using the tls.TLSSocket() constructor, which replaces the functionality of tls.createSecurePair(). It takes a raw socket and options to create a secure TLS socket directly, simplifying the connection process.
SOURCE: https://github.com/asana/node/blob/main/doc/api/tls.md#_snippet_12

LANGUAGE: javascript
CODE:
```
secureSocket = tls.TLSSocket(socket, options);
```

----------------------------------------

TITLE: Piping Output Between Two spawned Processes
DESCRIPTION: Shows how to chain two processes (`ps ax` and `grep ssh`) by piping the standard output of the first process to the standard input of the second process, simulating a shell pipeline.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_13

LANGUAGE: javascript
CODE:
```
const { spawn } = require('node:child_process');
const ps = spawn('ps', ['ax']);
const grep = spawn('grep', ['ssh']);

ps.stdout.on('data', (data) => {
  grep.stdin.write(data);
});

ps.stderr.on('data', (data) => {
  console.error(`ps stderr: ${data}`);
});

ps.on('close', (code) => {
  if (code !== 0) {
    console.log(`ps process exited with code ${code}`);
  }
  grep.stdin.end();
});

grep.stdout.on('data', (data) => {
  console.log(data.toString());
});

grep.stderr.on('data', (data) => {
  console.error(`grep stderr: ${data}`);
});

grep.on('close', (code) => {
  if (code !== 0) {
    console.log(`grep process exited with code ${code}`);
  }
});
```

----------------------------------------

TITLE: Deriving Key with Sync PBKDF2 (CommonJS)
DESCRIPTION: This snippet demonstrates the usage of the synchronous `crypto.pbkdf2Sync` function in Node.js using CommonJS modules. It requires `node:crypto`, then calls `pbkdf2Sync` with the password, salt, iteration count (100,000), key length (64 bytes), and digest. The derived key is returned synchronously and logged as a hexadecimal string.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_42

LANGUAGE: JavaScript
CODE:
```
const {
  pbkdf2Sync,
} = require('node:crypto');

const key = pbkdf2Sync('secret', 'salt', 100000, 64, 'sha512');
console.log(key.toString('hex'));  // '3745e48...08d59ae'
```

----------------------------------------

TITLE: Safe File Read Handling Non-Existence using fs.open (Node.js)
DESCRIPTION: This snippet demonstrates the recommended method for reading from a file, which involves directly attempting to open the file for reading (`'r'` flag) and handling potential errors like 'ENOENT' if the file does not exist. This avoids the race condition associated with checking existence separately using `fs.access`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_34

LANGUAGE: mjs
CODE:
```
import { open, close } from 'node:fs';

open('myfile', 'r', (err, fd) => {
  if (err) {
    if (err.code === 'ENOENT') {
      console.error('myfile does not exist');
      return;
    }

    throw err;
  }

  try {
    readMyData(fd);
  } finally {
    close(fd, (err) => {
      if (err) throw err;
    });
  }
});
```

----------------------------------------

TITLE: Handling Synchronous Errors with Try/Catch (JavaScript)
DESCRIPTION: Demonstrates how to use a `try...catch` block in JavaScript to handle synchronous errors like a `ReferenceError` that occurs when accessing an undefined variable. The `catch` block intercepts the error, preventing the Node.js process from crashing.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
// Throws with a ReferenceError because z is not defined.
try {
  const m = 1;
  const n = m + z;
} catch (err) {
  // Handle the error here.
}
```

----------------------------------------

TITLE: Debugging with Resume on Start and Basic Commands (Console)
DESCRIPTION: Demonstrates running the Node.js debugger with `NODE_INSPECT_RESUME_ON_START=1` to break on the first `debugger` statement. It includes examples of the `next` command for stepping, the `repl` command for evaluating code in the script's context, and the `.exit` command to quit.
SOURCE: https://github.com/asana/node/blob/main/doc/api/debugger.md#_snippet_1

LANGUAGE: console
CODE:
```
$ cat myscript.js
// myscript.js
global.x = 5;
setTimeout(() => {
  debugger;
  console.log('world');
}, 1000);
console.log('hello');
$ NODE_INSPECT_RESUME_ON_START=1 node inspect myscript.js
< Debugger listening on ws://127.0.0.1:9229/f1ed133e-7876-495b-83ae-c32c6fc319c2
< For help, see: https://nodejs.org/en/docs/inspector
<
connecting to 127.0.0.1:9229 ... ok
< Debugger attached.
<
< hello
<
break in myscript.js:4
  2 global.x = 5;
  3 setTimeout(() => {
> 4   debugger;
  5   console.log('world');
  6 }, 1000);
debug> next
break in myscript.js:5
  3 setTimeout(() => {
  4   debugger;
> 5   console.log('world');
  6 }, 1000);
  7 console.log('hello');
debug> repl
Press Ctrl+C to leave debug repl
> x
5
> 2 + 2
4
debug> next
< world
<
break in myscript.js:6
  4   debugger;
  5   console.log('world');
> 6 }, 1000);
  7 console.log('hello');
  8
debug> .exit
$
```

----------------------------------------

TITLE: Deriving Key with PBKDF2 in JavaScript
DESCRIPTION: This asynchronous function `pbkdf2Key` derives a cryptographic key from a password and salt using the PBKDF2 algorithm with SHA-512 hash. It imports the password as 'raw' key material and uses `subtle.deriveKey` to derive an AES-GCM key with a specified length (default 256), iterations (default 1000), marked as extractable, and authorized for encrypt/decrypt.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_15

LANGUAGE: javascript
CODE:
```
async function pbkdf2Key(pass, salt, iterations = 1000, length = 256) {
  const ec = new TextEncoder();
  const keyMaterial = await subtle.importKey(
    'raw',
    ec.encode(pass),
    'PBKDF2',
    false,
    ['deriveKey']);
  const key = await subtle.deriveKey({
    name: 'PBKDF2',
    hash: 'SHA-512',
    salt: ec.encode(salt),
    iterations,
  }, keyMaterial, {
    name: 'AES-GCM',
    length,
  }, true, ['encrypt', 'decrypt']);
  return key;
}
```

----------------------------------------

TITLE: Decrypting Data Using Decipher Stream (CJS)
DESCRIPTION: This example illustrates using the Node.js `Decipher` class as a decryption stream in a CommonJS environment. It utilizes 'readable' and 'end' event handlers to gather and display the unencrypted data received through the stream. It depends on the `node:crypto` and `node:buffer` modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_7

LANGUAGE: Javascript
CODE:
```
const {
  scryptSync,
  createDecipheriv,
} = require('node:crypto');
const { Buffer } = require('node:buffer');

const algorithm = 'aes-192-cbc';
const password = 'Password used to generate key';
// Key length is dependent on the algorithm. In this case for aes192, it is
// 24 bytes (192 bits).
// Use the async `crypto.scrypt()` instead.
const key = scryptSync(password, 'salt', 24);
// The IV is usually passed along with the ciphertext.
const iv = Buffer.alloc(16, 0); // Initialization vector.

const decipher = createDecipheriv(algorithm, key, iv);

let decrypted = '';
decipher.on('readable', () => {
  let chunk;
  while (null !== (chunk = decipher.read())) {
    decrypted += chunk.toString('utf8');
  }
});
decipher.on('end', () => {
  console.log(decrypted);
  // Prints: some clear text data
});

// Encrypted with same algorithm, key and iv.
const encrypted =
  'e5f79c5915c02171eec6b212d5520d44480993d7d622a7c4c2da32f6efda0ffa';
decipher.write(encrypted, 'hex');
decipher.end();
```

----------------------------------------

TITLE: Basic npm Command Usage - Bash
DESCRIPTION: This snippet demonstrates the fundamental syntax for executing npm commands from the terminal. <command> represents a specific npm operation like install, publish, test, etc., followed by any necessary arguments or options.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
npm <command>
```

----------------------------------------

TITLE: Implementing _construct, _write, and _destroy for File Writable Stream (JavaScript)
DESCRIPTION: Provides a complete example of a custom Writable stream that writes data to a file. It demonstrates implementing `_construct` for asynchronous initialization (opening the file), `_write` for writing chunks to the file descriptor, and `_destroy` for cleaning up resources (closing the file descriptor). Requires `node:stream` and `node:fs`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_77

LANGUAGE: JavaScript
CODE:
```
const { Writable } = require('node:stream');
const fs = require('node:fs');

class WriteStream extends Writable {
  constructor(filename) {
    super();
    this.filename = filename;
    this.fd = null;
  }
  _construct(callback) {
    fs.open(this.filename, (err, fd) => {
      if (err) {
        callback(err);
      } else {
        this.fd = fd;
        callback();
      }
    });
  }
  _write(chunk, encoding, callback) {
    fs.write(this.fd, chunk, callback);
  }
  _destroy(err, callback) {
    if (this.fd) {
      fs.close(this.fd, (er) => callback(er || err));
    } else {
      callback(err);
    }
  }
}
```

----------------------------------------

TITLE: Representing Local Path Dependency - JSON
DESCRIPTION: Shows how a local path dependency is represented in the `dependencies` section of the `package.json` file after being added with `npm install --save`. The `file:` prefix indicates a local file system reference.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_31

LANGUAGE: json
CODE:
```
{
  "name": "baz",
  "dependencies": {
    "bar": "file:../foo/bar"
  }
}
```

----------------------------------------

TITLE: Importing Node.js URL Module (ESM)
DESCRIPTION: This snippet demonstrates how to import the built-in Node.js 'url' module using the ESM `import` statement with the `node:` protocol prefix. The `node:` prefix is a recommended practice for clarity and performance when importing built-in modules in ESM.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/document_with_esm_and_cjs_code_snippet.md#_snippet_0

LANGUAGE: mjs
CODE:
```
import 'node:url';
```

----------------------------------------

TITLE: Perform Asynchronous Random Fill JavaScript (Callback)
DESCRIPTION: Illustrates an asynchronous single-call crypto operation using `crypto.randomFill`. It fills a `Uint8Array` with cryptographically strong random bytes and invokes a Node.js-style callback function upon completion. The operation is typically deferred to the libuv threadpool. Requires the built-in Node.js `crypto` module.
SOURCE: https://github.com/asana/node/blob/main/src/crypto/README.md#_snippet_2

LANGUAGE: javascript
CODE:
```
// Example asynchronous single-call operation
const buf = new Uint8Array(10);
crypto.randomFill(buf, (err, buf) => {
  console.log(buf);
});
```

----------------------------------------

TITLE: HTTPS Request with Certificate and Public Key Pinning Node.js
DESCRIPTION: This advanced snippet shows how to implement certificate and public key pinning using the `checkServerIdentity` option. It defines a custom function that checks the server certificate's common name, verifies the public key fingerprint (HPKP style), and checks the exact certificate fingerprint. The function returns an error if the certificate does not match the pinned values, enhancing security by mitigating Man-in-the-Middle attacks.
SOURCE: https://github.com/asana/node/blob/main/doc/api/https.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
const tls = require('node:tls');
const https = require('node:https');
const crypto = require('node:crypto');

function sha256(s) {
  return crypto.createHash('sha256').update(s).digest('base64');
}
const options = {
  hostname: 'github.com',
  port: 443,
  path: '/',
  method: 'GET',
  checkServerIdentity: function(host, cert) {
    // Make sure the certificate is issued to the host we are connected to
    const err = tls.checkServerIdentity(host, cert);
    if (err) {
      return err;
    }

    // Pin the public key, similar to HPKP pin-sha256 pinning
    const pubkey256 = 'pL1+qb9HTMRZJmuC/bB/ZI9d302BYrrqiVuRyW+DGrU=';
    if (sha256(cert.pubkey) !== pubkey256) {
      const msg = 'Certificate verification error: ' +
        `The public key of '${cert.subject.CN}' ` +
        'does not match our pinned fingerprint';
      return new Error(msg);
    }

    // Pin the exact certificate, rather than the pub key
    const cert256 = '25:FE:39:32:D9:63:8C:8A:FC:A1:9A:29:87:' +
      'D8:3E:4C:1D:98:DB:71:E4:1A:48:03:98:EA:22:6A:BD:8B:93:16';
    if (cert.fingerprint256 !== cert256) {
      const msg = 'Certificate verification error: ' +
        `The certificate of '${cert.subject.CN}' ` +
        'does not match our pinned fingerprint';
      return new Error(msg);
    }

    // This loop is informational only.
    // Print the certificate and public key fingerprints of all certs in the
    // chain. Its common to pin the public key of the issuer on the public
    // internet, while pinning the public key of the service in sensitive
    // environments.
    let hash;
    let lastprint256;
    do {
      console.log('Subject Common Name:', cert.subject.CN);
      console.log('  Certificate SHA256 fingerprint:', cert.fingerprint256);

      hash = crypto.createHash('sha256');
      console.log('  Public key ping-sha256:', sha256(cert.pubkey));

      lastprint256 = cert.fingerprint256;
      cert = cert.issuerCertificate;
    } while (cert && cert.fingerprint256 !== lastprint256);

  },
};

options.agent = new https.Agent(options);
const req = https.request(options, (res) => {
  console.log('All OK. Server matched our pinned cert or public key');
  console.log('statusCode:', res.statusCode);
  // Print the HPKP values
  console.log('headers:', res.headers['public-key-pins']);

  res.on('data', (d) => {});
});

req.on('error', (e) => {
  console.error(e.message);
});
req.end();
```

----------------------------------------

TITLE: Identifying Deprecated Buffer Warnings with Node.js Flags Console
DESCRIPTION: Demonstrates how to use Node.js environment variables and command-line flags (--trace-warnings, --pending-deprecation) to reveal deprecation warnings, including the one for the Buffer() constructor, along with stack traces. Shows the console commands to set flags and run a script that uses the deprecated API.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_1

LANGUAGE: Console
CODE:
```
$ export NODE_OPTIONS='--trace-warnings --pending-deprecation'\n$ cat example.js\n'use strict';\nconst foo = new Buffer('foo');\n$ node example.js\n(node:7147) [DEP0005] DeprecationWarning: The Buffer() and new Buffer() constructors are not recommended for use due to security and usability concerns. Please use the new Buffer.alloc(), Buffer.allocUnsafe(), or Buffer.from() construction methods instead.\n    at showFlaggedDeprecation (buffer.js:127:13)\n    at new Buffer (buffer.js:148:3)\n    at Object.<anonymous> (/path/to/example.js:2:13)\n    [... more stack trace lines ...]
```

----------------------------------------

TITLE: Using timersPromises.setTimeout (ESM and CommonJS)
DESCRIPTION: Provides examples for using the promise-based `setTimeout` function. The ESM example uses `await`, while the CommonJS example uses `.then()` to handle the promise resolution after a specified delay.
SOURCE: https://github.com/asana/node/blob/main/doc/api/timers.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import {
  setTimeout,
} from 'timers/promises';

const res = await setTimeout(100, 'result');

console.log(res);  // Prints 'result'
```

LANGUAGE: JavaScript
CODE:
```
const {
  setTimeout,
} = require('node:timers/promises');

setTimeout(100, 'result').then((res) => {
  console.log(res);  // Prints 'result'
});
```

----------------------------------------

TITLE: Checking if a value is null or undefined in JavaScript
DESCRIPTION: The `util.isNullOrUndefined()` API is deprecated. To check if a value is either `null` or `undefined`, use the strict equality operator (`===`) combined with the logical OR (`||`). This explicitly checks for both specific values.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
arg === null || arg === undefined
```

----------------------------------------

TITLE: Generating HMAC Key in JavaScript
DESCRIPTION: This asynchronous function `generateHmacKey` creates an HMAC key using `subtle.generateKey`, allowing specification of the hash algorithm (defaulting to 'SHA-256'). The generated key is extractable and authorized for signing/verification.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_4

LANGUAGE: javascript
CODE:
```
const { subtle } = globalThis.crypto;

async function generateHmacKey(hash = 'SHA-256') {
  const key = await subtle.generateKey({
    name: 'HMAC',
    hash,
  }, true, ['sign', 'verify']);

  return key;
}
```

----------------------------------------

TITLE: Sending Object Message via subprocess.send Node.js Parent
DESCRIPTION: Demonstrates how a Node.js parent process uses `subprocess.send()` to send a JavaScript object message to a child process created with `child_process.fork()`. It also sets up a listener to receive messages back from the child.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_24

LANGUAGE: javascript
CODE:
```
const cp = require('node:child_process');
const n = cp.fork(`${__dirname}/sub.js`);

n.on('message', (m) => {
  console.log('PARENT got message:', m);
});

// Causes the child to print: CHILD got message: { hello: 'world' }
n.send({ hello: 'world' });
```

----------------------------------------

TITLE: Checking if a value is a Number using typeof in JavaScript
DESCRIPTION: The `util.isNumber()` API is deprecated. The standard way to check if a value is a number primitive in JavaScript is by using the `typeof` operator.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
typeof arg === 'number'
```

----------------------------------------

TITLE: Handling Async Callback Errors (JavaScript)
DESCRIPTION: Illustrates the common Node.js pattern for handling errors in asynchronous APIs that use callbacks. The error, if any, is passed as the first argument (`err`) to the callback function. Code should check if `err` is truthy and handle the error inside the callback.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
const fs = require('node:fs');
fs.readFile('a file that does not exist', (err, data) => {
  if (err) {
    console.error('There was an error reading the file!', err);
    return;
  }
  // Otherwise handle the data
});
```

----------------------------------------

TITLE: Performing HTTP POST Request - Node.js (ES Module)
DESCRIPTION: This snippet demonstrates how to perform an HTTP POST request using `http.request` in Node.js with ES module syntax. It shows how to define request options, prepare the request body, set appropriate headers like `Content-Type` and `Content-Length`, handle the server response (status, headers, body chunks), and manage potential request errors. It highlights the importance of calling `req.end()` to finalize the request.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_54

LANGUAGE: mjs
CODE:
```
import http from 'node:http';
import { Buffer } from 'node:buffer';

const postData = JSON.stringify({
  'msg': 'Hello World!',
});

const options = {
  hostname: 'www.google.com',
  port: 80,
  path: '/upload',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': Buffer.byteLength(postData),
  },
};

const req = http.request(options, (res) => {
  console.log(`STATUS: ${res.statusCode}`);
  console.log(`HEADERS: ${JSON.stringify(res.headers)}`);
  res.setEncoding('utf8');
  res.on('data', (chunk) => {
    console.log(`BODY: ${chunk}`);
  });
  res.on('end', () => {
    console.log('No more data in response.');
  });
});

req.on('error', (e) => {
  console.error(`problem with request: ${e.message}`);
});

// Write data to request body
req.write(postData);
req.end();
```

----------------------------------------

TITLE: Making HTTPS GET Request (Node.js)
DESCRIPTION: This snippet illustrates how to make a basic HTTPS GET request using `https.get()`. It fetches content from a URL, logs the status code and headers, writes the received data to standard output, and handles potential errors.
SOURCE: https://github.com/asana/node/blob/main/doc/api/https.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
const https = require('node:https');

https.get('https://encrypted.google.com/', (res) => {
  console.log('statusCode:', res.statusCode);
  console.log('headers:', res.headers);

  res.on('data', (d) => {
    process.stdout.write(d);
  });

}).on('error', (e) => {
  console.error(e);
});
```

----------------------------------------

TITLE: Consuming Stream with AsyncIterator - Node.js Streams - JavaScript
DESCRIPTION: Shows how to consume a readable stream using the `for await...of` loop, which utilizes the stream's `[Symbol.asyncIterator]` method. It demonstrates reading chunks, setting encoding, and notes that the stream is automatically destroyed upon exiting the loop (via break, return, throw, or full consumption).
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_41

LANGUAGE: JavaScript
CODE:
```
const fs = require('node:fs');

async function print(readable) {
  readable.setEncoding('utf8');
  let data = '';
  for await (const chunk of readable) {
    data += chunk;
  }
  console.log(data);
}

print(fs.createReadStream('file')).catch(console.error);
```

----------------------------------------

TITLE: Making Basic HTTP Request using Undici Fetch API
DESCRIPTION: Illustrates the basic usage of the undici.fetch API to make an HTTP request and parse the response body as JSON, mirroring the standard Web Fetch API. This method is supported on Node.js 16.8+.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/README.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { fetch } from 'undici'


const res = await fetch('https://example.com')
const json = await res.json()
console.log(json)
```

----------------------------------------

TITLE: Attaching AbortSignal to Node.js Read Stream (JavaScript)
DESCRIPTION: This snippet demonstrates how to attach an `AbortSignal` to a Node.js `fs.createReadStream`. Calling `abort()` on the `AbortController` associated with the signal will destroy the stream, cancelling the read operation.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_68

LANGUAGE: javascript
CODE:
```
const fs = require('node:fs');

const controller = new AbortController();
const read = addAbortSignal(
  controller.signal,
  fs.createReadStream(('object.json')),
);
// Later, abort the operation closing the stream
controller.abort();
```

----------------------------------------

TITLE: Executing Basic Commands with child_process.exec in Node.js
DESCRIPTION: Demonstrates basic usage of `child_process.exec` for executing shell commands, including handling file paths with spaces and escaping shell variables. It shows how to require the module and invoke the function with simple command strings.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
const { exec } = require('node:child_process');

exec('"/path/to/test file/test.sh" arg1 arg2');
// Double quotes are used so that the space in the path is not interpreted as
// a delimiter of multiple arguments.

exec('echo "The \\$HOME variable is $HOME"');
// The $HOME variable is escaped in the first instance, but not in the second.
```

----------------------------------------

TITLE: Defining Generator Function - Javascript
DESCRIPTION: Defines a simple generator function `idMaker` using the `function*` syntax and `yield` expressions to produce a sequence of values (1, 2, 3).
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/inspector/debugger/get-possible-breakpoints-main-expected.txt#_snippet_26

LANGUAGE: javascript
CODE:
```
function* idMaker() {
  yield 1;
  yield 2;
  yield 3;
}
```

----------------------------------------

TITLE: Checking if a value is a String using typeof in JavaScript
DESCRIPTION: The `util.isString()` API is deprecated. The standard way to check if a value is a string primitive in JavaScript is by using the `typeof` operator.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
typeof arg === 'string'
```

----------------------------------------

TITLE: Adding Event Listener with emitter.on (JS)
DESCRIPTION: Adds a listener function to the end of the listeners array for the specified event name. Multiple calls with the same event and listener add the listener multiple times. The method returns the emitter instance for chaining.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_33

LANGUAGE: javascript
CODE:
```
server.on('connection', (stream) => {
  console.log('someone connected!');
});
```

----------------------------------------

TITLE: Making Basic HTTP Request using Undici Request API
DESCRIPTION: Demonstrates how to perform a simple GET request using the undici.request API, accessing the status code, headers, trailers, and streaming the response body data asynchronously.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/README.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { request } from 'undici'

const {
  statusCode,
  headers,
  trailers,
  body
} = await request('http://localhost:3000/foo')

console.log('response received', statusCode)
console.log('headers', headers)

for await (const data of body) {
  console.log('data', data)
}

console.log('trailers', trailers)
```

----------------------------------------

TITLE: Cloning Source Repository - Shell
DESCRIPTION: This snippet provides the shell command to clone the upstream source repository from GitHub. It's used for updating the local copy of the base64 project source code based on the original aklomp/base64 repository.
SOURCE: https://github.com/asana/node/blob/main/deps/base64/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
$ git clone https://github.com/aklomp/base64
```

----------------------------------------

TITLE: Aborting child_process.exec with AbortSignal in Node.js
DESCRIPTION: Demonstrates how to use the `signal` option with `AbortSignal` to abort a running child process spawned by `child_process.exec`. Aborting the process will terminate it and cause the callback function to be invoked with an `AbortError`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
const { exec } = require('node:child_process');
const controller = new AbortController();
const { signal } = controller;

const child = exec('grep ssh', { signal }, (error) => {
  console.error(error); // an AbortError
});

controller.abort();
```

----------------------------------------

TITLE: Running npm restart Command Bash
DESCRIPTION: Shows the basic command-line syntax for using `npm restart`. This command attempts to stop and then start a package, or run a dedicated 'restart' script if defined in the package.json. Optional arguments can be passed after `--`.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-restart.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm restart [-- <args>]
```

----------------------------------------

TITLE: Using AbortSignal with stream constructor (JavaScript)
DESCRIPTION: Shows how to pass an `AbortSignal` to the constructor of `stream.Readable` or `stream.Writable`. Aborting the signal behaves like calling `.destroy()` on the stream with an `AbortError`.
SOURCE: https://github.com/asana/node/blob/main/doc/changelogs/CHANGELOG_V15.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const { Readable } = require('stream');
const controller = new AbortController();
const read = new Readable({
  read(size) {
    // ...
  },
  signal: controller.signal,
});
// Later, abort the operation closing the stream
controller.abort();
```

----------------------------------------

TITLE: Advancing Mocked Time Incrementally in Node.js Test (CJS)
DESCRIPTION: Shows how `context.mock.timers.tick()` can be called multiple times to incrementally advance the mocked time, eventually triggering a timer set for a longer duration. Requires `node:assert` and `node:test` modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_47

LANGUAGE: javascript
CODE:
```
const assert = require('node:assert');
const { test } = require('node:test');

test('mocks setTimeout to be executed synchronously without having to actually wait for it', (context) => {
  const fn = context.mock.fn();
  context.mock.timers.enable({ apis: ['setTimeout'] });
  const nineSecs = 9000;
  setTimeout(fn, nineSecs);

  const twoSeconds = 3000;
  context.mock.timers.tick(twoSeconds);
  context.mock.timers.tick(twoSeconds);
  context.mock.timers.tick(twoSeconds);

  assert.strictEqual(fn.mock.callCount(), 1);
});
```

----------------------------------------

TITLE: Pausing and Resuming Data Flow - Node.js Streams JavaScript
DESCRIPTION: Illustrates how to temporarily stop a stream in flowing mode from emitting 'data' events using `readable.pause()` and then resume the flow later using `readable.resume()`. Data will buffer internally while paused.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_30

LANGUAGE: JavaScript
CODE:
```
const readable = getReadableStreamSomehow();
readable.on('data', (chunk) => {
  console.log(`Received ${chunk.length} bytes of data.`);
  readable.pause();
  console.log('There will be no additional data for 1 second.');
  setTimeout(() => {
    console.log('Now data will start flowing again.');
    readable.resume();
  }, 1000);
});
```

----------------------------------------

TITLE: Creating Simple HTTP Server in Node.js
DESCRIPTION: This JavaScript code snippet creates a basic HTTP server using Node.js's built-in 'http' module. It listens on hostname '127.0.0.1' and port 3000, responding to all requests with a 200 status code, 'text/plain' content type, and the body 'Hello, World!'.
SOURCE: https://github.com/asana/node/blob/main/doc/api/synopsis.md#_snippet_3

LANGUAGE: javascript
CODE:
```
const http = require('node:http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

----------------------------------------

TITLE: Using stream.pipeline (Promises API) for File Compression (CJS)
DESCRIPTION: Demonstrates using the Promise-based `stream.pipeline` function to efficiently pipe data through multiple streams. This example chains a file read stream, a gzip compression stream, and a file write stream to compress a tar archive, handling errors and completion automatically.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const { pipeline } = require('node:stream/promises');
const fs = require('node:fs');
const zlib = require('node:zlib');

async function run() {
  await pipeline(
    fs.createReadStream('archive.tar'),
    zlib.createGzip(),
    fs.createWriteStream('archive.tar.gz'),
  );
  console.log('Pipeline succeeded.');
}

run().catch(console.error);
```

----------------------------------------

TITLE: Configuring Scoped Authentication Tokens in .npmrc
DESCRIPTION: Compares insecure unscoped authentication token configuration (`_authToken=`) with secure, recommended URI-scoped configurations. Scoping ensures credentials are only sent to the specified registry host or path by prefixing the credential key with `//hostname/:` or `//hostname/path/:`.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/npmrc.md#_snippet_3

LANGUAGE: bash
CODE:
```
; bad config
_authToken=MYTOKEN

; good config
@myorg:registry=https://somewhere-else.com/myorg
@another:registry=https://somewhere-else.com/another
//registry.npmjs.org/:_authToken=MYTOKEN
; would apply to both @myorg and @another
; //somewhere-else.com/:_authToken=MYTOKEN
; would apply only to @myorg
//somewhere-else.com/myorg/:_authToken=MYTOKEN1
; would apply only to @another
//somewhere-else.com/another/:_authToken=MYTOKEN2
```

----------------------------------------

TITLE: Dual Package Config with Separate ESM Export (JSON)
DESCRIPTION: Configures a dual Node.js package using conditional exports. It sets the default export (.) to the CommonJS version (./index.cjs) for backward compatibility and adds a separate named export ("./module") pointing to the ES module wrapper (./wrapper.mjs) for explicit ESM consumption. Requires "type": "module".
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_24

LANGUAGE: json
CODE:
```
// ./node_modules/pkg/package.json
{
  "type": "module",
  "exports": {
    ".": "./index.cjs",
    "./module": "./wrapper.mjs"
  }
}
```

----------------------------------------

TITLE: Resuming TLS Sessions Example Javascript
DESCRIPTION: Illustrates how to implement TLS session resumption using the `newSession` and `resumeSession` events of the `tls.Server`. It stores session data in a simple in-memory object (`tlsSessionStore`) mapped by session ID.
SOURCE: https://github.com/asana/node/blob/main/doc/api/tls.md#_snippet_7

LANGUAGE: javascript
CODE:
```
const tlsSessionStore = {};
server.on('newSession', (id, data, cb) => {
  tlsSessionStore[id.toString('hex')] = data;
  cb();
});
server.on('resumeSession', (id, cb) => {
  cb(null, tlsSessionStore[id.toString('hex')] || null);
});
```

----------------------------------------

TITLE: Getting Serialized WHATWG URL String (href)
DESCRIPTION: Shows how to retrieve the full serialized URL string from a WHATWG `URL` object using the `href` property.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
console.log(myURL.href);
```

----------------------------------------

TITLE: Adding One-Time Listeners and Prepending with emitter.once/prependOnceListener (CommonJS)
DESCRIPTION: Demonstrates adding a one-time listener with `once` and then prepending another one-time listener with `prependOnceListener` for the same event using CommonJS syntax. Both listeners are removed after the first emission, and the prepended one is executed first. The example requires `EventEmitter` from `node:events`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_38

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');
const myEE = new EventEmitter();
myEE.once('foo', () => console.log('a'));
myEE.prependOnceListener('foo', () => console.log('b'));
myEE.emit('foo');
```

----------------------------------------

TITLE: Setting Single Header Value with setHeader (Node.js JS)
DESCRIPTION: Shows how to set a single HTTP header like 'Content-Type' using the `response.setHeader` method. The first argument is the header name, and the second is the value.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_29

LANGUAGE: js
CODE:
```
response.setHeader('Content-Type', 'text/html');
```

----------------------------------------

TITLE: Creating HTTPS Server with Key and Certificate (Node.js)
DESCRIPTION: This snippet shows how to create a basic HTTPS server in Node.js using `https.createServer()`. It configures the server with separate key and certificate files and listens on port 8000, responding with 'hello world'.
SOURCE: https://github.com/asana/node/blob/main/doc/api/https.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
// curl -k https://localhost:8000/
const https = require('node:https');
const fs = require('node:fs');

const options = {
  key: fs.readFileSync('test/fixtures/keys/agent2-key.pem'),
  cert: fs.readFileSync('test/fixtures/keys/agent2-cert.pem'),
};

https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end('hello world\n');
}).listen(8000);
```

----------------------------------------

TITLE: Executing the npm command (Bash)
DESCRIPTION: This snippet shows the basic command to invoke the npm CLI. Running `npm` without arguments typically displays help information or lists available commands. It serves as the entry point for interacting with the package manager.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm
```

----------------------------------------

TITLE: Installing ansi-regex package using npm (Shell)
DESCRIPTION: This command installs the `ansi-regex` package from the npm registry. It is the standard way to add the library to your Node.js project as a dependency.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/ansi-regex/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
$ npm install ansi-regex
```

----------------------------------------

TITLE: Getting and Setting Node.js URL Protocol - JavaScript
DESCRIPTION: Demonstrates retrieving the protocol of a URL using `myURL.protocol` and changing it. Also includes examples showing the specific behavior when changing between special (`ftp`, `file`, `http`, `https`, `ws`, `wss`) and non-special protocols according to the WHATWG URL Standard.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_23

LANGUAGE: javascript
CODE:
```
const myURL = new URL('https://example.org');
console.log(myURL.protocol);
// Prints https:

myURL.protocol = 'ftp';
console.log(myURL.href);
// Prints ftp://example.org/
```

LANGUAGE: javascript
CODE:
```
const u = new URL('http://example.org');
u.protocol = 'https';
console.log(u.href);
// https://example.org/
```

LANGUAGE: javascript
CODE:
```
const u = new URL('http://example.org');
u.protocol = 'fish';
console.log(u.href);
// http://example.org/
```

LANGUAGE: javascript
CODE:
```
const u = new URL('fish://example.org');
u.protocol = 'http';
console.log(u.href);
// fish://example.org
```

----------------------------------------

TITLE: Adding and Customizing Version Option Commander.js
DESCRIPTION: Adds a standard version option to a Commander.js program using `.version()`. By default, this adds `-V, --version`. The code shows the basic usage and how to customize the flags and description using additional parameters. When these flags are used on the command line, the program prints the specified version string and exits. The console output shows using the default version flag.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/postject-copy/node_modules/commander/Readme.md#_snippet_22

LANGUAGE: javascript
CODE:
```
program.version('0.0.1');
```

LANGUAGE: console
CODE:
```
$ ./examples/pizza -V
0.0.1
```

LANGUAGE: javascript
CODE:
```
program.version('0.0.1', '-v, --vers', 'output the current version');
```

----------------------------------------

TITLE: Consuming stream.Readable with Async Iterator - Node.js
DESCRIPTION: Demonstrates consuming data from a Node.js Readable stream using the `for await...of` syntax, providing a modern and convenient way to asynchronously iterate over stream chunks.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_98

LANGUAGE: Javascript
CODE:
```
(async function() {
  for await (const chunk of readable) {
    console.log(chunk);
  }
})();
```

----------------------------------------

TITLE: Defining Various Test Types (Synchronous, Async, Promise, Callback) in Node.js
DESCRIPTION: Illustrates different ways to define tests using the `node:test` module: synchronous tests that pass/fail based on exceptions, asynchronous tests using `async`/`await`, tests returning a Promise, and callback-based tests using a `done` function.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_4

LANGUAGE: js
CODE:
```
test('synchronous passing test', (t) => {
  // This test passes because it does not throw an exception.
  assert.strictEqual(1, 1);
});

test('synchronous failing test', (t) => {
  // This test fails because it throws an exception.
  assert.strictEqual(1, 2);
});

test('asynchronous passing test', async (t) => {
  // This test passes because the Promise returned by the async
  // function is settled and not rejected.
  assert.strictEqual(1, 1);
});

test('asynchronous failing test', async (t) => {
  // This test fails because the Promise returned by the async
  // function is rejected.
  assert.strictEqual(1, 2);
});

test('failing test using Promises', (t) => {
  // Promises can be used directly as well.
  return new Promise((resolve, reject) => {
    setImmediate(() => {
      reject(new Error('this will cause the test to fail'));
    });
  });
});

test('callback passing test', (t, done) => {
  // done() is the callback function. When the setImmediate() runs, it invokes
  // done() with no arguments.
  setImmediate(done);
});

test('callback failing test', (t, done) => {
  // When the setImmediate() runs, done() is invoked with an Error object and
  // the test fails.
  setImmediate(() => {
    done(new Error('callback failure'));
  });
});
```

----------------------------------------

TITLE: Checking Main Module Entry Point (Node.js/JavaScript)
DESCRIPTION: Explains how to determine if the current CommonJS module is the main entry point of the process by comparing `require.main` with `module`, replacing a common use case of the deprecated `module.parent`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
if (require.main === module) {
  // Code section that will run only if current file is the entry point.
}
```

----------------------------------------

TITLE: Checking if a value is undefined in JavaScript
DESCRIPTION: The `util.isUndefined()` API is deprecated. The most straightforward way to check if a value is strictly `undefined` in JavaScript is using the strict equality operator (`===`).
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
arg === undefined
```

----------------------------------------

TITLE: Defining JavaScript Class with Native Implementation (C)
DESCRIPTION: Creates a JavaScript constructor function representing a class, often used to expose C++ classes to JavaScript. It takes the class name, a constructor callback (`napi_callback`) for instance creation, optional data, and an array of `napi_property_descriptor`s defining static and instance members. The returned `napi_value` represents the JavaScript constructor function.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_174

LANGUAGE: c
CODE:
```
napi_status napi_define_class(napi_env env,
                              const char* utf8name,
                              size_t length,
                              napi_callback constructor,
                              void* data,
                              size_t property_count,
                              const napi_property_descriptor* properties,
                              napi_value* result);
```

----------------------------------------

TITLE: Specifying Author as String in package.json
DESCRIPTION: Defines a package author using a single string that npm can parse into name, email, and URL.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_12

LANGUAGE: JSON
CODE:
```
{
  "author": "Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"
}
```

----------------------------------------

TITLE: Handling 'error' Events (CJS)
DESCRIPTION: Demonstrates the best practice of registering a listener for the 'error' event using `on()` to catch emitted errors gracefully, preventing the Node.js process from crashing using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_15

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('error', (err) => {
  console.error('whoops! there was an error');
});
myEmitter.emit('error', new Error('whoops!'));
// Prints: whoops! there was an error
```

----------------------------------------

TITLE: Basic EventEmitter Usage (CJS)
DESCRIPTION: Demonstrates the fundamental process of creating an EventEmitter instance, registering a listener for a specific event using `on()`, and triggering the event using `emit()`. This shows the basic event-driven flow using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
  console.log('an event occurred!');
});
myEmitter.emit('event');
```

----------------------------------------

TITLE: Using escape-string-regexp function - JavaScript
DESCRIPTION: This JavaScript snippet demonstrates how to use the imported `escapeStringRegexp` function. It takes a string containing characters that have special meaning in regular expressions (like `$` and `?`) and returns a new string where these characters are escaped. This escaped string can then be safely used as a literal pattern when creating a new `RegExp` object, preventing unintended behavior.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/@babel/code-frame/node_modules/escape-string-regexp/readme.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const escapeStringRegexp = require('escape-string-regexp');

const escapedString = escapeStringRegexp('how much $ for a unicorn?');
//=> 'how much \$ for a unicorn\?'

new RegExp(escapedString);
```

----------------------------------------

TITLE: Reading File Contents Asynchronously Node.js
DESCRIPTION: Demonstrates the basic asynchronous reading of a file's entire contents using `fs.readFile` with a callback function. It shows how to handle potential errors and access the file data (as a Buffer if no encoding is specified).
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_55

LANGUAGE: mjs
CODE:
```
import { readFile } from 'node:fs';

readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

----------------------------------------

TITLE: Installing npm Package by Tag (Bash)
DESCRIPTION: Demonstrates the command syntax for installing a specific version of a package by referencing its distribution tag (`<tag>`) instead of using a literal version number or range. This is the standard way to install versions aliased by tags like `beta` or `stable`.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-dist-tag.md#_snippet_1

LANGUAGE: Bash
CODE:
```
npm install <name>@<tag>
```

----------------------------------------

TITLE: Mocking Undici Request with Persistence - JavaScript
DESCRIPTION: Shows how to make a mock interceptor persistent using the `persist()` method, allowing it to match multiple incoming requests indefinitely. It sets up a persistent mock for GET requests to `/foo`, demonstrating that subsequent requests to the same URL will continue to match the mock.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/MockPool.md#_snippet_12

LANGUAGE: javascript
CODE:
```
import { MockAgent, setGlobalDispatcher, request } from 'undici'

const mockAgent = new MockAgent()
setGlobalDispatcher(mockAgent)

const mockPool = mockAgent.get('http://localhost:3000')

mockPool.intercept({
  path: '/foo',
  method: 'GET'
}).reply(200, 'foo').persist()

const result1 = await request('http://localhost:3000/foo')
// Will match and return mocked data

const result2 = await request('http://localhost:3000/foo')
// Will match and return mocked data

// Etc
```

----------------------------------------

TITLE: Iterating Directory Entries Node.js FS Async
DESCRIPTION: Demonstrates asynchronously opening a directory using `fsPromises.opendir` and iterating through its entries (`fs.Dirent`) using an `for await...of` loop. It logs the name of each entry and includes basic error handling. Requires Node.js >= 12.12.0 and uses the Promises API.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_79

LANGUAGE: javascript
CODE:
```
import { opendir } from 'node:fs/promises';

try {
  const dir = await opendir('./');
  for await (const dirent of dir)
    console.log(dirent.name);
} catch (err) {
  console.error(err);
}
```

----------------------------------------

TITLE: Processing HTTP Response Body as JSON using Undici Request API
DESCRIPTION: Shows how to use the .json() body mixin available on the response body object returned by undici.request to conveniently parse the HTTP response body directly into a JavaScript object. Note that body mixins consume the stream and can only be called once.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/README.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { request } from 'undici'

const {
  statusCode,
  headers,
  trailers,
  body
} = await request('http://localhost:3000/foo')

console.log('response received', statusCode)
console.log('headers', headers)
console.log('data', await body.json())
console.log('trailers', trailers)
```

----------------------------------------

TITLE: Configuring Travis CI Install with npm ci (YAML)
DESCRIPTION: Provides an example configuration snippet for a `.travis.yml` file. It demonstrates how to set the build's `install` step to use `npm ci` for dependency installation and includes settings to cache the npm directory to improve build performance.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-ci.md#_snippet_3

LANGUAGE: yaml
CODE:
```
# .travis.yml
install:
- npm ci
# keep the npm cache around to speed up installs
cache:
  directories:
  - "$HOME/.npm"
```

----------------------------------------

TITLE: Reading Direct File Open Node.js (RECOMMENDED)
DESCRIPTION: This is the recommended approach for reading a file. It attempts to open the file directly using the 'r' flag and handles the 'ENOENT' error if the file does not exist, which is safer than checking existence beforehand.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_46

LANGUAGE: javascript
CODE:
```
import { open, close } from 'node:fs';

open('myfile', 'r', (err, fd) => {
  if (err) {
    if (err.code === 'ENOENT') {
      console.error('myfile does not exist');
      return;
    }

    throw err;
  }

  try {
    readMyData(fd);
  } finally {
    close(fd, (err) => {
      if (err) throw err;
    });
  }
});
```

----------------------------------------

TITLE: Starting Node.js in Watch Mode (Bash)
DESCRIPTION: Demonstrates how to run a Node.js script (`index.js`) with the `--watch` flag enabled. This causes the process to automatically restart when changes are detected in the watched files, which by default include the entry point and any required or imported modules. Requires Node.js v16.19.0 or later.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cli.md#_snippet_11

LANGUAGE: bash
CODE:
```
node --watch index.js
```

----------------------------------------

TITLE: Declaring Scoped Dependency - package.json JSON
DESCRIPTION: Adds an entry to the `dependencies` object in `package.json`, specifying a scoped package (`@myorg/mypackage`) and its version range (`^1.3.0`). This tells npm to install this specific package when `npm install` is run without arguments.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/scope.md#_snippet_2

LANGUAGE: JSON
CODE:
```
"dependencies": {
  "@myorg/mypackage": "^1.3.0"
}
```

----------------------------------------

TITLE: Default 'error' Event Behavior (ESM)
DESCRIPTION: Shows the critical default behavior when an 'error' event is emitted on an EventEmitter without any registered listeners: the error is thrown, leading to a crash and process exit.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_12

LANGUAGE: javascript
CODE:
```
import { EventEmitter } from 'node:events';
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('whoops!'));
// Throws and crashes Node.js
```

----------------------------------------

TITLE: Handling Back-Pressure with Writable Stream Drain Event: Javascript
DESCRIPTION: Provides a function example showing how to write a large amount of data to a Writable stream while respecting back-pressure. It checks the return value of `write()` and pauses writing if it returns `false`, resuming only when the `'drain'` event is emitted.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_13

LANGUAGE: javascript
CODE:
```
// Write the data to the supplied writable stream one million times.
// Be attentive to back-pressure.
function writeOneMillionTimes(writer, data, encoding, callback) {
  let i = 1000000;
  write();
  function write() {
    let ok = true;
    do {
      i--;
      if (i === 0) {
        // Last time!
        writer.write(data, encoding, callback);
      } else {
        // See if we should continue, or wait.
        // Don't pass the callback, because we're not done yet.
        ok = writer.write(data, encoding);
      }
    } while (i > 0 && ok);
    if (i > 0) {
      // Had to stop early!
      // Write some more once it drains.
      writer.once('drain', write);
    }
  }
}
```

----------------------------------------

TITLE: Importing Module with ES Module Syntax
DESCRIPTION: This snippet demonstrates the ES Module `import` syntax to import a built-in Node.js module (`node:url`). This syntax is used in files with a `.mjs` extension or in Node.js projects configured for ES Modules via `package.json`.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/document_with_cjs_and_esm_code_snippet.md#_snippet_1

LANGUAGE: mjs
CODE:
```
import 'node:url';
```

----------------------------------------

TITLE: Setting a Header in Node.js Outgoing Message
DESCRIPTION: Shows how to set or replace a single header value on the outgoing message using `setHeader()`. Multiple headers with the same name can be set using an array of strings as the value. Requires an `http.OutgoingMessage` instance.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_45

LANGUAGE: javascript
CODE:
```
outgoingMessage.setHeader(name, value);
```

----------------------------------------

TITLE: Checking if a value is a Date using instanceof in JavaScript
DESCRIPTION: The `util.isDate()` API is deprecated. Use the `instanceof` operator to check if a value is an instance of the `Date` class instead. This is the standard and recommended way in modern JavaScript.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
arg instanceof Date
```

----------------------------------------

TITLE: Using Partial<T> Utility Type in TypeScript
DESCRIPTION: This snippet demonstrates the `Partial<T>` utility type in TypeScript. It defines an interface `NodeConfig` and a class `NodeAppBuilder` with a method `config` that accepts `Partial<NodeConfig>`, allowing callers to provide only a subset of the interface's properties. The example shows how to instantiate `NodeAppBuilder` and call `config` with just the `appName` property.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/type-fest/readme.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
interface NodeConfig {
			appName: string;
			port: number;
	}

	class NodeAppBuilder {
			private configuration: NodeConfig = {
					appName: 'NodeApp',
					port: 3000
			};

			private updateConfig<Key extends keyof NodeConfig>(key: Key, value: NodeConfig[Key]) {
					this.configuration[key] = value;
			}

			config(config: Partial<NodeConfig>) {
					type NodeConfigKey = keyof NodeConfig;

					for (const key of Object.keys(config) as NodeConfigKey[]) {
							const updateValue = config[key];

							if (updateValue === undefined) {
									continue;
							}

							this.updateConfig(key, updateValue);
					}

					return this;
			}
	}

	// `Partial<NodeConfig>`` allows us to provide only a part of the
	// NodeConfig interface.
	new NodeAppBuilder().config({appName: 'ToDoApp'});
```

----------------------------------------

TITLE: Installing yocto-queue Package with npm (Shell)
DESCRIPTION: Provides the command-line instruction to install the yocto-queue package from the npm registry. This is the standard method for adding Node.js packages to a project. Requires npm and Node.js to be installed.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/yocto-queue/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
$ npm install yocto-queue
```

----------------------------------------

TITLE: Using Required<T> Utility Type in TypeScript
DESCRIPTION: This snippet illustrates the `Required<T>` utility type. It defines a `ContactForm` interface with optional properties. The `submitContactForm` function is typed to accept `Required<ContactForm>`, ensuring that both `email` and `message` properties must be provided when calling the function. The example shows a valid call and a commented-out call that would trigger a TypeScript error due to a missing required property.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/type-fest/readme.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
interface ContactForm {
			email?: string;
			message?: string;
	}

	function submitContactForm(formData: Required<ContactForm>) {
			// Send the form data to the server.
	}

	submitContactForm({
			email: 'ex@mple.com',
			message: 'Hi! Could you tell me more about…',
	});

	// TypeScript error: missing property 'message'
	submitContactForm({
			email: 'ex@mple.com',
	});
```

----------------------------------------

TITLE: Printing Stack Trace using console.trace in Node.js
DESCRIPTION: Demonstrates using `console.trace` to print the string 'Trace: ' followed by a formatted message and a stack trace to standard error, indicating the call sequence leading to the trace invocation.
SOURCE: https://github.com/asana/node/blob/main/doc/api/console.md#_snippet_4

LANGUAGE: javascript
CODE:
```
console.trace('Show me');
// Prints: (stack trace will vary based on where trace is called)
// Trace: Show me
// at repl:2:9
// at REPLServer.defaultEval (repl.js:248:27)
// at bound (domain.js:287:14)
// at REPLServer.runBound [as eval] (domain.js:300:12)
// at REPLServer.<anonymous> (repl.js:412:12)
// at emitOne (events.js:82:20)
// at REPLServer.emit (events.js:169:7)
// at REPLServer.Interface._onLine (readline.js:210:10)
// at REPLServer.Interface._line (readline.js:549:8)
// at REPLServer.Interface._ttyWrite (readline.js:826:14)
```

----------------------------------------

TITLE: Publishing NPM Package to Registry (Bash)
DESCRIPTION: Demonstrates the `npm publish` command, which uploads the package located in the current directory (or specified path/tarball) to the npm registry. By default, most files in the directory are included unless excluded via `.npmignore` or specified in the `files` array in `package.json`.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/developers.md#_snippet_6

LANGUAGE: bash
CODE:
```
npm publish
```

----------------------------------------

TITLE: Piping Async Iterator to stream.Writable with pipeline - Node.js
DESCRIPTION: Demonstrates using the `stream.pipeline` utility (both callback and promise versions) to pipe data from an async iterator to a Node.js Writable stream. `pipeline` correctly handles backpressure, errors, and stream cleanup.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_100

LANGUAGE: Javascript
CODE:
```
const fs = require('node:fs');
const { pipeline } = require('node:stream');
const { pipeline: pipelinePromise } = require('node:stream/promises');

const writable = fs.createWriteStream('./file');

const ac = new AbortController();
const signal = ac.signal;

const iterator = createIterator({ signal });

// Callback Pattern
pipeline(iterator, writable, (err, value) => {
  if (err) {
    console.error(err);
  } else {
    console.log(value, 'value returned');
  }
}).on('close', () => {
  ac.abort();
});
```

LANGUAGE: Javascript
CODE:
```
const fs = require('node:fs');
const { pipeline } = require('node:stream');
const { pipeline: pipelinePromise } = require('node:stream/promises');

const writable = fs.createWriteStream('./file');

const ac = new AbortController();
const signal = ac.signal;

const iterator = createIterator({ signal });

// Promise Pattern
pipelinePromise(iterator, writable)
  .then((value) => {
    console.log(value, 'value returned');
  })
  .catch((err) => {
    console.error(err);
    ac.abort();
  });
```

----------------------------------------

TITLE: Marking Peer Dependencies as Optional - JSON
DESCRIPTION: Shows how to use the `peerDependenciesMeta` field to provide additional information about peer dependencies, specifically marking one as optional. This prevents npm from emitting a warning if the optional peer dependency is not installed on the host system.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_34

LANGUAGE: json
CODE:
```
{
  "name": "tea-latte",
  "version": "1.3.5",
  "peerDependencies": {
    "tea": "2.x",
    "soy-milk": "1.2"
  },
  "peerDependenciesMeta": {
    "soy-milk": {
      "optional": true
    }
  }
}
```

----------------------------------------

TITLE: Overriding Module Type with .mjs and .cjs Extensions (JavaScript)
DESCRIPTION: Demonstrates how using `.cjs` or `.mjs` extensions forces a specific module type (CommonJS or ES module, respectively), regardless of the parent package's `"type"` field. This allows mixing module types within a single package.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import './legacy-file.cjs';
// Loaded as CommonJS since .cjs is always loaded as CommonJS.

import 'commonjs-package/src/index.mjs';
// Loaded as ES module since .mjs is always loaded as ES module.
```

----------------------------------------

TITLE: Chaining Streams with Pipe - Node.js Streams JavaScript
DESCRIPTION: Illustrates how to chain multiple streams together using the return value of `pipe()`. This example pipes data from a file read stream through a gzip transform stream and then to a compressed file write stream, effectively compressing the file content. Requires the 'node:fs' and 'node:zlib' modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_32

LANGUAGE: JavaScript
CODE:
```
const fs = require('node:fs');
const zlib = require('node:zlib');
const r = fs.createReadStream('file.txt');
const z = zlib.createGzip();
const w = fs.createWriteStream('file.txt.gz');
r.pipe(z).pipe(w);
```

----------------------------------------

TITLE: Importing node:url Module (ESM)
DESCRIPTION: Demonstrates how to import the built-in `node:url` module using ECMAScript module syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import url from 'node:url';
```

----------------------------------------

TITLE: Installing multiple packages with versions - Bash
DESCRIPTION: Shows how to install multiple packages in a single `npm install` command, including specifying a semver range for a specific package.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-install.md#_snippet_5

LANGUAGE: bash
CODE:
```
npm install sax@">=0.1.0 <0.2.0" bench supervisor
```

----------------------------------------

TITLE: Synopsis of npm run-script Command
DESCRIPTION: Provides the basic syntax for using the npm run-script command, which allows executing scripts defined in a package's package.json file. If no command is specified, it lists available scripts. Includes common aliases for the command.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-run-script.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run-script <command> [-- <args>]

aliases: run, rum, urn
```

----------------------------------------

TITLE: Creating & Reading ReadableStream (ESM)
DESCRIPTION: This snippet creates a ReadableStream that pushes performance.now() every second using setInterval from node:timers/promises. It then consumes the stream data using a for await...of loop. Requires Node.js 16.5.0+ for the Web Streams API and Node.js 16.0.0+ for timers/promises.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_0

LANGUAGE: mjs
CODE:
```
import {
  ReadableStream,
} from 'node:stream/web';

import {
  setInterval as every,
} from 'node:timers/promises';

import {
  performance,
} from 'node:perf_hooks';

const SECOND = 1000;

const stream = new ReadableStream({
  async start(controller) {
    for await (const _ of every(SECOND))
      controller.enqueue(performance.now());
  },
});

for await (const value of stream)
  console.log(value);
```

----------------------------------------

TITLE: Using Node.js Readline Promises API - JavaScript
DESCRIPTION: Demonstrates how to use the new promise-based readline API in Node.js. It sets up an interface to read from standard input and write to standard output, asks a question, waits for the answer using await, logs the response, and then closes the interface. This requires Node.js v17.0.0 or later.
SOURCE: https://github.com/asana/node/blob/main/doc/changelogs/CHANGELOG_V17.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import * as readline from 'node:readline/promises';
import { stdin as input, stdout as output } from 'process';

const rl = readline.createInterface({ input, output });

const answer = await rl.question('What do you think of Node.js? ');

console.log(`Thank you for your valuable feedback: ${answer}`);

rl.close();
```

----------------------------------------

TITLE: Creating HTTP Server using 'request' Event (CJS, Node.js)
DESCRIPTION: Illustrates creating a Node.js HTTP server using the `http.createServer()` method without a direct listener in CJS. The request handling logic is attached separately by listening to the `'request'` event emitted by the server instance. The server listens on port 8000 and responds with JSON.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_52

LANGUAGE: JavaScript
CODE:
```
const http = require('node:http');

// Create a local server to receive data from
const server = http.createServer();

// Listen to the request event
server.on('request', (request, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({
    data: 'Hello World!',
  }));
});

server.listen(8000);
```

----------------------------------------

TITLE: Importing Timers Promises API (ESM and CommonJS)
DESCRIPTION: Demonstrates how to import the promise-based timer functions (`setTimeout`, `setImmediate`, `setInterval`) from the `node:timers/promises` module using both ECMAScript Modules (`import`) and CommonJS (`require`) syntax in Node.js.
SOURCE: https://github.com/asana/node/blob/main/doc/api/timers.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import {
  setTimeout,
  setImmediate,
  setInterval,
} from 'timers/promises';
```

LANGUAGE: JavaScript
CODE:
```
const {
  setTimeout,
  setImmediate,
  setInterval,
} = require('node:timers/promises');
```

----------------------------------------

TITLE: Handling Port Closure with Node.js MessagePort Close Event
DESCRIPTION: This example demonstrates listening for the `'close'` event on a `MessagePort`. It sets up a channel, attaches listeners for both 'message' and 'close' events to one port (`port2`), sends a message from the other port (`port1`), and then explicitly closes the sending port to trigger the 'close' event on the receiving port. Requires `node:worker_threads`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_12

LANGUAGE: js
CODE:
```
const { MessageChannel } = require('node:worker_threads');
const { port1, port2 } = new MessageChannel();

// Prints:
//   foobar
//   closed!
port2.on('message', (message) => console.log(message));
port2.on('close', () => console.log('closed!'));

port1.postMessage('foobar');
port1.close();
```

----------------------------------------

TITLE: Implementing Node.js C++ Binding with V8 Fast API
DESCRIPTION: A comprehensive example of creating a Node.js C++ binding module that exposes a `divide` function with both a slow path (`SlowDivide`) and a fast path (`FastDivide`). It includes the necessary V8 API calls, fallback logic, module initialization (`Initialize`), and registration of external references (`RegisterExternalReferences`) required for V8 snapshots.
SOURCE: https://github.com/asana/node/blob/main/doc/contributing/adding-v8-fast-api.md#_snippet_2

LANGUAGE: C++
CODE:
```
#include "v8-fast-api-calls.h"

namespace node {
namespace custom_namespace {

static void SlowDivide(const FunctionCallbackInfo<Value>& args) {
  Environment* env = Environment::GetCurrent(args);
  CHECK_GE(args.Length(), 2);
  CHECK(args[0]->IsInt32());
  CHECK(args[1]->IsInt32());
  auto a = args[0].As<v8::Int32>();
  auto b = args[1].As<v8::Int32>();

  if (b->Value() == 0) {
    return node::THROW_ERR_INVALID_STATE(env, "Error");
  }

  double result = a->Value() / b->Value();
  args.GetReturnValue().Set(v8::Number::New(env->isolate(), result));
}

static double FastDivide(const int32_t a,
                         const int32_t b,
                         v8::FastApiCallbackOptions& options) {
  if (b == 0) {
    options.fallback = true;
    return 0;
  } else {
    return a / b;
  }
}

CFunction fast_divide_(CFunction::Make(FastDivide));

static void Initialize(Local<Object> target,
                       Local<Value> unused,
                       Local<Context> context,
                       void* priv) {
  SetFastMethod(context, target, "divide", SlowDivide, &fast_divide_);
}

void RegisterExternalReferences(ExternalReferenceRegistry* registry) {
  registry->Register(SlowDivide);
  registry->Register(FastDivide);
  registry->Register(fast_divide_.GetTypeInfo());
}

} // namespace custom_namespace
} // namespace node

NODE_BINDING_CONTEXT_AWARE_INTERNAL(custom_namespace,
                                    node::custom_namespace::Initialize);
NODE_BINDING_EXTERNAL_REFERENCE(
                      custom_namespace,
                      node::custom_namespace::RegisterExternalReferences);
```

----------------------------------------

TITLE: Creating & Using Keep-Alive Agent (Node.js HTTP ESM)
DESCRIPTION: This example demonstrates how to create a custom `http.Agent` instance with `keepAlive` enabled using ES Module syntax. The custom agent is then assigned to the `agent` option when making an HTTP request, allowing the agent to manage socket persistence and reuse for requests using this agent.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import { Agent, request } from 'node:http';
const keepAliveAgent = new Agent({ keepAlive: true });
options.agent = keepAliveAgent;
request(options, onResponseCallback);
```

----------------------------------------

TITLE: Quick Start Subcommand and Action Handler JavaScript
DESCRIPTION: Illustrates creating a multi-command CLI application using Commander.js. It defines a main program, adds a subcommand (`split`) with its own arguments and options, and provides an action handler function to execute the subcommand logic. Uses `Command` class for better structure.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/postject-copy/node_modules/commander/Readme.md#_snippet_4

LANGUAGE: js
CODE:
```
const { Command } = require('commander');
const program = new Command();

program
  .name('string-util')
  .description('CLI to some JavaScript string utilities')
  .version('0.8.0');

program.command('split')
  .description('Split a string into substrings and display as an array')
  .argument('<string>', 'string to split')
  .option('--first', 'display just the first substring')
  .option('-s, --separator <char>', 'separator character', ',')
  .action((str, options) => {
    const limit = options.first ? 1 : undefined;
    console.log(str.split(options.separator, limit));
  });

program.parse();
```

----------------------------------------

TITLE: Generating RSA Key Pair in JavaScript
DESCRIPTION: This asynchronous function `generateRsaKey` generates an RSASSA-PKCS1-v1_5 public and private key pair using `subtle.generateKey`. It allows specifying the modulus length (default 2048) and the hash algorithm (default SHA-256), using a fixed public exponent. The keys are extractable and intended for signing/verification.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_5

LANGUAGE: javascript
CODE:
```
const { subtle } = globalThis.crypto;
const publicExponent = new Uint8Array([1, 0, 1]);

async function generateRsaKey(modulusLength = 2048, hash = 'SHA-256') {
  const {
    publicKey,
    privateKey,
  } = await subtle.generateKey({
    name: 'RSASSA-PKCS1-v1_5',
    modulusLength,
    publicExponent,
    hash,
  }, true, ['sign', 'verify']);

  return { publicKey, privateKey };
}
```

----------------------------------------

TITLE: Checking for RegExp Instances in Node.js util.types (JavaScript)
DESCRIPTION: This snippet shows the usage of `util.types.isRegExp()` to check if a value is a regular expression object. It includes examples with both a regex literal and a RegExp object created with `new RegExp()`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_68

LANGUAGE: javascript
CODE:
```
util.types.isRegExp(/abc/);  // Returns true
util.types.isRegExp(new RegExp('abc'));  // Returns true
```

----------------------------------------

TITLE: Running Only Selected Tests/Subtests with --test-only and runOnly() in Node.js
DESCRIPTION: Explains and demonstrates how to use the `--test-only` command-line flag and the test context's `t.runOnly()` method to execute only tests or subtests marked with the `only: true` option, skipping others.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_10

LANGUAGE: js
CODE:
```
// Assume Node.js is run with the --test-only command-line option.
// The 'only' option is set, so this test is run.
test('this test is run', { only: true }, async (t) => {
  // Within this test, all subtests are run by default.
  await t.test('running subtest');

  // The test context can be updated to run subtests with the 'only' option.
  t.runOnly(true);
  await t.test('this subtest is now skipped');
  await t.test('this subtest is run', { only: true });

  // Switch the context back to execute all tests.
  t.runOnly(false);
  await t.test('this subtest is now run');

  // Explicitly do not run these tests.
  await t.test('skipped subtest 3', { only: false });
  await t.test('skipped subtest 4', { skip: true });
});

// The 'only' option is not set, so this test is skipped.
test('this test is not run', () => {
  // This code is not run.
  throw new Error('fail');
});
```

----------------------------------------

TITLE: Extending Node.js Writable Stream Class (JavaScript)
DESCRIPTION: This code demonstrates the basic structure for creating a custom writable stream by extending the `stream.Writable` class. It shows calling the parent constructor and highlights the need to implement required methods like `_write()` or `_writev()`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_71

LANGUAGE: javascript
CODE:
```
const { Writable } = require('node:stream');

class MyWritable extends Writable {
  constructor({ highWaterMark, ...options }) {
    super({ highWaterMark });
    // ...
  }
}
```

----------------------------------------

TITLE: Aborting readFile with AbortController (Node.js mjs)
DESCRIPTION: Demonstrates how to use an `AbortController` and `AbortSignal` to cancel an ongoing `fsPromises.readFile` operation. Shows creating a controller, passing its signal in options, and calling `controller.abort()`. Explains that an aborted request rejects the promise with an `AbortError`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_27

LANGUAGE: javascript
CODE:
```
import { readFile } from 'node:fs/promises';

try {
  const controller = new AbortController();
  const { signal } = controller;
  const promise = readFile(fileName, { signal });

  // Abort the request before the promise settles.
  controller.abort();

  await promise;
} catch (err) {
  // When a request is aborted - err is an AbortError
  console.error(err);
}
```

----------------------------------------

TITLE: Using deepEqual with Objects - Node.js Assert
DESCRIPTION: These snippets illustrate `assert.deepEqual`'s behavior when comparing objects. They show that nested enumerable "own" properties are compared recursively (`obj1` vs `obj2`, `obj1` vs `obj3`), but prototypes (`__proto__`) are ignored during the comparison, leading to assertions passing or failing based only on the own properties.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_21

LANGUAGE: mjs
CODE:
```
import assert from 'node:assert';

const obj1 = {
  a: {
    b: 1,
  },
};
const obj2 = {
  a: {
    b: 2,
  },
};
const obj3 = {
  a: {
    b: 1,
  },
};
const obj4 = { __proto__: obj1 };

assert.deepEqual(obj1, obj1);
// OK

// Values of b are different:
assert.deepEqual(obj1, obj2);
// AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }

assert.deepEqual(obj1, obj3);
// OK

// Prototypes are ignored:
assert.deepEqual(obj1, obj4);
// AssertionError: { a: { b: 1 } } deepEqual {}
```

LANGUAGE: cjs
CODE:
```
const assert = require('node:assert');

const obj1 = {
  a: {
    b: 1,
  },
};
const obj2 = {
  a: {
    b: 2,
  },
};
const obj3 = {
  a: {
    b: 1,
  },
};
const obj4 = { __proto__: obj1 };

assert.deepEqual(obj1, obj1);
// OK

// Values of b are different:
assert.deepEqual(obj1, obj2);
// AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }

assert.deepEqual(obj1, obj3);
// OK

// Prototypes are ignored:
assert.deepEqual(obj1, obj4);
// AssertionError: { a: { b: 1 } } deepEqual {}
```

----------------------------------------

TITLE: Importing Module Exporting Class and Instantiating in Node.js
DESCRIPTION: This snippet shows how to import a CommonJS module ('./square.js') that exports a class using `require()`. It then demonstrates creating a new instance of the imported class and calling a method (`area`) on that instance.
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
const Square = require('./square.js');
const mySquare = new Square(2);
console.log(`The area of mySquare is ${mySquare.area()}`);
```

----------------------------------------

TITLE: Mocking and Ticking setTimeout - ES Modules
DESCRIPTION: This snippet demonstrates how to enable mocking for `setTimeout` (from global, `node:timers`, and `node:timers/promises`) and advance the mocked time using `context.mock.timers.tick` to synchronously trigger the timers within a Node.js test.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_52

LANGUAGE: javascript
CODE:
```
import assert from 'node:assert';
import { test } from 'node:test';
import nodeTimers from 'node:timers';
import nodeTimersPromises from 'node:timers/promises';

test('mocks setTimeout to be executed synchronously without having to actually wait for it', async (context) => {
  const globalTimeoutObjectSpy = context.mock.fn();
  const nodeTimerSpy = context.mock.fn();
  const nodeTimerPromiseSpy = context.mock.fn();

  // Optionally choose what to mock
  context.mock.timers.enable({ apis: ['setTimeout'] });
  setTimeout(globalTimeoutObjectSpy, 9999);
  nodeTimers.setTimeout(nodeTimerSpy, 9999);

  const promise = nodeTimersPromises.setTimeout(9999).then(nodeTimerPromiseSpy);

  // Advance in time
  context.mock.timers.tick(9999);
  assert.strictEqual(globalTimeoutObjectSpy.mock.callCount(), 1);
  assert.strictEqual(nodeTimerSpy.mock.callCount(), 1);
  await promise;
  assert.strictEqual(nodeTimerPromiseSpy.mock.callCount(), 1);
});
```

----------------------------------------

TITLE: Generating and Signing with HMAC Key in JavaScript
DESCRIPTION: This asynchronous function demonstrates generating an HMAC key using `subtle.generateKey` and then using the generated key to sign a text message encoded as a `Uint8Array` using `subtle.sign`. It requires the `globalThis.crypto` object to be available.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const { subtle } = globalThis.crypto;

(async function() {

  const key = await subtle.generateKey({
    name: 'HMAC',
    hash: 'SHA-256',
    length: 256,
  }, true, ['sign', 'verify']);

  const enc = new TextEncoder();
  const message = enc.encode('I love cupcakes');

  const digest = await subtle.sign({
    name: 'HMAC',
  }, key, message);

})();
```

----------------------------------------

TITLE: Creating Error with Message in JavaScript (No Preview)
DESCRIPTION: Demonstrates creating an Error object initialized with a specific message string 'abc'. The evaluation result includes the message in the object's description field, confirming the message was captured.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/inspector/runtime/remote-object-expected.txt#_snippet_40

LANGUAGE: javascript
CODE:
```
new Error('abc')
```

----------------------------------------

TITLE: Decrypting Data Using Piped Streams (CJS)
DESCRIPTION: This example shows how to decrypt data from a file and write it to another file by piping a readable stream through a Node.js `Decipher` instance in a CommonJS setting. This is suitable for handling large files without loading the entire content into memory. It depends on `node:fs`, `node:buffer`, and `node:crypto`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_9

LANGUAGE: Javascript
CODE:
```
const {
  createReadStream,
  createWriteStream,
} = require('node:fs');
const {
  scryptSync,
  createDecipheriv,
} = require('node:crypto');
const { Buffer } = require('node:buffer');

const algorithm = 'aes-192-cbc';
const password = 'Password used to generate key';
// Use the async `crypto.scrypt()` instead.
const key = scryptSync(password, 'salt', 24);
// The IV is usually passed along with the ciphertext.
const iv = Buffer.alloc(16, 0); // Initialization vector.

const decipher = createDecipheriv(algorithm, key, iv);

const input = createReadStream('test.enc');
const output = createWriteStream('test.js');

input.pipe(decipher).pipe(output);
```

----------------------------------------

TITLE: Using AbortSignal with Writable Stream (JavaScript)
DESCRIPTION: Illustrates how to associate an `AbortSignal` with a Writable stream instance by passing it in the constructor options. Calling `abort()` on the corresponding `AbortController` will trigger the stream's destruction with an `AbortError`. Requires the `node:stream` module and `AbortController`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_76

LANGUAGE: JavaScript
CODE:
```
const { Writable } = require('node:stream');

const controller = new AbortController();
const myWritable = new Writable({
  write(chunk, encoding, callback) {
    // ...
  },
  writev(chunks, callback) {
    // ...
  },
  signal: controller.signal,
});
// Later, abort the operation closing the stream
controller.abort();
```

----------------------------------------

TITLE: Allocating and Filling Buffer with Encoded String using Buffer.alloc
DESCRIPTION: Demonstrates allocating a new Node.js Buffer of a specified size (11 bytes) and initializing it with a string ('aGVsbG8gd29ybGQ=') decoded using a specific encoding ('base64') via the `fill` and `encoding` arguments of `Buffer.alloc`. This decodes the base64 string 'hello world' into the buffer. Requires importing the `Buffer` class from `node:buffer`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_14

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

const buf = Buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');

console.log(buf);
```

LANGUAGE: javascript
CODE:
```
const { Buffer } = require('node:buffer');

const buf = Buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');

console.log(buf);
```

----------------------------------------

TITLE: Using assert.doesNotThrow (Matching Error) in MJS
DESCRIPTION: This example demonstrates `assert.doesNotThrow` with a synchronous function that throws a `TypeError`. The assertion specifies `TypeError` as the error type that should *not* be thrown. Since the thrown `TypeError` *does* match the specified `TypeError`, the assertion fails and throws an `AssertionError`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_30

LANGUAGE: mjs
CODE:
```
import assert from 'node:assert/strict';

assert.doesNotThrow(
  () => {
    throw new TypeError('Wrong value');
  },
  TypeError,
);
```

----------------------------------------

TITLE: Creating and Configuring an Independent DNS Resolver - JavaScript
DESCRIPTION: Demonstrates creating a new `dns.Resolver` instance and setting its DNS servers using `resolver.setServers`. Subsequent resolution calls on this specific resolver instance (`resolver.resolve4`) will use the configured servers, independent of global DNS settings.
SOURCE: https://github.com/asana/node/blob/main/doc/api/dns.md#_snippet_2

LANGUAGE: js
CODE:
```
const { Resolver } = require('node:dns');
const resolver = new Resolver();
resolver.setServers(['4.4.4.4']);

// This request will use the server at 4.4.4.4, independent of global settings.
resolver.resolve4('example.org', (err, addresses) => {
  // ...
});
```

----------------------------------------

TITLE: HTTPS Request Using URL Object as Options Node.js
DESCRIPTION: This example demonstrates the flexibility of `https.request()` by using a WHATWG `URL` object directly as the `options` parameter. The `https` module automatically parses the URL object to configure the request. This simplifies request setup when the target is easily described by a URL string.
SOURCE: https://github.com/asana/node/blob/main/doc/api/https.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
const options = new URL('https://abc:xyz@example.com');

const req = https.request(options, (res) => {
  // ...
});
```

----------------------------------------

TITLE: Install Chalk - Node.js - Console
DESCRIPTION: This snippet provides the standard command-line instruction for installing the Chalk library using the npm package manager. Dependencies will be automatically handled by npm.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/chalk/readme.md#_snippet_0

LANGUAGE: Console
CODE:
```
$ npm install chalk
```

----------------------------------------

TITLE: Ending Node.js Writable Stream with Final Chunk - JavaScript
DESCRIPTION: This example shows how to properly end a Node.js Writable stream using the `end()` method. It demonstrates writing a final chunk of data ('world!') immediately before closing the stream and notes that subsequent writes are not allowed. Requires the 'node:fs' module to create a file stream.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_21

LANGUAGE: JavaScript
CODE:
```
const fs = require('node:fs');
const file = fs.createWriteStream('example.txt');
file.write('hello, ');
file.end('world!');
// Writing more now is not allowed!
```

----------------------------------------

TITLE: Listening for Data Events - Node.js Streams JavaScript
DESCRIPTION: Shows how to attach a listener to the 'data' event of a Node.js Readable stream. The listener receives data chunks, which are logged to the console along with their size. Attaching this listener switches the stream into flowing mode if it wasn't already.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_25

LANGUAGE: JavaScript
CODE:
```
const readable = getReadableStreamSomehow();
readable.on('data', (chunk) => {
  console.log(`Received ${chunk.length} bytes of data.`);
});
```

----------------------------------------

TITLE: Installing qrcode-terminal CLI Globally | Shell
DESCRIPTION: Installs the qrcode-terminal package globally using npm. This makes the `qrcode-terminal` command available from any directory in your system's terminal.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/qrcode-terminal/README.md#_snippet_8

LANGUAGE: Shell
CODE:
```
npm install -g qrcode-terminal
```

----------------------------------------

TITLE: Calculating String Byte Length (Node.js Buffer - mjs/cjs)
DESCRIPTION: This snippet demonstrates how to use the static `Buffer.byteLength` method to determine the byte size of a string when encoded in UTF-8. It contrasts this with the character count provided by `String.prototype.length`, highlighting the difference between character and byte representations. Examples are provided for both ES Modules (mjs) and CommonJS (cjs).
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_17

LANGUAGE: mjs
CODE:
```
import { Buffer } from 'node:buffer';

const str = '\u00bd + \u00bc = \u00be';

console.log(`${str}: ${str.length} characters, ` +
            `${Buffer.byteLength(str, 'utf8')} bytes`);
// Prints: ½ + ¼ = ¾: 9 characters, 12 bytes
```

LANGUAGE: cjs
CODE:
```
const { Buffer } = require('node:buffer');

const str = '\u00bd + \u00bc = \u00be';

console.log(`${str}: ${str.length} characters, ` +
            `${Buffer.byteLength(str, 'utf8')} bytes`);
// Prints: ½ + ¼ = ¾: 9 characters, 12 bytes
```

----------------------------------------

TITLE: Using AbortSignal with child_process.spawn (JavaScript)
DESCRIPTION: Demonstrates how to use an `AbortSignal` with `child_process.spawn` to terminate the child process. When the signal is aborted, the process is killed and an `AbortError` is emitted on the 'error' event.
SOURCE: https://github.com/asana/node/blob/main/doc/changelogs/CHANGELOG_V15.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const controller = new AbortController();
const { signal } = controller;
const grep = spawn('grep', ['ssh'], { signal });
grep.on('error', (err) => {
  // This will be called with err being an AbortError if the controller aborts
});
controller.abort(); // stops the process
```

----------------------------------------

TITLE: Creating Buffer Instances - CommonJS
DESCRIPTION: Demonstrates various methods for creating Buffer instances using CommonJS require syntax, identical in functionality to the ESM examples but suitable for older Node.js modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const { Buffer } = require('node:buffer');

// Creates a zero-filled Buffer of length 10.
const buf1 = Buffer.alloc(10);

// Creates a Buffer of length 10,
// filled with bytes which all have the value `1`.
const buf2 = Buffer.alloc(10, 1);

// Creates an uninitialized buffer of length 10.
// This is faster than calling Buffer.alloc() but the returned
// Buffer instance might contain old data that needs to be
// overwritten using fill(), write(), or other functions that fill the Buffer's
// contents.
const buf3 = Buffer.allocUnsafe(10);

// Creates a Buffer containing the bytes [1, 2, 3].
const buf4 = Buffer.from([1, 2, 3]);

// Creates a Buffer containing the bytes [1, 1, 1, 1] – the entries
// are all truncated using `(value & 255)` to fit into the range 0–255.
const buf5 = Buffer.from([257, 257.5, -255, '1']);

// Creates a Buffer containing the UTF-8-encoded bytes for the string 'tést':
// [0x74, 0xc3, 0xa9, 0x73, 0x74] (in hexadecimal notation)
// [116, 195, 169, 115, 116] (in decimal notation)
const buf6 = Buffer.from('tést');

// Creates a Buffer containing the Latin-1 bytes [0x74, 0xe9, 0x73, 0x74].
const buf7 = Buffer.from('tést', 'latin1');
```

----------------------------------------

TITLE: Generating and Exporting HMAC Key in JavaScript
DESCRIPTION: This asynchronous function `generateAndExportHmacKey` generates an HMAC key using `subtle.generateKey` and then exports the generated key into a specified format (defaulting to 'jwk') using `subtle.exportKey`. It requires the `globalThis.crypto` object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_8

LANGUAGE: javascript
CODE:
```
const { subtle } = globalThis.crypto;

async function generateAndExportHmacKey(format = 'jwk', hash = 'SHA-512') {
  const key = await subtle.generateKey({
    name: 'HMAC',
    hash,
  }, true, ['sign', 'verify']);

  return subtle.exportKey(format, key);
}
```

----------------------------------------

TITLE: Using stream.finished (Promises API) to Wait for Stream (ESM)
DESCRIPTION: Shows how to use the Promise-based `stream.finished` function with ES Module syntax. This utility is used to asynchronously wait until a stream signals that it is finished, providing a Promise interface for stream completion.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_10

LANGUAGE: javascript
CODE:
```
import { finished } from 'node:stream/promises';
import { createReadStream } from 'node:fs';

const rs = createReadStream('archive.tar');

async function run() {
  await finished(rs);
  console.log('Stream is done reading.');
}

run().catch(console.error);
rs.resume(); // Drain the stream.
```

----------------------------------------

TITLE: Configuring Scoped Registry Authentication npm Bash
DESCRIPTION: These commands demonstrate how to log in and out of a private npm registry associated with a specific scope (e.g., `@mycorp`) using `npm login` and `npm logout`. Logging in links the scope to the custom registry URL and saves credentials, essential for interacting with packages under that scope. Logging out removes the link and authentication token.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-adduser.md#_snippet_1

LANGUAGE: bash
CODE:
```
# log in, linking the scope to the custom registry
npm login --scope=@mycorp --registry=https://registry.mycorp.com

# log out, removing the link and the auth token
npm logout --scope=@mycorp
```

----------------------------------------

TITLE: Simplified Duplex Stream Creation (Options Object, JavaScript)
DESCRIPTION: Presents an alternative, more concise way to create a `Duplex` stream by passing an options object containing the `read` and `write` implementation methods directly to the `new Duplex()` constructor. This approach avoids needing a separate class or function definition for simple cases.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_91

LANGUAGE: JavaScript
CODE:
```
const { Duplex } = require('node:stream');

const myDuplex = new Duplex({
  read(size) {
    // ...
  },
  write(chunk, encoding, callback) {
    // ...
  },
});
```

----------------------------------------

TITLE: Writing File Data with AbortSignal using fsPromises (Node.js)
DESCRIPTION: This snippet demonstrates how to use the `fsPromises.writeFile` function to write data to a file asynchronously. It includes an example of using an `AbortSignal` to cancel the write operation before it completes, showing how to handle the resulting `AbortError`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_29

LANGUAGE: mjs
CODE:
```
import { writeFile } from 'node:fs/promises';
import { Buffer } from 'node:buffer';

try {
  const controller = new AbortController();
  const { signal } = controller;
  const data = new Uint8Array(Buffer.from('Hello Node.js'));
  const promise = writeFile('message.txt', data, { signal });

  // Abort the request before the promise settles.
  controller.abort();

  await promise;
} catch (err) {
  // When a request is aborted - err is an AbortError
  console.error(err);
}
```

----------------------------------------

TITLE: Handling EADDRINUSE Error in net.Server
DESCRIPTION: This snippet demonstrates how to handle the common 'EADDRINUSE' error when a server attempts to listen on an address already in use. It sets up an error listener on the server to catch this specific error, log a message, close the server, and then retry listening after a 1-second delay.
SOURCE: https://github.com/asana/node/blob/main/doc/api/net.md#_snippet_4

LANGUAGE: js
CODE:
```
server.on('error', (e) => {
  if (e.code === 'EADDRINUSE') {
    console.error('Address in use, retrying...');
    setTimeout(() => {
      server.close();
      server.listen(PORT, HOST);
    }, 1000);
  }
});
```

----------------------------------------

TITLE: Getting Child Process PID (Node.js)
DESCRIPTION: Shows how to access the process ID (PID) of a spawned child process immediately after creation using the `subprocess.pid` property in Node.js.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_22

LANGUAGE: javascript
CODE:
```
const { spawn } = require('node:child_process');
const grep = spawn('grep', ['ssh']);

console.log(`Spawned child pid: ${grep.pid}`);
grep.stdin.end();
```

----------------------------------------

TITLE: Checking CLI Flags with hasFlag (JavaScript)
DESCRIPTION: Demonstrates how to import the `has-flag` module and use the `hasFlag` function to check for various command-line flags in `process.argv`. Shows examples with and without `--` or `-` prefixes and key-value pair formats.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/@babel/highlight/node_modules/has-flag/readme.md#_snippet_1

LANGUAGE: javascript
CODE:
```
// foo.js
const hasFlag = require('has-flag');

hasFlag('unicorn');
//=> true

hasFlag('--unicorn');
//=> true

hasFlag('f');
//=> true

hasFlag('-f');
//=> true

hasFlag('foo=bar');
//=> true

hasFlag('foo');
//=> false

hasFlag('rainbow');
//=> false
```

----------------------------------------

TITLE: Running npm adduser Command Bash
DESCRIPTION: This snippet shows the basic command `npm adduser` and its alias `add-user`. It is the fundamental way to initiate the user creation process for an npm registry. Executing this command typically prompts the user for required credentials like username, password, and email.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-adduser.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm adduser

alias: add-user
```

----------------------------------------

TITLE: Creating Secure HTTP/2 Server using http2.createSecureServer in Node.js
DESCRIPTION: This snippet demonstrates how to instantiate a secure HTTP/2 server. It requires loading TLS/SSL certificate and key files, setting them in the options object, attaching a stream event listener to handle incoming requests, and finally starting the server to listen on a specified port.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http2.md#_snippet_37

LANGUAGE: JavaScript
CODE:
```
const http2 = require('node:http2');
const fs = require('node:fs');

const options = {
  key: fs.readFileSync('server-key.pem'),
  cert: fs.readFileSync('server-cert.pem'),
};

// Create a secure HTTP/2 server
const server = http2.createSecureServer(options);

server.on('stream', (stream, headers) => {
  stream.respond({
    'content-type': 'text/html; charset=utf-8',
    ':status': 200,
  });
  stream.end('<h1>Hello World</h1>');
});

server.listen(8443);
```

----------------------------------------

TITLE: Configuring Binaries in package.json (Object)
DESCRIPTION: Defines executable files for a package using an object where keys are command names and values are relative file paths. This configuration is used by npm during global installation to create symlinks or command files, making the executables available in the system's PATH.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_17

LANGUAGE: json
CODE:
```
{
  "bin": {
    "myapp": "./cli.js"
  }
}
```

----------------------------------------

TITLE: Checking Outdated npm Packages in Bash
DESCRIPTION: Illustrates the basic syntax for using the `npm outdated` command in a Bash shell. This command checks installed packages in the current project against the npm registry to find outdated versions. Optional package specifications can be provided to check only specific dependencies.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-outdated.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm outdated [<package-spec> ...]
```

----------------------------------------

TITLE: Quick Start Basic Options Parsing JavaScript
DESCRIPTION: Demonstrates declaring simple boolean and value options using `.option()`. It shows how to parse command-line arguments with `program.parse()` and access parsed options and arguments via `program.opts()` and `program.args`. Requires the 'commander' library.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/postject-copy/node_modules/commander/Readme.md#_snippet_1

LANGUAGE: js
CODE:
```
const { program } = require('commander');

program
  .option('--first')
  .option('-s, --separator <char>');

program.parse();

const options = program.opts();
const limit = options.first ? 1 : undefined;
console.log(program.args[0].split(options.separator, limit));
```

----------------------------------------

TITLE: Initializing Node.js Cluster Workers (CJS)
DESCRIPTION: Demonstrates setting up a Node.js cluster using CommonJS. The primary process checks `cluster.isPrimary`, forks workers equal to the number of available CPUs, and logs worker exits. Worker processes (`!cluster.isPrimary`) create a simple HTTP server listening on port 8000.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cluster.md#_snippet_1

LANGUAGE: JavaScript (CJS)
CODE:
```
const cluster = require('node:cluster');
const http = require('node:http');
const numCPUs = require('node:os').availableParallelism();
const process = require('node:process');

if (cluster.isPrimary) {
  console.log(`Primary ${process.pid} is running`);

  // Fork workers.
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`worker ${worker.process.pid} died`);
  });
} else {
  // Workers can share any TCP connection
  // In this case it is an HTTP server
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('hello world\n');
  }).listen(8000);

  console.log(`Worker ${process.pid} started`);
}
```

----------------------------------------

TITLE: Using stream.pipeline with AbortSignal (Promises API, ESM)
DESCRIPTION: Shows how to use `stream.pipeline` with an `AbortSignal` and ES Module syntax. Aborting the signal will terminate the pipeline and the `pipeline` Promise will reject with an `AbortError`, demonstrating how to handle cancellation.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_4

LANGUAGE: javascript
CODE:
```
import { pipeline } from 'node:stream/promises';
import { createReadStream, createWriteStream } from 'node:fs';
import { createGzip } from 'node:zlib';

const ac = new AbortController();
const { signal } = ac;
setImmediate(() => ac.abort());
try {
  await pipeline(
    createReadStream('archive.tar'),
    createGzip(),
    createWriteStream('archive.tar.gz'),
    { signal },
  );
} catch (err) {
  console.error(err); // AbortError
}
```

----------------------------------------

TITLE: Checking for Map Instances in Node.js util.types (JavaScript)
DESCRIPTION: This snippet shows the usage of `util.types.isMap()` to check if a value is a built-in Map instance. It provides a simple example that returns true for a new Map object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_58

LANGUAGE: javascript
CODE:
```
util.types.isMap(new Map());  // Returns true
```

----------------------------------------

TITLE: AsyncLocalStorage.snapshot() in a Class
DESCRIPTION: This example shows how `AsyncLocalStorage.snapshot()` can be used within a class constructor to capture the current `AsyncLocalStorage` context. A method on the class instance can then use the captured snapshot function to retrieve the store value from the context that was active when the instance was created, even if the method is called from a different context.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
class Foo {
  #runInAsyncScope = AsyncLocalStorage.snapshot();

  get() { return this.#runInAsyncScope(() => asyncLocalStorage.getStore()); }
}

const foo = asyncLocalStorage.run(123, () => new Foo());
console.log(asyncLocalStorage.run(321, () => foo.get())); // returns 123
```

----------------------------------------

TITLE: Initializing and Running WASI Module with CJS
DESCRIPTION: Demonstrates how to use the `node:wasi` module in a CommonJS context. It shows requiring necessary modules, instantiating `WASI` with options like arguments, environment, and preopened directories, compiling and instantiating a WebAssembly module using an async IIFE, and starting the WASI execution. Requires Node.js with WASI support and a `demo.wasm` file.
SOURCE: https://github.com/asana/node/blob/main/doc/api/wasi.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
'use strict';
const { readFile } = require('node:fs/promises');
const { WASI } = require('wasi');
const { argv, env } = require('node:process');
const { join } = require('node:path');

const wasi = new WASI({
  version: 'preview1',
  args: argv,
  env,
  preopens: {
    '/local': '/some/real/path/that/wasm/can/access',
  },
});

(async () => {
  const wasm = await WebAssembly.compile(
    await readFile(join(__dirname, 'demo.wasm')),
  );
  const instance = await WebAssembly.instantiate(wasm, wasi.getImportObject());

  wasi.start(instance);
})();
```

----------------------------------------

TITLE: Advanced Color Conversion with ansi-styles (JavaScript)
DESCRIPTION: Provides detailed examples of converting various color formats (RGB, HSL, Hex) to ANSI escape codes for different color terminal capabilities (16, 256, and 16 million). Demonstrates using `ansi.rgb`, `ansi256.hsl`, `ansi16m.hex` methods for both foreground (`style.color`) and background (`style.bgColor`) colors. Requires the ansi-styles and color-convert packages and a terminal with corresponding color support.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/@babel/code-frame/node_modules/ansi-styles/readme.md#_snippet_4

LANGUAGE: javascript
CODE:
```
style.color.ansi.rgb(100, 200, 15); // RGB to 16 color ansi foreground code
style.bgColor.ansi.rgb(100, 200, 15); // RGB to 16 color ansi background code

style.color.ansi256.hsl(120, 100, 60); // HSL to 256 color ansi foreground code
style.bgColor.ansi256.hsl(120, 100, 60); // HSL to 256 color ansi foreground code

style.color.ansi16m.hex('#C0FFEE'); // Hex (RGB) to 16 million color foreground code
style.bgColor.ansi16m.hex('#C0FFEE'); // Hex (RGB) to 16 million color background code
```

----------------------------------------

TITLE: Installing package from GitLab URL - Bash
DESCRIPTION: Provides examples of installing an npm package from a GitLab repository using the `gitlab:` prefix and username/repository path. It also shows how to specify a version range using `#semver:`.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-install.md#_snippet_4

LANGUAGE: bash
CODE:
```
npm install gitlab:mygitlabuser/myproject
```

LANGUAGE: bash
CODE:
```
npm install gitlab:myusr/myproj#semver:^5.0
```

----------------------------------------

TITLE: Summing Array Elements with reduceRight JavaScript
DESCRIPTION: This snippet demonstrates using `reduceRight` to calculate the sum of elements in a simple array `[0, 1, 2, 3]` without providing an initial value. The accumulation starts from the last element.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/webkit/array-reduceRight-expected.txt#_snippet_0

LANGUAGE: javascript
CODE:
```
[0,1,2,3].reduceRight(function(a,b){ return a + b; })
```

----------------------------------------

TITLE: Passing Arguments and 'this' to Listeners (ESM)
DESCRIPTION: Illustrates how arguments passed to `emit()` are forwarded to listener functions. It also shows that, for standard function expressions, the `this` keyword inside the listener refers to the EventEmitter instance.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { EventEmitter } from 'node:events';
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('event', function(a, b) {
  console.log(a, b, this, this === myEmitter);
  // Prints:
  //   a b MyEmitter {
  //     _events: [Object: null prototype] { event: [Function (anonymous)] },
  //     _eventsCount: 1,
  //     _maxListeners: undefined,
  //     [Symbol(kCapture)]: false
  //   } true
});
myEmitter.emit('event', 'a', 'b');
```

----------------------------------------

TITLE: Demonstrating Race Condition with Async FS Operations (Node.js JS)
DESCRIPTION: Illustrates a potential race condition when executing asynchronous file system operations (`fs.rename` and `fs.stat`) sequentially without proper coordination using callback-based APIs. The `stat` operation might complete before `rename`, leading to errors or unexpected results. Requires the `node:fs` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_86

LANGUAGE: js
CODE:
```
const fs = require('node:fs');

fs.rename('/tmp/hello', '/tmp/world', (err) => {
  if (err) throw err;
  console.log('renamed complete');
});
fs.stat('/tmp/world', (err, stats) => {
  if (err) throw err;
  console.log(`stats: ${JSON.stringify(stats)}`);
});
```

----------------------------------------

TITLE: Handling Upgrade Event in Node.js HTTP Server and Client - CJS
DESCRIPTION: Provides a complete example using CommonJS modules (`require`) of a Node.js HTTP server that listens for the 'upgrade' event (e.g., for WebSockets) and a corresponding client that initiates an upgrade request and listens for the client-side 'upgrade' event. The server demonstrates writing a 101 response and piping the socket for echo, while the client logs a success message and exits.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_11

LANGUAGE: javascript
CODE:
```
const http = require('node:http');

// Create an HTTP server
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('okay');
});
server.on('upgrade', (req, socket, head) => {
  socket.write('HTTP/1.1 101 Web Socket Protocol Handshake\r\n' +               'Upgrade: WebSocket\r\n' +               'Connection: Upgrade\r\n' +               '\r\n');

  socket.pipe(socket); // echo back
});

// Now that server is running
server.listen(1337, '127.0.0.1', () => {

  // make a request
  const options = {
    port: 1337,
    host: '127.0.0.1',
    headers: {
      'Connection': 'Upgrade',
      'Upgrade': 'websocket',
    },
  };

  const req = http.request(options);
  req.end();

  req.on('upgrade', (res, socket, upgradeHead) => {
    console.log('got upgraded!');
    socket.end();
    process.exit(0);
  });
});
```

----------------------------------------

TITLE: Running Node.js Tests with Subtests JavaScript
DESCRIPTION: This snippet shows how to define a top-level test using `test` and create a subtest within it using the `t.test` method provided by the `TestContext`. It highlights the need to `await` the subtest promise to ensure the parent test waits for its completion, preventing the subtest from being canceled prematurely. The subtest demonstrates an asynchronous operation (a timeout) wrapped in a Promise.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_27

LANGUAGE: javascript
CODE:
```
test('top level test', async (t) => {
  // The setTimeout() in the following subtest would cause it to outlive its
  // parent test if 'await' is removed on the next line. Once the parent test
  // completes, it will cancel any outstanding subtests.
  await t.test('longer running subtest', async (t) => {
    return new Promise((resolve, reject) => {
      setTimeout(resolve, 1000);
    });
  });
});
```

----------------------------------------

TITLE: Using undici ProxyAgent Locally per Request - JavaScript
DESCRIPTION: Applies the created `ProxyAgent` instance to a single `undici` request using the `dispatcher` option within the request configuration. This method routes only the specific request through the proxy without affecting the global `undici` dispatcher setting.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/ProxyAgent.md#_snippet_3

LANGUAGE: javascript
CODE:
```
import { ProxyAgent, request } from 'undici'

const proxyAgent = new ProxyAgent('my.proxy.server')

const {
  statusCode,
  body
} = await request('http://localhost:3000/foo', { dispatcher: proxyAgent })

console.log('response received', statusCode) // response received 200

for await (const data of body) {
  console.log('data', data.toString('utf8')) // data foo
}
```

----------------------------------------

TITLE: Creating and Encoding with TextEncoder (JavaScript)
DESCRIPTION: This snippet demonstrates how to create a TextEncoder instance and use its `encode` method to convert a JavaScript string into a UTF-8 encoded Uint8Array. It requires the TextEncoder class, available globally in recent Node.js versions. The input is a string, and the output is a Uint8Array containing the encoded bytes.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_34

LANGUAGE: JavaScript
CODE:
```
const encoder = new TextEncoder();
const uint8array = encoder.encode('this is some data');
```

----------------------------------------

TITLE: Sample fs.StatFs Object Structure (Number)
DESCRIPTION: Displays the properties and their types (`number`) for a typical Node.js `fs.StatFs` object returned by `fs.statfs()` when the `bigint` option is false or not provided. It shows file system statistics like block size, block counts, and file node counts.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_83

LANGUAGE: console
CODE:
```
StatFs {
  type: 1397114950,
  bsize: 4096,
  blocks: 121938943,
  bfree: 61058895,
  bavail: 61058895,
  files: 999,
  ffree: 1000000
}
```

----------------------------------------

TITLE: Bidirectional Communication via parentPort and Worker - JavaScript
DESCRIPTION: Illustrates how the main thread sends a message to a worker using `worker.postMessage` and the worker receives it via `parentPort.on('message')`. It also shows the worker replying via `parentPort.postMessage` and the main thread receiving that message via `worker.on('message')`, enabling simple bidirectional communication.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
const { Worker, isMainThread, parentPort } = require('node:worker_threads');

if (isMainThread) {
  const worker = new Worker(__filename);
  worker.once('message', (message) => {
    console.log(message);  // Prints 'Hello, world!'.
  });
  worker.postMessage('Hello, world!');
} else {
  // When a message from the parent thread is received, send it back:
  parentPort.once('message', (message) => {
    parentPort.postMessage(message);
  });
}
```

----------------------------------------

TITLE: Importing CommonJS Named, Default, and Namespace Exports (JavaScript)
DESCRIPTION: Demonstrates the different ways to import from a CommonJS module with named exports (`cjs.cjs`) into an ES module, showing how to get the named export, the default export (`module.exports`), and the full namespace object with both.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_13

LANGUAGE: javascript
CODE:
```
import { name } from './cjs.cjs';
console.log(name);
// Prints: 'exported'

import cjs from './cjs.cjs';
console.log(cjs);
// Prints: { name: 'exported' }

import * as m from './cjs.cjs';
console.log(m);
// Prints: [Module] { default: { name: 'exported' }, name: 'exported' }
```

----------------------------------------

TITLE: Resolving Module Specifiers with import.meta.resolve (JavaScript)
DESCRIPTION: Illustrates the usage of `import.meta.resolve` to synchronously determine the absolute URL path for both a package dependency and a relative file path, providing examples of the expected output URLs.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_9

LANGUAGE: javascript
CODE:
```
const dependencyAsset = import.meta.resolve('component-lib/asset.css');
// file:///app/node_modules/component-lib/asset.css
import.meta.resolve('./dep.js');
// file:///app/dep.js
```

----------------------------------------

TITLE: Getting and Setting Node.js URL Pathname - JavaScript
DESCRIPTION: Shows how to retrieve the path segment of a URL using `myURL.pathname` and update it. Illustrates how changing the pathname updates the full URL string (`myURL.href`). Invalid URL characters assigned to `pathname` are percent-encoded.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_21

LANGUAGE: javascript
CODE:
```
const myURL = new URL('https://example.org/abc/xyz?123');
console.log(myURL.pathname);
// Prints /abc/xyz

myURL.pathname = '/abcdef';
console.log(myURL.href);
// Prints https://example.org/abcdef?123
```

----------------------------------------

TITLE: Example Legacy npm init Skip Prompts Bash
DESCRIPTION: Shows how to generate a package.json using the legacy `npm init` behavior without being prompted for details by using the `-y` or `--yes` flag. It automatically fills in defaults.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-init.md#_snippet_7

LANGUAGE: bash
CODE:
```
$ npm init -y
```

----------------------------------------

TITLE: Defining Package Exports with JSON
DESCRIPTION: Configures a Node.js package named "a-package" using the "exports" field. This allows modules within the package to reference the main export (.) pointing to ./index.mjs and a specific path ./foo.js pointing to itself. Required for enabling package self-referencing.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_12

LANGUAGE: json
CODE:
```
// package.json
{
  "name": "a-package",
  "exports": {
    ".": "./index.mjs",
    "./foo.js": "./foo.js"
  }
}
```

----------------------------------------

TITLE: Listening for Data and End Events - Node.js Streams JavaScript
DESCRIPTION: Illustrates attaching listeners for both the 'data' and 'end' events on a readable stream. The 'data' listener processes data chunks, while the 'end' listener is notified when no more data is available. The 'end' event is only emitted if the stream's data is fully consumed.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_26

LANGUAGE: JavaScript
CODE:
```
const readable = getReadableStreamSomehow();
readable.on('data', (chunk) => {
  console.log(`Received ${chunk.length} bytes of data.`);
});
readable.on('end', () => {
  console.log('There will be no more data.');
});
```

----------------------------------------

TITLE: Generating Asynchronous Random Bytes using Node.js Crypto - JavaScript
DESCRIPTION: Demonstrates how to asynchronously generate a specified number of cryptographically strong random bytes using `crypto.randomBytes`. It requires a callback function to handle the result or any errors. The callback receives an error object and a buffer containing the generated random data.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_43

LANGUAGE: JavaScript
CODE:
```
// Asynchronous
const {
  randomBytes,
} = await import('node:crypto');

randomBytes(256, (err, buf) => {
  if (err) throw err;
  console.log(`${buf.length} bytes of random data: ${buf.toString('hex')}`);
});
```

LANGUAGE: JavaScript
CODE:
```
// Asynchronous
const {
  randomBytes,
} = require('node:crypto');

randomBytes(256, (err, buf) => {
  if (err) throw err;
  console.log(`${buf.length} bytes of random data: ${buf.toString('hex')}`);
});
```

----------------------------------------

TITLE: Generating SHA256 Hash of File Node.js
DESCRIPTION: This snippet demonstrates how to calculate the SHA256 hash of a file in Node.js. It uses `fs.createReadStream` to read the file content in chunks and `crypto.createHash` to update the hash object with the data. The final hash is printed to the console in hexadecimal format.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_25

LANGUAGE: Node.js MJS
CODE:
```
import {
  createReadStream,
} from 'node:fs';
import { argv } from 'node:process';
const {
  createHash,
} = await import('node:crypto');

const filename = argv[2];

const hash = createHash('sha256');

const input = createReadStream(filename);
input.on('readable', () => {
  // Only one element is going to be produced by the
  // hash stream.
  const data = input.read();
  if (data)
    hash.update(data);
  else {
    console.log(`${hash.digest('hex')} ${filename}`);
  }
});
```

LANGUAGE: Node.js CJS
CODE:
```
const {
  createReadStream,
} = require('node:fs');
const {
  createHash,
} = require('node:crypto');
const { argv } = require('node:process');

const filename = argv[2];

const hash = createHash('sha256');

const input = createReadStream(filename);
input.on('readable', () => {
  // Only one element is going to be produced by the
  // hash stream.
  const data = input.read();
  if (data)
    hash.update(data);
  else {
    console.log(`${hash.digest('hex')} ${filename}`);
  }
});
```

----------------------------------------

TITLE: Testing Array.every Short-Circuiting - JavaScript
DESCRIPTION: This snippet tests the short-circuiting behavior of `Array.prototype.every`. It verifies that if the callback function (`isBigEnoughShortCircuit`) returns `false` for any element, the iteration stops immediately, and `every` returns `false` without checking the remaining elements. This demonstrates an optimization where not all elements need to be checked if a failing one is found early.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/webkit/array-every-expected.txt#_snippet_7

LANGUAGE: javascript
CODE:
```
[12, 5, 8, 130, 44].every(isBigEnoughShortCircuit)
```

----------------------------------------

TITLE: Demonstrating Unhandled Promise Rejection in Node.js EventEmitter (CJS)
DESCRIPTION: This snippet shows the default behavior of EventEmitter in CommonJS modules when an async listener throws an error or rejects a promise, resulting in an unhandled rejection. It requires the `node:events` module using `require`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_19

LANGUAGE: cjs
CODE:
```
const EventEmitter = require('node:events');
const ee = new EventEmitter();
ee.on('something', async (value) => {
  throw new Error('kaboom');
});
```

----------------------------------------

TITLE: Deleting File with fs Module (Callback) [MJS]
DESCRIPTION: Demonstrates asynchronously deleting a file using the callback-based `unlink` function from `node:fs` with ES module syntax, handling errors via the callback's first argument.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_6

LANGUAGE: javascript
CODE:
```
import { unlink } from 'node:fs';

unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('successfully deleted /tmp/hello');
});
```

----------------------------------------

TITLE: Using AbortSignal with Node.js Stream Async Iterable (JavaScript)
DESCRIPTION: This example shows how to use `addAbortSignal` with a Node.js stream consumed as an async iterable. It includes setting a timeout for automatic cancellation and demonstrates catching the `AbortError` during iteration.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_69

LANGUAGE: javascript
CODE:
```
const controller = new AbortController();
setTimeout(() => controller.abort(), 10_000); // set a timeout
const stream = addAbortSignal(
  controller.signal,
  fs.createReadStream(('object.json')),
);
(async () => {
  try {
    for await (const chunk of stream) {
      await process(chunk);
    }
  } catch (e) {
    if (e.name === 'AbortError') {
      // The operation was cancelled
    } else {
      throw e;
    }
  }
})();
```

----------------------------------------

TITLE: Changing file permissions with callback - Node.js fs - JavaScript
DESCRIPTION: Demonstrates the asynchronous `fs.chmod` method to change the permissions of a file. It sets the permissions of 'my_file.txt' to 0o775 (read/write/execute for owner/group, read/execute for others) and logs a success message upon completion or throws an error. Requires the 'node:fs' module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_38

LANGUAGE: javascript
CODE:
```
import { chmod } from 'node:fs';

chmod('my_file.txt', 0o775, (err) => {
  if (err) throw err;
  console.log('The permissions for file "my_file.txt" have been changed!');
});
```

----------------------------------------

TITLE: Converting C uint32_t to JS Number in Node-API (C)
DESCRIPTION: This function converts a C `uint32_t` value into a JavaScript `number`. It requires the Node-API environment, the 32-bit unsigned integer value, and a pointer to store the resulting napi_value. The JavaScript number type is a double-precision floating-point number.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_85

LANGUAGE: C
CODE:
```
napi_status napi_create_uint32(napi_env env, uint32_t value, napi_value* result)
```

----------------------------------------

TITLE: Reading Signed 8-bit Integer from Buffer (ESM/CJS)
DESCRIPTION: Shows how to read a signed 8-bit integer from a Node.js Buffer at a specified offset using the `buf.readInt8` method. Demonstrates reading positive and negative values and an example causing an `ERR_OUT_OF_RANGE` error.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_62

LANGUAGE: javascript (mjs)
CODE:
```
import { Buffer } from 'node:buffer';

const buf = Buffer.from([-1, 5]);

console.log(buf.readInt8(0));
// Prints: -1
console.log(buf.readInt8(1));
// Prints: 5
console.log(buf.readInt8(2));
// Throws ERR_OUT_OF_RANGE.
```

LANGUAGE: javascript (cjs)
CODE:
```
const { Buffer } = require('node:buffer');

const buf = Buffer.from([-1, 5]);

console.log(buf.readInt8(0));
// Prints: -1
console.log(buf.readInt8(1));
// Prints: 5
console.log(buf.readInt8(2));
// Throws ERR_OUT_OF_RANGE.
```

----------------------------------------

TITLE: Using Test Context AbortSignal for Async Operations in Node.js
DESCRIPTION: Illustrates accessing the `AbortSignal` via `t.signal` to link asynchronous operations (like `fetch`) to the test's lifecycle, allowing them to be aborted if the test is cancelled.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_67

LANGUAGE: JavaScript
CODE:
```
test('top level test', async (t) => {
  await fetch('some/uri', { signal: t.signal });
});
```

----------------------------------------

TITLE: Creating HTTPS Server with PFX File (Node.js)
DESCRIPTION: This example demonstrates another way to create an HTTPS server using `https.createServer()`, providing the server credentials via a single PFX (PKCS#12) file. It also listens on port 8000 and responds with 'hello world'.
SOURCE: https://github.com/asana/node/blob/main/doc/api/https.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const https = require('node:https');
const fs = require('node:fs');

const options = {
  pfx: fs.readFileSync('test/fixtures/test_cert.pfx'),
  passphrase: 'sample',
};

https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end('hello world\n');
}).listen(8000);
```

----------------------------------------

TITLE: Using Worker Thread Pool (CJS)
DESCRIPTION: This example demonstrates how to use the `WorkerPool` class implemented using CommonJS modules. It creates a pool, submits multiple tasks (adding two numbers), and logs the results, closing the pool once all tasks are finished. Requires the `WorkerPool` class to be in `./worker_pool.js`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_19

LANGUAGE: JavaScript (CJS)
CODE:
```
const WorkerPool = require('./worker_pool.js');
const os = require('node:os');

const pool = new WorkerPool(os.availableParallelism());

let finished = 0;
for (let i = 0; i < 10; i++) {
  pool.runTask({ a: 42, b: 100 }, (err, result) => {
    console.log(i, err, result);
    if (++finished === 10)
      pool.close();
  });
}
```

----------------------------------------

TITLE: Executing .bat/.cmd Files with exec and shell - Node.js (Windows)
DESCRIPTION: Provides alternative methods for running batch or command files on Windows. Shows using `child_process.exec` for simple command execution with a callback for results, and using `child_process.spawn` with the `{ shell: true }` option, demonstrating how to handle filenames containing spaces by quoting them. Requires the `node:child_process` module and is specific to Windows.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
// OR...
const { exec, spawn } = require('node:child_process');
exec('my.bat', (err, stdout, stderr) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(stdout);
});

// Script with spaces in the filename:
const bat = spawn('"my script.cmd"', ['a', 'b'], { shell: true });
// or:
exec('"my script.cmd" a b', (err, stdout, stderr) => {
  // ...
});
```

----------------------------------------

TITLE: Implementing Readable Stream via Options - Node.js Streams - JavaScript
DESCRIPTION: This snippet illustrates creating a basic Readable stream using the options object approach, similar to the `Writable` options example. It passes an object to the `Readable` constructor containing a `read` function implementation. This `read` function corresponds to the internal `_read` method and is where the stream's logic for pushing data originates. This is a concise way to create simple readable streams without defining a separate class.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_83

LANGUAGE: javascript
CODE:
```
const { Readable } = require('node:stream');

const myReadable = new Readable({
  read(size) {
    // ...
  },
});
```

----------------------------------------

TITLE: Looking up Host IP using dns.lookup - JavaScript
DESCRIPTION: Demonstrates using the `dns.lookup` function to resolve a hostname to an IP address using operating system facilities. The callback receives the resolved address and its family (IPv4 or IPv6). This method does not necessarily interact with DNS servers directly.
SOURCE: https://github.com/asana/node/blob/main/doc/api/dns.md#_snippet_0

LANGUAGE: js
CODE:
```
const dns = require('node:dns');

dns.lookup('example.org', (err, address, family) => {
  console.log('address: %j family: IPv%s', address, family);
});
// address: "93.184.216.34" family: IPv4
```

----------------------------------------

TITLE: Consuming ReadableStream Values (ESM)
DESCRIPTION: This snippet demonstrates iterating over the chunks of a ReadableStream using for await...of with stream.values({ preventCancel: true }). It includes handling potential binary data by converting a chunk (assumed to be a Buffer) to a string. Requires Node.js 16.5.0+. getSomeSource() is a placeholder for a stream source.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_6

LANGUAGE: mjs
CODE:
```
import { Buffer } from 'node:buffer';

const stream = new ReadableStream(getSomeSource());

for await (const chunk of stream.values({ preventCancel: true }))
  console.log(Buffer.from(chunk).toString());
```

----------------------------------------

TITLE: Syncing Feature Branch with Upstream using Rebase (shell)
DESCRIPTION: This snippet demonstrates how to update a local feature branch with the latest changes from the official upstream repository. It first fetches the changes from the upstream remote and then reapplies the commits from the current branch on top of the updated upstream branch, maintaining a linear history.
SOURCE: https://github.com/asana/node/blob/main/deps/uv/CONTRIBUTING.md#_snippet_4

LANGUAGE: shell
CODE:
```
$ git fetch upstream
$ git rebase upstream/v1.x  # or upstream/master
```

----------------------------------------

TITLE: Connecting Node.js HTTP/2 Client (Core API)
DESCRIPTION: This example demonstrates how to create an HTTP/2 client using `http2.connect` to connect to the secure server running locally. It uses the CA certificate for verification, sends a request to the root path, listens for the response headers and data, and closes the connection upon completion.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http2.md#_snippet_5

LANGUAGE: javascript
CODE:
```
const http2 = require('node:http2');
const fs = require('node:fs');
const client = http2.connect('https://localhost:8443', {
  ca: fs.readFileSync('localhost-cert.pem'),
});
client.on('error', (err) => console.error(err));

const req = client.request({ ':path': '/' });

req.on('response', (headers, flags) => {
  for (const name in headers) {
    console.log(`${name}: ${headers[name]}`);
  }
});

req.setEncoding('utf8');
let data = '';
req.on('data', (chunk) => { data += chunk; });
req.on('end', () => {
  console.log(`\n${data}`);
  client.close();
});
req.end();
```

----------------------------------------

TITLE: Promisifying Class Methods Using .call() or .bind()
DESCRIPTION: This example illustrates a common pitfall when using `util.promisify` with class methods that rely on the `this` context. It shows that a direct promisified call (`naiveBar()`) results in a `TypeError` and provides solutions using `.call(foo)` or `.bind(foo)` to correctly preserve the method's `this` binding.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_28

LANGUAGE: js
CODE:
```
const util = require('node:util');

class Foo {
  constructor() {
    this.a = 42;
  }

  bar(callback) {
    callback(null, this.a);
  }
}

const foo = new Foo();

const naiveBar = util.promisify(foo.bar);
// TypeError: Cannot read property 'a' of undefined
// naiveBar().then(a => console.log(a));

naiveBar.call(foo).then((a) => console.log(a)); // '42'

const bindBar = naiveBar.bind(foo);
bindBar().then((a) => console.log(a)); // '42'
```

----------------------------------------

TITLE: Testing Deep Strict Equality with assert.deepStrictEqual Node.js JavaScript
DESCRIPTION: Demonstrates the use of `assert.deepStrictEqual` to perform deep strict comparison between two values. The examples cover various scenarios including comparing objects with different property types, prototypes, type tags, primitive wrappers, zeros, Symbols, and WeakMaps, showing cases that pass and cases that throw an AssertionError.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_22

LANGUAGE: JavaScript (ESM)
CODE:
```
import assert from 'node:assert/strict';

// This fails because 1 !== '1'.
assert.deepStrictEqual({ a: 1 }, { a: '1' });
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
//   {
// +   a: 1
// -   a: '1'
//   }

// The following objects don't have own properties
const date = new Date();
const object = {};
const fakeDate = {};
Object.setPrototypeOf(fakeDate, Date.prototype);

// Different [[Prototype]]:
assert.deepStrictEqual(object, fakeDate);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + {}
// - Date {}

// Different type tags:
assert.deepStrictEqual(date, fakeDate);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + 2018-04-26T00:49:08.604Z
// - Date {}

assert.deepStrictEqual(NaN, NaN);
// OK because Object.is(NaN, NaN) is true.

// Different unwrapped numbers:
assert.deepStrictEqual(new Number(1), new Number(2));
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + [Number: 1]
// - [Number: 2]

assert.deepStrictEqual(new String('foo'), Object('foo'));
// OK because the object and the string are identical when unwrapped.

assert.deepStrictEqual(-0, -0);
// OK

// Different zeros:
assert.deepStrictEqual(0, -0);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + 0
// - -0

const symbol1 = Symbol();
const symbol2 = Symbol();
assert.deepStrictEqual({ [symbol1]: 1 }, { [symbol1]: 1 });
// OK, because it is the same symbol on both objects.

assert.deepStrictEqual({ [symbol1]: 1 }, { [symbol2]: 1 });
// AssertionError [ERR_ASSERTION]: Inputs identical but not reference equal:
//
// {
//   [Symbol()]: 1
// }

const weakMap1 = new WeakMap();
const weakMap2 = new WeakMap([[{}, {}]]);
const weakMap3 = new WeakMap();
weakMap3.unequal = true;

assert.deepStrictEqual(weakMap1, weakMap2);
// OK, because it is impossible to compare the entries

// Fails because weakMap3 has a property that weakMap1 does not contain:
assert.deepStrictEqual(weakMap1, weakMap3);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
//   WeakMap {
// +   [items unknown]
// -   [items unknown],
// -   unequal: true
//   }
```

LANGUAGE: JavaScript (CJS)
CODE:
```
const assert = require('node:assert/strict');

// This fails because 1 !== '1'.
assert.deepStrictEqual({ a: 1 }, { a: '1' });
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
//   {
// +   a: 1
// -   a: '1'
//   }

// The following objects don't have own properties
const date = new Date();
const object = {};
const fakeDate = {};
Object.setPrototypeOf(fakeDate, Date.prototype);

// Different [[Prototype]]:
assert.deepStrictEqual(object, fakeDate);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + {}
// - Date {}

// Different type tags:
assert.deepStrictEqual(date, fakeDate);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + 2018-04-26T00:49:08.604Z
// - Date {}

assert.deepStrictEqual(NaN, NaN);
// OK because Object.is(NaN, NaN) is true.

// Different unwrapped numbers:
assert.deepStrictEqual(new Number(1), new Number(2));
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + [Number: 1]
// - [Number: 2]

assert.deepStrictEqual(new String('foo'), Object('foo'));
// OK because the object and the string are identical when unwrapped.

assert.deepStrictEqual(-0, -0);
// OK

// Different zeros:
assert.deepStrictEqual(0, -0);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
// + 0
// - -0

const symbol1 = Symbol();
const symbol2 = Symbol();
assert.deepStrictEqual({ [symbol1]: 1 }, { [symbol1]: 1 });
// OK, because it is the same symbol on both objects.

assert.deepStrictEqual({ [symbol1]: 1 }, { [symbol2]: 1 });
// AssertionError [ERR_ASSERTION]: Inputs identical but not reference equal:
//
// {
//   [Symbol()]: 1
// }

const weakMap1 = new WeakMap();
const weakMap2 = new WeakMap([[{}, {}]]);
const weakMap3 = new WeakMap();
weakMap3.unequal = true;

assert.deepStrictEqual(weakMap1, weakMap2);
// OK, because it is impossible to compare the entries

// Fails because weakMap3 has a property that weakMap1 does not contain:
assert.deepStrictEqual(weakMap1, weakMap3);
// AssertionError: Expected inputs to be strictly deep-equal:
// + actual - expected
//
//   WeakMap {
// +   [items unknown]
// -   [items unknown],
// -   unequal: true
//   }
```

----------------------------------------

TITLE: Installing node-gyp globally
DESCRIPTION: This command uses the npm package manager to install `node-gyp` globally on your system, making the `node-gyp` command available from any directory. Ensure you have npm and Node.js installed before running this command.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/node-gyp/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm install -g node-gyp
```

----------------------------------------

TITLE: Validating Error Properties with Object and Instance (JS)
DESCRIPTION: This snippet demonstrates using an object or an error instance as the `error` parameter to validate properties of the thrown error. It shows how to match specific property values, including nested objects and regular expressions for string properties, and how validation against an error instance checks `message` and `name`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_52

LANGUAGE: javascript
CODE:
```
import assert from 'node:assert/strict';

const err = new TypeError('Wrong value');
err.code = 404;
err.foo = 'bar';
err.info = {
  nested: true,
  baz: 'text',
};
err.reg = /abc/i;

assert.throws(
  () => {
    throw err;
  },
  {
    name: 'TypeError',
    message: 'Wrong value',
    info: {
      nested: true,
      baz: 'text',
    },
    // Only properties on the validation object will be tested for.
    // Using nested objects requires all properties to be present. Otherwise
    // the validation is going to fail.
  },
);

// Using regular expressions to validate error properties:
assert.throws(
  () => {
    throw err;
  },
  {
    // The `name` and `message` properties are strings and using regular
    // expressions on those will match against the string. If they fail, an
    // error is thrown.
    name: /^TypeError$/,
    message: /Wrong/,
    foo: 'bar',
    info: {
      nested: true,
      // It is not possible to use regular expressions for nested properties!
      baz: 'text',
    },
    // The `reg` property contains a regular expression and only if the
    // validation object contains an identical regular expression, it is going
    // to pass.
    reg: /abc/i,
  },
);

// Fails due to the different `message` and `name` properties:
assert.throws(
  () => {
    const otherErr = new Error('Not found');
    // Copy all enumerable properties from `err` to `otherErr`.
    for (const [key, value] of Object.entries(err)) {
      otherErr[key] = value;
    }
    throw otherErr;
  },
  // The error's `message` and `name` properties will also be checked when using
  // an error as validation object.
  err,
);
```

LANGUAGE: javascript
CODE:
```
const assert = require('node:assert/strict');

const err = new TypeError('Wrong value');
err.code = 404;
err.foo = 'bar';
err.info = {
  nested: true,
  baz: 'text',
};
err.reg = /abc/i;

assert.throws(
  () => {
    throw err;
  },
  {
    name: 'TypeError',
    message: 'Wrong value',
    info: {
      nested: true,
      baz: 'text',
    },
    // Only properties on the validation object will be tested for.
    // Using nested objects requires all properties to be present. Otherwise
    // the validation is going to fail.
  },
);

// Using regular expressions to validate error properties:
assert.throws(
  () => {
    throw err;
  },
  {
    // The `name` and `message` properties are strings and using regular
    // expressions on those will match against the string. If they fail, an
    // error is thrown.
    name: /^TypeError$/,
    message: /Wrong/,
    foo: 'bar',
    info: {
      nested: true,
      // It is not possible to use regular expressions for nested properties!
      baz: 'text',
    },
    // The `reg` property contains a regular expression and only if the
    // validation object contains an identical regular expression, it is going
    // to pass.
    reg: /abc/i,
  },
);

// Fails due to the different `message` and `name` properties:
assert.throws(
  () => {
    const otherErr = new Error('Not found');
    // Copy all enumerable properties from `err` to `otherErr`.
    for (const [key, value] of Object.entries(err)) {
      otherErr[key] = value;
    }
    throw otherErr;
  },
  // The error's `message` and `name` properties will also be checked when using
  // an error as validation object.
  err,
);
```

----------------------------------------

TITLE: Implementing Writable Stream via Options - Node.js Streams - JavaScript
DESCRIPTION: This snippet demonstrates how to create a basic Node.js Writable stream by passing an options object to the `Writable` constructor. It implements the `write` method to process incoming data chunks. The `write` method checks if the chunk contains the character 'a' and calls the callback with an error if it does, otherwise calls the callback without arguments to indicate success. This approach is a simplified way to create writable streams without extending the class.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_78

LANGUAGE: javascript
CODE:
```
const { Writable } = require('node:stream');

const myWritable = new Writable({
  write(chunk, encoding, callback) {
    if (chunk.toString().indexOf('a') >= 0) {
      callback(new Error('chunk is invalid'));
    } else {
      callback();
    }
  },
});
```

----------------------------------------

TITLE: Finding Deprecated Buffer Usage with Grep Shell Command
DESCRIPTION: Uses the grep command-line utility to search for patterns indicating potential usage of the deprecated Buffer or SlowBuffer constructors within project files, excluding the node_modules directory. The pattern specifically looks for occurrences that are not preceded by letters, ensuring it matches constructor calls rather than property access.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_0

LANGUAGE: Shell
CODE:
```
grep -nrE '[^a-zA-Z](Slow)?Buffer\s*\(' --exclude-dir node_modules
```

----------------------------------------

TITLE: Decoding Uint8Array with util.TextDecoder
DESCRIPTION: This example shows how to use the `util.TextDecoder` class to decode a `Uint8Array` into a JavaScript string. It is an implementation of the WHATWG Encoding Standard and is typically used for converting binary data representing text back into a readable string.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_33

LANGUAGE: js
CODE:
```
const decoder = new TextDecoder();
const u8arr = new Uint8Array([72, 101, 108, 108, 111]);
console.log(decoder.decode(u8arr)); // Hello
```

----------------------------------------

TITLE: Broadcasting Messages with Node.js BroadcastChannel
DESCRIPTION: This example illustrates using `BroadcastChannel` for broadcasting messages from multiple worker threads to a main thread. The main thread sets up a listener for messages and creates several workers, while each worker connects to the same channel, sends a message, and then closes its connection. Requires `node:worker_threads` and `BroadcastChannel`. The main thread counts received messages and closes the channel after receiving a specific number.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_10

LANGUAGE: js
CODE:
```
'use strict';

const {
  isMainThread,
  BroadcastChannel,
  Worker,
} = require('node:worker_threads');

const bc = new BroadcastChannel('hello');

if (isMainThread) {
  let c = 0;
  bc.onmessage = (event) => {
    console.log(event.data);
    if (++c === 10) bc.close();
  };
  for (let n = 0; n < 10; n++)
    new Worker(__filename);
} else {
  bc.postMessage('hello from every worker');
  bc.close();
}
```

----------------------------------------

TITLE: Deriving Key Async with Scrypt - Node.js Crypto
DESCRIPTION: Illustrates using the asynchronous `crypto.scrypt` function to derive a cryptographic key from a password and salt using the scrypt algorithm. It shows both default options and specifying a custom cost parameter (N). The derived key is returned as a Buffer in the callback.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_52

LANGUAGE: javascript
CODE:
```
const {
  scrypt,
} = await import('node:crypto');

// Using the factory defaults.
scrypt('password', 'salt', 64, (err, derivedKey) => {
  if (err) throw err;
  console.log(derivedKey.toString('hex'));  // '3745e48...08d59ae'
});
// Using a custom N parameter. Must be a power of two.
scrypt('password', 'salt', 64, { N: 1024 }, (err, derivedKey) => {
  if (err) throw err;
  if (err) throw err;
  console.log(derivedKey.toString('hex'));  // '3745e48...aa39b34'
});
```

LANGUAGE: javascript
CODE:
```
const {
  scrypt,
} = require('node:crypto');

// Using the factory defaults.
scrypt('password', 'salt', 64, (err, derivedKey) => {
  if (err) throw err;
  console.log(derivedKey.toString('hex'));  // '3745e48...08d59ae'
});
// Using a custom N parameter. Must be a power of two.
scrypt('password', 'salt', 64, { N: 1024 }, (err, derivedKey) => {
  if (err) throw err;
  if (err) throw err;
  console.log(derivedKey.toString('hex'));  // '3745e48...aa39b34'
});
```

----------------------------------------

TITLE: Defining and Accessing Required/Optional Arguments - Commander.js - JavaScript
DESCRIPTION: Demonstrates defining required (<username>) and optional ([password]) arguments using .argument(), including a default value for the optional one. Shows how to access these arguments within the .action() callback function.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/postject-copy/node_modules/commander/Readme.md#_snippet_27

LANGUAGE: javascript
CODE:
```
program
  .version('0.1.0')
  .argument('<username>', 'user to login')
  .argument('[password]', 'password for user, if required', 'no password given')
  .action((username, password) => {
    console.log('username:', username);
    console.log('password:', password);
  });
```

----------------------------------------

TITLE: Handling Reused Socket Errors with Retry Node.js
DESCRIPTION: Demonstrates how to utilize the request.reusedSocket property to implement a basic retry mechanism for HTTP requests that might fail with an ECONNRESET error when sent over a reused socket via a keep-alive agent.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_18

LANGUAGE: mjs
CODE:
```
import http from 'node:http';
const agent = new http.Agent({ keepAlive: true });

function retriableRequest() {
  const req = http
    .get('http://localhost:3000', { agent }, (res) => {
      // ...
    })
    .on('error', (err) => {
      // Check if retry is needed
      if (req.reusedSocket && err.code === 'ECONNRESET') {
        retriableRequest();
      }
    });
}

retriableRequest();
```

LANGUAGE: cjs
CODE:
```
const http = require('node:http');
const agent = new http.Agent({ keepAlive: true });

function retriableRequest() {
  const req = http
    .get('http://localhost:3000', { agent }, (res) => {
      // ...
    })
    .on('error', (err) => {
      // Check if retry is needed
      if (req.reusedSocket && err.code === 'ECONNRESET') {
        retriableRequest();
      }
    });
}

retriableRequest();
```

----------------------------------------

TITLE: Signing Data with RSASSA-PKCS1-v1_5 in JavaScript
DESCRIPTION: This asynchronous function `sign` signs input data using a provided private key with the RSASSA-PKCS1-v1_5 algorithm. It encodes the input data using `TextEncoder` and uses `subtle.sign` to generate the signature as an `ArrayBuffer`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_12

LANGUAGE: javascript
CODE:
```
const { subtle } = globalThis.crypto;

async function sign(key, data) {
  const ec = new TextEncoder();
  const signature =
    await subtle.sign('RSASSA-PKCS1-v1_5', key, ec.encode(data));
  return signature;
}
```

----------------------------------------

TITLE: Installing locate-path Package via npm
DESCRIPTION: Installs the `locate-path` package globally or locally into your Node.js project's dependencies using the npm package manager.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/locate-path/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install locate-path

```

----------------------------------------

TITLE: Checking if a value is a Symbol using typeof in JavaScript
DESCRIPTION: The `util.isSymbol()` API is deprecated. The standard way to check if a value is a Symbol primitive in JavaScript is by using the `typeof` operator.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
typeof arg === 'symbol'
```

----------------------------------------

TITLE: Reading Subprocess Stdout Data (Node.js)
DESCRIPTION: Demonstrates how to access and read from the standard output of a spawned child process. It attaches a 'data' event listener to the `subprocess.stdout` stream, which is a Readable Stream. The listener receives and logs chunks of data as they are emitted by the child process.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_31

LANGUAGE: javascript
CODE:
```
const { spawn } = require('node:child_process');

const subprocess = spawn('ls');

subprocess.stdout.on('data', (data) => {
  console.log(`Received chunk ${data}`);
});
```

----------------------------------------

TITLE: Performing MockClient Request and Processing Response - JavaScript
DESCRIPTION: This example shows how to set up a mock response for a specific path (`/foo`) using `mockClient.intercept()` and then make a request to that path using `mockClient.request()`. It demonstrates how to receive the status code and stream the response body, converting data chunks to UTF-8 strings.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/MockClient.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import { MockAgent } from 'undici'

const mockAgent = new MockAgent({ connections: 1 })

const mockClient = mockAgent.get('http://localhost:3000')
mockClient.intercept({ path: '/foo' }).reply(200, 'foo')

const {
  statusCode,
  body
} = await mockClient.request({
  origin: 'http://localhost:3000',
  path: '/foo',
  method: 'GET'
})

console.log('response received', statusCode) // response received 200

for await (const data of body) {
  console.log('data', data.toString('utf8')) // data foo
}
```

----------------------------------------

TITLE: Creating ReadableStream from Async Iterable (Node.js CJS)
DESCRIPTION: Demonstrates creating a ReadableStream from an async generator using `ReadableStream.from()` in a CommonJS environment. An immediately invoked async function expression (IIFE) is used to handle the asynchronous operation and consume the stream.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_10

LANGUAGE: cjs
CODE:
```
const { ReadableStream } = require('node:stream/web');

async function* asyncIterableGenerator() {
  yield 'a';
  yield 'b';
  yield 'c';
}

(async () => {
  const stream = ReadableStream.from(asyncIterableGenerator());

  for await (const chunk of stream)
    console.log(chunk); // Prints: 'a', 'b', 'c'
})();
```

----------------------------------------

TITLE: Generating Heap Snapshots Near Memory Limit - Console
DESCRIPTION: This console command example demonstrates using the `--heapsnapshot-near-heap-limit` flag with Node.js. It shows how to automatically generate V8 heap snapshots when the process's memory usage approaches the limit specified by `--max-old-space-size`. The output includes messages indicating snapshot writes and the eventual fatal error due to out-of-memory.
SOURCE: https://github.com/asana/node/blob/main/doc/changelogs/CHANGELOG_V15.md#_snippet_6

LANGUAGE: console
CODE:
```
$ node --max-old-space-size=100 --heapsnapshot-near-heap-limit=3 index.js
Wrote snapshot to Heap.20200430.100036.49580.0.001.heapsnapshot
Wrote snapshot to Heap.20200430.100037.49580.0.002.heapsnapshot
Wrote snapshot to Heap.20200430.100038.49580.0.003.heapsnapshot

<--- Last few GCs --->

[49580:0x110000000]     4826 ms: Mark-sweep 130.6 (147.8) -> 130.5 (147.8) MB, 27.4 / 0.0 ms  (average mu = 0.126, current mu = 0.034) allocation failure scavenge might not succeed
[49580:0x110000000]     4845 ms: Mark-sweep 130.6 (147.8) -> 130.6 (147.8) MB, 18.8 / 0.0 ms  (average mu = 0.088, current mu = 0.031) allocation failure scavenge might not succeed


<--- JS stacktrace --->

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
....
```

----------------------------------------

TITLE: Invoking JavaScript Callback N-API C
DESCRIPTION: Allows a native add-on to call a JavaScript function object, typically after an asynchronous operation completes when no other script is on the stack. It requires an asynchronous context to maintain proper async hook tracking and takes the environment, `this` value, function object, arguments, and returns the result of the JavaScript call. Using this function is recommended over `napi_call_function` for callbacks from custom async operations.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_190

LANGUAGE: c
CODE:
```
NAPI_EXTERN napi_status napi_make_callback(napi_env env,
 napi_async_context async_context,
 napi_value recv,
 napi_value func,
 size_t argc,
 const napi_value* argv,
 napi_value* result);
```

----------------------------------------

TITLE: Using AsyncLocalStorage.snapshot()
DESCRIPTION: This snippet shows how `AsyncLocalStorage.snapshot()` captures the `AsyncLocalStorage` context active at the moment it is called. The returned function can then be called later, potentially from a different context, to execute a provided callback within the captured context, allowing `getStore()` to retrieve the original store value.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const asyncLocalStorage = new AsyncLocalStorage();
const runInAsyncScope = asyncLocalStorage.run(123, () => AsyncLocalStorage.snapshot());
const result = asyncLocalStorage.run(321, () => runInAsyncScope(() => asyncLocalStorage.getStore()));
console.log(result);  // returns 123
```

----------------------------------------

TITLE: Instantiating undici MockPool with MockAgent (JavaScript)
DESCRIPTION: Demonstrates the basic instantiation of a `MockPool` instance. A `MockAgent` is used as the factory to create a `MockPool` specific to a given origin (protocol, hostname, and port). This `MockPool` is then ready to have interception rules defined.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/MockPool.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import { MockAgent } from 'undici'

const mockAgent = new MockAgent()

const mockPool = mockAgent.get('http://localhost:3000')
```

----------------------------------------

TITLE: Piping Streams with stream.pipeline (Node.js)
DESCRIPTION: This snippet illustrates using `stream.pipeline` to connect a series of streams, ensuring proper error propagation and cleanup. It demonstrates piping a file read stream through a gzip transform stream to a file write stream and handling the final result or error in a callback.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_58

LANGUAGE: javascript
CODE:
```
const { pipeline } = require('node:stream');
const fs = require('node:fs');
const zlib = require('node:zlib');

// Use the pipeline API to easily pipe a series of streams
// together and get notified when the pipeline is fully done.

// A pipeline to gzip a potentially huge tar file efficiently:

pipeline(
  fs.createReadStream('archive.tar'),
  zlib.createGzip(),
  fs.createWriteStream('archive.tar.gz'),
  (err) => {
    if (err) {
      console.error('Pipeline failed.', err);
    } else {
      console.log('Pipeline succeeded.');
    }
  },
);
```

----------------------------------------

TITLE: Extending Writable Stream using ES6 Class (JavaScript)
DESCRIPTION: Demonstrates how to create a custom Node.js Writable stream by extending the built-in `Writable` class using ES6 syntax. The constructor calls the parent class constructor via `super(options)`. Requires the `node:stream` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_73

LANGUAGE: JavaScript
CODE:
```
const { Writable } = require('node:stream');

class MyWritable extends Writable {
  constructor(options) {
    // Calls the stream.Writable() constructor.
    super(options);
    // ...
  }
}
```

----------------------------------------

TITLE: Safe File Write Handling Existence using fs.open (Node.js)
DESCRIPTION: This snippet presents the recommended pattern for writing to a file while ensuring it doesn't already exist. It uses `fs.open` with the `'wx'` flag, which atomically checks for existence and opens the file for writing if it doesn't exist, simplifying error handling for the 'EEXIST' case.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_32

LANGUAGE: mjs
CODE:
```
import { open, close } from 'node:fs';

open('myfile', 'wx', (err, fd) => {
  if (err) {
    if (err.code === 'EEXIST') {
      console.error('myfile already exists');
      return;
    }

    throw err;
  }

  try {
    writeMyData(fd);
  } finally {
    close(fd, (err) => {
      if (err) throw err;
    });
  }
});
```

----------------------------------------

TITLE: Transform Stream with Writable Object Mode (JavaScript)
DESCRIPTION: Creates a `Transform` stream where `writableObjectMode` is set to `true`, allowing the `transform` method to receive arbitrary JavaScript values (here, numbers). The method converts the number to a hexadecimal string and pushes it onto the readable side, which is in default non-object mode, demonstrating how to handle different modes on each side of a duplex/transform stream.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_94

LANGUAGE: JavaScript
CODE:
```
const { Transform } = require('node:stream');

// All Transform streams are also Duplex Streams.
const myTransform = new Transform({
  writableObjectMode: true,

  transform(chunk, encoding, callback) {
    // Coerce the chunk to a number if necessary.
    chunk |= 0;

    // Transform the chunk into something else.
    const data = chunk.toString(16);

    // Push the data onto the readable queue.
    callback(null, '0'.repeat(data.length % 2) + data);
  },
});

myTransform.setEncoding('ascii');
myTransform.on('data', (chunk) => console.log(chunk));

myTransform.write(1);
// Prints: 01
myTransform.write(10);
// Prints: 0a
myTransform.write(100);
// Prints: 64
```

----------------------------------------

TITLE: Finding JS Files Ignoring node_modules - JavaScript
DESCRIPTION: Demonstrates using the async `glob` function to find all `.js` files recursively (`**/*.js`) while explicitly excluding the `node_modules` directory using the `ignore` option. Returns a Promise that resolves to an array of file paths. Requires the `glob` function.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/glob/README.md#_snippet_2

LANGUAGE: javascript
CODE:
```
// all js files, but don't look in node_modules
const jsfiles = await glob('**/*.js', { ignore: 'node_modules/**' })
```

----------------------------------------

TITLE: Aborting Undici Request with AbortController (JavaScript)
DESCRIPTION: Shows how to use Node.js's built-in AbortController to cancel an ongoing `undici` request. It sets up a simple HTTP server and client, makes a GET request passing the `signal` from the controller, and then explicitly calls `abort()` on the controller, triggering an error. Requires Node.js v15+.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/Dispatcher.md#_snippet_10

LANGUAGE: javascript
CODE:
```
import { createServer } from 'http';
import { Client } from 'undici';
import { once } from 'events';

const server = createServer((request, response) => {
  response.end('Hello, World!');
}).listen();

await once(server, 'listening');

const client = new Client(`http://localhost:${server.address().port}`);
const abortController = new AbortController();

try {
  client.request({
    path: '/',
    method: 'GET',
    signal: abortController.signal
  });
} catch (error) {
  console.error(error); // should print an RequestAbortedError
  client.close();
  server.close();
}

abortController.abort();
```

----------------------------------------

TITLE: Importing Builtin Module - node:events (JavaScript)
DESCRIPTION: Shows how to import a default export from a Node.js builtin module using the `node:` prefix and then instantiate the imported class.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_5

LANGUAGE: javascript
CODE:
```
import EventEmitter from 'node:events';
const e = new EventEmitter();
```

----------------------------------------

TITLE: Checking Color Support Flags Directly in JavaScript
DESCRIPTION: This JavaScript snippet shows an alternative way to use the `color-support` module by directly requiring the module and accessing its properties (`hasBasic`, `has256`, `has16m`) as flags to check for different levels of terminal color support without passing options.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/color-support/README.md#_snippet_1

LANGUAGE: javascript
CODE:
```
var colorSupport = require('color-support')

if (colorSupport.has16m) {
  console.log('\x1b[38;2;102;194;255m16m colors\x1b[0m')
} else if (colorSupport.has256) {
  console.log('\x1b[38;5;119m256 colors\x1b[0m')
} else if (colorSupport.hasBasic) {
  console.log('\x1b[31mbasic colors\x1b[0m')
} else {
  console.log('colors are not supported')
}
```

----------------------------------------

TITLE: Creating Buffer Subarray and Observing Memory Sharing
DESCRIPTION: This snippet demonstrates how to create a subarray from an existing Buffer using `buf.subarray()` and shows that modifying the original buffer affects the subarray because they share the same underlying memory.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_76

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

// Create a `Buffer` with the ASCII alphabet, take a slice, and modify one byte
// from the original `Buffer`.

const buf1 = Buffer.allocUnsafe(26);

for (let i = 0; i < 26; i++) {
  // 97 is the decimal ASCII value for 'a'.
  buf1[i] = i + 97;
}

const buf2 = buf1.subarray(0, 3);

console.log(buf2.toString('ascii', 0, buf2.length));
// Prints: abc

buf1[0] = 33;

console.log(buf2.toString('ascii', 0, buf2.length));
// Prints: !bc
```

LANGUAGE: javascript
CODE:
```
const { Buffer } = require('node:buffer');

// Create a `Buffer` with the ASCII alphabet, take a slice, and modify one byte
// from the original `Buffer`.

const buf1 = Buffer.allocUnsafe(26);

for (let i = 0; i < 26; i++) {
  // 97 is the decimal ASCII value for 'a'.
  buf1[i] = i + 97;
}

const buf2 = buf1.subarray(0, 3);

console.log(buf2.toString('ascii', 0, buf2.length));
// Prints: abc

buf1[0] = 33;

console.log(buf2.toString('ascii', 0, buf2.length));
// Prints: !bc
```

----------------------------------------

TITLE: Handling Events Only Once with 'once' (CJS)
DESCRIPTION: Demonstrates using the `eventEmitter.once()` method to register a listener that is automatically unregistered after the event is emitted and the listener is called for the first time, ensuring it runs at most once using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_11

LANGUAGE: javascript
CODE:
```
const EventEmitter = require('node:events');
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
let m = 0;
myEmitter.once('event', () => {
  console.log(++m);
});
myEmitter.emit('event');
// Prints: 1
myEmitter.emit('event');
// Ignored
```

----------------------------------------

TITLE: Enabling Capture Rejections with Constructor Option in Node.js EventEmitter (CJS)
DESCRIPTION: This snippet demonstrates using the `captureRejections: true` option in the EventEmitter constructor in CommonJS modules to handle promise rejections from async listeners. It shows routing rejections to an 'error' listener or a custom `Symbol.for('nodejs.rejection')` handler. Requires the `node:events` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_21

LANGUAGE: cjs
CODE:
```
const EventEmitter = require('node:events');
const ee1 = new EventEmitter({ captureRejections: true });
ee1.on('something', async (value) => {
  throw new Error('kaboom');
});

ee1.on('error', console.log);

const ee2 = new EventEmitter({ captureRejections: true });
ee2.on('something', async (value) => {
  throw new Error('kaboom');
});

ee2[Symbol.for('nodejs.rejection')] = console.log;
```

----------------------------------------

TITLE: Reading File Contents with Encoding Node.js
DESCRIPTION: Shows how to read a file's contents asynchronously using `fs.readFile` and specify the character encoding (e.g., 'utf8') using the options argument as a string. This ensures the data is returned as a string instead of a Buffer.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_56

LANGUAGE: mjs
CODE:
```
import { readFile } from 'node:fs';

readFile('/etc/passwd', 'utf8', callback);
```

----------------------------------------

TITLE: Inter-process Communication with Worker Messages (CJS)
DESCRIPTION: Demonstrates communication between primary and worker processes using CommonJS. Workers send a message (`process.send`) when an HTTP request is handled, and the primary listens (`worker.on('message')`) to count the total number of requests.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cluster.md#_snippet_9

LANGUAGE: JavaScript (CJS)
CODE:
```
const cluster = require('node:cluster');
const http = require('node:http');
const process = require('node:process');

if (cluster.isPrimary) {

  // Keep track of http requests
  let numReqs = 0;
  setInterval(() => {
    console.log(`numReqs = ${numReqs}`);
  }, 1000);

  // Count requests
  function messageHandler(msg) {
    if (msg.cmd && msg.cmd === 'notifyRequest') {
      numReqs += 1;
    }
  }

  // Start workers and listen for messages containing notifyRequest
  const numCPUs = require('node:os').availableParallelism();
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  for (const id in cluster.workers) {
    cluster.workers[id].on('message', messageHandler);
  }

} else {

  // Worker processes have a http server.
  http.Server((req, res) => {
    res.writeHead(200);
    res.end('hello world\n');

    // Notify primary about the request
    process.send({ cmd: 'notifyRequest' });
  }).listen(8000);
}
```

----------------------------------------

TITLE: Registering Node-API Module using Macro in C
DESCRIPTION: Registers a Node-API module using the `NAPI_MODULE` macro. This macro links the native addon's initialization function (`Init`) with the module system, making it discoverable by `require()`. `NODE_GYP_MODULE_NAME` is typically defined by the build system.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_59

LANGUAGE: C
CODE:
```
NAPI_MODULE(NODE_GYP_MODULE_NAME, Init)
```

----------------------------------------

TITLE: Generating Synchronous Random Bytes using Node.js Crypto - JavaScript
DESCRIPTION: Shows how to synchronously generate a specified number of cryptographically strong random bytes using `crypto.randomBytes`. It returns a Buffer containing the random data directly or throws an error if generation fails. This is a blocking operation.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_44

LANGUAGE: JavaScript
CODE:
```
// Synchronous
const {
  randomBytes,
} = await import('node:crypto');

const buf = randomBytes(256);
console.log(
  `${buf.length} bytes of random data: ${buf.toString('hex')}`);
```

LANGUAGE: JavaScript
CODE:
```
// Synchronous
const {
  randomBytes,
} = require('node:crypto');

const buf = randomBytes(256);
console.log(
  `${buf.length} bytes of random data: ${buf.toString('hex')}`);
```

----------------------------------------

TITLE: Checking File Stats using fs.stat Node.js (MJS)
DESCRIPTION: This snippet demonstrates how to use the asynchronous `fs.stat` function in Node.js (MJS syntax) to retrieve statistics for specified file paths. It iterates through an array of paths, calls `stat` for each, and logs whether the path points to a directory and the full stats object in the callback. It requires the `stat` function imported from the `node:fs` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_61

LANGUAGE: javascript
CODE:
```
import { stat } from 'node:fs';

const pathsToCheck = ['./txtDir', './txtDir/file.txt'];

for (let i = 0; i < pathsToCheck.length; i++) {
  stat(pathsToCheck[i], (err, stats) => {
    console.log(stats.isDirectory());
    console.log(stats);
  });
}
```

----------------------------------------

TITLE: Creating Subtest with Options in Node.js Test Runner
DESCRIPTION: Shows how to use `t.test()` to define and run a subtest within a parent test, including common options like `only`, `skip`, `concurrency`, and `todo`. The subtest function receives its own context.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_70

LANGUAGE: JavaScript
CODE:
```
test('top level test', async (t) => {
  await t.test(
    'This is a subtest',
    { only: false, skip: false, concurrency: 1, todo: false },
    (t) => {
      assert.ok('some relevant assertion here');
    },
  );
});
```

----------------------------------------

TITLE: Creating Basic TCP Echo Server in Node.js
DESCRIPTION: This JavaScript snippet demonstrates how to create a simple TCP echo server using the Node.js `net` module. It listens for connections on port 8124, logs client connection and disconnection events, sends a welcome message, and pipes received data back to the client.
SOURCE: https://github.com/asana/node/blob/main/doc/api/net.md#_snippet_11

LANGUAGE: javascript
CODE:
```
const net = require('node:net');
const server = net.createServer((c) => {
  // 'connection' listener.
  console.log('client connected');
  c.on('end', () => {
    console.log('client disconnected');
  });
  c.write('hello\r\n');
  c.pipe(c);
});
server.on('error', (err) => {
  throw err;
});
server.listen(8124, () => {
  console.log('server bound');
});
```

----------------------------------------

TITLE: Generating V8 Heap Snapshots via Signal (Shell)
DESCRIPTION: Illustrates how to enable a signal handler that triggers a heap dump when a specific signal (SIGUSR2 in this case) is received using the `--heapsnapshot-signal` flag. The example shows starting the process in the background, finding its process ID (`ps aux`), sending the signal (`kill -USR2`), and listing the generated snapshot file (`ls`).
SOURCE: https://github.com/asana/node/blob/main/doc/api/cli.md#_snippet_10

LANGUAGE: Shell
CODE:
```
node --heapsnapshot-signal=SIGUSR2 index.js &
```

LANGUAGE: Shell
CODE:
```
ps aux
```

LANGUAGE: Shell
CODE:
```
kill -USR2 1
```

LANGUAGE: Shell
CODE:
```
ls
```

----------------------------------------

TITLE: Sharing Environment Variables with Worker Threads - JavaScript
DESCRIPTION: Illustrates how passing the `SHARE_ENV` symbol as the `env` option when creating a `Worker` allows the worker and the main thread to access and modify the same environment variables object. This enables shared read/write access to process environment data across threads.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
const { Worker, SHARE_ENV } = require('node:worker_threads');
new Worker('process.env.SET_IN_WORKER = "foo"', { eval: true, env: SHARE_ENV })
  .on('exit', () => {
    console.log(process.env.SET_IN_WORKER);  // Prints 'foo'.
  });
```

----------------------------------------

TITLE: Handling Output and Errors with child_process.exec Callback in Node.js
DESCRIPTION: Illustrates how to use the callback function provided by `child_process.exec` to capture standard output (`stdout`) and standard error (`stderr`), and handle potential errors (`error`) that occur during command execution. The example runs a command with pipes and a non-existent file to demonstrate error reporting.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const { exec } = require('node:child_process');
exec('cat *.js missing_file | wc -l', (error, stdout, stderr) => {
  if (error) {
    console.error(`exec error: ${error}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});
```

----------------------------------------

TITLE: npm init Synopsis and Aliases Bash
DESCRIPTION: Provides the basic syntax for the `npm init` command, showing how it can be used with a package spec or a scope. It also lists the command's aliases.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-init.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm init <package-spec> (same as `npx <package-spec>`)
 npm init <@scope> (same as `npx <@scope>/create`)

aliases: create, innit
```

----------------------------------------

TITLE: Getting Random Integer with Range - Node.js Crypto
DESCRIPTION: Demonstrates how to generate a random integer within a specified range (inclusive minimum, exclusive maximum) using the `crypto.randomInt` function. This example uses the synchronous version but the principle applies asynchronously as well.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_51

LANGUAGE: javascript
CODE:
```
// With `min` argument
const {
  randomInt,
} = await import('node:crypto');

const n = randomInt(1, 7);
console.log(`The dice rolled: ${n}`);
```

LANGUAGE: javascript
CODE:
```
// With `min` argument
const {
  randomInt,
} = require('node:crypto');

const n = randomInt(1, 7);
console.log(`The dice rolled: ${n}`);
```

----------------------------------------

TITLE: Reading FileHandle Lines (mjs)
DESCRIPTION: This snippet demonstrates using the `readLines()` convenience method on a Node.js `FileHandle`. It creates a `readline` interface and allows asynchronous iteration over the lines of the file using `for await...of`. Each line read from the file is logged to the console.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_15

LANGUAGE: JavaScript
CODE:
```
import { open } from 'node:fs/promises';

const file = await open('./some/file/to/read');

for await (const line of file.readLines()) {
  console.log(line);
}
```

----------------------------------------

TITLE: Creating Mixed HTTPS/HTTP2 Server with ALPN Node.js
DESCRIPTION: Shows how to create a secure server that supports both HTTPS (HTTP/1) and HTTP/2 over the same port (4443) using ALPN negotiation. It requires TLS certificates and keys, sets `allowHTTP1: true`, and the request handler demonstrates how to detect which protocol (`httpVersion` and `alpnProtocol`) was negotiated for the incoming connection.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http2.md#_snippet_50

LANGUAGE: Javascript
CODE:
```
const { createSecureServer } = require('node:http2');
const { readFileSync } = require('node:fs');

const cert = readFileSync('./cert.pem');
const key = readFileSync('./key.pem');

const server = createSecureServer(
  { cert, key, allowHTTP1: true },
  onRequest,
).listen(4443);

function onRequest(req, res) {
  // Detects if it is a HTTPS request or HTTP/2
  const { socket: { alpnProtocol } } = req.httpVersion === '2.0' ?
    req.stream.session : req;
  res.writeHead(200, { 'content-type': 'application/json' });
  res.end(JSON.stringify({
    alpnProtocol,
    httpVersion: req.httpVersion,
  }));
}
```

----------------------------------------

TITLE: Setting Data Encoding to UTF8 - Node.js Readable
DESCRIPTION: Illustrates how to use `setEncoding('utf8')` to specify that data read from the stream should be decoded as UTF-8 strings rather than provided as Buffer objects. This is useful for handling text-based streams and ensures proper handling of multi-byte characters.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_37

LANGUAGE: javascript
CODE:
```
const readable = getReadableStreamSomehow();
readable.setEncoding('utf8');
readable.on('data', (chunk) => {
  assert.equal(typeof chunk, 'string');
  console.log('Got %d characters of string data:', chunk.length);
});
```

----------------------------------------

TITLE: Applying ANSI Styles (JavaScript)
DESCRIPTION: Demonstrates the basic usage of the ansi-styles library in JavaScript. It shows how to import the module and apply styles, such as green text, by concatenating the style's open and close escape codes with the string. It also includes examples of converting colors between different formats (HSL, RGB, Hex) for 16, 256, and 16 million color support.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/ansi-styles/readme.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const style = require('ansi-styles');

console.log(`${style.green.open}Hello world!${style.green.close}`);


// Color conversion between 16/256/truecolor
// NOTE: If conversion goes to 16 colors or 256 colors, the original color
//       may be degraded to fit that color palette. This means terminals
//       that do not support 16 million colors will best-match the
//       original color.
console.log(style.bgColor.ansi.hsl(120, 80, 72) + 'Hello world!' + style.bgColor.close);
console.log(style.color.ansi256.rgb(199, 20, 250) + 'Hello world!' + style.color.close);
console.log(style.color.ansi16m.hex('#abcdef') + 'Hello world!' + style.color.close);
```

----------------------------------------

TITLE: Configuring Arguments with Choices and Default - Commander.js - JavaScript
DESCRIPTION: Shows how to create and add Argument objects explicitly using new commander.Argument(). Demonstrates setting allowed choices for an argument and providing a default value along with a description of the default.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/postject-copy/node_modules/commander/Readme.md#_snippet_30

LANGUAGE: javascript
CODE:
```
program
  .addArgument(new commander.Argument('<drink-size>', 'drink cup size').choices(['small', 'medium', 'large']))
  .addArgument(new commander.Argument('[timeout]', 'timeout in seconds').default(60, 'one minute'))
```

----------------------------------------

TITLE: Consuming Events Asynchronously with events.on (Node.js)
DESCRIPTION: Demonstrates using `events.on` to iterate over events from an `EventEmitter` asynchronously. The loop processes events one by one and should not be used for concurrent execution. Requires the 'node:events' and 'node:process' modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_57

LANGUAGE: mjs
CODE:
```
import { on, EventEmitter } from 'node:events';
import process from 'node:process';

const ee = new EventEmitter();

// Emit later on
process.nextTick(() => {
  ee.emit('foo', 'bar');
  ee.emit('foo', 42);
});

for await (const event of on(ee, 'foo')) {
  // The execution of this inner block is synchronous and it
  // processes one event at a time (even with await). Do not use
  // if concurrent execution is required.
  console.log(event); // prints ['bar'] [42]
}
// Unreachable here
```

LANGUAGE: cjs
CODE:
```
const { on, EventEmitter } = require('node:events');

(async () => {
  const ee = new EventEmitter();

  // Emit later on
  process.nextTick(() => {
    ee.emit('foo', 'bar');
    ee.emit('foo', 42);
  });

  for await (const event of on(ee, 'foo')) {
    // The execution of this inner block is synchronous and it
    // processes one event at a time (even with await). Do not use
    // if concurrent execution is required.
    console.log(event); // prints ['bar'] [42]
  }
  // Unreachable here
})();
```

----------------------------------------

TITLE: Setting Up Inter-Thread Communication with MessageChannel (JavaScript)
DESCRIPTION: This snippet demonstrates how to establish custom communication channels between the main thread and a worker thread using MessageChannel. It creates a worker, a MessageChannel instance, sends one port of the channel (port1) to the worker via the worker's default channel, and retains the other port (port2) in the main thread. The worker then uses the received port to send a message back to the main thread. Requires the node:worker_threads module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_18

LANGUAGE: javascript
CODE:
```
const assert = require('node:assert');
const {
  Worker, MessageChannel, MessagePort, isMainThread, parentPort,
} = require('node:worker_threads');
if (isMainThread) {
  const worker = new Worker(__filename);
  const subChannel = new MessageChannel();
  worker.postMessage({ hereIsYourPort: subChannel.port1 }, [subChannel.port1]);
  subChannel.port2.on('message', (value) => {
    console.log('received:', value);
  });
} else {
  parentPort.once('message', (value) => {
    assert(value.hereIsYourPort instanceof MessagePort);
    value.hereIsYourPort.postMessage('the worker is sending this');
    value.hereIsYourPort.close();
  });
}
```

----------------------------------------

TITLE: Deriving Key with Sync PBKDF2 (ESM)
DESCRIPTION: This snippet shows how to use the synchronous `crypto.pbkdf2Sync` function in Node.js with ES modules. It imports the function, specifies the password, salt, iteration count (100,000), desired key length (64 bytes), and digest algorithm (SHA-512). The function returns the derived key directly, which is then converted to a hexadecimal string and logged. Requires Node.js version supporting `node:crypto` import.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_41

LANGUAGE: JavaScript
CODE:
```
const {
  pbkdf2Sync,
} = await import('node:crypto');

const key = pbkdf2Sync('secret', 'salt', 100000, 64, 'sha512');
console.log(key.toString('hex'));  // '3745e48...08d59ae'
```

----------------------------------------

TITLE: Using timersPromises.setInterval as Async Iterator (ESM and CommonJS)
DESCRIPTION: Demonstrates using the promise-based `setInterval` which returns an async iterator. The examples show how to iterate over the interval using `for await...of` in both ESM and CommonJS environments until a condition is met.
SOURCE: https://github.com/asana/node/blob/main/doc/api/timers.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import {
  setInterval,
} from 'timers/promises';

const interval = 100;
for await (const startTime of setInterval(interval, Date.now())) {
  const now = Date.now();
  console.log(now);
  if ((now - startTime) > 1000)
    break;
}
console.log(Date.now());
```

LANGUAGE: JavaScript
CODE:
```
const {
  setInterval,
} = require('node:timers/promises');
const interval = 100;

(async function() {
  for await (const startTime of setInterval(interval, Date.now())) {
    const now = Date.now();
    console.log(now);
    if ((now - startTime) > 1000)
      break;
  }
  console.log(Date.now());
})();
```

----------------------------------------

TITLE: Creating Readable Stream from Async Generator in Node.js (JS)
DESCRIPTION: Demonstrates creating a Node.js `Readable` stream from an async generator function using the `Readable.from()` utility. It requires importing the `Readable` class and shows how to consume the stream using the 'data' event listener.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_62

LANGUAGE: javascript
CODE:
```
const { Readable } = require('node:stream');

async function * generate() {
  yield 'hello';
  yield 'streams';
}

const readable = Readable.from(generate());

readable.on('data', (chunk) => {
  console.log(chunk);
});
```

----------------------------------------

TITLE: Importing Node.js Built-in Modules with node: Prefix
DESCRIPTION: This snippet demonstrates the use of the `node:` URL scheme to import Node.js built-in modules. It imports the `fs/promises` module, providing a clear and explicit way to reference core Node.js APIs within ES modules. This method ensures that the import refers to a built-in module rather than a potential package with the same name.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import fs from 'node:fs/promises';
```

----------------------------------------

TITLE: Invalid Asynchronous module.exports Assignment (Node.js)
DESCRIPTION: Illustrates that `module.exports` must be assigned synchronously at the top level of the module. Assigning it within a callback (like `setTimeout`) will not work as the module's export value is determined before the callback executes.
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_17

LANGUAGE: JavaScript
CODE:
```
setTimeout(() => {
  module.exports = { a: 'hello' };
}, 0);
```

LANGUAGE: JavaScript
CODE:
```
const x = require('./x');
console.log(x.a);
```

----------------------------------------

TITLE: Filtering Node.js Readable Stream (JavaScript)
DESCRIPTION: This snippet demonstrates using the `readable.filter` method in Node.js. It shows filtering a stream with a synchronous predicate (`x > 2`) and an asynchronous predicate that queries DNS records, utilizing the `concurrency` option to limit concurrent async operations.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_45

LANGUAGE: javascript
CODE:
```
import { Readable } from 'node:stream';
import { Resolver } from 'node:dns/promises';

// With a synchronous predicate.
for await (const chunk of Readable.from([1, 2, 3, 4]).filter((x) => x > 2)) {
  console.log(chunk); // 3, 4
}
// With an asynchronous predicate, making at most 2 queries at a time.
const resolver = new Resolver();
const dnsResults = Readable.from([
  'nodejs.org',
  'openjsf.org',
  'www.linuxfoundation.org',
]).filter(async (domain) => {
  const { address } = await resolver.resolve4(domain, { ttl: true });
  return address.ttl > 60;
}, { concurrency: 2 });
for await (const result of dnsResults) {
  // Logs domains with more than 60 seconds on the resolved dns record.
  console.log(result);
}
```

----------------------------------------

TITLE: Deriving Key with Async HKDF (ESM)
DESCRIPTION: This snippet shows how to use the asynchronous `crypto.hkdf` function in Node.js with ES modules. It imports the function, specifies the digest algorithm (SHA-512), input key material, salt, info, and desired key length (64 bytes). It then provides a callback to handle the result or error, logging the derived key as a hexadecimal string. Requires Node.js version supporting `node:crypto` import.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_35

LANGUAGE: JavaScript
CODE:
```
import { Buffer } from 'node:buffer';
const {
  hkdf,
} = await import('node:crypto');

hkdf('sha512', 'key', 'salt', 'info', 64, (err, derivedKey) => {
  if (err) throw err;
  console.log(Buffer.from(derivedKey).toString('hex'));  // '24156e2...5391653'
});
```

----------------------------------------

TITLE: Running JS File as ES Module (Bash)
DESCRIPTION: Illustrates running a JavaScript file ('my-app.js') from the command line within a directory where the nearest package.json file has the "type" field set to "module". The presence of "type": "module" ensures that the .js file is executed as an ES module by Node.js.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_35

LANGUAGE: bash
CODE:
```
node my-app.js # Runs as ES module
```

----------------------------------------

TITLE: Opening Escapable Node-API Handle Scope in C
DESCRIPTION: Opens a new escapable handle scope. This type of scope allows exactly one handle created within it to 'escape' or be promoted to the outer scope, extending its lifespan beyond the closure of the escapable scope. It takes the Node-API environment and a pointer to a napi_handle_scope to receive the new scope handle. It returns napi_ok.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_47

LANGUAGE: C
CODE:
```
NAPI_EXTERN napi_status
    napi_open_escapable_handle_scope(napi_env env,
                                     napi_handle_scope* result);
```

----------------------------------------

TITLE: AsyncLocalStorage.enterWith() with Event Handlers
DESCRIPTION: This example highlights that `enterWith()` applies the context for the *entire* synchronous execution block. If called within an event handler triggered by `emit`, subsequent handlers attached to the same event within that synchronous tick will also run within the context established by the `enterWith()` call in the first handler.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
const store = { id: 1 };

emitter.on('my-event', () => {
  asyncLocalStorage.enterWith(store);
});
emitter.on('my-event', () => {
  asyncLocalStorage.getStore(); // Returns the same object
});

asyncLocalStorage.getStore(); // Returns undefined
emitter.emit('my-event');
asyncLocalStorage.getStore(); // Returns the same object
```

----------------------------------------

TITLE: Performing Diffie-Hellman Key Exchange (Node.js)
DESCRIPTION: This snippet demonstrates how to perform a Diffie-Hellman key exchange between two parties ('Alice' and 'Bob') using Node.js's crypto module. It shows how to create DiffieHellman instances, generate public/private key pairs, exchange public keys, and compute the shared secret, asserting that the resulting secrets are identical.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_12

LANGUAGE: MJS
CODE:
```
import assert from 'node:assert';

const {
  createDiffieHellman,
} = await import('node:crypto');

// Generate Alice's keys...
const alice = createDiffieHellman(2048);
const aliceKey = alice.generateKeys();

// Generate Bob's keys...
const bob = createDiffieHellman(alice.getPrime(), alice.getGenerator());
const bobKey = bob.generateKeys();

// Exchange and generate the secret...
const aliceSecret = alice.computeSecret(bobKey);
const bobSecret = bob.computeSecret(aliceKey);

// OK
assert.strictEqual(aliceSecret.toString('hex'), bobSecret.toString('hex'));
```

LANGUAGE: CJS
CODE:
```
const assert = require('node:assert');

const {
  createDiffieHellman,
} = require('node:crypto');

// Generate Alice's keys...
const alice = createDiffieHellman(2048);
const aliceKey = alice.generateKeys();

// Generate Bob's keys...
const bob = createDiffieHellman(alice.getPrime(), alice.getGenerator());
const bobKey = bob.generateKeys();

// Exchange and generate the secret...
const aliceSecret = alice.computeSecret(bobKey);
const bobSecret = bob.computeSecret(aliceKey);

// OK
assert.strictEqual(aliceSecret.toString('hex'), bobSecret.toString('hex'));
```

----------------------------------------

TITLE: Sending Messages Between Primary and Worker in Node.js Cluster (JS)
DESCRIPTION: Demonstrates inter-process communication (IPC) between Node.js cluster primary and worker processes using `worker.send()` and `process.on('message')`. The primary forks a worker and sends a message, while the worker listens for incoming messages from the primary via `process.on('message')` and sends the received message back using `process.send()`. This requires `node:cluster` and `node:process`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cluster.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
if (cluster.isPrimary) {
  const worker = cluster.fork();
  worker.send('hi there');

} else if (cluster.isWorker) {
  process.on('message', (msg) => {
    process.send(msg);
  });
}
```

----------------------------------------

TITLE: Handling HTTP/2 Streams Node.js http2 SecureServer
DESCRIPTION: This snippet demonstrates how to listen for the 'stream' event on an Http2SecureServer. It shows how to access request headers using http2.constants, respond to the client using stream.respond(), and send data using stream.write() and stream.end(). It requires the node:http2 module and assumes server options are obtained elsewhere.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http2.md#_snippet_35

LANGUAGE: javascript
CODE:
```
const http2 = require('node:http2');
const {
  HTTP2_HEADER_METHOD,
  HTTP2_HEADER_PATH,
  HTTP2_HEADER_STATUS,
  HTTP2_HEADER_CONTENT_TYPE,
} = http2.constants;

const options = getOptionsSomehow();

const server = http2.createSecureServer(options);
server.on('stream', (stream, headers, flags) => {
  const method = headers[HTTP2_HEADER_METHOD];
  const path = headers[HTTP2_HEADER_PATH];
  // ...
  stream.respond({
    [HTTP2_HEADER_STATUS]: 200,
    [HTTP2_HEADER_CONTENT_TYPE]: 'text/plain; charset=utf-8',
  });
  stream.write('hello ');
  stream.end('world');
});
```

----------------------------------------

TITLE: Implementing Custom Inspect Method - util.inspect.custom - Node.js
DESCRIPTION: Defines a custom `[util.inspect.custom]` method on a class (`Box`) to control how instances of the class are represented as strings when inspected by `util.inspect`. The method receives inspection depth, options, and the inspect function itself, allowing recursive inspection of nested values. Requires the `node:util` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_19

LANGUAGE: JavaScript
CODE:
```
const util = require('node:util');

class Box {
  constructor(value) {
    this.value = value;
  }

  [util.inspect.custom](depth, options, inspect) {
    if (depth < 0) {
      return options.stylize('[Box]', 'special');
    }

    const newOptions = Object.assign({}, options, {
      depth: options.depth === null ? null : options.depth - 1,
    });

    // Five space padding because that's the size of "Box< ".
    const padding = ' '.repeat(5);
    const inner = inspect(this.value, newOptions)
                  .replace(/\n/g, `\n${padding}`);
    return `${options.stylize('Box', 'special')}< ${inner} >`;
  }
}

const box = new Box(true);

util.inspect(box);
// Returns: "Box< true >"
```

----------------------------------------

TITLE: Manipulating URL Search Parameters (Node.js)
DESCRIPTION: Demonstrates common operations on `URLSearchParams` associated with a `URL` object, including retrieving, appending, deleting, and setting parameters, as well as creating a new `URLSearchParams` instance from an existing one and assigning it back to the URL.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_30

LANGUAGE: js
CODE:
```
const myURL = new URL('https://example.org/?abc=123');
console.log(myURL.searchParams.get('abc'));
// Prints 123

myURL.searchParams.append('abc', 'xyz');
console.log(myURL.href);
// Prints https://example.org/?abc=123&abc=xyz

myURL.searchParams.delete('abc');
myURL.searchParams.set('a', 'b');
console.log(myURL.href);
// Prints https://example.org/?a=b

const newSearchParams = new URLSearchParams(myURL.searchParams);
// The above is equivalent to
// const newSearchParams = new URLSearchParams(myURL.search);

newSearchParams.append('a', 'c');
console.log(myURL.href);
// Prints https://example.org/?a=b
console.log(newSearchParams.toString());
// Prints a=b&a=c

// newSearchParams.toString() is implicitly called
myURL.search = newSearchParams;
console.log(myURL.href);
// Prints https://example.org/?a=b&a=c
newSearchParams.delete('a');
console.log(myURL.href);
// Prints https://example.org/?a=b&a=c
```

----------------------------------------

TITLE: Manually Migrating Buffer(number) for Node.js >= 0.12 JavaScript
DESCRIPTION: Provides a concise manual migration pattern for creating a zero-filled Buffer from a numeric size argument for Node.js versions 0.12.x and above. Uses a ternary operator to check for Buffer.alloc support, falling back to new Buffer(number).fill(0).
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const buf = Buffer.alloc ? Buffer.alloc(number) : new Buffer(number).fill(0);
```

----------------------------------------

TITLE: Getting String as UTF-8 using napi_get_value_string_utf8 (C)
DESCRIPTION: Copies the UTF8 encoded content of a JavaScript string into a provided C buffer. Returns the number of bytes copied (excluding null terminator). Returns `napi_ok` on success or `napi_string_expected` if the input is not a string. Pass `NULL` for the buffer to get the required size before allocating.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_112

LANGUAGE: C
CODE:
```
napi_status napi_get_value_string_utf8(napi_env env,
                                       napi_value value,
                                       char* buf,
                                       size_t bufsize,
                                       size_t* result);
```

----------------------------------------

TITLE: Running npm Script Across Workspaces, Ignoring Missing Scripts
DESCRIPTION: Demonstrates using the `--workspaces` flag along with the `--if-present` flag. This combination executes the specified script (`test`) in all configured workspaces but suppresses errors or warnings for workspaces whose package.json does not contain the requested script in their `scripts` section, allowing commands to succeed even if some workspaces don't define the script.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/workspaces.md#_snippet_11

LANGUAGE: bash
CODE:
```
npm run test --workspaces --if-present
```

----------------------------------------

TITLE: Closing Escapable Handle Scope (N-API C)
DESCRIPTION: This function closes the specified escapable handle scope. Scopes must be closed in the reverse order of their creation. It can be safely called even if a JavaScript exception is pending. The function takes the environment and the scope to close as input.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_48

LANGUAGE: c
CODE:
```
NAPI_EXTERN napi_status
    napi_close_escapable_handle_scope(napi_env env,
                                      napi_handle_scope scope);
```

----------------------------------------

TITLE: Defining lifecycle scripts for npm version - JSON
DESCRIPTION: Shows an example of defining `preversion`, `version`, and `postversion` scripts in the `scripts` section of a `package.json` file. These scripts execute automatically at specific stages of the `npm version` process, allowing for tasks like running tests, building assets, or pushing changes.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-version.md#_snippet_3

LANGUAGE: json
CODE:
```
{
  "scripts": {
    "preversion": "npm test",
    "version": "npm run build && git add -A dist",
    "postversion": "git push && git push --tags && rm -rf build/temp"
  }
}
```

----------------------------------------

TITLE: Implementing Custom Test Reporter (Transform Stream) - MJS
DESCRIPTION: This snippet demonstrates creating a custom test reporter using a Node.js `stream.Transform`. It processes events emitted by a `TestsStream`, such as `test:start`, `test:pass`, `test:fail`, etc., and transforms them into simple string outputs. This module uses ES Modules syntax for import and export.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_20

LANGUAGE: javascript
CODE:
```
import { Transform } from 'node:stream';

const customReporter = new Transform({
  writableObjectMode: true,
  transform(event, encoding, callback) {
    switch (event.type) {
      case 'test:dequeue':
        callback(null, `test ${event.data.name} dequeued`);
        break;
      case 'test:enqueue':
        callback(null, `test ${event.data.name} enqueued`);
        break;
      case 'test:watch:drained':
        callback(null, 'test watch queue drained');
        break;
      case 'test:start':
        callback(null, `test ${event.data.name} started`);
        break;
      case 'test:pass':
        callback(null, `test ${event.data.name} passed`);
        break;
      case 'test:fail':
        callback(null, `test ${event.data.name} failed`);
        break;
      case 'test:plan':
        callback(null, 'test plan');
        break;
      case 'test:diagnostic':
      case 'test:stderr':
      case 'test:stdout':
        callback(null, event.data.message);
        break;
      case 'test:coverage': {
        const { totalLineCount } = event.data.summary.totals;
        callback(null, `total line count: ${totalLineCount}\n`);
        break;
      }
    }
  }
});

export default customReporter;
```

----------------------------------------

TITLE: Tracing Callback-Based Functions - Node.js
DESCRIPTION: Demonstrates how to use `tracingChannel.traceCallback` to trace functions that utilize an error-first callback. This helper wraps the function, automatically emitting `start`, `end`, `asyncStart`, `asyncEnd`, and potential `error` events for the synchronous execution and the callback invocation, using a specified callback position and context.
SOURCE: https://github.com/asana/node/blob/main/doc/api/diagnostics_channel.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
import diagnostics_channel from 'node:diagnostics_channel';

const channels = diagnostics_channel.tracingChannel('my-channel');

channels.traceCallback((arg1, callback) => {
  // Do something
  callback(null, 'result');
}, 1, {
  some: 'thing',
}, thisArg, arg1, callback);
```

LANGUAGE: JavaScript
CODE:
```
const diagnostics_channel = require('node:diagnostics_channel');

const channels = diagnostics_channel.tracingChannel('my-channel');

channels.traceCallback((arg1, callback) => {
  // Do something
  callback(null, 'result');
}, {
  some: 'thing',
}, thisArg, arg1, callback);
```

----------------------------------------

TITLE: Reading Signed Little-Endian Integer of Variable Length from Buffer (ESM/CJS)
DESCRIPTION: Demonstrates reading a signed, little-endian integer of a specified byte length (1 to 6) from a Node.js Buffer at an offset using the `buf.readIntLE` method. Shows a basic example of successful reading.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_68

LANGUAGE: javascript (mjs)
CODE:
```
import { Buffer } from 'node:buffer';

const buf = Buffer.from([0x12, 0x34, 0x56, 0x78, 0x90, 0xab]);

console.log(buf.readIntLE(0, 6).toString(16));
// Prints: -546f87a9cbee
```

LANGUAGE: javascript (cjs)
CODE:
```
const { Buffer } = require('node:buffer');

const buf = Buffer.from([0x12, 0x34, 0x56, 0x78, 0x90, 0xab]);

console.log(buf.readIntLE(0, 6).toString(16));
// Prints: -546f87a9cbee
```

----------------------------------------

TITLE: Making a Listener Asynchronous (ESM)
DESCRIPTION: Demonstrates how a listener function, which is called synchronously by default, can delegate its work to run asynchronously using `setImmediate()`, allowing the event loop to continue processing other tasks.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_6

LANGUAGE: javascript
CODE:
```
import { EventEmitter } from 'node:events';
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('event', (a, b) => {
  setImmediate(() => {
    console.log('this happens asynchronously');
  });
});
myEmitter.emit('event', 'a', 'b');
```

----------------------------------------

TITLE: Installing escape-string-regexp package - Shell/npm
DESCRIPTION: This command installs the `escape-string-regexp` package from the npm registry. The `--save` flag adds it as a production dependency to the project's `package.json` file, making it available for use in your Node.js or browser-based application. It requires Node.js and npm to be installed and run from the project's root directory.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/@babel/code-frame/node_modules/escape-string-regexp/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
$ npm install --save escape-string-regexp
```

----------------------------------------

TITLE: Registering Various Event Listener Types on EventTarget (JavaScript)
DESCRIPTION: Illustrates registering different types of handlers for the 'foo' event, including standard functions, async functions, and objects implementing the `handleEvent` method. It shows how handlers are invoked with the event object and can mutate it, and also demonstrates the `once` option.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_63

LANGUAGE: javascript
CODE:
```
function handler1(event) {
  console.log(event.type);  // Prints 'foo'
  event.a = 1;
}

async function handler2(event) {
  console.log(event.type);  // Prints 'foo'
  console.log(event.a);  // Prints 1
}

const handler3 = {
  handleEvent(event) {
    console.log(event.type);  // Prints 'foo'
  },
};

const handler4 = {
  async handleEvent(event) {
    console.log(event.type);  // Prints 'foo'
  },
};

const target = new EventTarget();

target.addEventListener('foo', handler1);
target.addEventListener('foo', handler2);
target.addEventListener('foo', handler3);
target.addEventListener('foo', handler4, { once: true });
```

----------------------------------------

TITLE: Initializing Scoped NPM Package - Bash
DESCRIPTION: This snippet shows how to use `npm init` to create a new package with a specific scope (`@foo`). The `--scope` flag ensures the package name will follow the `@scope/package-name` format, and `--yes` accepts all default prompts.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-logout.md#_snippet_2

LANGUAGE: bash
CODE:
```
# accept all defaults, and create a package named "@foo/whatever",
# instead of just named "whatever"
npm init --scope=@foo --yes
```

----------------------------------------

TITLE: setTime Affects Date but Not Timers Directly - ES Modules
DESCRIPTION: This snippet illustrates that calling `context.mock.timers.setTime()` only advances the mocked `Date` object and does not cause any pending `setTimeout` or `setInterval` timers to tick or execute. Timers are only triggered by `tick` or `runAll`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_60

LANGUAGE: javascript
CODE:
```
import assert from 'node:assert';
import { test } from 'node:test';

test('runAll functions following the given order', (context) => {
  context.mock.timers.enable({ apis: ['setTimeout', 'Date'] });
  const results = [];
  setTimeout(() => results.push(1), 9999);

  assert.deepStrictEqual(results, []);
  context.mock.timers.setTime(12000);
  assert.deepStrictEqual(results, []);
  // The date is advanced but the timers don't tick
  assert.strictEqual(Date.now(), 12000);
});
```

----------------------------------------

TITLE: Implementing Type-Ahead with rl.line (Node.js)
DESCRIPTION: This example illustrates how to use the `rl.line` property to get the current input string from a TTY stream before the line is fully submitted. It sets up a readline interface reading from `process.stdin`, defines a debounce function to filter a list of values based on the current input (`rl.line`), and triggers this debounce function on each `keypress` event. This allows providing suggestions as the user types.
SOURCE: https://github.com/asana/node/blob/main/doc/api/readline.md#_snippet_15

LANGUAGE: JavaScript
CODE:
```
const values = ['lorem ipsum', 'dolor sit amet'];
const rl = readline.createInterface(process.stdin);
const showResults = debounce(() => {
  console.log(
    '\n',
    values.filter((val) => val.startsWith(rl.line)).join(' '),
  );
}, 300);
process.stdin.on('keypress', (c, k) => {
  showResults();
});
```

----------------------------------------

TITLE: Custom Inspect Returning Object - util.inspect.custom - Node.js
DESCRIPTION: Illustrates that a custom `[util.inspect.custom]` method does not have to return a string. If it returns a value of any other type, `util.inspect` will then format that returned value instead of the original object, effectively replacing the inspection target. Requires the `node:util` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_20

LANGUAGE: JavaScript
CODE:
```
const util = require('node:util');

const obj = { foo: 'this will not show up in the inspect() output' };
obj[util.inspect.custom] = (depth) => {
  return { bar: 'baz' };
};

util.inspect(obj);
// Returns: "{ bar: 'baz' }"
```

----------------------------------------

TITLE: Handling HTTP Request with Node.js Streams: Javascript
DESCRIPTION: Demonstrates creating a basic HTTP server that reads the request body using a readable stream (`req`), processes it (parses JSON), and writes a response using a writable stream (`res`). It shows handling 'data' and 'end' events on the readable stream to collect the full request body before responding.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_11

LANGUAGE: javascript
CODE:
```
const http = require('node:http');

const server = http.createServer((req, res) => {
  // `req` is an http.IncomingMessage, which is a readable stream.
  // `res` is an http.ServerResponse, which is a writable stream.

  let body = '';
  // Get the data as utf8 strings.
  // If an encoding is not set, Buffer objects will be received.
  req.setEncoding('utf8');

  // Readable streams emit 'data' events once a listener is added.
  req.on('data', (chunk) => {
    body += chunk;
  });

  // The 'end' event indicates that the entire body has been received.
  req.on('end', () => {
    try {
      const data = JSON.parse(body);
      // Write back something interesting to the user:
      res.write(typeof data);
      res.end();
    } catch (er) {
      // uh oh! bad json!
      res.statusCode = 400;
      return res.end(`error: ${er.message}`);
    }
  });
});

server.listen(1337);

// $ curl localhost:1337 -d "{}"
// object
// $ curl localhost:1337 -d "\"foo\""
// string
// $ curl localhost:1337 -d "not json"
// error: Unexpected token 'o', "not json" is not valid JSON
```

----------------------------------------

TITLE: Running Node.js Tests in Watch Mode
DESCRIPTION: Command line instruction to start the Node.js test runner in watch mode using the `--watch` flag. This allows the runner to automatically rerun tests when relevant files change.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_13

LANGUAGE: bash
CODE:
```
node --test --watch
```

----------------------------------------

TITLE: Generating Async RSA Key Pair - Node.js Crypto
DESCRIPTION: Generates an RSA asymmetric key pair asynchronously using the `crypto.generateKeyPair` function. It specifies the modulus length and encoding options for the public and private keys, recommending SPKI and encrypted PKCS8 formats respectively. The result is handled via a callback function.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_28

LANGUAGE: JavaScript
CODE:
```
const {
  generateKeyPair,
} = await import('node:crypto');

generateKeyPair('rsa', {
  modulusLength: 4096,
  publicKeyEncoding: {
    type: 'spki',
    format: 'pem',
  },
  privateKeyEncoding: {
    type: 'pkcs8',
    format: 'pem',
    cipher: 'aes-256-cbc',
    passphrase: 'top secret',
  },
}, (err, publicKey, privateKey) => {
  // Handle errors and use the generated key pair.
});
```

LANGUAGE: JavaScript
CODE:
```
const {
  generateKeyPair,
} = require('node:crypto');

generateKeyPair('rsa', {
  modulusLength: 4096,
  publicKeyEncoding: {
    type: 'spki',
    format: 'pem',
  },
  privateKeyEncoding: {
    type: 'pkcs8',
    format: 'pem',
    cipher: 'aes-256-cbc',
    passphrase: 'top secret',
  },
}, (err, publicKey, privateKey) => {
  // Handle errors and use the generated key pair.
});
```

----------------------------------------

TITLE: Converting Fetch Web Stream Body to Node.js Stream
DESCRIPTION: Explains how to convert the web stream returned by undici.fetch's response.body into a Node.js-compatible stream using Readable.fromWeb(), which is available in the Node.js 'stream' module. This is useful for integrating with existing Node.js stream-based APIs.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/README.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import { fetch } from 'undici'
import { Readable } from 'node:stream'

const response = await fetch('https://example.com')
const readableWebStream = response.body
const readableNodeStream = Readable.fromWeb(readableWebStream)
```

----------------------------------------

TITLE: Testing Caught Exception Handling - Javascript
DESCRIPTION: This function demonstrates catching an exception using a `try...catch` block. It calls `throwException` within the `try` block and includes a `return` statement in the `catch` block to handle the error gracefully. This tests debugger behavior during exception propagation and catching.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/inspector/debugger/get-possible-breakpoints-main-expected.txt#_snippet_29

LANGUAGE: javascript
CODE:
```
function testCaughtException() {
  try {
    throwException()
  } catch (e) {
    return;
  }
}
```

----------------------------------------

TITLE: Unhandled EventEmitter Error Crash (JavaScript)
DESCRIPTION: Shows the consequence of emitting an `'error'` event on an `EventEmitter` instance when no `'error'` handler is registered. This results in an uncaught exception being thrown, which typically causes the Node.js process to exit unless a global `process.on('uncaughtException', ...)` handler is present.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const EventEmitter = require('node:events');
const ee = new EventEmitter();

setImmediate(() => {
  // This will crash the process because no 'error' event
  // handler has been added.
  ee.emit('error', new Error('This will crash'));
});
```

----------------------------------------

TITLE: Resolving IPv4 and Performing Reverse Lookup - JavaScript
DESCRIPTION: Shows how to use `dns.resolve4` to get all IPv4 addresses for a hostname via a network DNS query. It then iterates through the resolved addresses to perform a reverse DNS lookup (`dns.reverse`) for each one, finding associated hostnames.
SOURCE: https://github.com/asana/node/blob/main/doc/api/dns.md#_snippet_1

LANGUAGE: js
CODE:
```
const dns = require('node:dns');

dns.resolve4('archive.org', (err, addresses) => {
  if (err) throw err;

  console.log(`addresses: ${JSON.stringify(addresses)}`);

  addresses.forEach((a) => {
    dns.reverse(a, (err, hostnames) => {
      if (err) {
        throw err;
      }
      console.log(`reverse for ${a}: ${JSON.stringify(hostnames)}`);
    });
  });
});
```

----------------------------------------

TITLE: Invoke JavaScript Callback from C++ Addon
DESCRIPTION: This C++ code defines a `RunCallback` function that accepts a JavaScript function as its first argument. It demonstrates how to cast the V8 `Value` to a `Function` and then call it using `cb->Call()`, passing arguments from C++. The addon exports this functionality, replacing the default `exports` object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/addons.md#_snippet_16

LANGUAGE: cpp
CODE:
```
// addon.cc
#include <node.h>

namespace demo {

using v8::Context;
using v8::Function;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Null;
using v8::Object;
using v8::String;
using v8::Value;

void RunCallback(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  Local<Context> context = isolate->GetCurrentContext();
  Local<Function> cb = Local<Function>::Cast(args[0]);
  const unsigned argc = 1;
  Local<Value> argv[argc] = {
      String::NewFromUtf8(isolate,
                          "hello world").ToLocalChecked() };
  cb->Call(context, Null(isolate), argc, argv).ToLocalChecked();
}

void Init(Local<Object> exports, Local<Object> module) {
  NODE_SET_METHOD(module, "exports", RunCallback);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, Init)

}  // namespace demo
```

----------------------------------------

TITLE: Adding Simple Event Listener to EventTarget (JavaScript)
DESCRIPTION: Shows how to create a new `EventTarget` instance and attach a basic event listener for a specific event type ('foo'). The listener is an arrow function that logs a message to the console when the event occurs. This is a fundamental example of using the `addEventListener` method.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_62

LANGUAGE: javascript
CODE:
```
const target = new EventTarget();

target.addEventListener('foo', (event) => {
  console.log('foo event happened!');
});
```

----------------------------------------

TITLE: Dispatching POST Request with Undici Client in Node.js
DESCRIPTION: Demonstrates sending a POST request with a JSON body using an `undici` `Client`. A Node.js HTTP server is configured to listen for POST requests, read the incoming data, parse it as JSON, modify it, and respond with the updated JSON. The client uses event handlers like `onConnect`, `onError`, `onHeaders`, `onData`, and `onComplete` to manage the request/response cycle, collect the response data chunks, and process the final response. Requires `undici`, `http`, and `events`.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/Dispatcher.md#_snippet_7

LANGUAGE: javascript
CODE:
```
import { createServer } from 'http'
import { Client } from 'undici'
import { once } from 'events'

const server = createServer((request, response) => {
  request.on('data', (data) => {
    console.log(`Request Data: ${data.toString('utf8')}`)
    const body = JSON.parse(data)
    body.message = 'World'
    response.end(JSON.stringify(body))
  })
}).listen()

await once(server, 'listening')

const client = new Client(`http://localhost:${server.address().port}`)

const data = []

client.dispatch({
  path: '/',
  method: 'POST',
  headers: {
    'content-type': 'application/json'
  },
  body: JSON.stringify({ message: 'Hello' })
}, {
  onConnect: () => {
    console.log('Connected!')
  },
  onError: (error) => {
    console.error(error)
  },
  onHeaders: (statusCode, headers) => {
    console.log(`onHeaders | statusCode: ${statusCode} | headers: ${headers}`)
  },
  onData: (chunk) => {
    console.log('onData: chunk received')
    data.push(chunk)
  },
  onComplete: (trailers) => {
    console.log(`onComplete | trailers: ${trailers}`)
    const res = Buffer.concat(data).toString('utf8')
    console.log(`Response Data: ${res}`)
    client.close()
    server.close()
  }
})
```

----------------------------------------

TITLE: AsyncLocalStorage.run() with Async/Await Pattern
DESCRIPTION: This snippet demonstrates the correct pattern for using `asyncLocalStorage.run()` within an `async` function when the asynchronous operation being awaited (`foo()`) should run within the context established by `run()`. The `await` call is placed *before* `run()`, and the function to be awaited is returned from the `run` callback, ensuring the context is active during the awaited operation.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
async function fn() {
  await asyncLocalStorage.run(new Map(), () => {
    asyncLocalStorage.getStore().set('key', value);
    return foo(); // The return value of foo will be awaited
  });
}
```

----------------------------------------

TITLE: Handling Async Promise Rejections with Try/Catch (JavaScript)
DESCRIPTION: Shows how to handle errors from asynchronous Promise-based APIs in Node.js, specifically using `fs/promises.readFile`. A `try...catch` block within an `async` function is used to catch potential Promise rejections, such as a file-not-found error, and handle them gracefully.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const fs = require('fs/promises');

(async () => {
  let data;
  try {
    data = await fs.readFile('a file that does not exist');
  } catch (err) {
    console.error('There was an error reading the file!', err);
    return;
  }
  // Otherwise handle the data
})();
```

----------------------------------------

TITLE: Creating Directory Recursively - Node.js MJS
DESCRIPTION: This example shows how to use `fsPromises.mkdir` from `node:fs/promises` to create a directory, including any necessary parent directories, by setting the `recursive` option to `true`. It demonstrates using a `URL` for the path and logs the path of the first directory created or any error encountered.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_20

LANGUAGE: mjs
CODE:
```
import { mkdir } from 'node:fs/promises';

try {
  const projectFolder = new URL('./test/project/', import.meta.url);
  const createDir = await mkdir(projectFolder, { recursive: true });

  console.log(`created ${createDir}`);
} catch (err) {
  console.error(err.message);
}
```

----------------------------------------

TITLE: Importing JS File as ES Module (JavaScript)
DESCRIPTION: Demonstrates an ES module 'import' statement within a JavaScript file that belongs to a package configured with "type": "module" in its package.json. The 'import' statement correctly loads the './startup.js' file as an ES module due to the package's default module type setting.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_36

LANGUAGE: javascript
CODE:
```
import './startup.js'; // Loaded as ES module because of package.json
```

----------------------------------------

TITLE: Mocking and Ticking setTimeout - CommonJS
DESCRIPTION: This snippet demonstrates how to enable mocking for `setTimeout` (from global, `node:timers`, and `node:timers/promises`) and advance the mocked time using `context.mock.timers.tick` to synchronously trigger the timers within a Node.js test, using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_53

LANGUAGE: javascript
CODE:
```
const assert = require('node:assert');
const { test } = require('node:test');
const nodeTimers = require('node:timers');
const nodeTimersPromises = require('node:timers/promises');

test('mocks setTimeout to be executed synchronously without having to actually wait for it', async (context) => {
  const globalTimeoutObjectSpy = context.mock.fn();
  const nodeTimerSpy = context.mock.fn();
  const nodeTimerPromiseSpy = context.mock.fn();

  // Optionally choose what to mock
  context.mock.timers.enable({ apis: ['setTimeout'] });
  setTimeout(globalTimeoutObjectSpy, 9999);
  nodeTimers.setTimeout(nodeTimerSpy, 9999);

  const promise = nodeTimersPromises.setTimeout(9999).then(nodeTimerPromiseSpy);

  // Advance in time
  context.mock.timers.tick(9999);
  assert.strictEqual(globalTimeoutObjectSpy.mock.callCount(), 1);
  assert.strictEqual(nodeTimerSpy.mock.callCount(), 1);
  await promise;
  assert.strictEqual(nodeTimerPromiseSpy.mock.callCount(), 1);
});
```

----------------------------------------

TITLE: Defining Module Entry Point using package.json (JSON)
DESCRIPTION: Demonstrates how to specify the main file for a module using the `main` field in a `package.json` file. This allows `require()` to load the specified file when requiring the folder.
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_7

LANGUAGE: JSON
CODE:
```
{
  "name" : "some-library",
  "main" : "./lib/some-library.js"
}
```

----------------------------------------

TITLE: Demonstrating nopt Command Line Examples in Console
DESCRIPTION: This snippet provides console examples showing how to run the `my-program.js` script and the resulting output produced by `console.log(parsed)`. It illustrates various scenarios including passing options with values, using `--no-` prefix to negate booleans, using single-character and combined shorthands, using the `--` separator to stop parsing, handling unknown options, options with equals signs, and how array-typed options accumulate multiple values.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/nopt/README.md#_snippet_1

LANGUAGE: console
CODE:
```
$ node my-program.js --foo "blerp" --no-flag
{ "foo" : "blerp", "flag" : false }

$ node my-program.js ---bar 7 --foo "Mr. Hand" --flag
{ bar: 7, foo: "Mr. Hand", flag: true }

$ node my-program.js --foo "blerp" -f -----p
{ foo: "blerp", flag: true, pick: true }

$ node my-program.js -fp --foofoo
{ foo: "Mr. Foo", flag: true, pick: true }

$ node my-program.js --foofoo -- -fp  # -- stops the flag parsing.
{ foo: "Mr. Foo", argv: { remain: ["-fp"] } }

$ node my-program.js --blatzk -fp # unknown opts are ok.
{ blatzk: true, flag: true, pick: true }

$ node my-program.js --blatzk=1000 -fp # but you need to use = if they have a value
{ blatzk: 1000, flag: true, pick: true }

$ node my-program.js --no-blatzk -fp # unless they start with "no-"
{ blatzk: false, flag: true, pick: true }

$ node my-program.js --baz b/a/z # known paths are resolved.
{ baz: "/Users/isaacs/b/a/z" }

# if Array is one of the types, then it can take many
# values, and will always be an array.  The other types provided
# specify what types are allowed in the list.

$ node my-program.js --many1 5 --many1 null --many1 foo
{ many1: ["5", "null", "foo"] }

$ node my-program.js --many2 foo --many2 bar
{ many2: ["/path/to/foo", "path/to/bar"] }
```

----------------------------------------

TITLE: Creating a JavaScript Promise
DESCRIPTION: This snippet creates a new JavaScript Promise in a pending state. It shows the basic syntax for instantiating a Promise with an executor function.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/inspector/runtime/deep-serialization-local-references-expected.txt#_snippet_31

LANGUAGE: javascript
CODE:
```
new Promise(()=>{})
```

----------------------------------------

TITLE: Comparing Node.js Buffers with compare (MJS)
DESCRIPTION: Demonstrates the basic usage of `buf.compare()` to compare different Buffer instances. It also shows how to use `Buffer.compare()` as a sorting function for an array of Buffers.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_34

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

const buf1 = Buffer.from('ABC');
const buf2 = Buffer.from('BCD');
const buf3 = Buffer.from('ABCD');

console.log(buf1.compare(buf1));
// Prints: 0
console.log(buf1.compare(buf2));
// Prints: -1
console.log(buf1.compare(buf3));
// Prints: -1
console.log(buf2.compare(buf1));
// Prints: 1
console.log(buf2.compare(buf3));
// Prints: 1
console.log([buf1, buf2, buf3].sort(Buffer.compare));
// Prints: [ <Buffer 41 42 43>, <Buffer 41 42 43 44>, <Buffer 42 43 44> ]
// (This result is equal to: [buf1, buf3, buf2].)
```

----------------------------------------

TITLE: Defining a Script using Local Binary
DESCRIPTION: Shows an example of defining a script in the package.json 'scripts' section that utilizes a binary installed locally as a dependency (e.g., 'tap'). npm automatically adds 'node_modules/.bin' to the PATH when running scripts, allowing direct use of the binary name.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-run-script.md#_snippet_2

LANGUAGE: json
CODE:
```
"scripts": {"test": "tap test/*.js"}
```

----------------------------------------

TITLE: Configuring Dual Package with Conditional Exports (JSON)
DESCRIPTION: Defines the package.json for a dual CommonJS/ES module package (pkg) using the "ES module wrapper" approach. It uses conditional "exports" to route import requests to ./wrapper.mjs and require requests to ./index.cjs, minimizing the dual package hazard.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_20

LANGUAGE: json
CODE:
```
// ./node_modules/pkg/package.json
{
  "type": "module",
  "exports": {
    "import": "./wrapper.mjs",
    "require": "./index.cjs"
  }
}
```

----------------------------------------

TITLE: Creating ReadStream from FileHandle (Byte Range) [MJS]
DESCRIPTION: Demonstrates creating a readable stream from a `FileHandle` using `createReadStream` with the `start` and `end` options to read a specific range of bytes (inclusive) from the file instead of the entire content.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_12

LANGUAGE: javascript
CODE:
```
import { open } from 'node:fs/promises';

const fd = await open('sample.txt');
fd.createReadStream({ start: 90, end: 99 });
```

----------------------------------------

TITLE: Incrementing Version with Prerelease Identifier (JS)
DESCRIPTION: Uses the `semver.inc` function in JavaScript to increment a version by 'prerelease' level and append a specified identifier (e.g., 'beta'). The first call with an identifier initializes the prerelease sequence.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/node_modules/semver/README.md#_snippet_3

LANGUAGE: js
CODE:
```
semver.inc('1.2.3', 'prerelease', 'beta')
// '1.2.4-beta.0'
```

----------------------------------------

TITLE: Example: Uninstalling Without Saving in Bash
DESCRIPTION: Shows how to uninstall a package (`lodash`) without modifying the project's `package.json` or lock files. This is achieved by using the `--no-save` flag, which overrides the default save behavior.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-uninstall.md#_snippet_2

LANGUAGE: bash
CODE:
```
npm uninstall lodash --no-save
```

----------------------------------------

TITLE: Using stream.pipeline with Async Generator Transformer (Promises API, ESM)
DESCRIPTION: Shows how to include a custom async generator as a transformation step in `stream.pipeline` using ES Module syntax. The generator processes chunks from the previous source stream and yields transformed data to the next destination.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_6

LANGUAGE: javascript
CODE:
```
import { pipeline } from 'node:stream/promises';
import { createReadStream, createWriteStream } from 'node:fs';

await pipeline(
  createReadStream('lowercase.txt'),
  async function* (source, { signal }) {
    source.setEncoding('utf8');  // Work with strings rather than `Buffer`s.
    for await (const chunk of source) {
      yield await processChunk(chunk, { signal });
    }
  },
  createWriteStream('uppercase.txt'),
);
console.log('Pipeline succeeded.');
```

----------------------------------------

TITLE: Importing node:url Module (CJS)
DESCRIPTION: Demonstrates how to require the built-in `node:url` module using CommonJS syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const url = require('node:url');
```

----------------------------------------

TITLE: Reading Directory Contents with fsPromises.readdir (Node.js mjs)
DESCRIPTION: Shows how to asynchronously read the names of files and directories within a given path using `fsPromises.readdir`. Requires importing `readdir` from `node:fs/promises`. Iterates through the resulting array and logs each name. Includes basic error handling.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_24

LANGUAGE: javascript
CODE:
```
import { readdir } from 'node:fs/promises';

try {
  const files = await readdir(path);
  for (const file of files)
    console.log(file);
} catch (err) {
  console.error(err);
}
```

----------------------------------------

TITLE: Transferring and Copying Buffers/Ports with Node.js MessagePort
DESCRIPTION: This extensive snippet shows different ways to handle binary data and ports when sending messages using `MessagePort.postMessage()`. It demonstrates posting a copy of a `Uint8Array`, transferring the underlying `ArrayBuffer` to make the original unusable, sharing data via `SharedArrayBuffer`, and transferring a `MessagePort` instance using the `transferList` option. Requires `node:worker_threads`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/worker_threads.md#_snippet_14

LANGUAGE: js
CODE:
```
const { MessageChannel } = require('node:worker_threads');
const { port1, port2 } = new MessageChannel();

port1.on('message', (message) => console.log(message));

const uint8Array = new Uint8Array([ 1, 2, 3, 4 ]);
// This posts a copy of `uint8Array`:
port2.postMessage(uint8Array);
// This does not copy data, but renders `uint8Array` unusable:
port2.postMessage(uint8Array, [ uint8Array.buffer ]);

// The memory for the `sharedUint8Array` is accessible from both the
// original and the copy received by `.on('message')`:
const sharedUint8Array = new Uint8Array(new SharedArrayBuffer(4));
port2.postMessage(sharedUint8Array);

// This transfers a freshly created message port to the receiver.
// This can be used, for example, to create communication channels between
// multiple `Worker` threads that are children of the same parent thread.
const otherChannel = new MessageChannel();
port2.postMessage({ port: otherChannel.port1 }, [ otherChannel.port1 ]);
```

----------------------------------------

TITLE: Using URL Object in http.request Options - Node.js
DESCRIPTION: This snippet demonstrates a simplified way to configure an `http.request` by passing a WHATWG `URL` object directly to the `options` parameter. This method allows specifying the protocol, host, port, and authentication details concisely within the URL string itself.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_56

LANGUAGE: js
CODE:
```
const options = new URL('http://abc:xyz@example.com');

const req = http.request(options, (res) => {
  // ...
});
```

----------------------------------------

TITLE: Resolving Paths with Absolute Segment using Node.js path.resolve
DESCRIPTION: This snippet shows how `path.resolve()` handles combining an absolute path (`/foo/bar`) with another absolute path (`/tmp/file/`). When an absolute path segment is encountered, subsequent segments are resolved from that point, resulting in the final absolute path from the rightmost absolute segment.
SOURCE: https://github.com/asana/node/blob/main/doc/api/path.md#_snippet_5

LANGUAGE: javascript
CODE:
```
path.resolve('/foo/bar', '/tmp/file/');
```

----------------------------------------

TITLE: Importing fs/promises Module [MJS]
DESCRIPTION: Imports the promise-based API of the Node.js file system module using ES module syntax. This provides asynchronous methods that return Promises.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import * as fs from 'node:fs/promises';
```

----------------------------------------

TITLE: Example package.json with Tilde Dependency (JSON)
DESCRIPTION: This snippet shows a 'dependencies' section in a package.json file using the tilde (`~`) semver range for 'dep1'. The tilde restricts updates to the latest patch version within the specified minor version (1.1.x in this case).
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-update.md#_snippet_3

LANGUAGE: json
CODE:
```
"dependencies": {
  "dep1": "~1.1.1"
}
```

----------------------------------------

TITLE: Generating ECDSA Key Pair in JavaScript
DESCRIPTION: This asynchronous function `generateEcKey` generates an ECDSA public and private key pair using `subtle.generateKey` for a specified named curve (defaulting to 'P-521'). The keys are extractable and intended for signing/verification operations.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webcrypto.md#_snippet_2

LANGUAGE: javascript
CODE:
```
const { subtle } = globalThis.crypto;

async function generateEcKey(namedCurve = 'P-521') {
  const {
    publicKey,
    privateKey,
  } = await subtle.generateKey({
    name: 'ECDSA',
    namedCurve,
  }, true, ['sign', 'verify']);

  return { publicKey, privateKey };
}
```

----------------------------------------

TITLE: Consuming Stream to Blob (ES Modules)
DESCRIPTION: Shows how to use the 'blob' consumer to read a stream (specifically from a Blob instance here) into a new Blob using ES Modules. It requires importing the 'blob' consumer and uses 'await' for asynchronous processing.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_20

LANGUAGE: javascript
CODE:
```
import { blob } from 'node:stream/consumers';

const dataBlob = new Blob(['hello world from consumers!']);

const readable = dataBlob.stream();
const data = await blob(readable);
console.log(`from readable: ${data.size}`);
// Prints: from readable: 27
```

----------------------------------------

TITLE: Using stream.pipeline with Async Generator Transformer (Promises API, CJS)
DESCRIPTION: Illustrates integrating a custom transform step implemented as an async generator function within `stream.pipeline` using CommonJS. The generator receives the source stream and an options object containing the `signal`, allowing custom data processing like encoding changes.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_5

LANGUAGE: javascript
CODE:
```
const { pipeline } = require('node:stream/promises');
const fs = require('node:fs');

async function run() {
  await pipeline(
    fs.createReadStream('lowercase.txt'),
    async function* (source, { signal }) {
      source.setEncoding('utf8');  // Work with strings rather than `Buffer`s.
      for await (const chunk of source) {
        yield await processChunk(chunk, { signal });
      }
    },
    fs.createWriteStream('uppercase.txt'),
  );
  console.log('Pipeline succeeded.');
}

run().catch(console.error);
```

----------------------------------------

TITLE: Reading File Content with fsPromises.readFile and path.resolve (Node.js cjs)
DESCRIPTION: Shows reading the entire content of a file asynchronously using `fsPromises.readFile` in a CommonJS environment. Uses `require` to import `readFile` and `path.resolve` to construct the file path. Specifies 'utf8' encoding and wraps the asynchronous call in an `async` function. Includes error handling.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_26

LANGUAGE: javascript
CODE:
```
const { readFile } = require('node:fs/promises');
const { resolve } = require('node:path');
async function logFile() {
  try {
    const filePath = resolve('./package.json');
    const contents = await readFile(filePath, { encoding: 'utf8' });
    console.log(contents);
  } catch (err) {
    console.error(err.message);
  }
}
logFile();
```

----------------------------------------

TITLE: Reading File Content with fsPromises.readFile and URL (Node.js mjs)
DESCRIPTION: Illustrates reading the entire content of a file asynchronously using `fsPromises.readFile`. Demonstrates constructing a file path using `URL` and `import.meta.url` for ES Modules. Specifies 'utf8' encoding to get string output. Includes error handling.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_25

LANGUAGE: javascript
CODE:
```
import { readFile } from 'node:fs/promises';
try {
  const filePath = new URL('./package.json', import.meta.url);
  const contents = await readFile(filePath, { encoding: 'utf8' });
  console.log(contents);
} catch (err) {
  console.error(err.message);
}
```

----------------------------------------

TITLE: Promisifying child_process.execFile with Async/Await - Node.js
DESCRIPTION: This snippet shows how to use `util.promisify` to convert the callback-based `execFile` function into a Promise-based one. This allows using async/await syntax for cleaner handling of asynchronous command execution, logging the standard output upon successful completion.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_8

LANGUAGE: js
CODE:
```
const util = require('node:util');
const execFile = util.promisify(require('node:child_process').execFile);
async function getVersion() {
  const { stdout } = await execFile('node', ['--version']);
  console.log(stdout);
}
getVersion();
```

----------------------------------------

TITLE: Performing dnsPromises.lookup
DESCRIPTION: Shows how to use `dnsPromises.lookup()` to resolve a hostname to an IP address. It demonstrates providing options like `family` and `hints`, and how to handle the result as a single object or an array of objects when the `all` option is set to true.
SOURCE: https://github.com/asana/node/blob/main/doc/api/dns.md#_snippet_12

LANGUAGE: js
CODE:
```
const dns = require('node:dns');
const dnsPromises = dns.promises;
const options = {
  family: 6,
  hints: dns.ADDRCONFIG | dns.V4MAPPED,
};

dnsPromises.lookup('example.com', options).then((result) => {
  console.log('address: %j family: IPv%s', result.address, result.family);
  // address: "2606:2800:220:1:248:1893:25c8:1946" family: IPv6
});

// When options.all is true, the result will be an Array.
options.all = true;
dnsPromises.lookup('example.com', options).then((result) => {
  console.log('addresses: %j', result);
  // addresses: [{"address":"2606:2800:220:1:248:1893:25c8:1946","family":6}]
});
```

----------------------------------------

TITLE: Creating CommonJS require in ESM
DESCRIPTION: Illustrates how to use `module.createRequire` within an ES Module file (`.mjs`) to create a CommonJS `require` function. This allows importing CommonJS modules from within an ESM context.
SOURCE: https://github.com/asana/node/blob/main/doc/api/module.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { createRequire } from 'node:module';
const require = createRequire(import.meta.url);

// sibling-module.js is a CommonJS module.
const siblingModule = require('./sibling-module');
```

----------------------------------------

TITLE: Detaching Process and Redirecting stdio to Files
DESCRIPTION: Demonstrates how to spawn a detached child process (`detached: true`) while redirecting its standard output and standard error to specified log files using the `stdio` option with file descriptors obtained via `fs.openSync()`. `subprocess.unref()` is used to allow the parent to exit independently.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_17

LANGUAGE: javascript
CODE:
```
const fs = require('node:fs');
const { spawn } = require('node:child_process');
const out = fs.openSync('./out.log', 'a');
const err = fs.openSync('./out.log', 'a');

const subprocess = spawn('prg', [], {
  detached: true,
  stdio: [ 'ignore', out, err ],
});

subprocess.unref();
```

----------------------------------------

TITLE: Copying Data Between Node.js Buffers with copy (MJS)
DESCRIPTION: Illustrates using `buf.copy()` to copy a specified range of bytes from one Buffer (`buf1`) into another Buffer (`buf2`) starting at a given target offset. It also notes the equivalent `TypedArray.prototype.set()` method.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_38

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

// Create two `Buffer` instances.
const buf1 = Buffer.allocUnsafe(26);
const buf2 = Buffer.allocUnsafe(26).fill('!');

for (let i = 0; i < 26; i++) {
  // 97 is the decimal ASCII value for 'a'.
  buf1[i] = i + 97;
}

// Copy `buf1` bytes 16 through 19 into `buf2` starting at byte 8 of `buf2`.
buf1.copy(buf2, 8, 16, 20);
// This is equivalent to:
// buf2.set(buf1.subarray(16, 20), 8);

console.log(buf2.toString('ascii', 0, 25));
// Prints: !!!!!!!!qrst!!!!!!!!!!!!!
```

----------------------------------------

TITLE: Defining napi_key_collection_mode Enum in Node-API C
DESCRIPTION: Defines the `napi_key_collection_mode` enum, which specifies whether property collection APIs should include properties from the object's prototype chain or be limited to the object's own properties only. Used as a filter parameter in property enumeration functions.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_65

LANGUAGE: C
CODE:
```
typedef enum {
  napi_key_include_prototypes,
  napi_key_own_only
} napi_key_collection_mode;
```

----------------------------------------

TITLE: Using describe/it Syntax for Test Suites in Node.js
DESCRIPTION: Demonstrates the `describe`/`it` syntax, providing an alternative way to structure tests into suites and individual test cases, similar to other testing frameworks like Mocha or Jest. Suites can be nested.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_7

LANGUAGE: js
CODE:
```
describe('A thing', () => {
  it('should work', () => {
    assert.strictEqual(1, 1);
  });

  it('should be ok', () => {
    assert.strictEqual(2, 2);
  });

  describe('a nested thing', () => {
    it('should work', () => {
      assert.strictEqual(3, 3);
    });
  });
});
```

----------------------------------------

TITLE: Using assert.doesNotReject with Promise.reject in MJS
DESCRIPTION: This example demonstrates `assert.doesNotReject` with a promise that rejects with a `TypeError`. The assertion specifies `TypeError` as the error type that should *not* be rejected. Since the rejection error (`TypeError`) *does* match the specified type, the assertion fails (rejects or throws).
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_26

LANGUAGE: mjs
CODE:
```
import assert from 'node:assert/strict';

assert.doesNotReject(Promise.reject(new TypeError('Wrong value')))
  .then(() => {
    // ...
  });
```

----------------------------------------

TITLE: Saving Uploaded Files to Disk with Busboy Node.js
DESCRIPTION: This example shows how to build an HTTP server using @fastify/busboy to handle file uploads by piping the incoming file streams directly to writable streams that save the files to a temporary directory on the server's file system. It uses Node.js built-in modules like `path`, `os`, and `fs`.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/node_modules/@fastify/busboy/README.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const http = require('node:http');
const path = require('node:path');
const os = require('node:os');
const fs = require('node:fs');

const Busboy = require('busboy');

http.createServer(function(req, res) {
  if (req.method === 'POST') {
    const busboy = new Busboy({ headers: req.headers });
    busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
      var saveTo = path.join(os.tmpdir(), path.basename(fieldname));
      file.pipe(fs.createWriteStream(saveTo));
    });
    busboy.on('finish', function() {
      res.writeHead(200, { 'Connection': 'close' });
      res.end("That's all folks!");
    });
    return req.pipe(busboy);
  }
  res.writeHead(404);
  res.end();
}).listen(8000, function() {
  console.log('Listening for requests');
});

```

----------------------------------------

TITLE: Handling Cluster Worker Online Event (Node.js)
DESCRIPTION: This snippet demonstrates how to attach a listener to the `cluster` module's 'online' event. This event is emitted by the primary process when a worker process successfully starts up and responds with an online message after being forked. The callback receives the `worker` object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cluster.md#_snippet_19

LANGUAGE: JavaScript
CODE:
```
cluster.on('online', (worker) => {
  console.log('Yay, the worker responded after it was forked');
});
```

----------------------------------------

TITLE: Handling Upgrade Event in Node.js HTTP Server and Client - MJS
DESCRIPTION: Provides a complete example using ES modules (`import`) of a Node.js HTTP server that listens for the 'upgrade' event (e.g., for WebSockets) and a corresponding client that initiates an upgrade request and listens for the client-side 'upgrade' event. The server demonstrates writing a 101 response and piping the socket for echo, while the client logs a success message and exits.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_10

LANGUAGE: javascript
CODE:
```
import http from 'node:http';
import process from 'node:process';

// Create an HTTP server
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('okay');
});
server.on('upgrade', (req, socket, head) => {
  socket.write('HTTP/1.1 101 Web Socket Protocol Handshake\r\n' +               'Upgrade: WebSocket\r\n' +               'Connection: Upgrade\r\n' +               '\r\n');

  socket.pipe(socket); // echo back
});

// Now that server is running
server.listen(1337, '127.0.0.1', () => {

  // make a request
  const options = {
    port: 1337,
    host: '127.0.0.1',
    headers: {
      'Connection': 'Upgrade',
      'Upgrade': 'websocket',
    },
  };

  const req = http.request(options);
  req.end();

  req.on('upgrade', (res, socket, upgradeHead) => {
    console.log('got upgraded!');
    socket.end();
    process.exit(0);
  });
});
```

----------------------------------------

TITLE: Copying File using fs.copyFileSync
DESCRIPTION: Demonstrates synchronously copying a file from a source path ('source.txt') to a destination path ('destination.txt'). The destination file will be created or overwritten by default.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_76

LANGUAGE: javascript
CODE:
```
import { copyFileSync, constants } from 'node:fs';

// destination.txt will be created or overwritten by default.
copyFileSync('source.txt', 'destination.txt');
console.log('source.txt was copied to destination.txt');

// By using COPYFILE_EXCL, the operation will fail if destination.txt exists.
copyFileSync('source.txt', 'destination.txt', constants.COPYFILE_EXCL);
```

----------------------------------------

TITLE: Aborting a Child Process with AbortSignal - child_process.execFile - Node.js
DESCRIPTION: This snippet illustrates how to abort a child process execution using the `AbortSignal` option provided to `execFile`. An `AbortController` and its signal are created, the signal is passed to the options, and calling `controller.abort()` terminates the process, resulting in an `AbortError` being passed to the callback.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_9

LANGUAGE: js
CODE:
```
const { execFile } = require('node:child_process');
const controller = new AbortController();
const { signal } = controller;
const child = execFile('node', ['--version'], { signal }, (error) => {
  console.error(error); // an AbortError
});
controller.abort();
```

----------------------------------------

TITLE: Executing - npm start Command - Bash
DESCRIPTION: Provides a practical example of running the `npm start` command and shows the typical output displayed by npm. This output includes npm indicating that it is running the 'start' script and the output from the script itself.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-start.md#_snippet_2

LANGUAGE: bash
CODE:
```
npm start

> npm@x.x.x start
> node foo.js

(foo.js output would be here)
```

----------------------------------------

TITLE: Defining Commands with Action Handler, Executable, or AddCommand - Commander.js - JavaScript
DESCRIPTION: Shows how to define commands using an inline action handler, by referencing a standalone executable (via description), and by adding a pre-configured command object using .addCommand(). Illustrates argument syntax within the command string.
SOURCE: https://github.com/asana/node/blob/main/test/fixtures/postject-copy/node_modules/commander/Readme.md#_snippet_26

LANGUAGE: javascript
CODE:
```
// Command implemented using action handler (description is supplied separately to `.command`)
// Returns new command for configuring.
program
  .command('clone <source> [destination]')
  .description('clone a repository into a newly created directory')
  .action((source, destination) => {
    console.log('clone command called');
  });

// Command implemented using stand-alone executable file, indicated by adding description as second parameter to `.command`.
// Returns `this` for adding more commands.
program
  .command('start <service>', 'start named service')
  .command('stop [service]', 'stop named service, or all if no name supplied');

// Command prepared separately.
// Returns `this` for adding more commands.
program
  .addCommand(build.makeBuildCommand());
```

----------------------------------------

TITLE: Using Compiled Node.js Addon in JavaScript
DESCRIPTION: This JavaScript snippet shows how to load and use a function exported by a compiled Node.js native addon. It uses `require` to load the `addon.node` file from the build directory and then calls the `hello()` method exposed by the loaded addon object. It demonstrates the typical way to interact with native functionality from JavaScript.
SOURCE: https://github.com/asana/node/blob/main/doc/api/addons.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
// hello.js
const addon = require('./build/Release/addon');

console.log(addon.hello());
// Prints: 'world'
```

----------------------------------------

TITLE: Handling Worker Exit and Restarting in Node.js Cluster (JS)
DESCRIPTION: Illustrates handling the `'exit'` event in the Node.js `cluster` primary to manage worker process termination. This event provides the worker object, exit code, and signal, allowing the primary to log the reason for death and automatically fork a new worker to maintain the desired cluster size. This requires the `node:cluster` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cluster.md#_snippet_16

LANGUAGE: JavaScript
CODE:
```
cluster.on('exit', (worker, code, signal) => {
  console.log('worker %d died (%s). restarting...',
              worker.process.pid, signal || code);
  cluster.fork();
});
```

----------------------------------------

TITLE: Importing Stream Consumers (ES Modules)
DESCRIPTION: Demonstrates importing various stream consumer functions (arrayBuffer, blob, buffer, json, text) from the 'node:stream/consumers' module using standard ES module import syntax. These functions provide convenient ways to consume stream data.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_16

LANGUAGE: javascript
CODE:
```
import {
  arrayBuffer,
  blob,
  buffer,
  json,
  text,
} from 'node:stream/consumers';
```

----------------------------------------

TITLE: Reading File Lines with 'line' Event in Node.js
DESCRIPTION: This example shows how to read a file line by line using the `readline` module's `'line'` event. It creates a read stream from a file and listens for the 'line' event, which is emitted for each line read. It uses `crlfDelay` for line ending handling and requires the `node:fs` and `node:readline` modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/readline.md#_snippet_31

LANGUAGE: JavaScript
CODE:
```
const fs = require('node:fs');
const readline = require('node:readline');

const rl = readline.createInterface({
  input: fs.createReadStream('sample.txt'),
  crlfDelay: Infinity,
});

rl.on('line', (line) => {
  console.log(`Line from file: ${line}`);
});
```

----------------------------------------

TITLE: Installing Acorn via npm (Shell)
DESCRIPTION: This command installs the Acorn JavaScript parser library from the npm registry. It's the standard and easiest way to add Acorn as a project dependency.
SOURCE: https://github.com/asana/node/blob/main/deps/acorn/acorn/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
npm install acorn
```

----------------------------------------

TITLE: Requiring a Workspace Package in Node.js
DESCRIPTION: Provides two JavaScript snippets: one defining a simple module in `./packages/a/index.js` and another (`./lib/index.js`) demonstrating how to `require()` the workspace by its declared package name (`a`). This highlights how workspaces enable easy consumption of local packages using standard Node.js `require` after npm symlinks them into the root node_modules.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/workspaces.md#_snippet_3

LANGUAGE: javascript
CODE:
```
// ./packages/a/index.js
module.exports = 'a'
```

LANGUAGE: javascript
CODE:
```
// ./lib/index.js
const moduleA = require('a')
console.log(moduleA) // -> a
```

----------------------------------------

TITLE: Using AbortSignal with child_process.fork in Node.js
DESCRIPTION: This example demonstrates how to use an `AbortSignal` with `child_process.fork` to terminate the child process. The parent process forks the current script, passes 'child' as an argument, and immediately aborts the signal, triggering an 'error' event on the child process handle with an `AbortError`. The child process includes a timeout that is interrupted by the abort signal.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
if (process.argv[2] === 'child') {
  setTimeout(() => {
    console.log(`Hello from ${process.argv[2]}!`);
  }, 1_000);
} else {
  const { fork } = require('node:child_process');
  const controller = new AbortController();
  const { signal } = controller;
  const child = fork(__filename, ['child'], { signal });
  child.on('error', (err) => {
    // This will be called with err being an AbortError if the controller aborts
  });
  controller.abort(); // Stops the child process
}
```

----------------------------------------

TITLE: Compressing File with Gzip Streams
DESCRIPTION: Demonstrates how to compress a file using the zlib.createGzip() Transform stream and the node:stream.pipeline() function. It pipes a readable stream from an input file through the gzip stream into a writable stream for an output file.
SOURCE: https://github.com/asana/node/blob/main/doc/api/zlib.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createGzip } = require('node:zlib');
const { pipeline } = require('node:stream');
const {
  createReadStream,
  createWriteStream,
} = require('node:fs');

const gzip = createGzip();
const source = createReadStream('input.txt');
const destination = createWriteStream('input.txt.gz');

pipeline(source, gzip, destination, (err) => {
  if (err) {
    console.error('An error occurred:', err);
    process.exitCode = 1;
  }
});
```

LANGUAGE: javascript
CODE:
```
const { createGzip } = require('node:zlib');
const { pipeline } = require('node:stream');
const {
  createReadStream,
  createWriteStream,
} = require('node:fs');
const { promisify } = require('node:util');
const pipe = promisify(pipeline);

async function do_gzip(input, output) {
  const gzip = createGzip();
  const source = createReadStream(input);
  const destination = createWriteStream(output);
  await pipe(source, gzip, destination);
}

do_gzip('input.txt', 'input.txt.gz')
  .catch((err) => {
    console.error('An error occurred:', err);
    process.exitCode = 1;
  });
```

----------------------------------------

TITLE: Signing and Verifying Data (Streams, mjs)
DESCRIPTION: Demonstrates using the Node.js `crypto` module's `Sign` and `Verify` classes as streams to generate and verify signatures. It generates an EC key pair, pipes data to the `Sign` instance, generates a hex-encoded signature, pipes the same data to the `Verify` instance, and verifies the signature using the public key. Requires `node:crypto` and uses ES Module syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_19

LANGUAGE: javascript
CODE:
```
const {
  generateKeyPairSync,
  createSign,
  createVerify,
} = await import('node:crypto');

const { privateKey, publicKey } = generateKeyPairSync('ec', {
  namedCurve: 'sect239k1',
});

const sign = createSign('SHA256');
sign.write('some data to sign');
sign.end();
const signature = sign.sign(privateKey, 'hex');

const verify = createVerify('SHA256');
verify.write('some data to sign');
verify.end();
console.log(verify.verify(publicKey, signature, 'hex'));
// Prints: true
```

----------------------------------------

TITLE: Creating Buffers from TypedArray and ArrayBuffer (Copy vs Share) - JavaScript
DESCRIPTION: Compares creating a `Buffer` directly from a `TypedArray` (which copies data) versus creating one from the `TypedArray`'s underlying `.buffer` (which shares memory). The example shows that modifying the original `Uint16Array` only affects the Buffer created using the shared ArrayBuffer (`buf2`), while the Buffer created by copying (`buf1`) remains unchanged.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_8

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

const arr = new Uint16Array(2);

arr[0] = 5000;
arr[1] = 4000;

// Copies the contents of `arr`.
const buf1 = Buffer.from(arr);

// Shares memory with `arr`.
const buf2 = Buffer.from(arr.buffer);

console.log(buf1);
// Prints: <Buffer 88 a0>
console.log(buf2);
// Prints: <Buffer 88 13 a0 0f>

arr[1] = 6000;

console.log(buf1);
// Prints: <Buffer 88 a0>
console.log(buf2);
// Prints: <Buffer 88 13 70 17>
```

LANGUAGE: javascript
CODE:
```
const { Buffer } = require('node:buffer');

const arr = new Uint16Array(2);

arr[0] = 5000;
arr[1] = 4000;

// Copies the contents of `arr`.
const buf1 = Buffer.from(arr);

// Shares memory with `arr`.
const buf2 = Buffer.from(arr.buffer);

console.log(buf1);
// Prints: <Buffer 88 a0>
console.log(buf2);
// Prints: <Buffer 88 13 a0 0f>

arr[1] = 6000;

console.log(buf1);
// Prints: <Buffer 88 a0>
console.log(buf2);
// Prints: <Buffer 88 13 70 17>
```

----------------------------------------

TITLE: Triggering ReferenceError by Accessing Undefined Variable in Node.js JavaScript
DESCRIPTION: Shows the simplest way to cause a `ReferenceError` in JavaScript by attempting to access a variable (`doesNotExist`) that has not been declared or defined in the current scope. Illustrates that Node.js/V8 throws this error when such an access occurs.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_11

LANGUAGE: javascript
CODE:
```
doesNotExist;
```

----------------------------------------

TITLE: Excluding Null and Undefined with NonNullable - TypeScript
DESCRIPTION: Illustrates the `NonNullable<T>` utility type by applying it to a class property derived from a union type including `null`. Shows how assigning `null` directly to the `NonNullable` property results in a TypeScript error, demonstrating its role in type safety when `strictNullChecks` is enabled.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/type-fest/readme.md#_snippet_9

LANGUAGE: typescript
CODE:
```
type PortNumber = string | number | null;

/** Part of a class definition that is used to build a server */
class ServerBuilder {
		portNumber!: NonNullable<PortNumber>;

		port(this: ServerBuilder, port: PortNumber): ServerBuilder {
			if (port == null) {
					this.portNumber = 8000;
			} else {
					this.portNumber = port;
			}

			return this;
		}
}

const serverBuilder = new ServerBuilder();

serverBuilder
		.port('8000')   // portNumber = '8000'
		.port(null)     // portNumber =  8000
		.port(3000);    // portNumber =  3000

// TypeScript error
serverBuilder.portNumber = null;
```

----------------------------------------

TITLE: Checking File Access Permissions with Node.js fs/promises JavaScript
DESCRIPTION: Shows how to use `fsPromises.access` with `constants.R_OK | constants.W_OK` to check read and write permissions for the file '/etc/passwd'. Uses a `try...catch` block to handle success or failure and log the result. Note that checking access *before* other operations like open/write can lead to race conditions; direct operation and error handling is often preferred. Requires the `node:fs/promises` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_18

LANGUAGE: javascript
CODE:
```
import { access, constants } from 'node:fs/promises';

try {
  await access('/etc/passwd', constants.R_OK | constants.W_OK);
  console.log('can access');
} catch {
  console.error('cannot access');
}
```

----------------------------------------

TITLE: Importing Assert Strict Subpath (CommonJS)
DESCRIPTION: This snippet shows an alternative way to import the `node:assert` module in strict assertion mode using the dedicated 'node:assert/strict' subpath with CommonJS syntax. This is another standard way to access the strict mode functions.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_3

LANGUAGE: javascript
CODE:
```
const assert = require('node:assert/strict');
```

----------------------------------------

TITLE: Running npm Script Across All Configured Workspaces
DESCRIPTION: Explains how to use the `--workspaces` (plural) flag to execute a given script (e.g., `test`) in every single workspace defined in the root package.json's `workspaces` property. This is useful for running common tasks like testing or building across an entire monorepo structure with a single command.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/workspaces.md#_snippet_10

LANGUAGE: bash
CODE:
```
npm run test --workspaces
```

----------------------------------------

TITLE: Consuming Stream to ArrayBuffer (ES Modules)
DESCRIPTION: Shows how to use the 'arrayBuffer' consumer function to read the entire contents of a Node.js Readable stream or ReadableStream into an ArrayBuffer using ES Modules. It requires importing 'arrayBuffer', 'Readable', and 'TextEncoder', and uses 'await'.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_18

LANGUAGE: javascript
CODE:
```
import { arrayBuffer } from 'node:stream/consumers';
import { Readable } from 'node:stream';
import { TextEncoder } from 'node:util';

const encoder = new TextEncoder();
const dataArray = encoder.encode('hello world from consumers!');

const readable = Readable.from(dataArray);
const data = await arrayBuffer(readable);
console.log(`from readable: ${data.byteLength}`);
// Prints: from readable: 76
```

----------------------------------------

TITLE: Chaining Customization Hooks (ESM)
DESCRIPTION: Illustrates calling `module.register` multiple times within an ES Module entry point to register several sets of customization hooks. The hooks will be applied in the order they are registered, forming a processing chain.
SOURCE: https://github.com/asana/node/blob/main/doc/api/module.md#_snippet_9

LANGUAGE: javascript
CODE:
```
// entrypoint.mjs
import { register } from 'node:module';

register('./first.mjs', import.meta.url);
register('./second.mjs', import.meta.url);
await import('./my-app.mjs');
```

----------------------------------------

TITLE: Exporting a Single Class from Node.js CommonJS Module
DESCRIPTION: This code defines a CommonJS module ('square.js') that exports a single value (a `Square` class) by assigning it directly to `module.exports`. It includes a comment noting that assigning to `exports` alone would not work for this pattern.
SOURCE: https://github.com/asana/node/blob/main/doc/api/modules.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
// Assigning to exports will not modify module, must use module.exports
module.exports = class Square {
  constructor(width) {
    this.width = width;
  }

  area() {
    return this.width ** 2;
  }
};
```

----------------------------------------

TITLE: Basic HTTP Test Example - Node.js - JavaScript
DESCRIPTION: This is a complete example of a basic Node.js test using the 'http' module. It demonstrates setting up an HTTP server and client, sending a request with UTF-8 headers, asserting the response status code, and properly closing the server. It utilizes 'common.mustCall' to ensure callbacks are executed and 'assert' for validation.
SOURCE: https://github.com/asana/node/blob/main/doc/contributing/writing-tests.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
'use strict';
const common = require('../common');
const fixtures = require('../common/fixtures');

// This test ensures that the http-parser can handle UTF-8 characters
// in the http header.

const assert = require('node:assert');
const http = require('node:http');

const server = http.createServer(common.mustCall((req, res) => {
  res.end('ok');
}));
server.listen(0, () => {
  http.get({
    port: server.address().port,
    headers: { 'Test': 'Düsseldorf' },
  }, common.mustCall((res) => {
    assert.strictEqual(res.statusCode, 200);
    server.close();
  }));
});
// ...
```

----------------------------------------

TITLE: Deleting File Synchronously with fs Module [MJS]
DESCRIPTION: Demonstrates synchronously deleting a file using the `unlinkSync` function from `node:fs` with ES module syntax, blocking the event loop until completion and using `try...catch` for error handling.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_8

LANGUAGE: javascript
CODE:
```
import { unlinkSync } from 'node:fs';

try {
  unlinkSync('/tmp/hello');
  console.log('successfully deleted /tmp/hello');
} catch (err) {
  // handle the error
}
```

----------------------------------------

TITLE: Creating TCP Client Connection in Node.js
DESCRIPTION: Shows how to create a TCP client socket connection to a specific port using `net.createConnection`. It includes listeners for the `'connect'`, `'data'`, and `'end'` events to handle connection establishment, data reception, and connection termination. Requires the 'node:net' module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/net.md#_snippet_9

LANGUAGE: javascript
CODE:
```
const net = require('node:net');
const client = net.createConnection({ port: 8124 }, () => {
  // 'connect' listener.
  console.log('connected to server!');
  client.write('world!\r\n');
});
client.on('data', (data) => {
  console.log(data.toString());
  client.end();
});
client.on('end', () => {
  console.log('disconnected from server');
});
```

----------------------------------------

TITLE: Configuring npm Scripts with External Files (JSON)
DESCRIPTION: This JSON snippet defines npm lifecycle scripts within a `package.json` file. It shows how to execute external script files (`scripts/install.js`, `scripts/uninstall.js`) for the `install`, `postinstall`, and `uninstall` phases, illustrating the use of local executables for package lifecycle management. The scripts run by `sh` and their exit code determines success or failure.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/scripts.md#_snippet_1

LANGUAGE: json
CODE:
```
{
  "scripts" : {
    "install" : "scripts/install.js",
    "postinstall" : "scripts/install.js",
    "uninstall" : "scripts/uninstall.js"
  }
}
```

----------------------------------------

TITLE: Linking Local NPM Package for Development (Bash)
DESCRIPTION: Shows how to create a symbolic link from the global npm folder to the current package directory using `npm link`. This allows other projects on the same machine to depend on the local development version of the package without publishing or reinstalling.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/developers.md#_snippet_2

LANGUAGE: bash
CODE:
```
npm link
```

----------------------------------------

TITLE: Calling 'on' Listeners Multiple Times (ESM)
DESCRIPTION: Shows that listeners registered with `eventEmitter.on()` are called every time the corresponding event is emitted, demonstrating persistent subscription.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_8

LANGUAGE: javascript
CODE:
```
import { EventEmitter } from 'node:events';
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
let m = 0;
myEmitter.on('event', () => {
  console.log(++m);
});
myEmitter.emit('event');
// Prints: 1
myEmitter.emit('event');
// Prints: 2
```

----------------------------------------

TITLE: Running npm doctor command with options - Bash
DESCRIPTION: Provides the basic command structure for running the `npm doctor` command in a bash shell. The optional arguments like `ping`, `registry`, `versions`, `environment`, `permissions`, and `cache` allow users to limit the specific health checks performed. Running the command without any arguments executes all available checks to diagnose potential issues with the npm installation.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-doctor.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm doctor [ping] [registry] [versions] [environment] [permissions] [cache]
```

----------------------------------------

TITLE: Compose & Nest Styles - Chalk - JavaScript
DESCRIPTION: This comprehensive snippet illustrates several key features: combining styled and normal strings, chaining multiple styles (e.g., blue.bgRed.bold), passing multiple arguments to apply the same style, and nesting different styles within one another for complex formatting.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/chalk/readme.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
const chalk = require('chalk');
const log = console.log;

// Combine styled and normal strings
log(chalk.blue('Hello') + ' World' + chalk.red('!'));

// Compose multiple styles using the chainable API
log(chalk.blue.bgRed.bold('Hello world!'));

// Pass in multiple arguments
log(chalk.blue('Hello', 'World!', 'Foo', 'bar', 'biz', 'baz'));

// Nest styles
log(chalk.red('Hello', chalk.underline.bgBlue('world') + '!'));

// Nest styles of the same type even (color, underline, background)
log(chalk.green(
	'I am a green line ' +
	chalk.blue.underline.bold('with a blue substring') +
	' that becomes green again!'
));

// ES2015 template literal
log(`
CPU: ${chalk.red('90%')}
RAM: ${chalk.green('40%')}
DISK: ${chalk.yellow('70%')}
`);

// ES2015 tagged template literal
log(chalk`
CPU: {red ${cpu.totalPercent}%}
RAM: {green ${ram.used / ram.total * 100}%}
DISK: {rgb(255,131,0) ${disk.used / disk.total * 100}%}
`);
```

----------------------------------------

TITLE: Initializing Node.js Socket and Setting Encoding (JavaScript)
DESCRIPTION: This snippet demonstrates how to create a new instance of a Node.js `net.Socket`. It requires the `node:net` module to access the `Socket` class. The example then calls `setEncoding('utf8')` on the socket instance to specify the character encoding for received data, which is relevant in the context of the `ERR_STREAM_WRAP` error.
SOURCE: https://github.com/asana/node/blob/main/doc/api/errors.md#_snippet_15

LANGUAGE: javascript
CODE:
```
const Socket = require('node:net').Socket;
const instance = new Socket();

instance.setEncoding('utf8');
```

----------------------------------------

TITLE: Calculating Relative Path on POSIX with Node.js path.relative
DESCRIPTION: This snippet shows how to use `path.relative()` to find the relative path from one directory to another on a POSIX-like system. It calculates the necessary navigation steps (..) to get from the 'test/aaa' directory to the 'impl/bbb' directory.
SOURCE: https://github.com/asana/node/blob/main/doc/api/path.md#_snippet_2

LANGUAGE: javascript
CODE:
```
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
```

----------------------------------------

TITLE: Opening Standard Node-API Handle Scope in C
DESCRIPTION: Opens a new handle scope, changing the scope to which newly created N-API handles are associated. Handles created within this scope will be invalidated when the scope is closed. The function takes the Node-API environment and a pointer to a napi_handle_scope where the new scope handle will be stored. It returns napi_ok.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_45

LANGUAGE: C
CODE:
```
NAPI_EXTERN napi_status napi_open_handle_scope(napi_env env,
                                               napi_handle_scope* result);
```

----------------------------------------

TITLE: Using Pick<T, K> Utility Type in TypeScript
DESCRIPTION: This snippet showcases the `Pick<T, K>` utility type, which creates a new type by selecting a subset of properties from an existing type `T` using keys specified in `K`. It defines an `Article` interface and then creates `ArticlePreview` using `Pick`, including only the `title` and `thumbnail` properties. The `renderArticlePreviews` function demonstrates using this new, narrower type.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/type-fest/readme.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
interface Article {
			title: string;
			thumbnail: string;
			content: string;
	}

	// Creates new type out of the `Article` interface composed
	// from the Articles' two properties: `title` and `thumbnail`.
	// `ArticlePreview = {title: string; thumbnail: string}`
	type ArticlePreview = Pick<Article, 'title' | 'thumbnail'>;

	// Render a list of articles using only title and description.
	function renderArticlePreviews(previews: ArticlePreview[]): HTMLElement {
			const articles = document.createElement('div');

			for (const preview of previews) {
					// Append preview to the articles.
			}

			return articles;
	}

	const articles = renderArticlePreviews([
			{
				title: 'TypeScript tutorial!',
				thumbnail: '/assets/ts.jpg'
			}
	]);
```

----------------------------------------

TITLE: Handling clientError Event in Node.js HTTP Server (Basic)
DESCRIPTION: Demonstrates attaching a listener to the `clientError` event. When a client connection error occurs, it sends a '400 Bad Request' HTTP response directly to the socket before closing it, overriding the default server behavior.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_21

LANGUAGE: mjs
CODE:
```
import http from 'node:http';

const server = http.createServer((req, res) => {
  res.end();
});
server.on('clientError', (err, socket) => {
  socket.end('HTTP/1.1 400 Bad Request\r\n\r\n');
});
server.listen(8000);
```

LANGUAGE: cjs
CODE:
```
const http = require('node:http');

const server = http.createServer((req, res) => {
  res.end();
});
server.on('clientError', (err, socket) => {
  socket.end('HTTP/1.1 400 Bad Request\r\n\r\n');
});
server.listen(8000);
```

----------------------------------------

TITLE: Comparing Number and String with Strict Equality (===) in JavaScript
DESCRIPTION: Demonstrates that strict equality (`===`) between a primitive number (`0`) and a string containing the numeric value (`"0"`) results in `false`. This is because `===` requires both the value *and* the type to be the same, and number is not the same type as string. Requires a standard JavaScript environment for execution.
SOURCE: https://github.com/asana/node/blob/main/deps/v8/test/webkit/equality-expected.txt#_snippet_3

LANGUAGE: javascript
CODE:
```
0 === "0"
```

----------------------------------------

TITLE: Performing Strict Equality Checks in Node.js
DESCRIPTION: Provides various examples demonstrating the behavior of `assert.strictEqual`, which tests strict equality using `Object.is()`. It shows examples of successful assertions, failures with numbers and strings, using a message template, and throwing a custom error.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_51

LANGUAGE: mjs
CODE:
```
import assert from 'node:assert/strict';

assert.strictEqual(1, 2);
// AssertionError [ERR_ASSERTION]: Expected inputs to be strictly equal:
//
// 1 !== 2

assert.strictEqual(1, 1);
// OK

assert.strictEqual('Hello foobar', 'Hello World!');
// AssertionError [ERR_ASSERTION]: Expected inputs to be strictly equal:
// + actual - expected
//
// + 'Hello foobar'
// - 'Hello World!'
//          ^

const apples = 1;
const oranges = 2;
assert.strictEqual(apples, oranges, `apples ${apples} !== oranges ${oranges}`);
// AssertionError [ERR_ASSERTION]: apples 1 !== oranges 2

assert.strictEqual(1, '1', new TypeError('Inputs are not identical'));
// TypeError: Inputs are not identical
```

LANGUAGE: cjs
CODE:
```
const assert = require('node:assert/strict');

assert.strictEqual(1, 2);
// AssertionError [ERR_ASSERTION]: Expected inputs to be strictly equal:
//
// 1 !== 2

assert.strictEqual(1, 1);
// OK

assert.strictEqual('Hello foobar', 'Hello World!');
// AssertionError [ERR_ASSERTION]: Expected inputs to be strictly equal:
// + actual - expected
//
// + 'Hello foobar'
// - 'Hello World!'
//          ^

const apples = 1;
const oranges = 2;
assert.strictEqual(apples, oranges, `apples ${apples} !== oranges ${oranges}`);
// AssertionError [ERR_ASSERTION]: apples 1 !== oranges 2

assert.strictEqual(1, '1', new TypeError('Inputs are not identical'));
// TypeError: Inputs are not identical
```

----------------------------------------

TITLE: Using pipeline with a Transform Stream (JavaScript)
DESCRIPTION: Demonstrates using Node.js `pipeline` to connect a readable file stream, a custom transform stream, and a writable file stream. The `Transform` stream buffers all input (`transform`), validates it as JSON (`flush`), and pushes the validated data, handling potential parsing errors. The `pipeline` utility ensures proper error handling and stream cleanup.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_92

LANGUAGE: JavaScript
CODE:
```
const { Transform, pipeline } = require('node:stream');
const fs = require('node:fs');

pipeline(
  fs.createReadStream('object.json')
    .setEncoding('utf8'),
  new Transform({
    decodeStrings: false, // Accept string input rather than Buffers
    construct(callback) {
      this.data = '';
      callback();
    },
    transform(chunk, encoding, callback) {
      this.data += chunk;
      callback();
    },
    flush(callback) {
      try {
        // Make sure is valid json.
        JSON.parse(this.data);
        this.push(this.data);
        callback();
      } catch (err) {
        callback(err);
      }
    },
  }),
  fs.createWriteStream('valid-object.json'),
  (err) => {
    if (err) {
      console.error('failed', err);
    } else {
      console.log('completed');
    }
  },
);
```

----------------------------------------

TITLE: Using npm install-ci-test Command Bash
DESCRIPTION: This snippet shows the basic syntax for running the npm install-ci-test command, which executes `npm ci` followed by `npm test` for the current project. It provides the command's primary usage and its common aliases.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-install-ci-test.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm install-ci-test

alias: cit, clean-install-test, sit
```

----------------------------------------

TITLE: Cancelling setTimeout Promise with AbortController (Node.js)
DESCRIPTION: Demonstrates how to cancel a promise-based `setTimeout` timer using an `AbortController`. It shows creating a controller and signal, passing the signal to `setTimeoutPromise`, and then calling `abort()` on the controller to cancel the timer, resulting in the promise being rejected with an `AbortError`. This requires the `node:timers/promises` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/timers.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const { setTimeout: setTimeoutPromise } = require('node:timers/promises');

const ac = new AbortController();
const signal = ac.signal;

setTimeoutPromise(1000, 'foobar', { signal })
  .then(console.log)
  .catch((err) => {
    if (err.name === 'AbortError')
      console.error('The timeout was aborted');
  });

ac.abort();
```

----------------------------------------

TITLE: Logging into scoped npm registry (Bash)
DESCRIPTION: Logs into a specific custom npm registry and associates a package scope with that registry. This configuration ensures that packages under the specified scope (@mycorp) are installed from the custom registry for future operations.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-login.md#_snippet_1

LANGUAGE: bash
CODE:
```
npm login --scope=@mycorp --registry=https://registry.mycorp.com
```

----------------------------------------

TITLE: Encrypting File using Cipher Stream Pipeline (MJS)
DESCRIPTION: Demonstrates encrypting the contents of a file and writing the output to another file using Node.js streams and the `crypto.Cipher` class in an ES Modules pipeline. It uses `fs.createReadStream`, `fs.createWriteStream`, and `stream.pipeline` to connect the input, cipher, and output streams.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import {
  createReadStream,
  createWriteStream,
} from 'node:fs';

import {
  pipeline,
} from 'node:stream';

const {
  scrypt,
  randomFill,
  createCipheriv,
} = await import('node:crypto');

const algorithm = 'aes-192-cbc';
const password = 'Password used to generate key';

// First, we'll generate the key. The key length is dependent on the algorithm.
// In this case for aes192, it is 24 bytes (192 bits).
scrypt(password, 'salt', 24, (err, key) => {
  if (err) throw err;
  // Then, we'll generate a random initialization vector
  randomFill(new Uint8Array(16), (err, iv) => {
    if (err) throw err;

    const cipher = createCipheriv(algorithm, key, iv);

    const input = createReadStream('test.js');
    const output = createWriteStream('test.enc');

    pipeline(input, cipher, output, (err) => {
      if (err) throw err;
    });
  });
});
```

----------------------------------------

TITLE: Creating ReadableStream from Async Iterable (Node.js MJS)
DESCRIPTION: Shows how to use the static `ReadableStream.from()` method to create a new ReadableStream instance from an async generator function in an ES module context. The resulting stream is then consumed using async iteration.
SOURCE: https://github.com/asana/node/blob/main/doc/api/webstreams.md#_snippet_9

LANGUAGE: mjs
CODE:
```
import { ReadableStream } from 'node:stream/web';

async function* asyncIterableGenerator() {
  yield 'a';
  yield 'b';
  yield 'c';
}

const stream = ReadableStream.from(asyncIterableGenerator());

for await (const chunk of stream)
  console.log(chunk); // Prints: 'a', 'b', 'c'
```

----------------------------------------

TITLE: Truncating File using fs.truncate Node.js (MJS)
DESCRIPTION: This snippet demonstrates how to asynchronously truncate a file to a specified length (defaulting to 0 if omitted) using `fs.truncate` in Node.js (MJS syntax). It calls `truncate` on 'path/file.txt', providing a callback that checks for errors and logs a success message upon completion. It requires the `truncate` function imported from the `node:fs` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_63

LANGUAGE: javascript
CODE:
```
import { truncate } from 'node:fs';
// Assuming that 'path/file.txt' is a regular file.
truncate('path/file.txt', (err) => {
  if (err) throw err;
  console.log('path/file.txt was truncated');
});
```

----------------------------------------

TITLE: Demonstrating Header Precedence (setHeaders vs writeHead) in Node.js
DESCRIPTION: Shows how headers set using `res.writeHead()` take precedence over headers previously set using `res.setHeaders()`. It creates a simple HTTP server where a `Content-Type` header is set with `setHeaders` (text/html) and then overwritten by `writeHead` (text/plain).
SOURCE: https://github.com/asana/node/blob/main/doc/api/http.md#_snippet_48

LANGUAGE: javascript
CODE:
```
// Returns content-type = text/plain
const server = http.createServer((req, res) => {
  const headers = new Headers({ 'Content-Type': 'text/html' });
  res.setHeaders(headers);
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('ok');
});
```

----------------------------------------

TITLE: Setting Parameters with set() (Node.js)
DESCRIPTION: Demonstrates the `set` method, which adds a new name-value pair or updates an existing one. If a pair with the same name already exists, it updates the value of the first such pair and removes all subsequent pairs with that same name.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_36

LANGUAGE: js
CODE:
```
const params = new URLSearchParams();
params.append('foo', 'bar');
params.append('foo', 'baz');
params.append('abc', 'def');
console.log(params.toString());
// Prints foo=bar&foo=baz&abc=def

params.set('foo', 'def');
params.set('xyz', 'opq');
console.log(params.toString());
// Prints foo=def&abc=def&xyz=opq
```

----------------------------------------

TITLE: Parsing URL String (WHATWG API)
DESCRIPTION: Shows how to parse a URL string into a WHATWG `URL` object using the `new URL()` constructor.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
const myURL =\n  new URL('https://user:pass@sub.example.com:8080/p/a/t/h?query=string#hash');
```

----------------------------------------

TITLE: Creating JavaScript Function Node-API C
DESCRIPTION: Creates a JavaScript function object that, when invoked from JavaScript, will execute the provided native C callback function (`napi_callback`). The function can be given an optional UTF8 name visible in JavaScript. The created function is not automatically visible and must be explicitly set as a property on a reachable JavaScript object (e.g., the module exports).
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_163

LANGUAGE: C
CODE:
```
napi_status napi_create_function(napi_env env,
                                 const char* utf8name,
                                 size_t length,
                                 napi_callback cb,
                                 void* data,
                                 napi_value* result);
```

LANGUAGE: C
CODE:
```
napi_value SayHello(napi_env env, napi_callback_info info) {
  printf("Hello\n");
  return NULL;
}

napi_value Init(napi_env env, napi_value exports) {
  napi_status status;

  napi_value fn;
  status = napi_create_function(env, NULL, 0, SayHello, NULL, &fn);
  if (status != napi_ok) return NULL;

  status = napi_set_named_property(env, exports, "sayHello", fn);
  if (status != napi_ok) return NULL;

  return exports;
}

NAPI_MODULE(NODE_GYP_MODULE_NAME, Init)
```

----------------------------------------

TITLE: Observing Performance Marks using Node.js PerformanceObserver (type)
DESCRIPTION: This snippet illustrates subscribing a `PerformanceObserver` to listen for performance entries of a single specified type using the `type` option in the `observe()` method. It shows marking multiple entries within a loop and notes that the observer callback is invoked asynchronously with a list containing the observed entries.
SOURCE: https://github.com/asana/node/blob/main/doc/api/perf_hooks.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
const {
  performance,
  PerformanceObserver,
} = require('node:perf_hooks');

const obs = new PerformanceObserver((list, observer) => {
  // Called once asynchronously. `list` contains three items.
});
obs.observe({ type: 'mark' });

for (let n = 0; n < 3; n++)
  performance.mark(`test${n}`);
```

----------------------------------------

TITLE: Reacting to Writable Stream Finish Event: Javascript
DESCRIPTION: Shows how to attach a listener to the `'finish'` event on a Writable stream. This event is emitted after the `end()` method has been called and all data currently buffered has been successfully flushed to the underlying system.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_14

LANGUAGE: javascript
CODE:
```
const writer = getWritableStreamSomehow();
for (let i = 0; i < 100; i++) {
  writer.write(`hello, #${i}!\n`);
}
writer.on('finish', () => {
  console.log('All writes are now complete.');
});
writer.end('This is the end\n');
```

----------------------------------------

TITLE: Sending Buffer UDP Datagram - Node.js (Connected Socket)
DESCRIPTION: Illustrates sending a Buffer message using a UDP socket that has been previously connected using `client.connect()`. For connected sockets, the destination port and address should not be provided in the `client.send` call.
SOURCE: https://github.com/asana/node/blob/main/doc/api/dgram.md#_snippet_5

LANGUAGE: mjs
CODE:
```
import dgram from 'node:dgram';
import { Buffer } from 'node:buffer';

const message = Buffer.from('Some bytes');
const client = dgram.createSocket('udp4');
client.connect(41234, 'localhost', (err) => {
  client.send(message, (err) => {
    client.close();
  });
});
```

LANGUAGE: cjs
CODE:
```
const dgram = require('node:dgram');
const { Buffer } = require('node:buffer');

const message = Buffer.from('Some bytes');
const client = dgram.createSocket('udp4');
client.connect(41234, 'localhost', (err) => {
  client.send(message, (err) => {
    client.close();
  });
});
```

----------------------------------------

TITLE: Getting Object Property Names Node-API (C)
DESCRIPTION: This C Node-API function retrieves the names of the enumerable properties of a given JavaScript object. It returns a `napi_value` representing a JavaScript array containing the property names as strings. Symbol keys are excluded.
SOURCE: https://github.com/asana/node/blob/main/doc/api/n-api.md#_snippet_145

LANGUAGE: c
CODE:
```
napi_status napi_get_property_names(napi_env env,
                                    napi_value object,
                                    napi_value* result);
```

----------------------------------------

TITLE: Closing net.Server using AbortSignal
DESCRIPTION: This snippet demonstrates how to use an `AbortSignal` to control the lifecycle of a `net.Server`. It creates an `AbortController`, passes its signal to the `listen` options, and later calls `controller.abort()` to close the listening server asynchronously.
SOURCE: https://github.com/asana/node/blob/main/doc/api/net.md#_snippet_6

LANGUAGE: js
CODE:
```
const controller = new AbortController();
server.listen({
  host: 'localhost',
  port: 80,
  signal: controller.signal,
});
// Later, when you want to close the server.
controller.abort();
```

----------------------------------------

TITLE: Mutating Node.js URL Query via searchParams - JavaScript
DESCRIPTION: Demonstrates accessing the read-only `URLSearchParams` object via `myURL.searchParams` and using its methods (like `sort()`) to modify the URL's query parameters. Highlights the difference in percent-encoding behavior between directly setting `url.search` and modifying the `URLSearchParams` object.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_25

LANGUAGE: javascript
CODE:
```
const myURL = new URL('https://example.org/abc?foo=~bar');

console.log(myURL.search);  // prints ?foo=~bar

// Modify the URL via searchParams...
myURL.searchParams.sort();

console.log(myURL.search);  // prints ?foo=%7Ebar
```

----------------------------------------

TITLE: Writing Unsigned Integer (BE) to Buffer - Node.js JavaScript (ESM)
DESCRIPTION: Writes an unsigned integer value into the Buffer at the specified offset using big-endian byte order. This method supports writing up to 48 bits of accuracy. The value must be an unsigned integer.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_105

LANGUAGE: JavaScript
CODE:
```
import { Buffer } from 'node:buffer';

const buf = Buffer.allocUnsafe(6);

buf.writeUIntBE(0x1234567890ab, 0, 6);

console.log(buf);
// Prints: <Buffer 12 34 56 78 90 ab>
```

----------------------------------------

TITLE: Specifying Multiple Licenses (SPDX Expression) in package.json
DESCRIPTION: Defines the package's license using an SPDX license expression string, allowing for multiple licenses or complex conditions. Recommended for packages under more than one common license.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_4

LANGUAGE: JSON
CODE:
```
{
  "license" : "(ISC OR GPL-3.0)"
}
```

----------------------------------------

TITLE: Specifying Git URLs for NPM Packages (Bash)
DESCRIPTION: Provides examples of valid Git URL formats that can be used in a package.json's dependencies or when installing packages directly via npm. Shows different protocols like git, ssh, http, and https, and how to specify a commit-ish (tag, sha, or branch).
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/using-npm/developers.md#_snippet_0

LANGUAGE: bash
CODE:
```
git://github.com/user/project.git#commit-ish
git+ssh://user@hostname:project.git#commit-ish
git+http://user@hostname/project/blah.git#commit-ish
git+https://user@hostname/project/blah.git#commit-ish
```

----------------------------------------

TITLE: Creating Buffer from ArrayBuffer (Node.js Buffer - mjs/cjs)
DESCRIPTION: This snippet demonstrates creating a Buffer view that shares memory with an existing `ArrayBuffer`, typically obtained from a `TypedArray`'s `.buffer` property, using `Buffer.from(arrayBuffer)`. It shows that modifications to the original TypedArray's memory are reflected in the Buffer. Examples are provided for both ES Modules (mjs) and CommonJS (cjs).
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_22

LANGUAGE: mjs
CODE:
```
import { Buffer } from 'node:buffer';

const arr = new Uint16Array(2);

arr[0] = 5000;
arr[1] = 4000;

// Shares memory with `arr`.
const buf = Buffer.from(arr.buffer);

console.log(buf);
// Prints: <Buffer 88 13 a0 0f>

// Changing the original Uint16Array changes the Buffer also.
arr[1] = 6000;

console.log(buf);
// Prints: <Buffer 88 13 70 17>
```

LANGUAGE: cjs
CODE:
```
const { Buffer } = require('node:buffer');

const arr = new Uint16Array(2);

arr[0] = 5000;
arr[1] = 4000;

// Shares memory with `arr`.
const buf = Buffer.from(arr.buffer);

console.log(buf);
// Prints: <Buffer 88 13 a0 0f>

// Changing the original Uint16Array changes the Buffer also.
arr[1] = 6000;

console.log(buf);
// Prints: <Buffer 88 13 70 17>
```

----------------------------------------

TITLE: Providing Explicit CJS and ESM Exports (JSON)
DESCRIPTION: Configures a Node.js package.json using the "exports" field to define the default package entry point ('.') as a CommonJS file ('index.cjs') and an alternative subpath export ('./module') as an ES module file ('index.mjs'). This gives consumers explicit control over which module format they load, simplifying resolution compared to conditional exports while still offering both options.
SOURCE: https://github.com/asana/node/blob/main/doc/api/packages.md#_snippet_29

LANGUAGE: json
CODE:
```
{
  "type": "module",
  "exports": {
    ".": "./index.cjs",
    "./module": "./index.mjs"
  }
}
```

----------------------------------------

TITLE: Updating Local Branch for Pull Request (Bash)
DESCRIPTION: This snippet provides the basic Git commands to stage changes, commit them locally, and push the updated commit to the remote branch on your fork. This action automatically updates the corresponding pull request on GitHub after you've made modifications based on feedback.
SOURCE: https://github.com/asana/node/blob/main/doc/contributing/pull-requests.md#_snippet_10

LANGUAGE: bash
CODE:
```
git add my/changed/files
git commit
git push origin my-branch
```

----------------------------------------

TITLE: Resetting Timer Mocks in Node.js Test (CJS)
DESCRIPTION: Restores the default behavior of all mocks previously created by the `MockTimers` instance associated with the mock tracker. This function is automatically called after each test by the test context's mock tracker. Requires the `node:test` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_43

LANGUAGE: javascript
CODE:
```
const { mock } = require('node:test');
mock.timers.reset();
```

----------------------------------------

TITLE: Running npm rebuild Command
DESCRIPTION: This snippet shows the basic syntax for the `npm rebuild` command in bash. It can be run without arguments to rebuild all packages or with specific package specifiers to target individual packages. The `rb` alias can also be used as a shortcut.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/commands/npm-rebuild.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm rebuild [<package-spec>] ...]

alias: rb
```

----------------------------------------

TITLE: Testing Promise Rejection with Error Class Validation in Node.js
DESCRIPTION: Shows how to use `assert.rejects` with a rejected promise directly, rather than an async function. The example validates that the rejected error is an instance of the specified `Error` class.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_50

LANGUAGE: mjs
CODE:
```
import assert from 'node:assert/strict';

assert.rejects(
  Promise.reject(new Error('Wrong value')),
  Error,
).then(() => {
  // ...
});
```

LANGUAGE: cjs
CODE:
```
const assert = require('node:assert/strict');

assert.rejects(
  Promise.reject(new Error('Wrong value')),
  Error,
).then(() => {
  // ...
});
```

----------------------------------------

TITLE: Truncating File with Node.js fs/promises JavaScript
DESCRIPTION: Demonstrates truncating a file using `filehandle.truncate` from the `node:fs/promises` module. Opens 'temp.txt' in read-write mode, truncates it to 4 bytes, and ensures the file handle is closed using a `try...finally` block and optional chaining. Requires the `node:fs/promises` module.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_17

LANGUAGE: javascript
CODE:
```
import { open } from 'node:fs/promises';

let filehandle = null;
try {
  filehandle = await open('temp.txt', 'r+');
  await filehandle.truncate(4);
} finally {
  await filehandle?.close();
}
```

----------------------------------------

TITLE: Specifying Bugs Object in package.json
DESCRIPTION: Defines the issue tracker URL and/or email address for reporting issues with the package. Used by the `npm bugs` command.
SOURCE: https://github.com/asana/node/blob/main/deps/npm/docs/content/configuring-npm/package-json.md#_snippet_1

LANGUAGE: JSON
CODE:
```
{
  "bugs": {
    "url": "https://github.com/owner/project/issues",
    "email": "project@hostname.com"
  }
}
```

----------------------------------------

TITLE: Initializing WebSocket with Protocols Array (Node.js/undici)
DESCRIPTION: This example illustrates the standard way to create a WebSocket connection using `undici` in Node.js when no custom dispatcher is needed. It initializes the connection to the given URL and requests specific subprotocols by passing an array of strings as the second argument. This is the recommended pattern for most basic WebSocket connections where only subprotocols are specified.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/WebSocket.md#_snippet_1

LANGUAGE: mjs
CODE:
```
import { WebSocket } from 'undici'

const ws = new WebSocket('wss://echo.websocket.events', ['echo', 'chat'])
```

----------------------------------------

TITLE: Renaming File with Callback (Node.js fs)
DESCRIPTION: This snippet demonstrates how to asynchronously rename a file using the `fs.rename` function from the Node.js file system module. It takes the original path and the new path as arguments, along with a callback function that handles completion or potential errors during the operation.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_60

LANGUAGE: javascript
CODE:
```
import { rename } from 'node:fs';

rename('oldFile.txt', 'newFile.txt', (err) => {
  if (err) throw err;
  console.log('Rename complete!');
});
```

----------------------------------------

TITLE: Basic Asynchronous Usage of find-up in Node.js
DESCRIPTION: Demonstrates the basic usage of the findUp function with a single file name, an array of file names, and an asynchronous matcher function to find a directory by walking up parent directories.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/find-up/readme.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const path = require('path');
const findUp = require('find-up');

(async () => {
	console.log(await findUp('unicorn.png'));
	//=> '/Users/sindresorhus/unicorn.png'

	console.log(await findUp(['rainbow.png', 'unicorn.png']));
	//=> '/Users/sindresorhus/unicorn.png'

	console.log(await findUp(async directory => {
		const hasUnicorns = await findUp.exists(path.join(directory, 'unicorn.png'));
		return hasUnicorns && directory;
	}, {type: 'directory'}));
	//=> '/Users/sindresorhus'
})();
```

----------------------------------------

TITLE: Mocking a GET Request with undici MockPool and setGlobalDispatcher (JavaScript)
DESCRIPTION: Shows how to configure `undici` to use a `MockAgent` globally via `setGlobalDispatcher`. It then defines an intercept rule on a `MockPool` for a `GET` request to `/foo` and specifies a simple `200 OK` reply with a string body. Finally, it executes the mocked request using `undici.request` and logs the response status and body.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/MockPool.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import { MockAgent, setGlobalDispatcher, request } from 'undici'

const mockAgent = new MockAgent()
setGlobalDispatcher(mockAgent)

// MockPool
const mockPool = mockAgent.get('http://localhost:3000')
mockPool.intercept({ path: '/foo' }).reply(200, 'foo')

const {
  statusCode,
  body
} = await request('http://localhost:3000/foo')

console.log('response received', statusCode) // response received 200

for await (const data of body) {
  console.log('data', data.toString('utf8')) // data foo
}
```

----------------------------------------

TITLE: Advancing Mocked Time to Trigger setTimeout in Node.js Test (MJS)
DESCRIPTION: Demonstrates using `context.mock.timers.tick(milliseconds)` to advance the mocked time, triggering a pending `setTimeout` callback immediately without waiting. Requires `node:assert` and `node:test` modules.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_44

LANGUAGE: javascript
CODE:
```
import assert from 'node:assert';
import { test } from 'node:test';

test('mocks setTimeout to be executed synchronously without having to actually wait for it', (context) => {
  const fn = context.mock.fn();

  context.mock.timers.enable({ apis: ['setTimeout'] });

  setTimeout(fn, 9999);

  assert.strictEqual(fn.mock.callCount(), 0);

  // Advance in time
  context.mock.timers.tick(9999);

  assert.strictEqual(fn.mock.callCount(), 1);
});
```

----------------------------------------

TITLE: Executing Shell Command with spawn and Handling Output
DESCRIPTION: Demonstrates how to spawn a command (`ls -lh /usr`), capture its standard output (`stdout`) and standard error (`stderr`) using data event listeners, and log the child process's exit code.
SOURCE: https://github.com/asana/node/blob/main/doc/api/child_process.md#_snippet_12

LANGUAGE: javascript
CODE:
```
const { spawn } = require('node:child_process');
const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`child process exited with code ${code}`);
});
```

----------------------------------------

TITLE: AsyncLocalStorage HTTP Request Logging (CommonJS)
DESCRIPTION: This example, functionally identical to the MJS version, demonstrates using `AsyncLocalStorage` within an HTTP server using the CommonJS `require` syntax. It shows how to associate a request ID with logs across asynchronous operations (`setImmediate`) by using `run()` to set the context and `getStore()` to retrieve it within the request's asynchronous flow.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
const http = require('node:http');
const { AsyncLocalStorage } = require('node:async_hooks');

const asyncLocalStorage = new AsyncLocalStorage();

function logWithId(msg) {
  const id = asyncLocalStorage.getStore();
  console.log(`${id !== undefined ? id : '-'}:`, msg);
}

let idSeq = 0;
http.createServer((req, res) => {
  asyncLocalStorage.run(idSeq++, () => {
    logWithId('start');
    // Imagine any chain of async operations here
    setImmediate(() => {
      logWithId('finish');
      res.end();
    });
  });
}).listen(8080);

http.get('http://localhost:8080');
http.get('http://localhost:8080');
// Prints:
//   0: start
//   1: start
//   0: finish
//   1: finish
```

----------------------------------------

TITLE: Deriving Key Sync with Scrypt - Node.js Crypto
DESCRIPTION: Illustrates using the synchronous `crypto.scryptSync` function to derive a cryptographic key from a password and salt using the scrypt algorithm. It shows both default options and specifying a custom cost parameter (N). The derived key is returned directly as a Buffer.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_53

LANGUAGE: javascript
CODE:
```
const {
  scryptSync,
} = await import('node:crypto');
// Using the factory defaults.

const key1 = scryptSync('password', 'salt', 64);
console.log(key1.toString('hex'));  // '3745e48...08d59ae'
// Using a custom N parameter. Must be a power of two.
const key2 = scryptSync('password', 'salt', 64, { N: 1024 });
console.log(key2.toString('hex'));  // '3745e48...aa39b34'
```

LANGUAGE: javascript
CODE:
```
const {
  scryptSync,
} = require('node:crypto');
// Using the factory defaults.

const key1 = scryptSync('password', 'salt', 64);
console.log(key1.toString('hex'));  // '3745e48...08d59ae'
// Using a custom N parameter. Must be a power of two.
const key2 = scryptSync('password', 'salt', 64, { N: 1024 });
console.log(key2.toString('hex'));  // '3745e48...aa39b34'
```

----------------------------------------

TITLE: Initializing Node.js OS Module
DESCRIPTION: Demonstrates the standard way to import the built-in Node.js `os` module using the `require` function. This module provides operating system-related utility methods and properties and is essential for accessing OS details.
SOURCE: https://github.com/asana/node/blob/main/doc/api/os.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const os = require('node:os');
```

----------------------------------------

TITLE: Using assert.match Node.js MJS
DESCRIPTION: Provides examples of using `assert.match` in Node.js ES modules to assert that a string matches a given regular expression. It shows a successful match, a failed match, and an error case where the input is not a string.
SOURCE: https://github.com/asana/node/blob/main/doc/api/assert.md#_snippet_44

LANGUAGE: javascript
CODE:
```
import assert from 'node:assert/strict';

assert.match('I will fail', /pass/);
// AssertionError [ERR_ASSERTION]: The input did not match the regular ...

assert.match(123, /pass/);
// AssertionError [ERR_ASSERTION]: The "string" argument must be of type string.

assert.match('I will pass', /pass/);
// OK
```

----------------------------------------

TITLE: Iterating Directory Entries with fsPromises.opendir (Node.js mjs)
DESCRIPTION: Demonstrates how to use `fsPromises.opendir` to asynchronously open a directory and iterate over its entries (`fs.Dirent` objects) using `for await...of`. Requires importing `opendir` from `node:fs/promises`. Includes basic error handling.
SOURCE: https://github.com/asana/node/blob/main/doc/api/fs.md#_snippet_23

LANGUAGE: javascript
CODE:
```
import { opendir } from 'node:fs/promises';

try {
  const dir = await opendir('./');
  for await (const dirent of dir)
    console.log(dirent.name);
} catch (err) {
  console.error(err);
}
```

----------------------------------------

TITLE: Wrapping Non-Stream Source with Readable Stream (JavaScript)
DESCRIPTION: Creates a custom `Readable` stream (`SourceWrapper`) that wraps a hypothetical lower-level source object (`_source`). It uses the `push()` method to add data from the source's `ondata` callback to the stream's internal buffer and handles flow control by calling `_source.readStop()` when `push()` returns `false`. The `_read()` method calls `_source.readStart()` to signal readiness for data.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_86

LANGUAGE: JavaScript
CODE:
```
// `_source` is an object with readStop() and readStart() methods,
// and an `ondata` member that gets called when it has data, and
// an `onend` member that gets called when the data is over.

class SourceWrapper extends Readable {
  constructor(options) {
    super(options);

    this._source = getLowLevelSourceObject();

    // Every time there's data, push it into the internal buffer.
    this._source.ondata = (chunk) => {
      // If push() returns false, then stop reading from source.
      if (!this.push(chunk))
        this._source.readStop();
    };

    // When the source ends, push the EOF-signaling `null` chunk.
    this._source.onend = () => {
      this.push(null);
    };
  }
  // _read() will be called when the stream wants to pull more data in.
  // The advisory size argument is ignored in this case.
  _read(size) {
    this._source.readStart();
  }
}
```

----------------------------------------

TITLE: Create and Return JavaScript Object from C++ Addon
DESCRIPTION: This C++ snippet defines `CreateObject` that demonstrates how to create a new V8 `Object` instance. It then sets a property named 'msg' on the object, using a string argument passed from JavaScript, and returns the newly created object back to the JavaScript environment.
SOURCE: https://github.com/asana/node/blob/main/doc/api/addons.md#_snippet_18

LANGUAGE: cpp
CODE:
```
// addon.cc
#include <node.h>

namespace demo {

using v8::Context;
using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void CreateObject(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  Local<Context> context = isolate->GetCurrentContext();

  Local<Object> obj = Object::New(isolate);
  obj->Set(context,
           String::NewFromUtf8(isolate,
                               "msg").ToLocalChecked(),
                               args[0]->ToString(context).ToLocalChecked())
           .FromJust();

  args.GetReturnValue().Set(obj);
}

void Init(Local<Object> exports, Local<Object> module) {
  NODE_SET_METHOD(module, "exports", CreateObject);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, Init)

}  // namespace demo
```

----------------------------------------

TITLE: Collecting HTTP/2 Performance Metrics (Node.js)
DESCRIPTION: This snippet demonstrates using the `PerformanceObserver` API from `node:perf_hooks` to collect `http2` performance entries. It shows how to create an observer, register it to observe `http2` entry types, and process the entries received, which can be either `Http2Session` or `Http2Stream` metrics.
SOURCE: https://github.com/asana/node/blob/main/doc/api/http2.md#_snippet_64

LANGUAGE: js
CODE:
```
const { PerformanceObserver } = require('node:perf_hooks');

const obs = new PerformanceObserver((items) => {
  const entry = items.getEntries()[0];
  console.log(entry.entryType);  // prints 'http2'
  if (entry.name === 'Http2Session') {
    // Entry contains statistics about the Http2Session
  } else if (entry.name === 'Http2Stream') {
    // Entry contains statistics about the Http2Stream
  }
});
obs.observe({ entryTypes: ['http2'] });
```

----------------------------------------

TITLE: Logging Tabular Data with console.table in Node.js
DESCRIPTION: Shows examples of using `console.table` to log data. It includes cases where the data cannot be parsed as tabular (like Symbol or undefined) and cases where it can (like an array of objects), demonstrating how to filter columns using the optional `properties` argument.
SOURCE: https://github.com/asana/node/blob/main/doc/api/console.md#_snippet_1

LANGUAGE: javascript
CODE:
```
// These can't be parsed as tabular data
console.table(Symbol());
// Symbol()

console.table(undefined);
// undefined

console.table([{ a: 1, b: 'Y' }, { a: 'Z', b: 2 }]);
// ┌─────────┬─────┬─────┐
// │ (index) │ a   │ b   │
// ├─────────┼─────┼─────┤
// │ 0       │ 1   │ 'Y' │
// │ 1       │ 'Z' │ 2   │
// └─────────┴─────┴─────┘

console.table([{ a: 1, b: 'Y' }, { a: 'Z', b: 2 }], ['a']);
// ┌─────────┬─────┐
// │ (index) │ a │
// ├─────────┼─────┤
// │ 0 │ 1 │
// │ 1 │ 'Z' │
// └─────────┴─────┘
```

----------------------------------------

TITLE: Generating V8 Heap Snapshots Near Limit (Shell)
DESCRIPTION: Shows how to configure Node.js to automatically write V8 heap snapshots to disk when the heap usage approaches the limit. The `--heapsnapshot-near-heap-limit=max_count` flag controls the maximum number of snapshots written. This example also uses `--max-old-space-size` to set a smaller heap limit for demonstration.
SOURCE: https://github.com/asana/node/blob/main/doc/api/cli.md#_snippet_9

LANGUAGE: Shell
CODE:
```
node --max-old-space-size=100 --heapsnapshot-near-heap-limit=3 index.js
```

----------------------------------------

TITLE: Implementing Synchronous readline Completer (JavaScript)
DESCRIPTION: Defines a synchronous `completer` function for Tab autocompletion. It filters a predefined list of commands based on the current line input and returns matching hits or the full list if no matches are found.
SOURCE: https://github.com/asana/node/blob/main/doc/api/readline.md#_snippet_20

LANGUAGE: javascript
CODE:
```
function completer(line) {
  const completions = '.help .error .exit .quit .q'.split(' ');
  const hits = completions.filter((c) => c.startsWith(line));
  // Show all completions if none found
  return [hits.length ? hits : completions, line];
}
```

----------------------------------------

TITLE: Encoding and Decoding Strings - ESM
DESCRIPTION: Illustrates converting strings to Buffers and Buffers back to strings using various character encodings like UTF-8, Hex, Base64, and UTF-16LE with ES Module syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

const buf = Buffer.from('hello world', 'utf8');

console.log(buf.toString('hex'));
// Prints: 68656c6c6f20776f726c64
console.log(buf.toString('base64'));
// Prints: aGVsbG8gd29ybGQ=

console.log(Buffer.from('fhqwhgads', 'utf8'));
// Prints: <Buffer 66 68 71 77 68 67 61 64 73>
console.log(Buffer.from('fhqwhgads', 'utf16le'));
// Prints: <Buffer 66 00 68 00 71 00 77 00 68 00 67 00 61 00 64 00 73 00>
```

----------------------------------------

TITLE: Adding Listeners and Prepending with emitter.on/prependListener (ESM)
DESCRIPTION: Demonstrates adding a listener with `on` and then prepending another listener with `prependListener` for the same event using ES Module syntax. The prepended listener is executed first when the event is emitted. The example imports `EventEmitter` from `node:events`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_34

LANGUAGE: javascript
CODE:
```
import { EventEmitter } from 'node:events';
const myEE = new EventEmitter();
myEE.on('foo', () => console.log('a'));
myEE.prependListener('foo', () => console.log('b'));
myEE.emit('foo');
```

----------------------------------------

TITLE: Starting net.Server with Exclusive Port Option
DESCRIPTION: This example shows how to use the `server.listen(options)` signature to start a TCP server listening on a specific host and port with the `exclusive` option set to `true`. Setting `exclusive: true` prevents cluster workers from sharing the underlying handle, ensuring only one worker listens on that specific port.
SOURCE: https://github.com/asana/node/blob/main/doc/api/net.md#_snippet_5

LANGUAGE: js
CODE:
```
server.listen({
  host: 'localhost',
  port: 80,
  exclusive: true,
});
```

----------------------------------------

TITLE: Install Package via npm Shell
DESCRIPTION: Install the `escape-string-regexp` Node.js package using the npm package manager. This command adds the package to your project's dependencies. Requires Node.js and npm installed.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/escape-string-regexp/readme.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install escape-string-regexp
```

----------------------------------------

TITLE: Using HEAD Method for Headers in Fetch JavaScript
DESCRIPTION: This snippet shows how to use the `HEAD` HTTP method with `fetch` to retrieve only the headers of a resource. Using `HEAD` is the most efficient way to get headers, as it avoids transferring the response body altogether, eliminating the need for manual consumption or cancellation. This is an alternative to fetching the full response body just to access headers.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/README.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
const headers = await fetch(url, { method: 'HEAD' })
  .then(res => res.headers)
```

----------------------------------------

TITLE: Implementing Worker Task Processor Script (MJS)
DESCRIPTION: This code snippet shows the content of a worker thread script (`task_processor.js`) designed to receive a message containing two numbers (`a` and `b`) and post their sum back to the parent thread. It uses ECMAScript module syntax.
SOURCE: https://github.com/asana/node/blob/main/doc/api/async_context.md#_snippet_14

LANGUAGE: JavaScript (MJS)
CODE:
```
import { parentPort } from 'node:worker_threads';
parentPort.on('message', (task) => {
  parentPort.postMessage(task.a + task.b);
});
```

----------------------------------------

TITLE: Importing and Using Function from ES Module in Node.js
DESCRIPTION: This snippet demonstrates how to import the `addTwo` function from the `./addTwo.mjs` file using the `import` statement. It then calls the imported function with an argument (4) and prints the result (6) to the console, showing how to consume exported members from another ES module. Requires the `addTwo.mjs` file to exist in the same directory.
SOURCE: https://github.com/asana/node/blob/main/doc/api/esm.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { addTwo } from './addTwo.mjs';

// Prints: 6
console.log(addTwo(4));
```

----------------------------------------

TITLE: Changing Node.js Mock Function Behavior with mockImplementation
DESCRIPTION: This snippet demonstrates how to create a mock function using `t.mock.fn()` and then change its implementation dynamically for all subsequent calls using `fn.mock.mockImplementation()`. It shows the initial behavior, the change, and the resulting behavior.
SOURCE: https://github.com/asana/node/blob/main/doc/api/test.md#_snippet_32

LANGUAGE: javascript
CODE:
```
test('changes a mock behavior', (t) => {
  let cnt = 0;

  function addOne() {
    cnt++;
    return cnt;
  }

  function addTwo() {
    cnt += 2;
    return cnt;
  }

  const fn = t.mock.fn(addOne);

  assert.strictEqual(fn(), 1);
  fn.mock.mockImplementation(addTwo);
  assert.strictEqual(fn(), 3);
  assert.strictEqual(fn(), 5);
});
```

----------------------------------------

TITLE: Removing Event Listener with emitter.removeListener (JS)
DESCRIPTION: Removes the specified listener function from the listeners array for the given event name. If the same listener was added multiple times, only the most recent instance is removed per call. The method returns the emitter instance for chaining.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_41

LANGUAGE: javascript
CODE:
```
const callback = (stream) => {
  console.log('someone connected!');
};
server.on('connection', callback);
// ...
server.removeListener('connection', callback);
```

----------------------------------------

TITLE: Getting and Setting Node.js URL Username - JavaScript
DESCRIPTION: Shows how to access the username portion of a URL using `myURL.username` and how to modify it. Demonstrates that changing the username updates the full URL string (`myURL.href`). Invalid URL characters assigned to `username` are percent-encoded.
SOURCE: https://github.com/asana/node/blob/main/doc/api/url.md#_snippet_26

LANGUAGE: javascript
CODE:
```
const myURL = new URL('https://abc:xyz@example.com');
console.log(myURL.username);
// Prints abc

myURL.username = '123';
console.log(myURL.href);
// Prints https://123:xyz@example.com/
```

----------------------------------------

TITLE: Creating Uint32Array from Buffer (Copy) - JavaScript
DESCRIPTION: Demonstrates creating a `Uint32Array` from a Node.js `Buffer` instance. The `Uint32Array` constructor copies the content of the Buffer, interpreting its byte data as an array of integers rather than a sequence of bytes of the target type. The original Buffer's integer values `[1, 2, 3, 4]` are directly mapped to the TypedArray elements.
SOURCE: https://github.com/asana/node/blob/main/doc/api/buffer.md#_snippet_6

LANGUAGE: javascript
CODE:
```
import { Buffer } from 'node:buffer';

const buf = Buffer.from([1, 2, 3, 4]);
const uint32array = new Uint32Array(buf);

console.log(uint32array);

// Prints: Uint32Array(4) [ 1, 2, 3, 4 ]
```

LANGUAGE: javascript
CODE:
```
const { Buffer } = require('node:buffer');

const buf = Buffer.from([1, 2, 3, 4]);
const uint32array = new Uint32Array(buf);

console.log(uint32array);

// Prints: Uint32Array(4) [ 1, 2, 3, 4 ]
```

----------------------------------------

TITLE: Checking if a value is a Function using typeof in JavaScript
DESCRIPTION: The `util.isFunction()` API is deprecated. The standard and simplest way to check if a value is a function in JavaScript is by using the `typeof` operator.
SOURCE: https://github.com/asana/node/blob/main/doc/api/deprecations.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
typeof arg === 'function'
```

----------------------------------------

TITLE: Handling Readable Event and Reading Data - Node.js Streams JavaScript
DESCRIPTION: Demonstrates using the 'readable' event, which is emitted when there is data available to read or the end of the stream is reached. The code enters a loop to repeatedly call `read()` until it returns `null`, consuming all available data in the internal buffer.
SOURCE: https://github.com/asana/node/blob/main/doc/api/stream.md#_snippet_27

LANGUAGE: JavaScript
CODE:
```
const readable = getReadableStreamSomehow();
readable.on('readable', function() {
  // There is some data to read now.
  let data;

  while ((data = this.read()) !== null) {
    console.log(data);
  }
});
```

----------------------------------------

TITLE: Subscribing to Undici Request Create Diagnostics Channel (Node.js)
DESCRIPTION: Demonstrates how to subscribe to the `undici:request:create` channel, published when a new outgoing request is initiated. The event data includes details about the request such as origin, method, path, and headers, and allows header modification before sending. This requires the Node.js `diagnostics_channel` module and Node.js v16+.
SOURCE: https://github.com/asana/node/blob/main/deps/undici/src/docs/api/DiagnosticsChannel.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import diagnosticsChannel from 'diagnostics_channel'

diagnosticsChannel.channel('undici:request:create').subscribe(({ request }) => {
  console.log('origin', request.origin)
  console.log('completed', request.completed)
  console.log('method', request.method)
  console.log('path', request.path)
  console.log('headers') // raw text, e.g: 'bar: bar\r\n'
  request.addHeader('hello', 'world')
  console.log('headers', request.headers) // e.g. 'bar: bar\r\nhello: world\r\n'
})
```

----------------------------------------

TITLE: Deriving Key with Sync HKDF (CommonJS)
DESCRIPTION: This snippet demonstrates the usage of the synchronous `crypto.hkdfSync` function in Node.js using CommonJS modules. It requires `node:crypto` and `Buffer`, then calls `hkdfSync` with the digest, input key material, salt, info, and key length. The derived key is returned synchronously and logged as a hexadecimal string.
SOURCE: https://github.com/asana/node/blob/main/doc/api/crypto.md#_snippet_38

LANGUAGE: JavaScript
CODE:
```
const {
  hkdfSync,
} = require('node:crypto');
const { Buffer } = require('node:buffer');

const derivedKey = hkdfSync('sha512', 'key', 'salt', 'info', 64);
console.log(Buffer.from(derivedKey).toString('hex'));  // '24156e2...5391653'
```

----------------------------------------

TITLE: Installing text-table Package (npm)
DESCRIPTION: This command demonstrates how to install the `text-table` package using npm, the Node Package Manager. Running this command in your terminal will download and add the package to your project's dependencies, making it ready for use.
SOURCE: https://github.com/asana/node/blob/main/tools/node_modules/eslint/node_modules/text-table/readme.markdown#_snippet_5

LANGUAGE: bash
CODE:
```
npm install text-table
```

----------------------------------------

TITLE: Encoding String Into Existing Uint8Array with TextEncoder.encodeInto (JavaScript)
DESCRIPTION: This snippet illustrates using the TextEncoder.encodeInto method to encode a string directly into a provided Uint8Array. It requires a TextEncoder instance, a source string, and a destination Uint8Array of sufficient size. The method returns an object indicating how many code units were read from the source and how many bytes were written to the destination array.
SOURCE: https://github.com/asana/node/blob/main/doc/api/util.md#_snippet_35

LANGUAGE: JavaScript
CODE:
```
const encoder = new TextEncoder();
const src = 'this is some data';
const dest = new Uint8Array(10);
const { read, written } = encoder.encodeInto(src, dest);
```

----------------------------------------

TITLE: Trigger Report Programmatically (Specify Filename and Error)
DESCRIPTION: Calls the `writeReport` method with both a filename and an Error object. This generates a diagnostic report, writes it to the specified file, and includes the JavaScript stack trace from the provided error.
SOURCE: https://github.com/asana/node/blob/main/doc/api/report.md#_snippet_4

LANGUAGE: javascript
CODE:
```
try {
  process.chdir('/non-existent-path');
} catch (err) {
  process.report.writeReport(filename, err);
}
// Any other code
```

----------------------------------------

TITLE: Cancelling events.on Iteration with AbortSignal (Node.js)
DESCRIPTION: Shows how to pass an `AbortSignal` to the `options` argument of `events.on` to stop the asynchronous iteration of events. The loop terminates when the signal is aborted. Requires the 'node:events' and 'node:process' modules, and the global `AbortController`.
SOURCE: https://github.com/asana/node/blob/main/doc/api/events.md#_snippet_58

LANGUAGE: mjs
CODE:
```
import { on, EventEmitter } from 'node:events';
import process from 'node:process';

const ac = new AbortController();

(async () => {
  const ee = new EventEmitter();

  // Emit later on
  process.nextTick(() => {
    ee.emit('foo', 'bar');
    ee.emit('foo', 42);
  });

  for await (const event of on(ee, 'foo', { signal: ac.signal })) {
    // The execution of this inner block is synchronous and it
    // processes one event at a time (even with await). Do not use
    // if concurrent execution is required.
    console.log(event); // prints ['bar'] [42]
  }
  // Unreachable here
})();

process.nextTick(() => ac.abort());
```

LANGUAGE: cjs
CODE:
```
const { on, EventEmitter } = require('node:events');

const ac = new AbortController();

(async () => {
  const ee = new EventEmitter();

  // Emit later on
  process.nextTick(() => {
    ee.emit('foo', 'bar');
    ee.emit('foo', 42);
  });

  for await (const event of on(ee, 'foo', { signal: ac.signal })) {
    // The execution of this inner block is synchronous and it
    // processes one event at a time (even with await). Do not use
    // if concurrent execution is required.
    console.log(event); // prints ['bar'] [42]
  }
  // Unreachable here
})();

process.nextTick(() => ac.abort());
```