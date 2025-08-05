# Pattern: New Project Setup

This pattern outlines the standard procedure for setting up a new project directory.

1.  **Initialize Git:**
    - `git init`
2.  **Create Documentation:**
    - Create a `README.md` with the project's purpose.
    - Create a `technical-summary.md` to document the final state.
    - Create a `latest-status.md` for in-progress work.
3.  **Create Remotes:**
    - **Keybase:**
        1. `keybase git create <repo-name>`
        2. `git remote add origin keybase://private/<username>/<repo-name>`
        3. `git push -u origin main`
    - **GitHub:**
        1. `gh repo create <org>/<repo-name> --private`
        2. `git remote add github git@github.com:<org>/<repo-name>.git`
        3. `git push github main`
4.  **Cleanup:**
    - Before finalizing, review all files in the directory.
    - Delete temporary files, credentials, and AI artifacts.
    - Commit the cleanup.
5.  **Push:**
    - Push all changes to both `origin` (Keybase) and `github` remotes.
