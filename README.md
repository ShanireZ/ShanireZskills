# ShanireZskills

[![License: GPL-3.0](https://img.shields.io/badge/license-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
![Type: Agent Skills marketplace](https://img.shields.io/badge/type-Agent%20Skills%20marketplace-success.svg?style=flat-square)
[![Spec: Agent Skills](https://img.shields.io/badge/spec-Agent%20Skills-blue.svg?style=flat-square)](https://agentskills.io)
[![Spec: Agent Plugins](https://img.shields.io/badge/spec-Agent%20Plugins-635BFF.svg?style=flat-square)](https://agent-plugins.org)
![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-D97757.svg?style=flat-square&logo=claude&logoColor=white)
![OpenAI Codex](https://img.shields.io/badge/Codex-compatible-412991.svg?style=flat-square&logo=openai&logoColor=white)

`ShanireZskills` 是一个公开的 Agent Skills 市场。仓库同时提供通用 Agent Skills、Agent Plugins、Codex/ChatGPT 插件和 Claude Code marketplace 入口，当前收录 3 个 skill。

## Skills

| Skill | 用途 | 来源与许可 |
|---|---|---|
| [`shanirez-style`](skills/shanirez-style/SKILL.md) | 复现 ShanireZ 的算法竞赛 C++14 代码风格 | 本仓库原创，GPL-3.0 |
| [`k12-lesson-planning`](skills/k12-lesson-planning/SKILL.md) | 创建符合教学标准的 K-12 课程方案与课堂材料 | Anthropic / Learning Commons，Apache-2.0 |
| [`k12-lesson-differentiation`](skills/k12-lesson-differentiation/SKILL.md) | 将已有课程分层适配不同熟练度与学生需求 | Anthropic / Learning Commons，Apache-2.0 |

两个 K-12 skill 从 [`anthropics/k12-teacher-skills`](https://github.com/anthropics/k12-teacher-skills) 的提交 `6fc400329540e068516bd34aa78120d89e5e4e8b` 原样复制。它们可以在没有 Learning Commons Knowledge Graph connector 的环境中运行；本市场没有复制上游的可选 MCP 配置。

## 安装

### skills CLI：Codex、Claude Code、Cursor、OpenCode 等

全局安装全部 skill：

```bash
npx skills add ShanireZ/Shanirez-Style -g
```

只安装一个 skill：

```bash
npx skills add ShanireZ/Shanirez-Style -g --skill shanirez-style
npx skills add ShanireZ/Shanirez-Style -g --skill k12-lesson-planning
npx skills add ShanireZ/Shanirez-Style -g --skill k12-lesson-differentiation
```

### Claude Code marketplace

```bash
claude plugin marketplace add ShanireZ/Shanirez-Style
claude plugin install shanirezskills@shanirezskills
```

### Codex / ChatGPT marketplace

```bash
codex plugin marketplace add ShanireZ/Shanirez-Style
```

随后在 Codex CLI 中打开 `/plugins`，从 `ShanireZskills` 市场安装同名 bundle。安装后新开一个会话，让三个 skill 进入新的上下文。

### GitHub Copilot CLI marketplace

```bash
copilot plugin marketplace add ShanireZ/Shanirez-Style
copilot plugin install shanirezskills@shanirezskills
```

### Agent Plugins 客户端

仓库根目录的 [`plugin.json`](plugin.json) 是 Agent Plugins v1 入口；兼容客户端可以直接导入本仓库。

## 分发结构

```text
.
├── plugin.json                         # Agent Plugins
├── .codex-plugin/plugin.json           # Codex / ChatGPT bundle
├── .agents/plugins/marketplace.json    # Codex / ChatGPT marketplace
├── .claude-plugin/plugin.json          # Claude Code plugin
├── .claude-plugin/marketplace.json     # Claude Code / Copilot CLI marketplace
└── skills/                             # 三个通用 Agent Skills
```

市场机器标识为 `shanirezskills`；界面名称为 `ShanireZskills`。原生插件以一个 bundle 发布三个 skill，而 skills CLI 仍可按名称单独安装。

## 许可

本仓库原创内容采用 [GNU General Public License v3.0](LICENSE) 发布。

`skills/k12-lesson-planning/` 和 `skills/k12-lesson-differentiation/` 中复制的第三方内容继续采用 Apache License 2.0；各目录保留自己的 `LICENSE` 和归属声明，仓库根目录的 [`NOTICE`](NOTICE) 记录了来源提交与上游版权信息。Apache-2.0 与 GPLv3 兼容，但第三方文件的原许可和 NOTICE 义务不会因此消失。

## 仓库

GitHub 为公开主仓库，[CNB](https://cnb.cool/Round1/Shanirez-Style) 为镜像，由 workspace 根目录的 `sync-github-cnb.ps1` 同步。
