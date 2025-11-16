# PandasAI v3 Architecture and Core Components

This document provides a detailed breakdown of the PandasAI v3 architecture, organized by package. Each section outlines the purpose of the package and the key classes and functions within it.

## 1. `pandasai/agent`

This is the central package of the library. It contains the `Agent` class, which is the main entry point for interacting with PandasAI. The agent is responsible for managing the conversation, coordinating the other components, and returning the final result to the user.

### `Agent` Class

The `Agent` class orchestrates the entire process of turning a natural language query into a result.

*   **`__init__(self, dfs, config=None, memory_size=10, vectorstore=None, description=None, sandbox=None)`**: Initializes the agent with one or more DataFrames, an optional configuration, memory size, vector store, and sandbox.
*   **`chat(self, query, output_type=None)`**: Starts a new chat interaction. This is the primary method for asking questions.
*   **`follow_up(self, query, output_type=None)`**: Continues an existing conversation with a follow-up question.
*   **`train(self, queries=None, codes=None, docs=None)`**: Trains the agent on a set of questions and answers or documents to improve its accuracy.
*   **`clear_memory(self)`**: Clears the conversation history.

## 2. `pandasai/core`

This package contains the core logic for code generation, execution, and response parsing. It's the "engine" of PandasAI.

### `core/code_generation`

*   **`CodeGenerator` Class**: Responsible for generating Python code from a natural language query. It uses a `BasePrompt` to construct the prompt that is sent to the LLM, and then it cleans and validates the generated code.

### `core/code_execution`

*   **`CodeExecutor` Class**: Executes the generated Python code in a secure environment. It uses an environment that can be customized to include additional libraries or variables.

### `core/prompts`

*   **`BasePrompt` Class**: The base class for all prompts. It uses Jinja2 templates to construct the prompts that are sent to the LLM.

### `core/response`

*   **`ResponseParser` Class**: Parses the dictionary returned by the executed code and wraps it in the appropriate response object (`NumberResponse`, `StringResponse`, `DataFrameResponse`, or `ChartResponse`).

## 3. `pandasai/dataframe`

This package provides the `DataFrame` and `VirtualDataFrame` classes, which extend the standard pandas DataFrame with natural language capabilities.

### `DataFrame` Class

A wrapper for pandas DataFrames that adds the `chat()` method.

*   **`__init__(self, data, name=None, description=None, custom_head=None)`**: Initializes the DataFrame wrapper.
*   **`chat(self, prompt, sandbox=None)`**: Starts a chat interaction with the DataFrame.

### `VirtualDataFrame` Class

A subclass of `DataFrame` for working with remote or large datasets. It uses a data loader to fetch only the necessary data.

## 4. `pandasai/llm`

This package contains the base `LLM` class, which defines the interface for all language models.

### `LLM` Class

The base class for all language models.

*   **`__init__(self, api_key=None, **kwargs)`**: Initializes the LLM with an API key and other optional parameters.
*   **`call(self, instruction, context=None)`**: Makes a call to the LLM.
*   **`generate_code(self, instruction, context)`**: Generates code using the LLM.

## 5. `pandasai/data_loader`

This package is responsible for loading data from various sources.

### `DatasetLoader` Class

An abstract base class for loading datasets. It includes factory methods for creating the appropriate loader based on the dataset type.

### `SQLLoader` Class

A loader for SQL datasets. It uses a `QueryBuilder` to construct the SQL queries and then executes them using the appropriate connector.

### `LocalLoader` Class

A loader for local datasets (CSV, Parquet). It uses DuckDB to execute SQL queries against these files.

## 6. `pandasai/vectorstores`

This package defines the `VectorStore` abstract base class, which provides an interface for using vector stores for training and context.

### `VectorStore` Class

The base class for all vector stores. Implementations for specific vector stores like ChromaDB and Pinecone are available in separate packages.

## 7. `pandasai/helpers`

This package contains various helper classes and functions that are used throughout the library.

### `Memory` Class

For storing and retrieving conversation history. It includes methods for adding messages, truncating long messages, and formatting the conversation for different LLM providers.

### `Logger` Class

For logging messages to the console and/or a file. It includes functionality for tracking the time between log messages and identifying the source of the log message.
