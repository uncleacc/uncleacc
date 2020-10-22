---
title: LCA最小公共祖先
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/81.webp'
date: 2020-09-29 21:44:21
authorLink:
categories: 算法
tags: 算法
keywords: 
description: 最小公共祖先问题
---

## LCA 公共祖先

什么是最小公共祖先，顾名思义就是俩点最近的公共祖先

{% fb_img https://cdn.luogu.com.cn/upload/pic/2282.png %}

如图所示：

1. 2和5的最小公共祖先就是4
2. 2和1的最小公共祖先就是4
3. 3和5的最小公共祖先是1

那么怎么求呢？

先介绍两种朴素的做法，~~也就是超时的做法🐷~~

第一种： `向上标记法`

想求两个点的最小公共祖先可以先从其中一个点往上找父亲结点，直到根节点，把路径标记一下，然后从另一个点开始做同样的操作，当遇到已经标记过的点的时候就停下来，这个点一定是最小公共祖先（ 每次查询时间复杂度：O(n) ）

### CODE

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN=500100;
vector<int> vt[MAXN];
int fa[MAXN];
bool vis[MAXN];
void dfs(int u){
	int len=vt[u].size();
	for(int i=0;i<len;i++){
		int v=vt[u][i];
		if(v==fa[u]) continue;
		fa[v]=u;
		dfs(v);
	}
}
int lca(int l,int r){
	memset(vis,0,sizeof vis);
	while(l){
		vis[l]=1;
		l=fa[l];
	}
	while(!vis[r]) r=fa[r];
	return r;
}
int main()
{
	ios::sync_with_stdio(0);
	int n,m,s;
	cin>>n>>m>>s;
	for(int i=1;i<=n-1;i++){
		int x,y;
		cin>>x>>y;
		vt[x].push_back(y);
		vt[y].push_back(x);
	}
	dfs(s); //找到每一个点的父亲结点是谁
	while(m--){
		int l,r;
		cin>>l>>r;
		cout<<lca(l,r)<<'\n';
	}
	return 0;
 } 
```

第二种： `利用深度法`

在上面的dfs函数稍微改一下，得到每一个点到根节点的深度（从0开始），当询问两个点的lca时，我们先把深度大的那个点网上搜，直到两个点的深度相同，深度相同后，两个点一起往上搜直到两个点合并到一起，那么这个点就是lca

### CODE

```c
#include <bits/stdc++.h>
using namespace std;
const int MAXN=500100;
vector<int> vt[MAXN];
int fa[MAXN],dep[MAXN];
void dfs(int u,int d){
	dep[u]=d; //处理出每一个点的深度
	int len=vt[u].size();
	for(int i=0;i<len;i++){
		int v=vt[u][i];
		if(v==fa[u]) continue;
		fa[v]=u;
		dfs(v,d+1); //子节点深度加一
	}
}
int lca(int l,int r){
	if(dep[l]<dep[r]) swap(l,r); //保证l是深度大的那个点
	while(dep[l]>dep[r]) l=fa[l]; //从深度大的那个开始往上走
	while(l!=r){ //一起往上
		l=fa[l];
		r=fa[r];
	}
	return r;
}
int main()
{
	ios::sync_with_stdio(0);
	int n,m,s;
	cin>>n>>m>>s;
	for(int i=1;i<=n-1;i++){
		int x,y;
		cin>>x>>y;
		vt[x].push_back(y);
		vt[y].push_back(x);
	}
	dfs(s,0);
	while(m--){
		int l,r;
		cin>>l>>r;
		cout<<lca(l,r)<<'\n';
	}
	return 0;
 } 
```

