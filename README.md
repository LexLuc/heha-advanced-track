# heha-advanced-track
Hello Hackathon 进阶场

## Skills 安装方法

本项目的 skills 位于 `skills/` 目录下。根据你使用的 AI 工具，需要将 skills 目录复制到对应的位置：

| AI 工具 | 目标目录 |
|---------|---------|
| Antigravity | `<workspace-root>/.agent/skills/` |
| Claude Code | `<workspace-root>/.claude/skills/` |
| Trae | `<workspace-root>/.trae/skills/` |
| Codex | `<workspace-root>/.agents/skills/` |
| Cursor | `<workspace-root>/.cursor/skills/` |

> 其它工具待补充。

示例（以 Claude Code 为例，在项目根目录下执行）：

**Shell (Linux/macOS):**

```bash
TARGET=.claude  # 根据 AI 工具修改，如 .trae、.agents、.cursor、.agent
mkdir -p $TARGET/skills && cp -r skills/ $TARGET/
```

**PowerShell/CMD (Windows):**

```powershell
$TARGET=".claude"  # 根据 AI 工具修改，如 .trae、.agents、.cursor、.agent
mkdir $TARGET\skills -Force
xcopy skills $TARGET\ /E /I
```

## 阅读：Skills 最佳实践

- [Trae - Best Practice for How to Write a Good Skill](https://docs.trae.ai/ide/best-practice-for-how-to-write-a-good-skill)
- [Claude Code - Agent Skills Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Antigravity - Skills Best Practices](https://antigravity.google/docs/skills#best-practices)
