# Overview

The `deepseek-eng.py` script is a command-line interface (CLI) tool that interacts with the DeepSeek API. It allows users to have a conversation with an AI model, leveraging its capabilities for code analysis, generation, and file manipulation through function calls. The script is designed to be an AI-powered software engineering assistant.

# Key Components

- **`client` (OpenAI object)**: An instance of the OpenAI client, configured to use the DeepSeek API endpoint and API key.
- **`tools` (list)**: A list of dictionaries defining the functions the AI model can call. These include:
    - `read_file`: Reads a single file.
    - `read_multiple_files`: Reads multiple files.
    - `create_file`: Creates or overwrites a single file.
    - `create_multiple_files`: Creates multiple files.
    - `edit_file`: Edits an existing file by replacing a snippet.
- **`system_PROMPT` (str)**: A detailed system prompt that instructs the AI on its role, capabilities, and how to interact with the user and tools.
- **`conversation_history` (list)**: A list that stores the messages exchanged between the user, the system, and the AI, including tool calls and their results.

**Helper Functions:**

- **`read_local_file(file_path: str) -> str`**: Reads and returns the content of a local file.
- **`create_file(path: str, content: str)`**: Creates or overwrites a file with the given content. Includes security checks and path normalization.
- **`show_diff_table(files_to_edit: List[FileToEdit]) -> None`**: (Currently not used directly in the main flow but available) Displays a table of proposed file edits.
- **`apply_diff_edit(path: str, original_snippet: str, new_snippet: str)`**: Applies a snippet replacement to a file.
- **`try_handle_add_command(user_input: str) -> bool`**: Handles the `/add` command to manually add file or directory contents to the conversation history.
- **`add_directory_to_conversation(directory_path: str)`**: Scans a directory and adds the content of non-excluded files to the conversation history.
- **`is_binary_file(file_path: str, peek_size: int = 1024) -> bool`**: Checks if a file is likely a binary file.
- **`ensure_file_in_context(file_path: str) -> bool`**: Ensures that the content of a specified file is present in the conversation history before an edit operation.
- **`normalize_path(path_str: str) -> str`**: Normalizes and resolves a file path, including security checks against directory traversal.
- **`execute_function_call_dict(tool_call_dict)` / `execute_function_call(tool_call)`**: Executes the function specified by the AI model's tool call and returns the result. These functions map the AI's requested function name to the actual Python helper functions.
- **`trim_conversation_history()`**: Trims the conversation history to prevent exceeding token limits, while trying to preserve recent context and system prompts.
- **`stream_openai_response(user_message: str)`**: Manages the interaction with the DeepSeek API. It sends the user's message and conversation history, streams the AI's response (including reasoning and content), handles tool calls, executes them, sends the results back to the AI, and streams the follow-up response.
- **`main()`**: The main function that initializes the application, displays a welcome message and instructions, and runs the interactive loop for user input and AI responses.

**Pydantic Models:**

- **`FileToCreate(BaseModel)`**: Defines the structure for creating a file (path, content).
- **`FileToEdit(BaseModel)`**: Defines the structure for editing a file (path, original_snippet, new_snippet).

# Important Variables/Constants

- **`console` (Console object)**: An instance of `rich.console.Console` for rich text output in the terminal.
- **`prompt_session` (PromptSession object)**: An instance of `prompt_toolkit.PromptSession` for user input with history and styling.
- **`DEEPSEEK_API_KEY` (environment variable)**: The API key for accessing the DeepSeek API. Loaded from a `.env` file.
- **`excluded_files` (set)**: A set of filenames and directory names to exclude when using the `/add <folder>` command.
- **`excluded_extensions` (set)**: A set of file extensions to exclude when using the `/add <folder>` command.
- **`max_files` (int)**: Maximum number of files to process when adding a directory.
- **`max_file_size` (int)**: Maximum size for a single file to be processed.

# Usage Examples

The script is run from the command line:
```bash
python deepseek-eng.py
```

Once running, the user can interact with the AI:

- **Ask questions or give instructions:**
  ```
  🔵 You> Can you write a Python function to sort a list of numbers?
  ```
- **Add files to the conversation context:**
  ```
  🔵 You> /add my_script.py
  ```
  ```
  🔵 You> /add src/
  ```
- **Request file operations (the AI will use function calls):**
  ```
  🔵 You> Read the file 'utils.py' and tell me what the 'helper_function' does.
  ```
  ```
  🔵 You> Create a new file named 'output.txt' with the content "Hello, World!".
  ```
  ```
  🔵 You> In 'main.py', replace "old_value" with "new_value".
  ```

# Dependencies and Interactions

**External Libraries:**

- **`openai`**: The official OpenAI Python client, used to interact with the DeepSeek API (which is OpenAI compatible).
- **`pydantic`**: For data validation and settings management using Python type annotations.
- **`python-dotenv`**: For loading environment variables from a `.env` file.
- **`rich`**: For creating rich text and beautifully formatted output in the terminal.
- **`prompt_toolkit`**: For creating interactive command-line interfaces.

**Internal Interactions:**

- The script primarily interacts with the DeepSeek API.
- It performs local file system operations (read, write, create) based on the AI's function calls.
- The `conversation_history` is central to maintaining context for the AI.
- Helper functions are called by `execute_function_call` based on the AI's requests.

**Environment:**

- Requires a `.env` file in the same directory (or environment variables set) with `DEEPSEEK_API_KEY` defined.
- Python 3.x environment with the listed dependencies installed.
