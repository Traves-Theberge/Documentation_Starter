TITLE: Starting a Dev Server in WebContainer - JavaScript
DESCRIPTION: This snippet shows how to start a dev server inside a WebContainer using `npm run start`. It listens for the `server-ready` event to determine when the server is ready to accept requests. Requires a running `webcontainerInstance` and a project with a `start` script in `package.json`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/running-processes.md#_snippet_3

LANGUAGE: js
CODE:
```
async function startDevServer() {
  // Run `npm run start` to start the Express app
  await webcontainerInstance.spawn('npm', ['run', 'start']);

  // Wait for `server-ready` event
  webcontainerInstance.on('server-ready', (port, url) => {
    // ...
  });
}
```

----------------------------------------

TITLE: Install dependencies function in WebContainer (JavaScript)
DESCRIPTION: This function asynchronously installs dependencies using `npm install` within the WebContainer. It uses `webcontainerInstance.spawn` to run the command and returns a promise that resolves with the exit code of the process. A `0` exit code indicates successful installation.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/3-installing-dependencies.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
async function installDependencies() {
  // Install dependencies
  const installProcess = await webcontainerInstance.spawn('npm', ['install']);
  // Wait for install command to exit
  return installProcess.exit;
}
```

----------------------------------------

TITLE: Initializing WebContainer and Starting Dev Server
DESCRIPTION: This JavaScript code sets up an event listener for the `load` event on the `window` object. It retrieves the content of `index.js`, initializes the WebContainer instance, mounts the files, installs dependencies, and starts the dev server by calling `startDevServer()`. An error is thrown if the dependency installation fails.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/4-running-dev-server.md#_snippet_1

LANGUAGE: javascript
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  const exitCode = await installDependencies();
  if (exitCode !== 0) {
    throw new Error('Installation failed');
  };

  startDevServer();
});
```

----------------------------------------

TITLE: Writing to the terminal in installDependencies
DESCRIPTION: This code snippet updates the `installDependencies` function to pipe the output of the `npm install` process to the terminal.  It takes terminal instance as an argument.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_7

LANGUAGE: js
CODE:
```
/**
 * @param {Terminal} terminal
 */
async function installDependencies(terminal) {
  // Install dependencies
  const installProcess = await webcontainerInstance.spawn('npm', ['install']);
  installProcess.output.pipeTo(new WritableStream({
    write(data) {
      terminal.write(data);
    }
  }))
  // Wait for install command to exit
  return installProcess.exit;
}
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for All Pages - NextJS
DESCRIPTION: This snippet configures COOP and COEP headers for all pages in a Next.js application using the `next.config.js` file. It uses the `headers` configuration option to add the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers to all routes via a wildcard source. This ensures that every page served by Next.js fulfills the cross-origin isolation requirements.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_11

LANGUAGE: javascript
CODE:
```
// https://nextjs.org/docs/api-reference/next.config.js/introduction
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'Cross-Origin-Embedder-Policy',
            value: 'require-corp',
          },
          {
            key: 'Cross-Origin-Opener-Policy',
            value: 'same-origin',
          },
        ],
      },
    ];
  },
};
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for All Pages - Netlify
DESCRIPTION: This snippet shows how to configure COOP and COEP headers for all pages in a `netlify.toml` file.  It adds the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` to all routes.  This enforces cross-origin isolation for all pages served by Netlify, meeting the requirements for WebContainers.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_2

LANGUAGE: yaml
CODE:
```
[[headers]]
  for = "/*"
  [headers.values]
    Cross-Origin-Embedder-Policy = "require-corp"
    Cross-Origin-Opener-Policy = "same-origin"
```

----------------------------------------

TITLE: Listening to textarea Changes and Updating File
DESCRIPTION: This code snippet demonstrates how to listen for changes in a textarea element and trigger a file write operation using the `writeIndexJS` function.  It attaches an 'input' event listener to the `textareaEl` and calls `writeIndexJS` function with the current value of the textarea as the content to be written. `files['index.js'].file.contents` is used to initialize the textarea with initial file content.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/5-editing-a-file-updating-the-iframe.md#_snippet_1

LANGUAGE: javascript
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;

  textareaEl.addEventListener('input', (e) => {
    writeIndexJS(e.currentTarget.value);
  });
});
```

----------------------------------------

TITLE: Reading Process Output in WebContainer - JavaScript
DESCRIPTION: This snippet shows how to read the output of a process spawned in a WebContainer.  It pipes the `output` stream of the process to a `WritableStream`, allowing you to process the output data. Requires a running `webcontainerInstance` and a spawned process.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/running-processes.md#_snippet_2

LANGUAGE: js
CODE:
```
const installProcess = await webcontainerInstance.spawn('npm', ['install']);

  installProcess.output.pipeTo(new WritableStream({
    write(data) {
      console.log(data);
    }
  }));
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for All Pages - Nuxt 3
DESCRIPTION: This snippet configures COOP and COEP headers for all pages in a Nuxt 3 application via the `nuxt.config.js` file.  It modifies the `routeRules` section within the `nitro` configuration to set the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers for all routes. This ensures that every page meets the cross-origin isolation requirements.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_9

LANGUAGE: javascript
CODE:
```
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  nitro: {
    routeRules: {
      '**': {
        headers: {
          'Cross-Origin-Embedder-Policy': 'require-corp',
          'Cross-Origin-Opener-Policy': 'same-origin',
        },
      },
    },
  },
});
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - Cloudflare
DESCRIPTION: This snippet shows how to configure COOP and COEP headers for a specific page (e.g., `/tutorial`) using the `_headers` file in Cloudflare. It sets the `Cross-Origin-Embedder-Policy` to `require-corp` and `Cross-Origin-Opener-Policy` to `same-origin` for only the specified route. This is useful for applying the headers only where WebContainers are used.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_1

LANGUAGE: yaml
CODE:
```
/tutorial
  Cross-Origin-Embedder-Policy: require-corp
  Cross-Origin-Opener-Policy: same-origin
```

----------------------------------------

TITLE: Deleting File or Directory using fs.rm (JavaScript)
DESCRIPTION: This snippet demonstrates how to delete a file or a directory using the `fs.rm` method. For deleting a directory, the `recursive` option must be set to `true`. The `force` option can also be used for deletion. It requires a WebContainer instance to be initialized.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_11

LANGUAGE: javascript
CODE:
```
// Delete a file
await webcontainerInstance.fs.rm('/src/main.js');

// Delete a directory
await webcontainerInstance.fs.rm('/src', { recursive: true });
```

----------------------------------------

TITLE: Starting Dev Server with WebContainers
DESCRIPTION: This JavaScript code defines an asynchronous function `startDevServer` that starts an Express app using `npm run start` within a WebContainer. It listens for the `server-ready` event emitted by the WebContainer instance and then updates the `src` attribute of an iframe element with the URL of the running server.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/4-running-dev-server.md#_snippet_0

LANGUAGE: javascript
CODE:
```
async function startDevServer() {
  // Run `npm run start` to start the Express app
  await webcontainerInstance.spawn('npm', ['run', 'start']);

  // Wait for `server-ready` event
  webcontainerInstance.on('server-ready', (port, url) => {
    iframeEl.src = url;
  });
}
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - SvelteKit (+page.server.js)
DESCRIPTION: This snippet demonstrates setting COOP and COEP headers for a specific page (e.g., `/tutorial`) using the `+page.server.js` file within SvelteKit. The `load` function, which runs on the server, sets the headers using the `setHeaders` function, ensuring that only this specific page has the configured headers. It is the preferred approach when you need to control headers on a per-page basis.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_8

LANGUAGE: javascript
CODE:
```
/** @type {import('./$types').PageServerLoad} */
export const load = ({ setHeaders }) => {
  setHeaders({
    'Cross-Origin-Embedder-Policy': 'require-corp',
    'Cross-Origin-Opener-Policy': 'same-origin',
  });
};
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - NextJS
DESCRIPTION: This snippet shows how to configure COOP and COEP headers for a specific page (e.g., `/tutorial`) within the `next.config.js` file in Next.js. The `headers` configuration option is used to apply the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers only to the `/tutorial` route. This enables cross-origin isolation only where required.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_12

LANGUAGE: javascript
CODE:
```
// https://nextjs.org/docs/api-reference/next.config.js/introduction
module.exports = {
  async headers() {
    return [
      {
        source: '/tutorial',
        headers: [
          {
            key: 'Cross-Origin-Embedder-Policy',
            value: 'require-corp',
          },
          {
            key: 'Cross-Origin-Opener-Policy',
            value: 'same-origin',
          },
        ],
      },
    ];
  },
};
```

----------------------------------------

TITLE: Mounting Snapshots in WebContainer - TypeScript
DESCRIPTION: Shows how to fetch a pre-generated snapshot (binary data) from a server and mount it within a WebContainer instance. It details the steps to fetch the snapshot as an ArrayBuffer and then mount it using `webcontainer.mount()`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { WebContainer } from '@webcontainer/api';

const webcontainer = await WebContainer.boot();

const snapshotResponse = await fetch('/snapshot');
const snapshot = await snapshotResponse.arrayBuffer();

await webcontainer.mount(snapshot);

```

----------------------------------------

TITLE: Writing to File with WebContainers fs API (main.js)
DESCRIPTION: This code snippet defines an asynchronous function `writeIndexJS` that writes content to the `/index.js` file within the WebContainers file system. It uses the `webcontainerInstance.fs.writeFile` method to perform the file writing operation. The function takes a string `content` as input, which represents the new content to be written to the file.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/5-editing-a-file-updating-the-iframe.md#_snippet_0

LANGUAGE: javascript
CODE:
```
/** @param {string} content*/

async function writeIndexJS(content) {
  await webcontainerInstance.fs.writeFile('/index.js', content);
};
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - Vercel
DESCRIPTION: This snippet demonstrates how to configure COOP and COEP headers for a specific page (e.g., `/tutorial`) using the `vercel.json` file. The configuration specifies a `source` for the `/tutorial` route, setting the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers accordingly for that particular page. It's used when only specific pages require cross-origin isolation.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_5

LANGUAGE: json
CODE:
```
{
  "headers": [
    {
      "source": "/tutorial",
      "headers": [
        {
          "key": "Cross-Origin-Embedder-Policy",
          "value": "require-corp"
        },
        {
          "key": "Cross-Origin-Opener-Policy",
          "value": "same-origin"
        }
      ]
    }
  ]
}
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - SvelteKit (hook)
DESCRIPTION: This snippet shows how to configure COOP and COEP headers for a specific page (e.g., `/tutorial`) within the `hooks.server.js` file in a SvelteKit application. The headers are set conditionally based on the request path. This approach allows for fine-grained control over which pages receive the cross-origin isolation headers.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_7

LANGUAGE: javascript
CODE:
```
export const handle = async ({ request, resolve }) => {
  const response = await resolve(request);

  if (request.path === '/tutorial') {
    response.headers.set('Cross-Origin-Embedder-Policy', 'require-corp');
    response.headers.set('Cross-Origin-Opener-Policy', 'same-origin');
  }

  return response;
};
```

----------------------------------------

TITLE: Mounting project files in WebContainer with JavaScript
DESCRIPTION: This JavaScript snippet demonstrates how to mount project files into the WebContainer's file system using the `mount` method. This effectively copies the files into the WebContainer environment for use within the container.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/quickstart.md#_snippet_3

LANGUAGE: javascript
CODE:
```
await webcontainerInstance.mount(projectFiles);
```

----------------------------------------

TITLE: Generating Snapshots for WebContainer - TypeScript
DESCRIPTION: Demonstrates how to generate a snapshot of a folder using `@webcontainer/snapshot` and serve it. It outlines both an Express-based server route and a SvelteKit-like approach for providing the snapshot as a response with the correct content type.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { snapshot } from '@webcontainer/snapshot';

// snapshot is a `Buffer`
const folderSnapshot = await snapshot(SOURCE_CODE_FOLDER);

// for an express-based application
app.get('/snapshot', (req, res) => {
  res
    .setHeader('content-type', 'application/octet-stream')
    .send(snapshot);
});

// for a SvelteKit-like application
export function getSnapshot(req: Request) {
  return new Response(sourceSnapshot, {
    headers: {
      'content-type': 'application/octet-stream',
    },
  });
}

```

----------------------------------------

TITLE: Mounting to a Different Path in WebContainer - JavaScript
DESCRIPTION: Shows how to mount files to a specific path within the WebContainer file system using the `mountPoint` property. It illustrates mounting files to a 'my-mount-point' directory.  Remember to create the directory first using `mkdir`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_6

LANGUAGE: javascript
CODE:
```
/** @type {import('@webcontainer/api').FileSystemTree} */

const files = {
  'package.json': {
    /* Omitted for brevity */
  },
  'index.html': {
    /* Omitted for brevity */
  },
};

await webcontainerInstance.mount(files, { mountPoint: 'my-mount-point' });

```

LANGUAGE: javascript
CODE:
```
await webcontainerInstance.fs.mkdir('my-mount-point');
await webcontainerInstance.mount(files, { mountPoint: 'my-mount-point' });

```

----------------------------------------

TITLE: Starting a Dev Server in WebContainer using JavaScript
DESCRIPTION: This JavaScript function shows how to spawn processes to install dependencies and start a dev server within a WebContainer. It handles the installation process and throws an error if it fails, then it starts the dev server.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/quickstart.md#_snippet_4

LANGUAGE: javascript
CODE:
```
async function startDevServer() {
  const installProcess = await webcontainerInstance.spawn('npm', ['install']);

  const installExitCode = await installProcess.exit;

  if (installExitCode !== 0) {
    throw new Error('Unable to run npm install');
  }

  // `npm run dev`
  await webcontainerInstance.spawn('npm', ['run', 'dev']);
}
```

----------------------------------------

TITLE: Adding CORS Headers with Vite Plugin (JavaScript)
DESCRIPTION: This snippet demonstrates how to create a Vite plugin to add CORS headers to responses in a Vite-based application. It sets the `Cross-Origin-Opener-Policy`, `Cross-Origin-Embedder-Policy`, and `Cross-Origin-Resource-Policy` headers to enable cross-origin isolation. This is particularly useful for WebContainers when dealing with `postMessage` errors related to `SharedArrayBuffers`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/troubleshooting.md#_snippet_0

LANGUAGE: javascript
CODE:
```
   {
      name: 'add-cors',

      configureServer(server) {
        server.middlewares.use((_req, res, next) => {
          res.setHeader('Cross-Origin-Opener-Policy', 'same-origin');
          res.setHeader('Cross-Origin-Embedder-Policy', 'require-corp');
          res.setHeader('Cross-Origin-Resource-Policy', 'cross-origin');
          next();
        });
      },
    }
```

----------------------------------------

TITLE: Writing a File with Encoding using fs.writeFile (JavaScript)
DESCRIPTION: This snippet demonstrates how to write a file with a specific encoding using the `fs.writeFile` method. The third argument allows specifying encoding options like `latin1`. It requires a WebContainer instance to be initialized.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_13

LANGUAGE: javascript
CODE:
```
await webcontainerInstance.fs.writeFile(
  '/src/main.js',
  '\xE5\x8D\x83\xE8\x91\x89\xE5\xB8\x82\xE3\x83\x96\xE3\x83\xAB\xE3\x83\xBC\xE3\x82\xB9',
  { encoding: 'latin1' }
);
```

----------------------------------------

TITLE: Handling Window Resize Event in WebContainer - JavaScript
DESCRIPTION: This code snippet demonstrates how to handle the window resize event to make a terminal within a WebContainer responsive. It uses the `FitAddon` from `Xterm.js` to adjust the terminal's dimensions and calls the `resize` method on the WebContainer shell process to redraw the screen with the new dimensions. The `cols` and `rows` properties are used to pass the new terminal dimensions.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_9

LANGUAGE: javascript
CODE:
```
window.addEventListener('resize', () => {
    fitAddon.fit();
    shellProcess.resize({
      cols: terminal.cols,
      rows: terminal.rows,
    });
  });
```

----------------------------------------

TITLE: Installing Dependencies in WebContainer - JavaScript
DESCRIPTION: This function demonstrates how to install dependencies within a WebContainer instance using `npm install`. It spawns the `npm install` command, waits for the process to exit, and returns the exit code. Requires a running `webcontainerInstance`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/running-processes.md#_snippet_1

LANGUAGE: js
CODE:
```
async function installDependencies() {
  // Install dependencies
  const installProcess = await webcontainerInstance.spawn('npm', ['install']);
  // Wait for install command to exit
  return installProcess.exit;
}
```

----------------------------------------

TITLE: Navigating and Installing Dependencies
DESCRIPTION: These commands navigate into the newly created Vite project directory, install the necessary dependencies using npm, and then start the development server.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_1

LANGUAGE: Bash
CODE:
```
cd webcontainers-express-app
npm install
npm run dev
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for All Pages - Vercel
DESCRIPTION: This snippet demonstrates how to configure COOP and COEP headers for all pages using the `vercel.json` file. The configuration includes a `source` pattern that matches all routes and sets the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers accordingly. This ensures that every page served by Vercel meets the cross-origin isolation requirement.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_4

LANGUAGE: json
CODE:
```
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "Cross-Origin-Embedder-Policy",
          "value": "require-corp"
        },
        {
          "key": "Cross-Origin-Opener-Policy",
          "value": "same-origin"
        }
      ]
    }
  ]
}
```

----------------------------------------

TITLE: Add Terminal Interactivity (JavaScript)
DESCRIPTION: This JavaScript code modifies the `startShell` function to enable terminal interactivity.  It obtains a writer for the shell process's input stream and then attaches an `onData` handler to the terminal. Each time data is received in the terminal, the handler writes that data to the input stream, effectively sending user input to the shell.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
async function startShell(terminal) {
  const shellProcess = await webcontainerInstance.spawn('jsh');
  shellProcess.output.pipeTo(
    new WritableStream({
      write(data) {
        terminal.write(data);
      },
    })
  );

  const input = shellProcess.input.getWriter();
  terminal.onData((data) => {
    input.write(data);
  });

  return shellProcess;
};
```

----------------------------------------

TITLE: Exporting Filesystem Data - Javascript
DESCRIPTION: Demonstrates how to export the filesystem of a WebContainer using the `export` method. It shows exporting a 'dist' directory as a zip file (Uint8Array) and then creating a Blob from the exported data for further use, such as downloading.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_8

LANGUAGE: javascript
CODE:
```
const data = await webcontainerInstance.export('dist', { format: 'zip' });

const zip = new Blob([data]);
```

----------------------------------------

TITLE: Defining SpawnOptions interface - TypeScript
DESCRIPTION: Defines the `SpawnOptions` interface, which allows specifying options for spawning a process within the WebContainer. These options include the current working directory (`cwd`), environment variables (`env`), output control, and terminal settings.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_30

LANGUAGE: ts
CODE:
```
export interface SpawnOptions {
  cwd?: string;
  env?: Record<string, string | number | boolean>;
  output?: boolean;
  terminal?: { cols: number; rows: number };
}
```

----------------------------------------

TITLE: Writing to terminal in startDevServer
DESCRIPTION: This code snippet updates the `startDevServer` function to pipe the output of the `npm run start` process to the terminal. It takes terminal instance as an argument.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_8

LANGUAGE: js
CODE:
```
/**
 * @param {Terminal} terminal
 */
async function startDevServer(terminal) {
  // Run `npm run start` to start the Express app
  const serverProcess = await webcontainerInstance.spawn('npm', [
    'run',
    'start',
  ]);
  serverProcess.output.pipeTo(
    new WritableStream({
      write(data) {
        terminal.write(data);
      },
    })
  );

  // Wait for `server-ready` event
  webcontainerInstance.on('server-ready', (port, url) => {
    iframeEl.src = url;
  });
}
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - Nuxt 3
DESCRIPTION: This snippet demonstrates configuring COOP and COEP headers for a specific page (e.g., `/tutorial`) in a Nuxt 3 application using `nuxt.config.js`. The `routeRules` configuration within the `nitro` object is used to define specific headers for the `/tutorial` route. This ensures that the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers are applied only to the designated page.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_10

LANGUAGE: javascript
CODE:
```
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  nitro: {
    routeRules: {
      '/tutorial': {
        headers: {
          'Cross-Origin-Embedder-Policy': 'require-corp',
          'Cross-Origin-Opener-Policy': 'same-origin',
        },
      },
    },
  },
});
```

----------------------------------------

TITLE: Start Shell Function (JavaScript)
DESCRIPTION: This JavaScript function, `startShell`, spawns a `jsh` shell within the WebContainer. It pipes the shell's output stream to the `Xterm.js` terminal instance, making the shell's output visible in the terminal. Dependencies include `webcontainerInstance` and `Terminal`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
/**
 * @param {Terminal} terminal
 */
async function startShell(terminal) {
  const shellProcess = await webcontainerInstance.spawn('jsh');
  shellProcess.output.pipeTo(
    new WritableStream({
      write(data) {
        terminal.write(data);
      },
    })
  );
  return shellProcess;
};
```

----------------------------------------

TITLE: Import FitAddon (JavaScript)
DESCRIPTION: This line imports the `FitAddon` class from the `@xterm/addon-fit` package. This allows the application to use the addon to automatically resize the terminal.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
import { FitAddon } from '@xterm/addon-fit';
```

----------------------------------------

TITLE: Creating Folders and Files in WebContainer - JavaScript
DESCRIPTION: Demonstrates how to create a directory structure including files within directories in WebContainers. The example showcases how to represent a 'src' directory containing 'main.js' and 'main.css' files along with 'package.json' and 'index.html' files at the root.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_5

LANGUAGE: javascript
CODE:
```
/** @type {import('@webcontainer/api').FileSystemTree} */

const files = {
  // This is a directory - provide its name as a key
  src: {
    // Because it's a directory, add the "directory" key
    directory: {
      // This is a file - provide its path as a key:
      'main.js': {
        // Because it's a file, add the "file" key
        file: {
          contents: `
            console.log('Hello from WebContainers!')
          `,
        },
      },
      // This is another file inside the same folder
      'main.css': {
        // Because it's a file, add the "file" key
        file: {
          contents: `
            body {
              margin: 0;
            }
          `,
        },
      },
    },
  },
  // This is a file outside the folder
  'package.json': {
    /* Omitted for brevity */
  },
  // This is another file outside the folder
  'index.html': {
    /* Omitted for brevity */
  },
};

await webcontainerInstance.mount(files);

```

----------------------------------------

TITLE: Streaming npm install output (JavaScript)
DESCRIPTION: This snippet shows how to capture and process the output stream of the `npm install` command. It uses the `output` property of the process spawned by `webcontainerInstance.spawn` and pipes the output to a `WritableStream`. This allows for real-time logging or processing of the command's output.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/3-installing-dependencies.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
const installProcess = await webcontainerInstance.spawn('npm', ['install']);
  
installProcess.output.pipeTo(new WritableStream({
  write(data) {
    console.log(data);
  }
}));
```

----------------------------------------

TITLE: Call startShell Function (JavaScript)
DESCRIPTION: This JavaScript code calls the `startShell` function at the end of the event listener attached to the `load` event. This initializes the shell after the WebContainer is booted and the files are mounted.  It passes the `terminal` object as a parameter to `startShell`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  textareaEl.addEventListener('input', (e) => {
    writeIndexJS(e.currentTarget.value);
  });

  const terminal = new Terminal({
    convertEol: true,
  });
  terminal.open(terminalEl);

  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  // Wait for `server-ready` event
  webcontainerInstance.on('server-ready', (port, url) => {
    iframeEl.src = url;
  });

  startShell(terminal);
});
```

----------------------------------------

TITLE: Defining FileSystemTree interface - TypeScript
DESCRIPTION: Defines the `FileSystemTree` interface, representing a tree-like structure for defining the contents of a folder to be mounted. It's a map of names to `FileNode`, `SymlinkNode`, or `DirectoryNode` objects.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_25

LANGUAGE: ts
CODE:
```
interface FileSystemTree {
  [name: string]: FileNode | SymlinkNode | DirectoryNode;
}
```

----------------------------------------

TITLE: Spawning a Process in WebContainer - JavaScript
DESCRIPTION: This snippet demonstrates how to spawn a process inside a WebContainer instance using the `spawn` method.  It takes the command as a string and arguments as an array. It returns a `WebContainerProcess` object. Requires a running `webcontainerInstance`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/running-processes.md#_snippet_0

LANGUAGE: js
CODE:
```
webcontainerInstance.spawn('npm', ['install']);
```

LANGUAGE: js
CODE:
```
webcontainerInstance.spawn('ls', ['src', '-l']);
```

----------------------------------------

TITLE: Representing a directory in WebContainers JavaScript
DESCRIPTION: Illustrates how a directory is represented in WebContainers as a JavaScript object.  It contains the directory name as a key and a nested 'directory' key.  Files are added inside the 'directory' key.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_1

LANGUAGE: javascript
CODE:
```
const files = {
  // This is a directory - provide its name as a key
  src: {
    // Because it's a directory, add the "directory" key
    directory: {
      // Here we will add files
    },
  },
};

```

----------------------------------------

TITLE: Setting COOP/COEP Headers for All Pages - Cloudflare
DESCRIPTION: This snippet demonstrates how to configure COOP and COEP headers for all pages using the `_headers` file in Cloudflare. It sets the `Cross-Origin-Embedder-Policy` to `require-corp` and `Cross-Origin-Opener-Policy` to `same-origin` for every route. This configuration ensures that all pages served by Cloudflare meet the cross-origin isolation requirement for WebContainers.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_0

LANGUAGE: yaml
CODE:
```
/*
  Cross-Origin-Embedder-Policy: require-corp
  Cross-Origin-Opener-Policy: same-origin
```

----------------------------------------

TITLE: Defining DirectoryNode interface - TypeScript
DESCRIPTION: Defines the `DirectoryNode` interface, representing a directory in the `FileSystemTree`.  The `directory` property contains another `FileSystemTree` representing the directory's contents.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_29

LANGUAGE: ts
CODE:
```
interface DirectoryNode {
  directory: FileSystemTree;
}
```

----------------------------------------

TITLE: Creating a FileSystemTree example - JavaScript
DESCRIPTION: Example of creating a `FileSystemTree` to represent a project structure.  It demonstrates how to define directories, files with content, and symbolic links within the tree.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_26

LANGUAGE: js
CODE:
```
const tree = {
  myproject: {
    directory: {
      'foo.js': {
        file: {
          contents: 'const x = 1;',
        },
      },
      'bar.js': {
        file: {
          symlink: './foo.js',
        },
      },
      '.envrc': {
        file: {
          contents: 'ENVIRONMENT=staging'
        }
      },
    },
  },
  emptyFolder: {
    directory: {}
  },
};
```

----------------------------------------

TITLE: Remove Unnecessary Code in main.js (JavaScript)
DESCRIPTION: This snippet removes the `installDependencies` and `startDevServer` methods from the `window.addEventListener('load', ...)` block in `main.js`. It retains the `server-ready` event listener, which is still needed to update the iframe's source when the server is ready.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  textareaEl.addEventListener('input', (e) => {
    writeIndexJS(e.currentTarget.value);
  });

  const terminal = new Terminal({
    convertEol: true,
  });
  terminal.open(terminalEl);

  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  // Wait for `server-ready` event
  webcontainerInstance.on('server-ready', (port, url) => {
    iframeEl.src = url;
  });
});
```

----------------------------------------

TITLE: Running npm install command in WebContainer (JavaScript)
DESCRIPTION: This snippet shows how to execute the `npm install` command inside a WebContainer instance using the `webcontainerInstance.spawn` method.  The command is broken down into its constituent parts (`npm` and `install`) and passed as arguments to the `spawn` method. This installs the project dependencies.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/3-installing-dependencies.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
webcontainerInstance.spawn('npm', ['install']);
```

----------------------------------------

TITLE: Vue Component Setup
DESCRIPTION: This is the setup block for a Vue component written in TypeScript. It imports and registers components for use within the template. The `lang="ts"` attribute indicates that the code within the `<script>` tag is TypeScript.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/builder-io-playground.md#_snippet_1

LANGUAGE: vue
CODE:
```
<script setup lang="ts">
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
</script>
```

----------------------------------------

TITLE: Defining ServerReadyListener Type - Typescript
DESCRIPTION: Defines the `ServerReadyListener` type, a function invoked when a server within the WebContainer is started and ready to receive traffic. It receives the port number and the URL of the server.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_5

LANGUAGE: typescript
CODE:
```
(port: number, url: string): void
```

----------------------------------------

TITLE: Spawning a Process with Arguments - Javascript
DESCRIPTION: Illustrates how to spawn a process within the WebContainer with command-line arguments, using the `spawn` method.  In this example, it executes the `npm i` command to install dependencies.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_6

LANGUAGE: javascript
CODE:
```
const install = await webcontainerInstance.spawn('npm', ['i']);
```

----------------------------------------

TITLE: Vue Component Setup with TypeScript
DESCRIPTION: This code snippet sets up a Vue component using TypeScript. It imports necessary components and data, defines the component using `<script setup lang="ts">`, and uses imported data to configure the page.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/stackblitz-web-publisher.md#_snippet_0

LANGUAGE: typescript
CODE:
```
<script setup lang="ts">
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
import VideoLink from '@theme/components/Helpers/VideoLink.vue';
import AttributionLinks from '@theme/components/Helpers/AttributionLinks.vue';
import { people } from '@theme/data/people';
const { SYLWIA_VARGAS } = people;
</script>
```

----------------------------------------

TITLE: Defining PortListener Type - Typescript
DESCRIPTION: Defines the `PortListener` type, which is a function that's called when a port is opened or closed by a process running within the WebContainer. It receives the port number, the type of event ('open' or 'close'), and the URL associated with the port.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_2

LANGUAGE: typescript
CODE:
```
(port: number, type: "open" | "close", url: string): void
```

----------------------------------------

TITLE: Installing WebContainer API using npm
DESCRIPTION: This command installs the WebContainer API package from npm, allowing you to use its functionalities in your project. Ensure you are in the project directory before running this command.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/quickstart.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm i @webcontainer/api
```

----------------------------------------

TITLE: Defining PreviewMessage Type - Typescript
DESCRIPTION: Defines the `PreviewMessage` type, representing messages received from a preview iframe. It's a union of `UncaughtExceptionMessage`, `UnhandledRejectionMessage`, and `ConsoleErrorMessage`, and includes base information such as `previewId`, `port`, `pathname`, `search`, and `hash`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_4

LANGUAGE: typescript
CODE:
```
type PreviewMessage = (UncaughtExceptionMessage | UnhandledRejectionMessage | ConsoleErrorMessage) & BasePreviewMessage;

interface BasePreviewMessage {
    previewId: string;
    port: number;
    pathname: string;
    search: string;
    hash: string;
}

interface UncaughtExceptionMessage {
    type: PreviewMessageType.UncaughtException;
    message: string;
    stack: string | undefined;
}

interface UnhandledRejectionMessage {
    type: PreviewMessageType.UnhandledRejection;
    message: string;
    stack: string | undefined;
}

interface ConsoleErrorMessage {
    type: PreviewMessageType.ConsoleError;
    args: any[];
    stack: string;
}
```

----------------------------------------

TITLE: Defining BootOptions Interface - Typescript
DESCRIPTION: Defines the `BootOptions` interface used when booting a WebContainer instance. It allows configuration of Cross-Origin Embedder Policy (COEP), setting a working directory name, and forwarding preview errors from embedded iframes to the parent page. These options influence the environment and error handling during the WebContainer's lifecycle.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_0

LANGUAGE: typescript
CODE:
```
interface BootOptions {
  coep?: 'require-corp' | 'credentialless' | 'none';
  workdirName?: string;
  forwardPreviewErrors?: boolean | 'exceptions-only';
}
```

----------------------------------------

TITLE: Calling installDependencies function (JavaScript)
DESCRIPTION: This snippet demonstrates how to call the `installDependencies` function within an event listener. It initializes the WebContainer, mounts files, and then calls `installDependencies`.  Error handling is included to catch installation failures. Dependencies include the `files` object and `webcontainerInstance`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/3-installing-dependencies.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  const exitCode = await installDependencies();
  if (exitCode !== 0) {
    throw new Error('Installation failed');
  };
});
```

----------------------------------------

TITLE: Using Vue Component with Data
DESCRIPTION: This HTML-like code snippet utilizes the imported `Home` Vue component and passes the `footerSections` data as a prop. This likely renders the `Home` component with dynamic content provided by the `footerSections` data, customizing its appearance or functionality.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/index.md#_snippet_1

LANGUAGE: html
CODE:
```
<Home :footerSections="footerSections" />
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for All Pages - SvelteKit
DESCRIPTION: This snippet configures COOP and COEP headers for all pages in a SvelteKit application using the `hooks.server.js` file. It intercepts every request, resolves it, and then sets the `Cross-Origin-Embedder-Policy` and `Cross-Origin-Opener-Policy` headers on the response before returning it. This ensures that all pages served have the necessary headers for cross-origin isolation.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_6

LANGUAGE: javascript
CODE:
```
export const handle = async ({ request, resolve }) => {
  const response = await resolve(request);

  response.headers.set('Cross-Origin-Embedder-Policy', 'require-corp');
  response.headers.set('Cross-Origin-Opener-Policy', 'same-origin');

  return response;
};
```

----------------------------------------

TITLE: Auth LoggedIn Example - TypeScript
DESCRIPTION: This snippet demonstrates how to use the `auth.loggedIn` function to wait for a user to be logged in before performing actions like installing private packages. It first boots the WebContainer instance, then waits for authentication using `auth.loggedIn`, and finally spawns an npm install process.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_12

LANGUAGE: ts
CODE:
```
const instance = await WebContainer.boot();

// wait until the user is logged in
await auth.loggedIn();

// we can now fetch private packages from our organisation
await instance.spawn('npm', ['install']);
```

----------------------------------------

TITLE: Vue Component Import
DESCRIPTION: Imports Vue components `PageHeading` and `Screenshot` for use in the current component. `PageHeading` is likely a custom component used for displaying the title and category of the project, while `Screenshot` displays an image with an optional link.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/builder-io-playground.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
```

----------------------------------------

TITLE: Defining Export Options Interface in TypeScript
DESCRIPTION: This TypeScript interface `ExportOptions` defines the structure for configuring data exports. It allows specifying the export `format` (json, binary, or zip) and providing globbing patterns for including (`includes`) or excluding (`excludes`) files during the export process. The `format` defaults to `json` if not specified.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_31

LANGUAGE: typescript
CODE:
```
export interface ExportOptions {
  format?: 'json' | 'binary' | 'zip',
  includes?: string[];
  excludes?: string[];
}
```

----------------------------------------

TITLE: Setting Preview Script - JavaScript
DESCRIPTION: This snippet demonstrates how to set a script that will be injected into all previews within a WebContainer instance. It defines a simple JavaScript script that logs "Hello world!" to the console and then uses the `setPreviewScript` method to apply it to all previews.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_9

LANGUAGE: js
CODE:
```
const script = `
  console.log('Hello world!');
`;

await webcontainerInstance.setPreviewScript(script);

// now all previews will always print hello world to the console if they serve HTML
```

----------------------------------------

TITLE: Running the documentation site in development mode
DESCRIPTION: This code snippet provides the commands needed to set up and run the WebContainer API documentation site locally in development mode. It assumes you have Node.js and npm installed. First, install the dependencies using `npm install`, then start the development server with `npm run dev`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
npm install
npm run dev
```

----------------------------------------

TITLE: Passing Terminal instance to functions
DESCRIPTION: This code snippet updates the `installDependencies` and `startDevServer` function calls to pass the `terminal` instance as an argument. This allows these functions to write output to the terminal.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_6

LANGUAGE: js
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  textareaEl.addEventListener('input', (e) => {
    writeIndexJS(e.currentTarget.value);
  });

  const terminal = new Terminal({
    convertEol: true,
  });
  terminal.open(terminalEl);

  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  const exitCode = await installDependencies(terminal);
  if (exitCode !== 0) {
    throw new Error('Installation failed');
  };

  startDevServer(terminal);
});
```

----------------------------------------

TITLE: Registering Preview Message Event Handler - Javascript
DESCRIPTION: Demonstrates how to register an event handler for 'preview-message' events. These events are emitted by the WebContainer when messages are received from a preview iframe, providing a mechanism for the parent page to process messages such as errors originating from within the previewed content.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_1

LANGUAGE: javascript
CODE:
```
webcontainerInstance.on('preview-message', (message) => {
  // process the message received from a preview
});
```

----------------------------------------

TITLE: Running cd hello-world command in WebContainer (JavaScript)
DESCRIPTION: This code demonstrates how to execute the `cd hello-world` command within a WebContainer instance. The command is split into the command itself (`cd`) and its argument (`hello-world`) and passed to the `webcontainerInstance.spawn` method.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/3-installing-dependencies.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
webcontainerInstance.spawn('cd', ['hello-world']);
```

----------------------------------------

TITLE: Writing a File using fs.writeFile (JavaScript)
DESCRIPTION: This snippet demonstrates how to write a file using the `fs.writeFile` method. If the file does not exist, it will be created. If it exists, it will be overwritten. It requires a WebContainer instance to be initialized.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_12

LANGUAGE: javascript
CODE:
```
await webcontainerInstance.fs.writeFile('/src/main.js', 'console.log("Hello from WebContainers!")');
```

----------------------------------------

TITLE: Load and Fit FitAddon (JavaScript)
DESCRIPTION: This JavaScript code creates a `FitAddon` instance, loads it into the terminal, and then calls the `fit()` method to adjust the terminal's dimensions to fit its container. This ensures that the terminal is properly sized when the page loads.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  textareaEl.addEventListener('input', (e) => {
    writeIndexJS(e.currentTarget.value);
  });

  const fitAddon = new FitAddon();

  const terminal = new Terminal({
    convertEol: true,
  });
  terminal.loadAddon(fitAddon);
  terminal.open(terminalEl);

  fitAddon.fit();

  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  // Wait for `server-ready` event
  webcontainerInstance.on('server-ready', (port, url) => {
    iframeEl.src = url;
  });

  startShell(terminal);
});
```

----------------------------------------

TITLE: SvelteKit Page Heading Component (Vue)
DESCRIPTION: This snippet utilizes the `PageHeading` component to display the title and category of the SvelteKit community project.  It sets the `title` prop to "SvelteKit" and the `category` prop to "tutorial" to provide context for the project documentation.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/sveltekit.md#_snippet_1

LANGUAGE: vue
CODE:
```
<PageHeading title="SvelteKit" category="tutorial" />
```

----------------------------------------

TITLE: Reading file with encoding using readFile - TypeScript
DESCRIPTION: Example demonstrating how to read a file with a specified encoding using the `readFile` method of the WebContainer's file system API.  It specifies the file path and the desired encoding (utf-8) as arguments.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_15

LANGUAGE: ts
CODE:
```
const content = await webcontainerInstance.fs.readFile('/package.json', 'utf-8');
```

----------------------------------------

TITLE: Creating and opening Terminal instance
DESCRIPTION: This code snippet creates a new `Terminal` instance and attaches it to the `terminalEl` element. The `convertEol` option is set to `true` to ensure the cursor always starts at the beginning of the next line.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_4

LANGUAGE: js
CODE:
```
window.addEventListener('load', async () => {
  textareaEl.value = files['index.js'].file.contents;
  textareaEl.addEventListener('input', (e) => {
    writeIndexJS(e.currentTarget.value);
  });

  const terminal = new Terminal({
    convertEol: true,
  });
  terminal.open(terminalEl);

  // Call only once
  webcontainerInstance = await WebContainer.boot();
  await webcontainerInstance.mount(files);

  const exitCode = await installDependencies();
  if (exitCode !== 0) {
    throw new Error('Installation failed');
  };

  startDevServer();
});
```

----------------------------------------

TITLE: Adding StackBlitz Domains to Edge Enhanced Security Exceptions
DESCRIPTION: This code snippet shows the domains that need to be added as exceptions in Microsoft Edge's 'Enhanced your security on the web' settings when set to 'Strict'. This allows StackBlitz projects to run by enabling WebAssembly usage.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/browser-config.md#_snippet_1

LANGUAGE: HTML
CODE:
```
stackblitz.com
[*.]staticblitz.com
```

----------------------------------------

TITLE: Deleting a directory recursively using rm - JavaScript
DESCRIPTION: Demonstrates how to delete a directory and its contents recursively using the `rm` method. It requires setting the `recursive` option to `true`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_20

LANGUAGE: js
CODE:
```
await webcontainerInstance.fs.rm('/src', { recursive: true });
```

----------------------------------------

TITLE: Updating main.js - Adding Textarea and Iframe
DESCRIPTION: This JavaScript snippet updates the content of the `main.js` file to include a `textarea` and an `iframe` within a container. It sets up the basic HTML structure for the editor and preview areas.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
document.querySelector('#app').innerHTML = `
  <div class="container">
    <div class="editor">
      <textarea>I am a textarea</textarea>
    </div>
    <div class="preview">
      <iframe src="loading.html"></iframe>
    </div>
  </div>
`
```

----------------------------------------

TITLE: Loading Multiple Files into WebContainer - JavaScript
DESCRIPTION: Loads multiple files (package.json and index.html) into a WebContainer using the `mount` method. The provided object defines each file and its content, allowing WebContainer to create these files in its virtual file system.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_4

LANGUAGE: javascript
CODE:
```
/** @type {import('@webcontainer/api').FileSystemTree} */

const files = {
  'package.json': {
    file: {
      contents: `
        {
          "name": "vite-starter",
          "private": true,
          "version": "0.0.0",
          "type": "module",
          "scripts": {
            "dev": "vite",
            "build": "vite build",
            "preview": "vite preview"
          },
          "devDependencies": {
            "vite": "^4.0.4"
          }
        }`,
    },
  },
  'index.html': {
    file: {
      contents: `
        <!DOCTYPE html>
        <html lang="en">
          <head>
            <meta charset="UTF-8" />
            <link rel="icon" type="image/svg+xml" href="/vite.svg" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Vite App</title>
          </head>
          <body>
            <div id="app"></div>
            <script type="module" src="/src/main.js"></script>
          </body>
        </html>`,
    },
  },
};

await webcontainerInstance.mount(files);

```

----------------------------------------

TITLE: Writing a file using writeFile - JavaScript
DESCRIPTION: Example showing how to write a file using the `writeFile` method with default options.  It takes the file path and content as arguments.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_21

LANGUAGE: js
CODE:
```
await webcontainerInstance.fs.writeFile('/src/main.js', 'console.log("Hello from WebContainers!")');
```

----------------------------------------

TITLE: Updating main.js - Initial Content
DESCRIPTION: This JavaScript snippet updates the content of the `main.js` file. It imports the CSS file and then clears the content of the element with the ID 'app'.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import './style.css';

document.querySelector('#app').innerHTML = ``;
```

----------------------------------------

TITLE: Renaming a file using rename - JavaScript
DESCRIPTION: Demonstrates how to rename a file using the `rename` method of the WebContainer's file system API. It takes the old path and the new path as arguments.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_17

LANGUAGE: js
CODE:
```
await webcontainerInstance.fs.rename('/src/index.js', '/src/main.js');
```

----------------------------------------

TITLE: Watching a directory recursively using watch - JavaScript
DESCRIPTION: Example showing how to watch a directory and its subdirectories for changes using the `watch` method with the `recursive` option set to `true`. It logs the filename and event type to the console.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_24

LANGUAGE: js
CODE:
```
webcontainerInstance.fs.watch('/src', { recursive: true }, (event, filename) => {
    console.log(`file: ${filename} action: ${event}`);
});
```

----------------------------------------

TITLE: Setting COOP/COEP Headers for Specific Page - Netlify
DESCRIPTION: This snippet shows how to configure COOP and COEP headers for a specific page (e.g., `/tutorial`) in a `netlify.toml` file. It sets the `Cross-Origin-Embedder-Policy` to `require-corp` and `Cross-Origin-Opener-Policy` to `same-origin` for a specified route only. This approach is beneficial when cross-origin isolation is required only for certain pages.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/configuring-headers.md#_snippet_3

LANGUAGE: yaml
CODE:
```
[[headers]]
  for = "/tutorial"
  [headers.values]
    Cross-Origin-Embedder-Policy = "require-corp"
    Cross-Origin-Opener-Policy = "same-origin"
```

----------------------------------------

TITLE: Writing a file with encoding using writeFile - JavaScript
DESCRIPTION: Example demonstrating how to write a file with a specified encoding using the `writeFile` method. It specifies the file path, content, and encoding (latin1) as arguments.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_22

LANGUAGE: js
CODE:
```
await webcontainerInstance.fs.writeFile(
  '/src/main.js',
  '\xE5\x8D\x83\xE8\x91\x89\xE5\xB8\x82\xE3\x83\x96\xE3\x83\xAB\xE3\x83\xBC\xE3\x82\xB9',
  { encoding: 'latin1' }
);
```

----------------------------------------

TITLE: Defining Options interface for readdir - TypeScript
DESCRIPTION: Defines the `Options` interface for the `readdir` method.  It allows specifying the encoding for the returned data and whether to return `Dirent` objects instead of filenames.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_14

LANGUAGE: ts
CODE:
```
interface Options {
  encoding?: BufferEncoding;
  withFileTypes?: boolean;
}
```

----------------------------------------

TITLE: Running ls src -l command in WebContainer (JavaScript)
DESCRIPTION: This example shows how to run the `ls src -l` command inside a WebContainer.  The command is split into its component parts: the command itself (`ls`) and the arguments (`src`, `-l`). These are passed as arguments to the `webcontainerInstance.spawn` method.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/3-installing-dependencies.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
webcontainerInstance.spawn('ls', ['src', '-l']);
```

----------------------------------------

TITLE: Defining Options interface for rm - TypeScript
DESCRIPTION: Defines the `Options` interface for the `rm` method. It includes options to force deletion and recursively remove directories.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_18

LANGUAGE: ts
CODE:
```
interface Options {
  force?: boolean;
  recursive?: boolean;
}
```

----------------------------------------

TITLE: Reading file without encoding using readFile - TypeScript
DESCRIPTION: Example demonstrating how to read a file without specifying an encoding using the `readFile` method. This returns a `UInt8Array` containing the file's content.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_16

LANGUAGE: ts
CODE:
```
const bytes = await webcontainerInstance.fs.readFile('/package.json');
```

----------------------------------------

TITLE: Resize Output Lines in Shell (JavaScript)
DESCRIPTION: This JavaScript code modifies the `startShell` function to pass the terminal's dimensions (columns and rows) to the `webcontainerInstance.spawn` method. This allows the WebContainer process to be aware of the terminal's size and properly wrap the output.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
async function startShell(terminal) {
  const shellProcess = await webcontainerInstance.spawn('jsh', {
    terminal: {
      cols: terminal.cols,
      rows: terminal.rows,
    },
  });
  shellProcess.output.pipeTo(
    new WritableStream({
      write(data) {
        terminal.write(data);
      },
    })
  );

  const input = shellProcess.input.getWriter();

  terminal.onData((data) => {
    input.write(data);
  });

  return shellProcess;
}
```

----------------------------------------

TITLE: Watching a file using watch - JavaScript
DESCRIPTION: Demonstrates how to watch a file for changes using the `watch` method.  It logs the event type to the console.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_23

LANGUAGE: js
CODE:
```
webcontainerInstance.fs.watch('/src/main.js', (event) => {
    console.log(`action: ${event}`);
});
```

----------------------------------------

TITLE: Adding StackBlitz URL Patterns to Chrome Cookie Exceptions
DESCRIPTION: This code snippet demonstrates the URL patterns that need to be added to the 'Sites that can always use cookies' section in Chrome's cookie settings to allow StackBlitz projects to use Service Workers. This is necessary if the 'Block Third Party Cookies' option is enabled in Chrome.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/browser-config.md#_snippet_0

LANGUAGE: HTML
CODE:
```
https://[*.]stackblitz.io
https://[*.]webcontainer.io
```

----------------------------------------

TITLE: Configuring Cross-Origin Isolation with YAML
DESCRIPTION: This YAML configuration snippet sets the necessary headers for cross-origin isolation, which is required by WebContainers. These headers ensure that `SharedArrayBuffer` can be used.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/quickstart.md#_snippet_2

LANGUAGE: yaml
CODE:
```
Cross-Origin-Embedder-Policy: require-corp
Cross-Origin-Opener-Policy: same-origin
```

----------------------------------------

TITLE: Loading a Single File into WebContainer - JavaScript
DESCRIPTION: Loads a single file (package.json) into the WebContainer's file system using the `mount` method.  The code demonstrates the FileSystemTree structure required for mounting files and then reads the file content using `readFile`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_3

LANGUAGE: javascript
CODE:
```
/** @type {import('@webcontainer/api').FileSystemTree} */

const files = {
  'package.json': {
    file: {
      contents: `
        {
          "name": "vite-starter",
          "private": true,
          "version": "0.0.0",
          "type": "module",
          "scripts": {
            "dev": "vite",
            "build": "vite build",
            "preview": "vite preview"
          },
          "devDependencies": {
            "vite": "^4.0.4"
          }
        }`,
    },
  },
};

await webcontainerInstance.mount(files);

```

LANGUAGE: javascript
CODE:
```
const file = await webcontainerInstance.fs.readFile('/package.json');

```

----------------------------------------

TITLE: Creating a WebContainer instance in JavaScript
DESCRIPTION: This JavaScript code snippet imports the `WebContainer` class and creates a single instance using the `boot` method. The `boot` method can only be called once and only one WebContainer instance can be created.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/quickstart.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import { WebContainer } from '@webcontainer/api';

// Call only once
const webcontainerInstance = await WebContainer.boot();
```

----------------------------------------

TITLE: Reading a Directory in WebContainer - JavaScript
DESCRIPTION: Explains how to read the contents of a directory within a WebContainer using the `readdir` method. It demonstrates retrieving an array of filenames and directory names, and mentions options for retrieving `Dirent` objects with file type information or specifying the encoding.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_10

LANGUAGE: javascript
CODE:
```
const files = await webcontainerInstance.fs.readdir('/src');
//    ^ is an array of strings

```

LANGUAGE: javascript
CODE:
```
const files = await webcontainerInstance.fs.readdir('/src', {
  withFileTypes: true,
});

```

LANGUAGE: javascript
CODE:
```
const files = await webcontainerInstance.fs.readdir('/src', { encoding: 'buffer' });
//    ^ it is an array of UInt8Array

```

----------------------------------------

TITLE: Representing package.json file in WebContainers JavaScript
DESCRIPTION: Demonstrates how a `package.json` file is represented as a nested JavaScript object within WebContainers. The object includes the file path, the 'file' key, and the 'contents' key holding the JSON content as a string.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_0

LANGUAGE: javascript
CODE:
```
const files = {
  // This is a file - provide its path as a key:
  'package.json': {
    // Because it's a file, add the "file" key
    file: {
      // Now add its contents
      contents: `
        {
          "name": "vite-starter",
          "private": true,
         // ...
          },
          "devDependencies": {
            "vite": "^4.0.4"
          }
        }`,
    },
  },
};

```

----------------------------------------

TITLE: Importing Xterm.js
DESCRIPTION: This line imports the `Terminal` class from the `@xterm/xterm` package. This is necessary to create an instance of the terminal.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_3

LANGUAGE: js
CODE:
```
import { Terminal } from '@xterm/xterm'
```

----------------------------------------

TITLE: Creating a Recursive Directory using fs.mkdir (JavaScript)
DESCRIPTION: This snippet demonstrates how to create a nested directory using the `recursive` option with the `fs.mkdir` method. This allows creating multiple levels of directories in a single operation. It requires a WebContainer instance to be initialized.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_15

LANGUAGE: javascript
CODE:
```
await webcontainerInstance.fs.mkdir('this/is/my/nested/folder', { recursive: true });
```

----------------------------------------

TITLE: Reading a File in WebContainer - JavaScript
DESCRIPTION: Illustrates how to read the contents of a file within a WebContainer using the `readFile` method of the `fs` property. It shows how to retrieve the file as a `UInt8Array` by default, or as a string if a specific encoding (e.g., 'utf-8') is provided.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_9

LANGUAGE: javascript
CODE:
```
const file = await webcontainerInstance.fs.readFile('/package.json');
//    ^ it is a UInt8Array

```

LANGUAGE: javascript
CODE:
```
const file = await webcontainerInstance.fs.readFile('/package.json', 'utf-8');
//    ^ it is a string

```

----------------------------------------

TITLE: Representing a directory with file in WebContainers JavaScript
DESCRIPTION: Demonstrates representing a directory with a file inside it using nested JavaScript objects in WebContainers. It showcases the structure for defining a directory and including a file (e.g., 'main.js') with its content.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_2

LANGUAGE: javascript
CODE:
```
const files = {
  // This is a directory - provide its name as a key
  src: {
    // Because it's a directory, add the "directory" key
    directory: {
      // This is a file - provide its path as a key:
      'main.js': {
        // Because it's a file, add the "file" key
        file: {
          contents: `
            console.log('Hello from WebContainers!')
          `,
        },
      },
    },
  },
};

```

----------------------------------------

TITLE: Setting up Vue component with TypeScript
DESCRIPTION: This code snippet demonstrates how to set up a Vue component using TypeScript. It imports necessary components and uses the `<script setup lang="ts">` syntax to define the component's logic.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/clack.md#_snippet_0

LANGUAGE: Vue
CODE:
```
<script setup lang="ts">
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
</script>
```

----------------------------------------

TITLE: Previewing the Dev Server in an Iframe using JavaScript
DESCRIPTION: This JavaScript code snippet sets the source of an iframe element to the URL provided by the `server-ready` event. This allows you to preview the running dev server within the iframe.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/quickstart.md#_snippet_5

LANGUAGE: javascript
CODE:
```
webcontainerInstance.on('server-ready', (port, url) => (iframeEl.src = url));
```

----------------------------------------

TITLE: Defining ErrorListener Type - Typescript
DESCRIPTION: Defines the `ErrorListener` type, which is a function that's called when an internal error is triggered within the WebContainer. It receives an error object containing a message.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_3

LANGUAGE: typescript
CODE:
```
(error: { message: string }): void
```

----------------------------------------

TITLE: Install @xterm/addon-fit (Bash)
DESCRIPTION: This command installs the `@xterm/addon-fit` plugin using npm.  This plugin provides the functionality to automatically adjust the terminal's columns and rows based on the size of its containing element.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_5

LANGUAGE: Bash
CODE:
```
npm install @xterm/addon-fit
```

----------------------------------------

TITLE: Creating a Directory using fs.mkdir (JavaScript)
DESCRIPTION: This snippet demonstrates how to create a directory using the `fs.mkdir` method. If the directory already exists, it will throw an error. It requires a WebContainer instance to be initialized.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/working-with-the-file-system.md#_snippet_14

LANGUAGE: javascript
CODE:
```
await webcontainerInstance.fs.mkdir('src');
```

----------------------------------------

TITLE: Installing Xterm.js using npm
DESCRIPTION: This command installs the `@xterm/xterm` package, which is a terminal frontend component, using npm. It's a prerequisite for adding a terminal to the web application.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm install @xterm/xterm
```

----------------------------------------

TITLE: Defining BufferEncoding Type Alias in TypeScript
DESCRIPTION: This TypeScript type alias `BufferEncoding` defines the allowed string values for buffer encoding formats. It encompasses various encoding schemes like 'ascii', 'utf8', 'base64', 'latin1', and 'hex'. This type is used to ensure type safety when specifying the encoding format for buffer operations.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_33

LANGUAGE: typescript
CODE:
```
type BufferEncoding =
  | 'ascii'
  | 'utf8'
  | 'utf-8'
  | 'utf16le'
  | 'ucs2'
  | 'ucs-2'
  | 'base64'
  | 'base64url'
  | 'latin1'
  | 'binary'
  | 'hex';
```

----------------------------------------

TITLE: Referencing terminal div element
DESCRIPTION: This code snippet creates a JavaScript variable `terminalEl` that references the terminal `div` element in the DOM. This reference is used later to attach the Xterm.js terminal to the `div`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_2

LANGUAGE: js
CODE:
```
/** @type {HTMLTextAreaElement | null} */
const terminalEl = document.querySelector('.terminal');
```

----------------------------------------

TITLE: Importing Xterm.js CSS
DESCRIPTION: This line imports the CSS styles for Xterm.js, which are necessary to style the terminal. The styles provide the default look and feel for the terminal.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_5

LANGUAGE: js
CODE:
```
import '@xterm/xterm/css/xterm.css';
```

----------------------------------------

TITLE: Defining Preview Script Options Interface in TypeScript
DESCRIPTION: The `PreviewScriptOptions` interface in TypeScript allows configuring attributes for scripts injected into previews. It includes options for setting the `type` attribute ('module' or 'importmap'), and boolean flags to enable the `defer` and `async` attributes. These options control how the script is loaded and executed within the preview environment.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_32

LANGUAGE: typescript
CODE:
```
export interface PreviewScriptOptions {
  type?: 'module' | 'importmap';
  defer?: boolean;
  async?: boolean;
}
```

----------------------------------------

TITLE: Adding terminal div element
DESCRIPTION: This code snippet adds a new `div` element with the class 'terminal' to the HTML structure of the application. This `div` will serve as the container for the Xterm.js terminal.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/6-connect-a-terminal.md#_snippet_1

LANGUAGE: js
CODE:
```
document.querySelector('#app').innerHTML = `
  <div class="container">
    <div class="editor">
      <textarea>I am a textarea</textarea>
    </div>
    <div class="preview">
      <iframe src="loading.html"></iframe>
    </div>
  </div>
  <div class="terminal"></div>
`;
```

----------------------------------------

TITLE: Defining FileNode interface - TypeScript
DESCRIPTION: Defines the `FileNode` interface, representing a file with contents in the `FileSystemTree`. The contents can be either a string or a `Uint8Array`.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_27

LANGUAGE: ts
CODE:
```
interface FileNode {
  file: {
    contents: string | Uint8Array;
  };
}
```

----------------------------------------

TITLE: Initializing Vite Project
DESCRIPTION: This command initializes a new Vite project using the vanilla JavaScript template. It sets up the basic project structure and configuration files.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_0

LANGUAGE: Bash
CODE:
```
npm create vite webcontainers-express-app --template vanilla
```

----------------------------------------

TITLE: Defining SymlinkNode interface - TypeScript
DESCRIPTION: Defines the `SymlinkNode` interface, representing a symbolic link in the `FileSystemTree`.  The `symlink` property specifies the target path of the symbolic link.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_28

LANGUAGE: ts
CODE:
```
interface SymlinkNode {
  file: {
    symlink: string;
  };
}
```

----------------------------------------

TITLE: Auth Init Options Interface - TypeScript
DESCRIPTION: This snippet defines the `AuthInitOptions` interface, which is used as the options parameter for the `auth.init` function. It includes properties for specifying the StackBlitz origin, client ID, and OAuth scope.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_10

LANGUAGE: ts
CODE:
```
interface AuthInitOptions {
  /**
   * StackBlitz' origin.
   *
   * @default https://stackblitz.com
   */
  editorOrigin?: string;

  /**
   * The client id for this OAuth application.
   */
  clientId: string;

  /**
   * OAuth scope. The value can be found under your `Teams Settings` > `API`.
   *
   * @see https://www.rfc-editor.org/rfc/rfc6749#section-3.3
   */
  scope: string;
}
```

----------------------------------------

TITLE: Importing Vue components in TypeScript
DESCRIPTION: This code snippet imports two Vue components, `PageHeading` and `Screenshot`, into the current TypeScript file. These components are used to render the page heading and a screenshot of the re:tune application, respectively. The components are imported from the specified paths within the project's theme directory. This is required to use these components within the Vue template.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/retune.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
```

----------------------------------------

TITLE: Updating style.css - Basic Styling
DESCRIPTION: This CSS snippet defines the styles for the application, including the container, textarea, and iframe. It sets up the layout and appearance of the editor and preview areas.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_5

LANGUAGE: CSS
CODE:
```
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  height: 100vh;
}

.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  height: 100%;
  width: 100%;
}

textarea {
  width: 100%;
  height: 100%;
  resize: none;
  border-radius: 0.5rem;
  background: black;
  color: white;
  padding: 0.5rem 1rem;
}

iframe {
  height: 100%;
  width: 100%;
  border-radius: 0.5rem;
}
```

----------------------------------------

TITLE: Auth Failed Error Interface - TypeScript
DESCRIPTION: This snippet defines the `AuthFailedError` interface, which represents an authentication failure. It includes a status indicating authentication failure, a short error description, and a detailed error description.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_11

LANGUAGE: ts
CODE:
```
interface AuthFailedError {
  status: 'auth-failed';

  /**
   * A short description of the error.
   */
  error: string;

  /**
   * A detailed description of the error.
   */
  description: string;
}
```

----------------------------------------

TITLE: Setting up a Vue component for the Angular tutorial page
DESCRIPTION: This code snippet sets up a Vue component for the Angular tutorial page, importing necessary components like `PageHeading` and `Screenshot`. It uses TypeScript for type safety and defines the component's script section.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/angular-tutorial.md#_snippet_0

LANGUAGE: typescript
CODE:
```
<script setup lang="ts">
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
</script>
```

----------------------------------------

TITLE: Querying Textarea and Iframe Elements
DESCRIPTION: This JavaScript code retrieves references to the `iframe` and `textarea` elements using `document.querySelector`. These references are stored in `iframeEl` and `textareaEl` variables for later use.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
/** @type {HTMLIFrameElement | null} */
const iframeEl = document.querySelector('iframe');

/** @type {HTMLTextAreaElement | null} */
const textareaEl = document.querySelector('textarea');
```

----------------------------------------

TITLE: Vue Component Setup
DESCRIPTION: This code snippet sets up a Vue component using the `<script setup lang="ts">` syntax. It imports `PageHeading` and `Screenshot` components for use in the page.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/schachnovelle.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
<script setup lang="ts">
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
</script>
```

----------------------------------------

TITLE: Deleting a file using rm - JavaScript
DESCRIPTION: Example showing how to delete a file using the `rm` method. It takes the file path as an argument.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_19

LANGUAGE: js
CODE:
```
await webcontainerInstance.fs.rm('/src/main.js');
```

----------------------------------------

TITLE: Importing Vue Components and Data
DESCRIPTION: This TypeScript code snippet imports a Vue component named `Home` and a data structure `footerSections` from specified modules. The `Home` component is likely used to render the main content of the page, while `footerSections` probably contains data for the footer.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/index.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import Home from '@theme/components/Home.vue';

import { footerSections } from '@theme/data/links';
```

----------------------------------------

TITLE: SvelteKit Component Imports (Vue)
DESCRIPTION: This snippet imports necessary Vue components for rendering the SvelteKit community project documentation page. It includes components for page headings, screenshots, video links, and attribution links, enhancing the page's visual and informational structure.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/community-projects/sveltekit.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import PageHeading from '@theme/components/Helpers/CommunityProjectPageHeading.vue';
import Screenshot from '@theme/components/Helpers/Screenshot.vue';
import VideoLink from '@theme/components/Helpers/VideoLink.vue';
import AttributionLinks from '@theme/components/Helpers/AttributionLinks.vue';
import { people } from '@theme/data/people';
const { RICH_HARRIS } = people;
```

----------------------------------------

TITLE: Update loading.html (HTML)
DESCRIPTION: This snippet updates the `loading.html` file to display the message "Use the terminal to run a command!". This informs the user that they can now interact with the terminal.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/7-add-interactivity.md#_snippet_1

LANGUAGE: HTML
CODE:
```
Use the terminal to run a command!
```

----------------------------------------

TITLE: Defining Options interface for mkdir - TypeScript
DESCRIPTION: Defines the `Options` interface for the `mkdir` method. The `recursive` option, when set to true, enables the creation of any missing folders in the specified path.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_13

LANGUAGE: ts
CODE:
```
interface Options {
  recursive?: boolean;
}
```

----------------------------------------

TITLE: Creating loading.html - Initial Content
DESCRIPTION: This HTML snippet defines the content of the `loading.html` file, displaying "Installing dependencies..." which will be shown while the WebContainer is getting ready.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/tutorial/1-build-your-first-webcontainer-app.md#_snippet_3

LANGUAGE: HTML
CODE:
```
Installing dependencies...
```

----------------------------------------

TITLE: Running Commands in a Specific Directory with Turbo (JavaScript)
DESCRIPTION: This snippet shows how to use the `turbo` command to execute commands in a specific directory within a WebContainer. It uses the `--cwd` flag to specify the working directory for the command. This is helpful for managing multiple projects within the same WebContainer environment.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/guides/troubleshooting.md#_snippet_1

LANGUAGE: javascript
CODE:
```
webcontainer.spawn('turbo', ['--cwd', `~/projects/${id}`, 'exec', 'astro', 'dev']);
```

----------------------------------------

TITLE: Spawning a Process without Arguments - Javascript
DESCRIPTION: Illustrates how to spawn a process within the WebContainer without command-line arguments. In this example, it executes the `yarn` command directly.
SOURCE: https://github.com/stackblitz/webcontainer-docs/blob/main/docs/api.md#_snippet_7

LANGUAGE: javascript
CODE:
```
const install = await webcontainerInstance.spawn('yarn');
```