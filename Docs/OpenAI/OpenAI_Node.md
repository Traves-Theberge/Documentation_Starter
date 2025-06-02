TITLE: Installing OpenAI Library with npm
DESCRIPTION: This command installs the OpenAI Node.js library using npm, making it available for use in JavaScript and TypeScript projects. It's the standard way to add the package as a dependency.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install openai
```

----------------------------------------

TITLE: Auto-parsing Function Tool Calls with Zod (TypeScript)
DESCRIPTION: This example illustrates how `client.chat.completions.parse()` can automatically parse function tool calls using `zodFunction`. It defines Zod schemas for a database query tool, including table, column, operator, and condition types, and then uses this schema to parse a user's natural language query into a structured tool call, demonstrating access to the parsed arguments. Dependencies include `openai/helpers/zod` and `zod`.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { zodFunction } from 'openai/helpers/zod';
import OpenAI from 'openai/index';
import { z } from 'zod';

const Table = z.enum(['orders', 'customers', 'products']);

const Column = z.enum([
  'id',
  'status',
  'expected_delivery_date',
  'delivered_at',
  'shipped_at',
  'ordered_at',
  'canceled_at',
]);

const Operator = z.enum(['=', '>', '<', '<=', '>=', '!=']);

const OrderBy = z.enum(['asc', 'desc']);

const DynamicValue = z.object({
  column_name: z.string(),
});

const Condition = z.object({
  column: z.string(),
  operator: Operator,
  value: z.union([z.string(), z.number(), DynamicValue]),
});

const Query = z.object({
  table_name: Table,
  columns: z.array(Column),
  conditions: z.array(Condition),
  order_by: OrderBy,
});

const client = new OpenAI();
const completion = await client.chat.completions.parse({
  model: 'gpt-4o-2024-08-06',
  messages: [
    {
      role: 'system',
      content:
        'You are a helpful assistant. The current date is August 6, 2024. You help users query for the data they are looking for by calling the query function.',
    },
    {
      role: 'user',
      content: 'look up all my orders in november of last year that were fulfilled but not delivered on time',
    },
  ],
  tools: [zodFunction({ name: 'query', parameters: Query })],
});
console.dir(completion, { depth: 10 });

const toolCall = completion.choices[0]?.message.tool_calls?.[0];
if (toolCall) {
  const args = toolCall.function.parsed_arguments as z.infer<typeof Query>;
  console.log(args);
  console.log(args.table_name);
}

main();

```

----------------------------------------

TITLE: Integrating Zod for Schema Validation in OpenAI Chat Completions (TypeScript)
DESCRIPTION: This example illustrates how to integrate `zod` for robust schema validation of tool parameters and assistant responses in OpenAI chat completions. By using `zod` schemas with `zod-to-json-schema`, the API automatically receives the correct JSON Schema for tool parameters, ensuring type safety and data integrity for function calls.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_40

LANGUAGE: typescript
CODE:
```
import OpenAI from 'openai';
import { z } from 'zod';
import { zodToJsonSchema } from 'zod-to-json-schema';

const client = new OpenAI();

async function main() {
  const runner = client.chat.completions
    .runTools({
      model: 'gpt-3.5-turbo',
      messages: [{ role: 'user', content: "How's the weather this week in Los Angeles?" }],
      tools: [
        {
          type: 'function',
          function: {
            function: getWeather,
            parse: GetWeatherParameters.parse,
            parameters: zodToJsonSchema(GetWeatherParameters),
          },
        },
      ],
    })
    .on('message', (message) => console.log(message));

  const finalContent = await runner.finalContent();
  console.log('Final content:', finalContent);
}

const GetWeatherParameters = z.object({
  location: z.enum(['Boston', 'New York City', 'Los Angeles', 'San Francisco']),
});

async function getWeather(args: z.infer<typeof GetWeatherParameters>) {
  const { location } = args;
  // … do lookup …
  return { temperature, precipitation };
}

main();
```

----------------------------------------

TITLE: Auto-parsing JSON Responses with Zod Schemas (TypeScript)
DESCRIPTION: This snippet demonstrates how to use `client.chat.completions.parse()` with `zodResponseFormat` to automatically parse model responses into a structured TypeScript object based on a Zod schema. It defines a `MathResponse` schema and uses it to parse a math problem's solution, showing how to access the parsed steps and final answer. Dependencies include `openai/helpers/zod` and `zod`.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { zodResponseFormat } from 'openai/helpers/zod';
import OpenAI from 'openai/index';
import { z } from 'zod';

const Step = z.object({
  explanation: z.string(),
  output: z.string(),
});

const MathResponse = z.object({
  steps: z.array(Step),
  final_answer: z.string(),
});

const client = new OpenAI();

const completion = await client.chat.completions.parse({
  model: 'gpt-4o-2024-08-06',
  messages: [
    { role: 'system', content: 'You are a helpful math tutor.' },
    { role: 'user', content: 'solve 8x + 31 = 2' },
  ],
  response_format: zodResponseFormat(MathResponse, 'math_response'),
});

console.dir(completion, { depth: 5 });

const message = completion.choices[0]?.message;
if (message?.parsed) {
  console.log(message.parsed.steps);
  console.log(`answer: ${message.parsed.final_answer}`);
}
```

----------------------------------------

TITLE: Bulk Uploading Files to OpenAI Vector Store (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `uploadAndPoll` helper function from the OpenAI Node.js library to upload multiple files to a specified vector store. It takes an array of file streams and the vector store ID, then initiates the upload and polls for its completion.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_42

LANGUAGE: TypeScript
CODE:
```
const fileList = [
  createReadStream('/home/data/example.pdf'),
  ...
];

const batch = await openai.vectorStores.fileBatches.uploadAndPoll(vectorStore.id, {files: fileList});
```

----------------------------------------

TITLE: Implementing Automated Function Calls with `runTools` in TypeScript
DESCRIPTION: This TypeScript example demonstrates how to use `client.chat.completions.runTools` to enable automated function calls. It defines `getCurrentLocation` and `getWeather` functions, which the model can invoke. The `getWeather` function uses `JSON.parse` for argument parsing, and the `on('message')` event listener logs intermediate messages, while `finalContent()` retrieves the final response from the model.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_16

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI();

async function main() {
  const runner = client.chat.completions
    .runTools({
      model: 'gpt-4o',
      messages: [{ role: 'user', content: 'How is the weather this week?' }],
      tools: [
        {
          type: 'function',
          function: {
            function: getCurrentLocation,
            parameters: { type: 'object', properties: {} },
          },
        },
        {
          type: 'function',
          function: {
            function: getWeather,
            parse: JSON.parse, // or use a validation library like zod for typesafe parsing.
            parameters: {
              type: 'object',
              properties: {
                location: { type: 'string' }
              }
            }
          }
        }
      ]
    })
    .on('message', (message) => console.log(message));

  const finalContent = await runner.finalContent();
  console.log();
  console.log('Final content:', finalContent);
}

async function getCurrentLocation() {
  return 'Boston'; // Simulate lookup
}

async function getWeather(args: { location: string }) {
  const { location } = args;
  // … do lookup …
  return { temperature, precipitation };
}

main();

// {role: "user",      content: "How's the weather this week?"}
// {role: "assistant", tool_calls: [{type: "function", function: {name: "getCurrentLocation", arguments: "{}"}, id: "123"}
// {role: "tool",      name: "getCurrentLocation", content: "Boston", tool_call_id: "123"}
// {role: "assistant", tool_calls: [{type: "function", function: {name: "getWeather", arguments: '{"location": "Boston"}'}, id: "1234"}]}
// {role: "tool",      name: "getWeather", content: '{"temperature": "50degF", "preciptation": "high"}', tool_call_id: "1234"}
// {role: "assistant", content: "It's looking cold and rainy - you might want to wear a jacket!"}
//
// Final content: "It's looking cold and rainy - you might want to wear a jacket!"
```

----------------------------------------

TITLE: OpenAI SDK Asynchronous Operation Polling Methods (TypeScript)
DESCRIPTION: This snippet lists the `_AndPoll` helper methods available in the OpenAI Node.js SDK for managing asynchronous API operations. These methods automatically poll the API until a terminal state is reached for tasks like creating and running threads, submitting tool outputs, or uploading files to vector stores, simplifying the handling of long-running processes.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_41

LANGUAGE: typescript
CODE:
```
client.beta.threads.createAndRunPoll(...)
client.beta.threads.runs.createAndPoll((...)
client.beta.threads.runs.submitToolOutputsAndPoll((...)
client.beta.vectorStores.files.uploadAndPoll((...)
client.beta.vectorStores.files.createAndPoll((...)
client.beta.vectorStores.fileBatches.createAndPoll((...)
client.beta.vectorStores.fileBatches.uploadAndPoll((...)
```

----------------------------------------

TITLE: Configuring OpenAI Node.js Library with Azure OpenAI in TypeScript
DESCRIPTION: This snippet shows how to configure the OpenAI Node.js library to work with Microsoft Azure OpenAI using the `AzureOpenAI` class. It demonstrates setting up Azure AD token authentication with `DefaultAzureCredential` and making a chat completion request.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_11

LANGUAGE: TypeScript
CODE:
```
import { AzureOpenAI } from 'openai';
import { getBearerTokenProvider, DefaultAzureCredential } from '@azure/identity';

const credential = new DefaultAzureCredential();
const scope = 'https://cognitiveservices.azure.com/.default';
const azureADTokenProvider = getBearerTokenProvider(credential, scope);

const openai = new AzureOpenAI({ azureADTokenProvider });

const result = await openai.chat.completions.create({
  model: 'gpt-4o',
  messages: [{ role: 'user', content: 'Say hello!' }],
});

console.log(result.choices[0]!.message?.content);
```

----------------------------------------

TITLE: Integrating with Microsoft Azure OpenAI (TypeScript)
DESCRIPTION: This snippet shows how to configure the OpenAI Node.js SDK to work with Azure OpenAI, using `AzureOpenAI` class and Azure AD credentials. It demonstrates setting up authentication with `DefaultAzureCredential` and making a chat completion request.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_21

LANGUAGE: TypeScript
CODE:
```
import { AzureOpenAI } from 'openai';
import { getBearerTokenProvider, DefaultAzureCredential } from '@azure/identity';

const credential = new DefaultAzureCredential();
const scope = 'https://cognitiveservices.azure.com/.default';
const azureADTokenProvider = getBearerTokenProvider(credential, scope);

const openai = new AzureOpenAI({
  azureADTokenProvider,
  apiVersion: '<The API version, e.g. 2024-10-01-preview>',
});

const result = await openai.chat.completions.create({
  model: 'gpt-4o',
  messages: [{ role: 'user', content: 'Say hello!' }],
});

console.log(result.choices[0]!.message?.content);
```

----------------------------------------

TITLE: Handling API Errors with OpenAI.APIError in TypeScript
DESCRIPTION: This snippet demonstrates how to catch and handle `OpenAI.APIError` instances when API calls fail. It shows how to access properties like `request_id`, `status`, `name`, and `headers` for debugging purposes. If the error is not an `APIError`, it is re-thrown.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_7

LANGUAGE: TypeScript
CODE:
```
async function main() {
  const job = await client.fineTuning.jobs
    .create({ model: 'gpt-4o', training_file: 'file-abc123' })
    .catch(async (err) => {
      if (err instanceof OpenAI.APIError) {
        console.log(err.request_id);
        console.log(err.status); // 400
        console.log(err.name); // BadRequestError
        console.log(err.headers); // {server: 'nginx', ...}
      } else {
        throw err;
      }
    });
}

main();
```

----------------------------------------

TITLE: Generating Text with OpenAI Chat Completions API in TypeScript
DESCRIPTION: This code demonstrates using the OpenAI Chat Completions API for text generation. It sets up the client and then uses `client.chat.completions.create` with a model and an array of messages to simulate a conversational interaction.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI({
  apiKey: process.env['OPENAI_API_KEY'], // This is the default and can be omitted
});

const completion = await client.chat.completions.create({
  model: 'gpt-4o',
  messages: [
    { role: 'developer', content: 'Talk like a pirate.' },
    { role: 'user', content: 'Are semicolons optional in JavaScript?' },
  ],
});

console.log(completion.choices[0].message.content);
```

----------------------------------------

TITLE: Importing OpenAI Main Client (After APIClient Removal) (TypeScript)
DESCRIPTION: This snippet illustrates the new recommended way to import the main `OpenAI` client class directly from the `openai` package. This replaces the need to import the now-removed `APIClient` base class.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { OpenAI } from 'openai';
```

----------------------------------------

TITLE: Generating Text with OpenAI Responses API in TypeScript
DESCRIPTION: This snippet shows how to use the OpenAI Responses API to generate text. It initializes the client with an API key, then calls `client.responses.create` with a model, instructions, and input to get a text response.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI({
  apiKey: process.env['OPENAI_API_KEY'], // This is the default and can be omitted
});

const response = await client.responses.create({
  model: 'gpt-4o',
  instructions: 'You are a coding assistant that talks like a pirate',
  input: 'Are semicolons optional in JavaScript?',
});

console.log(response.output_text);
```

----------------------------------------

TITLE: Initializing AzureOpenAI Client with Azure AD Authentication (TypeScript)
DESCRIPTION: This snippet demonstrates how to initialize the `AzureOpenAI` client using Azure Active Directory (AD) authentication. It sets up a `DefaultAzureCredential` and a bearer token provider to authenticate requests to Azure OpenAI services, then creates a chat completion. Required dependencies include `openai` and `@azure/identity`.
SOURCE: https://github.com/openai/openai-node/blob/master/azure.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { AzureOpenAI } from 'openai';
import { getBearerTokenProvider, DefaultAzureCredential } from '@azure/identity';

const credential = new DefaultAzureCredential();
const scope = 'https://cognitiveservices.azure.com/.default';
const azureADTokenProvider = getBearerTokenProvider(credential, scope);

const openai = new AzureOpenAI({
  azureADTokenProvider,
  apiVersion: '<The API version, e.g. 2024-10-01-preview>',
});

const result = await openai.chat.completions.create({
  model: 'gpt-4o',
  messages: [{ role: 'user', content: 'Say hello!' }],
});

console.log(result.choices[0]!.message?.content);
```

----------------------------------------

TITLE: Initiating Chat Completion Streaming with Runner (TypeScript)
DESCRIPTION: This method initiates a chat completion stream, returning a `ChatCompletionStreamingRunner`. The runner provides an async iterator, emits events, and includes helper methods for accumulating chunks and managing the conversation state. It's suitable for scenarios requiring more structured stream handling.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_15

LANGUAGE: TypeScript
CODE:
```
openai.chat.completions.stream({ stream?: false, … }, options?): ChatCompletionStreamingRunner
```

----------------------------------------

TITLE: Subscribing to Text Content Events in OpenAI Node.js
DESCRIPTION: These events specifically target the creation, incremental updates (delta), and completion of Text content within messages. The delta event provides both the incremental update and an accumulated snapshot for convenience.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_9

LANGUAGE: ts
CODE:
```
.on('textCreated', (content: Text) => ...)
.on('textDelta', (delta: TextDelta, snapshot: Text) => ...)
.on('textDone', (content: Text, snapshot: Message) => ...)
```

----------------------------------------

TITLE: Streaming OpenAI Responses in TypeScript
DESCRIPTION: This snippet illustrates how to stream responses from the OpenAI API using Server Sent Events (SSE). It calls `client.responses.create` with `stream: true` and then iterates over the asynchronous stream to log each event as it arrives.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI();

const stream = await client.responses.create({
  model: 'gpt-4o',
  input: 'Say "Sheep sleep deep" ten times fast!',
  stream: true,
});

for await (const event of stream) {
  console.log(event);
}
```

----------------------------------------

TITLE: Accessing Main Chat Completions (After Beta Namespace Removal) (TypeScript)
DESCRIPTION: This snippet shows the new method for accessing chat completion methods, such as `parse()`, `stream()`, and `runTools()`, directly under the main `chat.completions` namespace. The `beta.chat` namespace has been consolidated.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_22

LANGUAGE: ts
CODE:
```
client.chat.completions.parse()
client.chat.completions.stream()
client.chat.completions.runTools()
```

----------------------------------------

TITLE: Creating and Polling Vector Store File in Node.js
DESCRIPTION: This asynchronous method creates a new file within a specified vector store and then continuously polls its status until it reaches a terminal state (e.g., completed or failed). It requires the `vectorStoreId` and the file `body`, with optional `options` for configuration. The method returns a `Promise` that resolves to a `VectorStoreFile` object once the polling is complete.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_49

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.createAndPoll(vectorStoreId, body, options?) -> Promise<VectorStoreFile>
```

----------------------------------------

TITLE: Submitting Tool Outputs and Streaming in OpenAI Node.js
DESCRIPTION: This method is used to submit outputs for a tool call that an Assistant run is waiting on, and then immediately start streaming the subsequent response from the run.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_5

LANGUAGE: ts
CODE:
```
openai.beta.threads.runs.submitToolOutputsStream();
```

----------------------------------------

TITLE: Initializing OpenAI Realtime WebSocket for Conversational AI in TypeScript
DESCRIPTION: This snippet demonstrates how to initialize and use the `OpenAIRealtimeWebSocket` for building low-latency, multi-modal conversational experiences. It shows how to set up an event listener for `response.text.delta` to process streaming text output.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import { OpenAIRealtimeWebSocket } from 'openai/beta/realtime/websocket';

const rt = new OpenAIRealtimeWebSocket({ model: 'gpt-4o-realtime-preview-2024-12-17' });

rt.on('response.text.delta', (event) => process.stdout.write(event.delta));
```

----------------------------------------

TITLE: Creating Assistant with OpenAI Node.js Client
DESCRIPTION: This method creates a new assistant. It takes parameters defining the assistant's behavior and tools, returning an `Assistant` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_55

LANGUAGE: TypeScript
CODE:
```
client.beta.assistants.create({ ...params })
```

----------------------------------------

TITLE: Handling Stream Errors - OpenAI Node.js
DESCRIPTION: This event fires when an error is encountered during the streaming process, outside of a `parse` function or an abort signal. It provides the `OpenAIError` object for error handling and debugging.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_33

LANGUAGE: TypeScript
CODE:
```
.on('error', (error: OpenAIError) => …)
```

----------------------------------------

TITLE: Uploading Files to OpenAI in TypeScript
DESCRIPTION: This example demonstrates various methods for uploading files to the OpenAI API, including using Node.js `fs.createReadStream`, Web `File` API, `fetch` Response, and the `toFile` helper for buffers and Uint8Arrays. It shows flexibility in handling file inputs for purposes like fine-tuning.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import fs from 'fs';
import OpenAI, { toFile } from 'openai';

const client = new OpenAI();

// If you have access to Node `fs` we recommend using `fs.createReadStream()`:
await client.files.create({ file: fs.createReadStream('input.jsonl'), purpose: 'fine-tune' });

// Or if you have the web `File` API you can pass a `File` instance:
await client.files.create({ file: new File(['my bytes'], 'input.jsonl'), purpose: 'fine-tune' });

// You can also pass a `fetch` `Response`:
await client.files.create({ file: await fetch('https://somesite/input.jsonl'), purpose: 'fine-tune' });

// Finally, if none of the above are convenient, you can use our `toFile` helper:
await client.files.create({
  file: await toFile(Buffer.from('my bytes'), 'input.jsonl'),
  purpose: 'fine-tune',
});
await client.files.create({
  file: await toFile(new Uint8Array([0, 1, 2]), 'input.jsonl'),
  purpose: 'fine-tune',
});
```

----------------------------------------

TITLE: Auto-paginating List Methods with for await...of (TypeScript)
DESCRIPTION: This asynchronous function demonstrates how to automatically iterate through all pages of a paginated list method, such as `fineTuning.jobs.list`, using the `for await...of` syntax. It simplifies fetching all items without manual page management.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_18

LANGUAGE: TypeScript
CODE:
```
async function fetchAllFineTuningJobs(params) {
  const allFineTuningJobs = [];
  // Automatically fetches more pages as needed.
  for await (const fineTuningJob of client.fineTuning.jobs.list({ limit: 20 })) {
    allFineTuningJobs.push(fineTuningJob);
  }
  return allFineTuningJobs;
}
```

----------------------------------------

TITLE: Creating and Running a Stream with OpenAI Node.js
DESCRIPTION: This method allows adding a message to a thread, starting a new run, and then streaming the response from that run. It provides a convenient way to initiate a full interaction flow.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_4

LANGUAGE: ts
CODE:
```
openai.beta.threads.createAndRunStream();
```

----------------------------------------

TITLE: Iterating Through Paginated Results with for await (TypeScript)
DESCRIPTION: This snippet shows the `for await` syntax for iterating through paginated list results, such as fine-tuning jobs. This syntax automatically fetches more pages as needed and remains unaffected by recent pagination interface changes.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_16

LANGUAGE: ts
CODE:
```
for await (const fineTuningJob of client.fineTuning.jobs.list()) {
  console.log(fineTuningJob);
}
```

----------------------------------------

TITLE: Submitting Tool Outputs for a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Submits the outputs from tool calls back to a run that is in a 'requires_action' state. This method requires a `runID` and `params` containing the tool outputs. It returns the `Run` object, which will then continue processing.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_72

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.submitToolOutputs(runID, { ...params })
```

----------------------------------------

TITLE: Generating Images with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method generates new images from a text prompt using the OpenAI API. It accepts an object of parameters (`params`) including the prompt and image generation options. It returns an `ImagesResponse` containing the newly generated images.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_8

LANGUAGE: TypeScript
CODE:
```
client.images.generate({ ...params })
```

----------------------------------------

TITLE: Subscribing to Run Step Events in OpenAI Node.js
DESCRIPTION: These event listeners allow subscription to the creation, incremental updates (delta), and completion of a RunStep within an Assistant run. They provide granular control over monitoring the progress of individual steps.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_7

LANGUAGE: ts
CODE:
```
.on('runStepCreated', (runStep: RunStep) => ...)
.on('runStepDelta', (delta: RunStepDelta, snapshot: RunStep) => ...)
.on('runStepDone', (runStep: RunStep) => ...)
```

----------------------------------------

TITLE: Implementing Realtime Text Conversation with OpenAI Realtime API (ws)
DESCRIPTION: This snippet demonstrates how to establish a WebSocket connection using `OpenAIRealtimeWS` to engage in a text-based conversation. It initializes a session, sends a user message, and processes real-time text responses, closing the connection upon completion. Requires `ws` and `@types/ws`.
SOURCE: https://github.com/openai/openai-node/blob/master/realtime.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
// requires `yarn add ws @types/ws`
import { OpenAIRealtimeWS } from 'openai/beta/realtime/ws';

const rt = new OpenAIRealtimeWS({ model: 'gpt-4o-realtime-preview-2024-12-17' });

// access the underlying `ws.WebSocket` instance
rt.socket.on('open', () => {
  console.log('Connection opened!');
  rt.send({
    type: 'session.update',
    session: {
      modalities: ['text'],
      model: 'gpt-4o-realtime-preview',
    },
  });

  rt.send({
    type: 'conversation.item.create',
    item: {
      type: 'message',
      role: 'user',
      content: [{ type: 'input_text', text: 'Say a couple paragraphs!' }],
    },
  });

  rt.send({ type: 'response.create' });
});

rt.on('error', (err) => {
  // in a real world scenario this should be logged somewhere as you
  // likely want to continue processing events regardless of any errors
  throw err;
});

rt.on('session.created', (event) => {
  console.log('session created!', event.session);
  console.log();
});

rt.on('response.text.delta', (event) => process.stdout.write(event.delta));
rt.on('response.text.done', () => console.log());

rt.on('response.done', () => rt.close());

rt.socket.on('close', () => console.log('\nConnection closed!'));
```

----------------------------------------

TITLE: Creating and Running Thread with Streaming in OpenAI Node.js Client
DESCRIPTION: This method creates a new thread and runs it, streaming events as they occur. It takes the request body and optional `options` and returns an `AssistantStream`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_66

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.createAndRunStream(body, options?) -> AssistantStream
```

----------------------------------------

TITLE: Handling Errors in OpenAI Realtime API (TypeScript)
DESCRIPTION: This snippet illustrates the recommended approach for handling errors in the OpenAI Realtime API by registering an `error` event listener. It prevents unhandled promise rejections and ensures that errors are caught and potentially logged, allowing the underlying connection to remain usable.
SOURCE: https://github.com/openai/openai-node/blob/master/realtime.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
const rt = new OpenAIRealtimeWS({ model: 'gpt-4o-realtime-preview-2024-12-17' });
rt.on('error', (err) => {
  // in a real world scenario this should be logged somewhere as you
  // likely want to continue processing events regardless of any errors
  throw err;
});
```

----------------------------------------

TITLE: Creating Content Moderation with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method checks content for policy violations using the OpenAI API's moderation endpoint. It accepts an object of parameters (`params`) including the input text or image. It returns a `ModerationCreateResponse` indicating whether the content is flagged and for what categories.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_12

LANGUAGE: TypeScript
CODE:
```
client.moderations.create({ ...params })
```

----------------------------------------

TITLE: Receiving Content Deltas - OpenAI Node.js
DESCRIPTION: This event fires for every chunk containing new content during streaming. The `props` object provides the `delta` (new content string), `snapshot` (accumulated content), and `parsed` (partially parsed content if applicable).
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_18

LANGUAGE: TypeScript
CODE:
```
.on('content.delta', (props: ContentDeltaEvent) => ...)
```

----------------------------------------

TITLE: Creating and Subscribing to Run Events with OpenAI Node.js
DESCRIPTION: This snippet demonstrates how to create a streaming run for an OpenAI Assistant and subscribe to various events like 'textCreated', 'textDelta', 'toolCallCreated', and 'toolCallDelta'. It shows how to process and display output from the assistant, including code interpreter inputs and outputs, by writing to process.stdout.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_2

LANGUAGE: ts
CODE:
```
const run = openai.beta.threads.runs
  .stream(thread.id, { assistant_id: assistant.id })
  .on('textCreated', (text) => process.stdout.write('\nassistant > '))
  .on('textDelta', (textDelta, snapshot) => process.stdout.write(textDelta.value))
  .on('toolCallCreated', (toolCall) => process.stdout.write(`\nassistant > ${toolCall.type}\n\n`))
  .on('toolCallDelta', (toolCallDelta, snapshot) => {
    if (toolCallDelta.type === 'code_interpreter') {
      if (toolCallDelta.code_interpreter.input) {
        process.stdout.write(toolCallDelta.code_interpreter.input);
      }
      if (toolCallDelta.code_interpreter.outputs) {
        process.stdout.write('\noutput >\n');
        toolCallDelta.code_interpreter.outputs.forEach((output) => {
          if (output.type === 'logs') {
            process.stdout.write(`\n${output.logs}\n`);
          }
        });
      }
    }
  });
```

----------------------------------------

TITLE: Streaming Run Events - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Streams events for an existing run, providing real-time updates on its progress. This method requires a `threadId` and a `body` for the run, with optional `options`. It returns an `AssistantStream` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_76

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.stream(threadId, body, options?)
```

----------------------------------------

TITLE: Accessing Request ID using .withResponse() (TypeScript)
DESCRIPTION: This snippet shows an alternative method to access the request ID by using the `.withResponse()` method, which returns both the parsed data and the `request_id` for streaming responses. This is particularly useful when dealing with streamed outputs where the request ID might be needed alongside the data.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_17

LANGUAGE: TypeScript
CODE:
```
const { data: stream, request_id } = await openai.responses
  .create({
    model: 'gpt-4o',
    input: 'Say this is a test',
    stream: true,
  })
  .withResponse();
```

----------------------------------------

TITLE: Function Tool Call Arguments Completion - OpenAI Node.js
DESCRIPTION: This event fires when a function tool call's arguments are complete. The `props` object includes the `name`, `index`, `arguments` (full raw JSON string), and `parsed_arguments` (fully parsed arguments object), signaling the readiness of the function call.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_23

LANGUAGE: TypeScript
CODE:
```
.on('tool_calls.function.arguments.done', (props: FunctionToolCallArgumentsDoneEvent) => ...)
```

----------------------------------------

TITLE: Initializing OpenAI Realtime API with Web API WebSocket (TypeScript)
DESCRIPTION: This snippet shows how to initialize the OpenAI Realtime API client using the browser's native `WebSocket` API via `OpenAIRealtimeWebSocket`. It highlights the change in class name and the `addEventListener` method for handling socket events compared to the `ws` library.
SOURCE: https://github.com/openai/openai-node/blob/master/realtime.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { OpenAIRealtimeWebSocket } from 'openai/beta/realtime/websocket';

const rt = new OpenAIRealtimeWebSocket({ model: 'gpt-4o-realtime-preview-2024-12-17' });
// ...
rt.socket.addEventListener('open', () => {
  // ...
});
```

----------------------------------------

TITLE: Submitting Tool Outputs and Polling - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Submits tool outputs for a run and then polls its status until completion. This method requires `threadId`, `runId`, and a `body` for the tool outputs, with optional `options`. It returns a `Promise` that resolves to the final `Run` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_77

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.submitToolOutputsAndPoll(threadId, runId, body, options?)
```

----------------------------------------

TITLE: Accessing Request ID using .withResponse() Method in TypeScript
DESCRIPTION: This snippet illustrates how to use the `.withResponse()` method to obtain the `request_id` alongside the response data when making API calls, particularly useful for streaming responses. This provides direct access to the request identifier for debugging.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_9

LANGUAGE: TypeScript
CODE:
```
const { data: stream, request_id } = await openai.chat.completions
  .create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: 'Say this is a test' }],
    stream: true,
  })
  .withResponse();
```

----------------------------------------

TITLE: Creating Audio Transcriptions with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method transcribes audio into text using the OpenAI API. It accepts an object of parameters (`params`) including the audio file and transcription options. It returns a `TranscriptionCreateResponse` containing the transcribed text.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_9

LANGUAGE: TypeScript
CODE:
```
client.audio.transcriptions.create({ ...params })
```

----------------------------------------

TITLE: Waiting for File Processing with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This utility method polls the OpenAI API to check the processing status of a file until it's ready or a timeout occurs. It takes the file `id` and optional `pollInterval` and `maxWait` parameters. It returns a `Promise` that resolves to a `FileObject` once processing is complete.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
client.files.waitForProcessing(id, { pollInterval = 5000, maxWait = 30 * 60 * 1000 })
```

----------------------------------------

TITLE: Creating Thread with OpenAI Node.js Client
DESCRIPTION: This method creates a new thread. It can accept initial messages or metadata as parameters and returns a `Thread` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_60

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.create({ ...params })
```

----------------------------------------

TITLE: Recommended runTools Method (After runFunctions Removal) (TypeScript)
DESCRIPTION: This snippet refers to the `client.chat.completions.runTools()` method, which is the recommended replacement for the deprecated and removed `runFunctions()` method. It should be used for executing tools.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_26

LANGUAGE: ts
CODE:
```
client.chat.completions.runTools()
```

----------------------------------------

TITLE: Creating a Vector Store - Node.js
DESCRIPTION: This method creates a new Vector Store. It accepts an object of parameters and returns a VectorStore object upon successful creation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_29

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.create({ ...params })
```

----------------------------------------

TITLE: Subscribing to Tool Call Events in OpenAI Node.js
DESCRIPTION: These events allow subscription to the creation, incremental updates (delta), and completion of ToolCall objects. They are essential for monitoring and interacting with tool usage during an Assistant run.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_11

LANGUAGE: ts
CODE:
```
.on('toolCallCreated', (toolCall: ToolCall) => ...)
.on('toolCallDelta', (delta: RunStepDelta, snapshot: ToolCall) => ...)
.on('toolCallDone', (toolCall: ToolCall) => ...)
```

----------------------------------------

TITLE: Setting up Azure OpenAI Realtime Streaming Client (TypeScript)
DESCRIPTION: This snippet illustrates how to configure an `AzureOpenAI` client for real-time streaming capabilities. It initializes the client with Azure AD authentication and a specific deployment name, then passes this client to `OpenAIRealtimeWS.azure` to create a real-time WebSocket instance. This setup is a prerequisite for sending and receiving streaming responses.
SOURCE: https://github.com/openai/openai-node/blob/master/azure.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
const cred = new DefaultAzureCredential();
const scope = 'https://cognitiveservices.azure.com/.default';
const deploymentName = 'gpt-4o-realtime-preview-1001';
const azureADTokenProvider = getBearerTokenProvider(cred, scope);
const client = new AzureOpenAI({
  azureADTokenProvider,
  apiVersion: '2024-10-01-preview',
  deployment: deploymentName,
});
const rt = await OpenAIRealtimeWS.azure(client);
```

----------------------------------------

TITLE: Creating and Running Thread with Polling in OpenAI Node.js Client
DESCRIPTION: This method creates a new thread and runs it, then polls for the run's completion. It takes the request body and optional `options` and returns a `Promise<Threads.Run>`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_65

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.createAndRunPoll(body, options?) -> Promise<Threads.Run>
```

----------------------------------------

TITLE: Accessing Request ID from OpenAI Response Object in TypeScript
DESCRIPTION: This snippet shows how to retrieve the `_request_id` property directly from an OpenAI API response object, such as a chat completion. The `_request_id` corresponds to the `x-request-id` header and is useful for debugging and reporting issues to OpenAI.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const completion = await client.chat.completions.create({
  messages: [{ role: 'user', content: 'Say this is a test' }],
  model: 'gpt-4o',
});
console.log(completion._request_id); // req_123
```

----------------------------------------

TITLE: Creating a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Initiates a new run for a specific thread, allowing the Assistant to process messages. This method requires a `threadID` and accepts additional `params` for configuration. It returns a `Run` object representing the newly created run.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_67

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.create(threadID, { ...params })
```

----------------------------------------

TITLE: Configuring Default Request Timeout for OpenAI Client in TypeScript
DESCRIPTION: This snippet demonstrates how to set a default timeout for all API requests made using the OpenAI client. The `timeout` option, specified in milliseconds, determines how long the client will wait for a response before throwing an `APIConnectionTimeoutError`.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_14

LANGUAGE: TypeScript
CODE:
```
// Configure the default for all requests:
const client = new OpenAI({
  timeout: 20 * 1000, // 20 seconds (default is 10 minutes)
});
```

----------------------------------------

TITLE: Native Node.js Stream File Handling (After fileFromPath Removal) (TypeScript)
DESCRIPTION: This snippet shows the recommended approach for file handling using native Node.js streams, specifically `fs.createReadStream`. This method replaces the removed `OpenAI.fileFromPath` helper and is suitable for Node.js environments.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_12

LANGUAGE: ts
CODE:
```
import fs from 'fs';
fs.createReadStream('path/to/file');
```

----------------------------------------

TITLE: Handling Function Call Results - OpenAI Node.js
DESCRIPTION: This event fires when the function runner responds to a function call with `role: "function"`. The `content` of the response is provided as the first argument to the callback, allowing for processing of the function's output.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_17

LANGUAGE: TypeScript
CODE:
```
.on('functionCallResult', (content: string) => …)
```

----------------------------------------

TITLE: Creating and Running Thread with OpenAI Node.js Client
DESCRIPTION: This method creates a new thread and immediately runs it. It accepts parameters for both thread creation and run configuration, returning a `Run` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_64

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.createAndRun({ ...params })
```

----------------------------------------

TITLE: Initializing OpenAI Realtime WebSocket (TypeScript)
DESCRIPTION: This code demonstrates how to initialize and use the `OpenAIRealtimeWebSocket` for building low-latency, multi-modal conversational experiences. It sets up an event listener to process text deltas from the real-time API, enabling streaming output.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_20

LANGUAGE: TypeScript
CODE:
```
import { OpenAIRealtimeWebSocket } from 'openai/beta/realtime/websocket';

const rt = new OpenAIRealtimeWebSocket({ model: 'gpt-4o-realtime-preview-2024-12-17' });

rt.on('response.text.delta', (event) => process.stdout.write(event.delta));
```

----------------------------------------

TITLE: Creating a File with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method uploads a file to the OpenAI API. It accepts an object of parameters (`params`) which typically include the file content and its purpose. It returns a `FileObject` representing the uploaded file.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
client.files.create({ ...params })
```

----------------------------------------

TITLE: Receiving Final Assistant Content - OpenAI Node.js
DESCRIPTION: This event fires for the `content` of the last `role: "assistant"` message. It is not fired if there is no assistant message, providing the final generated content from the assistant.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_29

LANGUAGE: TypeScript
CODE:
```
.on('finalContent', (contentSnapshot: string) => …)
```

----------------------------------------

TITLE: Updating OpenAI Core Module Imports in TypeScript
DESCRIPTION: This change reflects the refactoring of `openai/core` and related modules. Public-facing code has moved to a new `core` folder, requiring updated import paths for modules like `error`, `pagination`, `resource`, `streaming`, and `uploads`.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
// Before
import 'openai/error';
import 'openai/pagination';
import 'openai/resource';
import 'openai/streaming';
import 'openai/uploads';
```

LANGUAGE: TypeScript
CODE:
```
// After
import 'openai/core/error';
import 'openai/core/pagination';
import 'openai/core/resource';
import 'openai/core/streaming';
import 'openai/core/uploads';
```

----------------------------------------

TITLE: Handling OpenAI Chat Completions with New `functionToolCall` Events (TypeScript)
DESCRIPTION: This snippet shows the updated event names for listening to `functionToolCall` related events with the `ChatCompletionRunner`'s `.runTools()` method. It reflects the changes made to better align with the tool-based API.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_28

LANGUAGE: TypeScript
CODE:
```
openai.chat.completions
  .runTools({
    // ..
  })
  .on('functionToolCall', (functionCall) => console.log('functionCall', functionCall))
  .on('functionToolCallResult', (functionCallResult) => console.log('functionCallResult', functionCallResult))
  .on('finalFunctionToolCall', (functionCall) => console.log('finalFunctionCall', functionCall))
  .on('finalFunctionToolCallResult', (result) => console.log('finalFunctionCallResult', result));
```

----------------------------------------

TITLE: Importing Main Chat Completion Types (After Relocation) (TypeScript)
DESCRIPTION: This snippet demonstrates the new import path for chat completion types such as `ParsedChatCompletion`, `ParsedChoice`, and `ParsedFunction` from `openai/resources/chat/completions`. These types are now available directly under the main chat resources.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_24

LANGUAGE: ts
CODE:
```
import { ParsedChatCompletion, ParsedChoice, ParsedFunction } from 'openai/resources/chat/completions';
```

----------------------------------------

TITLE: Creating Image Variations with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method generates variations of a given image using the OpenAI API. It accepts an object of parameters (`params`) including the original image and desired variations. It returns an `ImagesResponse` containing the generated image variations.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
client.images.createVariation({ ...params })
```

----------------------------------------

TITLE: Starting a Stream for an Existing Run in OpenAI Node.js
DESCRIPTION: This method initiates a stream for an existing run associated with a thread that already contains messages. It's used to stream the response from a run that has already been started.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_3

LANGUAGE: ts
CODE:
```
openai.beta.threads.runs.stream();
```

----------------------------------------

TITLE: Listing Fine-Tuning Jobs in OpenAI Node.js
DESCRIPTION: This method retrieves a paginated list of fine-tuning jobs. It accepts an optional `params` object for filtering or pagination and returns a `FineTuningJobsPage` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_18

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.list({ ...params })
```

----------------------------------------

TITLE: Integrating Custom Logger with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This example illustrates how to integrate a custom logging library, such as Pino, with the OpenAI Node.js client. A `pino` logger instance is provided to the `logger` client option, allowing all messages at or above the configured `logLevel` (here, 'debug') to be routed through the custom logger for advanced filtering and formatting.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_24

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';
import pino from 'pino';

const logger = pino();

const client = new OpenAI({
  logger: logger.child({ name: 'OpenAI' }),
  logLevel: 'debug', // Send all messages to pino, allowing it to filter
});
```

----------------------------------------

TITLE: Configuring Fetch Options for OpenAI Node.js Client (JavaScript)
DESCRIPTION: This snippet illustrates how to set custom `fetchOptions` when initializing the OpenAI Node.js client. These options, which correspond to `RequestInit` properties, are applied to all requests made by this client instance. Request-specific `fetchOptions` can further override these client-level settings.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_29

LANGUAGE: JavaScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI({
  fetchOptions: {
    // `RequestInit` options
  },
});
```

----------------------------------------

TITLE: Manual Pagination Interface (After Simplification) (TypeScript)
DESCRIPTION: This snippet demonstrates the simplified manual pagination interface using `page.nextPageRequestOptions()`. This new method streamlines the process by directly providing the request options for the next page.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_18

LANGUAGE: ts
CODE:
```
page.nextPageRequestOptions();
```

----------------------------------------

TITLE: Uploading Vector Store File in Node.js
DESCRIPTION: This asynchronous method uploads a file to a specified vector store. It takes the `vectorStoreId` and the `file` content as arguments, with optional `options` for the upload process. The method returns a `Promise` that resolves to a `VectorStoreFile` object, indicating the file has been successfully uploaded and is pending processing.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_51

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.upload(vectorStoreId, file, options?) -> Promise<VectorStoreFile>
```

----------------------------------------

TITLE: Zod Response Format with Nullable Optional Property (After Error Fix) (TypeScript)
DESCRIPTION: This code demonstrates the corrected way to define an optional property within `zodResponseFormat` by chaining `.nullable()` to `.optional()`. This ensures compliance with the OpenAI API's requirement for all fields in structured outputs to be required or explicitly nullable.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_15

LANGUAGE: ts
CODE:
```
const completion = await client.chat.completions.parse({
  // ...
  response_format: zodResponseFormat(
    z.object({
      optional_property: z.string().optional().nullable(),
    }),
    'schema',
  ),
});
```

----------------------------------------

TITLE: Configuring Per-Request Retry Behavior for OpenAI Client in JavaScript
DESCRIPTION: This snippet shows how to override the default retry settings for a specific API request. By passing `maxRetries` in the request options, you can customize the retry count for individual calls, allowing for fine-grained control over error handling.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
// Or, configure per-request:
await client.chat.completions.create({ messages: [{ role: 'user', content: 'How can I get the name of the current day in JavaScript?' }], model: 'gpt-4o' }, {
  maxRetries: 5,
});
```

----------------------------------------

TITLE: Creating Audio Translations with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method translates audio into English text using the OpenAI API. It accepts an object of parameters (`params`) including the audio file and translation options. It returns a `TranslationCreateResponse` containing the translated text.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_10

LANGUAGE: TypeScript
CODE:
```
client.audio.translations.create({ ...params })
```

----------------------------------------

TITLE: Receiving Final Function Call - OpenAI Node.js
DESCRIPTION: This event fires for the last message that includes a defined `function_call`. It provides the complete function call object, useful for executing the final requested function.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_31

LANGUAGE: TypeScript
CODE:
```
.on('finalFunctionCall', (functionCall: ChatCompletionMessage.FunctionCall) => …)
```

----------------------------------------

TITLE: Receiving Final Chat Completion - OpenAI Node.js
DESCRIPTION: This event fires for the final chat completion. If the function call runner exceeds `maxChatCompletions`, the last chat completion is provided, offering the complete interaction summary.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_28

LANGUAGE: TypeScript
CODE:
```
.on('finalChatCompletion', (completion: ChatCompletion) => …)
```

----------------------------------------

TITLE: Searching a Vector Store - Node.js
DESCRIPTION: This method performs a search within a specified Vector Store. It requires the vectorStoreID and search parameters, returning a paginated VectorStoreSearchResponsesPage.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_34

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.search(vectorStoreID, { ...params })
```

----------------------------------------

TITLE: Listing Models with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method lists all available models from the OpenAI API. It takes no parameters. It returns a `ModelsPage` containing a list of `Model` objects.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_14

LANGUAGE: TypeScript
CODE:
```
client.models.list()
```

----------------------------------------

TITLE: Retrieving a Model with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method retrieves information about a specific model by its ID from the OpenAI API. It takes the `model` ID as a string parameter. It returns a `Model` object containing details about the requested model.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_13

LANGUAGE: TypeScript
CODE:
```
client.models.retrieve(model)
```

----------------------------------------

TITLE: Handling Stream Abort - OpenAI Node.js
DESCRIPTION: This event fires when the stream receives a signal to abort. It provides the `APIUserAbortError` object, indicating that the streaming operation was intentionally cancelled by the user or system.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_34

LANGUAGE: TypeScript
CODE:
```
.on('abort', (error: APIUserAbortError) => …)
```

----------------------------------------

TITLE: Accessing Raw HTTP Response Data (TypeScript)
DESCRIPTION: This snippet demonstrates two methods, `.asResponse()` and `.withResponse()`, for accessing the underlying `Response` object from `fetch()`. `.asResponse()` returns the response headers immediately without consuming the body, while `.withResponse()` returns both the parsed data and the raw response after body consumption, allowing inspection of headers and status.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_22

LANGUAGE: TypeScript
CODE:
```
const client = new OpenAI();

const httpResponse = await client.responses
  .create({ model: 'gpt-4o', input: 'say this is a test.' })
  .asResponse();

// access the underlying web standard Response object
console.log(httpResponse.headers.get('X-My-Header'));
console.log(httpResponse.statusText);

const { data: modelResponse, response: raw } = await client.responses
  .create({ model: 'gpt-4o', input: 'say this is a test.' })
  .withResponse();
console.log(raw.headers.get('X-My-Header'));
console.log(modelResponse);
```

----------------------------------------

TITLE: Subscribing to Message Events in OpenAI Node.js
DESCRIPTION: These events enable subscription to the creation, incremental updates (delta), and completion of Message objects. The delta event conveniently includes both the incremental update and an accumulated snapshot of the message content.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_8

LANGUAGE: ts
CODE:
```
.on('messageCreated', (message: Message) => ...)
.on('messageDelta', (delta: MessageDelta, snapshot: Message) => ...)
.on('messageDone', (message: Message) => ...)
```

----------------------------------------

TITLE: Collecting Final Results from Assistant Streams (TypeScript)
DESCRIPTION: These asynchronous methods on the assistant streaming object are used to collect all messages or run steps at the end of a stream. Calling them consumes the entire stream until completion, then returns the accumulated objects.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_14

LANGUAGE: TypeScript
CODE:
```
await .finalMessages() : Promise<Message[]>

await .finalRunSteps(): Promise<RunStep[]>
```

----------------------------------------

TITLE: Receiving Final Function Call Result - OpenAI Node.js
DESCRIPTION: This event fires for the last message with a `role: "function"`. It provides the content of the function call result, indicating the final output from a function execution.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_32

LANGUAGE: TypeScript
CODE:
```
.on('finalFunctionCallResult', (content: string) => …)
```

----------------------------------------

TITLE: Submitting Tool Outputs and Streaming - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Submits tool outputs for a run and streams events related to its subsequent processing. This method requires `threadId`, `runId`, and a `body` for the tool outputs, with optional `options`. It returns an `AssistantStream` object for real-time updates.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_78

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.submitToolOutputsStream(threadId, runId, body, options?)
```

----------------------------------------

TITLE: Uploading and Polling Vector Store File in Node.js
DESCRIPTION: This asynchronous method combines file upload with status polling, ensuring the file is not only uploaded but also processed to completion within the vector store. It requires the `vectorStoreId` and the `file` content, with optional `options`. The method returns a `Promise` that resolves to a `VectorStoreFile` object once the file has been uploaded and its processing is complete.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_52

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.uploadAndPoll(vectorStoreId, file, options?) -> Promise<VectorStoreFile>
```

----------------------------------------

TITLE: Stream End Event - OpenAI Node.js
DESCRIPTION: This is the very last event fired in the stream, signaling that all data has been processed and the streaming operation has concluded successfully.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_36

LANGUAGE: TypeScript
CODE:
```
.on('end', () => …)
```

----------------------------------------

TITLE: Manually Paginating List Methods (TypeScript)
DESCRIPTION: This snippet illustrates how to manually paginate through list results, fetching one page at a time. It also shows the use of convenience methods like `hasNextPage()` and `getNextPage()` for step-by-step navigation through paginated data.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_19

LANGUAGE: TypeScript
CODE:
```
let page = await client.fineTuning.jobs.list({ limit: 20 });
for (const fineTuningJob of page.data) {
  console.log(fineTuningJob);
}

// Convenience methods are provided for manually paginating:
while (page.hasNextPage()) {
  page = await page.getNextPage();
  // ...
}
```

----------------------------------------

TITLE: Retrieving File Content with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method retrieves the raw content of a specific file by its ID from the OpenAI API. It takes the `fileID` as a string parameter. It returns a `Response` object, typically a stream or buffer, containing the file's data.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
client.files.content(fileID)
```

----------------------------------------

TITLE: Listing Messages with OpenAI Node.js Client
DESCRIPTION: This method lists messages within a specified thread. It requires a `threadID` and supports pagination/filtering parameters. It returns a `MessagesPage` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_84

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.messages.list(threadID, { ...params })
```

----------------------------------------

TITLE: Cancelling a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Cancels a run that is currently in progress. This method requires a `runID` and can take optional `params`. It returns the `Run` object with its status updated to 'cancelled'.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_71

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.cancel(runID, { ...params })
```

----------------------------------------

TITLE: Waiting for Stream Completion - OpenAI Node.js
DESCRIPTION: This method returns an empty promise that resolves when the streaming operation is fully complete. It provides a convenient way to await the entire stream's conclusion without needing to listen for the 'end' event.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_38

LANGUAGE: TypeScript
CODE:
```
await .done()
```

----------------------------------------

TITLE: Receiving Total Usage - OpenAI Node.js
DESCRIPTION: This event fires at the end of the call, returning the total usage of the API call. Note that usage is not currently reported with `stream` unless explicitly configured, providing billing and resource consumption details.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_35

LANGUAGE: TypeScript
CODE:
```
.on('totalUsage', (usage: CompletionUsage) => …)
```

----------------------------------------

TITLE: Configuring `tsconfig.json` for Node.js (JSON)
DESCRIPTION: This `tsconfig.json` configuration is for TypeScript projects running in Node.js environments. It specifies the ECMAScript target version, which helps resolve type errors related to Node.js features.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_32

LANGUAGE: JSON
CODE:
```
{
  "target": "ES2018"
}
```

----------------------------------------

TITLE: Deleting a Vector Store File - Node.js
DESCRIPTION: This method deletes a specific file from a Vector Store. It takes vectorStoreId and fileId, returning a VectorStoreFileDeleted object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_39

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.del(vectorStoreId, fileId)
```

----------------------------------------

TITLE: Listing Vector Store Files - Node.js
DESCRIPTION: This method lists files within a specified Vector Store, optionally filtered by parameters. It returns a paginated VectorStoreFilesPage object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_38

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.list(vectorStoreId, { ...params })
```

----------------------------------------

TITLE: Uploading and Polling File to Vector Store - Node.js
DESCRIPTION: This method uploads a file to a Vector Store and polls its status until completion. It returns a Promise resolving to a VectorStoreFile.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_44

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.uploadAndPoll(vectorStoreId, file, options?)
```

----------------------------------------

TITLE: Creating a Message with OpenAI Node.js Client
DESCRIPTION: This method creates a new message within a specified thread. It requires a `threadID` and an object containing message parameters. It returns a `Message` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_81

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.messages.create(threadID, { ...params })
```

----------------------------------------

TITLE: Creating Transcription Session with OpenAI Node.js Client
DESCRIPTION: This method initiates a new transcription session. It accepts various parameters for transcription settings and returns a `TranscriptionSession` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_54

LANGUAGE: TypeScript
CODE:
```
client.beta.realtime.transcriptionSessions.create({ ...params })
```

----------------------------------------

TITLE: Receiving Final Chat Message - OpenAI Node.js
DESCRIPTION: This event fires for the last message in the chat completion stream, providing the complete message object, regardless of its role or content.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_30

LANGUAGE: TypeScript
CODE:
```
.on('finalMessage', (message: ChatCompletionMessage) => …)
```

----------------------------------------

TITLE: Configuring OpenAI Client with fetchOptions for Proxies in TypeScript
DESCRIPTION: The `httpAgent` client option has been removed. This snippet demonstrates the new approach using `fetchOptions` with `undici.ProxyAgent` for configuring network agents, such as for proxy support, aligning with standard `fetch` API implementations.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';
import http from 'http';
import { HttpsProxyAgent } from 'https-proxy-agent';

// Configure the default for all requests:
const client = new OpenAI({
  httpAgent: new HttpsProxyAgent(process.env.PROXY_URL),
});
```

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';
import * as undici from 'undici';

const proxyAgent = new undici.ProxyAgent(process.env.PROXY_URL);
const client = new OpenAI({
  fetchOptions: {
    dispatcher: proxyAgent,
  },
});
```

----------------------------------------

TITLE: Creating and Streaming a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Initiates a new run and streams events related to its progress. This method requires a `threadId` and a `body` for the run creation, with optional `options`. It returns an `AssistantStream` object for real-time updates.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_74

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.createAndStream(threadId, body, options?)
```

----------------------------------------

TITLE: Retrieving a Message with OpenAI Node.js Client
DESCRIPTION: This method retrieves a specific message by its ID from a thread. It requires a `messageID` and optionally additional parameters. It returns a `Message` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_82

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.messages.retrieve(messageID, { ...params })
```

----------------------------------------

TITLE: Creating and Polling a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Creates a new run and then continuously polls its status until it completes or fails. This method requires a `threadId` and a `body` for the run creation, with optional `options`. It returns a `Promise` that resolves to the final `Run` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_73

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.createAndPoll(threadId, body, options?)
```

----------------------------------------

TITLE: Creating a Vector Store File - Node.js
DESCRIPTION: This method creates a new file within a specified Vector Store. It requires the vectorStoreId and file parameters, returning a VectorStoreFile object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_35

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.create(vectorStoreId, { ...params })
```

----------------------------------------

TITLE: Retrieving a File with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method retrieves a specific file by its ID from the OpenAI API. It takes the `fileID` as a string parameter. It returns a `FileObject` containing the details of the requested file.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
client.files.retrieve(fileID)
```

----------------------------------------

TITLE: Polling a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Continuously polls the status of an existing run until it reaches a terminal state (completed, failed, etc.). This method requires a `threadId` and `runId`, with optional `options`. It returns a `Promise` that resolves to the final `Run` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_75

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.poll(threadId, runId, options?)
```

----------------------------------------

TITLE: Creating Speech from Text with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method converts text into natural-sounding speech using the OpenAI API. It accepts an object of parameters (`params`) including the text and speech generation options. It returns a `Response` object, typically an audio stream, containing the generated speech.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_11

LANGUAGE: TypeScript
CODE:
```
client.audio.speech.create({ ...params })
```

----------------------------------------

TITLE: Aborting OpenAI Chat Completion on Function Call (TypeScript)
DESCRIPTION: This example demonstrates how to prematurely abort an OpenAI chat completion run when a specific function call is triggered. By calling `runner.abort()` within the tool's function implementation, the process is stopped, and the final function call can be retrieved, useful for workflows that conclude upon a certain tool invocation.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_39

LANGUAGE: typescript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI();

async function main() {
  const runner = client.chat.completions
    .runTools({
      model: 'gpt-3.5-turbo',
      messages: [{ role: 'user', content: "How's the weather this week in Los Angeles?" }],
      tools: [
        {
          type: 'function',
          function: {
            function: function updateDatabase(props, runner) {
              runner.abort()
            },
            …
          }
        },
      ],
    })
    .on('message', (message) => console.log(message));

  const finalFunctionCall = await runner.finalFunctionCall();
  console.log('Final function call:', finalFunctionCall);
}

main();
```

----------------------------------------

TITLE: Listing Assistants with OpenAI Node.js Client
DESCRIPTION: This method retrieves a paginated list of assistants. It can accept optional parameters for filtering or pagination and returns an `AssistantsPage` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_58

LANGUAGE: TypeScript
CODE:
```
client.beta.assistants.list({ ...params })
```

----------------------------------------

TITLE: Configuring Per-Request Timeout for OpenAI Client in TypeScript
DESCRIPTION: This snippet shows how to override the default timeout setting for a specific API request. By providing a `timeout` value in the request options, you can customize the waiting period for individual calls, allowing for more flexible control over request duration.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_15

LANGUAGE: TypeScript
CODE:
```
// Override per-request:
await client.chat.completions.create({ messages: [{ role: 'user', content: 'How can I list all files in a directory using Python?' }], model: 'gpt-4o' }, {
  timeout: 5 * 1000,
});
```

----------------------------------------

TITLE: Polling Vector Store File Status in Node.js
DESCRIPTION: This asynchronous method allows for polling the status of an existing file within a vector store until it reaches a terminal state. It requires the `vectorStoreId` and `fileId` of the file to be polled, along with optional `options`. The method returns a `Promise` that resolves to a `VectorStoreFile` object once the file processing is complete.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_50

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.poll(vectorStoreId, fileId, options?) -> Promise<VectorStoreFile>
```

----------------------------------------

TITLE: Polling Vector Store File Status - Node.js
DESCRIPTION: This method polls the status of a specific file within a Vector Store until it's processed. It returns a Promise resolving to a VectorStoreFile.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_42

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.poll(vectorStoreId, fileId, options?)
```

----------------------------------------

TITLE: Retrieving Thread with OpenAI Node.js Client
DESCRIPTION: This method retrieves an existing thread by its ID. It requires the `threadID` and returns the `Thread` object if found.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_61

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.retrieve(threadID)
```

----------------------------------------

TITLE: Importing OpenAI Resource Classes in TypeScript/CommonJS
DESCRIPTION: This snippet clarifies the updated method for accessing resource classes like `Completions`. Direct import from the package root is no longer supported; instead, they must be referenced as static properties of the `OpenAI` client or imported directly from their specific resource files.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
// Before
const { Completions } = require('openai');
```

LANGUAGE: JavaScript
CODE:
```
// After
const { OpenAI } = require('openai');
OpenAI.Completions; // or import directly from openai/resources/completions
```

----------------------------------------

TITLE: Subscribing to Stream End Event in OpenAI Node.js
DESCRIPTION: This event listener is triggered as the final event when a streaming operation concludes. It can be used for cleanup or to signal the completion of the stream.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_12

LANGUAGE: ts
CODE:
```
.on('end', () => ...)
```

----------------------------------------

TITLE: Creating Fine-Tuning Job in OpenAI Node.js
DESCRIPTION: This method initiates a new fine-tuning job. It requires a `params` object containing configuration details for the job and returns a `FineTuningJob` object upon successful creation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_16

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.create({ ...params })
```

----------------------------------------

TITLE: Updating Assistant with OpenAI Node.js Client
DESCRIPTION: This method updates an existing assistant identified by its ID. It takes the `assistantID` and an object of parameters to modify, returning the updated `Assistant` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_57

LANGUAGE: TypeScript
CODE:
```
client.beta.assistants.update(assistantID, { ...params })
```

----------------------------------------

TITLE: Retrieving Assistant with OpenAI Node.js Client
DESCRIPTION: This method retrieves an existing assistant by its ID. It requires the `assistantID` as a parameter and returns the `Assistant` object if found.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_56

LANGUAGE: TypeScript
CODE:
```
client.beta.assistants.retrieve(assistantID)
```

----------------------------------------

TITLE: Retrieving a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Fetches the details of a specific run by its ID. This method requires a `runID` and can take optional `params`. It returns a `Run` object containing the run's current state and information.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_68

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.retrieve(runID, { ...params })
```

----------------------------------------

TITLE: Aborting Streaming Request - OpenAI Node.js
DESCRIPTION: This method aborts the runner and the underlying streaming request, equivalent to calling `.controller.abort()`. Invoking `.abort()` on a `ChatCompletionStreamingRunner` will also cancel any in-flight network requests, stopping data transfer immediately.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_37

LANGUAGE: TypeScript
CODE:
```
.abort()
```

----------------------------------------

TITLE: Listing Runs - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Retrieves a paginated list of runs associated with a specific thread. This method requires a `threadID` and can accept `params` for filtering or pagination. It returns a `RunsPage` object containing the list of runs.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_70

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.list(threadID, { ...params })
```

----------------------------------------

TITLE: Configuring Default Retry Behavior for OpenAI Client in JavaScript
DESCRIPTION: This snippet demonstrates how to configure the default number of retries for all API requests made through the OpenAI client. Setting `maxRetries` to `0` disables automatic retries for connection errors, timeouts, and server errors.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
// Configure the default for all requests:
const client = new OpenAI({
  maxRetries: 0, // default is 2
});
```

----------------------------------------

TITLE: Listing Files in Vector Store Batch in Node.js
DESCRIPTION: This method retrieves a paginated list of files associated with a specific file batch in a vector store. It takes the `batchID` as a mandatory argument and supports optional `params` for pagination or filtering. The method returns a `VectorStoreFilesPage` object, which includes an array of files and pagination metadata.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_48

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.fileBatches.listFiles(batchID, { ...params }) -> VectorStoreFilesPage
```

----------------------------------------

TITLE: Subscribing to All Raw Assistant Stream Events in OpenAI Node.js
DESCRIPTION: This snippet shows how to subscribe to all possible raw events sent by the OpenAI streaming API using the generic 'event' listener. While comprehensive, more specific event listeners are often more convenient for targeted use cases.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_6

LANGUAGE: ts
CODE:
```
.on('event', (event: AssistantStreamEvent) => ...)
```

----------------------------------------

TITLE: Uploading File to Vector Store - Node.js
DESCRIPTION: This method uploads a file to a specified Vector Store. It returns a Promise resolving to a VectorStoreFile upon successful upload.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_43

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.upload(vectorStoreId, file, options?)
```

----------------------------------------

TITLE: Content Generation Completion - OpenAI Node.js
DESCRIPTION: This event fires when the content generation process is complete. The `props` object contains the `content` (full generated content) and `parsed` (fully parsed content if applicable), signaling the end of content streaming.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_19

LANGUAGE: TypeScript
CODE:
```
.on('content.done', (props: ContentDoneEvent<ParsedT>) => ...)
```

----------------------------------------

TITLE: Creating a Batch with OpenAI Node.js Client
DESCRIPTION: This method creates a new batch processing job. It requires an object containing batch parameters. It returns a `Batch` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_86

LANGUAGE: TypeScript
CODE:
```
client.batches.create({ ...params })
```

----------------------------------------

TITLE: Updating Thread with OpenAI Node.js Client
DESCRIPTION: This method updates an existing thread identified by its ID. It takes the `threadID` and an object of parameters to modify, returning the updated `Thread` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_62

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.update(threadID, { ...params })
```

----------------------------------------

TITLE: Deleting Assistant with OpenAI Node.js Client
DESCRIPTION: This method deletes an assistant by its ID. It requires the `assistantID` and returns an `AssistantDeleted` object indicating success.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_59

LANGUAGE: TypeScript
CODE:
```
client.beta.assistants.delete(assistantID)
```

----------------------------------------

TITLE: Passing Custom Fetch Function to OpenAI Node.js Client (JavaScript)
DESCRIPTION: This example shows how to provide a custom `fetch` function directly to the OpenAI Node.js client during instantiation. This allows for specific control over the network layer for a particular client instance, overriding the global `fetch` or default behavior without affecting other parts of the application.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_28

LANGUAGE: JavaScript
CODE:
```
import OpenAI from 'openai';
import fetch from 'my-fetch';

const client = new OpenAI({ fetch });
```

----------------------------------------

TITLE: Listing Run Steps - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Retrieves a paginated list of steps for a specific run. This method requires a `runID` and can accept `params` for filtering or pagination. It returns a `RunStepsPage` object containing the list of run steps.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_80

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.steps.list(runID, { ...params })
```

----------------------------------------

TITLE: Migrating `asResponse` Body Handling in OpenAI SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to adapt code that accesses the response body after migrating to the new OpenAI SDK. Previously, `res.body` was a `node:stream.Readable`, but it is now a Web `ReadableStream`. To pipe the Web `ReadableStream` to `process.stdout`, it must first be converted using `Readable.fromWeb` from `node:stream`.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
// Before:
const res = await client.example.retrieve('string/with/slash').asResponse();
res.body.pipe(process.stdout);
```

LANGUAGE: TypeScript
CODE:
```
// After:
import { Readable } from 'node:stream';
const res = await client.example.retrieve('string/with/slash').asResponse();
Readable.fromWeb(res.body).pipe(process.stdout);
```

----------------------------------------

TITLE: Listing Vector Stores - Node.js
DESCRIPTION: This method lists all Vector Stores, optionally filtered by parameters. It returns a paginated VectorStoresPage object containing the list of Vector Stores.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_32

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.list({ ...params })
```

----------------------------------------

TITLE: Creating Realtime Session with OpenAI Node.js Client
DESCRIPTION: This method creates a new real-time session. It requires parameters for session configuration and returns a `SessionCreateResponse` object upon successful creation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_53

LANGUAGE: TypeScript
CODE:
```
client.beta.realtime.sessions.create({ ...params })
```

----------------------------------------

TITLE: Configuring Log Level for OpenAI Node.js Client (TypeScript)
DESCRIPTION: This snippet demonstrates how to set the logging level for the OpenAI Node.js client during initialization. By setting `logLevel` to `'debug'`, all log messages, including debug, info, warnings, and errors, will be displayed. This option overrides the `OPENAI_LOG` environment variable if both are set.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_23

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI({
  logLevel: 'debug', // Show all log messages
});
```

----------------------------------------

TITLE: Renaming OpenAI Client Delete Methods in TypeScript
DESCRIPTION: This change addresses an internal naming conflict by renaming `del()` methods to `delete()` across various client modules, improving method naming intuition and consistency.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
// Before
client.chat.completions.del();
client.files.del();
client.models.del();
client.fineTuning.checkpoints.permissions.del();
client.vectorStores.del();
client.vectorStores.files.del();
client.beta.assistants.del();
client.beta.threads.del();
client.beta.threads.messages.del();
client.responses.del();
client.evals.del();
client.evals.runs.del();
client.containers.del();
client.containers.files.del();
```

LANGUAGE: TypeScript
CODE:
```
// After
client.chat.completions.delete();
client.files.delete();
client.models.delete();
client.fineTuning.checkpoints.permissions.delete();
client.vectorStores.delete();
client.vectorStores.files.delete();
client.beta.assistants.delete();
client.beta.threads.delete();
client.beta.threads.messages.delete();
client.responses.delete();
client.evals.delete();
client.evals.runs.delete();
client.containers.delete();
client.containers.files.delete();
```

----------------------------------------

TITLE: Updating a Vector Store - Node.js
DESCRIPTION: This method updates an existing Vector Store. It requires the vectorStoreID and an object of update parameters, returning the updated VectorStore object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_31

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.update(vectorStoreID, { ...params })
```

----------------------------------------

TITLE: Migrating Positional to Named Path Parameters in OpenAI SDK (TypeScript)
DESCRIPTION: This snippet illustrates the change in how multiple path parameters are handled. Previously, all parameters were positional. Now, only the last path parameter is positional, and preceding parameters must be passed as named arguments within an object for improved clarity and to prevent argument order errors.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
// Before
client.parents.children.retrieve('p_123', 'c_456');
```

LANGUAGE: TypeScript
CODE:
```
// After
client.parents.children.retrieve('c_456', { parent_id: 'p_123' });
```

----------------------------------------

TITLE: Deleting a Fine-tuned Model with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method deletes a fine-tuned model by its ID from the OpenAI API. It takes the `model` ID as a string parameter. It returns a `ModelDeleted` object indicating the success of the deletion.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_15

LANGUAGE: TypeScript
CODE:
```
client.models.delete(model)
```

----------------------------------------

TITLE: Receiving Function Tool Call Argument Deltas - OpenAI Node.js
DESCRIPTION: This event fires when a chunk contains part of a function tool call's arguments. The `props` object provides `name`, `index`, `arguments` (accumulated raw JSON string), `parsed_arguments` (partially parsed object), and `arguments_delta` (new JSON fragment).
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_22

LANGUAGE: TypeScript
CODE:
```
.on('tool_calls.function.arguments.delta', (props: FunctionToolCallArgumentsDeltaEvent) => ...)
```

----------------------------------------

TITLE: Adding Node.js Type Definitions to `package.json` (JSON)
DESCRIPTION: This `package.json` snippet shows how to add the `@types/node` package as a dev dependency. This is crucial for providing correct type definitions for Node.js APIs, preventing type errors in TypeScript projects.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_33

LANGUAGE: JSON
CODE:
```
{
  "devDependencies": {
    "@types/node": ">= 20"
  }
}
```

----------------------------------------

TITLE: Retrieving a Vector Store - Node.js
DESCRIPTION: This method retrieves a specific Vector Store by its ID. It takes the vectorStoreID as a parameter and returns a VectorStore object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_30

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.retrieve(vectorStoreID)
```

----------------------------------------

TITLE: Retrieving a Vector Store File - Node.js
DESCRIPTION: This method retrieves a specific file from a Vector Store. It takes vectorStoreId and fileId, returning a VectorStoreFile object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_36

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.retrieve(vectorStoreId, fileId)
```

----------------------------------------

TITLE: Creating and Polling Vector Store File - Node.js
DESCRIPTION: This method creates a new file in a Vector Store and polls its status until completion. It returns a Promise resolving to a VectorStoreFile.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_41

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.createAndPoll(vectorStoreId, body, options?)
```

----------------------------------------

TITLE: Accessing Current Context in Assistant Streams (TypeScript)
DESCRIPTION: These methods on the assistant streaming object allow access to the current event, run, message snapshot, or run step snapshot from within event handlers. They provide additional context beyond what's directly available in the handler's arguments, returning `undefined` if no relevant context exists.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_13

LANGUAGE: TypeScript
CODE:
```
.currentEvent(): AssistantStreamEvent | undefined

.currentRun(): Run | undefined

.currentMessageSnapshot(): Message

.currentRunStepSnapshot(): Runs.RunStep
```

----------------------------------------

TITLE: Deleting Thread with OpenAI Node.js Client
DESCRIPTION: This method deletes a thread by its ID. It requires the `threadID` and returns a `ThreadDeleted` object indicating success.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_63

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.delete(threadID)
```

----------------------------------------

TITLE: Listing Fine-Tuning Job Checkpoints in OpenAI Node.js
DESCRIPTION: This method retrieves a paginated list of checkpoints for a specific fine-tuning job. It requires the `fineTuningJobID` and accepts an optional `params` object, returning a `FineTuningJobCheckpointsPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_23

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.checkpoints.list(fineTuningJobID, { ...params })
```

----------------------------------------

TITLE: Listing Files with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method lists all files uploaded to the OpenAI API, with optional filtering parameters. It accepts an object of parameters (`params`) for pagination or filtering. It returns a `FileObjectsPage` containing a list of `FileObject` instances.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
client.files.list({ ...params })
```

----------------------------------------

TITLE: Subscribing to Image File Completion Events in OpenAI Node.js
DESCRIPTION: This event listener is triggered when an ImageFile content type becomes available, as image files are not sent incrementally. It provides the complete image file content upon completion.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_10

LANGUAGE: ts
CODE:
```
.on('imageFileDone', (content: ImageFile, snapshot: Message) => ...)
```

----------------------------------------

TITLE: Configuring `tsconfig.json` for Cloudflare Workers (JSON)
DESCRIPTION: This `tsconfig.json` configuration is tailored for TypeScript projects deployed on Cloudflare Workers. It includes specific `lib` and `types` settings to ensure compatibility and correct type checking for the Workers environment.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_34

LANGUAGE: JSON
CODE:
```
{
  "target": "ES2018",
  "lib": ["ES2020"],
  "types": ["@cloudflare/workers-types"]
}
```

----------------------------------------

TITLE: Creating Vector Store File Batch in Node.js
DESCRIPTION: This method initiates the creation of a new file batch within a specified vector store. It requires the `vectorStoreID` and an object of `params` which typically include the file IDs to be added to the batch. The operation returns a `VectorStoreFileBatch` object representing the newly created batch.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_45

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.fileBatches.create(vectorStoreID, { ...params }) -> VectorStoreFileBatch
```

----------------------------------------

TITLE: Zod Response Format with Optional Property (Before Error Fix) (TypeScript)
DESCRIPTION: This snippet shows the previous usage of `zodResponseFormat` where an optional property was defined without `.nullable()`. This pattern now throws an error because purely optional fields are not supported by the OpenAI API's structured outputs.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_14

LANGUAGE: ts
CODE:
```
const completion = await client.chat.completions.parse({
  // ...
  response_format: zodResponseFormat(
    z.object({
      optional_property: z.string().optional(),
    }),
    'schema',
  ),
});
```

----------------------------------------

TITLE: Polyfilling Global Fetch Function for OpenAI Node.js Client (JavaScript)
DESCRIPTION: This snippet demonstrates how to polyfill the global `fetch` function, which the OpenAI Node.js client uses by default. By assigning a custom `fetch` implementation to `globalThis.fetch`, all subsequent client requests will use this custom function, allowing for global customization of network behavior.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_27

LANGUAGE: JavaScript
CODE:
```
import fetch from 'my-fetch';

globalThis.fetch = fetch;
```

----------------------------------------

TITLE: Updating a Vector Store File - Node.js
DESCRIPTION: This method updates an existing file within a Vector Store. It requires vectorStoreId, fileId, and update parameters, returning the updated VectorStoreFile.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_37

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.update(vectorStoreId, fileId, { ...params })
```

----------------------------------------

TITLE: Listing Batches with OpenAI Node.js Client
DESCRIPTION: This method lists existing batch processing jobs. It supports pagination/filtering parameters. It returns a `BatchesPage` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_88

LANGUAGE: TypeScript
CODE:
```
client.batches.list({ ...params })
```

----------------------------------------

TITLE: Deleting a Message with OpenAI Node.js Client
DESCRIPTION: This method deletes a specific message by its ID from a thread. It requires a `messageID` and returns a `MessageDeleted` confirmation object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_85

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.messages.delete(messageID, { ...params })
```

----------------------------------------

TITLE: Deleting a File with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method deletes a specific file by its ID from the OpenAI API. It takes the `fileID` as a string parameter. It returns a `FileDeleted` object indicating the success of the deletion.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
client.files.delete(fileID)
```

----------------------------------------

TITLE: Importing Uploadable and toFile from OpenAI Core Uploads (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the `Uploadable` type and the `toFile` utility function, which are still exported from `openai/core/uploads` after the `core` refactor. These are the only remaining public exports from the former `openai/uploads` module.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { type Uploadable, toFile } from 'openai/core/uploads';
```

----------------------------------------

TITLE: Retrieving Fine-Tuning Job in OpenAI Node.js
DESCRIPTION: This method fetches the details of a specific fine-tuning job. It takes the `fineTuningJobID` as an argument and returns a `FineTuningJob` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_17

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.retrieve(fineTuningJobID)
```

----------------------------------------

TITLE: Resuming Fine-Tuning Job in OpenAI Node.js
DESCRIPTION: This method resumes a paused fine-tuning job. It takes the `fineTuningJobID` as an argument and returns the updated `FineTuningJob` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_22

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.resume(fineTuningJobID)
```

----------------------------------------

TITLE: Canceling Vector Store File Batch in Node.js
DESCRIPTION: This method is used to cancel an ongoing or pending file batch operation within a vector store. It requires the `batchID` of the batch to be canceled and can include optional `params`. Upon successful cancellation, it returns the updated `VectorStoreFileBatch` object with its status reflecting the cancellation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_47

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.fileBatches.cancel(batchID, { ...params }) -> VectorStoreFileBatch
```

----------------------------------------

TITLE: Retrieving Vector Store File Batch in Node.js
DESCRIPTION: This method allows for the retrieval of a specific file batch using its unique `batchID` from a given vector store. It can also accept optional `params` for additional filtering or configuration. The function returns a `VectorStoreFileBatch` object containing details about the requested batch.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_46

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.fileBatches.retrieve(batchID, { ...params }) -> VectorStoreFileBatch
```

----------------------------------------

TITLE: Accessing Request ID from OpenAI Response (TypeScript)
DESCRIPTION: This snippet demonstrates how to retrieve the `_request_id` property directly from an OpenAI API response object, which is useful for debugging and reporting issues to OpenAI. The `_request_id` is derived from the `x-request-id` response header.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_16

LANGUAGE: TypeScript
CODE:
```
const response = await client.responses.create({ model: 'gpt-4o', input: 'testing 123' });
console.log(response._request_id); // req_123
```

----------------------------------------

TITLE: Configuring Proxy with Undici for Node.js
DESCRIPTION: This snippet demonstrates how to configure a proxy for the OpenAI client in Node.js using `undici.ProxyAgent`. It requires importing `undici` and setting the `dispatcher` property within `fetchOptions` to an instance of `ProxyAgent` initialized with the proxy URL.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_30

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';
import * as undici from 'undici';

const proxyAgent = new undici.ProxyAgent('http://localhost:8888');
const client = new OpenAI({
  fetchOptions: {
    dispatcher: proxyAgent,
  },
});
```

----------------------------------------

TITLE: Updating OpenAI API List Method Calls in JavaScript
DESCRIPTION: This snippet illustrates the updated requirement for calling list methods without body, query, or header parameters. You must now explicitly pass `null`, `undefined`, or an empty object `{}` as the first argument to allow for custom request options.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
client.example.list();
client.example.list({}, { headers: { ... } });
client.example.list(null, { headers: { ... } });
client.example.list(undefined, { headers: { ... } });
```

----------------------------------------

TITLE: Canceling a Batch with OpenAI Node.js Client
DESCRIPTION: This method cancels a specific batch processing job by its ID. It requires a `batchID`. It returns the canceled `Batch` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_89

LANGUAGE: TypeScript
CODE:
```
client.batches.cancel(batchID)
```

----------------------------------------

TITLE: Configuring Proxy with fetchOptions for Bun
DESCRIPTION: This snippet shows how to configure a proxy for the OpenAI client when running in Bun. It directly utilizes the `proxy` property within `fetchOptions`, setting its value to the desired proxy URL.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_31

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI({
  fetchOptions: {
    proxy: 'http://localhost:8888',
  },
});
```

----------------------------------------

TITLE: Deprecated runFunctions Method (Before Removal) (TypeScript)
DESCRIPTION: This snippet refers to the `client.chat.completions.runFunctions()` method, which has been deprecated and subsequently removed from the library. Users should migrate to the `runTools()` method for similar functionality.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_25

LANGUAGE: ts
CODE:
```
client.chat.completions.runFunctions()
```

----------------------------------------

TITLE: Editing an Image with OpenAI Node.js Client (TypeScript)
DESCRIPTION: This method edits an image based on a text prompt and an optional mask using the OpenAI API. It accepts an object of parameters (`params`) including the image, prompt, and mask. It returns an `ImagesResponse` containing the edited image.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_7

LANGUAGE: TypeScript
CODE:
```
client.images.edit({ ...params })
```

----------------------------------------

TITLE: Migrating URI Encoding of Path Parameters in OpenAI SDK (TypeScript)
DESCRIPTION: This snippet shows the change in handling URI encoding for path parameters. The SDK now automatically encodes path parameters. Developers should no longer manually apply `encodeURIComponent` to parameters, as doing so would result in double encoding and incorrect paths.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
client.example.retrieve(encodeURIComponent('string/with/slash'))
```

LANGUAGE: TypeScript
CODE:
```
client.example.retrieve('string/with/slash') // retrieves /example/string%2Fwith%2Fslash
```

----------------------------------------

TITLE: Configuring Proxy with Deno.createHttpClient for Deno
DESCRIPTION: This snippet illustrates how to set up a proxy for the OpenAI client in Deno. It involves creating an `httpClient` using `Deno.createHttpClient` with the proxy URL, and then assigning this client to the `fetchOptions.client` property.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_32

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'npm:openai';

const httpClient = Deno.createHttpClient({ proxy: { url: 'http://localhost:8888' } });
const client = new OpenAI({
  fetchOptions: {
    client: httpClient,
  },
});
```

----------------------------------------

TITLE: Importing OpenAI from Root Path (TypeScript)
DESCRIPTION: This snippet shows the corrected and recommended way to import the OpenAI library directly from the `openai` package root. This resolves issues related to the removal of the `openai/src` directory.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_30

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai';
```

----------------------------------------

TITLE: Deleting a Vector Store - Node.js
DESCRIPTION: This method deletes a specific Vector Store by its ID. It takes the vectorStoreID and returns a VectorStoreDeleted object indicating success.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_33

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.delete(vectorStoreID)
```

----------------------------------------

TITLE: Accessing Beta Chat Completions (Before Namespace Removal) (TypeScript)
DESCRIPTION: This code demonstrates the previous way to access chat completion methods like `parse()`, `stream()`, and `runTools()` through the `beta.chat` namespace. This namespace has been removed.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_21

LANGUAGE: ts
CODE:
```
client.beta.chat.completions.parse()
client.beta.chat.completions.stream()
client.beta.chat.completions.runTools()
```

----------------------------------------

TITLE: Listing Containers with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to retrieve a paginated list of containers using the OpenAI Node.js client. It accepts optional `params` for filtering or pagination and returns a `ContainerListResponsesPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_108

LANGUAGE: TypeScript
CODE:
```
client.containers.list({ ...params })
```

----------------------------------------

TITLE: Importing OpenAI from JSR in Deno
DESCRIPTION: This import statement demonstrates how to directly import the OpenAI library from JSR in a Deno environment without a prior installation step, leveraging Deno's native JSR specifier support.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'jsr:@openai/openai';
```

----------------------------------------

TITLE: Creating an Upload with OpenAI Node.js Client
DESCRIPTION: This method initiates a new file upload. It requires an object containing upload parameters. It returns an `Upload` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_90

LANGUAGE: TypeScript
CODE:
```
client.uploads.create({ ...params })
```

----------------------------------------

TITLE: Retrieving Vector Store File Content - Node.js
DESCRIPTION: This method retrieves the content of a specific file from a Vector Store. It requires vectorStoreId and fileId, returning a FileContentResponsesPage.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_40

LANGUAGE: TypeScript
CODE:
```
client.vectorStores.files.content(vectorStoreId, fileId)
```

----------------------------------------

TITLE: Listing Output Items with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to retrieve a paginated list of output items for a specific eval run using the OpenAI Node.js client. It requires the `runID` and accepts optional `params` for filtering, returning an `OutputItemListResponsesPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_105

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.outputItems.list(runID, { ...params })
```

----------------------------------------

TITLE: Installing OpenAI Library from JSR for Deno
DESCRIPTION: These commands install the OpenAI library specifically for Deno projects using JSR (JavaScript Registry). `deno add` is for Deno's native package management, while `npx jsr add` uses `npx` to add the JSR package.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_1

LANGUAGE: Shell
CODE:
```
deno add jsr:@openai/openai
```

LANGUAGE: Shell
CODE:
```
npx jsr add @openai/openai
```

----------------------------------------

TITLE: Configuring `tsconfig.json` for Browsers (JSON)
DESCRIPTION: This `tsconfig.json` configuration is recommended for TypeScript projects targeting browser environments. It sets the compilation target and includes necessary DOM and iterable libraries to resolve type errors related to browser-specific features.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_31

LANGUAGE: JSON
CODE:
```
{
  "target": "ES2018",
  "lib": ["DOM", "DOM.Iterable", "ES2018"]
}
```

----------------------------------------

TITLE: Using Undocumented Request Parameters in OpenAI Node.js Client (TypeScript)
DESCRIPTION: This example shows how to include undocumented parameters in API requests by using `// @ts-expect-error` to bypass TypeScript's type checking. The library does not perform runtime validation, so any extra values provided will be sent directly to the API. For `GET` requests, extra parameters are sent as query parameters; otherwise, they are included in the request body.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_26

LANGUAGE: TypeScript
CODE:
```
client.foo.create({
  foo: 'my_param',
  bar: 12,
  // @ts-expect-error baz is not yet public
  baz: 'undocumented option',
});
```

----------------------------------------

TITLE: Retrieving a Run Step - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Fetches the details of a specific step within a run. This method requires a `stepID` and can take optional `params`. It returns a `RunStep` object containing the step's information.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_79

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.steps.retrieve(stepID, { ...params })
```

----------------------------------------

TITLE: Making Undocumented POST Requests with OpenAI Node.js Client (JavaScript)
DESCRIPTION: This snippet demonstrates how to make requests to undocumented API endpoints using the `client.post` method. It allows specifying a request body and query parameters, and client-level options like retries will be automatically applied. This is useful for interacting with non-public or experimental API features.
SOURCE: https://github.com/openai/openai-node/blob/master/README.md#_snippet_25

LANGUAGE: JavaScript
CODE:
```
await client.post('/some/path', {
  body: { some_prop: 'foo' },
  query: { some_query_arg: 'bar' },
});
```

----------------------------------------

TITLE: Completing an Upload with OpenAI Node.js Client
DESCRIPTION: This method marks a file upload as complete, making the uploaded file available. It requires an `uploadID` and an object containing completion parameters. It returns the completed `Upload` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_92

LANGUAGE: TypeScript
CODE:
```
client.uploads.complete(uploadID, { ...params })
```

----------------------------------------

TITLE: Receiving Refusal Deltas - OpenAI Node.js
DESCRIPTION: This event fires when a chunk contains part of a content refusal. The `props` object includes the `delta` (new refusal content string) and `snapshot` (accumulated refusal content string), allowing for real-time refusal tracking.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_20

LANGUAGE: TypeScript
CODE:
```
.on('refusal.delta', (props: RefusalDeltaEvent) => ...)
```

----------------------------------------

TITLE: Configuring `tsconfig.json` for Bun (JSON)
DESCRIPTION: This `tsconfig.json` configuration is for TypeScript projects running with Bun. It sets the ECMAScript target version, which is generally sufficient when using Bun's built-in type support or specific `@types/bun` definitions.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_36

LANGUAGE: JSON
CODE:
```
{
  "target": "ES2018"
}
```

----------------------------------------

TITLE: Retrieving a Batch with OpenAI Node.js Client
DESCRIPTION: This method retrieves a specific batch processing job by its ID. It requires a `batchID`. It returns a `Batch` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_87

LANGUAGE: TypeScript
CODE:
```
client.batches.retrieve(batchID)
```

----------------------------------------

TITLE: Adding Bun Type Definitions to `package.json` (JSON)
DESCRIPTION: This `package.json` snippet shows how to include `@types/bun` as a dev dependency. This package provides type definitions for Bun's runtime APIs, ensuring proper type checking for TypeScript projects using Bun.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_37

LANGUAGE: JSON
CODE:
```
{
  "devDependencies": {
    "@types/bun": ">= 1.2.0"
  }
}
```

----------------------------------------

TITLE: Listing Fine-Tuning Job Events in OpenAI Node.js
DESCRIPTION: This method retrieves a paginated list of events for a specific fine-tuning job. It takes the `fineTuningJobID` and an optional `params` object, returning a `FineTuningJobEventsPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_20

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.listEvents(fineTuningJobID, { ...params })
```

----------------------------------------

TITLE: Installing Dependencies with Bun
DESCRIPTION: This command installs all project dependencies using the Bun package manager. It's a prerequisite for running the application.
SOURCE: https://github.com/openai/openai-node/blob/master/ecosystem-tests/bun/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
bun install
```

----------------------------------------

TITLE: Canceling an Upload with OpenAI Node.js Client
DESCRIPTION: This method cancels an ongoing file upload by its ID. It requires an `uploadID`. It returns the canceled `Upload` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_91

LANGUAGE: TypeScript
CODE:
```
client.uploads.cancel(uploadID)
```

----------------------------------------

TITLE: Creating an Eval with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to create a new evaluation (eval) using the OpenAI Node.js client. It accepts an object of `params` to define the eval's configuration and returns an `EvalCreateResponse` upon successful creation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_94

LANGUAGE: TypeScript
CODE:
```
client.evals.create({ ...params })
```

----------------------------------------

TITLE: Adding Cloudflare Workers Type Definitions to `package.json` (JSON)
DESCRIPTION: This `package.json` snippet demonstrates adding `@cloudflare/workers-types` as a dev dependency. This package provides essential type definitions for Cloudflare Workers APIs, resolving type errors in TypeScript projects.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_35

LANGUAGE: JSON
CODE:
```
{
  "devDependencies": {
    "@cloudflare/workers-types": ">= 0.20221111.0"
  }
}
```

----------------------------------------

TITLE: Exporting Page Type Alias (After Class Removal) (TypeScript)
DESCRIPTION: This snippet illustrates the new approach where page classes, like `FineTuningJobsPage`, are now defined as type aliases. If you were importing these at runtime, you should switch to importing the base class or only import them at the type-level.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_20

LANGUAGE: ts
CODE:
```
export type FineTuningJobsPage = CursorPage<FineTuningJob>;
```

----------------------------------------

TITLE: Pausing Fine-Tuning Job in OpenAI Node.js
DESCRIPTION: This method pauses an active fine-tuning job. It requires the `fineTuningJobID` to specify which job to pause and returns the updated `FineTuningJob` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_21

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.pause(fineTuningJobID)
```

----------------------------------------

TITLE: Updating a Run - OpenAI Node.js SDK - TypeScript
DESCRIPTION: Modifies an existing run. This method requires a `runID` and accepts `params` to update various properties of the run. It returns the updated `Run` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_69

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.runs.update(runID, { ...params })
```

----------------------------------------

TITLE: Refusal Log Probabilities Completion - OpenAI Node.js
DESCRIPTION: This event fires when all refusal log probabilities have been received. The `props` object contains the `refusal` (full list of token log probabilities for the refusal), indicating the completion of refusal log probability streaming.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_27

LANGUAGE: TypeScript
CODE:
```
.on('logprobs.refusal.done', (props: LogProbsRefusalDoneEvent) => ...)
```

----------------------------------------

TITLE: Receiving Content Log Probabilities Deltas - OpenAI Node.js
DESCRIPTION: This event fires when a chunk contains new content log probabilities. The `props` object provides `content` (list of new log probabilities) and `snapshot` (accumulated log probabilities), enabling real-time analysis of token probabilities.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_24

LANGUAGE: TypeScript
CODE:
```
.on('logprobs.content.delta', (props: LogProbsContentDeltaEvent) => ...)
```

----------------------------------------

TITLE: Receiving Refusal Log Probabilities Deltas - OpenAI Node.js
DESCRIPTION: This event fires when a chunk contains new refusal log probabilities. The `props` object provides `refusal` (list of new log probabilities) and `snapshot` (accumulated log probabilities), allowing for real-time tracking of refusal token probabilities.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_26

LANGUAGE: TypeScript
CODE:
```
.on('logprobs.refusal.delta', (props: LogProbsRefusalDeltaEvent) => ...)
```

----------------------------------------

TITLE: Updating a Message with OpenAI Node.js Client
DESCRIPTION: This method updates an existing message within a thread. It requires a `messageID` and an object containing update parameters. It returns the updated `Message` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_83

LANGUAGE: TypeScript
CODE:
```
client.beta.threads.messages.update(messageID, { ...params })
```

----------------------------------------

TITLE: Listing Input Items with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to retrieve a paginated list of input items associated with a specific response ID using the OpenAI Node.js client. It requires a `responseID` and accepts optional `params` for filtering or pagination. The method returns a `ResponseItemsPage` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_93

LANGUAGE: TypeScript
CODE:
```
client.responses.inputItems.list(responseID, { ...params })
```

----------------------------------------

TITLE: Cancelling Fine-Tuning Job in OpenAI Node.js
DESCRIPTION: This method cancels an ongoing fine-tuning job. It requires the `fineTuningJobID` to identify the job and returns the updated `FineTuningJob` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_19

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.jobs.cancel(fineTuningJobID)
```

----------------------------------------

TITLE: Retrieving Checkpoint Permissions in OpenAI Node.js
DESCRIPTION: This method retrieves permissions for a specific fine-tuned model checkpoint. It takes the `fineTunedModelCheckpoint` identifier and optional `params`, returning a `PermissionRetrieveResponse`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_25

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.checkpoints.permissions.retrieve(fineTunedModelCheckpoint, { ...params })
```

----------------------------------------

TITLE: Handling OpenAI Chat Completions with Old `functionCall` Events (TypeScript)
DESCRIPTION: This snippet demonstrates how to listen for `functionCall` related events using the `ChatCompletionRunner`'s `.runTools()` method before the event names were updated. It logs the `functionCall` and `functionCallResult` at various stages.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_27

LANGUAGE: TypeScript
CODE:
```
openai.chat.completions
  .runTools({
    // ..
  })
  .on('functionCall', (functionCall) => console.log('functionCall', functionCall))
  .on('functionCallResult', (functionCallResult) => console.log('functionCallResult', functionCallResult))
  .on('finalFunctionCall', (functionCall) => console.log('finalFunctionCall', functionCall))
  .on('finalFunctionCallResult', (result) => console.log('finalFunctionCallResult', result));
```

----------------------------------------

TITLE: Deleting an Eval Run with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to delete a specific run of an evaluation by its ID using the OpenAI Node.js client. It requires the `runID` and accepts optional `params`, returning a `RunDeleteResponse` upon successful deletion.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_102

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.delete(runID, { ...params })
```

----------------------------------------

TITLE: Refusal Content Completion - OpenAI Node.js
DESCRIPTION: This event fires when the refusal content generation is complete. The `props` object contains the `refusal` (full refusal content), indicating that the complete refusal message has been received.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_21

LANGUAGE: TypeScript
CODE:
```
.on('refusal.done', (props: RefusalDoneEvent) => ...)
```

----------------------------------------

TITLE: Cancelling an Eval Run with OpenAI Node.js Client
DESCRIPTION: This snippet illustrates how to cancel a specific run of an evaluation by its ID using the OpenAI Node.js client. It requires the `runID` and accepts optional `params`, returning a `RunCancelResponse` upon successful cancellation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_103

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.cancel(runID, { ...params })
```

----------------------------------------

TITLE: Creating an Eval Run with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to create a new run for a specific evaluation (eval) using the OpenAI Node.js client. It requires the `evalID` and accepts `params` to configure the run, returning a `RunCreateResponse`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_99

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.create(evalID, { ...params })
```

----------------------------------------

TITLE: Listing Eval Runs with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to retrieve a paginated list of runs for a specific evaluation using the OpenAI Node.js client. It requires the `evalID` and accepts optional `params` for filtering, returning a `RunListResponsesPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_101

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.list(evalID, { ...params })
```

----------------------------------------

TITLE: Listing Evals with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to retrieve a paginated list of evaluations (evals) using the OpenAI Node.js client. It accepts optional `params` for filtering or pagination and returns an `EvalListResponsesPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_97

LANGUAGE: TypeScript
CODE:
```
client.evals.list({ ...params })
```

----------------------------------------

TITLE: Retrieving an Eval with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to retrieve a specific evaluation (eval) by its ID using the OpenAI Node.js client. It requires the `evalID` as a parameter and returns an `EvalRetrieveResponse` containing the eval's details.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_95

LANGUAGE: TypeScript
CODE:
```
client.evals.retrieve(evalID)
```

----------------------------------------

TITLE: Content Log Probabilities Completion - OpenAI Node.js
DESCRIPTION: This event fires when all content log probabilities have been received. The `props` object contains the `content` (full list of token log probabilities for the content), indicating the completion of log probability streaming.
SOURCE: https://github.com/openai/openai-node/blob/master/helpers.md#_snippet_25

LANGUAGE: TypeScript
CODE:
```
.on('logprobs.content.done', (props: LogProbsContentDoneEvent) => ...)
```

----------------------------------------

TITLE: Updating an Eval with OpenAI Node.js Client
DESCRIPTION: This snippet illustrates how to update an existing evaluation (eval) using the OpenAI Node.js client. It requires the `evalID` and an object of `params` for the fields to be updated, returning an `EvalUpdateResponse`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_96

LANGUAGE: TypeScript
CODE:
```
client.evals.update(evalID, { ...params })
```

----------------------------------------

TITLE: Retrieving an Output Item with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to retrieve a specific output item associated with an eval run using the OpenAI Node.js client. It requires the `outputItemID` and accepts optional `params`, returning an `OutputItemRetrieveResponse`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_104

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.outputItems.retrieve(outputItemID, { ...params })
```

----------------------------------------

TITLE: Retrieving an Eval Run with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to retrieve a specific run of an evaluation by its ID using the OpenAI Node.js client. It requires the `runID` and accepts optional `params`, returning a `RunRetrieveResponse`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_100

LANGUAGE: TypeScript
CODE:
```
client.evals.runs.retrieve(runID, { ...params })
```

----------------------------------------

TITLE: Deleting an Eval with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to delete a specific evaluation (eval) by its ID using the OpenAI Node.js client. It requires the `evalID` as a parameter and returns an `EvalDeleteResponse` upon successful deletion.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_98

LANGUAGE: TypeScript
CODE:
```
client.evals.delete(evalID)
```

----------------------------------------

TITLE: Deleting Checkpoint Permissions in OpenAI Node.js
DESCRIPTION: This method deletes permissions for a fine-tuned model checkpoint. It requires the `permissionID` and optional `params`, returning a `PermissionDeleteResponse`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_26

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.checkpoints.permissions.delete(permissionID, { ...params })
```

----------------------------------------

TITLE: Exporting Page Class (Before Type Alias Change) (TypeScript)
DESCRIPTION: This code shows the previous definition of a page class, `FineTuningJobsPage`, as an exported class extending `CursorPage`. These classes have now been converted to type aliases.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_19

LANGUAGE: ts
CODE:
```
export class FineTuningJobsPage extends CursorPage<FineTuningJob> {}
```

----------------------------------------

TITLE: Validating Alpha Grader in OpenAI Node.js
DESCRIPTION: This method validates an alpha grader configuration or operation. It requires a `params` object for validation details and returns a `GraderValidateResponse` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_28

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.alpha.graders.validate({ ...params })
```

----------------------------------------

TITLE: Retrieving a Container with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to retrieve a specific container by its ID using the OpenAI Node.js client. It requires the `containerID` as a parameter and returns a `ContainerRetrieveResponse` containing the container's details.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_107

LANGUAGE: TypeScript
CODE:
```
client.containers.retrieve(containerID)
```

----------------------------------------

TITLE: Importing OpenAI from `openai/src` (TypeScript)
DESCRIPTION: This snippet illustrates the old way of importing the OpenAI library, which incorrectly referenced the internal `openai/src` directory. This import path is deprecated and should be updated.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_29

LANGUAGE: TypeScript
CODE:
```
import OpenAI from 'openai/src';
```

----------------------------------------

TITLE: Deprecated File Handling with fileFromPath (Before Change) (TypeScript)
DESCRIPTION: This code demonstrates the use of the deprecated `OpenAI.fileFromPath` helper for handling files. This helper has been removed and should no longer be used for file operations.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_11

LANGUAGE: ts
CODE:
```
OpenAI.fileFromPath('path/to/file');
```

----------------------------------------

TITLE: Creating Checkpoint Permissions in OpenAI Node.js
DESCRIPTION: This method creates new permissions for a fine-tuned model checkpoint. It requires the `fineTunedModelCheckpoint` identifier and a `params` object, returning a `PermissionCreateResponsesPage`.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_24

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.checkpoints.permissions.create(fineTunedModelCheckpoint, { ...params })
```

----------------------------------------

TITLE: Importing APIClient (Before Change) (TypeScript)
DESCRIPTION: This code shows the previous method of importing the `APIClient` base class from `openai/core`. This class has since been removed and is no longer available for direct import.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { APIClient } from 'openai/core';
```

----------------------------------------

TITLE: Deleting a Container with OpenAI Node.js Client
DESCRIPTION: This snippet demonstrates how to delete a specific container by its ID using the OpenAI Node.js client. It requires the `containerID` as a parameter and returns `void` upon successful deletion, indicating no content.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_109

LANGUAGE: TypeScript
CODE:
```
client.containers.delete(containerID)
```

----------------------------------------

TITLE: Creating a Container with OpenAI Node.js Client
DESCRIPTION: This snippet shows how to create a new container using the OpenAI Node.js client. It accepts an object of `params` to define the container's configuration and returns a `ContainerCreateResponse` upon successful creation.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_106

LANGUAGE: TypeScript
CODE:
```
client.containers.create({ ...params })
```

----------------------------------------

TITLE: Adding a TypeScript Example File
DESCRIPTION: Illustrates the initial structure for adding a new TypeScript example file within the `examples/` directory. This directory is not modified by the code generator, allowing free edits and additions.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_1

LANGUAGE: ts
CODE:
```
// add an example to examples/<your-example>.ts

#!/usr/bin/env -S npm run tsn -T
…
```

----------------------------------------

TITLE: Setting up Development Environment with Yarn
DESCRIPTION: Installs all required dependencies and builds output files for the OpenAI Node.js library. This prepares the project for development by creating `dist/` files.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_0

LANGUAGE: sh
CODE:
```
$ yarn
$ yarn build
```

----------------------------------------

TITLE: Automatically Fixing Linting Issues
DESCRIPTION: Runs the formatting and lint-fixing process using Yarn. This command automatically corrects many code style and linting issues, ensuring adherence to project standards.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_9

LANGUAGE: sh
CODE:
```
$ yarn fix
```

----------------------------------------

TITLE: Linking Local Repository with Yarn
DESCRIPTION: Outlines the steps to clone the repository and then link it locally using Yarn. This allows developers to use their local copy of the library in another project for development and testing.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_4

LANGUAGE: sh
CODE:
```
$ git clone https://www.github.com/openai/openai-node
$ cd openai-node

# With yarn
$ yarn link
$ cd ../my-package
$ yarn link openai
```

----------------------------------------

TITLE: Linking Local Repository with pnpm
DESCRIPTION: Provides commands to link a local copy of the repository globally using pnpm. This enables other projects to consume the local library for development purposes, similar to `yarn link`.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_5

LANGUAGE: sh
CODE:
```
$ pnpm link --global
$ cd ../my-package
$ pnpm link -—global openai
```

----------------------------------------

TITLE: Running Code Linting with Yarn
DESCRIPTION: Executes the linting process using Yarn to check for code style and quality issues. This helps maintain consistent code standards across the repository.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_8

LANGUAGE: sh
CODE:
```
$ yarn lint
```

----------------------------------------

TITLE: Installing Repository from Git via npm
DESCRIPTION: Demonstrates how to install the OpenAI Node.js library directly from its Git repository using npm. This method is suitable for consuming the library from source without cloning it locally first.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_3

LANGUAGE: sh
CODE:
```
$ npm install git+ssh://git@github.com/openai/openai-node.git
```

----------------------------------------

TITLE: Running a Local TypeScript Example
DESCRIPTION: Provides commands to make a new example file executable and then run it using `yarn tsn`. This allows developers to test their examples against a local API.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_2

LANGUAGE: sh
CODE:
```
$ chmod +x examples/<your-example>.ts
# run the example against your api
$ yarn tsn -T examples/<your-example>.ts
```

----------------------------------------

TITLE: Manual Pagination Interface (Before Change) (TypeScript)
DESCRIPTION: This code illustrates the previous methods for manual pagination, `page.nextPageParams()` and `page.nextPageInfo()`. These methods required manual handling of different return types, such as `{ url }` or `{ params }`.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_17

LANGUAGE: ts
CODE:
```
page.nextPageParams();
page.nextPageInfo();
```

----------------------------------------

TITLE: Importing Beta Chat Completion Types (Before Relocation) (TypeScript)
DESCRIPTION: This code shows the previous import path for chat completion types like `ParsedChatCompletion`, `ParsedChoice`, and `ParsedFunction` from `openai/resources/beta/chat/completions`. These types have been relocated.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_23

LANGUAGE: ts
CODE:
```
import { ParsedChatCompletion, ParsedChoice, ParsedFunction } from 'openai/resources/beta/chat/completions';
```

----------------------------------------

TITLE: Setting up Mock Server for Tests
DESCRIPTION: Instructs how to set up a mock server using `npx prism` against an OpenAPI specification. This mock server is required for running most of the tests in the repository.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_6

LANGUAGE: sh
CODE:
```
$ npx prism mock path/to/your/openapi.yml
```

----------------------------------------

TITLE: Executing Tests with Yarn
DESCRIPTION: Runs the test suite for the OpenAI Node.js library. This command is typically executed after setting up a mock server to ensure all tests can run successfully.
SOURCE: https://github.com/openai/openai-node/blob/master/CONTRIBUTING.md#_snippet_7

LANGUAGE: sh
CODE:
```
$ yarn run test
```

----------------------------------------

TITLE: Running the Project with Bun
DESCRIPTION: This command executes the main TypeScript file (`index.ts`) of the project using the Bun runtime. It starts the application.
SOURCE: https://github.com/openai/openai-node/blob/master/ecosystem-tests/bun/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
bun run index.ts
```

----------------------------------------

TITLE: Configuring Web Fetch Shims (Before Removal) (TypeScript)
DESCRIPTION: This code illustrates the previous method of configuring the SDK to use global Web fetch types by importing `openai/shims/web`. This shim import has been removed and is no longer supported for type configuration.
SOURCE: https://github.com/openai/openai-node/blob/master/MIGRATION.md#_snippet_13

LANGUAGE: ts
CODE:
```
import 'openai/shims/web';
import OpenAI from 'openai';
```

----------------------------------------

TITLE: Running Alpha Grader in OpenAI Node.js
DESCRIPTION: This method executes an alpha grader operation. It requires a `params` object for configuration and returns a `GraderRunResponse` object.
SOURCE: https://github.com/openai/openai-node/blob/master/api.md#_snippet_27

LANGUAGE: TypeScript
CODE:
```
client.fineTuning.alpha.graders.run({ ...params })
```