---
title: 【NOIP2017】 奶酪
date: 2018-04-26 22:17:05
tags: [题解,搜索,bfs,dfs]
categories:
- OI   
---

Day2T1，普及/提高-
[题目链接](https://www.luogu.org/problemnew/show/P3958)

>写在前面：
>Day2的我花了半个小时想并查集算法，并没有注意到$n\leq1000$，于是乎天真的我就果断放弃转向两道noi-的题目，看完题面后便彻底放弃了自己。
>
>……（不可描述）
>
>距离结束还有40分钟，回到T1，哇，这不就是一个dfs吗？每一个洞只要遍历一次欸。
>
>开始打程序……
>
><!--more-->
>
>在距离结束还剩20分钟，打完把样例2带入发现完全正确，突然想上厕所。
>
>……
>
>回来发现电脑被锁定，打开电脑一看发现vim被强制退出。突然头皮发凉。
>
>打开工作目录，woc竟然没有保存！退回到了上一个历史版本。我的内心……无法描述。
>
>抓紧最后5分钟重来了一遍，结果"Yes"写成了"YES"，"No"写成了"NO"。





# 核心思路

> 用一个结构体point封装点。
>
> 从1到n找到所有的与下底面相连的洞，即abs(a[i].z)<=r。
>
> 从它开始dfs一步一步向与它所连的洞dist(a[i],a[x])>2*r搜索。
>
> 如果到达一个洞可以到达上表面，即a[x].z+r>=h，就找到答案了。
>
> 如果全部遍历了一遍都还没找到就可以确定不能到达了。



坐标距离公式，记得要`double`。

```cpp
double dist(point a,point b){
	return sqrt(pow(a.x-b.x,2)+pow(a.y-b.y,2)+pow(a.z-b.z,2));
}
```

一个没有任何技术含量的dfs。
```cpp
void dfs(long long x){
	if(arr||x>n)return;
	//cout<<"in:"<<x<<endl;
	if(a[x].z+r>=h){arr=1;return;}
	for(long long i=1;i<=n;i++){
	if(have[i]||dist(a[i],a[x])>2*r)continue;
		have[i]=1;
		dfs(i);
		//have[i]=0;千万千万不要回溯，每一个点遍历一次就好了，回溯百分百超时
	}
}
```

主函数。
```cpp
int main(){
	double H=read();
	for(double i=1;i<=H;i++){
		arr=0;//arr表示到或没到
		s=0;

		n=read(),h=read(),r=read();
		for(int i=1;i<=n;i++)have[i]=0;

		for(long long i=1;i<=n;i++){
			a[i].x=read();
			a[i].y=read();
			a[i].z=read();
        }//读入
		for(long long i=1;i<=n;i++)
			if(abs(a[i].z)<=r){
				have[i]=1;
				dfs(i);//如果与下表面联通就dfs
				for(int i=1;i<=n;i++)have[i]=0;//初始化
			}
		if(arr)cout<<"Yes \n";
		else cout<<"No \n";//不要再犯类似错误
	}
    
}
```

# 完整代码

```cpp
#include<bits/stdc++.h>
#define MAXN 1000+10
#define MAXM 10000+10
using namespace std;

double read(){
	double x=0,f=1;
	char ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}//丑陋一点不要介意

struct point{
	double x,y,z;
}a[MAXN];

double dist(point a,point b){
	return sqrt(pow(a.x-b.x,2)+pow(a.y-b.y,2)+pow(a.z-b.z,2));
}

bool have[MAXM],arr;
long long n,h,r;

int s=0;
void dfs(long long x){
	if(arr||x>n)return;
	//cout<<"in:"<<x<<endl;
	if(a[x].z+r>=h){arr=1;return;}
	for(long long i=1;i<=n;i++){
		if(have[i]||dist(a[i],a[x])>2*r)continue;
		have[i]=1;
		dfs(i);
		//have[i]=0;
	}
}
int main(){
	double H=read();
	for(double i=1;i<=H;i++){
		arr=0;
		s=0;

		n=read(),h=read(),r=read();
		for(int i=1;i<=n;i++)have[i]=0;

		for(long long i=1;i<=n;i++){
			a[i].x=read();
			a[i].y=read();
			a[i].z=read();
		}
		for(long long i=1;i<=n;i++)
			if(abs(a[i].z)<=r){
				have[i]=1;
				dfs(i);
				for(int i=1;i<=n;i++)have[i]=0;
			}
		if(arr)cout<<"Yes \n";
		else cout<<"No \n";
	}
	
	return 0;
}
```

