TITLE: Installing LM Studio SDK using Package Managers
DESCRIPTION: Commands for installing the @lmstudio/sdk package using different Node.js package managers (npm, yarn, and pnpm).
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/index.md#2025-04-22_snippet_0

LANGUAGE: bash
CODE:
```
npm install @lmstudio/sdk --save
```

LANGUAGE: bash
CODE:
```
yarn add @lmstudio/sdk
```

LANGUAGE: bash
CODE:
```
pnpm add @lmstudio/sdk
```

----------------------------------------

TITLE: Start LM Studio Server (Bash)
DESCRIPTION: Command to start the LM Studio server via the `lms` command-line interface, enabling programmatic interaction through the OpenAI-like REST API.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/tools.md#_snippet_0

LANGUAGE: bash
CODE:
```
lms server start
```

----------------------------------------

TITLE: Loading Models with LMS CLI in Bash
DESCRIPTION: This command uses the LMS CLI to load a specified model. It requires two parameters: the model name and the path to the model file. This allows users to dynamically load models into the LMStudio environment.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/3_cli/_lms-load.md#2025-04-22_snippet_0

LANGUAGE: bash
CODE:
```
lms load --model <model-name> --path <path-to-model>
```

----------------------------------------

TITLE: Configure OpenAI Client Base URL in Typescript
DESCRIPTION: Shows how to update a Typescript OpenAI client instance to connect to the LM Studio API by setting the `baseUrl` property in the client constructor options. This enables using existing Typescript OpenAI code with LM Studio.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/1_endpoints/openai.md#_snippet_1

LANGUAGE: Typescript
CODE:
```
import OpenAI from 'openai';

const client = new OpenAI({
+  baseUrl: "http://localhost:1234/v1"
});

// ... the rest of your code ...
```

----------------------------------------

TITLE: Basic Model Download Command
DESCRIPTION: Demonstrates how to download a model by specifying its name using the lms get command
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/3_cli/get.md#2025-04-22_snippet_1

LANGUAGE: shell
CODE:
```
lms get llama-3.1-8b
```

----------------------------------------

TITLE: Instantiating an LLM Model with LMStudio SDK in TypeScript
DESCRIPTION: This code demonstrates how to import the LMStudioClient and load a specific language model (qwen2.5-7b-instruct) for text completion tasks.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/completion.md#2025-04-22_snippet_0

LANGUAGE: typescript
CODE:
```
import { LMStudioClient } from "@lmstudio/sdk";

const client = new LMStudioClient();
const model = await client.llm.model("qwen2.5-7b-instruct");
```

----------------------------------------

TITLE: Downloading Models with TypeScript SDK
DESCRIPTION: Demonstrates how to search for models, select download options based on quantization, and download a model using the LM Studio TypeScript SDK. The example searches for 'llama 3.2 1b' models, filters for GGUF compatibility, and selects the Q4_K_M quantization option.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/5_manage-models/_download-models.md#2025-04-22_snippet_0

LANGUAGE: typescript
CODE:
```
import { LMStudioClient } from "@lmstudio/sdk";

const client = new LMStudioClient();

// 1. Search for the model you want
// Specify any/all of searchTerm, limit, compatibilityTypes
const searchResults = await client.repository.searchModels({
  searchTerm: "llama 3.2 1b",    // Search for Llama 3.2 1B
  limit: 5,                      // Get top 5 results
  compatibilityTypes: ["gguf"],  // Only download GGUFs
});

// 2. Find download options
const bestResult = searchResults[0];
const downloadOptions = await bestResult.getDownloadOptions();

// Let's download Q4_K_M, a good middle ground quantization
const desiredModel = downloadOptions.find(option => option.quantization === 'Q4_K_M');

// 3. Download it!
const modelKey = await desiredModel.download();

// This returns a path you can use to load the model
const loadedModel = await client.llm.model(modelKey);
```

----------------------------------------

TITLE: Chat with Llama Model using Scoped Resource API
DESCRIPTION: Example showing how to load a Llama 3.2 1B model and query it using the scoped resource API. This approach uses context managers to ensure deterministic resource cleanup.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/index.md#2025-04-22_snippet_2

LANGUAGE: python
CODE:
```
import lmstudio as lms

with lms.Client() as client:
    model = client.llm.model("llama-3.2-1b-instruct")
    result = model.respond("What is the meaning of life?")

    print(result)
```

----------------------------------------

TITLE: Installing lmstudio Python SDK with pip
DESCRIPTION: Command to install the lmstudio Python SDK package using pip package manager.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/index.md#2025-04-22_snippet_0

LANGUAGE: bash
CODE:
```
pip install lmstudio
```

----------------------------------------

TITLE: Streaming Tool-Enhanced Chatbot Example (Python)
DESCRIPTION: A complete Python script demonstrating how to build a simple command-line chatbot that interacts with the LM Studio `/v1/chat/completions` streaming endpoint. It includes logic to process streamed text and accumulate fragmented tool call information to execute a predefined tool (`get_current_time`).
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/tools.md#_snippet_24

LANGUAGE: python
CODE:
```
from openai import OpenAI
import time

client = OpenAI(base_url="http://127.0.0.1:1234/v1", api_key="lm-studio")
MODEL = "lmstudio-community/qwen2.5-7b-instruct"

TIME_TOOL = {
    "type": "function",
    "function": {
        "name": "get_current_time",
        "description": "Get the current time, only if asked",
        "parameters": {"type": "object", "properties": {}},
    },
}

def get_current_time():
    return {"time": time.strftime("%H:%M:%S")}

def process_stream(stream, add_assistant_label=True):
    """Handle streaming responses from the API"""
    collected_text = ""
    tool_calls = []
    first_chunk = True

    for chunk in stream:
        delta = chunk.choices[0].delta

        # Handle regular text output
        if delta.content:
            if first_chunk:
                print()
                if add_assistant_label:
                    print("Assistant:", end=" ", flush=True)
                first_chunk = False
            print(delta.content, end="", flush=True)
            collected_text += delta.content

        # Handle tool calls
        elif delta.tool_calls:
            for tc in delta.tool_calls:
                if len(tool_calls) <= tc.index:
                    tool_calls.append({
                        "id": "", "type": "function",
                        "function": {"name": "", "arguments": ""}
                    })
                tool_calls[tc.index] = {
                    "id": (tool_calls[tc.index]["id"] + (tc.id or "")),
                    "type": "function",
                    "function": {
                        "name": (tool_calls[tc.index]["function"]["name"] + (tc.function.name or "")),
                        "arguments": (tool_calls[tc.index]["function"]["arguments"] + (tc.function.arguments or ""))
                    }
                }
    return collected_text, tool_calls

def chat_loop():
    messages = []
    print("Assistant: Hi! I am an AI agent empowered with the ability to tell the current time (Type 'quit' to exit)")

    while True:
        user_input = input("\nYou: ").strip()
        if user_input.lower() == "quit":
            break

        messages.append({"role": "user", "content": user_input})

        # Get initial response
        response_text, tool_calls = process_stream(
            client.chat.completions.create(
                model=MODEL,
                messages=messages,
                tools=[TIME_TOOL],
                stream=True,
                temperature=0.2
            )
        )

        if not tool_calls:
            print()

        text_in_first_response = len(response_text) > 0
        if text_in_first_response:
            messages.append({"role": "assistant", "content": response_text})

        # Handle tool calls if any
        if tool_calls:
            tool_name = tool_calls[0]["function"]["name"]
            print()
            if not text_in_first_response:
                print("Assistant:", end=" ", flush=True)
            print(f"**Calling Tool: {tool_name}**")
            messages.append({"role": "assistant", "tool_calls": tool_calls})

            # Execute tool calls
            for tool_call in tool_calls:
                if tool_call["function"]["name"] == "get_current_time":
                    result = get_current_time()
                    messages.append({
                        "role": "tool",
                        "content": str(result),

```

----------------------------------------

TITLE: Implementing Multi-turn Chatbot in Python
DESCRIPTION: Complete example showing how to implement an interactive multi-turn chatbot with streaming responses.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/chat-completion.md#2025-04-22_snippet_7

LANGUAGE: python
CODE:
```
import lmstudio as lms

model = lms.llm()
chat = lms.Chat("You are a task focused AI assistant")

while True:
    try:
        user_input = input("You (leave blank to exit): ")
    except EOFError:
        print()
        break
    if not user_input:
        break
    chat.add_user_message(user_input)
    prediction_stream = model.respond_stream(
        chat,
        on_message=chat.append,
    )
    print("Bot: ", end="", flush=True)
    for fragment in prediction_stream:
        print(fragment.content, end="", flush=True)
    print()
```

----------------------------------------

TITLE: Chat with Llama Model using Convenience API
DESCRIPTION: Example demonstrating how to load a Llama 3.2 1B model and query it using the convenience API. This approach uses a default client instance for interactive use in Python prompts or Jupyter notebooks.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/index.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
import lmstudio as lms

model = lms.llm("llama-3.2-1b-instruct")
result = model.respond("What is the meaning of life?")

print(result)
```

----------------------------------------

TITLE: Interactive Chat Loop with File Creation Tool
DESCRIPTION: Implements a conversation loop with an LLM agent that can create files on the local machine. This example shows how to maintain chat context while allowing the LLM to create files based on user requests, with real-time response streaming.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/3_agent/act.md#2025-04-22_snippet_2

LANGUAGE: typescript
CODE:
```
import { Chat, LMStudioClient, tool } from "@lmstudio/sdk";
import { existsSync } from "fs";
import { writeFile } from "fs/promises";
import { createInterface } from "readline/promises";
import { z } from "zod";

const rl = createInterface({ input: process.stdin, output: process.stdout });
const client = new LMStudioClient();
const model = await client.llm.model();
const chat = Chat.empty();

const createFileTool = tool({
  name: "createFile",
  description: "Create a file with the given name and content.",
  parameters: { name: z.string(), content: z.string() },
  implementation: async ({ name, content }) => {
    if (existsSync(name)) {
      return "Error: File already exists.";
    }
    await writeFile(name, content, "utf-8");
    return "File created.";
  },
});

while (true) {
  const input = await rl.question("You: ");
  // Append the user input to the chat
  chat.append("user", input);

  process.stdout.write("Bot: ");
  await model.act(chat, [createFileTool], {
    // When the model finish the entire message, push it to the chat
    onMessage: (message) => chat.append(message),
    onPredictionFragment: ({ content }) => {
      process.stdout.write(content);
    },
  });
  process.stdout.write("\n");
}
```

----------------------------------------

TITLE: Configure OpenAI Client Base URL in Python
DESCRIPTION: Demonstrates how to modify an existing Python OpenAI client configuration to point to the LM Studio server by changing the `base_url` parameter during client initialization. This allows reusing existing OpenAI code with LM Studio.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/1_endpoints/openai.md#_snippet_0

LANGUAGE: Python
CODE:
```
from openai import OpenAI

client = OpenAI(
+    base_url="http://localhost:1234/v1"
)

# ... the rest of your code ...
```

----------------------------------------

TITLE: Streaming Chat Responses in Python
DESCRIPTION: Shows how to stream AI responses in real-time using both API styles. Streams text fragments as they are generated rather than waiting for complete response.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/chat-completion.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
import lmstudio as lms
model = lms.llm()

for fragment in model.respond_stream("What is the meaning of life?"):
    print(fragment.content, end="", flush=True)
print()
```

LANGUAGE: python
CODE:
```
import lmstudio as lms
with lms.Client() as client:
    model = client.llm.model()

    for fragment in model.respond_stream("What is the meaning of life?"):
        print(fragment.content, end="", flush=True)
    print()
```

----------------------------------------

TITLE: Defining Tools in Python
DESCRIPTION: These examples demonstrate three ways to define Python functions as tools compatible with the LM Studio SDK's `act()` method. The first uses a standard type-hinted function, the second wraps a function with `ToolFunctionDef.from_callable` to specify name/description, and the third uses `ToolFunctionDef` directly to define parameters and link an implementation.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/2_agent/tools.md#_snippet_0

LANGUAGE: python
CODE:
```
# Type hinted functions with clear names and docstrings
# may be used directly as tool definitions
def add(a: int, b: int) -> int:
    """Given two numbers a and b, returns the sum of them."""
    # The SDK ensures arguments are coerced to their specified types
    return a + b

# Pass `add` directly to `act()` as a tool definition
```

LANGUAGE: python
CODE:
```
from lmstudio import ToolFunctionDef

def cryptic_name(a: int, b: int) -> int:
    return a + b

# Type hinted functions with cryptic names and missing or poor docstrings
# can be turned into clear tool definitions with `from_callable`
tool_def = ToolFunctionDef.from_callable(
  cryptic_name,
  name="add",
  description="Given two numbers a and b, returns the sum of them."
)
# Pass `tool_def` to `act()` as a tool definition
```

LANGUAGE: python
CODE:
```
from lmstudio import ToolFunctionDef

def cryptic_name(a, b):
    return a + b

# Functions without type hints can be used without wrapping them
# at runtime by defining a tool function directly.
tool_def = ToolFunctionDef(
  name="add",
  description="Given two numbers a and b, returns the sum of them.",
  parameters={
    "a": int,
    "b": int,
  },
  implementation=cryptic_name,
)
# Pass `tool_def` to `act()` as a tool definition
```

----------------------------------------

TITLE: POST /api/v0/completions Example Request (curl)
DESCRIPTION: Example curl command to send a POST request to the /api/v0/completions endpoint for text completion. It includes parameters like model, prompt, temperature, max_tokens, stream, and stop.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/1_endpoints/rest.md#_snippet_7

LANGUAGE: bash
CODE:
```
curl http://localhost:1234/api/v0/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "granite-3.0-2b-instruct",
    "prompt": "the meaning of life is",
    "temperature": 0.7,
    "max_tokens": 10,
    "stream": false,
    "stop": "\n"
  }'
```

----------------------------------------

TITLE: Loading Specific Model in Python with LM Studio SDK
DESCRIPTION: Shows how to load or access a specific model by providing its key. This method will load the model if it's not already loaded, or return the existing instance if it is.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/5_manage-models/loading.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
import lmstudio as lms

model = lms.llm("llama-3.2-1b-instruct")
```

LANGUAGE: python
CODE:
```
import lmstudio as lms

with lms.Client() as client:
    model = client.llm.model("llama-3.2-1b-instruct")
```

----------------------------------------

TITLE: Generating Quick Chat Response with LMStudio
DESCRIPTION: Demonstrates how to initialize an LMStudio client and stream a response to a simple chat prompt using the respond() method.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/chat-completion.md#2025-04-22_snippet_0

LANGUAGE: typescript
CODE:
```
import { LMStudioClient } from "@lmstudio/sdk";
const client = new LMStudioClient();

const model = await client.llm.model();

for await (const fragment of model.respond("What is the meaning of life?")) {
  process.stdout.write(fragment.content);
}
```

----------------------------------------

TITLE: Generating Non-Streaming Structured Response with Zod in TypeScript
DESCRIPTION: Demonstrates how to use a Zod schema with the model.respond() method to enforce a structured response about a book. The result contains a parsed field with the properly typed data.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/structured-response.md#2025-04-22_snippet_1

LANGUAGE: typescript
CODE:
```
const result = await model.respond("Tell me about The Hobbit.",
  { structured: bookSchema },
  maxTokens: 100, // Recommended to avoid getting stuck
);

const book = result.parsed;
console.info(book);
//           ^
// Note that `book` is now correctly typed as { title: string, author: string, year: number }
```

----------------------------------------

TITLE: Create Chat Completion in Python
DESCRIPTION: Shows a Python example using the OpenAI client configured for LM Studio to send a chat history to the `/v1/chat/completions` endpoint and retrieve the model's response. It includes setting the base URL and API key, specifying the model, messages, and temperature.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/1_endpoints/openai.md#_snippet_4

LANGUAGE: Python
CODE:
```
# Example: reuse your existing OpenAI setup
from openai import OpenAI

# Point to the local server
client = OpenAI(base_url="http://localhost:1234/v1", api_key="lm-studio")

completion = client.chat.completions.create(
  model="model-identifier",
  messages=[
    {"role": "system", "content": "Always answer in rhymes."},
    {"role": "user", "content": "Introduce yourself."}
  ],
  temperature=0.7,
)

print(completion.choices[0].message)
```

----------------------------------------

TITLE: Process Tool Calls in Python
DESCRIPTION: This function takes an AI model's response containing tool calls and the current message history. It constructs an assistant message with the tool calls, appends it to the history, executes the functions corresponding to the tool calls (handling arguments), collects the results, appends the tool result messages to the history, and finally gets a new completion from the model based on the updated history.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/tools.md#_snippet_21

LANGUAGE: python
CODE:
```
# Create the assistant message with tool calls
assistant_tool_call_message = {
    "role": "assistant",
    "tool_calls": [
        {
            "id": tool_call.id,
            "type": tool_call.type,
            "function": tool_call.function,
        }
        for tool_call in tool_calls
    ],
}

# Add the assistant's tool call message to the history
messages.append(assistant_tool_call_message)

# Process each tool call and collect results
tool_results = []
for tool_call in tool_calls:
    # For functions with no arguments, use empty dict
    arguments = (
        json.loads(tool_call.function.arguments)
        if tool_call.function.arguments.strip()
        else {}
    )

    # Determine which function to call based on the tool call name
    if tool_call.function.name == "open_safe_url":
        result = open_safe_url(arguments["url"])
    elif tool_call.function.name == "get_current_time":
        result = get_current_time()
    elif tool_call.function.name == "analyze_directory":
        path = arguments.get("path", ".")
        result = analyze_directory(path)
    else:
        # llm tried to call a function that doesn't exist, skip
        continue

    # Add the result message
    tool_result_message = {
        "role": "tool",
        "content": json.dumps(result),
        "tool_call_id": tool_call.id,
    }
    tool_results.append(tool_result_message)
    messages.append(tool_result_message)

# Get the final response
final_response = client.chat.completions.create(
    model=model,
    messages=messages,
)

return final_response
```

----------------------------------------

TITLE: Multiple Tools Integration with Addition and Prime Number Checking
DESCRIPTION: Shows how to provide multiple tools in a single `.act()` call. This example implements an addition tool and a prime number checking tool, allowing the LLM to solve a complex mathematical problem by combining both tools.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/3_agent/act.md#2025-04-22_snippet_1

LANGUAGE: typescript
CODE:
```
import { LMStudioClient, tool } from "@lmstudio/sdk";
import { z } from "zod";

const client = new LMStudioClient();

const additionTool = tool({
  name: "add",
  description: "Given two numbers a and b. Returns the sum of them.",
  parameters: { a: z.number(), b: z.number() },
  implementation: ({ a, b }) => a + b,
});

const isPrimeTool = tool({
  name: "isPrime",
  description: "Given a number n. Returns true if n is a prime number.",
  parameters: { n: z.number() },
  implementation: ({ n }) => {
    if (n < 2) return false;
    const sqrt = Math.sqrt(n);
    for (let i = 2; i <= sqrt; i++) {
      if (n % i === 0) return false;
    }
    return true;
  },
});

const model = await client.llm.model("qwen2.5-7b-instruct");
await model.act(
  "Is the result of 12345 + 45668 a prime? Think step by step.",
  [additionTool, isPrimeTool],
  { onMessage: (message) => console.info(message.toString()) },
);
```

----------------------------------------

TITLE: Checking Chat Fit in Context Window in TypeScript
DESCRIPTION: This example shows how to determine if a given conversation fits into a model's context window by converting the chat to a string, counting tokens, and comparing to the model's context length.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/6_model-info/get-context-length.md#2025-04-22_snippet_1

LANGUAGE: typescript
CODE:
```
import { Chat, type LLM, LMStudioClient } from "@lmstudio/sdk";

async function doesChatFitInContext(model: LLM, chat: Chat) {
  // Convert the conversation to a string using the prompt template.
  const formatted = await model.applyPromptTemplate(chat);
  // Count the number of tokens in the string.
  const tokenCount = await model.countTokens(formatted);
  // Get the current loaded context length of the model
  const contextLength = await model.getContextLength();
  return tokenCount < contextLength;
}

const client = new LMStudioClient();
const model = await client.llm.model();

const chat = Chat.from([
  { role: "user", content: "What is the meaning of life." },
  { role: "assistant", content: "The meaning of life is..." },
  // ... More messages
]);

console.info("Fits in context:", await doesChatFitInContext(model, chat));
```

----------------------------------------

TITLE: Generating Streaming Text Completions with LMStudio SDK
DESCRIPTION: This snippet shows how to generate text completions in streaming mode, where tokens are processed and displayed as they are generated, with a maximum token limit of 100.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/completion.md#2025-04-22_snippet_1

LANGUAGE: typescript
CODE:
```
const completion = model.complete("My name is", {
  maxTokens: 100,
});

for await (const { content } of completion) {
  process.stdout.write(content);
}

console.info(); // Write a new line for cosmetic purposes
```

----------------------------------------

TITLE: Get Text Embedding in Python
DESCRIPTION: Provides a Python function using the OpenAI client configured for LM Studio to send text to the `/v1/embeddings` endpoint and retrieve its vector representation. It demonstrates initializing the client, defining a function to get embeddings, and handling newline characters in the input text.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/1_endpoints/openai.md#_snippet_5

LANGUAGE: Python
CODE:
```
# Make sure to \`pip install openai\` first
from openai import OpenAI
client = OpenAI(base_url="http://localhost:1234/v1", api_key="lm-studio")

def get_embedding(text, model="model-identifier"):
   text = text.replace("\n", " ")
   return client.embeddings.create(input = [text], model=model).data[0].embedding

print(get_embedding("Once upon a time, there was a cat."))
```

----------------------------------------

TITLE: Generating a Prediction with Image Input in TypeScript
DESCRIPTION: This code shows how to generate a prediction by passing an image to a Vision-Language Model using the `.respond()` method.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/image-input.md#2025-04-22_snippet_4

LANGUAGE: typescript
CODE:
```
const prediction = model.respond([
  { role: "user", content: "Describe this image please", images: [image] },
]);
```

----------------------------------------

TITLE: Configuring Inference Parameters in Python
DESCRIPTION: Demonstrates how to customize inference parameters like temperature and token limits for both streaming and non-streaming responses.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/chat-completion.md#2025-04-22_snippet_5

LANGUAGE: python
CODE:
```
prediction_stream = model.respond_stream(chat, config={
    "temperature": 0.6,
    "maxTokens": 50,
})
```

LANGUAGE: python
CODE:
```
result = model.respond(chat, config={
    "temperature": 0.6,
    "maxTokens": 50,
})
```

----------------------------------------

TITLE: Basic Multiplication Tool Implementation with LMStudio .act() API
DESCRIPTION: Demonstrates a simple example of using the `.act()` API with a multiplication tool. The code creates a tool that multiplies two numbers and lets the LLM autonomously decide when to use it to solve a mathematical problem.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/3_agent/act.md#2025-04-22_snippet_0

LANGUAGE: typescript
CODE:
```
import { LMStudioClient, tool } from "@lmstudio/sdk";
import { z } from "zod";

const client = new LMStudioClient();

const multiplyTool = tool({
  name: "multiply",
  description: "Given two numbers a and b. Returns the product of them.",
  parameters: { a: z.number(), b: z.number() },
  implementation: ({ a, b }) => a * b,
});

const model = await client.llm.model("qwen2.5-7b-instruct");
await model.act("What is the result of 12345 multiplied by 54321?", [multiplyTool], {
  onMessage: (message) => console.info(message.toString()),
});
```

----------------------------------------

TITLE: Installing lms on macOS/Linux
DESCRIPTION: Command to bootstrap the `lms` CLI tool on macOS or Linux systems by adding it to the system path.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/3_cli/index.md#2025-04-22_snippet_0

LANGUAGE: bash
CODE:
```
~/.lmstudio/bin/lms bootstrap
```

----------------------------------------

TITLE: Setting Inference Parameters with respond() and complete()
DESCRIPTION: Examples of setting inference-time parameters like temperature and maxTokens using both respond() and complete() methods. These parameters can be configured per request.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/parameters.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
result = model.respond(chat, config={
    "temperature": 0.6,
    "maxTokens": 50,
})
```

LANGUAGE: python
CODE:
```
result = model.complete(chat, config={
    "temperature": 0.6,
    "maxTokens": 50,
    "stop": ["\n\n"],
  })
```

----------------------------------------

TITLE: Generating Text Completions with LM Studio
DESCRIPTION: Demonstrates how to generate completions in both non-streaming and streaming modes using a loaded model. Non-streaming returns the complete result at once, while streaming yields text fragments as they are generated.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/completion.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
# The `chat` object is created in the previous step.
result = model.complete("My name is", config={"maxTokens": 100})

print(result)
```

LANGUAGE: python
CODE:
```
# The `chat` object is created in the previous step.
prediction_stream = model.complete_stream("My name is", config={"maxTokens": 100})

for fragment in prediction_stream:
    print(fragment.content, end="", flush=True)
print() # Advance to a new line at the end of the response
```

----------------------------------------

TITLE: Generating Chat Responses
DESCRIPTION: Shows how to generate responses in both streaming and non-streaming modes using the respond() method.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/chat-completion.md#2025-04-22_snippet_3

LANGUAGE: typescript
CODE:
```
// The `chat` object is created in the previous step.
const prediction = model.respond(chat);

for await (const { content } of prediction) {
  process.stdout.write(content);
}

console.info(); // Write a new line to prevent text from being overwritten by your shell.
```

LANGUAGE: typescript
CODE:
```
// The `chat` object is created in the previous step.
const result = await model.respond(chat);

console.info(result.content);
```

----------------------------------------

TITLE: Calling LM Studio Chat Completion (Python)
DESCRIPTION: Demonstrates how to make a chat completion request to an LM Studio model using the OpenAI client and print the response.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/tools.md#_snippet_11

LANGUAGE: python
CODE:
```
response = client.chat.completions.create(
    model=model,
    messages=completion_messages_payload,
)

print("\nFinal model response with knowledge of the tool call result:\n", flush=True)
print(response.choices[0].message.content, flush=True)
```

----------------------------------------

TITLE: Multi-Turn Function Calling Example Script (Python)
DESCRIPTION: A complete Python script illustrating a multi-turn interaction with a language model using tool/function calling. It includes setting up the client, defining a local function ('get_delivery_date'), defining the tool schema, initiating the conversation, processing the model's tool call request, executing the local function, and preparing the response to send the function result back to the model.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/tools.md#_snippet_10

LANGUAGE: python
CODE:
```
from datetime import datetime, timedelta
import json
import random
from openai import OpenAI

# Point to the local server
client = OpenAI(base_url="http://localhost:1234/v1", api_key="lm-studio")
model = "lmstudio-community/qwen2.5-7b-instruct"


def get_delivery_date(order_id: str) -> datetime:
    # Generate a random delivery date between today and 14 days from now
    # in a real-world scenario, this function would query a database or API
    today = datetime.now()
    random_days = random.randint(1, 14)
    delivery_date = today + timedelta(days=random_days)
    print(
        f"\nget_delivery_date function returns delivery date:\n\n{delivery_date}",
        flush=True,
    )
    return delivery_date


tools = [
    {
        "type": "function",
        "function": {
            "name": "get_delivery_date",
            "description": "Get the delivery date for a customer's order. Call this whenever you need to know the delivery date, for example when a customer asks 'Where is my package'",
            "parameters": {
                "type": "object",
                "properties": {
                    "order_id": {
                        "type": "string",
                        "description": "The customer's order ID.",
                    },
                },
                "required": ["order_id"],
                "additionalProperties": False,
            },
        },
    }
]

messages = [
    {
        "role": "system",
        "content": "You are a helpful customer support assistant. Use the supplied tools to assist the user.",
    },
    {
        "role": "user",
        "content": "Give me the delivery date and time for order number 1017",
    },
]

# LM Studio
response = client.chat.completions.create(
    model=model,
    messages=messages,
    tools=tools,
)

print("\nModel response requesting tool call:\n", flush=True)
print(response, flush=True)

# Extract the arguments for get_delivery_date
# Note this code assumes we have already determined that the model generated a function call.
tool_call = response.choices[0].message.tool_calls[0]
arguments = json.loads(tool_call.function.arguments)

order_id = arguments.get("order_id")

# Call the get_delivery_date function with the extracted order_id
delivery_date = get_delivery_date(order_id)

assistant_tool_call_request_message = {
    "role": "assistant",
    "tool_calls": [
        {
            "id": response.choices[0].message.tool_calls[0].id,
            "type": response.choices[0].message.tool_calls[0].type,
            "function": response.choices[0].message.tool_calls[0].function,
        }
    ],
}

# Create a message containing the result of the function call
function_call_result_message = {
    "role": "tool",
    "content": json.dumps(
        {
            "order_id": order_id,
            "delivery_date": delivery_date.strftime("%Y-%m-%d %H:%M:%S"),
        }
    ),
    "tool_call_id": response.choices[0].message.tool_calls[0].id,
}

# Prepare the chat completion call payload
completion_messages_payload = [
    messages[0],
    messages[1],
    assistant_tool_call_request_message,
    function_call_result_message,
]

# Call the OpenAI API's chat completions endpoint to send the tool call result back to the model
```

----------------------------------------

TITLE: Generating Basic Chat Response in Python
DESCRIPTION: Demonstrates how to obtain a simple chat response using both convenience and scoped resource APIs. Shows basic model initialization and response generation.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/chat-completion.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
import lmstudio as lms
model = lms.llm()
print(model.respond("What is the meaning of life?"))
```

LANGUAGE: python
CODE:
```
import lmstudio as lms
with lms.Client() as client:
    model = client.llm.model()
    print(model.respond("What is the meaning of life?"))
```

----------------------------------------

TITLE: Installing lmstudio-python Package (Bash)
DESCRIPTION: Provides commands to install the `lmstudio` Python library from PyPI using common package managers like pip, pdm, and uv. This is the first step to use the library in a project.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_getting-started/project-setup.md#_snippet_0

LANGUAGE: bash
CODE:
```
pip install lmstudio
```

LANGUAGE: bash
CODE:
```
pdm add lmstudio
```

LANGUAGE: bash
CODE:
```
uv add lmstudio
```

----------------------------------------

TITLE: Passing Images to a VLM in LM Studio
DESCRIPTION: Complete example of passing an image to a Vision-Language Model using the .respond() method in LM Studio. Shows how to create a chat with an image and get a prediction about it.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/image-input.md#2025-04-22_snippet_3

LANGUAGE: python
CODE:
```
import lmstudio as lms
image_path = "/path/to/image.jpg" # Replace with the path to your image
image_handle = lms.prepare_image(image_path)
model = lms.llm("qwen2-vl-2b-instruct")
chat = lms.Chat()
chat.add_user_message("Describe this image please", images=[image_handle])
prediction = model.respond(chat)
```

LANGUAGE: python
CODE:
```
import lmstudio as lms
with lms.Client() as client:
    image_path = "/path/to/image.jpg" # Replace with the path to your image
    image_handle = client.files.prepare_image(image_path)
    model = client.llm.model("qwen2-vl-2b-instruct")
    chat = lms.Chat()
    chat.add_user_message("Describe this image please", images=[image_handle])
    prediction = model.respond(chat)
```

----------------------------------------

TITLE: Applying Prompt Template with Chat Object in Python
DESCRIPTION: Demonstrates how to apply a prompt template to a Chat object using LM Studio SDK. Creates a new chat, adds system and user messages, and formats them using the model's prompt template.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/_more/_apply-prompt-template.md#2025-04-22_snippet_0

LANGUAGE: python
CODE:
```
import { Chat, LMStudioClient } from "@lmstudio/sdk"

client = new LMStudioClient()
model = client.llm.model() # Use any loaded LLM

chat = Chat.createEmpty()
chat.append("system", "You are a helpful assistant.")
chat.append("user", "What is LM Studio?")

formatted = model.applyPromptTemplate(chat)
print(formatted)
```

----------------------------------------

TITLE: Checking Chat Fit in Model Context Window using LMStudio
DESCRIPTION: This example demonstrates how to determine if a given conversation fits into a model's context window. It involves converting the conversation to a string, counting tokens, and comparing the count to the model's context length.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/6_model-info/get-context-length.md#2025-04-22_snippet_1

LANGUAGE: python
CODE:
```
import lmstudio as lms

def does_chat_fit_in_context(model: lms.LLM, chat: lms.Chat) -> bool:
    # Convert the conversation to a string using the prompt template.
    formatted = model.apply_prompt_template(chat)
    # Count the number of tokens in the string.
    token_count = len(model.tokenize(formatted))
    # Get the current loaded context length of the model
    context_length = model.get_context_length()
    return token_count < context_length

model = lms.llm()

chat = lms.Chat.from_history({
    "messages": [
        { "role": "user", "content": "What is the meaning of life." },
        { "role": "assistant", "content": "The meaning of life is..." },
        # ... More messages
    ]
})

print("Fits in context:", does_chat_fit_in_context(model, chat))
```

----------------------------------------

TITLE: Obtaining LLM Model Handle
DESCRIPTION: Shows how to obtain a specific model handle (Qwen2.5 7B Instruct) using the LMStudio client.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/2_llm-prediction/chat-completion.md#2025-04-22_snippet_1

LANGUAGE: typescript
CODE:
```
import { LMStudioClient } from "@lmstudio/sdk";
const client = new LMStudioClient();

const model = await client.llm.model("qwen2.5-7b-instruct");
```

----------------------------------------

TITLE: Generating Structured Responses Using JSON Schema in Python
DESCRIPTION: Illustrates how to use a JSON schema to generate structured responses from a language model. Includes examples for both non-streaming and streaming approaches.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/1_python/1_llm-prediction/structured-response.md#2025-04-22_snippet_3

LANGUAGE: python
CODE:
```
result = model.respond("Tell me about The Hobbit", response_format=schema)
book = result.parsed

print(book)
#     ^
# Note that `book` is correctly typed as { title: string, author: string, year: number }
```

LANGUAGE: python
CODE:
```
prediction_stream = model.respond_stream("Tell me about The Hobbit", response_format=schema)

# Stream the response
for fragment in prediction:
    print(fragment.content, end="", flush=True)
print()
# Note that even for structured responses, the *fragment* contents are still only text

# Get the final structured result
result = prediction_stream.result()
book = result.parsed

print(book)
#     ^
# Note that `book` is correctly typed as { title: string, author: string, year: number }
```

----------------------------------------

TITLE: Response Format for POST /api/v0/chat/completions (JSON)
DESCRIPTION: Shows the expected JSON structure returned by the `/api/v0/chat/completions` endpoint, including the generated assistant message, usage stats, and model info.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/1_endpoints/rest.md#_snippet_6

LANGUAGE: json
CODE:
```
{
  "id": "chatcmpl-i3gkjwthhw96whukek9tz",
  "object": "chat.completion",
  "created": 1731990317,
  "model": "granite-3.0-2b-instruct",
  "choices": [
    {
      "index": 0,
      "logprobs": null,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "Greetings, I'm a helpful AI, here to assist,\nIn providing answers, with no distress.\nI'll keep it short and sweet, in rhyme you'll find,\nA friendly companion, all day long you'll bind."
      }
    }
  ],
  "usage": {
    "prompt_tokens": 24,
    "completion_tokens": 53,
    "total_tokens": 77
  },
  "stats": {
    "tokens_per_second": 51.43709529007664,
    "time_to_first_token": 0.111,
    "generation_time": 0.954,
    "stop_reason": "eosFound"
  },
  "model_info": {
    "arch": "granite",
    "quant": "Q4_K_M",
    "format": "gguf",
    "context_length": 4096
  },
  "runtime": {
    "name": "llama.cpp-mac-arm64-apple-metal-advsimd",
    "version": "1.3.0",
    "supported_formats": ["gguf"]
  }
}
```

----------------------------------------

TITLE: Using Custom Tools with LLM Models in LMStudio SDK
DESCRIPTION: Shows how to integrate custom tools with LLM models using the LMStudio SDK. This example initializes an LLM client, loads the Qwen2.5-7b-instruct model, and passes the previously defined file creation tool to the model's `act()` method with a natural language prompt.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/2_typescript/3_agent/tools.md#2025-04-22_snippet_2

LANGUAGE: typescript
CODE:
```
import { LMStudioClient } from "@lmstudio/sdk";
import { createFileTool } from "./createFileTool";

const client = new LMStudioClient();

const model = await client.llm.model("qwen2.5-7b-instruct");
await model.act(
  "Please create a file named output.txt with your understanding of the meaning of life.",
  [createFileTool],
);
```

----------------------------------------

TITLE: Defining a Function Tool for LM Studio
DESCRIPTION: Defines a function tool named `get_delivery_date` with a single required string parameter `order_id`. This structure is provided to the LLM in the `tools` parameter of the chat completion request body, following the OpenAI Function Calling API specification.
SOURCE: https://github.com/lmstudio-ai/docs/blob/main/0_app/3_api/tools.md#_snippet_2

LANGUAGE: JSON
CODE:
```
[
  {
    "type": "function",
    "function": {
      "name": "get_delivery_date",
      "description": "Get the delivery date for a customer's order",
      "parameters": {
        "type": "object",
        "properties": {
          "order_id": {
            "type": "string"
          }
        },
        "required": ["order_id"]
      }
    }
  }
]
```