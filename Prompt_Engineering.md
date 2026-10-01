# Prompt Engineering Notes
---

## Table of Contents

1. [What is Prompt Engineering](#what-is-prompt-engineering)
2. [Core Principles of a Good Prompt](#core-principles-of-a-good-prompt)
3. [Prompt Structure Template](#prompt-structure-template)
4. [Techniques for Better Prompts](#techniques-for-better-prompts)
5. [Getting Perfect Code / Output from AI](#getting-perfect-code--output-from-ai)
6. [Debugging Code with AI](#debugging-code-with-ai)
7. [Useful Prompt Library](#useful-prompt-library)
8. [Common Mistakes to Avoid](#common-mistakes-to-avoid)
9. [Quick Checklist Before Hitting Enter](#quick-checklist-before-hitting-enter)

---

## What is Prompt Engineering

Prompt engineering is the practice of designing inputs (prompts) to guide an AI model toward producing the most accurate, relevant, and useful output. A well-engineered prompt reduces ambiguity, controls format, and minimizes the need for follow-up corrections.

Think of it like writing a very precise requirement document for a junior developer who has no context about your project, your codebase, or your intentions — except the AI has read most of the internet, so it's a very *fast* junior developer with no memory between sessions.

---

## Core Principles of a Good Prompt

| Principle | Description |
|---|---|
| **Clarity** | State exactly what you want. Avoid vague words like "better," "nice," "improve" without defining what that means. |
| **Context** | Give background — what the code/project is for, tech stack, constraints, audience. |
| **Specificity** | Mention exact inputs, expected outputs, edge cases, formats. |
| **Constraints** | Set boundaries: language/version, libraries allowed, performance limits, style guide. |
| **Examples** | Show a sample input/output pair when possible — models mimic patterns extremely well. |
| **Iteration** | Treat the first response as a draft. Refine with follow-up prompts rather than starting over. |
| **Role Assignment** | Tell the AI what "role" to take (e.g., "act as a senior backend engineer reviewing this PR"). |

---

## Prompt Structure Template

A reusable skeleton for almost any technical prompt:

```
ROLE:        Who should the AI act as?
TASK:        What exactly do you want done?
CONTEXT:     Relevant background (tech stack, project, constraints)
INPUT:       The actual code / data / question
FORMAT:      How should the output look? (code block, table, bullet list, etc.)
CONSTRAINTS: What NOT to do, limits, versions, style rules
```

**Example filled in:**

```
ROLE: Act as a senior Python developer and code reviewer.
TASK: Review this function for bugs and suggest optimizations.
CONTEXT: This is part of a Flask REST API that processes user signups.
         Python 3.11, no external libraries beyond what's already imported.
INPUT: [paste code here]
FORMAT: Point out issues as a numbered list, then give the corrected code
        in one block at the end.
CONSTRAINTS: Do not change the function signature. Keep it PEP8 compliant.
```

---

## Techniques for Better Prompts

### 1. Zero-shot vs Few-shot
- **Zero-shot**: Just ask directly. Works for simple, well-known tasks.
- **Few-shot**: Provide 1–3 examples of input → output before your real request. Dramatically improves consistency for formatting-heavy or unusual tasks.

### 2. Chain-of-Thought Prompting
Ask the model to reason step-by-step before giving a final answer:
> "Think through this step-by-step before giving the final answer."

Useful for debugging, math, logic problems, and algorithm design — reduces silly mistakes.

### 3. Role Prompting
Assigning a persona narrows the model's response style:
> "Act as a strict code reviewer who only approves clean, production-ready code."

### 4. Output Format Locking
Explicitly specify the format so you don't get a wall of prose when you wanted a table, or vice versa:
> "Respond only in a markdown table with columns: Issue | Severity | Fix"

### 5. Iterative Refinement
Don't try to get the perfect prompt in one shot. Ask → evaluate → refine:
1. Ask for a first draft.
2. Point out exactly what's wrong or missing.
3. Ask for a revision addressing only that.

### 6. Negative Constraints
Tell the AI what to avoid — this is often more effective than only saying what you want:
> "Don't use any external libraries. Don't change variable names. Don't add comments."

### 7. Temperature/Creativity Framing (conceptual)
When you want precise, deterministic output (like code), ask for "the most standard, conventional solution" rather than "a creative solution."

---

## Getting Perfect Code / Output from AI

1. **State the exact language + version** (e.g., "Python 3.11", "Node.js 20", "React 18 with hooks").
2. **Mention the environment** (browser, server, embedded device, Jupyter notebook, etc.) — code that works in one breaks in another.
3. **Provide sample input/output** if the function has a specific contract.
4. **Ask for edge case handling explicitly**: empty input, null values, large datasets, wrong types.
5. **Specify style/conventions**: PEP8, Airbnb JS style guide, your team's naming conventions.
6. **Ask for explanation alongside code** if you're learning — request comments or a short explanation of *why*, not just *what*.
7. **Request tests**: "Also write 3 unit tests covering normal, edge, and failure cases."
8. **Ask it to flag assumptions**: "If anything is ambiguous, state your assumptions before the code."
9. **Break large tasks into smaller prompts** — asking for an entire app in one prompt gives shallow, buggy results. Build incrementally: structure → core logic → error handling → polish.
10. **Always verify, never blindly copy-paste** — especially for security-sensitive code (auth, payments, SQL queries).

---

## Debugging Code with AI

### Step-by-step debugging prompt structure

```
1. Paste the exact error message/traceback (full, not summarized).
2. Paste the relevant code (not just one line — include the function/class).
3. Explain what you expected to happen vs what actually happened.
4. Mention what you've already tried.
5. Ask the AI to explain the root cause before fixing it.


Example:
> "I'm getting `IndexError: list index out of range` on line 14 of this function [paste code]. I expected it to return the last 3 items of the list, but it crashes when the list has fewer than 3 items. I already tried adding a length check but it still fails. Explain why this is happening, then fix it."

### Debugging tips

- **Don't just say "it's not working"** — always include the exact error text. AI can't guess an error it can't see.
- **Ask for root-cause analysis separately from the fix** — this helps you actually learn instead of just patching blindly.
- **Use "explain like I'm reviewing this for the first time"** when you inherited someone else's code.
- **For silent bugs (no error, wrong output)**: give both the actual output and expected output side by side.
- **For performance bugs**: mention data size and current runtime, ask for Big-O analysis of the current approach before optimizing.
- **Ask for a minimal reproducible example** if the bug is inside a large codebase — isolate before you prompt.
- **When a fix doesn't work**, don't just say "still broken" — say what changed and what didn't, and paste the new error.

---

## Useful Prompt Library

### Code Review
> "Act as a senior [language] developer. Review this code for bugs, security issues, and readability. List issues by severity (Critical/Major/Minor), then give the corrected version."

### Learning / Explaining Concepts
> "Explain [concept] to me like I already know basic [language/topic] but I'm new to this specific idea. Use a simple real-world analogy, then a code example."

### Refactoring
> "Refactor this function to be more readable and efficient without changing its behavior. Explain each change you made and why."

### Writing Documentation
> "Write clear docstrings/comments for this code following [Google style / NumPy style / JSDoc]. Don't change the logic."

### Generating Test Cases
> "Write unit tests for this function using [pytest/unittest/Jest]. Cover normal cases, edge cases, and invalid input."

### Optimizing Performance
> "This function currently runs in O(n²). Suggest ways to optimize it, explain the tradeoffs of each approach, then implement the best one."

### Understanding Someone Else's Code
> "Explain what this code does, section by section, as if I've never seen it before. Point out anything unusual or risky."

### Converting Between Languages/Frameworks
> "Convert this [Python] function to [JavaScript], keeping the same logic and handling equivalent edge cases for the new language."

### Architecture / System Design
> "Act as a system design mentor. I'm building [project description]. Suggest a high-level architecture, list the components, and explain the tradeoffs of at least two possible approaches."

### Interview Prep
> "Ask me a [data structures/algorithms] interview question of medium difficulty. Wait for my answer before giving hints or the solution."

---

## Common Mistakes to Avoid

- ❌ Vague requests: *"make this code better"* → ✅ *"reduce time complexity and add input validation"*
- ❌ Dumping code with zero context
- ❌ Asking for everything in one giant prompt
- ❌ Not specifying the output format, then complaining about the format
- ❌ Ignoring the model's clarifying questions
- ❌ Blindly trusting output without testing it
- ❌ Not giving the actual error message when debugging
- ❌ Re-asking the same failed prompt without changing anything

---

## Quick Checklist Before Hitting Enter

- [ ] Have I stated the goal clearly?
- [ ] Have I given enough context (language, framework, constraints)?
- [ ] Have I specified the desired output format?
- [ ] Have I included the actual error/code/data, not a paraphrase?
- [ ] Have I told it what NOT to do (if relevant)?
- [ ] Is this prompt asking for one clear thing, not five unrelated things?

---