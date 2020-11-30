---
title: 倍增与tarjan求解lca
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/86.webp'
date: 2020-11-23 21:50:44
authorLink:
categories: 算法
tags: tarjan
keywords: 
description: 谈谈我对倍增和tarjan算法的理解
---

> **倍增**
>
> 以倍增方式向上跳，时间复杂度是O(q*logn)
>
> **tarjan**
>
> 树上算法，实现过程通过dfs+并查集来离线求出lca(最近公共祖先)，时间复杂度O(n+q)，n是结点数，q是查询数

## 算法实现过程

**倍增算法流程:**

1. 用一个dfs得出每一个点的父亲节点还有它的深度，用数组保存起来，其中保存父亲的数组用dp[i]\[j]表示，意义是i节点向上跳2^j^步后到达的节点，父亲节点保存在dp[i]\[0]中

   ```C
   void dfs(int u,int fa,int d){  //得到每一个点的深度和父亲节点
   	dp[u][0]=fa;
   	dep[u]=d;
   	for(int i=head[u];~i;i=e[i].next){
   		int v=e[i].to;
   		if(v!=fa) dfs(v,u,d+1);
   	}
   }
   ```

2. 然后倍增预处理出每一个节点向上跳2^i^步到的的节点

   ```c
   void bz(){ //预处理
   	for(int i=1;i<=22;i++){
   		for(int u=1;u<=n;u++){
   			dp[u][i]=dp[dp[u][i-1]][i-1];
   		}
   	}
   }
   ```

3. 求两个点的lca时，先让深度小的跳到两个点深度相同的位置，如果跳后两个点重合则这个位置就是lca，否则两个点一起往上跳，直到lca的儿子节点

   ```c++
   int lca(int u,int v){
   	if(dep[u]<dep[v]) swap(u,v);
   	for(int i=log2(dep[u]-dep[v]);i>=0;i--){  //跳到相同深度
   		if((1<<i)<=dep[u]-dep[v]) u=dp[u][i]; //注意dep[x]-dep[y]时刻在变化，也正是因为这个所以dep[x]一定最后和dep[y]相等
   	}
   	if(u==v) return u;  //节点重合即lca
   	for(int i=log2(dep[u]);i>=0;i--){  //一起往上跳
   		if(dp[u][i]!=dp[v][i]){  //保证不会跳过lca，但同样的也不能跳到lca了，回跳到lca的儿子结点
   			u=dp[u][i];
   			v=dp[v][i];
   		}
   	}
   	return dp[u][0];  //父亲节点即为lca 
   }
   ```

**tarjan算法流程：**

1. 保存用两个图去存树和查询关系图

2. 对这棵树进行dfs搜索，从根开始，搜索到一个点把这个点标记，直到把当前结点的`所有子树都被标记并和它们的父亲结点合并`后，再查询哪些结点和当前结点有查询关系，对于这些结点如果已经被标记过了，那么这个节点的祖先就是这两个点的最近公共祖先(这里是难点)

   ```c
   void tarjan(int u){
   	vis[u]=1;
   	for(int i=head[u];~i;i=e[i].next){
   		int v=e[i].to;
   		if(vis[v]) continue;
   		tarjan(v);  //到这里u结点还没有向上合并
   		fa[v]=u;  //合并下一个结点和当前结点
   	}
   }
   ```

   

3. 当得出两个点的Lca后储存答案到lca数组中，因为查询关系图是无向图，不知道dfs搜索时顺序如何，需要给每一条查询关系图的边编个号，把lca答案储存到偶数或者奇数下标内

   ```c
   int ans=find(v);
   if(ve[u][i].id%2) lca[ve[u][i].id+1]=ans;
   else lca[ve[u][i].id]=ans;
   ```

为什么和当前点有查询关系的那个点被标记后那个点的祖先就是最近公共祖先呢？

无非就两种情况，用u表示当前结点，v表示另一个点，因为v被标记过，说明v一定在u之前被访问过，那么v要不就是在u的子树中，这种情况v的祖先就是u，`因为u还没有向上合并(u以下的所有子树都已经合并完成了)`，要不就是不和u在一个分支里，那么dfs一定是经过u和v的lca结点的，这时两个点的路径连线就是一个角，角的顶点就是lca

## 例题

[链接](https://www.luogu.com.cn/problem/P3379)

### tarjan

```c
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
const int MAXN=500100;
struct node{
	int to,next;
}e[MAXN<<1];  //无向边记得开两倍空间
struct xxx{  //关系图的编号要储存下来
	int to,id;
};
vector<xxx> ve[MAXN];  //存储关系图
bool vis[MAXN];
int fa[MAXN],head[MAXN],lca[MAXN*2];  //两倍查询关系无向图
int tot;
int find(int x){
	if(x==fa[x]) return x;
	return fa[x]=find(fa[x]);
}
void add(int u,int v){
	e[tot].to=v;
	e[tot].next=head[u];
	head[u]=tot++;
}
void tarjan(int u){
	vis[u]=1;
	for(int i=head[u];~i;i=e[i].next){
		int v=e[i].to;
		if(vis[v]) continue;
		tarjan(v);
		fa[v]=u;
	}
	int sz=ve[u].size();
	for(int i=0;i<sz;i++){
		int v=ve[u][i].to;
		if(vis[v]){
			int ans=find(v);
			if(ve[u][i].id%2) lca[ve[u][i].id+1]=ans;
			else lca[ve[u][i].id]=ans;
		}
	}
}
void init(int n){
	tot=0;
	for(int i=0;i<=n;i++){
		fa[i]=i;
		head[i]=-1;
	}
}
int main()
{
	int n,m,s;
	cin>>n>>m>>s;
	init(n);
	for(int i=0;i<n-1;i++){
		int u,v;
		cin>>u>>v;
		add(u,v);
		add(v,u);
	}
	for(int i=1;i<=m;i++){
		int u,v;
		cin>>u>>v;
		ve[u].push_back({v,i*2-1});  //注意这里放进去这条边的编号
		ve[v].push_back({u,i*2});
	}
	tarjan(s);
	for(int i=1;i<=m;i++){  //输出偶数下标的lca数组
		cout<<lca[i*2]<<'\n';
	}
	return 0;
 } 
```

### 倍增

```c
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
const int MAXN=500500;
struct node{
	int to,next;
}e[MAXN<<1];
int head[MAXN],dep[MAXN],dp[MAXN][25];
int tot,n,m,s;
void add(int u,int v){
	e[tot].to=v;
	e[tot].next=head[u];
	head[u]=tot++;
}
void dfs(int u,int fa,int d){  //得到每一个点的深度和父亲节点
	dp[u][0]=fa;
	dep[u]=d;
	for(int i=head[u];~i;i=e[i].next){
		int v=e[i].to;
		if(v!=fa) dfs(v,u,d+1);
	}
}
void bz(){ //预处理
	for(int i=1;i<=22;i++){
		for(int u=1;u<=n;u++){
			dp[u][i]=dp[dp[u][i-1]][i-1];
		}
	}
}
int lca(int u,int v){
	if(dep[u]<dep[v]) swap(u,v);
	for(int i=log2(dep[u]-dep[v]);i>=0;i--){  //跳到相同深度
		if((1<<i)<=dep[u]-dep[v]) u=dp[u][i]; //注意dep[x]-dep[y]时刻在变化，也正是因为这个所以dep[x]一定最后和dep[y]相等
	}
	if(u==v) return u;  //节点重合即lca
	for(int i=log2(dep[u]);i>=0;i--){  //一起往上跳
		if(dp[u][i]!=dp[v][i]){  //保证不会跳过lca，但同样的也不能跳到lca了，回跳到lca的儿子结点
			u=dp[u][i];
			v=dp[v][i];
		}
	}
	return dp[u][0];  //父亲节点即为lca 
}
int main()
{
	ios;
	cin>>n>>m>>s;
	for(int i=0;i<=n;i++) head[i]=-1;
	for(int i=0;i<n-1;i++){
		int u,v;
		cin>>u>>v;
		add(u,v);
		add(v,u);
	}
	dfs(s,0,0);
	bz();
	while(m--){
		int u,v;
		cin>>u>>v;
		cout<<lca(u,v)<<'\n';
	}
	return 0;
}
```

