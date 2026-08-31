---
type: procedure
title: shanirez-style 语料计数方法
description: SKILL.md 里每条统计断言的复算方法，针对 OJCode 语料。
tags: [style, corpus, verification]
status: stable
---

# shanirez-style 语料计数方法

Every statistic in `plugins/shanirez-style/skills/shanirez-style/SKILL.md` is measured against the [`../OJCode`](../OJCode) corpus — do not add, soften, or strengthen a claim without re-running the count. Method:

```bash
cd D:/Workspace/OJCode
git ls-files '*.cpp' > /tmp/all.txt
git log --diff-filter=A --since="<2 years ago>" --name-only --format="" -- '*.cpp' \
  | grep -v '^$' | sort -u > /tmp/added.txt
while IFS= read -r f; do [ -f "$f" ] && printf '%s\n' "$f"; done \
  < /tmp/added.txt > /tmp/recent.txt
xargs -a /tmp/recent.txt -d '\n' grep -lIE '<pattern>' | wc -l
```

- `--diff-filter=A` alone is not the recent set; intersect it with files that still exist.
- Where a habit shifted, the last two years win.
- State whether a percentage is whole-corpus or recent.
- Use `xargs -a <list> -d '\n'`; filenames contain spaces.
- Grep before asserting a name or the absence of a form.
