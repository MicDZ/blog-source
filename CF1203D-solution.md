---
title: 【CF1203D】Remove the Substring 题解
categories:
  - OI
date: 2019-08-14 15:16:40
tags: [题解,暴力,线性]
---

Codeforces Round #579 (Div. 3)  D题

<!--more-->

# 题目大意

给定你字符串$s$ 与字符串$t$，求解最多能从$s$中删去多少个元素使得$s$中仍包含$t$为子串（不要求连续，保证给定的$t$为$s$的子串）。

# 核心思路

D1的暴力十分好写，~~但码量似乎比D2还大些~~。

$O(n^2)$ 枚举左右端点，暴力加点然后判断加完后的串还有没有$t$这个子串。正宗的暴力写法。

代码如下：

```cpp
//submit on LG

#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
 
using namespace std;
#define REP(i,e,s) for(register int i=e; i<=s; i++)
#define DREP(i,e,s) for(register int i=e; i>=s; i--)
#define DE(...) fprintf(stderr,__VA_ARGS__);
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
#define ll long long
 
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
 
const ll INF=0x3f3f3f,MAXN=200+10;
 
char s[MAXN],t[MAXN],now[MAXN];
int lens,lent;
 
bool check(int l,int r) {
    int cnt=0;
    REP(i,1,l-1) now[++cnt]=s[i];
    REP(i,r+1,lens) now[++cnt]=s[i];
    
    int find=1;
    //printf("cnt: %d\n",cnt);
    REP(i,1,cnt) {
        if(now[i]==t[find]) find++;
    }
    if(find==lent+1) return 1;
    return 0;
}
 
int main() {
    scanf("%s",s+1);
    scanf("%s",t+1);
    
    lens=strlen(s+1),lent=strlen(t+1);
 
    int ans=0;
 
    REP(i,1,lens) REP(j,i,lens) {
        if(check(i,j)) {
            ans=max(ans,j-i+1);
            //printf("%d %d\n",i,j);
        }
    }   
    printf("%d\n",ans);
	return 0;
}
```

D2的数据范围很容易联想到二分答案，但是有了NOIp2018年积木大赛的教训后，看到$10^5$也要提防一下是不是线性的做法。

首先线性地扫一遍对于$t$中的每一个一个位置$i$，求出它之前在$s$中最早能够被满足的位置，再从右边扫一遍。那么得到这个位置之后我们就可以知道任意一段区间是否能够满足。最后再线性地统计一下答案即可。