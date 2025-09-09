# AGENTS.md

This file provides guidance to AI Agents such as Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Spec Kit is a Python CLI tool that implements Spec-Driven Development (SDD), a methodology where specifications drive implementation rather than serve as documentation. The project enables AI agents to build software through structured specification-to-implementation workflows.

## Core Architecture

### Spec-Driven Development Workflow
The SDD process follows three main phases:
1. **Specification (`/specify`)** - Create feature specifications with user stories and requirements
2. **Planning (`/plan`)** - Generate implementation plans with technical details and research
3. **Task Breakdown (`/tasks`)** - Create executable task lists for implementation

### Directory Structure
- `src/specify_cli/` - Python CLI implementation using Typer and Rich
- `templates/` - Specification, plan, and task templates
- `templates/commands/` - Custom slash command definitions for AI agents
- `scripts/` - Bash utilities for workflow automation
- `memory/` - Constitutional principles and organizational guidelines
- `specs/` - Generated specifications organized by feature branches

### Branch Naming Convention
Feature branches follow the pattern: `001-feature-name`, `002-another-feature`, etc.

## Development Commands

### Installation and Setup
```bash
# Install from repository
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>

# Development installation
pip install -e .
```

### Key CLI Commands
```bash
# Initialize new project
specify init <project_name> --ai claude|gemini|copilot

# Initialize in current directory
specify init --here --ai claude

# Skip agent tool checks
specify init --ignore-agent-tools
```

### Script Utilities
All scripts are located in `scripts/` and use common functionality from `scripts/common.sh`:

```bash
# Create new feature branch and spec
./scripts/create-new-feature.sh --json "{feature_description}"

# Setup implementation planning
./scripts/setup-plan.sh --json

# Check task prerequisites
./scripts/check-task-prerequisites.sh --json

# Update agent context
./scripts/update-agent-context.sh
```

## Custom Slash Commands

The project provides three custom commands for AI agents:

### `/specify` Command
- Creates feature specifications from natural language descriptions
- Generates new feature branch following naming convention
- Uses `templates/spec-template.md` for structure
- Outputs JSON with branch name and spec file path

### `/plan` Command  
- Generates implementation plans from specifications
- Creates research, data models, contracts, and quickstart guides
- Uses `templates/plan-template.md` for execution flow
- Requires existing specification from `/specify` phase

### `/tasks` Command
- Breaks down plans into executable tasks
- Generates numbered tasks (T001, T002, etc.) with dependency tracking
- Marks parallelizable tasks with [P] notation
- Uses `templates/tasks-template.md` for structure

## Testing and Development

### Python Environment
- Requires Python 3.11+
- Dependencies: typer, rich, httpx, platformdirs, readchar
- Build system: hatchling

### Running Tests
```bash
# Install development dependencies
pip install -e ".[dev]"

# Run tests (if test suite exists)
python -m pytest

# Lint code
python -m ruff check src/
python -m black src/
```

## Working with Specifications

### Feature Development Lifecycle
1. Use `/specify` to create specification and feature branch
2. Use `/plan` to generate implementation details and research
3. Use `/tasks` to create executable task breakdown
4. Implement tasks following the generated plan
5. Create pull request with detailed description

### File Organization
Each feature creates a directory structure:
```
specs/001-feature-name/
├── spec.md              # Functional requirements
├── plan.md              # Implementation plan  
├── research.md          # Technical research
├── data-model.md        # Entity definitions
├── quickstart.md        # Test scenarios
├── tasks.md             # Executable tasks
└── contracts/           # API specifications
```

### Constitutional Principles
The `memory/constitution.md` file contains organizational principles that guide all development decisions. Always reference this when making architectural choices.

## Development Guidelines

### Git Workflow
- Work on feature branches following `###-feature-name` pattern
- Commit specifications before implementation
- Use pull requests for feature integration
- Reference feature directory paths in commits

### Code Quality
- Follow existing patterns in the Python codebase
- Use type hints and documentation
- Maintain consistency with existing CLI interface
- Test custom commands thoroughly

### Template Modifications
When modifying templates in `templates/`:
- Test with all three AI agents (Claude, Gemini, Copilot)
- Ensure JSON output compatibility with scripts
- Validate workflow from specification to implementation
- Update this CLAUDE.md if architecture changes

## Troubleshooting

### Common Issues
- Ensure Python 3.11+ is installed
- Check Git Credential Manager setup on Linux (see README.md)
- Verify AI agent tools are installed (Claude Code, Gemini CLI, etc.)
- Use `--ignore-agent-tools` flag to bypass tool checks

### Script Debugging
Scripts output JSON for parsing by AI agents. For debugging:
- Run scripts manually with `--json` flag
- Check `scripts/common.sh` for shared functions
- Verify feature branch naming convention
- Ensure all paths are absolute when working with specs

### Workflow Recovery
If stuck in the middle of a workflow:
- Check current branch with `git branch`
- Verify spec files exist in `specs/current-branch-name/`
- Re-run appropriate script to continue from checkpoint
- Use `scripts/check-task-prerequisites.sh` to verify state
