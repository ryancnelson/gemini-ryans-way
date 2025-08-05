# Tools: CLI

This document outlines how to use key command-line tools.

### `gh` - GitHub CLI
- **Purpose:** Manage GitHub repositories.
- **Usage:** `gh repo create <org>/<repo-name> --private`
- **Note:** Do not assign teams unless explicitly told a valid team slug.

### `keybase` - Keybase CLI
- **Purpose:** Manage Keybase services, including git.
- **Usage (Git):**
    1. `keybase git create <repo-name>` (Must be run before first push)
    2. `git remote add origin keybase://private/<username>/<repo-name>`
    3. `git push origin main`

### `cognee` - Personal Knowledge Base
- **Purpose:** Add to and search Ryan's primary knowledge base.
- **Access:** Always via SSH to the `biggie` host.
- **Search Command:**
    ```bash
    ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-search 'your search query'"
    ```
- **Add Command:**
    ```bash
    echo "Content to add" | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
    ```
- **Note:** This is the primary tool for querying and storing long-term knowledge.

### `llm` - Multi-Model AI CLI
- **Purpose:** Access various AI models (OpenAI, Anthropic, Bedrock) for different tasks.
- **Usage:** `llm -m <model-slug> "Your prompt"`
- **Note:** See `patterns/multi_model_strategy.md` for guidance on which model to use.

### `cat` - The Universal Reader
- **Purpose:** Read files outside the designated workspace.
- **Usage:** `cat /path/to/any/file`
- **Note:** This is the fallback when the `read_file` tool is restricted by scope.
