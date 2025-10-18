## Development Rules

### General

- Update existing docs (Markdown files) in `./docs` directory before any code refactoring
- Add new docs (Markdown files) to `./docs` directory after new feature implementation (do not create duplicated docs)
- use `context7` mcp tools for docs of plugins/packages
- use `senera` mcp tools for semantic retrieval and editing capabilities
- use `psql` bash command to query database for debugging
- whenever you want to see the whole code base, use this command: `repomix` and read the output summary file.

### Environment Setup

- Use docker compose for development environment

### Code Quality Guidelines

- Don't be too harsh on code linting and formatting
- Prioritize functionality and readability over strict style enforcement
- Use reasonable code quality standards that enhance developer productivity
- Allow for minor style variations when they improve code clarity

### Pre-commit/Push Rules

- Run `./scripts/format_code.sh` before commit
- Run `./scripts/run_tests.sh` before push (DO NOT ignore failed tests just to pass the build or github actions)
- Keep commits focused on the actual code changes
- **DO NOT** commit and push any confidential information (such as dotenv files, API keys, database credentials, etc.) to git repository!
- NEVER automatically add AI attribution signatures like:
  "🤖 Generated with [Claude Code]"
  "Co-Authored-By: Claude noreply@anthropic.com"
  Any AI tool attribution or signature
- Create clean, professional commit messages without AI references. Use conventional commit format.
