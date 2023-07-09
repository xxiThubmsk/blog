# Dijkstra

## Dijkstra 有好几种写法，大概分为朴素算法和优化算法。

dijkstra 算法是基于贪心的算法。其大致思路是将所有点分为两部分，一部分是已经处理出最优路径的点，另一部分是未处理最优路径的点。  
其实也算局部最优解推出全局最优解。欸，这不就是贪心的思路？其实还是贪心。


### 
**以下为朴素算法**

```c++
struct edge {
  int v, w; // v表示边的终点，w表示边权
};

vector<edge> e[maxn]; // 邻接表存储图
int dis[maxn], vis[maxn]; // dis数组表示每个顶点到起点的最短距离，vis数组表示每个顶点的最短距离是否已经确定

void dijkstra(int n, int s) { // n表示顶点数，s表示起点
  memset(dis, 63, sizeof(dis)); // 将所有顶点的最短距离初始化为正无穷
  dis[s] = 0; // 将起点s的最短距离设为0
  for (int i = 1; i <= n; i++) { // 进行n次循环
    int u = 0, mind = 0x3f3f3f3f;
    for (int j = 1; j <= n; j++) // 找到一个未确定最短距离的顶点u，使得它的最短距离最小
      if (!vis[j] && dis[j] < mind) u = j, mind = dis[j];
    vis[u] = true; // 将顶点u的最短距离确定下来
    for (auto ed : e[u]) { // 遍历顶点u的所有出边
      int v = ed.v, w = ed.w;
      if (dis[v] > dis[u] + w) dis[v] = dis[u] + w; // 更新与顶点u相邻顶点的最短距离
    }
  }
}

```

**下面是优化算法**
优化算法是经过优先队列优化之后的。


```c++
struct edge {
  int v, w; // v表示边的终点，w表示边权
};

struct node {
  int dis, u; // dis表示顶点到起点的最短距离，u表示顶点编号

  bool operator>(const node& a) const { return dis > a.dis; } // 重载大于运算符
};

vector<edge> e[maxn]; // 邻接表存储图
int dis[maxn], vis[maxn]; // dis数组表示每个顶点到起点的最短距离，vis数组表示每个顶点的最短距离是否已经确定
priority_queue<node, vector<node>, greater<node> > q; // 定义一个小根堆

void dijkstra(int n, int s) { // n表示顶点数，s表示起点
  memset(dis, 63, sizeof(dis)); // 将所有顶点的最短距离初始化为正无穷
  dis[s] = 0; // 将起点s的最短距离设为0
  q.push({0, s}); // 将起点加入优先队列中
  while (!q.empty()) { // 当优先队列不为空时循环
    int u = q.top().u; // 取出堆顶元素
    q.pop();
    if (vis[u]) continue; // 如果顶点u的最短距离已经确定，则直接跳过
    vis[u] = 1; // 将顶点u标记为已确定
    for (auto ed : e[u]) { // 遍历顶点u的所有出边
      int v = ed.v, w = ed.w;
      if (dis[v] > dis[u] + w) { // 更新与顶点u相邻顶点的最短距离
        dis[v] = dis[u] + w;
        q.push({dis[v], v}); // 将更新后的顶点加入优先队列中
      }
    }
  }
}
```