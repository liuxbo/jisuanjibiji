---
title: 'PAT题解'
date: 2020-05-06 21:21:28
tags: [PAT]
published: true
hideInList: false
feature: 
isTop: true
---
### A1001(20  两数相加)

```
#include<iostream>
#include<string>
using namespace std;
int main(){
	int a,b;
	cin>>a>>b;
	string s=to_string(a+b);
	int len=s.length();
	for(int i=0;i<len;i++){
		cout<<s[i];
		if(s[i]=='-') continue;
		if((i+1)%3==len%3&&i!=len-1) cout<<",";
	}
	return 0;
}
```





### A1002(多项式求和)

```
#include<cstdio>
#include<string.h>
const int maxn=1001;
int count1,count2;
double coe[maxn]={},a;
int number=0;
int main(){
	scanf("%d",&count1);
	while(count1--){
		int i;
	    scanf("%d%lf",&i,&a);
		coe[i]=coe[i]+a;
	}
	scanf("%d",&count2);
	while(count2--){
        int k;
		scanf("%d%lf",&k,&a);
		coe[k]=coe[k]+a;
	}
	for(int k=maxn-1;k>=0;k--){
		if(coe[k]!=0){
			number++;
		}
	}
    printf("%d",number);
	for(int k=maxn-1;k>=0;k--){
		if(coe[k]!=0) printf(" %d %.1f",k,coe[k]);
	}
	return 0;
} 
```





### A1003(25 Dijkstra、点权、边权)

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int INF=0x7fffffff;
int main(){
	int e[510][510],w[510],weight[510],d[510],num[510],n,m,c1,c2;
	bool vis[510];
	scanf("%d%d%d%d",&n,&m,&c1,&c2);
	for(int i=0;i<n;i++) scanf("%d",&weight[i]);
	fill(e[0],e[0]+510*510,INF);
	for(int i=0;i<m;i++){
		int a,b,c;
		scanf("%d%d%d",&a,&b,&c);
		e[a][b]=e[b][a]=c;
	}
	fill(vis,vis+510,false);
	fill(d,d+510,INF);
	num[c1]=1;
	d[c1]=0;
	w[c1]=weight[c1];
	for(int i=0;i<n;i++){
		int u=-1,MIN=INF;
		for(int j=0;j<n;j++){
			if(vis[j]==false&&d[j]<MIN){
				u=j;
				MIN=d[j];
			}
		}
		if(u==-1) break;
		vis[u]=true;
		for(int j=0;j<n;j++){
			if(vis[j]==false&&e[u][j]!=INF){
				if(d[j]>d[u]+e[u][j]){
					d[j]=d[u]+e[u][j];
					w[j]=w[u]+weight[j];
					num[j]=num[u];
				}else if(d[j]==d[u]+e[u][j]){
					num[j]+=num[u];
					if(weight[j]+w[u]>w[j])	w[j]=weight[j]+w[u];
				}
			} 
		}
	}
	printf("%d %d",num[c2],w[c2]);
	return 0;
}
```



### A1004

```
1.DFS法
#include<cstdio>
#include<vector>
using namespace std;
int n,m,level[100]={0},maxdepth=-1;
vector<int> tree[100];
void dfs(int index,int depth){
	if(tree[index].size()==0){
		level[depth]++;
		if(depth>maxdepth)	maxdepth=depth;
		return;
	}
	for(int i=0;i<tree[index].size();i++) dfs(tree[index][i],depth+1);
}
int main(){
	int id,k,temp;
	scanf("%d%d",&n,&m);
	for(int i=0;i<m;i++){
		scanf("%d%d",&id,&k);
		while(k--){
			scanf("%d",&temp);
			tree[id].push_back(temp);
		}
	}
	dfs(1,0);
	for(int i=0;i<=maxdepth;i++){
		if(i>0) printf(" ");
		printf("%d",level[i]);
	}
	return 0;
}

2.BFS法
#include<cstdio>
#include<queue>
#include<algorithm>
using namespace std;
const int maxn = 110;
int hashtable[maxn] = { 0 };
vector<int> child[maxn];
int n, m, level[maxn] = { 0 },maxlevel=0;
void BFS() {
	level[1] = 1;
	queue<int> q;
	q.push(1);
	while (!q.empty()) {
		int now = q.front();
		q.pop();
		if (child[now].size() == 0) {
			hashtable[level[now]]++;
			maxlevel = max(maxlevel, level[now]);
		}
		for (int i = 0; i < child[now].size(); i++) {
			level[child[now][i]] = level[now] + 1;
			q.push(child[now][i]);
		}
	}
}
int main() {
	scanf("%d%d", &n, &m);
	int father,num,temp;
	while (m--) {
		scanf("%d%d", &father, &num);
		while (num--) {
			scanf("%d", &temp);
			child[father].push_back(temp);
		}
	}
	BFS();
	for (int i = 1; i <= maxlevel; i++) {
		printf("%d", hashtable[i]);
		if (i < maxlevel) printf(" ");
	}
	return 0;
}
```



### A1005(20 字符串)

```
#include<iostream>
#include<string>
using namespace std;
int main(){
	string n,spell[10]={"zero","one","two","three","four","five","six","seven","eight","nine"};
	cin>>n;
	int sum=0;
	for(int i=0;i<n.size();i++) sum+=(n[i]-'0');
	string ans=to_string(sum);
	cout<<spell[ans[0]-'0'];
	for(int i=1;i<ans.size();i++) cout<<" "<<spell[ans[i]-'0'];
	return 0;
}
```







### A1006

```
#include<cstdio> 

struct person {
	char id[16];
	int hh, mm, ss;
}temp,first,last;
bool great(person node1, person node2) {
	if (node1.hh != node2.hh) return node1.hh > node2.hh;
	if (node2.mm != node2.mm) return node1.mm > node2.mm;
	return node1.ss > node2.ss;
}
int main() {
	int n;
	scanf("%d", &n);
	first.hh = 24, first.mm = 60, first.ss = 60;
	last.hh = 0, last.mm = 0, last.ss = 0;
	for (int i = 0; i < n; i++) {
		scanf("%s %d:%d:%d", temp.id, &temp.hh, &temp.mm, &temp.ss);
		if (great(temp, first) == false)  first = temp;
		scanf("%d:%d:%d", &temp.hh, &temp.mm, &temp.ss);
		if (great(temp, last))   last = temp;
	}
	printf("%s %s", first.id, last.id);
	return 0;
}
```





### A1007

```
#include<cstdio>
const int maxn=10010;
int a[maxn],dp[maxn],s[maxn]={0};
int main(){
	int n;
	scanf("%d",&n);
	bool flag=false;
	for(int i=0;i<n;i++){
		scanf("%d",&a[i]);
		if(a[i]>=0) flag=true;
	}
	if(flag==false){
		printf("0 %d %d",a[0],a[n-1]);
		return 0;
	}
	dp[0]=a[0];
	for(int i=1;i<n;i++){
		if(a[i]>dp[i-1]+a[i]){
			dp[i]=a[i];
			s[i]=i;
		} 
		else {
			dp[i]=dp[i-1]+a[i];
			s[i]=s[i-1];
		}
	}
	int k=0;
	for(int i=1;i<n;i++){
		if(dp[i]>dp[k]){
			k=i;
		}
	}
	printf("%d %d %d",dp[k],a[s[k]],a[k]);
	return 0;
}
```



### A1008(20 水题)

```
#include<iostream>
#include<vector>
using namespace std;
int main(){
	int n,ans=0;
	cin>>n;
	vector<int> v(n+1,0);
	for(int i=1;i<=n;i++){
		cin>>v[i];
		if(v[i]>v[i-1]) ans+=(v[i]-v[i-1])*6;
		else ans+=(v[i-1]-v[i])*4;
		ans+=5;
	}
	cout<<ans;
	return 0;
}
```



### A1009 多项式相乘

```
#include<cstdio>
#include<string.h>
const int maxn=2001;
int count1,count2;
struct poly{
	int exp;
	double coef;
}coe[1001];
double result[maxn];
int number=0;
int main(){
	scanf("%d",&count1);
for(int i =0;i<count1;i++){
	scanf("%d%lf",&coe[i].exp,&coe[i].coef);
    }
	scanf("%d",&count2);
for(int i=0;i<count2;i++){
	int k;
	double a;
		scanf("%d%lf",&k,&a);
		for(int i=0;i<count1;i++){
			result[k+coe[i].exp]+=(a*coe[i].coef);
		}
	}
	for(int k=maxn-1;k>=0;k--){
		if(result[k]!=0){
			number++;
		}
	}
    printf("%d",number);
	for(int k=maxn-1;k>=0;k--){
		if(result[k]!=0) printf(" %d %.1f",k,result[k]);
	}
	return 0;
} 
```





### A1010(25 二分) 

```
#include<iostream>
#include<cmath>
#include<string>
#include<algorithm>
using namespace std;
long long convert(string n,long long radix){
	long long sum=0,index=0,temp;
	for(auto it=n.rbegin();it!=n.rend();it++){
		temp=isdigit(*it)? (*it)-'0' : (*it)-'a'+10;
		sum+=temp*pow(radix,index++);
	}
	return sum;
}
long long findradix(string n,long long num){
	char it=*max_element(n.begin(),n.end());
	long long low=(isdigit(it)? (it)-'0' : (it)-'a'+10)+1;
	long long high=max(low,num);
	while(low<=high){
		long long mid=(low+high)/2;
		long long temp=convert(n,mid);
		if(temp<0||temp>num) high=mid-1;//小于0的情况是进制太大导致溢出 
		else if(temp==num) return mid;
		else low=mid+1;
	}
	return -1;
}
int main(){
	string n1,n2;
	long long tag,radix,ans;
	cin>>n1>>n2>>tag>>radix;
	ans=tag==1?findradix(n2,convert(n1,radix)) : findradix(n1,convert(n2,radix));
	if(ans==-1) cout<<"Impossible";
	else cout<<ans;
	return 0;
}
```



### A1011

```
#include<cstdio> 
char s[4] = "WTL";
double a,res=1;
int main() {
	for (int i = 0; i < 3; i++) {
		int imax;
		double temp = 0;
		for (int i = 0; i < 3; i++) {
			scanf_s("%lf", &a);
			if (a > temp) {
				temp = a;
				imax = i;
			}
		}
		res *= temp;
		printf("%c ", s[imax]);
	}
	printf("%.2f", (res * 0.65 - 1) * 2);
	return 0;
}
```





### A1012(25 条件排序)

```
#include<cstdio>
#include<algorithm>
#include<map>
using namespace std;
struct node{
	int id;
	int score[4];
}stu[2000];
int now,rk[1000000][4];
bool cmp(node a,node b){
	return a.score[now]>b.score[now];
}
int main(){
	int n,m;
	scanf("%d%d",&n,&m);
	map<int,bool> mp;
	for(int i=0;i<n;i++){
		scanf("%d%d%d%d",&stu[i].id,&stu[i].score[1],&stu[i].score[2],&stu[i].score[3]);
		stu[i].score[0]=(int)((stu[i].score[1]+stu[i].score[2]+stu[i].score[3])*1.0/3+0.5);
		mp[stu[i].id]=true;
	}
	for(now=0;now<=3;now++){
		sort(stu,stu+n,cmp);
		rk[stu[0].id][now]=1;
		for(int i=1;i<n;i++){
			if(stu[i].score[now]==stu[i-1].score[now]) rk[stu[i].id][now]=rk[stu[i-1].id][now];
			else rk[stu[i].id][now]=i+1;
		}
	}
	char course[4]={'A','C','M','E'};
	for(int i=0;i<m;i++){
		int query,bestrank=9999,index=0;
		scanf("%d",&query);
		if(mp.count(query)==0){
			printf("N/A\n");
			continue;
		}
		for(int j=0;j<4;j++){
			if(rk[query][j]<bestrank){
				bestrank=rk[query][j];
				index=j;
			}
		}
		printf("%d %c\n",rk[query][index],course[index]);
	}
	return 0;
}
```





### A1013(25 图的dfs遍历，计算连通块个数)

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=10010;
int n,m,k,G[maxn][maxn],cnt=0;
bool vis[maxn];
void dfs(int v){
	vis[v]=true;
	for(int i=1;i<=n;i++){
		if(G[v][i]==1&&vis[i]==false) dfs(i);
	}
}
int main(){
	int a,b;
	scanf("%d%d%d",&n,&m,&k);
	while(m--){
		scanf("%d%d",&a,&b);
		G[a][b]=G[b][a]=1;
	}
	while(k--){
		int cnt=0;
		fill(vis,vis+maxn,false);
		scanf("%d",&a);
		vis[a]=true;
		for(int i=1;i<=n;i++){
			if(vis[i]==false){
				dfs(i);
				cnt++;
			}
		}
		printf("%d\n",cnt-1);//需要添加的路径为连通块个数减1 
	}
	return 0;
}
```





### A1014(30 模拟  难题)

```
#include<vector>
#include<cstdio>
#include<queue>
using namespace std;
struct node {
	int poptime, endtime;
	queue<int> q;
};
int main() {
	int n, m, k, Q, index = 1, query;
	scanf("%d%d%d%d", &n, &m, &k, &Q);
	vector<int> time(k + 1), ans(k + 1);
	for (int i = 1; i <= k; i++) scanf("%d", &time[i]);
	vector<node>window(n + 1);
	vector<bool>sorry(k + 1, false);
	for (int i = 1; i <= m; i++) {
		for (int j = 1; j <= n; j++) {
			if (index <= k) {
				window[j].q.push(time[index]);
				if (window[j].endtime >= 540) sorry[index] = true;
				window[j].endtime += time[index];
				if (i == 1) window[j].poptime = window[j].endtime;
				ans[index] = window[j].endtime;
				index++;
			}
		}
	}
	while (index <= k) {
		int minpoptime = window[1].poptime, tempwindow = 1;
		for (int i = 2; i <= n; i++) {
			if (window[i].poptime < minpoptime) {
				minpoptime = window[i].poptime;
				tempwindow = i;
			}
		}
		window[tempwindow].q.pop();
		window[tempwindow].poptime += window[tempwindow].q.front();
		window[tempwindow].q.push(time[index]);
		if (window[tempwindow].endtime >= 540) sorry[index] = true;
		window[tempwindow].endtime += time[index];
		ans[index] = window[tempwindow].endtime;
		index++;
	}
	while (Q--) {
		scanf("%d", &query);
		if (sorry[query] == true) printf("Sorry\n");
		else printf("%02d:%02d\n", (ans[query] + 480) / 60, (ans[query] + 480) % 60);
	}
	return 0;
}
```





### A1015(20 进制转化与素数判断)

```
#include<cstdio>
#include<cmath> 
#include<algorithm>
#include<vector>
using namespace std;
int change(int n,int b){
	vector<int> v;
	int ans=0;
	while(n!=0){
		v.push_back(n%b);
		n/=b;
	}
	for(int i=v.size()-1;i>=0;i--) ans+=v[i]*pow(b,v.size()-1-i);
	return ans;
}
bool isprime(int n){
	if(n<=1) return false;
	int sqr=sqrt(n*1.0);
	for(int i=2;i<=sqr;i++){
		if(n%i==0) return false;
	}
	return true;
}
int main(){
	int n,b;
	while(1){
		scanf("%d",&n);
		if(n<0) return 0;
		scanf("%d",&b);
		if(isprime(n)&&isprime(change(n,b))) printf("Yes\n");
		else printf("No\n");
	}
}
```



###  A1016(25 排序 处理多条通话时间记录)

```
#include<iostream>
#include<algorithm>
#include<map>
#include<vector>
#include<string> 
using namespace std;
struct node{
	string name;
	int status,time,month,day,hour,minute;
};
bool cmp(node a,node b){
	return a.name!=b.name? a.name<b.name : a.time<b.time;
}
double billfromzero(node a,int *rate){
	double total=a.minute*rate[a.hour]+a.day*60*rate[24];
	for(int i=0;i<a.hour;i++) total+=60*rate[i];
	return total/100.0;
}
 int main(){
 	int n,rate[25]={0};
 	for(int i=0;i<24;i++){
 		scanf("%d",&rate[i]);
 		rate[24]+=rate[i];
	 }
	 cin>>n;
	 vector<node> v(n);
	 for(int i=0;i<n;i++){
	 	cin>>v[i].name;
	 	scanf("%d:%d:%d:%d",&v[i].month,&v[i].day,&v[i].hour,&v[i].minute);
	 	v[i].time=v[i].day*24*60+v[i].hour*60+v[i].minute;
	 	string temp;
	 	cin>>temp;
	 	v[i].status=(temp=="on-line")? 1 : 0 ;
	 } 
	 sort(v.begin(),v.end(),cmp);
	 map<string,vector<node> >mp;
	 for(int i=1;i<n;i++){
	 	if(v[i].name==v[i-1].name&&v[i-1].status==1&&v[i].status==0){
	 		mp[v[i-1].name].push_back(v[i-1]);
	 		mp[v[i].name].push_back(v[i]);
		 }
	 }
	 for(auto it:mp){
	 	cout<< it.first;
	 	vector<node> temp=it.second;
	 	printf(" %02d\n",temp[0].month);
	 	double total=0.0;
	 	for(int i=1;i<temp.size();i+=2){
	 		double t=billfromzero(temp[i],rate)-billfromzero(temp[i-1],rate);
	 		printf("%02d:%02d:%02d",temp[i-1].day,temp[i-1].hour,temp[i-1].minute);
			printf(" %02d:%02d:%02d %d",temp[i].day,temp[i].hour,temp[i].minute,temp[i].time-temp[i-1].time);
			printf(" $%.2f\n",t);
			total+=t;
		 }
		 printf("Total amount: $%.2f\n",total);
	 } 
	 return 0;
 }
```



### A1017 （25 模拟 ，时间处理、排序、）

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	int cometime,servetime;
}; 
bool cmp(node a,node b){
	return a.cometime<b.cometime;
}
int main(){
	int n,k;
	scanf("%d%d",&n,&k);
	double ans=0.0;
	node temp;
	vector<node> v;
	for(int i=0;i<n;i++){
		int hh,mm,ss,serve;
		scanf("%d:%d:%d %d",&hh,&mm,&ss,&serve);
		temp.cometime=hh*3600+mm*60+ss;
		temp.servetime=serve*60;
		if(temp.cometime>61200) continue;
		v.push_back(temp);
	}
	sort(v.begin(),v.end(),cmp);
	vector<int> window(k,28800);
	for(int i=0;i<v.size();i++){
		int minend=window[0],index=0;
		for(int j=1;j<k;j++){
			if(window[j]<minend){
				minend=window[j];
				index=j;
			}
		}
		if(v[i].cometime>window[index]) window[index]=v[i].cometime+v[i].servetime;
		else if(v[i].cometime<=window[index]){
			ans+=(window[index]-v[i].cometime);
			window[index]+=v[i].servetime; 
		}
	}
	if(v.size()==0) printf("0.0");
	else printf("%.1f",(ans/60.0)/v.size());
	return 0;
}
```





###  A1018(30 dijkst+dfs)

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
const int maxn=520;
const int INF=0x7fffffff;
int cmax,n,sp,m,minneed=INF,minremain=INF;
int weight[maxn],G[maxn][maxn],d[maxn];
bool vis[maxn]={false};
vector<int>pre[maxn];
vector<int>temp,path;
void dijkst(int s){
	fill(d,d+maxn,INF);
	d[s]=0;
	for(int i=0;i<=n;i++){
		int u=-1,MIN=INF;
		for(int j=0;j<=n;j++){
			if(vis[j]==false&&d[j]<MIN){
				MIN=d[j];
				u=j;
			}
		}
		if(u==-1) return;
		vis[u]=true;
		for(int j=0;j<=n;j++){
			if(G[u][j]!=INF&&vis[j]==false){
				if(G[u][j]+d[u]<d[j]){
					d[j]=G[u][j]+d[u];
					pre[j].clear();
					pre[j].push_back(u);
				}else if(G[u][j]+d[u]==d[j]) pre[j].push_back(u);
			}
		}
	}
}
void dfs(int s){
	if(s==0){
		temp.push_back(s);//注意 
		int need=0,remain=0;
		for(int i=temp.size()-1;i>=0;i--){
			if(weight[temp[i]]>0) remain+=weight[temp[i]];
			if(weight[temp[i]]<0){
				if(abs(weight[temp[i]])>remain){
					need+=abs(weight[temp[i]])-remain;
				    remain=0;
				}else remain-=abs(weight[temp[i]]);
			}
		}
		if(need<minneed){
			minneed=need;
			minremain=remain;
			path=temp;
		}else if(need==minneed&&remain<minremain){
			minremain=remain;
			path=temp;
		}
		temp.pop_back();
		return;
	}
	temp.push_back(s);
	for(int i=0;i<pre[s].size();i++) dfs(pre[s][i]);
	temp.pop_back();
}
int main(){
	scanf("%d%d%d%d",&cmax,&n,&sp,&m);
	for(int i=1;i<=n;i++) {
		scanf("%d",&weight[i]);
		weight[i]-=cmax/2;
	}
	fill(G[0],G[0]+maxn*maxn,INF);
	for(int i=0;i<m;i++){
		int a,b,c;
		scanf("%d%d%d",&a,&b,&c);
		G[a][b]=G[b][a]=c;
	}
	dijkst(0);
	dfs(sp);
	printf("%d ",minneed);
	for(int i=path.size()-1;i>=0;i--){
		printf("%d",path[i]);
		if(i>0) printf("->");
	}
	printf(" %d",minremain);
	return 0;
}
```





### A1019

```
#include<cstdio>
 int main(){
 	int n,b,ans[50],index=0;
 	scanf("%d%d",&n,&b);
 	while(n!=0){
 		ans[index++]=n%b;
 		n/=b;
	 }
	 bool flag=true;
	 for(int i=0;i<=index/2;i++){
	 	if(ans[i]!=ans[index-i-1]) flag=false;
	 }
	 if(flag==false) printf("No\n");
	 else printf("Yes\n");
	 for(int i=index-1;i>=0;i--){
	 	if(i!=index-1) printf(" ");
	 	printf("%d",ans[i]);
	 }
	 return 0; 
 }
```





### A1020(25 后序+中序建树，输出层序)

```
#include<cstdio>
#include<queue>
using namespace std;
const int maxn=31;
struct node{
	int data;
	node *left,*right;
};
int post[maxn],in[maxn],n,num=0;
node * create(int postl,int postr,int inl,int inr){
	if(postl>postr) return NULL;
	node *root=new node;
	root->data=post[postr];
	int k=inl;
	while(in[k]!=root->data) k++;
	int leftnum=k-inl;
	root->left=create(postl,postl+leftnum-1,inl,k-1);
	root->right=create(postl+leftnum,postr-1,k+1,inr);
	return root;
}
void layerorder(node *root){
	queue<node*> q;
	q.push(root);
	while(!q.empty()){
		node* Top=q.front();
		q.pop();
		printf("%d",Top->data);
		num++;
		if(num<n) printf(" ");
		if(Top->left!=NULL) q.push(Top->left);
		if(Top->right!=NULL) q.push(Top->right);
	}
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d",&post[i]);
	for(int i=0;i<n;i++) scanf("%d",&in[i]);
	node*root=create(0,n-1,0,n-1);
	layerorder(root);
	return 0;
}
```





### A1021(25 找出图中使深度最深的结点(首先判断是否能看成树))

```
#include<cstdio>
#include<vector>
#include<algorithm>
#include<set>
using namespace std;
bool vis[10010]; 
int n,maxheight=-1;
vector<int> ans,G[10010];
void dfs(int index,int height){
	vis[index]=true;
	if(height>maxheight){
		maxheight=height;
		ans.clear();
		ans.push_back(index);
	}else if(height==maxheight) ans.push_back(index);
	for(int i=0;i<G[index].size();i++){
		if(vis[G[index][i]]==false) dfs(G[index][i],height+1); 
	} 
}
int main(){
	int v1,v2,cnt=0,st;
	set<int> res;
	scanf("%d",&n);
	for(int i=1;i<=n-1;i++){
		scanf("%d%d",&v1,&v2);
		G[v1].push_back(v2);
		G[v2].push_back(v1);
	} 
	fill(vis,vis+10010,false);
	for(int i=1;i<=n;i++){
		if(vis[i]==false){
			dfs(i,0);
			if(i==1){
				if(ans.size()!=0) st=ans[0];
				for(int j=0;j<ans.size();j++) res.insert(ans[j]);
			} 
			cnt++;
		}
	}
	if(cnt>1) printf("Error: %d components",cnt);
	else{
		ans.clear(); 
		fill(vis,vis+10010,false);
		maxheight=-1;
		dfs(st,0);
		for(int i=0;i<ans.size();i++) res.insert(ans[i]);
		for(auto it=res.begin();it!=res.end();it++) printf("%d\n",*it);
	}
	return 0;
}
```



### A1022

```
#include<cstdio>
#include<iostream>
#include<map>
#include<set>
#include<string>
using namespace std;
map<string, set<int> > mptitle, mpautor, mpkey, mppublish, mpyear;
void query(map<string, set<int> >& mp, string& str) {
	if (mp.count(str) == 0) cout << "Not Found\n";
	else {
		for (auto it = mp[str].begin(); it != mp[str].end(); it++) {
			printf("%07d\n", *it);
		}
	}
}
int main() {
	int n,m,id;
	cin >> n;
	string title, autor, keyword, publish, year;
	for (int i = 0; i < n; i++) {
		cin >> id;
		char c = getchar();
		getline(cin, title);
		mptitle[title].insert(id);
		getline(cin, autor);
		mpautor[autor].insert(id);
		while (cin >> keyword) {
			mpkey[keyword].insert(id);
			 c = getchar();
			if (c == '\n') break;
		}
		getline(cin, publish);
		mppublish[publish].insert(id);
		getline(cin, year);
		mpyear[year].insert(id);
	}
	cin >> m;
	while(m--){
		int type;
		string temp;
		scanf("%d: ", &type);
		getline(cin, temp);
		cout << type << ":" << " " << temp<<endl;
		if (type == 1) query(mptitle, temp);
		if (type == 2) query(mpautor, temp);
		if (type == 3) query(mpkey, temp);
		if (type == 4) query(mppublish, temp);
		if (type == 5) query(mpyear, temp);
	}
	return 0;
}
```



### A1023(20 大整数乘2)

```
方法一:
#include<cstdio>
#include<iostream>
#include<string>
#include<cstring>
using namespace std;
struct bign {
	int d[1000];
	int len;
	bign() {
		len = 0;
		memset(d, 0, sizeof(d));
	}
};
bign change(string str) {
	bign a;
	a.len = str.length();
	for (int i = 0; i < a.len; i++) {
		a.d[i] = str[a.len - 1 - i]-'0';
	}
	return a;
}
bign multi(bign a, int b) {
	bign c;
	int carry=0;
	for (int i = 0; i < a.len; i++) {
		int temp = a.d[i] * b + carry;
		c.d[c.len++] = temp % 10;
		carry = temp / 10;
	}
	while (carry != 0) {
		c.d[c.len++] = carry % 10;
		carry /= 10;
	}
	return c;
}
bool judge(bign a, bign b) {
	if (a.len != b.len) return false;
	else {
		int count[10] = { 0 };
		for (int i = 0; i < a.len; i++) {
			count[a.d[i]]++;
			count[b.d[i]]--;
		}
		for (int i = 0; i < 10; i++) {
			if (count[i] != 0) {
				return false;
			}
		}
	}
	return true;
}
void print(bign a) {
	for (int i = a.len - 1; i >= 0; i--) {
		printf("%d", a.d[i]);
	}
}
int main() {
	string str;
	cin >> str;
	bign a, ans;
	a=change(str);
	ans = multi(a, 2);
	if (judge(a, ans)) cout << "Yes" << endl;
	else cout << "No" << endl;
	print(ans);
	return 0;
}


方法二:
#include<iostream>
#include<string>
#include<map> 
using namespace std;
string Double(string s){
	string ans=s;
	int carry=0;
	for(int i=s.size()-1;i>=0;i--){
		ans[i]=(((s[i]-'0')*2+carry)%10)+'0';
		carry=((s[i]-'0')*2+carry)/10;
	}
	if(carry>0) ans="1"+ans;
	return ans;
}
int main(){
	string s,res;
	map<char,int>mp;
	cin>>s;
	for(int i=0;i<s.size();i++) mp[s[i]]++;
	res=Double(s);
	for(int i=0;i<res.size();i++) mp[res[i]]--;
	for(auto it=mp.begin();it!=mp.end();it++){
		if(it->second!=0){
			cout<<"No\n"<<res;
			return 0;
		}
	}
	cout<<"Yes\n"<<res;
	return 0;
}
```





### A1024(25 大整数反转相加，判断回文数)

```
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;
string rev(string s) {
	reverse(s.begin(),s.end());
	return s;
}
string add(string s1,string s2) {
	string ans=s1;
	int carry=0;
	for(int i=ans.size()-1; i>=0; i--) {
		ans[i]=((s1[i]-'0')+(s2[i]-'0')+carry)%10+'0';
		carry=((s1[i]-'0')+(s2[i]-'0')+carry)/10;
	}
	if(carry!=0) ans="1"+ans;
	return ans;
}
bool judge(string s) {
	for(int i=0; i<s.size()/2; i++) {
		if(s[i]!=s[s.size()-i-1]) return false;
	}
	return true;
}
int main() {
	string s;
	int step;
	cin>>s>>step;
	if(s.size()==1||judge(s)) cout<<s<<endl<<"0";
	else {
		for(int i=1; i<=step; i++) {
			s=add(s,rev(s));
			if(judge(s)||i==step) {
				cout<<s<<endl<<i;
				break;
			}
		}
	}
	return 0;
}
```



### A1025(25 条件排序)

```
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	string id;
	int localid,score,frank,lrank;
};
bool cmp(node a,node b){
	return a.score!=b.score? a.score>b.score : a.id<b.id;
}
int main(){
	int n,k;
	cin>>n;
	vector<node> total;
	for(int j=1;j<=n;j++){
		scanf("%d",&k);
		vector<node> v(k);
		for(int i=0;i<k;i++) {
			cin>>v[i].id>>v[i].score;
			v[i].localid=j;
		}
		sort(v.begin(),v.end(),cmp);
		v[0].lrank=1;
		for(int i=1;i<k;i++) {
			if(v[i].score==v[i-1].score) v[i].lrank=v[i-1].lrank;
			else v[i].lrank=i+1;
		}
		for(int i=0;i<k;i++) total.push_back(v[i]);
	}
	sort(total.begin(),total.end(),cmp);
	total[0].frank=1;
	for(int i=1;i<total.size();i++) {
		if(total[i].score==total[i-1].score) total[i].frank=total[i-1].frank;
		else total[i].frank=i+1;
	}
	cout<<total.size()<<endl;
	for(int i=0;i<total.size();i++){
		printf("%s %d %d %d\n",total[i].id.c_str(),total[i].frank,total[i].localid,total[i].lrank);
	}
	return 0;
}
```







### A1027

```
#include<cstdio>
int main(){
	char c[14]={"0123456789ABC"};
	printf("#");
	for(int i=0;i<3;i++){
		int n;
		scanf("%d",&n);
		printf("%c%c",c[n/13],c[n%13]);
	}
	return 0;
}
```





### A1028(25  排序)

```
#include<algorithm>
#include<vector>
#include<string>
#include<iostream>
using namespace std;
struct node{
	int id,grade;
	string name;
};
bool cmp1(node a,node b){
	return a.id<b.id;
}
bool cmp2(node a,node b){
	return a.name!=b.name? a.name<b.name :a.id<b.id;
}
bool cmp3(node a,node b){
	return a.grade!=b.grade? a.grade<b.grade : a.id<b.id;
}
int main(){
	int n,c;
	cin>>n>>c;
	vector<node> v(n);
	for(int i=0;i<n;i++) cin>>v[i].id>>v[i].name>>v[i].grade;
	if(c==1) sort(v.begin(),v.end(),cmp1);
	else if(c==2) sort(v.begin(),v.end(),cmp2);
	else if(c==3)sort(v.begin(),v.end(),cmp3);
	for(int i=0;i<n;i++) printf("%06d %s %d\n",v[i].id,v[i].name.c_str(),v[i].grade);
	return 0;
}
```



### A1029(25 求中位数 twopoints)

```
1.直接暴力
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=400010;
int main(){
    int n1,n2;
    long long a[maxn];
    scanf("%d",&n1);
    int i;
    for( i=0;i<n1;i++){
        scanf("%lld",&a[i]);
    }
      scanf("%d",&n2);
    for( i=n1;i<n2+n1;i++){
        scanf("%lld",&a[i]);
    }
    sort(a,a+n1+n2);
    printf("%d",a[(n1+n2-1)/2]);
    return 0;
}

2.two points

#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=400010;
const int INF=(1<<31)-1;
int main(){
    int n1,n2;
    long long a[maxn],b[maxn];
    scanf("%d",&n1);
    for(int i=0;i<n1;i++){
        scanf("%lld",&a[i]);
    }
      scanf("%d",&n2);
    for(int i=0;i<n2;i++){
        scanf("%lld",&b[i]);
    }
    a[n1]=INF;
    b[n2]=INF;
    int midpos=(n1+n2-1)/2;
    int count=0,j=0,i=0;
    while(count<midpos){
        if(a[i]<b[j]) i++;
        else j++;
        count++;
    }
    if(a[i]<b[j]) printf("%lld",a[i]);
    else printf("%lld",b[j]);
    return 0;
}
```





### A1030(30 dijkst,距离最短加花费最少)

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=510;
const int INF=0x7fffffff;
int G[maxn][maxn],n,m,st,ed,cost[maxn][maxn],d[maxn],c[maxn],pre[maxn];
bool vis[maxn];
void dijkst(int s){
	fill(d,d+maxn,INF);
	fill(c,c+maxn,INF);
	fill(vis,vis+maxn,false);
	d[s]=0;
	c[s]=0;
	for(int i=0;i<n;i++) pre[i]=i;
	for(int i=0;i<n;i++){
		int u=-1,MIN=INF;
		for(int j=0;j<n;j++){
			if(vis[j]==false&&d[j]<MIN){
				MIN=d[j];
				u=j;
			}
		}
		if(u==-1) return;
		vis[u]=true;
		for(int v=0;v<n;v++){
			if(vis[v]==false&&G[u][v]!=INF){
				if(d[u]+G[u][v]<d[v]){
					d[v]=d[u]+G[u][v];
					c[v]=cost[u][v]+c[u];
					pre[v]=u;
				}else if(d[u]+G[u][v]==d[v]){
					if(c[v]>c[u]+cost[u][v]){
						c[v]=cost[u][v]+c[u];
						pre[v]=u;
					}
				}
			}
		}
	} 
}
void dfs(int ed){
	if(ed==st){
        printf("%d ",ed);
        return;
    } 
	dfs(pre[ed]);
	printf("%d ",ed);
}
int main(){
	scanf("%d%d%d%d",&n,&m,&st,&ed);
	fill(G[0],G[0]+maxn*maxn,INF);
	while(m--){
		int v1,v2;
		scanf("%d%d",&v1,&v2);
		scanf("%d%d",&G[v1][v2],&cost[v1][v2]);
		G[v2][v1]=G[v1][v2];
		cost[v2][v1]=cost[v1][v2];
	}
	dijkst(st);
	dfs(ed);
	printf("%d %d",d[ed],c[ed]);
	return 0;
}
```



### A1031

```
#include<cstdio> 
#include<cstring>
int main() {
	char str[100], uu[40][40];
    scanf("%s",str); 
	int N = strlen(str);
	int n1=(N+2)/3;
	int n3=(N+2)/3;
	int n2=N-n1-n3+2;
	int pos=0;
    memset(uu,' ',sizeof(uu));
	for(int i=0;i<n1-1;i++){
		uu[i][0]=str[pos++];
	} 
	for(int j=0;j<n2;j++){
		uu[n1-1][j]=str[pos++];
	}
	for(int k=n3-2;k>=0;k--){
		uu[k][n2-1]=str[pos++];
	}
	for(int i=0;i<n1;i++){
		for(int j=0;j<n2;j++){
			printf("%c",uu[i][j]);
		}
		printf("\n");
	}
	return 0;
}
```





### A1032(25 链表  找公共后缀)

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=100010;
struct node{
	char data;
	bool flag;
	int next;
}list[maxn];
int main(){
	int address,begin1,begin2,n;
	scanf("%d%d%d",&begin1,&begin2,&n);
	for(int i=0;i<n;i++){
		scanf("%d",&address);
		scanf(" %c %d",&list[address].data,&list[address].next);
	}
	while(begin1!=-1){
		list[begin1].flag=true;
		begin1=list[begin1].next;
	}
	while(begin2!=-1){
		if(list[begin2].flag==true) {
			printf("%05d",begin2);
			return 0;
		}
		begin2=list[begin2].next;
	}
	printf("-1");
	return 0;
}
```





### A1033

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 510;
const int INF = 1000000000;
struct node{
	double price;
	double distance;
}station[maxn];
bool cmp(node a, node b) {
	return a.distance < b.distance;
}
int main() {
	double cmax, d, davg;
	int n;
	scanf("%lf%lf%lf%d", &cmax, &d, &davg, &n);
	for (int i = 0; i < n; i++) {
		scanf("%lf%lf", &station[i].price, &station[i].distance);
	}
	station[n].distance = d;
	station[n].price = 0;
	sort(station, station + n + 1, cmp);
	if (station[0].distance != 0) {
		printf("The maximum travel distance = 0.00");
		return 0;
	}
	int now = 0;
	double nowtank = 0, expend = 0,  canrun = cmax * davg;
	while (now < n) {
        int k=-1;
        double minprice=INF;
		for (int i = now + 1; (station[i].distance - station[now].distance) <= canrun && i <= n; i++) {
			if (station[i].price < minprice) {
				minprice = station[i].price;
				k = i;
				if (minprice < station[now].price) {
					break;
				}
			}
		}
		if (k == -1) break;
		double need = (station[k].distance - station[now].distance) / davg;
		if (station[k].price < station[now].price) {
			if (nowtank < need) {
				expend += (need - nowtank) * station[now].price;
				nowtank = 0;
			}
			else {
				nowtank -= need;
			}
		}
		else {
			expend += (cmax - nowtank) * station[now].price;
			nowtank = cmax - need;
		}
		now = k;
	}
	if (now == n) printf("%.2f", expend);
	else printf("The maximum travel distance = %.2f", station[now].distance + canrun);
	return 0;
}
```





### A1034(30 图的深度遍历、map、连通块、边权和点权)

```
#include<iostream>
#include<map>
#include<string>
using namespace std;
map<string,int> strtoint;
map<int,string> inttostr;
map<string,int> ans;
bool vis[2020];
int G[2020][2020],weight[2020],n,k,index=1;
int Stringtoint(string s){
	if(strtoint.count(s)==0){
		strtoint[s]=index;
		inttostr[index]=s;
		index++;//同时计数总的结点个数 
	}
	return strtoint[s];
}
void dfs(int id,int &totaltime,int&head,int &num){
	vis[id]=true;
	num++;
	if(weight[id]>weight[head]) head=id;
	for(int i=1;i<index;i++){
		if(G[id][i]!=0){
			totaltime+=G[id][i];
			G[id][i]=G[i][id]=0;
			if(vis[i]==false) dfs(i,totaltime,head,num);
		} 
	}
}
int main(){
	string s1,s2;
	cin>>n>>k;
	int id1,id2,calltime;
	for(int i=0;i<n;i++){
		cin>>s1>>s2>>calltime;
		id1=Stringtoint(s1),id2=Stringtoint(s2);
		G[id1][id2]+=calltime;
		G[id2][id1]+=calltime;
		weight[id1]+=calltime;
		weight[id2]+=calltime;
	}
	fill(vis,vis+2020,false);
	for(int i=1;i<index;i++){
		if(vis[i]==false){
			int totaltime=0,head=i,num=0;
			dfs(i,totaltime,head,num);
			if(num>2&&totaltime>k) ans[inttostr[head]]=num;
		}
	}
	cout<<ans.size()<<endl;
	for(auto it=ans.begin();it!=ans.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	return 0;
}
```



### A1035(20 字符串)

```
#include<iostream>
#include<string>
#include<vector>
using namespace std;
int main(){
	int n;
	cin>>n;
	vector<string> v;
	for(int j=0;j<n;j++){
		bool flag=false;
		string name,password,ans;
		cin>>name>>password;
		for(int i=0;i<password.length();i++){
			switch(password[i]){
				case '1':password[i]='@';flag=true;break;
				case '0':password[i]='%';flag=true;break;
				case 'l':password[i]='L';flag=true;break;
				case 'O':password[i]='o';flag=true;break;
			}
		}
		if(flag==true) v.push_back(name+" "+password);
	}
	if(n==1) printf("There is 1 account and no account is modified\n");
	else if(v.size()==0) printf("There are %d accounts and no account is modified\n",n);
	else{
		printf("%d\n",v.size());
		for(int i=0;i<v.size();i++) cout<<v[i]<<endl;
	}
	return 0;
}
```



### A1036

```
#include<cstdio> 
#include<algorithm>
using namespace std;
struct student {
	char name[11];
	char gender;
	char id[11];
	int score;
}male,female,temp;
int main() {
	int n;
	scanf("%d", &n);
	female.score = -1;
	male.score = 101;
	for (int i = 0; i < n; i++) {
		scanf("%s %c %s %d", temp.name, &temp.gender, temp.id, &temp.score);
		if (temp.gender == 'F') {
			if (temp.score > female.score) {
				female = temp;
			}
		}
		if (temp.gender == 'M') {
			if (temp.score < male.score) {
				male = temp;
			}
		}
	}
	if (female.score == -1) printf("Absent\n");
	if (female.score > -1) printf("%s %s\n", female.name, female.id);
	if (male.score == 101) printf("Absent\n");
	if (male.score <101) printf("%s %s\n", male.name, male.id);
	if (female.score == -1 || male.score == 101) printf("NA");
	if (female.score > -1 && male.score < 101) printf("%d", female.score - male.score);
	return 0;
}
```



### A1037(25 贪心)

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
bool cmp(int a,int b){return a>b;}
int main(){
 	int nc,np,c,p,ans=0;
 	vector<int>c1,c2,p1,p2;
 	scanf("%d",&nc);
 	for(int i=0;i<nc;i++){
 		scanf("%d",&c);
 		if(c>=0) c1.push_back(c);
 		else c2.push_back(c);
	 }
	 scanf("%d",&np);
	 for(int i=0;i<np;i++){
 		scanf("%d",&p);
 		if(p>=0) p1.push_back(p);
 		else p2.push_back(p);
	 }
	 sort(c1.begin(),c1.end(),cmp);
	 sort(p1.begin(),p1.end(),cmp);
	 sort(c2.begin(),c2.end());
	 sort(p2.begin(),p2.end());
	 for(int i=0;i<c1.size()&&i<p1.size();i++)	ans+=c1[i]*p1[i];
	 for(int i=0;i<c2.size()&&i<p2.size();i++)	ans+=c2[i]*p2[i];
	 printf("%d",ans);
	 return 0;
}
```



### A1038(30 贪心  求最小字符串序列)

```
#include<iostream>
#include<string>
#include<algorithm>
#include<vector>
using namespace std;
bool cmp(string &a,string &b){
	return a+b<b+a;
}
int main(){
	int n;
	cin>>n;
	string ans;
	vector<string> v(n);
	for(int i=0;i<n;i++) cin>>v[i];
	sort(v.begin(),v.end(),cmp);
	for(int i=0;i<n;i++) ans+=v[i];
	while(ans[0]=='0') ans.erase(0,1);
	if(ans.size()==0) cout<<"0";
	else cout<<ans;
	return 0;
}
```



### A1039(25 vector)

```
#include<vector>
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=26*26*26*10+10;
int getid(char *name){
	int id=0;
	for(int i=0;i<3;i++) id=26*id+(name[i]-'A');
	id=id*10+(name[3]-'0');
	return id;
}
int main(){
	int n,k,courseid,num;
	char name[4];
	scanf("%d%d",&n,&k);
	vector<int> v[maxn];
	for(int i=0;i<k;i++){
		scanf("%d%d",&courseid,&num);
		for(int j=0;j<num;j++){
			scanf("%s",name);
			v[getid(name)].push_back(courseid);
		}
	}
	for(int i=0;i<n;i++){
		scanf("%s",name);
		printf("%s %d",name,v[getid(name)].size());
		sort(v[getid(name)].begin(),v[getid(name)].end());
		for(int j=0;j<v[getid(name)].size();j++) printf(" %d",v[getid(name)][j]);
		printf("\n");
	}
	return 0;
}
```





### A1040

```
#include<iostream>
#include<cstring>
#include<string>
using namespace std;
const int maxn=1010;
string str;
int dp[maxn][maxn];
int main(){
	memset(dp,0,sizeof(dp));
	getline(cin,str);
	int ans=1,len=str.length();
	for(int i=0;i<len;i++){
		dp[i][i]=1;
		if(i<len-1){
			if(str[i]==str[i+1]){
				dp[i][i+1]=1;
				ans=2;
			}
		}
	}
	for(int L=3;L<=len;L++){
		for(int i=0;i+L-1<len;i++){
			int j=i+L-1;
			if(str[i]==str[j]&&dp[i+1][j-1]==1){
				dp[i][j]=1;
				ans=L;
			}
		}
	}
	printf("%d",ans);
	return 0;
}
```



### A1041(20 hash)

```
#include<string>
#include<vector>
#include<iostream>
using namespace std;
int main(){
	int n,flag[100001]={0},j;
	cin>>n;
	vector<int> v(n);
	for(int i=0;i<n;i++){
		scanf("%d",&v[i]);
		flag[v[i]]++;
	}
	for(j=0;j<n;j++){
		if(flag[v[j]]==1) {
			cout<<v[j];
			break;
		}
	}
	if(j==n) cout<<"None";
	return 0;
}
```



### A1042

```
#include<cstdio>
const int N = 55;
int main()
{
	char mp[5] = { 'S','H','C','D','J' };
	int start[N], next[N], end[N];
	int times;
	scanf("%d", &times);
	for (int i = 1; i <= 54; i++) {
		scanf("%d", &next[i]);
	}
	for (int i = 1; i <= 54; i++) {
		start[i] = i;
	}
	for (int step = 0; step < times; step++) {
		for (int i = 1; i <= 54; i++) {
			end[next[i]] = start[i];
		}
		for (int i = 1; i <= 54; i++) {
			start[i] = end[i];
		}
	}
	for (int i = 1; i <= 54; i++) {
		if (i != 1) printf(" ");
		start[i]--;
		printf("%c%d", mp[start[i] / 13], start[i] % 13+1);
	}
	return 0;
}
```





### A1043(25 二叉搜索树的遍历与反转)

```
#include<cstdio>
#include<vector>
using namespace std;
struct node{
	int data;
	node*left,*right;
};
void insert(node*&root,int data){
	if(root==NULL){
		root=new node;
		root->data=data;
		root->left=NULL;
		root->right=NULL;
		return;
	}
	if(root->data<=data) insert(root->right,data);
	else if(root->data>data) insert(root->left,data);
}
void preorder(node*root,vector<int>&v){
	if(root==NULL) return;
	v.push_back(root->data);
	preorder(root->left,v);
	preorder(root->right,v);
}
void preorderM(node*root,vector<int>&v){
	if(root==NULL) return;
	v.push_back(root->data);
	preorderM(root->right,v);
	preorderM(root->left,v);
}
void postorder(node*root,vector<int>&v){
	if(root==NULL) return;
	postorder(root->left,v);
	postorder(root->right,v);
	v.push_back(root->data);
}
void postorderM(node*root,vector<int>&v){
	if(root==NULL) return;
	postorderM(root->right,v);
	postorderM(root->left,v);
	v.push_back(root->data);
}
int main(){
	int n;
	scanf("%d",&n);
	vector<int> v(n),pre,preM,post,postM;
	node*root=NULL;
	for(int i=0;i<n;i++) {
		scanf("%d",&v[i]);
		insert(root,v[i]);
	}
	preorder(root,pre);
	preorderM(root,preM);
	if(pre==v){
		printf("YES\n");
		postorder(root,post);
		for(int i=0;i<n;i++){
			if(i>0) printf(" ");
			printf("%d",post[i]);
		}
	}else if(preM==v){
		printf("YES\n");
		postorderM(root,postM);
		for(int i=0;i<n;i++){
			if(i>0) printf(" ");
			printf("%d",postM[i]);
		}
	}else printf("NO");
	return 0;
}
```



### A1044(25 二分)

```
#include<iostream>
#include<vector>
using namespace std;
vector<int> sum,ans;
int n,m;
void func(int i,int &j,int &tempsum){
	int low=i,high=n;
	while(low<high){
		int mid=(low+high)/2;
		if(sum[mid]-sum[i-1]>=m) high=mid;
		else low=mid+1;
	}
	j=high;
	tempsum=sum[j]-sum[i-1];
}
int main(){
	cin>>n>>m;
	sum.resize(n+1);
	for(int i=1;i<=n;i++){
		cin>>sum[i];
		sum[i]+=sum[i-1];
	}
	int minans=sum[n];
	for(int i=1;i<=n;i++){
		int j,tempsum;
		func(i,j,tempsum);
		if(tempsum>minans) continue;
		if(tempsum>=m){
			if(tempsum<minans){
				ans.clear();
				minans=tempsum;
			}
			ans.push_back(i);
			ans.push_back(j);
		}
	}
	for(int i=0;i<ans.size();i+=2)	printf("%d-%d\n",ans[i],ans[i+1]);
	return 0;
}
```





### A1045

```
方法一:最长不下降子序列LIS
#include<cstdio> 
#include<algorithm>
using namespace std;
const int maxn=10010;
int hashtable[maxn],a[maxn],dp[maxn];
int main(){
	int num=0,ans=-1,n,m,L,temp;
	scanf("%d",&n);
	scanf("%d",&m);
	fill(hashtable,hashtable+maxn,-1);
	for(int i=0;i<m;i++){
		scanf("%d",&temp);
		hashtable[temp]=i;
	}
	scanf("%d",&L);
	for(int i=0;i<L;i++){
		scanf("%d",&temp);
		if(hashtable[temp]>=0){
			a[num++]=hashtable[temp];
		}
	}
	for(int i=0;i<num;i++){
		dp[i]=1;
		for(int j=0;j<i;j++){
			if(a[i]>=a[j]&&dp[j]+1>dp[i]){
				dp[i]=dp[j]+1;
			}
		}
		ans=max(ans,dp[i]);
	}
	printf("%d",ans);
	return 0;
}
方法二:最长公共子序列
#include<cstdio> 
#include<algorithm>
using namespace std;
const int maxn=10010;
int a[maxn],b[maxn],dp[maxn][maxn];
int main(){
	int n,m,L;
	scanf("%d",&n);
	scanf("%d",&m);
    for(int i=1;i<=m;i++) scanf("%d",&a[i]);
    scanf("%d",&L);
    for(int i=1;i<=L;i++) scanf("%d",&b[i]);
    for(int i=0;i<m;i++) dp[i][0]=0;
    for(int i=0;i<L;i++) dp[0][i]=0;
    for(int i=1;i<=m;i++){
    	for(int j=1;j<=L;j++){
    		int maxone=max(dp[i-1][j],dp[i][j-1]);
    		if(a[i]==b[j]) dp[i][j]=maxone+1;
    		else dp[i][j]=maxone;
		}
	}
	printf("%d",dp[m][L]);
	return 0;
}
```



### A1046

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 100010;
int dis[maxn], A[maxn];
int main() {
	int n,times,left,right,sum=0;
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &A[i]);
		sum += A[i];
        dis[i] = sum;
	}
	scanf("%d", &times);
	for (int i = 0; i < times; i++) {
		scanf("%d%d", &left, &right);
		if (left > right)  swap(left, right);
		int distance = dis[right - 1] - dis[left - 1];
			printf("%d\n", min(distance,sum-distance));
	}
	return 0;
}
```





### A1047(25 vector)

```
#include<cstdio>
#include<cstring>
#include<vector>
#include<algorithm>
using namespace std;
char name[40010][5];
vector<int> v[2510];
bool cmp(int a,int b){
	return strcmp(name[a],name[b])<0;
}
int main(){
	int n,k,c,courseid;
	scanf("%d%d",&n,&k);
	for(int i=0;i<n;i++){
		scanf("%s%d",name[i],&c);
		while(c--){
			scanf("%d",&courseid);
			v[courseid].push_back(i);
		}
	}
	for(int i=1;i<=k;i++){
		printf("%d %d\n",i,v[i].size());
		sort(v[i].begin(),v[i].end(),cmp);
		for(int j=0;j<v[i].size();j++) printf("%s\n",name[v[i][j]]);
	}
	return 0;
}
```





### A1048(25 )

```
1.哈希散列解法
#include<cstdio>
int main(){
	int n,pay,flag[100010]={0},a[100010];
	scanf("%d%d",&n,&pay);
	for(int i=0;i<n;i++) {
		scanf("%d",&a[i]);
		flag[a[i]]++;
	}
	for(int i=1;i<pay;i++){
		if(flag[i]>0&&flag[pay-i]>0){
			if((i==pay-i&&flag[i]>1)||i!=pay-i){
				printf("%d %d",i,pay-i);
			    return 0;
			}
		}
	} 
	printf("No Solution");
	return 0;
}


2.二分法解法
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 100010;
int main() {
	int n, m;
	int num[maxn];
	scanf("%d%d", &n, &m);
	for (int i = 0; i < n; i++) {
		scanf("%d", &num[i]);
	}
	sort(num, num + n);
	int j,i;
	for ( i = 0; i < n; i++) {
		 j = lower_bound(num + i, num +n, m - num[i]) - num;
		if ( (num[j] + num[i] == m&&j!=i)||(num[i]==num[i+1]&&num[i]*2==m)) {
			printf("%d %d", num[i], num[j]);
			break;
		}
	}
	if (i >= n) printf("No Solution");
	return 0;
}

3.two points
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn = 100010;
int main() {
	int n, m;
	int a[maxn];
	scanf("%d%d", &n, &m);
	for (int i = 0; i < n; i++) {
		scanf("%d", &a[i]);
	}
	sort(a, a + n);
	int j=n-1,i=0;
    while(i<j){
        if(a[i]+a[j]==m){
            printf("%d %d",a[i],a[j]);
            return 0;
        }
        else if(a[i]+a[j]>m) j--;
        else i++;
    }
	printf("No Solution");
	return 0;
}
```





### A1049(30  数学问题："1"的个数)

```
#include<iostream>
using namespace std;
int main(){
	int n,left=0,right=0,a=1,now=1,ans=0;
	cin>>n;
	while(n/a){
		left=n/(a*10),right=n%a,now=n/a%10;
		if(now==0) ans+=left*a;
		else if(now==1) ans+=left*a+right+1;
		else ans+=(left+1)*a;
		a*=10; 
	}
	cout<<ans;
	return 0;
}
```



### A1050(20 hash)

```
#include<string>
#include<iostream>
#include<vector>
using namespace std;
int main(){
	string s1,s2,ans;
	vector<int> v(128,0);
	getline(cin,s1);
	getline(cin,s2);
	for(int i=0;i<s2.length();i++) v[s2[i]]++;
	for(int i=0;i<s1.length();i++){
		if(v[s1[i]]==0) ans+=s1[i];
	}
	cout<<ans;
	return 0; 
}
```



### A1051(25 栈) 

```
#include<cstdio>
#include<stack>
#include<vector>
using namespace std;
int main(){
	int n,m,k;
	scanf("%d%d%d",&m,&n,&k);
	while(k--){
		vector<int> v(n+1);
		stack<int> st;
		bool flag=true;
		int current=1;
		for(int i=1;i<=n;i++) scanf("%d",&v[i]);
		for(int i=1;i<=n;i++){
			st.push(i);
			if(st.size()>m){
				flag=false;
				break;
			}
			while(!st.empty()&&st.top()==v[current]){
				st.pop();
				current++;
			}
		}
		if(current!=n+1) flag=false;
		if(flag) printf("YES\n");
		else printf("NO\n");
	}
	return 0;
}
```





### A1052(25 链表按key排序)

```
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;
const int maxn=100010;
struct node{
	int data,address,next;
}list[maxn]; 
bool cmp(node a,node b){return a.data<b.data;}
int main(){
	int n,begin,address;
	vector<node> v;
	scanf("%d%d",&n,&begin);
	for(int i=0;i<n;i++){
		scanf("%d",&address);
		scanf("%d%d",&list[address].data,&list[address].next);
		list[address].address=address;
	}
	while(begin!=-1){
		v.push_back(list[begin]);
		begin=list[begin].next;
	}
    if(v.size()==0) printf("0 -1");
    else{
        sort(v.begin(),v.end(),cmp);
	    printf("%d %05d\n",v.size(),v[0].address);
	    for(int i=0;i<v.size()-1;i++) printf("%05d %d %05d\n",v[i].address,v[i].data,v[i+1].address);
	    printf("%05d %d -1\n",v[v.size()-1].address,v[v.size()-1].data);
    }
    return 0;
}
```





### A1053(30 输出权值总和同为给定值的路径(dfs))

```
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;
int n,m,s,sum=0,path[101];
struct node{
	int w;
	vector<int> child;
}tree[101];
bool cmp(int a,int b){return tree[a].w>tree[b].w;}
void dfs(int index,int nodenum,int sum){
	if(sum>s) return;
	if(sum==s){
		if(tree[index].child.size()!=0) return;
		for(int i=0;i<nodenum;i++){
			if(i>0) printf(" ");
			printf("%d",tree[path[i]].w);
		}
		printf("\n");
		return;
	}
	for(int i=0;i<tree[index].child.size();i++){
		int child=tree[index].child[i];
		path[nodenum]=child;
		dfs(child,nodenum+1,sum+tree[child].w);
	} 
}
int main(){
	int id,k,c;
	scanf("%d%d%d",&n,&m,&s);
	for(int i=0;i<n;i++) scanf("%d",&tree[i].w);
	for(int i=0;i<m;i++){
		scanf("%d%d",&id,&k);
		for(int j=0;j<k;j++){
			scanf("%d",&c);
			tree[id].child.push_back(c);
		}
		sort(tree[id].child.begin(),tree[id].child.end(),cmp);
	}
	path[0]=0;
	dfs(0,1,tree[0].w);
	return 0;
}
```



### A1054(20 map)

```
#include<cstdio>
#include<map>
using namespace std;
int main(){
	int m,n,temp;
	scanf("%d%d",&m,&n);
	map<int,int> mp;
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			scanf("%d",&temp);
			mp[temp]++;
		}
	}
	int times=-1,ans;
	for(auto it=mp.begin();it!=mp.end();it++){
		if(it->second>times){
			times=it->second;
			ans=it->first;
		} 
	} 
	printf("%d",ans);
	return 0;
}
```



### A1055(25 按条件查找并排序输出)

```
#include<algorithm>
#include<vector>
#include<cstring>
#include<iostream>
using namespace std;
struct node{
	char name[10];
	int age,worth;
};
bool cmp(node a,node b){
	if(a.worth!=b.worth) return a.worth>b.worth;
	else if(a.age!=b.age) return a.age<b.age;
	else return strcmp(a.name,b.name)<0;
}
int main(){
	int n,m,k,amin,amax;
	scanf("%d%d",&n,&m);
	vector<node> t(n),v;
    vector<int> book(201,0);
	for(int i=0;i<n;i++)  scanf("%s%d%d",t[i].name,&t[i].age,&t[i].worth);
    sort(t.begin(),t.end(),cmp);   
    for(int i=0;i<n;i++){
        if(book[t[i].age]<100){
            v.push_back(t[i]);
            book[t[i].age]++;
        }
    }
	for(int i=1;i<=m;i++) {
		printf("Case #%d:\n",i);
		vector<node> ans;
        scanf("%d%d%d",&k,&amin,&amax);
		for(int j=0;j<v.size();j++) {
			if(v[j].age>=amin&&v[j].age<=amax) ans.push_back(v[j]);
		}
		if(ans.size()==0) printf("None\n");
		else if(k>=ans.size()){
			for(int j=0;j<ans.size();j++) printf("%s %d %d\n",ans[j].name,ans[j].age,ans[j].worth);
		}else if(k<ans.size())  {
			for(int j=0;j<k;j++) printf("%s %d %d\n",ans[j].name,ans[j].age,ans[j].worth);
		}   
	}
	return 0;
} 
```



### A1056(25 queue队列)

```
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;
struct node{
	int w,rank;
};
int main(){
	int np,ng,orderid;
	queue<int> q;
	scanf("%d%d",&np,&ng);
	vector<node> v(np);
	for(int i=0;i<np;i++) scanf("%d",&v[i].w);
	for(int i=0;i<np;i++) {
		scanf("%d",&orderid);
		q.push(orderid);
	}
	int temp=np,group;
	while(q.size()!=1){
		if(temp%ng==0) group=temp/ng;
		else group=temp/ng+1;
		for(int i=0;i<group;i++){
			int k=q.front();
			for(int j=1;j<=ng;j++){
				if(i*ng+j>temp) break;
				int front=q.front();
				if(v[front].w>v[k].w) k=front;
				v[front].rank=group+1;
				q.pop();
			}
			q.push(k); 
		}
		temp=group; 
	}
	v[q.front()].rank=1;
	for(int i=0;i<np;i++){
		if(i!=0) printf(" ");
		printf("%d",v[i].rank);
	}
	return 0;
} 
```





### A1057(30 分块、树状数组)

```
1.分块
#include<cstdio>
#include<cstring>
#include<stack>
using namespace std;
int table[100010],block[317];
stack<int> st;
void Peekmedian(int x) {
	int sum = 0, block_id = 0;
	while (sum + block[block_id] < x) {
		sum += block[block_id++];
	}
	int num = block_id * 316;
	while (sum + table[num] < x) sum+=table[num++];
	printf("%d\n", num);
}
void Push(int x) {
	st.push(x);
	table[x]++;
	block[x / 316]++;
}
void Pop() {
	int top = st.top();
	st.pop();
	table[top]--;
	block[top / 316]--;
	printf("%d\n", top);
}
int main() {
	int n, temp;
	char cmd[15];
	scanf("%d", &n);
	memset(table, 0, sizeof(table));
	memset(block, 0, sizeof(block));
	for (int i = 0; i < n; i++) {
		scanf("%s", cmd);
		if (strcmp(cmd, "Pop")==0) {
			if (st.empty()) printf("Invalid\n");
			else Pop();
		}
		else if (strcmp(cmd, "Push")==0) {
			scanf("%d", &temp);
			Push(temp);
		}
		else {
			if (st.empty()) printf("Invalid\n");
			else {
				int k = st.size();
				if (k % 2 == 0) k = k / 2;
				else k = k / 2 + 1;
				Peekmedian(k);
			}
		}
	}
	return 0;
}
2.树状数组

```



### A1058(20 水题)

```
#include<cstdio>
int main(){
	int a,b,c,d,e,f,ans[3];
	scanf("%d.%d.%d %d.%d.%d",&a,&b,&c,&d,&e,&f);
	ans[2]=(c+f)%29;
	ans[1]=(b+e+(c+f)/29)%17;
	ans[0]=a+d+(b+e+(c+f)/29)/17;
	for(int i=0;i<3;i++){
		if(i!=0) printf(".");
		printf("%d",ans[i]);
	}
	return 0;
}
```





### A1059(25 建立素数表 求素数因数)

```
#include<cstdio>
#include<cmath>
#include<vector>
using namespace std;
const int maxn=105000;
vector<int> prime;
long long n;
bool isprime(int n) {
	if(n<=1) return false;
	int sqr=sqrt(n*1.0);
	for(int i=2; i<=sqr; i++) {
		if(n%i==0) return false;
	}
	return true;
}
int main() {
	int index=0;
	scanf("%lld",&n);
	printf("%lld=",n);
	for(int i=2; i<=maxn; i++) {
		if(isprime(i)) prime.push_back(i);
	}
	if(n==1) printf("1");
	else {
		int hasprint=0;
		for(int i=0; n>=2; i++) {
			int cnt=0,flag=0;
			while(n%prime[i]==0) {
				n/=prime[i];
				cnt++;
				flag=1;
			}
			if(flag){
				if(hasprint) printf("*");
				printf("%lld",prime[i]);
				hasprint=1;
			}
			if(cnt>=2) printf("^%d",cnt);
		}
	}
	return 0;
}
```



### A1060(25 字符串 科学计数法)

```
#include<iostream>
#include<string>
using namespace std;
int n;
string deal(string s,int &e){
	int k=0;
	while(s[0]=='0'&&s.length()>0) s.erase(s.begin());//去除前导0 
	if(s[0]=='.'){//若去除后是小数点，说明是小于1的数 
		s.erase(s.begin());
		while(s[0]=='0'&&s.length()>0) {
			s.erase(s.begin());//去除小数点后非零位前的所有0 
			e--;
		}
	}else{
		while(s[k]!='.'&&k<s.length()){//寻找小数点 
			k++;
			e++;
		}
		if(k<s.length()) s.erase(s.begin()+k);//删除小数点 
	}
	if(s.length()==0) e=0;
	int num=0;
	k=0;
	string ans;
	while(num<n){
		if(k<s.length()) ans+=s[k++];
		else ans+='0';
		num++;
	}	
	return ans; 
}		
int main(){
	string s1,s2,s3,s4;
	cin>>n>>s1>>s2;
	int e1=0,e2=0;
	s3=deal(s1,e1);
	s4=deal(s2,e2);
	if(s3==s4&&e1==e2) cout<<"YES"<<" 0."<<s3<<"*10^"<<e1;	
	else cout<<"NO"<<" 0."<<s3<<"*10^"<<e1<<" 0."<<s4<<"*10^"<<e2;
	return 0;
}
```



### A1061(20 字符串)

```
#include<iostream>
#include<string>
#include<cctype>
using namespace std;
int main(){
	int i=0,pos=0;
	char ans[2];
	string a,b,c,d;
	cin>>a>>b>>c>>d;
	while(i<a.length()&&i<b.length()){
		if(a[i]==b[i]&&(a[i]>='A'&&a[i]<='G')){
			ans[0]=a[i];
			break;
		}
		i++;
	}
	++i;
	while(i<a.length()&&i<b.length()){
		if(a[i]==b[i]&&((a[i]>='A'&&a[i]<='N')||isdigit(a[i]))){
			ans[1]=a[i];
			break;
		}
		i++;
	}
	while(pos<c.length()&&pos<d.length()){
		if(c[pos]==d[pos]&&isalpha(c[pos])) break;
		pos++;
	}
	string weekday[7]={"MON","TUE","WED","THU","FRI","SAT","SUN"};
	printf("%s %02d:%02d",weekday[ans[0]-'A'].c_str(),isdigit(ans[1])?ans[1]-'0':ans[1]-'A'+10,pos);
	return 0;
} 
```





### A1062

```
#include<cstdio>
#include<algorithm>
#include<vector> 
using namespace std;
struct node{
	int num;
	int de,cai,rank;
}temp;
bool cmp( node a,node b){
	if(a.rank!=b.rank) return a.rank<b.rank;
	else if((a.de+a.cai)!=(b.de+b.cai)) return (a.de+a.cai)>(b.de+b.cai);
	else if(a.de!=b.de) return a.de>b.de;
	else return a.num<b.num;
}
int main(){
	int n,L,H;
	scanf("%d%d%d",&n,&L,&H);
	vector<node> v;
	for(int i=0;i<n;i++){
		int id,a,b,rank;
		scanf("%d%d%d",&id,&a,&b);
		if(a<L||b<L) continue;
		if(a>=H&&b>=H) rank=1;
		else if(a>=H&&b<H) rank=2;
		else if(a<H&&b<H&&a>=b) rank=3;
		else rank=4;
		v.push_back(node{id,a,b,rank});
	}
	sort(v.begin(),v.end(),cmp);
	printf("%d\n",v.size());
	for(int i=0;i<v.size();i++) printf("%08d %d %d\n",v[i].num,v[i].de,v[i].cai);
	return 0;
} 
```





### A1063(25 set)

```
#include<cstdio>
#include<set>
using namespace std;
const int maxn = 51;
set<int> v[maxn];
int main() {
	int n,m,temp,k;
	scanf("%d", &n);
	for (int i = 0; i < n; i++) {
		scanf("%d", &m);
		for (int j = 0; j < m; j++) {
			scanf("%d", &temp);
			v[i].insert(temp);
		}
	}
	int a, b;
	scanf("%d", &k);
	for (int i = 0; i < k; i++) {
		scanf("%d%d", &a, &b);
		int nc=0, nt=v[b-1].size();
		for (auto it = v[a - 1].begin(); it != v[a - 1].end(); it++) {
			if (v[b - 1].find(*it) != v[b - 1].end())  nc++;
			else nt++;
		}
		printf("%.1f%%\n", (nc * 100.0) / nt);
	}
}
```





### A1064(30 完全二叉树(中序遍历建树))

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=1001;
int CBT[maxn],a[maxn],n,index=0;
void inorder(int root){
	if(root>n) return;
	inorder(root*2);
	CBT[root]=a[index++];
	inorder(root*2+1);
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d",&a[i]);
	sort(a,a+n);
	inorder(1);
	for(int i=1;i<=n;i++){
		if(i>1) printf(" ");
		printf("%d",CBT[i]);
	}
	return 0;
}
```



### A1065 

```
#include<cstdio>
int main() {
	long long a, b, c;
	int n,times=1;
	scanf("%d", &n);
	while (n--) {
		scanf("%lld%lld%lld", &a, &b, &c);
		bool flag;
		long long sum = a + b;
		if (a > 0 && b > 0 && sum < 0)
			flag = true;
		else if (a < 0 && b < 0 && sum >= 0)
			flag = false;
		else if (sum > c)
			flag = true;
		else flag = false;
		if (flag == true)
			printf("Case #%d:true", times++);
		if (flag == false)
			printf("Case #%d:false", times++);
	}
	return 0;
}
```





### A1066(25 建立平衡二叉树)

```
#include<cstdio>
#include<algorithm>
using namespace std;
struct node{
	int v,height;
	node*left,*right;
};
node* newnode(int v){
	node* Node=new node;
	Node->v=v;
	Node->height=1;
	Node->left=Node->right=NULL;
	return Node;
}
int getheight(node*root){
	if(root==NULL) return 0;
	return root->height; 
}
void updateheight(node *root){
	root->height=max(getheight(root->right),getheight(root->left))+1;
}
int getbalance(node* root){
	return getheight(root->left)-getheight(root->right);
}
void L(node*&root){
	node*temp=root->right;
	root->right=temp->left;
	temp->left=root;
	updateheight(root);
	updateheight(temp);
	root=temp;
}
void R(node*&root){
	node*temp=root->left;
	root->left=temp->right;
	temp->right=root;
	updateheight(root);
	updateheight(temp);
	root=temp;
}
void insert(node*&root,int v){
	if(root==NULL){
		root=newnode(v);
		return;
	}
	if(v<root->v){
		insert(root->left,v);
		updateheight(root);
		if(getbalance(root)==2){
			if(getbalance(root->left)==1) R(root);
			else if(getbalance(root->left)==-1){
				L(root->left);
				R(root);
			}
		}
	}
	else{
		insert(root->right,v);
		updateheight(root);
		if(getbalance(root)==-2){
			if(getbalance(root->right)==-1) L(root);
			else if(getbalance(root->right)==1){
				R(root->right);
				L(root);
			}
		}
	}
}
int main(){
	int n,temp;
	node* root=NULL;
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		scanf("%d",&temp);
		insert(root,temp);
	}
	printf("%d",root->v);
	return 0;
}
```





### A1067(25  贪心 swap排序，求最小交换次数)

```
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
	int n,t,cnt=0,a[100001];
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>t;
		a[t]=i;
	}
	for(int i=1;i<n;i++){
		while(a[0]!=0){
			swap(a[0],a[a[0]]);
			cnt++;
		}
		if(a[i]!=i){
			swap(a[i],a[0]);
			cnt++;
		}
	} 
	cout<<cnt;
	return 0;
}
```





### A1068

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=10010;
const int maxv=110;
int dp[maxv]={0},w[maxn];
bool choice[maxn][maxv],flag[maxn];
bool cmp(int a,int b){
	return a>b;
}
int main(){
	int n,m;
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++){
		scanf("%d",&w[i]);
	}
	sort(w+1,w+1+n,cmp);
	for(int i=1;i<=n;i++){
		for(int v=m;v>=w[i];v--){
			if(dp[v]<=dp[v-w[i]]+w[i]){
				dp[v]=dp[v-w[i]]+w[i];
				choice[i][v]=1;
			}else{
				choice[i][v]=0;
			}
		}
	}
	if(dp[m]!=m) printf("No Solution");
	else{
		int k=n,num=0,v=m;
		while(k>=0){
			if(choice[k][v]==1){
				flag[k]=true;
				v-=w[k];
				num++;
			}else flag[k]=false;
			k--;
		}
		for(int i=n;i>0;i--){
			if(flag[i]==true){
				printf("%d",w[i]);
				num--;
				if(num>0) printf(" ");
			}
		}
	}
	return 0;
}
```



### A1069(20 数字黑洞)

```
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;
string strsort(string s){
	sort(s.begin(),s.end());
	s.insert(0,4-s.size(),'0');
	return s;
}
string rev(string s){
	reverse(s.begin(),s.end());
	return s;
}
int main(){
	string s;
	cin>>s;
	s.insert(0, 4 - s.length(), '0');
	if(rev(s)==s){
		cout<<s<<" - "<<s<<" = "<<"0000";
		return 0;
	}
	int ans=1;
	while(1){
		s=strsort(s);
		string s1=rev(s);
		ans=stoi(s1)-stoi(s);
		printf("%s - %s = %04d\n",s1.c_str(),s.c_str(),ans);
		if(ans==6174) return 0;
		s=to_string(ans);
	}
	return 0;
}
```





### A1070(25 贪心)

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	double inventory,price;
};
bool cmp(node &a,node &b){ return a.price>b.price;};
int main(){
	int n,D;
	scanf("%d%d",&n,&D);
	double prices,profit=0.0;
	vector<node> v(n);
	for(int i=0;i<n;i++) scanf("%lf",&v[i].inventory);
	for(int i=0;i<n;i++){
		scanf("%lf",&prices);
		v[i].price=prices/v[i].inventory;
	}
	sort(v.begin(),v.end(),cmp);
	for(int i=0;i<n;i++){
		if(v[i].inventory>=D){
			profit+=D*v[i].price;
			break;
		}else{
			profit+=v[i].inventory*v[i].price;
			D-=v[i].inventory;
		}
	}
	printf("%.2f",profit);
	return 0;
}
```





### A1071(25 map  字符串)

```
#include<string>
#include<map>
#include<cctype>
#include<iostream>
using namespace std;
int main(){
	map<string,int> mp;
	string s,temp,ans;
	getline(cin,s);
	int maxtimes=-1;
	for(int i=0;i<s.length();i++){
		if(isalnum(s[i])){
			if(isupper(s[i])) s[i]=tolower(s[i]);
			temp+=s[i];
		}
		if(!isalnum(s[i])||i==s.length()-1){
			if(!temp.empty()) mp[temp]++;
			temp.clear();
		}
	}
	for(auto it=mp.begin();it!=mp.end();it++){
		if(it->second>maxtimes) {
			ans=it->first;
			maxtimes=it->second;
		}
	}
	cout<<ans<<" "<<maxtimes;
	return 0;
}
```





### A1072(30 多源dijkst求最佳源)

```
#include<iostream>
#include<string>
#include<algorithm> 
#include<map>
using namespace std;
const int maxn=1100;
const int INF=0x7fffffff;
int n,m,k,ds,G[maxn][maxn],d[maxn],index_g=n+1;
bool vis[maxn];
map<string,int> mp;
void dijkst(int s){
	fill(d,d+maxn,INF);
	d[s]=0;
	fill(vis,vis+maxn,false);
	for(int i=1;i<=n+m;i++){
		int u=-1,MIN=INF;
		for(int j=1;j<=n+m;j++){
			if(vis[j]==false&&d[j]<MIN){
				u=j;
				MIN=d[j];
			}
		}
		if(u==-1) return;
		vis[u]=true;
		for(int v=1;v<=n+m;v++){
			if(vis[v]==false&&G[u][v]!=INF){
				if(d[u]+G[u][v]<d[v]) d[v]=d[u]+G[u][v];
			}
		}
	}
}
int getid(string s){
	if(s[0]=='G'){
		s.erase(s.begin());
		if(mp.count(s)==0) mp[s]=stoi(s)+n;
		return mp[s];
	}
	else return stoi(s);
}
int main(){
	int ansid=-1;
	double ansavg=INF,ansmindis=-1;
	string s1,s2;
	fill(G[0],G[0]+maxn*maxn,INF);
	scanf("%d%d%d%d",&n,&m,&k,&ds);
	while(k--){
		cin>>s1>>s2;
		int v1=getid(s1);
		int v2=getid(s2);
		cin>>G[v1][v2];
		G[v2][v1]=G[v1][v2];
	}
	for(int i=n+1;i<=n+m;i++){
		double mindis=INF,avg=0,sum=0;
		dijkst(i);
		for(int j=1;j<=n;j++){
			if(d[j]>ds){
				mindis=-1;
				break;
			}
			if(d[j]<mindis) mindis=d[j];
			avg+=1.0*d[j]/n;
		}
		if(mindis==-1) continue;
		if(mindis>ansmindis){
			ansid=i;
			ansmindis=mindis;
			ansavg=avg;
		}else if(mindis==ansmindis&&ansavg>avg){
			ansid=i;
			ansavg=avg;
		}
	}
	if(ansid==-1) printf("No Solution");
	else{
		printf("G%d\n",ansid-n);
	    printf("%.1f %.1f",ansmindis,ansavg);
	}
	return 0;
}
```



### A1073 科学计数法

```
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;
int main(){
	string s,t;
	int n,i=0;
	cin>>s;
	while(s[i]!='E') i++;
	t=s.substr(1,i-1);
	n=stoi(s.substr(i+1,s.length()-i-1));
	if(s[0]=='-') cout<<"-";
	if(n<0){
		cout<<"0.";
		for(int j=0;j<abs(n)-1;j++) cout<<"0";
		for(int j=0;j<t.size();j++) 
		    if(t[j]!='.') cout<<t[j];
	}else{
		cout<<t[0];
		int cnt,j;
		for(j=2,cnt=0;j<t.length()&&cnt<n;j++,cnt++) cout<<t[j];
		if(j==t.length()) {
			for(int k=0;k<n-cnt;k++) cout<<"0";
		} else{
			cout<<".";
			for(int k=j;k<t.size();k++) cout<<t[k];
		}
	}
	return 0;
}
```





### A1074(25 反转链表)

```
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=100010;
struct node{
	int address,data,next;
	int order;
}list[maxn];
bool cmp(node &a,node &b){
	return a.order<b.order;
}
int main(){
	int begin,n,k,address,cnt=0;
	for(int i=0;i<maxn;i++想·) list[i].order=maxn;
	scanf("%d%d%d",&begin,&n,&k);
	for(int i=0;i<n;i++){
		scanf("%d",&address);
		scanf("%d%d",&list[address].data,&list[address].next);
		list[address].address=address;
	}
	int p=begin;
	while(p!=-1){
		list[p].order=cnt++;
		p=list[p].next;
	}
	sort(list,list+maxn,cmp);
	for(int i=0;i<cnt/k;i++){
		for(int j=(i+1)*k-1;j>i*k;j--){
			printf("%05d %d %05d\n",list[j].address,list[j].data,list[j-1].address);
		}
		printf("%05d %d ",list[i*k].address,list[i*k].data);
		if(i<cnt/k-1) printf("%05d\n",list[(i+2)*k-1].address);
		else{
			if(cnt%k==0) printf("-1\n");
			else{
				printf("%05d\n",list[(i+1)*k].address);
				for(int j=(i+1)*k;j<cnt-1;j++){
				    printf("%05d %d %05d\n",list[j].address,list[j].data,list[j+1].address);
				}
				printf("%05d %d -1",list[cnt-1].address,list[cnt-1].data);
			}
		}
	}
	return 0;
}
```





### A1075(25 排序)

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	int rank,id,total=0;
	vector<int> score;
	int passnum=0;
	bool isshown=false;
}; 
bool cmp(node a,node b){
	if(a.total!=b.total) return a.total>b.total;
	else if(a.passnum!=b.passnum) return a.passnum>b.passnum;
	else return a.id<b.id;
}
int main(){
	int n,k,m,id,index,grade;
	scanf("%d%d%d",&n,&k,&m);
	vector<node> v(n+1);
	for(int i=1;i<=n;i++) v[i].score.resize(k+1,-1);
	vector<int> p(k+1);
	for(int i=1;i<=k;i++) scanf("%d",&p[i]);
	for(int i=0;i<m;i++){
	    scanf("%d%d%d",&id,&index,&grade);
	    v[id].id=id;
	    v[id].score[index]=max(grade, v[id].score[index]);
	    if(grade!=-1) v[id].isshown=true;
	    else if(v[id].score[index]==-1) v[id].score[index]=-2;
	}
	for(int i=1;i<=n;i++){
		for(int j=1;j<=k;j++){
			if(v[i].score[j]!=-1&&v[i].score[j]!=-2) v[i].total+=v[i].score[j];
			if(v[i].score[j]==p[j]) v[i].passnum++;
		}
	}
	sort(v.begin()+1,v.end(),cmp);
	v[1].rank=1;
	for(int i=2;i<=n;i++){
		if(v[i].total==v[i-1].total) v[i].rank=v[i-1].rank;
		else v[i].rank=i;
	}
	for(int i=1;i<=n;i++){
		if(v[i].isshown==true){
			printf("%d %05d %d",v[i].rank,v[i].id,v[i].total);
			for(int j=1;j<=k;j++) {
				if(v[i].score[j]==-1) printf(" -");
				else if(v[i].score[j]==-2) printf(" 0");
				else printf(" %d",v[i].score[j]);
			}
			printf("\n");
		}
	}
	return 0;
} 
```



### A1076(30 图的bfs、记录给定最大层数上的结点和)

```
#include<cstdio>
#include<queue>
#include<algorithm>
#include<vector>
using namespace std;
int n,l,k;
bool inq[1010];
vector<vector<int> > v;
struct node{
	int id,level;
};
int bfs(node a){
	fill(inq,inq+1010,false);
	int cnt=0;
	queue<node> q;
	q.push(a);
	inq[a.id]=true;
	while(!q.empty()){
		node t=q.front();
		q.pop();
		int tid=t.id;
		for(int i=0;i<v[tid].size();i++){
			int nextid=v[tid][i];
			if(inq[nextid]==false&&t.level<l){
				cnt++;
				node nextnode={nextid,t.level+1};
				q.push(nextnode);
				inq[nextid]=true;
			}
		}
	}
	return cnt;
}
int main(){
	scanf("%d%d",&n,&l);
	v.resize(n+1);
	int num,temp;
	for(int i=1;i<=n;i++){
		scanf("%d",&num);
		while(num--){
			scanf("%d",&temp);
			v[temp].push_back(i);
		}
	}
	scanf("%d",&k);
	while(k--){
		scanf("%d",&temp);
		node tnode={temp,0};
		printf("%d\n",bfs(tnode));
	}
	return 0;
}
```





### A1077(20  多个字符串找公共后缀)

```
#include<iostream>
#include<string>
#include<algorithm>
using namespace std;
int main(){
	int n;
	scanf("%d\n",&n);
	string ans;
	int minlen;
	for(int i=0;i<n;i++){
		string s;
		getline(cin,s);
		reverse(s.begin(),s.end());
		if(i==0){
			ans=s;
			minlen=ans.length();
		}else{
			int len=s.length();
			minlen=min(minlen,len);
			for(int j=0;j<minlen;j++){
				if(s[j]!=ans[j]) {
					ans=s.substr(0,j);
					break;
				}
			}
		}
	}
	reverse(ans.begin(),ans.end());
	if(ans.size()==0) cout<<"nai";
	else cout<<ans;
	return 0;
}
```





### A1078(25 hash 平方探查)

```
#include<cstdio>
#include<algorithm>
#include<cmath>
#include<vector>
using namespace std;
bool isprime(int n){
	if(n<=1) return false;
	int sqr=sqrt(1.0*n);
	for(int i=2;i<=sqr;i++){
		if(n%i==0) return false;
	}
	return true;
}
int main(){
	int msize,n,temp,cnt=0;
	scanf("%d%d",&msize,&n);
	while(!isprime(msize)) msize++;
	vector<int> v(msize,0);
	for(int i=0;i<n;i++){
		scanf("%d",&temp);
		int flag=0,ans;
		for(int j=0;j<msize;j++){
			int pos=(temp+j*j)%msize;
			if(v[pos]==0){
				v[pos]=temp;
				flag=1;
				ans=pos;
				break;
			}
		}
		if(cnt!=0) printf(" ");
		cnt++;
		if(flag) printf("%d",ans);
		else printf("-");
	}
	return 0;
}
```





### A1079(25 树的dfs)

```
#include<cstdio>
#include<vector>
#include<cmath>
using namespace std;
int n;
double p,r,ans=0.0;
struct node{
	int data;
	vector<int> child;
}tree[100010]; 
void dfs(int index,int depth){
	if(tree[index].child.size()==0){
		ans+=tree[index].data*p*pow((1+r/100),depth);
		return;
	}
	for(int i=0;i<tree[index].child.size();i++) dfs(tree[index].child[i],depth+1);
}
int main(){
	scanf("%d %lf %lf",&n,&p,&r);
	int num,root,ischild[100010]={0},temp;
	for(int i=0;i<n;i++){
		scanf("%d",&num);
		if(num==0){
			scanf("%d",&temp);
			tree[i].data=temp;
		}else{
			for(int j=0;j<num;j++) {
				scanf("%d",&temp);
				ischild[temp]=1;
				tree[i].child.push_back(temp);
			}	
		}
	}
	for(int i=0;i<n;i++){
		if(ischild[i]==0){
			root=i;
			break;
		}
	}
	dfs(root,0);
	printf("%.1f",ans);
	return 0;
}
```



### A1080(30 排序)

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	int ge,gi,sum,rank,id;
	vector<int> choice;
};
bool cmp1(node& a,node& b){
	if(a.sum!=b.sum) return a.sum>b.sum;
	else return a.ge>b.ge;
}
bool cmp2(node& a,node& b){
	return a.id<b.id;
}
int main(){
	int n,m,k;
	scanf("%d%d%d",&n,&m,&k);
	vector<int> sch_need(m);
	vector<node> stu(n);
	for(int i=0;i<m;i++) scanf("%d",&sch_need[i]);
	for(int i=0;i<n;i++){
		scanf("%d%d",&stu[i].ge,&stu[i].gi);
		stu[i].sum=stu[i].ge+stu[i].gi;
		stu[i].id=i;
		stu[i].choice.resize(k);
		for(int j=0;j<k;j++) scanf("%d",&stu[i].choice[j]);
	}
	sort(stu.begin(),stu.end(),cmp1);
	vector<node> ans[m];
	for(int i=0;i<n;i++){
		for(int j=0;j<k;j++){
			int temp=stu[i].choice[j];
			if(ans[temp].size()<sch_need[temp]||
			(ans[temp][ans[temp].size()-1].sum==stu[i].sum&&ans[temp][ans[temp].size()-1].ge==stu[i].ge)) {
				ans[temp].push_back(stu[i]);
				break;
			}
		}
	}
	for(int i=0;i<m;i++){
		sort(ans[i].begin(),ans[i].end(),cmp2);
		for(int j=0;j<ans[i].size();j++){
			if(j!=0) printf(" ");
			printf("%d",ans[i][j].id);
		}
		printf("\n");	
	}
	return 0;
}
```







### A1081(20 分数相加)

```
方法一:
#include<cstdio>
long long gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}
int main() {
	int n;
	scanf("%d", &n);
	long long a2 = 0, b2 = 1,gcdvalue;
	for (int i = 0; i < n; i++) {
		long long a, b;
		scanf("%lld/%lld", &a, &b);
		gcdvalue = gcd(a, b);
		a /= gcdvalue;
		b /= gcdvalue;
		a2 = a * b2 + b * a2;
		b2 = b * b2;
		gcdvalue = gcd(a2, b2);
		a2 /= gcdvalue;
		b2 /= gcdvalue;
	}
	long long integer = a2 / b2;
	a2 = a2 - integer * b2;
	if (integer != 0) {
		printf("%lld", integer);
		if (a2 > 0) printf(" %lld/%lld",a2,b2);
		else if (a2 < 0) printf(" %lld/%lld", -a2, b2);
	}
	if (integer == 0 && a2 != 0) printf("%lld/%lld", a2, b2);
	if (integer == 0 && a2 == 0) printf("0");
	return 0;
}
方法二:
#include<cstdio>
#include<algorithm>
using namespace std;
struct node{
	long long up,down;
};
long long gcd(long long a,long long b){
	return b==0? abs(a) : gcd(b,a%b);
}
node reduction(node res){
	if(res.down<0){
		res.up=-res.up;
		res.down=-res.down;
	}
	if(res.up==0) res.down=1;
	else{
		long long d=gcd(abs(res.up),abs(res.down));
		res.up/=d;
		res.down/=d;
	}
	return res;
}
node add(node f1,node f2){
	node res;
	res.up=f1.down*f2.up+f1.up*f2.down;
	res.down=f1.down*f2.down;
	return reduction(res);
}
void showres(node res){
	res=reduction(res);
	if(res.down==1) printf("%lld\n",res.up);
	else if(abs(res.up)>abs(res.down)){
		printf("%lld %lld/%lld\n",res.up/res.down,abs(res.up%res.down),res.down);
	}else printf("%lld/%lld\n",res.up,res.down);
}
int main(){
	int n;
	scanf("%d",&n);
	node temp,ans;
	ans.up=0,ans.down=1;
	for(int i=0;i<n;i++){
		scanf("%lld/%lld",&temp.up,&temp.down);
		ans=add(ans,temp);
	} 
	showres(ans);
	return 0;
}
```



### A1082(25 分段处理字符串)

```
#include<iostream>
#include<string>
using namespace std;
int main(){
	string n;
	cin>>n;
	int len=n.length();
	int left=0,right=len-1;
	string num[10]={"ling","yi","er","san","si","wu","liu","qi","ba","jiu"};
	string wei[5]={"Shi","Bai","Qian","Wan","Yi"};
	if(n[0]=='-') {
		cout<<"Fu";
		left++;
	}
	while(left+4<=right){
		right-=4;
	}
	while(left<len){
		bool flag=false;
		bool hasprint=false;
		while(left<=right){
			if(left>0&&n[left]=='0') flag=true;
			else{
				if(flag==true){
					cout<<" "<<"ling";
					flag=false;
				}
				if(left>0) cout<<" ";
				cout<<num[n[left]-'0'];
				hasprint=true;
				if(left!=right) cout<<" "<<wei[right-left-1];
			}
			left++;
		}
		if(right!=len-1&&hasprint==true)	cout<<" "<<wei[(len-1-right)/4+2]; 
		right+=4; 
	}
	return 0;
} 
```





### A1083(25 排序)

```
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;
struct node{
	string name,id;
	int grade;
}stu[1000];
bool cmp(node a,node b){return a.grade>b.grade;}
int main(){
	int n,score,g1,g2;
	string name,id;
	vector<node> ans;
	cin>>n;
	for(int i=0;i<n;i++) cin>>stu[i].name>>stu[i].id>>stu[i].grade;
	cin>>g1>>g2;
	for(int i=0;i<n;i++) {
		if(stu[i].grade>=g1&&stu[i].grade<=g2){
			ans.push_back(stu[i]);
		}
	}
	if(ans.size()==0) cout<<"NONE";
	else{
		sort(ans.begin(),ans.end(),cmp);
		for(int i=0;i<ans.size();i++) printf("%s %s\n",ans[i].name.c_str(),ans[i].id.c_str());
	} 
	return 0;
}
```





### A1084(20 字符串)

```
#include<iostream>
#include<string>
#include<cctype>
using namespace std;
int main(){
	string s1,s2,ans;
	cin>>s1>>s2;
	for(int i=0;i<s1.length();i++){
		if(s2.find(s1[i])==string::npos&&ans.find(toupper(s1[i]))==string::npos)
		  ans+=toupper(s1[i]); 
	}
	cout<<ans;
	return 0; 
}
```



### A1085(25 二分 twopoints 多解法)

```
二分法
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int main(){
	int n,p,ans=0;
	cin>>n>>p;
	vector<int> v(n);
	for(int i=0;i<n;i++)  cin>>v[i];
	sort(v.begin(),v.end());
	for(int i=0;i<n;i++){
		int j=upper_bound(v.begin()+i+1,v.begin()+n,(long long)p*v[i])-v.begin();
		ans=max(ans,j-i);
	}
	cout<<ans;
	return 0;
}






two points
#include<algorithm>
#include<cstdio>
using namespace std;
const int maxn = 100010;
int main() {
	int n, p,num[maxn];
	scanf("%d%d", &n, &p);
	for (int i = 0; i < n; i++) {
		scanf("%d", &num[i]);
	}
	sort(num,num + n);
	int count=0,i=0,j=0;
    while(i<n&&j<n){
        while(j<n&&num[j]<=(long long)num[i]*p){
            count=max(count,j-i+1);
            j++;
        }
        i++;
    }
    printf("%d",count);
    return 0;
}
```





### A1086(25 栈模拟中序和先序建树，输出后序)

```
#include<cstdio>
#include<stack>
#include<cstring>
using namespace std;
const int maxn=31;
int n,pre[maxn],in[maxn],num=0;
struct node{
	int data;
	node*left,*right;
};
node*create(int prel,int prer,int inl,int inr){
	if(prel>prer) return NULL;
	node*root=new node;
	root->data=pre[prel];
	int k=inl;
	while(in[k]!=root->data) k++;
	int leftnum=k-inl;
	root->left=create(prel+1,prel+leftnum,inl,k-1);
	root->right=create(prel+leftnum+1,prer,k+1,inr);
	return root;
}
void postorder(node*root){
	if(root==NULL) return;
	postorder(root->left);
	postorder(root->right);
	printf("%d",root->data);
	num++;
	if(num<n) printf(" ");
}
int main(){
	scanf("%d",&n);
	char s[5];
	stack<int> st;
	int temp,preindex=0,inindex=0;
	for(int i=0;i<2*n;i++){
		scanf("%s",s);
		if(strcmp(s,"Push")==0){
			scanf("%d",&temp);
			st.push(temp);
			pre[preindex++]=temp;
		}else{
			temp=st.top();
			st.pop();
			in[inindex++]=temp;
		}
	}
	node*root=create(0,n-1,0,n-1);
	postorder(root);
	return 0;
}
```





### A1087(30 dijkst+dfs,边权点权、总边权、平均点权)

```
#include<iostream>
#include<map>
#include<string> 
#include<vector>
#include<algorithm>
using namespace std;
const int maxn=210;
const int INF=0x7fffffff;
int n,k,G[maxn][maxn],d[maxn],happ[maxn],maxhapp=0,num=0;
double maxavg=0;
vector<int> pre[maxn];
bool vis[maxn]={false};
map<string,int> cityint;
map<int,string> citystr;
vector<int> temppath,path;
void dijkst(int s){
	fill(vis,vis+maxn,false);
	fill(d,d+maxn,INF);
	d[s]=0;
	for(int i=0;i<n;i++){
		int u=-1,MIN=INF;
		for(int j=0;j<n;j++){
			if(vis[j]==false&&d[j]<MIN){
				u=j;
				MIN=d[j];
			}
		}
		if(u==-1) return;
		vis[u]=true;
		for(int v=0;v<n;v++){
			if(vis[v]==false&&G[u][v]!=INF){
				if(d[u]+G[u][v]<d[v]){
					d[v]=G[u][v]+d[u];
					pre[v].clear();
					pre[v].push_back(u);
				}else if(d[u]+G[u][v]==d[v]) pre[v].push_back(u);
			}
		}
	}
}
void dfs(int s){
	if(s==0){
		num++;
		int temphapp=0;
		temppath.push_back(s);
		for(int i=temppath.size()-2;i>=0;i--) temphapp+=happ[temppath[i]];
		double tempavg=temphapp*1.0/(temppath.size()-1);
		if(temphapp>maxhapp){
			path=temppath;
			maxhapp=temphapp;
			maxavg=tempavg;
		}else if(temphapp==maxhapp&&tempavg>maxavg){
			path=temppath;
			maxavg=tempavg;
		}
		temppath.pop_back();
		return;
	}
	temppath.push_back(s);
	for(int i=0;i<pre[s].size();i++) dfs(pre[s][i]);
	temppath.pop_back();
}
int main(){
	string c1,c2;
	int a,index=0;
	cin>>n>>k>>c1;
	cityint[c1]=index++;
	citystr[cityint[c1]]=c1;
	for(int i=0;i<n-1;i++){
		cin>>c1>>a;
		cityint[c1]=index++;
	    citystr[cityint[c1]]=c1;
		happ[cityint[c1]]=a;
	}
	fill(G[0],G[0]+maxn*maxn,INF);
	while(k--){
		cin>>c1>>c2>>a;
		G[cityint[c1]][cityint[c2]]=a;
		G[cityint[c2]][cityint[c1]]=a;
	}
	dijkst(0);
	dfs(cityint["ROM"]);
	printf("%d %d %d %d\n",num,d[cityint["ROM"]],maxhapp,(int)maxavg);
	for(int i=path.size()-1;i>=0;i--){
		cout<<citystr[path[i]];
		if(i>0) printf("->");
	}
	return 0;
} 
```



### A1088(20  分数的加减乘除)

```
#include<cstdio>
#include<algorithm>
using namespace std;
struct node{
	long long up, down;
};
long long gcd(long long a,long long b){
	return b==0? abs(a) : gcd(b,a%b);
}
node reduction(node res){
	if(res.down<0){
		res.up=-res.up;
		res.down=-res.down;
	}
	if(res.up==0) res.down=1;
	else{
		long long d=gcd(abs(res.up),abs(res.down));
		res.up/=d;
		res.down/=d;
	}
	return res;
}
node add(node f1,node f2){//分数相加
	node res;
	res.up=f1.down*f2.up+f1.up*f2.down;
	res.down=f1.down*f2.down;
	return reduction(res);
}
node minu(node f1,node f2){//分数相减
	node res;
	res.up=f1.up*f2.down-f2.up*f1.down;
	res.down=f1.down*f2.down;
	return reduction(res);
}
node multi(node f1,node f2){//分数相乘
	node res;
	res.up=f1.up*f2.up;
	res.down=f1.down*f2.down;
	return reduction(res);
}
node divide(node f1,node f2){//分数相除
	node res;
	res.up=f1.up*f2.down;
	res.down=f1.down*f2.up;
	return reduction(res);
}
void showres(node res){
	res=reduction(res);
	if(res.up<0) printf("(");
	if(res.down==1) printf("%lld",res.up);
	else if(abs(res.up)>abs(res.down)){
		printf("%lld %lld/%lld",res.up/res.down,abs(res.up%res.down),res.down);
	}else printf("%lld/%lld",res.up,res.down);
	if(res.up<0) printf(")");
}
int main(){
	node a,b,ans;
	scanf("%lld/%lld %lld/%lld",&a.up,&a.down,&b.up,&b.down);
	showres(a);
	printf(" + ");
	showres(b);
	printf(" = ");
	showres(add(a,b));
	printf("\n");
	showres(a);
	printf(" - ");
	showres(b);
	printf(" = ");
	showres(minu(a,b));
	printf("\n");
	showres(a);
	printf(" * ");
	showres(b);
	printf(" = ");
	showres(multi(a,b));
	printf("\n");
	showres(a);
	printf(" / ");
	showres(b);
	printf(" = ");
	if(b.up==0) printf("Inf");
	else showres(divide(a,b));
	printf("\n");
	return 0;
}
```



### A1089  (25 插入排序和归并排序)

```
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
	int a[100],b[100],n,j,m;
	cin>>n;
	for(int i=0;i<n;i++) cin>>a[i];
	for(int i=0;i<n;i++) cin>>b[i];
	for(j=0;j<n-1&&b[j]<=b[j+1];j++);
	for(m=j+1;m<n&&a[m]==b[m];m++);
	if(m==n){
		cout<<"Insertion Sort\n";
		sort(a,a+j+2);
	}else{
		cout<<"Merge Sort\n";
		int flag=1,k=1;
		while(flag){
			flag=0;
			for(int i=0;i<n;i++){
				if(a[i]!=b[i]) flag=1;
			}
			k=k*2;
			for(int i=0;i<n/k;i++) sort(a+i*k,a+(i+1)*k);
			sort(a+(n/k)*k,a+n);
		}
	}
	for(int i=0;i<n;i++){
		if(i!=0) cout<<" ";
		cout<<a[i];
	}
	return 0;
}
```





### A1090(25 树的dfs求深度)

```
#include<cstdio>
#include<vector>
#include<cmath>
using namespace std;
int maxdepth=-1,num,n;
double p,r,ans=0;
vector<int> tree[100010];
void dfs(int index,int depth){
	if(tree[index].size()==0){
		if(depth>maxdepth){
			maxdepth=depth;
			num=1;
		}
		else if(depth==maxdepth) num++;
		return;
	}
	for(int i=0;i<tree[index].size();i++) dfs(tree[index][i],depth+1);
}
int main(){
	scanf("%d %lf %lf",&n,&p,&r);
	int father,root;
	for(int i=0;i<n;i++){
		scanf("%d",&father);
		if(father==-1) root=i;
	    else tree[father].push_back(i);
	}
	dfs(root,0);
	ans=p*pow((1+r/100),maxdepth);
	printf("%.2f %d",ans,num);
	return 0;
}
```



### A1091(30 经典BFS求矩阵中块的个数)

```
#include<cstdio>
#include<queue>
using namespace std;
struct node{
	int x,y,z;
}Node;
int m,n,L,t,ans=0;
bool inq[70][1300][140]={false};
int pix[70][1300][140]={0};
int Z[6]={0,0,0,0,1,-1};
int X[6]={0,0,1,-1,0,0};
int Y[6]={1,-1,0,0,0,0};
bool judge(int z,int y,int x){
	if(x>=n||x<0||y>=m||y<0||z<0||z>=L) return false;
	if(pix[z][y][x]==0||inq[z][y][x]==true) return false;
	return true;
}
int bfs(int z,int y,int x){
	int total=0;
	queue<node> q;
	Node.x=x,Node.y=y,Node.z=z;
	q.push(Node);
	inq[z][y][x]=true;
	while(!q.empty()){
		node Top=q.front();
		q.pop();
		total++;//注意在出队的时候计数 
		for(int i=0;i<6;i++){
			int newz=Top.z+Z[i];
			int newx=Top.x+X[i];
			int newy=Top.y+Y[i];
			if(judge(newz,newy,newx)){
				Node.x=newx,Node.y=newy,Node.z=newz;
				q.push(Node);
				inq[newz][newy][newx]=true;
			}
		}
	}
	if(total>=t) return total;
	else return 0;
}
int main(){
	scanf("%d%d%d%d",&m,&n,&L,&t);
	for(int i=0;i<L;i++){
		for(int j=0;j<m;j++){
			for(int k=0;k<n;k++){
				scanf("%d",&pix[i][j][k]);
			}
		}
	}
	for(int i=0;i<L;i++){
		for(int j=0;j<m;j++){
			for(int k=0;k<n;k++){
				if(inq[i][j][k]==false&&pix[i][j][k]==1) ans+=bfs(i,j,k);
			}
		}
	}
	printf("%d",ans);
	return 0;
}
```



### A1092(字符串 hash)

```
#include<iostream>
#include<string>
#include<vector>
using namespace std;
int main(){
	string s1,s2;
	cin>>s1>>s2;
	vector<int> v(128,0);
	bool can=true;
	int miss=0;
	for(int i=0;i<s1.length();i++)  v[s1[i]]++;
	for(int i=0;i<s2.length();i++){
	    v[s2[i]]-=1;
	    if(v[s2[i]]<0) {
	    	can=false;
	    	++miss;
		}
	}
	if(can==false) cout<<"No "<<miss;
	else cout<<"Yes "<<s1.length()-s2.length();
	return 0;
}
```





### A1093(25 逻辑题)

```
#include<iostream>
#include<string>
using namespace std;
int main(){
	string s;
	cin>>s;
	long long ans=0;
	int num1[100010]={0},num2[100010]={0};
	for(int i=1;i<s.length();i++){
		if(s[i-1]=='P') num1[i]=num1[i-1]+1;
		else num1[i]=num1[i-1];
	}
	for(int i=s.length()-2;i>=0;i--){
		if(s[i+1]=='T') num2[i]=num2[i+1]+1;
		else num2[i]=num2[i+1];
	}
	for(int i=1;i<s.length()-1;i++){
		if(s[i]=='A') ans+=num1[i]*num2[i];
	}
	cout<<ans%1000000007;
	return 0;
}
```



### A1094(25 树的dfs)

```
#include<cstdio>
#include<vector>
using namespace std;
int n,m,level[100]={0},maxdepth=-1,ansnum=-1,anslevel;
vector<int> tree[100];
void dfs(int index,int depth){
	level[depth]++;
	if(tree[index].size()==0){
		if(maxdepth<depth) maxdepth=depth;
		return;
	}
	for(int i=0;i<tree[index].size();i++) dfs(tree[index][i],depth+1);
}
int main(){
	int id,k,temp;
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++){
		scanf("%d%d",&id,&k);
		for(int j=0;j<k;j++){
			scanf("%d",&temp);
			tree[id].push_back(temp);
		}
	}
	dfs(1,1);
	for(int i=1;i<=maxdepth;i++){
		if(level[i]>ansnum){
			ansnum=level[i];
			anslevel=i;
		}
	}
	printf("%d %d",ansnum,anslevel);
	return 0;
}
```





### A1095(30 校园停车 排序  两辆配对  时间查询输出)

```
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <map>
using namespace std;
struct node{
	string id;
	int time,flag=0;
};
bool cmp1(node &a,node &b){
	return a.id!=b.id? a.id<b.id : a.time<b.time;
}
bool cmp2(node &a,node &b){
	return a.time<b.time;
}
int main(){
	int n,m,hh,mm,ss,maxparking=-1,tempindex=0;
	string status;
	scanf("%d%d",&n,&m);
	vector<node> record(n),car;
	for(int i=0;i<n;i++){
		cin>>record[i].id;
		scanf("%d:%d:%d",&hh,&mm,&ss);
		cin>>status;
		record[i].time=hh*3600+mm*60+ss;
		record[i].flag=(status=="in")? 1 : -1;
	}
	sort(record.begin(),record.end(),cmp1);
	map<string,int> mp;
	for(int i=1;i<n;i++){
		if(record[i-1].flag==1&&record[i].flag==-1&&record[i-1].id==record[i].id){
			car.push_back(record[i-1]);
			car.push_back(record[i]);
			mp[record[i].id]+=record[i].time-record[i-1].time;
		    if(maxparking<mp[record[i].id]) maxparking=mp[record[i].id];
		}
	}
	sort(car.begin(),car.end(),cmp2);
	vector<int> cnt(n);
	for(int i=0;i<car.size();i++){
		if(i==0) cnt[i]+=car[i].flag;
		else cnt[i]=cnt[i-1]+car[i].flag;
	}
	
	for(int i=0;i<m;i++){
		scanf("%d:%d:%d",&hh,&mm,&ss);
		int query=hh*3600+mm*60+ss;
		int j;
		for(j=tempindex;j<car.size();j++){
			if(query<car[j].time){
				printf("%d\n",cnt[j-1]);
				break;
			}else if(j==car.size()-1) printf("%d\n",cnt[j]);
		}
		tempindex=j;
	}
	for(auto it=mp.begin();it!=mp.end();it++){
		if(it->second==maxparking) cout<<it->first<<" ";
	}
	printf("%02d:%02d:%02d",maxparking/3600,maxparking%3600/60,maxparking%60);
	return 0;
}
```



### A1096(20 找出连续的因数)

```
#include<iostream>
#include<cmath>
long long n,temp=1;
int main(){
	scanf("%d",&n);
	int st=0,len=0;
	for(int i=2;i<=sqrt(n*1.0);i++){
		int j,temp=1;
		for(j=i;j<=sqrt(n*1.0);j++){
			temp*=j;
			if(n%temp!=0) break;
		}
		if(j-i>len){
			len=j-i;
			st=i;
		}
	}
	if(st==0) printf("1\n%d",n);
	else{
		printf("%d\n",len);
	    for(int i=0;i<len;i++){
		   if(i!=0) printf("*");
		   printf("%d",st+i);
    	}
	}
	
	return 0;
}
```



### A1097(25 链表分离)

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	int address,data,next;
}list[100010];
int main(){
	int begin,n,address;
	scanf("%d%d",&begin,&n);
	vector<int> flag(10010,0);
	vector<node> remain,remov;
	for(int i=0;i<n;i++){
		scanf("%d",&address);
		scanf("%d%d",&list[address].data,&list[address].next);
		list[address].address=address;
	}
	while(begin!=-1){
		if(flag[abs(list[begin].data)]==0) {
			flag[abs(list[begin].data)]=1;
			remain.push_back(list[begin]);
		}else remov.push_back(list[begin]);
		begin=list[begin].next;
	}
	for(int i=0;i<remain.size()-1;i++) printf("%05d %d %05d\n",remain[i].address,remain[i].data,remain[i+1].address);		
	printf("%05d %d -1\n",remain[remain.size()-1].address,remain[remain.size()-1].data);
	if(remov.size()!=0){
		for(int i=0;i<remov.size()-1;i++) printf("%05d %d %05d\n",remov[i].address,remov[i].data,remov[i+1].address);		
     	printf("%05d %d -1",remov[remov.size()-1].address,remov[remov.size()-1].data);
	}
	return 0;
}
```





### A1098(25 判断插入排序和堆排序)

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
void downadjust(vector<int>&b,int low,int high){
	int i=low,j=i*2;
	while(j<=high){
		if(j+1<=high&&b[j+1]>b[j]) j=j+1;
		if(b[i]>=b[j]) break;
		swap(b[i],b[j]);
		i=j;
		j=i*2;
	}
}
int main(){
	int n,p,q;
	scanf("%d",&n);
	vector<int>a(n+1),b(n+1);
	for(int i=1;i<=n;i++) scanf("%d",&a[i]);
	for(int i=1;i<=n;i++) scanf("%d",&b[i]);
	for(p=2;b[p-1]<=b[p]&&p<=n;p++);
	int index=p;
	while(index<=n&&a[index]==b[index]) index++;
	if(index==n+1){
		printf("Insertion Sort\n");
		sort(b.begin()+1,b.begin()+p+1);	
	}
	else{
		printf("Heap Sort\n");
		for(q=n;b[q]>=b[1]&&q>2;q--);
		swap(b[1],b[q]);
		downadjust(b,1,q-1);
	}
	for(int i=1;i<=n;i++){
		if(i>1) printf(" ");
		printf("%d",b[i]);
	}
	return 0;
} 
```



### A1099(30 中序遍历建立二叉搜索树)

```
#include<cstdio>
#include<queue>
#include<algorithm>
using namespace std;
const int maxn=101;
struct node{
	int v;
	int left,right;
}Node[maxn];
int n,a[maxn],index=0,num=0;
void inorder(int root){
	if(root==-1) return;
	inorder(Node[root].left);
	Node[root].v=a[index++];
	inorder(Node[root].right);
} 
void bfs(int root){
	queue<int> q;
	q.push(root);
	while(!q.empty()){
		int temp=q.front();
		q.pop();
		if(num>0) printf(" ");
		printf("%d",Node[temp].v);
		num++;
		if(Node[temp].left!=-1) q.push(Node[temp].left);
		if(Node[temp].right!=-1) q.push(Node[temp].right);
	}
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d%d",&Node[i].left,&Node[i].right);
	for(int i=0;i<n;i++) scanf("%d",&a[i]);
	sort(a,a+n);
	inorder(0);
	bfs(0);
	return 0;
} 
```



### A1100(20 字符串 string map)

```
#include<iostream>
#include<string>
#include<map>
using namespace std;
string unitdigit[13]={"tret", "jan", "feb", "mar", "apr", "may", "jun", "jly", "aug",
"sep", "oct", "nov", "dec"};
string tendigit[13]= {"tret", "tam", "hel", "maa", "huh", "tou", "kes", "hei", "elo",
"syy", "lok", "mer", "jou"};
string numtostr[169];
map<string,int> strtonum;
void init(){
	for(int i=0;i<13;i++){
		numtostr[i]=unitdigit[i];
		strtonum[unitdigit[i]]=i;
		numtostr[i*13]=tendigit[i];
		strtonum[tendigit[i]]=i*13;
	}
	for(int i=1;i<13;i++){
		for(int j=1;j<13;j++){
			string str=tendigit[i]+" "+unitdigit[j];
			numtostr[i*13+j]=str;
			strtonum[str]=i*13+j;
		}
	}
}
int main(){
	init();
	int n,temp;
	scanf("%d\n",&n);
	string s;
	while(n--){
		getline(cin,s);
		if(s[0]>='0'&&s[0]<='9'){
			temp=stoi(s);
			cout<<numtostr[temp]<<endl;
		}
		else cout<<strtonum[s]<<endl;
	}
	return 0;
}
```







### A1101(25 逻辑题)

```
#include<iostream>
#include<set>
using namespace std; 
int main(){
	int n,a[100010],leftmax=-1,rightmin=0x7fffffff;
	int left[100010],right[100010];
	set<int> ans;
	cin>>n;
	for(int i=0;i<n;i++) cin>>a[i];
	left[0]=a[0];
	for(int i=1;i<n;i++){
		if(a[i-1]>leftmax) leftmax=a[i-1];
		left[i]=leftmax;
	}
	right[n-1]=a[n-1];
	for(int i=n-2;i>=0;i--){
		if(a[i+1]<rightmin) rightmin=a[i+1];
		right[i]=rightmin;
	}
	for(int i=0;i<n;i++){
		if(a[i]>=left[i]&&a[i]<=right[i]) ans.insert(a[i]);
	}
	cout<<ans.size()<<endl;
	for(auto it=ans.begin();it!=ans.end();it++){
		if(it!=ans.begin()) cout<<" ";
		cout<<*it;
	}
	cout<<endl;
	 return 0;
}
```





### A1102(25 二叉树反转)

```
#include<cstdio> 
#include<algorithm>
#include<queue>
using namespace std;
struct node{
	int left,right;
}tree[11];
int n,ischild[11]={0},root,num1=0,num2=0;
void layerorder(int root){
	queue<int> q;
	q.push(root);
	while(!q.empty()){
		int Top=q.front();
		q.pop();
		printf("%d",Top);
		num1++;
		if(num1<n) printf(" ");
		if(tree[Top].left!=-1) q.push(tree[Top].left);
		if(tree[Top].right!=-1) q.push(tree[Top].right); 
	}
}
void inorder(int root){
	if(root==-1) return;
	inorder(tree[root].left);
	printf("%d",root);
	num2++;
	if(num2<n) printf(" ");
	inorder(tree[root].right);
}
void invert(int root){
	if(root==-1) return;
	invert(tree[root].left);
	invert(tree[root].right);
	swap(tree[root].left,tree[root].right);
} 
int main(){
	scanf("%d",&n);
	char c1,c2;
	for(int i=0;i<n;i++){
		scanf("%*c%c %c",&c1,&c2);
		if(c1=='-') tree[i].left=-1;
		else {
			ischild[c1-'0']=1;
			tree[i].left=c1-'0';
		}
		if(c2=='-') tree[i].right=-1;
		else {
			ischild[c2-'0']=1;
			tree[i].right=c2-'0';
		}
	}
	for(int i=0;i<n;i++){
		if(ischild[i]==0){
			root=i;
			break;
		} 
	}
	invert(root);
	layerorder(root);
	printf("\n");
	inorder(root);
	return 0;
}
```



### A1103(30 DFS 分解因式)

```
#include<cstdio>
#include<cmath>
#include<vector>
using namespace std;
int n,k,p,maxfac=-1;
vector<int> fac,temp,ans; 
void init(){
	int i=0;
	while(pow(i,p)<=n){
		fac.push_back(pow(i,p));
		i++;
	} 
}
void dfs(int index,int nowk,int sum,int facsum){
	if(nowk==k&&sum==n){
		if(facsum>maxfac){
			maxfac=facsum;
			ans=temp;
		}
		return;
	}
	if(nowk>k||sum>n) return;
	if(index>=1){
		temp.push_back(index);
		dfs(index,nowk+1,sum+fac[index],facsum+index);
		temp.pop_back();
		dfs(index-1,nowk,sum,facsum);
	}
}
int main(){
	scanf("%d%d%d",&n,&k,&p);
	init();
	dfs(fac.size()-1,0,0,0);
	if(maxfac==-1) printf("Impossible");
	else{
		printf("%d = ",n);
		for(int i=0;i<ans.size();i++){
		   if(i>0) printf(" + ");
		   printf("%d^%d",ans[i],p);
	   }
	}
	return 0;
}
```



### A1104(20 片段和 逻辑题)

```
#include<iostream>
using namespace std;
int main(){
	int n;
	long double ans=0,temp;
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>temp;
		ans+=temp*(n-i)*(i+1);
	}
	printf("%.2llf\n",ans);
	return 0; 
}
```



### A1105(25 顺时针螺旋二维数组，注意判断下标越界)

```
#include<cstdio>
#include<cmath>
#include<vector>
#include<algorithm>
using namespace std;
bool cmp(int a,int b) {
	return a>b;
}
int main() {
	int N, m,n;
	scanf("%d",&N);
	vector<int>t(N);
	for(int i=0; i<N; i++) scanf("%d",&t[i]);
	sort(t.begin(),t.end(),cmp);
	m=(int)(ceil(sqrt(N*1.0)));
	while(N%m!=0) m++;
	n=N/m;
	vector<vector<int> >v(m,vector<int>(n));
	int level=m/2+m%2,index=0;
	for(int i=0; i<level; i++) {
		for(int j=i; j<n-i&&index<N; j++)  v[i][j]=t[index++];
		for(int j=i+1; j<m-i&&index<N; j++) v[j][n-i-1]=t[index++];
		for(int j=n-i-2; j>=i&&index<N; j--) v[m-i-1][j]=t[index++];
		for(int j=m-i-2; j>i&&index<N; j--) v[j][i]=t[index++];
	}
	for(int i=0; i<m; i++) {
		if(i!=0) printf("\n");
		for(int j=0; j<n; j++) {
			if(j!=0) printf(" ");
			printf("%d",v[i][j]);
		}
	}
	return 0;
}
```



### A1106(25 树的dfs)

```
#include<cstdio>
#include<vector>
#include<cmath>
using namespace std;
int n,num=0,mindepth=0x7fffffff;
double p,r,ans=0.0;
vector<int> tree[100010]; 
void dfs(int index,int depth){
	if(tree[index].size()==0){
		if(mindepth>depth){
			mindepth=depth;
			num=1;
		}else if(mindepth==depth) num++;
		return;
	}
	for(int i=0;i<tree[index].size();i++) dfs(tree[index][i],depth+1);
}
int main(){
	scanf("%d %lf %lf",&n,&p,&r);
	int k,temp;
	for(int i=0;i<n;i++){
		scanf("%d",&k);
		for(int j=0;j<k;j++){
			scanf("%d",&temp);
			tree[i].push_back(temp);
		}
	}
	dfs(0,0);
	ans=p*pow((1+r/100),mindepth);
	printf("%.4f %d",ans,num);
	return 0;
}
```





### A1107(30 经典并查集)

```
#include<cstdio>
#include<algorithm>
#include<vector> 
using namespace std;
vector<int>father,isroot;
bool cmp(int a,int b){return a>b;}
int findfather(int x){
	int a=x;
	while(x!=father[x]) x=father[x];
	while(a!=father[a]){
		int z=a;
		a=father[a];
		father[z]=x;
	}
	return x;
}
void Union(int a,int b){
	int faA=findfather(a);
	int faB=findfather(b);
	if(faA!=faB) father[faA]=faB;
}
int main(){
	int course[1001]={0},n,num,courseid,cnt=0;
	scanf("%d",&n);
	father.resize(n+1);
	isroot.resize(n+1,0);
	for(int i=1;i<=n;i++) father[i]=i;
	for(int i=1;i<=n;i++){
		scanf("%d:",&num);
		while(num--){
			scanf("%d",&courseid);
			if(course[courseid]==0) course[courseid]=i;
			Union(i,findfather(course[courseid])); 
		}
	}
	for(int i=1;i<=n;i++) isroot[findfather(i)]++;
	for(int i=1;i<=n;i++){
		if(isroot[i]!=0) cnt++;
	}
	sort(isroot.begin(),isroot.end(),cmp);
	printf("%d\n",cnt);
	for(int i=0;i<cnt;i++){
		if(i>0) printf(" ");
		printf("%d",isroot[i]);
	}
	return 0;
}
```





### A1108(20 字符串 sscanf、sprintf)

```
#include<cstdio>
#include<cstring>
int main(){
	int n,num=0;
	double sum=0;
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		char s[50],b[50];
		double temp;
		int flag=0;
		scanf("%s",s);
		sscanf(s,"%lf",&temp);
		sprintf(b,"%.2f",temp);
		for(int j=0;j<strlen(s);j++){
			if(s[j]!=b[j]) flag=1;
		}
		if(flag||temp<-1000||temp>1000){
			printf("ERROR: %s is not a legal number\n",s);
			continue;
		}
		num++;
		sum+=temp;
	}
	if(num==0) printf("The average of 0 numbers is Undefined");
	else if(num==1) printf("The average of 1 number is %.2f",sum);
	else printf("The average of %d numbers is %.2f",num,sum/num);
	return 0;
}
```



### A1109(25 排序)

```
#include<cstdio>
#include<cstring>
#include<cmath>
#include<vector>
#include<algorithm>
using namespace std;
struct node{
	int height;
	char name[10];
}v[10010];
bool cmp(node a,node b){
	return a.height!=b.height? a.height>b.height : strcmp(a.name,b.name)<0;
}
int main(){
	int n,k,index=0,num;
	scanf("%d%d",&n,&k);
	int m=round(n*1.0/k);
	for(int i=0;i<n;i++) scanf("%s%d",v[i].name,&v[i].height);
	sort(v,v+n,cmp);
	for(int i=0;i<k;i++){
		if(i==0) num=n-m*(k-1);
		else num=m;
		vector<node> temp(num);
		temp[num/2]=v[index++];
		for(int j=0;j<num;j++){
			if(num/2-j-1>=0) temp[num/2-j-1]=v[index++];
			if(num/2+j+1<num) temp[num/2+j+1]=v[index++];
		}
		for(int j=0;j<num;j++){
			if(j>0) printf(" ");
			printf("%s",temp[j].name);
		}
		printf("\n");
	}
	return 0;
}
```





### A1110(25 判断是否是完全二叉树 dfs)

```
#include<iostream>
#include<string>
using namespace std;
struct node{
	int left,right;
}v[30];
int n,maxn=-1,ans;
void dfs(int root,int index){
	if(index>maxn){
		maxn=index;
		ans=root;
	}
	if(v[root].left!=-1) dfs(v[root].left,index*2);
	if(v[root].right!=-1) dfs(v[root].right,index*2+1);
}
int main(){
	scanf("%d",&n);
	string l,r;
	int ischild[30]={0},root=0;
	for(int i=0;i<n;i++){
		cin>>l>>r;
		if(l=="-") v[i].left=-1;
		else {
			v[i].left=stoi(l);
			ischild[v[i].left]=1;
		}
		if(r=="-") v[i].right=-1;
		else {
			v[i].right=stoi(r);
			ischild[v[i].right]=1;
		}
	}
	while(ischild[root]==1) root++;
	dfs(root,1);
	if(maxn==n) printf("YES %d",ans);
    else printf("NO %d",root);
	return 0;
} 
```





### A1111(30 两次dijkst加dfs,最短、最快路径、结点最少路径)

```
#include<cstdio>
#include<vector>
#include<algorithm> 
using namespace std;
const int INF=0x7fffffff;
const int maxn=520;
int n,m,st,ed,e[maxn][maxn],t[maxn][maxn],d[maxn],time[maxn],dispre[maxn],timepre[maxn],fast[maxn],fewnum[maxn];
bool vis[maxn];
vector<int> dispath,timepath;
void dis_dijkst(int s){
	fill(vis,vis+maxn,false);
	fill(d,d+maxn,INF);
	fill(fast,fast+maxn,INF);
	for(int i=0;i<n;i++) dispre[i]=i;
	d[s]=0;
	fast[s]=0;
	for(int i=0;i<n;i++){
		int u=-1,MIN=INF;
        for(int j=0;j<n;j++){
        	if(vis[j]==false&&d[j]<MIN){
        		u=j;
        		MIN=d[j];
			}
		}
		if(u==-1) return;
		vis[u]=true;
		for(int v=0;v<n;v++){
			if(vis[v]==false&&e[u][v]!=INF){
				if(d[u]+e[u][v]<d[v]){
					d[v]=d[u]+e[u][v];
					fast[v]=fast[u]+t[u][v];
					dispre[v]=u;
				}else if(d[u]+e[u][v]==d[v]&&fast[v]>fast[u]+t[u][v]){
					fast[v]=fast[u]+t[u][v];
					dispre[v]=u;
				}
			}
		} 
	}
}
void time_dijkst(int s){
	fill(time,time+maxn,INF);
	fill(vis,vis+maxn,false);
	time[s]=0;
	fewnum[s]=1;
	for(int i=0;i<n;i++) timepre[i]=i;
	for(int i=0;i<n;i++){
		int u=-1,MIN=INF;
        for(int j=0;j<n;j++){
        	if(vis[j]==false&&time[j]<MIN){
        		u=j;
        		MIN=time[j];
			}
		}
		if(u==-1) return;
		vis[u]=true;
		for(int v=0;v<n;v++){
			if(vis[v]==false&&t[u][v]!=INF){
				if(time[v]>time[u]+t[u][v]){
					time[v]=time[u]+t[u][v];
					timepre[v]=u;
					fewnum[v]=fewnum[u]+1;
				}else if(time[v]==time[u]+t[u][v]&&fewnum[v]>fewnum[u]+1){
					timepre[v]=u;
					fewnum[v]=fewnum[u]+1;
				}
			}
		}
	}
}
void dis_dfs(int ed){
	dispath.push_back(ed);
	if(ed==st) return;
	dis_dfs(dispre[ed]);
}
void time_dfs(int ed){
	timepath.push_back(ed);
	if(ed==st) return;
	time_dfs(timepre[ed]);
}
int main(){
	scanf("%d%d",&n,&m);
	int v1,v2,flag,len,ti;
	fill(e[0],e[0]+maxn*maxn,INF);
	fill(t[0],t[0]+maxn*maxn,INF);
	for(int i=0;i<m;i++){
		scanf("%d%d%d%d%d",&v1,&v2,&flag,&len,&ti);
		e[v1][v2]=len;
		t[v1][v2]=ti;
		if(flag!=1){
			e[v2][v1]=len;
			t[v2][v1]=ti;
		}
	}
	scanf("%d%d",&st,&ed);
	dis_dijkst(st);
	time_dijkst(st);
	dis_dfs(ed);
	time_dfs(ed);
	if(dispath==timepath){
		printf("Distance = %d; Time = %d: ",d[ed],time[ed]);
		for(int i=dispath.size()-1;i>=0;i--){
			printf("%d",dispath[i]);
			if(i>0) printf(" -> ");
		}
	}else{
		printf("Distance = %d: ",d[ed]);
		for(int i=dispath.size()-1;i>=0;i--){
			printf("%d",dispath[i]);
			if(i>0) printf(" -> ");
		}
		printf("\n");
		printf("Time = %d: ",time[ed]);
		for(int i=timepath.size()-1;i>=0;i--){
			printf("%d",timepath[i]);
			if(i>0) printf(" -> ");
		}
	}
	return 0;
}
```



### A1112(20 字符串、找出“坏键”)

```
#include<iostream>
#include<string>
#include<map>
using namespace std;
int main(){
	map<char,bool> mp,hasprint;
	string s,ans;
	int k,cnt=1;
	scanf("%d\n",&k);
	cin>>s;
	s="*"+s+"*";
	for(int i=1;i<s.length();i++){
		if(s[i]==s[i-1]) cnt++;
		else{
			if(cnt%k!=0) mp[s[i-1]]=true;
			cnt=1;
		}
	}
	for(int i=1;i<s.length()-1;i++){
		ans+=s[i];
		if(mp[s[i]]!=true){
			if(hasprint.count(s[i])!=true){
				cout<<s[i];
		     	hasprint[s[i]]=true;
			}
			i+=k-1;
		}
	}
	cout<<endl<<ans;
	return 0;
}
```





### A1113(25 水题)

```
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;
int main(){
	int n,s=0,s1=0;
	scanf("%d",&n);
	vector<int>v(n);
	for(int i=0;i<n;i++) scanf("%d",&v[i]);
	sort(v.begin(),v.end());
	for(int i=0;i<n;i++){
		if(i<n/2) s1+=v[i];
		s+=v[i];
	}
	printf("%d %d",n%2==0? 0:1,s-s1*2);
	return 0;
}
```





### A1114 (25 并查集)

```
#include<cstdio>
#include<vector>
#include<algorithm>
#include<set> 
using namespace std;
int n,father[10001],family[10001],exist[10001];
struct node{
	int sets,area;
}v[10001];
struct fam{
	int id,num=0;
	double avgset=0, avgarea=0;
}f[10001];
bool cmp(fam a,fam b){
	return a.avgarea!=b.avgarea ? a.avgarea>b.avgarea : a.id<b.id;
}
int findfather(int x){
	int a=x;
	while(x!=father[x]) x=father[x];
	while(a!=father[a]){
		int z=a;
		a=father[a];
		father[z]=x;
	}
	return x;
}
void Union(int a,int b){
	int faA=findfather(a);
	int faB=findfather(b);
	if(faA>faB) father[faA]=faB;
	else if(faA<faB) father[faB]=faA;
}
int main(){
	set<int> ans;
	scanf("%d",&n);
	for(int i=0;i<10001;i++) father[i]=i;
	for(int i=0;i<n;i++){
		int id,fa,mo,k,childid;
		scanf("%d%d%d%d",&id,&fa,&mo,&k);
		exist[id]=1;
		if(fa!=-1){
			exist[fa]=1;
			Union(id,fa);
		} 
		if(mo!=-1) {
			Union(id,mo);;
			exist[mo]=1;
		}
		while(k--){
		 	scanf("%d",&childid);
		 	exist[childid]=1;
		 	Union(id,childid);
		} 
		scanf("%d%d",&v[id].sets,&v[id].area);
	}
	for(int i=0;i<10001;i++){
		if(exist[i]==1){
			ans.insert(findfather(i));
			f[findfather(i)].id=findfather(i);
			f[findfather(i)].num++;
			f[findfather(i)].avgset+=v[i].sets;
			f[findfather(i)].avgarea+=v[i].area;
		}
	}
	for(auto it=ans.begin();it!=ans.end();it++){
		f[*it].avgarea=f[*it].avgarea/f[*it].num;
		f[*it].avgset=f[*it].avgset/f[*it].num;
	} 
	sort(f,f+10001,cmp);
	printf("%d\n",ans.size());
	for(int i=0;i<ans.size();i++) printf("%04d %d %.3f %.3f\n",f[i].id,f[i].num,f[i].avgset,f[i].avgarea);
	return 0;
}
```





### A1115(30 二叉搜索树建立与DFS)

```
#include<cstdio>
struct node{
	int v;
	node*left,*right;
};
int maxlevel=-1,level=0,n1=0,n2=0;
void insert(node *&root,int v){
	level++;
	if(root==NULL){
		root=new node;
		root->v=v;
		root->left=root->right=NULL;
		if(maxlevel<level) maxlevel=level;
		level=0;
		return;
	}
	if(v<=root->v) insert(root->left,v);
	else insert(root->right,v);
}
void dfs(node*root,int level){
	if(root==NULL) return;
	if(level==maxlevel) n1++;
	else if(level==maxlevel-1) n2++;
	dfs(root->left,level+1);
	dfs(root->right,level+1);
}
int main(){
	int n,a;
	node*root=NULL;
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		scanf("%d",&a);
		insert(root,a);
	}
	dfs(root,1);
	printf("%d + %d = %d",n1,n2,n1+n2);
	return 0;
}
```





### A1116(20 水题)

```
#include<cstdio>
#include<cmath>
#include<map>
using namespace std;
bool isprime(int x){
	if(x<=1) return false;
	int sqr=sqrt(1.0*x);
	for(int i=2;i<=sqr;i++){
		if(x%i==0) return false;
	}
	return true;
}
int main(){
	int n,temp,k;
	scanf("%d",&n);
	map<int,int> mp,hascheck;
	for(int i=1;i<=n;i++){
		scanf("%d",&temp);
		mp[temp]=i;
	}
	scanf("%d",&k);
	while(k--){
		scanf("%d",&temp);
		if(mp.count(temp)==0) printf("%04d: Are you kidding?\n",temp);
		else{
			if(hascheck.count(temp)==0){
			hascheck[temp]=1;
			if(mp[temp]==1) printf("%04d: Mystery Award\n",temp);
	     	else if(isprime(mp[temp])) printf("%04d: Minion\n",temp);
	     	else printf("%04d: Chocolate\n",temp);
		 }
		else printf("%04d: Checked\n",temp);
		}
	}
	return 0;
}
```





### A1117(25 逻辑题)

```
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
	int a[100010]={0},n,j=0;
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d",&a[i]);
	sort(a,a+n,greater<int>());
    while(j<n&&a[j]>j+1) j++;
	printf("%d",j);
	return 0;
}
```





### A1118(25 并查集)

```
#include<cstdio>
#include<set>
using namespace std;
int n,father[10006],tree[10006]={0};
int findfather(int x){
	int a=x;
	while(x!=father[x]) x=father[x];
	while(a!=father[a]){
		int z=a;
		a=father[a];
		father[z]=x;
	}
	return x;
} 
void Union(int a,int b){
	int fa=findfather(a);
	int fb=findfather(b);
	if(fa!=fb) father[fa]=fb;
}
int main(){
	int k,q,t,temp,num=0;
	scanf("%d",&n);
	set<int> birds;
	for(int i=0;i<10006;i++) father[i]=i;
	for(int i=0;i<n;i++){
		scanf("%d%d",&k,&t);
		birds.insert(t);
		for(int j=0;j<k-1;j++){
			scanf("%d",&temp);
			birds.insert(temp);
			Union(temp,t);
		}
	}
	for(auto it=birds.begin();it!=birds.end();it++)	tree[findfather(*it)]++;
	for(int i=0;i<10006;i++){
		if(tree[i]!=0) num++;
	}
	printf("%d %d\n",num,birds.size());
	scanf("%d",&q);
	while(q--){
		scanf("%d%d",&t,&temp);
		if(findfather(t)==findfather(temp)) printf("Yes\n");
		else printf("No\n");
	}
	return 0;
} 
```





### A1119 (30 树的前序和后序 求中序)

```
#include<cstdio>
#include<vector>
using namespace std;
int n,post[31],pre[30],num=0;
bool unique=true;
struct node{
	int v;
	node*left,*right;
};
node* create(int prel,int prer,int postl,int postr){
	if(prel>prer) return NULL;
	node*root=new node;
	root->v=pre[prel];
	int k=prel+1;
	while(k<=prer&&pre[k]!=post[postr-1]) k++;
	if(prel+1<n&&postr-1>=0&&pre[prel+1]==post[postr-1]) unique=false;
	int numleft=k-prel-1;
	root->left=create(prel+1,k-1,postl,postl+numleft-1);
	root->right=create(k,prer,postl+numleft,postr-1);
	return root;
}
void inorder(node*root){
	if(root==NULL) return;
	inorder(root->left);
    if(num>0) printf(" ");
	printf("%d",root->v);
	num++;
	inorder(root->right);
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d",&pre[i]);
	for(int i=0;i<n;i++) scanf("%d",&post[i]);
	node*root=NULL;
	root=create(0,n-1,0,n-1);
	if(unique) printf("Yes\n");
	else printf("No\n");
	inorder(root);
    printf("\n");
	return 0;
}
```



### A1120(20 水题)

```
#include<iostream>
#include<set>
#include<string>
using namespace std;
const int maxn=10010;
int main(){
	int n,sum[maxn]={0};
	string str;
	set<int> ans;
	cin>>n;
	bool flag[maxn]={false};
	for(int i=0;i<n;i++){
		cin>>str;
		int len=str.size();
		for(int j=0;j<len;j++){
			sum[i]+=(str[j]-'0');
		}
	}
	for(int i=0;i<n;i++) ans.insert(sum[i]);
	printf("%d\n",ans.size());
	for(auto it=ans.begin();it!=ans.end();it++) {
		if(it!=ans.begin()) printf(" %d",*it);
		else printf("%d",*it);
	}
	return 0;
}
```



### A1121 (25 map、set应用)

```
#include<cstdio>
#include<set>
#include<vector>
#include<map>
using namespace std;
bool flag[100000] = { false };
map<int, int>couple;
int main() {
	int n, m, p1, p2;
	set<int> ans;
	vector<int> guests;
	scanf("%d", &n);
	while (n--) {
		scanf("%d%d", &p1, &p2);
		couple[p1] = p2;
		couple[p2] = p1;
	}
	scanf("%d", &m);
	for (int i = 0; i < m; i++) {
		scanf("%d", &p1);
		guests.push_back(p1);
		flag[p1] = true;
	}
	for (int i = 0; i < guests.size(); i++) {
		if (couple.count(guests[i])==0||(flag[couple[guests[i]]]==false)&& couple.count(guests[i]) != 0) ans.insert(guests[i]);
	}
	printf("%d\n", ans.size());
	for (auto it = ans.begin(); it != ans.end(); it++) {
		if (it != ans.begin()) printf(" ");
		printf("%05d", *it);
	}
	return 0;
}
```





### A1122(25 哈密顿回路  set)

```
#include<cstdio>
#include<set>
#include<vector>
using namespace std;
int main(){
	int n,m,k,c1,c2,g[210][210];
	scanf("%d%d",&n,&m);
	while(m--){
		scanf("%d%d",&c1,&c2);
		g[c1][c2]=1;
		g[c2][c1]=1;
	}
	scanf("%d",&k);
	while(k--){
		int num,flag1=1,flag2=1;
		scanf("%d",&num);
		vector<int> v(num);
		set<int> s;
		for(int i=0;i<num;i++){
			scanf("%d",&v[i]);	
			s.insert(v[i]);
		}	
		if(num-1!=n||s.size()!=n||v[0]!=v[num-1]) flag1=0;
		for(int i=0;i<num-1;i++){
			if(g[v[i]][v[i+1]]!=1) flag2=0;
		}
		if(flag1==1&&flag2==1) printf("YES\n");
		else printf("NO\n");
	}
	return 0;
}
```





### A1123(30 AVL 、层序遍历、判断是否是完全二叉树)

```
#include<cstdio>
#include<queue>
#include<algorithm>
using namespace std;
struct node{
	int v,height;
	node* left,*right;
};
node* newnode(int v){
	node* Node=new node;
	Node->v=v;
	Node->left=Node->right=NULL;
	Node->height=1;
	return Node;
}
int getheight(node *root){
	if(root==NULL) return 0;
	return root->height;
}
void updateheight(node *root){
	root->height=max(getheight(root->left),getheight(root->right))+1;
}
int getbalance(node *root){
	return getheight(root->left)-getheight(root->right);
}
void R(node*&root){
	node*temp=root->left;
	root->left=temp->right;
	temp->right=root;
	updateheight(root);
	updateheight(temp);
	root=temp;
}
void L(node*&root){
    node*temp=root->right;
	root->right=temp->left;
	temp->left=root;
	updateheight(root);
	updateheight(temp);
	root=temp;
}
void insert(node*&root,int v){
	if(root==NULL){
		root=newnode(v);
		return;
	}
	if(root->v>v){
		insert(root->left,v);
		updateheight(root);
		if(getbalance(root)==2){
			if(getbalance(root->left)==1){
				R(root);
			}else if(getbalance(root->left)==-1){
				L(root->left);
				R(root);
			}
		}
	}else{
		insert(root->right,v);
		updateheight(root);
		if(getbalance(root)==-2){
			if(getbalance(root->right)==-1){
				L(root);
			}else if(getbalance(root->right)==1){
				R(root->right);
				L(root);
			}
		}
	}
}
int iscomplete=1,after=0,n,num=0;
void layerorder(node*root){
	queue<node*>q;
	q.push(root);
	while(!q.empty()){
		node* t=q.front();
		q.pop();
		if(num>0) printf(" ");
		printf("%d",t->v);
		num++;
		if(t->left!=NULL){
			if(after) iscomplete=0;
			q.push(t->left);
		}else after=1;
		if(t->right!=NULL){
			if(after) iscomplete=0;
			q.push(t->right);
		}else after=1;
	}
}
int main(){
	int temp;
	scanf("%d",&n);
	node*root=NULL;
	for(int i=0;i<n;i++){
		scanf("%d",&temp);
		insert(root,temp);
	}
	layerorder(root);
	if(iscomplete) printf("\nYES");
	else printf("\nNO");
	return 0;
}
```





### A1124  20 逻辑题

```
#include<iostream>
#include<vector>
#include<string>
#include<map>
using namespace std;
int main(){
	int m,n,s;
	cin>>m>>n>>s;
	vector<string> v(m+1);
	map<string,bool> win;
	for(int i=1;i<=m;i++)	cin>>v[i];
	if(s>m) cout<<"Keep going...";
	else{
		for(int i=s;i<=m;i++){
			if(win.count(v[i])==0){
				win[v[i]]=true;
				cout<<v[i]<<endl;
				i+=n-1;
			}
		}
	}
	return 0;
}
```



### A1125(25 贪心 排序)

```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
int main(){
	int n;
	scanf("%d",&n);
	vector<int> v(n);
	for(int i=0;i<n;i++) scanf("%d",&v[i]);
	sort(v.begin(),v.end());
	double seg=v[0];
	for(int i=0;i<n-1;i++) seg=(seg+v[i+1])/2;
	printf("%d",(int)seg);
	return 0;
}
```



### A1126(25 欧拉图 欧拉回路 欧拉路径)

```
#include<cstdio>
int g[510][510],degree[510]={0},n,m;
bool vis[510]={false};
void dfs(int u){
	vis[u]=true;
	for(int i=1;i<=n;i++){
		if(vis[i]==false&&g[u][i]==1){
			dfs(i);
		}
	}
}
int main(){
	scanf("%d%d",&n,&m);
	int v1,v2,evennum=0,block=0;
	while(m--){
		scanf("%d%d",&v1,&v2);
		g[v1][v2]=g[v2][v1]=1;
		degree[v1]++;
		degree[v2]++;
	}
	for(int i=1;i<=n;i++){
		if(i>1) printf(" ");
		printf("%d",degree[i]);
		if(degree[i]%2==0) evennum++;
	}
	for(int i=1;i<=n;i++){
		if(vis[i]==false){
			dfs(i);
			block++;
		}	
	}
	if(block==1&&evennum==n) printf("\nEulerian");
	else if(block==1&&evennum==n-2) printf("\nSemi-Eulerian");
	else printf("\nNon-Eulerian");
	return 0;
}
```



### A1127(30 中序后序建树，dfs，输出z字形层序遍历)

```
方法一:层序遍历用结构体统计每层结点
#include<cstdio>
#include<vector>
#include<queue>
using namespace std;
int n,post[31],in[31];
vector<int> level[35];
struct node{
	int v,depth;
	node* left,*right;
};
node* create(int postl,int postr,int inl,int inr){
	if(postl>postr) return NULL;
	node *root=new node;
	root->v=post[postr];
	int k=inl;
	while(in[k]!=root->v&&k<=inr) k++;
	int leftnum=k-inl;
	root->left=create(postl,postl+leftnum-1,inl,k-1);
	root->right=create(postl+leftnum,postr-1,k+1,inr);
	return root; 
}
void bfs(node*root){
	queue<node> q;
	q.push(node{root->v,1,root->left,root->right});
	while(!q.empty()){
		node t=q.front();
		q.pop();
		level[t.depth].push_back(t.v);
		if(t.left!=NULL) q.push(node{t.left->v,t.depth+1,t.left->left,t.left->right});
		if(t.right!=NULL) q.push(node{t.right->v,t.depth+1,t.right->left,t.right->right});
	}
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d",&in[i]);
	for(int i=0;i<n;i++) scanf("%d",&post[i]);
	node*root=create(0,n-1,0,n-1);
	bfs(root);
	printf("%d",root->v);
	for(int i=2;i<35;i++){
		if(i%2==0){
			for(int j=0;j<level[i].size();j++) printf(" %d",level[i][j]);
		}else{
			for(int j=level[i].size()-1;j>=0;j--) printf(" %d",level[i][j]);
		}
	}
	return 0;
}


方法二:层序遍历用当前队列的元素个数统计每层节点
#include<cstdio>
#include<vector>
#include<queue>
using namespace std;
int n,post[31],in[31],depth=1;
vector<int> level[31];
struct node{
	int v;
	node* left,*right;
};
node* create(int postl,int postr,int inl,int inr){
	if(postl>postr) return NULL;
	node *root=new node;
	root->v=post[postr];
	int k=inl;
	while(in[k]!=root->v&&k<=inr) k++;
	int leftnum=k-inl;
	root->left=create(postl,postl+leftnum-1,inl,k-1);
	root->right=create(postl+leftnum,postr-1,k+1,inr);
	return root; 
}
void bfs(node*root){
	queue<node*> q;
	q.push(root);
	while(!q.empty()){
		int count=q.size();
		while(count--){
			node* t=q.front();
			q.pop();
			level[depth].push_back(t->v);
			if(t->left!=NULL) q.push(t->left);
			if(t->right!=NULL) q.push(t->right);
		}
		depth++;
	}
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++) scanf("%d",&in[i]);
	for(int i=0;i<n;i++) scanf("%d",&post[i]);
	node*root=create(0,n-1,0,n-1);
	bfs(root);
	printf("%d",root->v);
	for(int i=2;i<=depth;i++){
		if(i%2==0){
			for(int j=0;j<level[i].size();j++) printf(" %d",level[i][j]);
		}else{
			for(int j=level[i].size()-1;j>=0;j--) printf(" %d",level[i][j]);
		}
	}
	return 0;
}
```





### A1128(20  n皇后问题)

```
#include<cstdio>
#include<algorithm>
using namespace std;
int main(){
	int k,n,a[1005];
	scanf("%d",&k);
	while(k--){
		int flag=1;
		scanf("%d",&n);
		for(int i=1;i<=n;i++) scanf("%d",&a[i]);
		for(int i=2;i<=n;i++){
			for(int j=1;j<i;j++){
				if(a[j]==a[i]||abs(i-j)==abs(a[j]-a[i])){
					flag=0;
					break;
				}
			}
			if(flag==0){
				printf("NO\n");
				break;
			}
		}
		if(flag) printf("YES\n");
	}
	return 0;
}
```



### A1129 (25 set的应用 结构体内运算符重载)

```
#include<cstdio>
#include<vector>
#include<set>
#include<algorithm>
using namespace std;
struct node{
	int id,fre;
};
bool operator < (const node &a,const node &b){
	return a.fre!=b.fre? a.fre>b.fre : a.id<b.id;
}
int main(){
	int n,k,item,times[50005]={0};
	scanf("%d%d",&n,&k);
	set<node> s; 
	for(int i=0;i<n;i++){
		scanf("%d",&item);
		if(i!=0){
			int j=0;
			printf("%d:",item);
			for(auto it=s.begin();j<k&&it!=s.end();it++,j++) printf(" %d",*it);
			printf("\n");
		}
		auto it=s.find(node{item,times[item]});
		times[item]++;
		if(it!=s.end()) s.erase(it);
		s.insert(node{item,times[item]});
	}
	return 0;
}
```





### A1130(中序遍历二叉树 输出中缀表达式)

```
//中序遍历可以得到中缀表达式
//如果不是叶节点且不是根节点就加括号
#include<iostream>
#include<string>
using namespace std;
struct node{
	string v;
	int left,right;
}tree[21];
string ans;
int n,l,r,notroot[21]={0},root=1;
void inorder(int index){
	bool flag=false;
	if((tree[index].left!=-1||tree[index].right!=-1)&&index!=root){
		flag=true;
		ans+="(";
	}
	if(tree[index].left!=-1) inorder(tree[index].left);
	ans+=tree[index].v;
	if(tree[index].right!=-1) inorder(tree[index].right);
	if(flag) ans+=")";
}
int main(){
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>tree[i].v>>tree[i].left>>tree[i].right;
		if(tree[i].left!=-1) notroot[tree[i].left]=1;
		if(tree[i].right!=-1) notroot[tree[i].right]=1;
	}
	while(notroot[root]!=0) root++;
	inorder(root);
	cout<<ans;
	return 0;
}
```



### A1131(30 DFS 、unordered_map邻接矩阵、 难题  )

```
#include<cstdio>
#include<vector>
#include<unordered_map>
using namespace std;
vector<vector<int> > v(10000);
int visit[10000] = { 0 }, mincnt, mintransfer, st, ed;
unordered_map<int, int> line;
vector<int> path, temppath;
int transfercnt(vector<int> a) {
	int cnt = -1, preline = 0;
	for (int i = 1; i < a.size(); i++) {
		if (line[a[i - 1] * 10000 + a[i]] != preline) cnt++;
		preline = line[a[i - 1] * 10000 + a[i]];
	}
	return cnt;
}
void dfs(int node, int cnt) {
	if (node == ed && (cnt < mincnt || (cnt == mincnt && transfercnt(temppath) < mintransfer))) {
		mincnt = cnt;
		mintransfer = transfercnt(temppath);
		path = temppath;
    return;
	}
	for (int i = 0; i < v[node].size(); i++) {
		if (visit[v[node][i]] == 0) {
			visit[v[node][i]] = 1;
			temppath.push_back(v[node][i]);
			dfs(v[node][i], cnt + 1);
			visit[v[node][i]] = 0;
			temppath.pop_back();
		}
	}
}
int main() {
	int n, m, pre, temp, k;
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%d%d", &m, &pre);
		for (int j = 1; j < m; j++) {
			scanf("%d", &temp);
			v[pre].push_back(temp);
			v[temp].push_back(pre);
			line[pre * 10000 + temp]=line[temp*10000+pre] = i;
			pre = temp;
		}
	}
	scanf("%d", &k);
	while (k--) {
		mintransfer = mincnt = 1000000000;
		scanf("%d%d", &st, &ed);
		temppath.clear();
		temppath.push_back(st);
		visit[st] = 1;
		dfs(st, 0);
		visit[st] = 0;
		printf("%d\n", mincnt);
		int preline = 0, pretransfer = st;
		for (int i = 1; i < path.size(); i++) {
			if (line[path[i - 1] * 10000 + path[i]] != preline) {
				if (preline != 0) printf("Take Line#%d from %04d to %04d.\n", preline, pretransfer, path[i - 1]);
				preline = line[path[i - 1] * 10000 + path[i]];
				pretransfer = path[i - 1];
			}
		}
		printf("Take Line#%d from %04d to %04d.\n", preline, pretransfer, ed);
	}
	return 0;
}
```



### A1132(20 字符串 水题)

```
#include<iostream>
#include<string>
using namespace std;
int main() {
	string s, s1, s2;
	int n, len;
	cin >> n;
	while (n--) {
		cin >> s;
		len = s.size();
		s1 = s.substr(0, len / 2);
		s2 = s.substr(len / 2, len / 2);
		if (stoi(s1) == 0 || stoi(s2) == 0) cout << "No" << endl;
		else if (stoi(s) % (stoi(s1) * stoi(s2)) == 0) cout << "Yes" << endl;
		else cout << "No" << endl;
	}
	return 0;
}
```



### A1133(链表  重新排列)

```
#include<cstdio>
#include<vector>
using namespace std;
struct node {
	int id, data, next;
};
vector<node> v, ans;
int main() {
	node a[100010];
	int begin, n, k;
	int id, data, next;
	scanf("%d%d%d", &begin, &n, &k);
	for (int i = 0; i < n; i++) {
		scanf("%d%d%d", &id, &data, &next);
		a[id] = { id,data,next };
	}
	for (; begin != -1; begin = a[begin].next) v.push_back(a[begin]);
	for (int i = 0; i < v.size(); i++) {
		if (v[i].data < 0) ans.push_back(v[i]);
	}
	for (int i = 0; i < v.size(); i++) {
		if (v[i].data >= 0 && v[i].data <= k) ans.push_back(v[i]);
	}
	for (int i = 0; i < v.size(); i++) {
		if (v[i].data > k) ans.push_back(v[i]);
	}
	for (int i = 0; i < ans.size() - 1; i++) {
		printf("%05d %d %05d\n", ans[i].id, ans[i].data, ans[i + 1].id);
	}
	printf("%05d %d -1", ans[ans.size() - 1].id, ans[ans.size() - 1].data);
	return 0;
}
```



### A1134(25 hash散列)

```
#include<cstdio>
#include<vector>
using namespace std;
int main(){
	int n,m,k,exist[10001];
	scanf("%d%d",&n,&m);
	vector<int> v[10001];
	for(int i=0;i<m;i++){
		int v1,v2;
		scanf("%d%d",&v1,&v2);
		v[i].push_back(v1);
		v[i].push_back(v2);
	} 
	scanf("%d",&k);
	while(k--){
		int num,hashtable[10001]={0},temp,flag=1;
		scanf("%d",&num);
		for(int i=0;i<num;i++){
			scanf("%d",&temp);
			hashtable[temp]=1;
		}
		for(int i=0;i<m;i++){
			if(hashtable[v[i][0]]!=1&&hashtable[v[i][1]]!=1){
				flag=0;
				break;
			}
		}
		if(flag==1) printf("Yes\n");
		else printf("No\n");
	}
	return 0;
}
```





### A1135(30 判断红黑树 递归判断)

```
#include<cstdio>
#include<algorithm>
using namespace std;
struct node{
	int v;
	node*left,*right;
};
void create(node*&root,int v){
	if(root==NULL){
		root=new node;
		root->v=v;
		root->left=root->right=NULL;
		return;
	}
	if(abs(v)<=abs(root->v)) create(root->left,v);
	else create(root->right,v);
}
bool judge1(node*root){
	if(root==NULL) return true;
	if(root->v<0){
		if(root->left!=NULL&&root->left->v<0) return false;
		if(root->right!=NULL&&root->right->v<0) return false;
	}
	return judge1(root->left)&&judge1(root->right);
}
int getnum(node*root){
	if(root==NULL) return 0;
	int l=getnum(root->left);
	int r=getnum(root->right);
	return root->v>0? max(l,r)+1 :max(l,r);
}
bool judge2(node*root){
	if(root==NULL) return true;
	int l=getnum(root->left);
	int r=getnum(root->right);
	if(l!=r) return false;
	return judge2(root->left)&&judge2(root->right);
}
int main(){
	int k,n,temp;
	scanf("%d",&k);
	while(k--){
		node*root=NULL;
		scanf("%d",&n);
		for(int i=0;i<n;i++){
			scanf("%d",&temp);
			create(root,temp);
		}
		if(root->v>0&&judge1(root)&&judge2(root)) printf("Yes\n");
		else printf("No\n");
	}
	return 0;
}
```



### A1136 (20 回文串 字符串)

```
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;
string revs(string s){
	reverse(s.begin(),s.end());
	return s;
}
string add(string s1,string s2){
	string s=s1;
	int carry=0;
	for(int i=s1.size()-1;i>=0;i--){
		s[i]=((s1[i]-'0')+(s2[i]-'0')+carry)%10+'0';
		carry=((s1[i]-'0')+(s2[i]-'0')+carry)/10;
	}
	if(carry>0) s='1'+s;
	return s;
}
int main(){
	string s,rev;
	cin>>s;
	if(s==revs(s)) {
		cout<<s<<" is a palindromic number.";
		return 0;
	}
	for(int i=1;i<=10;i++){
		string sum=add(s,revs(s));
		cout<<s<<" + "<<revs(s)<<" = "<<sum<<endl;
		s=sum;
		if(s==revs(s)){
			cout<<s<<" is a palindromic number.";
			break;
		}else{
			if(i==10) cout<<"Not found in 10 iterations.";
		}
	}
	return 0;
}
```



### A1137(25 map 排序)

```
#include<iostream>
#include<string>
#include<map>
#include<vector>
#include<algorithm>
#include<cmath>
using namespace std;
struct node{
	string id;
	int gp=-1,gm=-1,gf=-1,g;
}stu[10001];
map<string,int> stoint;
int index=0;
vector<node> v;
int strtoint(string s){
	if(stoint.count(s)==0) stoint[s]=index++;
	return stoint[s];
}
bool cmp(node a,node b){
	return a.g!=b.g? a.g>b.g : a.id<b.id;
}
int main(){
	int p,m,n,score,tid;
	string id;
	cin>>p>>m>>n;
	for(int i=0;i<p;i++){
		cin>>id>>score;
		int tid=strtoint(id);
		stu[tid].gp=score;
		stu[tid].id=id;
	}
	for(int i=0;i<m;i++){
		cin>>id>>score;
		int tid=strtoint(id);
		stu[tid].gm=score;
		stu[tid].id=id;
	}
	for(int i=0;i<n;i++){
		cin>>id>>score;
		int tid=strtoint(id);
		stu[tid].gf=score;
		stu[tid].id=id;
	}
	for(int i=0;i<index;i++){
		if(stu[i].gp>=200){
			if(stu[i].gm>stu[i].gf){
				if(stu[i].gf!=-1) stu[i].g=round(stu[i].gm*0.4+stu[i].gf*0.6);
				else stu[i].g=round(stu[i].gm*0.4);
			}else{
				if(stu[i].gf==-1) stu[i].g=0;
				else stu[i].g=stu[i].gf;
			}
			if(stu[i].g>=60) v.push_back(stu[i]);
		}
	}
	sort(v.begin(),v.end(),cmp);
	for(int i=0;i<v.size();i++){
		printf("%s %d %d %d %d\n",v[i].id.c_str(),v[i].gp,v[i].gm,v[i].gf,v[i].g);	
	}
	return 0;
}
```



### A1138(25 树 前序加中序输出后序)

```
#include <iostream>
#include <vector>
using namespace std;
vector<int> pre, in;
bool flag = false;
void postOrder(int prel, int inl, int inr) {
 if (inl > inr || flag == true) return;
 int i = inl;
 while (in[i] != pre[prel]) i++;
 postOrder(prel+1, inl, i-1);
 postOrder(prel+i-inl+1, i+1, inr);
 if (flag == false) {
 printf("%d", in[i]);
 flag = true;
 }
}
int main() {
 int n;
 scanf("%d", &n);
 pre.resize(n);
 in.resize(n);
 for (int i = 0; i < n; i++) scanf("%d", &pre[i]);
 for (int i = 0; i < n; i++) scanf("%d", &in[i]);
 postOrder(0, 0, n-1);
 return 0;
}
```



### A1139(30 逻辑题 unordered_map)

```
#include<unordered_map>
#include<vector>
#include<string>
#include<iostream>
#include<algorithm>
using namespace std;
struct node{
	int a,b;
};
bool cmp(const node &i,const node &j){
	return i.a!=j.a? i.a<j.a : i.b<j.b;
}
unordered_map<int ,bool> isfri;
int main(){
	int n,m,k,c,d;
	cin>>n>>m;
	string a,b;
	vector<int> v[10000];
	for(int i=0;i<m;i++){
		cin>>a>>b;
		if(a.length()==b.length()) {
			v[abs(stoi(a))].push_back(abs(stoi(b)));
			v[abs(stoi(b))].push_back(abs(stoi(a)));
		}
isfri[abs(stoi(a))*10000+abs(stoi(b))]=isfri[abs(stoi(b))*10000+abs(stoi(a))]=true;
	}
	cin>>k;
	while(k--){
		vector<node> ans;
		cin>>c>>d;
		for(int i=0;i<v[abs(c)].size();i++){
			for(int j=0;j<v[abs(d)].size();j++){
				if(v[abs(c)][i]==abs(d)||v[abs(d)][j]==abs(c)) continue;
				if(isfri[v[abs(c)][i]*10000+v[abs(d)][j]]==true) ans.push_back(node{v[abs(c)][i],v[abs(d)][j]});
			}
		}
		sort(ans.begin(),ans.end(),cmp);
		cout<<ans.size()<<endl;
		for(int i=0;i<ans.size();i++) printf("%04d %04d\n",ans[i].a,ans[i].b);
	}
	return 0;
}
```



### A1140(20 字符串)

```
#include<iostream> 
#include<string>
using namespace std;
int main(){
	string s;
	int n,j;
	cin>>s>>n;
	for(int x=1;x<n;x++){
       string ans;
       for(int i=0;i<s.length();i=j){
       	for(j=i;j<s.length()&&s[j]==s[i];j++);
       	ans+=s[i]+to_string(j-i);
	   }
	   s=ans;
	}
	cout<<s;
	return 0;
} 
```



### A1141(25 stl、排序)

```
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
#include<unordered_map>
using namespace std;
struct node{
	string name;
	double score=0;
	int num=0,fscore=0,rank;
}t[100010];
int index=0;
unordered_map<string,int> mp;
int change(string &s){
	for(int i=0;i<s.size();i++){
		if(isupper(s[i])) s[i]=tolower(s[i]);
	}
	if(mp.count(s)==0) mp[s]=index++;
	return mp[s]; 
}
bool cmp(node a,node b){
	if(a.fscore!=b.fscore) return a.fscore>b.fscore;
	else if(a.num!=b.num) return a.num<b.num;
	else return a.name<b.name;
}
int main(){
	int n;
	cin>>n;
	//cout<<"pppp\n";
	for(int i=0;i<n;i++){
		string id,code;
		int score,tid;
		cin>>id>>score>>code;
		tid=change(code);
		t[tid].name=code;
		t[tid].num++;
		if(id[0]=='B') t[tid].score+=score/1.5;
		if(id[0]=='A') t[tid].score+=score*1.0;
		if(id[0]=='T') t[tid].score+=score*1.5;
	}
	for(int i=0;i<index;i++) t[i].fscore=(int)(t[i].score);
	sort(t,t+index,cmp);
	cout<<index<<endl;
	t[0].rank=1;
	printf("%d %s %d %d\n",t[0].rank,t[0].name.c_str(),t[0].fscore,t[0].num);
	for(int i=1;i<index;i++){
		if(t[i].fscore==t[i-1].fscore) t[i].rank=t[i-1].rank;
		else t[i].rank=i+1;
		printf("%d %s %d %d\n",t[i].rank,t[i].name.c_str(),t[i].fscore,t[i].num);
	}
	return 0;
}
```





### A1142(25 无向完全图 最大子图  两点相连)

```
#include<cstdio>
#include<vector>
using namespace std;
int main(){
	int nv,ne,m,k,g[210][210],v1,v2;
	scanf("%d%d",&nv,&ne);
	for(int i=0;i<ne;i++){
		scanf("%d%d",&v1,&v2);
		g[v1][v2]=g[v2][v1]=1;
	}
	scanf("%d",&m);
	while(m--){
		int isclique=1,ismax=1,hash[210]={0};
		scanf("%d",&k);
		vector<int> v(k);
		for(int i=0;i<k;i++) {
			scanf("%d",&v[i]);
			hash[v[i]]=1;
		}
		for(int i=0;i<k-1;i++){
			if(isclique==0) break;
			for(int j=i+1;j<k;j++){
				if(g[v[i]][v[j]]!=1){
					isclique=0;
					printf("Not a Clique\n");
				    break;
				}
			}
		}
		if(isclique==0) continue;
		for(int i=1;i<=nv;i++){
			if(ismax==0) break;
			if(hash[i]!=1){
				for(int j=0;j<k;j++){
					if(g[i][v[j]]==1){
						if(j==k-1) ismax=0;
					}else break;
				}
			}
		}
		if(ismax==1) printf("Yes\n");
		else printf("Not Maximal\n");
	} 
	return 0;
}
```







### A1143(30 树 LCA 最低公共祖先)

```
#include<cstdio>
#include<unordered_map>
#include<vector>
using namespace std;
int main(){
	int m,n,u,v,ans;
	scanf("%d%d",&m,&n);
	unordered_map<int,bool>mp;
	vector<int> pre(n);
	for(int i=0;i<n;i++){
		scanf("%d",&pre[i]);
		mp[pre[i]]=true;
	}
	for(int i=0;i<m;i++){
		scanf("%d%d",&u,&v);
		for(int j=0;j<n;j++){
			if((pre[j]>=u&&pre[j]<=v)||(pre[j]<=u&&pre[j]>=v)){
				ans=pre[j];
				break;
			}
		}
		if(mp.count(u)==0&&mp.count(v)==0) printf("ERROR: %d and %d are not found.\n",u,v);
		else if(mp.count(u)==0&&mp.count(v)!=0) printf("ERROR: %d is not found.\n",u);
		else if(mp.count(v)==0&&mp.count(u)!=0) printf("ERROR: %d is not found.\n",v);
		else if(ans==u||ans==v) printf("%d is an ancestor of %d.\n",ans,ans==u?v:u);
		else printf("LCA of %d and %d is %d.\n",u,v,ans);
	}
	return 0;
}
```





### A1145(25 hash 平方探查)

```
#include<cstdio>
#include<vector>
#include<cmath>
using namespace std;
bool isprime(int x){
	int sqr=sqrt(1.0*x);
	for(int i=2;i<=sqr;i++){
		if(x%i==0) return false;
	}
	return true;
}
int main(){
	int msize,n,m,a;
	scanf("%d%d%d",&msize,&n,&m);
	while(!isprime(msize)) msize++;
	vector<int> v(msize);
	for(int i=0;i<n;i++){
		scanf("%d",&a);
		int flag=0;
		for(int j=0;j<msize;j++){
			int pos=(a+j*j)%msize;
			if(v[pos]==0){
				flag=1;
				v[pos]=a;
				break;
			}
		}
		if(flag==0) printf("%d cannot be inserted.\n",a);
	}
	int cnt=0,temp;
	for(int i=0;i<m;i++){
		scanf("%d",&temp);
		for(int j=0;j<=msize;j++){//注意这里的<=，因为要回到初始的位置才知道是否能插入
			cnt++;
			if(v[(temp+j*j)%msize]==temp||v[(temp+j*j)%msize]==0) break;
		}
	}
	printf("%.1f",cnt*1.0/m);
    return 0;
}
```



### A1144(20 找丢失的最小正数)

```
#include<iostream>
#include<map>
using namespace std;
int main(){
	int n,ans=1,temp;
    map<int,int> mp;
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>temp;
		if(temp>0) mp[temp]++;
	}
	while(mp[ans]!=0) ans++;
	cout<<ans;
	return 0;
} 
```





### A1145(25 hash 平方探查法插入与查找)

```
#include<cstdio>
#include<cmath>
#include<vector>
#include<algorithm>
using namespace std;
bool isprime(int n){
	if(n<=1) return false;
	int sqr=sqrt(n*1.0);
	for(int i=2;i<=sqr;i++){
		if(n%i==0) return false;
	}
	return true;
}
int main(){
	int msize,n,m,temp,cnt=0;;
	scanf("%d%d%d",&msize,&n,&m);
	while(!isprime(msize)) msize++;
	vector<int> v(msize,0);
	for(int i=0;i<n;i++){
		scanf("%d",&temp);
		int flag=0;
		for(int j=0;j<msize;j++){//注意是小于号 
			int pos=(temp+j*j)%msize;
			if(v[pos]==0){
				v[pos]=temp;
				flag=1;
				break;
			}
		}
		if(flag==0) printf("%d cannot be inserted.\n",temp);
	}
	for(int i=0;i<m;i++){
		scanf("%d",&temp);
		for(int j=0;j<=msize;j++){//注意这里是等号,与插入时不同，要回到初始位置才知道查找失败 
			int pos=(temp+j*j)%msize;
			cnt++;
			if(v[pos]==temp||v[pos]==0) break;
		}
	}
	printf("%.1f",cnt*1.0/m);
}
```



### A1146(25 判断是否为拓扑排序序列)

```
#include<cstdio>
#include<vector>
using namespace std;
int main(){
	int n,m,k,in[1001]={0},flag=0,temp;
	scanf("%d%d",&n,&m);
	vector<int> v[1001];
	for(int i=0;i<m;i++){
		int v1,v2;
		scanf("%d%d",&v1,&v2);
		v[v1].push_back(v2);
		in[v2]++;
	}
	scanf("%d",&k);
	for(int i=0;i<k;i++){
		vector<int> tin(in,in+n+1);
		int judge=1;
		for(int j=0;j<n;j++){
			scanf("%d",&temp);
			if(tin[temp]!=0) judge=0;
			for(int x=0;x<v[temp].size();x++) tin[v[temp][x]]--;
		}
		if(judge==1) continue;
		if(flag==1) printf(" ");
		printf("%d",i);
		flag=1;	
	}
	return 0;
} 
```





### A1147(30  判断大顶堆小顶堆 后序遍历)

```
#include<cstdio>
#include<vector>
using namespace std;
int m, n, num = 0;
vector<int>v;
void dfs(int index) {//后序遍历
	if (index > n) return;
	dfs(index * 2);
	dfs(index * 2 + 1);
	printf("%d", v[index]);
	if (num < n - 1)printf(" ");
	num++;
}
int main() {
	scanf_s("%d%d", &m, &n);
	while (m--) {
		v.resize(n+1);
		bool flagmin = true, flagmax = true;
		for (int i = 1; i <= n; i++) scanf("%d", &v[i]);
		for (int i = 2; i <= n; i++) {
			if (v[i] >= v[i / 2]) flagmax = false;
			if (v[i] <= v[i / 2]) flagmin = false;
		}
		if (flagmin) printf("Min Heap\n");
		else if (flagmax) printf("Max Heap\n");
		else if (flagmax == false && flagmin == false) printf("Not Heap\n");
		dfs(1);
		num = 0;
		printf("\n");
	}
	return 0;
}
```



### A1148(狼人杀 找到两个狼人)

```
#include<iostream>
#include<vector>
#include<cmath>
using namespace std;
int main(){
	int n;
	cin>>n;
	vector<int> v(n+1);
	for(int i=1;i<=n;i++) cin>>v[i];
	for(int i=1;i<=n;i++){
		for(int j=i+1;j<=n;j++){
			vector<int> lie,a(n+1,1);
			a[i]=a[j]=-1;
			for(int k=1;k<=n;k++){
				if(v[k]*a[abs(v[k])]<0) lie.push_back(k);
			}
			if(lie.size()==2&&a[lie[0]]+a[lie[1]]==0){
				cout<<i<<" "<<j;
				return 0;
			}
		}
	}
	cout<<"No Solution";
	return 0;
} 
```



### A1149(25 map)

```
#include<cstdio>
#include<map>
#include<vector>
using namespace std;
int main(){
	int n,m,g1,g2,num;
	scanf("%d%d",&n,&m);
	map<int,vector<int> > mp;
	while(n--){
		scanf("%d%d",&g1,&g2);
		mp[g1].push_back(g2);
		mp[g2].push_back(g1);
	}
	while(m--){
		scanf("%d",&num);
		vector<int> v(num);
		int exist[100005]={0},flag=1;
		for(int i=0;i<num;i++){
			scanf("%d",&v[i]);
			exist[v[i]]=1;
		}
		for(int i=0;i<num;i++){
			for(int j=0;j<mp[v[i]].size();j++){
				 if(exist[mp[v[i]][j]]==1){
				    printf("No\n");
				    flag=0;
				    break;
			    }
			}
			if(flag==0) break;
		}
		if(flag==1) printf("Yes\n");
	}
	return 0;
}
```





### A1150(25 判断循环图 输出最小路径)

```
#include<cstdio>
#include<map>
#include<set>
#include<vector>
using namespace std;
int main(){
	int n,m,k,dis[210][210]={0},mindis=0x7fffffff,ansid;
	scanf("%d%d",&n,&m);
	while(m--){
		int c1,c2,d;
		scanf("%d%d%d",&c1,&c2,&d);
		dis[c1][c2]=dis[c2][c1]=d;
	}
	scanf("%d",&k);
	for(int x=1;x<=k;x++){
		int num,total=0,flag=1;
		scanf("%d",&num);
		set<int> s;
		vector<int> v(num);
		for(int i=0;i<num;i++){
			scanf("%d",&v[i]);
			s.insert(v[i]);
		}
		for(int i=0;i<num-1;i++){
			if(dis[v[i]][v[i+1]]==0){
				flag=0;
				break;
			}
			total+=dis[v[i]][v[i+1]];
		} 
		printf("Path %d:",x);
		if(flag==0) printf(" NA ");
		else printf(" %d ",total);
		if(v[0]!=v[num-1]||s.size()!=n||flag==0) printf("(Not a TS cycle)\n");
		else if(num-1==n&&s.size()==n&&v[0]==v[num-1]){
			if(total<mindis) {
			    mindis=total;
		     	ansid=x;
		    }
			printf("(TS simple cycle)\n");
		} 
		else if(num-1!=n&&s.size()==n&&v[0]==v[num-1]){
			if(total<mindis) {
			    mindis=total;
		     	ansid=x;
		    }
		    printf("(TS cycle)\n");
		} 
	}
	printf("Shortest Dist(%d) = %d",ansid,mindis);
	return 0;
}
```



### A1151(30  树 中序加先序求LCA)

```
方法一:
#include<cstdio>
#include<map>
#include<vector>
using namespace std;
const int maxn=10010;
map<int,int> pos;
vector<int> ins,pre;
void lca(int inl,int inr,int preroot,int a,int b){
	if(inl>inr) return;
	int inroot=pos[pre[preroot]],ain=pos[a],bin=pos[b];
	if(ain<inroot&&bin<inroot) lca(inl,inroot-1,preroot+1,a,b);
	else if((ain<inroot&&bin>inroot)||(ain>inroot&&bin<inroot)){
		printf("LCA of %d and %d is %d.\n",a,b,ins[inroot]);
	}
	else if(ain>inroot&&bin>inroot) lca(inroot+1,inr,preroot+1+(inroot-inl),a,b);
	else if(ain==inroot) printf("%d is an ancestor of %d.\n",a,b);
	else if(bin==inroot) printf("%d is an ancestor of %d.\n",b,a);
}
int main(){
	int m,n,a,b;
	scanf("%d%d",&m,&n);
	ins.resize(n + 1), pre.resize(n + 1);
	for(int i=1;i<=n;i++){
		scanf("%d",&ins[i]);
		pos[ins[i]]=i;
	}
	for(int i=1;i<=n;i++) scanf("%d",&pre[i]);
	for(int i=0;i<m;i++){
		scanf("%d%d",&a,&b);
		if(pos.count(a)==0&&pos.count(b)!=0) printf("ERROR: %d is not found.\n",a);
		if(pos.count(b)==0&&pos.count(a)!=0) printf("ERROR: %d is not found.\n",b);
		if(pos.count(a)==0&&pos.count(b)==0) printf("ERROR: %d and %d are not found.\n",a,b);
		if(pos.count(a)!=0&&pos.count(b)!=0) lca(1,n,1,a,b);
	}
	return 0;
}

方法二:建树加dfs
#include<cstdio>
#include<unordered_map>
using namespace std;
const int maxn=10010;
int in[maxn],pre[maxn];
struct node{
	int v;
	node*left,*right;
};
node* create(int prel,int prer,int inl,int inr){
	if(prel>prer) return NULL;
	node*root=new node;
	root->v=pre[prel];
	int k=inl;
	while(in[k]!=root->v&&k<=inr) k++;
	int leftnum=k-inl;
	root->left=create(prel+1,prel+leftnum,inl,k-1);
	root->right=create(prel+leftnum+1,prer,k+1,inr);
	return root;
}
node* lca(node*root,int a,int b){
	if(root==NULL||root->v==a||root->v==b) return root;
	node*l=lca(root->left,a,b);
	node*r=lca(root->right,a,b);
	if(l==NULL) return r;
	if(r==NULL) return l;
	return root;
}
int main(){
	int m,n,u,v,res;
	scanf("%d%d",&m,&n);
	unordered_map<int,bool>mp;
	for(int i=0;i<n;i++){
		scanf("%d",&in[i]);
		mp[in[i]]=true;
	}
	for(int i=0;i<n;i++) scanf("%d",&pre[i]);
	node*root=create(0,n-1,0,n-1);
	for(int i=0;i<m;i++){
		scanf("%d%d",&u,&v);
		if(mp.count(u)==0&&mp.count(v)==0) printf("ERROR: %d and %d are not found.\n",u,v);
		else if(mp.count(u)==0&&mp.count(v)!=0) printf("ERROR: %d is not found.\n",u);
		else if(mp.count(v)==0&&mp.count(u)!=0) printf("ERROR: %d is not found.\n",v);
		else{
			node*ans=lca(root,u,v);
			res=ans->v;
			if(res==u||res==v) printf("%d is an ancestor of %d.\n",res,res==u?v:u);
		    else printf("LCA of %d and %d is %d.\n",u,v,res);
		}
	}
	return 0;
}
```





### A1152(20 字符串中找第一个k位素数)

```
#include<iostream>
#include<string>
#include<cmath>
using namespace std;
bool isprime(string s){
	if(stoi(s)<=1) return false;
	int sqr=sqrt(stoi(s));
	for(int i=2;i<=sqr;i++){
		if(stoi(s)%i==0) return false;
	}
	return true;
}
int main(){
	int l,k;
    string s;
	cin>>l>>k>>s;
	for(int i=0;i<s.size();i++){
		if(i+k<=s.size()){
			string t=s.substr(i,k);
			if(isprime(t)){
				cout<<t;
				return 0;
			}
		}
	}
	cout<<"404";
	return 0;
}
```



### A1153(25 模拟 排序引用传参  unordered_map)

```
#include<iostream>
#include<vector>
#include<algorithm>
#include<string>
#include<unordered_map>
using namespace std;
struct node {
	string info;
	int score;
};
bool cmp(const node& a, const node& b) {
	return a.score != b.score ? a.score > b.score : a.info < b.info;
}
int main() {
	int n, m, type;
	string t;
	cin >> n >> m;
	vector<node> v(n);
	for (int i = 0; i < n; i++) cin >> v[i].info >> v[i].score;
	for (int i = 1; i <= m; i++) {
		vector<node> ans;
		cin >> type >> t;
		printf("Case %d: %d %s\n", i, type, t.c_str());
		int sum = 0, cnt = 0;
		if (type == 1) {
			for (int j = 0; j < n; j++) {
				if (t[0] == v[j].info[0]) ans.push_back(v[j]);
			}
		}
		else if (type == 2) {
			for (int j = 0; j < n; j++) {
				if (v[j].info.substr(1, 3) == t) {
					cnt++;
					sum += v[j].score;
				}
			}
			if (cnt != 0) printf("%d %d\n", cnt, sum);
		}
		else {
			unordered_map<string, int> m;
			for (int j = 0; j < n; j++) {
				if (v[j].info.substr(4, 6) == t) m[v[j].info.substr(1, 3)]++;
			}
			for (auto it : m) ans.push_back({ it.first,it.second });
		}
		sort(ans.begin(), ans.end(), cmp);
		for (int j = 0; j < ans.size(); j++) printf("%s %d\n", ans[j].info.c_str(), ans[j].score);
		if (((type == 1 || type == 3) && ans.size() == 0) || (type == 2 && cnt == 0)) printf("NA\n");
	}
	return 0;
}
```



### A1154(25 图  边的两端点的判断  )

```
#include<cstdio>
#include<vector>
#include<set>
using namespace std;
struct node{
	int v1,v2;
};
int main(){
	int n,m,k;
	scanf("%d%d",&n,&m);
	vector<node> v(m);
	vector<int> colo(n);
	for(int i=0;i<m;i++) scanf("%d%d",&v[i].v1,&v[i].v2);
	scanf("%d",&k);
	while(k--){
		int flag=1;
		set<int> s;
		for(int i=0;i<n;i++){
			scanf("%d",&colo[i]);
			s.insert(colo[i]);
		}
		for(int i=0;i<m;i++){
			if(colo[v[i].v1]==colo[v[i].v2]){
				flag=0;
				break;
			}
		}
		if(flag==0) printf("No\n");
		else printf("%d-coloring\n",s.size());
	}
	return 0;
}
```



### A1155(30 完全二叉树  判断大顶堆小顶堆 dfs  打印路径)

```
#include<cstdio>
#include<vector>
using namespace std;
int n,t[1005],index;
vector<int> v;
void dfs(int i){
	if(i*2+1>n&&i*2>n){
		if(i<=n){
			for(int i=0;i<v.size();i++){
				if(i>0) printf(" ");
				printf("%d",v[i]);
			}
			printf("\n");
		}
		return;
	}
	v.push_back(t[i*2+1]);
	dfs(i*2+1);
	v.pop_back();
	v.push_back(t[i*2]);
	dfs(i*2);
	v.pop_back();
}
int main(){
	scanf("%d",&n);
	int flagmax=1,flagmin=1;
	for(int i=1;i<=n;i++) scanf("%d",&t[i]);
	v.push_back(t[1]);
	for(int i=2;i<=n;i++){
		if(t[i]<t[i/2]) flagmin=0;
		if(t[i]>t[i/2]) flagmax=0;
	}
	dfs(1);
	if(flagmin==1) printf("Min Heap");
	else if(flagmax==1) printf("Max Heap");
	else if(flagmin==0&&flagmax==0) printf("Not Heap");
	return 0;
} 
```

