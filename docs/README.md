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
