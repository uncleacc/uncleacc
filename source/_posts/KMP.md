---
title: KMP
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/80.webp'
date: 2020-11-05 21:58:19
authorLink:
categories: 算法
tags: KMP
keywords:
description: KMP算法
---

> 暑假学习了KMP，奈何掌握不深，现在又来复习，结果又是从零开始

## 什么是KMP？

现在有一个原字符串，再给你一段模式串，问你在原字符串中是否存在一段子串等于模式串，或者模式串在原串中出现几次？

BF算法，也就是人人都会的指针回朔暴力算法，略过

原串： ABABABAABA  （i）

模式串： ABAA （j）

当匹配时第一个失配的位置是3(下标从0开始)，然后朴素做法是把i和j指针都回朔，但其实可以利用之前已经匹配的信息的，可以找到当前失配字符之前的最大公共前后缀长度，假设长度为k，则s[i-k]...s[i-1]==t[j-k]...t[j-1]，而t[0]...t[k-1]==t[j-k]...t[j-1]，所以s[i-k]..s[i-1]==t[0]...t[k-1]，所以只需要把j移到k位置就可以了，i指针不回朔，这样一来就只要j指针回朔，而且大概率没有回朔到0，省去大量时间，那么问题就来了，怎么找到模式串中每一个位置的k呢？

前面已经说了，k是每一个位置之前字符串(不包括k位置)的最长公共前后缀长度，而公共前后缀与原串无关，只是在模式串中求即可

## 求解NEXT

用next[i]表示i位置之前字符串的最长公共前后缀，所以求解next数组其实就是求模式串每一个前缀子串的最大公共前后缀！

ABABABAB

这段字符串的next数组就是：-1 0 0 1 2 3 4 5

先来看代码吧

```c
void get_next(int len){
	int i=0, j=-1;  //这里j是初始化为-1！
    ne[0]=-1;
	while(i<len){
		if(j==-1 || ch[i]==ch[j]){
			++i; ++j;
			ne[i]=j;
		}
		else j=ne[j];
	}
}
```

首先明白next[i]表示的是t[0]...t[i-1]的最大公共前后缀，是不包括t[i]的！！！

因此next[0]就初始化为-1，然后next[1]一定是0，因为就有一个字符，首先当两个位置字符相同时很好处理，next[i]=j+1，但是当位置字符不一样时为什么j=ne[j]呢？想一下，求next数组本质就是自己和自己匹配嘛(也就是模式串中抽象出来一个模式串)，当两个位置字符失配时，只需要把j移到之前的最大公共前后缀位置就可以了，而最大位置就是next[j]

明白了这个next就求出来了

<font color="red" size=5>需要说明的是，next数组真正有效的部分是从1到n的，也就是原串从0开始，而next数组往后移了一位</font>

## 求解KMP

接下来就好办了，和求next差不多的

```c
int kmp(int len1, int len2){
	int i=0, j=0, cnt=0;
	while(i<len2){
		if(j==-1 || ch1[j]==ch2[i]){
			++i; ++j;
		}
		else j=ne[j];
		if(j==len1) cnt++;  // 匹配到
	}
	return cnt;
}
```

OK，到这里就讲完了

## 全代码

模板题：[ **KMP算法**](https://vjudge.net/contest/388842#problem/H)

```c
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
const int MAXN=1e6+100;
int ne[MAXN];
char ch1[MAXN],ch2[MAXN];
void get_next(int len){
	int i=0, j=-1;
	ne[0]=-1;
	while(i<len){
		if(j==-1 || ch1[i]==ch1[j]){
			++i; ++j;
			ne[i]=j;
		}
		else j=ne[j];
	}
	return ;
}
int kmp(int len1, int len2){
	int i=0, j=0, cnt=0;
	while(i<len2){
		if(j==-1 || ch1[j]==ch2[i]){
			++i; ++j;
		}
		else j=ne[j];
		if(j==len1) cnt++;
	}
	return cnt;
}
int main()
{
	ios;
	int t;
	cin>>t;
	while(t--){
		cin>>ch1>>ch2;
		int len1=strlen(ch1), len2=strlen(ch2);
		get_next(len1);
		int cnt=kmp(len1, len2);
		cout<<cnt<<'\n';
	}
	return 0;
 } 
```

## 补充

next数组并不是最优的，当一个模式串所有字符都相同，比如T=(aaaaaaaa)，那么求解出来的next数组表示的是当前位置之前字符串的最大前后缀，因为字符都相同，那么所有的i，ne[i]=i-1，但实际上所有字符都是相同的，当前字符失配的话那就可以直接把指针回朔到最前面了，也就是所有的nextval[i]=ne[0]=-1，这样进行下次循环时原串指针直接后移且模式串指针回朔到第一个位置，省去了一个一个位置回朔的时间

## 求解nextval数组

当前位置的next数组值为下标的前一个位置的字符如果和当前字符一样，则当前位置的nextval值就是前面位置的nextval值，否则就是当前位置的next数组值，初始化第一个位置的nextval值为-1

```c
void get_nextval(int len){
	int i=0, j=-1;
	nextval[0]=-1;
	while(i<len){
		if(j==-1 || ch1[i]==ch1[j]){
			++i; ++j;
            // 和求解next数组唯一不一样的地方
			if(ch1[i]!=ch1[j]) nextval[i]=j;  
			else nextval[i]=nextval[j]; //当前位置和next[i]的字符相同时，则不需要回朔到next[i]位置，因为这个位置的字符一定会失配，所以让nextval[i]直接指向nextval[next[i]]，当没有跳步回朔操作时next和nextval值是一样的
		}
		else j=nextval[j];
	}
}
```

也可以先通过求解出next数组，然后求解出nextval数组