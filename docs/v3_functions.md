# PandasAI v3 Function Structure

This document outlines the structure of the core functions available in PandasAI v3. The primary interface for interacting with PandasAI is the `Agent` class.

## `Agent` Class

The `Agent` class is the main entry point for using PandasAI v3. It orchestrates the conversation, code generation, and execution.

### `__init__(self, dfs, config=None, memory_size=10, vectorstore=None, description=None, sandbox=None)`

Initializes the Agent.

*   **`dfs`**: A single DataFrame or a list of DataFrames to work with.
*   **`config`**: Configuration options for the agent, including LLM settings. (Deprecated)
*   **`memory_size`**: The number of previous conversations to remember.
*   **`vectorstore`**: A vector store for training and context.
*   **`description`**: A description of the agent's purpose.
*   **`sandbox`**: A sandbox environment for code execution.

### `chat(self, query, output_type=None)`

Starts a new chat interaction.

*   **`query`**: The natural language query from the user.
*   **`output_type`**: A hint for the LLM about the desired output type (e.g., "plot", "number", "string", "dataframe").

### `follow_up(self, query, output_type=None)`

Continues an existing chat interaction.

*   **`query`**: The follow-up natural language query from the user.
*   **`output_type`**: A hint for the LLM about the desired output type.

### `generate_code(self, query)`

Generates code based on a query.

*   **`query`**: The user's query.

### `execute_code(self, code)`

Executes a given code snippet.

*   **`code`**: The code to execute.

### `generate_code_with_retries(self, query)`

Generates code with retry logic in case of errors.

*   **`query`**: The user's query.

### `execute_with_retries(self, code)`

Executes code with retry logic.

*   **`code`**: The code to execute.

### `train(self, queries=None, codes=None, docs=None)`

Trains the agent on a set of questions and answers or documents.

*   **`queries`**: A list of training queries.
*   **`codes`**: A list of corresponding code answers.
*   **`docs`**: A list of documents to add to the vector store.

### `clear_memory(self)`

Clears the conversation history.

### `add_message(self, message, is_user=False)`

Adds a message to the conversation memory.

*   **`message`**: The message to add.
*   **`is_user`**: Whether the message is from the user.

### `start_new_conversation(self)`

Clears the memory to start a new conversation.

### Properties

*   **`last_generated_code`**: Returns the last code snippet generated.
*   **`last_code_executed`**: Returns the last code snippet executed.
*   **`last_prompt_used`**: Returns the last prompt used to generate code.

## `DataFrame` Class

A wrapper for pandas DataFrames that adds new capabilities.

### `__init__(self, data, name=None, description=None, custom_head=None)`

*   **`data`**: The pandas DataFrame to wrap.
*   **`name`**: The name of the DataFrame.
*   **`description`**: A description of the DataFrame.
*   **`custom_head`**: A custom head to display for the DataFrame.

### `chat(self, prompt, sandbox=None)`

Starts a chat interaction with the DataFrame.

*   **`prompt`**: The user's query.
*   **`sandbox`**: A sandbox environment for code execution.

### `follow_up(self, query, output_type=None)`

Continues an existing chat interaction with the DataFrame.

*   **`query`**: The user's query.
*   **`output_type`**: A hint for the LLM about the desired output type.

## `VirtualDataFrame` Class

A subclass of `DataFrame` for working with remote or large datasets.

## `LLM` Class

The base class for all language models.

### `__init__(self, api_key=None, **kwargs)`

*   **`api_key`**: The API key for the LLM.
*   **`kwargs`**: Additional keyword arguments for the LLM.

### `call(self, instruction, context=None)`

Makes a call to the LLM.

*   **`instruction`**: The prompt to send to the LLM.
*   **`context`**: The agent's state.

### `generate_code(self, instruction, context)`

Generates code using the LLM.

*   **`instruction`**: The prompt to send to the LLM.
*   **`context`**: The agent's state.

## `Sandbox` Class

For executing code in a secure environment.

### `__init__(self, **kwargs)`

*   **`kwargs`**: Additional keyword arguments for the sandbox.

### `execute(self, code, environment)`

Executes code in the sandbox.

*   **`code`**: The code to execute.
*   **`environment`**: The environment to execute the code in.

## Data Loaders

### `DatasetLoader`

An abstract base class for loading datasets.

### `SQLLoader`

A loader for SQL datasets.

### `LocalLoader`

A loader for local datasets.

## Helpers

### `Memory`

For storing conversation history.

### `Logger`

For logging messages.

## Vector Stores

Vector store integrations like ChromaDB, Pinecone, etc., are not included in the core library. They need to be installed as separate packages. The base class for all vector stores is `VectorStore`.

## Deprecated Classes

The `SmartDataframe` and `SmartDatalake` classes are deprecated and will be removed in a future version. Please use the `Agent` class instead.
