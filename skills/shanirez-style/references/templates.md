# Verified algorithm templates (ShanireZ OJ style)

Canonical forms lifted from the `OJCode` corpus. Each already follows the house style: Allman
braces, 4-space indent, **no blank lines**, 1-based indexing, global allocation, short names,
`endl`, explicit `long long`. Copy the shape, not just the idea — including the density: these
code blocks have no blank line between functions because the real files don't.

Contents:
1. Fast input — hand-written `read()`
2. DSU / Union-Find (`dfn` + `g[]`)
3. Closed-interval binary search
4. Fenwick / BIT (`lowbit`)
5. Lazy segment tree (`lc/rc` node pool)
6. Graph storage (adjacency `vector`; 链式前向星 as a legacy form to read, not to write)
7. Dijkstra (priority_queue + `vector<Edge>`)
8. Binary-lifting LCA (iterative BFS build)
9. Grid BFS (`ms[4][2]` offsets, `nx`/`ny`)
10. Sorting structs (`operator<` and `cmp`)
11. Formatted float output (PAT)
12. Multi-test-case scaffold

---

## 1. Fast input — hand-written `read()`

Use only when input is large enough to matter (otherwise plain `cin`). Reads a non-negative int;
add a sign branch if negatives are possible.

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
```

---

## 2. DSU / Union-Find

House form: parent array named `g[]`, find function named **`dfn`** (path compression), initialize
`g[i] = i` in `main`, merge inline with the comma operator.

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

The house segment tree is a **static struct pool** named `ns`, indexed by an allocation counter
`npos`, with explicit `lc/rc` child pointers (never `now << 1`). The builder is `init` and takes
the child slot by reference; the range update is `edit`. **`mid` is a plain local
`int mid = (l + r) / 2;` recomputed in each function** — there is no `#define mid`. This is
range-add / range-sum.

```cpp
#define MX 100005
struct Node
{
    long long lc, rc, v, tag;
};
Node ns[MX * 4];
long long n, m, root, npos, a[MX];
void init(long long &now, int l, int r)
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

**The form to write — adjacency `vector`.** Unweighted: `vector<int> es[MX]`. Weighted (or carrying
extra per-edge data): a small `struct Edge`. Iterate with range-for when the edge index isn't
needed. `push_back` and `emplace_back` are both native here; stay consistent within a file.

```cpp
// unweighted
vector<int> es[MX];
es[u].push_back(v), es[v].push_back(u);
// ...
for (int nxt : es[now])
{
    // visit neighbour nxt
}
// weighted / extra data
struct Edge
{
    int to, w;
};
vector<Edge> es[MX];
es[u].emplace_back(Edge{v, w}), es[v].emplace_back(Edge{u, w});
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
`vector<Edge>`, neighbours walked with range-for.

```cpp
#define MX 100005
struct Edge
{
    int to, w;
};
struct Path
{
    int t, d;
    bool operator<(const Path &oth) const
    {
        return d > oth.d;
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
        es[u].emplace_back(Edge{v, w});
    }
    memset(dis, 0x3f, sizeof(dis));
    q.push(Path{s, 0});
    while (q.size())
    {
        int f = q.top().t, d = q.top().d;
        q.pop();
        if (dis[f] != 0x3f3f3f3f)
        {
            continue;
        }
        dis[f] = d;
        for (Edge e : es[f])
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

Iterative BFS to fix depth and immediate parent (safe on a degenerate chain — no recursion to blow
the stack), then double the ancestor table. `anc[i][j]` is the 2^j-th ancestor of `i`; loops use the
literal height (18 covers n ≤ 2.6e5), no `#define LOG`. Because the table is `anc[i][j]`, the build
loop is `for (j) for (i)` — outer over the level, inner over the node.

```cpp
#define MX 200005
vector<int> es[MX];
queue<int> que;
int n, dep[MX], anc[MX][20], vis[MX];
void bfs()
{
    que.push(1), vis[1] = 1;
    while (que.size())
    {
        int now = que.front();
        que.pop();
        for (int nxt : es[now])
        {
            if (vis[nxt])
            {
                continue;
            }
            vis[nxt] = 1, dep[nxt] = dep[now] + 1, anc[nxt][0] = now;
            que.push(nxt);
        }
    }
    for (int j = 1; j <= 18; j++)
    {
        for (int i = 1; i <= n; i++)
        {
            anc[i][j] = anc[anc[i][j - 1]][j - 1];
        }
    }
}
int lca(int x, int y)
{
    if (dep[x] < dep[y])
    {
        swap(x, y);
    }
    for (int i = 18; i >= 0; i--)
    {
        if (dep[anc[x][i]] >= dep[y])
        {
            x = anc[x][i];
        }
    }
    if (x == y)
    {
        return x;
    }
    for (int i = 18; i >= 0; i--)
    {
        if (anc[x][i] != anc[y][i])
        {
            x = anc[x][i], y = anc[y][i];
        }
    }
    return anc[x][0];
}
```

---

## 9. Grid BFS

Offsets live in a global `ms[4][2]` table (never `dx`/`dy`) on its own declaration line; the
candidate cell is `nx`/`ny`; the queue is a global `queue<pair<int, int>> q` pushed with
`q.emplace(make_pair(x, y))` and read through `.first`/`.second`. The bounds check and the visited
check merge into one `||` guard with `continue`. Use `ms[8][2]` for eight directions, or a second
table `ms2[4][2]` for a distinct move set.

```cpp
#define MX 1005
int n, m, dis[MX][MX], vis[MX][MX];
int ms[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
char mp[MX][MX];
queue<pair<int, int>> q;
void bfs(int sx, int sy)
{
    q.emplace(make_pair(sx, sy)), vis[sx][sy] = 1;
    while (q.size())
    {
        int x = q.front().first, y = q.front().second;
        q.pop();
        for (int i = 0; i < 4; i++)
        {
            int nx = x + ms[i][0], ny = y + ms[i][1];
            if (nx < 1 || nx > n || ny < 1 || ny > m || mp[nx][ny] == '#' || vis[nx][ny])
            {
                continue;
            }
            vis[nx][ny] = 1, dis[nx][ny] = dis[x][y] + 1;
            q.emplace(make_pair(nx, ny));
        }
    }
}
```

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
