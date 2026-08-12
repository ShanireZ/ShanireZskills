# Shanirez-Style

![Type: agent skill](https://img.shields.io/badge/type-agent%20skill-success.svg?style=flat-square)
[![Spec: Agent Skills](https://img.shields.io/badge/spec-Agent%20Skills-blue.svg?style=flat-square)](https://agentskills.io)
![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-D97757.svg?style=flat-square&logo=claude&logoColor=white)
![OpenAI Codex](https://img.shields.io/badge/Codex-compatible-412991.svg?style=flat-square&logo=openai&logoColor=white)
![Target: C++14](https://img.shields.io/badge/target-C%2B%2B14-00599C.svg?style=flat-square&logo=cplusplus&logoColor=white)

个人 Agent Skill：把 ShanireZ 在 [OJCode](https://github.com/ShanireZ/OJCode) 里的竞赛 C++ 代码风格固化下来，让 agent 生成或改写的题解读起来和自己手写的一样。

## 内容

| 路径 | 说明 |
|---|---|
| [`skills/shanirez-style/SKILL.md`](skills/shanirez-style/SKILL.md) | 风格指纹本体：骨架、Allman 大括号、零空行、1-based、命名、I/O、惯用写法 |
| [`skills/shanirez-style/references/templates.md`](skills/shanirez-style/references/templates.md) | 13 份从语料里摘出来的算法模板（DSU、线段树、Dijkstra、倍增 LCA、网格 BFS、`__int128` 输出…） |

风格结论全部来自 `OJCode` 语料的实测统计（全量 3208 个 `.cpp`，其中近 2 年新增且现存的 940 个）。习惯发生过漂移的地方，以近 2 年为准。

## 安装

**Codex / Claude Code / 其它 agent（推荐，用 [skills CLI](https://github.com/vercel-labs/skills)）**

```bash
npx skills add ShanireZ/Shanirez-Style
```

装到 `~/.agents/skills/shanirez-style/`，Codex 与 Claude Code 都能发现。

**只装 Claude Code**

```bash
git clone https://github.com/ShanireZ/Shanirez-Style.git /tmp/sy && cp -r /tmp/sy/skills/shanirez-style ~/.claude/skills/
```

**只装 Codex**

Codex 按 `.agents/skills` → 父目录 → 仓库根 → `$HOME/.agents/skills` → `/etc/codex/skills` 的顺序发现技能，把 `skills/shanirez-style/` 放进其中任一位置即可。

## 何时触发

粘贴题面、报题号（Luogu `P*`/`B*`、CodeForces `CF*`、LOJ `#*`、PAT、AtCoder `ABC*`/`ARC*`/`AGC*`、SPOJ `SP*`、UVa `UVA*`），或要求改写 `OJCode` 里已有的 `.cpp`。

不适用于通用 C++ 工程（GUI、服务端、嵌入式、构建系统、库/API 设计），也不适用于非 C++ 任务。

## 仓库

GitHub 为主仓库，[CNB](https://cnb.cool/Round1/Shanirez-Style) 为镜像，由 workspace 根目录的 `sync-github-cnb.ps1` 同步。
