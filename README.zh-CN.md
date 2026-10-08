# agent-skills

个人常用 Agent Skills 仓库，给 [StackFerry](https://github.com/Ninthless/StackFerry)、Grok CLI、Claude Code、Codex 等兼容 [Agent Skills](https://agentskills.io) 的工具用。

StackFerry 把这个仓库识别成 `m4rr5er/agent-skills`，并扫描 `skills/` 下每个带 `SKILL.md` 的目录。

## 目录结构

```text
skills/
  <skill-name>/
    SKILL.md          必填
    references/       可选，详细资料
    scripts/          可选，辅助脚本
```

每个 `SKILL.md` 以 YAML frontmatter 开头：

```markdown
---
name: my-skill
description: 一两句话说明做什么、什么时候用。
---

# My Skill

给 Agent 看的操作说明。
```

`name` 只用小写字母、数字和连字符。`description` 会出现在 StackFerry / Grok 的目录里，也用来决定要不要自动触发。

## 在 StackFerry 里接入

1. 打开 StackFerry → Skills。
2. 添加 GitHub 源：`m4rr5er/agent-skills`，分支 `main`。
3. 按需把 skill 装到 Grok / Claude / Codex。

StackFerry 下载的是 `https://github.com/m4rr5er/agent-skills/archive/refs/heads/main.zip`，所以这个仓库保持公开。

## 不经过 StackFerry，直接给 Grok CLI 用

克隆到本机后，在配置里加上 `skills/` 目录：

```toml
# ~/.grok/config.toml
[skills]
paths = ["C:/projects/agent-skills/skills"]
```

也可以把单个 skill 拷到 `~/.grok/skills/<name>/`。

## 新增一个 skill

1. 新建 `skills/<name>/SKILL.md`。
2. 把触发场景写进 `description`。
3. 提交并推到 `main`。
4. 在 StackFerry 里刷新这个源。

不要把密钥、token、本机私有路径写进这个仓库。

## Skill 列表

| Skill | 用途 |
| --- | --- |
| [`context-handoff`](./skills/context-handoff/) | 上下文约 70% 时，把工作区脚本/文件/文档分好类，写好 `HANDOFF.md`，给自动压缩做交接。 |
