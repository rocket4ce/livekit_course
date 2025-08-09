---
mode: agent
---
## We use Tidewave
Tidewave is a powerful tool that enhances the development experience by providing a suite of features tailored for Elixir projects. It integrates seamlessly with your LLM (Large Language Model) to assist in various tasks, including:
  * get documentation
  * inspect your application logs to help debug errors
  * execute SQL queries and inspect your database
  * evaluate custom Elixir code in the context of your project
  * find Hex packages and search your dependencies
  ### Available tools
  * project_eval
  * get_docs
  * get_source_location
  * get_package_location
  * get_logs
  * get_models / get_schemas
  * execute_sql_query
  * search_package_docs
## We use Context7
Context7 MCP provides the following tools that LLMs can use this tool we use when tidewave is not available or when we need to access external libraries:
  * resolve-library-id: Resolves a general library name into a Context7-compatible library ID.
  * libraryName (required): The name of the library to search for
  * get-library-docs: Fetches documentation for a library using a Context7-compatible library ID.
  * context7CompatibleLibraryID (required): Exact Context7-compatible library ID (e.g., /mongodb/docs, /vercel/next.js)
  * topic (optional): Focus the docs on a specific topic (e.g., "routing", "hooks")
  * tokens (optional, default 10000): Max number of tokens to return. Values less than the default value of 10000 are automatically increased to 10000.

## Workflow
Before you start working on a new feature or bug fix, make sure you follow these steps:
- Think about the feature or bug fix you need to implement.
- TDD
**Test-Driven Development (TDD)**: Write tests before implementing the feature. This will help you clearly define expected behavior and avoid regressions in the future.
**Run tests before you start**: Make sure all existing tests pass before you begin working on a new feature or bug fix.
- If there are errors:
**Inspect the logs**: If you find an error, use `get_logs` to review the application logs and get more context.
**Evaluate existing code**: Use `project_eval` to run code snippets in the context of your project and better understand how it works.
**Query the database**: If you need specific data, `execute_sql_query` will allow you to perform SQL queries directly against the database.
- If you need any dependencies
**Search for Elixir dependencies**: If you need an external library, `search_package_docs` will help you find documentation and usage examples.
**Search for Elixir external dependencies**: If you need an external library, `get-library-docs` will help you find documentation and usage examples.
- Definition of done
**Definition of "Done"**: Before completing any feature, run the following commands: `mix format`, `mix credo`, and `mix test`. If all pass, you can consider the feature complete.