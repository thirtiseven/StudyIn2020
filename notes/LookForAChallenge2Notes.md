[TOC]

# 16th Polish Collegiate Programming Contest

claris的题解 [X Open Cup named after E.V. Pankratiev. European Grand Prix](https://www.cnblogs.com/clrs97/p/8861303.html)

原来gym上有这套题 https://codeforces.com/gym/100523

## A Arithmetic Rectangle

### 题意

给出一个n*m的子矩阵，求出其最大等差子矩阵的面积。

等差矩阵即每行每列都为等差序列之矩阵。

测试组数不超过10000，n,m <= 3000 元素值不超过1e9，每个测试文件大小不超过20MB



### 引导问题1：Air crashes

给出一个数组a，对于每一个元素，找出其左侧最近的比它小的元素。

#### 解法：*following the arrows*

在数组前添加一个哨兵节点，其值设为负无穷。用一个数组，称其为arrow，来维护每个元素左侧比它小的最近元素。

从左向右遍历数组a，若$a_{i-1} < a_i$ 则$a_{i-1}$ 即是$a_i$左侧最近的比它小的元素。否则，比较 $arrow[a_{i-1}]$ 指向的值与 $a_i$ 的大小关系，不断循环，直到找到为止。

此解法的时间复杂度为线性，因为每一支箭都最多只被向前移动一次。



### 引导问题2：Plot

给出一个01矩阵A，找出其中面积最大的全1子矩阵之面积。

#### 解法：O(nm)

建立一个辅助数组D，D[i,j] 表示(i,j)下方连续的1的个数，该数组可以方便地线性求得。

那么，对于每一个非零的D[i,j]，我们在同一行里从(i,j)出发寻找左右两侧离其最近的D[i,k]使得D[i,k]<=D[i,j]。

计左侧点的标号为j'，右侧点的标号为j''，那么 $D[i,j]\times (j''-j'-1)$ 即为一个符合条件的子矩阵面积。

对于矩阵中的每一个元素计算上式，最大的结果即为答案，时间复杂度为$O(nm)$



### 原问题解法

经过观察，我们可以将一个数列分解成若干个子序列，使得每一个子序列都是等差数列。显然，相邻的两个子序列有且仅有有一个公共元素。

于是我们可以在线性时间内处理出从宽度为1的最大等差子矩阵，相似地，我们也可以这样处理出长度为1，宽度为2和长度为2的最大等差子矩阵。

下面就只剩长度和宽度都大于3的情况待解决。对于每个3\*3的子矩阵，我们判断它是否为等差矩阵，如果是，则标记该子矩阵的中心元素。经过进一步观察，我们发现，如果一个子矩阵是等差矩阵，当且仅当该矩阵的每一个3\*3大小的子矩阵都为等差矩阵。

于是，利用Plot一题中的方法，求出标记过为3*3子矩阵中心的最大子矩阵，即能求出a矩阵的最大等差子矩阵。

总的时间复杂度为O(nm).



### 代码

```c++
#include <iostream>
#include <algorithm>
#include <climits>

const int maxn = 3000+5;
const int inf = INT_MIN;

int T;
int n, m;
int a[maxn][maxn], d[maxn][maxn];
int arrow_left[maxn], arrow_right[maxn];
int ans, now;
int diff, diff1, diff2;

int main(int argc, char *argv[]) { 
	std::ios::sync_with_stdio(false);
	std::cin.tie(0); 
	std::cin >> T;
	while (T--) {
		std::cin >> n >> m;
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= m; j++) {
				std::cin >> a[i][j];
			}
		}
		ans = 1;
		if (m > 1) {
			ans = std::max(ans, 2);
			for (int i = 1; i <= n; i++) {
				now = 2;
				diff = a[i][2]-a[i][1];
				for (int j = 3; j <= m; j++) {
					if (a[i][j] - a[i][j-1] == diff) {
						now++;
					} else {
						diff = a[i][j] - a[i][j-1];
						now = 2;
					}
					ans = std::max(ans, now);
				}
			}
		}
		if (n > 1) {
			ans = std::max(ans, 2);
			for (int j = 1; j <= m; j++) {
				now = 2;
				diff = a[2][j]-a[1][j];
				for (int i = 3; i <= n; i++) {
					if (a[i][j] - a[i-1][j] == diff) {
						now++;
					} else {
						diff = a[i][j] - a[i-1][j];
						now = 2;
					}
					ans = std::max(ans, now);
				}
			}
		}
		if (n > 1 && m > 1) {
			ans = std::max(ans, 4);
			for (int i = 1; i <= n-1; i++) {
				now = 4;
				diff1 = a[i][2] - a[i][1], diff2 = a[i+1][2] - a[i+1][1];
				for (int j = 3; j <= m; j++) {
					if (a[i][j] - a[i][j-1] == diff1 && a[i+1][j] - a[i+1][j-1] == diff2) {
						now+=2;
					} else {
						diff1 = a[i][j] - a[i][j-1], diff2 = a[i+1][j] - a[i+1][j-1];
						now = 4;
					}
					ans = std::max(ans, now);
				}
			}
			for (int j = 1; j <= m-1; j++) {
				now = 4;
				diff1 = a[2][j] - a[1][j], diff2 = a[2][j+1] - a[1][j+1];
				for (int i = 3; i <= n; i++) {
					if (a[i][j] - a[i-1][j] == diff1 && a[i][j+1] - a[i-1][j+1] == diff2) {
						now+=2;
					} else {
						diff1 = a[i][j] - a[i-1][j], diff2 = a[i][j+1] - a[i-1][j+1];
						now = 4;
					}
					ans = std::max(ans, now);
				}
			}
		}
		if (n >= 3 && m >= 3) {
			for (int i = 0; i <= n; i++) {
				for (int j = 0; j <= m; j++) {
					d[i][j] = 0;
				}
			}
			for (int i = n-1; i >= 2; i--) {
				for (int j = 2; j <= m-1; j++) {
					int yes = 1;
					if (a[i-1][j+1] - a[i-1][j] != a[i-1][j] - a[i-1][j-1]) {
						yes = 0;
					}
					if (a[i][j+1] - a[i][j] != a[i][j] - a[i][j-1]) {
						yes = 0;
					}
					if (a[i+1][j+1] - a[i+1][j] != a[i+1][j] - a[i+1][j-1]) {
						yes = 0;
					}
					if (a[i+1][j-1] - a[i][j-1] != a[i][j-1] - a[i-1][j-1]) {
						yes = 0;
					}
					if (a[i+1][j] - a[i][j] != a[i][j] - a[i-1][j]) {
						yes = 0;
					}
					if (a[i+1][j+1] - a[i][j+1] != a[i][j+1] - a[i-1][j+1]) {
						yes = 0;
					}
					if (yes) {
						d[i][j] = d[i+1][j] + 1;
					} else {
						d[i][j] = 0;
					}
				}
			}
			for (int i = 2; i <= n-1; i++) {
				d[i][1] = inf;
				d[i][m] = inf;
				for (int j = 2; j <= m-1; j++) {
					if (d[i][j-1] < d[i][j]) {
						arrow_left[j] = j-1;
					} else {
						arrow_left[j] = arrow_left[j-1];
						while (d[i][arrow_left[j]] >= d[i][j]) {
							arrow_left[j] = arrow_left[arrow_left[j]];
						}
					}
				}
				for (int j = m-1; j >= 2; j--) {
					if (d[i][j+1] < d[i][j]) {
						arrow_right[j] = j+1;
					} else {
						arrow_right[j] = arrow_right[j+1];
						while (d[i][arrow_right[j]] >= d[i][j]) {
							arrow_right[j] = arrow_right[arrow_right[j]];
						}
					}
				}
				for (int j = 2; j <= m-1; j++) {
					if (d[i][j] > 0) ans = std::max(ans, (d[i][j]+2)*(arrow_right[j]-arrow_left[j]+1));
				}
			}
		}
		std::cout << ans << '\n';
	}
}
```





## B Bytean Road Race

### 题意

一张网格图中一些线段被标出，跑步选手可以从左上到右下选择路线跑步。多次询问两个节点，判断是否有能经过这两个节点的跑步路线。

### 解法：O(nlogn)

先把网格向旋转135度，现在它变成了一个根在下面的DAG。

定义左路径是从一个点出发尽可能向左走的路径，右路径是从一个点出发尽可能向右走的路径。

如果p和q之间有路径，那么做一条经过点q的水平线，则：

从p出发的左路径与水平线的交点要么是q，要么在q左侧；从p出发的右路径和水平线的交点要么是q，要么在q右侧。

#### 左右路径的结构：倍增

构造二维数组left，对于每个v和i  <=log2n，left[i,j] 表示从v出发沿着左路径走2^i步能到达的点的标号。同理构造right。然后我们可以在O(nlogn)时间内计算出left和right的值。
$$
left[u,i]:=left[left[u,i-1],i-1].
$$
然后，就可以计算出left和right与水平线的交点，进而在log时间内回答每次询问。

### 引导问题：树上相互的位置

给一颗二叉树，询问某两个点的位置关系：高低左右

（高低指是否是直系亲属）

#### 解法

深度优先搜索，记录每个点第一次被访问的时间 $I$ 和访问完其所有子节点后离开它的时间$O$。

a 高于 b 当且仅当  Ia < Ib 且 Ob < Oa

a 在 b 的左边当且仅当 Oa < Ib

### 解法：O(n)

将所有左路径建成一颗树，将所有右路径也建成一棵树。注意，此时所有的边都汇入终点，因此它的根就是最后一个交叉点。在翻转的图形中，正好形成了一个有根树。

经过观察，我们发现在左树中如果p在q的右侧，则从p出发的做路径与水平线的交点在q的右侧。反之亦然。于是便可以在常数时间内回答每次询问。

总的时间复杂度为 O(n)。

### 代码

```c++
#include <iostream>
#include <vector>

const int maxn = 100000+7;

int n, m, k;
int u, v;
std::pair<int, int> tl[maxn], tr[maxn];
int inl[maxn], inr[maxn], outl[maxn], outr[maxn];

void inti() {
	std::cin >> n >> m >> k;
	std::pair<int, int> p[n+1];
	std::vector<int> g[n+1];
	for (int i = 1; i <= n; i++) {
		std::cin >> p[i].first >> p[i].second;
	}
	for (int i = 1; i <= m; i++) {
		std::cin >> u >> v;
		g[u].push_back(v);
		g[v].push_back(u);
	}
	int nextl, nextr;
	for (int i = 1; i <= n; i++) {
		tl[i] = tr[i] = std::make_pair(0, 0);
	}
	for (int i = 1; i <= n; i++) {
		nextl = nextr = 0;
		for (auto it: g[i]) {
			if (p[it].first > p[i].first) {
				nextl = it;
			}
			if (p[it].second < p[i].second) {
				nextr = it;
			}
		}
		if (nextr == 0) {
			nextr = nextl;
		}
		if (nextl == 0) {
			nextl = nextr;
		}
 		if (p[nextl].second == p[i].second) {
			tl[nextl].first = i;
		} else {
			tl[nextl].second = i;
		}
		if (p[nextr].second == p[i].second) {
			tr[nextr].first = i;
		} else {
			tr[nextr].second = i;
		}
	}
}

int cur;

void dfsl(int now) {
	cur++;
	inl[now] = cur;
	if (tl[now].first != 0) {
		dfsl(tl[now].first);
	}
	if (tl[now].second != 0) {
		dfsl(tl[now].second);
	}
	cur++;
	outl[now] = cur;
}

void dfsr(int now) {
	cur++;
	inr[now] = cur;
	if (tr[now].first != 0) {
		dfsr(tr[now].first);
	}
	if (tr[now].second != 0) {
		dfsr(tr[now].second);
	}
	cur++;
	outr[now] = cur;
}

int main(int argc, char *argv[]) {  
	std::ios::sync_with_stdio(false);
	std::cin.tie(0); 
	inti();
	cur = 0;
	dfsl(n);
	cur = 0;
	dfsr(n);	
	for (int i = 1; i <= k; i++) {
		std::cin >> u >> v;
		bool yes = 0;
		if (inl[v] < inl[u] && outl[v] > outl[u]) {
			yes = 1;
		}
		if (inr[v] < inr[u] && outr[v] > outr[u]) {
			yes = 1;
		}
		if (inl[u] < inl[v] && outl[u] > outl[v]) {
			yes = 1;
		}
		if (inr[u] < inr[v] && outr[u] > outr[v]) {
			yes = 1;
		}
		if (outl[v] < inl[u] && outr[u] < inr[v]) {
			yes = 1;
		}
		if (outl[u] < inl[v] && outr[v] < inr[u]) {
			yes = 1;
		}
		if (yes) {
			std::cout << "TAK\n";
		} else {
			std::cout << "NIE\n";
		}
	}
}
```





## C Will It Stop?

### 题意

输入n，当n>1时，如果n是偶数则除2，否则n=3n+3，不断循环，问程序能否停止。

### 解法

判断n是否是2的幂。使用位运算快速判断。

### 代码

```c++
#include <iostream>

int main(int argc, char *argv[]) {  
	std::ios::sync_with_stdio(false);
	std::cin.tie(0); 
	unsigned long long n;
	std::cin >> n;
	std::cout << ((((n^(n-1))&n)==n)?"TAK\n":"NIE\n");
}
```



## D Ants

### 题意

给一棵二叉树，有两只蚂蚁分别从根的两侧沿着正反的树欧拉序爬树，第一只蚂蚁向上爬一边需要两秒，向下需要一秒。第二只蚂蚁爬树的速度是第一只的两倍。当蚂蚁相遇时，他们回头继续爬。当蚂蚁碰到地面时，它们折回继续爬。问两只蚂蚁第二次相遇的时间。n1e8，输入数据比给的空间大很多。

### 解法

留坑...



## E Gophers

### 题意

数轴上有一些住着人的洞窟，有一个人试图用m台CD机播放摇滚音乐来打扰洞窟里的住民。每台CD机可以影响的范围为l。CD机的主人会移动一台CD机。每次移动后，询问有多少个洞窟会受到音乐的影响。

### 解法

由于区间等长，把区间中点扔进set里，每次修改找前驱和后继查询一下受影响的点有几个，删点，查询一下修改后位置前驱后继看看受影响的点有几个，加点。维护答案。

```c++
#include <iostream>
#include <set>
#include <algorithm>

const int maxn = 500000+7;

int n, m, d, l;
int x[maxn];
std::set<int> z;
int p, r;
int zz;

int main(int argc, char *argv[]) {  
	std::ios::sync_with_stdio(false);
	std::cin.tie(0); 
	std::cin >> n >> m >> d >> l;
	x[1] = 0;
	for (int i = 2; i <= n; i++) {
		std::cin >> x[i];
	}
	for (int i = 0; i < m; i++) {
		std::cin >> zz;
		z.insert(zz);
	}
	z.insert(-1000000007);
	z.insert(2000000007);
	int ans = 0;
	for (int i = 1; i <= n; i++) {
		auto it1 = z.lower_bound(x[i]);
		auto it2 = it1;
		it2--;
		if (std::abs(*it1-x[i]) <= l || std::abs(*it2-x[i]) <= l) {
			ans++;
		}
	}
	std::cout << ans << '\n';
	int left, right, ll, rr;
	for (int i = 0; i < d; i++) {
		std::cin >> p >> r;
		auto it1 = z.lower_bound(p);
		auto it2 = it1;
		it1++;
		it2--;
		left = std::max(*it2+l+1, p-l);
		right = std::min(*it1-l-1, p+l);
		ll = std::lower_bound(x+1, x+n+1, left)-(x+1);
		rr = std::lower_bound(x+1, x+n+1, right+1)-(x+1);
		ans -= std::max(0, rr-ll);
		z.erase(p);
		auto it3 = z.lower_bound(r);
		auto it4 = it3;
		it4--;
		left = std::max(*it4+l+1, r-l);
		right = std::min(*it3-l-1, r+l);
		ll = std::lower_bound(x+1, x+n+1, left)-(x+1);
		rr = std::lower_bound(x+1, x+n+1, right+1)-(x+1);
		ans += std::max(0, rr-ll);
		std::cout << ans << '\n';
		z.insert(r);
	}
}
```



## F Laundry

### 题意

几个人的衣服要晾晒。挂衣夹满足如下要求：

- 一只袜子用一个衣夹
- 一件T恤用三个衣夹
- 一个人的袜子用同一种颜色
- 一个人的T恤用同一种颜色
- 两个人的衣服不能是同一种颜色
- 需要的衣夹颜色最少

给出一大堆衣夹，算出谁应该用哪种衣夹。

### 题解

全扔进set里面贪心。每次从待洗的衣服里优先拿拆分后的最大的，然后拿没拆分过的最大的，每次从衣夹里取能取的最小的，取不到的话就拆分，拆不了就无解。



### 代码

```c++
#include <iostream>
#include <set>
#include <queue>

int n, k;
std::multiset<int> l;
std::priority_queue<std::pair<int, int>> d;
int t;

int main(int argc, char *argv[]) {  
	std::ios::sync_with_stdio(false);
	std::cin.tie(0); 
	std::cin >> n >> k;
	for (int i = 0; i < n; i++) {
		std::cin >> t;
		d.push(std::make_pair(0, t*5));
	}
	for (int i = 0; i < k; i++) {
		std::cin >> t;
		l.insert(t);
	}
	l.insert(6000000);
	int ans = 0;
	while (!d.empty()) {
		std::pair<int, int> x = d.top();
		d.pop();
		auto it = l.lower_bound(x.second);
//		std::cout << *it << ' ' << x.first << ' ' << x.second << '\n';
		if (*it == 6000000) {
			if (x.first == 0) {
				d.push(std::make_pair(1, x.second/5*3));
				d.push(std::make_pair(1, x.second/5*2));
			} else {
				std::cout << "NIE\n";
				exit(0);
			}
		} else {
			l.erase(it);
			ans++;
		}
	}
	std::cout << ans << '\n';
}
```



