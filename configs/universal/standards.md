# Cross-Project Coding Standards

Standards that apply to all projects regardless of which AI tool is active.

## General

- Modular, single-responsibility functions
- No placeholder comments -- implement or flag explicitly
- Flag security issues proactively
- Always explain architecture and propose folder structure before writing code
- No hardcoded secrets, credentials, or API keys in code

## Python

- Type hints on ALL functions
- Docstrings on all public functions and classes
- uv for virtual environments and dependency management
- FastAPI for all backend/API work
- pytest for testing -- write tests alongside code, not after
- Structure: src/ layout with separate modules per domain
- Ruff for linting and formatting (replaces flake8, black, isort)

## TypeScript / JavaScript

- TypeScript preferred over plain JS
- React Native + Expo for mobile
- Functional components and hooks only -- no class components
- Axios or fetch for API calls
- Jest for testing

## Git

- Commit as you go -- not one giant commit at the end
- Branch for every feature: phase-2/feature-name, fix/bug-name
- Never git add -A blindly -- stage specific files
- Semantic versioning: v{major}.{minor}.{patch}
- Initialize a version-control repo at the start of every new project
- Trunk-based flow: the main branch stays always-stable; open a PR to merge a feature branch, then delete the branch after merge
- Solo projects stay lean: skip develop/staging branches, branch protection rules, and forced squashing

### Commit Prefixes

| Prefix | Use When |
|--------|----------|
| `feat:` | New feature or capability |
| `fix:` | Bug fix |
| `refactor:` | Code restructuring, no behavior change |
| `chore:` | Dependencies, config, tooling |
| `docs:` | Documentation only |

### Semantic Versioning

Format: `v{major}.{minor}.{patch}`

- **Major** = new phase or breaking change
- **Minor** = new feature
- **Patch** = bug fix

Tag milestones: `git tag -a v1.0.0 -m "description"` then `git push origin v1.0.0`
