# Conventional Commit Skill

This repository packages an Agent Skill that teaches compatible coding agents how
to draft and apply conventional commit messages from the actual changes in a git
repository.

The skill emphasizes two things:

- conventional commit subjects such as `feat(cli): add repo command`
- safe commit behavior, including explicit permission before committing and a
  guard against committing directly to `main`

## Contents

```text
conventional-commit/
├── SKILL.md
└── agents/
    └── openai.yaml
```

`SKILL.md` is the portable skill definition used by Codex CLI, Claude Code, and
Kiro. `agents/openai.yaml` is optional Codex metadata for display name, short
description, and default prompt; other agents can ignore it.

The repository also includes `.claude-plugin/marketplace.json` so Claude Code can
install the skill through its native plugin marketplace commands.

## Install For Codex CLI

Start Codex and use its built-in skill installer:

```text
$skill-installer install https://github.com/mrburrito/conventional-commit-skill/tree/main/conventional-commit
```

Restart Codex so it discovers the new skill.

## Install For Claude Code

In Claude Code, add this repository as a plugin marketplace:

```text
/plugin marketplace add mrburrito/conventional-commit-skill
```

Then install the plugin:

```text
/plugin install conventional-commit@conventional-commit-skill
```

Run `/reload-plugins` after installing or updating the plugin. Claude Code
installs plugins to user scope by default; use the interactive `/plugin`
interface if you want project or local scope instead.

## Install For Kiro

In Kiro IDE:

1. Open the Agent Steering & Skills section in the Kiro panel.
2. Click `+` and select `Import a skill`.
3. Choose `GitHub`.
4. Paste this skill URL:

```text
https://github.com/mrburrito/conventional-commit-skill/tree/main/conventional-commit
```

The URL must point to the `conventional-commit` subdirectory or directly to
`conventional-commit/SKILL.md`, not the repository root. Kiro copies imported
skills into the selected skills directory and makes them available immediately.

## Usage

Ask the agent to draft or review a commit message, for example:

```text
Use the conventional commit skill to draft a commit message for these changes.
```

When you ask the agent to commit, the skill instructs it to inspect the git
state, draft the exact message, and ask for confirmation before running
`git commit`.
