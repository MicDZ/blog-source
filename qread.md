---
title: 快速读入的玄学
date: 2018-11-08 22:48:46
tags: [黑科技,随笔]
categories:
- OI   
---
在面对超大数据的读入时，我们不得不面对快速的读入方式。

<!--more-->

## 测试用数据

测试所使用的数据通过以下代码生成。

```cpp
#include<bits/stdc++.h>
using namespace std;

#define REP(i,e,s) for(register int i=e; i<=s; i++)

int main() {
	freopen("testdata.in","w",stdout);
	int n=100000;
	printf("%d\n",n);
	srand(time(0));
	REP(i,1,n) printf("%d\n",rand());
	return 0;
}
```

第一行一个整数$n$，接下来$n$行一个随机数，行末无空格。

## 使用cin读入测试用数据

测试用代码如下：

```cpp
#include<bits/stdc++.h>
using namespace std;

#define REP(i,e,s) for(register int i=e; i<=s; i++)

int main() {
	freopen("testdata.in","r",stdin);
	double start=clock();
	int n;
	cin>>n;
	int a;
	REP(i,1,n) cin>>a;
	double end=clock();

	printf("%.3llf\n",end-start);
	return 0;
}
```

如图所示![](https://www.micdz.cn/img/2019-10-02-14.png)

使用cin为2011ms。

## 使用scanf读入测试用数据

测试用代码如下：

```cpp
#include<bits/stdc++.h>
using namespace std;

#define REP(i,e,s) for(register int i=e; i<=s; i++)

int main() {
	freopen("testdata.in","r",stdin);
	double start=clock();
	int n;
	scanf("%d",&n);
	int a;
	REP(i,1,n) scanf("%d",&a);
	double end=clock();

	printf("%.3llf\n",end-start);
	return 0;
}
```

如图所示![](https://www.micdz.cn/img/2019-10-02-15.png)

使用scanf为970ms。

## 使用快读读入测试用数据

测试用代码如下：

```cpp
#include<bits/stdc++.h>
using namespace std;

#define REP(i,e,s) for(register int i=e; i<=s; i++)

int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

int main() {
	freopen("testdata.in","r",stdin);
	double start=clock();
	int n;
	n=read();
	int a;
	REP(i,1,n) a=read();
	double end=clock();

	printf("%.3llf\n",end-start);
	return 0;
}
```

如图所示![](https://www.micdz.cn/img/2019-10-02-16.png)

使用快读为364ms。

## 使用关闭同步cin测试用数据


测试用代码如下：

```cpp
#include<bits/stdc++.h>
using namespace std;

#define REP(i,e,s) for(register int i=e; i<=s; i++)

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	freopen("testdata.in","r",stdin);
	double start=clock();
	int n;
	cin>>n;
	int a;
	REP(i,1,n) cin>>a;
	double end=clock();

	printf("%.3llf\n",end-start);
	return 0;
}

```

如图所示![](https://www.micdz.cn/img/2019-10-02-17.png)

使用关闭同步cin为624ms。

但是此方法极其不稳定，不建议使用。

## 小结

当面对大数据读入的时候，一定要记得使用快读或scanf。