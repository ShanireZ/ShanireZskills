# Verified algorithm templates (ShanireZ OJ style)

Canonical forms lifted from the `OJCode` corpus. Each already follows the house style: Allman
braces, 4-space indent, **no blank lines**, 1-based indexing, global allocation, short names,
`endl`, explicit `long long`. Copy the shape, not just the idea — including the density: these
code blocks have no blank line between functions because the real files don't.

Contents:
1. Fast input/output — hand-written `read()` + `printf`
2. DSU / Union-Find (`dfn` + `g[]`)
3. Closed-interval binary search
4. Fenwick / BIT (`lowbit`)
5. Lazy segment tree (`lc/rc` node pool)
6. Graph storage (`to[]` unweighted, `es[]` weighted; 链式前向星 as a legacy form to read)
7. Dijkstra (priority_queue + `vector<Edge>`)
8. Binary-lifting LCA (recursive DFS build)
9. Grid BFS (`struct Node` queue, `ms[4][2]` offsets, `nx`/`ny`)
10. Sorting structs (`operator<` and `cmp`)
11. Formatted float output (PAT)
12. Multi-test-case scaffold
13. `__int128` output

---

## 1. Fast input/output — hand-written `read()` + `printf`

Use only when input is large enough to matter (otherwise plain `cin`) — 13 of the last 940 files.
Reads a non-negative int; add a sign branch if negatives are possible. **Fast input and fast
output come as a pair**: a file with `read()` prints with `printf`, not `cout` (13 of the 18
recent `printf` files also define `read()`; only 2 files mix `cin` with `printf`).

```cpp
int read()
{
    int ans = 0;
    char ch = getchar();
    while (ch < '0' || ch > '9')
    {
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9')
    {
        ans = ans * 10 + ch - '0';
        ch = getchar();
    }
    return ans;
}
// ...
int n = read();
// ...
printf("%d\n", ans[i]);
```

---

## 2. DSU / Union-Find

House form: parent array named `g[]`, find function named **`dfn`** (path compression), initialize
`g[i] = i` in `main`, merge inline with the comma operator. Two spellings of `dfn` are equally
native across the last two years — the block form below, and a one-line ternary that assigns
inside the branch:

```cpp
int dfn(int x)
{
    return x == g[x] ? x : g[x] = dfn(g[x]);
}
```

A 带权并查集 keeps the same shape but grabs the old parent before recursing, so the stored
weight can be folded on the way back up (file suffix ` PowDfn`):

```cpp
int dfn(int x)
{
    if (x != g[x])
    {
        int f = g[x];
        g[x] = dfn(g[x]);
        v[x] += v[f];
    }
    return g[x];
}
```

```cpp
int n, m, cnt, g[100005];
int dfn(int x)
{
    if (g[x] != x)
    {
        g[x] = dfn(g[x]);
    }
    return g[x];
}
int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
    {
        g[i] = i;
    }
    for (int i = 1; i <= m; i++)
    {
        int u, v;
        cin >> u >> v;
        int gu = dfn(u), gv = dfn(v);
        if (gu != gv)
        {
            g[gu] = gv, cnt++;
        }
    }
    return 0;
}
```

---

## 3. Closed-interval binary search

Search on `[l, r]`; the answer is `r` after the loop. The decision is written as a one-line ternary.

```cpp
while (l <= r)
{
    long long mid = (l + r) / 2;
    check(mid) ? l = mid + 1 : r = mid - 1;
}
cout << r << endl;
```

---

## 4. Fenwick / BIT

```cpp
long long c[100005];
int lowbit(int x)
{
    return x & -x;
}
void edit(int x, long long k)
{
    while (x <= n)
    {
        c[x] += k;
        x += lowbit(x);
    }
}
long long query(int x)
{
    long long ans = 0;
    while (x)
    {
        ans += c[x];
        x -= lowbit(x);
    }
    return ans;
}
```

---

## 5. Lazy segment tree (node pool with `lc/rc`)

The house segment tree is a **static struct pool** named `ns`, indexed by an allocation counter,
with explicit `lc/rc` child pointers (never `now << 1`). The builder takes the child slot by
reference; the range update is `edit`. **`mid` is a plain local `int mid = (l + r) / 2;`
recomputed in each function** — there is no `#define mid`. This is range-add / range-sum.

Two naming pairs are equally native, so pick one and keep it: the builder is **`init`** or
**`maketree`** (6 files vs 4 in the last two years) and the counter is **`npos`** or **`pos`**
(6 vs 6). **The child links are always `int`** — all 13 recent pool files declare `int lc, rc`,
which is also what the "structural data stays `int`" rule in `SKILL.md` says. Only the fields
that carry a value (`v`, `tag`) widen to `long long`, and only when the sums actually need it.

```cpp
#define MX 100005
struct Node
{
    int lc, rc;
    long long v, tag;
};
Node ns[MX * 4];
int n, m, root, npos;
long long a[MX];
void init(int &now, int l, int r)
{
    now = ++npos;
    if (l == r)
    {
        ns[now].v = a[l];
        return;
    }
    int mid = (l + r) / 2;
    init(ns[now].lc, l, mid), init(ns[now].rc, mid + 1, r);
    ns[now].v = ns[ns[now].lc].v + ns[ns[now].rc].v;
}
void pushdown(int now, int l, int r)
{
    if (ns[now].tag)
    {
        int mid = (l + r) / 2;
        ns[ns[now].lc].v += (mid - l + 1) * ns[now].tag;
        ns[ns[now].rc].v += (r - mid) * ns[now].tag;
        ns[ns[now].lc].tag += ns[now].tag;
        ns[ns[now].rc].tag += ns[now].tag;
        ns[now].tag = 0;
    }
}
void edit(int now, int l, int r, int x, int y, long long k)
{
    if (x <= l && y >= r)
    {
        ns[now].v += (r - l + 1) * k;
        ns[now].tag += k;
        return;
    }
    pushdown(now, l, r);
    int mid = (l + r) / 2;
    if (x <= mid)
    {
        edit(ns[now].lc, l, mid, x, y, k);
    }
    if (y > mid)
    {
        edit(ns[now].rc, mid + 1, r, x, y, k);
    }
    ns[now].v = ns[ns[now].lc].v + ns[ns[now].rc].v;
}
long long query(int now, int l, int r, int x, int y)
{
    if (x <= l && y >= r)
    {
        return ns[now].v;
    }
    pushdown(now, l, r);
    int mid = (l + r) / 2;
    long long ans = 0;
    if (x <= mid)
    {
        ans += query(ns[now].lc, l, mid, x, y);
    }
    if (y > mid)
    {
        ans += query(ns[now].rc, mid + 1, r, x, y);
    }
    return ans;
}
// build with: init(root, 1, n);
```

When the `ns[now].lc` / `ns[now].rc` spelling gets noisy, the corpus shortens it with the one
sanctioned pair of macros — `#define LC ns[now].lc` and `#define RC ns[now].rc` — so the recursive
calls read `init(LC, l, mid), init(RC, mid + 1, r);`. Use both or neither.

---

## 6. Graph storage

**The form to write — adjacency `vector`, named for its payload.** Unweighted lists are
`vector<int> to[MX]` (29 recent files, vs 13 for `es` and 7 for `g`); weighted ones, or any
carrying extra per-edge data, use a small `struct Edge` in `vector<Edge> es[MX]` (25 files —
`vector<Edge> to[]` never appears). Push with **`push_back`** (22 : 2 over `emplace_back` for
struct edges, 21 : 7 unweighted). Iterate with range-for when the edge index isn't needed.

```cpp
// unweighted
vector<int> to[MX];
to[u].push_back(v), to[v].push_back(u);
// ...
for (int nxt : to[now])
{
    // visit neighbour nxt
}
// weighted / extra data
struct Edge
{
    int to, w;
};
vector<Edge> es[MX];
es[u].push_back(Edge{v, w}), es[v].push_back(Edge{u, w});
// ...
for (Edge e : es[now])
{
    int nxt = e.to;
    // use e.w
}
```

**Legacy — 链式前向星 (chained forward star).** 72 old files use this and **none** from the last
two years. Recognize it when reading or editing old code; do not write it into a new file.
`last[u]` is the head of `u`'s list, `pre[i]` the previous edge, `to[i]`/`w[i]` the endpoint/weight.

```cpp
int last[100005], pre[200005], to[200005], w[200005], epos;
void addEdge(int u, int v, int val)
{
    ++epos;
    to[epos] = v, w[epos] = val;
    pre[epos] = last[u];
    last[u] = epos;
}
// iterate: for (int i = last[u]; i; i = pre[i]) { int v = to[i]; ... }
```

---

## 7. Dijkstra (priority_queue + `vector<Edge>`)

Lazy Dijkstra: a min-heap via a reversed `operator<`, `0x3f` infinity (a whole-array fill, so
`memset` rather than a `1e9` literal), finalize each node the first time it's popped. Graph held as
`vector<Edge>`, neighbours walked with range-for. The heap payload is a second small struct —
`Path { int v, w; }` (or `Node { int id, dis; }`) — pushed with `push_back`/`push`, never a lambda
comparator.

```cpp
#define MX 100005
struct Edge
{
    int to, w;
};
struct Path
{
    int v, w;
    bool operator<(const Path &oth) const
    {
        return w > oth.w;
    }
};
vector<Edge> es[MX];
priority_queue<Path> q;
int dis[MX], n, m, s;
int main()
{
    cin >> n >> m >> s;
    for (int i = 1; i <= m; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        es[u].push_back(Edge{v, w});
    }
    memset(dis, 0x3f, sizeof(dis));
    q.push(Path{s, 0});
    while (q.size())
    {
        int now = q.top().v, d = q.top().w;
        q.pop();
        if (dis[now] != 0x3f3f3f3f)
        {
            continue;
        }
        dis[now] = d;
        for (Edge e : es[now])
        {
            if (dis[e.to] != 0x3f3f3f3f)
            {
                continue;
            }
            q.push(Path{e.to, d + e.w});
        }
    }
    for (int i = 1; i <= n; i++)
    {
        cout << dis[i] << " ";
    }
    return 0;
}
```

---

## 8. Binary-lifting LCA

**A recursive DFS, not a BFS.** All 8 recent files that build an `anc[][]` table use
`void dfs(int now, int from)`; `void bfs` appears in 2 files in the whole corpus. The local build
links with `-Wl,-stack=1073741824`, so a 1 GB stack makes recursion safe even on a degenerate
chain. Two details that are easy to get wrong:

- **The doubling happens at the top of `dfs`, for `now` alone** — not as a separate
  `for (j) for (i)` pass after the traversal. Each node's row is complete because its parent's
  row was filled before the recursive call.
- **Depth lives in `h[]`** (29 recent files) rather than `dep[]` (5), and the parent is skipped
  with the `from` guard instead of a `vis[]` array.

`anc[i][j]` is the 2^j-th ancestor of `i`; loops use the literal height (19 covers n ≤ 5e5) — a
`#define LG 19` is acceptable but the literal is more common.

```cpp
#define MX 300005
vector<int> to[MX];
int n, h[MX], anc[MX][20];
void dfs(int now, int from)
{
    for (int i = 1; i <= 19; i++)
    {
        anc[now][i] = anc[anc[now][i - 1]][i - 1];
    }
    for (int nxt : to[now])
    {
        if (nxt == from)
        {
            continue;
        }
        anc[nxt][0] = now, h[nxt] = h[now] + 1;
        dfs(nxt, now);
    }
}
int lca(int a, int b)
{
    if (h[a] < h[b])
    {
        swap(a, b);
    }
    for (int i = 19; i >= 0; i--)
    {
        if (h[anc[a][i]] >= h[b])
        {
            a = anc[a][i];
        }
    }
    if (a == b)
    {
        return a;
    }
    for (int i = 19; i >= 0; i--)
    {
        if (anc[a][i] != anc[b][i])
        {
            a = anc[a][i], b = anc[b][i];
        }
    }
    return anc[a][0];
}
// root it with: h[1] = 1, dfs(1, 0);
```

---

## 9. Grid BFS

Offsets live in a global `ms[4][2]` table (never `dx`/`dy`) on its own declaration line; the
candidate cell is `nx`/`ny`. **The cell carried through the queue is a two-field `struct Node`**,
pushed with `q.push(Node{x, y})` — 30 recent files push a braced struct this way, against 2 that
use `q.emplace(make_pair(...))`. The bounds check and the visited check merge into one `||` guard
with `continue`. Use `ms[8][2]` for eight directions, or a second table `ms2[4][2]` for a distinct
move set.

The **grid itself is usually a `string` array** (19 recent files vs 10 for `char mp[][]`), read
with `cin >> s[i]`. A `string` grid is 0-based, so the bounds read `nx >= 0 && nx < n` — one of
the few places the 1-based default doesn't apply.

```cpp
#define MX 1005
struct Node
{
    int x, y;
};
queue<Node> q;
string s[MX];
int n, m, dis[MX][MX], vis[MX][MX];
int ms[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
void bfs(int sx, int sy)
{
    q.push(Node{sx, sy}), vis[sx][sy] = 1;
    while (q.size())
    {
        int x = q.front().x, y = q.front().y;
        q.pop();
        for (int i = 0; i < 4; i++)
        {
            int nx = x + ms[i][0], ny = y + ms[i][1];
            if (nx < 0 || nx >= n || ny < 0 || ny >= m || s[nx][ny] == '#' || vis[nx][ny])
            {
                continue;
            }
            vis[nx][ny] = 1, dis[nx][ny] = dis[x][y] + 1;
            q.push(Node{nx, ny});
        }
    }
}
```

When the queue has to carry a third value and no struct is warranted, `queue<pair<int, int>>` with
`q.emplace(make_pair(now, from))` and `.first`/`.second` is the secondary form (5 files) — read it,
but reach for `struct Node` first.

---

## 10. Sorting structs

A member `operator<` is the strong default — **always by const reference, parameter named `oth`**.
A free `cmp` is the fallback when the same data is sorted several ways. Always 1-based:
`sort(ns + 1, ns + n + 1, ...)`.

```cpp
// (a) operator< on the struct — the default
struct Node
{
    int x, y;
    bool operator<(const Node &oth) const
    {
        return x == oth.x ? y < oth.y : x < oth.x;   // ascending by x, then y
    }
};
sort(ns + 1, ns + n + 1);
// (b) free comparator — when one struct needs several orders
struct Stu
{
    int a, c, id;
};
bool cmp(Stu x, Stu y)
{
    return x.a > y.a;     // descending by a
}
sort(ns + 1, ns + n + 1, cmp);
```

---

## 11. Formatted float output (PAT)

Decimal precision via `<iomanip>` — the judged form on PAT. Not `printf("%.2f")`.

```cpp
#include <iomanip>
// ...
cout << fixed << setprecision(2) << ans << endl;
```

---

## 12. Multi-test-case scaffold

```cpp
int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        // solve one case; reset any reused globals here (memset, loop, etc.)
    }
    return 0;
}
```

When the case number is part of the output, index the loop instead:

```cpp
int main()
{
    int T;
    cin >> T;
    for (int t = 1; t <= T; t++)
    {
        int ans = 1e9;
        // solve one case
        cout << "Case #" << t << ": " << (ans == 1e9 ? -1 : ans) << endl;
    }
    return 0;
}
```

---

## 13. `__int128` output

`__int128` is the overflow escape hatch (11 recent files) — an intermediate product past
`long long`, or a comparison that has to be exact. `cin`/`cout` don't know the type, so printing
goes through a short recursive `write()` on `putchar`. Use `unsigned __int128` when the value is
known non-negative.

```cpp
void write(unsigned __int128 x)
{
    if (x > 9)
    {
        write(x / 10);
    }
    putchar(x % 10 + '0');
}
// ...
unsigned __int128 ans = 1;
ans = ans * n * (n + 1) * (n * 2 + 1) / 6;
write(ans);
```

Reading is the mirror image (a `read()` loop accumulating into an `__int128`), but far more often
the value is read as `long long` and widened at the point of use:

```cpp
__int128 res = min((__int128)b, (__int128)n * a);
```
