# twomynds Python Template

The standard template repo for Python projects at twomynds.

Use this to start new services and libraries with the same structure, tooling,
and conventions. Keep it simple, consistent, and easy to onboard.

## Quick start

1) Create a new repo from this template.
2) Rename the package folder under `src/` and update `name` in `pyproject.toml`.
3) Update `README.md` to describe the new project.
4) If you need env vars, add them to `.env.example`.
5) Commit and push.

## What's in the template (and why)

- `.ai/AGENTS.md`: instructions for AI agents collaborating on the project; extend this file with project-specific guidance.
- `.github/ISSUE_TEMPLATE/`: structured issue forms for features, bugs, and tasks so reports are consistent and actionable.
- `.github/workflows/pre-commit.yml`: lightweight CI that runs the same pre-commit checks on every push/PR.
- `.vscode/settings.json`: editor defaults so VS Code users automatically format on save.
- `src/`: dedicated source layout to avoid import path ambiguity and keep project structure clean.
- `tests/`: a clear split between unit and integration tests to keep test intent obvious.
- `.env.example`: placeholder for environment variables; kept empty because this template needs none by default.
- `.pre-commit-config.yaml`: defines the exact hooks that run before commits, so everyone formats code the same way.
- `.python-version`: pins the Python version for tools like pyenv and asdf to keep local dev consistent with CI.
- `pyproject.toml`: single source of truth for project metadata, Python version, and formatting rules. Keeps tool config centralized.
