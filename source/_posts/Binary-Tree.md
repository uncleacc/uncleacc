---
title: Binary Tree
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/88.webp'
date: 2020-11-30 22:11:18
authorLink:
categories: 算法
tags: 数据结构
keywords:
description: 二叉树数据结构实验
---

> 实验目标：
> 1、创建二叉树
> 2、用非递归算法先中后序遍历二叉树 (难点)
> 3、分别求出二叉树中度为 0、1、2的结点个数
> 4、求出树的高度

> 参考博客: 
> [Article_1](https://www.cnblogs.com/dolphin0520/archive/2011/08/25/2153720.html)
> [Article_2](https://www.cnblogs.com/rain-lei/p/3705680.html ) 

难点在于非递归遍历，用栈来模拟递归的过程

## 二叉树结构

```c
typedef struct node{
	char data;
	struct node *lchild, *rchild;
}BiTNode,*BiTree;
```

## 创建二叉树

```c
void CreateBiTree(BiTree &t){
	char ch; cin>>ch;
	if(ch=='#') t=NULL;
	else{
		t=new node;
		t->data=ch;
		CreateBiTree(t->lchild);  //创建左子树 
		CreateBiTree(t->rchild);  //创建右子树 
	}
}
```

## 层序遍历

```c
void Level(BiTree L){
	cout<<"层序遍历:\n";
	if(!L) return ;
	queue<BiTree> q;  //STL队列定义 
	q.push(L);
	while(!q.empty()){
		BiTree fr=q.front();
		q.pop();
		if(fr) cout<<fr->data<<' ';
		else continue;
		q.push(fr->lchild);
		q.push(fr->rchild);
	}
}
```

## 前序遍历

#### 递归

```c
void preOrder1(BinTree *root)     //递归前序遍历 
{
    if(root!=NULL)
    {
        cout<<root->data<<"";
        preOrder1(root->lchild);
        preOrder1(root->rchild);
    }
}
```

#### 非递归

`用栈模拟递归` 

对比递归算法，当递归调用自己时就`把当前状态入栈`!

操作： 对于当前子树，1.不空就输出根节点并把当前指针入栈，然后更新指针指向左子树，2. 空就更新指针指向栈顶的右子树

```c
void PreOreder(BiTree L){
	cout<<"前序遍历:\n";
	if(!L) return ;
	stack<BiTree> st;
	while(L || !st.empty()){
		while(L){
			cout<<L->data<<' ';
			st.push(L);
			L=L->lchild;
		}
		if(!st.empty()){
			L=st.top();
			st.pop();
			L=L->rchild;
		}
	}
}
```

## 中序遍历

#### 递归

```c
void inOrder1(BinTree *root)      //递归中序遍历
{
    if(root!=NULL)
    {
        inOrder1(root->lchild);
        cout<<root->data<<"";
        inOrder1(root->rchild);
    }
} 
```

#### 非递归

和前序一样，只不过改成在当前节点没有左子树时输出

```c
void MidOreder(BiTree L){
	cout<<"中序遍历:\n";
	stack<BiTree> st;
	while(L || !st.empty()){
		while(L){
			st.push(L);
			L = L->lchild;
		}
		if (!st.empty()){
			L = st.top();
			st.pop();
			cout<<L->data<<' ';
			L=L->rchild;
		}
	}
}
```

## 后序遍历

#### 递归

```c
void postOrder1(BinTree *root)    //递归后序遍历
{
    if(root!=NULL)
    {
        postOrder1(root->lchild);
        postOrder1(root->rchild);
        cout<<root->data<<"";
    }    
} 
```

#### 非递归

后序遍历的非递归实现是三种遍历方式中最难的一种。因为在后序遍历中，要保证左孩子和右孩子都已被访问并且左孩子在右孩子前访问才能访问根结点，这就为流程的控制带来了难题，解决方案如下。

要保证根结点在左孩子和右孩子访问之后才能访问，因此对于任一结点P，先将其入栈。如果P不存在左孩子和右孩子，则可以直接访问它；或者P存在左孩子或者右孩子，但是其左孩子和右孩子都已被访问过了，则同样可以直接访问该结点。若非上述两种情况，则将P的右孩子和左孩子依次入栈，这样就保证了每次取栈顶元素的时候，左孩子在右孩子前面被访问，左孩子和右孩子都在根结点前面被访问。

```c
/*
也就是模拟递归
对于当前节点，只有当左子树和右子树都访问过或者为空时才能输出当前节点，有一个存在且没有访问过就需要先访问它，而栈是先进后
出，所以入栈顺序先放右子树在放左子树
如何判断是否访问过？
我们可以保存上一个访问的节点pre，如果满足 (p->right==NULL && pre==p->left) || pre=p->right，那么显然p的孩子都访问
过了，接下来可以访问p 
*/ 
void PostOrder(BiTree L){
	cout<<"后序遍历:\n";
	if(!L) return ;
	stack<BiTree> st; 
	st.push(L);
	BiTree pre=L;
	while(!st.empty()){
		BiTree temp=st.top();
		if((!temp->lchild && !temp->rchild) || (!temp->rchild && temp->lchild==pre) || temp->rchild==pre){
			cout<<temp->data<<' ';
			pre=temp;
			st.pop();
		}
		else{
			if(temp->rchild) st.push(temp->rchild);
			if(temp->lchild) st.push(temp->lchild);
		}
	}
}
```

求度数简单这里不单独放代码了

## 全代码

```c
/*
实验目标：
1、创建二叉树
2、用非递归算法先中后序遍历二叉树
3、分别求出二叉树中度为 0、1、2的结点个数
4、求出树的高度
 
难点在于非递归遍历，用栈来模拟递归的过程，这里我用了STL里面的栈，只需要知道栈的操作干什么即可，不需要知道原理 

代码输入方式: 输入一颗树的先序序列，空节点用"#"代替 
参考博客: 
https://www.cnblogs.com/dolphin0520/archive/2011/08/25/2153720.html
https://www.cnblogs.com/rain-lei/p/3705680.html 
*/
#include <bits/stdc++.h>
using namespace std;
typedef struct node{
	char data;
	struct node *lchild, *rchild;
}BiTNode,*BiTree;
int cnt0,cnt1,cnt2,high;
//递归利用先序遍历方式创建二叉树，空节点用"#"代替 
void CreateBiTree(BiTree &t){
	char ch; cin>>ch;
	if(ch=='#') t=NULL;
	else{
		t=new node;
		t->data=ch;
		CreateBiTree(t->lchild);  //创建左子树 
		CreateBiTree(t->rchild);  //创建右子树 
	}
}
void find(BiTree t,int h){  //传入高度参数 
	if(!t) return ;
	high=max(high,h);  //以最大高度节点为准 
	if(t->lchild && t->rchild) cnt2++;  //度为2 
	else if(!t->lchild && !t->rchild) cnt0++;  //度为0 
	else cnt1++;  //度为1 
	find(t->lchild,h+1);  //左子树 
	find(t->rchild,h+1);  //右子树 
}
//层序遍历利用BFS输出 (实验不做要求)
void Level(BiTree L){
	cout<<"层序遍历:\n";
	if(!L) return ;
	queue<BiTree> q;  //STL队列定义 
	q.push(L);
	while(!q.empty()){
		BiTree fr=q.front();
		q.pop();
		if(fr) cout<<fr->data<<' ';
		else continue;
		q.push(fr->lchild);
		q.push(fr->rchild);
	}
}
/*
用栈模拟递归 
操作：
对于当前子树根节点，1.如果当前子树不为空就输出子树根节点并把左子树入栈，指针指向左子树，2.为空则pop出栈顶元素，并
将此时栈顶根节点的右子树入栈 
*/ 
void PreOreder(BiTree L){
	cout<<"前序遍历:\n";
	if(!L) return ;
	stack<BiTree> st;
	while(L || !st.empty()){
		while(L){
			cout<<L->data<<' ';
			st.push(L);
			L=L->lchild;
		}
		if(!st.empty()){
			L=st.top();
			st.pop();
			L=L->rchild;
		}
	}
}
/*
和先序类似，就是先处理完左子树再输出根节点然后把右子树入栈 
*/ 
void MidOreder(BiTree L){
	cout<<"中序遍历:\n";
	stack<BiTree> st;
	while(L || !st.empty()){
		while(L){
			st.push(L);
			L = L->lchild;
		}
		if (!st.empty()){
			L = st.top();
			st.pop();
			cout<<L->data<<' ';
			L=L->rchild;
		}
	}
}
/*
也就是模拟递归
对于当前节点，只有当左子树和右子树都访问过或者为空时才能输出当前节点，有一个存在且没有访问过就需要先访问它，而栈是先进后
出，所以入栈顺序先放右子树在放左子树
如何判断是否访问过？
我们可以保存上一个访问的节点pre，如果满足 (p->right==NULL && pre==p->left) || pre=p->right，那么显然p的孩子都访问
过了，接下来可以访问p 
*/ 
void PostOrder(BiTree L){
	cout<<"后序遍历:\n";
	if(!L) return ;
	stack<BiTree> st; 
	st.push(L);
	BiTree pre=L;
	while(!st.empty()){
		BiTree temp=st.top();
		if((!temp->lchild && !temp->rchild) || (!temp->rchild && temp->lchild==pre) || temp->rchild==pre){
			cout<<temp->data<<' ';
			pre=temp;
			st.pop();
		}
		else{
			if(temp->rchild) st.push(temp->rchild);
			if(temp->lchild) st.push(temp->lchild);
		}
	}
}
void Traverse(BiTree L){
	cout<<'\n'; 
	Level(L);  //层序遍历 
	cout<<'\n';
	PreOreder(L);  //前序遍历 
	cout<<'\n';
	MidOreder(L);  //中序遍历 
	cout<<'\n';
	PostOrder(L);  //后序遍历 
	cout<<'\n';
}
int main()
{
	BiTree L;
	CreateBiTree(L);  //创建二叉树 
	Traverse(L);  //遍历二叉树 
	find(L,1);  //找到二叉树度数为0、1、2的结点数，并得出深度 
	cout<<"树的高度为: "<<high<<'\n';
	cout<<"度数为0的节点数量: "<<cnt0<<'\n';
	cout<<"度数为1的节点数量: "<<cnt1<<'\n';
	cout<<"度数为2的节点数量: "<<cnt2<<'\n';
	return 0;
 } 
```

