---
name: shanirez-style
description: >-
  ShanireZ's personal C++ competitive-programming / online-judge coding style. Use whenever the
  user wants C++ for an algorithm or online-judge problem — pasting a problem statement ("题目如下，
  帮我分析解决", "帮我完成这道题", "帮我写题解 / 给出完整代码"), naming a judge problem id (Luogu
  P*/B*, CodeForces CF*, LOJ #*, PAT, AtCoder ABC*/ARC*/AGC*, SPOJ SP*, UVa UVA*), or asking to
  write, debug, optimize, or refactor an existing .cpp in the OJCode workspace. Read this first so
  the code matches the author's own hand (Allman braces, 4-space, no blank lines, 1-based, global
  state, terse names, endl, explicit long long, // TAG line). Do NOT use for general-purpose C++
  engineering (GUI/Qt apps, servers/networking, embedded, build systems, library/API design,
  business-logic debugging) or for non-C++ tasks — those are not OJCode solutions.
---

# ShanireZ OJ Style (shanirez-style)

## What this is

A style fingerprint distilled from ~3,200 single-file C++ solutions in the `OJCode` workspace
(`Luogu/`, `CodeForces/`, `LOJ/`, `PAT/`, `SP UVA AT/`). The goal is simple: code you generate
or touch should read as if the author wrote it themselves — same skeleton, same brace style, same
short names, same idioms. These are **observed habits**, not rules handed down from on high; where
the corpus is genuinely mixed, this file says so, and you should match the *dominant* habit unless
the surrounding file in the same folder does otherwise.

Where a habit has **shifted over time**, this file follows the last two years of the corpus — the
940 files added since 2024-08 that still exist — not the all-time average. That's what "the
author's current hand" means, and every "N of the last 940" figure below is measured against that
set.

Solutions here are throwaway-style scripts: one file, one problem, global state, terse names, no
build system, no abstraction layers. Resist the instinct to "engineer" them. No OOP wrappers
around algorithms, no namespaces, no templates-for-reuse, no exceptions, no logging, no test
scaffolding. The whole point is a dense, fast, self-contained script.

The C++ dialect is **C++14** — the workspace build task compiles with
`g++ -std=c++14 -O2 -Wall -m64 -static-libgcc -fexec-charset=UTF-8 -Wl,-stack=1073741824`.
Don't reach for C++17/20-only library features. Note the **1 GB stack** in that flag list:
deep recursion is safe on this machine, which is why the recursive form stays the house form
for tree algorithms that a textbook would write iteratively.

---

## The skeleton

Every file looks like this. Internalize it.

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;
#define MX 100005
int n, m, ans, a[MX];
int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
    }
    cout << ans << endl;
    return 0;
}
// TAG: 算法 标签
```

- **Explicit headers, alphabetically sorted** (the editor sorts them automatically).
  `<iostream>` and `<algorithm>` are the workhorses — they appear in ~100% and ~96% of files —
  then `<vector> <queue> <cstring> <cmath> <map> <iomanip> <set> <string>` as needed.
  **Avoid `<bits/stdc++.h>`** (12 files in the whole corpus) unless the existing file in that
  folder already uses it. The one sanctioned `bits/` header is `<bits/extc++.h>` for PBDS —
  see the PBDS note below.
- **`using namespace std;`** — universal (99.9%). Always include it.
- **Globals right after the headers**: big arrays, struct definitions, helper functions, then
  `int main()`. Helpers (dfs, dfn, query…) are free top-level functions above `main`, never methods.
- **`int main()`** (never `signed main()`), ending in an explicit `return 0;`.
- **No `#pragma GCC optimize`** (0 occurrences).

---

## Formatting (the part that's most visible)

### Where the formatting comes from

The layout isn't maintained by hand: it's what `clang-format` produces under the VS Code C/C++
extension's default *Visual Studio* fallback style — **Allman braces, 4-space indent,
`UseTab: Never`, `SortIncludes` on, `ColumnLimit: 0`**. Two consequences are worth internalizing:

- **Includes are always alphabetically sorted.** Zero of the last 940 files have an out-of-order
  include block.
- **There is no column limit — do not wrap lines to fit 80 or 100 columns.** Longest-line
  percentiles over the last 940 files: p50 = 53, p90 = 83, p95 = 98, p99 = 134, max = 155. A long
  condition, a long `cout` chain, or a long comma-operator line stays on one line. Break only
  where the author would naturally break — a multi-clause `||` guard, continuation aligned under
  the opening paren — never to satisfy a ruler.

### Allman braces — always, on every block

The opening `{` goes on **its own line**, for functions, structs, loops, and conditionals — and
braces are **never omitted**, even for a single-statement `if`/`for`/`while`/`else`. This is the
single most consistent habit in the corpus (effectively 100%). Getting this wrong is the fastest
way to make code look foreign.

```cpp
// house style
if (gx != gy)
{
    g[gx] = gy;
}
```

```cpp
// NOT this — brace omitted
if (gx != gy) g[gx] = gy;
// and NOT K&R
if (gx != gy) {
    g[gx] = gy;
}
```

### Indentation: 4 spaces — but check the file you're editing

**Write 4 spaces.** That's what the local editor config produces and what ~90% of the last two
years' files use.

The exception is real and not rare: **96 of the last 940 files (10.2%) are tab-indented.** It
arrives in bursts rather than as a drift — every file added in 2025-08, 41 of 51 in 2025-12, and
11 of 36 in 2026-01 are tabs, while 2026-02 through 2026-04 and 2026-07 have none at all. Read it
as "written somewhere without the workspace's editor config", not as a change of taste.

So: **new file → 4 spaces. Editing an existing file → look at what it already uses and match it**;
roughly one file in ten will be tabs. Never mix the two inside one file, and never reindent a
whole file just to normalize it.

### No blank lines, ever

A solution is **one solid block of code** — no blank line between the headers and the globals,
between two functions, or anywhere inside a function. 938 of the last 940 files (99.8%) contain
zero blank lines. This is as visible as the brace style: a file with airy paragraph spacing reads
as someone else's.

### No trailing newline at end of file

The last byte is the closing `}` or the `// TAG:` line — no final `\n`. Every recent file in the
corpus is written this way.

---

## Arrays, variables, and naming

- **Large arrays live at global scope**, sized with a safety margin (e.g. `100005`, `200005`,
  `MX * 4` for a segment tree). Global allocation avoids stack overflow and is zero-initialized for
  free. Loop counters and small per-query scalars are declared locally where they're used.
- **STL containers and strings also go global**, not declared inside a function:
  **`queue<int> q;`** (52 recent files declare the queue `q`; `que` appears in none),
  `priority_queue<Path> q;` — or `pq` when a plain `queue` already owns `q` —
  `stack<int> st;`, `string s;` at file scope. The corpus is emphatic for containers
  (`queue`/`priority_queue`/`stack`/`deque` are almost always global) and leans global for strings
  too. The one thing to remember: a reused global container must be cleared between independent
  uses (e.g. on each test case).
- **Sizing:** an inline literal (`int a[100005];`) is the most common; `#define MX 100005` is the
  next most common (25 of the last 940 files, ~3%; ~6% all-time) and typical in larger solutions
  so the bound has one name. `const int maxn` is essentially never used here — don't introduce it.
- **`#define` is for named bounds and shorthands, and the list is short.** The main forms are
  **`MX`** (array bound), **`MOD`** (modulus), and **`LC`/`RC`** (segment-tree child shorthand,
  `#define LC ns[now].lc`). A few side forms show up too and read fine when they earn their keep:
  **`MX2` / `M` / `N`** for a second bound in the same file, **`LG`** for a binary-lifting height,
  and `ull` / a `gc()` `fread` macro inside a hand-rolled fast reader. What stays out is the
  reflex macro that replaces plain code: `#define mid (l + r) / 2` reads as foreign (one file in
  the whole corpus) — write `int mid = (l + r) / 2;` in each function instead. When in doubt,
  prefer a literal or a plain variable over a new macro.
- **Pack related globals on one line:** `int n, m, cnt, g[100005];`
- **Short, traditional names.** This is competitive code; verbosity reads as foreign.
  - sizes/counts: `n m k q T`
  - **loop indices:** `i` for a single loop; `i` and `j` for the first and second dimension of a
    2-D array — named by **array dimension, not loop-nesting order** (`anc[i][j]`, so the build loop
    is `for (j...) for (i...)`, outer `j`, inner `i`). Reserve `k` for a genuinely special third
    index, e.g. Floyd's intermediate vertex `for (k) for (i) for (j) dis[i][j] = min(..., dis[i][k] + dis[k][j])`.
  - graph: `u v w` for an edge's two endpoints and weight; **`to`** for an unweighted adjacency
    list and **`es`** for a `vector<Edge>` one (see the graph-storage idiom below). `pre`/`last`
    belong to the legacy 链式前向星 form — read them, don't write them.
  - **depth in a rooted tree is `h[]`** (29 recent files) more often than `dep[]` (5).
  - **node you're at vs. node you step to: `now` = the current node, `nxt` = the next/neighbour
    node** (`for (int nxt : to[now])`, `int nxt = e.to;`). Keep this pair consistent.
  - accumulators: `ans cnt tot sum res`
  - state/pointers: `vis dis dp pos id fa`
  - **boolean flags / on-off state: `ok`, occasionally `trig`** — and declared `int`, not `bool`
    (`int ok = 1; ... ok = 0;`). Never a long descriptive name.
  - intervals: `l r mid`
  - structs: PascalCase — **`Node` (100 recent files), `Edge` (39), `Path` (13)** cover almost
    everything — with terse fields `lc rc v tag`, `x y`, `v w`, `to w`. **The global array of a
    struct is named `ns`** — `Node ns[MX * 4];`, `sort(ns + 1, ns + n + 1)`. Its allocation
    counter is `pos` or `npos` (equally common). The one exception to PascalCase is a
    **comparator functor**, which takes a lowercase name — `cmpa` / `cmpb` / `cmp11` / `hsh` —
    used when one struct needs several orderings or a custom hash.
  - a Chinese-pinyin initialism is fine for a derived table with no short English name —
    `qzmx`/`qzmn` for 前缀 max/min, `hzmx`/`hzmn` for 后缀. Don't force it where `pre`/`suf` reads fine.
- **`long long` is written out explicitly** where range demands it. **Never `#define int long long`**
  (0 occurrences in the corpus) and no `typedef long long ll`. On a long-long-range problem the rule
  is pragmatic, not dogmatic: if a handful of variables fit on **one short declaration line**, just
  make the whole line `long long` rather than splitting hairs over which one actually overflows (the
  author's lazy-but-safe default). But when declarations are many or spread across **big arrays**,
  keep only the value-carrying ones `long long` and leave structural data — indices, depths, node
  ids, visited flags, small counts — as `int` (no point burning memory on a `long long anc[MX][20]`).

### Indexing: 1-based by default

Algorithmic data is 1-based: `for (int i = 1; i <= n; i++)`, arrays used as `a[1..n]`,
`sort(a + 1, a + n + 1)`. Across the last 940 files that's 1,481 `for (i = 1; i <= …)` loops
against 168 `for (i = 0; i < …)`. Drop to 0-based only for things that are natively 0-indexed —
string characters, bitmask bits, and a **grid held as `string s[MX]`, which is 0-based in both
dimensions** — and cast `.size()` to `int` when comparing in a loop bound.

---

## I/O

- **`cin`/`cout` by default** (~90% of files). Plain, unadorned.
- **`endl` is the line terminator** — 830 of the last 940 files (88%). Default to it; it's the
  author's reflex. When a file does use a bare newline instead, it's the **char `'\n'`**
  (27 files) more often than the string `"\n"` (17) — and it's a whole-file choice, not a
  per-line one: 828 files use only `endl`, 15 use only a newline literal, 2 mix them.
- **`ios::sync_with_stdio(false);` is occasional, not default** — ~2% of files, reserved for
  genuinely I/O-heavy problems. Written as that **one statement on its own**, sometimes chained as
  `cin.tie(0)->ios::sync_with_stdio(false);` or `cin.tie(0)->sync_with_stdio(false);`. Don't
  sprinkle it on every file; reach for it (or the hand-written `read()` below) only when input
  size actually warrants it.
- **Hand-written `read()`** (getchar loop) is rarer than it used to be — **13 of the last 940
  files (1.4%)**, down from ~9% all-time. When you do use it, **`printf` comes with it**: 13 of
  the 18 recent files that call `printf` also define `read()`. Fast input and fast output travel
  as a pair, and `cin` + `printf` is a mix that appears in only 2 files. See
  `references/templates.md`.
- **Formatted floats** use `cout << fixed << setprecision(n)` (with `<iomanip>`), *not*
  `printf("%.2f")`. This matters on PAT, where output precision is judged.
- **`scanf` is nearly gone** — 5 of the last 940 files (0.5%). **`printf` is not** (18 files,
  1.9%), but almost every occurrence is the output half of the `read()` + `printf` fast-IO pair
  above, or a `%llu`. Don't reach for `printf` on its own; don't pair it with `cin`.
- **An EOF-driven input loop is `while (cin >> n)`** — 17 recent files, against 5 for
  `while (scanf(...) != EOF)`. Use the `scanf` form only when matching an existing file.
- **Multiple test cases:** `int T; cin >> T; while (T--) { ... }` (95 recent files; the variable
  is `T` or `t`).
  Switch to `for (int t = 1; t <= T; t++)` only when the case number is part of the output
  (`cout << "Case #" << t << ": " << ans << endl;`).

---

## Idioms that make it look authentic

- **Comma operator for grouped side-effects on one line** — a signature habit. Use it for tightly
  related updates:
  ```cpp
  g[gu] = gv, cnt++;                          // DSU merge + count
  anc[nxt][0] = now, h[nxt] = h[now] + 1;     // fix parent + depth before recursing
  vis[nx][ny] = 1, dis[nx][ny] = dis[x][y] + 1;
  ans += es[i].w, d -= es[i].cnt;             // greedy take
  ```
- **Inline ternary for simple branches**, including as a statement and inside output — this has
  become steadily more common:
  ```cpp
  check(mid) ? l = mid + 1 : r = mid - 1;
  s[now] = (sc[j] >= 60 ? 1 : -1);
  cout << (ans == 1e9 ? -1 : ans) << endl;
  ```
- **Infinity is a literal, not a named constant.** `1e9` for `int` range and `1e18` for
  `long long` are the current default (52 vs 27 recent files against `0x3f3f3f3f`) — assign it
  directly and compare against it directly:
  ```cpp
  int ans = 1e9, l = 1, r = 1e9;
  dp[i] = 1e9;
  if (c0 == 1e9 && c1 == 1e9) { ... }
  ```
  **`memset(x, 0x3f, sizeof(x))`** stays the form when you need to fill a whole array at once
  (needs `<cstring>`); then compare against `0x3f3f3f3f` (or `0x3f3f3f3f3f3f3f3f` for `long long`).
  Never `INT_MAX`/`LLONG_MAX`/`#define INF`.
- **Compute a shared prerequisite once, then merge guard clauses with `||`** — don't write a
  staircase of early-exits. Get the value both guards need first, then OR the conditions:
  ```cpp
  int l = lca(x, y);
  if (a[x] == 0 || z[x] + z[y] - 2 * z[l] > 0)   // both "trivially Yes" cases, together
  {
      cout << "Yes" << endl;
      continue;
  }
  ```
  rather than an `if (a[x] == 0) {...continue;}`, then the `lca`, then a separate
  `if (zero on path) {...continue;}`. One guard, all the trivial cases visible at once.
- **STL, used plainly:** `queue`, `priority_queue` (min-heap via a reversed `operator<`, or the
  three-argument `priority_queue<int, vector<int>, greater<int>>` when the element is a bare
  scalar), `map`, `set`.
- **A container is tested for emptiness with `.size()`, never `.empty()`.** `while (q.size())`
  appears in 48 recent files and `while (!q.empty())` in **zero**; `.empty()` shows up anywhere at
  all in only 5. The same reflex applies to a plain truth test: `if (now.size())`, `if (v.size())`.
- **Graph storage — adjacency `vector`, and the name follows the payload.** Unweighted:
  **`vector<int> to[MX]`** — 29 recent files, against `es` 13 and `g` 7. Weighted, or carrying
  extra per-edge data: a small `struct Edge { int to, w; };` held in **`vector<Edge> es[MX]`** —
  25 files, and `vector<Edge> to[]` never appears. Keeping the two names apart is the habit: `to`
  when the element *is* the neighbour, `es` when it's an edge record.
  **Push with `push_back`** — `push_back(Edge{v, w})` 22 files vs `emplace_back(Edge{v, w})` 2,
  and unweighted `push_back(v)` 21 vs `emplace_back(v)` 7. `emplace_back` isn't foreign, but
  `push_back` is the default; stay consistent within a file.
  **链式前向星 (chained forward star) is legacy**: 72 old files use it and **zero** in the last two
  years. Recognize it when reading old code (`last[u]` head, `pre[i]` previous edge, `to[i]`
  endpoint); don't write it in a new file.
- **Iterate edges with range-for when you don't need the edge index:** `for (int nxt : to[now])` or
  `for (Edge e : es[now])`. Only fall back to an indexed loop when the index itself matters, and
  cast the bound: `for (int i = 0; i < (int)es[now].size(); i++)`.
- **A tree DFS carries its parent and skips it — no `vis[]`.** The signature is
  `void dfs(int now, int from)` and the first thing in the loop is the parent guard; ~25 recent
  files do this, and it's the reason `vis[]` is scarce in tree code (it stays for general graphs
  and grids):
  ```cpp
  void dfs(int now, int from)
  {
      for (Edge e : es[now])
      {
          if (e.to == from)
          {
              continue;
          }
          h[e.to] = h[now] + 1;
          dfs(e.to, now);
      }
  }
  ```
- **Grid movement uses a global `ms[4][2]` offsets table and `nx`/`ny`** — never `dx`/`dy`:
  ```cpp
  int ms[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
  // ...
  for (int i = 0; i < 4; i++)
  {
      int nx = x + ms[i][0], ny = y + ms[i][1];
      if (nx < 1 || nx > n || ny < 1 || ny > m || vis[nx][ny])
      {
          continue;
      }
  }
  ```
  Diagonals go in `ms[8][2]`, or a second table `ms2[4][2]` when the two move sets are distinct.
  **The grid itself is usually a `string` array** — `string s[MX];` read with `cin >> s[i]` and
  indexed `s[x][y]` (19 recent files, against 10 for a `char mp[MX][MX]`). A `string` grid is
  naturally **0-based**, so that's one of the places the 1-based default doesn't apply; bounds
  read `nx >= 0 && nx < n`.
- **`__int128` is the overflow escape hatch** — 11 recent files. Reach for it when an intermediate
  product would blow past `long long` (`__int128 res = min((__int128)b, (__int128)n * a);`) or when
  a comparison has to be done exactly. `cin`/`cout` can't handle it, so printing needs a
  hand-written recursive `write()` on `putchar` — see `references/templates.md`. `unsigned
  __int128` shows up too when the value is known non-negative.
- **One function with a parameter, not near-duplicate functions.** When two helpers would share the
  same body and differ only by a constant, write a single function that takes that constant as an
  argument — `val(x, 2)` / `val(x, 5)`, not separate `val2` / `val5`:
  ```cpp
  int val(long long x, int p)   // count how many times prime p divides x
  {
      int c = 0;
      while (x % p == 0)
      {
          x /= p, c++;
      }
      return c;
  }
  ```
- **No redundant casts.** Don't cast an operand wide when the *other* operand is already wide —
  promotion handles it. With `long long scale`, write `fp += (s[i] - '0') * scale;`, not
  `fp += (long long)(s[i] - '0') * scale;`. Only cast when the whole expression would otherwise be
  evaluated narrow (e.g. `(long long)a * b` when both are `int`).
- **Sorting:** a member `bool operator<(const T &oth) const` is the strong default — 79 of the 82
  recent files that define one, always **by const reference, with the parameter named `oth`** (the
  three strays take it by value or name it `other`). A free `bool cmp(T a, T b)` passed to
  `sort(ns + 1, ns + n + 1, cmp)` is the fallback when you sort the same data several ways, and a
  lowercase functor struct (`cmpa`, `cmpb`) when a container needs the ordering as a type. Keep it
  1-based. **No lambdas** — they never appear here, in a sort or anywhere else (0 of 940 files).
- **`auto` is rare** — mostly for iterators. Prefer explicit types.

### PBDS — the one sanctioned `bits/` header

For a hash table or an order-statistic tree, the author reaches for GNU PBDS. It's a small but
entirely recent habit (7 files, all from the last two years), and it's the sole exception to the
"no `bits/*` headers" rule:

```cpp
#include <algorithm>
#include <bits/extc++.h>
#include <iostream>
using namespace std;
using namespace __gnu_pbds;
cc_hash_table<int, int> mp;
```

The include still sorts alphabetically (`algorithm` < `bits/extc++.h` < `iostream`), and
`using namespace __gnu_pbds;` goes on the line after `using namespace std;`. When PBDS is an
*alternative* solution to a problem you already solved another way, name the file with the
` PBDS` suffix — `P3014 PBDS.cpp`.

---

## Comments and the `// TAG:` line

Comments are **sparse** — 129 of the last 940 files carry one at all. A short Chinese note
(UTF-8) for a non-obvious step, a math derivation, or a struct field's meaning, **trailing on the
same line as the code it explains** (239 trailing lines vs 48 standalone). Block comments
(`/* … */`) never appear. Don't narrate line by line; the code carries itself.

End a file you create or substantially touch with an algorithm tag — space-separated
techniques, Chinese (sometimes mixed with English names).

**This one is an aspiration, not a reproduction of the current hand.** Only 17 of the last 940
files (1.8%) carry a `// TAG:` line, and the habit effectively stopped after 2024-10; across the
whole corpus it's 243 of 3,208 (7.6%). The reason to write it anyway is that the repo README
lists "为题解补齐算法标签" as an open Todo — so adding it is helping with a stated goal. Don't
mistake its absence in surrounding files for a signal to drop it, and don't cite it as evidence
of what the author's recent code looks like.

```cpp
// TAG: 网络流 最大流 最小割 Dinic
// TAG: 数位DP 回文
// TAG: 树状数组 线段树 逆序对 动态维护
// TAG: 双向搜索 meet in the middle
```

This line is the last thing in the file, with no newline after it.

---

## File placement & naming

- Put the file in the right judge folder: `Luogu/`, `CodeForces/`, `LOJ/`, `PAT/Basic/`,
  `PAT/Advanced/` (or a contest subfolder like `PAT/22Q2/Basic/`), `SP UVA AT/`.
- Name it by the **exact problem id**: `P3372.cpp`, `B3609.cpp`, `CF1009F.cpp`, `#10000.cpp`,
  `ABC403G.cpp`, `ARC085E.cpp`, `UVA10079.cpp`, `SP1043.cpp`. Luogu's other id prefixes keep the
  same rule — `T777250.cpp` (团队/私有题), `U422706.cpp` (用户上传题).
- For an **alternative approach or partial solution**, append `space + Suffix`:
  `P3367 Dfn.cpp`, `B3609 Tarjan.cpp`, `B3609 Kosaraju.cpp`, `P3372 Zkw.cpp`, `P9753 90pts.cpp`,
  `P3014 PBDS.cpp`, `P1486 Seg.cpp`, `P1525 PowDfn.cpp` (带权并查集), `P2216 DQ.cpp`.

---

## Verified algorithm templates

When you need a standard data structure or algorithm, use the canonical forms in
[`references/templates.md`](references/templates.md) — they're lifted directly from this codebase and
already follow every convention above (Allman, no blank lines, 1-based, global pools, `dfn`/`g[]`
DSU, `lc/rc` segment-tree pool, `to[]`/`es[]` graphs with range-for, recursive binary-lifting LCA,
`read()` + `printf`, Dijkstra, grid BFS, struct sort, `__int128` output, fixed-precision float;
链式前向星 included only as a legacy form to read). Read that file when implementing one of those;
don't reinvent the form from memory.

---

## Don't do these (they read as foreign here)

- Blank lines anywhere in the file, or a trailing newline after the last `}` / `// TAG:` line.
- Wrapping a long line to fit 80 or 100 columns — there is no column limit here.
- Reordering an include block out of alphabetical order.
- `#include <bits/stdc++.h>` in a new file — use the specific headers. (`<bits/extc++.h>` for PBDS
  is the one exception.)
- `#define int long long` — declare the wide variables as `long long` instead.
- `#define mid (l + r) / 2` or a macro standing in for ordinary code — recompute
  `int mid = (l + r) / 2;` in each function. (Named *bounds* are fine: `MX`, `MX2`, `M`, `N`,
  plus `MOD`, `LC`/`RC`, `LG`.)
- K&R braces, or omitting braces on a single-line `if`/`for`/`while`/`else` — always Allman, always braced.
- Tab indentation **in a new file** — 4 spaces. (In an existing file, match what's already there;
  about one in ten is tabs.)
- `while (!q.empty())` — the house form is `while (q.size())`.
- 链式前向星 in a new file — `vector<int> to[MX]` unweighted, `vector<Edge> es[MX]` weighted.
- `vector<int> es[MX]` for an *unweighted* graph — that name is reserved for `vector<Edge>`; the
  unweighted list is `to[]`.
- `dx`/`dy` grid offsets — the table is `ms[4][2]`, the targets are `nx`/`ny`.
- `operator<` taking its argument by value, or naming it `o`/`np` — it's `const T &oth`.
- Wrapping an algorithm in a class/namespace/reusable template — keep raw globals + free functions
  (`using namespace __gnu_pbds;` for PBDS is not an exception to this; it's just a using-directive).
- Defaulting every file to `ios::sync_with_stdio(false)` — it's situational here, not a habit.
- Long descriptive identifiers (`parentArray`, `currentNode`) — use `g`/`fa`, `now`.
- `const int maxn = ...`, `typedef long long ll;`, `INT_MAX`/`LLONG_MAX` — match the inline-literal /
  `#define MX` / explicit-`long long` / `1e9`-`1e18` conventions instead.
