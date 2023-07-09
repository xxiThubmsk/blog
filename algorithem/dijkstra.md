# Dijkstra

## Dijkstra �кü���д������ŷ�Ϊ�����㷨���Ż��㷨��

dijkstra �㷨�ǻ���̰�ĵ��㷨�������˼·�ǽ����е��Ϊ�����֣�һ�������Ѿ����������·���ĵ㣬��һ������δ��������·���ĵ㡣  
��ʵҲ��ֲ����Ž��Ƴ�ȫ�����Ž⡣�G���ⲻ����̰�ĵ�˼·����ʵ����̰�ġ�

**���ˣ�Dijkstra�㷨��Ե��ǵ�Դ��һ���㵽�ܶ�����������·�����Լ�û�и�Ȩ**

### 
**����Ϊ�����㷨**

```c++
struct edge {
  int v, w; // v��ʾ�ߵ��յ㣬w��ʾ��Ȩ
};

vector<edge> e[maxn]; // �ڽӱ�洢ͼ
int dis[maxn], vis[maxn]; // dis�����ʾÿ�����㵽������̾��룬vis�����ʾÿ���������̾����Ƿ��Ѿ�ȷ��

void dijkstra(int n, int s) { // n��ʾ��������s��ʾ���
  memset(dis, 63, sizeof(dis)); // �����ж������̾����ʼ��Ϊ������
  dis[s] = 0; // �����s����̾�����Ϊ0
  for (int i = 1; i <= n; i++) { // ����n��ѭ��
    int u = 0, mind = 0x3f3f3f3f;
    for (int j = 1; j <= n; j++) // �ҵ�һ��δȷ����̾���Ķ���u��ʹ��������̾�����С
      if (!vis[j] && dis[j] < mind) u = j, mind = dis[j];
    vis[u] = true; // ������u����̾���ȷ������
    for (auto ed : e[u]) { // ��������u�����г���
      int v = ed.v, w = ed.w;
      if (dis[v] > dis[u] + w) dis[v] = dis[u] + w; // �����붥��u���ڶ������̾���
    }
  }
}

```

**�������Ż��㷨**
�Ż��㷨�Ǿ������ȶ����Ż�֮��ġ�


```c++
struct edge {
  int v, w; // v��ʾ�ߵ��յ㣬w��ʾ��Ȩ
};

struct node {
  int dis, u; // dis��ʾ���㵽������̾��룬u��ʾ������

  bool operator>(const node& a) const { return dis > a.dis; } // ���ش��������
};

vector<edge> e[maxn]; // �ڽӱ�洢ͼ
int dis[maxn], vis[maxn]; // dis�����ʾÿ�����㵽������̾��룬vis�����ʾÿ���������̾����Ƿ��Ѿ�ȷ��
priority_queue<node, vector<node>, greater<node> > q; // ����һ��С����

void dijkstra(int n, int s) { // n��ʾ��������s��ʾ���
  memset(dis, 63, sizeof(dis)); // �����ж������̾����ʼ��Ϊ������
  dis[s] = 0; // �����s����̾�����Ϊ0
  q.push({0, s}); // �����������ȶ�����
  while (!q.empty()) { // �����ȶ��в�Ϊ��ʱѭ��
    int u = q.top().u; // ȡ���Ѷ�Ԫ��
    q.pop();
    if (vis[u]) continue; // �������u����̾����Ѿ�ȷ������ֱ������
    vis[u] = 1; // ������u���Ϊ��ȷ��
    for (auto ed : e[u]) { // ��������u�����г���
      int v = ed.v, w = ed.w;
      if (dis[v] > dis[u] + w) { // �����붥��u���ڶ������̾���
        dis[v] = dis[u] + w;
        q.push({dis[v], v}); // �����º�Ķ���������ȶ�����
      }
    }
  }
}
```