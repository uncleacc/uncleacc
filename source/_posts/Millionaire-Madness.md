---
title: Millionaire Madness
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/78.webp'
date: 2020-09-11 15:26:43
authorLink:
categories: 题目
tags: BFS
keywords:
description: 优先队列加BFS
---



> 计蒜客之前比赛的一道题目，记录一下

## G Millionaire Madness

> A close friend of yours, a duck with financial problems, has requested your help with a matter that will help him pay off his debts. He is the nephew of an extremely wealthy duck, who has a large vault, filled with mountains of coins. This wealthy duck has a certain coin in his possession which has a lot of sentimental value to him. Usually, it is kept under a protective glass dome on a velvet cushion.However, during a recent relocating of the coins in the vault, the special coin was accidentally moved into the vault, leading to an extremely stressful situation for your friend’s uncle. Luckily, the coin has recently been located. Unfortunately, it is completely opposite to the entrance to the vault, and due to the mountains of coins inside the vault, actually reaching the coin is no simple task.He is therefore willing to pay your friend to retrieve this coin, provided that he brings his own equipment to scale the mountains of coins. Your friend has decided he will bring a ladder, but he is still uncertain about its length. While a longer ladder means that he can scale higher cliffs, it also costs more money. He therefore wants to buy the shortest ladder such that he can reach the special coin, so that he has the largest amount of money left to pay off his debts.The vault can be represented as a rectangular grid of stacks of coins of various heights (in meters), with the entrance at the north west corner (the first height in the input, the entrance to the vault is at this height as well) and the special coin at the south east corner (the last height in the input). Your avian companion has figured out the height of the coins in each of these squares. From a stack of coins he can attempt to climb up or jump down to the stack immediately north, west, south or east of it. Because your friend cannot jump or fly (he is a very special kind of duck that even wears clothes), successfully performing a climb of n*n* meters will require him to bring a ladder of at least n*n* meters. He does not mind jumping down, no matter the height; he just lets gravity do all the work. 
>
> **Input**
>
> The first line contains two integers: the length M*M*, and the width N*N* of the vault, satisfying 1 \leq M,N \leq 10001≤*M*,*N*≤1000.The following M*M* lines each contain N*N* integers. Each integer specifies the height of the pile of coins in the vault at the corresponding position. (The first line describes the north-most stacks from west to east; the last line describes the south-most stacks from west to east). The heights are given in meters and all heights are at least 00 and at most 10^9109 (yes, your friend’s uncle is very rich). 
>
> **Output**
>
> Output a single line containing a single integer: the length in meters of the shortest ladder that allows you to get from the north west corner to the south east corner.
>
> #### 样例输入1
>
> ```
> 3 3
> 1 2 3
> 6 5 4
> 7 8 9
> ```
>
> #### 样例输出1
>
> ```
> 1
> ```
>
> #### 样例输入2
>
> ```
> 1 4
> 4 3 2 1
> ```
>
> #### 样例输出2
>
> ```
> 0
> ```
>
> #### 样例输入3
>
> ```
> 7 5
> 10 11 12 13 14
> 11 20 16 17 16
> 12 10 18 21 24
> 14 10 14 14 22
> 16 18 20 20 25
> 25 24 22 10 25
> 26 27 28 21 25
> ```
>
> #### 样例输出3
>
> ```
> 3
> ```

### 大意

给定一个矩阵，从第一个点到最后一个点需要的最短梯子长度，从一个数到一个比它大的数需要一个两数之差的梯子长度，需要注意这个梯子可以反复使用，其实就是求从起点到终点的一条路径，这条路径的任意两个相邻数最大的差值最小，问这个差值是多少？

### 想法

既然要求从起点到终点最短梯子长度，那可以这样想，求从起点到离终点只有一步的那个点到终点需要的梯子长度和从起点到终点所需的最短梯子长度取大的，同理继续往前推可以知道每一次递推都是由之前的那个点到起点所需最短梯子长度推过来的，因此每次都是求这个点到下一个点的最小梯子长度，如何求最短呢？肯定想到优先队列，而一层一层的往外推则是bfs，因此这道题就出来了

```c
#include <bits/stdc++.h> 
#define ios ios::sync_with_stdio(0)
using namespace std;
struct node{
    int x,y,v;
    bool operator<(const node & o)const{ //v代表每一个点到起点需要的梯子数量
        return v>o.v;
    }
};
priority_queue<node> p;
bool vis[1006][1006];
int arr[1006][1006],dir[4][2]={1,0,-1,0,0,1,0,-1},n,m;
int bfs(){
    int num=0;
    while(!p.empty()){
        node d=p.top();
        p.pop();
        int x=d.x,y=d.y,v=d.v;
        if(x==n&&y==m) return v; //到达了终点
        if(vis[x][y]) continue;
        vis[x][y]=1; //记得标记
        for(int i=0;i<4;++i){
            int x1=x+dir[i][0],y1=y+dir[i][1];
            if(x1<=0||x1>n||y1<=0||y1>m) continue;
            int tmp;
            if(arr[x1][y1]-arr[x][y]<0) tmp=0; //这是刚开始如果下一个点比这个点小不能用负数，得用0
            else tmp=arr[x1][y1]-arr[x][y]; 这段需要的梯子长度
            int v1=max(v,tmp); //取他们较大的那个
            p.push({x1,y1,v1}); //放进队列
        }
    }
}
int main()
{
    ios;
    cin>>n>>m;
    for(int i=1;i<=n;++i){
        for(int j=1;j<=m;++j){
            cin>>arr[i][j];
        }
    }
    p.push({1,1,0});
    cout<<bfs()<<'\n';
}
```

