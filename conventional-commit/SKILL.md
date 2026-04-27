---
name: conventional-commit
description: "Use when drafting, reviewing, or applying git commit messages in a repository, especially when the user asks for a conventional commit message or asks the agent to commit changes."
---

# Conventional Commit

## Overview

Write concise conventional commit messages from the actual repository changes. Treat committing as a separate, permissioned action: draft the message freely, but never create or amend a commit unless the user explicitly asks and then confirms.

## Safety First

Before committing, check the repository state:

```bash
git rev-parse --is-inside-work-tree
git status --short --branch
git branch --show-current
```

Follow these rules:

- Never commit directly to `main`.
- If the current branch is `main`, create or switch to a feature branch before committing. Use an obvious branch name when the user has already provided one; otherwise ask for the branch name.
- Do not commit when HEAD is detached, including when checked out at a tag. Ask to create or switch to a branch first.
- Never amend a previous commit unless the user specifically asks for an amend.
- Never commit unless the user specifically requests a commit.
- Always ask for permission immediately before running `git commit`, even when the user requested a commit earlier in the conversation.

## Inspect the Changes

Base the message on committed intent, not guesses. Prefer staged changes when present:

```bash
git diff --staged --stat
git diff --staged
```

If nothing is staged and the user only asked for a message, inspect the working tree without staging:

```bash
git diff --stat
git diff
git status --short
```

If the changes are unrelated, propose separate commit messages instead of combining them. If the staged set is unclear or includes accidental files, point that out before drafting or committing.

## Message Format

Use this format:

```text
type(scope): description

optional body
```

Use a scope when it makes the affected module, package, command, or component clearer. Omit the scope for broad or cross-cutting changes.

Keep the description:

- imperative: `add`, `fix`, `update`, `remove`
- lowercase after the type unless a proper noun requires capitalization
- under 72 characters when practical
- without a trailing period

Use a body only when it adds useful context, such as why the change was made, important behavior changes, or migration notes. Wrap body lines at about 72 characters.

## Type Selection

Choose the narrowest accurate type:

| Type | Use for |
| --- | --- |
| `feat` | New feature or user-visible functionality |
| `fix` | Bug fix or corrected behavior |
| `refactor` | Code change that neither fixes a bug nor adds functionality |
| `test` | Adding or updating tests |
| `docs` | Documentation-only changes |
| `chore` | Maintenance tasks, metadata, housekeeping |
| `build` | Build system or external dependency changes |

If multiple types seem possible, choose based on the main user-visible intent. For example, a bug fix with tests is `fix`, while a test-only update is `test`.

## Committing

When the user asks to commit:

1. Inspect the branch and HEAD safety checks.
2. Inspect the staged changes. If nothing is staged, ask whether to stage specific files or use the current working tree.
3. Draft the conventional commit message and show it to the user.
4. Ask for confirmation to commit with that exact message.
5. Run `git commit` only after confirmation.

Use separate `-m` flags for subject and body:

```bash
git commit -m "type(scope): description" -m "body text"
```

For a subject-only message:

```bash
git commit -m "type(scope): description"
```

## Examples

```text
feat(config): implement YAML config loading and validation
feat(cli): add repo add subcommand with prefix validation
fix(sync): handle missing steering directory gracefully
test: add property tests for config validation
refactor(models): extract common types to shared module
docs: add installation instructions
build: update dependency lockfile
chore: remove obsolete generated files
```
