# Aptinery

[![License: GPL-3.0](https://img.shields.io/badge/license-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
![Type: Agent Skills marketplace](https://img.shields.io/badge/type-Agent%20Skills%20marketplace-success.svg?style=flat-square)
[![Spec: Agent Skills](https://img.shields.io/badge/spec-Agent%20Skills-blue.svg?style=flat-square)](https://agentskills.io)
[![Spec: Agent Plugins](https://img.shields.io/badge/spec-Agent%20Plugins-635BFF.svg?style=flat-square)](https://agent-plugins.org)
![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-D97757.svg?style=flat-square&logo=claude&logoColor=white)
![OpenAI Codex](https://img.shields.io/badge/Codex-compatible-412991.svg?style=flat-square&logo=openai&logoColor=white)

**Aptinery — A curated foundry for Agent Skills.**

精选、改造并跨客户端发行 Agent Skills 的个人技能工坊与市场。仓库同时提供通用 Agent Skills、Agent Plugins、Codex/ChatGPT 插件和 Claude Code marketplace 入口，当前收录 20 个可独立安装的 skill；市场不把它们捆绑成一个聚合插件。

## Skills

| Skill | 用途 | 来源与许可 |
| --- | --- | --- |
| [`shanirez-style`](plugins/shanirez-style/skills/shanirez-style/SKILL.md) | 结构清晰的算法竞赛 C++14 代码风格 | 原创，GPL-3.0 |
| [`k12-lesson-plan-creation`](plugins/k12-lesson-plan-creation/skills/k12-lesson-plan-creation/SKILL.md) | 创建符合教学标准的 K-12 课程方案、课堂材料与评估 | Anthropic / Learning Commons，Apache-2.0 |
| [`k12-lesson-differentiation`](plugins/k12-lesson-differentiation/skills/k12-lesson-differentiation/SKILL.md) | 将已有课程分层适配不同熟练度与学生需求 | Anthropic / Learning Commons，Apache-2.0 |
| [`k12-lesson-prep`](plugins/k12-lesson-prep/skills/k12-lesson-prep/SKILL.md) | 帮助教师内化已有课程的关键任务并生成课前准备笔记 | Anthropic / Learning Commons，Apache-2.0 |
| [`k12-check-for-understanding`](plugins/k12-check-for-understanding/skills/k12-check-for-understanding/SKILL.md) | 为数学主题创建基于常见误解的形成性理解检查 | Anthropic / Learning Commons，Apache-2.0 |
| [`emil-design-eng`](plugins/emil-design-eng/skills/emil-design-eng/SKILL.md) | 打磨 UI、组件设计和动画决策 | Emil Kowalski，MIT |
| [`animate`](plugins/animate/skills/animate/SKILL.md) | 从零实现有目的、可中断的 Web 动画 | Emil Kowalski，MIT |
| [`animate-expo`](plugins/animate-expo/skills/animate-expo/SKILL.md) | 实现高性能 React Native / Expo 动画、手势与触觉反馈 | Emil Kowalski，MIT |
| [`review-animations`](plugins/review-animations/skills/review-animations/SKILL.md) | 按严格工艺标准审查动画代码 | Emil Kowalski，MIT |
| [`improve-animations`](plugins/improve-animations/skills/improve-animations/SKILL.md) | 审计代码库动效并生成可执行改进计划 | Emil Kowalski，MIT |
| [`find-animation-opportunities`](plugins/find-animation-opportunities/skills/find-animation-opportunities/SKILL.md) | 找出真正值得加入动效的界面位置 | Emil Kowalski，MIT |
| [`animation-vocabulary`](plugins/animation-vocabulary/skills/animation-vocabulary/SKILL.md) | 将模糊的动效描述定位到准确术语 | Emil Kowalski，MIT |
| [`apple-design`](plugins/apple-design/skills/apple-design/SKILL.md) | 将 Apple 的界面与流畅动效原则应用到 Web | Emil Kowalski，MIT |
| [`write-swift`](plugins/write-swift/skills/write-swift/SKILL.md) | 编写和审查现代 Swift、并发、性能与测试代码 | Emil Kowalski，MIT |
| [`pick-ui-library`](plugins/pick-ui-library/skills/pick-ui-library/SKILL.md) | 从精选清单中为前端任务选择合适的库 | Emil Kowalski，MIT |
| [`prototype`](plugins/prototype/skills/prototype/SKILL.md) | 创建多个差异化 UI 方案并通过可视选择器比较 | Emil Kowalski，MIT |
| [`ask-sonner`](plugins/ask-sonner/skills/ask-sonner/SKILL.md) | 安装、配置、样式化和排查 Sonner toast | Emil Kowalski，MIT |
| [`punk-cover`](plugins/punk-cover/skills/punk-cover/SKILL.md) | 将文章、帖子和主题生成平台适配的封面图或复用提示词 | AdrianPunk 与贡献者，GPL-3.0 |
| [`punk-avatar`](plugins/punk-avatar/skills/punk-avatar/SKILL.md) | 将人物、宠物、物品照片或描述生成头像与纪念卡 | AdrianPunk 与贡献者，GPL-3.0 |
| [`punk-poster-layout`](plugins/punk-poster-layout/skills/punk-poster-layout/SKILL.md) | 仅在明确要求结构构图控制时，为独立海报、传单、主视觉或单画布编辑版块选择 32 种构图；普通网站/UI、通用 Hero 与未明确要求构图控制的平台封面不默认触发 | 原文 AdrianPunk；Agent Skill 由 Aptinery 贡献者改编与包装；GPL-3.0 |

## 安装

以下单项安装命令均以 `animate` 为例，可替换为上表中的任一 skill 名称。

### skills CLI：Codex、Claude Code、Cursor、OpenCode 等

全局安装全部 skill：

```bash
npx skills add ShanireZ/Aptinery -g
```

单独安装表格中的任一 skill：

```bash
npx skills add ShanireZ/Aptinery -g --skill animate
```

### Claude Code marketplace

```bash
claude plugin marketplace add ShanireZ/Aptinery
claude plugin install animate@aptinery
```

`pick-ui-library`、`prototype` 和 `review-animations` 保留上游的 Claude 显式调用契约；安装后分别通过 `/pick-ui-library:pick-ui-library`、`/prototype:prototype` 和 `/review-animations:review-animations` 调用。

### Codex / ChatGPT marketplace

```bash
codex plugin marketplace add ShanireZ/Aptinery
codex plugin add animate@aptinery
```

上述 3 个显式调用 skill 在 Codex 中通过各自的 `agents/openai.yaml` 禁止隐式调用，可用 `$pick-ui-library`、`$prototype` 和 `$review-animations` 显式调用。

### GitHub Copilot CLI marketplace

```bash
copilot plugin marketplace add ShanireZ/Aptinery
copilot plugin install animate@aptinery
```

### Agent Plugins 客户端

每个 `plugins/<skill-name>/plugin.json` 都是独立的 Agent Plugins v1 入口；可安装名称与上表完全一致。

## 来源与许可

本仓库原创内容采用 [GNU General Public License v3.0](LICENSE) 发布。

第三方内容继续采用各自的 `LICENSE` 和归属声明：

- Anthropic / Learning Commons 的四个 K-12 skill 使用 Apache-2.0，固定来源 commit 记录在 [`NOTICE`](NOTICE)。
- Emil Kowalski 的 12 个设计与工程 skill 使用 MIT，固定来源 commit 记录在 [`NOTICE`](NOTICE)。
- AdrianPunk 与贡献者的三个 Punk skill 使用 GPL-3.0；两个视觉生成 skill 的固定来源 commit，以及构图 skill 的文章来源和许可记录在 [`NOTICE`](NOTICE)。

每个主插件根目录及其 skill 目录都包含适用的许可证副本。Claude 专用镜像仅用于保留 3 个 skill 的 `disable-model-invocation: true` 语义，不增加新的市场条目或许可条件。
