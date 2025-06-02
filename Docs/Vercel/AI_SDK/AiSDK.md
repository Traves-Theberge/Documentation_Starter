TITLE: Displaying Loading State with useCompletion Hook (React)
DESCRIPTION: This snippet demonstrates how to use the `isLoading` state returned by the `useCompletion` hook to display a loading indicator (e.g., a `Spinner` component) while the AI response is being processed, enhancing user feedback.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/05-completion.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
const { isLoading, ... } = useCompletion()

return(
  <>
    {isLoading ? <Spinner /> : null}
  </>
)
```

----------------------------------------

TITLE: Server-Side AI Conversation Logic with Token Usage Tracking (AI SDK)
DESCRIPTION: This server action (`app/actions.tsx`) implements the core AI conversation logic using `ai/rsc` and `@ai-sdk/openai`. It streams UI updates back to the client, defines a 'deploy' tool, and critically, utilizes the `onFinish` callback within `streamUI` to log `promptTokens`, `completionTokens`, and `totalTokens` for usage tracking after the AI stream completes. It depends on `createAI`, `getMutableAIState`, `streamUI`, and `openai`.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/92-stream-ui-record-token-usage.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use server';

import { createAI, getMutableAIState, streamUI } from 'ai/rsc';
import { openai } from '@ai-sdk/openai';
import { ReactNode } from 'react';
import { z } from 'zod';
import { generateId } from 'ai';

export interface ServerMessage {
  role: 'user' | 'assistant';
  content: string;
}

export interface ClientMessage {
  id: string;
  role: 'user' | 'assistant';
  display: ReactNode;
}

export async function continueConversation(
  input: string,
): Promise<ClientMessage> {
  'use server';

  const history = getMutableAIState();

  const result = await streamUI({
    model: openai('gpt-3.5-turbo'),
    messages: [...history.get(), { role: 'user', content: input }],
    text: ({ content, done }) => {
      if (done) {
        history.done((messages: ServerMessage[]) => [
          ...messages,
          { role: 'assistant', content },
        ]);
      }

      return <div>{content}</div>;
    },
    tools: {
      deploy: {
        description: 'Deploy repository to vercel',
        parameters: z.object({
          repositoryName: z
            .string()
            .describe('The name of the repository, example: vercel/ai-chatbot'),
        }),
        generate: async function* ({ repositoryName }) {
          yield <div>Cloning repository {repositoryName}...</div>; // [!code highlight:5]
          await new Promise(resolve => setTimeout(resolve, 3000));
          yield <div>Building repository {repositoryName}...</div>;
          await new Promise(resolve => setTimeout(resolve, 2000));
          return <div>{repositoryName} deployed!</div>;
        },
      },
    },
    onFinish: ({ usage }) => {
      const { promptTokens, completionTokens, totalTokens } = usage;
      // your own logic, e.g. for saving the chat history or recording usage
      console.log('Prompt tokens:', promptTokens);
      console.log('Completion tokens:', completionTokens);
      console.log('Total tokens:', totalTokens);
    },
  });

  return {
    id: generateId(),
    role: 'assistant',
    display: result.value,
  };
}
```

----------------------------------------

TITLE: Integrating Multiple Tools for AI Chatbot with AI SDK (TypeScript)
DESCRIPTION: This TypeScript code demonstrates how to build an AI chatbot using the Vercel AI SDK, integrating two distinct tools: a 'weather' tool to fetch location-based temperatures in Celsius and a 'convertCelsiusToFahrenheit' tool to convert temperatures. It showcases a multi-step interaction where the model can sequentially call tools to gather and process information before generating a natural language response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/06-nodejs.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { CoreMessage, streamText, tool } from 'ai';
import dotenv from 'dotenv';
import { z } from 'zod';
import * as readline from 'node:readline/promises';

dotenv.config();

const terminal = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const messages: CoreMessage[] = [];

async function main() {
  while (true) {
    const userInput = await terminal.question('You: ');

    messages.push({ role: 'user', content: userInput });

    const result = streamText({
      model: openai('gpt-4o'),
      messages,
      tools: {
        weather: tool({
          description: 'Get the weather in a location (in Celsius)',
          parameters: z.object({
            location: z
              .string()
              .describe('The location to get the weather for'),
          }),
          execute: async ({ location }) => ({
            location,
            temperature: Math.round((Math.random() * 30 + 5) * 10) / 10 // Random temp between 5°C and 35°C
          }),
        }),
        convertCelsiusToFahrenheit: tool({
          description: 'Convert a temperature from Celsius to Fahrenheit',
          parameters: z.object({
            celsius: z
              .number()
              .describe('The temperature in Celsius to convert'),
          }),
          execute: async ({ celsius }) => {
            const fahrenheit = (celsius * 9) / 5 + 32;
            return { fahrenheit: Math.round(fahrenheit * 100) / 100 };
          }
        })
      },
      maxSteps: 5,
      onStepFinish: step => {
        console.log(JSON.stringify(step, null, 2));
      }
    });

    let fullResponse = '';
    process.stdout.write('\nAssistant: ');
    for await (const delta of result.textStream) {
      fullResponse += delta;
      process.stdout.write(delta);
    }
    process.stdout.write('\n\n');

    messages.push({ role: 'assistant', content: fullResponse });
  }
}

main().catch(console.error);
```

----------------------------------------

TITLE: Using Tools with OpenAI Responses API using AI SDK
DESCRIPTION: This snippet illustrates how to integrate tool calling with the OpenAI Responses API via the AI SDK. It defines a 'getWeather' tool and includes it in the `generateText` call, allowing the model to potentially use the tool based on the prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/19-openai-responses.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai.responses('gpt-4o'),
  prompt: 'What is the weather like today in San Francisco?',
  tools: {
    getWeather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
});
```

----------------------------------------

TITLE: Batch Embedding Multiple Texts using AI SDK and OpenAI (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to perform batch text embedding using the `@ai-sdk/openai` and `ai` libraries. It utilizes the `embedMany` function to convert an array of text values into corresponding embeddings and provides usage statistics. This approach significantly improves performance by reducing API calls when processing large datasets.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/61-embed-text-batch.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { embedMany } from 'ai';
import 'dotenv/config';

async function main() {
  const { embeddings, usage } = await embedMany({
    model: openai.embedding('text-embedding-3-small'),
    values: [
      'sunny day at the beach',
      'rainy afternoon in the city',
      'snowy night in the mountains'
    ]
  });

  console.log(embeddings);
  console.log(usage);
}

main().catch(console.error);
```

----------------------------------------

TITLE: Defining setRoomTemperature Function for OpenAI Assistant
DESCRIPTION: Specifies the JSON schema for the `setRoomTemperature` tool, allowing the OpenAI Assistant to adjust the temperature in a given room. It requires both 'room' and 'temperature' parameters, with 'room' being an enumerated location and 'temperature' a number.
SOURCE: https://github.com/vercel/ai/blob/main/examples/next-openai/app/api/assistant/assistant-setup.md#_snippet_2

LANGUAGE: JSON
CODE:
```
{
  "name": "setRoomTemperature",
  "description": "Set the temperature in a room",
  "parameters": {
    "type": "object",
    "properties": {
      "room": {
        "type": "string",
        "enum": ["bedroom", "home office", "living room", "kitchen", "bathroom"]
      },
      "temperature": { "type": "number" }
    },
    "required": ["room", "temperature"]
  }
}
```

----------------------------------------

TITLE: Implementing RAG Language Model Middleware (TypeScript)
DESCRIPTION: This snippet illustrates how to implement Retrieval Augmented Generation (RAG) as middleware using the `transformParams` function. It extracts the last user message, finds relevant sources (using placeholder helper functions), formats the sources as an instruction, and adds this instruction to the last user message in the parameters before they are sent to the language model.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/40-middleware.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import type { LanguageModelV1Middleware } from 'ai';

export const yourRagMiddleware: LanguageModelV1Middleware = {
  transformParams: async ({ params }) => {
    const lastUserMessageText = getLastUserMessageText({
      prompt: params.prompt,
    });

    if (lastUserMessageText == null) {
      return params; // do not use RAG (send unmodified parameters)
    }

    const instruction =
      'Use the following information to answer the question:\n' +
      findSources({ text: lastUserMessageText })
        .map(chunk => JSON.stringify(chunk))
        .join('\n');

    return addToLastUserMessage({ params, text: instruction });
  },
};
```

----------------------------------------

TITLE: Building Next.js Chat UI with AI SDK UI useChat Hook (TSX)
DESCRIPTION: Shows how to build a simple chat interface in a Next.js client component using the `useChat` hook from `@ai-sdk/react`. It imports the hook, uses its state and handlers (`messages`, `input`, `handleInputChange`, `handleSubmit`), and renders the chat messages and input form.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/24-o3.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.content}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Streaming Structured Data with Image Prompt (URL) using AI SDK and Node.js
DESCRIPTION: This snippet demonstrates how to stream structured data from an OpenAI gpt-4-turbo model using the AI SDK. It includes an image prompt provided as a URL, allowing the model to process visual information. The output is streamed as a partial object, validated against a Zod schema for 'stamps' with 'country' and 'date' fields.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/41-stream-object-with-image-prompt.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { streamObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import dotenv from 'dotenv';
import { z } from 'zod';

dotenv.config();

async function main() {
  const { partialObjectStream } = streamObject({
    model: openai('gpt-4-turbo'),
    maxTokens: 512,
    schema: z.object({
      stamps: z.array(
        z.object({
          country: z.string(),
          date: z.string(),
        }),
      ),
    }),
    messages: [
      {
        role: 'user',
        content: [
          {
            type: 'text',
            text: 'list all the stamps in these passport pages?',
          },
          {
            type: 'image',
            image: new URL(
              'https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/WW2_Spanish_official_passport.jpg/1498px-WW2_Spanish_official_passport.jpg',
            ),
          },
        ],
      },
    ],
  });

  for await (const partialObject of partialObjectStream) {
    console.clear();
    console.log(partialObject);
  }
}

main();
```

----------------------------------------

TITLE: Generating Structured Data with OpenAI Responses API using AI SDK
DESCRIPTION: This snippet shows how to use the AI SDK Core's `generateObject` function to generate structured JSON data that conforms to a specified Zod schema, leveraging the OpenAI Responses API.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/19-openai-responses.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const { object } = await generateObject({
  model: openai.responses('gpt-4o'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.object({ name: z.string(), amount: z.string() })),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});
```

----------------------------------------

TITLE: Server-Side UI Streaming with AI SDK RSC createStreamableUI
DESCRIPTION: This snippet illustrates how to use 'createStreamableUI' from 'ai/rsc' to stream React components directly from the server. Within a tool's 'execute' function (e.g., 'getWeather'), a React component (<WeatherCard/>) is rendered with dynamic data and then streamed to the client using 'uiStream.done()'. This shifts UI rendering logic to the server, simplifying client-side code and enabling server-driven UI updates.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/07-rendering-ui-with-language-models.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { createStreamableUI } from 'ai/rsc'

const uiStream = createStreamableUI();

const text = generateText({
  model: openai('gpt-3.5-turbo'),
  system: 'you are a friendly assistant'
  prompt: 'what is the weather in SF?'
  tools: {
    getWeather: {
      description: 'Get the weather for a location',
      parameters: z.object({
        city: z.string().describe('The city to get the weather for'),
        unit: z
          .enum(['C', 'F'])
          .describe('The unit to display the temperature in')
      }),
      execute: async ({ city, unit }) => {
        const weather = getWeather({ city, unit })
        const { temperature, unit, description, forecast } = weather

        uiStream.done(
          <WeatherCard
            weather={{
              temperature: 47,
              unit: 'F',
              description: 'sunny'
              forecast,
            }}
          />
        )
      }
    }
  }
})

return {
  display: uiStream.value
}
```

----------------------------------------

TITLE: Enforcing Structured Output via experimental_output with generateText (TypeScript)
DESCRIPTION: This snippet shows how to achieve structured outputs when using `generateText` by leveraging the `experimental_output` option. It passes a Zod schema to `Output.object`, guiding the model to produce text that can be parsed into the defined JSON structure for ingredients and steps.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
// Using generateText
const result = await generateText({
  model: openai.responses('gpt-4.1'),
  prompt: 'How do I make a pizza?',
  experimental_output: Output.object({
    schema: z.object({
      ingredients: z.array(z.string()),
      steps: z.array(z.string()),
    }),
  }),
});
```

----------------------------------------

TITLE: Language Model Generate Method Cache Middleware (TypeScript)
DESCRIPTION: This 'wrapGenerate' method within the 'cacheMiddleware' intercepts 'doGenerate' calls for single-response language model interactions. It constructs a cache key from the cleaned prompt, checks for a cached result, and if found, returns it immediately. Otherwise, it proceeds with the actual generation, caches the result, and then returns it, ensuring subsequent identical requests are served from cache.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/80-local-caching-middleware.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
export const cacheMiddleware: LanguageModelV1Middleware = {
  wrapGenerate: async ({ doGenerate, params }) => {
    const cacheKey = JSON.stringify({
      ...cleanPrompt(params.prompt),
      _function: 'generate',
    });
    console.log('Cache Key:', cacheKey);

    const cached = getCachedResult(cacheKey) as Awaited<
      ReturnType<LanguageModelV1['doGenerate']>
    > | null;

    if (cached && cached !== null) {
      console.log('Cache Hit');
      return {
        ...cached,
        response: {
          ...cached.response,
          timestamp: cached?.response?.timestamp
            ? new Date(cached?.response?.timestamp)
            : undefined,
        },
      };
    }

    console.log('Cache Miss');
    const result = await doGenerate();

    updateCache(cacheKey, result);

    return result;
  },
  wrapStream: async ({ doStream, params }) => {
    const cacheKey = JSON.stringify({
      ...cleanPrompt(params.prompt),
      _function: 'stream',
    });
    console.log('Cache Key:', cacheKey);

    // Check if the result is in the cache
    const cached = getCachedResult(cacheKey);

    // If cached, return a simulated ReadableStream that yields the cached result
    if (cached && cached !== null) {
      console.log('Cache Hit');
      // Format the timestamps in the cached response
      const formattedChunks = (cached as LanguageModelV1StreamPart[]).map(p => {
        if (p.type === 'response-metadata' && p.timestamp) {
          return { ...p, timestamp: new Date(p.timestamp) };
        } else return p;
      });
      return {
        stream: simulateReadableStream({
          initialDelayInMs: 0,
          chunkDelayInMs: 10,
          chunks: formattedChunks,
        }),
        rawCall: { rawPrompt: null, rawSettings: {} },
      };
    }

    console.log('Cache Miss');
    // If not cached, proceed with streaming
    const { stream, ...rest } = await doStream();

    const fullResponse: LanguageModelV1StreamPart[] = [];

    const transformStream = new TransformStream<
      LanguageModelV1StreamPart,
      LanguageModelV1StreamPart
    >({
      transform(chunk, controller) {
        fullResponse.push(chunk);
        controller.enqueue(chunk);
      },
      flush() {
        // Store the full response in the cache after streaming is complete
        updateCache(cacheKey, fullResponse);
      },
    });

    return {
      stream: stream.pipeThrough(transformStream),
      ...rest,
    };
  },
};
```

----------------------------------------

TITLE: Generating Text with OpenAI Language Model
DESCRIPTION: This example illustrates how to use an OpenAI language model with the `generateText` function from the AI SDK. It takes a model instance and a prompt to generate a text response, such as a recipe.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

const { text } = await generateText({
  model: openai('gpt-4-turbo'),
  prompt: 'Write a vegetarian lasagna recipe for 4 people.',
});
```

----------------------------------------

TITLE: Implementing Chat UI with AI SDK's useChat Hook in Next.js
DESCRIPTION: This snippet demonstrates how to set up a basic chat interface in a Next.js application using the `@ai-sdk/react`'s `useChat` hook. It shows how to display chat messages, handle user input, and submit messages to a backend API, leveraging the `messages`, `input`, `handleInputChange`, and `handleSubmit` properties provided by the hook.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      {messages.map(message => (
        <div key={message.id} className="whitespace-pre-wrap">
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.parts.map((part, i) => {
            switch (part.type) {
              case 'text':
                return <div key={`${message.id}-${i}`}>{part.text}</div>;
            }
          })}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed dark:bg-zinc-900 bottom-0 w-full max-w-md p-2 mb-8 border border-zinc-300 dark:border-zinc-800 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Client-Side AI Chat with Tool Rendering in React
DESCRIPTION: This snippet demonstrates a React component for an AI chat interface using `@ai-sdk/react`. It shows how to initialize `useChat` with an API endpoint and `maxSteps`, and crucially, how to handle `onToolCall` for client-side tool execution (e.g., `getLocation`) and how to render different UI components based on `toolInvocations` for tools like `askForConfirmation` and `getWeatherInformation`, allowing for interactive user responses to tool calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/90-render-visual-interface-in-chat.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { ToolInvocation } from 'ai';
import { Message, useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit, addToolResult } =
    useChat({
      api: '/api/use-chat',
      maxSteps: 5,

      // run client-side tools that are automatically executed:
      async onToolCall({ toolCall }) {
        if (toolCall.toolName === 'getLocation') {
          const cities = [
            'New York',
            'Los Angeles',
            'Chicago',
            'San Francisco',
          ];
          return cities[Math.floor(Math.random() * cities.length)];
        }
      },
    });

  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch gap-4">
      {messages?.map((m: Message) => (
        <div key={m.id} className="whitespace-pre-wrap flex flex-col gap-1">
          <strong>{`${m.role}: `}</strong>
          {m.content}
          {m.toolInvocations?.map((toolInvocation: ToolInvocation) => {
            const toolCallId = toolInvocation.toolCallId;

            // render confirmation tool (client-side tool with user interaction)
            if (toolInvocation.toolName === 'askForConfirmation') {
              return (
                <div
                  key={toolCallId}
                  className="text-gray-500 flex flex-col gap-2"
                >
                  {toolInvocation.args.message}
                  <div className="flex gap-2">
                    {'result' in toolInvocation ? (
                      <b>{toolInvocation.result}</b>
                    ) : (
                      <>
                        <button
                          className="px-4 py-2 font-bold text-white bg-blue-500 rounded hover:bg-blue-700"
                          onClick={() =>
                            addToolResult({
                              toolCallId,
                              result: 'Yes, confirmed.',
                            })
                          }
                        >
                          Yes
                        </button>
                        <button
                          className="px-4 py-2 font-bold text-white bg-red-500 rounded hover:bg-red-700"
                          onClick={() =>
                            addToolResult({
                              toolCallId,
                              result: 'No, denied',
                            })
                          }
                        >
                          No
                        </button>
                      </>
                    )}
                  </div>
                </div>
              );
            }

            // other tools:
            return 'result' in toolInvocation ? (
              toolInvocation.toolName === 'getWeatherInformation' ? (
                <div
                  key={toolCallId}
                  className="flex flex-col gap-2 p-4 bg-blue-400 rounded-lg"
                >
                  <div className="flex flex-row justify-between items-center">
                    <div className="text-4xl text-blue-50 font-medium">
                      {toolInvocation.result.value}°
                      {toolInvocation.result.unit === 'celsius' ? 'C' : 'F'}
                    </div>

                    <div className="h-9 w-9 bg-amber-400 rounded-full flex-shrink-0" />
                  </div>
                  <div className="flex flex-row gap-2 text-blue-50 justify-between">
                    {toolInvocation.result.weeklyForecast.map(
                      (forecast: any) => (
                        <div
                          key={forecast.day}
                          className="flex flex-col items-center"
                        >
                          <div className="text-xs">{forecast.day}</div>
                          <div>{forecast.value}°</div>
                        </div>
                      ),
                    )}
                  </div>
                </div>
              ) : toolInvocation.toolName === 'getLocation' ? (
                <div
                  key={toolCallId}
                  className="text-gray-500 bg-gray-100 rounded-lg p-4"
                >
                  User is in {toolInvocation.result}.
                </div>
              ) : (
                <div key={toolCallId} className="text-gray-500">
                  Tool call {`${toolInvocation.toolName}: `}
                  {toolInvocation.result}
                </div>
              )
            ) : (
              <div key={toolCallId} className="text-gray-500">
                Calling {toolInvocation.toolName}...
              </div>
            );
          })}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed bottom-0 w-full max-w-md p-2 mb-8 border border-gray-300 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Multi-Step Text Streaming with AI SDK (Server)
DESCRIPTION: This server-side code demonstrates how to create a multi-step AI response using `createDataStreamResponse` and `streamText` from the AI SDK. It shows how to force a tool call in the first step and then continue the stream with a different model and system prompt in the second step, controlling the `start` and `finish` events to merge results into a single client-side message.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/24-stream-text-multistep.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { createDataStreamResponse, streamText, tool } from 'ai';
import { z } from 'zod';

export async function POST(req: Request) {
  const { messages } = await req.json();

  return createDataStreamResponse({
    execute: async dataStream => {
      // step 1 example: forced tool call
      const result1 = streamText({
        model: openai('gpt-4o-mini', { structuredOutputs: true }),
        system: 'Extract the user goal from the conversation.',
        messages,
        toolChoice: 'required', // force the model to call a tool
        tools: {
          extractGoal: tool({
            parameters: z.object({ goal: z.string() }),
            execute: async ({ goal }) => goal, // no-op extract tool
          }),
        },
      });

      // forward the initial result to the client without the finish event:
      result1.mergeIntoDataStream(dataStream, {
        experimental_sendFinish: false, // omit the finish event
      });

      // note: you can use any programming construct here, e.g. if-else, loops, etc.
      // workflow programming is normal programming with this approach.

      // example: continue stream with forced tool call from previous step
      const result2 = streamText({
        // different system prompt, different model, no tools:
        model: openai('gpt-4o'),
        system:
          'You are a helpful assistant with a different system prompt. Repeat the extract user goal in your answer.',
        // continue the workflow stream with the messages from the previous step:
        messages: [
          ...convertToCoreMessages(messages),
          ...(await result1.response).messages,
        ],
      });

      // forward the 2nd result to the client (incl. the finish event):
      result2.mergeIntoDataStream(dataStream, {
        experimental_sendStart: false, // omit the start event
      });
    },
  });
}
```

----------------------------------------

TITLE: Generating Structured Data with AI SDK and Zod (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `generateObject` function from the AI SDK to generate structured JSON data. It utilizes Zod to define a precise schema for a recipe object, ensuring the AI model's output conforms to the expected structure. The `openai` model is used to process the prompt and generate the structured response, which is then logged to the console.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/30-generate-object.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const result = await generateObject({
  model: openai('gpt-4-turbo'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(
        z.object({
          name: z.string(),
          amount: z.string()
        })
      ),
      steps: z.array(z.string())
    })
  }),
  prompt: 'Generate a lasagna recipe.'
});

console.log(JSON.stringify(result.object.recipe, null, 2));
```

----------------------------------------

TITLE: Server-side Object Streaming with streamObject (TypeScript)
DESCRIPTION: This server-side API route handles the actual object generation and streaming. It uses `streamObject` from the `ai` package with an OpenAI model and the predefined `notificationSchema`, taking a context from the request body to generate relevant notifications and returning the result as a text stream.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/40-stream-object.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamObject } from 'ai';
import { notificationSchema } from './schema';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const context = await req.json();

  const result = streamObject({
    model: openai('gpt-4-turbo'),
    schema: notificationSchema,
    prompt:
      `Generate 3 notifications for a messages app in this context:` + context,
  });

  return result.toTextStreamResponse();
}
```

----------------------------------------

TITLE: Generating Structured Output with generateText in TypeScript
DESCRIPTION: This snippet demonstrates how to use `generateText` with the `experimental_output` setting to define a structured output schema using `z.object`. It specifies the expected data shape for generating an example person, including nested objects and nullable fields, for a single, complete output.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/10-generating-structured-data.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
// experimental_output is a structured object that matches the schema:
const { experimental_output } = await generateText({
  // ...
  experimental_output: Output.object({
    schema: z.object({
      name: z.string(),
      age: z.number().nullable().describe('Age of the person.'),
      contact: z.object({
        type: z.literal('email'),
        value: z.string(),
      }),
      occupation: z.object({
        type: z.literal('employed'),
        company: z.string(),
        position: z.string(),
      }),
    }),
  }),
  prompt: 'Generate an example person for testing.',
});
```

----------------------------------------

TITLE: Generating an Array with a Schema (TypeScript)
DESCRIPTION: This example shows how to use `generateObject` to generate an array of structured items. The `output` parameter is set to 'array', and the schema defines the structure of each item in the array, such as hero names, classes, and descriptions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/03-generate-object.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateObject } from 'ai';
import { z } from 'zod';

const { object } = await generateObject({
  model: openai('gpt-4-turbo'),
  output: 'array',
  schema: z.object({
    name: z.string(),
    class: z
      .string()
      .describe('Character class, e.g. warrior, mage, or thief.'),
    description: z.string()
  }),
  prompt: 'Generate 3 hero descriptions for a fantasy role playing game.'
});
```

----------------------------------------

TITLE: Implementing Client-Side Assistant Chat Interface (React/Next.js)
DESCRIPTION: This React component (`app/page.tsx`) sets up a client-side chat interface using the `useAssistant` hook from `@ai-sdk/react`. It displays the assistant's status and messages, and provides an input field for users to send messages. The input is disabled when the assistant is not awaiting a message, ensuring proper interaction flow.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/121-stream-assistant-response-with-tools.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { Message, useAssistant } from '@ai-sdk/react';

export default function Page() {
  const { status, messages, input, submitMessage, handleInputChange } =
    useAssistant({ api: '/api/assistant' });

  return (
    <div className="flex flex-col gap-2">
      <div className="p-2">status: {status}</div>

      <div className="flex flex-col p-2 gap-2">
        {messages.map((message: Message) => (
          <div key={message.id} className="flex flex-row gap-2">
            <div className="w-24 text-zinc-500">{`${message.role}: `}</div>
            <div className="w-full">{message.content}</div>
          </div>
        ))}
      </div>

      <form onSubmit={submitMessage} className="fixed bottom-0 p-2 w-full">
        <input
          disabled={status !== 'awaiting_message'}
          value={input}
          onChange={handleInputChange}
          className="bg-zinc-100 w-full p-2"
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Retrieval Augmented Generation (RAG) with AI SDK in Node.js
DESCRIPTION: This snippet demonstrates a basic Retrieval Augmented Generation (RAG) workflow. It reads an essay, chunks it, embeds the chunks using OpenAI's 'text-embedding-3-small' model, and stores them in an in-memory vector database. It then takes a user input, embeds it, retrieves the most similar chunks from the database, and uses these chunks as context to generate an answer using 'gpt-4o'. Dependencies include 'fs', 'path', 'dotenv', '@ai-sdk/openai', and 'ai'. The input is a question, and the output is the generated answer based on the provided context.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/100-retrieval-augmented-generation.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import fs from 'fs';
import path from 'path';
import dotenv from 'dotenv';
import { openai } from '@ai-sdk/openai';
import { cosineSimilarity, embed, embedMany, generateText } from 'ai';

dotenv.config();

async function main() {
  const db: { embedding: number[]; value: string }[] = [];

  const essay = fs.readFileSync(path.join(__dirname, 'essay.txt'), 'utf8');
  const chunks = essay
    .split('.')
    .map(chunk => chunk.trim())
    .filter(chunk => chunk.length > 0 && chunk !== '\n');

  const { embeddings } = await embedMany({
    model: openai.embedding('text-embedding-3-small'),
    values: chunks,
  });
  embeddings.forEach((e, i) => {
    db.push({
      embedding: e,
      value: chunks[i],
    });
  });

  const input =
    'What were the two main things the author worked on before college?';

  const { embedding } = await embed({
    model: openai.embedding('text-embedding-3-small'),
    value: input,
  });
  const context = db
    .map(item => ({
      document: item,
      similarity: cosineSimilarity(embedding, item.embedding),
    }))
    .sort((a, b) => b.similarity - a.similarity)
    .slice(0, 3)
    .map(r => r.document.value)
    .join('\n');

  const { text } = await generateText({
    model: openai('gpt-4o'),
    prompt: `Answer the following question based only on the provided context:
             ${context}

             Question: ${input}`,
  });
  console.log(text);
}

main().catch(console.error);
```

----------------------------------------

TITLE: Handling OpenAI Assistant API Route with Next.js
DESCRIPTION: This server-side Next.js API route (`POST` handler) demonstrates how to interact with the OpenAI Assistant API. It handles creating threads, adding user messages, and streaming assistant responses back to the client using `AssistantResponse`. It also includes logic for handling `requires_action` status to submit tool outputs, ensuring continuous interaction with the assistant.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/10-openai-assistants.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AssistantResponse } from 'ai';
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY || '',
});

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  // Parse the request body
  const input: {
    threadId: string | null;
    message: string;
  } = await req.json();

  // Create a thread if needed
  const threadId = input.threadId ?? (await openai.beta.threads.create({})).id;

  // Add a message to the thread
  const createdMessage = await openai.beta.threads.messages.create(threadId, {
    role: 'user',
    content: input.message,
  });

  return AssistantResponse(
    { threadId, messageId: createdMessage.id },
    async ({ forwardStream, sendDataMessage }) => {
      // Run the assistant on the thread
      const runStream = openai.beta.threads.runs.stream(threadId, {
        assistant_id:
          process.env.ASSISTANT_ID ??
          (() => {
            throw new Error('ASSISTANT_ID is not set');
          })(),
      });

      // forward run status would stream message deltas
      let runResult = await forwardStream(runStream);

      // status can be: queued, in_progress, requires_action, cancelling, cancelled, failed, completed, or expired
      while (
        runResult?.status === 'requires_action' &&
        runResult.required_action?.type === 'submit_tool_outputs'
      ) {
        const tool_outputs =
          runResult.required_action.submit_tool_outputs.tool_calls.map(
            (toolCall: any) => {
              const parameters = JSON.parse(toolCall.function.arguments);

              switch (toolCall.function.name) {
                // configure your tool calls here

                default:
                  throw new Error(
                    `Unknown tool call function: ${toolCall.function.name}`,
                  );
              }
            },
          );

        runResult = await forwardStream(
          openai.beta.threads.runs.submitToolOutputsStream(
            threadId,
            runResult.id,
            { tool_outputs },
          ),
        );
      }
    },
  );
}
```

----------------------------------------

TITLE: Generating Text with OpenAI Responses API using AI SDK
DESCRIPTION: This snippet demonstrates the basic usage of the AI SDK Core's `generateText` function to call the OpenAI Responses API with the 'gpt-4o' model and generate text based on a given prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/19-openai-responses.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai.responses('gpt-4o'),
  prompt: 'Explain the concept of quantum entanglement.',
});
```

----------------------------------------

TITLE: Implementing Client Chat Interface with AI SDK and React
DESCRIPTION: This React client component creates a chat interface allowing users to send messages and receive streamed responses from an AI assistant. It manages input state, displays messages, and uses `ai/rsc`'s `useActions` hook to submit messages and update the UI with the assistant's responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/120-stream-assistant-response.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { ClientMessage } from './actions';
import { useActions } from 'ai/rsc';

export default function Home() {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState<ClientMessage[]>([]);
  const { submitMessage } = useActions();

  const handleSubmission = async () => {
    setMessages(currentMessages => [
      ...currentMessages,
      {
        id: '123',
        status: 'user.message.created',
        text: input,
        gui: null,
      },
    ]);

    const response = await submitMessage(input);
    setMessages(currentMessages => [...currentMessages, response]);
    setInput('');
  };

  return (
    <div className="flex flex-col-reverse">
      <div className="flex flex-row gap-2 p-2 bg-zinc-100 w-full">
        <input
          className="bg-zinc-100 w-full p-2 outline-none"
          value={input}
          onChange={event => setInput(event.target.value)}
          placeholder="Ask a question"
          onKeyDown={event => {
            if (event.key === 'Enter') {
              handleSubmission();
            }
          }}
        />
        <button
          className="p-2 bg-zinc-900 text-zinc-100 rounded-md"
          onClick={handleSubmission}
        >
          Send
        </button>
      </div>

      <div className="flex flex-col h-[calc(100dvh-56px)] overflow-y-scroll">
        <div>
          {messages.map(message => (
            <div key={message.id} className="flex flex-col gap-1 border-b p-2">
              <div className="flex flex-row justify-between">
                <div className="text-sm text-zinc-500">{message.status}</div>
              </div>
              <div>{message.text}</div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining and Using a Tool with streamUI in TSX
DESCRIPTION: This snippet demonstrates how to define a 'getWeather' tool and integrate it into a streamUI call. The tool includes a description for the model, parameters defined using Zod, and a 'generate' function that uses a generator to yield a loading component initially and then a weather component after fetching data asynchronously.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/02-streaming-react-components.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
const result = await streamUI({
  model: openai('gpt-4o'),
  prompt: 'Get the weather for San Francisco',
  text: ({ content }) => <div>{content}</div>,
  tools: {
    getWeather: {
      description: 'Get the weather for a location',
      parameters: z.object({ location: z.string() }),
      generate: async function* ({ location }) {
        yield <LoadingComponent />;
        const weather = await getWeather(location);
        return <WeatherComponent weather={weather} location={location} />;
      },
    },
  },
});
```

----------------------------------------

TITLE: Using render with OpenAI SDK Provider (TSX)
DESCRIPTION: This snippet shows how to use the `render` function from `ai/rsc` with the `openai` SDK provider. It defines a Server Action that sends a message to the AI model, includes a tool (`get_city_weather`) that uses `zod` for parameter validation, yields a loading component (`Spinner`), and returns a React Server Component (`Weather`) based on the tool's result. It requires the `ai/rsc` and `openai` packages, along with custom components and utilities.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/08-migration-guides/39-migration-guide-3-1.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { render } from 'ai/rsc';
import OpenAI from 'openai';
import { z } from 'zod';
import { Spinner, Weather } from '@/components';
import { getWeather } from '@/utils';

const openai = new OpenAI();

async function submitMessage(userInput = 'What is the weather in SF?') {
  'use server';

  return render({
    provider: openai,
    model: 'gpt-4-turbo',
    messages: [
      { role: 'system', content: 'You are a helpful assistant' },
      { role: 'user', content: userInput },
    ],
    text: ({ content }) => <p>{content}</p>,
    tools: {
      get_city_weather: {
        description: 'Get the current weather for a city',
        parameters: z
          .object({
            city: z.string().describe('the city'),
          })
          .required(),
        render: async function* ({ city }) {
          yield <Spinner />;
          const weather = await getWeather(city);
          return <Weather info={weather} />;
        },
      },
    },
  });
}
```

----------------------------------------

TITLE: Creating Chat API Route with AI SDK Core (Next.js/TSX)
DESCRIPTION: Illustrates setting up a Next.js API route (`app/api/chat/route.ts`) to handle incoming chat messages. It uses `streamText` from the AI SDK Core and the OpenAI provider to process messages and return a streamed response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/23-o1.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

// Allow responses up to 5 minutes
export const maxDuration = 300;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('o1-mini'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Generating Text with a Simple Prompt in AI SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to use a basic text prompt with the AI SDK's `generateText` function. It sets a static string as the prompt to instruct the model to invent a holiday and describe its traditions. The `yourModel` dependency represents an initialized language model.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/03-prompts.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: yourModel,
  prompt: 'Invent a new holiday and describe its traditions.',
});
```

----------------------------------------

TITLE: Implementing Chat UI with useChat Hook in Next.js (TypeScript)
DESCRIPTION: This snippet demonstrates how to integrate the `@ai-sdk/react`'s `useChat` hook into a Next.js page. It manages chat messages, user input, and form submission, rendering a dynamic chat interface where user and AI messages are displayed and an input field allows for new messages.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/03-nextjs-pages-router.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      {messages.map(message => (
        <div key={message.id} className="whitespace-pre-wrap">
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.parts.map((part, i) => {
            switch (part.type) {
              case 'text':
                return <div key={`${message.id}-${i}`}>{part.text}</div>;
            }
          })}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed dark:bg-zinc-900 bottom-0 w-full max-w-md p-2 mb-8 border border-zinc-300 dark:border-zinc-800 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining a Weather Tool with `tool()` in TypeScript
DESCRIPTION: This snippet demonstrates how to define a `weatherTool` using the `tool()` helper from the `ai` library in TypeScript. It utilizes `zod` for defining the `parameters` schema, specifically for a `location` string. The `tool()` function enables TypeScript to correctly infer the type of `location` within the `execute` method's arguments, ensuring type safety for the tool's execution logic.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/20-tool.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { tool } from 'ai';
import { z } from 'zod';

export const weatherTool = tool({
  description: 'Get the weather in a location',
  parameters: z.object({
    location: z.string().describe('The location to get the weather for'),
  }),
  // location below is inferred to be a string:
  execute: async ({ location }) => ({
    location,
    temperature: 72 + Math.floor(Math.random() * 21) - 10,
  }),
});
```

----------------------------------------

TITLE: Handling Full Streaming Errors with AI SDK Core (TypeScript)
DESCRIPTION: This snippet shows how to manage errors in full streams that provide dedicated error parts. Errors are handled by checking the `part.type` within the stream iteration, specifically for the 'error' case. It is also recommended to include an outer `try/catch` block to capture errors that might occur outside the streaming loop.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/50-error-handling.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';

try {
  const { fullStream } = streamText({
    model: yourModel,
    prompt: 'Write a vegetarian lasagna recipe for 4 people.',
  });

  for await (const part of fullStream) {
    switch (part.type) {
      // ... handle other part types

      case 'error': {
        const error = part.error;
        // handle error
        break;
      }
    }
  }
} catch (error) {
  // handle error
}
```

----------------------------------------

TITLE: Using Tools with generateText (Single Step)
DESCRIPTION: Demonstrates how to pass a tools object to the `generateText` function. It defines a 'weather' tool using the `tool` helper, specifying its description, Zod schema parameters, and an asynchronous execute function that simulates fetching weather data.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/15-tools-and-tool-calling.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { z } from 'zod';
import { generateText, tool } from 'ai';

const result = await generateText({
  model: yourModel,
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  prompt: 'What is the weather in San Francisco?',
});
```

----------------------------------------

TITLE: Streaming Text Generations with OpenAI GPT-4o (TypeScript)
DESCRIPTION: This snippet demonstrates how to use `streamText` to generate text from an OpenAI GPT-4o model. It initializes the model with a prompt and then iterates asynchronously over the `textStream` to print each part of the generated text to standard output, suitable for real-time display in interactive applications.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/02-stream-text.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

const { textStream } = streamText({
  model: openai('gpt-4o'),
  prompt: 'Invent a new holiday and describe its traditions.',
});

for await (const textPart of textStream) {
  process.stdout.write(textPart);
}
```

----------------------------------------

TITLE: Calling Tools with Image Prompt using AI SDK and OpenAI
DESCRIPTION: This snippet demonstrates how to use the AI SDK's `generateText` function to interact with a multimodal model (GPT-4 Turbo) by providing both text and image content. It defines a `logFood` tool with Zod-validated parameters (`name`, `calories`) that the model can call based on the image input, simulating a meal logging application. Dependencies include `ai`, `@ai-sdk/openai`, and `zod`.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/52-call-tools-with-image-prompt.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const result = await generateText({
  model: openai('gpt-4-turbo'),
  messages: [
    {
      role: 'user',
      content: [
        { type: 'text', text: 'can you log this meal for me?' },
        {
          type: 'image',
          image: new URL(
            'https://upload.wikimedia.org/wikipedia/commons/thumb/e/e4/Cheeseburger_%2817237580619%29.jpg/640px-Cheeseburger_%2817237580619%29.jpg',
          ),
        },
      ],
    },
  ],
  tools: {
    logFood: tool({
      description: 'Log a food item',
      parameters: z.object({
        name: z.string(),
        calories: z.number(),
      }),
      execute({ name, calories }) {
        storeInDatabase({ name, calories });
      },
    }),
  },
});
```

----------------------------------------

TITLE: Calculating Cosine Similarity with AI SDK and OpenAI Embeddings (TypeScript)
DESCRIPTION: This snippet demonstrates how to calculate cosine similarity between two text embeddings using the `ai` SDK and OpenAI's embedding model. It first generates embeddings for two different phrases using `embedMany` and then computes their similarity with `cosineSimilarity`, logging the result.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/50-cosine-similarity.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { cosineSimilarity, embedMany } from 'ai';

const { embeddings } = await embedMany({
  model: openai.embedding('text-embedding-3-small'),
  values: ['sunny day at the beach', 'rainy afternoon in the city'],
});

console.log(
  `cosine similarity: ${cosineSimilarity(embeddings[0], embeddings[1])}`,
);
```

----------------------------------------

TITLE: Streaming UI Components with AI SDK RSC and Tools (Next.js Server Action)
DESCRIPTION: This Next.js Server Action (app/actions.tsx) uses the AI SDK RSC streamUI function to stream dynamic React components from the server. It includes a tool definition (getWeather) that can yield intermediate loading states before returning the final component based on tool execution.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_8

LANGUAGE: TSX
CODE:
```
'use server';

import { streamUI } from 'ai/rsc';
import { deepinfra } from '@ai-sdk/deepinfra';
import { z } from 'zod';

export async function streamComponent() {
  const result = await streamUI({
    model: deepinfra('meta-llama/Meta-Llama-3.1-70B-Instruct'),
    prompt: 'Get the weather for San Francisco',
    text: ({ content }) => <div>{content}</div>,
    tools: {
      getWeather: {
        description: 'Get the weather for a location',
        parameters: z.object({ location: z.string() }),
        generate: async function* ({ location }) {
          yield <div>loading...</div>;
          const weather = '25c'; // await getWeather(location);
          return (
            <div>
              the weather in {location} is {weather}.
            </div>
          );
        },
      },
    },
  });
  return result.value;
}
```

----------------------------------------

TITLE: Defining a Weather Tool with AI SDK and Zod
DESCRIPTION: This snippet creates a `weatherTool` using `ai`'s `createTool` function. It defines parameters for location using `zod` and simulates fetching weather data after a delay. This tool is designed for the LLM to call when specific weather information is requested, demonstrating how to create custom actions for generative UI.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/04-generative-user-interfaces.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { tool as createTool } from 'ai';
import { z } from 'zod';

export const weatherTool = createTool({
  description: 'Display the weather for a location',
  parameters: z.object({
    location: z.string().describe('The location to get the weather for'),
  }),
  execute: async function ({ location }) {
    await new Promise(resolve => setTimeout(resolve, 2000));
    return { weather: 'Sunny', temperature: 75, location };
  },
});

export const tools = {
  displayWeather: weatherTool,
};
```

----------------------------------------

TITLE: Generating Structured Objects with OpenAI (TypeScript)
DESCRIPTION: This example demonstrates how to use OpenAI's structured outputs feature with `generateObject` to produce JSON objects that strictly adhere to a defined Zod schema. It highlights enabling `structuredOutputs` in the model configuration and defining the expected output structure.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateObject } from 'ai';
import { z } from 'zod';

const result = await generateObject({
  model: openai('gpt-4o-2024-08-06', {
    structuredOutputs: true,
  }),
  schemaName: 'recipe',
  schemaDescription: 'A recipe for lasagna.',
  schema: z.object({
    name: z.string(),
    ingredients: z.array(
      z.object({
        name: z.string(),
        amount: z.string(),
      }),
    ),
    steps: z.array(z.string()),
  }),
  prompt: 'Generate a lasagna recipe.',
});

console.log(JSON.stringify(result.object, null, 2));
```

----------------------------------------

TITLE: Defining AI Tools and Server Action for Flight Booking in TSX
DESCRIPTION: This snippet defines two asynchronous functions, `searchFlights` and `lookupFlight`, which simulate flight data retrieval. It then exports a `submitUserMessage` server action that uses `streamUI` from `@ai-sdk/openai` to interact with a GPT-4o model. This action integrates the defined flight tools, allowing the AI to search for and look up flight details based on user prompts, streaming UI components as responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/04-multistep-interfaces.mdx#_snippet_0

LANGUAGE: TSX
CODE:
```
import { streamUI } from 'ai/rsc';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const searchFlights = async (
  source: string,
  destination: string,
  date: string,
) => {
  return [
    {
      id: '1',
      flightNumber: 'AA123',
    },
    {
      id: '2',
      flightNumber: 'AA456',
    },
  ];
};

const lookupFlight = async (flightNumber: string) => {
  return {
    flightNumber: flightNumber,
    departureTime: '10:00 AM',
    arrivalTime: '12:00 PM',
  };
};

export async function submitUserMessage(input: string) {
  'use server';

  const ui = await streamUI({
    model: openai('gpt-4o'),
    system: 'you are a flight booking assistant',
    prompt: input,
    text: async ({ content }) => <div>{content}</div>,
    tools: {
      searchFlights: {
        description: 'search for flights',
        parameters: z.object({
          source: z.string().describe('The origin of the flight'),
          destination: z.string().describe('The destination of the flight'),
          date: z.string().describe('The date of the flight'),
        }),
        generate: async function* ({ source, destination, date }) {
          yield `Searching for flights from ${source} to ${destination} on ${date}...`;
          const results = await searchFlights(source, destination, date);

          return (
            <div>
              {results.map(result => (
                <div key={result.id}>
                  <div>{result.flightNumber}</div>
                </div>
              ))}
            </div>
          );
        },
      },
      lookupFlight: {
        description: 'lookup details for a flight',
        parameters: z.object({
          flightNumber: z.string().describe('The flight number'),
        }),
        generate: async function* ({ flightNumber }) {
          yield `Looking up details for flight ${flightNumber}...`;
          const details = await lookupFlight(flightNumber);

          return (
            <div>
              <div>Flight Number: {details.flightNumber}</div>
              <div>Departure Time: {details.departureTime}</div>
              <div>Arrival Time: {details.arrivalTime}</div>
            </div>
          );
        },
      },
    },
  });

  return ui.value;
}
```

----------------------------------------

TITLE: Submitting User Messages and Streaming AI Responses (OpenAI, TSX)
DESCRIPTION: This server action `submitMessage` handles user input, creates or updates an OpenAI thread, initiates a run with the Assistant API, and streams the assistant's response back to the client using `createStreamableUI` and `createStreamableValue`. It manages multiple runs in a queue and processes events from the OpenAI stream.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/120-stream-assistant-response.mdx#_snippet_2

LANGUAGE: TSX
CODE:
```
'use server';

import { generateId } from 'ai';
import { createStreamableUI, createStreamableValue } from 'ai/rsc';
import { OpenAI } from 'openai';
import { ReactNode } from 'react';
import { Message } from './message';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export interface ClientMessage {
  id: string;
  status: ReactNode;
  text: ReactNode;
}

const ASSISTANT_ID = 'asst_xxxx';
let THREAD_ID = '';
let RUN_ID = '';

export async function submitMessage(question: string): Promise<ClientMessage> {
  const statusUIStream = createStreamableUI('thread.init');

  const textStream = createStreamableValue('');
  const textUIStream = createStreamableUI(
    <Message textStream={textStream.value} />,
  );

  const runQueue = [];

  (async () => {
    if (THREAD_ID) {
      await openai.beta.threads.messages.create(THREAD_ID, {
        role: 'user',
        content: question,
      });

      const run = await openai.beta.threads.runs.create(THREAD_ID, {
        assistant_id: ASSISTANT_ID,
        stream: true,
      });

      runQueue.push({ id: generateId(), run });
    } else {
      const run = await openai.beta.threads.createAndRun({
        assistant_id: ASSISTANT_ID,
        stream: true,
        thread: {
          messages: [{ role: 'user', content: question }],
        },
      });

      runQueue.push({ id: generateId(), run });
    }

    while (runQueue.length > 0) {
      const latestRun = runQueue.shift();

      if (latestRun) {
        for await (const delta of latestRun.run) {
          const { data, event } = delta;

          statusUIStream.update(event);

          if (event === 'thread.created') {
            THREAD_ID = data.id;
          } else if (event === 'thread.run.created') {
            RUN_ID = data.id;
          } else if (event === 'thread.message.delta') {
            data.delta.content?.map(part => {
              if (part.type === 'text') {
                if (part.text) {
                  textStream.append(part.text.value as string);
                }
              }
            });
          } else if (event === 'thread.run.failed') {
            console.error(data);
          }
        }
      }
    }

    statusUIStream.done();
    textStream.done();
  })();

  return {
    id: generateId(),
    status: statusUIStream.value,
    text: textUIStream.value,
  };
}
```

----------------------------------------

TITLE: Creating a Next.js API Route Handler for Chat Streaming
DESCRIPTION: This TypeScript code defines a Next.js App Router API route handler at `/api/chat` for streaming AI responses. It imports `openai` from `@ai-sdk/openai` and `streamText` from `ai`. The `POST` function extracts `messages` from the request body, uses `streamText` with the `gpt-4o` model to generate a response, and then converts the result to a data stream response for the client. `maxDuration` is set to 30 seconds to allow for longer streaming responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Streaming Structured Data with Image Prompt (File Buffer) using AI SDK and Node.js
DESCRIPTION: This snippet illustrates streaming structured data from an OpenAI gpt-4-turbo model with an image prompt provided as a file buffer. It uses the AI SDK to process the image from a local file (fs.readFileSync) and stream the response as a partial object, validated by a Zod schema for 'stamps'.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/41-stream-object-with-image-prompt.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { streamObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import dotenv from 'dotenv';
import { z } from 'zod';

dotenv.config();

async function main() {
  const { partialObjectStream } = streamObject({
    model: openai('gpt-4-turbo'),
    maxTokens: 512,
    schema: z.object({
      stamps: z.array(
        z.object({
          country: z.string(),
          date: z.string(),
        }),
      ),
    }),
    messages: [
      {
        role: 'user',
        content: [
          {
            type: 'text',
            text: 'list all the stamps in these passport pages?',
          },
          {
            type: 'image',
            image: fs.readFileSync('./node/attachments/eclipse.jpg'),
          },
        ],
      },
    ],
  });

  for await (const partialObject of partialObjectStream) {
    console.clear();
    console.log(partialObject);
  }
}

main();
```

----------------------------------------

TITLE: Processing PDF Uploads and Generating Summaries with AI SDK in Next.js API Route
DESCRIPTION: This Next.js API route handles incoming POST requests containing PDF files. It extracts the PDF from the form data and uses the AI SDK's `generateObject` function with Anthropic's Claude model to analyze the PDF content. The model is prompted to generate a structured 50-word summary, which is then returned as the API response.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/31-generate-object-with-file-prompt.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { generateObject } from 'ai';
import { anthropic } from '@ai-sdk/anthropic';
import { z } from 'zod';

export async function POST(request: Request) {
  const formData = await request.formData();
  const file = formData.get('pdf') as File;

  const result = await generateObject({
    model: anthropic('claude-3-5-sonnet-latest'),
    messages: [
      {
        role: 'user',
        content: [
          {
            type: 'text',
            text: 'Analyze the following PDF and generate a summary.',
          },
          {
            type: 'file',
            data: await file.arrayBuffer(),
            mimeType: 'application/pdf',
          },
        ],
      },
    ],
    schema: z.object({
      summary: z.string().describe('A 50 word summary of the PDF.'),
    }),
  });

  return new Response(result.object.summary);
}
```

----------------------------------------

TITLE: Generating Structured Data with AI SDK Core and Zod (TSX)
DESCRIPTION: Demonstrates how to use the `generateObject` function from AI SDK Core to generate structured JSON data based on a Zod schema. It shows importing necessary modules, defining the schema, and calling the function with a model and prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/24-o3.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const { object } = await generateObject({
  model: openai('o3-mini'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.object({ name: z.string(), amount: z.string() })),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});
```

----------------------------------------

TITLE: Updating Embedding Logic for Content Retrieval (TypeScript)
DESCRIPTION: This code updates the embedding utility file (`lib/ai/embedding.ts`). It adds a `generateEmbedding` function to create a single embedding for a query and a `findRelevantContent` function that uses this embedding to search the database for semantically similar content based on cosine distance, returning the most relevant items.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/01-rag-chatbot.mdx#_snippet_21

LANGUAGE: TypeScript
CODE:
```
import { embed, embedMany } from 'ai';
import { openai } from '@ai-sdk/openai';
import { db } from '../db';
import { cosineDistance, desc, gt, sql } from 'drizzle-orm';
import { embeddings } from '../db/schema/embeddings';

const embeddingModel = openai.embedding('text-embedding-ada-002');

const generateChunks = (input: string): string[] => {
  return input
    .trim()
    .split('.')
    .filter(i => i !== '');
};

export const generateEmbeddings = async (
  value: string,
): Promise<Array<{ embedding: number[]; content: string }>> => {
  const chunks = generateChunks(value);
  const { embeddings } = await embedMany({
    model: embeddingModel,
    values: chunks,
  });
  return embeddings.map((e, i) => ({ content: chunks[i], embedding: e }));
};

export const generateEmbedding = async (value: string): Promise<number[]> => {
  const input = value.replaceAll('\n', ' ');
  const { embedding } = await embed({
    model: embeddingModel,
    value: input,
  });
  return embedding;
};

export const findRelevantContent = async (userQuery: string) => {
  const userQueryEmbedded = await generateEmbedding(userQuery);
  const similarity = sql<number>`1 - (${cosineDistance(
    embeddings.embedding,
    userQueryEmbedded,
  )})`;
  const similarGuides = await db
    .select({ name: embeddings.content, similarity })
    .from(embeddings)
    .where(gt(similarity, 0.5))
    .orderBy(t => desc(t.similarity))
    .limit(4);
  return similarGuides;
};
```

----------------------------------------

TITLE: Caching OpenAI Responses with Vercel KV and Next.js using AI SDK
DESCRIPTION: This snippet demonstrates how to cache OpenAI responses using the `onFinish` lifecycle callback from the AI SDK Core. It utilizes Upstash Redis (Vercel KV) to store the generated text for 1 hour, preventing redundant API calls for identical requests. It checks for a cached response before calling the model and stores the new response upon completion.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/04-caching.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { formatDataStreamPart, streamText } from 'ai';
import { Redis } from '@upstash/redis';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

const redis = new Redis({
  url: process.env.KV_URL,
  token: process.env.KV_TOKEN,
});

export async function POST(req: Request) {
  const { messages } = await req.json();

  // come up with a key based on the request:
  const key = JSON.stringify(messages);

  // Check if we have a cached response
  const cached = await redis.get(key);
  if (cached != null) {
    return new Response(formatDataStreamPart('text', cached), {
      status: 200,
      headers: { 'Content-Type': 'text/plain' },
    });
  }

  // Call the language model:
  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    async onFinish({ text }) {
      // Cache the response text:
      await redis.set(key, text);
      await redis.expire(key, 60 * 60);
    },
  });

  // Respond with the stream
  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Implementing Server-Side Tool Calling with AI SDK and OpenAI
DESCRIPTION: This server action defines the `continueConversation` function, which uses the AI SDK's `generateText` to interact with an OpenAI model. It includes a custom tool, `celsiusToFahrenheit`, with Zod-defined parameters, enabling the model to perform specific conversions and extend its capabilities beyond basic text generation.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/50-call-tools.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use server';

import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

export interface Message {
  role: 'user' | 'assistant';
  content: string;
}

export async function continueConversation(history: Message[]) {
  'use server';

  const { text, toolResults } = await generateText({
    model: openai('gpt-3.5-turbo'),
    system: 'You are a friendly assistant!',
    messages: history,
    tools: {
      celsiusToFahrenheit: {
        description: 'Converts celsius to fahrenheit',
        parameters: z.object({
          value: z.string().describe('The value in celsius')
        }),
        execute: async ({ value }) => {
          const celsius = parseFloat(value);
          const fahrenheit = celsius * (9 / 5) + 32;
          return `${celsius}°C is ${fahrenheit.toFixed(2)}°F`;
        }
      }
    }
  });

  return {
    messages: [
      ...history,
      {
        role: 'assistant' as const,
        content:
          text || toolResults.map(toolResult => toolResult.result).join('\n')
      }
    ]
  };
}
```

----------------------------------------

TITLE: Implementing Chat Interface with useChat Hook (Next.js/React)
DESCRIPTION: This snippet demonstrates a client-side chat interface built with Next.js and React, utilizing the `@ai-sdk/react`'s `useChat` hook. It handles message display, user input, form submission, and error handling for streaming AI responses. The component renders messages, including tool invocations, and provides an input field for new messages.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/122-caching-middleware.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();
  if (error) return <div>{error.message}</div>;

  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      <div className="space-y-4">
        {messages.map(m => (
          <div key={m.id} className="whitespace-pre-wrap">
            <div>
              <div className="font-bold">{m.role}</div>
              {m.toolInvocations ? (
                <pre>{JSON.stringify(m.toolInvocations, null, 2)}</pre>
              ) : (
                <p>{m.content}</p>
              )}
            </div>
          </div>
        ))}
      </div>

      <form onSubmit={handleSubmit}>
        <input
          className="fixed bottom-0 w-full max-w-md p-2 mb-8 border border-gray-300 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Managing Conversation UI State and User Input (React Client Component)
DESCRIPTION: This client-side `Home` component manages the user interface for the conversation. It uses `useUIState` to display the conversation history and `useState` for user input. The `onClick` handler for the "Send" button updates the UI with the user's message, calls the `continueConversation` server action, and then updates the UI again with the AI's response, providing an interactive chat experience.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/61-restore-messages-from-database.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { useState, useEffect } from 'react';
import { ClientMessage } from './actions';
import { useActions, useUIState } from 'ai/rsc';
import { generateId } from 'ai';

export default function Home() {
  const [conversation, setConversation] = useUIState();
  const [input, setInput] = useState<string>('');
  const { continueConversation } = useActions();

  return (
    <div>
      <div className="conversation-history">
        {conversation.map((message: ClientMessage) => (
          <div key={message.id} className={`message ${message.role}`}>
            {message.role}: {message.display}
          </div>
        ))}
      </div>

      <div className="input-area">
        <input
          type="text"
          value={input}
          onChange={e => setInput(e.target.value)}
          placeholder="Type your message..."
        />
        <button
          onClick={async () => {
            // Add user message to UI
            setConversation((currentConversation: ClientMessage[]) => [
              ...currentConversation,
              { id: generateId(), role: 'user', display: input },
            ]);

            // Get AI response
            const message = await continueConversation(input);

            // Add AI response to UI
            setConversation((currentConversation: ClientMessage[]) => [
              ...currentConversation,
              message,
            ]);

            setInput('');
          }}
        >
          Send
        </button>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Creating Nested Streamable UIs for Dynamic Components
DESCRIPTION: This function illustrates how to create nested streamable UI components. It initializes a main `ui` streamable, then asynchronously fetches stock data. A `historyChart` streamable is created and passed as a prop to `StockCard`, allowing the chart to update independently within the parent component as its data becomes available. This enables complex, dynamic UI compositions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/05-multiple-streamables.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
async function getStockHistoryChart({ symbol: string }) {
  'use server';

  const ui = createStreamableUI(<Spinner />);

  // We need to wrap this in an async IIFE to avoid blocking.
  (async () => {
    const price = await getStockPrice({ symbol });

    // Show a spinner as the history chart for now.
    const historyChart = createStreamableUI(<Spinner />);
    ui.done(<StockCard historyChart={historyChart.value} price={price} />);

    // Getting the history data and then update that part of the UI.
    const historyData = await fetch('https://my-stock-data-api.com');
    historyChart.done(<HistoryChart data={historyData} />);
  })();

  return ui;
}
```

----------------------------------------

TITLE: Using Web Search Tool with AI SDK generateText (Basic)
DESCRIPTION: This snippet shows how to integrate the `webSearch` tool into a text generation call using the AI SDK. It uses `openai.responses` with `generateText` and includes the `web_search_preview` tool to allow the model to search the web for information related to the prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/19-openai-responses.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

const result = await generateText({
  model: openai.responses('gpt-4o-mini'),
  prompt: 'What happened in San Francisco last week?',
  tools: {
    web_search_preview: openai.tools.webSearchPreview(),
  },
});

console.log(result.text);
console.log(result.sources);
```

----------------------------------------

TITLE: Implementing Basic Chat UI with useChat Hook (React/TSX)
DESCRIPTION: This snippet demonstrates the fundamental usage of the `useChat` hook within a React component to build a simple chat interface. It retrieves essential state (`messages`, `input`) and handlers (`handleInputChange`, `handleSubmit`) from the hook to display chat history and manage user input submission. This setup enables real-time message streaming from a backend API.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/02-chatbot.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit } = useChat({});

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.content}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Setting Up Chat API Route with AI SDK and OpenAI
DESCRIPTION: This snippet defines a server-side API route (`POST`) that handles chat requests. It uses `@ai-sdk/openai` and `ai`'s `streamText` function to process messages with the `gpt-4o` model and stream responses back to the client, serving as the backend for the chat interface.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/04-generative-user-interfaces.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(request: Request) {
  const { messages } = await request.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'You are a friendly assistant!',
    messages,
    maxSteps: 5,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Configuring Multi-Step Tool Calls with AI SDK and OpenAI
DESCRIPTION: This snippet demonstrates how to configure `generateText` for multi-step tool calls using the AI SDK and OpenAI's GPT-4-turbo model. It sets `maxSteps` to 5, enabling the model to make up to five iterative calls to tools. A `weather` tool is defined with Zod for parameter validation, simulating a weather lookup and showcasing how to integrate custom tool execution logic.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/53-call-tools-multiple-steps.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const { text } = await generateText({
  model: openai('gpt-4-turbo'),
  maxSteps: 5,
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }: { location: string }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  prompt: 'What is the weather in San Francisco?',
});
```

----------------------------------------

TITLE: Wrapping Application with AI Context in TSX
DESCRIPTION: This snippet demonstrates how to wrap the root layout of a Next.js application with the `AI` context created in `app/ai.ts`. This ensures that all child components within the application have access to the AI's UI and AI states, enabling seamless integration of the conversational AI features throughout the application.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/04-multistep-interfaces.mdx#_snippet_2

LANGUAGE: TSX
CODE:
```
import { type ReactNode } from 'react';
import { AI } from './ai';

export default function RootLayout({
  children,
}: Readonly<{ children: ReactNode }>) {
  return (
    <AI>
      <html lang="en">
        <body>{children}</body>
      </html>
    </AI>
  );
}
```

----------------------------------------

TITLE: Combining Multiple Anthropic Computer Use Tools in AI SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to initialize and combine multiple Anthropic Computer Use tools (computer, bash, text editor) within a single `generateText` request using the AI SDK. It shows how to define custom execution logic for tools like `bashTool` and `textEditorTool` and then pass them to the `tools` parameter of the `generateText` function, enabling complex multi-step operations.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/05-computer-use.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
const computerTool = anthropic.tools.computer_20241022({
  ...
});

const bashTool = anthropic.tools.bash_20241022({
  execute: async ({ command, restart }) => execSync(command).toString()
});

const textEditorTool = anthropic.tools.textEditor_20241022({
  execute: async ({
    command,
    path,
    file_text,
    insert_line,
    new_str,
    old_str,
    view_range
  }) => {
    // Handle file operations based on command
    switch(command) {
      return executeTextEditorFunction({
        command,
        path,
        fileText: file_text,
        insertLine: insert_line,
        newStr: new_str,
        oldStr: old_str,
        viewRange: view_range
      });
    }
  }
});


const response = await generateText({
  model: anthropic("claude-3-5-sonnet-20241022"),
  prompt: "Create a new file called example.txt, write 'Hello World' to it, and run 'cat example.txt' in the terminal",
  tools: {
    computer: computerTool,
    bash: bashTool,
    str_replace_editor: textEditorTool
  }
});
```

----------------------------------------

TITLE: Creating Chat API Endpoint with AI SDK (Node.js/TypeScript)
DESCRIPTION: This backend snippet defines a Next.js API route (`/api/chat`) that serves as the endpoint for the `useChat` hook. It receives user messages, uses the AI SDK's `streamText` function with an OpenAI model to generate a response, and streams the result back to the client. It requires the `@ai-sdk/openai` and `ai` packages and sets a maximum duration for the request.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/02-chatbot.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4-turbo'),
    system: 'You are a helpful assistant.',
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Building Frontend Chat UI with AI SDK UI (React/TSX)
DESCRIPTION: Shows a basic React component (`app/page.tsx`) utilizing the `useChat` hook from `@ai-sdk/react`. This hook manages the chat state, handles user input changes, submits messages to the API route, and displays the conversation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/23-o1.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.content}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Server Action for Generating Structured Notifications (TypeScript)
DESCRIPTION: This server-side function `getNotifications` uses the `generateObject` function from the AI SDK to create structured notification data. It leverages Zod to define a precise schema for the `notifications` array, ensuring the AI model generates data conforming to the specified `name`, `message`, and `minutesAgo` fields. It uses `gpt-4-turbo` model.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/30-generate-object.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
'use server';

import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

export async function getNotifications(input: string) {
  'use server';

  const { object: notifications } = await generateObject({
    model: openai('gpt-4-turbo'),
    system: 'You generate three notifications for a messages app.',
    prompt: input,
    schema: z.object({
      notifications: z.array(
        z.object({
          name: z.string().describe('Name of a fictional person.'),
          message: z.string().describe('Do not use emojis or links.'),
          minutesAgo: z.number()
        })
      )
    })
  });

  return { notifications };
}
```

----------------------------------------

TITLE: Defining AI Tool for Weather Retrieval in Next.js API
DESCRIPTION: This Next.js API route (`/api/chat`) defines a server-side endpoint for handling chat interactions and tool calls. It uses the `streamText` function from the `ai` module with the `openai` model and includes a custom `getWeather` tool. The tool's parameters are validated using Zod, and its `execute` function simulates fetching weather data for a given city and unit.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/70-call-tools.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { ToolInvocation, streamText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

interface Message {
  role: 'user' | 'assistant';
  content: string;
  toolInvocations?: ToolInvocation[];
}

export async function POST(req: Request) {
  const { messages }: { messages: Message[] } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'You are a helpful assistant.',
    messages,
    tools: {
      getWeather: {
        description: 'Get the weather for a location',
        parameters: z.object({
          city: z.string().describe('The city to get the weather for'),
          unit: z
            .enum(['C', 'F'])
            .describe('The unit to display the temperature in'),
        }),
        execute: async ({ city, unit }) => {
          const weather = {
            value: 24,
            description: 'Sunny',
          };

          return `It is currently ${weather.value}°${unit} and ${weather.description} in ${city}!`;
        },
      },
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Defining Chatbot API Route with AI SDK Tools (TypeScript)
DESCRIPTION: This snippet defines an API route (`POST`) for a chatbot using `@ai-sdk/openai` and `ai`. It demonstrates how to configure `streamText` with a language model (GPT-4o) and define various tools. It includes a server-side tool (`getWeatherInformation`) with an `execute` function, a client-side tool requiring user interaction (`askForConfirmation`), and an automatically executed client-side tool (`getLocation`), showcasing different tool types and their parameter definitions using Zod.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-tool-usage.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';
import { z } from 'zod';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      // server-side tool with execute function:
      getWeatherInformation: {
        description: 'show the weather in a given city to the user',
        parameters: z.object({ city: z.string() }),
        execute: async ({}: { city: string }) => {
          const weatherOptions = ['sunny', 'cloudy', 'rainy', 'snowy', 'windy'];
          return weatherOptions[
            Math.floor(Math.random() * weatherOptions.length)
          ];
        },
      },
      // client-side tool that starts user interaction:
      askForConfirmation: {
        description: 'Ask the user for confirmation.',
        parameters: z.object({
          message: z.string().describe('The message to ask for confirmation.'),
        }),
      },
      // client-side tool that is automatically executed on the client:
      getLocation: {
        description:
          'Get the user location. Always ask for confirmation before using this tool.',
        parameters: z.object({}),
      },
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Streaming Text Generation with AI SDK and OpenAI (TypeScript)
DESCRIPTION: This snippet demonstrates how to implement text streaming using the AI SDK's `streamText` function with OpenAI's `gpt-4-turbo` model. It shows how to initialize the model, define a prompt, and iterate over the `textStream` to log parts of the generated text as they become available, improving user experience by displaying responses incrementally.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/05-streaming.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

const { textStream } = streamText({
  model: openai('gpt-4-turbo'),
  prompt: 'Write a poem about embedding models.',
});

for await (const textPart of textStream) {
  console.log(textPart);
}
```

----------------------------------------

TITLE: Updating Meal Log with `delete_meal` Tool (TXT)
DESCRIPTION: This snippet illustrates a multi-step interaction where the application context is crucial. Following a meal logging action, the user requests an update. The model, leveraging the conversation history, correctly identifies the meal to be deleted and calls the `delete_meal` tool, demonstrating how prior actions influence subsequent tool calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/09-multistep-interfaces.mdx#_snippet_1

LANGUAGE: txt
CODE:
```
User: Log a chicken shawarma for lunch.
Tool: log_meal("chicken shawarma", "250g", "12:00 PM")
Model: Chicken shawarma has been logged for lunch.
...
...
User: I skipped lunch today, can you update my log?
Tool: delete_meal("chicken shawarma")
Model: Chicken shawarma has been deleted from your log.
```

----------------------------------------

TITLE: Generating Structured Data on Client-side with React
DESCRIPTION: This React component demonstrates how to trigger the generation of structured data (notifications) from a Next.js API endpoint. It sends a POST request with a prompt to `/api/completion` and displays the received JSON data, managing loading states. It uses `useState` to manage the generated data and loading status.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/30-generate-object.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';

export default function Page() {
  const [generation, setGeneration] = useState();
  const [isLoading, setIsLoading] = useState(false);

  return (
    <div>
      <div
        onClick={async () => {
          setIsLoading(true);

          await fetch('/api/completion', {
            method: 'POST',
            body: JSON.stringify({
              prompt: 'Messages during finals week.',
            }),
          }).then(response => {
            response.json().then(json => {
              setGeneration(json.notifications);
              setIsLoading(false);
            });
          });
        }}
      >
        Generate
      </div>

      {isLoading ? 'Loading...' : <pre>{JSON.stringify(generation)}</pre>}
    </div>
  );
}
```

----------------------------------------

TITLE: Configuring Max Steps for Client-Side Chat with AI SDK React
DESCRIPTION: This snippet demonstrates how to configure the `maxSteps` option within the `useChat` hook from `@ai-sdk/react`. Setting `maxSteps` to 5 allows the AI model to perform up to 5 sequential steps for a single generation, enabling more complex, multi-turn interactions and tool usage on the client side.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_12

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat({
    maxSteps: 5,
  });

  // ... rest of your component code
}
```

----------------------------------------

TITLE: Intercepting and Confirming AI SDK Tool Calls in React (TSX)
DESCRIPTION: This frontend React component (using `useChat` from `@ai-sdk/react`) shows how to intercept tool invocations from the AI model. It specifically checks for the `getWeatherInformation` tool call, presents a confirmation UI to the user, and uses `addToolResult` to send the user's "Yes" or "No" decision back to the model, completing the Human-in-the-Loop flow.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/75-human-in-the-loop.mdx#_snippet_3

LANGUAGE: TSX
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit, addToolResult } =
    useChat();

  return (
    <div>
      <div>
        {messages?.map(m => (
          <div key={m.id}>
            <strong>{`${m.role}: `}</strong>
            {m.parts?.map((part, i) => {
              switch (part.type) {
                case 'text':
                  return <div key={i}>{part.text}</div>;
                case 'tool-invocation':
                  const toolInvocation = part.toolInvocation;
                  const toolCallId = toolInvocation.toolCallId;

                  // render confirmation tool (client-side tool with user interaction)
                  if (
                    toolInvocation.toolName === 'getWeatherInformation' &&
                    toolInvocation.state === 'call'
                  ) {
                    return (
                      <div key={toolCallId}>
                        Get weather information for {toolInvocation.args.city}?
                        <div>
                          <button
                            onClick={() =>
                              addToolResult({
                                toolCallId,
                                result: 'Yes, confirmed.',
                              })
                            }
                          >
                            Yes
                          </button>
                          <button
                            onClick={() =>
                              addToolResult({
                                toolCallId,
                                result: 'No, denied.',
                              })
                            }
                          >
                            No
                          </button>
                        </div>
                      </div>
                    );
                  }
              }
            })}
            <br />
          </div>
        ))}
      </div>

      <form onSubmit={handleSubmit}>
        <input
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Sequential AI Generations with AI SDK
DESCRIPTION: This snippet demonstrates how to create a sequential chain of AI generations using the AI SDK. It uses `generateText` with the `gpt-4o` model to first generate blog post ideas, then selects the best idea from the generated list, and finally creates a detailed outline based on the chosen idea. Each subsequent generation uses the output of the previous one as its input, illustrating a "chain" pattern.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/09-sequential-generations.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

async function sequentialActions() {
  // Generate blog post ideas
  const ideasGeneration = await generateText({
    model: openai('gpt-4o'),
    prompt: 'Generate 10 ideas for a blog post about making spaghetti.',
  });

  console.log('Generated Ideas:\n', ideasGeneration);

  // Pick the best idea
  const bestIdeaGeneration = await generateText({
    model: openai('gpt-4o'),
    prompt: `Here are some blog post ideas about making spaghetti:\n${ideasGeneration}\n\nPick the best idea from the list above and explain why it's the best.`,
  });

  console.log('\nBest Idea:\n', bestIdeaGeneration);

  // Generate an outline
  const outlineGeneration = await generateText({
    model: openai('gpt-4o'),
    prompt: `We've chosen the following blog post idea about making spaghetti:\n${bestIdeaGeneration}\n\nCreate a detailed outline for a blog post based on this idea.`,
  });

  console.log('\nBlog Post Outline:\n', outlineGeneration);
}

sequentialActions().catch(console.error);
```

----------------------------------------

TITLE: LLM Interaction with RAG Context (txt)
DESCRIPTION: Demonstrates how providing relevant context alongside the user's prompt enables the Large Language Model to generate an accurate and specific response based on the provided information, showcasing the benefit of RAG.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/01-rag-chatbot.mdx#_snippet_1

LANGUAGE: txt
CODE:
```
**input**
Respond to the user's prompt using only the provided context.
user prompt: 'What is my favorite food?'
context: user loves chicken nuggets

**generation**
Your favorite food is chicken nuggets!
```

----------------------------------------

TITLE: Implementing Basic Chat Interface with AI SDK React
DESCRIPTION: This snippet demonstrates a basic client-side chat interface using the `useChat` hook from `@ai-sdk/react`. It displays messages from both user and AI and provides an input field for sending new messages, forming the foundation for a generative UI chat application.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/04-generative-user-interfaces.mdx#_snippet_0

LANGUAGE: TSX
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(message => (
        <div key={message.id}>
          <div>{message.role === 'user' ? 'User: ' : 'AI: '}</div>
          <div>{message.content}</div>
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Type a message..."
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining a Recipe Schema with Valibot and AI SDK
DESCRIPTION: This snippet demonstrates how to define a complex schema for a recipe using Valibot types (`object`, `string`, `array`) and convert it into an AI SDK-compatible schema using `valibotSchema`. This schema can then be used for structured data generation or tool definitions within the AI SDK. It defines fields for `name`, `ingredients` (each with `name` and `amount`), and `steps`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/27-valibot-schema.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { valibotSchema } from '@ai-sdk/valibot';
import { object, string, array } from 'valibot';

const recipeSchema = valibotSchema(
  object({
    name: string(),
    ingredients: array(
      object({
        name: string(),
        amount: string(),
      }),
    ),
    steps: array(string()),
  }),
);
```

----------------------------------------

TITLE: Creating an AI Chat API Route with Caching Middleware (TypeScript)
DESCRIPTION: This TypeScript snippet defines a Next.js API route (`/api/chat`) that processes incoming chat messages. It integrates the `cacheMiddleware` by wrapping an OpenAI model, enabling cached responses for `streamText` operations. The route also demonstrates tool usage for a simulated weather lookup, returning the AI model's response as a data stream.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/122-caching-middleware.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { cacheMiddleware } from '@/ai/middleware';
import { openai } from '@ai-sdk/openai';
import { wrapLanguageModel, streamText, tool } from 'ai';
import { z } from 'zod';

const wrappedModel = wrapLanguageModel({
  model: openai('gpt-4o-mini'),
  middleware: cacheMiddleware,
});

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: wrappedModel,
    messages,
    tools: {
      weather: tool({
        description: 'Get the weather in a location',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for'),
        }),
        execute: async ({ location }) => ({
          location,
          temperature: 72 + Math.floor(Math.random() * 21) - 10,
        }),
      }),
    },
  });
  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Handling Errors in AI SDK streamText (TSX)
DESCRIPTION: This snippet demonstrates how to use the `onError` callback with the `streamText` function to log errors that occur during streaming. Since `streamText` streams errors rather than throwing them, the `onError` callback provides a mechanism to capture and process these errors, preventing silent failures and enabling custom error logging or handling logic.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/09-troubleshooting/15-stream-text-not-working.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { streamText } from 'ai';

const result = streamText({
  model: yourModel,
  prompt: 'Invent a new holiday and describe its traditions.',
  onError({ error }) {
    console.error(error); // your error logging logic here
  },
});
```

----------------------------------------

TITLE: Processing AI Tool Calls with Human Confirmation in TypeScript
DESCRIPTION: This asynchronous function processes tool invocations within a message stream, specifically handling cases where human input is required. It executes tools only when explicitly authorized (via `APPROVAL.YES`) and streams the results back to the client. It takes `messages`, a `dataStream`, and a map of `executeFunctions` for tools requiring confirmation, returning an updated array of messages.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/75-human-in-the-loop.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
export async function processToolCalls<
  Tools extends ToolSet,
  ExecutableTools extends {
    [Tool in keyof Tools as Tools[Tool] extends { execute: Function }
      ? never
      : Tool]: Tools[Tool];
  },
>(
  {
    dataStream,
    messages,
  }: {
    tools: Tools; // used for type inference
    dataStream: DataStreamWriter;
    messages: Message[];
  },
  executeFunctions: {
    [K in keyof Tools & keyof ExecutableTools]?: (
      args: z.infer<ExecutableTools[K]['parameters']>,
      context: ToolExecutionOptions,
    ) => Promise<any>;
  },
): Promise<Message[]> {
  const lastMessage = messages[messages.length - 1];
  const parts = lastMessage.parts;
  if (!parts) return messages;

  const processedParts = await Promise.all(
    parts.map(async part => {
      // Only process tool invocations parts
      if (part.type !== 'tool-invocation') return part;

      const { toolInvocation } = part;
      const toolName = toolInvocation.toolName;

      // Only continue if we have an execute function for the tool (meaning it requires confirmation) and it's in a 'result' state
      if (!(toolName in executeFunctions) || toolInvocation.state !== 'result')
        return part;

      let result;

      if (toolInvocation.result === APPROVAL.YES) {
        // Get the tool and check if the tool has an execute function.
        if (
          !isValidToolName(toolName, executeFunctions) ||
          toolInvocation.state !== 'result'
        ) {
          return part;
        }

        const toolInstance = executeFunctions[toolName];
        if (toolInstance) {
          result = await toolInstance(toolInvocation.args, {
            messages: convertToCoreMessages(messages),
            toolCallId: toolInvocation.toolCallId,
          });
        } else {
          result = 'Error: No execute function found on tool';
        }
      } else if (toolInvocation.result === APPROVAL.NO) {
        result = 'Error: User denied access to tool execution';
      } else {
        // For any unhandled responses, return the original part.
        return part;
      }

      // Forward updated tool result to the client.
      dataStream.write(
        formatDataStreamPart('tool_result', {
          toolCallId: toolInvocation.toolCallId,
          result,
        }),
      );

      // Return updated toolInvocation with the actual result.
      return {
        ...part,
        toolInvocation: {
          ...toolInvocation,
          result,
        },
      };
    }),
  );

  // Finally return the processed messages
  return [...messages.slice(0, -1), { ...lastMessage, parts: processedParts }];
}
```

----------------------------------------

TITLE: Generating Text with OpenAI Provider (TypeScript)
DESCRIPTION: Demonstrates how to use the `generateText` function from the AI SDK with the OpenAI provider. It configures the model using `openai('gpt-4-turbo')` and provides a prompt to generate text. The generated text is then extracted from the result. Requires `@ai-sdk/openai` and `ai` packages.
SOURCE: https://github.com/vercel/ai/blob/main/packages/openai/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

const { text } = await generateText({
  model: openai('gpt-4-turbo'),
  prompt: 'Write a vegetarian lasagna recipe for 4 people.',
});
```

----------------------------------------

TITLE: Server-Side Text Streaming API Route (Next.js/AI SDK)
DESCRIPTION: This Next.js API route (`/api/completion`) handles incoming `POST` requests to generate and stream text. It extracts a `prompt` from the request body, uses the `streamText` function from the `ai` module with an OpenAI GPT-4 model, and sets a system prompt. The generated text is then streamed back to the client using `toDataStreamResponse()`.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/20-stream-text.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

export async function POST(req: Request) {
  const { prompt }: { prompt: string } = await req.json();

  const result = streamText({
    model: openai('gpt-4'),
    system: 'You are a helpful assistant.',
    prompt,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Accessing Tool Calls from AI SDK Generation Result
DESCRIPTION: This code demonstrates how to iterate through the `toolCalls` property of the `generateText` result to access typed tool call arguments. It uses a `switch` statement to handle different tool names ('cityAttractions', 'weather') and retrieve their specific parameters, such as `city` or `location`, allowing for conditional logic based on the model's tool invocation.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/50-call-tools.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, tool } from 'ai';
import dotenv from 'dotenv';
import { z } from 'zod';

dotenv.config();

async function main() {
  const result = await generateText({
    model: openai('gpt-3.5-turbo'),
    maxTokens: 512,
    tools: {
      weather: tool({
        description: 'Get the weather in a location',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for')
        }),
        execute: async ({ location }) => ({
          location,
          temperature: 72 + Math.floor(Math.random() * 21) - 10
        })
      }),
      cityAttractions: tool({
        parameters: z.object({ city: z.string() })
      })
    },
    prompt:
      'What is the weather in San Francisco and what attractions should I visit?'
  });

  // typed tool calls:
  for (const toolCall of result.toolCalls) {
    switch (toolCall.toolName) {
      case 'cityAttractions': {
        toolCall.args.city; // string
        break;
      }

      case 'weather': {
        toolCall.args.location; // string
        break;
      }
    }
  }

  console.log(JSON.stringify(result, null, 2));
}

main().catch(console.error);
```

----------------------------------------

TITLE: Client-Side AI Interaction Page in TSX
DESCRIPTION: This client-side component handles user input and displays the AI conversation. It uses `useState` for managing the input field, `useUIState` to display the conversation history, and `useActions` to call the `submitUserMessage` server action. The `handleSubmit` function sends user messages to the AI and updates the conversation UI with both user input and AI responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/04-multistep-interfaces.mdx#_snippet_3

LANGUAGE: TSX
CODE:
```
'use client';

import { useState } from 'react';
import { AI } from './ai';
import { useActions, useUIState } from 'ai/rsc';

export default function Page() {
  const [input, setInput] = useState<string>('');
  const [conversation, setConversation] = useUIState<typeof AI>();
  const { submitUserMessage } = useActions();

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setInput('');
    setConversation(currentConversation => [
      ...currentConversation,
      <div>{input}</div>,
    ]);
    const message = await submitUserMessage(input);
    setConversation(currentConversation => [...currentConversation, message]);
  };

  return (
    <div>
      <div>
        {conversation.map((message, i) => (
          <div key={i}>{message}</div>
        ))}
      </div>
      <div>
        <form onSubmit={handleSubmit}>
          <input
            type="text"
            value={input}
            onChange={e => setInput(e.target.value)}
          />
          <button>Send Message</button>
        </form>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Client-Side Chat with AI SDK UI and Tool Invocations (TypeScript)
DESCRIPTION: This TypeScript React component demonstrates a client-side chatbot using the `useChat` hook from `@ai-sdk/react`. It configures `maxSteps` for multi-turn interactions, defines an `onToolCall` callback for automatic client-side tool execution (e.g., `getLocation`), and renders chat messages by distinguishing between text and various tool invocation states (e.g., `askForConfirmation`, `getLocation`, `getWeatherInformation`), allowing user interaction to add tool results.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-tool-usage.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { ToolInvocation } from 'ai';
import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit, addToolResult } =
    useChat({
      maxSteps: 5,

      // run client-side tools that are automatically executed:
      async onToolCall({ toolCall }) {
        if (toolCall.toolName === 'getLocation') {
          const cities = [
            'New York',
            'Los Angeles',
            'Chicago',
            'San Francisco',
          ];
          return cities[Math.floor(Math.random() * cities.length)];
        }
      },
    });

  return (
    <>
      {messages?.map(message => (
        <div key={message.id}>
          <strong>{`${message.role}: `}</strong>
          {message.parts.map(part => {
            switch (part.type) {
              // render text parts as simple text:
              case 'text':
                return part.text;

              // for tool invocations, distinguish between the tools and the state:
              case 'tool-invocation': {
                const callId = part.toolInvocation.toolCallId;

                switch (part.toolInvocation.toolName) {
                  case 'askForConfirmation': {
                    switch (part.toolInvocation.state) {
                      case 'call':
                        return (
                          <div key={callId}>
                            {part.toolInvocation.args.message}
                            <div>
                              <button
                                onClick={() =>
                                  addToolResult({
                                    toolCallId: callId,
                                    result: 'Yes, confirmed.',
                                  })
                                }
                              >
                                Yes
                              </button>
                              <button
                                onClick={() =>
                                  addToolResult({
                                    toolCallId: callId,
                                    result: 'No, denied',
                                  })
                                }
                              >
                                No
                              </button>
                            </div>
                          </div>
                        );
                      case 'result':
                        return (
                          <div key={callId}>
                            Location access allowed:{' '}
                            {part.toolInvocation.result}
                          </div>
                        );
                    }
                    break;
                  }

                  case 'getLocation': {
                    switch (part.toolInvocation.state) {
                      case 'call':
                        return <div key={callId}>Getting location...</div>;
                      case 'result':
                        return (
                          <div key={callId}>
                            Location: {part.toolInvocation.result}
                          </div>
                        );
                    }
                    break;
                  }

                  case 'getWeatherInformation': {
                    switch (part.toolInvocation.state) {
                      // example of pre-rendering streaming tool calls:
                      case 'partial-call':
                        return (
                          <pre key={callId}>
                            {JSON.stringify(part.toolInvocation, null, 2)}
                          </pre>
                        );
                      case 'call':
                        return (
```

----------------------------------------

TITLE: Accessing Tool Results from AI SDK Generation Result
DESCRIPTION: This snippet illustrates how to access the `toolResults` property from the `generateText` output. It iterates over the results, specifically demonstrating how to retrieve the `location` and `temperature` from the 'weather' tool's execution result. This property is only populated if the tool definition includes an `execute` function.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/50-call-tools.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, tool } from 'ai';
import dotenv from 'dotenv';
import { z } from 'zod';

dotenv.config();

async function main() {
  const result = await generateText({
    model: openai('gpt-3.5-turbo'),
    maxTokens: 512,
    tools: {
      weather: tool({
        description: 'Get the weather in a location',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for')
        }),
        execute: async ({ location }) => ({
          location,
          temperature: 72 + Math.floor(Math.random() * 21) - 10
        })
      }),
      cityAttractions: tool({
        parameters: z.object({ city: z.string() })
      })
    },
    prompt:
      'What is the weather in San Francisco and what attractions should I visit?'
  });

  // typed tool results for tools with execute method:
  for (const toolResult of result.toolResults) {
    switch (toolResult.toolName) {
      case 'weather': {
        toolResult.args.location; // string
        toolResult.result.location; // string
        toolResult.result.temperature; // number
        break;
      }
    }
  }

  console.log(JSON.stringify(result, null, 2));
}

main().catch(console.error);
```

----------------------------------------

TITLE: Calling OpenAI GPT-4.5 with AI SDK TypeScript
DESCRIPTION: Demonstrates how to use the AI SDK's `generateText` function to interact with the OpenAI GPT-4.5 model. It imports necessary functions and the OpenAI provider, then calls `generateText` with the model name and a prompt to get a text response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/22-gpt-4-5.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai('gpt-4.5-preview'),
  prompt: 'Explain the concept of quantum entanglement.',
});
```

----------------------------------------

TITLE: Streaming AI Responses with Tools using Vercel AI SDK (TypeScript)
DESCRIPTION: This server-side API route (api/chat.ts) processes incoming chat messages, streams text responses from the 'gpt-4-turbo' model using '@ai-sdk/openai', and defines various tools. It includes a server-executed tool ('getWeatherInformation') and client-side tools ('askForConfirmation', 'getLocation') that can be invoked by the AI model to extend its capabilities.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/90-render-visual-interface-in-chat.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';
import { z } from 'zod';

export default async function POST(request: Request) {
  const { messages } = await request.json();

  const result = streamText({
    model: openai('gpt-4-turbo'),
    messages,
    tools: {
      // server-side tool with execute function:
      getWeatherInformation: {
        description: 'show the weather in a given city to the user',
        parameters: z.object({ city: z.string() }),
        execute: async ({}: { city: string }) => {
          return {
            value: 24,
            unit: 'celsius',
            weeklyForecast: [
              { day: 'Monday', value: 24 },
              { day: 'Tuesday', value: 25 },
              { day: 'Wednesday', value: 26 },
              { day: 'Thursday', value: 27 },
              { day: 'Friday', value: 28 },
              { day: 'Saturday', value: 29 },
              { day: 'Sunday', value: 30 }
            ]
          };
        }
      },
      // client-side tool that starts user interaction:
      askForConfirmation: {
        description: 'Ask the user for confirmation.',
        parameters: z.object({
          message: z.string().describe('The message to ask for confirmation.')
        })
      },
      // client-side tool that is automatically executed on the client:
      getLocation: {
        description:
          'Get the user location. Always ask for confirmation before using this tool.',
        parameters: z.object({})
      }
    }
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Generating Marketing Copy with Quality Check using AI Chains (TypeScript)
DESCRIPTION: This snippet demonstrates a sequential workflow for generating and validating marketing copy. It first uses `generateText` to create copy, then `generateObject` to perform a quality check (call to action, emotional appeal, clarity). If the quality metrics are below a threshold, it regenerates the copy with specific instructions. It relies on `@ai-sdk/openai` and `zod` for schema validation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, generateObject } from 'ai';
import { z } from 'zod';

async function generateMarketingCopy(input: string) {
  const model = openai('gpt-4o');

  // First step: Generate marketing copy
  const { text: copy } = await generateText({
    model,
    prompt: `Write persuasive marketing copy for: ${input}. Focus on benefits and emotional appeal.`,
  });

  // Perform quality check on copy
  const { object: qualityMetrics } = await generateObject({
    model,
    schema: z.object({
      hasCallToAction: z.boolean(),
      emotionalAppeal: z.number().min(1).max(10),
      clarity: z.number().min(1).max(10),
    }),
    prompt: `Evaluate this marketing copy for:
    1. Presence of call to action (true/false)
    2. Emotional appeal (1-10)
    3. Clarity (1-10)

    Copy to evaluate: ${copy}`,
  });

  // If quality check fails, regenerate with more specific instructions
  if (
    !qualityMetrics.hasCallToAction ||
    qualityMetrics.emotionalAppeal < 7 ||
    qualityMetrics.clarity < 7
  ) {
    const { text: improvedCopy } = await generateText({
      model,
      prompt: `Rewrite this marketing copy with:
      ${!qualityMetrics.hasCallToAction ? '- A clear call to action' : ''}
      ${qualityMetrics.emotionalAppeal < 7 ? '- Stronger emotional appeal' : ''}
      ${qualityMetrics.clarity < 7 ? '- Improved clarity and directness' : ''}

      Original copy: ${copy}`,
    });
    return { copy: improvedCopy, qualityMetrics };
  }

  return { copy, qualityMetrics };
}
```

----------------------------------------

TITLE: Displaying AI Tool Invocations in Chat UI (React/TypeScript)
DESCRIPTION: This code updates the React UI component to display different types of message parts received from the AI model. It uses the `useChat` hook from `@ai-sdk/react` to manage chat state. For 'text' parts, it renders the text, and for 'tool-invocation' parts, it serializes and displays the tool call details as JSON, allowing users to see when and how tools are invoked.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      {messages.map(message => (
        <div key={message.id} className="whitespace-pre-wrap">
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.parts.map((part, i) => {
            switch (part.type) {
              case 'text':
                return <div key={`${message.id}-${i}`}>{part.text}</div>;
              case 'tool-invocation':
                return (
                  <pre key={`${message.id}-${i}`}>
                    {JSON.stringify(part.toolInvocation, null, 2)}
                  </pre>
                );
            }
          })}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed dark:bg-zinc-900 bottom-0 w-full max-w-md p-2 mb-8 border border-zinc-300 dark:border-zinc-800 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Initializing OpenAI Chat Model with Default Settings (TypeScript)
DESCRIPTION: This snippet demonstrates how to initialize an OpenAI chat model using the `openai.chat()` factory method. It takes the model ID, such as 'gpt-3.5-turbo', as its primary argument, creating a model instance with default configurations. This is the basic way to get started with OpenAI chat models.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const model = openai.chat('gpt-3.5-turbo');
```

----------------------------------------

TITLE: Instantiating OpenAI Language Model
DESCRIPTION: This snippet shows how to create a language model instance by invoking the `openai` provider with a model ID, such as 'gpt-4-turbo'. The provider automatically selects the correct API based on the model ID.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
const model = openai('gpt-4-turbo');
```

----------------------------------------

TITLE: Generating Structured Data with generateObject (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `generateObject` function from the AI SDK to generate structured data. It defines a Zod schema for a `recipe` object, including its name, ingredients, and steps, and then prompts the model to generate a lasagna recipe conforming to this schema. The `object` property of the result contains the validated, generated data.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/10-generating-structured-data.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateObject } from 'ai';
import { z } from 'zod';

const { object } = await generateObject({
  model: yourModel,
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.object({ name: z.string(), amount: z.string() })),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});
```

----------------------------------------

TITLE: Updating AI State in Server Action with getMutableAIState (TypeScript)
DESCRIPTION: Illustrates how to modify the AI state from within a server action using `getMutableAIState`. This function provides methods (`.update()` and `.done()`) to append new messages (user input and AI responses) to the conversation history, ensuring state synchronization.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/03-generative-ui-state.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
import { getMutableAIState } from 'ai/rsc';

export async function sendMessage(message: string) {
  'use server';

  const history = getMutableAIState();

  // Update the AI state with the new user message.
  history.update([...history.get(), { role: 'user', content: message }]);

  const response = await generateText({
    model: openai('gpt-3.5-turbo'),
    messages: history.get(),
  });

  // Update the AI state again with the response from the model.
  history.done([...history.get(), { role: 'assistant', content: response }]);

  return response;
}
```

----------------------------------------

TITLE: Adding getInformation Tool to Chat Route Handler (TypeScript)
DESCRIPTION: This code modifies the chat route handler (`api/chat/route.ts`) to include a new tool named `getInformation`. This tool is designed to take a user's question, execute the `findRelevantContent` function (imported from the embedding utility) to retrieve relevant information from the knowledge base, and provide that information as context for the AI model's response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/01-rag-chatbot.mdx#_snippet_22

LANGUAGE: TypeScript
CODE:
```
import { createResource } from '@/lib/actions/resources';
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';
import { findRelevantContent } from '@/lib/ai/embedding';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    system: `You are a helpful assistant. Check your knowledge base before answering any questions.
    Only respond to questions using information from tool calls.
    if no relevant information is found in the tool calls, respond, "Sorry, I don't know."`,
    tools: {
      addResource: tool({
        description: `add a resource to your knowledge base.
          If the user provides a random piece of knowledge unprompted, use this tool without asking for confirmation.`,
        parameters: z.object({
          content: z
            .string()
            .describe('the content or resource to add to the knowledge base'),
        }),
        execute: async ({ content }) => createResource({ content }),
      }),
      getInformation: tool({
        description: `get information from your knowledge base to answer questions.`,
        parameters: z.object({
          question: z.string().describe('the users question'),
        }),
        execute: async ({ question }) => findRelevantContent(question),
      }),
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Calling Server Actions with useActions Hook in Next.js
DESCRIPTION: This snippet demonstrates how to call a server action (`sendMessage`) from a client component using the `useActions` hook from `ai/rsc`. It also shows how to manage and update the UI state with `useUIState` to display user messages and AI responses dynamically. The `handleSubmit` function prevents default form submission, updates the UI with the user's message, calls the server action, and then updates the UI again with the AI's response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/03-generative-ui-state.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
'use client';

import { useActions, useUIState } from 'ai/rsc';
import { AI } from './ai';

export default function Page() {
  const { sendMessage } = useActions<typeof AI>();
  const [messages, setMessages] = useUIState();

  const handleSubmit = async event => {
    event.preventDefault();

    setMessages([
      ...messages,
      { id: Date.now(), role: 'user', display: event.target.message.value },
    ]);

    const response = await sendMessage(event.target.message.value);

    setMessages([
      ...messages,
      { id: Date.now(), role: 'assistant', display: response },
    ]);
  };

  return (
    <>
      <ul>
        {messages.map(message => (
          <li key={message.id}>{message.display}</li>
        ))}
      </ul>
      <form onSubmit={handleSubmit}>
        <input type="text" name="message" />
        <button type="submit">Send</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Defining a JSON Schema with jsonSchema() in TypeScript
DESCRIPTION: This snippet demonstrates how to define a complex JSON schema using the `jsonSchema` helper function from the AI SDK. It specifies an object type for a recipe, including nested properties for name, ingredients (an array of objects), and steps (an array of strings), along with required fields. This schema can be used for structured data generation or tool definitions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/25-json-schema.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { jsonSchema } from 'ai';

const mySchema = jsonSchema<{
  recipe: {
    name: string;
    ingredients: { name: string; amount: string }[];
    steps: string[];
  };
}>({
  type: 'object',
  properties: {
    recipe: {
      type: 'object',
      properties: {
        name: { type: 'string' },
        ingredients: {
          type: 'array',
          items: {
            type: 'object',
            properties: {
              name: { type: 'string' },
              amount: { type: 'string' }
            },
            required: ['name', 'amount']
          }
        },
        steps: {
          type: 'array',
          items: { type: 'string' }
        }
      },
      required: ['name', 'ingredients', 'steps']
    }
  },
  required: ['recipe']
});
```

----------------------------------------

TITLE: Generating Text with OpenAI GPT-4o in TypeScript
DESCRIPTION: This snippet demonstrates how to use the `generateText` function from the `ai` library with an OpenAI `gpt-4o` model. It shows a basic example of prompting the model to invent a new holiday and then logging the generated text to the console. This is ideal for non-interactive text generation tasks.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/01-generate-text.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

const { text } = await generateText({
  model: openai('gpt-4o'),
  prompt: 'Invent a new holiday and describe its traditions.',
});

console.log(text);
```

----------------------------------------

TITLE: Implementing Chat UI with useChat Hook in React
DESCRIPTION: This React component utilizes the `@ai-sdk/react`'s `useChat` hook to manage chat state and interact with a backend API. It captures user input, sends messages to the `/api/chat` endpoint, and displays streamed responses, providing a real-time chat experience.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/21-stream-text-with-chat-prompt.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, setInput, append } = useChat();

  return (
    <div>
      <input
        value={input}
        onChange={event => {
          setInput(event.target.value);
        }}
        onKeyDown={async event => {
          if (event.key === 'Enter') {
            append({ content: input, role: 'user' });
          }
        }}
      />

      {messages.map((message, index) => (
        <div key={index}>{message.content}</div>
      ))}
    </div>
  );
}
```

----------------------------------------

TITLE: Generating Text Embeddings with AI SDK and OpenAI (TypeScript)
DESCRIPTION: This snippet demonstrates how to generate text embeddings using the `@ai-sdk/openai` and `ai` libraries in a Node.js environment. It initializes an OpenAI embedding model, processes a given text value ('sunny day at the beach'), and outputs the resulting high-dimensional vector embedding along with the token usage. This functionality is crucial for tasks like semantic search and document similarity.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/60-embed-text.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { embed } from 'ai';
import 'dotenv/config';

async function main() {
  const { embedding, usage } = await embed({
    model: openai.embedding('text-embedding-3-small'),
    value: 'sunny day at the beach',
  });

  console.log(embedding);
  console.log(usage);
}

main().catch(console.error);
```

----------------------------------------

TITLE: Calling Claude 3.7 Sonnet with AI SDK (Anthropic)
DESCRIPTION: This snippet demonstrates the basic usage of the AI SDK to call Claude 3.7 Sonnet directly via the Anthropic provider. It shows how to import the necessary modules, call `generateText` with the model and a prompt, and log the resulting text.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/20-sonnet-3-7.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { anthropic } from '@ai-sdk/anthropic';
import { generateText } from 'ai';

const { text, reasoning, reasoningDetails } = await generateText({
  model: anthropic('claude-3-7-sonnet-20250219'),
  prompt: 'How many people will live in the world in 2040?',
});
console.log(text); // text response
```

----------------------------------------

TITLE: Streaming Text with OpenAI GPT-3.5 Turbo in TypeScript
DESCRIPTION: This snippet demonstrates how to stream text responses from an OpenAI chat model using the AI SDK. It initializes the `streamText` function with a GPT-3.5 Turbo model, sets a maximum token limit, defines a system prompt, and provides a conversation history. The `for await...of` loop then iterates over the `textStream` to log each part of the generated text as it becomes available.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/21-stream-text-with-chat-prompt.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

const result = streamText({
  model: openai('gpt-3.5-turbo'),
  maxTokens: 1024,
  system: 'You are a helpful chatbot.',
  messages: [
    {
      role: 'user',
      content: 'Hello!',
    },
    {
      role: 'assistant',
      content: 'Hello! How can I help you today?',
    },
    {
      role: 'user',
      content: 'I need help with my computer.',
    },
  ],
});

for await (const textPart of result.textStream) {
  console.log(textPart);
}
```

----------------------------------------

TITLE: Creating Chat Completion API Endpoint with AI SDK and OpenAI
DESCRIPTION: This server-side Next.js API route handles POST requests for chat completions. It uses `streamText` from the `ai` library and integrates with `openai` to generate responses based on the conversation history, streaming the output back to the client for real-time display.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/21-stream-text-with-chat-prompt.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { streamText, UIMessage } from 'ai';
import { openai } from '@ai-sdk/openai';

export async function POST(req: Request) {
  const { messages }: { messages: UIMessage[] } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'You are a helpful assistant.',
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Implementing Caching with AI SDK Language Model Middleware in TypeScript
DESCRIPTION: This TypeScript code defines `cacheMiddleware`, a `LanguageModelV1Middleware` that integrates Redis for caching AI model responses. It implements `wrapGenerate` to cache direct responses and `wrapStream` to cache and simulate streamed responses using `simulateReadableStream`. Dependencies include `@upstash/redis` and `ai` SDK types, requiring `KV_URL` and `KV_TOKEN` environment variables for Redis connection.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/04-caching.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Redis } from '@upstash/redis';
import {
  type LanguageModelV1,
  type LanguageModelV1Middleware,
  type LanguageModelV1StreamPart,
  simulateReadableStream,
} from 'ai';

const redis = new Redis({
  url: process.env.KV_URL,
  token: process.env.KV_TOKEN,
});

export const cacheMiddleware: LanguageModelV1Middleware = {
  wrapGenerate: async ({ doGenerate, params }) => {
    const cacheKey = JSON.stringify(params);

    const cached = (await redis.get(cacheKey)) as Awaited<
      ReturnType<LanguageModelV1['doGenerate']>
    > | null;

    if (cached !== null) {
      return {
        ...cached,
        response: {
          ...cached.response,
          timestamp: cached?.response?.timestamp
            ? new Date(cached?.response?.timestamp)
            : undefined,
        },
      };
    }

    const result = await doGenerate();

    redis.set(cacheKey, result);

    return result;
  },
  wrapStream: async ({ doStream, params }) => {
    const cacheKey = JSON.stringify(params);

    // Check if the result is in the cache
    const cached = await redis.get(cacheKey);

    // If cached, return a simulated ReadableStream that yields the cached result
    if (cached !== null) {
      // Format the timestamps in the cached response
      const formattedChunks = (cached as LanguageModelV1StreamPart[]).map(p => {
        if (p.type === 'response-metadata' && p.timestamp) {
          return { ...p, timestamp: new Date(p.timestamp) };
        } else return p;
      });
      return {
        stream: simulateReadableStream({
          initialDelayInMs: 0,
          chunkDelayInMs: 10,
          chunks: formattedChunks,
        }),
        rawCall: { rawPrompt: null, rawSettings: {} },
      };
    }

    // If not cached, proceed with streaming
    const { stream, ...rest } = await doStream();

    const fullResponse: LanguageModelV1StreamPart[] = [];

    const transformStream = new TransformStream<
      LanguageModelV1StreamPart,
      LanguageModelV1StreamPart
    >({
      transform(chunk, controller) {
        fullResponse.push(chunk);
        controller.enqueue(chunk);
      },
      flush() {
        // Store the full response in the cache after streaming is complete
        redis.set(cacheKey, fullResponse);
      },
    });

    return {
      stream: stream.pipeThrough(transformStream),
      ...rest,
    };
  },
};
```

----------------------------------------

TITLE: Setting up OpenTelemetry SDK with Langfuse Exporter in Node.js (TypeScript)
DESCRIPTION: This comprehensive Node.js TypeScript example demonstrates the full setup of the OpenTelemetry SDK with `LangfuseExporter`. It initializes the SDK with auto-instrumentations, starts tracing, performs a `generateText` call with detailed experimental telemetry (including `functionId` and `metadata`), logs the result, and ensures traces are flushed to Langfuse by gracefully shutting down the SDK.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/05-observability/langfuse.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';
import { LangfuseExporter } from 'langfuse-vercel';

const sdk = new NodeSDK({
  traceExporter: new LangfuseExporter(),
  instrumentations: [getNodeAutoInstrumentations()],
});

sdk.start();

async function main() {
  const result = await generateText({
    model: openai('gpt-4o'),
    maxTokens: 50,
    prompt: 'Invent a new holiday and describe its traditions.',
    experimental_telemetry: {
      isEnabled: true,
      functionId: 'my-awesome-function',
      metadata: {
        something: 'custom',
        someOtherThing: 'other-value',
      },
    },
  });

  console.log(result.text);

  await sdk.shutdown(); // Flushes the trace to Langfuse
}

main().catch(console.error);
```

----------------------------------------

TITLE: Implementing API Rate Limiting with Vercel KV and Upstash Ratelimit (TypeScript)
DESCRIPTION: This snippet demonstrates how to implement API rate limiting for a Next.js API route using Vercel KV as the Redis store and Upstash Ratelimit. It configures a fixed window limiter allowing 5 requests per 30 seconds based on the client's IP address, returning a 429 status code if the limit is exceeded. The API also processes and streams text responses using `@ai-sdk/openai`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/06-rate-limiting.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import kv from '@vercel/kv';
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';
import { Ratelimit } from '@upstash/ratelimit';
import { NextRequest } from 'next/server';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

// Create Rate limit
const ratelimit = new Ratelimit({
  redis: kv,
  limiter: Ratelimit.fixedWindow(5, '30s'),
});

export async function POST(req: NextRequest) {
  // call ratelimit with request ip
  const ip = req.ip ?? 'ip';
  const { success, remaining } = await ratelimit.limit(ip);

  // block the request if unsuccessfull
  if (!success) {
    return new Response('Ratelimited!', { status: 429 });
  }

  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-3.5-turbo'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Generating Text with Chat Prompt using AI SDK and OpenAI (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `generateText` function from the AI SDK to perform a chat completion. It configures the OpenAI `gpt-3.5-turbo` model, sets a maximum token limit, defines a system prompt, and provides a series of user and assistant messages to simulate a conversation. The generated text from the model is then logged to the console.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/11-generate-text-with-chat-prompt.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const result = await generateText({
  model: openai('gpt-3.5-turbo'),
  maxTokens: 1024,
  system: 'You are a helpful chatbot.',
  messages: [
    {
      role: 'user',
      content: 'Hello!',
    },
    {
      role: 'assistant',
      content: 'Hello! How can I help you today?',
    },
    {
      role: 'user',
      content: 'I need help with my computer.',
    },
  ],
});

console.log(result.text);
```

----------------------------------------

TITLE: Streaming Tool Data via Route Handler with streamText (After)
DESCRIPTION: This snippet demonstrates the updated approach for generative UIs using a Next.js API route handler. It leverages `streamText` to stream only the data (props) from tool executions to the client, where `useChat` can then decode and render the UI. Dependencies include `ai`, `@ai-sdk/openai`, `zod`, and custom query utilities.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/10-migrating-to-ui.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
import { z } from 'zod';
import { openai } from '@ai-sdk/openai';
import { getWeather } from '@/utils/queries';
import { streamText } from 'ai';

export async function POST(request) {
  const { messages } = await request.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'you are a friendly assistant!',
    messages,
    tools: {
      displayWeather: {
        description: 'Display the weather for a location',
        parameters: z.object({
          latitude: z.number(),
          longitude: z.number()
        }),
        execute: async function ({ latitude, longitude }) {
          const props = await getWeather({ latitude, longitude });
          return props;
        }
      }
    }
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Initializing a Provider Registry with Multiple AI Providers (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a central registry for AI providers and their models using `createProviderRegistry`. It shows how to register providers like Anthropic with a default setup and OpenAI with a custom setup, including an API key. This setup allows for easy access to models via a unified interface.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/40-provider-registry.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { anthropic } from '@ai-sdk/anthropic';
import { createOpenAI } from '@ai-sdk/openai';
import { createProviderRegistry } from 'ai';

export const registry = createProviderRegistry({
  // register provider with prefix and default setup:
  anthropic,

  // register provider with prefix and custom setup:
  openai: createOpenAI({
    apiKey: process.env.OPENAI_API_KEY,
  }),
});
```

----------------------------------------

TITLE: Implementing Uppercase Text Transformation with Vercel AI (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a custom TransformStream that converts all 'text-delta' chunks in a stream to uppercase. The transformation function receives available tools and returns a function used to process the stream, ensuring text modifications are applied dynamically.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/05-generating-text.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
const upperCaseTransform =
  <TOOLS extends ToolSet>() =>
  (options: { tools: TOOLS; stopStream: () => void }) =>
    new TransformStream<TextStreamPart<TOOLS>, TextStreamPart<TOOLS>>({
      transform(chunk, controller) {
        controller.enqueue(
          // for text-delta chunks, convert the text to uppercase:
          chunk.type === 'text-delta'
            ? { ...chunk, textDelta: chunk.textDelta.toUpperCase() }
            : chunk,
        );
      },
    });
```

----------------------------------------

TITLE: Configuring OpenAI API Key in .env.local
DESCRIPTION: This snippet shows how to add the OpenAI API key to the `.env.local` file. Replace `xxxxxxxxx` with your actual OpenAI API key. The AI SDK's OpenAI Provider automatically uses this environment variable for authentication with the OpenAI service.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_6

LANGUAGE: Env
CODE:
```
OPENAI_API_KEY=xxxxxxxxx
```

----------------------------------------

TITLE: Implementing a Chat UI with useChat Hook (TSX/React)
DESCRIPTION: A React component using the useChat hook from @ai-sdk/react to manage chat state, display messages (including text and reasoning parts), handle input changes, and submit new messages via a form.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/20-sonnet-3-7.mdx#_snippet_5

LANGUAGE: TSX
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.parts.map((part, index) => {
            // text parts:
            if (part.type === 'text') {
              return <div key={index}>{part.text}</div>;
            }
            // reasoning parts:
            if (part.type === 'reasoning') {
              return (
                <pre key={index}>
                  {part.details.map(detail =>
                    detail.type === 'text' ? detail.text : '<redacted>',
                  )}
                </pre>
              );
            }
          })}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Defining Multi-Step Tools with streamText API (TypeScript/Next.js)
DESCRIPTION: This Next.js API route (`/api/chat`) uses the `ai` module's `streamText` function to process chat messages and execute AI tools. It defines `getLocation` and `getWeather` tools with Zod schemas for parameter validation, demonstrating how to chain tool invocations (e.g., getting location then weather) within the `maxSteps` context set on the client.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/72-call-tools-multiple-steps.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { ToolInvocation, streamText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

interface Message {
  role: 'user' | 'assistant';
  content: string;
  toolInvocations?: ToolInvocation[];
}

function getLocation({ lat, lon }) {
  return { lat: 37.7749, lon: -122.4194 };
}

function getWeather({ lat, lon, unit }) {
  return { value: 25, description: 'Sunny' };
}

export async function POST(req: Request) {
  const { messages }: { messages: Message[] } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'You are a helpful assistant.',
    messages,
    tools: {
      getLocation: {
        description: 'Get the location of the user',
        parameters: z.object({}),
        execute: async () => {
          const { lat, lon } = getLocation();
          return `Your location is at latitude ${lat} and longitude ${lon}`;
        },
      },
      getWeather: {
        description: 'Get the weather for a location',
        parameters: z.object({
          lat: z.number().describe('The latitude of the location'),
          lon: z.number().describe('The longitude of the location'),
          unit: z
            .enum(['C', 'F'])
            .describe('The unit to display the temperature in'),
        }),
        execute: async ({ lat, lon, unit }) => {
          const { value, description } = getWeather({ lat, lon, unit });
          return `It is currently ${value}°${unit} and ${description}!`;
        },
      },
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Implementing Chat Interface with AI SDK and RSC
DESCRIPTION: This React client component sets up a chat interface allowing users to send messages and receive streamed responses from an AI assistant. It manages input state, displays messages, and uses `ai/rsc`'s `useActions` hook to interact with server actions for message submission.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/121-stream-assistant-response-with-tools.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { ClientMessage, submitMessage } from './actions';
import { useActions } from 'ai/rsc';

export default function Home() {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState<ClientMessage[]>([]);
  const { submitMessage } = useActions();

  const handleSubmission = async () => {
    setMessages(currentMessages => [
      ...currentMessages,
      {
        id: '123',
        status: 'user.message.created',
        text: input,
        gui: null,
      },
    ]);

    const response = await submitMessage(input);
    setMessages(currentMessages => [...currentMessages, response]);
    setInput('');
  };

  return (
    <div className="flex flex-col-reverse">
      <div className="flex flex-row gap-2 p-2 bg-zinc-100 w-full">
        <input
          className="bg-zinc-100 w-full p-2 outline-none"
          value={input}
          onChange={event => setInput(event.target.value)}
          placeholder="Ask a question"
          onKeyDown={event => {
            if (event.key === 'Enter') {
              handleSubmission();
            }
          }}
        />
        <button
          className="p-2 bg-zinc-900 text-zinc-100 rounded-md"
          onClick={handleSubmission}
        >
          Send
        </button>
      </div>

      <div className="flex flex-col h-[calc(100dvh-56px)] overflow-y-scroll">
        <div>
          {messages.map(message => (
            <div key={message.id} className="flex flex-col gap-1 border-b p-2">
              <div className="flex flex-row justify-between">
                <div className="text-sm text-zinc-500">{message.status}</div>
              </div>
              <div className="flex flex-col gap-2">{message.gui}</div>
              <div>{message.text}</div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining MCP Tools Schema with Zod (TypeScript)
DESCRIPTION: Illustrates how to explicitly define the schemas for MCP tools using Zod, allowing control over which tools are loaded and providing TypeScript type safety and IDE autocompletion for tool parameters.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/15-tools-and-tool-calling.mdx#_snippet_24

LANGUAGE: typescript
CODE:
```
import { z } from 'zod';

const tools = await mcpClient.tools({
  schemas: {
    'get-data': {
      parameters: z.object({
        query: z.string().describe('The data query'),
        format: z.enum(['json', 'text']).optional(),
      }),
    },
    // For tools with zero arguments, you should use an empty object:
    'tool-with-no-args': {
      parameters: z.object({}),
    },
  },
});
```

----------------------------------------

TITLE: Streaming React UI Components with AI SDK RSC in TypeScript
DESCRIPTION: This server action illustrates how to stream React components from the server to the client using `createStreamableUI`. It initializes a stream with a loading state, then updates it with the final UI component after a simulated delay, allowing for dynamic and progressive rendering of UI elements.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/05-streaming-values.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use server';

import { createStreamableUI } from 'ai/rsc';

export async function getWeather() {
  const weatherUI = createStreamableUI();

  weatherUI.update(<div style={{ color: 'gray' }}>Loading...</div>);

  setTimeout(() => {
    weatherUI.done(<div>It&apos;s a sunny day!</div>);
  }, 1000);

  return weatherUI.value;
}
```

----------------------------------------

TITLE: Using File Inputs with Google Generative AI
DESCRIPTION: This code snippet demonstrates how to use file inputs, such as PDF files, with the Google Generative AI provider. It reads a PDF file and passes it as content to the `generateText` function.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/15-google-generative-ai.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
import { google } from '@ai-sdk/google';
import { generateText } from 'ai';

const result = await generateText({
  model: google('gemini-1.5-flash'),
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'What is an embedding model according to this document?',
        },
        {
          type: 'file',
          data: fs.readFileSync('./data/ai.pdf'),
          mimeType: 'application/pdf',
        },
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Forcing Structured Outputs with Answer Tool in AI SDK
DESCRIPTION: This snippet demonstrates how to use an 'answer' tool with 'toolChoice: 'required'' to force the LLM to provide a structured final output. It also includes a 'calculate' tool for mathematical evaluations. The 'answer' tool has no 'execute' function, causing the agent to terminate upon its invocation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, tool } from 'ai';
import 'dotenv/config';
import { z } from 'zod';

const { toolCalls } = await generateText({
  model: openai('gpt-4o-2024-08-06', { structuredOutputs: true }),
  tools: {
    calculate: tool({
      description:
        'A tool for evaluating mathematical expressions. Example expressions: ' +
        "'1.2 * (2 + 4.5)', '12.7 cm to inch', 'sin(45 deg) ^ 2'.",
      parameters: z.object({ expression: z.string() }),
      execute: async ({ expression }) => mathjs.evaluate(expression),
    }),
    // answer tool: the LLM will provide a structured answer
    answer: tool({
      description: 'A tool for providing the final answer.',
      parameters: z.object({
        steps: z.array(
          z.object({
            calculation: z.string(),
            reasoning: z.string(),
          }),
        ),
        answer: z.string(),
      }),
      // no execute function - invoking it will terminate the agent
    }),
  },
  toolChoice: 'required',
  maxSteps: 10,
  system:
    'You are solving math problems. ' +
    'Reason step by step. ' +
    'Use the calculator when necessary. ' +
    'The calculator can only do simple additions, subtractions, multiplications, and divisions. ' +
    'When you give the final answer, provide an explanation for how you got it.',
  prompt:
    'A taxi driver earns $9461 per 1-hour work. ' +
    'If he works 12 hours a day and in 1 hour he uses 14-liters petrol with price $134 for 1-liter. ' +
    'How much money does he earn in one day?',
});

console.log(`FINAL TOOL CALLS: ${JSON.stringify(toolCalls, null, 2)}`);
```

----------------------------------------

TITLE: Recommending Items with Context using Crosshatch
DESCRIPTION: This example demonstrates using the `streamObject` function with Crosshatch to re-rank a list of items based on a user's recent purchase history. It queries past orders from 'personalTimeline' via the `replace` option, using this context to inform the model's re-ranking logic and outputting structured JSON according to a defined schema.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/21-crosshatch.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { streamObject } from 'ai';
import createCrosshatch from '@crosshatch/ai-provider';
const crosshatch = createCrosshatch();

const itemSummaries = [...]; // list of items
const ids = (itemSummaries?.map(({ itemId }) => itemId) ?? []) as string[];

const { elementStream } = streamObject({
  output: "array",
  mode: "json",
  model: crosshatch.languageModel("gpt-4o-mini", {
    token,
    replace: {
      "orders": {
        select: ["originalTimestamp", "entity_name", "order_total", "order_summary"],
        from: "personalTimeline",
        where: [{ field: "event", op: "=", value: "purchased" }],
        orderBy: [{ field: "originalTimestamp", dir: "desc" }],
        limit: 5
      }
    }
  }),
  system: `Rerank the following items based on alignment with users recent purchases {orders}`,
  messages: [{role: "user", content: `Heres a list of item: ${JSON.stringify(itemSummaries)}`}],
  schema: jsonSchema<{ id: string; reason: string }>({
    type: "object",
    properties: {
      id: { type: "string", enum: ids },
      reason: { type: "string", description: "Explain your ranking." }
    }
  })
});
```

----------------------------------------

TITLE: Implementing Multi-Step Tool Usage with maxSteps in TypeScript
DESCRIPTION: This example illustrates how to create an AI agent that solves math problems by iteratively using a calculator tool. It leverages the `maxSteps` parameter in `generateText` to allow the LLM to break down complex tasks and call external tools (`mathjs`) until a solution is found or the step limit is reached. The `structuredOutputs` option is used for the model.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, tool } from 'ai';
import * as mathjs from 'mathjs';
import { z } from 'zod';

const { text: answer } = await generateText({
  model: openai('gpt-4o-2024-08-06', { structuredOutputs: true }),
  tools: {
    calculate: tool({
      description:
        'A tool for evaluating mathematical expressions. ' +
        'Example expressions: ' +
        "'1.2 * (2 + 4.5)', '12.7 cm to inch', 'sin(45 deg) ^ 2'.",
      parameters: z.object({ expression: z.string() }),
      execute: async ({ expression }) => mathjs.evaluate(expression),
    }),
  },
  maxSteps: 10,
  system:
    'You are solving math problems. ' +
    'Reason step by step. ' +
    'Use the calculator when necessary. ' +
    'When you give the final answer, ' +
    'provide an explanation for how you arrived at it.',
  prompt:
    'A taxi driver earns $9461 per 1-hour of work. ' +
    'If he works 12 hours a day and in 1 hour ' +
    'he uses 12 liters of petrol with a price  of $134 for 1 liter. ' +
    'How much money does he earn in one day?',
});

console.log(`ANSWER: ${answer}`);
```

----------------------------------------

TITLE: Implementing Streaming AI Chatbot (TypeScript)
DESCRIPTION: This TypeScript code sets up a command-line chatbot using the AI SDK. It initializes a readline interface for user input, maintains conversation history, streams responses from the OpenAI model, and prints the assistant's output to the terminal in real-time.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/06-nodejs.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { CoreMessage, streamText } from 'ai';
import dotenv from 'dotenv';
import * as readline from 'node:readline/promises';

dotenv.config();

const terminal = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const messages: CoreMessage[] = [];

async function main() {
  while (true) {
    const userInput = await terminal.question('You: ');

    messages.push({ role: 'user', content: userInput });

    const result = streamText({
      model: openai('gpt-4o'),
      messages,
    });

    let fullResponse = '';
    process.stdout.write('\nAssistant: ');
    for await (const delta of result.textStream) {
      fullResponse += delta;
      process.stdout.write(delta);
    }
    process.stdout.write('\n\n');

    messages.push({ role: 'assistant', content: fullResponse });
  }
}

main().catch(console.error);
```

----------------------------------------

TITLE: Consuming Streamable Value in React Component (TypeScript)
DESCRIPTION: This example shows how to use the `useStreamableValue` hook within a React functional component to consume a streamable value passed via props. It effectively manages and displays the loading, error, and final data states of the asynchronous stream, providing a clear user interface based on the stream's progress.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/03-ai-sdk-rsc/11-use-streamable-value.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
function MyComponent({ streamableValue }) {
  const [data, error, pending] = useStreamableValue(streamableValue);

  if (pending) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return <div>Data: {data}</div>;
}
```

----------------------------------------

TITLE: Creating a Server-Side Chat API Endpoint with AI SDK
DESCRIPTION: This server-side API route (`/api/chat`) handles incoming chat messages. It uses the Vercel AI SDK's `generateText` function with the `@ai-sdk/openai` package and the `gpt-4` model to generate an AI response based on the provided conversation history. The response, including the assistant's message, is then returned as JSON.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/11-generate-text-with-chat-prompt.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { CoreMessage, generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

export async function POST(req: Request) {
  const { messages }: { messages: CoreMessage[] } = await req.json();

  const { response } = await generateText({
    model: openai('gpt-4'),
    system: 'You are a helpful assistant.',
    messages,
  });

  return Response.json({ messages: response.messages });
}
```

----------------------------------------

TITLE: Defining Custom Tools for AI Chatbot Route Handler with AI SDK
DESCRIPTION: This snippet demonstrates how to define and integrate multiple custom tools (`weather` and `convertFahrenheitToCelsius`) within an AI SDK route handler. These tools, defined using `ai`'s `tool` function and `zod` for parameter validation, allow the AI model to perform specific actions like fetching weather data or converting units, enhancing its ability to respond to complex queries.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';

export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      weather: tool({
        description: 'Get the weather in a location (fahrenheit)',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for'),
        }),
        execute: async ({ location }) => {
          const temperature = Math.round(Math.random() * (90 - 32) + 32);
          return {
            location,
            temperature,
          };
        },
      }),
      convertFahrenheitToCelsius: tool({
        description: 'Convert a temperature in fahrenheit to celsius',
        parameters: z.object({
          temperature: z
            .number()
            .describe('The temperature in fahrenheit to convert'),
        }),
        execute: async ({ temperature }) => {
          const celsius = Math.round((temperature - 32) * (5 / 9));
          return {
            celsius,
          };
        },
      }),
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Creating Chat Route Handler with Anthropic and AI SDK in Next.js
DESCRIPTION: This TypeScript route handler defines a POST endpoint for handling chat requests. It uses `@ai-sdk/anthropic` to stream text from the Claude-4-sonnet model, forwarding messages from the client and optionally sending reasoning tokens back. It's essential for the backend processing of AI chat interactions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/18-claude-4.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { anthropic, AnthropicProviderOptions } from '@ai-sdk/anthropic';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: anthropic('claude-4-sonnet-20250514'),
    messages,
    headers: {
      'anthropic-beta': 'interleaved-thinking-2025-05-14',
    },
    providerOptions: {
      anthropic: {
        thinking: { type: 'enabled', budgetTokens: 15000 },
      } satisfies AnthropicProviderOptions,
    },
  });

  return result.toDataStreamResponse({
    sendReasoning: true,
  });
}
```

----------------------------------------

TITLE: Creating a Chat API Route with AI SDK and Anthropic (TypeScript)
DESCRIPTION: Defines a Next.js API route handler (POST) that receives messages, uses streamText from the AI SDK with the Anthropic provider (Claude 3.7 Sonnet), and streams the response back to the client, optionally including reasoning tokens.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/20-sonnet-3-7.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { anthropic, AnthropicProviderOptions } from '@ai-sdk/anthropic';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: anthropic('claude-3-7-sonnet-20250219'),
    messages,
    providerOptions: {
      anthropic: {
        thinking: { type: 'enabled', budgetTokens: 12000 },
      } satisfies AnthropicProviderOptions,
    },
  });

  return result.toDataStreamResponse({
    sendReasoning: true,
  });
}
```

----------------------------------------

TITLE: Create Next.js API Route for Chat
DESCRIPTION: Sets up a Next.js API route handler to process incoming chat messages, stream text responses from the OpenAI model, and return the result as a data stream response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/22-gpt-4-5.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

// Allow responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4.5-preview'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Implementing AI SDK Chat UI with useChat Hook (TSX)
DESCRIPTION: Implements a simple chat interface in a Next.js page using the @ai-sdk/react useChat hook. It displays messages, handles user input, submits messages to the API route, and shows potential reasoning tokens.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/25-r1.mdx#_snippet_5

LANGUAGE: TSX
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.reasoning && <pre>{message.reasoning}</pre>}
          {message.content}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Generating Basic Text with AI SDK's generateText (TypeScript)
DESCRIPTION: This snippet demonstrates the fundamental usage of the `generateText` function from the AI SDK. It's suitable for non-interactive scenarios where a complete text response is needed, such as drafting emails or summarizing content. The function takes a model and a prompt, returning the generated text.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/05-generating-text.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';

const { text } = await generateText({
  model: yourModel,
  prompt: 'Write a vegetarian lasagna recipe for 4 people.',
});
```

----------------------------------------

TITLE: Implementing Human-in-the-Loop Tool Confirmation in a React Chat UI (TSX)
DESCRIPTION: This React component (`Chat`) uses the `@ai-sdk/react`'s `useChat` hook to build a conversational UI. It integrates custom logic to identify AI tool calls that require human confirmation using `getToolsRequiringConfirmation` and `APPROVAL` values. The UI dynamically renders 'Yes'/'No' buttons for user approval, disabling the chat input while a confirmation is pending, and uses `addToolResult` to send the user's decision back to the AI system.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/75-human-in-the-loop.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
'use client';

import { Message, useChat } from '@ai-sdk/react';
import {
  APPROVAL,
  getToolsRequiringConfirmation,
} from '../api/use-chat-human-in-the-loop/utils';
import { tools } from '../api/use-chat-human-in-the-loop/tools';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit, addToolResult } =
    useChat({
      maxSteps: 5,
    });

  const toolsRequiringConfirmation = getToolsRequiringConfirmation(tools);

  // used to disable input while confirmation is pending
  const pendingToolCallConfirmation = messages.some((m: Message) =>
    m.parts?.some(
      part =>
        part.type === 'tool-invocation' &&
        part.toolInvocation.state === 'call' &&
        toolsRequiringConfirmation.includes(part.toolInvocation.toolName),
    ),
  );

  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      {messages?.map((m: Message) => (
        <div key={m.id} className="whitespace-pre-wrap">
          <strong>{`${m.role}: `}</strong>
          {m.parts?.map((part, i) => {
            switch (part.type) {
              case 'text':
                return <div key={i}>{part.text}</div>;
              case 'tool-invocation':
                const toolInvocation = part.toolInvocation;
                const toolCallId = toolInvocation.toolCallId;
                const dynamicInfoStyles = 'font-mono bg-gray-100 p-1 text-sm';

                // render confirmation tool (client-side tool with user interaction)
                if (
                  toolsRequiringConfirmation.includes(
                    toolInvocation.toolName,
                  ) &&
                  toolInvocation.state === 'call'
                ) {
                  return (
                    <div key={toolCallId} className="text-gray-500">
                      Run{' '}
                      <span className={dynamicInfoStyles}>
                        {toolInvocation.toolName}
                      </span>{' '}
                      with args:{' '}
                      <span className={dynamicInfoStyles}>
                        {JSON.stringify(toolInvocation.args)}
                      </span>
                      <div className="flex gap-2 pt-2">
                        <button
                          className="px-4 py-2 font-bold text-white bg-blue-500 rounded hover:bg-blue-700"
                          onClick={() =>
                            addToolResult({
                              toolCallId,
                              result: APPROVAL.YES,
                            })
                          }
                        >
                          Yes
                        </button>
                        <button
                          className="px-4 py-2 font-bold text-white bg-red-500 rounded hover:bg-red-700"
                          onClick={() =>
                            addToolResult({
                              toolCallId,
                              result: APPROVAL.NO,
                            })
                          }
                        >
                          No
                        </button>
                      </div>
                    </div>
                  );
                }
            }
          })}
          <br />
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          disabled={pendingToolCallConfirmation}
          className="fixed bottom-0 w-full max-w-md p-2 mb-8 border border-gray-300 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Chat UI with AI SDK useChat Hook (Next.js Client)
DESCRIPTION: This Next.js client component (app/page.tsx) demonstrates building a chat interface using the @ai-sdk/react useChat hook. It manages the message list, handles user input changes, and submits messages to the backend API route.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_7

LANGUAGE: TSX
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.content}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Generating Text with OpenAI GPT-3.5 Turbo in TypeScript
DESCRIPTION: This snippet demonstrates how to use the `generateText` function from the AI SDK to generate text. It imports necessary modules, initializes the OpenAI GPT-3.5 Turbo model, and sends a simple prompt. The generated text result is then logged to the console. This requires the `@ai-sdk/openai` package and an OpenAI API key.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/10-generate-text.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const result = await generateText({
  model: openai('gpt-3.5-turbo'),
  prompt: 'Why is the sky blue?',
});

console.log(result);
```

----------------------------------------

TITLE: Enabling Search Grounding with Gemini Models
DESCRIPTION: This snippet demonstrates how to use Gemini models with search grounding enabled to access the latest information via Google search. It shows how to generate text, retrieve sources, and access provider-specific metadata like grounding and safety ratings.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/56-web-search-agent.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import { google } from '@ai-sdk/google';
import { generateText } from 'ai';

const { text, sources, providerMetadata } = await generateText({
  model: google('gemini-1.5-pro', {
    useSearchGrounding: true,
  }),
  prompt:
    'List the top 5 San Francisco news from the past week.' +
    'You must include the date of each article.',
});

console.log(text);
console.log(sources);

// access the grounding metadata.
const metadata = providerMetadata?.google;
const groundingMetadata = metadata?.groundingMetadata;
const safetyRatings = metadata?.safetyRatings;
```

----------------------------------------

TITLE: Creating an AI Chat API Route with AI SDK
DESCRIPTION: This TypeScript code defines a POST API route (`/api/chat`) for an Expo application. It handles incoming chat messages, uses the AI SDK's `streamText` function with OpenAI's `gpt-4o` model to generate a response, and streams the result back to the client as an `application/octet-stream`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/07-expo.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
  });

  return result.toDataStreamResponse({
    headers: {
      'Content-Type': 'application/octet-stream',
      'Content-Encoding': 'none',
    },
  });
}
```

----------------------------------------

TITLE: Handling Chat API Route with AI SDK and DeepInfra (Next.js)
DESCRIPTION: This Next.js API route (app/api/chat/route.ts) processes incoming chat messages using the AI SDK. It utilizes the @ai-sdk/deepinfra provider to stream text responses from the Llama 3.1 model, setting a maximum duration for the response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_6

LANGUAGE: TSX
CODE:
```
import { streamText } from 'ai';
import { deepinfra } from '@ai-sdk/deepinfra';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: deepinfra('meta-llama/Meta-Llama-3.1-70B-Instruct'),
    system: 'You are a helpful assistant.',
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: AI Prompt for SQL Query Explanation (Text)
DESCRIPTION: Defines the role and instructions for an AI model to explain SQL queries. It includes the database schema and specifies the required output format, breaking down the explanation by query section.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/04-natural-language-postgres.mdx#_snippet_10

LANGUAGE: Text
CODE:
```
You are a SQL (postgres) expert. Your job is to explain to the user write a SQL query you wrote to retrieve the data they asked for. The table schema is as follows:
unicorns (
  id SERIAL PRIMARY KEY,
  company VARCHAR(255) NOT NULL UNIQUE,
  valuation DECIMAL(10, 2) NOT NULL,
  date_joined DATE,
  country VARCHAR(255) NOT NULL,
  city VARCHAR(255) NOT NULL,
  industry VARCHAR(255) NOT NULL,
  select_investors TEXT NOT NULL
);

When you explain you must take a section of the query, and then explain it. Each "section" should be unique. So in a query like: "SELECT * FROM unicorns limit 20", the sections could be "SELECT *", "FROM UNICORNS", "LIMIT 20".
If a section doesn't have any explanation, include it, but leave the explanation empty.
```

----------------------------------------

TITLE: Handling Assistant Messages in Next.js API Route (TSX)
DESCRIPTION: This server-side API route handles incoming messages from the client, interacts with the OpenAI Assistant API, and streams responses back. It uses the `AssistantResponse` function from `ai` to manage the streaming process, creating new threads if necessary and forwarding the run stream from OpenAI to the client.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/120-stream-assistant-response.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import OpenAI from 'openai';
import { AssistantResponse } from 'ai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY || '',
});

export async function POST(req: Request) {
  const input: {
    threadId: string | null;
    message: string;
  } = await req.json();

  const threadId = input.threadId ?? (await openai.beta.threads.create({})).id;

  const createdMessage = await openai.beta.threads.messages.create(threadId, {
    role: 'user',
    content: input.message,
  });

  return AssistantResponse(
    { threadId, messageId: createdMessage.id },
    async ({ forwardStream }) => {
      const runStream = openai.beta.threads.runs.stream(threadId, {
        assistant_id:
          process.env.ASSISTANT_ID ??
          (() => {
            throw new Error('ASSISTANT_ID environment is not set');
          })(),
      });

      await forwardStream(runStream);
    },
  );
}
```

----------------------------------------

TITLE: Accessing Multi-modal Image Outputs from Language Models in AI SDK (TypeScript)
DESCRIPTION: This snippet illustrates how to generate and access image outputs from multi-modal language models like Google `gemini-2.0-flash-exp` using `generateText`. It shows how to configure `providerOptions` to request image modalities and then iterate through the `files` property of the `result` to process generated images, which can be accessed as base64 strings or Uint8Array binary data.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/35-image-generation.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
import { google } from '@ai-sdk/google';
import { generateText } from 'ai';

const result = await generateText({
  model: google('gemini-2.0-flash-exp'),
  providerOptions: {
    google: { responseModalities: ['TEXT', 'IMAGE'] }
  },
  prompt: 'Generate an image of a comic cat'
});

for (const file of result.files) {
  if (file.mimeType.startsWith('image/')) {
    // The file object provides multiple data formats:
    // Access images as base64 string, Uint8Array binary data, or check type
    // - file.base64: string (data URL format)
    // - file.uint8Array: Uint8Array (binary data)
    // - file.mimeType: string (e.g. "image/png")
  }
}
```

----------------------------------------

TITLE: Generating Text with User, Assistant Tool Call, and Tool Result Messages
DESCRIPTION: This comprehensive example demonstrates a full interaction flow involving user input, an assistant's `tool-call` message, and a subsequent `tool` message containing the `tool-result`. It shows how to pass tool execution results back to the model for continued conversation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/03-prompts.mdx#_snippet_17

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: yourModel,
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'How many calories are in this block of cheese?',
        },
        { type: 'image', image: fs.readFileSync('./data/roquefort.jpg') },
      ],
    },
    {
      role: 'assistant',
      content: [
        {
          type: 'tool-call',
          toolCallId: '12345',
          toolName: 'get-nutrition-data',
          args: { cheese: 'Roquefort' },
        },
        // there could be more tool calls here (parallel calling)
      ],
    },
    {
      role: 'tool',
      content: [
        {
          type: 'tool-result',
          toolCallId: '12345', // needs to match the tool call id
          toolName: 'get-nutrition-data',
          result: {
            name: 'Cheese, roquefort',
            calories: 369,
            fat: 31,
            protein: 22,
          },
        },
        // there could be more tool results here (parallel calling)
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Integrating AI Tool Invocations with React Chat UI
DESCRIPTION: This 'page.tsx' file demonstrates how to integrate AI tool outputs into a React chat interface using '@ai-sdk/react'. It processes messages from the 'useChat' hook, specifically checking 'message.toolInvocations' to dynamically render UI. When the 'displayWeather' tool is invoked and its state is 'result', the 'Weather' component is rendered with the tool's output; otherwise, a loading message is shown.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/04-generative-user-interfaces.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';
import { Weather } from '@/components/weather';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(message => (
        <div key={message.id}>
          <div>{message.role === 'user' ? 'User: ' : 'AI: '}</div>
          <div>{message.content}</div>

          <div>
            {message.toolInvocations?.map(toolInvocation => {
              const { toolName, toolCallId, state } = toolInvocation;

              if (state === 'result') {
                if (toolName === 'displayWeather') {
                  const { result } = toolInvocation;
                  return (
                    <div key={toolCallId}>
                      <Weather {...result} />
                    </div>
                  );
                }
              } else {
                return (
                  <div key={toolCallId}>
                    {toolName === 'displayWeather' ? (
                      <div>Loading weather...</div>
                    ) : null}
                  </div>
                );
              }
            })}
          </div>
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Type a message..."
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Creating Next.js Chat API Route with AI SDK (TSX)
DESCRIPTION: Provides a Next.js API route handler (`app/api/chat/route.ts`) that uses the AI SDK `streamText` function to handle incoming chat messages. It imports the necessary modules, sets `maxDuration`, and defines a POST handler to process messages and stream the response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/24-o3.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

// Allow responses up to 5 minutes
export const maxDuration = 300;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('o3-mini'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Generating Structured Objects with ChromeAI
DESCRIPTION: This example demonstrates using `generateObject` with ChromeAI to produce structured JSON output based on a Zod schema. It prompts the model to generate a lasagna recipe, ensuring the output conforms to the defined `recipe` object structure, which is useful for integrating AI responses into applications requiring specific data formats.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/04-chrome-ai.mdx#_snippet_7

LANGUAGE: javascript
CODE:
```
import { generateObject } from 'ai';
import { chromeai } from 'chrome-ai';
import { z } from 'zod';

const { object } = await generateObject({
  model: chromeai('text'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(
        z.object({
          name: z.string(),
          amount: z.string()
        })
      ),
      steps: z.array(z.string())
    })
  }),
  prompt: 'Generate a lasagna recipe.'
});

console.log(object);
// { recipe: {...} }
```

----------------------------------------

TITLE: Configuring AI Model with maxSteps and Tool Usage in TypeScript
DESCRIPTION: This TypeScript snippet demonstrates how to configure an AI model using `@ai-sdk/openai` to enable multi-step interactions and log intermediate steps. It sets `maxSteps` to 5 for a `streamText` call and includes an `onStepFinish` callback to log each step. It also defines a `weather` tool for fetching location-based temperature, showcasing how the model can use tools to respond to user queries.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/06-nodejs.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { CoreMessage, streamText, tool } from 'ai';
import dotenv from 'dotenv';
import { z } from 'zod';
import * as readline from 'node:readline/promises';

dotenv.config();

const terminal = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const messages: CoreMessage[] = [];

async function main() {
  while (true) {
    const userInput = await terminal.question('You: ');

    messages.push({ role: 'user', content: userInput });

    const result = streamText({
      model: openai('gpt-4o'),
      messages,
      tools: {
        weather: tool({
          description: 'Get the weather in a location (in Celsius)',
          parameters: z.object({
            location: z
              .string()
              .describe('The location to get the weather for'),
          }),
          execute: async ({ location }) => ({
            location,
            temperature: Math.round((Math.random() * 30 + 5) * 10) / 10 // Random temp between 5°C and 35°C
          })
        })
      },
      maxSteps: 5,
      onStepFinish: step => {
        console.log(JSON.stringify(step, null, 2));
      }
    });

    let fullResponse = '';
    process.stdout.write('\nAssistant: ');
    for await (const delta of result.textStream) {
      fullResponse += delta;
      process.stdout.write(delta);
    }
    process.stdout.write('\n\n');

    messages.push({ role: 'assistant', content: fullResponse });
  }
}

main().catch(console.error);
```

----------------------------------------

TITLE: Displaying AI Tool Invocations in React UI (TSX)
DESCRIPTION: This React component updates the chat UI to correctly display different types of message parts received from the AI model. It iterates through `message.parts` and renders `text` parts as standard text, while `tool-invocation` parts are displayed as a formatted JSON string of the tool call and its result. This allows users to see when and how the AI model utilizes defined tools.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/03-nextjs-pages-router.mdx#_snippet_11

LANGUAGE: TSX
CODE:
```
import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto stretch">
      {messages.map(message => (
        <div key={message.id} className="whitespace-pre-wrap">
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.parts.map((part, i) => {
            switch (part.type) {
              case 'text':
                return <div key={`${message.id}-${i}`}>{part.text}</div>;
              case 'tool-invocation':
                return (
                  <pre key={`${message.id}-${i}`}>
                    {JSON.stringify(part.toolInvocation, null, 2)}
                  </pre>
                );
            }
          })}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed dark:bg-zinc-900 bottom-0 w-full max-w-md p-2 mb-8 border border-zinc-300 dark:border-zinc-800 rounded shadow-xl"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Creating Server Action with streamUI and Tools (TSX)
DESCRIPTION: Defines a Next.js Server Action (`streamComponent`) that utilizes the AI SDK's `streamUI` to interact with an AI model (gpt-4o). It includes a `getWeather` tool definition with loading and rendering components (`LoadingComponent`, `WeatherComponent`) to simulate fetching and displaying weather data based on model output. Requires dependencies like `ai/rsc`, `@ai-sdk/openai`, and `zod`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/02-streaming-react-components.mdx#_snippet_2

LANGUAGE: TSX
CODE:
```
'use server';

import { streamUI } from 'ai/rsc';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const LoadingComponent = () => (
  <div className="animate-pulse p-4">getting weather...</div>
);

const getWeather = async (location: string) => {
  await new Promise(resolve => setTimeout(resolve, 2000));
  return '82°F️ ☀️';
};

interface WeatherProps {
  location: string;
  weather: string;
}

const WeatherComponent = (props: WeatherProps) => (
  <div className="border border-neutral-200 p-4 rounded-lg max-w-fit">
    The weather in {props.location} is {props.weather}
  </div>
);

export async function streamComponent() {
  const result = await streamUI({
    model: openai('gpt-4o'),
    prompt: 'Get the weather for San Francisco',
    text: ({ content }) => <div>{content}</div>,
    tools: {
      getWeather: {
        description: 'Get the weather for a location',
        parameters: z.object({
          location: z.string(),
        }),
        generate: async function* ({ location }) {
          yield <LoadingComponent />;
          const weather = await getWeather(location);
          return <WeatherComponent weather={weather} location={location} />;
        },
      },
    },
  });

  return result.value;
}
```

----------------------------------------

TITLE: Generating AI Response Stream on Server with AI SDK RSC
DESCRIPTION: This server-side `generateResponse` function uses the AI SDK to stream text responses. It initializes a `streamableValue` and an asynchronous IIFE (Immediately Invoked Function Expression) to call `streamText` with `openai('gpt-4o')`. As text chunks are received from the AI model, `stream.update()` sends them to the client. Finally, `stream.done()` signals completion, and `stream.value` is returned for client consumption.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/06-loading-state.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
'use server';

import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { createStreamableValue } from 'ai/rsc';

export async function generateResponse(prompt: string) {
  const stream = createStreamableValue();

  (async () => {
    const { textStream } = streamText({
      model: openai('gpt-4o'),
      prompt,
    });

    for await (const text of textStream) {
      stream.update(text);
    }

    stream.done();
  })();

  return stream.value;
}
```

----------------------------------------

TITLE: Streaming Array of Objects with streamObject and array Output (TypeScript)
DESCRIPTION: This snippet illustrates how to use `streamObject` with the `array` output strategy to generate and stream a list of structured objects. It defines a Zod schema for individual array elements (e.g., hero descriptions) and then iterates over the `elementStream` to process each generated object as it becomes available, suitable for generating multiple structured items.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/10-generating-structured-data.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamObject } from 'ai';
import { z } from 'zod';

const { elementStream } = streamObject({
  model: openai('gpt-4-turbo'),
  output: 'array',
  schema: z.object({
    name: z.string(),
    class: z
      .string()
      .describe('Character class, e.g. warrior, mage, or thief.'),
    description: z.string(),
  }),
  prompt: 'Generate 3 hero descriptions for a fantasy role playing game.',
});

for await (const hero of elementStream) {
  console.log(hero);
}
```

----------------------------------------

TITLE: Implementing Chat UI with Tool Handling in React
DESCRIPTION: This React component demonstrates client-side chat functionality using the `@ai-sdk/react`'s `useChat` hook. It handles user input, sends messages to a backend API (`/api/chat`), and displays streamed assistant responses, including automatic rendering of tool calls. The `maxSteps` parameter limits the number of backend LLM calls to prevent infinite loops.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/70-call-tools.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, setInput, append } = useChat({
    api: '/api/chat',
    maxSteps: 2,
  });

  return (
    <div>
      <input
        value={input}
        onChange={event => {
          setInput(event.target.value);
        }}
        onKeyDown={async event => {
          if (event.key === 'Enter') {
            append({ content: input, role: 'user' });
          }
        }}
      />

      {messages.map((message, index) => (
        <div key={index}>{message.content}</div>
      ))}
    </div>
  );
}
```

----------------------------------------

TITLE: Generating Text with AI SDK Core (TypeScript)
DESCRIPTION: Example demonstrating how to use the generateText function from the AI SDK Core with the OpenAI provider in a Node.js environment. It shows how to set the model, system prompt, and user prompt, and log the generated text. Requires the OPENAI_API_KEY environment variable.
SOURCE: https://github.com/vercel/ai/blob/main/packages/ai/README.md#_snippet_2

LANGUAGE: ts
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai'; // Ensure OPENAI_API_KEY environment variable is set

const { text } = await generateText({
  model: openai('gpt-4o'),
  system: 'You are a friendly assistant!',
  prompt: 'Why is the sky blue?',
});

console.log(text);
```

----------------------------------------

TITLE: Generating an Object with a Schema (TypeScript)
DESCRIPTION: This example demonstrates how to use `generateObject` to create a structured object based on a Zod schema. It generates a lasagna recipe with a name, ingredients, and steps, then logs the resulting object as a formatted JSON string.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/03-generate-object.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateObject } from 'ai';
import { z } from 'zod';

const { object } = await generateObject({
  model: openai('gpt-4-turbo'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.string()),
      steps: z.array(z.string())
    })
  }),
  prompt: 'Generate a lasagna recipe.'
});

console.log(JSON.stringify(object, null, 2));
```

----------------------------------------

TITLE: Defining AI State Types and Server Action in AI SDK RSC (TypeScript)
DESCRIPTION: Defines the types for `ServerMessage` (AI state) and `ClientMessage` (UI state) used within the AI SDK. It also declares an asynchronous `sendMessage` server action, which is intended to handle user input and interact with the AI model, returning a `ClientMessage`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/03-generative-ui-state.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
// Define the AI state and UI state types
export type ServerMessage = {
  role: 'user' | 'assistant';
  content: string;
};

export type ClientMessage = {
  id: string;
  role: 'user' | 'assistant';
  display: ReactNode;
};

export const sendMessage = async (input: string): Promise<ClientMessage> => {
  "use server"
  ...
}
```

----------------------------------------

TITLE: Defining a Zod Schema for Recipe Data in TypeScript
DESCRIPTION: This TypeScript code defines a Zod schema named `recipeSchema` for structuring recipe data. It specifies that a recipe object must contain a name (string), an array of ingredients (each with a name and amount), and an array of steps (strings). This schema can be used by LLMs to generate structured output or validate tool calls, ensuring data adheres to a predefined format.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/04-tools.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import z from 'zod';

const recipeSchema = z.object({
  recipe: z.object({
    name: z.string(),
    ingredients: z.array(
      z.object({
        name: z.string(),
        amount: z.string(),
      }),
    ),
    steps: z.array(z.string()),
  }),
});
```

----------------------------------------

TITLE: Defining and Registering Tools with AI SDK
DESCRIPTION: This snippet demonstrates how to define and register custom tools like 'weather' and 'cityAttractions' using the AI SDK's `tool` function. It uses Zod for parameter validation and integrates with the `generateText` function from `@ai-sdk/openai` to enable the model to call these tools based on the prompt. The 'weather' tool includes an `execute` function to simulate fetching weather data.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/50-call-tools.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const result = await generateText({
  model: openai('gpt-4-turbo'),
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for')
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10
      })
    }),
    cityAttractions: tool({
      parameters: z.object({ city: z.string() })
    })
  },
  prompt:
    'What is the weather in San Francisco and what attractions should I visit?'
});
```

----------------------------------------

TITLE: Displaying Flights and Triggering Actions in Vercel AI UI (TSX)
DESCRIPTION: This client component displays a list of flights. When a flight number is clicked, it uses the `useActions` hook to call `submitUserMessage` with a `lookupFlight` command, then updates the UI state via `setMessages` with the returned React component. It depends on `ai/rsc` hooks and `ReactNode`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/04-multistep-interfaces.mdx#_snippet_4

LANGUAGE: TSX
CODE:
```
'use client';

import { useActions, useUIState } from 'ai/rsc';
import { ReactNode } from 'react';

interface FlightsProps {
  flights: { id: string; flightNumber: string }[];
}

export const Flights = ({ flights }: FlightsProps) => {
  const { submitUserMessage } = useActions();
  const [_, setMessages] = useUIState();

  return (
    <div>
      {flights.map(result => (
        <div
            onClick={async () => {
              const display = await submitUserMessage(
                `lookupFlight ${result.flightNumber}`,
              );

              setMessages((messages: ReactNode[]) => [...messages, display]);
            }}
          >
            {result.flightNumber}
          </div>
        </div>
      ))}
    </div>
  );
};
```

----------------------------------------

TITLE: Streaming Structured Data Object with AI SDK in TypeScript
DESCRIPTION: This snippet demonstrates how to use the `streamObject` function from the AI SDK to stream a large, structured data object (a recipe in this case) in real-time. It utilizes `@ai-sdk/openai` for the model and `zod` for defining the output schema. The `partialObjectStream` is then iterated over to log the incrementally generated parts of the object, enabling real-time UI updates as the data becomes available.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/40-stream-object.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamObject } from 'ai';
import { z } from 'zod';

const { partialObjectStream } = streamObject({
  model: openai('gpt-4-turbo'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.string()),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});

for await (const partialObject of partialObjectStream) {
  console.clear();
  console.log(partialObject);
}
```

----------------------------------------

TITLE: Creating AI SDK Chat Route Handler (TSX)
DESCRIPTION: Defines a Next.js API route handler (POST) that uses the AI SDK and DeepSeek provider to stream text responses based on user messages. It configures the model and streams the result, optionally including reasoning tokens.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/25-r1.mdx#_snippet_4

LANGUAGE: TSX
CODE:
```
import { deepseek } from '@ai-sdk/deepseek';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: deepseek('deepseek-reasoner'),
    messages,
  });

  return result.toDataStreamResponse({
    sendReasoning: true,
  });
}
```

----------------------------------------

TITLE: Chat UI Component (React/AI SDK) - TSX
DESCRIPTION: A React component that uses the `@ai-sdk/react` `useChat` hook. It initializes the hook with the provided chat ID and initial messages, handles input changes and form submission, and renders the messages and input field. It demonstrates integrating persistence with the UI.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-message-persistence.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
'use client';

import { Message, useChat } from '@ai-sdk/react';

export default function Chat({
  id,
  initialMessages,
}: { id?: string | undefined; initialMessages?: Message[] } = {}) {
  const { input, handleInputChange, handleSubmit, messages } = useChat({
    id, // use the provided chat ID
    initialMessages, // initial messages if provided
    sendExtraMessageFields: true, // send id and createdAt for each message
  });

  // simplified rendering code, extend as needed:
  return (
    <div>
      {messages.map(m => (
        <div key={m.id}>
          {m.role === 'user' ? 'User: ' : 'AI: '}
          {m.content}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Define Add Resource Tool in AI SDK Route Handler (TSX)
DESCRIPTION: This code defines an API route handler using the AI SDK's `streamText` function. It includes a `tool` definition named `addResource` with a description and parameters defined using Zod. The tool's `execute` function calls a `createResource` action to add content to a knowledge base.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/01-rag-chatbot.mdx#_snippet_18

LANGUAGE: TSX
CODE:
```
import { createResource } from '@/lib/actions/resources';
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: `You are a helpful assistant. Check your knowledge base before answering any questions.\n    Only respond to questions using information from tool calls.\n    if no relevant information is found in the tool calls, respond, \"Sorry, I don't know.\"`,
    messages,
    tools: {
      addResource: tool({
        description: `add a resource to your knowledge base.\n          If the user provides a random piece of knowledge unprompted, use this tool without asking for confirmation.`,
        parameters: z.object({
          content: z
            .string()
            .describe('the content or resource to add to the knowledge base'),
        }),
        execute: async ({ content }) => createResource({ content }),
      }),
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Calling generateQuery in Frontend handleSubmit - TypeScript
DESCRIPTION: Modifies the frontend `handleSubmit` function in `app/page.tsx` to invoke the `generateQuery` Server Action with the user's input. It processes the returned SQL query, calls `runGeneratedSQLQuery` to fetch data, and updates the component's state to display the generated query and results. Includes basic input validation and error handling. Depends on `generateQuery` and `runGeneratedSQLQuery` actions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/04-natural-language-postgres.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
/* ...other imports... */
import { runGeneratedSQLQuery, generateQuery } from './actions';

/* ...rest of the file... */

const handleSubmit = async (suggestion?: string) => {
  clearExistingData();

  const question = suggestion ?? inputValue;
  if (inputValue.length === 0 && !suggestion) return;

  if (question.trim()) {
    setSubmitted(true);
  }

  setLoading(true);
  setLoadingStep(1);
  setActiveQuery('');

  try {
    const query = await generateQuery(question);

    if (query === undefined) {
      toast.error('An error occurred. Please try again.');
      setLoading(false);
      return;
    }

    setActiveQuery(query);
    setLoadingStep(2);

    const companies = await runGeneratedSQLQuery(query);
    const columns = companies.length > 0 ? Object.keys(companies[0]) : [];
    setResults(companies);
    setColumns(columns);

    setLoading(false);
  } catch (e) {
    toast.error('An error occurred. Please try again.');
    setLoading(false);
  }
};

/* ...rest of the file... */
```

----------------------------------------

TITLE: Configuring Vercel AI SDK Provider (TypeScript)
DESCRIPTION: This code defines the `AI` provider using `createAI` from the Vercel AI SDK. It registers `submitMessage` as an available action for the AI model and initializes the AI and UI states, setting up the core AI interaction logic for the application.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/121-stream-assistant-response-with-tools.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
import { createAI } from 'ai/rsc';
import { submitMessage } from './actions';

export const AI = createAI({
  actions: {
    submitMessage,
  },
  initialAIState: [],
  initialUIState: [],
});
```

----------------------------------------

TITLE: React Client for Streaming Chat with AI SDK
DESCRIPTION: This React component (`Home`) manages the conversation state and user input for a chat interface. It leverages `continueConversation` (a server action) to send user messages and `readStreamableValue` from `ai/rsc` to asynchronously stream and display the AI assistant's responses in real-time, providing an immediate user experience. It also configures `maxDuration` for the streaming response.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/21-stream-text-with-chat-prompt.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { Message, continueConversation } from './actions';
import { readStreamableValue } from 'ai/rsc';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export default function Home() {
  const [conversation, setConversation] = useState<Message[]>([]);
  const [input, setInput] = useState<string>('');

  return (
    <div>
      <div>
        {conversation.map((message, index) => (
          <div key={index}>
            {message.role}: {message.content}
          </div>
        ))}
      </div>

      <div>
        <input
          type="text"
          value={input}
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button
          onClick={async () => {
            const { messages, newMessage } = await continueConversation([
              ...conversation,
              { role: 'user', content: input },
            ]);

            let textContent = '';

            for await (const delta of readStreamableValue(newMessage)) {
              textContent = `${textContent}${delta}`;

              setConversation([
                ...messages,
                { role: 'assistant', content: textContent },
              ]);
            }
          }}
        >
          Send Message
        </button>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Using streamUI with AI SDK OpenAI Provider (TSX)
DESCRIPTION: This snippet illustrates the usage of the new `streamUI` function from `ai/rsc` with the `@ai-sdk/openai` provider. It defines a Server Action that interacts with the AI model, includes a tool (`get_city_weather`) with `zod` validation, yields a loading component (`Spinner`), and returns a React Server Component (`Weather`) based on the tool's output. It requires the `ai/rsc` and `@ai-sdk/openai` packages, along with custom components and utilities.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/08-migration-guides/39-migration-guide-3-1.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { streamUI } from 'ai/rsc';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';
import { Spinner, Weather } from '@/components';
import { getWeather } from '@/utils';

async function submitMessage(userInput = 'What is the weather in SF?') {
  'use server';

  const result = await streamUI({
    model: openai('gpt-4-turbo'),
    system: 'You are a helpful assistant',
    messages: [{ role: 'user', content: userInput }],
    text: ({ content }) => <p>{content}</p>,
    tools: {
      get_city_weather: {
        description: 'Get the current weather for a city',
        parameters: z
          .object({
            city: z.string().describe('Name of the city'),
          })
          .required(),
        generate: async function* ({ city }) {
          yield <Spinner />;
          const weather = await getWeather(city);
          return <Weather info={weather} />;
        },
      },
    },
  });

  return result.value;
}
```

----------------------------------------

TITLE: Streaming Text with ModelMessage Conversion (AI SDK)
DESCRIPTION: This snippet demonstrates how to handle incoming messages in an API route, convert UIMessage objects to ModelMessage objects using `convertToModelMessages`, and stream the response from an OpenAI model using `streamText`. The result is then converted back to a UIMessage stream response for the client.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/01-announcing-ai-sdk-5-alpha/index.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
import { openai } from '@ai-sdk/openai';
import { convertToModelMessages, streamText, UIMessage } from 'ai';

export async function POST(req: Request) {
  const { messages }: { messages: UIMessage[] } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages: convertToModelMessages(messages),
  });

  return result.toUIMessageStreamResponse();
}
```

----------------------------------------

TITLE: Supporting Multi-modal Tool Responses with convertToCoreMessages (TypeScript)
DESCRIPTION: This example illustrates how `convertToCoreMessages` can be used with tools that support multi-modal content. It defines a `screenshotTool` that uses `experimental_toToolResultContent` to transform its result into an image content part, demonstrating how non-textual data can be handled within the message conversion process.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/02-ai-sdk-ui/31-convert-to-core-messages.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { tool } from 'ai';
import { z } from 'zod';

const screenshotTool = tool({
  parameters: z.object({}),
  execute: async () => 'imgbase64',
  experimental_toToolResultContent: result => [{ type: 'image', data: result }],
});

const result = streamText({
  model: openai('gpt-4'),
  messages: convertToCoreMessages(messages, {
    tools: {
      screenshot: screenshotTool,
    },
  }),
});
```

----------------------------------------

TITLE: Server-side Object Streaming with streamObject (TypeScript)
DESCRIPTION: This server-side API route handles the object generation process using `streamObject` from the `ai` library. It configures an OpenAI model (`gpt-4-turbo`), applies the `notificationSchema` for structured output, and generates notifications based on the provided `context` from the client request, streaming the result back as a text stream.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/08-object-generation.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamObject } from 'ai';
import { notificationSchema } from './schema';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const context = await req.json();

  const result = streamObject({
    model: openai('gpt-4-turbo'),
    schema: notificationSchema,
    prompt:
      `Generate 3 notifications for a messages app in this context:` + context,
  });

  return result.toTextStreamResponse();
}
```

----------------------------------------

TITLE: Adding Weather Tool to AI Chat Route Handler (TypeScript)
DESCRIPTION: This snippet updates the server-side route handler to include a new 'weather' tool. It uses `@ai-sdk/openai` for the model and `ai` for `streamText` and `tool` functionality, along with `zod` for schema validation of tool parameters. The `execute` function simulates fetching weather data for a given location, demonstrating how to integrate external API calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';

export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      weather: tool({
        description: 'Get the weather in a location (fahrenheit)',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for'),
        }),
        execute: async ({ location }) => {
          const temperature = Math.round(Math.random() * (90 - 32) + 32);
          return {
            location,
            temperature,
          };
        },
      }),
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Integrating Weather Tool with AI-SDK in TypeScript
DESCRIPTION: This snippet demonstrates how to integrate a custom 'weather' tool into an AI chatbot using `@ai-sdk/openai`. It defines the tool's description and parameters using Zod, and provides an asynchronous `execute` function that simulates fetching weather data. The `streamText` function is used to process user input, invoke the tool when appropriate, and stream the AI's text response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/06-nodejs.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { CoreMessage, streamText, tool } from 'ai';
import dotenv from 'dotenv';
import { z } from 'zod';
import * as readline from 'node:readline/promises';

dotenv.config();

const terminal = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const messages: CoreMessage[] = [];

async function main() {
  while (true) {
    const userInput = await terminal.question('You: ');

    messages.push({ role: 'user', content: userInput });

    const result = streamText({
      model: openai('gpt-4o'),
      messages,
      tools: {
        weather: tool({
          description: 'Get the weather in a location (in Celsius)',
          parameters: z.object({
            location: z
              .string()
              .describe('The location to get the weather for'),
          }),
          execute: async ({ location }) => ({
            location,
            temperature: Math.round((Math.random() * 30 + 5) * 10) / 10, // Random temp between 5°C and 35°C
          }),
        }),
      },
    });

    let fullResponse = '';
    process.stdout.write('\nAssistant: ');
    for await (const delta of result.textStream) {
      fullResponse += delta;
      process.stdout.write(delta);
    }
    process.stdout.write('\n\n');

    messages.push({ role: 'assistant', content: fullResponse });
  }
}

main().catch(console.error);
```

----------------------------------------

TITLE: Generating Structured Objects with OpenAI Responses API (TypeScript)
DESCRIPTION: This snippet demonstrates how to use `generateObject` with the OpenAI Responses API to enforce a structured JSON output. It defines a Zod schema for a recipe, ensuring the model's response adheres to the specified object structure for name, ingredients, and steps.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_26

LANGUAGE: TypeScript
CODE:
```
// Using generateObject
const result = await generateObject({
  model: openai.responses('gpt-4.1'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(
        z.object({
          name: z.string(),
          amount: z.string(),
        }),
      ),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});
```

----------------------------------------

TITLE: Generate Text with Tool Calling using Mem0 and Vercel AI SDK (TypeScript)
DESCRIPTION: This snippet shows how to use `generateText` from the Vercel AI SDK with the `@mem0/vercel-ai-provider`. It initializes Mem0, defines a tool (`weather`) using Zod for parameter validation, and calls `generateText` with the Mem0-enhanced model and the tool definition. It logs the result, which may include tool calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/70-mem0.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';
import { createMem0 } from '@mem0/vercel-ai-provider';
import { z } from 'zod';

const mem0 = createMem0({
  provider: 'anthropic',
  apiKey: 'anthropic-api-key',
  mem0Config: {
    // Global User ID
    user_id: 'borat',
  },
});

const prompt = 'What the temperature in the city that I live in?';

const result = await generateText({
  model: mem0('claude-3-5-sonnet-20240620'),
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  prompt: prompt,
});

console.log(result);
```

----------------------------------------

TITLE: Configuring OpenAI API Key
DESCRIPTION: This snippet shows the content for the `.env.local` file, where `OPENAI_API_KEY` is set to your actual OpenAI API key. This key is crucial for authenticating your application with the OpenAI service and enabling AI model interactions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/07-expo.mdx#_snippet_4

LANGUAGE: Env
CODE:
```
OPENAI_API_KEY=xxxxxxxxx
```

----------------------------------------

TITLE: Implementing Server-Side Multi-Step Tool Calls with AI SDK
DESCRIPTION: This server-side API route demonstrates how to define and execute multi-step tool calls using `streamText`. It includes a `getWeatherInformation` tool with a `zod` schema for parameters and an `execute` function, allowing the AI model to perform actions on the server. `maxSteps` limits the number of tool execution steps.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-tool-usage.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';
import { z } from 'zod';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      getWeatherInformation: {
        description: 'show the weather in a given city to the user',
        parameters: z.object({ city: z.string() }),
        // tool has execute function:
        execute: async ({}: { city: string }) => {
          const weatherOptions = ['sunny', 'cloudy', 'rainy', 'snowy', 'windy'];
          return weatherOptions[
            Math.floor(Math.random() * weatherOptions.length)
          ];
        },
      },
    },
    maxSteps: 5,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Defining AI Tools with 'ai' and 'zod' in TypeScript
DESCRIPTION: This snippet defines two AI tools, 'getWeatherInformation' and 'getLocalTime', using the 'ai' library's 'tool' function and 'zod' for parameter validation. 'getWeatherInformation' is designed for human-in-the-loop confirmation (lacks an 'execute' function), while 'getLocalTime' includes an 'execute' function for direct execution without confirmation.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/75-human-in-the-loop.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
import { tool } from 'ai';
import { z } from 'zod';

const getWeatherInformation = tool({
  description: 'show the weather in a given city to the user',
  parameters: z.object({ city: z.string() }),
  // no execute function, we want human in the loop
});

const getLocalTime = tool({
  description: 'get the local time for a specified location',
  parameters: z.object({ location: z.string() }),
  // including execute function -> no confirmation required
  execute: async ({ location }) => {
    console.log(`Getting local time for ${location}`);
    return '10am';
  },
});

export const tools = {
  getWeatherInformation,
  getLocalTime,
};
```

----------------------------------------

TITLE: Implementing the AI SDK Example Provider in TypeScript
DESCRIPTION: This comprehensive snippet details the main implementation of an AI SDK provider. It defines the `ExampleProviderSettings` interface for configuring the provider (e.g., API key, base URL, custom fetch) and the `ExampleProvider` interface which outlines methods for creating various model types (chat, completion, embedding, image). The `createExample` function initializes the provider, handling API key loading, header generation, and URL construction, then instantiates OpenAI-compatible models with common configurations.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/02-openai-compatible-providers/01-custom-providers.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { LanguageModelV1, EmbeddingModelV1 } from '@ai-sdk/provider';
import {
  OpenAICompatibleChatLanguageModel,
  OpenAICompatibleCompletionLanguageModel,
  OpenAICompatibleEmbeddingModel,
  OpenAICompatibleImageModel,
} from '@ai-sdk/openai-compatible';
import {
  FetchFunction,
  loadApiKey,
  withoutTrailingSlash,
} from '@ai-sdk/provider-utils';
// Import your model id and settings here.

export interface ExampleProviderSettings {
  /**
Example API key.
*/
  apiKey?: string;
  /**
Base URL for the API calls.
*/
  baseURL?: string;
  /**
Custom headers to include in the requests.
*/
  headers?: Record<string, string>;
  /**
Optional custom url query parameters to include in request urls.
*/
  queryParams?: Record<string, string>;
  /**
Custom fetch implementation. You can use it as a middleware to intercept requests,
or to provide a custom fetch implementation for e.g. testing.
*/
  fetch?: FetchFunction;
}

export interface ExampleProvider {
  /**
Creates a model for text generation.
*/
  (
    modelId: ExampleChatModelId,
    settings?: ExampleChatSettings,
  ): LanguageModelV1;

  /**
Creates a chat model for text generation.
*/
  chatModel(
    modelId: ExampleChatModelId,
    settings?: ExampleChatSettings,
  ): LanguageModelV1;

  /**
Creates a completion model for text generation.
*/
  completionModel(
    modelId: ExampleCompletionModelId,
    settings?: ExampleCompletionSettings,
  ): LanguageModelV1;

  /**
Creates a text embedding model for text generation.
*/
  textEmbeddingModel(
    modelId: ExampleEmbeddingModelId,
    settings?: ExampleEmbeddingSettings,
  ): EmbeddingModelV1<string>;

  /**
Creates an image model for image generation.
*/
  imageModel(
    modelId: ExampleImageModelId,
    settings?: ExampleImageSettings,
  ): ImageModelV1;
}

export function createExample(
  options: ExampleProviderSettings = {},
): ExampleProvider {
  const baseURL = withoutTrailingSlash(
    options.baseURL ?? 'https://api.example.com/v1',
  );
  const getHeaders = () => ({
    Authorization: `Bearer ${loadApiKey({
      apiKey: options.apiKey,
      environmentVariableName: 'EXAMPLE_API_KEY',
      description: 'Example API key',
    })}`,
    ...options.headers,
  });

  interface CommonModelConfig {
    provider: string;
    url: ({ path }: { path: string }) => string;
    headers: () => Record<string, string>;
    fetch?: FetchFunction;
  }

  const getCommonModelConfig = (modelType: string): CommonModelConfig => ({
    provider: `example.${modelType}`,
    url: ({ path }) => {
      const url = new URL(`${baseURL}${path}`);
      if (options.queryParams) {
        url.search = new URLSearchParams(options.queryParams).toString();
      }
      return url.toString();
    },
    headers: getHeaders,
    fetch: options.fetch,
  });

  const createChatModel = (
    modelId: ExampleChatModelId,
    settings: ExampleChatSettings = {},
  ) => {
    return new OpenAICompatibleChatLanguageModel(modelId, settings, {
      ...getCommonModelConfig('chat'),
      defaultObjectGenerationMode: 'tool',
    });
  };

  const createCompletionModel = (
    modelId: ExampleCompletionModelId,
    settings: ExampleCompletionSettings = {},
  ) =>
    new OpenAICompatibleCompletionLanguageModel(
      modelId,
      settings,
      getCommonModelConfig('completion'),
    );

  const createTextEmbeddingModel = (
    modelId: ExampleEmbeddingModelId,
    settings: ExampleEmbeddingSettings = {},
  ) =>
    new OpenAICompatibleEmbeddingModel(
      modelId,
      settings,
      getCommonModelConfig('embedding'),
    );

  const createImageModel = (
    modelId: ExampleImageModelId,
    settings: ExampleImageSettings = {},
  ) =>
    new OpenAICompatibleImageModel(
      modelId,
      settings,
      getCommonModelConfig('image'),
    );

  const provider = (
    modelId: ExampleChatModelId,
    settings?: ExampleChatSettings,
  ) => createChatModel(modelId, settings);

  provider.completionModel = createCompletionModel;
  provider.chatModel = createChatModel;
  provider.textEmbeddingModel = createTextEmbeddingModel;
  provider.imageModel = createImageModel;

  return provider;
}

// Export default instance
export const example = createExample();
```

----------------------------------------

TITLE: Passing Base64-encoded PDF to Anthropic Model (TypeScript)
DESCRIPTION: Illustrates how to provide a PDF document to the Anthropic `claude-3-5-sonnet-20241022` model by reading it from a local file and passing its content. The PDF data is read using `fs.readFileSync` and included in the `messages` array as a `file` type, with `mimeType` set to `application/pdf`. This method is suitable for local PDF files.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/05-anthropic.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: anthropic('claude-3-5-sonnet-20241022'),
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'What is an embedding model according to this document?',
        },
        {
          type: 'file',
          data: fs.readFileSync('./data/ai.pdf'),
          mimeType: 'application/pdf',
        },
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Handling Parallel Tool Calls on Server with AI SDK
DESCRIPTION: This server-side route (`/api/chat`) uses the `streamText` function from the `ai` module to generate assistant responses. It defines a `getWeather` tool with a Zod schema for its parameters (`city`, `unit`), allowing the AI model to invoke it. The `execute` function for `getWeather` simulates fetching weather data and returns a formatted string.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/71-call-tools-in-parallel.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
import { ToolInvocation, streamText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

interface Message {
  role: 'user' | 'assistant';
  content: string;
  toolInvocations?: ToolInvocation[];
}

function getWeather({ city, unit }) {
  return { value: 25, description: 'Sunny' };
}

export async function POST(req: Request) {
  const { messages }: { messages: Message[] } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'You are a helpful assistant.',
    messages,
    tools: {
      getWeather: {
        description: 'Get the weather for a location',
        parameters: z.object({
          city: z.string().describe('The city to get the weather for'),
          unit: z
            .enum(['C', 'F'])
            .describe('The unit to display the temperature in'),
        }),
        execute: async ({ city, unit }) => {
          const { value, description } = getWeather({ city, unit });
          return `It is currently ${value}°${unit} and ${description} in ${city}!`;
        },
      },
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Enabling Predicted Outputs with OpenAI (TypeScript)
DESCRIPTION: This snippet demonstrates how to enable predicted outputs for OpenAI models like `gpt-4o` using the `streamText` function. By specifying a `prediction` object within `providerOptions.openai`, you can provide a base text (`existingCode`) that the model should modify, thereby reducing latency. This feature is available for `gpt-4o` and `gpt-4o-mini`.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_13

LANGUAGE: ts
CODE:
```
const result = streamText({
  model: openai('gpt-4o'),
  messages: [
    {
      role: 'user',
      content: 'Replace the Username property with an Email property.',
    },
    {
      role: 'user',
      content: existingCode,
    },
  ],
  providerOptions: {
    openai: {
      prediction: {
        type: 'content',
        content: existingCode,
      },
    },
  },
});
```

----------------------------------------

TITLE: Handling Slack Events in API Route (TypeScript)
DESCRIPTION: Defines an asynchronous POST function to handle incoming requests from Slack's Events API. It verifies the request signature, handles URL verification, and processes different event types like 'app_mention', 'assistant_thread_started', and 'message' by calling specific handler functions wrapped in `waitUntil`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/03-slackbot.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import type { SlackEvent } from '@slack/web-api';
import {
  assistantThreadMessage,
  handleNewAssistantMessage,
} from '../lib/handle-messages';
import { waitUntil } from '@vercel/functions';
import { handleNewAppMention } from '../lib/handle-app-mention';
import { verifyRequest, getBotId } from '../lib/slack-utils';

export async function POST(request: Request) {
  const rawBody = await request.text();
  const payload = JSON.parse(rawBody);
  const requestType = payload.type as 'url_verification' | 'event_callback';

  // See https://api.slack.com/events/url_verification
  if (requestType === 'url_verification') {
    return new Response(payload.challenge, { status: 200 });
  }

  await verifyRequest({ requestType, request, rawBody });

  try {
    const botUserId = await getBotId();

    const event = payload.event as SlackEvent;

    if (event.type === 'app_mention') {
      waitUntil(handleNewAppMention(event, botUserId));
    }

    if (event.type === 'assistant_thread_started') {
      waitUntil(assistantThreadMessage(event));
    }

    if (
      event.type === 'message' &&
      !event.subtype &&
      event.channel_type === 'im' &&
      !event.bot_id &&
      !event.bot_profile &&
      event.bot_id !== botUserId
    ) {
      waitUntil(handleNewAssistantMessage(event, botUserId));
    }

    return new Response('Success!', { status: 200 });
  } catch (error) {
    console.error('Error generating response', error);
    return new Response('Error generating response', { status: 500 });
  }
}
```

----------------------------------------

TITLE: Calling streamUI with OpenAI Model (TSX)
DESCRIPTION: This snippet demonstrates a basic invocation of the `streamUI` function from the AI SDK RSC. It configures the function to use the `gpt-4o` model from OpenAI, provides a simple text prompt, and defines a function to render plain text responses as a `div` element. It also includes an empty `tools` object, indicating no specific tools are provided for the model to use in this example.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/02-streaming-react-components.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
const result = await streamUI({
  model: openai('gpt-4o'),
  prompt: 'Get the weather for San Francisco',
  text: ({ content }) => <div>{content}</div>,
  tools: {},
});
```

----------------------------------------

TITLE: Defining AI Weather Tool in Route Handler (TypeScript)
DESCRIPTION: This snippet updates the `app/api/chat/route.ts` file to integrate a new `weather` tool using the Vercel AI SDK. It defines the tool's purpose, specifies `location` as a required parameter using Zod for schema validation, and includes an `execute` function that simulates fetching weather data. This enables the AI model to call the tool and retrieve information based on user queries.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/03-nextjs-pages-router.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';

export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      weather: tool({
        description: 'Get the weather in a location (fahrenheit)',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for'),
        }),
        execute: async ({ location }) => {
          const temperature = Math.round(Math.random() * (90 - 32) + 32);
          return {
            location,
            temperature,
          };
        },
      }),
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Installing AI SDK Dependencies
DESCRIPTION: These commands install the core AI SDK package (`ai`), the OpenAI provider (`@ai-sdk/openai`), the React utilities for AI SDK (`@ai-sdk/react`), and `zod` for schema validation. These packages are essential for building AI-powered features with streaming UIs.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/07-expo.mdx#_snippet_2

LANGUAGE: Shell
CODE:
```
pnpm add ai @ai-sdk/openai @ai-sdk/react zod
```

LANGUAGE: Shell
CODE:
```
npm install ai @ai-sdk/openai @ai-sdk/react zod
```

LANGUAGE: Shell
CODE:
```
yarn add ai @ai-sdk/openai @ai-sdk/react zod
```

----------------------------------------

TITLE: Defining Anthropic Computer Tool with AI SDK
DESCRIPTION: This TypeScript snippet defines a `computerTool` using the Anthropic AI SDK provider, configuring display dimensions and an `execute` function to handle actions like taking screenshots or performing general computer actions. It also includes `experimental_toToolResultContent` for converting tool results into a model-processable format, supporting multi-modal outputs.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/05-computer-use.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { anthropic } from '@ai-sdk/anthropic';
import { getScreenshot, executeComputerAction } from '@/utils/computer-use';

const computerTool = anthropic.tools.computer_20241022({
  displayWidthPx: 1920,
  displayHeightPx: 1080,
  execute: async ({ action, coordinate, text }) => {
    switch (action) {
      case 'screenshot': {
        return {
          type: 'image',
          data: getScreenshot(),
        };
      }
      default: {
        return executeComputerAction(action, coordinate, text);
      }
    }
  },
  experimental_toToolResultContent(result) {
    return typeof result === 'string'
      ? [{ type: 'text', text: result }]
      : [{ type: 'image', data: result.data, mimeType: 'image/png' }];
  }
});
```

----------------------------------------

TITLE: Handling Chat Conversation in React Client Component
DESCRIPTION: This React client component manages the state of a user-model conversation. It displays messages, captures user input, and sends the updated conversation history to a server action (`continueConversation`) to generate new responses. The component uses `useState` to manage the `conversation` array and the current `input` message.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/11-generate-text-with-chat-prompt.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { Message, continueConversation } from './actions';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export default function Home() {
  const [conversation, setConversation] = useState<Message[]>([]);
  const [input, setInput] = useState<string>('');

  return (
    <div>
      <div>
        {conversation.map((message, index) => (
          <div key={index}>
            {message.role}: {message.content}
          </div>
        ))}
      </div>

      <div>
        <input
          type="text"
          value={input}
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button
          onClick={async () => {
            const { messages } = await continueConversation([
              ...conversation,
              { role: 'user', content: input },
            ]);

            setConversation(messages);
          }}
        >
          Send Message
        </button>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Typical OpenTelemetry Setup for AI SDK (JavaScript)
DESCRIPTION: This comprehensive snippet outlines a typical OpenTelemetry (OTEL) setup for AI SDK applications, presented for comparison with Helicone's proxy. It involves installing multiple packages, configuring an exporter, setting up the NodeSDK with instrumentations, starting the SDK, enabling experimental telemetry on requests, and finally shutting down the SDK to flush traces. This highlights the complexity often associated with full OTEL instrumentation.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/05-observability/helicone.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
// Install multiple packages
// @vercel/otel, @opentelemetry/sdk-node, @opentelemetry/auto-instrumentations-node, etc.

// Create exporter
const exporter = new OtherProviderExporter({
  projectApiKey: process.env.API_KEY
});

// Setup SDK
const sdk = new NodeSDK({
  traceExporter: exporter,
  instrumentations: [getNodeAutoInstrumentations()],
  resource: new Resource({...}),
});

// Start SDK
sdk.start();

// Enable telemetry on each request
const response = await generateText({
  model: openai("gpt-4o-mini"),
  prompt: "Hello world",
  experimental_telemetry: { isEnabled: true }
});

// Shutdown SDK to flush traces
await sdk.shutdown();
```

----------------------------------------

TITLE: Streaming Structured Object with Zod Schema (TypeScript)
DESCRIPTION: Demonstrates how to use `streamObject` to generate a structured JSON object (e.g., a recipe) based on a Zod schema. It streams partial objects as they are generated by the language model, allowing for real-time display or processing of the output. Requires `@ai-sdk/openai` and `ai`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/04-stream-object.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamObject } from 'ai';
import { z } from 'zod';

const { partialObjectStream } = streamObject({
  model: openai('gpt-4-turbo'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.string()),
      steps: z.array(z.string())
    })
  }),
  prompt: 'Generate a lasagna recipe.'
});

for await (const partialObject of partialObjectStream) {
  console.clear();
  console.log(partialObject);
}
```

----------------------------------------

TITLE: Generating Structured Data on Server-side with Next.js API Route
DESCRIPTION: This Next.js API route handles POST requests to generate structured notification objects using the `generateObject` function from the `ai` module. It defines a Zod schema (`z.object`) to enforce the structure of the generated JSON, including `name`, `message`, and `minutesAgo` fields. The route uses OpenAI's GPT-4 model and returns the generated object as a JSON response.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/30-generate-object.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

export async function POST(req: Request) {
  const { prompt }: { prompt: string } = await req.json();

  const result = await generateObject({
    model: openai('gpt-4'),
    system: 'You generate three notifications for a messages app.',
    prompt,
    schema: z.object({
      notifications: z.array(
        z.object({
          name: z.string().describe('Name of a fictional person.'),
          message: z.string().describe('Do not use emojis or links.'),
          minutesAgo: z.number(),
        }),
      ),
    }),
  });

  return result.toJsonResponse();
}
```

----------------------------------------

TITLE: Accessing Sources with streamText (TSX)
DESCRIPTION: This snippet illustrates how to use `streamText` with Google's Gemini model to stream text and access sources as they become available in the `fullStream`. It iterates through parts of the stream, specifically checking for `source` type parts with `url` sourceType to log details like ID, title, URL, and provider metadata. This method is suitable for real-time source display.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/05-generating-text.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
const result = streamText({
  model: google('gemini-2.0-flash-exp', { useSearchGrounding: true }),
  prompt: 'List the top 5 San Francisco news from the past week.',
});

for await (const part of result.fullStream) {
  if (part.type === 'source' && part.source.sourceType === 'url') {
    console.log('ID:', part.source.id);
    console.log('Title:', part.source.title);
    console.log('URL:', part.source.url);
    console.log('Provider metadata:', part.source.providerMetadata);
    console.log();
  }
}
```

----------------------------------------

TITLE: Submitting Tool Outputs to OpenAI Threads (TypeScript)
DESCRIPTION: This snippet demonstrates how to submit tool outputs back to an OpenAI thread using the `submitToolOutputs` method. It includes logic for processing tool calls, handling `thread.run.failed` events, and managing a run queue for subsequent operations within an AI assistant interaction.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/121-stream-assistant-response-with-tools.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
                      tool_call_id: toolCallId,
                      output: JSON.stringify(fakeEmails),
                    });
                  }
                }

                const nextRun: any =
                  await openai.beta.threads.runs.submitToolOutputs(
                    THREAD_ID,
                    RUN_ID,
                    {
                      tool_outputs,
                      stream: true,
                    },
                  );

                runQueue.push({ id: generateId(), run: nextRun });
              }
            }
          } else if (event === 'thread.run.failed') {
            console.log(data);
          }
        }
      }
    }

    status.done();
    textUIStream.done();
    gui.done();
  })();

  return {
    id: generateId(),
    status: status.value,
    text: textUIStream.value,
    gui: gui.value,
  };
```

----------------------------------------

TITLE: Updating API Route with Weather Tool in TypeScript
DESCRIPTION: This snippet modifies the `app/api/chat+api.ts` file to integrate a `weather` tool into an AI chatbot's API route. It uses `@ai-sdk/openai` for the model and `ai` for tool definition, leveraging `zod` for parameter validation. The `weather` tool is configured with a description, a `location` parameter, and an `execute` function that simulates fetching temperature data, allowing the model to invoke external capabilities.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/07-expo.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      weather: tool({
        description: 'Get the weather in a location (fahrenheit)',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for'),
        }),
        execute: async ({ location }) => {
          const temperature = Math.round(Math.random() * (90 - 32) + 32);
          return {
            location,
            temperature,
          };
        },
      }),
    },
  });

  return result.toDataStreamResponse({
    headers: {
      'Content-Type': 'application/octet-stream',
      'Content-Encoding': 'none',
    },
  });
}
```

----------------------------------------

TITLE: Implementing Auto-Resume Logic with useAutoResume Hook in TSX
DESCRIPTION: This custom hook provides a more resilient approach to stream resumption by handling race conditions and processing `append-message` SSE data. It uses two `useEffect` hooks: one to trigger `experimental_resume` if the last message is from the user, and another to process incoming `data` parts and update messages.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-message-persistence.mdx#_snippet_13

LANGUAGE: TSX
CODE:
```
'use client';

import { useEffect } from 'react';
import type { UIMessage } from 'ai';
import type { UseChatHelpers } from '@ai-sdk/react';

export type DataPart = { type: 'append-message'; message: string };

export interface Props {
  autoResume: boolean;
  initialMessages: UIMessage[];
  experimental_resume: UseChatHelpers['experimental_resume'];
  data: UseChatHelpers['data'];
  setMessages: UseChatHelpers['setMessages'];
}

export function useAutoResume({
  autoResume,
  initialMessages,
  experimental_resume,
  data,
  setMessages,
}: Props) {
  useEffect(() => {
    if (!autoResume) return;

    const mostRecentMessage = initialMessages.at(-1);

    if (mostRecentMessage?.role === 'user') {
      experimental_resume();
    }

    // we intentionally run this once
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  useEffect(() => {
    if (!data || data.length === 0) return;

    const dataPart = data[0] as DataPart;

    if (dataPart.type === 'append-message') {
      const message = JSON.parse(dataPart.message) as UIMessage;
      setMessages([...initialMessages, message]);
    }
  }, [data, initialMessages, setMessages]);
}
```

----------------------------------------

TITLE: Enabling Extended Thinking for Claude 3.7 Sonnet
DESCRIPTION: This snippet demonstrates how to enable Claude 3.7 Sonnet's new extended thinking feature using the `providerOptions`. It shows how to specify the thinking type and budget in tokens and access the reasoning output.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/20-sonnet-3-7.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { anthropic, AnthropicProviderOptions } from '@ai-sdk/anthropic';
import { generateText } from 'ai';

const { text, reasoning, reasoningDetails } = await generateText({
  model: anthropic('claude-3-7-sonnet-20250219'),
  prompt: 'How many people will live in the world in 2040?',
  providerOptions: {
    anthropic: {
      thinking: { type: 'enabled', budgetTokens: 12000 },
    } satisfies AnthropicProviderOptions,
  },
});

console.log(reasoning); // reasoning text
console.log(reasoningDetails); // reasoning details including redacted reasoning
console.log(text); // text response
```

----------------------------------------

TITLE: Implementing Web Search with Exa in TypeScript
DESCRIPTION: This snippet defines a 'webSearch' tool using the 'ai' SDK and Exa API. It allows an AI model to perform web searches, retrieving titles, URLs, and truncated content from search results. It requires an 'EXA_API_KEY' environment variable for authentication.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/56-web-search-agent.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';
import Exa from 'exa-js';

export const exa = new Exa(process.env.EXA_API_KEY);

export const webSearch = tool({
  description: 'Search the web for up-to-date information',
  parameters: z.object({
    query: z.string().min(1).max(100).describe('The search query'),
  }),
  execute: async ({ query }) => {
    const { results } = await exa.searchAndContents(query, {
      livecrawl: 'always',
      numResults: 3,
    });
    return results.map(result => ({
      title: result.title,
      url: result.url,
      content: result.text.slice(0, 1000), // take just the first 1000 characters
      publishedDate: result.publishedDate,
    }));
  },
});

const { text } = await generateText({
  model: openai('gpt-4o-mini'), // can be any model that supports tools
  prompt: 'What happened in San Francisco last week?',
  tools: {
    webSearch,
  },
  maxSteps: 2,
});
```

----------------------------------------

TITLE: Handling Customer Queries with AI Routing (TypeScript)
DESCRIPTION: This snippet illustrates a routing pattern where an AI model dynamically directs the workflow. It first classifies a customer query using `generateObject` to determine its type (general, refund, technical) and complexity. Based on this classification, it then uses `generateText` with a dynamically selected model and a tailored system prompt to generate a response. It relies on `@ai-sdk/openai` and `zod`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateObject, generateText } from 'ai';
import { z } from 'zod';

async function handleCustomerQuery(query: string) {
  const model = openai('gpt-4o');

  // First step: Classify the query type
  const { object: classification } = await generateObject({
    model,
    schema: z.object({
      reasoning: z.string(),
      type: z.enum(['general', 'refund', 'technical']),
      complexity: z.enum(['simple', 'complex']),
    }),
    prompt: `Classify this customer query:
    ${query}

    Determine:
    1. Query type (general, refund, or technical)
    2. Complexity (simple or complex)
    3. Brief reasoning for classification`,
  });

  // Route based on classification
  // Set model and system prompt based on query type and complexity
  const { text: response } = await generateText({
    model:
      classification.complexity === 'simple'
        ? openai('gpt-4o-mini')
        : openai('o3-mini'),
    system: {
      general:
        'You are an expert customer service agent handling general inquiries.',
      refund:
        'You are a customer service agent specializing in refund requests. Follow company policy and collect necessary information.',
      technical:
        'You are a technical support specialist with deep product knowledge. Focus on clear step-by-step troubleshooting.',
    }[classification.type],
    prompt: query,
  });

  return { response, classification };
}
```

----------------------------------------

TITLE: Conditionally Rendering WeatherCard Component (React/TSX)
DESCRIPTION: This React component snippet demonstrates how to process messages from a language model, specifically handling messages with a 'function' role. It extracts the structured weather data from the message content and uses it to conditionally render a `<WeatherCard/>` component, passing the data as props for dynamic UI display.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/07-rendering-ui-with-language-models.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
return (
  <div>
    {messages.map(message => {
      if (message.role === 'function') {
        const { name, content } = message
        const { temperature, unit, description, forecast } = content;

        return (
          <WeatherCard
            weather={{
              temperature: 47,
              unit: 'F',
              description: 'sunny'
              forecast,
            }}
          />
        )
      }
    })}
  </div>
)
```

----------------------------------------

TITLE: Configure maxSteps in useChat (TSX)
DESCRIPTION: This snippet demonstrates how to add the `maxSteps` configuration option to the `useChat` hook in a Next.js page component. Setting `maxSteps` to a value greater than 1 allows the AI model to make multiple tool calls sequentially or respond after a tool call has been executed, improving the user experience by providing follow-up information.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/01-rag-chatbot.mdx#_snippet_20

LANGUAGE: tsx
CODE:
```
// ... Rest of your code

const { messages, input, handleInputChange, handleSubmit } = useChat({
  maxSteps: 3,
});

// ... Rest of your code
```

----------------------------------------

TITLE: Displaying Streamable AI Messages in React
DESCRIPTION: This React client component is designed to display streamed text content from an AI assistant. It utilizes the `useStreamableValue` hook from `ai/rsc` to efficiently render values that are streamed over time, ensuring the UI updates as new parts of the message arrive.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/121-stream-assistant-response-with-tools.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { StreamableValue, useStreamableValue } from 'ai/rsc';

export function Message({ textStream }: { textStream: StreamableValue }) {
  const [text] = useStreamableValue(textStream);

  return <div>{text}</div>;
}
```

----------------------------------------

TITLE: Generating AI Responses in TypeScript Server Action
DESCRIPTION: This server action, `continueConversation`, is responsible for interacting with the AI model to generate text responses. It takes the conversation `history` as input, uses the `@ai-sdk/openai` package to call the `gpt-3.5-turbo` model with a system prompt, and appends the AI's generated response to the conversation history before returning it. It also defines the `Message` interface for conversation objects.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/11-generate-text-with-chat-prompt.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
'use server';

import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

export interface Message {
  role: 'user' | 'assistant';
  content: string;
}

export async function continueConversation(history: Message[]) {
  'use server';

  const { text } = await generateText({
    model: openai('gpt-3.5-turbo'),
    system: 'You are a friendly assistant!',
    messages: history,
  });

  return {
    messages: [
      ...history,
      {
        role: 'assistant' as const,
        content: text,
      },
    ],
  };
}
```

----------------------------------------

TITLE: Generating Structured JSON Output with Zod Schema (TypeScript)
DESCRIPTION: Demonstrates generating structured JSON objects using a FriendliAI model and a Zod schema. This ensures the model's output strictly adheres to a predefined JSON structure, making it suitable for extracting specific data points reliably.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/08-friendliai.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
import { friendli } from '@friendliai/ai-provider';
import { generateObject } from 'ai';
import { z } from 'zod';

const { object } = await generateObject({
  model: friendli('meta-llama-3.3-70b-instruct'),
  schemaName: 'CalendarEvent',
  schema: z.object({
    name: z.string(),
    date: z.string(),
    participants: z.array(z.string()),
  }),
  system: 'Extract the event information.',
  prompt: 'Alice and Bob are going to a science fair on Friday.',
});

console.log(object);
```

----------------------------------------

TITLE: Streaming Object Response with Route Handler (After)
DESCRIPTION: This Next.js API route handler demonstrates how to stream a structured object response using `streamObject` from the AI SDK. It takes context from the request body, generates notifications using an OpenAI model and a schema, and returns the result as a text stream.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/10-migrating-to-ui.mdx#_snippet_18

LANGUAGE: ts
CODE:
```
import { streamObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { notificationSchema } from '@/utils/schemas';

export async function POST(req: Request) {
  const context = await req.json();

  const result = streamObject({
    model: openai('gpt-4-turbo'),
    schema: notificationSchema,
    prompt:
      `Generate 3 notifications for a messages app in this context:` + context,
  });

  return result.toTextStreamResponse();
}
```

----------------------------------------

TITLE: Defining AI SDK Tools for Weather and Temperature Conversion in TypeScript
DESCRIPTION: This TypeScript snippet updates the `server/api/chat.ts` file to define two new tools: `weather` and `convertFahrenheitToCelsius`. The `weather` tool simulates fetching temperature for a given location, while `convertFahrenheitToCelsius` converts a Fahrenheit temperature to Celsius. These tools are integrated into the `streamText` function, allowing the AI model to call them based on user queries. It requires `@ai-sdk/openai` and `zod` for schema validation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/05-nuxt.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
import { streamText, tool } from 'ai';
import { createOpenAI } from '@ai-sdk/openai';
import { z } from 'zod';

export default defineLazyEventHandler(async () => {
  const apiKey = useRuntimeConfig().openaiApiKey;
  if (!apiKey) throw new Error('Missing OpenAI API key');
  const openai = createOpenAI({
    apiKey: apiKey,
  });

  return defineEventHandler(async (event: any) => {
    const { messages } = await readBody(event);

    const result = streamText({
      model: openai('gpt-4o-preview'),
      messages,
      tools: {
        weather: tool({
          description: 'Get the weather in a location (fahrenheit)',
          parameters: z.object({
            location: z
              .string()
              .describe('The location to get the weather for'),
          }),
          execute: async ({ location }) => {
            const temperature = Math.round(Math.random() * (90 - 32) + 32);
            return {
              location,
              temperature,
            };
          },
        }),
        convertFahrenheitToCelsius: tool({
          description: 'Convert a temperature in fahrenheit to celsius',
          parameters: z.object({
            temperature: z
              .number()
              .describe('The temperature in fahrenheit to convert'),
          }),
          execute: async ({ temperature }) => {
            const celsius = Math.round((temperature - 32) * (5 / 9));
            return {
              celsius,
            };
          },
        }),
      },
    });

    return result.toDataStreamResponse();
  });
});
```

----------------------------------------

TITLE: Implementing Chat UI with AI SDK React Hook (TSX)
DESCRIPTION: Client-side component example for a Next.js App Router application using the useChat hook from @ai-sdk/react. It demonstrates fetching messages, handling user input, submitting the form, and displaying messages.
SOURCE: https://github.com/vercel/ai/blob/main/packages/ai/README.md#_snippet_4

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleSubmit, handleInputChange, status } =
    useChat();

  return (
    <div>
      {messages.map(message => (
        <div key={message.id}>
          <strong>{`${message.role}: `}</strong>
          {message.parts.map((part, index) => {
            switch (part.type) {
              case 'text':
                return <span key={index}>{part.text}</span>;

              // other cases can handle images, tool calls, etc
            }
          })}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          value={input}
          placeholder="Send a message..."
          onChange={handleInputChange}
          disabled={status !== 'ready'}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Agents with AI SDK generateText and maxSteps (TSX)
DESCRIPTION: Illustrates how to create an agent-like behavior using the AI SDK by setting `maxSteps` and providing a tool. The example defines a 'calculate' tool for evaluating math expressions and configures the model with a system prompt to reason step-by-step.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_5

LANGUAGE: TSX
CODE:
```
import { generateText, tool } from 'ai';
import { deepinfra } from '@ai-sdk/deepinfra';
import * as mathjs from 'mathjs';
import { z } from 'zod';

const problem =
  'Calculate the profit for a day if revenue is $5000 and expenses are $3500.';

const { text: answer } = await generateText({
  model: deepinfra('meta-llama/Meta-Llama-3.1-70B-Instruct'),
  system:
    'You are solving math problems. Reason step by step. Use the calculator when necessary.',
  prompt: problem,
  tools: {
    calculate: tool({
      description: 'A tool for evaluating mathematical expressions.',
      parameters: z.object({ expression: z.string() }),
      execute: async ({ expression }) => mathjs.evaluate(expression),
    }),
  },
  maxSteps: 5,
});
```

----------------------------------------

TITLE: Installing AI SDK Dependencies with npm
DESCRIPTION: This command installs the core AI SDK package (`ai`), its React hooks (`@ai-sdk/react`), the OpenAI provider (`@ai-sdk/openai`), and `zod` for schema validation using npm. These packages are essential for building AI-powered features and interacting with OpenAI models in the Next.js application.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/02-nextjs-app-router.mdx#_snippet_3

LANGUAGE: Shell
CODE:
```
npm install ai @ai-sdk/react @ai-sdk/openai zod
```

----------------------------------------

TITLE: Dynamic AI Model Routing for Multi-modal Chat (TypeScript)
DESCRIPTION: This TypeScript route handler (`app/api/chat/route.ts`) intelligently routes incoming chat messages to different AI models based on their attachments. It detects PDF attachments and directs those requests to Anthropic's Claude 3.5 Sonnet, while routing other requests (e.g., image-only) to OpenAI's GPT-4o, enabling versatile multi-modal processing.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/02-multi-modal-chatbot.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { anthropic } from '@ai-sdk/anthropic';
import { streamText, type Message } from 'ai';

export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages }: { messages: Message[] } = await req.json();

  // check if user has sent a PDF
  const messagesHavePDF = messages.some(message =>
    message.experimental_attachments?.some(
      a => a.contentType === 'application/pdf',
    ),
  );

  const result = streamText({
    model: messagesHavePDF
      ? anthropic('claude-3-5-sonnet-latest')
      : openai('gpt-4o'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Server-side AI Text Streaming API Route (Next.js TypeScript)
DESCRIPTION: This Next.js API route handles incoming POST requests for AI text completion. It extracts the prompt from the request body, uses `streamText` from `ai` with the `openai` model (`gpt-4o`), and returns the result as a data stream response using `toDataStreamResponse()`. The `maxDuration` is set to 30 seconds to allow for longer streaming responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/50-stream-protocol.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export async function POST(req: Request) {
  const { prompt }: { prompt: string } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    prompt,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Creating Streamable Value with AI SDK RSC in TypeScript
DESCRIPTION: This snippet demonstrates how to create and manage a streamable JavaScript value on the server using `createStreamableValue` from `ai/rsc`. It initializes a stream with a default status, then simulates asynchronous updates and a final completion after a delay, returning the stream's current value. This is useful for streaming real-time status updates or progress from server-side operations.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/05-streaming-values.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
'use server';

import { createStreamableValue } from 'ai/rsc';

export const runThread = async () => {
  const streamableStatus = createStreamableValue('thread.init');

  setTimeout(() => {
    streamableStatus.update('thread.run.create');
    streamableStatus.update('thread.run.update');
    streamableStatus.update('thread.run.end');
    streamableStatus.done('thread.end');
  }, 1000);

  return {
    status: streamableStatus.value,
  };
};
```

----------------------------------------

TITLE: Using Tools with AI SDK and OpenAI GPT-4.5 TypeScript
DESCRIPTION: Demonstrates how to integrate tools with the AI SDK's `generateText` function to enable model interaction with external systems. It defines a `getWeather` tool with parameters and an execution function, allowing the model to call this tool based on the user prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/22-gpt-4-5.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const { text } = await generateText({
  model: openai('gpt-4.5-preview'),
  prompt: 'What is the weather like today in San Francisco?',
  tools: {
    getWeather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for')
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10
      })
    })
  }
});
```

----------------------------------------

TITLE: Defining Weather Tool in AI API Route (TypeScript)
DESCRIPTION: This snippet modifies the server-side API route to include a new 'weather' tool. It defines the tool's description, parameters (using Zod for 'location' string validation), and an asynchronous 'execute' function that simulates fetching weather data. This allows the AI model to call this function when a user query requires weather information.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/05-nuxt.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
import { streamText, tool } from 'ai';
import { createOpenAI } from '@ai-sdk/openai';
import { z } from 'zod';

export default defineLazyEventHandler(async () => {
  const apiKey = useRuntimeConfig().openaiApiKey;
  if (!apiKey) throw new Error('Missing OpenAI API key');
  const openai = createOpenAI({
    apiKey: apiKey,
  });

  return defineEventHandler(async (event: any) => {
    const { messages } = await readBody(event);

    const result = streamText({
      model: openai('gpt-4o'),
      messages,
      tools: {
        weather: tool({
          description: 'Get the weather in a location (fahrenheit)',
          parameters: z.object({
            location: z
              .string()
              .describe('The location to get the weather for'),
          }),
          execute: async ({ location }) => {
            const temperature = Math.round(Math.random() * (90 - 32) + 32);
            return {
              location,
              temperature,
            };
          },
        }),
      },
    });

    return result.toDataStreamResponse();
  });
});
```

----------------------------------------

TITLE: Implementing Client-Side Chat Interface with AI SDK (TypeScript/React)
DESCRIPTION: This React client component manages the chat conversation state and handles user input. It uses `useUIState` and `useActions` from `ai/rsc` to interact with server actions, displaying messages and sending user input to the AI model. It also sets `maxDuration` for streaming responses, allowing up to 30 seconds.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/90-render-visual-interface-in-chat.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { ClientMessage } from './actions';
import { useActions, useUIState } from 'ai/rsc';
import { generateId } from 'ai';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export default function Home() {
  const [input, setInput] = useState<string>('');
  const [conversation, setConversation] = useUIState();
  const { continueConversation } = useActions();

  return (
    <div>
      <div>
        {conversation.map((message: ClientMessage) => (
          <div key={message.id}>
            {message.role}: {message.display}
          </div>
        ))}
      </div>

      <div>
        <input
          type="text"
          value={input}
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button
          onClick={async () => {
            setConversation((currentConversation: ClientMessage[]) => [
              ...currentConversation,
              { id: generateId(), role: 'user', display: input },
            ]);

            const message = await continueConversation(input);

            setConversation((currentConversation: ClientMessage[]) => [
              ...currentConversation,
              message,
            ]);
          }}
        >
          Send Message
        </button>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining AI Server Actions and Tools with Vercel AI SDK (TSX)
DESCRIPTION: This snippet defines the core server-side logic for an AI conversation. It uses 'ai/rsc' to manage mutable AI state and stream UI updates. It includes tool definitions for 'showStockInformation' (fetching stock data) and 'showFlightStatus' (fetching flight status), demonstrating how to integrate external functionality into the AI's responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/90-render-visual-interface-in-chat.mdx#_snippet_3

LANGUAGE: TSX
CODE:
```
'use server';

import { getMutableAIState, streamUI } from 'ai/rsc';
import { openai } from '@ai-sdk/openai';
import { ReactNode } from 'react';
import { z } from 'zod';
import { generateId } from 'ai';
import { Stock } from '@/components/stock';
import { Flight } from '@/components/flight';

export interface ServerMessage {
  role: 'user' | 'assistant';
  content: string;
}

export interface ClientMessage {
  id: string;
  role: 'user' | 'assistant';
  display: ReactNode;
}

export async function continueConversation(
  input: string,
): Promise<ClientMessage> {
  'use server';

  const history = getMutableAIState();

  const result = await streamUI({
    model: openai('gpt-3.5-turbo'),
    messages: [...history.get(), { role: 'user', content: input }],
    text: ({ content, done }) => {
      if (done) {
        history.done((messages: ServerMessage[]) => [
          ...messages,
          { role: 'assistant', content },
        ]);
      }

      return <div>{content}</div>;
    },
    tools: {
      showStockInformation: {
        description:
          'Get stock information for symbol for the last numOfMonths months',
        parameters: z.object({
          symbol: z
            .string()
            .describe('The stock symbol to get information for'),
          numOfMonths: z
            .number()
            .describe('The number of months to get historical information for'),
        }),
        generate: async ({ symbol, numOfMonths }) => {
          history.done((messages: ServerMessage[]) => [
            ...messages,
            {
              role: 'assistant',
              content: `Showing stock information for ${symbol}`,
            },
          ]);

          return <Stock symbol={symbol} numOfMonths={numOfMonths} />;
        },
      },
      showFlightStatus: {
        description: 'Get the status of a flight',
        parameters: z.object({
          flightNumber: z
            .string()
            .describe('The flight number to get status for'),
        }),
        generate: async ({ flightNumber }) => {
          history.done((messages: ServerMessage[]) => [
            ...messages,
            {
              role: 'assistant',
              content: `Showing flight status for ${flightNumber}`,
            },
          ]);

          return <Flight flightNumber={flightNumber} />;
        },
      },
    },
  });

  return {
    id: generateId(),
    role: 'assistant',
    display: result.value,
  };
}
```

----------------------------------------

TITLE: Handling AI Assistant Messages and Tool Calls with OpenAI (TSX)
DESCRIPTION: This asynchronous function, `submitMessage`, serves as the primary entry point for user interactions with the AI assistant. It manages the lifecycle of an OpenAI thread, either creating a new one or appending to an existing one. The function streams the assistant's responses back to the client, updating both text and a dynamic UI. It also includes logic to detect and handle tool calls, specifically demonstrating how to execute a `search_emails` function and display its results within the streamed UI. Dependencies include 'ai' for streamable UI/values, 'openai' for API interaction, and local 'searchEmails' and 'Message' components.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/121-stream-assistant-response-with-tools.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
'use server';

import { generateId } from 'ai';
import { createStreamableUI, createStreamableValue } from 'ai/rsc';
import { OpenAI } from 'openai';
import { ReactNode } from 'react';
import { searchEmails } from './function';
import { Message } from './message';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export interface ClientMessage {
  id: string;
  status: ReactNode;
  text: ReactNode;
  gui: ReactNode;
}

const ASSISTANT_ID = 'asst_xxxx';
let THREAD_ID = '';
let RUN_ID = '';

export async function submitMessage(question: string): Promise<ClientMessage> {
  const status = createStreamableUI('thread.init');
  const textStream = createStreamableValue('');
  const textUIStream = createStreamableUI(
    <Message textStream={textStream.value} />,
  );
  const gui = createStreamableUI();

  const runQueue = [];

  (async () => {
    if (THREAD_ID) {
      await openai.beta.threads.messages.create(THREAD_ID, {
        role: 'user',
        content: question,
      });

      const run = await openai.beta.threads.runs.create(THREAD_ID, {
        assistant_id: ASSISTANT_ID,
        stream: true,
      });

      runQueue.push({ id: generateId(), run });
    } else {
      const run = await openai.beta.threads.createAndRun({
        assistant_id: ASSISTANT_ID,
        stream: true,
        thread: {
          messages: [{ role: 'user', content: question }],
        },
      });

      runQueue.push({ id: generateId(), run });
    }

    while (runQueue.length > 0) {
      const latestRun = runQueue.shift();

      if (latestRun) {
        for await (const delta of latestRun.run) {
          const { data, event } = delta;

          status.update(event);

          if (event === 'thread.created') {
            THREAD_ID = data.id;
          } else if (event === 'thread.run.created') {
            RUN_ID = data.id;
          } else if (event === 'thread.message.delta') {
            data.delta.content?.map((part: any) => {
              if (part.type === 'text') {
                if (part.text) {
                  textStream.append(part.text.value);
                }
              }
            });
          } else if (event === 'thread.run.requires_action') {
            if (data.required_action) {
              if (data.required_action.type === 'submit_tool_outputs') {
                const { tool_calls } = data.required_action.submit_tool_outputs;
                const tool_outputs = [];

                for (const tool_call of tool_calls) {
                  const { id: toolCallId, function: fn } = tool_call;
                  const { name, arguments: args } = fn;

                  if (name === 'search_emails') {
                    const { query, has_attachments } = JSON.parse(args);

                    gui.append(
                      <div className="flex flex-row gap-2 items-center">
                        <div>
                          Searching for emails: {query}, has_attachments:
                          {has_attachments ? 'true' : 'false'}
                        </div>
                      </div>,
                    );

                    await new Promise(resolve => setTimeout(resolve, 2000));

                    const fakeEmails = searchEmails({ query, has_attachments });

                    gui.append(
                      <div className="flex flex-col gap-2">
                        {fakeEmails.map(email => (
                          <div
                            key={email.id}
                            className="p-2 bg-zinc-100 rounded-md flex flex-row gap-2 items-center justify-between"
                          >
                            <div className="flex flex-row gap-2 items-center">
                              <div>{email.subject}</div>
                            </div>
                            <div className="text-zinc-500">{email.date}</div>
                          </div>
                        ))}
                      </div>,
                    );

                    tool_outputs.push({
```

----------------------------------------

TITLE: Saving Chat History with streamText onFinish Callback (TypeScript)
DESCRIPTION: This TypeScript route handler demonstrates saving chat messages using the `onFinish` callback of `streamText` from the AI SDK. It converts incoming messages, streams text from an OpenAI model, and then saves the complete chat history (including AI responses) to the database upon completion.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/10-migrating-to-ui.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { saveChat } from '@/utils/queries';
import { streamText, convertToCoreMessages } from 'ai';

export async function POST(request) {
  const { id, messages } = await request.json();

  const coreMessages = convertToCoreMessages(messages);

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'you are a friendly assistant!',
    messages: coreMessages,
    onFinish: async ({ response }) => {
      try {
        await saveChat({
          id,
          messages: [...coreMessages, ...response.messages],
        });
      } catch (error) {
        console.error('Failed to save chat');
      }
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Performing Parallel Code Review with AI-SDK in TypeScript
DESCRIPTION: This snippet demonstrates the "Parallel Processing" pattern by concurrently performing security, performance, and maintainability code reviews using multiple `generateObject` calls with specialized system prompts. It then aggregates the results into a summary using `generateText`. It leverages `@ai-sdk/openai` and `zod` for structured output.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, generateObject } from 'ai';
import { z } from 'zod';

// Example: Parallel code review with multiple specialized reviewers
async function parallelCodeReview(code: string) {
  const model = openai('gpt-4o');

  // Run parallel reviews
  const [securityReview, performanceReview, maintainabilityReview] =
    await Promise.all([
      generateObject({
        model,
        system:
          'You are an expert in code security. Focus on identifying security vulnerabilities, injection risks, and authentication issues.',
        schema: z.object({
          vulnerabilities: z.array(z.string()),
          riskLevel: z.enum(['low', 'medium', 'high']),
          suggestions: z.array(z.string()),
        }),
        prompt: `Review this code:
      ${code}`,
      }),

      generateObject({
        model,
        system:
          'You are an expert in code performance. Focus on identifying performance bottlenecks, memory leaks, and optimization opportunities.',
        schema: z.object({
          issues: z.array(z.string()),
          impact: z.enum(['low', 'medium', 'high']),
          optimizations: z.array(z.string()),
        }),
        prompt: `Review this code:
      ${code}`,
      }),

      generateObject({
        model,
        system:
          'You are an expert in code quality. Focus on code structure, readability, and adherence to best practices.',
        schema: z.object({
          concerns: z.array(z.string()),
          qualityScore: z.number().min(1).max(10),
          recommendations: z.array(z.string()),
        }),
        prompt: `Review this code:
      ${code}`,
      }),
    ]);

  const reviews = [
    { ...securityReview.object, type: 'security' },
    { ...performanceReview.object, type: 'performance' },
    { ...maintainabilityReview.object, type: 'maintainability' },
  ];

  // Aggregate results using another model instance
  const { text: summary } = await generateText({
    model,
    system: 'You are a technical lead summarizing multiple code reviews.',
    prompt: `Synthesize these code review results into a concise summary with key actions:
    ${JSON.stringify(reviews, null, 2)}`,
  });

  return { reviews, summary };
}
```

----------------------------------------

TITLE: Streaming UI Components from Server with AI SDK (`streamUI`)
DESCRIPTION: This server-side function `generateResponse` utilizes `streamUI` to stream React components directly to the client. It demonstrates yielding an initial 'loading...' component while the AI model processes the prompt, and then returning the final generated content wrapped in a `div`. Note that while the snippet is `.ts`, it should be renamed to `.tsx` to properly define React components within the `streamUI` function.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/06-loading-state.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
'use server';

import { openai } from '@ai-sdk/openai';
import { streamUI } from 'ai/rsc';

export async function generateResponse(prompt: string) {
  const result = await streamUI({
    model: openai('gpt-4o'),
    prompt,
    text: async function* ({ content }) {
      yield <div>loading...</div>;
      return <div>{content}</div>;
    },
  });

  return result.value;
}
```

----------------------------------------

TITLE: Streaming Object Generation with React Client Component (TSX)
DESCRIPTION: This React client component demonstrates how to initiate object generation and stream partial results to the UI. It uses `readStreamableValue` from `@ai-sdk/rsc` to process the incoming stream and update the component's state, displaying the generated notifications in real-time.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/40-stream-object.mdx#_snippet_0

LANGUAGE: TSX
CODE:
```
'use client';

import { useState } from 'react';
import { generate } from './actions';
import { readStreamableValue } from 'ai/rsc';

// Allow streaming responses up to 30 seconds
export const maxDuration = 30;

export default function Home() {
  const [generation, setGeneration] = useState<string>('');

  return (
    <div>
      <button
        onClick={async () => {
          const { object } = await generate('Messages during finals week.');

          for await (const partialObject of readStreamableValue(object)) {
            if (partialObject) {
              setGeneration(
                JSON.stringify(partialObject.notifications, null, 2)
              );
            }
          }
        }}
      >
        Ask
      </button>

      <pre>{generation}</pre>
    </div>
  );
}
```

----------------------------------------

TITLE: Updating Client to Use useChat Hook and Render Tool Invocations (AI SDK React)
DESCRIPTION: This snippet demonstrates how to update a Next.js client page to use the `@ai-sdk/react` `useChat` hook. It shows how to render messages and dynamically display components (like `Weather`) based on `toolInvocations` state, handling both 'result' and loading states for tools.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/10-migrating-to-ui.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';
import { Weather } from '@/components/weather';

export default function Page() {
  const { messages, input, setInput, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(message => (
        <div key={message.id}>
          <div>{message.role}</div>
          <div>{message.content}</div>

          <div>
            {message.toolInvocations.map(toolInvocation => {
              const { toolName, toolCallId, state } = toolInvocation;

              if (state === 'result') {
                const { result } = toolInvocation;

                return (
                  <div key={toolCallId}>
                    {toolName === 'displayWeather' ? (
                      <Weather weatherAtLocation={result} />
                    ) : null}
                  </div>
                );
              } else {
                return (
                  <div key={toolCallId}>
                    {toolName === 'displayWeather' ? (
                      <div>Loading weather...</div>
                    ) : null}
                  </div>
                );
              }
            })}
          </div>
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={input}
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Client-Side Text Completion with useCompletion Hook (React)
DESCRIPTION: This React component demonstrates how to use the `useCompletion` hook from `@ai-sdk/react` to create a user interface for text completions. It manages input, handles form submission, and displays the streamed completion from a `/api/completion` endpoint, providing a seamless real-time experience.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/05-completion.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useCompletion } from '@ai-sdk/react';

export default function Page() {
  const { completion, input, handleInputChange, handleSubmit } = useCompletion({
    api: '/api/completion',
  });

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="prompt"
        value={input}
        onChange={handleInputChange}
        id="input"
      />
      <button type="submit">Submit</button>
      <div>{completion}</div>
    </form>
  );
}
```

----------------------------------------

TITLE: Streaming Text with Image Prompt using AI SDK and Anthropic (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to use the AI SDK to stream text responses from a vision-language model. It utilizes Anthropic's Claude 3.5 Sonnet model, providing both a text prompt and an image (read from a local file) as input. The generated text is then streamed part by part to standard output.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/22-stream-text-with-image-prompt.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { anthropic } from '@ai-sdk/anthropic';
import { streamText } from 'ai';
import 'dotenv/config';
import fs from 'node:fs';

async function main() {
  const result = streamText({
    model: anthropic('claude-3-5-sonnet-20240620'),
    messages: [
      {
        role: 'user',
        content: [
          { type: 'text', text: 'Describe the image in detail.' },
          { type: 'image', image: fs.readFileSync('./data/comic-cat.png') }
        ]
      }
    ]
  });

  for await (const textPart of result.textStream) {
    process.stdout.write(textPart);
  }
}

main().catch(console.error);
```

----------------------------------------

TITLE: Logging Language Model Middleware (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a middleware for logging parameters and results of language model calls (`doGenerate` and `doStream`). It logs input parameters before the call and the generated text after the call. For streaming, it uses a `TransformStream` to capture the full generated text before logging it in the `flush` method.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/40-middleware.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import type { LanguageModelV1Middleware, LanguageModelV1StreamPart } from 'ai';

export const yourLogMiddleware: LanguageModelV1Middleware = {
  wrapGenerate: async ({ doGenerate, params }) => {
    console.log('doGenerate called');
    console.log(`params: ${JSON.stringify(params, null, 2)}`);

    const result = await doGenerate();

    console.log('doGenerate finished');
    console.log(`generated text: ${result.text}`);

    return result;
  },

  wrapStream: async ({ doStream, params }) => {
    console.log('doStream called');
    console.log(`params: ${JSON.stringify(params, null, 2)}`);

    const { stream, ...rest } = await doStream();

    let generatedText = '';

    const transformStream = new TransformStream<
      LanguageModelV1StreamPart,
      LanguageModelV1StreamPart
    >({
      transform(chunk, controller) {
        if (chunk.type === 'text-delta') {
          generatedText += chunk.textDelta;
        }

        controller.enqueue(chunk);
      },

      flush() {
        console.log('doStream finished');
        console.log(`generated text: ${generatedText}`);
      },
    });

    return {
      stream: stream.pipeThrough(transformStream),
      ...rest,
    };
  },
};
```

----------------------------------------

TITLE: Configuring OpenAI API Key
DESCRIPTION: This snippet shows how to set the `OPENAI_API_KEY` environment variable in a `.env` file, which is required for authenticating with the OpenAI API when using the AI SDK.
SOURCE: https://github.com/vercel/ai/blob/main/examples/hono/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
OPENAI_API_KEY="YOUR_OPENAI_API_KEY"
```

----------------------------------------

TITLE: Creating Server Action to Generate Chart Config (TypeScript)
DESCRIPTION: This snippet defines an asynchronous Server Action `generateChartConfig` that takes SQL query results and a user query. It uses the `generateObject` function with an AI model (gpt-4o) and the previously defined `configSchema` to generate the chart configuration. It then overrides the generated colors with shadcn theme colors before returning the configuration.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/04-natural-language-postgres.mdx#_snippet_15

LANGUAGE: TypeScript
CODE:
```
/* ...other imports... */
import { Config, configSchema, explanationsSchema, Result } from '@/lib/types';

/* ...rest of the file... */

export const generateChartConfig = async (
  results: Result[],
  userQuery: string,
) => {
  'use server';

  try {
    const { object: config } = await generateObject({
      model: openai('gpt-4o'),
      system: 'You are a data visualization expert.',
      prompt: `Given the following data from a SQL query result, generate the chart config that best visualises the data and answers the users query.
      For multiple groups use multi-lines.

      Here is an example complete config:
      export const chartConfig = {
        type: "pie",
        xKey: "month",
        yKeys: ["sales", "profit", "expenses"],
        colors: {
          sales: "#4CAF50",    // Green for sales
          profit: "#2196F3",   // Blue for profit
          expenses: "#F44336"  // Red for expenses
        },
        legend: true
      }

      User Query:
      ${userQuery}

      Data:
      ${JSON.stringify(results, null, 2)}`,
      schema: configSchema,
    });

    // Override with shadcn theme colors
    const colors: Record<string, string> = {};
    config.yKeys.forEach((key, index) => {
      colors[key] = `hsl(var(--chart-${index + 1}))`;
    });

    const updatedConfig = { ...config, colors };
    return { config: updatedConfig };
  } catch (e) {
    console.error(e);
    throw new Error('Failed to generate chart suggestion');
  }
};
```

----------------------------------------

TITLE: Passing PDF Files to OpenAI Responses API (TypeScript)
DESCRIPTION: This snippet demonstrates how to include a PDF file as part of the message content when interacting with an OpenAI model. It uses the `file` type within the `messages` array, specifying the file data, MIME type, and an optional filename. The model can then process the PDF content for question answering.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_25

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: openai.responses('gpt-4o'),
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'What is an embedding model?',
        },
        {
          type: 'file',
          data: fs.readFileSync('./data/ai.pdf'),
          mimeType: 'application/pdf',
          filename: 'ai.pdf', // optional
        },
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Generating Text with Claude 4 Sonnet using AI SDK
DESCRIPTION: This TypeScript snippet demonstrates how to perform a basic text generation call to Claude 4 Sonnet using the AI SDK. It imports the `anthropic` provider and `generateText` function, then sends a prompt to the model and logs the resulting text. This serves as a foundational example for integrating Anthropic models.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/18-claude-4.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { anthropic } from '@ai-sdk/anthropic';
import { generateText } from 'ai';

const { text, reasoning, reasoningDetails } = await generateText({
  model: anthropic('claude-4-sonnet-20250514'),
  prompt: 'How will quantum computing impact cryptography by 2050?',
});
console.log(text);
```

----------------------------------------

TITLE: Passing URL-based PDF to Anthropic Model (TypeScript)
DESCRIPTION: Demonstrates how to pass a PDF document to the Anthropic `claude-3-5-sonnet-20241022` model using a URL. The PDF is included in the `messages` array as a `file` type, with the `data` field containing a `URL` object pointing to the PDF and `mimeType` set to `application/pdf`. This allows the model to read and answer questions about the document's content.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/05-anthropic.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: anthropic('claude-3-5-sonnet-20241022'),
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'What is an embedding model according to this document?',
        },
        {
          type: 'file',
          data: new URL(
            'https://github.com/vercel/ai/blob/main/examples/ai-core/data/ai.pdf?raw=true',
          ),
          mimeType: 'application/pdf',
        },
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Use File Inputs with Vertex AI (generateText) - TypeScript
DESCRIPTION: Illustrates how to send file content, such as a PDF read from the filesystem, as part of a user message to a Vertex AI model using `generateText`. Explains the structure of the message content array including `type: 'file'`, `data` (the file content), and `mimeType`.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/16-google-vertex.mdx#_snippet_12

LANGUAGE: typescript
CODE:
```
import { vertex } from '@ai-sdk/google-vertex';
import { generateText } from 'ai';

const { text } = await generateText({
  model: vertex('gemini-1.5-pro'),
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'What is an embedding model according to this document?',
        },
        {
          type: 'file',
          data: fs.readFileSync('./data/ai.pdf'),
          mimeType: 'application/pdf',
        },
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Streaming Text with AI SDK's streamText (TypeScript)
DESCRIPTION: This code demonstrates how to use the `streamText` function for real-time text generation, which is essential for interactive applications like chatbots. It shows how to consume the `textStream` as an asynchronous iterable, allowing parts of the generated text to be processed as they become available, improving perceived performance.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/05-generating-text.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { streamText } from 'ai';

const result = streamText({
  model: yourModel,
  prompt: 'Invent a new holiday and describe its traditions.',
});

// example: use textStream as an async iterable
for await (const textPart of result.textStream) {
  console.log(textPart);
}
```

----------------------------------------

TITLE: Cancelling AI SDK Core Streams with abortSignal (Server-Side)
DESCRIPTION: This snippet demonstrates how to cancel server-side streams in AI SDK Core using the `abortSignal` argument. It shows forwarding the `req.signal` from an incoming request to the `streamText` function, allowing the LLM API call to be aborted if the client request is cancelled. This is useful for stopping unwanted or long-running server-to-LLM interactions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/02-stopping-streams.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { prompt } = await req.json();

  const result = streamText({
    model: openai('gpt-4-turbo'),
    prompt,
    // forward the abort signal:
    abortSignal: req.signal,
  });

  return result.toTextStreamResponse();
}
```

----------------------------------------

TITLE: Reading Streamable Value on Client with AI SDK RSC in TypeScript
DESCRIPTION: This client-side React component demonstrates how to consume a streamable value created on the server. Upon a button click, it calls a server action (`runThread`) and then uses `readStreamableValue` to asynchronously iterate over and log each update received from the stream, providing real-time feedback to the client.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/05-streaming-values.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { readStreamableValue } from 'ai/rsc';
import { runThread } from '@/actions';

export default function Page() {
  return (
    <button
      onClick={async () => {
        const { status } = await runThread();

        for await (const value of readStreamableValue(status)) {
          console.log(value);
        }
      }}
    >
      Ask
    </button>
  );
}
```

----------------------------------------

TITLE: Streaming Structured Data with streamObject (TypeScript)
DESCRIPTION: This snippet shows how to use `streamObject` from the AI SDK to stream structured data as it is generated by the model. It retrieves a `partialObjectStream` which can be iterated asynchronously to receive partial updates of the generated object, improving responsiveness for interactive use cases.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/10-generating-structured-data.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { streamObject } from 'ai';

const { partialObjectStream } = streamObject({
  // ...
});

// use partialObjectStream as an async iterable
for await (const partialObject of partialObjectStream) {
  console.log(partialObject);
}
```

----------------------------------------

TITLE: Client-side Object Streaming with useObject (TSX)
DESCRIPTION: This client-side component demonstrates using `experimental_useObject` from `@ai-sdk/react` to initiate and display streamed object generation. It shows how to bind the API endpoint and schema, and how to render partial results as they arrive, specifically handling potentially `undefined` values during streaming.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/40-stream-object.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { experimental_useObject as useObject } from '@ai-sdk/react';
import { notificationSchema } from './api/use-object/schema';

export default function Page() {
  const { object, submit } = useObject({
    api: '/api/use-object',
    schema: notificationSchema,
  });

  return (
    <div>
      <button onClick={() => submit('Messages during finals week.')}>
        Generate notifications
      </button>

      {object?.notifications?.map((notification, index) => (
        <div key={index}>
          <p>{notification?.name}</p>
          <p>{notification?.message}</p>
        </div>
      ))}
    </div>
  );
}
```

----------------------------------------

TITLE: Integrating OpenAI Assistant with React UI
DESCRIPTION: This client-side React component demonstrates how to use the `useAssistant` hook to manage the UI state for an OpenAI assistant. It displays messages, handles user input, and updates the UI automatically as the assistant streams responses. Key properties like `status`, `messages`, `input`, `submitMessage`, and `handleInputChange` are utilized for a dynamic chat interface.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/10-openai-assistants.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { Message, useAssistant } from '@ai-sdk/react';

export default function Chat() {
  const { status, messages, input, submitMessage, handleInputChange } =
    useAssistant({ api: '/api/assistant' });

  return (
    <div>
      {messages.map((m: Message) => (
        <div key={m.id}>
          <strong>{`${m.role}: `}</strong>
          {m.role !== 'data' && m.content}
          {m.role === 'data' && (
            <>
              {(m.data as any).description}
              <br />
              <pre className={'bg-gray-200'}>
                {JSON.stringify(m.data, null, 2)}
              </pre>
            </>
          )}
        </div>
      ))}

      {status === 'in_progress' && <div />}

      <form onSubmit={submitMessage}>
        <input
          disabled={status !== 'awaiting_message'}
          value={input}
          placeholder="What is the temperature in the living room?"
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining and Using AI Tools with generateText in TypeScript
DESCRIPTION: This TypeScript snippet demonstrates how to define and use a tool (`getWeather`) with an AI model (GPT-3.5-turbo) via the `generateText` function. It shows how the model can deterministically call a predefined function based on user prompts, such as retrieving weather information for a specified location, while also illustrating cases where no function is called if the prompt is out of scope. The `z.object` and `z.string` indicate a dependency on a Zod-like schema validation library for tool parameters.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/08-model-as-router.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
const sendMessage = (prompt: string) =>
  generateText({
    model: 'gpt-3.5-turbo',
    system: 'you are a friendly weather assistant!',
    prompt,
    tools: {
      getWeather: {
        description: 'Get the weather in a location',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for')
        }),
        execute: async ({ location }: { location: string }) => ({
          location,
          temperature: 72 + Math.floor(Math.random() * 21) - 10
        })
      }
    }
  });

sendMessage('What is the weather in San Francisco?'); // getWeather is called
sendMessage('What is the weather in New York?'); // getWeather is called
sendMessage('What events are happening in London?'); // No function is called
```

----------------------------------------

TITLE: Authenticating Server Action for Weather Data (TSX)
DESCRIPTION: This `getWeather` Server Action demonstrates how to implement authentication for server-side operations using Next.js and the AI SDK. It retrieves a 'token' from cookies, validates it using `validateToken`, and if unauthorized, returns an error. Upon successful authentication, it uses `createStreamableUI` to stream UI components like a `Skeleton` and `Weather` component, ensuring data is only returned to authorized users.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/09-authentication.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use server';

import { cookies } from 'next/headers';
import { createStremableUI } from 'ai/rsc';
import { validateToken } from '../utils/auth';

export const getWeather = async () => {
  const token = cookies().get('token');

  if (!token || !validateToken(token)) {
    return {
      error: 'This action requires authentication'
    };
  }
  const streamableDisplay = createStreamableUI(null);

  streamableDisplay.update(<Skeleton />);
  streamableDisplay.done(<Weather />);

  return {
    display: streamableDisplay.value
  };
};
```

----------------------------------------

TITLE: Consuming Streamed UI with React Error Boundary (TSX)
DESCRIPTION: This client-side React component consumes the streamed UI from the `getStreamedUI` action and displays it. It utilizes a `useState` hook to manage the streamed content and wraps the display in a `ErrorBoundary` component. This setup provides a robust mechanism to catch and handle rendering errors that might occur within the streamed UI, preventing the entire application from crashing.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/08-error-handling.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { getStreamedUI } from '@/actions';
import { useState } from 'react';
import { ErrorBoundary } from './ErrorBoundary';

export default function Page() {
  const [streamedUI, setStreamedUI] = useState(null);

  return (
    <div>
      <button
        onClick={async () => {
          const newUI = await getStreamedUI();
          setStreamedUI(newUI);
        }}
      >
        What does the new UI look like?
      </button>
      <ErrorBoundary>{streamedUI}</ErrorBoundary>
    </div>
  );
}
```

----------------------------------------

TITLE: Notifying on Each Completed Step in AI SDK
DESCRIPTION: This snippet illustrates the use of the 'onStepFinish' callback within 'generateText'. This callback is triggered once a step is fully completed, providing access to the step's text, tool calls, tool results, finish reason, and usage, enabling custom logic like chat history logging or usage tracking.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';

const result = await generateText({
  model: yourModel,
  maxSteps: 10,
  onStepFinish({ text, toolCalls, toolResults, finishReason, usage }) {
    // your own logic, e.g. for saving the chat history or recording usage
  },
  // ...
});
```

----------------------------------------

TITLE: Using Perplexity Sonar Models for Web Search
DESCRIPTION: This example illustrates how to integrate Perplexity's Sonar models for real-time web search. It generates text grounded in current web data, providing detailed citations, and is suitable for queries requiring up-to-date information.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/56-web-search-agent.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { perplexity } from '@ai-sdk/perplexity';
import { generateText } from 'ai';

const { text, sources } = await generateText({
  model: perplexity('sonar-pro'),
  prompt: 'What are the latest developments in quantum computing?',
});

console.log(text);
console.log(sources);
```

----------------------------------------

TITLE: Generating AI Response with AI SDK (TypeScript)
DESCRIPTION: This asynchronous function takes a history of messages and uses the AI SDK's `generateText` function to interact with the OpenAI `gpt-4o-mini` model. It provides a system prompt to define the bot's persona and behavior. After receiving the raw text response from the model, it formats specific markdown elements (links and bold text) into Slack's `mrkdwn` format before returning the result.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/03-slackbot.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { CoreMessage, generateText } from 'ai';

export const generateResponse = async (
  messages: CoreMessage[],
  updateStatus?: (status: string) => void,
) => {
  const { text } = await generateText({
    model: openai('gpt-4o-mini'),
    system: `You are a Slack bot assistant. Keep your responses concise and to the point.
    - Do not tag users.
    - Current date is: ${new Date().toISOString().split('T')[0]}`,
    messages,
  });

  // Convert markdown to Slack mrkdwn format
  return text.replace(/[(.*?)]\((.*?)\)/g, '<$2|$1>').replace(/\*\*/g, '*');
};
```

----------------------------------------

TITLE: Implementing Event Callbacks with useObject Hook in TypeScript
DESCRIPTION: This example illustrates the use of `onFinish` and `onError` callbacks within the `useObject` hook. `onFinish` is triggered upon successful object generation, providing the generated object and any schema validation errors. `onError` handles errors occurring during the API fetch request, allowing for custom error handling like logging.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/08-object-generation.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
'use client';

import { experimental_useObject as useObject } from '@ai-sdk/react';
import { notificationSchema } from './api/notifications/schema';

export default function Page() {
  const { object, submit } = useObject({
    api: '/api/notifications',
    schema: notificationSchema,
    onFinish({ object, error }) {
      // typed object, undefined if schema validation fails:
      console.log('Object generation completed:', object);

      // error, undefined if schema validation succeeds:
      console.log('Schema validation error:', error);
    },
    onError(error) {
      // error during fetch request:
      console.error('An error occurred:', error);
    },
  });

  return (
    <div>
      <button onClick={() => submit('Messages during finals week.')}>
        Generate notifications
      </button>

      {object?.notifications?.map((notification, index) => (
        <div key={index}>
          <p>{notification?.name}</p>
          <p>{notification?.message}</p>
        </div>
      ))}
    </div>
  );
}
```

----------------------------------------

TITLE: Defining `searchFlights` Tool with Zod and React Component Return (TSX)
DESCRIPTION: This snippet defines the `searchFlights` tool within an `actions.tsx` file, specifying its description and parameters using Zod for validation. The `generate` function simulates a flight search and returns a `Flights` React component to be rendered in the UI, demonstrating how server actions can yield and return React elements.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/04-multistep-interfaces.mdx#_snippet_5

LANGUAGE: TSX
CODE:
```
searchFlights: {
  description: 'search for flights',
  parameters: z.object({
    source: z.string().describe('The origin of the flight'),
    destination: z.string().describe('The destination of the flight'),
    date: z.string().describe('The date of the flight'),
  }),
  generate: async function* ({ source, destination, date }) {
    yield `Searching for flights from ${source} to ${destination} on ${date}...`;
    const results = await searchFlights(source, destination, date);
    return (<Flights flights={results} />);
  },
}
```

----------------------------------------

TITLE: Implementing generateQuery with AI SDK - TypeScript
DESCRIPTION: Provides the full implementation for the `generateQuery` Server Action. It uses `ai/generateObject` with `zod` to constrain the AI model's output to a JSON object containing a `query` string field, ensuring only the SQL query is returned. Includes basic error handling. Requires `@ai-sdk/openai`, `ai`, and `zod` dependencies.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/04-natural-language-postgres.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
/* ...other imports... */
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

/* ...rest of the file... */

export const generateQuery = async (input: string) => {
  'use server';
  try {
    const result = await generateObject({
      model: openai('gpt-4o'),
      system: `You are a SQL (postgres) ...`, // SYSTEM PROMPT AS ABOVE - OMITTED FOR BREVITY
      prompt: `Generate the query necessary to retrieve the data the user wants: ${input}`,
      schema: z.object({
        query: z.string(),
      }),
    });
    return result.object.query;
  } catch (e) {
    console.error(e);
    throw new Error('Failed to generate query');
  }
};
```

----------------------------------------

TITLE: Configuring Multi-Step Agentic Generations with AI SDK
DESCRIPTION: This example demonstrates how to enable multi-step (agentic) generations by adding the `maxSteps` parameter to the `streamText` function. Setting `maxSteps` allows the AI model to perform multiple tool invocations and subsequent generations without requiring user intervention, facilitating complex automated workflows.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/05-computer-use.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
const stream = streamText({
  model: anthropic('claude-3-5-sonnet-20241022'),
  prompt: 'Open the browser and navigate to vercel.com',
  tools: { computer: computerTool },
  maxSteps: 10
});
```

----------------------------------------

TITLE: Saving AI State with onSetAIState Callback in TypeScript
DESCRIPTION: This snippet demonstrates how to save the AI state using the `onSetAIState` callback provided by `createAI`. It saves the chat history to a database (`saveChatToDB`) only when the AI generation process is marked as `done`, ensuring persistence of completed interactions. This is crucial for maintaining application state across user sessions.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/03-saving-and-restoring-states.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
export const AI = createAI<ServerMessage[], ClientMessage[]> ({
  actions: {
    continueConversation,
  },
  onSetAIState: async ({ state, done }) => {
    'use server';

    if (done) {
      saveChatToDB(state);
    }
  },
});
```

----------------------------------------

TITLE: Streaming Text with FriendliAI Built-in Tools (TypeScript)
DESCRIPTION: This snippet demonstrates how to stream text responses from a FriendliAI model using built-in tools like `web:search` and `math:calculator`. It shows how to configure the model with specific tools to enable advanced capabilities for answering complex prompts requiring external data or calculations. The `streamText` function is used to process the response asynchronously.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/08-friendliai.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import { friendli } from '@friendliai/ai-provider';
import { streamText } from 'ai';

const result = streamText({
  model: friendli('meta-llama-3.3-70b-instruct', {
    tools: [{ type: 'web:search' }, { type: 'math:calculator' }],
  }),
  prompt:
    'Find the current USD to CAD exchange rate and calculate how much $5,000 USD would be in Canadian dollars.',
});

for await (const textPart of result.textStream) {
  console.log(textPart);
}
```

----------------------------------------

TITLE: Initializing AI State with Chat History in Next.js Layout (TSX)
DESCRIPTION: This snippet demonstrates how to initialize the AI SDK's state with existing chat history retrieved from a database. It uses the `initialAIState` prop of the `AI` component to pre-populate the conversation, ensuring continuity when a user revisits or continues a previous chat.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/60-save-messages-to-database.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { ServerMessage } from './actions';
import { AI } from './ai';

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  // get chat history from database
  const history: ServerMessage[] = getChat();

  return (
    <html lang="en">
      <body>
        <AI initialAIState={history} initialUIState={[]}>
          {children}
        </AI>
      </body>
    </html>
  );
}
```

----------------------------------------

TITLE: Enabling Tool Call Streaming in AI SDK API Route
DESCRIPTION: This snippet demonstrates how to enable `toolCallStreaming` in a server-side API route using `streamText`. When set to `true`, partial tool calls are streamed as they are generated, allowing for real-time UI updates.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-tool-usage.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
export async function POST(req: Request) {
  // ...

  const result = streamText({
    toolCallStreaming: true,
    // ...
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Streaming Text Without Reader using AI SDK and TypeScript
DESCRIPTION: This snippet demonstrates how to stream text generation results using the Vercel AI SDK and OpenAI's `gpt-3.5-turbo` model in TypeScript. It iterates directly over the `textStream` asynchronous iterator to log each part of the generated text as it arrives, suitable for simple real-time display.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/20-stream-text.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

const result = streamText({
  model: openai('gpt-3.5-turbo'),
  maxTokens: 512,
  temperature: 0.3,
  maxRetries: 5,
  prompt: 'Invent a new holiday and describe its traditions.',
});

for await (const textPart of result.textStream) {
  console.log(textPart);
}
```

----------------------------------------

TITLE: Defining AI SDK Tools for Weather and Temperature Conversion in Route Handler
DESCRIPTION: This server-side snippet defines two AI SDK tools, `weather` and `convertFahrenheitToCelsius`, within an API route handler. The `weather` tool simulates fetching weather data for a given location, while the `convertFahrenheitToCelsius` tool performs a temperature unit conversion. These tools extend the AI model's capabilities by allowing it to interact with external logic and data, enabling multi-step reasoning and more accurate responses.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/03-nextjs-pages-router.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamText, tool } from 'ai';
import { z } from 'zod';

export const maxDuration = 30;

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai('gpt-4o'),
    messages,
    tools: {
      weather: tool({
        description: 'Get the weather in a location (fahrenheit)',
        parameters: z.object({
          location: z.string().describe('The location to get the weather for'),
        }),
        execute: async ({ location }) => {
          const temperature = Math.round(Math.random() * (90 - 32) + 32);
          return {
            location,
            temperature,
          };
        },
      }),
      convertFahrenheitToCelsius: tool({
        description: 'Convert a temperature in fahrenheit to celsius',
        parameters: z.object({
          temperature: z
            .number()
            .describe('The temperature in fahrenheit to convert'),
        }),
        execute: async ({ temperature }) => {
          const celsius = Math.round((temperature - 32) * (5 / 9));
          return {
            celsius,
          };
        },
      }),
    },
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Streaming AI Text Completions with AI SDK UI Route Handler
DESCRIPTION: This Next.js API route handler demonstrates the recommended approach with AI SDK UI, separating AI text generation from UI rendering. It uses `streamText` to generate and stream the AI response based on incoming messages, returning a `DataStreamResponse` that can be consumed by client-side hooks like `useChat`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/10-migrating-to-ui.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

export async function POST(request) {
  const { messages } = await request.json();

  const result = streamText({
    model: openai('gpt-4o'),
    system: 'you are a friendly assistant!',
    messages,
    tools: {
      // tool definitions
    }
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Using Tools with AI SDK generateText (TSX)
DESCRIPTION: Shows how to integrate tools with the AI SDK's `generateText` function. It defines a 'weather' tool with a description, parameters (using Zod), and an execute function, allowing the model to call this tool based on the prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_4

LANGUAGE: TSX
CODE:
```
import { generateText, tool } from 'ai';
import { deepinfra } from '@ai-sdk/deepinfra';
import { getWeather } from './weatherTool';

const { text } = await generateText({
  model: deepinfra('meta-llama/Meta-Llama-3.1-70B-Instruct'),
  prompt: 'What is the weather like today?',
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
});
```

----------------------------------------

TITLE: Disabling Content Encoding for AI SDK Streaming Responses (TypeScript/React)
DESCRIPTION: This code snippet demonstrates how to configure the AI SDK's `toDataStreamResponse` method to prevent proxy middleware from compressing the streaming response. By explicitly setting the `'Content-Encoding'` header to `'none'`, it ensures that the stream is delivered without compression, resolving issues where streaming fails and only a full response is returned after a delay. This solution specifically targets the streaming API.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/09-troubleshooting/06-streaming-not-working-when-proxied.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
return result.toDataStreamResponse({
    headers: {
      'Content-Encoding': 'none',
    },
  });
```

----------------------------------------

TITLE: Adding Chunked Transfer-Encoding and Keep-Alive Headers in TSX
DESCRIPTION: This snippet demonstrates how to add 'Transfer-Encoding: chunked' and 'Connection: keep-alive' headers to the response using the AI SDK's 'toDataStreamResponse' method. These headers are crucial for ensuring proper streaming behavior in deployed environments, as they can prevent buffering issues that cause the full response to be sent at once instead of streamed.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/09-troubleshooting/06-streaming-not-working-when-deployed.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
return result.toDataStreamResponse({
  headers: {
    'Transfer-Encoding': 'chunked',
    Connection: 'keep-alive'
  }
});
```

----------------------------------------

TITLE: Implementing AI Response Generation with Tools (TypeScript)
DESCRIPTION: This snippet defines the `generateResponse` function which utilizes the AI SDK to process messages and potentially invoke tools. It includes definitions for a `getWeather` tool using Open-Meteo API and a `searchWeb` tool using Exa API. The function handles multi-step agentic flows and formats the final response for Slack.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/03-slackbot.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { CoreMessage, generateText, tool } from 'ai';
import { z } from 'zod';
import { exa } from './utils';

export const generateResponse = async (
  messages: CoreMessage[],
  updateStatus?: (status: string) => void,
) => {
  const { text } = await generateText({
    model: openai('gpt-4o'),
    system: `You are a Slack bot assistant. Keep your responses concise and to the point.
    - Do not tag users.
    - Current date is: ${new Date().toISOString().split('T')[0]}
    - Always include sources in your final response if you use web search.`,
    messages,
    maxSteps: 10,
    tools: {
      getWeather: tool({
        description: 'Get the current weather at a location',
        parameters: z.object({
          latitude: z.number(),
          longitude: z.number(),
          city: z.string(),
        }),
        execute: async ({ latitude, longitude, city }) => {
          updateStatus?.(`is getting weather for ${city}...`);

          const response = await fetch(
            `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current=temperature_2m,weathercode,relativehumidity_2m&timezone=auto`,
          );

          const weatherData = await response.json();
          return {
            temperature: weatherData.current.temperature_2m,
            weatherCode: weatherData.current.weathercode,
            humidity: weatherData.current.relativehumidity_2m,
            city,
          };
        },
      }),
      searchWeb: tool({
        description: 'Use this to search the web for information',
        parameters: z.object({
          query: z.string(),
          specificDomain: z
            .string()
            .nullable()
            .describe(
              'a domain to search if the user specifies e.g. bbc.com. Should be only the domain name without the protocol',
            ),
        }),
        execute: async ({ query, specificDomain }) => {
          updateStatus?.(`is searching the web for ${query}...`);
          const { results } = await exa.searchAndContents(query, {
            livecrawl: 'always',
            numResults: 3,
            includeDomains: specificDomain ? [specificDomain] : undefined,
          });

          return {
            results: results.map(result => ({
              title: result.title,
              url: result.url,
              snippet: result.text.slice(0, 1000),
            })),
          };
        },
      }),
    },
  });

  // Convert markdown to Slack mrkdwn format
  return text.replace(/[(.*?)]\((.*?)\)/g, '<$2|$1>').replace(/\*\*/g, '*');
};
```

----------------------------------------

TITLE: Implementing Web Scraping with Firecrawl in TypeScript
DESCRIPTION: This snippet defines a 'webSearch' tool (acting as a web crawler) using the 'ai' SDK and Firecrawl API. It enables an AI model to crawl a specified URL and retrieve its content in markdown or HTML format. It requires a 'FIRECRAWL_API_KEY' environment variable for authentication.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/56-web-search-agent.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';
import FirecrawlApp from '@mendable/firecrawl-js';
import 'dotenv/config';

const app = new FirecrawlApp({ apiKey: process.env.FIRECRAWL_API_KEY });

export const webSearch = tool({
  description: 'Search the web for up-to-date information',
  parameters: z.object({
    urlToCrawl: z
      .string()
      .url()
      .min(1)
      .max(100)
      .describe('The URL to crawl (including http:// or https://)'),
  }),
  execute: async ({ urlToCrawl }) => {
    const crawlResponse = await app.crawlUrl(urlToCrawl, {
      limit: 1,
      scrapeOptions: {
        formats: ['markdown', 'html'],
      },
    });
    if (!crawlResponse.success) {
      throw new Error(`Failed to crawl: ${crawlResponse.error}`);
    }
    return crawlResponse.data;
  },
});

const main = async () => {
  const { text } = await generateText({
    model: openai('gpt-4o-mini'), // can be any model that supports tools
    prompt: 'Get the latest blog post from vercel.com/blog',
    tools: {
      webSearch,
    },
    maxSteps: 2,
  });
  console.log(text);
};

main();
```

----------------------------------------

TITLE: Implementing Chat UI with AI SDK in Svelte
DESCRIPTION: This Svelte component sets up a basic chat interface using the `@ai-sdk/svelte` `Chat` class. It displays chat messages, iterating through `chat.messages` and their `parts`, and provides an input field bound to `chat.input` with a submit button that calls `chat.handleSubmit` to send user messages.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/04-svelte.mdx#_snippet_8

LANGUAGE: svelte
CODE:
```
<script>
  import { Chat } from '@ai-sdk/svelte';

  const chat = new Chat();
</script>

<main>
  <ul>
    {#each chat.messages as message, messageIndex (messageIndex)}
      <li>
        <div>{message.role}</div>
        <div>
          {#each message.parts as part, partIndex (partIndex)}
            {#if part.type === 'text'}
              <div>{part.text}</div>
            {/if}
          {/each}
        </div>
      </li>
    {/each}
  </ul>
  <form onsubmit={chat.handleSubmit}>
    <input bind:value={chat.input} />
    <button type="submit">Send</button>
  </form>
</main>
```

----------------------------------------

TITLE: Integrating LangSmith AISDKExporter with Sentry OpenTelemetry (Node.js)
DESCRIPTION: This snippet illustrates how to integrate the `AISDKExporter` with Sentry's OpenTelemetry instrumentation. It initializes the Sentry client and then adds the `AISDKExporter` as a `BatchSpanProcessor` to Sentry's trace provider, allowing LangSmith to receive traces alongside Sentry.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/05-observability/langsmith.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import * as Sentry from '@sentry/node';
import { BatchSpanProcessor } from '@opentelemetry/sdk-trace-base';
import { AISDKExporter } from 'langsmith/vercel';

const client = Sentry.init({
  dsn: '[Sentry DSN]',
  tracesSampleRate: 1.0,
});

client?.traceProvider?.addSpanProcessor(
  new BatchSpanProcessor(new AISDKExporter()),
);
```

----------------------------------------

TITLE: Generate Text with OpenAI using AI SDK (TSX)
DESCRIPTION: This snippet demonstrates how to use the AI SDK's `generateText` function with an OpenAI model (`gpt-4-turbo`). It shows importing the necessary functions and the OpenAI provider, then calling `generateText` with the model and a prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai('gpt-4-turbo'),
  prompt: 'What is love?',
});
```

----------------------------------------

TITLE: Querying OpenAI with AI SDK Core (TSX)
DESCRIPTION: Refactored example Route Handler showing how to query the OpenAI API using the unified AI SDK Core `streamText` function and the `@ai-sdk/openai` provider. It simplifies the process compared to using legacy provider SDKs directly.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/08-migration-guides/39-migration-guide-3-1.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = await streamText({
    model: openai('gpt-4-turbo'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Generating Multiple Embeddings with AI SDK and OpenAI (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `embedMany` function to generate embeddings for a list of text values. It imports `openai` from `@ai-sdk/openai` and `embedMany` from `ai`, then calls `embedMany` with an OpenAI embedding model and an array of strings. The function returns the generated `embeddings` which are in the same order as the input `values`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/07-reference/01-ai-sdk-core/06-embed-many.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { embedMany } from 'ai';

const { embeddings } = await embedMany({
  model: openai.embedding('text-embedding-3-small'),
  values: [
    'sunny day at the beach',
    'rainy afternoon in the city',
    'snowy night in the mountains',
  ],
});
```

----------------------------------------

TITLE: Defining Image Generation Tool in Next.js API Route (TypeScript)
DESCRIPTION: This server-side Next.js API route (`/api/chat`) processes chat messages, integrating an `experimental_generateImage` tool from the AI SDK. The `generateImage` tool uses DALL-E 3 to create images based on user prompts, returning base64 image data. It also includes logic to redact image data from messages before sending them to the AI model to prevent errors.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/12-generate-image-with-chat-prompt.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { experimental_generateImage, Message, streamText, tool } from 'ai';
import { z } from 'zod';

export async function POST(request: Request) {
  const { messages }: { messages: Message[] } = await request.json();

  // filter through messages and remove base64 image data to avoid sending to the model
  const formattedMessages = messages.map(m => {
    if (m.role === 'assistant' && m.toolInvocations) {
      m.toolInvocations.forEach(ti => {
        if (ti.toolName === 'generateImage' && ti.state === 'result') {
          ti.result.image = `redacted-for-length`;
        }
      });
    }
    return m;
  });

  const result = streamText({
    model: openai('gpt-4o'),
    messages: formattedMessages,
    tools: {
      generateImage: tool({
        description: 'Generate an image',
        parameters: z.object({
          prompt: z.string().describe('The prompt to generate the image from'),
        }),
        execute: async ({ prompt }) => {
          const { image } = await experimental_generateImage({
            model: openai.image('dall-e-3'),
            prompt,
          });
          // in production, save this image to blob storage and return a URL
          return { image: image.base64, prompt };
        },
      }),
    },
  });
  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Displaying Errors with useChat Hook in React
DESCRIPTION: This snippet demonstrates how to use the `error` object returned by the `useChat` hook to display an error message and a retry button in the UI. It also shows how to disable the input field when an error is present, preventing further submissions until the error is resolved or retried.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/21-error-handling.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit, error, reload } =
    useChat({});

  return (
    <div>
      {messages.map(m => (
        <div key={m.id}>
          {m.role}: {m.content}
        </div>
      ))}

      {error && (
        <>
          <div>An error occurred.</div>
          <button type="button" onClick={() => reload()}>
            Retry
          </button>
        </>
      )}

      <form onSubmit={handleSubmit}>
        <input
          value={input}
          onChange={handleInputChange}
          disabled={error != null}
        />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Handling Slack App Mentions with AI Response in TypeScript
DESCRIPTION: This function processes `app_mention` events. It checks if the message is from a bot, creates a status updater to indicate processing, retrieves thread history if applicable, calls an AI function (`generateResponse`) to get the response, and updates the initial status message with the final AI response. It utilizes a helper function `updateStatusUtil` to manage the initial message posting and subsequent updates.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/03-slackbot.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { AppMentionEvent } from '@slack/web-api';
import { client, getThread } from './slack-utils';
import { generateResponse } from './ai';

const updateStatusUtil = async (
  initialStatus: string,
  event: AppMentionEvent,
) => {
  const initialMessage = await client.chat.postMessage({
    channel: event.channel,
    thread_ts: event.thread_ts ?? event.ts,
    text: initialStatus,
  });

  if (!initialMessage || !initialMessage.ts)
    throw new Error('Failed to post initial message');

  const updateMessage = async (status: string) => {
    await client.chat.update({
      channel: event.channel,
      ts: initialMessage.ts as string,
      text: status,
    });
  };
  return updateMessage;
};

export async function handleNewAppMention(
  event: AppMentionEvent,
  botUserId: string,
) {
  console.log('Handling app mention');
  if (event.bot_id || event.bot_id === botUserId || event.bot_profile) {
    console.log('Skipping app mention');
    return;
  }

  const { thread_ts, channel } = event;
  const updateMessage = await updateStatusUtil('is thinking...', event);

  if (thread_ts) {
    const messages = await getThread(channel, thread_ts, botUserId);
    const result = await generateResponse(messages, updateMessage);
    updateMessage(result);
  } else {
    const result = await generateResponse(
      [{ role: 'user', content: event.text }],
      updateMessage,
    );
    updateMessage(result);
  }
}
```

----------------------------------------

TITLE: Generating Structured Data with AI SDK generateObject (TSX)
DESCRIPTION: Demonstrates how to use the `generateObject` function from the AI SDK to generate structured JSON data based on a Zod schema. It shows importing necessary modules, defining a schema for a recipe, and calling the function with a model and prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/21-llama-3_1.mdx#_snippet_3

LANGUAGE: TSX
CODE:
```
import { generateObject } from 'ai';
import { deepinfra } from '@ai-sdk/deepinfra';
import { z } from 'zod';

const { object } = await generateObject({
  model: deepinfra('meta-llama/Meta-Llama-3.1-70B-Instruct'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.object({ name: z.string(), amount: z.string() })),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});
```

----------------------------------------

TITLE: Handling LangChain Completion in a React Component with AI SDK
DESCRIPTION: This client-side React component utilizes the AI SDK's useCompletion hook to manage and display streamed AI completions. It provides state variables for the completion text and user input, along with handlers for input changes (handleInputChange) and form submission (handleSubmit). The component renders the streamed completion and an input field within a form, enabling interactive chat functionality.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/04-adapters/01-langchain.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
'use client';

import { useCompletion } from '@ai-sdk/react';

export default function Chat() {
  const { completion, input, handleInputChange, handleSubmit } =
    useCompletion();

  return (
    <div>
      {completion}
      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Persisting Chat History with AI SDK Responses API
DESCRIPTION: This snippet illustrates how to maintain chat history across multiple calls using the Responses API. It shows two sequential `generateText` calls where the second call uses the `previousResponseId` from the first result's provider metadata to provide context from the previous turn.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/19-openai-responses.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

const result1 = await generateText({
  model: openai.responses('gpt-4o-mini'),
  prompt: 'Invent a new holiday and describe its traditions.',
});

const result2 = await generateText({
  model: openai.responses('gpt-4o-mini'),
  prompt: 'Summarize in 2 sentences',
  providerOptions: {
    openai: {
      previousResponseId: result1.providerMetadata?.openai.responseId as string,
    },
  },
});
```

----------------------------------------

TITLE: Displaying Multi-Step AI Responses with AI SDK (Client)
DESCRIPTION: This client-side React component demonstrates how to consume and display multi-part AI responses using the `useChat` hook from `@ai-sdk/react`. It iterates through message parts, rendering text directly and formatting tool invocations as JSON, providing a unified UI for multi-step AI interactions.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/24-stream-text-multistep.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div>
      {messages?.map(message => (
        <div key={message.id}>
          <strong>{`${message.role}: `}</strong>
          {message.parts.map((part, index) => {
            switch (part.type) {
              case 'text':
                return <span key={index}>{part.text}</span>;
              case 'tool-invocation': {
                return (
                  <pre key={index}>
                    {JSON.stringify(part.toolInvocation, null, 2)}
                  </pre>
                );
              }
            }
          })}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Defining a `getWeather` Tool Returning JSON (TypeScript)
DESCRIPTION: This code modifies the `getWeather` tool's `execute` function to return a structured JSON object containing weather details (temperature, unit, description, forecast) instead of a plain string. This structured output is crucial for enabling the rendering of rich UI components based on the model's tool calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/07-rendering-ui-with-language-models.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
const text = generateText({
  model: openai('gpt-3.5-turbo'),
  system: 'You are a friendly assistant',
  prompt: 'What is the weather in SF?',
  tools: {
    getWeather: {
      description: 'Get the weather for a location',
      parameters: z.object({
        city: z.string().describe('The city to get the weather for'),
        unit: z
          .enum(['C', 'F'])
          .describe('The unit to display the temperature in'),
      }),
      execute: async ({ city, unit }) => {
        const weather = getWeather({ city, unit });
        const { temperature, unit, description, forecast } = weather;

        return {
          temperature,
          unit,
          description,
          forecast,
        };
      },
    },
  },
});
```

----------------------------------------

TITLE: Installing AI SDK and Anthropic Provider
DESCRIPTION: This command installs the core AI SDK package and the Anthropic AI SDK provider, which are prerequisites for integrating Anthropic's models and tools into your application.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/05-computer-use.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
pnpm add ai @ai-sdk/anthropic
```

----------------------------------------

TITLE: Integrating Tool Results into Chat UI in TSX
DESCRIPTION: This TSX snippet updates the main 'Page' component to dynamically render results from AI tool invocations within a chat interface. It uses 'useChat' from '@ai-sdk/react' to manage messages and input. The component iterates through 'message.toolInvocations', conditionally rendering the 'Weather' or 'Stock' component based on the 'toolName' and 'state' of the tool call, providing a rich Generative UI experience.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/04-generative-user-interfaces.mdx#_snippet_8

LANGUAGE: TSX
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';
import { Weather } from '@/components/weather';
import { Stock } from '@/components/stock';

export default function Page() {
  const { messages, input, setInput, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(message => (
        <div key={message.id}>
          <div>{message.role}</div>
          <div>{message.content}</div>

          <div>
            {message.toolInvocations?.map(toolInvocation => {
              const { toolName, toolCallId, state } = toolInvocation;

              if (state === 'result') {
                if (toolName === 'displayWeather') {
                  const { result } = toolInvocation;
                  return (
                    <div key={toolCallId}>
                      <Weather {...result} />
                    </div>
                  );
                } else if (toolName === 'getStockPrice') {
                  const { result } = toolInvocation;
                  return <Stock key={toolCallId} {...result} />;
                }
              } else {
                return (
                  <div key={toolCallId}>
                    {toolName === 'displayWeather' ? (
                      <div>Loading weather...</div>
                    ) : toolName === 'getStockPrice' ? (
                      <div>Loading stock price...</div>
                    ) : (
                      <div>Loading...</div>
                    )}
                  </div>
                );
              }
            })}
          </div>
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={input}
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Using Tools with AI SDK Core (TSX)
DESCRIPTION: Illustrates how to define and use a tool (`getWeather`) with the AI SDK `generateText` function. It shows importing `generateText` and `tool`, defining the tool with a description, parameters (using Zod), and an `execute` function, and passing it to `generateText`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/24-o3.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai('o3-mini'),
  prompt: 'What is the weather like today in San Francisco?',
  tools: {
    getWeather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
});
```

----------------------------------------

TITLE: Embedding a Single Value using AI SDK with OpenAI
DESCRIPTION: This snippet demonstrates how to embed a single text value using the `embed` function from the AI SDK. It utilizes the OpenAI `text-embedding-3-small` model to convert the input string into a numerical vector (embedding). The result is a single embedding object, which is an array of numbers.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/30-embeddings.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { embed } from 'ai';
import { openai } from '@ai-sdk/openai';

// 'embedding' is a single embedding object (number[])
const { embedding } = await embed({
  model: openai.embedding('text-embedding-3-small'),
  value: 'sunny day at the beach',
});
```

----------------------------------------

TITLE: Implementing Chat UI with useChat Hook in Next.js
DESCRIPTION: This snippet demonstrates how to build a client-side chat interface using the `useChat` hook from `@ai-sdk/react`. It manages chat messages, user input, and form submission, rendering messages and providing an input field for new messages. Dependencies include `@ai-sdk/react`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/10-migrating-to-ui.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, setInput, handleSubmit } = useChat();

  return (
    <div>
      {messages.map(message => (
        <div key={message.id}>
          <div>{message.role}</div>
          <div>{message.content}</div>
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={input}
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Implement Chat UI with useChat Hook in Next.js
DESCRIPTION: Creates a React component for a Next.js page that utilizes the useChat hook from @ai-sdk/react to manage chat state, handle user input, submit messages to the API route, and display the conversation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/22-gpt-4-5.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();

  return (
    <>
      {messages.map(message => (
        <div key={message.id}>
          {message.role === 'user' ? 'User: ' : 'AI: '}
          {message.content}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input name="prompt" value={input} onChange={handleInputChange} />
        <button type="submit">Submit</button>
      </form>
    </>
  );
}
```

----------------------------------------

TITLE: Defining Server-Side AI Tool for Parallel Weather Queries
DESCRIPTION: This server-side snippet defines an AI tool (`getWeather`) using the AI SDK and OpenAI. It demonstrates how to integrate a custom function for fetching weather data, specifying its parameters with Zod, and executing it within the `generateText` function to allow parallel tool calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/51-call-tools-in-parallel.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
'use server';

import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

export interface Message {
  role: 'user' | 'assistant';
  content: string;
}

function getWeather({ city, unit }) {
  // This function would normally make an
  // API request to get the weather.

  return { value: 25, description: 'Sunny' };
}

export async function continueConversation(history: Message[]) {
  'use server';

  const { text, toolResults } = await generateText({
    model: openai('gpt-3.5-turbo'),
    system: 'You are a friendly weather assistant!',
    messages: history,
    tools: {
      getWeather: {
        description: 'Get the weather for a location',
        parameters: z.object({
          city: z.string().describe('The city to get the weather for'),
          unit: z
            .enum(['C', 'F'])
            .describe('The unit to display the temperature in'),
        }),
        execute: async ({ city, unit }) => {
          const weather = getWeather({ city, unit });
          return `It is currently ${weather.value}°${unit} and ${weather.description} in ${city}!`;
        },
      },
    },
  });

  return {
    messages: [
      ...history,
      {
        role: 'assistant' as const,
        content:
          text || toolResults.map(toolResult => toolResult.result).join('\n'),
      },
    ],
  };
}
```

----------------------------------------

TITLE: Using the onStepFinish Callback (TSX)
DESCRIPTION: Shows how to provide an `onStepFinish` callback to `generateText` or `streamText`. This callback is triggered after each step completes, providing access to the step's text, tool calls, tool results, finish reason, and usage. Useful for logging or state management.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/15-tools-and-tool-calling.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { generateText } from 'ai';

const result = await generateText({
  // ...
  onStepFinish({ text, toolCalls, toolResults, finishReason, usage }) {
    // your own logic, e.g. for saving the chat history or recording usage
  },
});
```

----------------------------------------

TITLE: Implementing Stop Word Transformation with Stream Control in Vercel AI (TypeScript)
DESCRIPTION: This example illustrates a custom TransformStream that can stop the text stream prematurely if a specific "STOP" word is detected in a 'text-delta' chunk. It's crucial to simulate 'step-finish' and 'finish' events after stopping the stream to ensure all callbacks are invoked and a well-formed stream is returned.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/05-generating-text.mdx#_snippet_10

LANGUAGE: ts
CODE:
```
const stopWordTransform =
  <TOOLS extends ToolSet>() =>
  ({ stopStream }: { stopStream: () => void }) =>
    new TransformStream<TextStreamPart<TOOLS>, TextStreamPart<TOOLS>>({
      // note: this is a simplified transformation for testing;
      // in a real-world version more there would need to be
      // stream buffering and scanning to correctly emit prior text
      // and to detect all STOP occurrences.
      transform(chunk, controller) {
        if (chunk.type !== 'text-delta') {
          controller.enqueue(chunk);
          return;
        }

        if (chunk.textDelta.includes('STOP')) {
          // stop the stream
          stopStream();

          // simulate the step-finish event
          controller.enqueue({
            type: 'step-finish',
            finishReason: 'stop',
            logprobs: undefined,
            usage: {
              completionTokens: NaN,
              promptTokens: NaN,
              totalTokens: NaN,
            },
            request: {},
            response: {
              id: 'response-id',
              modelId: 'mock-model-id',
              timestamp: new Date(0),
            },
            warnings: [],
            isContinued: false,
          });

          // simulate the finish event
          controller.enqueue({
            type: 'finish',
            finishReason: 'stop',
            logprobs: undefined,
            usage: {
              completionTokens: NaN,
              promptTokens: NaN,
              totalTokens: NaN,
            },
            response: {
              id: 'response-id',
              modelId: 'mock-model-id',
              timestamp: new Date(0),
            },
          });

          return;
        }

        controller.enqueue(chunk);
      },
    });
```

----------------------------------------

TITLE: Client-Side Conditional UI Rendering in React
DESCRIPTION: This snippet demonstrates client-side conditional rendering of React components based on the 'message.name' property, which typically corresponds to a tool call. It checks the message role and then dynamically renders different UI components (e.g., Courses, People, Meetings) by passing the 'message.content' as props. This approach centralizes UI logic on the client, requiring the client to manage and render all possible UI variations.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/07-rendering-ui-with-language-models.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
{
  message.role === 'tool' ? (
    message.name === 'api-search-course' ? (
      <Courses courses={message.content} />
    ) : message.name === 'api-search-profile' ? (
      <People people={message.content} />
    ) : message.name === 'api-meetings' ? (
      <Meetings meetings={message.content} />
    ) : message.name === 'api-search-building' ? (
      <Buildings buildings={message.content} />
    ) : message.name === 'api-events' ? (
      <Events events={message.content} />
    ) : message.name === 'api-meals' ? (
      <Meals meals={message.content} />
    ) : null
  ) : (
    <div>{message.content}</div>
  );
}
```

----------------------------------------

TITLE: Wrapping Application with AI Provider (React, TSX)
DESCRIPTION: This layout component wraps the application's children with the `AI` component, making the AI context and actions available to all nested components. This is a crucial step for integrating the Vercel AI SDK into a React application.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/120-stream-assistant-response.mdx#_snippet_4

LANGUAGE: TSX
CODE:
```
import { ReactNode } from 'react';
import { AI } from './ai';

export default function Layout({ children }: { children: ReactNode }) {
  return <AI>{children}</AI>;
}
```

----------------------------------------

TITLE: Text Generation with AI SDK Responses API (After Migration)
DESCRIPTION: This snippet shows the updated syntax for text generation using the new AI SDK Responses API. The model is now instantiated using `openai.responses(modelId)`, illustrating the primary change when migrating from the Completions API.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/19-openai-responses.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
// Responses API
const { text } = await generateText({
  model: openai.responses('gpt-4o'),
  prompt: 'Explain the concept of quantum entanglement.',
});
```

----------------------------------------

TITLE: Configuring HTTP Request Options for `useCompletion` Hook in TypeScript
DESCRIPTION: This snippet illustrates how to customize the HTTP POST request sent by the `useCompletion` hook. It shows how to change the API endpoint, add custom headers, include additional fields in the request body, and set credentials, providing flexibility for server-side integration and authentication.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/05-completion.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const { messages, input, handleInputChange, handleSubmit } = useCompletion({
  api: '/api/custom-completion',
  headers: {
    Authorization: 'your_token'
  },
  body: {
    user_id: '123'
  },
  credentials: 'same-origin'
});
```

----------------------------------------

TITLE: Grouping Multiple AI SDK Executions in a Langfuse Trace (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a parent Langfuse trace and associate multiple AI SDK `generateText` calls with it. By passing the `parentTraceId` via `experimental_telemetry.metadata.langfuseTraceId`, all subsequent generations are grouped under the same trace, with `functionId` defining the root span name for each execution.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/05-observability/langfuse.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { randomUUID } from 'crypto';
import { Langfuse } from 'langfuse';

const langfuse = new Langfuse();
const parentTraceId = randomUUID();

langfuse.trace({
  id: parentTraceId,
  name: 'holiday-traditions',
});

for (let i = 0; i < 3; i++) {
  const result = await generateText({
    model: openai('gpt-3.5-turbo'),
    maxTokens: 50,
    prompt: 'Invent a new holiday and describe its traditions.',
    experimental_telemetry: {
      isEnabled: true,
      functionId: `holiday-tradition-${i}`,
      metadata: {
        langfuseTraceId: parentTraceId,
        langfuseUpdateParent: false, // Do not update the parent trace with execution results
      },
    },
  });

  console.log(result.text);
}

await langfuse.flushAsync();
await sdk.shutdown();
```

----------------------------------------

TITLE: Handling Multi-modal Tool Results with Anthropic (TypeScript)
DESCRIPTION: This snippet demonstrates how to specify multi-part and multi-modal tool results using the `experimental_content` property within a 'tool' role message. It shows how to include both a regular `result` for models without multi-part support and a `content` array for models that do, allowing for text and image data. This feature is experimental and currently only supported by Anthropic models.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/03-prompts.mdx#_snippet_18

LANGUAGE: ts
CODE:
```
const result = await generateText({
  model: yourModel,
  messages: [
    // ...
    {
      role: 'tool',
      content: [
        {
          type: 'tool-result',
          toolCallId: '12345', // needs to match the tool call id
          toolName: 'get-nutrition-data',
          // for models that do not support multi-part tool results,
          // you can include a regular result part:
          result: {
            name: 'Cheese, roquefort',
            calories: 369,
            fat: 31,
            protein: 22,
          },
          // for models that support multi-part tool results,
          // you can include a multi-part content part:
          content: [
            {
              type: 'text',
              text: 'Here is an image of the nutrition data for the cheese:',
            },
            {
              type: 'image',
              data: fs.readFileSync('./data/roquefort-nutrition-data.png'),
              mimeType: 'image/png',
            },
          ],
        }
      ]
    }
  ]
});
```

----------------------------------------

TITLE: Stop Agent by Custom Condition (TypeScript)
DESCRIPTION: Configures the agent to stop execution based on a custom condition, such as reaching a maximum total token count, using a function like `maxTotalTokens`.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/01-announcing-ai-sdk-5-alpha/index.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
const result = generateText({
  // ...
  // stop loop at your own custom condition
  continueUntil: maxTotalTokens(20000),
});
```

----------------------------------------

TITLE: Sending Custom Data with AI SDK Data Stream in Nest.js (TypeScript)
DESCRIPTION: This example illustrates how to send custom data alongside AI-generated content using `pipeDataStreamToResponse` with an `execute` function. It allows writing initial data to the stream and then merging the AI stream into the same data stream, including a basic error handling mechanism.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/15-api-servers/50-nest.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { Controller, Post, Res } from '@nestjs/common';
import { openai } from '@ai-sdk/openai';
import { pipeDataStreamToResponse, streamText } from 'ai';
import { Response } from 'express';

@Controller()
export class AppController {
  @Post('/stream-data')
  async streamData(@Res() res: Response) {
    pipeDataStreamToResponse(res, {
      execute: async dataStreamWriter => {
        dataStreamWriter.writeData('initialized call');

        const result = streamText({
          model: openai('gpt-4o'),
          prompt: 'Invent a new holiday and describe its traditions.',
        });

        result.mergeIntoDataStream(dataStreamWriter);
      },
      onError: error => {
        // Error messages are masked by default for security reasons.
        // If you want to expose the error message to the client, you can do so here:
        return error instanceof Error ? error.message : String(error);
      },
    });
  }
}
```

----------------------------------------

TITLE: Creating Flight Status Display Component (TypeScript/React)
DESCRIPTION: This asynchronous React component fetches and displays flight status information for a given flight number. It makes an API call to `https://api.example.com/flight/{flightNumber}` and renders the flight number, status, source, and destination details.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/90-render-visual-interface-in-chat.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
export async function Flight({ flightNumber }) {
  const data = await fetch(`https://api.example.com/flight/${flightNumber}`);

  return (
    <div>
      <div>{flightNumber}</div>
      <div>{data.status}</div>
      <div>{data.source}</div>
      <div>{data.destination}</div>
    </div>
  );
}
```

----------------------------------------

TITLE: Initializing AI Provider for Conversation Management (TypeScript)
DESCRIPTION: This snippet initializes the AI provider using `createAI` from `ai/rsc`. It defines how server messages are mapped to client UI messages, registers the `continueConversation` action, and implements callbacks for persisting AI state (`onSetAIState`) and transforming server state for UI rendering (`onGetUIState`).
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/60-save-messages-to-database.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { createAI } from 'ai/rsc';
import { ServerMessage, ClientMessage, continueConversation } from './actions';

export const AI = createAI<ServerMessage[], ClientMessage[]> ({
  actions: {
    continueConversation,
  },
  onSetAIState: async ({ state, done }) => {
    'use server';

    if (done) {
      saveChat(state);
    }
  },
  onGetUIState: async () => {
    'use server';

    const history: ServerMessage[] = getAIState();

    return history.map(({ role, content }) => ({
      id: generateId(),
      role,
      display:
        role === 'function' ? <Stock {...JSON.parse(content)} /> : content,
    }));
  },
});
```

----------------------------------------

TITLE: Restoring AI State with initialAIState Prop in TypeScript
DESCRIPTION: This example illustrates how to restore the AI state by passing the `initialAIState` prop to the `AI` context provider. It asynchronously loads the chat history from a database (`loadChatFromDB`) and uses it to initialize the AI state when the `RootLayout` component is mounted, ensuring that the application starts with the previously saved state.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/03-saving-and-restoring-states.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { ReactNode } from 'react';
import { AI } from './ai';

export default async function RootLayout({
  children,
}: Readonly<{ children: ReactNode }>) {
  const chat = await loadChatFromDB();

  return (
    <html lang="en">
      <body>
        <AI initialAIState={chat}>{children}</AI>
      </body>
    </html>
  );
}
```

----------------------------------------

TITLE: Using AI SDK's useCompletion Hook in a React Component
DESCRIPTION: This React client component (`app/page.tsx`) showcases how to consume a completion stream from an API route using the AI SDK's `useCompletion` hook. It provides a simple UI to display the streamed completion, an input field for user prompts, and handles form submission to trigger the completion process. This component depends on the `@ai-sdk/react` package for its functionality.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/04-adapters/02-llamaindex.mdx#_snippet_1

LANGUAGE: TSX
CODE:
```
'use client';

import { useCompletion } from '@ai-sdk/react';

export default function Chat() {
  const { completion, input, handleInputChange, handleSubmit } =
    useCompletion();

  return (
    <div>
      {completion}
      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Backend API Route for AI Chat with Tool Calling (Next.js)
DESCRIPTION: This snippet defines a Next.js API route (`POST` handler) for processing chat messages. It uses the AI SDK to stream text from an OpenAI model (`gpt-4o`) and includes a `getWeatherInformation` tool. The tool is defined with a description and parameters using `zod`, and its `execute` function simulates fetching weather data, merging the AI generation into a `DataStreamResponse`.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/75-human-in-the-loop.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { createDataStreamResponse, streamText, tool } from 'ai';
import { z } from 'zod';

export async function POST(req: Request) {
  const { messages } = await req.json();

  return createDataStreamResponse({
    execute: async dataStream => {
      const result = streamText({
        model: openai('gpt-4o'),
        messages,
        tools: {
          getWeatherInformation: tool({
            description: 'show the weather in a given city to the user',
            parameters: z.object({ city: z.string() }),
            execute: async ({}: { city: string }) => {
              const weatherOptions = ['sunny', 'cloudy', 'rainy', 'snowy'];
              return weatherOptions[
                Math.floor(Math.random() * weatherOptions.length)
              ];
            },
          }),
        },
      });

      result.mergeIntoDataStream(dataStream);
    },
  });
}
```

----------------------------------------

TITLE: Server-side streamObject for Array Output (TypeScript)
DESCRIPTION: This server-side API route uses streamObject from ai to generate an array of notification objects. It configures the model (GPT-4 Turbo), specifies 'output: 'array'', and uses the notificationSchema to guide the generation, returning the result as a text stream response. It also sets maxDuration for the API route.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/40-stream-object.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { streamObject } from 'ai';
import { notificationSchema } from './schema';

export const maxDuration = 30;

export async function POST(req: Request) {
  const context = await req.json();

  const result = streamObject({
    model: openai('gpt-4-turbo'),
    output: 'array',
    schema: notificationSchema,
    prompt:
      `Generate 3 notifications for a messages app in this context:` + context,
  });

  return result.toTextStreamResponse();
}
```

----------------------------------------

TITLE: Handling Server-Side Assistant Responses and Tool Calls (Next.js API)
DESCRIPTION: This Next.js API route (`app/api/assistant/route.ts`) handles incoming chat messages, interacts with the OpenAI Assistant API, and streams responses back to the client. It manages thread creation, message submission, and crucially, processes tool calls (like 'celsiusToFahrenheit') by executing the tool's logic and submitting the outputs back to the assistant to continue the conversation.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/121-stream-assistant-response-with-tools.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { AssistantResponse } from 'ai';
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY || '',
});

export async function POST(req: Request) {
  const input: {
    threadId: string | null;
    message: string;
  } = await req.json();

  const threadId = input.threadId ?? (await openai.beta.threads.create({})).id;

  const createdMessage = await openai.beta.threads.messages.create(threadId, {
    role: 'user',
    content: input.message,
  });

  return AssistantResponse(
    { threadId, messageId: createdMessage.id },
    async ({ forwardStream }) => {
      const runStream = openai.beta.threads.runs.stream(threadId, {
        assistant_id:
          process.env.ASSISTANT_ID ??
          (() => {
            throw new Error('ASSISTANT_ID is not set');
          })(),
      });

      let runResult = await forwardStream(runStream);

      while (
        runResult?.status === 'requires_action' &&
        runResult.required_action?.type === 'submit_tool_outputs'
      ) {
        const tool_outputs =
          runResult.required_action.submit_tool_outputs.tool_calls.map(
            (toolCall: any) => {
              const parameters = JSON.parse(toolCall.function.arguments);

              switch (toolCall.function.name) {
                case 'celsiusToFahrenheit':
                  const celsius = parseFloat(parameters.value);
                  const fahrenheit = celsius * (9 / 5) + 32;

                  return {
                    tool_call_id: toolCall.id,
                    output: `${celsius}°C is ${fahrenheit.toFixed(2)}°F`,
                  };

                default:
                  throw new Error(
                    `Unknown tool call function: ${toolCall.function.name}`,
                  );
              }
            },
          );

        runResult = await forwardStream(
          openai.beta.threads.runs.submitToolOutputsStream(
            threadId,
            runResult.id,
            { tool_outputs },
          ),
        );
      }
    },
  );
}
```

----------------------------------------

TITLE: Client-side Object Generation with useObject (TSX)
DESCRIPTION: This client-side code demonstrates the use of the `experimental_useObject` hook from `@ai-sdk/react` to initiate and stream structured JSON object generation. It connects to the `/api/notifications` endpoint, uses the predefined `notificationSchema`, and renders partial results as they are received, handling potentially `undefined` values during streaming.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/08-object-generation.mdx#_snippet_1

LANGUAGE: TSX
CODE:
```
'use client';

import { experimental_useObject as useObject } from '@ai-sdk/react';
import { notificationSchema } from './api/notifications/schema';

export default function Page() {
  const { object, submit } = useObject({
    api: '/api/notifications',
    schema: notificationSchema,
  });

  return (
    <>
      <button onClick={() => submit('Messages during finals week.')}>
        Generate notifications
      </button>

      {object?.notifications?.map((notification, index) => (
        <div key={index}>
          <p>{notification?.name}</p>
          <p>{notification?.message}</p>
        </div>
      ))}
    </>
  );
}
```

----------------------------------------

TITLE: Testing streamText with MockLanguageModelV1 and simulateReadableStream in TypeScript
DESCRIPTION: This example illustrates how to unit test the `streamText` function using `MockLanguageModelV1` and `simulateReadableStream`. The `doStream` method of the mock model is configured to emit a sequence of text chunks and a finish event, simulating a streaming response for testing purposes.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/55-testing.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { streamText, simulateReadableStream } from 'ai';
import { MockLanguageModelV1 } from 'ai/test';

const result = streamText({
  model: new MockLanguageModelV1({
    doStream: async () => ({
      stream: simulateReadableStream({
        chunks: [
          { type: 'text-delta', textDelta: 'Hello' },
          { type: 'text-delta', textDelta: ', ' },
          { type: 'text-delta', textDelta: `world!` },
          {
            type: 'finish',
            finishReason: 'stop',
            logprobs: undefined,
            usage: { completionTokens: 10, promptTokens: 3 }
          }
        ]
      }),
      rawCall: { rawPrompt: null, rawSettings: {} }
    })
  }),
  prompt: 'Hello, test!',
});
```

----------------------------------------

TITLE: Generate Text with OpenAI o1-mini (AI SDK)
DESCRIPTION: Demonstrates how to use the AI SDK Core's `generateText` function to call the OpenAI `o1-mini` model. It imports the necessary functions and the OpenAI provider, then calls `generateText` with the model and a prompt to get a text response.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/23-o1.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai('o1-mini'),
  prompt: 'Explain the concept of quantum entanglement.',
});
```

----------------------------------------

TITLE: Generating Structured Data with AI SDK and Zod TypeScript
DESCRIPTION: Illustrates generating type-safe structured JSON data using the AI SDK's `generateObject` function. It defines a Zod schema for a recipe and prompts the model to generate data conforming to that schema, ensuring predictable output structure.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/22-gpt-4-5.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const { object } = await generateObject({
  model: openai('gpt-4.5-preview'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.object({ name: z.string(), amount: z.string() })),
      steps: z.array(z.string())
    })
  }),
  prompt: 'Generate a lasagna recipe.'
});
```

----------------------------------------

TITLE: Handling Client Disconnects in AI SDK Route (TSX)
DESCRIPTION: This code snippet demonstrates how to use consumeStream in a Next.js API route (app/api/chat/route.ts) with the Vercel AI SDK streamText function. It ensures that the onFinish callback is triggered and the chat state is saved using saveChat, even if the client disconnects before the stream completes. The consumeStream() call removes backpressure, allowing the backend to process the full stream.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/04-ai-sdk-ui/03-chatbot-message-persistence.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
import { appendResponseMessages, streamText } from 'ai';
import { saveChat } from '@tools/chat-store';

export async function POST(req: Request) {
  const { messages, id } = await req.json();

  const result = streamText({
    model,
    messages,
    async onFinish({ response }) {
      await saveChat({
        id,
        messages: appendResponseMessages({
          messages,
          responseMessages: response.messages,
        }),
      });
    },
  });

  // consume the stream to ensure it runs to completion & triggers onFinish
  // even when the client response is aborted:
  result.consumeStream(); // no await

  return result.toDataStreamResponse();
}
```

----------------------------------------

TITLE: Implementing Chat UI with useChat Hook in Next.js
DESCRIPTION: This React component, designed for a Next.js client page, utilizes the `useChat` hook from `@ai-sdk/react` to manage chat state, input, and submission. It renders messages, including user and AI responses, and provides an input form for user interaction, demonstrating a complete frontend chat interface.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/18-claude-4.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
'use client';

import { useChat } from '@ai-sdk/react';

export default function Page() {
  const { messages, input, handleInputChange, handleSubmit, error } = useChat();

  return (
    <div className="flex flex-col h-screen max-w-2xl mx-auto p-4">
      <div className="flex-1 overflow-y-auto space-y-4 mb-4">
        {messages.map(message => (
          <div
            key={message.id}
            className={`p-3 rounded-lg ${
              message.role === 'user' ? 'bg-blue-50 ml-auto' : 'bg-gray-50'
            }`}
          >
            <p className="font-semibold">
              {message.role === 'user' ? 'You' : 'Claude 4'}
            </p>
            {message.parts.map((part, index) => {
              if (part.type === 'text') {
                return (
                  <div key={index} className="mt-1">
                    {part.text}
                  </div>
                );
              }
              if (part.type === 'reasoning') {
                return (
                  <pre
                    key={index}
                    className="bg-gray-100 p-2 rounded mt-2 text-xs overflow-x-auto"
                  >
                    <details>
                      <summary className="cursor-pointer">
                        View reasoning
                      </summary>
                      {part.details.map(detail =>
                        detail.type === 'text' ? detail.text : '<redacted>',
                      )}
                    </details>
                  </pre>
                );
              }
            })}
          </div>
        ))}
      </div>
      <form onSubmit={handleSubmit} className="flex gap-2">
        <input
          name="prompt"
          value={input}
          onChange={handleInputChange}
          className="flex-1 p-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
          placeholder="Ask Claude 4 something..."
        />
        <button
          type="submit"
          className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        >
          Send
        </button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: Displaying AI Tool Invocations in Svelte UI
DESCRIPTION: This snippet updates the Svelte UI component to render different parts of a chat message. It displays regular text messages as before, but for `tool-invocation` parts, it serializes and displays the `toolInvocation` object as JSON. This allows users to see when and how the AI model uses a tool.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-getting-started/04-svelte.mdx#_snippet_10

LANGUAGE: Svelte
CODE:
```
<script>
  import { Chat } from '@ai-sdk/svelte';

  const chat = new Chat();
</script>

<main>
  <ul>
    {#each chat.messages as message, messageIndex (messageIndex)}
      <li>
        <div>{message.role}</div>
        <div>
          {#each message.parts as part, partIndex (partIndex)}
            {#if part.type === 'text'}
              <div>{part.text}</div>
            {:else if part.type === 'tool-invocation'}
              <pre>{JSON.stringify(part.toolInvocation, null, 2)}</pre>
            {/if}
          {/each}
        </div>
      </li>
    {/each}
  </ul>
  <form onsubmit={chat.handleSubmit}>
    <input bind:value={chat.input} />
    <button type="submit">Send</button>
  </form>
</main>
```

----------------------------------------

TITLE: Enabling Multi-Step Tool Calls with maxSteps
DESCRIPTION: Illustrates how to enable multi-step generation by setting the `maxSteps` parameter in the `generateText` function. This allows the model to make tool calls and then generate further text based on the tool results within a single call, up to the specified maximum number of steps.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/15-tools-and-tool-calling.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { z } from 'zod';
import { generateText, tool } from 'ai';

const { text, steps } = await generateText({
  model: yourModel,
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  maxSteps: 5, // allow up to 5 steps
  prompt: 'What is the weather in San Francisco?',
});
```

----------------------------------------

TITLE: Configuring Common Settings for AI SDK generateText (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `generateText` function with common settings such as `maxTokens`, `temperature`, and `maxRetries`. These settings allow fine-grained control over the LLM's output, influencing aspects like response length, randomness, and robustness against transient errors.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/25-settings.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: yourModel,
  maxTokens: 512,
  temperature: 0.3,
  maxRetries: 5,
  prompt: 'Invent a new holiday and describe its traditions.',
});
```

----------------------------------------

TITLE: Calculating Cosine Similarity between Embeddings with AI SDK
DESCRIPTION: This snippet demonstrates how to calculate the cosine similarity between two embeddings using the `cosineSimilarity` function from the AI SDK. It first embeds two distinct phrases using `embedMany` and then computes their similarity score, which can be used to measure how related the phrases are.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/03-ai-sdk-core/30-embeddings.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
import { openai } from '@ai-sdk/openai';
import { cosineSimilarity, embedMany } from 'ai';

const { embeddings } = await embedMany({
  model: openai.embedding('text-embedding-3-small'),
  values: ['sunny day at the beach', 'rainy afternoon in the city'],
});

console.log(
  `cosine similarity: ${cosineSimilarity(embeddings[0], embeddings[1])}`,
);
```

----------------------------------------

TITLE: Displaying Weather Card in Chat UI (JSX)
DESCRIPTION: This snippet illustrates how to render a visual `WeatherCard` component within a chat interface. It is embedded within the `outputMessage.display` property of a `ChatGeneration` component, showcasing how a language model's tool output can be transformed into a rich, interactive UI element for the user.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/90-render-visual-interface-in-chat.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
<div className="py-4">\n  <WeatherCard\n    content={{\n      weather: {\n        temperature: 24,\n        condition: 'Sunny'\n      }\n    }}\n  />\n</div>
```

----------------------------------------

TITLE: Wrapping Language Model with Cache Middleware (TypeScript)
DESCRIPTION: This function wraps a given 'LanguageModelV1' instance with the 'cacheMiddleware'. It's a utility to easily apply the caching logic to any language model, ensuring that subsequent calls leverage the cache for improved performance and reduced external API calls.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/80-local-caching-middleware.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
export const cached = (model: LanguageModelV1) =>
  wrapLanguageModel({
    middleware: cacheMiddleware,
    model,
  });
```

----------------------------------------

TITLE: Handling New Slack Bot Direct Messages (TypeScript)
DESCRIPTION: This function processes incoming direct messages for the bot. It first validates that the message is not from another bot and is part of a thread. It then retrieves the conversation history, calls `generateResponse` to get the AI's reply, and finally posts the response back to the Slack thread using the Slack Web API client. It also includes status updates during processing.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/03-slackbot.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import type { GenericMessageEvent } from '@slack/web-api';
import { client, getThread } from './slack-utils';
import { generateResponse } from './ai';

export async function handleNewAssistantMessage(
  event: GenericMessageEvent,
  botUserId: string,
) {
  if (
    event.bot_id ||
    event.bot_id === botUserId ||
    event.bot_profile ||
    !event.thread_ts
  )
    return;

  const { thread_ts, channel } = event;
  const updateStatus = updateStatusUtil(channel, thread_ts);
  updateStatus('is thinking...');

  const messages = await getThread(channel, thread_ts, botUserId);
  const result = await generateResponse(messages, updateStatus);

  await client.chat.postMessage({
    channel: channel,
    thread_ts: thread_ts,
    text: result,
    unfurl_links: false,
    blocks: [
      {
        type: 'section',
        text: {
          type: 'mrkdwn',
          text: result,
        },
      },
    ],
  });

  updateStatus('');
}
```

----------------------------------------

TITLE: Implementing an Evaluator-Optimizer for Translation in TypeScript
DESCRIPTION: This snippet demonstrates an evaluator-optimizer pattern for improving text translations. It iteratively translates text, evaluates the translation quality using a larger model, and then refines the translation based on feedback until a quality threshold is met or maximum iterations are reached. It uses `@ai-sdk/openai` and `zod` for schema validation.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/06-agents.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import { generateText, generateObject } from 'ai';
import { z } from 'zod';

async function translateWithFeedback(text: string, targetLanguage: string) {
  let currentTranslation = '';
  let iterations = 0;
  const MAX_ITERATIONS = 3;

  // Initial translation
  const { text: translation } = await generateText({
    model: openai('gpt-4o-mini'), // use small model for first attempt
    system: 'You are an expert literary translator.',
    prompt: `Translate this text to ${targetLanguage}, preserving tone and cultural nuances:
    ${text}`,
  });

  currentTranslation = translation;

  // Evaluation-optimization loop
  while (iterations < MAX_ITERATIONS) {
    // Evaluate current translation
    const { object: evaluation } = await generateObject({
      model: openai('gpt-4o'), // use a larger model to evaluate
      schema: z.object({
        qualityScore: z.number().min(1).max(10),
        preservesTone: z.boolean(),
        preservesNuance: z.boolean(),
        culturallyAccurate: z.boolean(),
        specificIssues: z.array(z.string()),
        improvementSuggestions: z.array(z.string()),
      }),
      system: 'You are an expert in evaluating literary translations.',
      prompt: `Evaluate this translation:

      Original: ${text}
      Translation: ${currentTranslation}

      Consider:
      1. Overall quality
      2. Preservation of tone
      3. Preservation of nuance
      4. Cultural accuracy`,
    });

    // Check if quality meets threshold
    if (
      evaluation.qualityScore >= 8 &&
      evaluation.preservesTone &&
      evaluation.preservesNuance &&
      evaluation.culturallyAccurate
    ) {
      break;
    }

    // Generate improved translation based on feedback
    const { text: improvedTranslation } = await generateText({
      model: openai('gpt-4o'), // use a larger model
      system: 'You are an expert literary translator.',
      prompt: `Improve this translation based on the following feedback:
      ${evaluation.specificIssues.join('\n')}
      ${evaluation.improvementSuggestions.join('\n')}

      Original: ${text}
      Current Translation: ${currentTranslation}`,
    });

    currentTranslation = improvedTranslation;
    iterations++;
  }

  return {
    finalTranslation: currentTranslation,
    iterationsRequired: iterations,
  };
}
```

----------------------------------------

TITLE: Streaming Granular Loading State from Server with AI SDK RSC
DESCRIPTION: This updated server-side `generateResponse` function enhances loading feedback by introducing a separate `loadingState` streamable value. Initially, `loadingState` is set to `{ loading: true }`. After the AI text stream (`textStream`) completes and `stream.done()` is called, `loadingState.done({ loading: false })` is invoked to signal the end of the loading phase. Both the response stream and the loading state stream are returned to the client.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/06-loading-state.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use server';

import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { createStreamableValue } from 'ai/rsc';

export async function generateResponse(prompt: string) {
  const stream = createStreamableValue();
  const loadingState = createStreamableValue({ loading: true });

  (async () => {
    const { textStream } = streamText({
      model: openai('gpt-4o'),
      prompt,
    });

    for await (const text of textStream) {
      stream.update(text);
    }

    stream.done();
    loadingState.done({ loading: false });
  })();

  return { response: stream.value, loadingState: loadingState.value };
}
```

----------------------------------------

TITLE: Using System Prompts for Constraining Model Behavior in AI SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to use a `system` prompt alongside a regular `prompt` in the `generateText` function. The system prompt provides initial instructions to guide the model's overall behavior, ensuring it acts as a travel itinerary planner and responds with a list of stops. `yourModel` is the required language model, and `destination` and `lengthOfStay` are dynamic variables for the user prompt.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/03-prompts.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
const result = await generateText({
  model: yourModel,
  system:
    `You help planning travel itineraries. ` +
    `Respond to the users' request with a list ` +
    `of the best stops to make in their destination.`,
  prompt:
    `I am planning a trip to ${destination} for ${lengthOfStay} days. ` +
    `Please suggest the best tourist activities for me to do.`,
});
```

----------------------------------------

TITLE: Using Message Prompts for Chat Interfaces in AI SDK (TypeScript)
DESCRIPTION: This example shows how to use an array of `messages` to create a conversational prompt for the `streamUI` function, suitable for chat interfaces. Each message object includes a `role` (e.g., 'user', 'assistant') and `content`. This allows for multi-turn conversations and more complex prompt structures. `yourModel` is the required language model.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-foundations/03-prompts.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
const result = await streamUI({
  model: yourModel,
  messages: [
    { role: 'user', content: 'Hi!' },
    { role: 'assistant', content: 'Hello, how can I help?' },
    { role: 'user', content: 'Where can I buy the best Currywurst in Berlin?' },
  ],
});
```

----------------------------------------

TITLE: Handling Loading State and Stopping Stream with useObject (TSX)
DESCRIPTION: This enhanced client-side component demonstrates how to manage the loading state and provide a stop mechanism for object generation. It utilizes the `isLoading` and `stop` properties returned by `experimental_useObject` to disable the submit button during loading and offer a button to terminate the streaming process.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/40-stream-object.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
'use client';

import { experimental_useObject as useObject } from '@ai-sdk/react';
import { notificationSchema } from './api/use-object/schema';

export default function Page() {
  const { object, submit, isLoading, stop } = useObject({
    api: '/api/use-object',
    schema: notificationSchema,
  });

  return (
    <div>
      <button
        onClick={() => submit('Messages during finals week.')}
        disabled={isLoading}
      >
        Generate notifications
      </button>

      {isLoading && (
        <div>
          <div>Loading...</div>
          <button type="button" onClick={() => stop()}>
            Stop
          </button>
        </div>
      )}

      {object?.notifications?.map((notification, index) => (
        <div key={index}>
          <p>{notification?.name}</p>
          <p>{notification?.message}</p>
        </div>
      ))}
    </div>
  );
}
```

----------------------------------------

TITLE: Generating Text with AI SDK and OpenAI (Image from URL) - TypeScript
DESCRIPTION: This snippet demonstrates how to generate text using the Vercel AI SDK and OpenAI's `gpt-4-turbo` model by providing an image via a URL. It sends a user message containing both a text query and an image URL, allowing the model to interpret visual content. The `generateText` function takes the model, `maxTokens`, and a `messages` array as parameters, with the image content specified as `type: 'image'` and `image: new URL(...)`.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/05-node/12-generate-text-with-image-prompt.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const result = await generateText({
  model: openai('gpt-4-turbo'),
  maxTokens: 512,
  messages: [
    {
      role: 'user',
      content: [
        {
          type: 'text',
          text: 'what are the red things in this image?',
        },
        {
          type: 'image',
          image: new URL(
            'https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/2024_Solar_Eclipse_Prominences.jpg/720px-2024_Solar_Eclipse_Prominences.jpg',
          ),
        },
      ],
    },
  ],
});

console.log(result);
```

----------------------------------------

TITLE: Generating Structured Text Output with Regex (TypeScript)
DESCRIPTION: Shows how to constrain the output format of a FriendliAI language model using a regular expression. This ensures the generated text conforms to a specified pattern, useful for specific data formats or restricting character sets.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/08-friendliai.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { friendli } from '@friendliai/ai-provider';
import { generateText } from 'ai';

const { text } = await generateText({
  model: friendli('meta-llama-3.1-8b-instruct', {
    regex: new RegExp('[\\n ,.?!0-9\\uac00-\\ud7af]*'),
  }),
  prompt: 'Who is the first king of the Joseon Dynasty?',
});

console.log(text);
```

----------------------------------------

TITLE: Rendering AI SDK Messages with New Parts Array in React - JavaScript
DESCRIPTION: This React component demonstrates how to render AI SDK messages using the new `message.parts` array with the `useChat` hook. It iterates through each message and then through its `parts` array, conditionally rendering different UI elements based on the `part.type` (e.g., text, source, reasoning, tool invocation, file). This approach simplifies handling complex, multi-modal AI responses in the UI.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/08-migration-guides/27-migration-guide-4-2.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
function Chat() {
  const { messages } = useChat();
  return (
    <div>
      {messages.map(message =>
        message.parts.map((part, i) => {
          switch (part.type) {
            case 'text':
              return <p key={i}>{part.text}</p>;
            case 'source':
              return <p key={i}>{part.source.url}</p>;
            case 'reasoning':
              return <div key={i}>{part.reasoning}</div>;
            case 'tool-invocation':
              return <div key={i}>{part.toolInvocation.toolName}</div>;
            case 'file':
              return (
                <img
                  key={i}
                  src={`data:${part.mimeType};base64,${part.data}`}
                />
              );
          }
        })
      )}
    </div>
  );
}
```

----------------------------------------

TITLE: Handling Multiple Streamable UIs with AI SDK RSC
DESCRIPTION: This function demonstrates how to create and manage multiple independent streamable UI components using `createStreamableUI` from `ai/rsc`. It updates and completes `weatherUI` and `forecastUI` asynchronously, returning their values along with other data in a single response. This allows for decoupled and independently updating UI sections on the client side.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/06-advanced/05-multiple-streamables.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use server';

import { createStreamableUI } from 'ai/rsc';

export async function getWeather() {
  const weatherUI = createStreamableUI();
  const forecastUI = createStreamableUI();

  weatherUI.update(<div>Loading weather...</div>);
  forecastUI.update(<div>Loading forecast...</div>);

  getWeatherData().then(weatherData => {
    weatherUI.done(<div>{weatherData}</div>);
  });

  getForecastData().then(forecastData => {
    forecastUI.done(<div>{forecastData}</div>);
  });

  // Return both streamable UIs and other data fields.
  return {
    requestedAt: Date.now(),
    weather: weatherUI.value,
    forecast: forecastUI.value,
  };
}
```

----------------------------------------

TITLE: Setting OpenAI Image Input Detail (TypeScript)
DESCRIPTION: This snippet illustrates how to control the detail level for image inputs when using OpenAI's vision models. By setting `providerOptions.openai.imageDetail` to `low`, `high`, or `auto` within an image message object, you can specify how the model processes the image, impacting both quality and cost.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/01-ai-sdk-providers/03-openai.mdx#_snippet_15

LANGUAGE: ts
CODE:
```
const result = await generateText({
  model: openai('gpt-4o'),
  messages: [
    {
      role: 'user',
      content: [
        { type: 'text', text: 'Describe the image in detail.' },
        {
          type: 'image',
          image:
            'https://github.com/vercel/ai/blob/main/examples/ai-core/data/comic-cat.png?raw=true',

          // OpenAI specific options - image detail:
          providerOptions: {
            openai: { imageDetail: 'low' },
          },
        },
      ],
    },
  ],
});
```

----------------------------------------

TITLE: Handling Streamed Text and Loading State in Next.js Client (AI SDK)
DESCRIPTION: This client-side React component demonstrates how to consume streamed text responses and a separate loading state from a server action. It uses `readStreamableValue` to iteratively update the UI with incoming text deltas and to manage a loading indicator, providing detailed feedback to the user during the AI generation process. The `maxDuration` export ensures the page remains dynamic for streaming.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/06-loading-state.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { generateResponse } from './actions';
import { readStreamableValue } from 'ai/rsc';

// Force the page to be dynamic and allow streaming responses up to 30 seconds
export const maxDuration = 30;

export default function Home() {
  const [input, setInput] = useState<string>('');
  const [generation, setGeneration] = useState<string>('');
  const [loading, setLoading] = useState<boolean>(false);

  return (
    <div>
      <div>{generation}</div>
      <form
        onSubmit={async e => {
          e.preventDefault();
          setLoading(true);
          const { response, loadingState } = await generateResponse(input);

          let textContent = '';

          for await (const responseDelta of readStreamableValue(response)) {
            textContent = `${textContent}${responseDelta}`;
            setGeneration(textContent);
          }
          for await (const loadingDelta of readStreamableValue(loadingState)) {
            if (loadingDelta) {
              setLoading(loadingDelta.loading);
            }
          }
          setInput('');
          setLoading(false);
        }}
      >
        <input
          type="text"
          value={input}
          disabled={loading}
          className="disabled:opacity-50"
          onChange={event => {
            setInput(event.target.value);
          }}
        />
        <button>Send Message</button>
      </form>
    </div>
  );
}
```

----------------------------------------

TITLE: AI SDK Configuration for Conversation State Management
DESCRIPTION: This file (`app/ai.ts`) configures the AI SDK's state management. It uses `createAI` to define the structure of server and client messages, linking the `continueConversation` server action to the AI state. It initializes both the AI state and UI state as empty arrays, setting up the foundation for the conversational application.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/92-stream-ui-record-token-usage.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import { createAI } from 'ai/rsc';
import { ServerMessage, ClientMessage, continueConversation } from './actions';

export const AI = createAI<ServerMessage[], ClientMessage[]> ({
  actions: {
    continueConversation,
  },
  initialAIState: [],
  initialUIState: [],
});
```

----------------------------------------

TITLE: Creating AI Context for State Management in TypeScript
DESCRIPTION: This snippet initializes the AI context using `createAI` from `ai/rsc`. It defines the initial states for both UI and AI, and registers the `submitUserMessage` server action. This context is crucial for managing the conversational state across the application, enabling the AI to maintain continuity and interact with the UI.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/04-multistep-interfaces.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { createAI } from 'ai/rsc';
import { submitUserMessage } from './actions';

export const AI = createAI<any[], React.ReactNode[]>(
  {
    initialUIState: [],
    initialAIState: [],
    actions: {
      submitUserMessage,
    },
  },
);

```

----------------------------------------

TITLE: Generating and Streaming Text in React Server Action (app/actions.ts)
DESCRIPTION: This server-side `generate` function uses the `@ai-sdk/openai` model to produce text based on an input prompt. It employs `createStreamableValue` from `ai/rsc` to wrap the `textStream` and send incremental updates to the client, enabling real-time display of the generated content.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/20-rsc/20-stream-text.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
'use server';

import { streamText } from 'ai';
import { openai } from '@ai-sdk/openai';
import { createStreamableValue } from 'ai/rsc';

export async function generate(input: string) {
  const stream = createStreamableValue('');

  (async () => {
    const { textStream } = streamText({
      model: openai('gpt-3.5-turbo'),
      prompt: input,
    });

    for await (const delta of textStream) {
      stream.update(delta);
    }

    stream.done();
  })();

  return { output: stream.value };
}
```

----------------------------------------

TITLE: Installing React Types for AI SDK in TypeScript
DESCRIPTION: This snippet provides the command to install the `@types/react` package, which is a required dependency for the AI SDK to resolve the "Cannot find namespace 'JSX'" TypeScript error, especially when the SDK is used in non-React environments like an Hono server. Installing this package makes the `JSX` namespace available to TypeScript.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/09-troubleshooting/40-typescript-cannot-find-namespace-jsx.mdx#_snippet_0

LANGUAGE: Bash
CODE:
```
npm install @types/react
```

----------------------------------------

TITLE: Rendering Streamable UI with React and AI SDK (Client-side)
DESCRIPTION: This client-side React component demonstrates how to call a server action (`getWeather`) and render its streamable UI output. It uses `useState` to manage the UI content and imports `readStreamableValue` (though not directly invoked in this snippet, it's part of the `ai/rsc` context). The UI updates dynamically when the button is clicked, showing a loading message followed by the actual weather information.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/05-ai-sdk-rsc/05-streaming-values.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
'use client';

import { useState } from 'react';
import { readStreamableValue } from 'ai/rsc';
import { getWeather } from '@/actions';

export default function Page() {
  const [weather, setWeather] = useState<React.ReactNode | null>(null);

  return (
    <div>
      <button
        onClick={async () => {
          const weatherUI = await getWeather();
          setWeather(weatherUI);
        }}
      >
        What&apos;s the weather?
      </button>

      {weather}
    </div>
  );
}
```

----------------------------------------

TITLE: Extracting Reasoning with FriendliAI Model (TypeScript)
DESCRIPTION: Illustrates how to use `extractReasoningMiddleware` with a FriendliAI model to parse and expose reasoning embedded within the generated text, typically marked by specific tags like `<think>`. This enhances the model's output by separating the core response from its internal thought process.
SOURCE: https://github.com/vercel/ai/blob/main/content/providers/03-community-providers/08-friendliai.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { friendli } from '@friendliai/ai-provider';
import { wrapLanguageModel, extractReasoningMiddleware } from 'ai';

const enhancedModel = wrapLanguageModel({
  model: friendli('deepseek-r1'),
  middleware: extractReasoningMiddleware({ tagName: 'think' }),
});

const { text, reasoning } = await generateText({
  model: enhancedModel,
  prompt: 'Explain quantum entanglement.',
});
```

----------------------------------------

TITLE: Using Tools with AI SDK Core and OpenAI (TSX)
DESCRIPTION: Demonstrates how to define and use a tool with the AI SDK's `generateText` function. It shows how to pass a tool object with a description, parameters (defined using Zod), and an asynchronous execute function to the model.
SOURCE: https://github.com/vercel/ai/blob/main/content/docs/02-guides/23-o1.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { generateText, tool } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai('o1'),
  prompt: 'What is the weather like today?',
  tools: {
    getWeather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
});
```

----------------------------------------

TITLE: Handling Tool Confirmation Response in AI API Route (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how an API route handler processes tool invocation results, specifically focusing on confirmation states. It checks if a 'tool-invocation' part is for 'getWeatherInformation' and in a 'result' state. Based on the confirmation ('Yes, confirmed.' or 'No, denied.'), it either executes the tool via 'executeWeatherTool' or sets an error message, then updates the tool result and forwards it to the client. The updated messages are then streamed to the language model.
SOURCE: https://github.com/vercel/ai/blob/main/content/cookbook/01-next/75-human-in-the-loop.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai';
import {
  createDataStreamResponse,
  formatDataStreamPart,
  Message,
  streamText,
  tool,
} from 'ai';
import { z } from 'zod';

export async function POST(req: Request) {
  const { messages }: { messages: Message[] } = await req.json();

  return createDataStreamResponse({
    execute: async dataStream => {
      // pull out last message
      const lastMessage = messages[messages.length - 1];

      lastMessage.parts = await Promise.all(
        // map through all message parts
        lastMessage.parts?.map(async part => {
          if (part.type !== 'tool-invocation') {
            return part;
          }
          const toolInvocation = part.toolInvocation;
          // return if tool isn't weather tool or in a result state
          if (
            toolInvocation.toolName !== 'getWeatherInformation' ||
            toolInvocation.state !== 'result'
          ) {
            return part;
          }

          // switch through tool result states (set on the frontend)
          switch (toolInvocation.result) {
            case 'Yes, confirmed.': {
              const result = await executeWeatherTool(toolInvocation.args);

              // forward updated tool result to the client:
              dataStream.write(
                formatDataStreamPart('tool_result', {
                  toolCallId: toolInvocation.toolCallId,
                  result,
                }),
              );

              // update the message part:
              return { ...part, toolInvocation: { ...toolInvocation, result } };
            }
            case 'No, denied.': {
              const result = 'Error: User denied access to weather information';

              // forward updated tool result to the client:
              dataStream.write(
                formatDataStreamPart('tool_result', {
                  toolCallId: toolInvocation.toolCallId,
                  result,
                }),
              );

              // update the message part:
              return { ...part, toolInvocation: { ...toolInvocation, result } };
            }
            default:
              return part;
          }
        }) ?? [],
      );

      const result = streamText({
        model: openai('gpt-4o'),
        messages,
        tools: {
          getWeatherInformation: tool({
            description: 'show the weather in a given city to the user',
            parameters: z.object({ city: z.string() }),
          }),
        },
      });

      result.mergeIntoDataStream(dataStream);
    },
  });
}

async function executeWeatherTool({}: { city: string }) {
  const weatherOptions = ['sunny', 'cloudy', 'rainy', 'snowy'];
  return weatherOptions[Math.floor(Math.random() * weatherOptions.length)];
}
```