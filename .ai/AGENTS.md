# AI Agent Instructions

## Project Context
This document provides standards and conventions for AI agents working on projects at this organization.

## Technology Stack

### Core Technologies
- **Language**: Python 3.11+
- **APIs**: FastAPI (async-first)
- **Data Processing**: Polars (preferred) > DuckDB > PySpark > pandas (avoid unless necessary)
- **Database**: PostgreSQL
- **Cloud**: Azure (internal), flexible per customer
- **Package Manager**: uv
- **Version Control**: Git (GitHub)
- **Containerization**: Docker (when necessary)

### Development Tools
- **Formatting**: ruff (format only, not lint)
- **Pre-commit**: Always configured in repo
- **Testing**: pytest
- **Logging**: loguru

## Project Structure

### Standard Layout
```
project/
├── src/<project_name>/
├── tests/
├── docs/
├── pyproject.toml
├── README.md
├── .env
├── .env.example
└── .gitignore
```

### Important Notes
- Project structure templates already exist - do NOT create new structures from scratch
- Analyze existing structure before making changes unless explicitly instructed otherwise
- Keep all local scripts in `scripts/`

## Code Standards

### Style & Conventions
- Follow PEP 8 for formatting and style
- Use descriptive names (variables, functions, classes express their purpose clearly)
- Keep functions small and focused (one task per function)
- Code readability > memory efficiency (unless explicitly required otherwise)
- KISS principle: Keep it simple, stupid - code must be understandable
- All code, comments, and documentation in English

### Documentation
- Docstrings required for all public functions and classes (Google Style)
- Public API must be documented
- README.md at project root
- docs/<DOCUMENTATION>.md for larger projects

### Type Hints
- Optional by default
- Required when it makes sense (e.g., FastAPI applications)
- Not needed for simple data processing scripts

### Error Handling
- As simple as possible, as comprehensive as necessary
- No secrets in code (use .env files)

## Dependencies & Environment

### pyproject.toml Template
Standard template exists in repo. Key settings:
- Python: >=3.11
- Use dependency-groups when meaningful
- Ruff: line-length = 120, format only

### Dependency Management
- Use uv for package management
- No specific policy for dependency updates
- Organize dependencies in groups (dev, test, etc.) when useful

## Testing
- Use pytest
- Only write tests if tests already exist in project
- Do not create test infrastructure from scratch unless explicitly requested

## Version Control
- Agent must NEVER commit code automatically
- Agent only modifies code, commits are manual
- .gitignore always present

## Data Processing Guidelines
- Prefer lazy evaluation (especially with Polars)
- Code readability > memory efficiency (unless explicitly stated otherwise)
- Scripts > Notebooks
- Performance: as efficient as possible while maintaining readability

## Agent Behavior

### General Principles
- Explain code changes only when asked
- Understand project structure before proposing changes (unless told otherwise)
- Legacy code: if not broken, don't fix it
- Proactively warn about potential performance issues when spotted

### What NOT To Do
- Do NOT commit code
- Do NOT touch TODO/FIXME comments unless explicitly requested
- Do NOT use pandas unless unavoidable (prefer: polars > duckdb > spark > pandas)

### Communication
- Code explanations only on request
- All output in English
- Concise and to the point

## Additional Notes
- Pre-commit hooks already configured in repos
- Templates (pyproject.toml, .gitignore) already exist
- Focus on pragmatic solutions over perfect architecture
