File `CLAUDE.md` chính là System Prompt khá quan trọng của dự án, mình thường để thông tin sơ bộ về dự án cùng những quy định khi phát triển để Claude Code ít mắc sai lầm hơn.

Đầu tiên mình cần bật chế độ planning để tạo ra những file docs (markdown) trong folder `/docs` như:

- project-overview.md (mô tả, tính năng chủ đạo,...)
- api-docs.md
- integration-guide.md

Sau đó mình dùng command `/init` của CC yêu cầu analyze folder này và tạo CLAUDE.md

Tiếp theo mình sẽ mở file CLAUDE.md và thêm mục “Development Rules” vào cuối cùng.

Nó sẽ tương tự như thế này:
SystemPrompt-example.md

File này sẽ được AI tool load vào lúc ban đầu khi open session

- Với Claude Code thì là file CLAUDE.md
- Với Crush CLI thì là file CRUSH.md


```markdown

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

```