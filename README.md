# Ryan's Way - Gemini Edition

Welcome, AI Assistant. This is your primer for collaborating with Ryan on his workstation, 'minnie'. Your goal is to be a precise, efficient, and safe CLI partner.

This framework is designed to be your "short-term memory" and operational guide. Refer to it to understand how to work effectively.

## 1. The Core Workflow

Your operational loop should be:
1.  **Orient:** Start here. Read this `README.md` file.
2.  **Understand the Environment:** Read the files in the `/environment` directory to understand the workstation, network, and cloud setup.
3.  **Consult Patterns & Tools:** Before executing a complex task, consult the relevant document in the `/patterns` or `/tools` directories.
4.  **Execute:** Perform the task using the knowledge you've gathered.
5.  **Remember:** If you learn a new, persistent fact about Ryan's preferences or environment, add it to the appropriate file in this framework, especially in `/user/preferences.md`.

## 2. How to Expand Your Capabilities

This framework is your launchpad, not your limit.

*   **If you need to access Ryan's knowledge base:** See `tools/cli_tools.md` for how to use the `cognee` commands.
*   **If you need to choose the right AI model for a task:** See `patterns/multi_model_strategy.md`.
*   **If you encounter a new tool or concept:** Ask Ryan to document it and add it to this framework for future sessions.

## 3. Critical Lessons Learned (The "Don't Forget" List)

Our previous sessions have taught us several critical lessons. These are not just suggestions; they are rules for avoiding errors and wasted time.

*   **You CAN read any file:** If `read_file` is restricted, use `run_shell_command` with `cat` to read files outside the workspace (e.g., `cat /path/to/file`).
*   **`tmux` is your friend:** Use it to interact with the user's session directly. See `tools/tmux.md`.
*   **Check your prompt:** Always ensure a command has finished before sending another. Use `Ctrl-C` to be safe.
*   **`timeout` is mandatory for tests:** Wrap potentially hanging network commands (like `mysql`, `nc`, `curl`) in a timeout (e.g., `timeout 15 ...`).
*   **Avoid pagers:** Pipe commands that might trigger a pager (like `less` or `apt`) to `cat` (e.g., `systemctl status | cat`).
*   **AWS Networking is subtle:** Refer to the `patterns/debugging.md` file for a detailed breakdown of the VPC routing, Security Group, and Source/Destination Check issues we've solved before.

By following this framework, you will be a more effective and helpful assistant.
