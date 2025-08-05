# Ryan's Way - Gemini Edition

Welcome, AI Assistant. This is your primer for collaborating with Ryan on his workstation, 'minnie'. Your goal is to be a precise, efficient, and safe CLI partner.

This framework is designed to be your "short-term memory" and operational guide. Refer to it to understand how to work effectively.

## 1. How This Framework is Organized

This repository contains two distinct types of information:

- **The Primer (Root Directory):** The files in the root (`/`), `environment/`, `tools/`, and `patterns/` directories are the quick-start guide. They are distilled, actionable, and designed to be read at the start of every session to give you the immediate "how-to" for operating effectively.

- **The Reference Library (`/reference`):** The files in the `reference/` directory contain the deep, comprehensive, and historical documentation. This is the "why" behind the primer. If you need more context, historical design decisions, or legacy patterns, you will find them here.

## 2. The Core Workflow

Your operational loop should be:
1.  **Orient:** Start here. Read this `README.md` file and the other primer documents.
2.  **Consult Patterns & Tools:** Before executing a complex task, consult the relevant document in the `/patterns` or `/tools` directories.
3.  **Deep Dive (If Necessary):** If the primer lacks sufficient detail, consult the documents in the `/reference` directory.
4.  **Execute:** Perform the task using the knowledge you've gathered.
5.  **Remember:** If you learn a new, persistent fact, add it to the appropriate file in the primer.

## 3. How to Expand Your Capabilities

*   **If you need to access Ryan's knowledge base:** See `tools/cli_tools.md` for how to use the `cognee` commands.
*   **If you need to choose the right AI model for a task:** See `patterns/multi_model_strategy.md`.

## 4. Critical Lessons Learned (The "Don't Forget" List)

*   **You CAN read any file:** Use `run_shell_command` with `cat` to read files outside the workspace.
*   **`tmux` is your friend:** Use it to interact with the user's session directly. See `tools/tmux.md`.
*   **Check your prompt:** Always ensure a command has finished before sending another. Use `Ctrl-C` to be safe.
*   **`timeout` is mandatory for tests:** Wrap potentially hanging network commands in a `timeout`.
*   **Avoid pagers:** Pipe commands that might open a pager to `cat`.
*   **AWS Networking is subtle:** Refer to the `patterns/debugging.md` file for a detailed breakdown of the VPC routing, Security Group, and Source/Destination Check issues we've solved before.
