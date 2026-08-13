# ShanireZskills

[![License: GPL-3.0](https://img.shields.io/badge/license-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
![Type: Agent Skills marketplace](https://img.shields.io/badge/type-Agent%20Skills%20marketplace-success.svg?style=flat-square)
[![Spec: Agent Skills](https://img.shields.io/badge/spec-Agent%20Skills-blue.svg?style=flat-square)](https://agentskills.io)
[![Spec: Agent Plugins](https://img.shields.io/badge/spec-Agent%20Plugins-635BFF.svg?style=flat-square)](https://agent-plugins.org)
![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-D97757.svg?style=flat-square&logo=claude&logoColor=white)
![OpenAI Codex](https://img.shields.io/badge/Codex-compatible-412991.svg?style=flat-square&logo=openai&logoColor=white)

`ShanireZskills` 是一个公开的 Agent Skills 市场。仓库同时提供通用 Agent Skills、Agent Plugins、Codex/ChatGPT 插件和 Claude Code marketplace 入口，当前收录 3 个可独立安装的 skill；市场不再把它们捆绑成一个聚合插件。

## Skills

| Skill                                                                      | 用途                                       | 来源与许可                               |
| -------------------------------------------------------------------------- | ------------------------------------------ | ---------------------------------------- |
| [`shanirez-style`](plugins/shanirez-style/skills/shanirez-style/SKILL.md)                                         | 结构清晰的算法竞赛 C++14 代码风格          | 原创，GPL-3.0                            |
| [`k12-lesson-planning`](plugins/k12-lesson-planning/skills/k12-lesson-planning/SKILL.md)                           | 创建符合教学标准的 K-12 课程方案与课堂材料 | Anthropic / Learning Commons，Apache-2.0 |
| [`k12-lesson-differentiation`](plugins/k12-lesson-differentiation/skills/k12-lesson-differentiation/SKILL.md)     | 将已有课程分层适配不同熟练度与学生需求     | Anthropic / Learning Commons，Apache-2.0 |

## 安装

### skills CLI：Codex、Claude Code、Cursor、OpenCode 等

全局安装全部 skill：

```bash
npx skills add ShanireZ/ShanireZskills -g
```

单独安装指定 skill：

```bash
npx skills add ShanireZ/ShanireZskills -g --skill shanirez-style
npx skills add ShanireZ/ShanireZskills -g --skill k12-lesson-planning
npx skills add ShanireZ/ShanireZskills -g --skill k12-lesson-differentiation
```

### Claude Code marketplace

添加市场后，只安装需要的插件（下面 3 条安装命令按需选择）：

```bash
claude plugin marketplace add ShanireZ/ShanireZskills
claude plugin install shanirez-style@shanirezskills
claude plugin install k12-lesson-planning@shanirezskills
claude plugin install k12-lesson-differentiation@shanirezskills
```

### Codex / ChatGPT marketplace

添加市场后，只安装需要的插件（下面 3 条安装命令按需选择）：

```bash
codex plugin marketplace add ShanireZ/ShanireZskills
codex plugin add shanirez-style@shanirezskills
codex plugin add k12-lesson-planning@shanirezskills
codex plugin add k12-lesson-differentiation@shanirezskills
```

### GitHub Copilot CLI marketplace

添加市场后，只安装需要的插件（下面 3 条安装命令按需选择）：

```bash
copilot plugin marketplace add ShanireZ/ShanireZskills
copilot plugin install shanirez-style@shanirezskills
copilot plugin install k12-lesson-planning@shanirezskills
copilot plugin install k12-lesson-differentiation@shanirezskills
```

### Agent Plugins 客户端

每个目录下的 Agent Plugins v1 manifest 都是独立入口，兼容客户端按需导入：

- [`plugins/shanirez-style/plugin.json`](plugins/shanirez-style/plugin.json)
- [`plugins/k12-lesson-planning/plugin.json`](plugins/k12-lesson-planning/plugin.json)
- [`plugins/k12-lesson-differentiation/plugin.json`](plugins/k12-lesson-differentiation/plugin.json)

## 许可

本仓库原创内容采用 [GNU General Public License v3.0](LICENSE) 发布。

第三方内容继续采用各自的 `LICENSE` 和归属声明，仓库根目录的 [`NOTICE`](NOTICE) 记录了来源提交与上游版权信息，第三方文件的原许可和 NOTICE 义务不会消失。
