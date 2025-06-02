TITLE: Initializing OpenAI Client in Python
DESCRIPTION: Sets up the OpenAI client and imports pandas library for data manipulation. This is a foundational step for using the OpenAI API for evaluations.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/Getting_Started_with_OpenAI_Evals.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI
import pandas as pd

client = OpenAI()
```

----------------------------------------

TITLE: Importing and Initializing OpenAI Client - Python
DESCRIPTION: This snippet imports required modules and instantiates the OpenAI API client. It uses the OPENAI_API_KEY environment variable for secure authentication, with a fallback default. Dependencies: the openai package and os module. 'OpenAI' client must be created before making API requests, and the API key is critical for authentication. The client object will be used in subsequent calls to run chat completions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# import the OpenAI Python library for calling the OpenAI API
from openai import OpenAI
import os

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))
```

----------------------------------------

TITLE: Standard Chat Completion Request
DESCRIPTION: Example of a non-streaming chat completion request to count to 100, including timing measurement.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_stream_completions.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# record the time before the request is sent
start_time = time.time()

# send a ChatCompletion request to count to 100
response = client.chat.completions.create(
    model='gpt-4o-mini',
    messages=[
        {'role': 'user', 'content': 'Count to 100, with a comma between each number and no newlines. E.g., 1, 2, 3, ...'}
    ],
    temperature=0,
)
# calculate the time it took to receive the response
response_time = time.time() - start_time

# print the time delay and text received
print(f"Full response received {response_time:.2f} seconds after request")
print(f"Full response received:\n{response}")
```

----------------------------------------

TITLE: Retrying API Calls With Exponential Backoff Using Backoff Library - Python
DESCRIPTION: This Python code uses the 'backoff' library to add exponential backoff retry logic to OpenAI API requests, automatically retrying up to six times or 60 seconds on 'RateLimitError' exceptions. With '@backoff.on_exception', it intercepts errors and retries functions with increasing delays. Depends on 'backoff' and a properly configured OpenAI client. Inputs are keyword arguments for API requests; the decorated function returns the API response or raises an exception after max retries.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_handle_rate_limits.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
import backoff  # for exponential backoff\n\n@backoff.on_exception(backoff.expo, openai.RateLimitError, max_time=60, max_tries=6)\ndef completions_with_backoff(**kwargs):\n    return client.chat.completions.create(**kwargs)\n\n\ncompletions_with_backoff(model="gpt-4o-mini", messages=[{"role": "user", "content": "Once upon a time,"}])\n
```

----------------------------------------

TITLE: Processing Dynamic Queries with Responses API in Python
DESCRIPTION: Processes a list of user queries by constructing message lists, invoking the OpenAI Responses API with tool support, and conditionally calling downstream tools (such as PineconeSearchDocuments or a simulated web search) based on the type of response. After a tool call, results are appended to the conversational context, and a final answer is generated with an additional API call. Required dependencies include an initialized OpenAI API client, access to the relevant tools (e.g., Pinecone index), and a compatible Python environment. Inputs are user queries, and outputs are structured results augmented by tool-based knowledge; responses are managed via a dynamic message structure.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_api_tool_orchestration.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
# Process each query dynamically.
for item in queries:
    input_messages = [{"role": "user", "content": item["query"]}]
    print("\n🌟--- Processing Query ---🌟")
    print(f"🔍 **User Query:** {item['query']}")
    
    # Call the Responses API with tools enabled and allow parallel tool calls.
    response = client.responses.create(
        model="gpt-4o",
        input=[
            {"role": "system", "content": "When prompted with a question, select the right tool to use based on the question."
            },
            {"role": "user", "content": item["query"]}
        ],
        tools=tools,
        parallel_tool_calls=True
    )
    
    print("\n✨ **Initial Response Output:**")
    print(response.output)
    
    # Determine if a tool call is needed and process accordingly.
    if response.output:
        tool_call = response.output[0]
        if tool_call.type in ["web_search_preview", "function_call"]:
            tool_name = tool_call.name if tool_call.type == "function_call" else "web_search_preview"
            print(f"\n🔧 **Model triggered a tool call:** {tool_name}")
            
            if tool_name == "PineconeSearchDocuments":
                print("🔍 **Invoking PineconeSearchDocuments tool...**")
                res = query_pinecone_index(client, index, MODEL, item["query"])
                if res["matches"]:
                    best_match = res["matches"][0]["metadata"]
                    result = f"**Question:** {best_match.get('Question', 'N/A')}\n**Answer:** {best_match.get('Answer', 'N/A')}"
                else:
                    result = "**No matching documents found in the index.**"
                print("✅ **PineconeSearchDocuments tool invoked successfully.**")
            else:
                print("🔍 **Invoking simulated web search tool...**")
                result = "**Simulated web search result.**"
                print("✅ **Simulated web search tool invoked successfully.**")
            
            # Append the tool call and its output back into the conversation.
            input_messages.append(tool_call)
            input_messages.append({
                "type": "function_call_output",
                "call_id": tool_call.call_id,
                "output": str(result)
            })
            
            # Get the final answer incorporating the tool's result.
            final_response = client.responses.create(
                model="gpt-4o",
                input=input_messages,
                tools=tools,
                parallel_tool_calls=True
            )
            print("\n💡 **Final Answer:**")
            print(final_response.output_text)
        else:
            # If no tool call is triggered, print the response directly.
            print("💡 **Final Answer:**")
            print(response.output_text)

```

----------------------------------------

TITLE: Transforming and Saving Invoice JSONs with GPT-4o - Python
DESCRIPTION: This code provides Python functions to automate mapping of extracted invoice JSON data to a standardized schema using GPT-4o. The function 'transform_invoice_data' constructs a prompt for GPT-4o, requesting the AI to both structure the data according to schema fields and translate all content into English, returning strictly JSON output. The 'main_transform' function batch processes all files in a specified directory, transforming them according to the schema and saving them to disk. Key dependencies include the OpenAI client library (for GPT-4o API calls), standard 'json' and 'os' Python modules, and access to local file paths for raw and schema data. Limitations include the need for valid API credentials, internet access, and the assumption that some data may be missing or language translation is required; any data not fitting the schema will be omitted or filled with null.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Data_extraction_transformation.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
def transform_invoice_data(json_raw, json_schema):
    system_prompt = f"""
    You are a data transformation tool that takes in JSON data and a reference JSON schema, and outputs JSON data according to the schema.
    Not all of the data in the input JSON will fit the schema, so you may need to omit some data or add null values to the output JSON.
    Translate all data into English if not already in English.
    Ensure values are formatted as specified in the schema (e.g. dates as YYYY-MM-DD).
    Here is the schema:
    {json_schema}

    """
    
    response = client.chat.completions.create(
        model="gpt-4o",
        response_format={ "type": "json_object" },
        messages=[
            {
                "role": "system",
                "content": system_prompt
            },
            {
                "role": "user",
                "content": [
                    {"type": "text", "text": f"Transform the following raw JSON data according to the provided schema. Ensure all data is in English and formatted as specified by values in the schema. Here is the raw JSON: {json_raw}"}
                ]
            }
        ],
        temperature=0.0,
    )
    return json.loads(response.choices[0].message.content)



def main_transform(extracted_invoice_json_path, json_schema_path, save_path):
    # Load the JSON schema
    with open(json_schema_path, 'r', encoding='utf-8') as f:
        json_schema = json.load(f)

    # Ensure the save directory exists
    os.makedirs(save_path, exist_ok=True)

    # Process each JSON file in the extracted invoices directory
    for filename in os.listdir(extracted_invoice_json_path):
        if filename.endswith(".json"):
            file_path = os.path.join(extracted_invoice_json_path, filename)

            # Load the extracted JSON
            with open(file_path, 'r', encoding='utf-8') as f:
                json_raw = json.load(f)

            # Transform the JSON data
            transformed_json = transform_invoice_data(json_raw, json_schema)

            # Save the transformed JSON to the save directory
            transformed_filename = f"transformed_{filename}"
            transformed_file_path = os.path.join(save_path, transformed_filename)
            with open(transformed_file_path, 'w', encoding='utf-8') as f:
                json.dump(transformed_json, f, ensure_ascii=False, indent=2)

   
    extracted_invoice_json_path = "./data/hotel_invoices/extracted_invoice_json"
    json_schema_path = "./data/hotel_invoices/invoice_schema.json"
    save_path = "./data/hotel_invoices/transformed_invoice_json"

    main_transform(extracted_invoice_json_path, json_schema_path, save_path)
```

----------------------------------------

TITLE: Initializing OpenAI Embeddings Client and Defining Embedding Function in Python
DESCRIPTION: This snippet sets up an OpenAI API client, model constants, and a robust embedding query function with retry logic, leveraging tenacity for resilience against network or rate-limit failures. It demonstrates the setup required to get embeddings for a text or token sequence using the 'text-embedding-3-small' model (context length and encoding specified). Requires 'openai' and 'tenacity' Python packages. Inputs include the text/token sequence and an optional model name; output is the embedding vector. Designed to skip retrying on invalid requests for demonstration.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Embedding_long_inputs.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI
import os
import openai
from tenacity import retry, wait_random_exponential, stop_after_attempt, retry_if_not_exception_type

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))

EMBEDDING_MODEL = 'text-embedding-3-small'
EMBEDDING_CTX_LENGTH = 8191
EMBEDDING_ENCODING = 'cl100k_base'

# let's make sure to not retry on an invalid request, because that is what we want to demonstrate
@retry(wait=wait_random_exponential(min=1, max=20), stop=stop_after_attempt(6), retry=retry_if_not_exception_type(openai.BadRequestError))
def get_embedding(text_or_tokens, model=EMBEDDING_MODEL):
    return client.embeddings.create(input=text_or_tokens, model=model).data[0].embedding
```

----------------------------------------

TITLE: Batching Multiple Prompts with Structured Outputs using OpenAI and Pydantic in Python
DESCRIPTION: This snippet shows how to batch multiple prompts into a single OpenAI API request using structured outputs, specified via a Pydantic model. The StoryResponse model enforces a schema with a list of stories and their count, and all prompts are joined and sent as a bulk message. The response is parsed with response_format to guarantee structured, type-safe data, reducing the need for manual post-processing. Dependencies include the pydantic package, OpenAI client.beta interface, and a suitable model. Inputs are a customizable number of prompts and content; outputs conform to the StoryResponse schema. This approach is efficient for applications needing reliable bulk responses within token and rate limits.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_handle_rate_limits.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
from pydantic import BaseModel

# Define the Pydantic model for the structured output
class StoryResponse(BaseModel):
    stories: list[str]
    story_count: int

num_stories = 10
content = "Once upon a time,"

prompt_lines = [f"Story #{{i+1}}: {{content}}" for i in range(num_stories)]
prompt_text = "\n".join(prompt_lines)

messages = [
    {
        "role": "developer",
        "content": "You are a helpful assistant. Please respond to each prompt as a separate short story."
    },
    {
        "role": "user",
        "content": prompt_text
    }
]

# batched example, with all story completions in one request and using structured outputs
response = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=messages,
    response_format=StoryResponse,
)

print(response.choices[0].message.content)
```

----------------------------------------

TITLE: Importing Required Libraries and Initializing OpenAI Client
DESCRIPTION: Imports necessary Python libraries and initializes the OpenAI client for API interactions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Structured_Outputs_Intro.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import json
from textwrap import dedent
from openai import OpenAI
client = OpenAI()
```

----------------------------------------

TITLE: Handling Multi-Turn Conversation with Function Calling (Python)
DESCRIPTION: This Python snippet demonstrates how to manage a multi-turn conversation with an OpenAI client that supports function calling and reasoning. It iterates through user messages, sends them to the model, and processes the response in a loop, handling reasoning steps, invoking functions, and appending outputs back to the conversation history until the model indicates it is finished (no more function calls). It requires an initialized 'client' object, a list of 'user_messages', and a 'MODEL_DEFAULTS' dictionary, as well as an 'invoke_functions_from_response' function.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
total_tokens_used = 0
user_messages = [
    (
        "Of those cities that have hosted the summer Olympic games in the last 20 years - "
        "do any of them have IDs beginning with a number and a temperate climate? "
        "Use your available tools to look up the IDs for each city and make sure to search the web to find out about the climate."
    ),
    "Great thanks! We've just updated the IDs - could you please check again?"
    ]

conversation = []
for message in user_messages:
    conversation_item = {
        "role": "user",
        "type": "message",
        "content": message
    }
    print(f"{'*' * 79}\nUser message: {message}\n{'*' * 79}")
    conversation.append(conversation_item)
    while True: # Response loop
        response = client.responses.create(
            input=conversation,
            **MODEL_DEFAULTS
        )
        total_tokens_used += response.usage.total_tokens
        reasoning = [rx.to_dict() for rx in response.output if rx.type == 'reasoning']
        function_calls = [rx.to_dict() for rx in response.output if rx.type == 'function_call']
        messages = [rx.to_dict() for rx in response.output if rx.type == 'message']
        if len(reasoning) > 0:
            print("More reasoning required, continuing...")
            # Ensure we capture any reasoning steps
            conversation.extend(reasoning)
            print('\n'.join(s['text'] for r in reasoning for s in r['summary']))
        if len(function_calls) > 0:
            function_outputs = invoke_functions_from_response(response)
            # Preserve order of function calls and outputs in case of multiple function calls (currently not supported by reasoning models, but worth considering)
            interleaved = [val for pair in zip(function_calls, function_outputs) for val in pair]
            conversation.extend(interleaved)
        if len(messages) > 0:
            print(response.output_text)
            conversation.extend(messages)
        if len(function_calls) == 0:  # No more functions = We're done reasoning and we're ready for the next user message
            break
print(f"Total tokens used: {total_tokens_used} ({total_tokens_used / 200_000:.2%} of o4-mini's context window)")
```

----------------------------------------

TITLE: Embedding with Retry Logic Using Tenacity and OpenAI API - Python
DESCRIPTION: This snippet illustrates a best practice for robust text embedding generation using tenacity's @retry decorator for automatic exponential backoff retries on failures. Dependencies include openai and tenacity. The get_embedding function fetches an embedding for the given text and model, retrying up to 6 times with delays between 1 and 20 seconds as needed to manage API rate limits. It prints the length of the resulting embedding vector and is suitable for production usage where rate limiting is expected.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Using_embeddings.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Best practice
from tenacity import retry, wait_random_exponential, stop_after_attempt
from openai import OpenAI
client = OpenAI()

# Retry up to 6 times with exponential backoff, starting at 1 second and maxing out at 20 seconds delay
@retry(wait=wait_random_exponential(min=1, max=20), stop=stop_after_attempt(6))
def get_embedding(text: str, model="text-embedding-3-small") -> list[float]:
    return client.embeddings.create(input=[text], model=model).data[0].embedding

embedding = get_embedding("Your text goes here", model="text-embedding-3-small")
print(len(embedding))
```

----------------------------------------

TITLE: Importing OpenAI SDK and Utilities in Python
DESCRIPTION: This snippet imports essential modules such as ast for syntax verification, os for environment variable access, and the OpenAI Python client for communicating with GPT models. The OpenAI client is initialized using an API key which can either be set as an environment variable or directly passed in the code. This setup is required for all subsequent interactions with GPT.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Unit_test_writing_using_a_multi-step_prompt.ipynb#_snippet_0

LANGUAGE: Python
CODE:
```
# imports needed to run the code in this notebook\nimport ast  # used for detecting whether generated Python code is valid\nimport os\nfrom openai import OpenAI\n\nclient = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))\n
```

----------------------------------------

TITLE: Providing User-Specific Recommendations with GPT-3.5-Turbo and Google Places API (Python)
DESCRIPTION: This Python function generates personalized recommendations for users by combining their profile data with context-aware natural language understanding through GPT-3.5-Turbo, and actionable results via the Google Places API. It depends on functions like fetch_customer_profile, call_google_places_api, an OpenAI client (client.chat.completions.create), and the json module. Inputs include user_input (the user's query) and user_id (to fetch user profile); outputs are tailored recommendation strings or error messages. The snippet expects the external dependencies to be set up and appropriate API credentials to be configured, and is meant to be integrated within a backend or service that handles user sessions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Function_calling_finding_nearby_places.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
def provide_user_specific_recommendations(user_input, user_id):\n    customer_profile = fetch_customer_profile(user_id)\n    if customer_profile is None:\n        return \"I couldn't find your profile. Could you please verify your user ID?\"\n\n    customer_profile_str = json.dumps(customer_profile)\n\n    food_preference = customer_profile.get('preferences', {}).get('food', [])[0] if customer_profile.get('preferences', {}).get('food') else None\n\n\n    response = client.chat.completions.create(\n        model=\"gpt-3.5-turbo\",\n        messages=[\n    {\n        \"role\": \"system\",\n        \"content\": f\"You are a sophisticated AI assistant, a specialist in user intent detection and interpretation. Your task is to perceive and respond to the user's needs, even when they're expressed in an indirect or direct manner. You excel in recognizing subtle cues: for example, if a user states they are 'hungry', you should assume they are seeking nearby dining options such as a restaurant or a cafe. If they indicate feeling 'tired', 'weary', or mention a long journey, interpret this as a request for accommodation options like hotels or guest houses. However, remember to navigate the fine line of interpretation and assumption: if a user's intent is unclear or can be interpreted in multiple ways, do not hesitate to politely ask for additional clarification. Make sure to tailor your responses to the user based on their preferences and past experiences which can be found here {customer_profile_str}\"\n    },\n    {\"role\": \"user\", \"content\": user_input}\n],\n        temperature=0,\n        tools=[\n            {\n                \"type\": \"function\",\n                \"function\" : {\n                    \"name\": \"call_google_places_api\",\n                    \"description\": \"This function calls the Google Places API to find the top places of a specified type near a specific location. It can be used when a user expresses a need (e.g., feeling hungry or tired) or wants to find a certain type of place (e.g., restaurant or hotel).\",\n                    \"parameters\": {\n                        \"type\": \"object\",\n                        \"properties\": {\n                            \"place_type\": {\n                                \"type\": \"string\",\n                                \"description\": \"The type of place to search for.\"\n                            }\n                        }\n                    },\n                    \"result\": {\n                        \"type\": \"array\",\n                        \"items\": {\n                            \"type\": \"string\"\n                        }\n                    }\n                }\n            }\n        ],\n    )\n\n    print(response.choices[0].message.tool_calls)\n\n    if response.choices[0].finish_reason=='tool_calls':\n        function_call = response.choices[0].message.tool_calls[0].function\n        if function_call.name == \"call_google_places_api\":\n            place_type = json.loads(function_call.arguments)[\"place_type\"]\n            places = call_google_places_api(user_id, place_type, food_preference)\n            if places:  # If the list of places is not empty\n                return f\"Here are some places you might be interested in: {' '.join(places)}\"\n            else:\n                return \"I couldn't find any places of interest nearby.\"\n\n    return \"I am sorry, but I could not understand your request.\"\n
```

----------------------------------------

TITLE: Orchestrating OpenAI Conversation with Tool Calls Loop (Python)
DESCRIPTION: Illustrates a loop structure for managing an OpenAI conversation that involves tool calls. It sends an initial prompt, processes the model's response by invoking tools, and sends the tool outputs back to the model until the model provides a final text output.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
initial_question = (
    "What are the internal IDs for the cities that have hosted the Olympics in the last 20 years, "
    "and which of those cities have recent news stories (in 2025) about the Olympics? "
    "Use your internal tools to look up the IDs and the web search tool to find the news stories."
)

# We fetch a response and then kick off a loop to handle the response
response = client.responses.create(
    input=initial_question,
    **MODEL_DEFAULTS,
)
while True:   
    function_responses = invoke_functions_from_response(response)
    if len(function_responses) == 0: # We're done reasoning
        print(response.output_text)
        break
    else:
        print("More reasoning required, continuing...")
        response = client.responses.create(
            input=function_responses,
            previous_response_id=response.id,
            **MODEL_DEFAULTS
        )
```

----------------------------------------

TITLE: Structuring Tax Eligibility Reasoning Prompts for GPT-3.5 Turbo - Markdown Prompt - gpt-3.5-turbo-instruct
DESCRIPTION: This snippet provides a structured prompt template for GPT-3.5 Turbo, instructing it to answer questions about federal tax credit eligibility for vehicles based on IRS guidelines. Required dependencies: a model that understands prompt engineering (e.g., OpenAI GPT-3.5 Turbo Instruct), and access to IRS tax credit criteria. The prompt includes sections for stepwise criterion assessment and a final, reasoned answer. Inputs include factual vehicle purchase data; outputs are detailed criterion analyses and summary conclusions. Limitations: assumes complete vehicle data and correct model interpretation of the guidance.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/techniques_to_improve_reliability.md#_snippet_12

LANGUAGE: gpt-3.5-turbo-instruct
CODE:
```
Using the IRS guidance below, answer the following questions using this format:\n(1) For each criterion, determine whether it is met by the vehicle purchase\n- {Criterion} Let's think step by step. {explanation} {yes or no, or if the question does not apply then N/A}.\n(2) After considering each criterion in turn, phrase the final answer as \"Because of {reasons}, the answer is likely {yes or no}.\"\n\nIRS guidance:\n"""\nYou may be eligible for a federal tax credit under Section 30D if you purchased a car or truck that meets the following criteria:\n- Does the vehicle have at least four wheels?\n- Does the vehicle weigh less than 14,000 pounds?\n- Does the vehicle draw energy from a battery with at least 4 kilowatt hours that may be recharged from an external source?\n- Was the vehicle purchased in a year before 2022?\n  - If so, has the manufacturer sold less than 200,000 qualifying vehicles? (Tesla and GM have sold more than 200,000 qualifying vehicles.)\n- Was the vehicle purchased in a year after 2022?\n  - If so, is the vehicle present in the following list of North American-assembled vehicles? (The only electric vehicles assembled in North America are the Audi Q5, BMW 330e, BMW X5, Chevrolet Bolt EUV, Chevrolet Bolt EV, Chrysler Pacifica PHEV, Ford Escape PHEV, Ford F Series, Ford Mustang MACH E, Ford Transit Van, GMC Hummer Pickup, GMC Hummer SUV, Jeep Grand Cherokee PHEV, Jeep Wrangler PHEV, Lincoln Aviator PHEV, Lincoln Corsair Plug-in, Lucid Air, Nissan Leaf, Rivian EDV, Rivian R1S, Rivian R1T, Tesla Model 3, Tesla Model S, Tesla Model X, Tesla Model Y, Volvo S60, BMW 330e, Bolt EV, Cadillac Lyriq, Mercedes EQS SUV, and Nissan Leaf.)\n"""\n\nQuestion: Can I claim a federal tax credit for my Toyota Prius Prime bought in 2021?\n\nSolution:\n\n(1) For each criterion, determine whether it is met by the vehicle purchase\n- Does the vehicle have at least four wheels? Let's think step by step.
```

----------------------------------------

TITLE: Synthesizing Final Answer using Top Search Results and GPT - Python
DESCRIPTION: Prepares the top search results as input, then prompts GPT to generate a comprehensive answer that references these sources with markdown links. Uses the OpenAI chat completion endpoint and streams the output for live display (suitable for Jupyter/IPython environments). Inputs are structured representations of top results and the original question; the output is a synthesized, reference-rich answer intended for user consumption.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Question_answering_using_a_search_API.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
formatted_top_results = [
    {
        "title": article["title"],
        "description": article["description"],
        "url": article["url"],
    }
    for article, _score in sorted_articles[0:5]
]

ANSWER_INPUT = f"""
Generate an answer to the user's question based on the given search results. 
TOP_RESULTS: {formatted_top_results}
USER_QUESTION: {USER_QUESTION}

Include as much information as possible in the answer. Reference the relevant search result urls as markdown links.
"""

completion = client.chat.completions.create(
    model=GPT_MODEL,
    messages=[{"role": "user", "content": ANSWER_INPUT}],
    temperature=0.5,
    stream=True,
)

text = ""
for chunk in completion:
    text += chunk.choices[0].delta.content
    display.clear_output(wait=True)
    display.display(display.Markdown(text))
```

----------------------------------------

TITLE: Extracting Movie Categories and Summaries using Chat Completions - Python
DESCRIPTION: Prepares and sends a chat completion request to OpenAI to extract movie genres (categories) and produce a one-sentence summary from movie descriptions. Uses a structured system prompt and JSON response mode for machine-readable results. Requires a valid OpenAI client and pandas DataFrame row descriptions as inputs. Returns parsed assistant output as a JSON string.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/batch_processing.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
categorize_system_prompt = '''
Your goal is to extract movie categories from movie descriptions, as well as a 1-sentence summary for these movies.
You will be provided with a movie description, and you will output a json object containing the following information:

{
    categories: string[] // Array of categories based on the movie description,
    summary: string // 1-sentence summary of the movie based on the movie description
}

Categories refer to the genre or type of the movie, like "action", "romance", "comedy", etc. Keep category names simple and use only lower case letters.
Movies can have several categories, but try to keep it under 3-4. Only mention the categories that are the most obvious based on the description.
'''

def get_categories(description):
    response = client.chat.completions.create(
    model="gpt-4o-mini",
    temperature=0.1,
    # This is to enable JSON mode, making sure responses are valid json objects
    response_format={ 
        "type": "json_object"
    },
    messages=[
        {
            "role": "system",
            "content": categorize_system_prompt
        },
        {
            "role": "user",
            "content": description
        }
    ],
    )

    return response.choices[0].message.content
```

----------------------------------------

TITLE: Converting PDF Pages to Images and Interpreting with OpenAI Vision in Python
DESCRIPTION: These functions handle page-level PDF-to-image conversion and visual interpretation using OpenAI GPT-4 Vision. Dependencies include the pillow (PIL) library for image manipulation, OpenAI API client, and OS utilities. Inputs consist of PDF pages as bytes and prompts; outputs are image file paths and vision API responses. Limitation: each page must be processed individually and images are saved to a relative directory.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Using_vision_modality_for_RAG_with_Pinecone.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Function to convert page to image     
def convert_page_to_image(pdf_bytes, page_number):
    # Convert the PDF page to an image
    images = convert_from_bytes(pdf_bytes)
    image = images[0]  # There should be only one page

    # Define the directory to save images (relative to your script)
    images_dir = 'images'  # Use relative path here

    # Ensure the directory exists
    os.makedirs(images_dir, exist_ok=True)

    # Save the image to the images directory
    image_file_name = f"page_{page_number}.png"
    image_file_path = os.path.join(images_dir, image_file_name)
    image.save(image_file_path, 'PNG')

    # Return the relative image path
    return image_file_path


# Pass the image to the LLM for interpretation  
def get_vision_response(prompt, image_path):
    # Getting the base64 string
    base64_image = encode_image(image_path)

    response = oai_client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {
                "role": "user",
                "content": [
                    {"type": "text", "text": prompt},
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": f"data:image/jpeg;base64,{base64_image}"
                        },
                    },
                ],
            }
        ],
    )
    return response

```

----------------------------------------

TITLE: Loading encoding for a specific OpenAI model
DESCRIPTION: Loading the appropriate encoding for a specific model (gpt-4o-mini) using the encoding_for_model method, which automatically selects the correct encoding.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
encoding = tiktoken.encoding_for_model("gpt-4o-mini")
```

----------------------------------------

TITLE: Getting Embedding Vectors with OpenAI Embeddings API (Python)
DESCRIPTION: This snippet initializes the OpenAI client with an API key, sets the embedding model, and requests embeddings for a list of input texts using the 'embeddings.create' method. Required dependencies include the 'openai' Python package and a valid API key. Inputs are a list of strings; outputs are embedding vectors. The code targets compatibility with OpenAI v1.0+ and demonstrates a batched embedding API call.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_CQL.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
client = openai.OpenAI(api_key=OPENAI_API_KEY)
embedding_model_name = "text-embedding-3-small"

result = client.embeddings.create(
    input=[
        "This is a sentence",
        "A second sentence"
    ],
    model=embedding_model_name,
)
```

----------------------------------------

TITLE: Implementing Customer Service Conversation and Tool Execution Logic (Python)
DESCRIPTION: Defines the central message submission, tool-calling, and function-execution logic for the customer service agent flow. The 'submit_user_message' function processes user queries in a loop, ensuring required tool usage per message, while 'execute_function' dispatches logic based on the called tool and updates the conversation state. Dependencies: OpenAI client, tool and instruction definitions, and a system prompt specifying dialog behavior. Inputs: user queries and conversation state. Outputs: an updated conversation history. Limitations: Tool calls must resolve before a user-facing response, only one tool per message.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Using_tool_required_for_customer_service.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
assistant_system_prompt = """You are a customer service assistant. Your role is to answer user questions politely and competently.
You should follow these instructions to solve the case:
- Understand their problem and get the relevant instructions.
- Follow the instructions to solve the customer's problem. Get their confirmation before performing a permanent operation like a refund or similar.
- Help them with any other problems or close the case.

Only call a tool once in a single message.
If you need to fetch a piece of information from a system or document that you don't have access to, give a clear, confident answer with some dummy values."""

def submit_user_message(user_query,conversation_messages=[]):
    """Message handling function which loops through tool calls until it reaches one that requires a response.
    Once it receives respond=True it returns the conversation_messages to the user."""

    # Initiate a respond object. This will be set to True by our functions when a response is required
    respond = False
    
    user_message = {"role":"user","content": user_query}
    conversation_messages.append(user_message)

    print(f"User: {user_query}")

    while respond is False:

        # Build a transient messages object to add the conversation messages to
        messages = [
            {
                "role": "system",
                "content": assistant_system_prompt
            }
        ]

        # Add the conversation messages to our messages call to the API
        [messages.append(x) for x in conversation_messages]

        # Make the ChatCompletion call with tool_choice='required' so we can guarantee tools will be used
        response = client.chat.completions.create(model=GPT_MODEL
                                                  ,messages=messages
                                                  ,temperature=0
                                                  ,tools=tools
                                                  ,tool_choice='required'
                                                 )

        conversation_messages.append(response.choices[0].message)

        # Execute the function and get an updated conversation_messages object back
        # If it doesn't require a response, it will ask the assistant again. 
        # If not the results are returned to the user.
        respond, conversation_messages = execute_function(response.choices[0].message,conversation_messages)
    
    return conversation_messages

def execute_function(function_calls,messages):
    """Wrapper function to execute the tool calls"""

    for function_call in function_calls.tool_calls:
    
        function_id = function_call.id
        function_name = function_call.function.name
        print(f"Calling function {function_name}")
        function_arguments = json.loads(function_call.function.arguments)
    
        if function_name == 'get_instructions':

            respond = False
    
            instruction_name = function_arguments['problem']
            instructions = INSTRUCTIONS['type' == instruction_name]
    
            messages.append(
                                {
                                    "tool_call_id": function_id,
                                    "role": "tool",
                                    "name": function_name,
                                    "content": instructions['instructions'],
                                }
                            )
    
        elif function_name != 'get_instructions':

            respond = True
    
            messages.append(
                                {
                                    "tool_call_id": function_id,
                                    "role": "tool",
                                    "name": function_name,
                                    "content": function_arguments['message'],
                                }
                            )
    
            print(f"Assistant: {function_arguments['message']}")
    
    return (respond, messages)
    
```

----------------------------------------

TITLE: Installing jsonref and openai Dependencies in Python
DESCRIPTION: Installs the necessary Python packages `jsonref` (for resolving JSON references like $ref in OpenAPI specs) and `openai` (for interacting with OpenAI's API). No input or output beyond installing the packages. Must be run in an environment that supports shell commands (e.g., Jupyter notebook or Colab).
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Function_calling_with_an_OpenAPI_spec.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
!pip install -q jsonref # for resolving $ref's in the OpenAPI spec\n!pip install -q openai
```

----------------------------------------

TITLE: Creating an Iterative Loop for OpenAI Chat Completions - Node.js JavaScript
DESCRIPTION: Implements a loop that sends up to five chat completion requests to the OpenAI API, processing outputs for tool-directed actions or returning an answer upon completion. If the GPT response requests a function call, the loop locates and executes the required function, adds the result to the conversation history, and continues. Dependencies are OpenAI's Node.js SDK and user-defined functions; expected input is the messages and tools arrays and available function implementations. The loop exits with an answer if GPT replies with finish_reason 'stop', or with an error message after 5 attempts.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_an_agent_with_the_node_sdk.mdx#_snippet_7

LANGUAGE: JavaScript
CODE:
```
for (let i = 0; i < 5; i++) {
  const response = await openai.chat.completions.create({
    model: "gpt-4",
    messages: messages,
    tools: tools,
  });
  const { finish_reason, message } = response.choices[0];

  if (finish_reason === "tool_calls" && message.tool_calls) {
    const functionName = message.tool_calls[0].function.name;
    const functionToCall = availableTools[functionName];
    const functionArgs = JSON.parse(message.tool_calls[0].function.arguments);
    const functionArgsArr = Object.values(functionArgs);
    const functionResponse = await functionToCall.apply(null, functionArgsArr);

    messages.push({
      role: "function",
      name: functionName,
      content: `
          The result of the last function was this: ${JSON.stringify(
            functionResponse
          )}
          `,
    });
  } else if (finish_reason === "stop") {
    messages.push(message);
    return message.content;
  }
}
return "The maximum number of iterations has been met without a suitable answer. Please try again with a more specific input.";
```

----------------------------------------

TITLE: Embedding and Batch Inserting Book Data into Milvus - Python
DESCRIPTION: Batches titles and descriptions, periodically calls OpenAI for embeddings, and inserts results into Milvus. Handles partial batch at the end for complete data ingestion. Uses tqdm for progress display. All input data is processed, but interruption is possible; smaller batches reduce memory pressure.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/milvus/Getting_started_with_Milvus_and_OpenAI.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
from tqdm import tqdm

data = [
    [], # title
    [], # description
]

# Embed and insert in batches
for i in tqdm(range(0, len(dataset))):
    data[0].append(dataset[i]['title'])
    data[1].append(dataset[i]['description'])
    if len(data[0]) % BATCH_SIZE == 0:
        data.append(embed(data[1]))
        collection.insert(data)
        data = [[],[]]

# Embed and insert the remainder 
if len(data[0]) != 0:
    data.append(embed(data[1]))
    collection.insert(data)
    data = [[],[]]

```

----------------------------------------

TITLE: Initializing OpenAI Client and Model Defaults (Python)
DESCRIPTION: This snippet demonstrates how to import necessary libraries, initialize the OpenAI client, and define default parameters for API calls using a reasoning model like 'o4-mini', including setting reasoning effort and summary options.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
# pip install openai
# Import libraries 
import json
from openai import OpenAI
from uuid import uuid4
from typing import Callable

client = OpenAI()
MODEL_DEFAULTS = {
    "model": "o4-mini", # 200,000 token context window
    "reasoning": {"effort": "low", "summary": "auto"}, # Automatically summarise the reasoning process. Can also choose "detailed" or "none"
}
```

----------------------------------------

TITLE: Securely Prompting for OpenAI API Key - Python
DESCRIPTION: Requests the user to input their OpenAI API Key using getpass for secure entry. This key is used for authenticating all further OpenAI API requests, especially for generating embeddings and LLM completions as part of the quote finder/generator system. No data is leaked to output.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_CQL.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
OPENAI_API_KEY = getpass("Please enter your OpenAI API Key: ")
```

----------------------------------------

TITLE: Semantic Search and Question Answering with GPT-4o in Python
DESCRIPTION: This function performs semantic search using Pinecone and generates answers to user questions using GPT-4o. It embeds the question, queries Pinecone for relevant pages, compiles context metadata, and uses GPT-4o to generate a response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Using_vision_modality_for_RAG_with_Pinecone.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
import json


# Function to get response to a user's question 
def get_response_to_question(user_question, pc_index):
    # Get embedding of the question to find the relevant page with the information 
    question_embedding = get_embedding(user_question)

    # get response vector embeddings 
    response = pc_index.query(
        vector=question_embedding,
        top_k=2,
        include_values=True,
        include_metadata=True
    )

    # Collect the metadata from the matches
    context_metadata = [match['metadata'] for match in response['matches']]

    # Convert the list of metadata dictionaries to prompt a JSON string
    context_json = json.dumps(context_metadata, indent=3)

    prompt = f"""You are a helpful assistant. Use the following context and images to answer the question. In the answer, include the reference to the document, and page number you found the information on between <source></source> tags. If you don't find the information, you can say "I couldn't find the information"

    question: {user_question}
    
    <SOURCES>
    {context_json}
    </SOURCES>
    """

    # Call completions end point with the prompt 
    completion = oai_client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {"role": "system", "content": prompt}
        ]
    )

    return completion.choices[0].message.content
```

----------------------------------------

TITLE: Structuring Documents with XML for Long Context
DESCRIPTION: Shows how to use XML tags and attributes to structure individual documents when providing a large number of documents as input context, improving performance in long context scenarios.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/gpt4-1_prompting_guide.ipynb#_snippet_18

LANGUAGE: XML
CODE:
```
<doc id='1' title='The Fox'>The quick brown fox jumps over the lazy dog</doc>
```

----------------------------------------

TITLE: Getting Started with Semantic Vector Search in Weaviate using OpenAI - Python Notebook
DESCRIPTION: This Python notebook demonstrates how to integrate Weaviate with OpenAI's text2vec-openai module for semantic vector search. The notebook guides the user through configuration, connection, data loading, vectorization, and searching workflows using Python client libraries. The required dependencies are Python, the Weaviate client SDK, and valid OpenAI API credentials. Inputs include data objects for indexing; outputs include ranked search results by semantic similarity. Constraints involve correct API setup and Docker or cloud-based Weaviate instance availability.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/README.md#_snippet_1

LANGUAGE: Python Notebook
CODE:
```
# Example Python code (from getting-started-with-weaviate-and-openai.ipynb)
import weaviate
client = weaviate.Client("http://localhost:8080")
# Insert data and perform semantic search ...

```

----------------------------------------

TITLE: Verifying OpenAI API Key Configuration
DESCRIPTION: Checks if the OpenAI API key is correctly set as an environment variable. Also shows how to temporarily set the variable in the code if needed.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/question-answering-with-weaviate-and-openai.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Test that your OpenAI API key is correctly set as an environment variable
# Note. if you run this notebook locally, you will need to reload your terminal and the notebook for the env variables to be live.
import os

# Note. alternatively you can set a temporary env variable like this:
# os.environ['OPENAI_API_KEY'] = 'your-key-goes-here'

if os.getenv("OPENAI_API_KEY") is not None:
    print ("OPENAI_API_KEY is ready")
else:
    print ("OPENAI_API_KEY environment variable not found")
```

----------------------------------------

TITLE: Verifying OpenAI API Key Availability
DESCRIPTION: Tests that the OpenAI API key is correctly set as an environment variable. Alternatively shows how to set a temporary environment variable within the script.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/generative-search-with-weaviate-and-openai.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# Test that your OpenAI API key is correctly set as an environment variable
# Note. if you run this notebook locally, you will need to reload your terminal and the notebook for the env variables to be live.
import os

# Note. alternatively you can set a temporary env variable like this:
# os.environ["OPENAI_API_KEY"] = 'your-key-goes-here'

if os.getenv("OPENAI_API_KEY") is not None:
    print ("OPENAI_API_KEY is ready")
else:
    print ("OPENAI_API_KEY environment variable not found")
```

----------------------------------------

TITLE: Validating OPENAI_API_KEY in Python Environment
DESCRIPTION: Tests if the 'OPENAI_API_KEY' is properly set in the environment variables and prints an appropriate message. The snippet uses Python's 'os' module and is intended to be run in an environment where the key was previously exported. The essential parameter is the presence/value of 'OPENAI_API_KEY' in the OS environment.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/hybrid-search-with-weaviate-and-openai.ipynb#_snippet_2

LANGUAGE: Python
CODE:
```
# Test that your OpenAI API key is correctly set as an environment variable\n# Note. if you run this notebook locally, you will need to reload your terminal and the notebook for the env variables to be live.\nimport os\n\n# Note. alternatively you can set a temporary env variable like this:\n# os.environ['OPENAI_API_KEY'] = 'your-key-goes-here'\n\nif os.getenv(\"OPENAI_API_KEY\") is not None:\n    print (\"OPENAI_API_KEY is ready\")\nelse:\n    print (\"OPENAI_API_KEY environment variable not found\")
```

----------------------------------------

TITLE: Completing the Context-Infused Query using LLM Completion in Python
DESCRIPTION: Executes the final language model completion step using the prompt assembled from retrieved contexts. The 'complete' function should encapsulate an API call to a language model, and expects 'query_with_contexts' as the full-prompt input. This concludes the pipeline by generating an answer based on both query and retrieved contexts.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Gen_QA.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
# then we complete the context-infused query
complete(query_with_contexts)
```

----------------------------------------

TITLE: Implementing Math Tutor Function with Structured Output
DESCRIPTION: Defines a function that uses OpenAI's API to generate step-by-step solutions for math problems, utilizing Structured Outputs to ensure a consistent response format.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Structured_Outputs_Intro.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
math_tutor_prompt = '''
    You are a helpful math tutor. You will be provided with a math problem,
    and your goal will be to output a step by step solution, along with a final answer.
    For each step, just provide the output as an equation use the explanation field to detail the reasoning.
'''

def get_math_solution(question):
    response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {
            "role": "system", 
            "content": dedent(math_tutor_prompt)
        },
        {
            "role": "user", 
            "content": question
        }
    ],
    response_format={
        "type": "json_schema",
        "json_schema": {
            "name": "math_reasoning",
            "schema": {
                "type": "object",
                "properties": {
                    "steps": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {
                                "explanation": {"type": "string"},
                                "output": {"type": "string"}
                            },
                            "required": ["explanation", "output"],
                            "additionalProperties": False
                        }
                    },
                    "final_answer": {"type": "string"}
                },
                "required": ["steps", "final_answer"],
                "additionalProperties": False
            },
            "strict": True
        }
    }
    )

    return response.choices[0].message
```

----------------------------------------

TITLE: Making a Basic Chat Completion API Request - Python
DESCRIPTION: This example demonstrates making a chat completion API call to start a simulated conversation. It creates a message list representing a short exchange and calls client.chat.completions.create() with temperature set to zero (deterministic output). Required parameters include the 'model' name and crafted 'messages'. Outputs a response object containing the model's reply and relevant metadata. Prerequisites: openai library initialized; SAMPLE API key configured.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Example OpenAI Python library request
MODEL = "gpt-3.5-turbo"
response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Knock knock."},
        {"role": "assistant", "content": "Who's there?"},
        {"role": "user", "content": "Orange."},
    ],
    temperature=0,
)

```

----------------------------------------

TITLE: Updating an Assistant with Code Interpreter, File Search, and Function Calling Tools (OpenAI Assistants API, Python)
DESCRIPTION: This snippet updates the Assistant to support all three tools: Code Interpreter, File Search, and a custom display_quiz function for function calling. Input: Assistant ID, function schema. Output: updated Assistant object capable of all listed tool types. Prerequisites: existing Assistant, function_json defined, previously configured tools' resources.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Assistants_API_overview_python.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
assistant = client.beta.assistants.update(
    MATH_ASSISTANT_ID,
    tools=[
        {"type": "code_interpreter"},
        {"type": "file_search"},
        {"type": "function", "function": function_json},
    ],
)
show_json(assistant)
```

----------------------------------------

TITLE: Composing Contextual OpenAI GPT QA Messages with Source Insertion (Python)
DESCRIPTION: This code defines functions to count tokens, select context from search, build a source-augmented prompt, and send a QA query to OpenAI's API. Requires 'tiktoken', OpenAI client, embeddings dataframe, and prepared functions from previous steps. Inputs are user queries, model selection, and optionally print flag. The output is a reliable, referenced answer from GPT, with options for debugging prompt construction.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Question_answering_using_embeddings.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
def num_tokens(text: str, model: str = GPT_MODELS[0]) -> int:
    """Return the number of tokens in a string."""
    encoding = tiktoken.encoding_for_model(model)
    return len(encoding.encode(text))


def query_message(
    query: str,
    df: pd.DataFrame,
    model: str,
    token_budget: int
) -> str:
    """Return a message for GPT, with relevant source texts pulled from a dataframe."""
    strings, relatednesses = strings_ranked_by_relatedness(query, df)
    introduction = 'Use the below articles on the 2022 Winter Olympics to answer the subsequent question. If the answer cannot be found in the articles, write "I could not find an answer."'
    question = f"\n\nQuestion: {query}"
    message = introduction
    for string in strings:
        next_article = f'\n\nWikipedia article section:\n"""\n{string}\n"""'
        if (
            num_tokens(message + next_article + question, model=model)
            > token_budget
        ):
            break
        else:
            message += next_article
    return message + question


def ask(
    query: str,
    df: pd.DataFrame = df,
    model: str = GPT_MODELS[0],
    token_budget: int = 4096 - 500,
    print_message: bool = False,
) -> str:
    """Answers a query using GPT and a dataframe of relevant texts and embeddings."""
    message = query_message(query, df, model=model, token_budget=token_budget)
    if print_message:
        print(message)
    messages = [
        {"role": "system", "content": "You answer questions about the 2022 Winter Olympics."},
        {"role": "user", "content": message},
    ]
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        temperature=0
    )
    response_message = response.choices[0].message.content
    return response_message

```

----------------------------------------

TITLE: Defining S3 Function Schema for ChatGPT Function Calling - Python
DESCRIPTION: Provides detailed schema definitions for S3-related operations that the ChatGPT API can interpret and invoke, such as listing buckets, uploading or downloading files, and searching objects. Each entry describes the function name, operation description, and expected parameters. This schema enables ChatGPT to map user instructions to specific AWS S3 tasks.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
# Functions dict to pass S3 operations details for the GPT model\nfunctions = [\n    {   \n        "type": "function",\n        "function":{\n            "name": "list_buckets",\n            "description": "List all available S3 buckets",\n            "parameters": {\n                "type": "object",\n                "properties": {}\n            }\n        }\n    },\n    {\n        "type": "function",\n        "function":{\n            "name": "list_objects",\n            "description": "List the objects or files inside a given S3 bucket",\n            "parameters": {\n                "type": "object",\n                "properties": {\n                    "bucket": {"type": "string", "description": "The name of the S3 bucket"},\n                    "prefix": {"type": "string", "description": "The folder path in the S3 bucket"},\n                },\n                "required": ["bucket"],\n            },\n        }\n    },\n    {   \n        "type": "function",\n        "function":{\n            "name": "download_file",\n            "description": "Download a specific file from an S3 bucket to a local distribution folder.",\n            "parameters": {\n                "type": "object",\n                "properties": {\n                    "bucket": {"type": "string", "description": "The name of the S3 bucket"},\n                    "key": {"type": "string", "description": "The path to the file inside the bucket"},\n                    "directory": {"type": "string", "description": "The local destination directory to download the file, should be specificed by the user."},\n                },\n                "required": ["bucket", "key", "directory"],\n            }\n        }\n    },\n    {\n        "type": "function",\n        "function":{\n            "name": "upload_file",\n            "description": "Upload a file to an S3 bucket",\n            "parameters": {\n                "type": "object",\n                "properties": {\n                    "source": {"type": "string", "description": "The local source path or remote URL"},\n                    "bucket": {"type": "string", "description": "The name of the S3 bucket"},\n                    "key": {"type": "string", "description": "The path to the file inside the bucket"},\n                    "is_remote_url": {"type": "boolean", "description": "Is the provided source a URL (True) or local path (False)"},\n                },\n                "required": ["source", "bucket", "key", "is_remote_url"],\n            }\n        }\n    },\n    {\n        "type": "function",\n        "function":{\n            "name": "search_s3_objects",\n            "description": "Search for a specific file name inside an S3 bucket",\n            "parameters": {\n                "type": "object",\n                "properties": {\n                    "search_name": {"type": "string", "description": "The name of the file you want to search for"},\n                    "bucket": {"type": "string", "description": "The name of the S3 bucket"},\n                    "prefix": {"type": "string", "description": "The folder path in the S3 bucket"},\n                    "exact_match": {"type": "boolean", "description": "Set exact_match to True if the search should match the exact file name. Set exact_match to False to compare part of the file name string (the file contains)"}\n                },\n                "required": ["search_name"],\n            },\n        }\n    }\n]
```

----------------------------------------

TITLE: Initialize OpenAI Client
DESCRIPTION: Imports the `openai` library and initializes the OpenAI client using an API key obtained from the environment variable `OPENAI_API_KEY`. Prints a status message indicating if the client was successfully initialized or if the API key was not found.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/chroma/hyde-with-chroma-and-openai.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import os
from openai import OpenAI

# Uncomment the following line to set the environment variable in the notebook
# os.environ["OPENAI_API_KEY"] = 'sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

api_key = os.getenv("OPENAI_API_KEY")

if api_key:
    client = OpenAI(api_key=api_key)
    print("OpenAI client is ready")
else:
    print("OPENAI_API_KEY environment variable not found")
```

----------------------------------------

TITLE: Initializing OpenAI Client and Importing Dependencies in Python
DESCRIPTION: Initializes the OpenAI client using an API key, which is either fetched from the OPENAI_API_KEY environment variable or provided directly as a fallback. The snippet also imports required Python libraries (json, openai, os, and requests) to support API operations. This code should be placed at the top of the script to provide necessary dependencies and client configuration for later interactions with OpenAI or HTTP requests.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Function_calling_finding_nearby_places.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import json
from openai import OpenAI
import os
import requests

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))

```

----------------------------------------

TITLE: Estimating Fine-tuning Costs Based on Token Count in Python
DESCRIPTION: Calculates the expected token usage and cost for fine-tuning based on dataset size. Determines the optimal number of training epochs, estimates billable tokens, and provides a cost estimate for the fine-tuning process.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Chat_finetuning_data_prep.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
# Pricing and default n_epochs estimate
MAX_TOKENS_PER_EXAMPLE = 16385

TARGET_EPOCHS = 3
MIN_TARGET_EXAMPLES = 100
MAX_TARGET_EXAMPLES = 25000
MIN_DEFAULT_EPOCHS = 1
MAX_DEFAULT_EPOCHS = 25

n_epochs = TARGET_EPOCHS
n_train_examples = len(dataset)
if n_train_examples * TARGET_EPOCHS < MIN_TARGET_EXAMPLES:
    n_epochs = min(MAX_DEFAULT_EPOCHS, MIN_TARGET_EXAMPLES // n_train_examples)
elif n_train_examples * TARGET_EPOCHS > MAX_TARGET_EXAMPLES:
    n_epochs = max(MIN_DEFAULT_EPOCHS, MAX_TARGET_EXAMPLES // n_train_examples)

n_billing_tokens_in_dataset = sum(min(MAX_TOKENS_PER_EXAMPLE, length) for length in convo_lens)
print(f"Dataset has ~{n_billing_tokens_in_dataset} tokens that will be charged for during training")
print(f"By default, you'll train for {n_epochs} epochs on this dataset")
print(f"By default, you'll be charged for ~{n_epochs * n_billing_tokens_in_dataset} tokens")
```

----------------------------------------

TITLE: Executing Asynchronous Input/Output Moderation with OpenAI in Python
DESCRIPTION: Demonstrates an asynchronous workflow that checks both user input and generated LLM responses against moderation rules using the OpenAI Moderation API before allowing them to proceed. The function runs input moderation and chat completion concurrently via asyncio, cancels chat processing if the input fails moderation, and ensures output moderation on the model response. Requires 'asyncio' and assumes functions 'check_moderation_flag' and 'get_chat_response' are defined elsewhere and interface correctly with the OpenAI API. Takes user input as a parameter and returns either an appropriate error/message or the moderated LLM response. Limitations: Partial error handling, must be used in an async context.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_use_moderation.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
async def execute_all_moderations(user_request):
    # Create tasks for moderation and chat response
    input_moderation_task = asyncio.create_task(check_moderation_flag(user_request))
    chat_task = asyncio.create_task(get_chat_response(user_request))

    while True:
        done, _ = await asyncio.wait(
            [input_moderation_task, chat_task], return_when=asyncio.FIRST_COMPLETED
        )

        # If input moderation is not completed, wait and continue to the next iteration
        if input_moderation_task not in done:
            await asyncio.sleep(0.1)
            continue

        # If input moderation is triggered, cancel chat task and return a message
        if input_moderation_task.result() == True:
            chat_task.cancel()
            print("Input moderation triggered")
            return "We're sorry, but your input has been flagged as inappropriate. Please rephrase your input and try again."

        # Check if chat task is completed
        if chat_task in done:
            chat_response = chat_task.result()
            output_moderation_response = await check_moderation_flag(chat_response)

            # Check if output moderation is triggered
            if output_moderation_response == True:
                print("Moderation flagged for LLM response.")
                return "Sorry, we're not permitted to give this answer. I can help you with any general queries you might have."
            
            print('Passed moderation')
            return chat_response

        # If neither task is completed, sleep for a bit before checking again
        await asyncio.sleep(0.1)

```

----------------------------------------

TITLE: Extracting the Assistant's Reply from a Chat Response - Python
DESCRIPTION: This short snippet shows how to extract only the generated message content from the response. It accesses 'response.choices[0].message.content', which is the assistant's reply to the most recent user message. Assumes a successful API call and that 'choices' contains at least one result. Outputs a string representing the assistant's message.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
response.choices[0].message.content

```

----------------------------------------

TITLE: Initializing OpenAI Client - Python
DESCRIPTION: Imports necessary libraries (openai, urllib, os) and initializes the OpenAI client using an API key from environment variables or a placeholder.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Whisper_prompting_guide.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI  # for making OpenAI API calls
import urllib  # for downloading example audio files
import os

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))
```

----------------------------------------

TITLE: Tokenizing Text and Checking Token Count with tiktoken in Python
DESCRIPTION: This snippet uses the tiktoken library to specify the encoding for the GPT-4 Turbo model and encodes the loaded text to obtain the token count. It helps determine if the document must be chunked further before sending to the GPT API, ensuring compliance with model context limits. The function requires the earlier import of tiktoken and the variable containing the loaded document.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Summarizing_long_documents.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# load encoding and check the length of dataset\nencoding = tiktoken.encoding_for_model('gpt-4-turbo')\nlen(encoding.encode(artificial_intelligence_wikipedia_text))
```

----------------------------------------

TITLE: Initializing OpenAI SDK in Node.js
DESCRIPTION: Sets up the OpenAI SDK with API authentication for browser-based usage. Includes a safety flag for client-side requests.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_an_agent_with_the_node_sdk.mdx#_snippet_0

LANGUAGE: javascript
CODE:
```
import OpenAI from "openai";

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
  dangerouslyAllowBrowser: true,
});
```

----------------------------------------

TITLE: Fallback to Secondary Model on Rate Limit Error - OpenAI Python
DESCRIPTION: This snippet defines a function that gracefully handles rate limit errors by retrying an OpenAI chat completion request with a different, fallback model when an error occurs. The input parameters are the primary and fallback model names and OpenAI completion request arguments. Requires an initialized OpenAI client and proper error handling with 'openai.RateLimitError'. Returns the completion result from either the primary or fallback model.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_handle_rate_limits.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
def completions_with_fallback(fallback_model, **kwargs):\n    try:\n        return client.chat.completions.create(**kwargs)\n    except openai.RateLimitError:\n        kwargs['model'] = fallback_model\n        return client.chat.completions.create(**kwargs)\n    \n    \ncompletions_with_fallback(fallback_model="gpt-4o", model="gpt-4o-mini", messages=[{"role": "user", "content": "Once upon a time,"}])
```

----------------------------------------

TITLE: Building Message Format for OpenAI Fine-Tuning - Python
DESCRIPTION: Joins the fine-tuning prep DataFrame with the class mapping and adds a column 'messages' that formats user prompts and expected assistant responses for each sample. The column is intended for OpenAI fine-tuning. Requires access to the 'format_prompt' function from earlier steps.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Multiclass_classification_for_transactions.ipynb#_snippet_18

LANGUAGE: python
CODE:
```
ft_df_with_class = ft_prep_df.merge(class_df,left_on='Classification',right_on='class',how='inner')

# Creating a list of messages for the fine-tuning job. The user message is the prompt, and the assistant message is the response from the model
ft_df_with_class['messages'] = ft_df_with_class.apply(lambda x: [{"role": "user", "content": format_prompt(x)}, {"role": "assistant", "content": x['class']}],axis=1)
ft_df_with_class[['messages', 'class']].head()

```

----------------------------------------

TITLE: Implementing Custom Moderation with GPT-4 via OpenAI Python SDK
DESCRIPTION: Defines a 'custom_moderation' function that leverages GPT-4 (gpt-4o-mini) to evaluate content for appropriateness based on user-supplied parameters. Builds a moderation prompt, calls the GPT model via OpenAI's SDK for chat completions with JSON response format, and parses the assessment from the returned message. Dependencies: assumes configured OpenAI Python SDK ('client'), API access to 'gpt-4o-mini', and proper API authentication. Inputs are the content to check and a string of moderation parameters; output is a JSON string assessment. Useful for building parameterized, adaptive guardrails outside OpenAI's standard moderation API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_use_moderation.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
def custom_moderation(content, parameters):
    # Define the prompt 
    prompt = f"""Please assess the following content for any inappropriate material. You should base your assessment on the given parameters.
    Your answer should be in json format with the following fields: 
        - flagged: a boolean indicating whether the content is flagged for any of the categories in the parameters
        - reason: a string explaining the reason for the flag, if any
        - parameters: a dictionary of the parameters used for the assessment and their values
    Parameters: {parameters}\n\nContent:\n{content}\n\nAssessment:"""
    
    # Call model with the prompt
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        response_format={ "type": "json_object" },
        messages=[
            {"role": "system", "content": "You are a content moderation assistant."},
            {"role": "user", "content": prompt}
        ]
    )
    
    # Extract the assessment from the response
    assessment = response.choices[0].message.content
    
    return assessment

```

----------------------------------------

TITLE: Converting Python Functions to OpenAI Function Schemas - Python
DESCRIPTION: Defines 'function_to_schema' helper that inspects a Python function's signature, docstring, and type annotations, producing a schema suitable for OpenAI tool API integration. Handles mapping of basic Python types to OpenAI parameter types and collects required parameters. Requires the 'inspect' and 'json' modules in Python.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
import inspect

def function_to_schema(func) -> dict:
    type_map = {
        str: "string",
        int: "integer",
        float: "number",
        bool: "boolean",
        list: "array",
        dict: "object",
        type(None): "null",
    }

    try:
        signature = inspect.signature(func)
    except ValueError as e:
        raise ValueError(
            f"Failed to get signature for function {func.__name__}: {str(e)}"
        )

    parameters = {}
    for param in signature.parameters.values():
        try:
            param_type = type_map.get(param.annotation, "string")
        except KeyError as e:
            raise KeyError(
                f"Unknown type annotation {param.annotation} for parameter {param.name}: {str(e)}"
            )
        parameters[param.name] = {"type": param_type}

    required = [
        param.name
        for param in signature.parameters.values()
        if param.default == inspect._empty
    ]

    return {
        "type": "function",
        "function": {
            "name": func.__name__,
            "description": (func.__doc__ or "").strip(),
            "parameters": {
                "type": "object",
                "properties": parameters,
                "required": required,
            },
        },
    }
```

----------------------------------------

TITLE: Process Function Call and Make Follow-up API Call
DESCRIPTION: Appends the model's initial response (including the reasoning item) to the conversation context. It then extracts the function call details, executes the local `get_weather` function, adds the function output to the context, and makes a second API call with the updated context to allow the model to generate a final response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/reasoning_items.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
context += response.output # Add the response to the context (including the reasoning item)

tool_call = response.output[1]
args = json.loads(tool_call.arguments)


# calling the function
result = get_weather(args["latitude"], args["longitude"])

context.append({
    "type": "function_call_output",
    "call_id": tool_call.call_id,
    "output": str(result)
})

# we are calling the api again with the added function call output. Note that while this is another API call, we consider this as a single turn in the conversation.
response_2 = client.responses.create(
    model="o4-mini",
    input=context,
    tools=tools,
)

print(response_2.output_text)
```

----------------------------------------

TITLE: Comparing different tokenizer encodings
DESCRIPTION: A function to compare how different encodings (r50k_base, p50k_base, cl100k_base, o200k_base) tokenize the same string, showing their token counts and representations.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
def compare_encodings(example_string: str) -> None:
    """Prints a comparison of three string encodings."""
    # print the example string
    print(f'\nExample string: "{example_string}"')
    # for each encoding, print the # of tokens, the token integers, and the token bytes
    for encoding_name in ["r50k_base", "p50k_base", "cl100k_base", "o200k_base"]:
        encoding = tiktoken.get_encoding(encoding_name)
        token_integers = encoding.encode(example_string)
        num_tokens = len(token_integers)
        token_bytes = [encoding.decode_single_token_bytes(token) for token in token_integers]
        print()
        print(f"{encoding_name}: {num_tokens} tokens")
        print(f"token integers: {token_integers}")
        print(f"token bytes: {token_bytes}")
```

----------------------------------------

TITLE: Performing Semantic Search with OpenAI Embeddings and MyScale
DESCRIPTION: Demonstrates a semantic search by embedding a query using OpenAI's API, then using the resulting vector to find similar content in the MyScale database based on cosine distance.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/myscale/Using_MyScale_for_embeddings_search.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
query = "Famous battles in Scottish history"

# creates embedding vector from user query
embed = openai.Embedding.create(
    input=query,
    model="text-embedding-3-small",
)["data"][0]["embedding"]

# query the database to find the top K similar content to the given query
top_k = 10
results = client.query(f"""
SELECT id, url, title, distance(content_vector, {embed}) as dist
FROM default.articles
ORDER BY dist
LIMIT {top_k}
""")

# display results
for i, r in enumerate(results.named_results()):
    print(i+1, r['title'])
```

----------------------------------------

TITLE: Creating a Thread and Run to Generate Fibonacci Numbers (OpenAI Assistants API, Python)
DESCRIPTION: This snippet creates a new thread and run with a prompt that asks the Assistant to generate the first 20 Fibonacci numbers using code, then waits for the run to complete and prints the response. Requires prior Assistant setup with the Code Interpreter enabled and availability of helper functions for thread and run management. Input: Prompt string. Output: Assistant's response with generated numbers.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Assistants_API_overview_python.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
thread, run = create_thread_and_run(
    "Generate the first 20 fibbonaci numbers with code."
)
run = wait_on_run(run, thread)
pretty_print(get_response(thread))
```

----------------------------------------

TITLE: Defining the QA Chain with VectorDBQA and OpenAI - Python
DESCRIPTION: Configures a question answering chain with Langchain using the OpenAI LLM and the previously created vectorstore. Sets `chain_type` to 'stuff' (for condensed context prompt inclusion) and disables returning source documents. Requires properly initialized `llm` and `doc_store` objects.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/analyticdb/QA_with_Langchain_AnalyticDB_and_OpenAI.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
from langchain.chains import RetrievalQA

llm = OpenAI()
qa = VectorDBQA.from_chain_type(
    llm=llm,
    chain_type=\"stuff\",
    vectorstore=doc_store,
    return_source_documents=False,
)
```

----------------------------------------

TITLE: Initializing OpenAI Client (Python)
DESCRIPTION: Imports the necessary libraries (`openai`, `os`) and initializes the OpenAI client object. This client is used for interacting with the OpenAI API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/use-cases/responses-evaluation.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
import openai
import os


client = openai.OpenAI()
```

----------------------------------------

TITLE: Counting tokens for chat completions with tool calls - Python
DESCRIPTION: This code defines a function for estimating the total token usage when chat completions involve tool calls (functions). The function, `num_tokens_for_tools`, accounts for intricate tokenization rules of various OpenAI chat models by calculating tokens for tool definitions and their properties, including enums. It uses the `tiktoken` package for encoding and depends on the structure of the functions/tools and messages as well as the OpenAI model name. The main input parameters are the functions (tools), messages, and target model name. Outputs include the total token count required for the messages and tools. Limitations include that not all models are supported and outputs may differ if encoding or schema expectations change.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb#_snippet_15

LANGUAGE: python
CODE:
```
def num_tokens_for_tools(functions, messages, model):
    
    # Initialize function settings to 0
    func_init = 0
    prop_init = 0
    prop_key = 0
    enum_init = 0
    enum_item = 0
    func_end = 0
    
    if model in [
        "gpt-4o",
        "gpt-4o-mini"
    ]:
        
        # Set function settings for the above models
        func_init = 7
        prop_init = 3
        prop_key = 3
        enum_init = -3
        enum_item = 3
        func_end = 12
    elif model in [
        "gpt-3.5-turbo",
        "gpt-4"
    ]:
        # Set function settings for the above models
        func_init = 10
        prop_init = 3
        prop_key = 3
        enum_init = -3
        enum_item = 3
        func_end = 12
    else:
        raise NotImplementedError(
            f"""num_tokens_for_tools() is not implemented for model {model}."""
        )
    
    try:
        encoding = tiktoken.encoding_for_model(model)
    except KeyError:
        print("Warning: model not found. Using o200k_base encoding.")
        encoding = tiktoken.get_encoding("o200k_base")
    
    func_token_count = 0
    if len(functions) > 0:
        for f in functions:
            func_token_count += func_init  # Add tokens for start of each function
            function = f["function"]
            f_name = function["name"]
            f_desc = function["description"]
            if f_desc.endswith("."):
                f_desc = f_desc[:-1]
            line = f_name + ":" + f_desc
            func_token_count += len(encoding.encode(line))  # Add tokens for set name and description
            if len(function["parameters"]["properties"]) > 0:
                func_token_count += prop_init  # Add tokens for start of each property
                for key in list(function["parameters"]["properties"].keys()):
                    func_token_count += prop_key  # Add tokens for each set property
                    p_name = key
                    p_type = function["parameters"]["properties"][key]["type"]
                    p_desc = function["parameters"]["properties"][key]["description"]
                    if "enum" in function["parameters"]["properties"][key].keys():
                        func_token_count += enum_init  # Add tokens if property has enum list
                        for item in function["parameters"]["properties"][key]["enum"]:
                            func_token_count += enum_item
                            func_token_count += len(encoding.encode(item))
                    if p_desc.endswith("."):
                        p_desc = p_desc[:-1]
                    line = f"{p_name}:{p_type}:{p_desc}"
                    func_token_count += len(encoding.encode(line))
        func_token_count += func_end
        
    messages_token_count = num_tokens_from_messages(messages, model)
    total_tokens = messages_token_count + func_token_count
    
    return total_tokens
```

----------------------------------------

TITLE: Making ChatCompletion API Request (Python)
DESCRIPTION: This function sends a request to the OpenAI ChatCompletion API. It includes retry logic for robustness and handles potential exceptions during the API call. It takes messages, optional functions, and a model name as input and returns the API response or an error.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_for_knowledge_retrieval.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
@retry(wait=wait_random_exponential(min=1, max=40), stop=stop_after_attempt(3))
def chat_completion_request(messages, functions=None, model=GPT_MODEL):
    try:
        response = client.chat.completions.create(
            model=model,
            messages=messages,
            functions=functions,
        )
        return response
    except Exception as e:
        print("Unable to generate ChatCompletion response")
        print(f"Exception: {e}")
        return e
```

----------------------------------------

TITLE: Setting up Message History Array
DESCRIPTION: Initializes the message history array with a system message that defines the AI assistant's behavior.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_an_agent_with_the_node_sdk.mdx#_snippet_4

LANGUAGE: javascript
CODE:
```
const messages = [
  {
    role: "system",
    content:
      "You are a helpful assistant. Only use the functions you have been provided with.",
  },
];
```

----------------------------------------

TITLE: Defining Python Tool for Code, Terminal, and Patching
DESCRIPTION: This Python dictionary defines the structure and parameters for a tool named 'python'. It specifies the tool's type as 'function', provides a detailed description of its capabilities (executing Python, terminal commands, and applying patches), and defines the 'input' parameter as a required string.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/gpt4-1_prompting_guide.ipynb#_snippet_7

LANGUAGE: Python
CODE:
```
python_bash_patch_tool = {
  "type": "function",
  "name": "python",
  "description": PYTHON_TOOL_DESCRIPTION,
  "parameters": {
      "type": "object",
      "strict": True,
      "properties": {
          "input": {
              "type": "string",
              "description": " The Python code, terminal command (prefaced by exclamation mark), or apply_patch command that you wish to execute.",
          }
      },
      "required": ["input"],
  },
}
```

----------------------------------------

TITLE: Configuring GPT Instructions for Workday HR Automation - Markdown
DESCRIPTION: This Markdown code snippet contains the instructions used as prompt guidance for a GPT agent that automates HR processes on Workday. The instructions define the context for supporting PTO submission, retrieving worker details, and inquiring about benefit plans. Each scenario is broken down into stepwise guidance for question/answer style exchanges, referencing API calls such as Request_Time_Off and Get_Report_As_A_Service. Inputs required from users (e.g., PTO start date, end date) are highlighted and the typical API workflow is mapped out. To utilize this, paste the snippet into a GPT agent configuration as a prompt template. No additional dependencies are required.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_workday.md#_snippet_0

LANGUAGE: markdown
CODE:
```
# **Context:** You support employees by providing detailed information about their PTO submissions, worker details, and benefit plans through the Workday system. You help them submit PTO requests, retrieve personal and job-related information, and view their benefit plans. Assume the employees are familiar with basic HR terminologies.
# **Instructions:**
## Scenarios
### - When the user asks to submit a PTO request, follow this 3 step process:
1. Ask the user for PTO details, including start date, end date, and type of leave.
2. Submit the request using the `Request_Time_Off` API call.
3. Provide a summary of the submitted PTO request, including any information on approvals.

### - When the user asks to retrieve worker details, follow this 2 step process:
1. Retrieve the worker’s details using `Get_Workers`.
2. Summarize the employee’s job title, department, and contact details for easy reference.

### - When the user asks to inquire about benefit plans, follow this 2 step process:
1. Retrieve benefit plan details using `Get_Report_As_A_Service`.
2. Present a summary of the benefits.
```

----------------------------------------

TITLE: Converting PDFs to Base64-Encoded Images with PyMuPDF and PIL in Python
DESCRIPTION: These functions handle reading multipage PDF files, converting each page to a PNG using PyMuPDF and Pillow, saving temporaries, and encoding each image as base64 strings for further processing. Required dependencies include fitz (PyMuPDF), PIL, and base64. The core methods include a static method to encode images and a PDF-to-base64-images converter, which returns an array of base64-encoded images for downstream use.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Data_extraction_transformation.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI
import fitz  # PyMuPDF
import io
import os
from PIL import Image
import base64
import json

api_key = os.getenv("OPENAI_API_KEY")
client = OpenAI(api_key=api_key)


@staticmethod
def encode_image(image_path):
    with open(image_path, "rb") as image_file:
        return base64.b64encode(image_file.read()).decode("utf-8")


def pdf_to_base64_images(pdf_path):
    #Handles PDFs with multiple pages
    pdf_document = fitz.open(pdf_path)
    base64_images = []
    temp_image_paths = []

    total_pages = len(pdf_document)

    for page_num in range(total_pages):
        page = pdf_document.load_page(page_num)
        pix = page.get_pixmap()
        img = Image.open(io.BytesIO(pix.tobytes()))
        temp_image_path = f"temp_page_{page_num}.png"
        img.save(temp_image_path, format="PNG")
        temp_image_paths.append(temp_image_path)
        base64_image = encode_image(temp_image_path)
        base64_images.append(base64_image)

    for temp_image_path in temp_image_paths:
        os.remove(temp_image_path)

    return base64_images
```

----------------------------------------

TITLE: Setting Up OpenAI Assistants API Client and Helpers - Python
DESCRIPTION: Initializes packages and the OpenAI API client using an API key from the environment. Includes helper functions for displaying JSON, submitting user messages to Assistants, and retrieving message responses. Dependencies include openai, IPython, PIL, pandas, and other standard libraries. Functions expect thread and assistant objects created later and are required for streamlined Assistant interactions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Creating_slides_with_Assistants_API_and_DALL-E3.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from IPython.display import display, Image
from openai import OpenAI
import os
import pandas as pd
import json
import io
from PIL import Image
import requests

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))

#Lets import some helper functions for assistants from https://cookbook.openai.com/examples/assistants_api_overview_python
def show_json(obj):
    display(json.loads(obj.model_dump_json()))

def submit_message(assistant_id, thread, user_message,file_ids=None):
    params = {
        'thread_id': thread.id,
        'role': 'user',
        'content': user_message,
    }
    if file_ids:
        params['file_ids']=file_ids

    client.beta.threads.messages.create(
        **params
)
    return client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant_id,
)

def get_response(thread):
    return client.beta.threads.messages.list(thread_id=thread.id)

```

----------------------------------------

TITLE: Processing Model Response and Executing Function Call in Python
DESCRIPTION: This snippet handles the model's response, checks for function calls, executes the specified function (in this case, querying the database), and processes the results. It demonstrates the complete workflow of function calling with the Chat Completions API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_19

LANGUAGE: python
CODE:
```
tool_calls = response_message.tool_calls
if tool_calls:
    tool_call_id = tool_calls[0].id
    tool_function_name = tool_calls[0].function.name
    tool_query_string = json.loads(tool_calls[0].function.arguments)['query']

    if tool_function_name == 'ask_database':
        results = ask_database(conn, tool_query_string)
        
        messages.append({
            "role":"tool", 
            "tool_call_id":tool_call_id, 
            "name": tool_function_name, 
            "content":results
        })
        
        model_response_with_function_call = client.chat.completions.create(
            model="gpt-4o",
            messages=messages,
        )
        print(model_response_with_function_call.choices[0].message.content)
    else: 
        print(f"Error: function {tool_function_name} does not exist")
else: 
    print(response_message.content)
```

----------------------------------------

TITLE: Generating and Chunking Embeddings Using OpenAI API in Python
DESCRIPTION: Provides two functions: generate_embeddings generates vector embeddings for given text using a specified OpenAI model; len_safe_get_embedding splits long texts into chunks, embeds each chunk, and returns both chunked embeddings and their decoded string representations. Dependencies include the openai-client, tiktoken, and the previously defined chunked_tokens function. Key parameters: text to embed, model, max_tokens (chunk size), and encoding name. Functions return embeddings and original text for each processed chunk; must handle token length constraints.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/rag-quickstart/azure/Azure_AI_Search_with_Azure_Functions_and_GPT_Actions_in_ChatGPT.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
def generate_embeddings(text, model):\n    # Generate embeddings for the provided text using the specified model\n    embeddings_response = openai_client.embeddings.create(model=model, input=text)\n    # Extract the embedding data from the response\n    embedding = embeddings_response.data[0].embedding\n    return embedding\n\ndef len_safe_get_embedding(text, model=embeddings_model, max_tokens=EMBEDDING_CTX_LENGTH, encoding_name=EMBEDDING_ENCODING):\n    # Initialize lists to store embeddings and corresponding text chunks\n    chunk_embeddings = []\n    chunk_texts = []\n    # Iterate over chunks of tokens from the input text\n    for chunk in chunked_tokens(text, chunk_length=max_tokens, encoding_name=encoding_name):\n        # Generate embeddings for each chunk and append to the list\n        chunk_embeddings.append(generate_embeddings(chunk, model=model))\n        # Decode the chunk back to text and append to the list\n        chunk_texts.append(tiktoken.get_encoding(encoding_name).decode(chunk))\n    # Return the list of chunk embeddings and the corresponding text chunks\n    return chunk_embeddings, chunk_texts\n
```

----------------------------------------

TITLE: Executing Conversations with OpenAI GPT via Python Function
DESCRIPTION: Defines a function to manage an automated conversation flow with an LLM, accepting an 'objective' string input. The function loops until an explicit termination condition is met, maintaining chat history, formatting messages for the system and user, and building appropriate prompts for the GPT API. It depends on pre-existing definitions for 'submit_user_message', 'customer_system_prompt', 'client', and the GPT model specifics. Inputs include user objectives and conversation history; outputs are updated chat logs and eventual program print statements when objectives are met. Special care is taken to construct and iterate over formatted messages.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Using_tool_required_for_customer_service.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
def execute_conversation(objective):\n\n    conversation_messages = []\n\n    done = False\n\n    user_query = objective\n\n    while done is False:\n\n        conversation_messages = submit_user_message(user_query,conversation_messages)\n\n        messages_string = ''\n        for x in conversation_messages:\n            if isinstance(x,dict):\n                if x['role'] == 'user':\n                    messages_string += 'User: ' + x['content'] + '\\n'\n                elif x['role'] == 'tool':\n                    if x['name'] == 'speak_to_user':\n                        messages_string += 'Assistant: ' + x['content'] + '\\n'\n            else:\n                continue\n\n        messages = [\n            {\n            "role": "system",\n            "content": customer_system_prompt.format(query=objective,chat_history=messages_string)\n            },\n            {\n            "role": "user",\n            "content": "Continue the chat to solve your query. Remember, you are in the user in this exchange. Do not provide User: or Assistant: in your response"\n            }\n        ]\n\n        user_response = client.chat.completions.create(model=GPT_MODEL,messages=messages,temperature=0.5)\n\n        conversation_messages.append({\n            "role": "user",\n            "content": user_response.choices[0].message.content\n            })\n\n        if 'DONE' in user_response.choices[0].message.content:\n            done = True\n            print("Achieved objective, closing conversation\\n\\n")\n\n        else:\n            user_query = user_response.choices[0].message.content
```

----------------------------------------

TITLE: Authenticating with OpenAI API
DESCRIPTION: Configures OpenAI API authentication by setting the API key as an environment variable. Includes validation to ensure the key has the correct format.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/Openai_monitoring_with_wandb_weave.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
# authenticate with OpenAI
from getpass import getpass

if os.getenv("OPENAI_API_KEY") is None:
  os.environ["OPENAI_API_KEY"] = getpass("Paste your OpenAI key from: https://platform.openai.com/account/api-keys\n")
assert os.getenv("OPENAI_API_KEY", "").startswith("sk-"), "This doesn't look like a valid OpenAI API key"
print("OpenAI API key configured")
```

----------------------------------------

TITLE: Initializing Tool Metadata for Data Processing Agents in Python
DESCRIPTION: Initializes structured metadata for various tools (triage, preprocessing, analysis, and visualization) as Python lists of dictionaries. Each dictionary specifies a function, its description, and the parameters required. This setup is essential for dynamic tool discovery and invocation by agent systems, and it enforces input contract validation based on defined schemas. Dependencies include a Python runtime and may require integration into a larger multi-agent orchestration framework. All parameters and their expected data types and relationships are explicitly defined within each tool entry.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Structured_outputs_multi_agent.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
triage_tools = [
    {
        "type": "function",
        "function": {
            "name": "send_query_to_agents",
            "description": "Sends the user query to relevant agents based on their capabilities.",
            "parameters": {
                "type": "object",
                "properties": {
                    "agents": {
                        "type": "array",
                        "items": {"type": "string"},
                        "description": "An array of agent names to send the query to."
                    },
                    "query": {
                        "type": "string",
                        "description": "The user query to send."
                    }
                },
                "required": ["agents", "query"]
            }
        },
        "strict": True
    }
]

preprocess_tools = [
    {
        "type": "function",
        "function": {
            "name": "clean_data",
            "description": "Cleans the provided data by removing duplicates and handling missing values.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The dataset to clean. Should be in a suitable format such as JSON or CSV."
                    }
                },
                "required": ["data"],
                "additionalProperties": False
            }
        },
        "strict": True
    },
    {
        "type": "function",
        "function": {
            "name": "transform_data",
            "description": "Transforms data based on specified rules.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The data to transform. Should be in a suitable format such as JSON or CSV."
                    },
                    "rules": {
                        "type": "string",
                        "description": "Transformation rules to apply, specified in a structured format."
                    }
                },
                "required": ["data", "rules"],
                "additionalProperties": False
            }
        },
        "strict": True

    },
    {
        "type": "function",
        "function": {
            "name": "aggregate_data",
            "description": "Aggregates data by specified columns and operations.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The data to aggregate. Should be in a suitable format such as JSON or CSV."
                    },
                    "group_by": {
                        "type": "array",
                        "items": {"type": "string"},
                        "description": "Columns to group by."
                    },
                    "operations": {
                        "type": "string",
                        "description": "Aggregation operations to perform, specified in a structured format."
                    }
                },
                "required": ["data", "group_by", "operations"],
                "additionalProperties": False
            }
        },
        "strict": True
    }
]


analysis_tools = [
    {
        "type": "function",
        "function": {
            "name": "stat_analysis",
            "description": "Performs statistical analysis on the given dataset.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The dataset to analyze. Should be in a suitable format such as JSON or CSV."
                    }
                },
                "required": ["data"],
                "additionalProperties": False
            }
        },
        "strict": True
    },
    {
        "type": "function",
        "function": {
            "name": "correlation_analysis",
            "description": "Calculates correlation coefficients between variables in the dataset.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The dataset to analyze. Should be in a suitable format such as JSON or CSV."
                    },
                    "variables": {
                        "type": "array",
                        "items": {"type": "string"},
                        "description": "List of variables to calculate correlations for."
                    }
                },
                "required": ["data", "variables"],
                "additionalProperties": False
            }
        },
        "strict": True
    },
    {
        "type": "function",
        "function": {
            "name": "regression_analysis",
            "description": "Performs regression analysis on the dataset.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The dataset to analyze. Should be in a suitable format such as JSON or CSV."
                    },
                    "dependent_var": {
                        "type": "string",
                        "description": "The dependent variable for regression."
                    },
                    "independent_vars": {
                        "type": "array",
                        "items": {"type": "string"},
                        "description": "List of independent variables."
                    }
                },
                "required": ["data", "dependent_var", "independent_vars"],
                "additionalProperties": False
            }
        },
        "strict": True
    }
]

visualization_tools = [
    {
        "type": "function",
        "function": {
            "name": "create_bar_chart",
            "description": "Creates a bar chart from the provided data.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The data for the bar chart. Should be in a suitable format such as JSON or CSV."
                    },
                    "x": {
                        "type": "string",
                        "description": "Column for the x-axis."
                    },
                    "y": {
                        "type": "string",
                        "description": "Column for the y-axis."
                    }
                },
                "required": ["data", "x", "y"],
                "additionalProperties": False
            }
        },
        "strict": True
    },
    {
        "type": "function",
        "function": {
            "name": "create_line_chart",
            "description": "Creates a line chart from the provided data.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The data for the line chart. Should be in a suitable format such as JSON or CSV."
                    },
                    "x": {
                        "type": "string",
                        "description": "Column for the x-axis."
                    },
                    "y": {
                        "type": "string",
                        "description": "Column for the y-axis."
                    }
                },
                "required": ["data", "x", "y"],
                "additionalProperties": False
            }
        },
        "strict": True
    },
    {
        "type": "function",
        "function": {
            "name": "create_pie_chart",
            "description": "Creates a pie chart from the provided data.",
            "parameters": {
                "type": "object",
                "properties": {
                    "data": {
                        "type": "string",
                        "description": "The data for the pie chart. Should be in a suitable format such as JSON or CSV."
                    },
                    "labels": {
                        "type": "string",
                        "description": "Column for the labels."
                    },
                    "values": {
                        "type": "string",
                        "description": "Column for the values."
                    }
                },
                "required": ["data", "labels", "values"],
                "additionalProperties": False
            }
        },
        "strict": True
    }
]

```

----------------------------------------

TITLE: Creating a Custom QA Chain with Langchain and Custom Prompt in Python
DESCRIPTION: This code creates a VectorDBQA chain using the 'stuff' prompt type, but adds a custom PromptTemplate for modifying the LLM's answer behavior. It configures the underlying LLM, Tair vectorstore, and ensures that the custom prompt is used for Q&A interaction. This is useful for richer or constrained answer patterns.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/tair/QA_with_Langchain_Tair_and_OpenAI.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
custom_qa = VectorDBQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    vectorstore=doc_store,
    return_source_documents=False,
    chain_type_kwargs={"prompt": custom_prompt_template},
)
```

----------------------------------------

TITLE: Generate DataFrame Embeddings - Python
DESCRIPTION: This function takes a pandas DataFrame and a column name, extracts the text from the specified column, calls `embed_corpus` to generate embeddings, and adds the resulting embeddings as a new column named 'embeddings' to the DataFrame.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_combine_GPT4o_with_RAG_Outfit_Assistant.ipynb#_snippet_6

LANGUAGE: Python
CODE:
```
def generate_embeddings(df, column_name):
    # Initialize an empty list to store embeddings
    descriptions = df[column_name].astype(str).tolist()
    embeddings = embed_corpus(descriptions)

    # Add the embeddings as a new column to the DataFrame
    df['embeddings'] = embeddings
    print("Embeddings created successfully.")
```

----------------------------------------

TITLE: Chunking and Tokenizing Large Documents for Summarization in Python
DESCRIPTION: This code defines helper functions for tokenizing text and dividing it into chunks based on a maximum token count and a specified delimiter. It uses tiktoken for encoding and assumes the existence of a 'combine_chunks_with_no_minimum' utility for assembling compliant chunks. Key parameters include the input text, max_tokens, and delimiter. It warns if text overflows context limits and formats output accordingly; dependencies include tiktoken and the unspecified chunk-combining utility.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Summarizing_long_documents.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
def tokenize(text: str) -> List[str]:\n    encoding = tiktoken.encoding_for_model('gpt-4-turbo')\n    return encoding.encode(text)\n\n\n# This function chunks a text into smaller pieces based on a maximum token count and a delimiter.\ndef chunk_on_delimiter(input_string: str,\n                       max_tokens: int, delimiter: str) -> List[str]:\n    chunks = input_string.split(delimiter)\n    combined_chunks, _, dropped_chunk_count = combine_chunks_with_no_minimum(\n        chunks, max_tokens, chunk_delimiter=delimiter, add_ellipsis_for_overflow=True\n    )\n    if dropped_chunk_count > 0:\n        print(f\"warning: {dropped_chunk_count} chunks were dropped due to overflow\")\n    combined_chunks = [f\"{chunk}{delimiter}\" for chunk in combined_chunks]\n    return combined_chunks
```

----------------------------------------

TITLE: Flagging Visual Content and Generating Embeddings with OpenAI in Python
DESCRIPTION: This code adds a flag for visual elements based on markers in extracted text, then generates semantic embeddings for each page's text via OpenAI's Embedding API. Prerequisites include an initialized OpenAI client, pandas DataFrame with 'PageText', and tqdm for progress tracking. The flag column 'Visual_Input_Processed' is heuristically set. The embeddings column contains a list of floats of size 3072. Outputs are added in-place to the DataFrame; page-level logic may need adjustment for large documents.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Using_vision_modality_for_RAG_with_Pinecone.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
# Add a column to flag pages with visual content
df['Visual_Input_Processed'] = df['PageText'].apply(
    lambda x: 'Y' if 'DESCRIPTION OF THE IMAGE OR CHART' in x or 'TRANSCRIPTION OF THE TABLE' in x else 'N'
)


# Function to get embeddings
def get_embedding(text_input):
    response = oai_client.embeddings.create(
        input=text_input,
        model="text-embedding-3-large"
    )
    return response.data[0].embedding


# Generate embeddings with a progress bar
embeddings = []
for text in tqdm(df['PageText'], desc='Generating Embeddings'):
    embedding = get_embedding(text)
    embeddings.append(embedding)

# Add the embeddings to the DataFrame
df['Embeddings'] = embeddings

```

----------------------------------------

TITLE: Generating Unit Tests with Multi-Step GPT Prompts in Python
DESCRIPTION: This comprehensive function, unit_tests_from_function, takes a Python function as a string and returns a full unit test suite generated via a 3-step GPT prompting workflow: explanation, planning, and execution. It incorporates advanced logic such as conditional elaboration based on output length, output streaming, and re-parsing on failure using the ast module. Dependencies include the OpenAI Python SDK, access to a GPT API key, and (optionally) the pytest package for generated tests.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Unit_test_writing_using_a_multi-step_prompt.ipynb#_snippet_4

LANGUAGE: Python
CODE:
```
# example of a function that uses a multi-step prompt to write unit tests\ndef unit_tests_from_function(\n    function_to_test: str,  # Python function to test, as a string\n    unit_test_package: str = "pytest",  # unit testing package; use the name as it appears in the import statement\n    approx_min_cases_to_cover: int = 7,  # minimum number of test case categories to cover (approximate)\n    print_text: bool = False,  # optionally prints text; helpful for understanding the function & debugging\n    explain_model: str = "gpt-3.5-turbo",  # model used to generate text plans in step 1\n    plan_model: str = "gpt-3.5-turbo",  # model used to generate text plans in steps 2 and 2b\n    execute_model: str = "gpt-3.5-turbo",  # model used to generate code in step 3\n    temperature: float = 0.4,  # temperature = 0 can sometimes get stuck in repetitive loops, so we use 0.4\n    reruns_if_fail: int = 1,  # if the output code cannot be parsed, this will re-run the function up to N times\n) -> str:\n    """Returns a unit test for a given Python function, using a 3-step GPT prompt."""\n\n    # Step 1: Generate an explanation of the function\n\n    # create a markdown-formatted message that asks GPT to explain the function, formatted as a bullet list\n    explain_system_message = {\n        "role": "system",\n        "content": "You are a world-class Python developer with an eagle eye for unintended bugs and edge cases. You carefully explain code with great detail and accuracy. You organize your explanations in markdown-formatted, bulleted lists.",\n    }\n    explain_user_message = {\n        "role": "user",\n        "content": f"""Please explain the following Python function. Review what each element of the function is doing precisely and what the author's intentions may have been. Organize your explanation as a markdown-formatted, bulleted list.\n\n```python\n{function_to_test}\n```""",\n    }\n    explain_messages = [explain_system_message, explain_user_message]\n    if print_text:\n        print_messages(explain_messages)\n\n    explanation_response = client.chat.completions.create(model=explain_model,\n    messages=explain_messages,\n    temperature=temperature,\n    stream=True)\n    explanation = ""\n    for chunk in explanation_response:\n        delta = chunk.choices[0].delta\n        if print_text:\n            print_message_delta(delta)\n        if "content" in delta:\n            explanation += delta.content\n    explain_assistant_message = {"role": "assistant", "content": explanation}\n\n    # Step 2: Generate a plan to write a unit test\n\n    # Asks GPT to plan out cases the units tests should cover, formatted as a bullet list\n    plan_user_message = {\n        "role": "user",\n        "content": f"""A good unit test suite should aim to:\n- Test the function's behavior for a wide range of possible inputs\n- Test edge cases that the author may not have foreseen\n- Take advantage of the features of `{unit_test_package}` to make the tests easy to write and maintain\n- Be easy to read and understand, with clean code and descriptive names\n- Be deterministic, so that the tests always pass or fail in the same way\n\nTo help unit test the function above, list diverse scenarios that the function should be able to handle (and under each scenario, include a few examples as sub-bullets).""",\n    }\n    plan_messages = [\n        explain_system_message,\n        explain_user_message,\n        explain_assistant_message,\n        plan_user_message,\n    ]\n    if print_text:\n        print_messages([plan_user_message])\n    plan_response = client.chat.completions.create(model=plan_model,\n    messages=plan_messages,\n    temperature=temperature,\n    stream=True)\n    plan = ""\n    for chunk in plan_response:\n        delta = chunk.choices[0].delta\n        if print_text:\n            print_message_delta(delta)\n        if "content" in delta:\n            explanation += delta.content\n    plan_assistant_message = {"role": "assistant", "content": plan}\n\n    # Step 2b: If the plan is short, ask GPT to elaborate further\n    # this counts top-level bullets (e.g., categories), but not sub-bullets (e.g., test cases)\n    num_bullets = max(plan.count("\n-"), plan.count("\n*"))\n    elaboration_needed = num_bullets < approx_min_cases_to_cover\n    if elaboration_needed:\n        elaboration_user_message = {\n            "role": "user",\n            "content": f"""In addition to those scenarios above, list a few rare or unexpected edge cases (and as before, under each edge case, include a few examples as sub-bullets).""",\n        }\n        elaboration_messages = [\n            explain_system_message,\n            explain_user_message,\n            explain_assistant_message,\n            plan_user_message,\n            plan_assistant_message,\n            elaboration_user_message,\n        ]\n        if print_text:\n            print_messages([elaboration_user_message])\n        elaboration_response = client.chat.completions.create(model=plan_model,\n        messages=elaboration_messages,\n        temperature=temperature,\n        stream=True)\n        elaboration = ""\n        for chunk in elaboration_response:\n            delta = chunk.choices[0].delta\n        if print_text:\n            print_message_delta(delta)\n        if "content" in delta:\n            explanation += delta.content\n        elaboration_assistant_message = {"role": "assistant", "content": elaboration}\n\n    # Step 3: Generate the unit test\n\n    # create a markdown-formatted prompt that asks GPT to complete a unit test\n    package_comment = ""\n    if unit_test_package == "pytest":\n        package_comment = "# below, each test case is represented by a tuple passed to the @pytest.mark.parametrize decorator"\n    execute_system_message = {\n        "role": "system",\n        "content": "You are a world-class Python developer with an eagle eye for unintended bugs and edge cases. You write careful, accurate unit tests. When asked to reply only with code, you write all of your code in a single block.",\n    }\n    execute_user_message = {\n        "role": "user",\n        "content": f"""Using Python and the `{unit_test_package}` package, write a suite of unit tests for the function, following the cases above. Include helpful comments to explain each line. Reply only with code, formatted as follows:\n\n```python\n# imports\nimport {unit_test_package}  # used for our unit tests\n{{insert other imports as needed}}\n\n# function to test\n{function_to_test}\n"""\n    }\n
```

----------------------------------------

TITLE: Invoking OpenAI Chat Completion with Function Tool in Python
DESCRIPTION: Implements an API invocation function with retry logic for enriching entities. Assembles messages, sets the function to invoke via 'tool_choice', and processes the model's response by calling the specified enrichment function with parsed arguments. Dependencies include the 'openai', 'json', 'logging', and 'tenacity' (for retry decorator) libraries, as well as helper message formatting and enrichment functions. Input includes entity labels and the target text; output returns both the raw model response and the enrichment function's processed output.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Named_Entity_Recognition_to_enrich_text.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
@retry(wait=wait_random_exponential(min=1, max=10), stop=stop_after_attempt(5))
def run_openai_task(labels, text):
    messages = [
          {"role": "system", "content": system_message(labels=labels)},
          {"role": "assistant", "content": assisstant_message()},
          {"role": "user", "content": user_message(text=text)}
      ]

    # TODO: functions and function_call are deprecated, need to be updated
    # See: https://platform.openai.com/docs/api-reference/chat/create#chat-create-tools
    response = openai.chat.completions.create(
        model="gpt-3.5-turbo-0613",
        messages=messages,
        tools=generate_functions(labels),
        tool_choice={"type": "function", "function" : {"name": "enrich_entities"}}, 
        temperature=0,
        frequency_penalty=0,
        presence_penalty=0,
    )

    response_message = response.choices[0].message
    
    available_functions = {"enrich_entities": enrich_entities}  
    function_name = response_message.tool_calls[0].function.name
    
    function_to_call = available_functions[function_name]
    logging.info(f"function_to_call: {function_to_call}")

    function_args = json.loads(response_message.tool_calls[0].function.arguments)
    logging.info(f"function_args: {function_args}")

    function_response = function_to_call(text, function_args)

    return {"model_response": response, 
            "function_response": function_response}
```

----------------------------------------

TITLE: Retrieving and Building Contextual Prompt using OpenAI Embeddings and Pinecone in Python
DESCRIPTION: Defines a function to embed a user query via OpenAI, retrieve relevant contexts from Pinecone, and construct a prompt string clipped to a specified token limit. Requires the global 'embed_model', an OpenAI key, and a configured Pinecone 'index'. Accepts a 'query' string, and returns a prompt string with concatenated contexts for downstream completion. Handles prompt size constraints and automatically segments contextual data.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Gen_QA.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
limit = 3750

def retrieve(query):
    res = openai.Embedding.create(
        input=[query],
        engine=embed_model
    )

    # retrieve from Pinecone
    xq = res['data'][0]['embedding']

    # get relevant contexts
    res = index.query(xq, top_k=3, include_metadata=True)
    contexts = [
        x['metadata']['text'] for x in res['matches']
    ]

    # build our prompt with the retrieved contexts included
    prompt_start = (
        "Answer the question based on the context below.\n\n"+
        "Context:\n"
    )
    prompt_end = (
        f"\n\nQuestion: {query}\nAnswer:"
    )
    # append contexts until hitting limit
    for i in range(1, len(contexts)):
        if len("\n\n---\n\n".join(contexts[:i])) >= limit:
            prompt = (
                prompt_start +
                "\n\n---\n\n".join(contexts[:i-1]) +
                prompt_end
            )
            break
        elif i == len(contexts)-1:
            prompt = (
                prompt_start +
                "\n\n---\n\n".join(contexts) +
                prompt_end
            )
    return prompt
```

----------------------------------------

TITLE: Allowing Model to Choose Function Automatically - Python
DESCRIPTION: Lets the assistant choose whether and which function to call, by omitting the 'tool_choice' parameter in the chat completion API call. Illustrates the model's natural clarification or function argument generation abilities depending on user input ambiguity and context.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
# if we don't force the model to use get_n_day_weather_forecast it may not\nmessages = []\nmessages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})\nmessages.append({"role": "user", "content": "Give me a weather report for Toronto, Canada."})\nchat_response = chat_completion_request(\n    messages, tools=tools\n)\nchat_response.choices[0].message
```

----------------------------------------

TITLE: Extracting Structured Invoice Data from Images Using GPT-4o (OpenAI) in Python
DESCRIPTION: This function sends each base64-encoded image to the OpenAI GPT-4o model with a detailed prompt instructing the LLM to extract information from hotel invoice documents, preserving original field names, grouping related data, filling blanks with null, and retaining tabular structure. Required dependencies are the OpenAI Python SDK and a valid API key. Inputs include a single base64_image string, and the output is a JSON object string returned by the model.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Data_extraction_transformation.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
def extract_invoice_data(base64_image):
    system_prompt = f"""
    You are an OCR-like data extraction tool that extracts hotel invoice data from PDFs.
   
    1. Please extract the data in this hotel invoice, grouping data according to theme/sub groups, and then output into JSON.

    2. Please keep the keys and values of the JSON in the original language. 

    3. The type of data you might encounter in the invoice includes but is not limited to: hotel information, guest information, invoice information,
    room charges, taxes, and total charges etc. 

    4. If the page contains no charge data, please output an empty JSON object and don't make up any data.

    5. If there are blank data fields in the invoice, please include them as "null" values in the JSON object.
    
    6. If there are tables in the invoice, capture all of the rows and columns in the JSON object. 
    Even if a column is blank, include it as a key in the JSON object with a null value.
    
    7. If a row is blank denote missing fields with "null" values. 
    
    8. Don't interpolate or make up data.

    9. Please maintain the table structure of the charges, i.e. capture all of the rows and columns in the JSON object.

    """
    
    response = client.chat.completions.create(
        model="gpt-4o",
        response_format={ "type": "json_object" },
        messages=[
            {
                "role": "system",
                "content": system_prompt
            },
            {
                "role": "user",
                "content": [
                    {"type": "text", "text": "extract the data in this hotel invoice and output into JSON "},
                    {"type": "image_url", "image_url": {"url": f"data:image/png;base64,{base64_image}", "detail": "high"}}
                ]
            }
        ],
        temperature=0.0,
    )
    return response.choices[0].message.content

```

----------------------------------------

TITLE: Invoking Tool Functions from OpenAI Response (Python)
DESCRIPTION: Defines a Python function to process an OpenAI model response, identify tool calls, execute the corresponding local Python functions using a mapping, and format the results as messages to continue the conversation. Handles potential errors during execution.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
def invoke_functions_from_response(response,
                                   tool_mapping: dict[str, Callable] = tool_mapping
                                   ) -> list[dict]:
    """Extract all function calls from the response, look up the corresponding tool function(s) and execute them.
    (This would be a good place to handle asynchroneous tool calls, or ones that take a while to execute.)
    This returns a list of messages to be added to the conversation history.
    """
    intermediate_messages = []
    for response_item in response.output:
        if response_item.type == 'function_call':
            target_tool = tool_mapping.get(response_item.name)
            if target_tool:
                try:
                    arguments = json.loads(response_item.arguments)
                    print(f"Invoking tool: {response_item.name}({arguments})")
                    tool_output = target_tool(**arguments)
                except Exception as e:
                    msg = f"Error executing function call: {response_item.name}: {e}"
                    tool_output = msg
                    print(msg)
            else:
                msg = f"ERROR - No tool registered for function call: {response_item.name}"
                tool_output = msg
                print(msg)
            intermediate_messages.append({
                "type": "function_call_output",
                "call_id": response_item.call_id,
                "output": tool_output
            })
        elif response_item.type == 'reasoning':
            print(f'Reasoning step: {response_item.summary}')
    return intermediate_messages
```

----------------------------------------

TITLE: Perform Content-Based Vector Search in Tair (Python)
DESCRIPTION: This snippet demonstrates how to use the `query_tair` function to perform a vector similarity search based on a text query ("Famous battles in Scottish history") against the "content_vector" index. It then iterates through the returned results, retrieves the title attribute for each result's key using `client.tvs_hmget`, and prints the result number, title, and distance.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/tair/Getting_started_with_Tair_and_OpenAI.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
# This time we'll query using content vector
query_result = query_tair(client=client, query="Famous battles in Scottish history", vector_name="content_vector")
for i in range(len(query_result)):
    title = client.tvs_hmget(index+"_"+"content_vector", query_result[i][0].decode('utf-8'), "title")
    print(f"{i + 1}. {title[0].decode('utf-8')} (Distance: {round(query_result[i][1],3)})")
```

----------------------------------------

TITLE: Initializing OpenAI Chat Completion Client and Constants - Python
DESCRIPTION: Imports libraries and initializes the OpenAI client for making chat completion requests. Also sets a default GPT model and imports retry utilities and colored output modules. Dependencies include openai, tenacity, and termcolor. This snippet must precede API-related calls in the notebook.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import json\nfrom openai import OpenAI\nfrom tenacity import retry, wait_random_exponential, stop_after_attempt\nfrom termcolor import colored  \n\nGPT_MODEL = "gpt-4o"\nclient = OpenAI()
```

----------------------------------------

TITLE: Implementing Article Summarization with Structured Output
DESCRIPTION: Defines a function to summarize articles using OpenAI's API with Structured Outputs, utilizing a Pydantic model for the response format.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Structured_Outputs_Intro.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
summarization_prompt = '''
    You will be provided with content from an article about an invention.
    Your goal will be to summarize the article following the schema provided.
    Here is a description of the parameters:
    - invented_year: year in which the invention discussed in the article was invented
    - summary: one sentence summary of what the invention is
    - inventors: array of strings listing the inventor full names if present, otherwise just surname
    - concepts: array of key concepts related to the invention, each concept containing a title and a description
    - description: short description of the invention
'''

class ArticleSummary(BaseModel):
    invented_year: int
    summary: str
    inventors: list[str]
    description: str

    class Concept(BaseModel):
        title: str
        description: str

    concepts: list[Concept]

def get_article_summary(text: str):
    completion = client.beta.chat.completions.parse(
        model=MODEL,
        temperature=0.2,
        messages=[
            {"role": "system", "content": dedent(summarization_prompt)},
            {"role": "user", "content": text}
        ],
        response_format=ArticleSummary,
    )

    return completion.choices[0].message.parsed
```

----------------------------------------

TITLE: Orchestrating Chatbot Conversation with Function Execution - Python
DESCRIPTION: Implements the main conversation loop where the user's input is passed to the OpenAI chat model, function calls identified by the model are executed via available_functions, and a follow-up response is generated. The function maintains the conversation context, handles logging, and restricts out-of-scope answers with a controllable topic. It ensures a full round-trip of chat, function execution, and summarization.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
def run_conversation(user_input, topic="S3 bucket functions.", is_log=False):\n\n    system_message=f"Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous. If the user ask question not related to {topic} response your scope is {topic} only."\n    \n    messages = [{"role": "system", "content": system_message},\n                {"role": "user", "content": user_input}]\n    \n    # Call the model to get a response\n    response = chat_completion_request(messages, functions=functions)\n    response_message = response.choices[0].message\n    \n    if is_log:\n        print(response.choices)\n    \n    # check if GPT wanted to call a function\n    if response_message.tool_calls:\n        function_name = response_message.tool_calls[0].function.name\n        function_args = json.loads(response_message.tool_calls[0].function.arguments)\n        \n        # Call the function\n        function_response = available_functions[function_name](**function_args)\n        \n        # Add the response to the conversation\n        messages.append(response_message)\n        messages.append({\n            "role": "tool",\n            "content": function_response,\n            "tool_call_id": response_message.tool_calls[0].id,\n        })\n        \n        # Call the model again to summarize the results\n        second_response = chat_completion_request(messages)\n        final_message = second_response.choices[0].message.content\n    else:\n        final_message = response_message.content\n\n    return final_message
```

----------------------------------------

TITLE: Setting OpenAI API Key and Default GPT Model - Python
DESCRIPTION: This code retrieves the OpenAI API key from environment variables and sets the default GPT model to use for conversation. It ensures that the OpenAI client is authenticated and that a specific model is chosen for all interactions. The code assumes that the OPENAI_API_KEY environment variable is set.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
OpenAI.api_key = os.environ.get("OPENAI_API_KEY")\nGPT_MODEL = "gpt-3.5-turbo"
```

----------------------------------------

TITLE: Importing Libraries and Initializing OpenAI Client
DESCRIPTION: Imports various Python libraries required for the notebook, including arxiv, json, os, pandas, openai, etc. It also defines the GPT model and embedding model names and initializes the OpenAI client.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_for_knowledge_retrieval.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import arxiv
import ast
import concurrent
import json
import os
import pandas as pd
import tiktoken
from csv import writer
from IPython.display import display, Markdown, Latex
from openai import OpenAI
from PyPDF2 import PdfReader
from scipy import spatial
from tenacity import retry, wait_random_exponential, stop_after_attempt
from tqdm import tqdm
from termcolor import colored

GPT_MODEL = "gpt-4o-mini"
EMBEDDING_MODEL = "text-embedding-ada-002"
client = OpenAI()
```

----------------------------------------

TITLE: Create AI Assistant Response with Tools in Python
DESCRIPTION: This Python code demonstrates how to initiate a conversation with an AI client using defined tools. It calls the `client.responses.create` method, providing instructions, the model to use, a list of tool configurations, and the user's input query. The subsequent line accesses the output from the generated response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/gpt4-1_prompting_guide.ipynb#_snippet_16

LANGUAGE: Python
CODE:
```
response = client.responses.create(
    instructions=SYS_PROMPT_CUSTOMER_SERVICE,
    model="gpt-4.1-2025-04-14",
    tools=[get_policy_doc, get_user_acct],
    input="How much will it cost for international service? I'm traveling to France.",
    # input="Why was my last bill so high?"
)

response.to_dict()["output"]
```

----------------------------------------

TITLE: Dubbing Audio Files from English to Hindi and Extracting Multimodal Output with GPT-4o in Python
DESCRIPTION: Prepares a prompt and configuration to directly dub English audio to Hindi while preserving certain terms in English, then calls process_audio_with_gpt_4o for multimodal (text/audio) output using the same base64-encoded audio. Relies on the process_audio_with_gpt_4o function and a glossary, expects the API to return a multimodal response, and extracts the message containing both audio and transcript results. Requires base64-encoded audio data and a defined glossary; output is the GPT-4o response containing the dubbed Hindi transcript and audio in base64.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/voice_solutions/voice_translation_into_different_languages_using_GPT-4o.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
glossary_of_terms_to_keep_in_original_language = "Turbo, OpenAI, token, GPT, Dall-e, Python"

modalities = ["text", "audio"]
prompt = f"The user will provide an audio file in English. Dub the complete audio, word for word in Hindi. Keep certain words in English for which a direct translation in Hindi does not exist such as  ${glossary_of_terms_to_keep_in_original_language}."

response_json = process_audio_with_gpt_4o(english_audio_base64, modalities, prompt)

message = response_json['choices'][0]['message']
```

----------------------------------------

TITLE: Generating JSON Schema for OpenAI Function Calling in Python
DESCRIPTION: Defines a Python function to generate the JSON schema used for OpenAI's function calling (tools parameter). This schema restricts parameters to specific labels, enforces them as arrays of strings, and disables additional properties for precise input validation. Prerequisites include a list of label keys and compatibility with OpenAI's chat completions API tool schema.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Named_Entity_Recognition_to_enrich_text.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
def generate_functions(labels: dict) -> list:
    return [
        {   
            "type": "function",
            "function": {
                "name": "enrich_entities",
                "description": "Enrich Text with Knowledge Base Links",
                "parameters": {
                    "type": "object",
                        "properties": {
                            "r'^(?:' + '|'.join({labels}) + ')$'": 
                            {
                                "type": "array",
                                "items": {
                                    "type": "string"
                                }
                            }
                        },
                        "additionalProperties": False
                },
            }
        }
    ]
```

----------------------------------------

TITLE: Processing Audio Files with GPT-4o Chat Completions API in Python
DESCRIPTION: Defines the process_audio_with_gpt_4o function for sending audio data (base64-encoded) and configuration options to OpenAI's GPT-4o API for voice-to-text, voice-to-voice, or other multimodal tasks. Requires dependencies: requests, os, json, and an 'OPENAI_API_KEY' environment variable. Accepts audio content, output modality (text/audio), and a custom system prompt; sends a POST request and returns the API response or prints an error. Inputs are the base64 audio, modalities list, and prompt string; output is the response JSON or None. API key must be set in the environment. Sound file is expected to be in WAV format.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/voice_solutions/voice_translation_into_different_languages_using_GPT-4o.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
# Make sure requests package is installed  
import requests 
import os
import json

# Load the API key from the environment variable
api_key = os.getenv("OPENAI_API_KEY")


def process_audio_with_gpt_4o(base64_encoded_audio, output_modalities, system_prompt):
    # Chat Completions API end point 
    url = "https://api.openai.com/v1/chat/completions"

    # Set the headers
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {api_key}"
    }

    # Construct the request data
    data = {
        "model": "gpt-4o-audio-preview",
        "modalities": output_modalities,
        "audio": {
            "voice": "alloy",
            "format": "wav"
        },
        "messages": [
            {
                "role": "system",
                "content": system_prompt
            },
            {
                "role": "user",
                "content": [
                    {
                        "type": "input_audio",
                        "input_audio": {
                            "data": base64_encoded_audio,
                            "format": "wav"
                        }
                    }
                ]
            }
        ]
    }
    
    request_response = requests.post(url, headers=headers, data=json.dumps(data))
    if request_response.status_code == 200:
        return request_response.json()
    else:  
        print(f"Error {request_response.status_code}: {request_response.text}")
        return

```

----------------------------------------

TITLE: Uploading a File to S3 Bucket with Python Assistant Bot
DESCRIPTION: This snippet provides a template for uploading a local file to a specific S3 bucket by leveraging the assistant's natural language capabilities. All placeholders must be replaced with actual values before execution. It requires the run_conversation function and prints the assistant's response to standard output.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_16

LANGUAGE: python
CODE:
```
local_file = '<file_name>'\nbucket_name = '<bucket_name>'\nprint(run_conversation(f'upload {local_file} to {bucket_name} bucket'))
```

----------------------------------------

TITLE: Generating Deterministic Chat Completions with Constant Seed and Temperature - Python
DESCRIPTION: This snippet modifies the previous flow to add a fixed seed (123) and temperature (0) to each chat completion request, thus maximizing determinism across runs. The responses are then compared for similarity, with a lower average distance expected. All other parameters, including prompt and message values, are unchanged between runs. Outputs include printed completions and the computed average distance. Dependencies are unchanged from the previous example.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Reproducible_outputs_with_the_seed_parameter.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
SEED = 123
responses = []


async def get_response(i):
    print(f'Output {i + 1}\n{"-" * 10}')
    response = await get_chat_response(
        system_message=system_message,
        seed=SEED,
        temperature=0,
        user_request=user_request,
    )
    return response


responses = await asyncio.gather(*[get_response(i) for i in range(5)])

average_distance = calculate_average_distance(responses)
print(f"The average distance between responses is: {average_distance}")
```

----------------------------------------

TITLE: Feeding Function Results Back Into Chat Completion - Python
DESCRIPTION: Appends the response from the invoked function as a new message with role 'function' and sends an updated message sequence to the chat completions API. The model incorporates function return values into its natural language response, which is then printed. Requires proper message and function definition objects, as well as the Azure OpenAI client to be initialized.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/functions.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
messages.append(
    {
        \"role\": \"function\",
        \"name\": \"get_current_weather\",
        \"content\": json.dumps(response)
    }
)

function_completion = client.chat.completions.create(
    model=deployment,
    messages=messages,
    tools=functions,
)

print(function_completion.choices[0].message.content.strip())
```

----------------------------------------

TITLE: Extending Tools List for LLM Agent with Custom Retrieval and Search - Python
DESCRIPTION: This snippet illustrates the expansion of an LLM agent's toolset by defining two tools: a 'Search' tool for current events using a generic `search.run`, and a 'Knowledge Base' tool utilizing the Pinecone-backed RetrievalQA chain to answer general questions. Dependencies include the `Tool` class/object and previously defined `search` and `podcast_retriever` methods. Each tool contains a descriptive purpose and callable function; inputs should be well-formed questions. Limitations depend on the underlying tool functionality and input constraints.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_28

LANGUAGE: Python
CODE:
```
expanded_tools = [
    Tool(
        name = "Search",
        func=search.run,
        description="useful for when you need to answer questions about current events"
    ),
    Tool(
        name = 'Knowledge Base',
        func=podcast_retriever.run,
        description="Useful for general questions about how to do things and for details on interesting topics. Input should be a fully formed question."
    )
]
```

----------------------------------------

TITLE: Token Counting Utilities for Chat Model Fine-tuning in Python
DESCRIPTION: Implements utility functions for token counting in chat datasets using OpenAI's tiktoken library. Includes functions to count total tokens in messages, tokens in assistant messages only, and print statistical distributions of numeric values.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Chat_finetuning_data_prep.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
encoding = tiktoken.get_encoding("cl100k_base")

# not exact!
# simplified from https://github.com/openai/openai-cookbook/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb
def num_tokens_from_messages(messages, tokens_per_message=3, tokens_per_name=1):
    num_tokens = 0
    for message in messages:
        num_tokens += tokens_per_message
        for key, value in message.items():
            num_tokens += len(encoding.encode(value))
            if key == "name":
                num_tokens += tokens_per_name
    num_tokens += 3
    return num_tokens

def num_assistant_tokens_from_messages(messages):
    num_tokens = 0
    for message in messages:
        if message["role"] == "assistant":
            num_tokens += len(encoding.encode(message["content"]))
    return num_tokens

def print_distribution(values, name):
    print(f"\n#### Distribution of {name}:")
    print(f"min / max: {min(values)}, {max(values)}")
    print(f"mean / median: {np.mean(values)}, {np.median(values)}")
    print(f"p5 / p95: {np.quantile(values, 0.1)}, {np.quantile(values, 0.9)}")
```

----------------------------------------

TITLE: Validating OpenAI API Key Presence in Python Environment
DESCRIPTION: Checks that the OPENAI_API_KEY environment variable is set by reading from the OS environment. Provides user feedback through print statements if the API key is properly set. Includes commented code for optionally setting the key within the current Python session.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/qdrant/Getting_started_with_Qdrant_and_OpenAI.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
# Test that your OpenAI API key is correctly set as an environment variable
# Note. if you run this notebook locally, you will need to reload your terminal and the notebook for the env variables to be live.
import os

# Note. alternatively you can set a temporary env variable like this:
# os.environ["OPENAI_API_KEY"] = "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

if os.getenv("OPENAI_API_KEY") is not None:
    print("OPENAI_API_KEY is ready")
else:
    print("OPENAI_API_KEY environment variable not found")
```

----------------------------------------

TITLE: Processing DataFrame Rows in Parallel using concurrent.futures and TQDM in Python
DESCRIPTION: Processes each row in a DataFrame in parallel using ThreadPoolExecutor, applying a model completion function, updating a progress bar with tqdm, and saving results into a dynamic DataFrame column named after the model. Relies on previously defined 'generate_prompt', 'call_model', and 'varieties' for input generation and model invocation. Takes a DataFrame, a model string, and updates global progress state. Inputs include a pandas DataFrame and model name; outputs are DataFrame with model results added. Limitations: concurrency may require thread-safe handling of DataFrame updates.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Leveraging_model_distillation_to_fine-tune_a_model.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
def process_example(index, row, model, df, progress_bar):
    global progress_index

    try:
        # Generate the prompt using the row
        prompt = generate_prompt(row, varieties)

        df.at[index, model + "-variety"] = call_model(model, prompt)
        
        # Update the progress bar
        progress_bar.update(1)
        
        progress_index += 1
    except Exception as e:
        print(f"Error processing model {model}: {str(e)}")

def process_dataframe(df, model):
    global progress_index
    progress_index = 1  # Reset progress index

    # Create a tqdm progress bar
    with tqdm(total=len(df), desc="Processing rows") as progress_bar:
        # Process each example concurrently using ThreadPoolExecutor
        with concurrent.futures.ThreadPoolExecutor() as executor:
            futures = {executor.submit(process_example, index, row, model, df, progress_bar): index for index, row in df.iterrows()}
            
            for future in concurrent.futures.as_completed(futures):
                try:
                    future.result()  # Wait for each example to be processed
                except Exception as e:
                    print(f"Error processing example: {str(e)}")

    return df

```

----------------------------------------

TITLE: Defining Embedding Generation Function with OpenAI - Python
DESCRIPTION: Defines a function 'generate_embedding' that uses OpenAI's embeddings API to generate a vector representation for a given input string. The function takes a string and returns a list of floats generated by the specified 'text-embedding-3-small' model. Requires the OpenAI API key to be set in the environment and assumes network connectivity.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/mongodb_atlas/semantic_search_using_mongodb_atlas_vector_search.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
model = "text-embedding-3-small"
def generate_embedding(text: str) -> list[float]:
    return openai.embeddings.create(input = [text], model=model).data[0].embedding

```

----------------------------------------

TITLE: Embed Corpus with Batching and Parallel Processing - Python
DESCRIPTION: This function takes a list of strings (corpus), encodes them using tiktoken, calculates token statistics and cost, and then generates embeddings in batches using a thread pool executor. It returns a list of embeddings.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_combine_GPT4o_with_RAG_Outfit_Assistant.ipynb#_snippet_5

LANGUAGE: Python
CODE:
```
def embed_corpus(
    corpus: List[str],
    batch_size=64,
    num_workers=8,
    max_context_len=8191,
):
    # Encode the corpus, truncating to max_context_len
    encoding = tiktoken.get_encoding("cl100k_base")
    encoded_corpus = [
        encoded_article[:max_context_len] for encoded_article in encoding.encode_batch(corpus)
    ]

    # Calculate corpus statistics: the number of inputs, the total number of tokens, and the estimated cost to embed
    num_tokens = sum(len(article) for article in encoded_corpus)
    cost_to_embed_tokens = num_tokens / 1000 * EMBEDDING_COST_PER_1K_TOKENS
    print(
        f"num_articles={len(encoded_corpus)}, num_tokens={num_tokens}, est_embedding_cost={cost_to_embed_tokens:.2f} USD"
    )

    # Embed the corpus
    with concurrent.futures.ThreadPoolExecutor(max_workers=num_workers) as executor:
        
        futures = [
            executor.submit(get_embeddings, text_batch)
            for text_batch in batchify(encoded_corpus, batch_size)
        ]

        with tqdm(total=len(encoded_corpus)) as pbar:
            for _ in concurrent.futures.as_completed(futures):
                pbar.update(batch_size)

        embeddings = []
        for future in futures:
            data = future.result()
            embeddings.extend(data)

        return embeddings
```

----------------------------------------

TITLE: Making a Chat Completion Request with Tool Functionality - Python
DESCRIPTION: Defines a function to send a chat completion request to OpenAI, optionally including function/tool definitions for model-driven function calling. It allows passing context messages, function schemas, a function call mode (auto/manual), and a model choice. This function is central for GPT-powered, tool-augmented automation.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
def chat_completion_request(messages, functions=None, function_call='auto', \n                            model_name=GPT_MODEL):\n    \n    if functions is not None:\n        return client.chat.completions.create(\n            model=model_name,\n            messages=messages,\n            tools=functions,\n            tool_choice=function_call)\n    else:\n        return client.chat.completions.create(\n            model=model_name,\n            messages=messages)
```

----------------------------------------

TITLE: Importing and Configuring OpenAI Client in Python
DESCRIPTION: This snippet initializes and configures the OpenAI Python client, imports necessary modules, and prepares for making API calls. It requires the 'openai' library and accesses the API key from environment variables, with a fallback option. This setup enables subsequent transcription API requests using the instantiated client.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Whisper_correct_misspelling.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
# imports\nfrom openai import OpenAI  # for making OpenAI API calls\nimport urllib  # for downloading example audio files\nimport os  # for accessing environment variables\n\nclient = OpenAI(api_key=os.environ.get(\"OPENAI_API_KEY\", \"<your OpenAI API key if not set as env var>\"))
```

----------------------------------------

TITLE: Analyze Image with OpenAI GPT-4o-mini (Python)
DESCRIPTION: Defines a Python function to call the OpenAI chat completions API with an image and text prompt. It sends a base64 encoded image and instructions to gpt-4o-mini to analyze clothing items and return structured JSON containing suggested outfit items, category, and gender.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_combine_GPT4o_with_RAG_Outfit_Assistant.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
def analyze_image(image_base64, subcategories):
    response = client.chat.completions.create(
        model=GPT_MODEL,
        messages=[
            {
            "role": "user",
            "content": [
                {
                "type": "text",
                "text": f"""Given an image of an item of clothing, analyze the item and generate a JSON output with the following fields: "items", "category", and "gender".
                           Use your understanding of fashion trends, styles, and gender preferences to provide accurate and relevant suggestions for how to complete the outfit.
                           The items field should be a list of items that would go well with the item in the picture. Each item should represent a title of an item of clothing that contains the style, color, and gender of the item.
                           The category needs to be chosen between the types in this list: {subcategories}.
                           You have to choose between the genders in this list: [Men, Women, Boys, Girls, Unisex]
                           Do not include the description of the item in the picture. Do not include the ```json ``` tag in the output.

                           Example Input: An image representing a black leather jacket.

                           Example Output: {{\"items\": [\"Fitted White Women's T-shirt\", \"White Canvas Sneakers\", \"Women's Black Skinny Jeans\"], \"category\": \"Jackets\", \"gender\": \"Women\"}}
                           """,
                },
                {
                "type": "image_url",
                "image_url": {
                    "url": f"data:image/jpeg;base64,{image_base64}",
                },
                }
            ],
            }
        ]
    )
    # Extract relevant features from the response
    features = response.choices[0].message.content
    return features
```

----------------------------------------

TITLE: Inefficient Multiple Embedding Requests Without Rate Limit Handling - Python
DESCRIPTION: This snippet reflects a negative example where multiple text embeddings are requested in a for-loop without any rate limiting or error handling, making it subject to OpenAI API rate limits. It requires the openai package and expects num_embeddings texts to be embedded one by one, printing their lengths. This approach can lead to slowdowns and failed requests due to rate limiting.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Using_embeddings.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# Negative example (slow and rate-limited)
from openai import OpenAI
client = OpenAI()

num_embeddings = 10000 # Some large number
for i in range(num_embeddings):
    embedding = client.embeddings.create(
        input="Your text goes here", model="text-embedding-3-small"
    ).data[0].embedding
    print(len(embedding))
```

----------------------------------------

TITLE: Multi-Tool Orchestrated Querying with Responses API in Python
DESCRIPTION: Illustrates how to set up a sequential tool calling flow for a single query: first performing a web search, then querying the Pinecone knowledge base. Messages are prepared and passed to the Responses API with explicit system instructions dictating the tool call sequence. The code assumes pre-existing 'client', 'tools', and related configurations and relies on handling the response output for further post-processing. The input is a user query string and outputs include the tool outputs and the aggregated response from the API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_api_tool_orchestration.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
# Process one query as an example to understand the tool calls and function calls as part of the response output
item = "What is the most common cause of death in the United States"

# Initialize input messages with the user's query.
input_messages = [{"role": "user", "content": item}]
print("\n🌟--- Processing Query ---🌟")
print(f"🔍 **User Query:** {item}")
    
    # Call the Responses API with tools enabled and allow parallel tool calls.
print("\n🔧 **Calling Responses API with Tools Enabled")
print("\n🕵️‍♂️ **Step 1: Web Search Call**")
print("   - Initiating web search to gather initial information.")
print("\n📚 **Step 2: Pinecone Search Call**")
print("   - Querying Pinecone to find relevant examples from the internal knowledge base.")
    
response = client.responses.create(
        model="gpt-4o",
        input=[
            {"role": "system", "content": "Every time it's prompted with a question, first call the web search tool for results, then call `PineconeSearchDocuments` to find real examples in the internal knowledge base."},
            {"role": "user", "content": item}
        ],
        tools=tools,
        parallel_tool_calls=True
    )
    
# Print the initial response output.
print("input_messages", input_messages)

print("\n✨ **Initial Response Output:**")
print(response.output)

```

----------------------------------------

TITLE: Defining Specialized Agents with OpenAI Agents SDK (Python)
DESCRIPTION: Defines four distinct Agent instances using the OpenAI Agents SDK, each specialized for a specific task: extracting features, listing pros/cons, analyzing sentiment, and summarizing recommendations from a review. These agents are then collected into a list for parallel execution.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/agents_sdk/parallel_agents.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# Agent focusing on product features
features_agent = Agent(
    name="FeaturesAgent",
    instructions="Extract the key product features from the review."
)

# Agent focusing on pros & cons
pros_cons_agent = Agent(
    name="ProsConsAgent",
    instructions="List the pros and cons mentioned in the review."
)

# Agent focusing on sentiment analysis
sentiment_agent = Agent(
    name="SentimentAgent",
    instructions="Summarize the overall user sentiment from the review."
)

# Agent focusing on recommendation summary
recommend_agent = Agent(
    name="RecommendAgent",
    instructions="State whether you would recommend this product and why."
)

parallel_agents = [
    features_agent,
    pros_cons_agent,
    sentiment_agent,
    recommend_agent
]
```

----------------------------------------

TITLE: Processing Multi-image Inputs with OpenAI Chat API - Python
DESCRIPTION: This snippet demonstrates accepting multiple image URLs and textual queries as part of chat messages for the OpenAI API, using a helper function to encapsulate image and text message formatting and completion invocation. Dependencies are openai, json, and time modules, along with a valid OpenAI client and the necessary image URLs. Inputs include URLs and a user query string; outputs are pretty-printed JSON responses from the API for three different runs, illustrating cache hits and misses. Constraint: expects all URLs to be reachable and images to be supported by the API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Prompt_Caching101.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
sauce_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/12-04-20-saucen-by-RalfR-15.jpg/800px-12-04-20-saucen-by-RalfR-15.jpg"
veggie_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Veggies.jpg/800px-Veggies.jpg"
eggs_url= "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a2/Egg_shelf.jpg/450px-Egg_shelf.jpg"
milk_url= "https://upload.wikimedia.org/wikipedia/commons/thumb/c/cd/Lactaid_brand.jpg/800px-Lactaid_brand.jpg"

def multiimage_completion(url1, url2, user_query):
    completion = client.chat.completions.create(
    model="gpt-4o-2024-08-06",
    messages=[
        {
        "role": "user",
        "content": [
            {
            "type": "image_url",
            "image_url": {
                "url": url1,
                "detail": "high"
            },
            },
            {
            "type": "image_url",
            "image_url": {
                "url": url2,
                "detail": "high"
            },
            },
            {"type": "text", "text": user_query}
        ],
        }
    ],
    max_tokens=300,
    )
    print(json.dumps(completion.to_dict(), indent=4))
    

def main(sauce_url, veggie_url):
    multiimage_completion(sauce_url, veggie_url, "Please list the types of sauces are shown in these images")
    #delay for 20 seconds
    time.sleep(20)
    multiimage_completion(sauce_url, veggie_url, "Please list the types of vegetables are shown in these images")
    time.sleep(20)
    multiimage_completion(milk_url, sauce_url, "Please list the types of sauces are shown in these images")

if __name__ == "__main__":
    main(sauce_url, veggie_url)
```

----------------------------------------

TITLE: Encoding Images and Querying GPT-4 Vision
DESCRIPTION: Functions to encode images in base64 format and query the GPT-4 Vision model with both text and image inputs. This allows for visual understanding of images by the language model.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/custom_image_embedding_search.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
def encode_image(image_path):
    with open(image_path, 'rb') as image_file:
        encoded_image = base64.b64encode(image_file.read())
        return encoded_image.decode('utf-8')

def image_query(query, image_path):
    response = client.chat.completions.create(
        model='gpt-4-vision-preview',
        messages=[
            {
            "role": "user",
            "content": [
                {
                "type": "text",
                "text": query,
                },
                {
                "type": "image_url",
                "image_url": {
                    "url": f"data:image/jpeg;base64,{encode_image(image_path)}",
                },
                }
            ],
            }
        ],
        max_tokens=300,
    )
    # Extract relevant features from the response
    return response.choices[0].message.content
image_query('Write a short label of what is show in this image?', image_path)
```

----------------------------------------

TITLE: Verifying OPENAI_API_KEY Environment Variable - Python
DESCRIPTION: Checks if the `OPENAI_API_KEY` environment variable is set and prints a corresponding message. If running locally, ensure to relaunch the environment to refresh variables. Optionally, you can set the variable at runtime using `os.environ`. No external dependencies beyond `os`.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/analyticdb/QA_with_Langchain_AnalyticDB_and_OpenAI.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Test that your OpenAI API key is correctly set as an environment variable
# Note. if you run this notebook locally, you will need to reload your terminal and the notebook for the env variables to be live.
import os

# Note. alternatively you can set a temporary env variable like this:
# os.environ[\"OPENAI_API_KEY\"] = \"sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\"

if os.getenv(\"OPENAI_API_KEY\") is not None:
    print(\"OPENAI_API_KEY is ready\")
else:
    print(\"OPENAI_API_KEY environment variable not found\")
```

----------------------------------------

TITLE: Setting OpenAI API Key Environment Variable
DESCRIPTION: Exports the OpenAI API key as an environment variable to be used for vector embeddings and Q&A functionality.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/question-answering-with-weaviate-and-openai.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# Export OpenAI API Key
!export OPENAI_API_KEY="your key"
```

----------------------------------------

TITLE: Generating a RAG Response Using Search Data and OpenAI LLM in Python
DESCRIPTION: This snippet builds a prompt incorporating structured search results and the user query, sends it to the OpenAI LLM, and prints the resulting answer. Dependencies include the 'json' module and an initialized OpenAI 'client'. Inputs are a search data list and original search terms. The output is a generated response citing sources based on the retrieved web data. Suitable for use at the final workflow step to provide up-to-date, citation-aware answers.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/Web_search_with_google_api_bring_your_own_browser_tool.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
import json \n\nfinal_prompt = (\n    f"The user will provide a dictionary of search results in JSON format for search query {search_term} Based on on the search results provided by the user, provide a detailed response to this query: **'{search_query}'**. Make sure to cite all the sources at the end of your answer."\n)\n\nresponse = client.chat.completions.create(\n    model="gpt-4o",\n    messages=[\n        {"role": "system", "content": final_prompt},\n        {"role": "user", "content": json.dumps(results)}],\n    temperature=0\n\n)\nsummary = response.choices[0].message.content\n\nprint(summary)\n
```

----------------------------------------

TITLE: Implementing Visual Content Handling in RAG System with GPT-4o
DESCRIPTION: This function processes user questions by retrieving relevant context from a Pinecone index and conditionally uses either text or images as context based on metadata flags. It encodes images to base64 format when needed and constructs appropriate message content for GPT-4o, which can process both text and image inputs.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Using_vision_modality_for_RAG_with_Pinecone.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
import base64
import json


def get_response_to_question_with_images(user_question, pc_index):
    # Get embedding of the question to find the relevant page with the information 
    question_embedding = get_embedding(user_question)

    # Get response vector embeddings 
    response = pc_index.query(
        vector=question_embedding,
        top_k=3,
        include_values=True,
        include_metadata=True
    )

    # Collect the metadata from the matches
    context_metadata = [match['metadata'] for match in response['matches']]

    # Build the message content
    message_content = []

    # Add the initial prompt
    initial_prompt = f"""You are a helpful assistant. Use the text and images provided by the user to answer the question. You must include the reference to the page number or title of the section you the answer where you found the information. If you don't find the information, you can say "I couldn't find the information"

    question: {user_question}
    """
    
    message_content.append({"role": "system", "content": initial_prompt})
    
    context_messages = []

    # Process each metadata item to include text or images based on 'Visual_Input_Processed'
    for metadata in context_metadata:
        visual_flag = metadata.get('GraphicIncluded')
        page_number = metadata.get('pageNumber')
        page_text = metadata.get('text')
        message =""

        if visual_flag =='Y':
            # Include the image
            print(f"Adding page number {page_number} as an image to context")
            image_path = metadata.get('ImagePath', None)
            try:
                base64_image = encode_image(image_path)
                image_type = 'jpeg'
                # Prepare the messages for the API call
                context_messages.append({
                    "type": "image_url",
                    "image_url": {
                        "url": f"data:image/{image_type};base64,{base64_image}"
                    },
                })
            except Exception as e:
                print(f"Error encoding image at {image_path}: {e}")
        else:
            # Include the text
            print(f"Adding page number {page_number} as text to context")
            context_messages.append({
                    "type": "text",
                    "text": f"Page {page_number} - {page_text}",
                })
        
                # Prepare the messages for the API call
        messages =  {
                "role": "user",
                "content": context_messages
        }
    
    message_content.append(messages)

    completion = oai_client.chat.completions.create(
    model="gpt-4o",
    messages=message_content
    )

    return completion.choices[0].message.content
```

----------------------------------------

TITLE: Handling Prompt Flagged by Azure Content Filter - Python
DESCRIPTION: Shows how to detect and handle cases where a submitted prompt violates Azure's content filter policy. Attempts to create a chat completion with intentionally violating content, then catches the 'BadRequestError' and inspects its details. Outputs which content filter category was triggered, its filter status, and severity. Requires the 'openai' and 'json' modules.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
import json

messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "<text violating the content policy>"}
]

try:
    completion = client.chat.completions.create(
        messages=messages,
        model=deployment,
    )
except openai.BadRequestError as e:
    err = json.loads(e.response.text)
    if err["error"]["code"] == "content_filter":
        print("Content filter triggered!")
        content_filter_result = err["error"]["innererror"]["content_filter_result"]
        for category, details in content_filter_result.items():
            print(f"{category}:\n filtered={details['filtered']}\n severity={details['severity']}")
```

----------------------------------------

TITLE: Retrieving, Summarizing, and Structuring Search Results using Python and OpenAI API
DESCRIPTION: This snippet contains functions to scrape web pages, summarize their content using an OpenAI LLM, and compile search results into a structured list suitable for downstream processing or LLM ingestion. Dependencies include 'requests' and 'beautifulsoup4' for web scraping, and a properly initialized OpenAI 'client' for summaries. Key parameters are the URL to scrape, the LLM model ID, and character or token limits for summaries. The output is a list of dictionaries with ordering, links, titles, and summaries. Each component is modular for handling failures, truncation, and context constraints.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/Web_search_with_google_api_bring_your_own_browser_tool.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
import requests\nfrom bs4 import BeautifulSoup\n\nTRUNCATE_SCRAPED_TEXT = 50000  # Adjust based on your model's context window\nSEARCH_DEPTH = 5\n\ndef retrieve_content(url, max_tokens=TRUNCATE_SCRAPED_TEXT):\n        try:\n            headers = {'User-Agent': 'Mozilla/5.0'}\n            response = requests.get(url, headers=headers, timeout=10)\n            response.raise_for_status()\n\n            soup = BeautifulSoup(response.content, 'html.parser')\n            for script_or_style in soup(['script', 'style']):\n                script_or_style.decompose()\n\n            text = soup.get_text(separator=' ', strip=True)\n            characters = max_tokens * 4  # Approximate conversion\n            text = text[:characters]\n            return text\n        except requests.exceptions.RequestException as e:\n            print(f"Failed to retrieve {url}: {e}")\n            return None\n        \ndef summarize_content(content, search_term, character_limit=500):\n        prompt = (\n            f"You are an AI assistant tasked with summarizing content relevant to '{search_term}'. "\n            f"Please provide a concise summary in {character_limit} characters or less."\n        )\n        try:\n            response = client.chat.completions.create(\n                model="gpt-4o-mini",\n                messages=[\n                    {"role": "system", "content": prompt},\n                    {"role": "user", "content": content}]\n            )\n            summary = response.choices[0].message.content\n            return summary\n        except Exception as e:\n            print(f"An error occurred during summarization: {e}")\n            return None\n\ndef get_search_results(search_items, character_limit=500):\n    # Generate a summary of search results for the given search term\n    results_list = []\n    for idx, item in enumerate(search_items, start=1):\n        url = item.get('link')\n        \n        snippet = item.get('snippet', '')\n        web_content = retrieve_content(url, TRUNCATE_SCRAPED_TEXT)\n        \n        if web_content is None:\n            print(f"Error: skipped URL: {url}")\n        else:\n            summary = summarize_content(web_content, search_term, character_limit)\n            result_dict = {\n                'order': idx,\n                'link': url,\n                'title': snippet,\n                'Summary': summary\n            }\n            results_list.append(result_dict)\n    return results_list\n
```

----------------------------------------

TITLE: Enhanced Classification with Logprobs and Top Logprobs - OpenAI Chat API - Python
DESCRIPTION: This snippet performs classification using the same set of headlines but calls the completions API with logprobs enabled and top_logprobs=2, extracting the two most likely output tokens with their log probabilities and computing linear probability. The results are displayed using HTML formatting for clarity. Requires: NumPy for exponentiation, initialization of get_completion, headlines, CLASSIFICATION_PROMPT, and IPython.display for HTML rendering. Outputs model confidence per token, supporting thresholding and interpretability.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Using_logprobs.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
for headline in headlines:
    print(f"\nHeadline: {headline}")
    API_RESPONSE = get_completion(
        [{"role": "user", "content": CLASSIFICATION_PROMPT.format(headline=headline)}],
        model="gpt-4o-mini",
        logprobs=True,
        top_logprobs=2,
    )
    top_two_logprobs = API_RESPONSE.choices[0].logprobs.content[0].top_logprobs
    html_content = ""
    for i, logprob in enumerate(top_two_logprobs, start=1):
        html_content += (
            f"<span style='color: cyan'>Output token {i}:</span> {logprob.token}, "
            f"<span style='color: darkorange'>logprobs:</span> {logprob.logprob}, "
            f"<span style='color: magenta'>linear probability:</span> {np.round(np.exp(logprob.logprob)*100,2)}%<br>"
        )
    display(HTML(html_content))
    print("\n")
```

----------------------------------------

TITLE: Creating HNSW Index for Fast Vector Search in Postgres SQL
DESCRIPTION: This SQL snippet creates an HNSW (Hierarchical Navigable Small World) index on the 'embedding' column of the 'documents' table, using the 'vector_ip_ops' operator class for inner product similarity search. This improves performance for large-scale vector searches. Requires the 'documents' table and pgvector extension. Input: none; Output: index created on 'embedding' column.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/supabase/semantic-search.mdx#_snippet_2

LANGUAGE: sql
CODE:
```
create index on documents using hnsw (embedding vector_ip_ops);
```

----------------------------------------

TITLE: Defining Custom Prompt Templates with Tooling Support in LangChain (Python)
DESCRIPTION: Defines a CustomPromptTemplate extending BaseChatPromptTemplate to format agent prompts dynamically based on context, tool list, and intermediate steps. The format_messages method constructs the prompt for the LLM, interpolating current thinking, available tools, and scratchpad information. Dependencies include LangChain classes such as BaseChatPromptTemplate, HumanMessage, and expects Tool objects. Required parameters include a template string and a list of Tool instances; outputs a formatted prompt as a HumanMessage. Limitations relate to expected presence/names of variables in the template.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
# Set up a prompt template
class CustomPromptTemplate(BaseChatPromptTemplate):
    # The template to use
    template: str
    # The list of tools available
    tools: List[Tool]
    
    def format_messages(self, **kwargs) -> str:
        # Get the intermediate steps (AgentAction, Observation tuples)
        
        # Format them in a particular way
        intermediate_steps = kwargs.pop("intermediate_steps")
        thoughts = ""
        for action, observation in intermediate_steps:
            thoughts += action.log
            thoughts += f"\nObservation: {observation}\nThought: "
            
        # Set the agent_scratchpad variable to that value
        kwargs["agent_scratchpad"] = thoughts
        
        # Create a tools variable from the list of tools provided
        kwargs["tools"] = "\n".join([f"{tool.name}: {tool.description}" for tool in self.tools])
        
        # Create a list of tool names for the tools provided
        kwargs["tool_names"] = ", ".join([tool.name for tool in self.tools])
        formatted = self.template.format(**kwargs)
        return [HumanMessage(content=formatted)]
    
prompt = CustomPromptTemplate(
    template=template,
    tools=tools,
    # This omits the `agent_scratchpad`, `tools`, and `tool_names` variables because those are generated dynamically
    # This includes the `intermediate_steps` variable because that is needed
    input_variables=["input", "intermediate_steps"]
)
```

----------------------------------------

TITLE: Initializing Customer Support Guardrails and Message History - Python
DESCRIPTION: This snippet initializes a list of chat messages with a highly detailed system prompt outlining customer support guardrails and a sample initial user query, setting strict behavioral and security boundaries for the support AI assistant. Dependencies include use in conjunction with the OpenAI Python SDK and a defined "tools" object for further API interactions. Inputs are predefined message dicts; output is a ready-to-use list for chat workflows. Limitation: no dynamic insertion, all prompts are hard-coded.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Prompt_Caching101.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Enhanced system message with guardrails
messages = [
    {
        "role": "system", 
        "content": (
            "You are a professional, empathetic, and efficient customer support assistant. Your mission is to provide fast, clear, "
            "and comprehensive assistance to customers while maintaining a warm and approachable tone. "
            "Always express empathy, especially when the user seems frustrated or concerned, and ensure that your language is polite and professional. "
            "Use simple and clear communication to avoid any misunderstanding, and confirm actions with the user before proceeding. "
            "In more complex or time-sensitive cases, assure the user that you're taking swift action and provide regular updates. "
            "Adapt to the user’s tone: remain calm, friendly, and understanding, even in stressful or difficult situations."
            "\n\n"
            "Additionally, there are several important guardrails that you must adhere to while assisting users:"
            "\n\n"
            "1. **Confidentiality and Data Privacy**: Do not share any sensitive information about the company or other users. When handling personal details such as order IDs, addresses, or payment methods, ensure that the information is treated with the highest confidentiality. If a user requests access to their data, only provide the necessary information relevant to their request, ensuring no other user's information is accidentally revealed."
            "\n\n"
            "2. **Secure Payment Handling**: When updating payment details or processing refunds, always ensure that payment data such as credit card numbers, CVVs, and expiration dates are transmitted and stored securely. Never display or log full credit card numbers. Confirm with the user before processing any payment changes or refunds."
            "\n\n"
            "3. **Respect Boundaries**: If a user expresses frustration or dissatisfaction, remain calm and empathetic but avoid overstepping professional boundaries. Do not make personal judgments, and refrain from using language that might escalate the situation. Stick to factual information and clear solutions to resolve the user's concerns."
            "\n\n"
            "4. **Legal Compliance**: Ensure that all actions you take comply with legal and regulatory standards. For example, if the user requests a refund, cancellation, or return, follow the company’s refund policies strictly. If the order cannot be canceled due to being shipped or another restriction, explain the policy clearly but sympathetically."
            "\n\n"
            "5. **Consistency**: Always provide consistent information that aligns with company policies. If unsure about a company policy, communicate clearly with the user, letting them know that you are verifying the information, and avoid providing false promises. If escalating an issue to another team, inform the user and provide a realistic timeline for when they can expect a resolution."
            "\n\n"
            "6. **User Empowerment**: Whenever possible, empower the user to make informed decisions. Provide them with relevant options and explain each clearly, ensuring that they understand the consequences of each choice (e.g., canceling an order may result in loss of loyalty points, etc.). Ensure that your assistance supports their autonomy."
            "\n\n"
            "7. **No Speculative Information**: Do not speculate about outcomes or provide information that you are not certain of. Always stick to verified facts when discussing order statuses, policies, or potential resolutions. If something is unclear, tell the user you will investigate further before making any commitments."
            "\n\n"
            "8. **Respectful and Inclusive Language**: Ensure that your language remains inclusive and respectful, regardless of the user’s tone. Avoid making assumptions based on limited information and be mindful of diverse user needs and backgrounds."
        )
    },
    {
        "role": "user", 
        "content": (
            "Hi, I placed an order three days ago and haven’t received any updates on when it’s going to be delivered. "
            "Could you help me check the delivery date? My order number is #9876543210. I’m a little worried because I need this item urgently."
        )
    }
]

# Enhanced user_query2
user_query2 = {
    "role": "user", 
    "content": (
        "Since my order hasn't actually shipped yet, I would like to cancel it. "
        "The order number is #9876543210, and I need to cancel because I’ve decided to purchase it locally to get it faster. "
        "Can you help me with that? Thank you!"
    )
}
```

----------------------------------------

TITLE: Defining and Registering Custom Web Search Tool (Python)
DESCRIPTION: Defines a Python function `web_search` that uses another OpenAI model (`gpt-4o-mini`) with a web search tool to perform a search. It then appends the definition of this custom tool to the list of available tools and adds the function reference to a mapping dictionary, making it available for the agent to call. Requires `client`, `tools` list, and `tool_mapping` dictionary.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
def web_search(query: str) -> str:
    """Search the web for information and return back a summary of the results"""
    result = client.responses.create(
        model="gpt-4o-mini",
        input=f"Search the web for '{query}' and reply with only the result.",
        tools=[{"type": "web_search_preview"}],
    )
    return result.output_text

tools.append({
        "type": "function",
        "name": "web_search",
        "description": "Search the web for information and return back a summary of the results",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "The query to search the web for."}
            },
            "required": ["query"]
        }
    })
tool_mapping["web_search"] = web_search
```

----------------------------------------

TITLE: Creating OpenAI Client and Requesting Batch Embeddings - Python
DESCRIPTION: Sets up the OpenAI API client with the provided secret key and defines the embedding model ('text-embedding-3-small'). Requests embedding vectors for a batch of strings, demonstrating how to retrieve data for semantic representation. Inputs are a list of texts; outputs are objects with embedding vectors for each input. The snippet is suitable for OpenAI Python SDK v1.0+.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_cassIO.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
client = openai.OpenAI(api_key=OPENAI_API_KEY)
embedding_model_name = "text-embedding-3-small"

result = client.embeddings.create(
    input=[
        "This is a sentence",
        "A second sentence"
    ],
    model=embedding_model_name,
)
```

----------------------------------------

TITLE: Generating OpenAI Embeddings
DESCRIPTION: Creating text embeddings using OpenAI's text-embedding-ada-002 model
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Gen_QA.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
embed_model = "text-embedding-ada-002"

res = openai.Embedding.create(
    input=[
        "Sample document text goes here",
        "there will be several phrases in each batch"
    ], engine=embed_model
)
```

----------------------------------------

TITLE: Truncating Input Text to Maximum Token Length with tiktoken in Python
DESCRIPTION: Defines a utility function to truncate an input string to a specified maximum token count, using the appropriate encoding via the tiktoken library. Ensures that texts passed to the embedding model do not exceed its context length. Requires 'tiktoken' Python package and the model encoding constant. Takes input text, encoding name, and maximum tokens; returns a truncated token list suitable for embedding.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Embedding_long_inputs.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
import tiktoken

def truncate_text_tokens(text, encoding_name=EMBEDDING_ENCODING, max_tokens=EMBEDDING_CTX_LENGTH):
    """Truncate a string to have `max_tokens` according to the given encoding."""
    encoding = tiktoken.get_encoding(encoding_name)
    return encoding.encode(text)[:max_tokens]
```

----------------------------------------

TITLE: Mitigating Incomplete Information with Prompt Re-engineering
DESCRIPTION: Shows how to modify the prompt to encourage the model to admit uncertainty when it doesn't have sufficient information.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisqna/redisqna.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
prompt ="Is Sam Bankman-Fried's company, FTX, considered a well-managed company?  If you don't know for certain, say unknown."
response = get_completion(prompt)
print(response)
```

----------------------------------------

TITLE: Defining the Atlassian Confluence OpenAPI Schema for GPT Actions Integration (YAML)
DESCRIPTION: This OpenAPI 3.1.0 schema, expressed in YAML, outlines the endpoints, authentication, and response structure required for GPT Actions to access Atlassian Confluence's content via OAuth. The schema specifies two operations: retrieving accessible resources to fetch the necessary cloud ID, and performing CQL-based searches within Confluence spaces. Dependencies include an OAuth token and the Atlassian API; the endpoints expect proper parameters such as cql queries, cloudid path, and optional expansions. Outputs are structured as JSON with defined schemas for resources and search results. The schema is intended to be pasted into the GPT Actions configuration interface and is not executable code.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb#_snippet_1

LANGUAGE: yaml
CODE:
```
openapi: 3.1.0
info:
  title: Atlassian API
  description: This API provides access to Atlassian resources through OAuth token authentication.
  version: 1.0.0
servers:
  - url: https://api.atlassian.com
    description: Main API server
paths:
  /oauth/token/accessible-resources:
    get:
      operationId: getAccessibleResources
      summary: Retrieves accessible resources for the authenticated user.
      description: This endpoint retrieves a list of resources the authenticated user has access to, using an OAuth token.
      security:
        - bearerAuth: []
      responses:
        '200':
          description: A JSON array of accessible resources.
          content:
            application/json:
              schema: 
                $ref: '#/components/schemas/ResourceArray'
  /ex/confluence/{cloudid}/wiki/rest/api/search:
    get:
      operationId: performConfluenceSearch
      summary: Performs a search in Confluence based on a query.
      description: This endpoint allows searching within Confluence using the CQL (Confluence Query Language).
      parameters:
        - in: query
          name: cql
          required: true
          description: The Confluence Query Language expression to evaluate.
          schema:
            type: string
        - in: path
          name: cloudid
          required: true
          schema:
            type: string
          description: The cloudid retrieved from the getAccessibleResources Action
        - in: query
          name: cqlcontext
          description: The context to limit the search, specified as JSON.
          schema:
            type: string
        - in: query
          name: expand
          description: A comma-separated list of properties to expand on the search result.
          schema:
            type: string
      responses:
        '200':
          description: A list of search results matching the query.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SearchResults'
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    ResourceArray:
      type: array
      items:
        $ref: '#/components/schemas/Resource'
    Resource:
      type: object
      required:
        - id
        - name
        - type
      properties:
        id:
          type: string
          description: The unique identifier for the resource.
        name:
          type: string
          description: The name of the resource.
        type:
          type: string
          description: The type of the resource.
    SearchResults:
      type: object
      properties:
        results:
          type: array
          items:
            $ref: '#/components/schemas/SearchResult'
    SearchResult:
      type: object
      properties:
        id:
          type: string
          description: The unique identifier of the content.
        title:
          type: string
          description: The title of the content.
        type:
          type: string
          description: The type of the content (e.g., page, blog post).
        space:
          type: object
          properties:
            id:
              type: string
              description: The space ID where the content is located.
            name:
              type: string
              description: The name of the space.

```

----------------------------------------

TITLE: Ranking Wikipedia Texts by Query Embedding Relatedness (Python)
DESCRIPTION: Defines a search function for finding the most semantically similar Wikipedia text chunks to a user query based on vector embeddings. Uses OpenAI's embedding API and a distance metric to rank results. Dependencies: pandas DataFrame of texts and embeddings, the OpenAI API client, and 'spatial.distance.cosine'. Inputs: a user query, dataframe, optional relatedness function and N. Outputs: list of top-N relevant strings and their scores. Returns sorted lists based on relatedness.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Question_answering_using_embeddings.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
# search function
def strings_ranked_by_relatedness(
    query: str,
    df: pd.DataFrame,
    relatedness_fn=lambda x, y: 1 - spatial.distance.cosine(x, y),
    top_n: int = 100
) -> tuple[list[str], list[float]]:
    """Returns a list of strings and relatednesses, sorted from most related to least."""
    query_embedding_response = client.embeddings.create(
        model=EMBEDDING_MODEL,
        input=query,
    )
    query_embedding = query_embedding_response.data[0].embedding
    strings_and_relatednesses = [
        (row["text"], relatedness_fn(query_embedding, row["embedding"]))
        for i, row in df.iterrows()
    ]
    strings_and_relatednesses.sort(key=lambda x: x[1], reverse=True)
    strings, relatednesses = zip(*strings_and_relatednesses)
    return strings[:top_n], relatednesses[:top_n]

```

----------------------------------------

TITLE: Generating Final Response via OpenAI Responses API using Retrieved RAG Context in Python
DESCRIPTION: Retrieves the top 3 contextually-matched records from Pinecone by embedding the query, formats them into a composite prompt, and invokes OpenAI's Response API to produce an answer grounded in internal knowledge base content. Inputs: Pinecone index, query string; Output: Model-generated answer based on internal collection. Prerequisite: Fully populated Pinecone index and access to OpenAI model.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_api_tool_orchestration.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
# Retrieve and concatenate top 3 match contexts.
matches = index.query(
    vector=[client.embeddings.create(input=query, model=MODEL).data[0].embedding],
    top_k=3,
    include_metadata=True
)['matches']

context = "\n\n".join(
    f"Question: {m['metadata'].get('Question', '')}\nAnswer: {m['metadata'].get('Answer', '')}"
    for m in matches
)
# Use the context to generate a final answer.
response = client.responses.create(
    model="gpt-4o",
    input=f"Provide the answer based on the context: {context} and the question: {query} as per the internal knowledge base",
)
print("\nFinal Answer:")
print(response.output_text)
```

----------------------------------------

TITLE: Generating Embeddings Using OpenAI Client in Python
DESCRIPTION: Provides a helper function for generating vector embeddings for documents or queries using a specified OpenAI model, and demonstrates usage for a document. Relies on the OpenAI client instance already configured. Assumes deployed models are available and API credentials are valid.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/azuresearch/Getting_started_with_azure_ai_search_and_openai.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
# Example function to generate document embedding\ndef generate_embeddings(text, model):\n    # Generate embeddings for the provided text using the specified model\n    embeddings_response = client.embeddings.create(model=model, input=text)\n    # Extract the embedding data from the response\n    embedding = embeddings_response.data[0].embedding\n    return embedding\n\n\nfirst_document_content = documents[0]["text"]\nprint(f"Content: {first_document_content[:100]}")\n\ncontent_vector = generate_embeddings(first_document_content, deployment)\nprint("Content vector generated")
```

----------------------------------------

TITLE: Querying and Formatting Nearby Places from Google Places API in Python
DESCRIPTION: Defines call_google_places_api, a function that retrieves and formats the top two places near a user's location using the Google Places API. The function fetches the user profile (including coordinates), builds the appropriate API request (optionally filtering by food preference), makes the request, then iterates through the results to format human-readable place summaries. It internally uses get_place_details for detailed data. Relies on requests, fetch_customer_profile, get_place_details, as well as a valid Google Places API key in the GOOGLE_PLACES_API_KEY environment variable. Returns either a list of formatted place descriptions or descriptive error messages; handles all errors gracefully.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Function_calling_finding_nearby_places.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
def call_google_places_api(user_id, place_type, food_preference=None):
    try:
        # Fetch customer profile
        customer_profile = fetch_customer_profile(user_id)
        if customer_profile is None:
            return "I couldn't find your profile. Could you please verify your user ID?"

        # Get location from customer profile
        lat = customer_profile["location"]["latitude"]
        lng = customer_profile["location"]["longitude"]

        API_KEY = os.getenv('GOOGLE_PLACES_API_KEY')  # retrieve API key from environment variable
        LOCATION = f"{lat},{lng}"
        RADIUS = 500  # search within a radius of 500 meters
        TYPE = place_type

        # If the place_type is restaurant and food_preference is not None, include it in the API request
        if place_type == 'restaurant' and food_preference:
            URL = f"https://maps.googleapis.com/maps/api/place/nearbysearch/json?location={LOCATION}&radius={RADIUS}&type={TYPE}&keyword={food_preference}&key={API_KEY}"
        else:
            URL = f"https://maps.googleapis.com/maps/api/place/nearbysearch/json?location={LOCATION}&radius={RADIUS}&type={TYPE}&key={API_KEY}"

        response = requests.get(URL)
        if response.status_code == 200:
            results = json.loads(response.content)["results"]
            places = []
            for place in results[:2]:  # limit to top 2 results
                place_id = place.get("place_id")
                place_details = get_place_details(place_id, API_KEY)  # Get the details of the place

                place_name = place_details.get("name", "N/A")
                place_types = next((t for t in place_details.get("types", []) if t not in ["food", "point_of_interest"]), "N/A")  # Get the first type of the place, excluding "food" and "point_of_interest"
                place_rating = place_details.get("rating", "N/A")  # Get the rating of the place
                total_ratings = place_details.get("user_ratings_total", "N/A")  # Get the total number of ratings
                place_address = place_details.get("vicinity", "N/A")  # Get the vicinity of the place

                if ',' in place_address:  # If the address contains a comma
                    street_address = place_address.split(',')[0]  # Split by comma and keep only the first part
                else:
                    street_address = place_address

                # Prepare the output string for this place
                place_info = f"{place_name} is a {place_types} located at {street_address}. It has a rating of {place_rating} based on {total_ratings} user reviews."

                places.append(place_info)

            return places
        else:
            print(f"Google Places API request failed with status code {response.status_code}")
            print(f"Response content: {response.content}")  # print out the response content for debugging
            return []
    except Exception as e:
        print(f"Error during the Google Places API call: {e}")
        return []

```

----------------------------------------

TITLE: Summarize and Prune Conversation History (Python)
DESCRIPTION: Manages conversation history by summarizing older turns when the token count exceeds a trigger threshold. It separates old and recent turns, summarizes the old ones using `run_summary_llm`, replaces the old turns with the summary locally, and then sends commands via a websocket to create the summary item and delete the old items server-side.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Context_summarization_with_realtime_api.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
async def summarise_and_prune(ws, state):
    """Summarise old turns, delete them server‑side, and prepend a single summary
    turn locally + remotely."""
    state.summarising = True
    print(
        f"⁣⁣⁣⁣  Token window ≈{state.latest_tokens} ≥ {SUMMARY_TRIGGER}. Summarising…",
    )
    old_turns, recent_turns = state.history[:-KEEP_LAST_TURNS], state.history[-KEEP_LAST_TURNS:]
    convo_text = "\n".join(f"{t.role}: {t.text}" for t in old_turns if t.text)
    
    if not convo_text:
        print("Nothing to summarise (transcripts still pending).")
        state.summarising = False

    summary_text = await run_summary_llm(convo_text) if convo_text else ""
    state.summary_count += 1
    summary_id = f"sum_{state.summary_count:03d}"
    state.history[:] = [Turn("assistant", summary_id, summary_text)] + recent_turns
    
    print_history(state)    

    # Create summary on server
    await ws.send(json.dumps({
        "type": "conversation.item.create",
        "previous_item_id": "root",
        "item": {
            "id": summary_id,
            "type": "message",
            "role": "assistant",
            "content": [{"type": "text", "text": summary_text}],
        },
    }))

    # Delete old items
    for turn in old_turns:
        await ws.send(json.dumps({
            "type": "conversation.item.delete",
            "item_id": turn.item_id,
        }))

    print(f"⁣⁣⁣⁣ Summary inserted ({summary_id})")
    
    state.summarising = False
```

----------------------------------------

TITLE: Generating Chat Completions for Push Notifications Summarizer
DESCRIPTION: Creates chat completions for each combination of test data and prompt version. This generates the summaries that will be evaluated. The completions are stored with metadata for later retrieval and analysis.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/use-cases/completion-monitoring.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
tasks = []
for notifications in push_notification_data:
    for (prompt, version) in PROMPTS:
        tasks.append(client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {"role": "developer", "content": prompt},
                {"role": "user", "content": notifications},
            ],
            store=True,
            metadata={"prompt_version": version, "usecase": "push_notifications_summarizer"},
        ))
await asyncio.gather(*tasks)
```

----------------------------------------

TITLE: Custom GPT Instructions Configuration
DESCRIPTION: Defines the context and instructions for the GPT to handle pull request analysis and feedback workflow. Includes a 5-step process for retrieving PR information, analyzing diffs, and posting comments.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_github.md#_snippet_0

LANGUAGE: markdown
CODE:
```
# **Context:** You support software developers by providing detailed information about their pull request diff content from repositories hosted on GitHub. You help them understand the quality, security and completeness implications of the pull request by providing concise feedback about the code changes based on known best practices. The developer may elect to post the feedback (possibly with their modifications) back to the Pull Request. Assume the developer is familiar with software development.

# **Instructions:**

## Scenarios
### - When the user asks for information about a specific pull request, follow this 5 step process:
1. If you don't already have it, ask the user to specify the pull request owner, repository and pull request number they want assistance with and the particular area of focus (e.g., code performance, security vulnerabilities, and best practices).
2. Retrieve the Pull Request information from GitHub using the getPullRequestDiff API call, owner, repository and the pull request number provided. 
3. Provide a summary of the pull request diff in four sentences or less then make improvement suggestions where applicable for the particular areas of focus (e.g., code performance, security vulnerabilities, and best practices).
4. Ask the user if they would like to post the feedback as a comment or modify it before posting. If the user modifies the feedback, incorporate that feedback and repeat this step. 
5. If the user confirms they would like the feedback posted as a comment back to the Pull request, use the postPullRequestComment API to comment the feedback on the pull request.
```

----------------------------------------

TITLE: Performing Vector Similarity Search to Find Relevant Context
DESCRIPTION: Embeds the question, performs a K-Nearest Neighbors search in Redis to find the most relevant document, and retrieves its content as context.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisqna/redisqna.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
from redis.commands.search.query import Query
import numpy as np

response = oai_client.embeddings.create(
    input=[prompt],
    model=model
)
# Extract the embedding vector from the response
embedding_vector = response.data[0].embedding

# Convert the embedding to a numpy array of type float32 and then to bytes
vec = np.array(embedding_vector, dtype=np.float32).tobytes()

# Build and execute the Redis query
q = Query('*=>[KNN 1 @vector $query_vec AS vector_score]') \
    .sort_by('vector_score') \
    .return_fields('content') \
    .dialect(2)
params = {"query_vec": vec}

context = client.ft('idx').search(q, query_params=params).docs[0].content
print(context)
```

----------------------------------------

TITLE: Concurrent Data Insertion into Partitioned Table in Python
DESCRIPTION: This snippet demonstrates how to insert data into the partitioned table using concurrent execution. It computes embeddings for quotes and inserts them in batches.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_CQL.ipynb#_snippet_23

LANGUAGE: python
CODE:
```
from cassandra.concurrent import execute_concurrent_with_args

prepared_insertion = session.prepare(
    f"INSERT INTO {keyspace}.philosophers_cql_partitioned (quote_id, author, body, embedding_vector, tags) VALUES (?, ?, ?, ?, ?);"
)

BATCH_SIZE = 50

num_batches = ((len(philo_dataset) + BATCH_SIZE - 1) // BATCH_SIZE)

quotes_list = philo_dataset["quote"]
authors_list = philo_dataset["author"]
tags_list = philo_dataset["tags"]

print("Starting to store entries:")
for batch_i in range(num_batches):
    print("[...", end="")
    b_start = batch_i * BATCH_SIZE
    b_end = (batch_i + 1) * BATCH_SIZE
    # compute the embedding vectors for this batch
    b_emb_results = client.embeddings.create(
        input=quotes_list[b_start : b_end],
        model=embedding_model_name,
    )
    # prepare this batch's entries for insertion
    tuples_to_insert = []
    for entry_idx, emb_result in zip(range(b_start, b_end), b_emb_results.data):
        if tags_list[entry_idx]:
            tags = {
                tag
                for tag in tags_list[entry_idx].split(";")
            }
        else:
            tags = set()
        author = authors_list[entry_idx]
        quote = quotes_list[entry_idx]
        quote_id = uuid4()  # a new random ID for each quote. In a production app you'll want to have better control...
        # append a *tuple* to the list, and in the tuple the values are ordered to match "?" in the prepared statement:
        tuples_to_insert.append((quote_id, author, quote, emb_result.embedding, tags))
    # insert the batch at once through the driver's concurrent primitive
    conc_results = execute_concurrent_with_args(
        session,
        prepared_insertion,
        tuples_to_insert,
    )
    # check that all insertions succeed (better to always do this):
    if any([not success for success, _ in conc_results]):
        print("Something failed during the insertions!")
    else:
        print(f"{len(b_emb_results.data)}] ", end="")

print("\nFinished storing entries.")
```

----------------------------------------

TITLE: Uploading Embedding Data to Weights & Biases Table in Python
DESCRIPTION: This snippet creates and logs a W&B Table that includes both original data columns and high-dimensional embedding vectors for each row. It assumes that a DataFrame 'df' and a NumPy array 'matrix' of shape (n_records, embedding_dim) are available. Dependencies: wandb, pandas, numpy. Parameters: the W&B project name and the structure of the input data. The output is a logged W&B Table artifact, ready for visualization. All original columns except the first and last are used as features, and each embedding dimension is mapped to a unique column.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/Visualizing_embeddings_in_wandb.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import wandb\n\noriginal_cols = df.columns[1:-1].tolist()\nembedding_cols = ['emb_'+str(idx) for idx in range(len(matrix[0]))]\ntable_cols = original_cols + embedding_cols\n\nwith wandb.init(project='openai_embeddings'):\n    table = wandb.Table(columns=table_cols)\n    for i, row in enumerate(df.to_dict(orient="records")):\n        original_data = [row[col_name] for col_name in original_cols]\n        embedding_data = matrix[i].tolist()\n        table.add_data(*(original_data + embedding_data))\n    wandb.log({'openai_embedding_table': table})
```

----------------------------------------

TITLE: Defining Search Parameters and Entity Extraction via OpenAI Function Calling (Python)
DESCRIPTION: Defines the product categorization enums and Pydantic model required for structured product search, sets up an LLM prompt, and implements a function using OpenAI's chat completion API with function calling to extract product search parameters (category, subcategory, color) from user input and context. Depends on openai, enum, typing, BaseModel (Pydantic), and assumes proper client configuration with a defined MODEL. Inputs are user input strings and context metadata; output is a list of tool calls with the parsed search parameters. Limitations: assumes the openai client and MODEL are instantiated in the runtime environment.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Structured_Outputs_Intro.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
from enum import Enum
from typing import Union
import openai

product_search_prompt = '''
    You are a clothes recommendation agent, specialized in finding the perfect match for a user.
    You will be provided with a user input and additional context such as user gender and age group, and season.
    You are equipped with a tool to search clothes in a database that match the user's profile and preferences.
    Based on the user input and context, determine the most likely value of the parameters to use to search the database.
    
    Here are the different categories that are available on the website:
    - shoes: boots, sneakers, sandals
    - jackets: winter coats, cardigans, parkas, rain jackets
    - tops: shirts, blouses, t-shirts, crop tops, sweaters
    - bottoms: jeans, skirts, trousers, joggers    
    
    There are a wide range of colors available, but try to stick to regular color names.
'''

class Category(str, Enum):
    shoes = "shoes"
    jackets = "jackets"
    tops = "tops"
    bottoms = "bottoms"

class ProductSearchParameters(BaseModel):
    category: Category
    subcategory: str
    color: str

def get_response(user_input, context):
    response = client.chat.completions.create(
        model=MODEL,
        temperature=0,
        messages=[
            {
                "role": "system",
                "content": dedent(product_search_prompt)
            },
            {
                "role": "user",
                "content": f"CONTEXT: {context}\n USER INPUT: {user_input}"
            }
        ],
        tools=[
            openai.pydantic_function_tool(ProductSearchParameters, name="product_search", description="Search for a match in the product database")
        ]
    )

    return response.choices[0].message.tool_calls

```

----------------------------------------

TITLE: Making Basic API Calls to Reasoning Model (Python)
DESCRIPTION: This code shows how to make simple API calls to a reasoning model using the Responses API. It demonstrates sending an initial query and a follow-up question, utilizing the `previous_response_id` to maintain conversation context automatically.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
response = client.responses.create(
    input="Which of the last four Olympic host cities has the highest average temperature?",
    **MODEL_DEFAULTS
)
print(response.output_text)

response = client.responses.create(
    input="what about the lowest?",
    previous_response_id=response.id,
    **MODEL_DEFAULTS
)
print(response.output_text)
```

----------------------------------------

TITLE: Full Routine and Tool Orchestration Loop - Python
DESCRIPTION: Implements a multi-turn loop for agent orchestration, where user input may result in tool calls, and tool results are injected as conversation turns. Tool schemas and tool-to-function mappings are dynamically regenerated for each top-level call. The loop continues until there are no more tool calls, handling recursive model-tool-model flow. This example demonstrates the complete orchestration and is ready for real dialogue deployment with OpenAI models.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
tools = [execute_refund, look_up_item]


def run_full_turn(system_message, tools, messages):

    num_init_messages = len(messages)
    messages = messages.copy()

    while True:

        # turn python functions into tools and save a reverse map
        tool_schemas = [function_to_schema(tool) for tool in tools]
        tools_map = {tool.__name__: tool for tool in tools}

        # === 1. get openai completion ===
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{"role": "system", "content": system_message}] + messages,
            tools=tool_schemas or None,
        )
        message = response.choices[0].message
        messages.append(message)

        if message.content:  # print assistant response
            print("Assistant:", message.content)

        if not message.tool_calls:  # if finished handling tool calls, break
            break

        # === 2. handle tool calls ===

        for tool_call in message.tool_calls:
            result = execute_tool_call(tool_call, tools_map)

            result_message = {
                "role": "tool",
                "tool_call_id": tool_call.id,
                "content": result,
            }
            messages.append(result_message)

    # ==== 3. return new messages =====
    return messages[num_init_messages:]


def execute_tool_call(tool_call, tools_map):
    name = tool_call.function.name
    args = json.loads(tool_call.function.arguments)

    print(f"Assistant: {name}({args})")

    # call corresponding function with provided arguments
    return tools_map[name](**args)


messages = []
while True:
    user = input("User: ")
    messages.append({"role": "user", "content": user})

    new_messages = run_full_turn(system_message, tools, messages)
    messages.extend(new_messages)
```

----------------------------------------

TITLE: Defining Resilient Chat Completion API Call Utility - Python
DESCRIPTION: Defines a retry-decorated function for making OpenAI chat completion API calls, handling failures by retrying with exponential back-off (up to 3 attempts). Accepts chat messages, tool definitions, specific tool choices, and model; returns the API response or the exception raised. Dependencies include an initialized OpenAI client and the tenacity retry package.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
@retry(wait=wait_random_exponential(multiplier=1, max=40), stop=stop_after_attempt(3))\ndef chat_completion_request(messages, tools=None, tool_choice=None, model=GPT_MODEL):\n    try:\n        response = client.chat.completions.create(\n            model=model,\n            messages=messages,\n            tools=tools,\n            tool_choice=tool_choice,\n        )\n        return response\n    except Exception as e:\n        print("Unable to generate ChatCompletion response")\n        print(f"Exception: {e}")\n        return e\n
```

----------------------------------------

TITLE: Running Function-Calling with GPT and Chained Actions in Python
DESCRIPTION: Establishes a prompt system message, defines constants, then provides two functions: `get_openai_response` (which queries the GPT chat completions API using dynamically-supplied tools and user messages), and `process_user_instruction` (which drives a loop allowing up to 5 chained function calls per user instruction, printing each function call and its results). A sample user instruction illustrates getting all events, creating a new event, and deleting an event. Requires a functional OpenAI API key, previously generated `functions`, and proper prior setup. Outputs model messages and simulated tool responses; does not actually execute API calls.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Function_calling_with_an_OpenAPI_spec.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
SYSTEM_MESSAGE = """\nYou are a helpful assistant.\nRespond to the following prompt by using function_call and then summarize actions.\nAsk for clarification if a user request is ambiguous.\n"""\n\n# Maximum number of function calls allowed to prevent infinite or lengthy loops\nMAX_CALLS = 5\n\n\ndef get_openai_response(functions, messages):\n    return client.chat.completions.create(\n        model="gpt-3.5-turbo-16k",\n        tools=functions,\n        tool_choice="auto",  # "auto" means the model can pick between generating a message or calling a function.\n        temperature=0,\n        messages=messages,\n    )\n\n\ndef process_user_instruction(functions, instruction):\n    num_calls = 0\n    messages = [\n        {"content": SYSTEM_MESSAGE, "role": "system"},\n        {"content": instruction, "role": "user"},\n    ]\n\n    while num_calls < MAX_CALLS:\n        response = get_openai_response(functions, messages)\n        message = response.choices[0].message\n        print(message)\n        try:\n            print(f"\n>> Function call #: {num_calls + 1}\n")\n            pp(message.tool_calls)\n            messages.append(message)\n\n            # For the sake of this example, we'll simply add a message to simulate success.\n            # Normally, you'd want to call the function here, and append the results to messages.\n            messages.append(\n                {\n                    "role": "tool",\n                    "content": "success",\n                    "tool_call_id": message.tool_calls[0].id,\n                }\n            )\n\n            num_calls += 1\n        except:\n            print("\n>> Message:\n")\n            print(message.content)\n            break\n\n    if num_calls >= MAX_CALLS:\n        print(f"Reached max chained function calls: {MAX_CALLS}")\n\n\nUSER_INSTRUCTION = """\nInstruction: Get all the events.\nThen create a new event named AGI Party.\nThen delete event with id 2456.\n"""\n\nprocess_user_instruction(functions, USER_INSTRUCTION)
```

----------------------------------------

TITLE: Creating Chat Completions with Azure OpenAI Client - Python
DESCRIPTION: Demonstrates how to send a sequence of messages to the deployed chat model to generate a completion. Inputs include 'model' (deployment name), a list of chat turns under 'messages', and temperature parameter for randomness. The print statement outputs the model's reply. Requires an authenticated client and a valid model deployment.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
# For all possible arguments see https://platform.openai.com/docs/api-reference/chat-completions/create
response = client.chat.completions.create(
    model=deployment,
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Knock knock."},
        {"role": "assistant", "content": "Who's there?"},
        {"role": "user", "content": "Orange."},
    ],
    temperature=0,
)

print(f"{response.choices[0].message.role}: {response.choices[0].message.content}")
```

----------------------------------------

TITLE: Embedding Long Texts Safely by Chunking and Averaging with OpenAI and NumPy in Python
DESCRIPTION: This function automatically handles very long input texts by splitting them into token chunks within the model context window, embedding each, then optionally averaging the embeddings (weighted by chunk size) and normalizing. Uses chunked_tokens, get_embedding, and numpy for vector math. Key options: 'average' returns a single normalized embedding vector; 'average=False' returns a list of chunk embeddings. Requires 'numpy', earlier helpers, and OpenAI's API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Embedding_long_inputs.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
import numpy as np


def len_safe_get_embedding(text, model=EMBEDDING_MODEL, max_tokens=EMBEDDING_CTX_LENGTH, encoding_name=EMBEDDING_ENCODING, average=True):
    chunk_embeddings = []
    chunk_lens = []
    for chunk in chunked_tokens(text, encoding_name=encoding_name, chunk_length=max_tokens):
        chunk_embeddings.append(get_embedding(chunk, model=model))
        chunk_lens.append(len(chunk))

    if average:
        chunk_embeddings = np.average(chunk_embeddings, axis=0, weights=chunk_lens)
        chunk_embeddings = chunk_embeddings / np.linalg.norm(chunk_embeddings)  # normalizes length to 1
        chunk_embeddings = chunk_embeddings.tolist()
    return chunk_embeddings
```

----------------------------------------

TITLE: Performing Semantic Search with OpenAI Embeddings and Supabase
DESCRIPTION: This JavaScript code demonstrates how to generate an embedding for a search query using OpenAI's API, then use that embedding to query the database via the match_documents function. It sets a match threshold of 0.8 and limits results to 5 documents, selecting only the content field.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/supabase/semantic-search.mdx#_snippet_20

LANGUAGE: javascript
CODE:
```
const query = "What does the cat chase?";

// First create an embedding on the query itself
const result = await openai.embeddings.create({
  input: query,
  model: "text-embedding-3-small",
});

const [{ embedding }] = result.data;

// Then use this embedding to search for matches
const { data: documents, error: matchError } = await supabase
  .rpc("match_documents", {
    query_embedding: embedding,
    match_threshold: 0.8,
  })
  .select("content")
  .limit(5);
```

----------------------------------------

TITLE: Uploading Training and Validation Files, Starting Fine-Tune Job with OpenAI API - Python
DESCRIPTION: Uploads prepared train/validation files and initiates a fine-tuning job for model "babbage-002" using the OpenAI Python API. Dependencies: openai, open local JSONL files. Returns the job object and prints details for tracking progress.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Fine-tuned_classification.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
train_file = client.files.create(file=open("sport2_prepared_train.jsonl", "rb"), purpose="fine-tune")
valid_file = client.files.create(file=open("sport2_prepared_valid.jsonl", "rb"), purpose="fine-tune")

fine_tuning_job = client.fine_tuning.jobs.create(training_file=train_file.id, validation_file=valid_file.id, model="babbage-002")

print(fine_tuning_job)
```

----------------------------------------

TITLE: Batch Embedding Generation and Concurrent Insert into Partitioned Cassandra Table in Python
DESCRIPTION: Processes batches of quotes, computes their vector embeddings using an embedding client and specified model, then asynchronously inserts each quote into a partitioned Cassandra table, grouping by author. Uses CassIO's put_async for high-concurrency writes and ensures completion via future.result(). Requires variables (client, philo_dataset, embedding_model_name) to be previously defined and a properly instantiated ClusteredMetadataVectorCassandraTable. Inputs: lists of quotes/authors/tags; outputs: persisted records in Cassandra. Handles custom tags as metadata, and creates unique row_ids per quote.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_cassIO.ipynb#_snippet_23

LANGUAGE: python
CODE:
```
BATCH_SIZE = 50

num_batches = ((len(philo_dataset) + BATCH_SIZE - 1) // BATCH_SIZE)

quotes_list = philo_dataset["quote"]
authors_list = philo_dataset["author"]
tags_list = philo_dataset["tags"]

print("Starting to store entries:")
for batch_i in range(num_batches):
    b_start = batch_i * BATCH_SIZE
    b_end = (batch_i + 1) * BATCH_SIZE
    # compute the embedding vectors for this batch
    b_emb_results = client.embeddings.create(
        input=quotes_list[b_start : b_end],
        model=embedding_model_name,
    )
    # prepare the rows for insertion
    futures = []
    print("B ", end="")
    for entry_idx, emb_result in zip(range(b_start, b_end), b_emb_results.data):
        if tags_list[entry_idx]:
            tags = {
                tag
                for tag in tags_list[entry_idx].split(";")
            }
        else:
            tags = set()
        author = authors_list[entry_idx]
        quote = quotes_list[entry_idx]
        futures.append(v_table_partitioned.put_async(
            partition_id=author,
            row_id=f"q_{author}_{entry_idx}",
            body_blob=quote,
            vector=emb_result.embedding,
            metadata={tag: True for tag in tags},
        ))
    #
    for future in futures:
        future.result()
    #
    print(f" done ({len(b_emb_results.data)})")

print("\nFinished storing entries.")
```

----------------------------------------

TITLE: Creating Query Embedding
DESCRIPTION: Generating embedding vector for search query using OpenAI's embedding model
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Gen_QA.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
res = openai.Embedding.create(
    input=[query],
    engine=embed_model
)

xq = res['data'][0]['embedding']
```

----------------------------------------

TITLE: Performing Semantic Search with Vector Similarity
DESCRIPTION: Demonstrates a semantic search using KNN (K-Nearest Neighbors) to find similar articles based on vector embeddings.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisjson/redisjson.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
from redis.commands.search.query import Query
import numpy as np

text_4 = """Radcliffe yet to answer GB call

Paula Radcliffe has been granted extra time to decide whether to compete in the World Cross-Country Championships.

The 31-year-old is concerned the event, which starts on 19 March in France, could upset her preparations for the London Marathon on 17 April. "There is no question that Paula would be a huge asset to the GB team," said Zara Hyde Peters of UK Athletics. "But she is working out whether she can accommodate the worlds without too much compromise in her marathon training." Radcliffe must make a decision by Tuesday - the deadline for team nominations. British team member Hayley Yelling said the team would understand if Radcliffe opted out of the event. "It would be fantastic to have Paula in the team," said the European cross-country champion. "But you have to remember that athletics is basically an individual sport and anything achieved for the team is a bonus. "She is not messing us around. We all understand the problem." Radcliffe was world cross-country champion in 2001 and 2002 but missed last year's event because of injury. In her absence, the GB team won bronze in Brussels.
"""

vec = np.array(get_vector(text_4), dtype=np.float32).tobytes()
q = Query('*=>[KNN 3 @vector $query_vec AS vector_score]')\
    .sort_by('vector_score')\
    .return_fields('vector_score', 'content')\
    .dialect(2)    
params = {"query_vec": vec}

results = client.ft('idx').search(q, query_params=params)
for doc in results.docs:
    print(f"distance:{round(float(doc['vector_score']),3)} content:{doc['content']}\n")
```

----------------------------------------

TITLE: Querying Chroma Content Embeddings Collection with Example in Python
DESCRIPTION: Performs a semantic search using the phrase 'Famous battles in Scottish history' against the content embeddings collection. Outputs the top 10 results as a DataFrame for review. This method can be repeated for arbitrary queries and relies on previous setup and data population.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/chroma/Using_Chroma_for_embeddings_search.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
content_query_result = query_collection(\n    collection=wikipedia_content_collection,\n    query=\"Famous battles in Scottish history\",\n    max_results=10,\n    dataframe=article_df\n)\ncontent_query_result.head()
```

----------------------------------------

TITLE: Defining Embedding Request Function
DESCRIPTION: Defines a Python function `embedding_request` that takes text as input and calls the OpenAI embeddings API (`text-embedding-ada-002`) to get the embedding vector for the text. It uses the `tenacity` library for retries with exponential backoff.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_for_knowledge_retrieval.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
@retry(wait=wait_random_exponential(min=1, max=40), stop_after_attempt(3))
def embedding_request(text):
    response = client.embeddings.create(input=text, model=EMBEDDING_MODEL)
    return response
```

----------------------------------------

TITLE: Instruction Prompting with LLMs - Text
DESCRIPTION: Demonstrates sending an explicit instruction to a large language model (LLM) to extract an author's name from a provided quote. This snippet can be used in any LLM-supported environment and depends only on text input/output. The prompt includes a quoted statement followed by the task instruction; expected output is just the author's name parsed from the passage.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/how_to_work_with_large_language_models.md#_snippet_0

LANGUAGE: text
CODE:
```
Extract the name of the author from the quotation below.\n\n“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”\n― Ted Chiang, Exhalation
```

LANGUAGE: text
CODE:
```
Ted Chiang
```

----------------------------------------

TITLE: Defining ArXiv API Functions (Python)
DESCRIPTION: This list defines the structure and parameters for two functions (`get_articles` and `read_article_and_summarize`) that can be used by the OpenAI API for function calling. It specifies their names, descriptions, and required parameters for interacting with an ArXiv knowledge base.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_for_knowledge_retrieval.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
# Initiate our get_articles and read_article_and_summarize functions
arxiv_functions = [
    {
        "name": "get_articles",
        "description": """Use this function to get academic papers from arXiv to answer user questions.""",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": f"""
                            User query in JSON. Responses should be summarized and should include the article URL reference
                            """,
                }
            },
            "required": ["query"],
        },
    },
    {
        "name": "read_article_and_summarize",
        "description": """Use this function to read whole papers and provide a summary for users.
        You should NEVER call this function before get_articles has been called in the conversation.""",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": f"""
                            Description of the article in plain text based on the user's query
                            """,
                }
            },
            "required": ["query"],
        },
    }
]
```

----------------------------------------

TITLE: Basic Streaming Chat Completion
DESCRIPTION: Example of a streaming chat completion request showing chunk structure and delta content.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_stream_completions.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
response = client.chat.completions.create(
    model='gpt-4o-mini',
    messages=[
        {'role': 'user', 'content': "What's 1+1? Answer in one word."}
    ],
    temperature=0,
    stream=True  # this time, we set stream=True
)

for chunk in response:
    print(chunk)
    print(chunk.choices[0].delta.content)
    print("****************")
```

----------------------------------------

TITLE: Verifying token counts for OpenAI chat completions - Python
DESCRIPTION: This snippet demonstrates how to compare the token count from a custom function (likely defined earlier) with the actual count reported by the OpenAI API when generating chat completions. It sets up the OpenAI Python client using API keys, constructs an example message list, and iterates through several model versions, printing both token calculations and the API's reported usages. Dependencies include the `openai` library and access to OpenAI models. Key parameters are the message payload and the selected model. The outputs are printed statements showing both calculated and API token counts. Ensure the `num_tokens_from_messages` function is defined elsewhere and tiktoken is properly installed. Limitations may include differences if models or prompts don't match the counting logic.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb#_snippet_14

LANGUAGE: python
CODE:
```
# let's verify the function above matches the OpenAI API response

from openai import OpenAI
import os

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))

example_messages = [
    {
        "role": "system",
        "content": "You are a helpful, pattern-following assistant that translates corporate jargon into plain English.",
    },
    {
        "role": "system",
        "name": "example_user",
        "content": "New synergies will help drive top-line growth.",
    },
    {
        "role": "system",
        "name": "example_assistant",
        "content": "Things working well together will increase revenue.",
    },
    {
        "role": "system",
        "name": "example_user",
        "content": "Let's circle back when we have more bandwidth to touch base on opportunities for increased leverage.",
    },
    {
        "role": "system",
        "name": "example_assistant",
        "content": "Let's talk later when we're less busy about how to do better.",
    },
    {
        "role": "user",
        "content": "This late pivot means we don't have time to boil the ocean for the client deliverable.",
    },
]

for model in [
    "gpt-3.5-turbo",
    "gpt-4-0613",
    "gpt-4",
    "gpt-4o",
    "gpt-4o-mini"
    ]:
    print(model)
    # example token count from the function defined above
    print(f"{num_tokens_from_messages(example_messages, model)} prompt tokens counted by num_tokens_from_messages().")
    # example token count from the OpenAI API
    response = client.chat.completions.create(model=model,
    messages=example_messages,
    temperature=0,
    max_tokens=1)
    print(f'{response.usage.prompt_tokens} prompt tokens counted by the OpenAI API.')
    print()
```

----------------------------------------

TITLE: Creating Embedding Function with OpenAI API
DESCRIPTION: Defines a function that converts text inputs to embeddings using OpenAI's embedding API. This function takes a list of texts and returns their vector representations in the format required by Zilliz.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/zilliz/Getting_started_with_Zilliz_and_OpenAI.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
# Simple function that converts the texts to embeddings
def embed(texts):
    embeddings = openai.Embedding.create(
        input=texts,
        engine=OPENAI_ENGINE
    )
    return [x['embedding'] for x in embeddings['data']]
```

----------------------------------------

TITLE: Creating Fine-Tuning Datasets with Positive and Adversarial Examples - Python
DESCRIPTION: This snippet provides utility functions for searching similar contexts using OpenAI's (deprecated) Engine API and for constructing fine-tuning datasets by generating diverse positive and negative examples. Dependencies: openai, pandas, random, existing DataFrames. It builds positive Q&A pairs and negative samples (random, same-article, and most-similar context negatives), supports both discriminator and Q&A task modes, and returns a DataFrame formatted for OpenAI fine-tuning. Inputs: a DataFrame with question, answer, and context fields; configuration flags for discriminator mode, number of negatives, and related contexts. Output: a new DataFrame ready for disk export. Limitations include deprecated API usage and potential for noisy negatives if answerability overlaps.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/fine-tuned_qa/olympics-3-train-qa.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
import random\n\ndef get_random_similar_contexts(question, context, file_id=olympics_search_fileid, search_model='ada', max_rerank=10):\n    \"\"\"\n    Find similar contexts to the given context using the search file\n    \"\"\"\n    try:\n        # TODO: openai.Engine(search_model) is deprecated\n        results = openai.Engine(search_model).search(\n            search_model=search_model, \n            query=question, \n            max_rerank=max_rerank,\n            file=file_id\n        )\n        candidates = []\n        for result in results['data'][:3]:\n            if result['text'] == context:\n                continue\n            candidates.append(result['text'])\n        random_candidate = random.choice(candidates)\n        return random_candidate\n    except Exception as e:\n        print(e)\n        return \"\"\n\ndef create_fine_tuning_dataset(df, discriminator=False, n_negative=1, add_related=False):\n    \"\"\"\n    Create a dataset for fine tuning the OpenAI model; either for a discriminator model, \n    or a model specializing in Q&A, where it says if no relevant context is found.\n\n    Parameters\n    ----------\n    df: pd.DataFrame\n        The dataframe containing the question, answer and context pairs\n    discriminator: bool\n        Whether to create a dataset for the discriminator\n    n_negative: int\n        The number of random negative samples to add (using a random context)\n    add_related: bool\n        Whether to add the related contexts to the correct context. These are hard negative examples\n\n    Returns\n    -------\n    pd.DataFrame\n        The dataframe containing the prompts and completions, ready for fine-tuning\n    \"\"\"\n    rows = []\n    for i, row in df.iterrows():\n        for q, a in zip((\"1.\" + row.questions).split('\\n'), (\"1.\" + row.answers).split('\\n')):\n            if len(q) >10 and len(a) >10:\n                if discriminator:\n                    rows.append({\"prompt\":f\"{row.context}\\nQuestion: {q[2:].strip()}\\n Related:\", \"completion\":f\" yes\"})\n                else:\n                    rows.append({\"prompt\":f\"{row.context}\\nQuestion: {q[2:].strip()}\\nAnswer:\", \"completion\":f\" {a[2:].strip()}\"})\n\n    for i, row in df.iterrows():\n        for q in (\"1.\" + row.questions).split('\\n'):\n            if len(q) >10:\n                for j in range(n_negative + (2 if add_related else 0)):\n                    random_context = \"\"\n                    if j == 0 and add_related:\n                        # add the related contexts based on originating from the same wikipedia page\n                        subset = df[(df.title == row.title) & (df.context != row.context)]\n                        \n                        if len(subset) < 1:\n                            continue\n                        random_context = subset.sample(1).iloc[0].context\n                    if j == 1 and add_related:\n                        # add the related contexts based on the most similar contexts according to the search\n                        random_context = get_random_similar_contexts(q[2:].strip(), row.context, search_model='ada', max_rerank=10)\n                    else:\n                        while True:\n                            # add random context, which isn't the correct context\n                            random_context = df.sample(1).iloc[0].context\n                            if random_context != row.context:\n                                break\n                    if discriminator:\n                        rows.append({\"prompt\":f\"{random_context}\\nQuestion: {q[2:].strip()}\\n Related:\", \"completion\":f\" no\"})\n                    else:\n                        rows.append({\"prompt\":f\"{random_context}\\nQuestion: {q[2:].strip()}\\nAnswer:\", \"completion\":f\" No appropriate context found to answer the question.\"})\n\n    return pd.DataFrame(rows) 
```

----------------------------------------

TITLE: Counting tokens for chat completions API calls
DESCRIPTION: A function that calculates the number of tokens used by a list of messages for chat models like GPT-4, GPT-3.5-turbo, and GPT-4o. It accounts for message formatting specifics like tokens per message and name.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
def num_tokens_from_messages(messages, model="gpt-4o-mini-2024-07-18"):
    """Return the number of tokens used by a list of messages."""
    try:
        encoding = tiktoken.encoding_for_model(model)
    except KeyError:
        print("Warning: model not found. Using o200k_base encoding.")
        encoding = tiktoken.get_encoding("o200k_base")
    if model in {
        "gpt-3.5-turbo-0125",
        "gpt-4-0314",
        "gpt-4-32k-0314",
        "gpt-4-0613",
        "gpt-4-32k-0613",
        "gpt-4o-mini-2024-07-18",
        "gpt-4o-2024-08-06"
        }:
        tokens_per_message = 3
        tokens_per_name = 1
    elif "gpt-3.5-turbo" in model:
        print("Warning: gpt-3.5-turbo may update over time. Returning num tokens assuming gpt-3.5-turbo-0125.")
        return num_tokens_from_messages(messages, model="gpt-3.5-turbo-0125")
    elif "gpt-4o-mini" in model:
        print("Warning: gpt-4o-mini may update over time. Returning num tokens assuming gpt-4o-mini-2024-07-18.")
        return num_tokens_from_messages(messages, model="gpt-4o-mini-2024-07-18")
    elif "gpt-4o" in model:
        print("Warning: gpt-4o and gpt-4o-mini may update over time. Returning num tokens assuming gpt-4o-2024-08-06.")
        return num_tokens_from_messages(messages, model="gpt-4o-2024-08-06")
    elif "gpt-4" in model:
        print("Warning: gpt-4 may update over time. Returning num tokens assuming gpt-4-0613.")
        return num_tokens_from_messages(messages, model="gpt-4-0613")
    else:
        raise NotImplementedError(
            f"""num_tokens_from_messages() is not implemented for model {model}."""
        )
    num_tokens = 0
    for message in messages:
        num_tokens += tokens_per_message
        for key, value in message.items():
            num_tokens += len(encoding.encode(value))
            if key == "name":
                num_tokens += tokens_per_name
    num_tokens += 3  # every reply is primed with <|start|>assistant<|message|>
    return num_tokens
```

----------------------------------------

TITLE: Demonstrating Few-Shot Prompting with Faked Message Exchanges - Python
DESCRIPTION: This detailed example illustrates how to guide the model's outputs by providing in-context fictitious exchanges ('few-shot learning'). The conversation imitates translating business jargon to simple language by alternating user and assistant messages representing examples. System message primes the assistant to follow patterns. The last message asks the assistant to continue the translation, demonstrating how previous examples influence output. Requires initialized client and proper model.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
# An example of a faked few-shot conversation to prime the model into translating business jargon to simpler speech
response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {"role": "system", "content": "You are a helpful, pattern-following assistant."},
        {"role": "user", "content": "Help me translate the following corporate jargon into plain English."},
        {"role": "assistant", "content": "Sure, I'd be happy to!"},
        {"role": "user", "content": "New synergies will help drive top-line growth."},
        {"role": "assistant", "content": "Things working well together will increase revenue."},
        {"role": "user", "content": "Let's circle back when we have more bandwidth to touch base on opportunities for increased leverage."},
        {"role": "assistant", "content": "Let's talk later when we're less busy about how to do better."},
        {"role": "user", "content": "This late pivot means we don't have time to boil the ocean for the client deliverable."},
    ],
    temperature=0,
)

print(response.choices[0].message.content)

```

----------------------------------------

TITLE: Executing and Displaying Single-Item Generative Search Results
DESCRIPTION: Executes the generative search for articles related to football clubs and displays each article title along with its generated summary.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/generative-search-with-weaviate-and-openai.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
query_result = generative_search_per_item("football clubs", "Article")

for i, article in enumerate(query_result):
    print(f"{i+1}. { article['title']}")
    print(article['_additional']['generate']['singleResult']) # print generated response
    print("-----------------------")
```

----------------------------------------

TITLE: Providing Example Stepwise Tax Credit Reasoning for GPT-3.5 Turbo - Markdown Prompt - gpt-3.5-turbo-instruct
DESCRIPTION: This snippet offers a sample stepwise answer for GPT-3.5 Turbo to mimic when supplied with the earlier structured prompt for IRS tax guidelines. Dependencies: the template prompt structure and access to vehicle facts. The code gives explicit reasoning for, and answers to, each eligibility criterion, concluding with an overall result sentence per instruction. Inputs are explicit vehicle details (Toyota Prius Prime bought in 2021); output is yes/no/N/A judgments and a summary. Limitations: accuracy depends on model knowledge and prompt alignment.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/techniques_to_improve_reliability.md#_snippet_13

LANGUAGE: gpt-3.5-turbo-instruct
CODE:
```
 The Toyota Prius Prime has four wheels, so the answer is yes.\n- Does the vehicle weigh less than 14,000 pounds? Let's think step by step. The Toyota Prius Prime weighs less than 14,000 pounds, so the answer is yes.\n- Does the vehicle draw energy from a battery with at least 4 kilowatt hours that may be recharged from an external source? Let's think step by step. The Toyota Prius Prime has a battery with at least 4 kilowatt hours that may be recharged from an external source, so the answer is yes.\n- Was the vehicle purchased in a year before 2022? Let's think step by step. The Toyota Prius Prime was purchased in 2021, which is before 2022, so the answer is yes.\n- Was the vehicle purchased in a year after 2022? N/A\n- If so, is the vehicle present in the following list of North American-assembled vehicles? N/A\n\n(2) After considering each criterion in turn, phrase the final answer as \"Because of {reasons}, the answer is likely {yes or no}.\"\n\nBecause the Toyota Prius Prime meets all of the criteria for a federal tax credit, the answer is likely yes.
```

----------------------------------------

TITLE: Author-Specific Quote Generation in Python
DESCRIPTION: Example of generating a new quote about animals inspired specifically by quotes from Schopenhauer.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_cassIO.ipynb#_snippet_20

LANGUAGE: python
CODE:
```
q_topic = generate_quote("animals", author="schopenhauer")
print("\nA new generated quote:")
print(q_topic)
```

----------------------------------------

TITLE: Computing Text Embeddings with OpenAI (Python)
DESCRIPTION: This snippet demonstrates how to compute embeddings for a list of text strings (`wikipedia_strings`) using the OpenAI API's `text-embedding-3-small` model. It processes the strings in batches to stay within API limits and stores the resulting embeddings in a list, verifying the order.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Embedding_Wikipedia_articles_for_search.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
EMBEDDING_MODEL = "text-embedding-3-small"
BATCH_SIZE = 1000  # you can submit up to 2048 embedding inputs per request

embeddings = []
for batch_start in range(0, len(wikipedia_strings), BATCH_SIZE):
    batch_end = batch_start + BATCH_SIZE
    batch = wikipedia_strings[batch_start:batch_end]
    print(f"Batch {batch_start} to {batch_end-1}")
    response = client.embeddings.create(model=EMBEDDING_MODEL, input=batch)
    for i, be in enumerate(response.data):
        assert i == be.index  # double check embeddings are in same order as input
    batch_embeddings = [e.embedding for e in response.data]
    embeddings.extend(batch_embeddings)

df = pd.DataFrame({"text": wikipedia_strings, "embedding": embeddings})
```

----------------------------------------

TITLE: Querying OpenAI with Additional Context from Redis
DESCRIPTION: Sends a prompt to OpenAI that includes the relevant context retrieved from Redis, resulting in a more informed response about FTX's management.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisqna/redisqna.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
prompt = f"""
Using the information delimited by triple backticks, answer this question: Is Sam Bankman-Fried's company, FTX, considered a well-managed company?

Context: ```{context}```
"""

response = get_completion(prompt)
print(response)
```

----------------------------------------

TITLE: Requesting Final Answer after Tool Invocation with Responses API in Python
DESCRIPTION: After the tool call sequence, this code sends the accumulated input_messages back to the Responses API to generate a final, enriched answer incorporating tool results. Supports parallel tool calls, and prints the resulting API response. Assumes proper conversation state management and relevant dependencies are initialized. Inputs are the augmented message list and API parameters; outputs are the final API object and, ultimately, the synthesized response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_api_tool_orchestration.ipynb#_snippet_16

LANGUAGE: python
CODE:
```

# Get the final answer incorporating the tool's result.
print("\n🔧 **Calling Responses API for Final Answer**")

response_2 = client.responses.create(
    model="gpt-4o",
    input=input_messages,
)
print(response_2)

```

----------------------------------------

TITLE: Implementing Numeric Rater for LLM-as-a-Judge in Python
DESCRIPTION: This snippet defines a function 'numeric_rater' that uses the OpenAI API to rate a submitted answer against an expert answer on a scale of 1 to 10. It includes a prompt template and uses function calling to extract the rating.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Custom-LLM-as-a-Judge.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
import json

PROMPT = """
You are comparing a submitted answer to an expert answer on a given question. Here is the data:
[BEGIN DATA]
************
[Question]: {input}
************
[Expert]: {expected}
************
[Submission]: {output}
************
[END DATA]

Compare the factual content of the submitted answer with the expert answer. Ignore any differences in style, grammar, or punctuation.
Rate the submission on a scale of 1 to 10.
"""


@braintrust.traced
async def numeric_rater(input, output, expected):
    response = await client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {
                "role": "user",
                "content": PROMPT.format(input=input, output=output, expected=expected),
            }
        ],
        temperature=0,
        tools=[
            {
                "type": "function",
                "function": {
                    "name": "rate",
                    "description": "Rate the submission on a scale of 1 to 10.",
                    "parameters": {
                        "type": "object",
                        "properties": {
                            "rating": {"type": "integer", "minimum": 1, "maximum": 10},
                        },
                        "required": ["rating"],
                    },
                },
            }
        ],
        tool_choice={"type": "function", "function": {"name": "rate"}},
    )
    arguments = json.loads(response.choices[0].message.tool_calls[0].function.arguments)
    return (arguments["rating"] - 1) / 9


print(qa_pairs[10].question, "On a correct answer:", qa_pairs[10].generated_answer)
print(
    await numeric_rater(
        qa_pairs[10].question,
        qa_pairs[10].generated_answer,
        qa_pairs[10].expected_answer,
    )
)

print(
    hallucinations[10].question,
    "On a hallucinated answer:",
    hallucinations[10].generated_answer,
)
print(
    await numeric_rater(
        hallucinations[10].question,
        hallucinations[10].generated_answer,
        hallucinations[10].expected_answer,
    )
)
```

----------------------------------------

TITLE: Installing Prerequisite Python Packages for Azure OpenAI Integration
DESCRIPTION: Installs essential Python package dependencies for programmatic access to Azure OpenAI and secure environment variable management. 'openai' is required for SDK access to OpenAI resources, and 'python-dotenv' is used for managing authentication and configuration variables set in a .env file. Prerequisite to all subsequent steps.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat_with_your_own_data.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
! pip install "openai>=1.0.0,<2.0.0"
! pip install python-dotenv
```

----------------------------------------

TITLE: Streaming Azure OpenAI Chat Completion Responses with Context (Python)
DESCRIPTION: Initiates a chat completion request using Azure OpenAI with streaming enabled, so the response can be processed in chunks as it is generated. Each chunk is checked for a role, content, and possible context, providing the ability to display response tokens and cited context in real time. Useful for large or latency-sensitive outputs; requires Python SDK and an authenticated client configured as shown previously.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat_with_your_own_data.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
response = client.chat.completions.create(
    messages=[{"role": "user", "content": "What are the differences between Azure Machine Learning and Azure AI services?"}],
    model=deployment,
    extra_body={
        "dataSources": [
            {
                "type": "AzureCognitiveSearch",
                "parameters": {
                    "endpoint": os.environ["SEARCH_ENDPOINT"],
                    "key": os.environ["SEARCH_KEY"],
                    "indexName": os.environ["SEARCH_INDEX_NAME"],
                }
            }
        ]
    },
    stream=True,
)

for chunk in response:
    delta = chunk.choices[0].delta

    if delta.role:
        print("\n"+ delta.role + ": ", end="", flush=True)
    if delta.content:
        print(delta.content, end="", flush=True)
    if delta.model_extra.get("context"):
        print(f"Context: {delta.model_extra['context']}", end="", flush=True)
```

----------------------------------------

TITLE: Implementing Async Classifier Function with GPT-4o for Hallucination Detection
DESCRIPTION: An asynchronous function that uses OpenAI's GPT-4o to classify answers as hallucinations or valid responses. It leverages function calling to force a structured output and applies the previously defined scoring system to rate the response quality.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Custom-LLM-as-a-Judge.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
@braintrust.traced
async def classifier(input, output, expected):
    response = await client.chat.completions.create(
        model="gpt-4o",
        messages=[
            {
                "role": "user",
                "content": PROMPT.format(input=input, output=output, expected=expected),
            }
        ],
        temperature=0,
        tools=[
            {
                "type": "function",
                "function": {
                    "name": "rate",
                    "description": "Call this function to select a choice.",
                    "parameters": {
                        "properties": {
                            "reasons": {
                                "description": "Write out in a step by step manner your reasoning to be sure that your conclusion is correct. Avoid simply stating the correct answer at the outset.",
                                "type": "string",
                            },
                            "choice": {
                                "description": "The choice",
                                "type": "string",
                                "enum": ["A", "B", "C", "D", "E"],
                            },
                        },
                        "required": ["reasons", "choice"],
                        "type": "object",
                    },
                },
            }
        ],
        tool_choice={"type": "function", "function": {"name": "rate"}},
    )
    arguments = json.loads(response.choices[0].message.tool_calls[0].function.arguments)
    choice = arguments["choice"]
    return CHOICE_SCORES[choice] if choice in CHOICE_SCORES else None
```

----------------------------------------

TITLE: Querying Weaviate for Relevant Articles Based on Text Query - Python
DESCRIPTION: Defines a function to search for semantically similar articles given a text query using Weaviate's with_near_text filtering and OpenAI-generated vector embeddings. Returns the top 10 closest articles along with 'certainty' and 'distance' metrics. Handles cases where API rate limits are exceeded, raising an exception as appropriate. Dependencies: properly configured Weaviate client and sufficient OpenAI API quota. Inputs: query string and collection name. Outputs: list of matching article objects, or raises error if API rate exceeded.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/getting-started-with-weaviate-and-openai.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
def query_weaviate(query, collection_name):
    
    nearText = {
        "concepts": [query],
        "distance": 0.7,
    }

    properties = [
        "title", "content", "url",
        "_additional {certainty distance}"
    ]

    result = (
        client.query
        .get(collection_name, properties)
        .with_near_text(nearText)
        .with_limit(10)
        .do()
    )
    
    # Check for errors
    if ("errors" in result):
        print ("\033[91mYou probably have run out of OpenAI API calls for the current minute – the limit is set at 60 per minute.")
        raise Exception(result["errors"][0]['message'])
    
    return result["data"]["Get"][collection_name]
```

----------------------------------------

TITLE: Exporting OpenAI API Key to Environment Variable - Python
DESCRIPTION: This snippet illustrates setting the OpenAI API key as an environment variable (OPENAI_API_KEY) using a shell export command. This environment variable is critical as it enables secure, programmatic access to the OpenAI API from Python code and connected services. It must be run in a Unix shell or notebook cell with shell execution support, and users should replace 'your key' with their actual API key.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/weaviate/getting-started-with-weaviate-and-openai.ipynb#_snippet_1

LANGUAGE: Python
CODE:
```
# Export OpenAI API Key\n!export OPENAI_API_KEY=\"your key\"
```

----------------------------------------

TITLE: Make Basic API Call with Responses API
DESCRIPTION: Demonstrates a simple call to the OpenAI Responses API using the 'o4-mini' model with a basic text input. This shows how to get a response object from the API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/reasoning_items.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
response = client.responses.create(
    model="o4-mini",
    input="tell me a joke",
)
```

----------------------------------------

TITLE: Re-initializing LLM Agent with Expanded Tools and Custom Prompt - Python
DESCRIPTION: This code sets up a new agent capable of using multiple tools, including a custom prompt template that supports conversation history. It instantiates a prompt with tools and input variables, assembles an LLMChain from the LLM and prompt, and authorizes the agent to use all expanded tools. Prerequisites are `CustomPromptTemplate`, the tools and LLM created earlier, as well as supporting variables (`template_with_history`, `output_parser`). The agent is ready for execution with tool switching and stop sequence handling.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_29

LANGUAGE: Python
CODE:
```
# Re-initialize the agent with our new list of tools
prompt_with_history = CustomPromptTemplate(
    template=template_with_history,
    tools=expanded_tools,
    input_variables=["input", "intermediate_steps", "history"]
)
llm_chain = LLMChain(llm=llm, prompt=prompt_with_history)
multi_tool_names = [tool.name for tool in expanded_tools]
multi_tool_agent = LLMSingleActionAgent(
    llm_chain=llm_chain, 
    output_parser=output_parser,
    stop=["\nObservation:"], 
    allowed_tools=multi_tool_names
)
```

----------------------------------------

TITLE: Implementing and Running Asynchronous LLM Guardrail Checks in Python
DESCRIPTION: This snippet defines asynchronous functions for performing moderation checks and orchestrating both topical and output guardrails using asyncio. It constructs a moderation prompt, invokes an OpenAI chat model for a severity score, and coordinates concurrent topical and chat checks. Main dependencies include OpenAI's Python client, asyncio, and predefined functions/variables (e.g., 'topical_guardrail', 'get_chat_response', 'GPT_MODEL'). Parameters include user and chat responses. The output is a response string based on the moderation score; guardrails cancel or filter outputs scoring 3 or higher. Limitations: Assumes existence of OpenAI keys, external functions, and correct model/chat structure.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_use_guardrails.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
async def moderation_guardrail(chat_response):
    print("Checking moderation guardrail")
    mod_messages = [
        {"role": "user", "content": moderation_system_prompt.format(
            domain=domain,
            scoring_criteria=animal_advice_criteria,
            scoring_steps=animal_advice_steps,
            content=chat_response
        )},
    ]
    response = openai.chat.completions.create(
        model=GPT_MODEL, messages=mod_messages, temperature=0
    )
    print("Got moderation response")
    return response.choices[0].message.content
    
    
async def execute_all_guardrails(user_request):
    topical_guardrail_task = asyncio.create_task(topical_guardrail(user_request))
    chat_task = asyncio.create_task(get_chat_response(user_request))

    while True:
        done, _ = await asyncio.wait(
            [topical_guardrail_task, chat_task], return_when=asyncio.FIRST_COMPLETED
        )
        if topical_guardrail_task in done:
            guardrail_response = topical_guardrail_task.result()
            if guardrail_response == "not_allowed":
                chat_task.cancel()
                print("Topical guardrail triggered")
                return "I can only talk about cats and dogs, the best animals that ever lived."
            elif chat_task in done:
                chat_response = chat_task.result()
                moderation_response = await moderation_guardrail(chat_response)

                if int(moderation_response) >= 3:
                    print(f"Moderation guardrail flagged with a score of {int(moderation_response)}")
                    return "Sorry, we're not permitted to give animal breed advice. I can help you with any general queries you might have."

                else:
                    print('Passed moderation')
                    return chat_response
        else:
            await asyncio.sleep(0.1)  # sleep for a bit before checking the tasks again

```

----------------------------------------

TITLE: Enforcing Type-Safe Structured Outputs via Pydantic and GPT-4o-mini in Python
DESCRIPTION: This snippet illustrates the chaining of o1-preview and gpt-4o-mini to achieve reliable structured outputs in Python. It defines Pydantic models for company data, sends a follow-up request to gpt-4o-mini that reformats and parses the o1-preview response using the declared schema for guaranteed type safety. Prerequisites include the openai, pydantic, and devtools packages. The input is a raw string, possibly in JSON, and the output is a parsed Python object conforming to the CompaniesData schema. Any unexpected model output or API failure needs handling as a limitation.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/o1/Using_chained_calls_for_o1_structured_outputs.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
from pydantic import BaseModel
from devtools import pprint

class CompanyData(BaseModel):
    company_name: str
    page_link: str
    reason: str

class CompaniesData(BaseModel):
    companies: list[CompanyData]

o1_response = client.chat.completions.create(
    model="o1-preview",
    messages=[
        {
            "role": "user", 
            "content": f"""
You are a business analyst designed to understand how AI technology could be used across large corporations.

- Read the following html and return which companies would benefit from using AI technology: {html_content}.
- Rank these propects by opportunity by comparing them and show me the top 3. Return each with {CompanyData.__fields__.keys()}
"""
        }
    ]
)

o1_response_content = o1_response.choices[0].message.content

response = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user", 
            "content": f"""
Given the following data, format it with the given response format: {o1_response_content}
"""
        }
    ],
    response_format=CompaniesData,
)

pprint(response.choices[0].message.parsed)

```

----------------------------------------

TITLE: Prompting Step-By-Step Reasoning in GPT-3.5-Turbo-Instruct - Prompt Engineering - gpt-3.5-turbo-instruct
DESCRIPTION: This snippet demonstrates a prompt modification that instructs GPT-3.5-Turbo-Instruct to reason step by step before answering the original question. The addition of 'Let's think step by step.' prompts the model to generate an explicit logical sequence, improving accuracy on logic-based tasks. Access to GPT-3.5-Turbo-Instruct is required. The main parameter is the inclusion of reasoning instructions; expected output is a detailed, reasoned response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/techniques_to_improve_reliability.md#_snippet_2

LANGUAGE: gpt-3.5-turbo-instruct
CODE:
```
Q: A juggler has 16 balls. Half of the balls are golf balls and half of the golf balls are blue. How many blue golf balls are there?
A: Let's think step by step.
```

----------------------------------------

TITLE: Defining Moderation Prompt Components for LLM Guardrails in Python
DESCRIPTION: This snippet sets up domain-specific criteria, grading steps, and a templated system prompt for moderation. It is intended to structure the evaluation prompt for an LLM moderation assistant who will assess if animal breed advice is given and its severity. Dependencies include proper string handling and formatting; variables such as 'domain', 'animal_advice_criteria', and 'animal_advice_steps' are meant to be filled into 'moderation_system_prompt' for subsequent use in moderation logic.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_use_guardrails.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
domain = "animal breed recommendation"

animal_advice_criteria = """
Assess the presence of explicit recommendation of cat or dog breeds in the content.
The content should contain only general advice about cats and dogs, not specific breeds to purchase."""

animal_advice_steps = """
1. Read the content and the criteria carefully.
2. Assess how much explicit recommendation of cat or dog breeds is contained in the content.
3. Assign an animal advice score from 1 to 5, with 1 being no explicit cat or dog breed advice, and 5 being multiple named cat or dog breeds.
"""

moderation_system_prompt = """
You are a moderation assistant. Your role is to detect content about {domain} in the text provided, and mark the severity of that content.

## {domain}

### Criteria

{scoring_criteria}

### Instructions

{scoring_steps}

### Content

{content}

### Evaluation (score only!)
"""

```

----------------------------------------

TITLE: Querying ChatGPT with Context
DESCRIPTION: Demonstrates how to use the context-aware query function to get an answer from ChatGPT about the 2022 Olympics curling gold medal.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/SingleStoreDB/OpenAI_wikipedia_semantic_search.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
from pprint import pprint

answer = ask('Who won the gold medal for curling in Olymics 2022?')

pprint(answer)
```

----------------------------------------

TITLE: Main Conversation Loop Implementation
DESCRIPTION: Implements the main conversation loop that handles user input and agent responses, managing the current active agent and message history.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_15

LANGUAGE: python
CODE:
```
agent = triage_agent
messages = []

while True:
    user = input("User: ")
    messages.append({"role": "user", "content": user})

    response = run_full_turn(agent, messages)
    agent = response.agent
    messages.extend(response.messages)
```

----------------------------------------

TITLE: Processing Function Calls from Model Output - Python
DESCRIPTION: Defines a Python function for illustrative function call execution ('get_current_weather'), extracts the first function call from the model response, and conditionally calls the illustrative backend. Demonstrates how to use the provided arguments and structure the expected result for returning to the model. Requires the 'json' module and the prior chat completion response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/functions.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
import json

def get_current_weather(request):
    """
    This function is for illustrative purposes.
    The location and unit should be used to determine weather
    instead of returning a hardcoded response.
    """
    location = request.get("location")
    unit = request.get("unit")
    return {"temperature": "22", "unit": "celsius", "description": "Sunny"}

function_call = chat_completion.choices[0].message.tool_calls[0].function
print(function_call.name)
print(function_call.arguments)

if function_call.name == "get_current_weather":
    response = get_current_weather(json.loads(function_call.arguments))
```

----------------------------------------

TITLE: Querying Pinecone Index for Relevant Contexts in Python
DESCRIPTION: Demonstrates how to execute a query against the Pinecone index for the most relevant contexts to a given embedding. Relies on an initialized Pinecone index object and requires 'xq', the embedding vector for the query text. Returns up to two results with metadata for further context construction. The result is used to assemble a context-rich prompt.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/pinecone/Gen_QA.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
# get relevant contexts (including the questions)
res = index.query(xq, top_k=2, include_metadata=True)
```

----------------------------------------

TITLE: Handling OpenAI Function Calls
DESCRIPTION: Processes OpenAI's response and executes the requested functions with proper argument handling.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_an_agent_with_the_node_sdk.mdx#_snippet_6

LANGUAGE: javascript
CODE:
```
const availableTools = {
  getCurrentWeather,
  getLocation,
};

const { finish_reason, message } = response.choices[0];

if (finish_reason === "tool_calls" && message.tool_calls) {
  const functionName = message.tool_calls[0].function.name;
  const functionToCall = availableTools[functionName];
  const functionArgs = JSON.parse(message.tool_calls[0].function.arguments);
  const functionArgsArr = Object.values(functionArgs);
  const functionResponse = await functionToCall.apply(null, functionArgsArr);
  console.log(functionResponse);
}
```

----------------------------------------

TITLE: Creating Vector Store and Bulk Uploading PDFs via OpenAI SDK in Python
DESCRIPTION: Creates a new vector store and uploads all PDF files to it using previously defined functions. This snippet expects the functions for creation and uploading to be defined, and that OpenAI credentials and PDF inputs are available. The resulting vector store details are captured for downstream operations.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/File_Search_Responses.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
store_name = "openai_blog_store"
vector_store_details = create_vector_store(store_name)
upload_pdf_files_to_vector_store(vector_store_details["id"])
```

----------------------------------------

TITLE: Listing S3 Buckets with Python Assistant Bot
DESCRIPTION: This snippet invokes the run_conversation function to list all available S3 buckets via a natural language prompt. It requires the run_conversation function to be defined and assumes access to any configurations or authentication needed for S3 communication. The input is a static string prompt, and the output is printed to standard output—typically, a textual list of bucket names.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
print(run_conversation('list my S3 buckets'))
```

----------------------------------------

TITLE: Defining Whisper Transcription Function - Python
DESCRIPTION: Defines a Python function `transcribe` that takes an audio file path and a prompt string, calls the OpenAI Whisper API to transcribe the audio, and returns the transcribed text.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Whisper_prompting_guide.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
def transcribe(audio_filepath, prompt: str) -> str:
    """Given a prompt, transcribe the audio file."""
    transcript = client.audio.transcriptions.create(
        file=open(audio_filepath, "rb"),
        model="whisper-1",
        prompt=prompt,
    )
    return transcript.text
```

----------------------------------------

TITLE: Author-Filtered Quote Search in Python
DESCRIPTION: Example of using the quote search function with an author filter to retrieve quotes only from a specific philosopher.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_cassIO.ipynb#_snippet_14

LANGUAGE: python
CODE:
```
find_quote_and_author("We struggle all our life for nothing", 2, author="nietzsche")
```

----------------------------------------

TITLE: Setting Up Environment Variables and Dependencies with OpenAI API - Python
DESCRIPTION: Initializes API keys, imports required libraries, and sets up helper functions for interfacing with the OpenAI API and embedding generation. Sets environment variables for News API access and defines utility functions for prompting GPT and obtaining embeddings. Dependencies include openai, requests, numpy, tqdm, IPython, and python-dotenv. Inputs are API keys via environment; outputs are helper methods used throughout the workflow.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Question_answering_using_a_search_API.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
%%capture
%env NEWS_API_KEY = YOUR_NEWS_API_KEY

```

LANGUAGE: python
CODE:
```
# Dependencies
from datetime import date, timedelta  # date handling for fetching recent news
from IPython import display  # for pretty printing
import json  # for parsing the JSON api responses and model outputs
from numpy import dot  # for cosine similarity
from openai import OpenAI
import os  # for loading environment variables
import requests  # for making the API requests
from tqdm.notebook import tqdm  # for printing progress bars

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))

# Load environment variables
news_api_key = os.getenv("NEWS_API_KEY")

GPT_MODEL = "gpt-3.5-turbo"


# Helper functions
def json_gpt(input: str):
    completion = client.chat.completions.create(model=GPT_MODEL,
    messages=[
        {"role": "system", "content": "Output only valid JSON"},
        {"role": "user", "content": input},
    ],
    temperature=0.5)

    text = completion.choices[0].message.content
    parsed = json.loads(text)

    return parsed


def embeddings(input: list[str]) -> list[list[str]]:
    response = client.embeddings.create(model="text-embedding-3-small", input=input)
    return [data.embedding for data in response.data]
```

----------------------------------------

TITLE: Running Conversational Queries with the Multi-Tool Executor - Python
DESCRIPTION: These snippets demonstrate two example queries run via the agent executor, showcasing how conversational queries are dispatched through the expanded tool set. Each `run` call accepts a natural language question and returns an agent-generated answer (via the appropriate tool). The executor determines which tool to use based on the query, using the context maintained in previous snippets. Expected input is a single string; output is the agent's generated response.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_31

LANGUAGE: Python
CODE:
```
multi_tool_executor.run("Hi, I'd like to know how you can live without a bank account")
```

LANGUAGE: Python
CODE:
```
multi_tool_executor.run('Can you tell me some interesting facts about whether zoos are good or bad for animals')
```

----------------------------------------

TITLE: Creating Streaming Chat Completions with Azure OpenAI - Python
DESCRIPTION: Illustrates streaming token-by-token chat completions from Azure OpenAI via the Python client. The 'stream=True' parameter enables streaming responses, and the loop processes response chunks as they arrive. This approach allows incremental display of the assistant's reply, suitable for interactive applications.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
response = client.chat.completions.create(
    model=deployment,
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Knock knock."},
        {"role": "assistant", "content": "Who's there?"},
        {"role": "user", "content": "Orange."},
    ],
    temperature=0,
    stream=True
)

for chunk in response:
    if len(chunk.choices) > 0:
        delta = chunk.choices[0].delta

        if delta.role:
            print(delta.role + ": ", end="", flush=True)
        if delta.content:
            print(delta.content, end="", flush=True)
```

----------------------------------------

TITLE: Completion Prompting for Author Extraction - Text
DESCRIPTION: Shows a completion-style prompt for a large language model to infer the desired continuation of a pattern, leading the model to output the author's name. This method requires careful setup, as the LLM attempts to output what would naturally follow the input string. The snippet involves an incomplete sentence ending with a cue for the author’s name and works in plain text interaction with LLMs.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/how_to_work_with_large_language_models.md#_snippet_1

LANGUAGE: text
CODE:
```
“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”\n― Ted Chiang, Exhalation\n\nThe author of this quote is
```

LANGUAGE: text
CODE:
```
 Ted Chiang
```

----------------------------------------

TITLE: Processing Model Function Calls and Preparing Output (Python)
DESCRIPTION: Iterates through function calls identified in the model's response, looks up the corresponding local tool, executes the tool with the provided arguments, and formats the tool's output into a list of `function_call_output` objects for subsequent model interaction. Requires `response.output`, `tool_mapping`, and the `json` library.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
# Extract the function call(s) from the response
new_conversation_items = []
function_calls = [rx for rx in response.output if rx.type == 'function_call']
for function_call in function_calls:
    target_tool = tool_mapping.get(function_call.name)
    if not target_tool:
        raise ValueError(f"No tool found for function call: {function_call.name}")
    arguments = json.loads(function_call.arguments) # Load the arguments as a dictionary
    tool_output = target_tool(**arguments) # Invoke the tool with the arguments
    new_conversation_items.append({
        "type": "function_call_output",
        "call_id": function_call.call_id, # We map the response back to the original function call
        "output": tool_output
    })
```

----------------------------------------

TITLE: Splitting Reasoning into Subtasks - Stepwise Prompting - GPT-3.5-Turbo-Instruct - gpt-3.5-turbo-instruct
DESCRIPTION: This prompt instructs GPT-3.5-Turbo-Instruct to follow a defined procedure: evaluating clues for relevance, combining relevant clues, and mapping the answer explicitly to a multiple-choice option. Intended to enhance logical reasoning by structuring the task into subtasks. Dependencies: clear stepwise instructions, same clue/context as before. Inputs: clues and question; outputs: structured reasoning and answer. Helps mitigate reasoning failures by enforcing stepwise logic.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/techniques_to_improve_reliability.md#_snippet_6

LANGUAGE: gpt-3.5-turbo-instruct
CODE:
```
Use the following clues to answer the following multiple-choice question, using the following procedure:
(1) First, go through the clues one by one and consider whether the clue is potentially relevant
(2) Second, combine the relevant clues to reason out the answer to the question
(3) Third, map the answer to one of the multiple choice answers: either (a), (b), or (c)

Clues:
1. Miss Scarlett was the only person in the lounge.
2. The person with the pipe was in the kitchen.
3. Colonel Mustard was the only person in the observatory.
4. Professor Plum was not in the library nor the billiard room.
5. The person with the candlestick was in the observatory.

Question: Was Colonel Mustard in the observatory with the candlestick?
(a) Yes; Colonel Mustard was in the observatory with the candlestick
(b) No; Colonel Mustard was not in the observatory with the candlestick
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick

Solution:
(1) First, go through the clues one by one and consider whether the clue is potentially relevant:
```

----------------------------------------

TITLE: Initializing AsyncOpenAI Client for Push Notifications Summarizer Evaluation
DESCRIPTION: Sets up the AsyncOpenAI client with the API key for interacting with OpenAI's API. This is a prerequisite for running the evaluation tasks.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/use-cases/completion-monitoring.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import AsyncOpenAI
import os
import asyncio

os.environ["OPENAI_API_KEY"] = os.environ.get("OPENAI_API_KEY", "your-api-key")
client = AsyncOpenAI()
```

----------------------------------------

TITLE: Translating Business Jargon using OpenAI Chat Completions API - Python
DESCRIPTION: Demonstrates how to use the OpenAI Chat Completions API to translate corporate jargon into plain English, setting up a conversation with a sequence of system and user/assistant message examples to condition the model. Requires the OpenAI Python client initialized as 'client' and a defined MODEL variable. The input is a list of message dicts, and the model's output is printed; the translation occurs in the context of a few-shot prompt. Output is the assistant's plain English translation of the last user message; integration with the OpenAI API is necessary for execution.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
# The business jargon translation example, but with example names for the example messages
response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {"role": "system", "content": "You are a helpful, pattern-following assistant that translates corporate jargon into plain English."},
        {"role": "system", "name":"example_user", "content": "New synergies will help drive top-line growth."},
        {"role": "system", "name": "example_assistant", "content": "Things working well together will increase revenue."},
        {"role": "system", "name":"example_user", "content": "Let's circle back when we have more bandwidth to touch base on opportunities for increased leverage."},
        {"role": "system", "name": "example_assistant", "content": "Let's talk later when we're less busy about how to do better."},
        {"role": "user", "content": "This late pivot means we don't have time to boil the ocean for the client deliverable."},
    ],
    temperature=0,
)

print(response.choices[0].message.content)

```

----------------------------------------

TITLE: Generating Function Arguments via OpenAI Chat Completion - Python
DESCRIPTION: Demonstrates initializing a conversation, prompting for weather information, sending the message history and tool specs to the chat completion utility, and extracting the assistant's structured response. Intended to show how the model requests clarifying details before generating function arguments; prerequisite: earlier setup and utility definitions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
messages = []\nmessages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})\nmessages.append({"role": "user", "content": "What's the weather like today"})\nchat_response = chat_completion_request(\n    messages, tools=tools\n)\nassistant_message = chat_response.choices[0].message\nmessages.append(assistant_message)\nassistant_message\n
```

----------------------------------------

TITLE: Forcing Use of Specific Function via tool_choice Argument - Python
DESCRIPTION: Forces the model to generate arguments for the 'get_n_day_weather_forecast' function via the 'tool_choice' parameter in the API call. Demonstrates bypassing the model's automatic function selection logic. Useful where deterministic behavior is required; the assistant's message directly reflects the specified function.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
# in this cell we force the model to use get_n_day_weather_forecast\nmessages = []\nmessages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})\nmessages.append({"role": "user", "content": "Give me a weather report for Toronto, Canada."})\nchat_response = chat_completion_request(\n    messages, tools=tools, tool_choice={"type": "function", "function": {"name": "get_n_day_weather_forecast"}}\n)\nchat_response.choices[0].message
```

----------------------------------------

TITLE: Automating Multi-Policy Generation with GPT-4o and ThreadPoolExecutor - Python
DESCRIPTION: This snippet provides two Python functions to automate the creation of policy instructions using GPT-4o. The 'generate_policy' function formats and sends prompts to the OpenAI API and returns the generated policy. The 'generate_policies' function orchestrates parallel generation of multiple policies using ThreadPoolExecutor for improved throughput. Dependencies: OpenAI client library, ThreadPoolExecutor, and definitions for 'system_input_prompt', 'user_policy_input', and example prompt variables. The output is a list of detailed, instructive policies for various support scenarios. Key arguments are the policy string and the policies list. Inputs should be valid policy topics; outputs are GPT-4o generated policies as strings.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Developing_hallucination_guardrails.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
def generate_policy(policy: str) -> str:
    input_message = user_policy_input.replace("{{POLICY}}", policy)
    
    response = client.chat.completions.create(
        messages= [
            {"role": "system", "content": system_input_prompt},
            {"role": "user", "content": user_policy_example_1},
            {"role": "assistant", "content": assistant_policy_example_1},
            {"role": "user", "content": input_message},
        ],
        model="gpt-4o"
    )
    
    return response.choices[0].message.content

def generate_policies() -> List[str]:
    # List of different types of policies to generate 
    policies = ['PRODUCT FEEDBACK POLICY', 'SHIPPING POLICY', 'WARRANTY POLICY', 'ACCOUNT DELETION', 'COMPLAINT RESOLUTION']
    
    with ThreadPoolExecutor() as executor:
        policy_instructions_list = list(executor.map(generate_policy, policies))
        
    return policy_instructions_list

policy_instructions = generate_policies()
```

----------------------------------------

TITLE: Batch Embedding and Insertion of Quotes into Vector Store - Python
DESCRIPTION: Processes the entire quote dataset in batches (default size 50) to create embeddings using the OpenAI API, then inserts them into the Cassandra/Astra DB vector table with associated metadata (such as author and tags). Handles tag parsing, constructs unique row IDs, and prints progress. Optimizes API usage and ensures metadata consistency, but inserts synchronously for demonstration.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_cassIO.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
BATCH_SIZE = 50

num_batches = ((len(philo_dataset) + BATCH_SIZE - 1) // BATCH_SIZE)

quotes_list = philo_dataset["quote"]
authors_list = philo_dataset["author"]
tags_list = philo_dataset["tags"]

print("Starting to store entries:")
for batch_i in range(num_batches):
    b_start = batch_i * BATCH_SIZE
    b_end = (batch_i + 1) * BATCH_SIZE
    # compute the embedding vectors for this batch
    b_emb_results = client.embeddings.create(
        input=quotes_list[b_start : b_end],
        model=embedding_model_name,
    )
    # prepare the rows for insertion
    print("B ", end="")
    for entry_idx, emb_result in zip(range(b_start, b_end), b_emb_results.data):
        if tags_list[entry_idx]:
            tags = {
                tag
                for tag in tags_list[entry_idx].split(";")
            }
        else:
            tags = set()
        author = authors_list[entry_idx]
        quote = quotes_list[entry_idx]
        v_table.put(
            row_id=f"q_{author}_{entry_idx}",
            body_blob=quote,
            vector=emb_result.embedding,
            metadata={**{tag: True for tag in tags}, **{"author": author}},
        )
        print("*", end="")
    print(f" done ({len(b_emb_results.data)})")

print("\nFinished storing entries.")
```

----------------------------------------

TITLE: Updating an Assistant with Code Interpreter (OpenAI Assistants API, Python)
DESCRIPTION: This code snippet updates an existing Assistant by enabling the Code Interpreter tool using the OpenAI Python client. Dependencies include a configured OpenAI Python client and an existing Assistant ID. The assistant is updated via the API and the JSON response is displayed. Inputs: Assistant ID, tool list. Outputs: Updated Assistant object. Requires valid authentication for client operations.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Assistants_API_overview_python.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
assistant = client.beta.assistants.update(
    MATH_ASSISTANT_ID,
    tools=[{"type": "code_interpreter"}],
)
show_json(assistant)
```

----------------------------------------

TITLE: Batch Upserting Embedded Documents with Metadata into Pinecone Index in Python
DESCRIPTION: Processes records in batches to create embeddings using OpenAI's API, extracts question and answer as metadata, and upserts embedded vectors into the Pinecone index. Updates or modifies metadata per entry as needed. Inputs: DataFrame, client, embedding model, batch size. Outputs: Upserted batches in Pinecone. Limitation: Assumes batch operations fit into memory and API quota.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_api_tool_orchestration.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
batch_size = 32
for i in tqdm(range(0, len(ds_dataframe['merged']), batch_size), desc="Upserting to Pinecone"):
    i_end = min(i + batch_size, len(ds_dataframe['merged']))
    lines_batch = ds_dataframe['merged'][i: i_end]
    ids_batch = [str(n) for n in range(i, i_end)]
    
    # Create embeddings for the current batch.
    res = client.embeddings.create(input=[line for line in lines_batch], model=MODEL)
    embeds = [record.embedding for record in res.data]
    
    # Prepare metadata by extracting original Question and Answer.
    meta = []
    for record in ds_dataframe.iloc[i:i_end].to_dict('records'):
        q_text = record['Question']
        a_text = record['Response']
        # Optionally update metadata for specific entries.
        meta.append({"Question": q_text, "Answer": a_text})
    
    # Upsert the batch into Pinecone.
    vectors = list(zip(ids_batch, embeds, meta))
    index.upsert(vectors=vectors)

```

----------------------------------------

TITLE: Ranking Strings by Relatedness (Python)
DESCRIPTION: This function ranks a list of strings (represented by filepaths in a pandas DataFrame) based on their semantic relatedness to a given query string. It uses embeddings and a relatedness function (defaulting to cosine distance) to calculate scores and returns the top N strings. Requires pandas and scipy.spatial.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_for_knowledge_retrieval.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
def strings_ranked_by_relatedness(
    query: str,
    df: pd.DataFrame,
    relatedness_fn=lambda x, y: 1 - spatial.distance.cosine(x, y),
    top_n: int = 100,
) -> list[str]:
    """Returns a list of strings and relatednesses, sorted from most related to least."""
    query_embedding_response = embedding_request(query)
    query_embedding = query_embedding_response.data[0].embedding
    strings_and_relatednesses = [
        (row["filepath"], relatedness_fn(query_embedding, row["embedding"]))
        for i, row in df.iterrows()
    ]
    strings_and_relatednesses.sort(key=lambda x: x[1], reverse=True)
    strings, relatednesses = zip(*strings_and_relatednesses)
    return strings[:top_n]
```

----------------------------------------

TITLE: Fetching and Prompting for JSON Outputs with OpenAI o1-preview in Python
DESCRIPTION: This snippet demonstrates fetching an HTML page listing large US companies and prompting the o1-preview model to extract and rank companies that could benefit most from AI, returning the result in a specified JSON format. It shows the use of the requests and openai Python libraries, requires an available OpenAI API key, and specifies how to interpret and extract model responses. The key parameters are the URL to fetch, the custom prompt content, and the expected JSON format; the output is model-generated JSON text that should be validated before use.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/o1/Using_chained_calls_for_o1_structured_outputs.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
import requests
from openai import OpenAI

client = OpenAI()

def fetch_html(url):
    response = requests.get(url)
    if response.status_code == 200:
        return response.text
    else:
        return None

url = "https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue"
html_content = fetch_html(url)

json_format = """
{
    companies: [
        {
            \"company_name\": \"OpenAI\",
            \"page_link\": \"https://en.wikipedia.org/wiki/OpenAI\",
            \"reason\": \"OpenAI would benefit because they are an AI company...\"
        }
    ]
}
"""

o1_response = client.chat.completions.create(
    model="o1-preview",
    messages=[
        {
            "role": "user", 
            "content": f"""
You are a business analyst designed to understand how AI technology could be used across large corporations.

- Read the following html and return which companies would benefit from using AI technology: {html_content}.
- Rank these propects by opportunity by comparing them and show me the top 3. Return only as a JSON with the following format: {json_format}""
"""
        }
    ]
)

print(o1_response.choices[0].message.content)

```

----------------------------------------

TITLE: Formatting Function Call Output for Model Input (JSON)
DESCRIPTION: Defines the JSON structure required to send the result of a tool or function execution back to the OpenAI model. It includes the type, the ID of the original function call, and the actual output from the tool. Requires the `call_id` from the model's function call request and the `tool_output`.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/reasoning_function_calls.ipynb#_snippet_7

LANGUAGE: json
CODE:
```
{
    "type": "function_call_output",
    "call_id": function_call.call_id,
    "output": tool_output
}
```

----------------------------------------

TITLE: Generating Text Embeddings with OpenAI API - Python
DESCRIPTION: This snippet demonstrates how to generate a single text embedding using the OpenAI API's text-embedding-3-small model in Python. It requires the openai Python package and assumes the API key is configured. The 'input' parameter specifies the text to embed, and the snippet outputs the length of the resulting embedding vector as a basic check. There are no explicit error handling or rate limit mitigation features in this example.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Using_embeddings.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI
client = OpenAI()

embedding = client.embeddings.create(
    input="Your text goes here", model="text-embedding-3-small"
).data[0].embedding
len(embedding)

```

----------------------------------------

TITLE: Requesting Thematic Explanation from ChatGPT with System Message - Python
DESCRIPTION: This code sends a themed instruction to the model with a system message for personality priming. The example prompts the model to explain asynchronous programming like Blackbeard the pirate. Parameters include the chosen model, a system message, and a user instruction. Outputs a printed response emulating the specified style. Requires initialized OpenAI client.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
# example with a system message
response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain asynchronous programming in the style of the pirate Blackbeard."},
    ],
    temperature=0,
)

print(response.choices[0].message.content)

```

----------------------------------------

TITLE: Retrieving and Formatting Drive Item Content from O365 (JavaScript)
DESCRIPTION: This asynchronous helper fetches a file's content from O365/SharePoint via Microsoft Graph client, streams it, concatenates all byte chunks, and encodes the content as a base64 string. It also fetches and returns key metadata such as mime_type and filename, restructuring output into an OpenAI-compatible format. Dependencies: Microsoft Graph Client, Buffer (Node.js). Required parameters: Graph client, driveId, itemId, and file name. Throws on error; expects caller to handle output.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/sharepoint_azure_function/Using_Azure_Functions_and_Microsoft_Graph_to_Query_SharePoint.md#_snippet_2

LANGUAGE: javascript
CODE:
```
const getDriveItemContent = async (client, driveId, itemId, name) => {
   try
       const filePath = `/drives/${driveId}/items/${itemId}`;
       const downloadPath = filePath + `/content`
       // this is where we get the contents and convert to base64
       const fileStream = await client.api(downloadPath).getStream();
       let chunks = [];
           for await (let chunk of fileStream) {
               chunks.push(chunk);
           }
       const base64String = Buffer.concat(chunks).toString('base64');
       // this is where we get the other metadata to include in response
       const file = await client.api(filePath).get();
       const mime_type = file.file.mimeType;
       const name = file.name;
       return {"name":name, "mime_type":mime_type, "content":base64String}
   } catch (error) {
       console.error('Error fetching drive content:', error);
       throw new Error(`Failed to fetch content for ${name}: ${error.message}`);
   }
```

----------------------------------------

TITLE: Finding Similar Items with Cosine Similarity (Python)
DESCRIPTION: Takes an input embedding and a list of embeddings, calculates cosine similarity using `cosine_similarity_manual`, filters results by a threshold, sorts by score, and returns the top-k indices and scores. Parameters include the input embedding, a list of embeddings to search, an optional similarity threshold (default 0.5), and an optional number of top results to return (default 2).
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_combine_GPT4o_with_RAG_Outfit_Assistant.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
def find_similar_items(input_embedding, embeddings, threshold=0.5, top_k=2):
    """Find the most similar items based on cosine similarity."""
    
    # Calculate cosine similarity between the input embedding and all other embeddings
    similarities = [(index, cosine_similarity_manual(input_embedding, vec)) for index, vec in enumerate(embeddings)]
    
    # Filter out any similarities below the threshold
    filtered_similarities = [(index, sim) for index, sim in similarities if sim >= threshold]
    
    # Sort the filtered similarities by similarity score
    sorted_indices = sorted(filtered_similarities, key=lambda x: x[1], reverse=True)[:top_k]

    # Return the top-k most similar items
    return sorted_indices
```

----------------------------------------

TITLE: Batch Inserting Embedded Movie Data Into Zilliz Using Python
DESCRIPTION: Iteratively processes the Hugging Face dataset to batch-embed descriptions, accumulate relevant metadata, and insert records into the Zilliz collection in chunks of configurable batch size. After the loop, any remaining entries are also inserted. Dependencies include tqdm for progress bars and the previously defined 'embed' function. Inputs: full dataset; outputs: batched, embedded records stored in Zilliz. The code assumes that fields align with the collection schema and handles missing data by default values.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/zilliz/Filtered_search_with_Zilliz_and_OpenAI.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
from tqdm import tqdm

data = [
    [], # title
    [], # type
    [], # release_year
    [], # rating
    [], # description
]

# Embed and insert in batches
for i in tqdm(range(0, len(dataset))):
    data[0].append(dataset[i]['title'] or '')
    data[1].append(dataset[i]['type'] or '')
    data[2].append(dataset[i]['release_year'] or -1)
    data[3].append(dataset[i]['rating'] or '')
    data[4].append(dataset[i]['description'] or '')
    if len(data[0]) % BATCH_SIZE == 0:
        data.append(embed(data[4]))
        collection.insert(data)
        data = [[],[],[],[],[]]

# Embed and insert the remainder 
if len(data[0]) != 0:
    data.append(embed(data[4]))
    collection.insert(data)
    data = [[],[],[],[],[]]

```

----------------------------------------

TITLE: Creating a Qdrant Collection with Multiple Vector Types in Python
DESCRIPTION: Initializes a new Qdrant collection named 'Articles' and configures it to accept both 'title' and 'content' vectors, specifying each's size and distance metric. Requires the rest models from qdrant_client.http and definition of the vector size before execution. No schema migration is necessary; vectors are accessed by name for flexible queries.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/qdrant/Getting_started_with_Qdrant_and_OpenAI.ipynb#_snippet_10

LANGUAGE: python
CODE:
```
from qdrant_client.http import models as rest

vector_size = len(article_df["content_vector"][0])

client.create_collection(
    collection_name="Articles",
    vectors_config={
        "title": rest.VectorParams(
            distance=rest.Distance.COSINE,
            size=vector_size,
        ),
        "content": rest.VectorParams(
            distance=rest.Distance.COSINE,
            size=vector_size,
        ),
    }
)
```

----------------------------------------

TITLE: Demonstration (Few-Shot) Prompting - Text
DESCRIPTION: Depicts a demonstration/few-shot prompting strategy where multiple input-output pairs are shown. The model is provided with examples of quotes paired with corresponding authors, and then asked to predict the author for a new quote. This approach increases output consistency and is suitable for cases where clear examples can be provided. No code or external dependencies are required; the prompt is purely textual.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/how_to_work_with_large_language_models.md#_snippet_3

LANGUAGE: text
CODE:
```
Quote:\n“When the reasoning mind is forced to confront the impossible again and again, it has no choice but to adapt.”\n― N.K. Jemisin, The Fifth Season\nAuthor: N.K. Jemisin\n\nQuote:\n“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”\n― Ted Chiang, Exhalation\nAuthor:
```

LANGUAGE: text
CODE:
```
 Ted Chiang
```

----------------------------------------

TITLE: Transcribing Hindi Audio to English using GPT-4o in Python
DESCRIPTION: This snippet demonstrates how to transcribe a Hindi audio file to English text using the GPT-4o model. It defines the modalities as 'text' and constructs a prompt instructing the model to return only the English transcription. The function 'process_audio_with_gpt_4o' is called with base64-encoded Hindi audio data, and the response is parsed to retrieve the English content. Requires prior definition or import of 'process_audio_with_gpt_4o', and assumes 'hindi_audio_data_base64' contains properly encoded audio data. Outputs the English transcription to standard output. The snippet expects inputs as base64 audio and returns the English transcription string.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/voice_solutions/voice_translation_into_different_languages_using_GPT-4o.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
# Translate the audio output file generated by the model back into English and compare with the reference text 
modalities = ["text"]
prompt = "The user will provide an audio file in Hindi. Transcribe the audio to English text word for word. Only provide the language transcription, do not include background noises such as applause. "

response_json = process_audio_with_gpt_4o(hindi_audio_data_base64, modalities, prompt)

re_translated_english_text = response_json['choices'][0]['message']['content']

print(re_translated_english_text)
```

----------------------------------------

TITLE: Developer Prompt for Baseline Summarization - Python
DESCRIPTION: Defines the string constant DEVELOPER_PROMPT used in the correct summarization procedures. This prompt instructs the LLM to generate concise and merged push notification summaries and serves as the baseline against which regressions are tested. It is a prerequisite for the summarization functions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/use-cases/regression.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
DEVELOPER_PROMPT = """
You are a helpful assistant that summarizes push notifications.
You are given a list of push notifications and you need to collapse them into a single one.
Output only the final summary, nothing else.
"""
```

----------------------------------------

TITLE: Manual Exponential Backoff Retry Decorator Implementation - Python
DESCRIPTION: This fully manual example implements an exponential backoff retry mechanism using a custom decorator, handling specified exceptions like 'openai.RateLimitError'. Parameters for initial delay, exponential base, jitter, maximum retries, and errors can be provided. It retries the wrapped function on failure, sleeping for increasing random intervals, and raises an exception after exceeding retries. Requires 'random', 'time', and an OpenAI client; used here with a completion function.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_handle_rate_limits.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
# imports\nimport random\nimport time\n\n# define a retry decorator\ndef retry_with_exponential_backoff(\n    func,\n    initial_delay: float = 1,\n    exponential_base: float = 2,\n    jitter: bool = True,\n    max_retries: int = 10,\n    errors: tuple = (openai.RateLimitError,),\n):\n    """Retry a function with exponential backoff."""\n\n    def wrapper(*args, **kwargs):\n        # Initialize variables\n        num_retries = 0\n        delay = initial_delay\n\n        # Loop until a successful response or max_retries is hit or an exception is raised\n        while True:\n            try:\n                return func(*args, **kwargs)\n\n            # Retry on specified errors\n            except errors as e:\n                # Increment retries\n                num_retries += 1\n\n                # Check if max retries has been reached\n                if num_retries > max_retries:\n                    raise Exception(\n                        f"Maximum number of retries ({max_retries}) exceeded."\n                    )\n\n                # Increment the delay\n                delay *= exponential_base * (1 + jitter * random.random())\n\n                # Sleep for the delay\n                time.sleep(delay)\n\n            # Raise exceptions for any errors not specified\n            except Exception as e:\n                raise e\n\n    return wrapper\n\n\n@retry_with_exponential_backoff\ndef completions_with_backoff(**kwargs):\n    return client.chat.completions.create(**kwargs)\n\n\ncompletions_with_backoff(model="gpt-4o-mini", messages=[{"role": "user", "content": "Once upon a time,"}])
```

----------------------------------------

TITLE: Parallel Function Calling with GPT Models in Python
DESCRIPTION: This snippet demonstrates how to call multiple functions in one turn using newer GPT models like gpt-4o or gpt-3.5-turbo. It sets up the conversation and makes a chat completion request with specified tools.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
messages = []
messages.append({"role": "system", "content": "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."})
messages.append({"role": "user", "content": "what is the weather going to be like in San Francisco and Glasgow over the next 4 days"})
chat_response = chat_completion_request(
    messages, tools=tools, model=GPT_MODEL
)

assistant_message = chat_response.choices[0].message.tool_calls
assistant_message
```

----------------------------------------

TITLE: Querying OpenAI Chat Completions API in Python
DESCRIPTION: Demonstrates how to use the OpenAI Python client to send a user query to GPT-4o and extract the model's text completion. The code depends on the openai Python package and an API key. Key parameters include the model name and the user prompt. The input is a string query and the output is the model's generated response. Ensure OPENAI_API_KEY is properly set up in your environment.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/Web_search_with_google_api_bring_your_own_browser_tool.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI\n\nclient = OpenAI()\n\nsearch_query = \"List the latest OpenAI product launches in chronological order from latest to oldest in the past 2 years\"\n\n\nresponse = client.chat.completions.create(\n    model=\"gpt-4o\",\n    messages=[\n        {\"role\": \"system\", \"content\": \"You are a helpful agent.\"},\n        {\"role\": \"user\", \"content\": search_query}]\n).choices[0].message.content\n\nprint(response)\n
```

----------------------------------------

TITLE: Executing Tool Calls and Continuing the Conversation - Python
DESCRIPTION: Defines a 'tools_map' for function lookup, a helper 'execute_tool_call' that deserializes arguments and executes the matched Python function, and updates the conversation with tool results. This is an intermediate agent loop supporting function tool responses, necessary for dynamic and stateful workflows. Depends on previous tool definitions and the OpenAI API response format.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
tools_map = {tool.__name__: tool for tool in tools}

def execute_tool_call(tool_call, tools_map):
    name = tool_call.function.name
    args = json.loads(tool_call.function.arguments)

    print(f"Assistant: {name}({args})")

    # call corresponding function with provided arguments
    return tools_map[name](**args)

for tool_call in message.tool_calls:
            result = execute_tool_call(tool_call, tools_map)

            # add result back to conversation 
            result_message = {
                "role": "tool",
                "tool_call_id": tool_call.id,
                "content": result,
            }
            messages.append(result_message)
```

----------------------------------------

TITLE: Implementing Full Turn Logic
DESCRIPTION: Implements the main logic for processing a full conversation turn, including handling tool calls and agent transfers.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
def run_full_turn(agent, messages):

    current_agent = agent
    num_init_messages = len(messages)
    messages = messages.copy()

    while True:

        # turn python functions into tools and save a reverse map
        tool_schemas = [function_to_schema(tool) for tool in current_agent.tools]
        tools = {tool.__name__: tool for tool in current_agent.tools}

        # === 1. get openai completion ===
        response = client.chat.completions.create(
            model=agent.model,
            messages=[{"role": "system", "content": current_agent.instructions}]
            + messages,
            tools=tool_schemas or None,
        )
        message = response.choices[0].message
        messages.append(message)

        if message.content:  # print agent response
            print(f"{current_agent.name}:", message.content)

        if not message.tool_calls:  # if finished handling tool calls, break
            break

        # === 2. handle tool calls ===

        for tool_call in message.tool_calls:
            result = execute_tool_call(tool_call, tools, current_agent.name)

            if type(result) is Agent:  # if agent transfer, update current agent
                current_agent = result
                result = (
                    f"Transfered to {current_agent.name}. Adopt persona immediately."
                )

            result_message = {
                "role": "tool",
                "tool_call_id": tool_call.id,
                "content": result,
            }
            messages.append(result_message)

    # ==== 3. return last agent used and new messages =====
    return Response(agent=current_agent, messages=messages[num_init_messages:])


def execute_tool_call(tool_call, tools, agent_name):
    name = tool_call.function.name
    args = json.loads(tool_call.function.arguments)

    print(f"{agent_name}:", f"{name}({args})")

    return tools[name](**args)  # call corresponding function with provided arguments
```

----------------------------------------

TITLE: Initializing OpenAI Standard (Non-Azure) API and Embedding Function in Python
DESCRIPTION: Configures the OpenAI client for direct API key usage and defines an embed() function for generating an embedding using the 'text-embedding-3-small' model, compatible with precomputed samples. Only run this cell if using OpenAI directly (not Azure). Update api_key as needed. Input is a query string; output is its embedding vector list.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/kusto/Getting_started_with_kusto_and_openai_embeddings.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
openai.api_key = ""


def embed(query):
    # Creates embedding vector from user query
    embedded_query = openai.Embedding.create(
        input=query,
        model="text-embedding-3-small",
    )["data"][0]["embedding"]
    return embedded_query
```

----------------------------------------

TITLE: Building Custom QA Chain with PromptTemplate - Python
DESCRIPTION: Constructs a specialized QA chain in Langchain, injecting the custom prompt template into the 'stuff' chain type via chain_type_kwargs. Uses the previously created LLM and vectorstore instances. This allows response strategies to be customized, including fallback behaviors if answers are unknown.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/qdrant/QA_with_Langchain_Qdrant_and_OpenAI.ipynb#_snippet_16

LANGUAGE: python
CODE:
```
custom_qa = VectorDBQA.from_chain_type(
    llm=llm, 
    chain_type="stuff", 
    vectorstore=doc_store,
    return_source_documents=False,
    chain_type_kwargs={"prompt": custom_prompt_template},
)
```

----------------------------------------

TITLE: Initializing GraphCypherQAChain with OpenAI LLM for Natural-Language Graph Queries
DESCRIPTION: Configures LangChain's GraphCypherQAChain to allow natural language queries over the Neo4j graph using a ChatOpenAI model. This construction enables semantic user input to be translated into actual database Cypher queries, providing a bridge between LLMs and graph data.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/RAG_with_graph_db.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
from langchain.chains import GraphCypherQAChain
from langchain.chat_models import ChatOpenAI

chain = GraphCypherQAChain.from_llm(
    ChatOpenAI(temperature=0), graph=graph, verbose=True,
)
```

----------------------------------------

TITLE: Stepwise Reasoning Model Output for Multi-Choice - GPT-3.5-Turbo-Instruct - gpt-3.5-turbo-instruct
DESCRIPTION: This snippet shows the model's output after receiving stepwise prompting, including evaluating the relevance of clues, reasoning the answer, and selecting the correct multiple-choice response. Dependent on the prior procedural prompt. Input: stepwise prompt with clues and question; output: full stepwise reasoning and final answer. Demonstrates effectiveness of decomposing reasoning for improved model accuracy.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/techniques_to_improve_reliability.md#_snippet_7

LANGUAGE: gpt-3.5-turbo-instruct
CODE:
```
Clue 1: Miss Scarlett was the only person in the lounge. This clue is not relevant.
Clue 2: The person with the pipe was in the kitchen. This clue is not relevant.
Clue 3: Colonel Mustard was the only person in the observatory. This clue is relevant.
Clue 4: Professor Plum was not in the library nor the billiard room. This clue is not relevant.
Clue 5: The person with the candlestick was in the observatory. This clue is relevant.

(2) Second, combine the relevant clues to reason out the answer to the question:
Clue 3 and Clue 5 both say that the person with the candlestick was in the observatory. Colonel Mustard was the only person in the observatory, so Colonel Mustard was the person with the candlestick.

(3) Third, write the final answer:
(a) Yes; Colonel Mustard was in the observatory with the candlestick
```

----------------------------------------

TITLE: Defining Function Specification for SQL Queries in Python
DESCRIPTION: This snippet defines a function specification for the 'ask_database' function. It includes the database schema in the description, allowing the model to generate appropriate SQL queries based on the database structure.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_16

LANGUAGE: python
CODE:
```
tools = [
    {
        "type": "function",
        "function": {
            "name": "ask_database",
            "description": "Use this function to answer user questions about music. Input should be a fully formed SQL query.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": f"""
                                SQL query extracting info to answer the user's question.
                                SQL should be written using this database schema:
                                {database_schema_string}
                                The query should be returned in plain text, not in JSON.
                                """,
                    }
                },
                "required": ["query"],
            },
        }
    }
]
```

----------------------------------------

TITLE: Guided Language Identification and Summarization Prompt - GPT-3.5-Turbo-Instruct - gpt-3.5-turbo-instruct
DESCRIPTION: This prompt enhances summarization reliability by first instructing the model to identify the language of the text, then produce a summary in that language. It demonstrates a decomposition strategy to avoid premature translation to English. Dependencies: GPT-3.5-Turbo-Instruct model and structured stepwise instructions. Inputs: non-English text; outputs: language identification and summary. Intent is to enforce context awareness and use of original language.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/techniques_to_improve_reliability.md#_snippet_10

LANGUAGE: gpt-3.5-turbo-instruct
CODE:
```
First, identify the language of the text. Second, summarize the text using the original language of the text. The summary should be one sentence long.

Text:
"""
La estadística (la forma femenina del término alemán Statistik, derivado a su vez del italiano statista, "hombre de Estado")​ es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.​ La estadística es una ciencia formal deductiva, con un conocimiento propio, dinámico y en continuo desarrollo obtenido a través del método científico formal. En ocasiones, las ciencias fácticas necesitan utilizar técnicas estadísticas durante su proceso de investigación factual, con el fin de obtener nuevos conocimientos basados en la experimentación y en la observación. En estos casos, la aplicación de la estadística permite el análisis de datos provenientes de una muestra representativa, que busca explicar las correlaciones y dependencias de un fenómeno físico o natural, de ocurrencia en forma aleatoria o condicional.
"""

Language:
```

----------------------------------------

TITLE: Defining Callable Functions for Azure OpenAI - Python
DESCRIPTION: Creates a list of function definitions following the expected JSON schema format. Each function includes its name, description, and the expected parameters (with types, descriptions, and optional enums). Example shown is a 'get_current_weather' function, which expects a location and a format. This object is passed into chat completion requests as the 'tools' argument.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/functions.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
functions = [
    {
        \"name\": \"get_current_weather\",
        \"description\": \"Get the current weather\",
        \"parameters\": {
            \"type\": \"object\",
            \"properties\": {
                \"location\": {
                    \"type\": \"string\",
                    \"description\": \"The city and state, e.g. San Francisco, CA\",
                },
                \"format\": {
                    \"type\": \"string\",
                    \"enum\": [\"celsius\", \"fahrenheit\"],
                    \"description\": \"The temperature unit to use. Infer this from the users location.\",
                },
            },
            \"required\": [\"location\"],
        },
    }
]
```

----------------------------------------

TITLE: Integrating Tools and Handling a Tool Call Response - Python
DESCRIPTION: Shows how to define the available agent tools, convert them to OpenAI-compatible schemas, and perform a model completion that includes tools. Provides example for accessing tool call results from the OpenAI response. Tools must conform to the schema generation process; OpenAI Python SDK required.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
messages = []

tools = [execute_refund, look_up_item]
tool_schemas = [function_to_schema(tool) for tool in tools]

response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[{"role": "user", "content": "Look up the black boot."}],
            tools=tool_schemas,
        )
message = response.choices[0].message

message.tool_calls[0].function
```

----------------------------------------

TITLE: Initializing a Pinecone-Based RetrievalQA Chain with LangChain - Python
DESCRIPTION: This snippet demonstrates how to create a RetrievalQA chain using LangChain with a Pinecone-powered retriever, enabling question-answering over a vector store. It relies on `langchain.chains.RetrievalQA`, an OpenAI LLM instance, and an existing document retriever (`docsearch.as_retriever()`). Required dependencies are LangChain, a configured OpenAI LLM, and a retriever connected to Pinecone. The output is an initialized RetrievalQA object, allowing further question answering through the tool.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_27

LANGUAGE: Python
CODE:
```
from langchain.chains import RetrievalQA

retrieval_llm = OpenAI(temperature=0)

podcast_retriever = RetrievalQA.from_chain_type(llm=retrieval_llm, chain_type="stuff", retriever=docsearch.as_retriever())
```

----------------------------------------

TITLE: Scenario-Based Prompting with LLMs - Text
DESCRIPTION: Illustrates the use of scenario/role-based prompting by instructing the LLM to assume the task of extracting an author's name from text. By stating a hypothetical or role description, this prompt method guides the model's behavior for more complex or imaginative queries. The output is expected to be the author's name only.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/articles/how_to_work_with_large_language_models.md#_snippet_2

LANGUAGE: text
CODE:
```
Your role is to extract the name of the author from any given text\n\n“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”\n― Ted Chiang, Exhalation
```

LANGUAGE: text
CODE:
```
 Ted Chiang
```

----------------------------------------

TITLE: Initialize OpenAI Client for Agentic Task
DESCRIPTION: Initializes the OpenAI client using an API key from environment variables or a fallback value. This setup is part of a sample agentic prompt designed for tasks like SWE-bench Verified, providing the necessary client object to interact with the OpenAI API.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/gpt4-1_prompting_guide.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.environ.get(
        "OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"
    )
)
```

----------------------------------------

TITLE: Triggering a Rate Limit Error With Fast Loop - OpenAI Python
DESCRIPTION: This code intentionally triggers an OpenAI API rate limit error by sending 100 chat completion requests in quick succession using a loop, suitable for demonstrating and testing error handling mechanisms. Input parameters such as 'model', 'messages', and 'max_tokens' are included for each request. It requires a previously created 'client' object and the appropriate API key. Returns responses or raises a 'RateLimitError' once the API's request threshold is exceeded.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_handle_rate_limits.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# request a bunch of completions in a loop\nfor _ in range(100):\n    client.chat.completions.create(\n        model="gpt-4o-mini",\n        messages=[{"role": "user", "content": "Hello"}],\n        max_tokens=10,\n    )
```

----------------------------------------

TITLE: Initializing OpenAI Client with Organization - Python
DESCRIPTION: This snippet initializes the OpenAI client for programmatic use within a specified organization, using an API key loaded from environment variables. The dependencies are the openai package, and the OpenAI API key must be set as an environment variable (OPENAI_API_KEY). Inputs are the API key and organization ID; the output is a client instance used for subsequent API calls. Ensure the organization string is correct and that the API key provided has access rights to the specified org.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Prompt_Caching101.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
from openai import OpenAI\nimport os\nimport json \nimport time\n\n\napi_key = os.getenv(\"OPENAI_API_KEY\")\nclient = OpenAI(organization='org-l89177bnhkme4a44292n5r3j', api_key=api_key)\n
```

----------------------------------------

TITLE: Initializing Environment and API Client - Python
DESCRIPTION: This snippet initializes the environment by optionally loading variables from a .env file using dotenv and instantiates the OpenAI API client with the key retrieved from environment variables or a fallback value. It also defines constants used later in the notebook for model and data paths. No additional external dependencies beyond those listed in imports are required. The input is reliant on a valid OpenAI API key being available either in the environment or directly provided.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Clustering_for_transaction_classification.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
# optional env import\nfrom dotenv import load_dotenv\nload_dotenv()
```

LANGUAGE: python
CODE:
```
# imports\n \nfrom openai import OpenAI\nimport pandas as pd\nimport numpy as np\nfrom sklearn.cluster import KMeans\nfrom sklearn.manifold import TSNE\nimport matplotlib\nimport matplotlib.pyplot as plt\nimport os\nfrom ast import literal_eval\n\nclient = OpenAI(api_key=os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as env var>"))\nCOMPLETIONS_MODEL = "gpt-3.5-turbo"\n\n# This path leads to a file with data and precomputed embeddings\nembedding_path = "data/library_transactions_with_embeddings_359.csv"
```

----------------------------------------

TITLE: Setting Up OpenAI API Key in .env File
DESCRIPTION: Creates a .env file and adds the OpenAI API key to it for secure access.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisqna/redisqna.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
OPENAI_API_KEY=your_key
```

----------------------------------------

TITLE: Define Customer Service System Prompt - Python
DESCRIPTION: This Python snippet defines the system prompt string `SYS_PROMPT_CUSTOMER_SERVICE`. It establishes the persona of a helpful customer service agent for 'NewTelco' and instructs the AI model to efficiently fulfill user requests while strictly following provided guidelines.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/gpt4-1_prompting_guide.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
SYS_PROMPT_CUSTOMER_SERVICE = """You are a helpful customer service agent working for NewTelco, helping a user efficiently fulfill their request while adhering closely to provided guidelines.\n
```

----------------------------------------

TITLE: Defining a Prompt Template Supporting History and Scratchpad in LangChain (Python)
DESCRIPTION: Creates a rich multi-input prompt template with embedded history and dynamic variables for use with conversation-aware LLM agents. Designed to guide agent reasoning, include conversation context, enumerate possible actions/tools, and define input/output structure for processing follow-up questions. Requires template strings referencing variables for tools, history, input, and agent scratchpad; does not perform any computation directly.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_14

LANGUAGE: python
CODE:
```
# Set up a prompt template which can interpolate the history
template_with_history = """You are SearchGPT, a professional search engine who provides informative answers to users. Answer the following questions as best you can. You have access to the following tools:

{tools}

Use the following format:

Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Begin! Remember to give detailed, informative answers

Previous conversation history:
{history}

New question: {input}
{agent_scratchpad}"""

```

----------------------------------------

TITLE: Providing GPT Prompt Instructions for Canvas Student Course Assistant - Markdown
DESCRIPTION: This snippet supplies a sample Markdown-formatted prompt to guide ChatGPT in assisting students with Canvas-hosted courses. No runtime dependencies are required, as instructions target prompt design. Parameters outlined include user query patterns (e.g., course requests, practice tests, study guides) and corresponding multistep response logic. The inputs are user questions; outputs are structured, context-aware AI responses. Constraints include using Canvas API call names and maintaining instructional clarity.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_canvas.md#_snippet_0

LANGUAGE: Markdown
CODE:
```
# **Context:** You support college students by providing detailed information about their courses hosted on the Canvas Learning Management System. You help them understand course content, generate practice exams based on provided materials, and offer insightful feedback to aid their learning journey. Assume the students are familiar with basic academic terminologies.

# **Instructions:**

## Scenarios

### - When the user asks for information about a specific course, follow this 5 step process:
1. Ask the user to specify the course they want assistance with and the particular area of focus (e.g., overall course overview, specific module).
2. If you do not know the Course ID for the course requested, use the listYourCourses to find the right course and corresponding ID in Canvas. If none of the courses listed returned courses that seem to match the course request, use the searchCourses to see if there are any similarly named course. 
3. Retrieve the course information from Canvas using the getSingleCourse API call and the listModules API call. 
4. Ask the user which module(s) they would like to focus on and use the listModuleItems to retrieve the requested module items. For any assignments, share links to them.
5. Ask if the user needs more information or if they need to prepare for an exam.

### When a user asks to take a practice test or practice exam for a specific course, follow this 6 step process:
1. Ask how many questions
2. Ask which chapters or topics they want to be tested on, provide a couple examples from the course modules in Canvas.
3. Ask 1 question at a time, be sure the questions are multiple choice (do not generate the next question until the question is answered)
4. When the user answers, tell them if its right or wrong and give a description for the correct answer 
5. Ask the user if they want to export the test results and write the code to create the PDF
6. Offer additional resources and study tips tailored to the user's needs and progress, and inquire if they require further assistance with other courses or topics.

### When a user asks to create a study guide
- Format the generated study guide in a table
```

----------------------------------------

TITLE: Storing Quotes with Embeddings in Astra DB
DESCRIPTION: Processes the philosophy quotes dataset in batches, computes embeddings for each quote using OpenAI, and stores the quotes, embeddings, and metadata in the Astra DB vector collection.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_AstraPy.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
BATCH_SIZE = 20

num_batches = ((len(philo_dataset) + BATCH_SIZE - 1) // BATCH_SIZE)

quotes_list = philo_dataset["quote"]
authors_list = philo_dataset["author"]
tags_list = philo_dataset["tags"]

print("Starting to store entries: ", end="")
for batch_i in range(num_batches):
    b_start = batch_i * BATCH_SIZE
    b_end = (batch_i + 1) * BATCH_SIZE
    # compute the embedding vectors for this batch
    b_emb_results = client.embeddings.create(
        input=quotes_list[b_start : b_end],
        model=embedding_model_name,
    )
    # prepare the documents for insertion
    b_docs = []
    for entry_idx, emb_result in zip(range(b_start, b_end), b_emb_results.data):
        if tags_list[entry_idx]:
            tags = {
                tag: True
                for tag in tags_list[entry_idx].split(";")
            }
        else:
            tags = {}
        b_docs.append({
            "quote": quotes_list[entry_idx],
            "$vector": emb_result.embedding,
            "author": authors_list[entry_idx],
            "tags": tags,
        })
    # write to the vector collection
    collection.insert_many(b_docs)
    print(f"[{len(b_docs)}]", end="")

print("\nFinished storing entries.")
```

----------------------------------------

TITLE: Comparing encodings with Japanese text
DESCRIPTION: Example of using compare_encodings to show how different tokenizers handle non-English text, specifically Japanese characters.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
compare_encodings("お誕生日おめでとう")
```

----------------------------------------

TITLE: Generating SQL Queries with GPT-4 Based on Database Schema
DESCRIPTION: This code defines a system prompt and user input to generate SQL queries for a given database schema. It uses GPT-4-turbo to create multiple example questions and corresponding SQL queries based on the provided schema information.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/Getting_Started_with_OpenAI_Evals.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
system_prompt = """You are a helpful assistant that can ask questions about a database table and write SQL queries to answer the question.
    A user will pass in a table schema and your job is to return a question answer pairing. The question should relevant to the schema of the table,
    and you can speculate on its contents. You will then have to generate a SQL query to answer the question. Below are some examples of what this should look like.

    Example 1
    ```````````
    User input: Table museum, columns = [*,Museum_ID,Name,Num_of_Staff,Open_Year]\nTable visit, columns = [*,Museum_ID,visitor_ID,Num_of_Ticket,Total_spent]\nTable visitor, columns = [*,ID,Name,Level_of_membership,Age]\nForeign_keys = [visit.visitor_ID = visitor.ID,visit.Museum_ID = museum.Museum_ID]\n
    Assistant Response:
    Q: How many visitors have visited the museum with the most staff?
    A: SELECT count ( * )  FROM VISIT AS T1 JOIN MUSEUM AS T2 ON T1.Museum_ID   =   T2.Museum_ID WHERE T2.Num_of_Staff   =   ( SELECT max ( Num_of_Staff )  FROM MUSEUM ) 
    ```````````

    Example 2
    ```````````
    User input: Table museum, columns = [*,Museum_ID,Name,Num_of_Staff,Open_Year]\nTable visit, columns = [*,Museum_ID,visitor_ID,Num_of_Ticket,Total_spent]\nTable visitor, columns = [*,ID,Name,Level_of_membership,Age]\nForeign_keys = [visit.visitor_ID = visitor.ID,visit.Museum_ID = museum.Museum_ID]\n
    Assistant Response:
    Q: What are the names who have a membership level higher than 4?
    A: SELECT Name   FROM VISITOR AS T1 WHERE T1.Level_of_membership   >   4 
    ```````````

    Example 3
    ```````````
    User input: Table museum, columns = [*,Museum_ID,Name,Num_of_Staff,Open_Year]\nTable visit, columns = [*,Museum_ID,visitor_ID,Num_of_Ticket,Total_spent]\nTable visitor, columns = [*,ID,Name,Level_of_membership,Age]\nForeign_keys = [visit.visitor_ID = visitor.ID,visit.Museum_ID = museum.Museum_ID]\n
    Assistant Response:
    Q: How many tickets of customer id 5?
    A: SELECT count ( * )  FROM VISIT AS T1 JOIN VISITOR AS T2 ON T1.visitor_ID   =   T2.ID WHERE T2.ID   =   5 
    ```````````
    """

user_input = "Table car_makers, columns = [*,Id,Maker,FullName,Country]\nTable car_names, columns = [*,MakeId,Model,Make]\nTable cars_data, columns = [*,Id,MPG,Cylinders,Edispl,Horsepower,Weight,Accelerate,Year]\nTable continents, columns = [*,ContId,Continent]\nTable countries, columns = [*,CountryId,CountryName,Continent]\nTable model_list, columns = [*,ModelId,Maker,Model]\nForeign_keys = [countries.Continent = continents.ContId,car_makers.Country = countries.CountryId,model_list.Maker = car_makers.Id,car_names.Model = model_list.Model,cars_data.Id = car_names.MakeId]"

messages = [{
        "role": "system",
        "content": system_prompt
    },
    {
        "role": "user",
        "content": user_input
    }
]

completion = client.chat.completions.create(
    model="gpt-4-turbo-preview",
    messages=messages,
    temperature=0.7,
    n=5
)

for choice in completion.choices:
    print(choice.message.content + "\n")
```

----------------------------------------

TITLE: Initializing OpenAI Client and Embeddings Model Settings (Python)
DESCRIPTION: Sets up OpenAI API key credential (either from environment variable or explicit input), initializes the OpenAI Python client, and specifies the embedding model name to be used for vectorization. This config block is essential for later steps that require document embedding via OpenAI APIs. Key parameters include the API key (env var OPENAI_API_KEY or literal placeholder) and embeddings_model name. No inputs required unless overriding via os.environ; outputs a configured OpenAI client and ready-to-use model name variable.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/rag-quickstart/azure/Azure_AI_Search_with_Azure_Functions_and_GPT_Actions_in_ChatGPT.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
openai_api_key = os.environ.get("OPENAI_API_KEY", "<your OpenAI API key if not set as an env var>") # Saving this as a variable to reference in function app in later step
openai_client = OpenAI(api_key=openai_api_key)
embeddings_model = "text-embedding-3-small" # We'll use this by default, but you can change to your text-embedding-3-large if desired
```

----------------------------------------

TITLE: Defining OpenAPI Endpoint for Retool Workflow Trigger - OpenAPI Schema (YAML)
DESCRIPTION: This YAML snippet defines an OpenAPI 3.1.0 specification for a custom endpoint that triggers a Retool workflow via an HTTP POST request. Prerequisites include a deployed Retool workflow and API key; parameters 'first' and 'second' represent numeric inputs required by the workflow and must be included in the request body. The endpoint responds with standard 200 (success), 400 (bad request), and 401 (unauthorized) codes, and expects API key authentication through a custom header. To use this, developers must replace '<WORKFLOW_ID>' in the path with their actual workflow ID, and the schema assumes integration in a ChatGPT Action setup.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_retool_workflow.md#_snippet_0

LANGUAGE: yaml
CODE:
```
openapi: 3.1.0\ninfo:\n  title: Retool Workflow API\n  description: API for interacting with Retool workflows.\n  version: 1.0.0\nservers:\n  - url: https://api.retool.com/v1\n    description: Main (production) server\npaths:\n  /workflows/<WORKFLOW_ID>/startTrigger:\n    post:\n      operationId: add_numbers\n      summary: Takes 2 numbers and adds them.\n      description: Initiates a workflow in Retool by triggering a specific workflow ID.\n      requestBody:\n        required: true\n        content:\n          application/json:\n            schema:\n              type: object\n              properties:\n                first:\n                  type: integer\n                  description: First parameter for the workflow.\n                second:\n                  type: integer\n                  description: Second parameter for the workflow.\n      responses:\n        \"200\":\n          description: Workflow triggered successfully.\n        \"400\":\n          description: Bad Request - Invalid parameters or missing data.\n        \"401\":\n          description: Unauthorized - Invalid or missing API key.\n      security:\n        - apiKeyAuth: []
```

----------------------------------------

TITLE: Running Multi-turn Chat Completions with Tools - Python
DESCRIPTION: Defines a function to perform completions with message and tool inputs using the OpenAI API, and includes a main handler to perform two sequential runs, simulating a real support workflow that appends a user follow-up. Requires the openai and json packages and a valid OpenAI client instance. Outputs usage data JSON for each run; expects 'messages', 'tools', and an additional user message object as parameters. Limitation: Assumes tools and API client are correctly configured elsewhere.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Prompt_Caching101.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
# Function to run completion with the provided message history and tools
def completion_run(messages, tools):
    completion = client.chat.completions.create(
        model="gpt-4o-mini",
        tools=tools,
        messages=messages,
        tool_choice="required"
    )
    usage_data = json.dumps(completion.to_dict(), indent=4)
    return usage_data

# Main function to handle the two runs
def main(messages, tools, user_query2):
    # Run 1: Initial query
    print("Run 1:")
    run1 = completion_run(messages, tools)
    print(run1)

    # Delay for 7 seconds
    time.sleep(7)

    # Append user_query2 to the message history
    messages.append(user_query2)

    # Run 2: With appended query
    print("\nRun 2:")
    run2 = completion_run(messages, tools)
    print(run2)


# Run the main function
main(messages, tools, user_query2)
```

----------------------------------------

TITLE: Loading Environment Variables for OpenAI and AWS - Python
DESCRIPTION: This snippet imports the necessary modules and loads environment variables using python-dotenv. It retrieves OpenAI and AWS credentials from the local environment, ensuring authentication for subsequent API calls. Dependencies include openai, boto3, dotenv, and standard libraries such as os and datetime.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
from openai import OpenAI\nimport json\nimport boto3\nimport os\nimport datetime\nfrom urllib.request import urlretrieve\n\n# load environment variables\nfrom dotenv import load_dotenv\nload_dotenv() 
```

----------------------------------------

TITLE: Implementing Document Relevance Function
DESCRIPTION: Creating a function to assess document relevance using GPT with retry logic and custom prompt engineering.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Search_reranking_with_cross-encoders.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
@retry(wait=wait_random_exponential(min=1, max=40), stop=stop_after_attempt(3))
def document_relevance(query, document):
    response = openai.chat.completions.create(
        model="text-davinci-003",
        message=prompt.format(query=query, document=document),
        temperature=0,
        logprobs=True,
        logit_bias={3363: 1, 1400: 1},
    )

    return (
        query,
        document,
        response.choices[0].message.content,
        response.choices[0].logprobs.token_logprobs[0],
    )
```

----------------------------------------

TITLE: Authenticating to Azure OpenAI using Azure Active Directory - Python
DESCRIPTION: Shows how to authenticate to Azure OpenAI via Azure Active Directory using 'DefaultAzureCredential' and 'get_bearer_token_provider'. Assumes the environment is appropriately configured for AAD credentials. Requires the 'azure-identity' package. The client is created with a bearer token provider that automatically obtains and refreshes access tokens for the Azure Cognitive Services scope.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
from azure.identity import DefaultAzureCredential, get_bearer_token_provider

if use_azure_active_directory:
    endpoint = os.environ["AZURE_OPENAI_ENDPOINT"]

    client = openai.AzureOpenAI(
        azure_endpoint=endpoint,
        azure_ad_token_provider=get_bearer_token_provider(DefaultAzureCredential(), "https://cognitiveservices.azure.com/.default"),
        api_version="2023-09-01-preview"
    )
```

----------------------------------------

TITLE: Defining Routine, Tools, and Example Agent - Python
DESCRIPTION: Defines a customer service routine as a system prompt for an OpenAI assistant, including multi-step, conditional instructions. Implements stub functions for 'look_up_item' and 'execute_refund' to use as agent tools. These functions represent callable tools that will be invoked by the agent in later orchestration steps. Requires Python standard library.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Orchestrating_agents.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
# Customer Service Routine

system_message = (
    "You are a customer support agent for ACME Inc."
    "Always answer in a sentence or less."
    "Follow the following routine with the user:"
    "1. First, ask probing questions and understand the user's problem deeper.\n"
    " - unless the user has already provided a reason.\n"
    "2. Propose a fix (make one up).\n"
    "3. ONLY if not satisfied, offer a refund.\n"
    "4. If accepted, search for the ID and then execute refund."
    ""
)

def look_up_item(search_query):
    """Use to find item ID.
    Search query can be a description or keywords."""

    # return hard-coded item ID - in reality would be a lookup
    return "item_132612938"


def execute_refund(item_id, reason="not provided"):

    print("Summary:", item_id, reason) # lazy summary
    return "success"

```

----------------------------------------

TITLE: Loading Data Files into Redis as JSON Objects with Text and Vector Fields
DESCRIPTION: Reads content from files, creates embeddings using OpenAI's API, and stores both content and embeddings as JSON objects in Redis.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisqna/redisqna.ipynb#_snippet_8

LANGUAGE: python
CODE:
```
directory = './assets/'
model = 'text-embedding-3-small'
i = 1

for file in os.listdir(directory):
    with open(os.path.join(directory, file), 'r') as f:
        content = f.read()
        # Create the embedding using the new client-based method
        response = oai_client.embeddings.create(
            model=model,
            input=[content]
        )
        # Access the embedding from the response object
        vector = response.data[0].embedding
        
        # Store the content and vector using your JSON client
        client.json().set(f'doc:{i}', '$', {'content': content, 'vector': vector})
    i += 1
```

----------------------------------------

TITLE: Defining Structured Output Format for Wine Classification
DESCRIPTION: Creates a JSON schema for structured outputs to ensure consistent and valid responses from the model for grape variety classification.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Leveraging_model_distillation_to_fine-tune_a_model.ipynb#_snippet_6

LANGUAGE: python
CODE:
```
response_format = {
    "type": "json_schema",
    "json_schema": {
        "name": "grape-variety",
        "schema": {
            "type": "object",
            "properties": {
                "variety": {
                    "type": "string",
                    "enum": varieties.tolist()
                }
            },
            "additionalProperties": False,
            "required": ["variety"],
        },
        "strict": True
    }
}
```

----------------------------------------

TITLE: Defining OpenAPI Specification for ChatGPT Middleware - Python
DESCRIPTION: This code snippet provides a template for crafting an OpenAPI 3.1.0 schema in YAML syntax, suitable for defining backend API endpoints that a custom GPT action can interact with. It details where to input the function application and endpoint information, crucial for OAuth authenticated connections from ChatGPT. The parameters such as title, description, function_app_name, and endpoint id need to be specified by the implementer. The expected input is a post request to a secure URL; the output and operation specifics should be filled as per the application's functionality. Ensure you have the OpenAPI specification properly completed and that endpoint IDs and authentication details are kept confidential.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
openapi: 3.1.0\ninfo:\n  title: {insert title}\n  description: {insert description}\n  version: 1.0.0\nservers:\n  - url: https://{your_function_app_name}.azurewebsites.net/api\n    description: {insert description}\npaths:\n  /{your_function_name}?code={enter your specific endpoint id here}:\n    post:\n      operationId: {insert operationID}\n      summary: {insert summary}\n      requestBody: \n{the rest of this is specific to your application}\n
```

----------------------------------------

TITLE: Basic Chain-of-Thought Prompt Instruction
DESCRIPTION: A simple instruction to append to a prompt to encourage the model to perform step-by-step reasoning, specifically focusing on identifying and listing relevant documents by title and ID.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/gpt4-1_prompting_guide.ipynb#_snippet_11

LANGUAGE: text
CODE:
```
...

First, think carefully step by step about what documents are needed to answer the query. Then, print out the TITLE and ID of each document. Then, format the IDs into a list.
```

----------------------------------------

TITLE: Creating Text Embeddings with OpenAI API
DESCRIPTION: Defines a function to create embeddings using OpenAI's API and processes three text examples into document objects with vectors.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/redis/redisjson/redisjson.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import openai
import os
from dotenv import load_dotenv

load_dotenv()
openai.api_key = os.getenv("OPENAI_API_KEY")

def get_vector(text, model="text-embedding-3-small"):
    text = text.replace("\n", " ")
    return openai.Embedding.create(input = [text], model = model)['data'][0]['embedding']

text_1 = """Japan narrowly escapes recession

Japan's economy teetered on the brink of a technical recession in the three months to September, figures show.

Revised figures indicated growth of just 0.1% - and a similar-sized contraction in the previous quarter. On an annual basis, the data suggests annual growth of just 0.2%, suggesting a much more hesitant recovery than had previously been thought. A common technical definition of a recession is two successive quarters of negative growth.
The government was keen to play down the worrying implications of the data. "I maintain the view that Japan's economy remains in a minor adjustment phase in an upward climb, and we will monitor developments carefully," said economy minister Heizo Takenaka. But in the face of the strengthening yen making exports less competitive and indications of weakening economic conditions ahead, observers were less sanguine. "It's painting a picture of a recovery... much patchier than previously thought," said Paul Sheard, economist at Lehman Brothers in Tokyo. Improvements in the job market apparently have yet to feed through to domestic demand, with private consumption up just 0.2% in the third quarter.
"""

text_2 = """Dibaba breaks 5,000m world record

Ethiopia's Tirunesh Dibaba set a new world record in winning the women's 5,000m at the Boston Indoor Games.

Dibaba won in 14 minutes 32.93 seconds to erase the previous world indoor mark of 14:39.29 set by another Ethiopian, Berhane Adera, in Stuttgart last year. But compatriot Kenenisa Bekele's record hopes were dashed when he miscounted his laps in the men's 3,000m and staged his sprint finish a lap too soon. Ireland's Alistair Cragg won in 7:39.89 as Bekele battled to second in 7:41.42. "I didn't want to sit back and get out-kicked," said Cragg. "So I kept on the pace. The plan was to go with 500m to go no matter what, but when Bekele made the mistake that was it. The race was mine." Sweden's Carolina Kluft, the Olympic heptathlon champion, and Slovenia's Jolanda Ceplak had winning performances, too. Kluft took the long jump at 6.63m, while Ceplak easily won the women's 800m in 2:01.52. 
"""


text_3 = """Google's toolbar sparks concern

Search engine firm Google has released a trial tool which is concerning some net users because it directs people to pre-selected commercial websites.

The AutoLink feature comes with Google's latest toolbar and provides links in a webpage to Amazon.com if it finds a book's ISBN number on the site. It also links to Google's map service, if there is an address, or to car firm Carfax, if there is a licence plate. Google said the feature, available only in the US, "adds useful links". But some users are concerned that Google's dominant position in the search engine market place could mean it would be giving a competitive edge to firms like Amazon.

AutoLink works by creating a link to a website based on information contained in a webpage - even if there is no link specified and whether or not the publisher of the page has given permission.

If a user clicks the AutoLink feature in the Google toolbar then a webpage with a book's unique ISBN number would link directly to Amazon's website. It could mean online libraries that list ISBN book numbers find they are directing users to Amazon.com whether they like it or not. Websites which have paid for advertising on their pages may also be directing people to rival services. Dan Gillmor, founder of Grassroots Media, which supports citizen-based media, said the tool was a "bad idea, and an unfortunate move by a company that is looking to continue its hypergrowth". In a statement Google said the feature was still only in beta, ie trial, stage and that the company welcomed feedback from users. It said: "The user can choose never to click on the AutoLink button, and web pages she views will never be modified. "In addition, the user can choose to disable the AutoLink feature entirely at any time."

The new tool has been compared to the Smart Tags feature from Microsoft by some users. It was widely criticised by net users and later dropped by Microsoft after concerns over trademark use were raised. Smart Tags allowed Microsoft to link any word on a web page to another site chosen by the company. Google said none of the companies which received AutoLinks had paid for the service. Some users said AutoLink would only be fair if websites had to sign up to allow the feature to work on their pages or if they received revenue for any "click through" to a commercial site. Cory Doctorow, European outreach coordinator for digital civil liberties group Electronic Fronter Foundation, said that Google should not be penalised for its market dominance. "Of course Google should be allowed to direct people to whatever proxies it chooses. "But as an end user I would want to know - 'Can I choose to use this service?', 'How much is Google being paid?', 'Can I substitute my own companies for the ones chosen by Google?'." Mr Doctorow said the only objection would be if users were forced into using AutoLink or "tricked into using the service".
"""

doc_1 = {"content": text_1, "vector": get_vector(text_1)}
doc_2 = {"content": text_2, "vector": get_vector(text_2)}
doc_3 = {"content": text_3, "vector": get_vector(text_3)}
```

----------------------------------------

TITLE: Creating a Pinecone Index Dynamically using Embedding Dimensions in Python
DESCRIPTION: Determines the embedding dimensionality by generating an embedding for a sample document using OpenAI's API, and proceeds to create a Pinecone vector index using that dimension. Ensures index is uniquely named and avoids recreation if it exists. Inputs: DataFrame with merged text; output: Pinecone index with embedding-ready configuration. Dependencies: OpenAI API, Pinecone client, environment variables for authentication.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_api_tool_orchestration.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
MODEL = "text-embedding-3-small"  # Replace with your production embedding model if needed
# Compute an embedding for the first document to obtain the embedding dimension.
sample_embedding_resp = client.embeddings.create(
    input=[ds_dataframe['merged'].iloc[0]],
    model=MODEL
)
embed_dim = len(sample_embedding_resp.data[0].embedding)
print(f"Embedding dimension: {embed_dim}")
```

----------------------------------------

TITLE: Loading Environment Variables Securely in Python using python-dotenv
DESCRIPTION: Facilitates secure management of sensitive configuration such as API keys by loading from .env files using python-dotenv. This code includes optional installation and usage pattern, setting up OpenAI environment variable manually, and includes commented-out print/debug lines.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/RAG_with_graph_db.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Optional: run to load environment variables from a .env file.
# This is not required if you have exported your env variables in another way or if you set it manually
!pip3 install python-dotenv
from dotenv import load_dotenv
load_dotenv()

# Set the OpenAI API key env variable manually
# os.environ["OPENAI_API_KEY"] = "<your_api_key>"

# print(os.environ["OPENAI_API_KEY"])
```

----------------------------------------

TITLE: Creating an LLM-Based Grader Configuration and Prompt Templates in Python
DESCRIPTION: This snippet defines a classification prompt, user prompt template, and configuration for a model-based grader for push notification summaries. The grader is an LLM labeler which categorizes the summary's quality, using variables injected from the Eval schema. Key parameters include model name, roles, allowed labels, and passing criteria. Requires contextual variables ({{item.notifications}}, {{sample.output_text}}) to be filled by the Eval runtime.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/use-cases/bulk-experimentation.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
GRADER_DEVELOPER_PROMPT = """\nCategorize the following push notification summary into the following categories:\n1. concise-and-snappy\n2. drops-important-information\n3. verbose\n4. unclear\n5. obscures-meaning\n6. other \n\nYou'll be given the original list of push notifications and the summary like this:\n\n<push_notifications>\n...notificationlist...\n</push_notifications>\n<summary>\n...summary...\n</summary>\n\nYou should only pick one of the categories above, pick the one which most closely matches and why.\n"""\nGRADER_TEMPLATE_PROMPT = """\n<push_notifications>{{item.notifications}}</push_notifications>\n<summary>{{sample.output_text}}</summary>\n"""\npush_notification_grader = {\n    \"name\": \"Push Notification Summary Grader\",\n    \"type\": \"label_model\",\n    \"model\": \"o3-mini\",\n    \"input\": [\n        {\n            \"role\": \"developer\",\n            \"content\": GRADER_DEVELOPER_PROMPT,\n        },\n        {\n            \"role\": \"user\",\n            \"content\": GRADER_TEMPLATE_PROMPT,\n        },\n    ],\n    \"passing_labels\": [\"concise-and-snappy\"],\n    \"labels\": [\n        \"concise-and-snappy\",\n        \"drops-important-information\",\n        \"verbose\",\n        \"unclear\",\n        \"obscures-meaning\",\n        \"other\",\n    ],\n}
```

----------------------------------------

TITLE: Exporting DataFrame to CSV using Pandas in Python
DESCRIPTION: This snippet saves the DataFrame ('results_df') to a CSV file named 'hallucination_results.csv' without the index column. The 'to_csv' function from pandas is used, which requires prior creation of the DataFrame. The exported file serves as persistent storage for downstream evaluation and enables result sharing or backup.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Developing_hallucination_guardrails.ipynb#_snippet_12

LANGUAGE: python
CODE:
```
results_df.to_csv('hallucination_results.csv', index=False)\n
```

----------------------------------------

TITLE: Retrieving Secure Credentials for MongoDB and OpenAI - Python
DESCRIPTION: Prompts the user to securely input the MongoDB Atlas Cluster URI and the OpenAI API Key using Python's getpass module. This ensures that sensitive credentials are not exposed in the code or output. The variables 'MONGODB_ATLAS_CLUSTER_URI' and 'OPENAI_API_KEY' will store these secrets for later connection setup, with manual entry required for both.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/mongodb_atlas/semantic_search_using_mongodb_atlas_vector_search.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import getpass

MONGODB_ATLAS_CLUSTER_URI = getpass.getpass("MongoDB Atlas Cluster URI:")
OPENAI_API_KEY = getpass.getpass("OpenAI API Key:")

```

----------------------------------------

TITLE: Initializing OpenAI and GPT Model for Python
DESCRIPTION: Imports the OpenAI library and sets the GPT model to be used for the guardrails implementation.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_use_guardrails.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
import openai

GPT_MODEL = 'gpt-4o-mini'
```

----------------------------------------

TITLE: Defining Gmail Email API Schema with OpenAPI in Python
DESCRIPTION: This snippet outlines the complete OpenAPI 3.1.0 specification for Gmail operations, including endpoint definitions, HTTP methods, parameters, request and response schemas, and data models such as Message, FullMessage, Draft, LabelModification, and EmailDraft. Although formatted as a Python code block, the content is actually YAML and intended to be pasted into Custom GPT Actions for integration with the Gmail API. Inputs include email and message IDs, and returned objects follow the structures outlined in the 'components/schemas' section; authentication and correct request payloads are required for successful usage. Limitations include strict adherence to the schema for request/response payloads and the necessity of having appropriate OAuth credentials for resource access.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
openapi: 3.1.0

info:
  title: Gmail Email API
  version: 1.0.0
  description: API to read, write, and send emails in a Gmail account.

servers:
  - url: https://gmail.googleapis.com

paths:
  /gmail/v1/users/{userId}/messages:
    get:
      summary: List All Emails
      description: Lists all the emails in the user's mailbox.
      operationId: listAllEmails
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
        - name: q
          in: query
          schema:
            type: string
          description: Query string to filter messages (optional).
        - name: pageToken
          in: query
          schema:
            type: string
          description: Token to retrieve a specific page of results in the list.
        - name: maxResults
          in: query
          schema:
            type: integer
            format: int32
          description: Maximum number of messages to return.
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MessageList'
        '400':
          description: Bad Request
        '401':
          description: Unauthorized
        '403':
          description: Forbidden
        '404':
          description: Not Found
        '500':
          description: Internal Server Error

  /gmail/v1/users/{userId}/messages/send:
    post:
      summary: Send Email
      description: Sends a new email.
      operationId: sendEmail
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Message'
      responses:
        '200':
          description: Email sent successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Message'
        '400':
          description: Bad Request
        '401':
          description: Unauthorized
        '403':
          description: Forbidden
        '500':
          description: Internal Server Error

  /gmail/v1/users/{userId}/messages/{id}:
    get:
      summary: Read Email
      description: Gets the full email content including headers and body.
      operationId: readEmail
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
        - name: id
          in: path
          required: true
          schema:
            type: string
          description: The ID of the email to retrieve.
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/FullMessage'
        '400':
          description: Bad Request
        '401':
          description: Unauthorized
        '403':
          description: Forbidden
        '404':
          description: Not Found
        '500':
          description: Internal Server Error

  /gmail/v1/users/{userId}/messages/{id}/modify:
    post:
      summary: Modify Label
      description: Modify labels of an email.
      operationId: modifyLabels
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
        - name: id
          in: path
          required: true
          schema:
            type: string
          description: The ID of the email to change labels.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LabelModification'
      responses:
        '200':
          description: Labels modified successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Message'
        '400':
          description: Bad Request
        '401':
          description: Unauthorized
        '403':
          description: Forbidden
        '500':
          description: Internal Server Error

  /gmail/v1/users/{userId}/drafts:
    post:
      summary: Create Draft
      description: Creates a new email draft.
      operationId: createDraft
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Draft'
      responses:
        '200':
          description: Draft created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Draft'
        '400':
          description: Bad Request
        '401':
          description: Unauthorized
        '403':
          description: Forbidden
        '500':
          description: Internal Server Error

  /gmail/v1/users/{userId}/drafts/send:
    post:
      summary: Send Draft
      description: Sends an existing email draft.
      operationId: sendDraft
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SendDraftRequest'
      responses:
        '200':
          description: Draft sent successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Message'
        '400':
          description: Bad Request
        '401':
          description: Unauthorized
        '403':
          description: Forbidden
        '500':
          description: Internal Server Error

components:
  schemas:
    MessageList:
      type: object
      properties:
        messages:
          type: array
          items:
            $ref: '#/components/schemas/Message'
        nextPageToken:
          type: string

    Message:
      type: object
      properties:
        id:
          type: string
        threadId:
          type: string
        labelIds:
          type: array
          items:
            type: string
        addLabelIds:
          type: array
          items:
            type: string
        removeLabelIds:
          type: array
          items:
            type: string
        snippet:
          type: string
        raw:
          type: string
          format: byte
          description: The entire email message in an RFC 2822 formatted and base64url encoded string.

    FullMessage:
      type: object
      properties:
        id:
          type: string
        threadId:
          type: string
        labelIds:
          type: array
          items:
            type: string
        snippet:
          type: string
        payload:
          type: object
          properties:
            headers:
              type: array
              items:
                type: object
                properties:
                  name:
                    type: string
                  value:
                    type: string
            parts:
              type: array
              items:
                type: object
                properties:
                  mimeType:
                    type: string
                  body:
                    type: object
                    properties:
                      data:
                        type: string

    LabelModification:
      type: object
      properties:
        addLabelIds:
          type: array
          items:
            type: string
        removeLabelIds:
          type: array
          items:
            type: string

    Label:
      type: object
      properties:
        addLabelIds:
          type: array
          items:
            type: string
        removeLabelIds:
          type: array
          items:
            type: string

    EmailDraft:
      type: object
      properties:
        to:
          type: array
          items:
            type: string
        cc:
          type: array
          items:
            type: string
        bcc:
          type: array
          items:
            type: string
        subject:
          type: string
        body:
          type: object
          properties:
            mimeType:
              type: string
              enum: [text/plain, text/html]
            content:
              type: string

    Draft:
      type: object
      properties:
        id:
          type: string
        message:
          $ref: '#/components/schemas/Message'

    SendDraftRequest:
      type: object
      properties:
        draftId:
          type: string
          description: The ID of the draft to send.
        userId:
          type: string
          description: The user's email address. Use "me" to indicate the authenticated user.
```

----------------------------------------

TITLE: Retrieving and Converting SharePoint Drive Item Content (Node.js JavaScript)
DESCRIPTION: This JavaScript async function fetches and processes the contents of files stored in SharePoint/OneDrive via the Microsoft Graph API. It handles file type checks, automatic conversion of eligible files to PDF for text extraction, and different strategies for processing PDF, text, and CSV files. It uses dependency 'pdfParse' for PDF text extraction and 'Buffer' for binary data handling, and gracefully returns errors for unsupported file types. Required dependencies: Microsoft Graph JavaScript SDK, pdfParse, and Node.js Buffer class. Inputs are client (Graph API client), driveId, itemId, and file name; output is the extracted text content or an error message.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/sharepoint_azure_function/Using_Azure_Functions_and_Microsoft_Graph_to_Query_SharePoint.md#_snippet_6

LANGUAGE: javascript
CODE:
```
const getDriveItemContent = async (client, driveId, itemId, name) => {\n    try {\n        const fileType = path.extname(name).toLowerCase();\n        // the below files types are the ones that are able to be converted to PDF to extract the text. See https://learn.microsoft.com/en-us/graph/api/driveitem-get-content-format?view=graph-rest-1.0&tabs=http\n        const allowedFileTypes = ['.pdf', '.doc', '.docx', '.odp', '.ods', '.odt', '.pot', '.potm', '.potx', '.pps', '.ppsx', '.ppsxm', '.ppt', '.pptm', '.pptx', '.rtf'];\n        // filePath changes based on file type, adding ?format=pdf to convert non-pdf types to pdf for text extraction, so all files in allowedFileTypes above are converted to pdf\n        const filePath = `/drives/${driveId}/items/${itemId}/content` + ((fileType === '.pdf' || fileType === '.txt' || fileType === '.csv') ? '' : '?format=pdf');\n        if (allowedFileTypes.includes(fileType)) {\n            response = await client.api(filePath).getStream();\n            // The below takes the chunks in response and combines\n            let chunks = [];\n            for await (let chunk of response) {\n                chunks.push(chunk);\n            }\n            let buffer = Buffer.concat(chunks);\n            // the below extracts the text from the PDF.\n            const pdfContents = await pdfParse(buffer);\n            return pdfContents.text;\n        } else if (fileType === '.txt') {\n            // If the type is txt, it does not need to create a stream and instead just grabs the content\n            response = await client.api(filePath).get();\n            return response;\n        }  else if (fileType === '.csv') {\n            response = await client.api(filePath).getStream();\n            let chunks = [];\n            for await (let chunk of response) {\n                chunks.push(chunk);\n            }\n            let buffer = Buffer.concat(chunks);\n            let dataString = buffer.toString('utf-8');\n            return dataString\n            \n    } else {\n        return 'Unsupported File Type';\n    }\n     \n    } catch (error) {\n        console.error('Error fetching drive content:', error);\n        throw new Error(`Failed to fetch content for ${name}: ${error.message}`);\n    }\n};
```

----------------------------------------

TITLE: Splitting LaTeX Content and Counting Tokens per Chunk - Python
DESCRIPTION: Splits LaTeX content into chunks using double newlines as delimiters, computes the token count for each chunk using the tokenizer, and prints statistics on chunk sizes. Assumes 'text' and 'tokenizer' are defined as in previous steps. Outputs the maximum chunk size and the total number of chunks for downstream batching.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/book_translation/translate_latex_book.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
chunks = text.split('\n\n')
ntokens = []
for chunk in chunks:
    ntokens.append(len(tokenizer.encode(chunk)))
print("Size of the largest chunk: ", max(ntokens))
print("Number of chunks: ", len(chunks))
```

----------------------------------------

TITLE: Querying ChatGPT for Olympics Information
DESCRIPTION: Sets up the OpenAI API key and sends a query to ChatGPT about the 2022 Olympics curling gold medal.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/SingleStoreDB/OpenAI_wikipedia_semantic_search.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
openai.api_key = 'OPENAI API KEY'

response = openai.ChatCompletion.create(
  model=GPT_MODEL,
  messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Who won the gold medal for curling in Olymics 2022?"},
    ]
)

print(response['choices'][0]['message']['content'])
```

----------------------------------------

TITLE: Building LLM Prompt and Assessing Claims with Context (OpenAI, Python)
DESCRIPTION: Defines two functions: `build_prompt_with_context` creates a structured prompt for an LLM using a claim and provided context, and `assess_claims_with_context` iterates through claims and their contexts, calls the OpenAI API (`client.chat.completions.create`) to get assessments, and handles cases with no context.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/chroma/hyde-with-chroma-and-openai.ipynb#_snippet_13

LANGUAGE: Python
CODE:
```
def build_prompt_with_context(claim, context):
    return [{'role': 'system', 'content': "I will ask you to assess whether a particular scientific claim, based on evidence provided. Output only the text 'True' if the claim is true, 'False' if the claim is false, or 'NEE' if there's not enough evidence."}, {'role': 'user', 'content': f"""
The evidence is the following:

{' '.join(context)}

Assess the following claim on the basis of the evidence. Output only the text 'True' if the claim is true, 'False' if the claim is false, or 'NEE' if there's not enough evidence. Do not output any other text.

Claim:
{claim}

Assessment:
"""}]

def assess_claims_with_context(claims, contexts):
    responses = []
    # Query the OpenAI API
    for claim, context in zip(claims, contexts):
        # If no evidence is provided, return NEE
        if len(context) == 0:
            responses.append('NEE')
            continue
        response = client.chat.completions.create(
            model=OPENAI_MODEL,
            messages=build_prompt_with_context(claim=claim, context=context),
            max_tokens=3,
        )
        # Strip any punctuation or whitespace from the response
        responses.append(response.choices[0].message.content.strip('., '))

    return responses
```

----------------------------------------

TITLE: Loading Environment Variables Using dotenv in Node.js
DESCRIPTION: This JavaScript snippet loads environment variables from a .env file using the 'dotenv' package, assigns Supabase configuration values to local variables, and prepares them for use by Supabase client instantiation. Requires 'dotenv' to be installed and .env file present. Output: 'supabaseUrl' and 'supabaseServiceRoleKey' variables available.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/supabase/semantic-search.mdx#_snippet_14

LANGUAGE: javascript
CODE:
```
import { config } from \"dotenv\";\n\n// Load .env file\nconfig();\n\nconst supabaseUrl = process.env[\"SUPABASE_URL\"];\nconst supabaseServiceRoleKey = process.env[\"SUPABASE_SERVICE_ROLE_KEY\"];
```

----------------------------------------

TITLE: Token Chunking with Tiktoken
DESCRIPTION: Function that encodes text into tokens and splits them into chunks using OpenAI's tiktoken tokenizer. Supports different encoding schemes including cl100k_base.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/rag-quickstart/gcp/Getting_started_with_bigquery_vector_search_and_openai.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
def chunked_tokens(text, chunk_length, encoding_name='cl100k_base'):
    # Get the encoding object for the specified encoding name. OpenAI's tiktoken library, which is used in this notebook, currently supports two encodings: 'bpe' and 'cl100k_base'. The 'bpe' encoding is used for GPT-3 and earlier models, while 'cl100k_base' is used for newer models like GPT-4.
    encoding = tiktoken.get_encoding(encoding_name)
    # Encode the input text into tokens
    tokens = encoding.encode(text)
    # Create an iterator that yields chunks of tokens of the specified length
    chunks_iterator = batched(tokens, chunk_length)
    # Yield each chunk from the iterator
    yield from chunks_iterator
```

----------------------------------------

TITLE: Generating Hypothetical Evidence with OpenAI API (Python)
DESCRIPTION: This Python function `hallucinate_evidence` takes a list of claims and queries the OpenAI API for each claim. It uses a specific model and a prompt built by `build_hallucination_prompt` to generate hypothetical evidence documents. The function returns a list of these generated documents.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/chroma/hyde-with-chroma-and-openai.ipynb#_snippet_19

LANGUAGE: python
CODE:
```
def hallucinate_evidence(claims):
    responses = []
    # Query the OpenAI API
    for claim in claims:
        response = client.chat.completions.create(
            model=OPENAI_MODEL,
            messages=build_hallucination_prompt(claim),
        )
        responses.append(response.choices[0].message.content)
    return responses
```

----------------------------------------

TITLE: Authenticating to Azure OpenAI using Azure Active Directory in Python
DESCRIPTION: Initializes an OpenAI client for Azure using a dynamically-managed Azure Active Directory token credential. Uses 'DefaultAzureCredential' and 'get_bearer_token_provider' from 'azure.identity' for secure authentication and token refresh. Pulls endpoint and deployment ID from environment variables as above. This code block requires the 'azure-identity' and 'openai' packages.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat_with_your_own_data.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
from azure.identity import DefaultAzureCredential, get_bearer_token_provider

if use_azure_active_directory:
    endpoint = os.environ["AZURE_OPENAI_ENDPOINT"]
    api_key = os.environ["AZURE_OPENAI_API_KEY"]
    # set the deployment name for the model we want to use
    deployment = "<deployment-id-of-the-model-to-use>"

    client = openai.AzureOpenAI(
        base_url=f"{endpoint}/openai/deployments/{deployment}/extensions",
        azure_ad_token_provider=get_bearer_token_provider(DefaultAzureCredential(), "https://cognitiveservices.azure.com/.default"),
        api_version="2023-09-01-preview"
    )
```

----------------------------------------

TITLE: Defining Weather API Function Specifications for Tool Use - Python
DESCRIPTION: Specifies a list of tool (function) definitions according to the OpenAI Chat Completion API tool schema. These allow the model to generate structured calls for current weather and N-day forecasts, including required parameters such as location, format (with enum), and number of days. The snippet is a setup step for enabling model-guided function argument generation.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_call_functions_with_chat_models.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
tools = [\n    {\n        "type": "function",\n        "function": {\n            "name": "get_current_weather",\n            "description": "Get the current weather",\n            "parameters": {\n                "type": "object",\n                "properties": {\n                    "location": {\n                        "type": "string",\n                        "description": "The city and state, e.g. San Francisco, CA",\n                    },\n                    "format": {\n                        "type": "string",\n                        "enum": ["celsius", "fahrenheit"],\n                        "description": "The temperature unit to use. Infer this from the users location.",\n                    },\n                },\n                "required": ["location", "format"],\n            },\n        }\n    },\n    {\n        "type": "function",\n        "function": {\n            "name": "get_n_day_weather_forecast",\n            "description": "Get an N-day weather forecast",\n            "parameters": {\n                "type": "object",\n                "properties": {\n                    "location": {\n                        "type": "string",\n                        "description": "The city and state, e.g. San Francisco, CA",\n                    },\n                    "format": {\n                        "type": "string",\n                        "enum": ["celsius", "fahrenheit"],\n                        "description": "The temperature unit to use. Infer this from the users location.",\n                    },\n                    "num_days": {\n                        "type": "integer",\n                        "description": "The number of days to forecast",\n                    }\n                },\n                "required": ["location", "format", "num_days"]\n            },\n        }\n    },\n]
```

----------------------------------------

TITLE: Calculating Token Usage for GPT-4 Model
DESCRIPTION: Estimates the total number of tokens and associated costs for running completions on the wine dataset using GPT-4 and GPT-4-mini models.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Leveraging_model_distillation_to_fine-tune_a_model.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
enc = tiktoken.encoding_for_model("gpt-4o")

total_tokens = 0

for index, row in df_france_subset.iterrows():
    prompt = generate_prompt(row, varieties)
    tokens = enc.encode(prompt)
    token_count = len(tokens)
    total_tokens += token_count

print(f"Total number of tokens in the dataset: {total_tokens}")
print(f"Total number of prompts: {len(df_france_subset)}")
```

LANGUAGE: python
CODE:
```
gpt4o_token_price = 2.50 / 1_000_000  # $2.50 per 1M tokens
gpt4o_mini_token_price = 0.150 / 1_000_000  # $0.15 per 1M tokens

total_gpt4o_cost = gpt4o_token_price*total_tokens
total_gpt4o_mini_cost = gpt4o_mini_token_price*total_tokens

print(total_gpt4o_cost)
print(total_gpt4o_mini_cost)
```

----------------------------------------

TITLE: Submitting a Quiz and Monitoring Status (OpenAI Assistants API, Python)
DESCRIPTION: This code sends a thread/run request to the Assistant to prepare a quiz with both open ended and multiple choice questions, waits for completion, and checks the run status. Inputs: Query string about quiz requirements. Outputs: Thread, run, and run.status objects from OpenAI's API. Requires earlier setup of Assistant and helper functions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Assistants_API_overview_python.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
thread, run = create_thread_and_run(
    "Make a quiz with 2 questions: One open ended, one multiple choice. Then, give me feedback for the responses."
)
run = wait_on_run(run, thread)
run.status
```

----------------------------------------

TITLE: Generating Embeddings for Code Functions
DESCRIPTION: Code for creating embeddings from the extracted functions using OpenAI's text-embedding-3-small model and storing the results in a DataFrame.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Code_search_using_embeddings.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
from utils.embeddings_utils import get_embedding

df = pd.DataFrame(all_funcs)
df['code_embedding'] = df['code'].apply(lambda x: get_embedding(x, model='text-embedding-3-small'))
df['filepath'] = df['filepath'].map(lambda x: Path(x).relative_to(code_root))
df.to_csv("data/code_search_openai-python.csv", index=False)
df.head()
```

----------------------------------------

TITLE: Configuring Custom GPT Instructions for Google Drive Integration
DESCRIPTION: Example instructions to set up a GPT Action that can interact with Google Drive files. The code includes context setting, functional instructions, and comprehensive documentation for using the listFiles function with various query parameters.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_google_drive.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
*** Context *** 

You are an office helper who takes a look at files within Google Drive and reads in information.  In this way, when asked about something, please take a look at all of the relevant information within the drive.  Respect file names, but also take a look at each document and sheet.

*** Instructions ***

Use the 'listFiles' function to get a list of files available within docs.  In this way, determine out of this list which files make the most sense for you to pull back taking into account name and title.  After the output of listFiles is called into context, act like a normal business analyst.  Things you could be asked to be are:

- Summaries: what happens in a given file?  Please give a consistent, concise answer and read through the entire file before giving an answer.
- Professionalism: Behave professionally, providing clear and concise responses.
- Synthesis, Coding, and Data Analysis: ensure coding blocks are explained.
- When handling dates: make sure that dates are searched using date fields and also if you don't find anything, use titles.
- Clarification: Ask for clarification when needed to ensure accuracy and completeness in fulfilling user requests.  Try to make sure you know exactly what is being asked. 
- Privacy and Security: Respect user privacy and handle all data securely.

*** Examples of Documentation ***
Here is the relevant query documentation from Google for the listFiles function:
What you want to query	Example
Files with the name "hello"	name = 'hello'
Files with a name containing the words "hello" and "goodbye"	name contains 'hello' and name contains 'goodbye'
Files with a name that does not contain the word "hello"	not name contains 'hello'
Folders that are Google apps or have the folder MIME type	mimeType = 'application/vnd.google-apps.folder'
Files that are not folders	mimeType != 'application/vnd.google-apps.folder'
Files that contain the text "important" and in the trash	fullText contains 'important' and trashed = true
Files that contain the word "hello"	fullText contains 'hello'
Files that do not have the word "hello"	not fullText contains 'hello'
Files that contain the exact phrase "hello world"	fullText contains '"hello world"'
Files with a query that contains the "\" character (e.g., "\authors")	fullText contains '\\authors'
Files with ID within a collection, e.g. parents collection	'1234567' in parents
Files in an application data folder in a collection	'appDataFolder' in parents
Files for which user "test@example.org" has write permission	'test@example.org' in writers
Files for which members of the group "group@example.org" have write permission	'group@example.org' in writers
Files modified after a given date	modifiedTime > '2012-06-04T12:00:00' // default time zone is UTC
Files shared with the authorized user with "hello" in the name	sharedWithMe and name contains 'hello'
Files that have not been shared with anyone or domains (only private, or shared with specific users or groups)	visibility = 'limited'
Image or video files modified after a specific date	modifiedTime > '2012-06-04T12:00:00' and (mimeType contains 'image/' or mimeType contains 'video/')
```

----------------------------------------

TITLE: Creating Multi-Tool Agent Executor with Conversational Memory - Python
DESCRIPTION: This snippet builds an agent executor that can use multiple tools while maintaining conversational memory using LangChain's `AgentExecutor` and `ConversationBufferWindowMemory`. Dependencies are previous agent/tool objects, LangChain's memory module, and an initialized multi-tool agent. The memory buffer tracks dialogue context (window size = 2). The executor enables verbose interaction logging and orchestrates all tool interactions within conversation sessions.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_30

LANGUAGE: Python
CODE:
```
multi_tool_memory = ConversationBufferWindowMemory(k=2)
multi_tool_executor = AgentExecutor.from_agent_and_tools(agent=multi_tool_agent, tools=expanded_tools, verbose=True, memory=multi_tool_memory)
```

----------------------------------------

TITLE: Configuring Azure OpenAI Client Using API Key in Python
DESCRIPTION: This snippet configures and initializes the OpenAI Azure client for authentication using an API key. It reads the Azure OpenAI endpoint and API key from environment variables, then creates an 'openai.AzureOpenAI' client instance with required parameters like endpoint and API version. Ensure 'AZURE_OPENAI_ENDPOINT' and 'AZURE_OPENAI_API_KEY' are defined in the environment for successful execution.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/embeddings.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
if not use_azure_active_directory:
    endpoint = os.environ[\"AZURE_OPENAI_ENDPOINT\"]
    api_key = os.environ[\"AZURE_OPENAI_API_KEY\"]

    client = openai.AzureOpenAI(
        azure_endpoint=endpoint,
        api_key=api_key,
        api_version=\"2023-09-01-preview\"
    )
```

----------------------------------------

TITLE: Load Clothing Styles Dataset
DESCRIPTION: Loads the clothing styles dataset from a specified CSV file path into a pandas DataFrame, skipping any lines with parsing errors. It confirms successful loading by printing the first few rows and the total count of clothing items in the dataset.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_combine_GPT4o_with_RAG_Outfit_Assistant.ipynb#_snippet_2

LANGUAGE: Python
CODE:
```
styles_filepath = "data/sample_clothes/sample_styles.csv"
styles_df = pd.read_csv(styles_filepath, on_bad_lines='skip')
print(styles_df.head())
print("Opened dataset successfully. Dataset has {} items of clothing.".format(len(styles_df)))
```

----------------------------------------

TITLE: Querying the Zilliz Movie Database with Embeddings and Metadata Filter in Python
DESCRIPTION: Defines a query function to search for movies based on text description and a metadata filter expression. It performs embedding of the query text, runs the vector search with specified metadata constraints, and prints the results with rank, title, and other metadata. Inputs: a tuple (text, filter-expression); key dependencies: textwrap, previously created embed and collection objects. Outputs: printed ranked search results with all relevant movie fields. Customizable parameters include the number of results and filter expression syntax as per Milvus documentation.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/zilliz/Filtered_search_with_Zilliz_and_OpenAI.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
import textwrap

def query(query, top_k = 5):
    text, expr = query
    res = collection.search(embed(text), anns_field='embedding', expr = expr, param=QUERY_PARAM, limit = top_k, output_fields=['title', 'type', 'release_year', 'rating', 'description'])
    for i, hit in enumerate(res):
        print('Description:', text, 'Expression:', expr)
        print('Results:')
        for ii, hits in enumerate(hit):
            print('\t' + 'Rank:', ii + 1, 'Score:', hits.score, 'Title:', hits.entity.get('title'))
            print('\t\t' + 'Type:', hits.entity.get('type'), 'Release Year:', hits.entity.get('release_year'), 'Rating:', hits.entity.get('rating'))
            print(textwrap.fill(hits.entity.get('description'), 88))
            print()

my_query = ('movie about a fluffly animal', 'release_year < 2019 and rating like \"PG%\"')

query(my_query)
```

----------------------------------------

TITLE: Importing and Loading Configuration for Azure OpenAI - Python
DESCRIPTION: Demonstrates module imports and loading environment variables from a .env file using 'dotenv'. This is necessary for securely managing sensitive credentials (endpoint, API Key, etc.) throughout the authentication and client setup examples. No parameters required; assumes configuration is present in the environment.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
import os
import openai
import dotenv

dotenv.load_dotenv()
```

----------------------------------------

TITLE: Configuring Azure OpenAI Client with Active Directory Authentication
DESCRIPTION: Sets up the Azure OpenAI client using Azure Active Directory authentication. Uses DefaultAzureCredential and get_bearer_token_provider for token management.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/whisper.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
from azure.identity import DefaultAzureCredential, get_bearer_token_provider

if use_azure_active_directory:
    endpoint = os.environ["AZURE_OPENAI_ENDPOINT"]
    api_key = os.environ["AZURE_OPENAI_API_KEY"]

    client = openai.AzureOpenAI(
        azure_endpoint=endpoint,
        azure_ad_token_provider=get_bearer_token_provider(DefaultAzureCredential(), "https://cognitiveservices.azure.com/.default"),
        api_version="2023-09-01-preview"
    )
```

----------------------------------------

TITLE: Creating User Message Function for OpenAI NER Task (Python)
DESCRIPTION: This function creates the user prompt message by injecting the specific input text into a formatted string. It is intended for use with OpenAI's ChatCompletion API, providing the text to be processed for named entity recognition. It takes `text` (str) as a parameter and returns a ready-to-send user message.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Named_Entity_Recognition_to_enrich_text.ipynb#_snippet_5

LANGUAGE: python
CODE:
```
def user_message(text):\n    return f"""\nTASK:\n    Text: {text}\n"""
```

----------------------------------------

TITLE: Loading PDF Documents as LlamaIndex Documents - Python
DESCRIPTION: Loads PDF files for Lyft and Uber 10-Ks (2021) by converting them into lists of LlamaIndex Document objects, separated by page. Uses 'SimpleDirectoryReader' to handle the file parsing and processing. Requires the specified PDF files to exist at provided paths ('../data/10k/lyft_2021.pdf', '../data/10k/uber_2021.pdf').
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/financial_document_analysis_with_llamaindex.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
lyft_docs = SimpleDirectoryReader(input_files=["../data/10k/lyft_2021.pdf"]).load_data()
uber_docs = SimpleDirectoryReader(input_files=["../data/10k/uber_2021.pdf"]).load_data()
```

----------------------------------------

TITLE: Securely Inputting Astra DB Credentials - Python
DESCRIPTION: Prompts the user to securely enter the Astra DB token (using getpass) and database ID (using input), which are required for authentication with Astra DB or Cassandra cluster. These credentials will be used to initialize the database connection.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/cassandra_astradb/Philosophical_Quotes_cassIO.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
astra_token = getpass("Please enter your Astra token ('AstraCS:...')")
database_id = input("Please enter your database id ('3df2a5b6-...')")
```

----------------------------------------

TITLE: Constructing Agent Prompt Template for Langchain in Python
DESCRIPTION: Defines a string prompt template with specific instructions and decision logic for a Langchain agent handling conversational product searches. The template guides the agent through multiple steps, fallback logic, and example outputs. Inputs are expected for tools, entity types, and the user's prompt.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/RAG_with_graph_db.ipynb#_snippet_28

LANGUAGE: python
CODE:
```
from langchain.prompts import StringPromptTemplate
from typing import Callable


prompt_template = '''Your goal is to find a product in the database that best matches the user prompt.
You have access to these tools:

{tools}

Use the following format:

Question: the input prompt from the user
Thought: you should always think about what to do
Action: the action to take (refer to the rules below)
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Rules to follow:

1. Start by using the Query tool with the prompt as parameter. If you found results, stop here.
2. If the result is an empty array, use the similarity search tool with the full initial user prompt. If you found results, stop here.
3. If you cannot still cannot find the answer with this, probe the user to provide more context on the type of product they are looking for. 

Keep in mind that we can use entities of the following types to search for products:

{entity_types}.

3. Repeat Step 1 and 2. If you found results, stop here.

4. If you cannot find the final answer, say that you cannot help with the question.

Never return results if you did not find any results in the array returned by the query tool or the similarity search tool.

If you didn't find any result, reply: "Sorry, I didn't find any suitable products."

If you found results from the database, this is your final answer, reply to the user by announcing the number of results and returning results in this format (each new result should be on a new line):

name_of_the_product (id_of_the_product)"

Only use exact names and ids of the products returned as results when providing your final answer.


User prompt:
{input}

{agent_scratchpad}

'''
```

----------------------------------------

TITLE: Get Embeddings with Retry Logic
DESCRIPTION: Defines a Python function `get_embeddings` designed to generate vector embeddings for a list of text inputs using the specified OpenAI embedding model. The function incorporates robust retry logic with exponential backoff and a maximum attempt limit to handle potential API call failures gracefully.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_combine_GPT4o_with_RAG_Outfit_Assistant.ipynb#_snippet_3

LANGUAGE: Python
CODE:
```
@retry(wait=wait_random_exponential(min=1, max=40), stop=stop_after_attempt(10))
def get_embeddings(input: List):
    response = client.embeddings.create(
        input=input,
        model=EMBEDDING_MODEL
    ).data
    return [data.embedding for data in response]
```

----------------------------------------

TITLE: Defining a VectorDBQA Chain with Custom Prompt - Python
DESCRIPTION: Constructs a new QA chain similar to before, but injects the custom prompt template via `chain_type_kwargs`. This alters how the LLM formulates its response, adhering to the constraints in the custom template. Relies on previous definition of `llm`, `doc_store`, and `custom_prompt_template`.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/analyticdb/QA_with_Langchain_AnalyticDB_and_OpenAI.ipynb#_snippet_15

LANGUAGE: python
CODE:
```
custom_qa = VectorDBQA.from_chain_type(
    llm=llm,
    chain_type=\"stuff\",
    vectorstore=doc_store,
    return_source_documents=False,
    chain_type_kwargs={"prompt": custom_prompt_template},
)
```

----------------------------------------

TITLE: Using Web Search Tool with Responses API - Python
DESCRIPTION: Illustrates usage of the hosted web_search tool by including it in the tools parameter of the API call. The API selects and invokes the web_search tool automatically as needed. Designed for queries that benefit from real-time information retrieval. Inputs include the model, user question, and a tools list. The output contains structured results possibly referencing sources.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_example.ipynb#_snippet_7

LANGUAGE: python
CODE:
```
response = client.responses.create(\n    model="gpt-4o",  # or another supported model\n    input="What's the latest news about AI?",\n    tools=[\n        {\n            "type": "web_search"\n        }\n    ]\n)
```

----------------------------------------

TITLE: Counting Tokens in Message Payloads with tiktoken for OpenAI Chat API - Python
DESCRIPTION: Provides a utility function to estimate the number of tokens used by a list of message payloads for the OpenAI Chat API, covering multiple model types and version differences. Relies on the 'tiktoken' library to obtain token encodings. Main parameters: messages (list of dicts as formatted for OpenAI API), model (string model name), returns the integer token count. Limitations include rough estimation due to possible future changes in model encoding and message structures.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_format_inputs_to_ChatGPT_models.ipynb#_snippet_11

LANGUAGE: python
CODE:
```
import tiktoken


def num_tokens_from_messages(messages, model="gpt-3.5-turbo-0613"):
    """Return the number of tokens used by a list of messages."""
    try:
        encoding = tiktoken.encoding_for_model(model)
    except KeyError:
        print("Warning: model not found. Using cl100k_base encoding.")
        encoding = tiktoken.get_encoding("cl100k_base")
    if model in {
        "gpt-3.5-turbo-0613",
        "gpt-3.5-turbo-16k-0613",
        "gpt-4-0314",
        "gpt-4-32k-0314",
        "gpt-4-0613",
        "gpt-4-32k-0613",
        }:
        tokens_per_message = 3
        tokens_per_name = 1
    elif model == "gpt-3.5-turbo-0301":
        tokens_per_message = 4  # every message follows <|start|>{role/name}\n{content}<|end|>\n
        tokens_per_name = -1  # if there's a name, the role is omitted
    elif "gpt-3.5-turbo" in model:
        print("Warning: gpt-3.5-turbo may update over time. Returning num tokens assuming gpt-3.5-turbo-0613.")
        return num_tokens_from_messages(messages, model="gpt-3.5-turbo-0613")
    elif "gpt-4" in model:
        print("Warning: gpt-4 may update over time. Returning num tokens assuming gpt-4-0613.")
        return num_tokens_from_messages(messages, model="gpt-4-0613")
    else:
        raise NotImplementedError(
            f"""num_tokens_from_messages() is not implemented for model {model}."""
        )
    num_tokens = 0
    for message in messages:
        num_tokens += tokens_per_message
        for key, value in message.items():
            num_tokens += len(encoding.encode(value))
            if key == "name":
                num_tokens += tokens_per_name
    num_tokens += 3  # every reply is primed with <|start|>assistant<|message|>
    return num_tokens

```

----------------------------------------

TITLE: Creating Assistant Example Message Function for NER Extraction (Python)
DESCRIPTION: This function constructs a formatted assistant message for demos or few-shot examples when using OpenAI chat-based API for NER tasks. It does not take parameters and returns a string showing a sample text and a corresponding entity-labeled JSON response. This improves model performance by providing a one-shot example; it relies only on standard Python features.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Named_Entity_Recognition_to_enrich_text.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
def assisstant_message():\n    return f"""\nEXAMPLE:\n    Text: 'In Germany, in 1440, goldsmith Johannes Gutenberg invented the movable-type printing press. His work led to an information revolution and the unprecedented mass-spread / \n    of literature throughout Europe. Modelled on the design of the existing screw presses, a single Renaissance movable-type printing press could produce up to 3,600 pages per workday.'\n    {{\n        "gpe": ["Germany", "Europe"],\n        "date": ["1440"],\n        "person": ["Johannes Gutenberg"],\n        "product": ["movable-type printing press"],\n        "event": ["Renaissance"],\n        "quantity": ["3,600 pages"],\n        "time": ["workday"]\n    }}\n--"""
```

----------------------------------------

TITLE: Summarizing Push Notifications Using OpenAI Chat Completions - Python
DESCRIPTION: Defines a prompt template and a function that sends a list of push notifications to the OpenAI chat API, expecting a summarized statement as the response. It demonstrates usage with a PushNotifications instance and prints the summary. Requires proper OpenAI API credentials and the openai package.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/evaluation/use-cases/regression.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
DEVELOPER_PROMPT = """
You are a helpful assistant that summarizes push notifications.
You are given a list of push notifications and you need to collapse them into a single one.
Output only the final summary, nothing else.
"""

def summarize_push_notification(push_notifications: str) -> ChatCompletion:
    result = openai.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "developer", "content": DEVELOPER_PROMPT},
            {"role": "user", "content": push_notifications},
        ],
    )
    return result

example_push_notifications_list = PushNotifications(notifications="""
- Alert: Unauthorized login attempt detected.
- New comment on your blog post: "Great insights!"
- Tonight's dinner recipe: Pasta Primavera.
""")
result = summarize_push_notification(example_push_notifications_list.notifications)
print(result.choices[0].message.content)
```

----------------------------------------

TITLE: Run Multiple Agents in Parallel - Python
DESCRIPTION: Defines the main asynchronous function `run_agents` that executes a list of `parallel_agents` concurrently using `asyncio.gather`. It then collects their outputs, formats them, and passes the combined summaries to the `meta_agent` for final processing.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/agents_sdk/parallel_agents.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
async def run_agents(review_text: str):
    responses = await asyncio.gather(
        *(run_agent(agent, review_text) for agent in parallel_agents)
    )

    labeled_summaries = [
        f"### {resp.last_agent.name}\n{resp.final_output}"
        for resp in responses
    ]

    collected_summaries = "\n".join(labeled_summaries)
    final_summary = await run_agent(meta_agent, collected_summaries)


    print('Final summary:', final_summary.final_output)

    return
```

----------------------------------------

TITLE: Instantiating an Agent Chain with Conversation History in LangChain (Python)
DESCRIPTION: Shows how to create a CustomPromptTemplate that includes conversation history, build an LLMChain with this prompt, and instantiate an LLMSingleActionAgent as before. This agent is thus able to leverage prior turns in the conversation for context, enabling more sophisticated multi-turn dialogue. Relies on prior definition of template_with_history, and requires that agent, tools, and output_parser are already defined.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_build_a_tool-using_agent_with_Langchain.ipynb#_snippet_15

LANGUAGE: python
CODE:
```
prompt_with_history = CustomPromptTemplate(
    template=template_with_history,
    tools=tools,
    # The history template includes "history" as an input variable so we can interpolate it into the prompt
    input_variables=["input", "intermediate_steps", "history"]
)

llm_chain = LLMChain(llm=llm, prompt=prompt_with_history)
tool_names = [tool.name for tool in tools]
agent = LLMSingleActionAgent(
    llm_chain=llm_chain, 
    output_parser=output_parser,
    stop=["\nObservation:"], 
    allowed_tools=tool_names
)

```

----------------------------------------

TITLE: Configure OpenAI API Key - Python
DESCRIPTION: Sets the OpenAI API key using the OPENAI_API_KEY environment variable. It includes a check to ensure the key is present, raising a ValueError if not found.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Context_summarization_with_realtime_api.ipynb#_snippet_2

LANGUAGE: python
CODE:
```
# Set your API key safely
openai.api_key = os.getenv("OPENAI_API_KEY", "")
if not openai.api_key:
    raise ValueError("OPENAI_API_KEY not found – please set env var or edit this cell.")
```

----------------------------------------

TITLE: Defining a Custom Prompt Template for QA - Python
DESCRIPTION: Creates a custom prompt string for the QA chain that requests the LLM provide a concise single-sentence answer if possible, or suggest a random song title if uncertain. Retains required placeholders for Langchain compatibility. Wrapped in a Langchain PromptTemplate for programmatic use.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/vector_databases/analyticdb/QA_with_Langchain_AnalyticDB_and_OpenAI.ipynb#_snippet_14

LANGUAGE: python
CODE:
```
from langchain.prompts import PromptTemplate
custom_prompt = """
Use the following pieces of context to answer the question at the end. Please provide
a short single-sentence summary answer only. If you don't know the answer or if it's
not present in given context, don't try to make up an answer, but suggest me a random
unrelated song title I could listen to.
Context: {context}
Question: {question}
Helpful Answer:
"""

custom_prompt_template = PromptTemplate(
    template=custom_prompt, input_variables=["context", "question"]
)
```

----------------------------------------

TITLE: Implementing Semantic Search Function with Cosine Similarity in Python
DESCRIPTION: Defines a function to search through reviews using cosine similarity between the embedding of a search query and the embeddings of reviews. The function returns the top n most similar reviews to the query.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/Semantic_text_search_using_embeddings.ipynb#_snippet_1

LANGUAGE: python
CODE:
```
from utils.embeddings_utils import get_embedding, cosine_similarity

# search through the reviews for a specific product
def search_reviews(df, product_description, n=3, pprint=True):
    product_embedding = get_embedding(
        product_description,
        model="text-embedding-3-small"
    )
    df["similarity"] = df.embedding.apply(lambda x: cosine_similarity(x, product_embedding))

    results = (
        df.sort_values("similarity", ascending=False)
        .head(n)
        .combined.str.replace("Title: ", "")
        .str.replace("; Content:", ": ")
    )
    if pprint:
        for r in results:
            print(r[:200])
            print()
    return results


results = search_reviews(df, "delicious beans", n=3)

```

----------------------------------------

TITLE: Downloading a File from S3 Bucket with Python Assistant Bot
DESCRIPTION: This example demonstrates how to download a file from a specified S3 bucket to a local directory using natural language via run_conversation. Replace <file_name>, <bucket_name>, and <directory_path> with valid values prior to use. The assistant handles command parsing, and the result appears on standard output.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/third_party/How_to_automate_S3_storage_with_functions.ipynb#_snippet_15

LANGUAGE: python
CODE:
```
search_file = '<file_name>'\nbucket_name = '<bucket_name>'\nlocal_directory = '<directory_path>'\nprint(run_conversation(f'download {search_file} from {bucket_name} bucket to {local_directory} directory'))
```

----------------------------------------

TITLE: Defining Prompt Engineering Instructions and Schema Context for GPT SQL Action - Python
DESCRIPTION: This code snippet demonstrates best practices for writing prompt instructions and embedding SQL schema context to guide GPT in generating valid SQL queries for a PostgreSQL database. There are no external dependencies required, but it assumes the middleware exposes a `databaseQuery` API method. Key elements include a detailed natural-language description of the database schema and instructional steps for the GPT. Inputs include the user’s business question and expected output is an SQL query with possible further analysis via the code interpreter. Limitations: the schema is hardcoded, which works best for small, static schemas, and assumes PostgreSQL compatibility.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb#_snippet_0

LANGUAGE: python
CODE:
```
# Context\nYou are a data analyst. Your job is to assist users with their business questions by analyzing the data contained in a PostgreSQL database.\n\n## Database Schema\n\n### Accounts Table\n**Description:** Stores information about business accounts.\n\n| Column Name  | Data Type      | Constraints                        | Description                             |\n|--------------|----------------|------------------------------------|-----------------------------------------|\n| account_id   | INT            | PRIMARY KEY, AUTO_INCREMENT, NOT NULL | Unique identifier for each account      |\n| account_name | VARCHAR(255)   | NOT NULL                           | Name of the business account            |\n| industry     | VARCHAR(255)   |                                    | Industry to which the business belongs  |\n| created_at   | TIMESTAMP      | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Timestamp when the account was created  |\n\n### Users Table\n**Description:** Stores information about users associated with the accounts.\n\n| Column Name  | Data Type      | Constraints                        | Description                             |\n|--------------|----------------|------------------------------------|-----------------------------------------|\n| user_id      | INT            | PRIMARY KEY, AUTO_INCREMENT, NOT NULL | Unique identifier for each user         |\n| account_id   | INT            | NOT NULL, FOREIGN KEY (References Accounts(account_id)) | Foreign key referencing Accounts(account_id) |\n| username     | VARCHAR(50)    | NOT NULL, UNIQUE                   | Username chosen by the user             |\n| email        | VARCHAR(100)   | NOT NULL, UNIQUE                   | User's email address                    |\n| role         | VARCHAR(50)    |                                    | Role of the user within the account     |\n| created_at   | TIMESTAMP      | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Timestamp when the user was created     |\n\n### Revenue Table\n**Description:** Stores revenue data related to the accounts.\n\n| Column Name  | Data Type      | Constraints                        | Description                             |\n|--------------|----------------|------------------------------------|-----------------------------------------|\n| revenue_id   | INT            | PRIMARY KEY, AUTO_INCREMENT, NOT NULL | Unique identifier for each revenue record |\n| account_id   | INT            | NOT NULL, FOREIGN KEY (References Accounts(account_id)) | Foreign key referencing Accounts(account_id) |\n| amount       | DECIMAL(10, 2) | NOT NULL                           | Revenue amount                          |\n| revenue_date | DATE           | NOT NULL                           | Date when the revenue was recorded      |\n\n# Instructions:\n1. When the user asks a question, consider what data you would need to answer the question and confirm that the data should be available by consulting the database schema.\n2. Write a PostgreSQL-compatible query and submit it using the `databaseQuery` API method.\n3. Use the response data to answer the user's question.\n4. If necessary, use code interpreter to perform additional analysis on the data until you are able to answer the user's question.\n
```

----------------------------------------

TITLE: Authenticating to Azure OpenAI using API Key in Python
DESCRIPTION: Sets up an OpenAI client authenticated with an Azure API Key, referencing endpoint and API key from environment variables. The deployment variable specifies which deployed chat model version to use. This block is only executed if not using Azure Active Directory authentication, as toggled by 'use_azure_active_directory'. Dependencies: 'openai', Python >=3.7.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/azure/chat_with_your_own_data.ipynb#_snippet_3

LANGUAGE: python
CODE:
```
if not use_azure_active_directory:
    endpoint = os.environ["AZURE_OPENAI_ENDPOINT"]
    api_key = os.environ["AZURE_OPENAI_API_KEY"]
    # set the deployment name for the model we want to use
    deployment = "<deployment-id-of-the-model-to-use>"

    client = openai.AzureOpenAI(
        base_url=f"{endpoint}/openai/deployments/{deployment}/extensions",
        api_key=api_key,
        api_version="2023-09-01-preview"
    )
```

----------------------------------------

TITLE: Multimodal and Tool-Augmented Conversation with Text and Image - Python
DESCRIPTION: Demonstrates a fully multimodal API call, sending both text and an image URL as input, while specifying a hosted tool (web_search). The model is instructed to extract keywords from the image, search the web for relevant news, summarize findings, and cite sources. This single call achieves tasks that would otherwise require multiple API requests, leveraging both multimodal analysis and tool orchestration. Requires openai, base64, and IPython display modules; accepts diverse inputs (text, images), and returns a complex response object.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/responses_api/responses_example.ipynb#_snippet_9

LANGUAGE: python
CODE:
```
import base64\n\nfrom IPython.display import Image, display\n\n# Display the image from the provided URL\nurl = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/15/Cat_August_2010-4.jpg/2880px-Cat_August_2010-4.jpg"\ndisplay(Image(url=url, width=400))\n\nresponse_multimodal = client.responses.create(\n    model="gpt-4o",\n    input=[\n        {\n            "role": "user",\n            "content": [\n                {"type": "input_text", "text": \n                 "Come up with keywords related to the image, and search on the web using the search tool for any news related to the keywords"\n                 ", summarize the findings and cite the sources."},\n                {"type": "input_image", "image_url": "https://upload.wikimedia.org/wikipedia/commons/thumb/1/15/Cat_August_2010-4.jpg/2880px-Cat_August_2010-4.jpg"}\n            ]\n        }\n    ],\n    tools=[\n        {"type": "web_search"}\n    ]\n)\n
```

----------------------------------------

TITLE: Testing Guardrails with Bad Request in Python
DESCRIPTION: Executes the chat with guardrails function using a bad request that should be blocked by the topical guardrail.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/How_to_use_guardrails.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
# Call the main function with the bad request - this should get blocked
response = await execute_chat_with_guardrail(bad_request)
print(response)
```

----------------------------------------

TITLE: Document Processing Functions
DESCRIPTION: Functions for processing PDF and text files, generating embeddings, and preparing data for storage. Includes support for concurrent processing and metadata extraction.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/chatgpt/rag-quickstart/gcp/Getting_started_with_bigquery_vector_search_and_openai.ipynb#_snippet_13

LANGUAGE: python
CODE:
```
def extract_text_from_pdf(pdf_path):
    # Initialize the PDF reader
    reader = PdfReader(pdf_path)
    text = ""
    # Iterate through each page in the PDF and extract text
    for page in reader.pages:
        text += page.extract_text()
    return text

def process_file(file_path, idx, categories, embeddings_model):
    file_name = os.path.basename(file_path)
    print(f"Processing file {idx + 1}: {file_name}")
    
    # Read text content from .txt files
    if file_name.endswith('.txt'):
        with open(file_path, 'r', encoding='utf-8') as file:
            text = file.read()
    # Extract text content from .pdf files
    elif file_name.endswith('.pdf'):
        text = extract_text_from_pdf(file_path)
    
    title = file_name
    # Generate embeddings for the title
    title_vectors, title_text = len_safe_get_embedding(title, embeddings_model)
    print(f"Generated title embeddings for {file_name}")
    
    # Generate embeddings for the content
    content_vectors, content_text = len_safe_get_embedding(text, embeddings_model)
    print(f"Generated content embeddings for {file_name}")
    
    category = categorize_text(' '.join(content_text), categories)
    print(f"Categorized {file_name} as {category}")
    
    # Prepare the data to be appended
    data = []
    for i, content_vector in enumerate(content_vectors):
        data.append({
            "id": f"{idx}_{i}",
            "vector_id": f"{idx}_{i}",
            "title": title_text[0],
            "text": content_text[i],
            "title_vector": json.dumps(title_vectors[0]),  # Assuming title is short and has only one chunk
            "content_vector": json.dumps(content_vector),
            "category": category
        })
        print(f"Appended data for chunk {i + 1}/{len(content_vectors)} of {file_name}")
    
    return data
```

----------------------------------------

TITLE: Processing Articles and Generating Routines in Parallel - Python
DESCRIPTION: Defines 'process_article' to generate a routine for each article, then uses ThreadPoolExecutor to parallelize processing of the entire article list. Each result includes original policy, content, and the generated routine, supporting scalable batch conversion. Prerequisites: 'generate_routine', 'articles', and necessary imports. Input is a list of article dictionaries; output is a list of routine-structured dictionaries.
SOURCE: https://github.com/openai/openai-cookbook.git/blob/main/examples/o1/Using_reasoning_for_routine_generation.ipynb#_snippet_4

LANGUAGE: python
CODE:
```
def process_article(article):
    routine = generate_routine(article['content'])
    return {"policy": article['policy'], "content": article['content'], "routine": routine}


with ThreadPoolExecutor() as executor:
    results = list(executor.map(process_article, articles))
```