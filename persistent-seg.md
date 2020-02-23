---
title: 可持久化线段树学习笔记
categories:
  - [OI,学习笔记]
date: 2020-01-19 16:19:03
tags: [线段树,可持久化,学习笔记]
---
主席树又称可持久化线段树

<!--more-->

## 权值线段树

普通线段树的每一个节点表示区间，记录的是原序列在该区间上的一些信息。而权值线段树记录的是在整个序列中属于这个区间的元素的个数。

这样，在权值线段树中，元素与元素之间是**无序**的，类似与一棵平衡树。

下面是我用动态开点权值线段树写的普通平衡树。

其查询方式与BST类似。

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>

using namespace std;

#define REP(i,e,s) for(register int i=(e); i<=(s); i++)
#define DREP(i,e,s) for(register int i=(e); i>=(s); i--)
#define ll long long
#define DE(...) fprintf(stderr,__VA_ARGS__)
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
#define int ll
const int MAXN=100000+10;

struct SegmentTree {
	int lson[MAXN<<5],rson[MAXN<<5],a[MAXN];
	int lf[MAXN<<5],rg[MAXN<<5],sum[MAXN<<5],add[MAXN<<5],cnt;
#define l(x) lf[x]
#define r(x) rg[x]
#define sum(x) sum[x]
#define add(x) add[x]
#define len(x) (rg[x]-lf[x]+1)
	void pushup(int p) {
		sum(p)=sum(lson[p])+sum(rson[p]);
	}
	void pushdown(int p) {
		if(add(p)) {
			if(!lson[p]) lson[p]=++cnt;
			if(!rson[p]) rson[p]=++cnt;
			int mid=(l(p)+r(p))>>1;
			add(lson[p])=add(lson[p])+add(p);
			add(rson[p])=add(rson[p])+add(p);
			sum(lson[p])=sum(lson[p])+add(p)*(mid-l(p)+1);
			sum(rson[p])=sum(rson[p])+add(p)*(r(p)-mid);
			add(p)=0;
		}
	}
	void change(int &p,int l,int r,int L,int R,int d) {
		if(!p) p=++cnt,l(p)=l,r(p)=r;
		if(l(p)>=L&&r(p)<=R) {
			sum(p)=sum(p)+d*len(p);
			add(p)=add(p)+d;
			return ;
		}
		pushdown(p);
		int mid=(l(p)+r(p))>>1;
		if(L<=mid) change(lson[p],l,mid,L,R,d);
		if(R>mid) change(rson[p],mid+1,r,L,R,d);
		pushup(p);
	}
	int ask(int p,int l,int r) {
		if(!p) return 0;
		if(l(p)>=l&&r(p)<=r) return sum(p);
		pushdown(p);
		int mid=(l(p)+r(p))>>1;
		int ans=0;
		if(l<=mid) ans=ans+ask(lson[p],l,r);
		if(r>mid) ans=ans+ask(rson[p],l,r);
		return ans;
	}
	int getvl(int p,int l,int r,int rk) {
		if(l==r) return l; 
		int mid=(l+r)>>1;
		if(sum(lson[p])>=rk) return getvl(lson[p],l,mid,rk);
		else return getvl(rson[p],mid+1,r,rk-sum(lson[p]));
	}
	int getrk(int p,int l,int r,int vl) {
		if(!p) return 0;
		if(l==r) return 1;
		int mid=(l+r)>>1;
		if(vl<=mid) return getrk(lson[p],l,mid,vl);
		else return sum[lson[p]]+getrk(rson[p],mid+1,r,vl);
	}
} s;

const int N=1e7+10;

signed main() {
	int n=read(),rt=0;
	REP(i,1,n) {
		int op=read();
		if(op==1) {
			int x=read();
			s.change(rt,-N,N,x,x,1);
		}
		if(op==2) {
			int x=read();
			s.change(rt,-N,N,x,x,-1);
		}
		if(op==3) {
			int x=read();
			printf("%lld\n",s.getrk(1,-N,N,x));
		}
		if(op==4) {
			int k=read();
			printf("%lld\n",s.getvl(1,-N,N,k));
		}
		if(op==5) {
			int x=read();
			printf("%lld\n",s.getvl(1,-N,N,s.ask(1,-N,x-1)));
		}
		if(op==6) {
			int x=read();
			printf("%lld\n",s.getvl(1,-N,N,s.ask(1,-N,x)+1));
		}
	}
	return 0;
}
```

权值线段树可以解决整个区间第 $k$ 大的问题，那么如何动态查询区间第 $k$ 大呢？权值线段树显然是无法完成的。

## 可持久化线段树

权值线段树支持查询区间 $[1,n]$ 的第 $k$ 大查询。

考虑解决查询 $[l,r]$ 的第 $k$ 大需要那些信息。与权值线段树类似的，我们只需要建立一棵 $[l,r]$ 的权值线段树。

考虑如何快速地建立出这样的一棵权值线段树。

主席树就是解决这一问题的数据结构。

我们建立 $n$ 棵权值线段树，第 $i$ 棵表示原序列 $[1,i]$ 的权值线段树。当我们在查询 $[l,r]$ 的时候即可通过 $[1,r]-[1,l-1]$ 得到 $[l,r]$ 的权值线段树。

为什么可以这么相减呢？假设现在我们要求解区间 $[l,r]$，我们已知 $[1,l-1]$ 与 $[1,r]$ 两棵权值线段树。假设有一个元素 $a$ 在原序列中处于 $[1,l-1]$ ，那么这一个节点不应被统计到答案中。我们再来看这个元素在两棵权值线段树中出现的位置，在第一棵权值线段树中所有包含于 $\{a\}$ 的节点都会出现，在第二棵权值线段树中同样也是如此。因此，只要将两棵权值线段树前相同的部分相减即可得到 $[l,r]$ 的权值线段树。

建树的过程也非常巧妙，我们可以发现所有与之前相同的节点在后续询问中永远不可能被访问，因此这一部分节点完全没有必要建立出来。可以参考下图。
![](/img/2020-1-19-1.png)

下面是[【模板】可持久化线段树 1（主席树）](https://www.luogu.com.cn/problem/P3834)的代码。

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>

using namespace std;

#define REP(i,e,s) for(register int i=(e); i<=(s); i++)
#define DREP(i,e,s) for(register int i=(e); i>=(s); i--)
#define ll long long
#define DE(...) fprintf(stderr,__VA_ARGS__)
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int MAXN=200000+10,MAXM=MAXN*18;;

int rt[MAXM],a[MAXN],b[MAXN];

struct Persistent_Tree {
	int cnt,ls[MAXM],rs[MAXM],sum[MAXM];
	Persistent_Tree() {
		cnt=0;
	}
	void build(int &p,int l,int r) {
		p=++cnt;
		if(l==r) return ;
		int mid=(l+r)>>1;
		build(ls[p],l,mid);
		build(rs[p],mid+1,r);
	}
	void change(int &p,int l,int r,int u,int x) {
		p=++cnt,ls[p]=ls[u],rs[p]=rs[u],sum[p]=sum[u]+1;	
		if(l==r) return ;
		int mid=(l+r)>>1;
		if(x<=mid) change(ls[p],l,mid,ls[u],x);
		else change(rs[p],mid+1,r,rs[u],x);
	}
	int ask(int x,int y,int l,int r,int k) {
		if(l==r) return l;
		int mid=(l+r)>>1;
		int v=sum[ls[y]]-sum[ls[x]];
		if(v>=k) return ask(ls[x],ls[y],l,mid,k);
		else return ask(rs[x],rs[y],mid+1,r,k-v);
	}
} s;


int main() {
	int n=read(),m=read();
	REP(i,1,n) a[i]=b[i]=read();
	sort(b+1,b+1+n);
	int sz=unique(b+1,b+1+n)-b-1;
	s.build(rt[0],1,sz);
	REP(i,1,n) s.change(rt[i],1,sz,rt[i-1],lower_bound(b+1,b+sz+1,a[i])-b);

	while(m--) {
		int l=read(),r=read(),k=read();
		printf("%d\n",b[s.ask(rt[l-1],rt[r],1,sz,k)]);
	}
	return 0;
}
```

### 树上主席树

与主席树前缀和思想相似的，利用树上差分的情况也可以用主席树维护。

与序列不同的是，建树时是儿子节点与父亲节点不同的插入，查询时利用树上差分计算。

下面是[[SDOI2013]森林](https://www.luogu.com.cn/problem/P3302)的代码。
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
 
using namespace std;
 
#define REP(i,e,s) for(register int i=(e); i<=(s); i++)
#define DREP(i,e,s) for(register int i=(e); i>=(s); i--)
#define ll long long
#define DE(...) fprintf(stderr,__VA_ARGS__)
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
int read() {
    int x=0,f=1,ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
 
const int MAXN=80000+10,MAXM=MAXN*200;
 
int rt[MAXM],a[MAXN],b[MAXN];
 
struct President_Tree {
    int cnt,ls[MAXM],rs[MAXM],sum[MAXM];
    void init() {
        cnt=0;
        memset(ls,0,sizeof(ls));
        memset(rs,0,sizeof(rs));
        memset(sum,0,sizeof(sum));
    }
    void build(int &p,int l,int r) {
        p=++cnt;
        if(l==r) return ;
        int mid=(l+r)>>1;
        build(ls[p],l,mid);
        build(rs[p],mid+1,r);
    }
    void change(int &p,int l,int r,int u,int x) {
        p=++cnt,ls[p]=ls[u],rs[p]=rs[u],sum[p]=sum[u]+1;    
 
        if(l==r) return ;
        int mid=(l+r)>>1;
        if(x<=mid) change(ls[p],l,mid,ls[u],x);
        else change(rs[p],mid+1,r,rs[u],x);
    }
    int ask(int x,int y,int l,int r,int lc,int falc,int k) {
        if(x==y) return x;
        int mid=(x+y)>>1;
        int v=sum[ls[l]]+sum[ls[r]]-sum[ls[lc]]-sum[ls[falc]];
        if(v>=k) return ask(x,mid,ls[l],ls[r],ls[lc],ls[falc],k);
        else return ask(mid+1,y,rs[l],rs[r],rs[lc],rs[falc],k-v);
    }
} s;
 
 
int head[MAXN],_next[MAXN<<1],to[MAXN<<1],cnt;
 
void addedge(int u,int v) {
    cnt++;
    _next[cnt]=head[u];
    head[u]=cnt;
    to[cnt]=v;
}
 
int fa[MAXN][25],dist[MAXN];
 
queue<int> q;
 
int sz;
 
void bfs(int s) {
    q.push(s);
    while(!q.empty()) {
        int u=q.front();q.pop();
        for(int i=head[u]; i; i=_next[i]) {
            int v=to[i];
            if(fa[u][0]==v) continue;
            fa[v][0]=u;
            dist[v]=dist[u]+1;
            q.push(v);
        }
    }
}
 
int f[MAXN],son[MAXN],vis[MAXN];
 
int find(int x) {
    if(f[x]==x) return x;
    return f[x]=find(f[x]);
}
 
void dfs(int u,int p,int _rt) {
    fa[u][0]=p;
    REP(i,1,19) fa[u][i]=fa[fa[u][i-1]][i-1];
    son[_rt]++;
    dist[u]=dist[p]+1;
    f[u]=p;
    vis[u]=1;
    s.change(rt[u],1,sz,rt[p],a[u]);
    for(int i=head[u]; i; i=_next[i]) {
        int v=to[i];
        if(v==p) continue;
        dfs(v,u,_rt);
    }
}
 
int lca(int u,int v) {
    if(dist[u]<dist[v]) swap(u,v);
    int len=dist[u]-dist[v];
    DREP(i,19,0) if(1<<i&len) u=fa[u][i];
    if(u==v) return u;
    DREP(i,19,0) if(fa[u][i]!=fa[v][i]) u=fa[u][i],v=fa[v][i];
    return fa[u][0];
}
 
int last=0;
 
void init() {
    memset(head,0,sizeof(head));
    memset(vis,0,sizeof(vis));
    memset(fa,0,sizeof(fa));
    memset(dist,0,sizeof(dist));
    memset(son,0,sizeof(son));
    memset(rt,0,sizeof(rt));
    cnt=0;
}
 
int main() {
    read();
    int n=read(),m=read(),q=read();
    REP(i,1,n) a[i]=b[i]=read(),f[i]=i;
    sort(b+1,b+1+n);
    sz=unique(b+1,b+1+n)-b-1;
    s.build(rt[0],1,sz);
 
    REP(i,1,n) a[i]=lower_bound(b+1,b+1+sz,a[i])-b; 
    REP(i,1,m) {
        int u=read(),v=read();
        addedge(u,v);
        addedge(v,u);
    }
    REP(i,1,n) {
        if(!vis[i]) {
            dfs(i,0,i);
            f[i]=i;
        }
    }
    int last=0;
 
    REP(i,1,q) {
        char op[3];
        scanf("%s",op+1);
        if(op[1]=='Q') {
            int u=read()^last,v=read()^last,k=read()^last,_lca=lca(u,v);
            last=b[s.ask(1,sz,rt[u],rt[v],rt[_lca],rt[fa[_lca][0]],k)];
            printf("%d\n",last);
        }
        else {
            int u=read()^last,v=read()^last,rtu=find(u),rtv=find(v);
            addedge(u,v);addedge(v,u);
            if(son[rtu]<son[rtv]) swap(u,v),swap(rtu,rtv);
            dfs(v,u,rtu);
        }   
    }
    return 0;
}
```

### 可修改主席树

咕咕咕