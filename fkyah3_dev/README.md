# fkyah3/oh-my-openagent-fkyah3 — Windows-compatible OMO 分支

> 基于 [code-yeongyu/oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent)
> 这些 bug 在社区挂了几个月，没人愿意修 Windows 平台的问题。我们修了。

---

## 本分支修复了什么 / What This Fork Fixes

### 1. Git-master 前缀注入根治 / Git-master Prefix Triple Fix

每次执行 git 命令时自动注入 `GIT_MASTER=1` 前缀，且通过配置 `disabled_skills` 无法关闭。根因有三层：

- Schema 默认值硬编码 `"GIT_MASTER=1"`
- Ultrawork 指令模板硬编码 `load_skills=["git-master"]`，绕过 `disabled_skills`
- `injectGitMasterConfig` 后备值 `?? "GIT_MASTER=1"` 在默认值清空后重新生成

**修复**：三层全部清零。分支：`fix/git-master-disable-default`

### 2. Non-interactive-env Hook Windows 兼容 / Hook Windows Compatibility

在 Windows 上执行 git 命令时报错 `export : 无法将“export”项识别为 cmdlet`。根因：`detectShellType()` 检测到 `MSYSTEM` 环境变量（Git Bash）返回 `"unix"`，生成 `export KEY=val;` 语法，但实际执行环境是 PowerShell。

**修复**：在 `non-interactive-env-hook.ts` 中，Windows 平台固定使用 PowerShell 语法（`$env:KEY='val';`），不再依赖父进程 shell 类型检测。

---

## 分支 / Branches

| 分支 | 说明 |
|------|------|
| `fix/git-master-disable-default` | git-master + non-interactive-env 修复 |
| `main` | 上游同步 |

---

## 目录结构 / Directory Layout

| 路径 | 说明 |
|------|------|
| `fkyah3_dev/README.md` | 本文件 |
| `fkyah3_dev/git-master-issue.md` | git-master 问题详细分析 |
| `fkyah3_dev/non-interactive-env-fix.md` | non-interactive-env 修复文档 |

---

## 开源许可 / License

SUL-1.0（继承上游 code-yeongyu/oh-my-openagent）
