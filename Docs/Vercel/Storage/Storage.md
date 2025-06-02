TITLE: Basic KV Operations with JavaScript
DESCRIPTION: Demonstrates common operations using the default `kv` client for various Redis data types including strings, sorted sets, lists, hashes, sets, and scanning keys.
SOURCE: https://github.com/vercel/storage/blob/main/packages/kv/README.md#_snippet_1

LANGUAGE: js
CODE:
```
import { kv } from '@vercel/kv';

// string
await kv.set('key', 'value');
let data = await kv.get('key');
console.log(data); // 'value'

await kv.set('key2', 'value2', { ex: 1 });

// sorted set
await kv.zadd(
  'scores',
  { score: 1, member: 'team1' },
  { score: 2, member: 'team2' },
);
data = await kv.zrange('scores', 0, 0);
console.log(data); // [ 'team1' ]

// list
await kv.lpush('elements', 'magnesium');
data = await kv.lrange('elements', 0, 100);
console.log(data); // [ 'magnesium' ]

// hash
await kv.hset('people', { name: 'joe' });
data = await kv.hget('people', 'name');
console.log(data); // 'joe'

// sets
await kv.sadd('animals', 'cat');
data = await kv.spop('animals', 1);
console.log(data); // [ 'cat' ]

// scan for keys
for await (const key of kv.scanIterator()) {
  console.log(key);
}
```

----------------------------------------

TITLE: Executing a one-shot query with default sql client
DESCRIPTION: Demonstrates executing a single SQL query using the default `sql` client import. It shows how to select data with a parameterized query using template literals.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
// no-config
import { sql } from '@vercel/postgres';

const id = 100;

// A one-shot query
const { rows } = await sql`SELECT * FROM users WHERE id = ${userId};`;
```

----------------------------------------

TITLE: Create Kysely Instance and Query (TypeScript)
DESCRIPTION: Demonstrates creating a Kysely database instance using `createKysely` from `@vercel/postgres-kysely`. It shows examples of inserting data into the 'pet' table and selecting data with a join from 'person' and 'pet'.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres-kysely/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { createKysely } from '@vercel/postgres-kysely';

interface Database {
  person: PersonTable;
  pet: PetTable;
  movie: MovieTable;
}

const db = createKysely<Database>();

await db
  .insertInto('pet')
  .values({ name: 'Catto', species: 'cat', owner_id: id })
  .execute();

const person = await db
  .selectFrom('person')
  .innerJoin('pet', 'pet.owner_id', 'person.id')
  .select(['first_name', 'pet.name as pet_name'])
  .where('person.id', '=', id)
  .executeTakeFirst();
```

----------------------------------------

TITLE: Use @vercel/edge-config in Vercel Edge Function (JavaScript)
DESCRIPTION: This example shows how to integrate the @vercel/edge-config client within a Vercel Edge Function (compatible with Next.js or other frameworks). It imports the `get` function, defines an asynchronous request handler, reads a value, and returns an HTTP response. The `config` export sets the runtime to 'edge'.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_6

LANGUAGE: js
CODE:
```
// Next.js (pages/api/edge.js) (npm i next@canary)
// Other frameworks (api/edge.js) (npm i -g vercel@canary)

import { get } from '@vercel/edge-config';

export default async (req) => {
  const value = await get("someKey")
  return new Response(`someKey contains value "${value})"`);
};

export const config = { runtime: 'edge' };
```

----------------------------------------

TITLE: Importing default sql client in TypeScript
DESCRIPTION: Imports the default `sql` client from `@vercel/postgres` for basic usage without custom configuration. This client is a modified Pool object ready for immediate use.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
// Don't need any custom config?:
import { sql } from '@vercel/postgres';
// `sql` is already set up and ready to go; no further action needed
```

----------------------------------------

TITLE: Read Single Value from Default Edge Config (JavaScript)
DESCRIPTION: This snippet demonstrates how to read a single value from the default Edge Config using the `get` function. It returns the value if the key exists, `undefined` if not, and throws errors on invalid tokens, deleted configs, or network issues. Requires the connection string in `process.env.EDGE_CONFIG`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_1

LANGUAGE: js
CODE:
```
import { get } from '@vercel/edge-config';
await get('someKey');
```

----------------------------------------

TITLE: Uploading Files with Multipart Option (TypeScript)
DESCRIPTION: Shows how to enable multipart uploads using the `multipart: true` option in both `put` and `upload` calls for Vercel Blob, improving reliability for larger files. This option is available for both server and client uploads.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
const blob = await put('file.png', file, {
  access: 'public',
  multipart: true, // `false` by default
});

// and:
const blob = await upload('file.png', file, {
  access: 'public',
  handleUploadUrl: '/api/upload',
  multipart: true,
});
```

----------------------------------------

TITLE: Executing multiple queries using a dedicated client
DESCRIPTION: Shows how to obtain a dedicated client from the default `sql` pool using `sql.connect()`, execute multiple queries on the same connection for improved performance, and release the client using `client.release()` afterwards. Important: Do not share clients across requests.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
// Multiple queries on the same connection (improves performance)
// warning: Do not share clients across requests and be sure to release them!
const client = await sql.connect();
const { rows } = await client.sql`SELECT * FROM users WHERE id = ${userId};`;
await client.sql`UPDATE users SET status = 'satisfied' WHERE id = ${userId};`;
client.release();
```

----------------------------------------

TITLE: Uploading Files with Multipart in Vercel Blob (TypeScript)
DESCRIPTION: Shows how to use the new `multipart: true` option when uploading files with `put` or `upload` in Vercel Blob. This option enables reliable uploads for medium and large files by splitting them into parts and retrying failures.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
const blob = await put('file.png', file, {
  access: 'public',
  multipart: true, // `false` by default
});

// and:
const blob = await upload('file.png', file, {
  access: 'public',
  handleUploadUrl: '/api/upload',
  multipart: true,
});
```

----------------------------------------

TITLE: Track Upload Progress with Vercel Blob (JS/TS)
DESCRIPTION: Demonstrates how to use the `onUploadProgress` option when uploading a file with `@vercel/blob.put` to monitor the upload progress, including loaded bytes, total bytes, and percentage.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const blob = await put('big-file.pdf', file, {
  access: 'public',
  onUploadProgress(event) {
    console.log(event.loaded, event.total, event.percentage);
  }
});
```

----------------------------------------

TITLE: Tracking Upload Progress with onUploadProgress in Vercel Blob (JS)
DESCRIPTION: This example shows how to use the `onUploadProgress` option with the `put` or `upload` methods in Vercel Blob. The callback function receives an event object containing `loaded`, `total`, and `percentage` properties, allowing you to monitor the upload progress in real-time.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const blob = await put('big-file.pdf', file, {
  access: 'public',
  onUploadProgress(event) {
    console.log(event.loaded, event.total, event.percentage);
  }
});
```

----------------------------------------

TITLE: Read Multiple Items from Default Edge Config (JavaScript)
DESCRIPTION: This snippet shows how to read specific items in a batch from the default Edge Config by passing an array of keys to the `getAll` function. It returns an object containing only the requested key-value pairs and throws errors on invalid configurations or network issues. Requires the connection string in `process.env.EDGE_CONFIG`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_4

LANGUAGE: js
CODE:
```
import { getAll } from '@vercel/edge-config';
await getAll(['keyA', 'keyB']);
```

----------------------------------------

TITLE: Install Vercel Blob SDK with npm
DESCRIPTION: Use this command to install the @vercel/blob JavaScript API client as a project dependency using the npm package manager.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/README.md#_snippet_0

LANGUAGE: shell
CODE:
```
npm install @vercel/blob
```

----------------------------------------

TITLE: Installing @vercel/postgres with pnpm
DESCRIPTION: Command to install the `@vercel/postgres` package using the pnpm package manager.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
pnpm install @vercel/postgres
```

----------------------------------------

TITLE: Install @vercel/edge-config Package (Shell)
DESCRIPTION: This command installs the @vercel/edge-config package using npm. It is the first step required to use the Edge Config client in your project.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
npm install @vercel/edge-config
```

----------------------------------------

TITLE: Getting Postgres connection URLs from environment variables
DESCRIPTION: Shows how to use the `postgresConnectionString` function to retrieve the pooled or direct connection string based on standard Vercel environment variables (`POSTGRES_URL` or `POSTGRES_URL_NON_POOLING`).
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { postgresConnectionString } from '@vercel/postgres';

const pooledConnectionString = postgresConnectionString('pool');
const directConnectionString = postgresConnectionString('direct');
```

----------------------------------------

TITLE: Performing Multipart Upload with Uploader Helper - Vercel Blob SDK - TypeScript
DESCRIPTION: Illustrates the use of the `createMultipartUploader` helper function for multipart uploads. This helper simplifies the process by internally managing the uploadId and key, offering convenient `uploadPart` and `complete` methods. It is suitable when consistent data (uploadId, key) is used across multiple parts.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
const uploader = await vercelBlob.createMultipartUploader('big-file.txt', {
  access: 'public',
});

const part1 = await uploader.uploadPart(1, createReadStream(fullPath));

const part2 = await uploader.uploadPart(2, createReadStream(fullPath));

const blob = await uploader.complete([part1, part2]);
```

----------------------------------------

TITLE: Install @vercel/kv Package
DESCRIPTION: Installs the @vercel/kv package using npm, making it available for use in your project.
SOURCE: https://github.com/vercel/storage/blob/main/packages/kv/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
npm install @vercel/kv
```

----------------------------------------

TITLE: Using Multipart Uploader Helper - Vercel Blob - TypeScript
DESCRIPTION: Shows how to use the `createMultipartUploader` helper function for multipart uploads. This helper manages the upload ID and key internally, simplifying the process with `uploadPart` and `complete` methods. Requires a readable stream for each part.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_3

LANGUAGE: typescript
CODE:
```
const uploader = await vercelBlob.createMultipartUploader('big-file.txt', {
  access: 'public',
});

const part1 = await uploader.uploadPart(1, createReadStream(fullPath));

const part2 = await uploader.uploadPart(2, createReadStream(fullPath));

const blob = await uploader.complete([part1, part2]);
```

----------------------------------------

TITLE: Install Kysely Peer Dependency (Bash)
DESCRIPTION: Installs Kysely as a project dependency using pnpm. Kysely is a required peer dependency for `@vercel/postgres-kysely`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres-kysely/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
pnpm i kysely
```

----------------------------------------

TITLE: Overwriting Blobs with allowOverwrite in Vercel Blob (JS)
DESCRIPTION: This snippet demonstrates the new behavior in Vercel Blob v1.0.0 regarding overwriting existing blobs. By default, calling `put` with the same pathname will now throw an error. To explicitly allow overwriting, you must pass the `allowOverwrite: true` option in the configuration object.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
await put('file.png', file, { access: 'public' });

await put('file.png', file, { access: 'public' }); // This will throw

put('file.png', file, { access: 'public', allowOverwrite: true }); // This will work
```

----------------------------------------

TITLE: Performing Multipart Upload with Individual Methods - Vercel Blob SDK - TypeScript
DESCRIPTION: Demonstrates how to perform a multipart upload using the individual functions: `createMultipartUpload`, `uploadPart`, and `completeMultipartUpload`. This approach provides fine-grained control over each step of the upload process. Requires specifying the key, uploadId, and partNumber for each part.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
const { key, uploadId } = await vercelBlob.createMultipartUpload(
  'big-file.txt',
  { access: 'public' },
);

const part1 = await vercelBlob.uploadPart(fullPath, 'first part', {
  access: 'public',
  key,
  uploadId,
  partNumber: 1,
});

const part2 = await vercelBlob.uploadPart(fullPath, 'second part', {
  access: 'public',
  key,
  uploadId,
  partNumber: 2,
});

const blob = await vercelBlob.completeMultipartUpload(
  fullPath,
  [part1, part2],
  {
    access: 'public',
    key,
    uploadId,
  },
);
```

----------------------------------------

TITLE: Performing Multipart Upload - Vercel Blob - TypeScript
DESCRIPTION: Demonstrates how to perform a multipart file upload using the individual `createMultipartUpload`, `uploadPart`, and `completeMultipartUpload` functions from the @vercel/blob SDK. Requires the full path to the file and the SDK instance. Each part must be uploaded sequentially with correct part numbers.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_2

LANGUAGE: typescript
CODE:
```
const { key, uploadId } = await vercelBlob.createMultipartUpload(
  'big-file.txt',
  { access: 'public' },
);

const part1 = await vercelBlob.uploadPart(fullPath, 'first part', {
  access: 'public',
  key,
  uploadId,
  partNumber: 1,
});

const part2 = await vercelBlob.uploadPart(fullPath, 'second part', {
  access: 'public',
  key,
  uploadId,
  partNumber: 2,
});

const blob = await vercelBlob.completeMultipartUpload(
  fullPath,
  [part1, part2],
  {
    access: 'public',
    key,
    uploadId,
  },
);
```

----------------------------------------

TITLE: Define Kysely Database Schema (TypeScript)
DESCRIPTION: Defines TypeScript interfaces that map to the database schema, providing type safety for Kysely queries. It demonstrates using `Generated` for auto-incrementing columns and `ColumnType` for specific type mappings.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres-kysely/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Generated, ColumnType } from 'kysely';

interface PersonTable {
  // Columns that are generated by the database should be marked
  // using the `Generated` type. This way they are automatically
  // made optional in inserts and updates.
  id: Generated<number>;

  first_name: string;
  gender: 'male' | 'female' | 'other';

  // If the column is nullable in the database, make its type nullable.
  // Don't use optional properties. Optionality is always determined
  // automatically by Kysely.
  last_name: string | null;

  // You can specify a different type for each operation (select, insert and
  // update) using the `ColumnType<SelectType, InsertType, UpdateType>`
  // wrapper. Here we define a column `modified_at` that is selected as
  // a `Date`, can optionally be provided as a `string` in inserts and
  // can never be updated:
  modified_at: ColumnType<Date, string | undefined, never>;
}

interface PetTable {
  id: Generated<number>;
  name: string;
  owner_id: number;
  species: 'dog' | 'cat';
}

interface MovieTable {
  id: Generated<string>;
  stars: number;
}

// Keys of this interface are table names.
interface Database {
  person: PersonTable;
  pet: PetTable;
  movie: MovieTable;
}
```

----------------------------------------

TITLE: Cancel Blob Upload with AbortController (TypeScript)
DESCRIPTION: Shows how to use an `AbortController` and its `signal` to cancel an ongoing file upload operation using `@vercel/blob.put`. The example sets up a timeout to trigger the abort.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const abortController = new AbortController();

vercelBlob
  .put('canceled.txt', 'test', {
    access: 'public',
    abortSignal: abortController.signal,
  })
  .then((blob) => {
    console.log('Blob created:', blob);
  });

setTimeout(function () {
  // Abort the upload
  abortController.abort();
}, 100);
```

----------------------------------------

TITLE: Check Key Existence in Default Edge Config (JavaScript)
DESCRIPTION: This snippet shows how to check if a specific key exists in the default Edge Config using the `has` function. It returns `true` if the key exists, `false` otherwise, and throws errors for invalid configurations or network problems. Requires the connection string in `process.env.EDGE_CONFIG`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_2

LANGUAGE: js
CODE:
```
import { has } from '@vercel/edge-config';
await has('someKey');
```

----------------------------------------

TITLE: Canceling Blob Operations with AbortSignal in Vercel Blob (TS)
DESCRIPTION: This snippet demonstrates how to use the `abortSignal` option with Vercel Blob methods to cancel ongoing requests. An `AbortController` is created, its signal is passed to the `put` method, and the `abort()` method is called on the controller to cancel the operation after a delay.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
const abortController = new AbortController();

vercelBlob
  .put('canceled.txt', 'test', {
    access: 'public',
    abortSignal: abortController.signal,
  })
  .then((blob) => {
    console.log('Blob created:', blob);
  });

setTimeout(function () {
  // Abort the upload
  abortController.abort();
}, 100);
```

----------------------------------------

TITLE: Importing and creating a Pool in TypeScript
DESCRIPTION: Imports the `createPool` function to create a connection pool with custom configuration options. This is useful for managing multiple connections efficiently.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
// Need to customize your config?:
import { createPool } from '@vercel/postgres';
const pool = createPool({
  /* config */
});
```

----------------------------------------

TITLE: Read All Items from Default Edge Config (JavaScript)
DESCRIPTION: This snippet demonstrates how to retrieve all items stored in the default Edge Config using the `getAll` function without arguments. It returns an object containing all key-value pairs and throws errors on invalid configurations or network issues. Requires the connection string in `process.env.EDGE_CONFIG`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_3

LANGUAGE: js
CODE:
```
import { getAll } from '@vercel/edge-config';
await getAll();
```

----------------------------------------

TITLE: Uploading Node.js Stream with Multipart in Vercel Blob (TypeScript)
DESCRIPTION: Illustrates uploading a file using a Node.js `createReadStream` with the `multipart: true` option. Vercel Blob will gradually read and upload the stream without excessive memory usage.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { createReadStream } from 'node:fs';

const blob = await vercelBlob.put(
  'elon.mp4',
  // this works 👍, it will gradually read the file from the system and upload it
  createReadStream('/users/Elon/me.mp4'),
  { access: 'public', multipart: true },
);
```

----------------------------------------

TITLE: Cloning Edge Config Value for Modification (Post-1.0.0)
DESCRIPTION: This snippet shows the recommended way to modify a value retrieved from Edge Config after version 1.0.0. The `clone` function is used to create a modifiable copy of the read-only data.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/CHANGELOG.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { get, clone } from '@vercel/edge-config';

const myArray = await get('listOfAllowedIPs');
const myArrayClone = clone(myArray); // Clones the data to make it modifiable
myArrayClone.push('127.0.0.1'); // The `push` operation will work now
```

----------------------------------------

TITLE: Uploading Node.js Stream with Multipart (TypeScript)
DESCRIPTION: Illustrates uploading a file from the local filesystem using a Node.js `createReadStream` and the `multipart: true` option with `vercelBlob.put`. This allows the file to be read and uploaded gradually without excessive memory usage.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { createReadStream } from 'node:fs';

const blob = await vercelBlob.put(
  'elon.mp4',
  // this works 👍, it will gradually read the file from the system and upload it
  createReadStream('/users/Elon/me.mp4'),
  { access: 'public', multipart: true },
);
```

----------------------------------------

TITLE: Querying using a Pool with a connection string
DESCRIPTION: Demonstrates creating a connection pool using `createPool` with a specified connection string obtained from environment variables and executing a query using the pool's `sql` method.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { createPool } from '@vercel/postgres';

const pool = createPool({
  connectionString: process.env.SOME_POSTGRES_CONNECTION_STRING,
});

const likes = 100;
const { rows, fields } =
  await pool.sql`SELECT * FROM posts WHERE likes > ${likes};`;
```

----------------------------------------

TITLE: Uploading ReadableStream with Multipart (TypeScript)
DESCRIPTION: Shows how to upload a file directly from a network response body (a ReadableStream) using `vercelBlob.put` and the `multipart: true` option. This enables gradual upload from a URL without loading the entire file into memory.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_7

LANGUAGE: TypeScript
CODE:
```
const response = await fetch(
  'https://example-files.online-convert.com/video/mp4/example_big.mp4',
);

const blob = await vercelBlob.put(
  'example_big.mp4',
  // this works too 👍, it will gradually read the file from internet and upload it
  response.body,
  { access: 'public', multipart: true },
);
```

----------------------------------------

TITLE: Using Edge Config client with SvelteKit private environment variables
DESCRIPTION: Demonstrates an alternative method for providing the Edge Config connection string explicitly in SvelteKit by importing it from `$env/static/private` instead of relying on `process.env`, which is not exposed by default.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_11

LANGUAGE: diff
CODE:
```
import { createClient } from '@vercel/edge-config';
+ import { EDGE_CONFIG } from '$env/static/private';

- const edgeConfig = createClient(process.env.ANOTHER_EDGE_CONFIG);
+ const edgeConfig = createClient(EDGE_CONFIG);
await edgeConfig.get('someKey');
```

----------------------------------------

TITLE: Creating an Empty Folder (TypeScript)
DESCRIPTION: Demonstrates how to create an empty folder in Vercel Blob storage by using `vercelBlob.put` with a pathname ending in a trailing slash. Setting `addRandomSuffix` to `false` is typically used for predictable folder names.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/CHANGELOG.md#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const blob = await vercelBlob.put('folder/', {
  access: 'public',
  addRandomSuffix: false,
});
```

----------------------------------------

TITLE: Create KV Client with Custom Environment Variables
DESCRIPTION: Shows how to create a KV client instance using `createClient` and explicitly provide the connection URL and token, overriding the default behavior of reading from `process.env`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/kv/README.md#_snippet_2

LANGUAGE: js
CODE:
```
import { createClient } from '@vercel/kv';

const kv = createClient({
  url: 'https://<hostname>.redis.vercel-storage.com',
  token: '<token>',
});

await kv.set('key', 'value');
```

----------------------------------------

TITLE: Provide Blob Token Explicitly in SvelteKit
DESCRIPTION: This code diff illustrates how to access a private environment variable (BLOB_TOKEN) in SvelteKit using $env/static/private and pass it explicitly to the @vercel/blob SDK function, bypassing reliance on process.env.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/README.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import { put } from '@vercel/blob';
+ import { BLOB_TOKEN } from '$env/static/private';

const blob = await head("filepath", {
-  token: '<token>',
+  token: BLOB_TOKEN,
});
```

----------------------------------------

TITLE: Create Edge Config Client with Force Cache Option (JavaScript)
DESCRIPTION: This snippet shows how to create an Edge Config client using `createClient` and pass an options object with `cache: 'force-cache'`. This configures the underlying fetch calls to use the browser's or runtime's cache, potentially serving stale data but enabling static page generation.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_8

LANGUAGE: js
CODE:
```
import { createClient } from '@vercel/edge-config';

const edgeConfigClient = createClient(process.env.EDGE_CONFIG, {
  cache: 'force-cache',
});

// then use the client as usual
edgeConfigClient.get('someKey');
```

----------------------------------------

TITLE: Provide Connection String Explicitly (SvelteKit Diff)
DESCRIPTION: Illustrates how to explicitly pass the database connection string to the `createKysely` function in SvelteKit. This approach uses SvelteKit's `$env/static/private` module to access private environment variables, avoiding reliance on `process.env`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres-kysely/README.md#_snippet_5

LANGUAGE: diff
CODE:
```
import { createKysely } from '@vercel/postgres-kysely';
+ import { POSTGRES_URL } from '$env/static/private';

interface Database {
  person: PersonTable;
  pet: PetTable;
  movie: MovieTable;
}

- const db = createKysely<Database>();
+ const db = createKysely<Database>({
+  connectionString: POSTGRES_URL,
+ });
```

----------------------------------------

TITLE: Install @vercel/postgres-kysely Package (Bash)
DESCRIPTION: Installs the `@vercel/postgres-kysely` package using the pnpm package manager. This package provides the wrapper functionality for Kysely.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres-kysely/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
pnpm install @vercel/postgres-kysely
```

----------------------------------------

TITLE: Creating Folder in Vercel Blob (TypeScript)
DESCRIPTION: Demonstrates how to create an empty folder in Vercel Blob by providing a pathname that ends with a trailing slash. This operation does not require a body.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
const blob = await vercelBlob.put('folder/', {
  access: 'public',
  addRandomSuffix: false,
});
```

----------------------------------------

TITLE: Explicitly Provide Connection String (SvelteKit Example)
DESCRIPTION: Demonstrates how to explicitly pass the database connection string to `createPool` using a variable sourced from SvelteKit's private environment variables (`$env/static/private`), bypassing the need for `process.env`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_10

LANGUAGE: Diff
CODE:
```
import { createPool } from '@vercel/postgres';
+ import { POSTGRES_URL } from '$env/static/private';

import { createPool } from '@vercel/postgres';
const pool = createPool({
-  /* config */
+  connectionString: POSTGRES_URL
});
```

----------------------------------------

TITLE: SvelteKit Integration with Private Environment Variables
DESCRIPTION: Shows how to integrate `@vercel/kv` into a SvelteKit project by explicitly passing environment variables loaded via `$env/static/private` to the `createClient` function.
SOURCE: https://github.com/vercel/storage/blob/main/packages/kv/README.md#_snippet_5

LANGUAGE: diff
CODE:
```
import { createClient } from '@vercel/kv';
+ import { KV_REST_API_URL, KV_REST_API_TOKEN } from '$env/static/private';

const kv = createClient({
-  url: 'https://<hostname>.redis.vercel-storage.com',
-  token: '<token>',
+  url: KV_REST_API_URL,
+  token: KV_REST_API_TOKEN,
});

await kv.set('key', 'value');
```

----------------------------------------

TITLE: Read Value from Specific Edge Config Client (JavaScript)
DESCRIPTION: This snippet illustrates how to create a client for a specific Edge Config using `createClient` with a connection string (e.g., from another environment variable). It then demonstrates reading a value from this custom client instance using its `get` method.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_5

LANGUAGE: js
CODE:
```
import { createClient } from '@vercel/edge-config';
const edgeConfig = createClient(process.env.ANOTHER_EDGE_CONFIG);
await edgeConfig.get('someKey');
```

----------------------------------------

TITLE: Configuring Vite to load environment variables using dotenv-expand
DESCRIPTION: Shows how to modify the `vite.config.js` file to manually load environment variables from the `.env` file into `process.env` during development mode. It uses `loadEnv` from Vite and `dotenvExpand.expand`.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_10

LANGUAGE: javascript
CODE:
```
import dotenvExpand from 'dotenv-expand';
import { loadEnv, defineConfig } from 'vite';

export default defineConfig(({ mode }) => {
  // This check is important!
  if (mode === 'development') {
    const env = loadEnv(mode, process.cwd(), '');
    dotenvExpand.expand({ parsed: env });
  }

  return {
    ...
  };
});
```

----------------------------------------

TITLE: Direct Redis Client for Streams
DESCRIPTION: Demonstrates how to use a standard Node.js Redis client (like `node-redis`) to connect directly to Vercel KV for functionalities not supported by `@vercel/kv`, such as Redis Streams.
SOURCE: https://github.com/vercel/storage/blob/main/packages/kv/README.md#_snippet_6

LANGUAGE: js
CODE:
```
import { createClient } from 'redis';

const client = createClient({
  url: process.env.KV_URL,
});

await client.connect();
await client.xRead({ key: 'mystream', id: '0' }, { COUNT: 2 });
```

----------------------------------------

TITLE: Vite Configuration for dotenv-expand
DESCRIPTION: Provides a Vite configuration snippet that uses `dotenv-expand` to manually load and expand environment variables from a `.env` file into `process.env` during development, addressing Vite's default behavior.
SOURCE: https://github.com/vercel/storage/blob/main/packages/kv/README.md#_snippet_4

LANGUAGE: js
CODE:
```
// vite.config.js
import dotenvExpand from 'dotenv-expand';
import { loadEnv, defineConfig } from 'vite';

export default defineConfig(({ mode }) => {
  // This check is important!
  if (mode === 'development') {
    const env = loadEnv(mode, process.cwd(), '');
    dotenvExpand.expand({ parsed: env });
  }

  return {
    ...
  };
});
```

----------------------------------------

TITLE: Configure Vite to Load Environment Variables
DESCRIPTION: This JavaScript code snippet shows how to modify your vite.config.js file to manually load and expand environment variables from your .env file into process.env during development mode using dotenv-expand.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/README.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import dotenvExpand from 'dotenv-expand';
import { loadEnv, defineConfig } from 'vite';

export default defineConfig(({ mode }) => {
  // This check is important!
  if (mode === 'development') {
    const env = loadEnv(mode, process.cwd(), '');
    dotenvExpand.expand({ parsed: env });
  }

  return {
    ...
  };
});
```

----------------------------------------

TITLE: Configure Vite for Environment Variables (JavaScript)
DESCRIPTION: Shows how to modify the Vite configuration file (`vite.config.js`) to load and expand environment variables from `.env` into `process.env` during development using `dotenv-expand`. This is necessary because Vite doesn't expose `.env` variables on `process.env` by default.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres-kysely/README.md#_snippet_4

LANGUAGE: javascript
CODE:
```
// vite.config.js
import dotenvExpand from 'dotenv-expand';
import { loadEnv, defineConfig } from 'vite';

export default defineConfig(({ mode }) => {
  // This check is important!
  if (mode === 'development') {
    const env = loadEnv(mode, process.cwd(), '');
    dotenvExpand.expand({ parsed: env });
  }

  return {
    ...
  };
});
```

----------------------------------------

TITLE: Configure Vite to Load Env Vars with dotenv-expand
DESCRIPTION: Modifies the Vite configuration file (`vite.config.js`) to use `loadEnv` and `dotenvExpand` to populate `process.env` with environment variables during development, addressing Vite's default behavior.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
// vite.config.js
import dotenvExpand from 'dotenv-expand';
import { loadEnv, defineConfig } from 'vite';

export default defineConfig(({ mode }) => {
  // This check is important!
  if (mode === 'development') {
    const env = loadEnv(mode, process.cwd(), '');
    dotenvExpand.expand({ parsed: env });
  }

  return {
    ...
  };
});
```

----------------------------------------

TITLE: Install dotenv and dotenv-expand
DESCRIPTION: Installs the necessary development dependencies (`dotenv` and `dotenv-expand`) required for manually loading and expanding environment variables into `process.env` in a Vite project.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_8

LANGUAGE: Shell
CODE:
```
pnpm install --save-dev dotenv dotenv-expand
```

----------------------------------------

TITLE: Importing and creating a single Client in TypeScript
DESCRIPTION: Imports the `createClient` function to create a single database client instance with custom configuration options. This is suitable for scenarios requiring a dedicated connection.
SOURCE: https://github.com/vercel/storage/blob/main/packages/postgres/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
// Need a single client?:
import { createClient } from '@vercel/postgres';
const client = createClient({
  /* config */
});
```

----------------------------------------

TITLE: Installing dotenv and dotenv-expand with pnpm
DESCRIPTION: Installs the necessary development dependencies, `dotenv` and `dotenv-expand`, using pnpm. These libraries are used to load environment variables from a `.env` file into `process.env` for frameworks like Vite.
SOURCE: https://github.com/vercel/storage/blob/main/packages/edge-config/README.md#_snippet_9

LANGUAGE: shell
CODE:
```
pnpm install --save-dev dotenv dotenv-expand
```

----------------------------------------

TITLE: Install dotenv and dotenv-expand with pnpm
DESCRIPTION: Install these development dependencies using pnpm to help load environment variables from a .env file into process.env, which is useful for frameworks like Vite that don't expose them by default.
SOURCE: https://github.com/vercel/storage/blob/main/packages/blob/README.md#_snippet_2

LANGUAGE: shell
CODE:
```
pnpm install --save-dev dotenv dotenv-expand
```

----------------------------------------

TITLE: Uploading ReadableStream with Multipart in Vercel Blob (TypeScript)
DESCRIPTION: Provides an example of uploading a file from a web `ReadableStream` (like `response.body` from `fetch`) using the `multipart: true` option. The stream is read and uploaded gradually.
SOURCE: https://github.com/vercel/storage/blob/main/test/next/CHANGELOG.md#_snippet_7

LANGUAGE: TypeScript
CODE:
```
const response = await fetch(
  'https://example-files.online-convert.com/video/mp4/example_big.mp4',
);

const blob = await vercelBlob.put(
  'example_big.mp4',
  // this works too 👍, it will gradually read the file from internet and upload it
  response.body,
  { access: 'public', multipart: true },
);
```