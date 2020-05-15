---
title: codeforces div4
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink: www.fezhu.top
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-12 08:40:16
tags: 比赛
keywords:
description: 感觉这种思维题还行，对我这种不会算法的人就喜欢思维题
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/25.webp
---
<blockquote>
唯一一场每道题都有思路的比赛，感觉还行，虽然有思路不代表能AC，不过还是很开心的，因为除了E题的桶没想到外其他都是自力更生做出来的😊   
</blockquote>
{% fb_img  https://cdn.jsdelivr.net/gh/uncleacc/Sucai/8~1.webp  name  %}

## A Sum of Round Numbers
### 分析
签到题，就是遍历数的每一位，求出非0的位数有几位，然后int一个v=1，之后没走一个数v*=10，然后当一位数不等于0时就乘上v就行了，这道题用字符串应该更简单，但是我想试试用while，练练手
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4+100;
int main()
{
	ios;
	int t,k;
	cin>>t;
	while(t--){
		cin>>k;
		int cnt=0,x=k,v=1;
		while(k/10){
			int tt=k%10;
			if(tt!=0) cnt++;
			k/=10;
		}
		cout<<cnt+1<<endl;
		while(x/10){
			int tt=x%10;
			if(x/10&&tt!=0) cout<<tt*v<<" ";
			x/=10; v*=10;
		}
		cout<<(x%10)*v<<endl;
	}
	return 0;
}
```
## B - Same Parity Summands
### 分析
这道题是给你一个a和b，让你用b个同为偶数或者奇数的数加起来等于a，输出这些数，刚开始我一直在找规律，感觉很麻烦，找了半小时也没涵盖所有情况，后来发现直接暴力枚举就行了，我们可以考虑极端，当都为奇数时，让除了最后一个数以外的数都是1，然后最后一个数=a-(b-1)，加起来正好等于a，同理都为偶数时，让除了最后一个数以外的数全部变成2，最后一个数为n-2*(k-1)，条件都不符合输出NO~~想通这个就AC了~~
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4+100;
int main()
{
	ios;
	int t,k,n;
	cin>>t;
	while(t--){
		cin>>n>>k;
		int m=n-(k-1);
		if(m>0&&m%2){
			cout<<"YES"<<endl;
			for(int i=0;i<k-1;i++){
				cout<<1<<" ";
			}
			cout<<m<<endl;
			continue;
		}
		m=n-2*(k-1);
		if(m>0&&m%2==0){
			cout<<"YES"<<endl;
			for(int i=0;i<k-1;i++){
				cout<<2<<" ";
			}
			cout<<m<<endl;
			continue;
		}
		cout<<"NO"<<endl;
		
		
	}
	return 0;
}
```
## C - K-th Not Divisible by n
### 分析
看题目就知道大意，不被n整除的第K个数，首先我们清楚被N整除的数之间的数数量一定是相同的，比如能整除8的：8 16 24...数之间都相差8，那这就是一个找规律嘛，用“%”找到该数在一段区间的那个位置，然后用“/”找到在第几个区间就行了
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4+100;
int main()
{
	ios;
	int t,k,n;
	cin>>t;
	while(t--){
		int a,b;
		cin>>a>>b;
		int k=b%(a-1);
		if(k==0){
			cout<<(b/(a-1)*a)-1<<endl;
		}
		else cout<<(b/(a-1)*a)+k<<endl;
	}
	return 0;
}
```
## D. Alice, Bob and Candies
### 分析
这道题好长啊，我第一眼就被吓住了，~~{% span red, 不战而屈人之兵 %}~~，不过狠下心来读一读，发现就是一个双向指针往中间合拢，~~好像这叫双向队列~~，再多定义几个变量记录每次每个人吃多少，每个人上次吃了多少，就是一个模拟，只要能写对条件，就能AC了
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4;
int val[MAXN];
int main()
{
	ios;
	int t; cin>>t;
	while(t--){
		int n,pa=0,pb=0,flg=0;; cin>>n;
		int sum=0;
		for(int i=1;i<=n;i++){
			cin>>val[i];
			sum+=val[i];
		}
		int ea=0,eb=0,cnt=1,sa,sb,head=1,tail=n;
		while(tail-head>=0){
			sa=0; sb=0;
			if(flg==0&&sum-ea-eb>pb){
				while(sa<=pb){
					sa+=val[head];
					ea+=val[head];
					head++; 
				}
				pa=sa; flg=1; cnt++;
				continue;
			}
			if(flg==1&&sum-ea-eb>pa){
				while(sb<=pa){
					sb+=val[tail];
					eb+=val[tail];
					tail--;
				}
				pb=sb; flg=0; cnt++;
				continue;
			}
			cnt++;
			if(flg==0){
				ea+=sum-ea-eb;
				break;
			}else{
				eb+=sum-ea-eb;
				break;
			}
		}
		cout<<cnt-1<<" "<<ea<<" "<<eb<<" "<<endl;
	} 
	return 0;
}
```
## E. Special Elements
### 分析
最好理解的一道题目🌝也是我唯一一道看了题解的题目😂一个前缀和，这道题数据量很小，只有8000，直接装进桶里就行了，而我就是没想到，一直在纠结怎么降低复杂度（好菜啊）求出每一个区间的和看看这个和对应的桶编号里面装没装数，~~其实是道水题~~
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4;
int val[MAXN],book[MAXN];
int main()
{
	ios;
	int t; cin>>t;
	while(t--){
		memset(book,0,sizeof book);
		memset(val,0,sizeof val);
		int n; cin>>n;
		for(int i=1;i<=n;i++){
			cin>>val[i];
			book[val[i]]++;
			val[i]+=val[i-1];
		}
		int cnt=0;
		for(int i=0;i<n-1;i++){
			for(int j=i+2;j<=n;j++){
				int sum=val[j]-val[i];
//				cout<<sum<<' '<<book[sum]<<endl;
				if(sum<=n&&book[sum]){
					cnt+=book[sum]; 					
					book[sum]=0;
				}
			}
		}
		cout<<cnt<<endl;
	} 
	return 0;
}
```
## F - Binary String Reconstruction
### 分析
又是思维题，给你一个n0,n1,n2让你构造一个二进制序列，这个序列子序列中00的个数为n0，01或10的个数为n1，11的个数为n2，首先n0和n2的序列容易构造，因为只包含一个数字，n1我们可以定义顺序为1010...n1=1:10 n1=2:101 n1=3:1010，每次增加一个数，单独构造容易，合起来难，三个序列合起来中间肯定会多出一些，所以只要在本上演算一下，再把多出的那一部分去掉就行了，然后我构造的序列顺序是n0n2n1，感觉这样构造简单🐶说实话我交代码时都没敢想能AC，因为感觉没考虑全，但是测试几组数据发现能过就抱着试一试的态度交上去了，结果A了:smile:
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4;
int val[MAXN],book[MAXN];
int main()
{
	ios;
	int t; cin>>t;
	while(t--){
		int n0,n1,n2;
		string ans;
		cin>>n0>>n1>>n2;
		for(int i=1;i<=n0+1;i++){
			if(n1!=0||n1==0&&n2==0) ans+="0";
		}
		for(int i=1;i<=n2;i++){
			ans+="1";
		}
		for(int i=1;i<=n1;i++){
			if(i%2) ans+="1";
			else ans+="0";
		}
		if(n0==0&&n1==0) ans+="1";
		cout<<ans<<endl;
	} 
	return 0;
}
```
## G - Special Permutation
### 分析
说实话我不知道这道题为啥放在最后，~~感觉难度并不大，反而挺简单的~~🌚就是让你构造一个相邻两个数之差的绝对值在2到4的区间内（闭区间），我们只要把奇偶分开就行了，但是我有一个疑问，我的思路是把偶数写在前面奇数的倒序列放在后面，例如：  
8：2 4 6 8 7 5 3 1   
很明显只有中间不符合条件，所以只动中间就行了，让7和5（奇数序列的前两个）交换：  
8：2 4 6 8 5 7 3 1（满足条件）  
再举个例子：  
9：2 4 6 8 9 7 5 3 1  
还是只动中间，因为9比8（奇数第一个和偶数最后一个）大所以让5跑到9中间（奇数第3个移动到奇数和偶数序列之间）：  
9：2 4 6 8 5 9 7 3 1（满足条件）  
这样的思路我仔细想了想感觉没毛病，就交了，结果wrong了？我不服气，发现中间可能是换行的问题，又交又wrong...    
{% fb_img  https://cdn.jsdelivr.net/gh/uncleacc/Sucai/5~1.webp name %}    
我枯了，为啥？  
后来换思路：  
给2 4 1 3的序列两边添加数，先左后右轮换添加，例如：   
1. 8：2 4 1 3  
2. 8：5 2 4 1 3  
3. 8：5 2 4 1 3 6  
4. 8：7 5 2 4 1 3 6  
5. 8：7 5 2 4 1 3 6 8  
完成~，交了AC了。。AC后看了测试数据发现我原来的代码测试数据1应该是对的啊！希望网友那位可以为我解惑（不胜感激）  
{% fb_img  https://cdn.jsdelivr.net/gh/uncleacc/Sucai/7~1.webp name  %}   
{% fb_img  https://cdn.jsdelivr.net/gh/uncleacc/Sucai/6~1.webp name  %}
### wrong CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4;
void swap(int &a,int &b){
	int t=a;
	a=b;
	b=t;
}
int val[MAXN],book[MAXN];
int main()
{
	ios;
	int t; cin>>t;
	while(t--){
		memset(val,0,sizeof val); 
		int n; cin>>n;
		if(n==2||n==3){
			cout<<-1<<endl;
			continue;
		}
		int tail=0;
		for(int i=2;i<=n;i+=2){
			val[++tail]=i;
		}
		int flag=tail;
		int p=n%2?n:n-1;
		for(int i=p;i>=1;i-=2){
			val[++tail]=i;
		}
		if(val[flag+1]<val[flag]) swap(val[flag+1],val[flag+2]);
		else{
			for(int i=1;i<=n;i++){
				if(i==flag+1){
					printf("%d%c",val[flag+3],i==n?'\n':' ');
				}
				if(i==flag+3){
					continue;
				}
				printf("%d%c",val[i],i==n?'\n':' ');
			}
			if(n==5) cout<<endl;
			continue;
		}
		for(int i=1;i<=n;i++) printf("%d%c",val[i],i==n?'\n':' ');
	} 
	return 0;
}
```
### AC CODE
```
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<iostream>
using namespace std;
string ans;
void bd(int n){
	if(n<=9){
		ans=(char)(n+'0')+ans;
	}else{
		while(n!=0){
			char c=(char)(n%10+'0');
			ans=c+ans;
			n/=10;
		}
	}
}
void pd(int n){
		if(n<=9){
		ans+=(char)(n+'0');
	}else{
		char ch[2000];
		int tail=0;
		while(n!=0){
			char c=(char)(n%10+'0');
			ch[++tail]=c;
			n/=10;
		}
		for(int i=tail;i>=1;i--){
			ans+=ch[i];
		}
	}
}
int main()
{
	int t; cin>>t;
	while(t--){
		ans.clear();
		int n; cin>>n;
		if(n<=3){
			puts("-1");
			continue;
		}
		ans+="2 4 1 3";
		int flag=1;
		for(int i=5;i<=n;i++){
			if(i%2){
				ans=" "+ans;
				bd(i);
			}
			else{
				ans+=" ";
				pd(i);
			}
		}
		cout<<ans<<endl;
}
```
Ending~撒花
{% p center logo large, 不要白嫖了，留下一个赞吧👍 %}