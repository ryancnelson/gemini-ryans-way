# Pattern: Multi-Model AI Strategy

This pattern outlines the strategy for using the right AI model for the right task, leveraging the `llm` and `gemini` CLI tools.

## Guiding Principle
Different AI models have different strengths. Do not rely on a single model. Select the best tool for the job to achieve the best results in reasoning, implementation, and speed.

## Model Selection Guide

- **`o1` (via `llm`):**
    - **Strengths:** Complex reasoning, deep technical analysis, hardware debugging, system architecture problems.
    - **When to Use:** When you need the most powerful reasoning engine to analyze a complex problem from first principles.

- **`gpt-4o` (via `llm`):
    - **Strengths:** Code implementation, detailed explanations, general-purpose tasks, creative text generation.
    - **When to Use:** When you have a solution in mind and need the exact code or a clear, step-by-step plan.

- **`gemini-2.5-pro` / `gemini-1.5-pro` (via `gemini` or `llm`):
    - **Strengths:** Analyzing massive contexts (up to 1M tokens), multimodal tasks (text, images, etc.), providing a different perspective from OpenAI models for cross-validation.
    - **When to Use:** When you need to analyze a very large amount of data (e.g., an entire codebase) or want a "second opinion" on a complex topic.

- **`gpt-4o-mini` / `gemini-2.0-flash` (via `llm` or `gemini`):
    - **Strengths:** Very fast, cost-effective. Good for simple, quick tasks.
    - **When to Use:** For quick syntax help, simple command generation, or summarizing text.

## The Debugging Workflow

This is the recommended workflow for tackling a difficult technical problem:

1.  **Document the Problem in Cognee:**
    - Store the initial state, error messages, and symptoms in your personal knowledge base.
    ```bash
    echo "Debugging: [Problem Description]" | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
    ```

2.  **Get Expert Analysis (Reasoning):**
    - Send the detailed problem description to the strongest reasoning model.
    ```bash
    llm -m o1 "Analyze this complex technical problem: [details]"
    ```

3.  **Get Practical Implementation (Code):**
    - Take the analysis from the reasoning model and ask an implementation model for the code.
    ```bash
    llm -m gpt-4o "Based on this analysis, show me the exact code to fix it."
    ```

4.  **Cross-Validate (Second Opinion):**
    - If the problem is particularly tricky, get a second opinion from a different model family.
    ```bash
    gemini "Provide an alternative perspective on this solution."
    ```

5.  **Document the Solution in Cognee:**
    - Once solved, store the complete solution for future reference.
    ```bash
    echo "SOLVED: [Problem Title]\nRoot Cause:...
Solution:..." | ssh -l ryan biggie "/home/ryan/devel/cognee-quickstart/cognee-add"
    ```

