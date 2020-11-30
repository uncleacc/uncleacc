---
title: SPFA
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/87.webp'
date: 2020-11-26 09:31:25
authorLink:
categories: 算法
tags: 算法
keywords:
description: 判负环的SPFA算法
---

> **SPFA算法**
>
> 判断图中是否存在负环，`负环：一个环的边权和为负数`
>
> 时间复杂度：SPFA总的期望时间复杂度为O(nlognlog(m/n) + m)，但是容易被卡成O(nm)

## 作用

1. 求含有负权边的最短路
2. 判负环

## 算法流程

求含有负权边的最短(长)路：

大致模板和迪杰斯特拉一样，不过SPFA每一个点可能不止进入队列一次，新开了inq数组判断当前松弛点是否在队列中，对于当前松弛的点，假如这个点不在队列里面就把它扔到队列里面让它再去更新其他点，如果在队列里面就不用管了，因为到这个点距离已经更新过了，下次它出队时回去更新其他点的

判负环：

对于一个没有负环的图，每一个点最多入队n-1次，也就是每一点最多被其他点更新n-1次，一个点一次(只有松弛时当前点不在队列里才入队)，用times数组记录松弛点当前被松弛了几次，一个没有负环的图中的点最多被除了这个点以外的所有点松弛也就是n-1次，所以当松弛次数==n时就说明存在负环

```c
bool spfa(int s){
	dis[s]=0;  //记得初始化起点
	q.push(s);
	inq[s]=1;
	while(!q.empty()){
		int fr=q.front(); q.pop();
		inq[fr]=0; //出队后记为0
		for(int i=head[fr];~i;i=e[i].next){
			int v=e[i].to, w=e[i].w;
			if(dis[fr]+w<dis[v]){  //可松弛
				dis[v]=dis[fr]+w;
				if(!inq[v]){  //不在队列就放到队列里
                    times[v]++;
					inq[v]=1;
					q.push(v);	
				} 
				if(times[v]>=n) return 1;  //有负环
			}
		}
	}
	return 0;
}
```

代码中`times[v]++`，一定要写到if里面，只有这个点不在队列里面才加加

看两道道例题

[HDU6201](http://acm.hdu.edu.cn/showproblem.php?pid=6201)

题目大意，n个城市，每个城市都有一个售货价格，从一个城市到另一个城市需要花费路费，问你从一个城市进货到另一个城市卖出最大利润是多少？这题可以把点权加到边权里面，两个点边权是c[b]-c[a]+w，表示从a进货在b卖出的利润，这样一来就变成了一个含有负权边的图求最长的路径，典型SPFA，刚开始把所有点都塞进去，可以虚拟出一个点，建立这个点到其他点边权为0的边

```c
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
const int MAXN=100100;
struct node{
	int to,w;
};
vector<node> vt[MAXN];
int c[MAXN],dis[MAXN];
bool inq[MAXN];
int n,t;
queue<int> q;
int spfa(int s){
	q.push(s);
	dis[s]=0;
	while(!q.empty()){
		int fr=q.front(); q.pop();
		inq[fr]=0;
		int sz=vt[fr].size();
		for(int i=0;i<sz;i++){
			int v=vt[fr][i].to, w=vt[fr][i].w;
			if(dis[v]<dis[fr]+w){
				dis[v]=dis[fr]+w;
				if(!inq[v]){
					inq[v]=1;
					q.push(v);
				}
			}
		}
	}
}
void init(){
	for(int i=0;i<=n;i++) vt[i].clear();
	memset(dis,-1,sizeof dis);
	memset(inq,0,sizeof inq);
}
int main()
{
	ios;
	cin>>t;
	while(t--){
		cin>>n;
		init();
		for(int i=1;i<=n;i++) cin>>c[i];
		for(int i=0;i<n-1;i++){
			int u,v,w;
			cin>>u>>v>>w;
			vt[u].push_back({v,c[v]-c[u]-w});
			vt[v].push_back({u,c[u]-c[v]-w});
		}
		for(int i=1;i<=n;i++) vt[0].push_back({i,0});  //放进去边权为0的边
		spfa(0);
		int mx=-1;
		for(int i=1;i<=n;i++) mx=max(mx,dis[i]);
		cout<<mx<<'\n';
	}
	return 0;
 } 
```

[洛谷P3385判负环](https://www.luogu.com.cn/problem/P3385) 

这道题开始我第一次AC用的是vector，后来改成链式前向星最后一个点就是过不去，后来检查了一下数据发现就是因为链式前向星是按照读边的顺序反着遍历的，这组数据正好卡了这个，正解就是把 times[v]++写到if里面，这样就防止了：1更新了2，过了一会3又把1更新成更小的，1又去更新2了，实际上应该先把1更新成最小的然后只更新2一次的，而修改后正好解决了这个问题

贴一下代码和数据

### 数据

```txt
1
5 9
1 5 -1
5 4 -1
1 4 -1
1 3 -2
1 2 -3
2 4 -5
2 3 -6
2 5 -1000
3 4 -4
```

### AC代码

只改了一个地方就是，把times[v]++写到了if里面！

```c
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
typedef long long ll;
const int INF=0x3f3f3f3f;
const int MAXN=2e3+100;
struct node{
	int to,w,next;
}e[MAXN*MAXN];
int head[MAXN],inq[MAXN],times[MAXN];
int dis[MAXN];
int tot,n,m;
void add(int u,int v,int w){
	e[tot].to=v;
	e[tot].next=head[u];
	e[tot].w=w;
	head[u]=tot++;
}
queue<int> q; 
bool spfa(int s){
	dis[s]=0;
	q.push(s);
	inq[s]=1;
	while(!q.empty()){
		int fr=q.front(); q.pop();
		inq[fr]=0;
		for(int i=head[fr];~i;i=e[i].next){
			int v=e[i].to, w=e[i].w;
			if(dis[fr]+w<dis[v]){
				dis[v]=dis[fr]+w;
				if(!inq[v]){
                    times[v]++;
					inq[v]=1;
					q.push(v);	
				} 
				if(times[v]>=n) return 1;
			}
		}
	}
	return 0;
}
void init(){
	tot=0;
	memset(times,0,sizeof times);
	memset(inq,0,sizeof inq);
	memset(head,-1,sizeof head);
	memset(dis,INF,sizeof dis);
	while(!q.empty()) q.pop();
}
int main()
{
	ios;
	int t;
	cin>>t;
	while(t--){
		init();
		cin>>n>>m;
		while(m--){
			int u,v,w;
			cin>>u>>v>>w;
			add(u,v,w);
			if(w>=0) add(v,u,w);
		}
		if(spfa(1)) cout<<"YES"<<'\n';
		else cout<<"NO"<<'\n';
	}
	return 0;
 } 
```

