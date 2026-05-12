---
name: git-guardrails-claude-code
description: 设置 Claude Code hooks，在危险的 git 命令（push、reset --hard、clean、branch -D 等）执行前拦截它们。当用户想要阻止破坏性 git 操作、添加 git 安全 hooks 或在 Claude Code 中阻止 git push/reset 时使用。 (Set up Claude Code hooks to block dangerous git commands (push, reset --hard, clean, branch -D, etc.) before they execute. Use when user wants to prevent destructive git operations, add git safety hooks, or block git push/reset in Claude Code.)
---

# 设置 Git Guardrails

设置一个 PreToolUse hook，在 Claude 执行之前拦截和阻止危险的 git 命令。

> Sets up a PreToolUse hook that intercepts and blocks dangerous git commands before Claude executes them.

## 被阻止的命令 (What Gets Blocked)

- `git push`（包括 `--force` 等所有变体）
  > `git push` (all variants including `--force`)
- `git reset --hard`
- `git clean -f` / `git clean -fd`
- `git branch -D`
- `git checkout .` / `git restore .`

被阻止时，Claude 会看到一条消息，告知它没有权限访问这些命令。

> When blocked, Claude sees a message telling it that it does not have authority to access these commands.

## 步骤 (Steps)

### 1. 询问范围

询问用户：仅安装到**本项目**（`.claude/settings.json`）还是**所有项目**（`~/.claude/settings.json`）？

> Ask the user: install for **this project only** (`.claude/settings.json`) or **all projects** (`~/.claude/settings.json`)?

### 2. 复制 hook 脚本

捆绑脚本位于：[scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

> The bundled script is at: [scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

根据范围复制到目标位置：
> Copy it to the target location based on scope:

- **项目级**：`.claude/hooks/block-dangerous-git.sh`
  > **Project**: `.claude/hooks/block-dangerous-git.sh`
- **全局**：`~/.claude/hooks/block-dangerous-git.sh`
  > **Global**: `~/.claude/hooks/block-dangerous-git.sh`

使用 `chmod +x` 使其可执行。
> Make it executable with `chmod +x`.

### 3. 将 hook 添加到 settings

添加到相应的 settings 文件：
> Add to the appropriate settings file:

**项目级**（`.claude/settings.json`）：
> **Project** (`.claude/settings.json`):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

**全局**（`~/.claude/settings.json`）：
> **Global** (`~/.claude/settings.json`):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

如果 settings 文件已存在，将 hook 合并到现有的 `hooks.PreToolUse` 数组中 —— 不要覆盖其他设置。

> If the settings file already exists, merge the hook into existing `hooks.PreToolUse` array — don't overwrite other settings.

### 4. 询问是否需要自定义

询问用户是否想从阻止列表中添加或删除任何模式。相应地编辑复制的脚本。

> Ask if user wants to add or remove any patterns from the blocked list. Edit the copied script accordingly.

### 5. 验证

运行快速测试：
> Run a quick test:

```bash
echo '{"tool_input":{"command":"git push origin main"}}' | <path-to-script>
```

应以代码 2 退出，并向 stderr 输出 BLOCKED 消息。
> Should exit with code 2 and print a BLOCKED message to stderr.
