# Tools: tmux

This document outlines the patterns for interacting with Ryan's `tmux` sessions.

## The Workflow
Direct interaction with the user's session is preferred.

1.  **List Panes:** Use `tmux list-windows -t <session-name>` and `tmux list-panes -t <session-name>:<window-index>` to understand the layout.
2.  **Select a Pane:** Choose an appropriate pane for your work, preferably one that is idle or empty.
3.  **Send Commands:** Use `tmux send-keys -t <target-pane> "your command here" C-m`.
4.  **Check for Prompt:** Before sending a command, ensure the previous one has finished. If unsure, send a `C-c` (`tmux send-keys -t <target-pane> C-c`).
5.  **Capture Output:** Use `tmux capture-pane -p -t <target-pane>` to read the result of your command.

## Critical Rules
- **`timeout`:** Always wrap potentially hanging network commands in a `timeout` (e.g., `timeout 15 nc ...`).
- **`cat`:** Always pipe commands that might open a pager to `cat` (e.g., `systemctl status | cat`).
- **Quoting:** Be extremely careful with nested quotes in `send-keys`. Use single quotes for SQL or other commands with internal double quotes where possible.
