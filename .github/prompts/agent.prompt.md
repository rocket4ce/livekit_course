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