---
title: 【LG1784】 解数独 题解
date: 2018-08-16 22:49:58
tags: [题解,dfs]
categories:
- [OI,题解]   
---

一道即水的搜索题，不过还是有需要留意的地方，题目链接[here](https://www.luogu.org/problemnew/show/P1784)。

<!--more-->

## 核心思路

### dfs搜索

由左上至右下一次搜索，当找到答案或到达最右下角时结束。

值得一提的是，如何确定每一个格子所在的9宫格中的哪一个。

我用了一个mark函数进行计算。

```cpp
int mark(int x,int y){	
    int u=ceil(x*1.0/3),v=ceil(y*1.0/3);
    return (u-1)*3+v;
}
```

整体的思维难度不大。但是我在题解中看到一种神奇的写法。

```cpp
int mark(int x,int y){
    if(y<=3) if(x<=3) return 1; else if(x<=6) return 2; else return 3;
    else if(y<=6) if(x<=3) return 4; else if(x<=6) return 5; else return 6;
    else{
        if(x<=3) return 7; else if(x<=6) return 8; else return 9;
    }
}
```

### 剪枝

并没有什么值得剪枝的，复杂度其实完全复杂要求。唯一要做到的一点是，不能算重。

## 完整代码

```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAXN 10+1
#define INF 0x3f3f3f
#define file(a) freopen("j"a".in","r",stdin),freopen("j"a".ans","w",stdout)

int read() {
    int x=0,f=1;
    char ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch<='9'&&ch>='0'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

int a[MAXN][MAXN],line[MAXN][MAXN],rall[MAXN][MAXN],box[MAXN][MAXN];

int mark(int x,int y){	
	int u=ceil(x*1.0/3),v=ceil(y*1.0/3);
	return (u-1)*3+v;
}

bool found=0;

void print() {
	for(int i=1; i<=9; i++){
		for(int j=1; j<=9; j++)
		 	cout<<a[i][j]<<" ";
		cout<<endl;
	}
}

void dfs(int x,int y) {
    if(x==10) y++,x=1;
    if(y==10&&x==1){
        print();
        found=1;
        return ;
    }
    if(a[x][y]){
        dfs(x+1,y);
        return ;
    }
    int q=mark(x,y);
    for(int i=1;i<=9;i++){
        if(!line[x][i]&&!rall[y][i]&&!box[q][i]){
            line[x][i]=1,rall[y][i]=1,box[q][i]=1,a[x][y]=i;
            dfs(x+1,y);
            line[x][i]=0,rall[y][i]=0,box[q][i]=0,a[x][y]=0;
        }
    }
}

int main() {
//file("44");
	for(int i=1; i<=9; i++) 
		for(int j=1; j<=9; j++) {
			a[i][j]=read();
			if(a[i][j]!=0) {
				line[i][a[i][j]]=1;
				rall[j][a[i][j]]=1;
				box[mark(i,j)][a[i][j]]=1;
			}
		}
	dfs(1,1);
    return 0;
}
```

# 趣事

首先，你进入[百度百科](https://baike.baidu.com/item/%E4%B8%96%E7%95%8C%E6%9C%80%E9%9A%BE%E6%95%B0%E7%8B%AC/13848819?fr=aladdin)。然后仔细阅读。然后小心求证一下。

很有趣不是么？