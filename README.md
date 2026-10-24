# qa-kit

A small Claude Code plugin for the end of a coding session: summarise what changed on your branch, then get a quick review of it.

## What's inside

| Component | Type | What it does |
|-----------|------|--------------|
| `summarize-changes` | Slash command | Lists every file touched on the current branch with a one-line description of the change. Short enough to paste into a pull-request description. |
| `code-reviewer` | Subagent | Reviews recent changes for bugs, missing error handling and unclear names. Returns findings grouped by severity (high, medium, low), one sentence per item. Finds the changes with `git status` / `git diff` and does not modify anything. Tools: `Read`, `Grep`, `Glob`, `Bash`. |

## Structure

```
.
├── .claude-plugin/
│   └── plugin.json        # manifest
├── commands/
│   └── summarize-changes.md
├── agents/
│   └── code-reviewer.md
└── README.md
```

## Usage

Load the plugin from the repo root:

```bash
claude --plugin-dir .
```

Then, inside Claude Code:

- **Summarise the branch:** run `/qa-kit:summarize-changes`.
- **Review your changes:** ask Claude to review your recent changes, e.g. *"Review the changes I just made."* Claude delegates to the `code-reviewer` subagent. You can also name it explicitly: *"Use the code-reviewer subagent to review my changes."*

After editing any plugin file, run `/reload-plugins` to pick up the changes.

## Validation

The **Validate plugin** GitHub Actions workflow (`.github/workflows/validate.yml`) runs on every push and pull request. It checks that `.claude-plugin/plugin.json` is valid JSON with a `name`, that component folders sit at the repo root, and that at least one component exists. Run it locally with:

```bash
node .github/scripts/validate-plugin.js
```
