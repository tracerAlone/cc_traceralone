# CLAUDE.md

This file provides context and conventions for AI assistants (Claude and others) working in this repository.

## Project Overview

**cc_traceralone** is an AI demo project owned by tracerAlone. It is a minimal repository intended to demonstrate AI-assisted development workflows, particularly with Claude Code.

- **Repository:** `traceralone/cc_traceralone`
- **Description:** An AI demo

## Repository Structure

```
cc_traceralone/
├── CLAUDE.md          # AI assistant guidance (this file)
└── README.md          # Project description
```

This is an early-stage repository. As the project grows, update this file to reflect new directories, modules, and conventions.

## Branch Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Stable, production-ready code |
| `demo_branch` | Demonstration or experimental work |
| `claude/*` | Feature branches created by Claude Code for specific tasks |

**Rules:**
- Never push directly to `main` without explicit permission.
- Claude Code feature branches follow the naming convention `claude/<task-description>-<id>`.
- All development work should happen on a dedicated feature branch, then be merged into `main` via a pull request.
- Commits are signed with SSH keys — do not skip signing (`--no-gpg-sign`).

## Git Conventions

- **Commit messages:** Use the imperative mood, concise subject line (≤72 chars), e.g. `Add CLAUDE.md with project documentation`.
- **Commit signing:** All commits are signed. Never bypass signing.
- **Force pushes:** Never force-push to `main` or `demo_branch`.
- **Remote:** `http://local_proxy@127.0.0.1:32515/git/tracerAlone/cc_traceralone`

## Development Workflow

1. **Branch:** Always develop on the designated feature branch (e.g. `claude/add-claude-documentation-25p9U`).
2. **Edit:** Make changes using the appropriate tools (Edit, Write, etc.).
3. **Commit:** Stage and commit with a descriptive message.
4. **Push:** Push to the feature branch with `git push -u origin <branch-name>`.
5. **PR:** Only create a pull request if the user explicitly requests one.

## AI Assistant Guidelines

### General
- Read files before editing them.
- Do not create new files unless necessary; prefer editing existing ones.
- Do not add features, refactors, or "improvements" beyond what was asked.
- Keep code changes minimal and targeted.

### Making Changes
- Always confirm the current branch before committing: `git branch --show-current`.
- Use `git status` to verify the state before committing.
- Commit only files relevant to the task; avoid accidental inclusion of `.env` or secrets.

### GitHub Interactions
- Use the `mcp__github__*` tool family for all GitHub operations (PRs, issues, comments).
- Scope all GitHub operations to `traceralone/cc_traceralone` only.
- Do not post GitHub comments unless a reply is genuinely necessary.

### What to Avoid
- Skipping commit hooks (`--no-verify`).
- Amending published commits.
- Running destructive git commands (`reset --hard`, `clean -f`, `push --force`) without explicit user instruction.
- Guessing or generating external URLs.

## Testing

No test suite exists yet. When tests are added, document the test runner, test file conventions, and how to run them here.

## Environment & Configuration

No environment variables or configuration files are required at this time. When added, document them here and provide an `.env.example`.

## Notes for Future Maintainers

- This CLAUDE.md should be updated whenever the project structure, tech stack, or conventions change.
- Sections marked "No X yet" should be filled in once X is added.
