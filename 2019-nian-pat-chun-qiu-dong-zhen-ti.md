---
title: '2019年PAT春、秋、冬真题'
date: 2020-07-19 21:39:55
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
### 1156(20 Sexy Primes  素数)

题目描述：

Sexy primes are pairs of primes of the form (p, p+6), so-named since “sex” is the Latin word for “six”. (Quoted from http://mathworld.wolfram.com/SexyPrimes.html)

Now given an integer, you are supposed to tell if it is a sexy prime.

Input Specification:
Each input file contains one test case. Each case gives a positive integer N (≤10^8).

Output Specification:
For each case, print in a line Yes if N is a sexy prime, then print in the next line the other sexy prime paired with N (if the answer is not unique, output the smaller number). Or if N is not a sexy prime, print No instead, then print in the next line the smallest sexy prime which is larger than N.

```
Sample Input 1:
47
Sample Output 1:
Yes
41
Sample Input 2:
21
Sample Output 2:
No
23
```


————————————————

```
#include<iostream>
#include<cmath>
using namespace std;
bool isprime(int x) {
	if(x<=1) return false;
	int sqr=sqrt(1.0*x);
	for(int i=2; i<=sqr; i++) {
		if(x%i==0) return false;
	}
	return true;
}
bool sexprime(int x) {
	if(((isprime(x)&&isprime(x+6)))||((isprime(x))&&isprime(x-6)))
		return true;
	return false;
}
int main() {
	int n;
	cin>>n;
	if(sexprime(n)) cout<<"Yes\n"<<n-6;
	else {
		cout<<"No\n";
		while(!sexprime(n)) n++;
		cout<<n;
	}
	return 0;
}
```





### 1157(25 Anniversary unordered_map/set)

题目描述:

Zhejiang University is about to celebrate her 122th anniversary in 2019. To prepare for the celebration, the alumni association （校友会） has gathered the ID’s of all her alumni. Now your job is to write a program to count the number of alumni
among all the people who come to the celebration.

Input Specification:
Each input file contains one test case. For each case, the first part is about the information of all the alumni. Given in the first line is a positive integer NNN (≤105).
Then NNN lines follow, each contains an ID number of an alumnus. An ID number is a string of 18 digits or the letter X. It is guaranteed that all the ID’s are distinct.
The next part gives the information of all the people who come to the celebration.
Again given in the first line is a positive integer MMM(≤105). Then MMM lines follow, each contains an ID number of a guest. It is guaranteed that all the ID’s are distinct.

Output Specification:
First print in a line the number of alumni among all the people who come to the celebration. Then in the second line, print the ID of the oldest alumnus – notice that the 7th - 14th digits of the ID gives one’s birth date. If no alumnus comes, output the ID of the oldest guest instead. It is guaranteed that such an alumnus or guest is unique.

```
Sample Input:
5
372928196906118710
610481197806202213
440684198612150417
13072819571002001X
150702193604190912
6
530125197901260019
150702193604190912
220221196701020034
610481197806202213
440684198612150417
370205198709275042

Sample Output:
3
150702193604190912
```



```
#include<iostream>
#include<string>
#include<unordered_map>
using namespace std;
int main() {
	int n,m,cnt=0;
	cin>>n;
	getchar();//注意 
	string s,oldest="999999999999999999";
	unordered_map<string,bool> mp;
	for(int i=0; i<n; i++) {
		cin>>s;
		if(s.substr(6,8)<oldest.substr(6,8)) oldest=s;
		mp[s]=true;
	}
	cin>>m;
	while(m--) {
		cin>>s;
		if(mp[s]==true) cnt++;
	}
	if(cnt!=0) cout<<cnt<<endl<<oldest;
    return 0;
}
```





### 1158(25 电信诈骗 并查集 图)

题目描述:

Telefraud（电信诈骗） remains a common and persistent problem in our society. In some cases, unsuspecting victims lose their entire life savings. To stop this crime, you are supposed to write a program to detect those suspects from a huge amount of phone call records.

A person must be detected as a suspect if he/she makes more than K short phone calls to different people everyday, but no more than 20% of these people would call back. And more, if two suspects are calling each other, we say they might belong to the same gang. A makes a short phone call to B means that the total duration of the calls from A to B is no more than 5 minutes.

Input Specification:
Each input file contains one test case. For each case, the first line gives 3 positive integers K (≤500, the threshold（阈值） of the amount of short phone calls), N (≤10^3, the number of different phone numbers), and M (≤10^5, the number of phone call records). Then M lines of one day's records are given, each in the format:

```
caller receiver duration
```

where caller and receiver are numbered from 1 to N, and duration is no more than 1440 minutes in a day.

Output Specification:
Print in each line all the detected suspects in a gang, in ascending order of their numbers. The gangs are printed in ascending order of their first members. The numbers in a line must be separated by exactly 1 space, and there must be no extra space at the beginning or the end of the line.

If no one is detected, output None instead.

```
Sample Input 1:

5 15 31
1 4 2
1 5 2
1 5 4
1 7 5
1 8 3
1 9 1
1 6 5
1 15 2
1 15 5
3 2 2
3 5 15
3 13 1
3 12 1
3 14 1
3 10 2
3 11 5
5 2 1
5 3 10
5 1 1
5 7 2
5 6 1
5 13 4
5 15 1
11 10 5
12 14 1
6 1 1
6 9 2
6 10 5
6 11 2
6 12 1
6 13 1

Sample Output 1:
3 5
6

Sample Input 2:
5 7 8
1 2 1
1 3 1
1 4 1
1 5 1
1 6 1
1 7 1
2 1 1
3 1 1

Sample Output 2:
None
```

```
#include<cstdio>
#include<algorithm>
#include<vector>
#include<unordered_map>
#include<map>
using namespace std;
const int maxn=1001;
int k,n,m,father[maxn],g[maxn][maxn],gangnum=0;
int findfather(int x){
	while(x!=father[x]) x=father[x];
	return x;
}
void Union(int a,int b) {
	int faA=findfather(a);
	int faB=findfather(b);
	if(faA>faB) father[faA]=faB;
	else if(faA<faB) father[faB]=faA;
}
int main() {
	for(int i=1; i<=maxn; i++) father[i]=i;
	scanf("%d%d%d",&k,&n,&m);
	vector<int> gang[n+1],susp;
	int v1,v2,time;
	fill(g[0],g[0]+maxn*maxn,0);
	for(int i=0; i<m; i++) {
		scanf("%d%d%d",&v1,&v2,&time);
		g[v1][v2]=time;
	}
	for(int i=1;i<=n;i++){
		int total=0,cnt=0;
		for(int j=1;j<=n;j++){
			if(g[i][j]>0&&g[i][j]<=5){
				total++;
				if(g[j][i]>0) cnt++;
			}
		}
		if((double)(cnt*1.0/total)<=0.2&&total>k) susp.push_back(i);
	}
	for(int i=0;i<susp.size();i++){
		for(int j=0;j<susp.size();j++){
			if(g[susp[i]][susp[j]]>0&&g[susp[j]][susp[i]]>0) Union(susp[i],susp[j]);
		}
	}
	for(int i=0;i<susp.size();i++){
		gang[findfather(susp[i])].push_back(susp[i]);
	}
	for(int i=1;i<=n;i++){
		if(gang[i].size()!=0){
			for(int j=0;j<gang[i].size();j++){
				if(j>0) printf(" ");
				printf("%d",gang[i][j]);
			}
			gangnum++;
			printf("\n");
		}
	}
	if(gangnum==0) printf("None");
	return 0;
}
```









### 1159(30 map、sscanf、中序后序建树加树的综合性考察)

题目描述:

Suppose that all the keys in a binary tree are distinct positive integers. Given the postorder and inorder traversal sequences, a binary tree can be uniquely determined.

Now given a sequence of statements about the structure of the resulting tree, you are supposed to tell if they are correct or not. A statment is one of the following:

```
A is the root
A and B are siblings
A is the parent of B
A is the left child of B
A is the right child of B
A and B are on the same level
It is a full tree
```

Note:

* Two nodes are on the same level, means that they have the same depth.

* A full binary tree is a tree in which every node other than the
  leaves has two children.

**Input Specification:**
Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the postorder sequence and the third line gives the inorder sequence. All the numbers in a line are no more than 103 and are separated by a space.

Then another positive integer M (≤30) is given, followed by M lines of statements. It is guaranteed that both A and B in the statements are in the tree.

**Output Specification:**
For each statement, print in a line Yes if it is correct, or No if not.

**Sample Input:**

```
9
16 7 11 32 28 2 23 8 15
16 23 7 32 11 2 28 15 8
7
15 is the root
8 and 2 are siblings
32 is the parent of 11
23 is the left child of 16
28 is the right child of 2
7 and 11 are on the same level
It is a full tree
```

**Sample Output:**

```
Yes
No
Yes
No
Yes
Yes
Yes
```





```
#include<iostream>
#include<string>
#include<vector>
#include<unordered_map>
using namespace std;
struct node {
	int v,level;
	node*parent,*lc,*rc;
	node(int val,int lev) : v(val),parent(NULL),lc(NULL),rc(NULL),level(lev) {}  //构造函数 
};
unordered_map<int,node*> mp;//妙啊
vector<int> in,post;
node*root=NULL;
bool isfull=true;
node*create(int postl,int postr,int inl,int inr,int level) {
	if(postl>postr) return NULL;
	node*root=new node(post[postr],level);
	int k=inl;
	while(k<=inr&&in[k]!=root->v) k++;
	int leftnum=k-inl;
	root->lc=create(postl,postl+leftnum-1,inl,k-1,level+1);
	root->rc=create(postl+leftnum,postr-1,k+1,inr,level+1);
	if(root->lc!=NULL) root->lc->parent=root;
	if(root->rc!=NULL) root->rc->parent=root;
	if((root->lc==NULL&&root->rc!=NULL)||(root->lc!=NULL&&root->rc==NULL)) isfull=false;
	mp[root->v]=root;
	return root;
}
bool judge(string s) {
	int a,b;
	if(s.find("root")!=string::npos) {
		sscanf(s.c_str(),"%d is the root",&a);
		return a==root->v;
	} else if(s.find("siblings")!=string::npos) {
		sscanf(s.c_str(),"%d and %d are siblings",&a,&b);
		return mp[a]->parent==mp[b]->parent;
	} else if(s.find("parent")!=string::npos) {
		sscanf(s.c_str(),"%d is the parent of %d",&a,&b);
		return mp[b]->parent==mp[a];
	} else if(s.find("left")!=string::npos) {
		sscanf(s.c_str(),"%d is the left child of %d",&a,&b);
		return mp[b]->lc==mp[a]; //注意在判断时一定要判断地址是否相等， 不要判断数值，因为可能会有空地址 
	} else if(s.find("right")!=string::npos) {
		sscanf(s.c_str(),"%d is the right child of %d",&a,&b);
		return mp[b]->rc==mp[a];
	}else if(s.find("on")!=string::npos){
		sscanf(s.c_str(),"%d and %d are on the same level",&a,&b);
		return mp[a]->level==mp[b]->level;
	}else return isfull;
}
int main(){
	int n,m;
	string s;
	cin>>n;
	in.resize(n),post.reserve(n);
	for(int i=0;i<n;i++) cin>>post[i];
	for(int i=0;i<n;i++) cin>>in[i];
	root=create(0,n-1,0,n-1,0);
	cin>>m;
	getchar();//注意 
	while(m--){
		getline(cin,s);
		if(judge(s)) cout<<"Yes\n";
		else cout<<"No\n";
	}
	return 0;
}
```



### 1160 (20 Forever dfs+回溯剪枝)

题目描述:

"Forever number" is a positive integer *A* with *K* digits, satisfying the following constrains:

- the sum of all the digits of *A* is *m*;
- the sum of all the digits of *A*+1 is *n*; and
- the greatest common divisor of *m* and *n* is a prime number which is greater than 2.

Now you are supposed to find these forever numbers.

Input Specification:

Each input file contains one test case. For each test case, the first line contains a positive integer *N* (≤5). Then *N* lines follow, each gives a pair of *K* (3<*K*<10) and *m* (1<*m*<90), of which the meanings are given in the problem description.

 Output Specification:

For each pair of *K* and *m*, first print in a line `Case X`, where `X` is the case index (starts from 1). Then print *n* and *A* in the following line. The numbers must be separated by a space. If the solution is not unique, output in the ascending order of *n*. If still not unique, output in the ascending order of *A*. If there is no solution, output `No Solution`.

**Sample Input:**

```
2
6 45
7 80
```



**Sample Output:**

```
Case 1
10 189999
10 279999
10 369999
10 459999
10 549999
10 639999
10 729999
10 819999
10 909999
Case 2
No Solution
```





```
方法一:dfs
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;
struct node {
	int n, A;	//n A+1各个位数之和 A求的数
};
int n,m,N,k;
vector<node> ans;
bool cmp(node a,node b) {	//按照n递增排序，n相同按照A递增排序
	if(a.n != b.n) return a.n < b.n;
	else return a.A < b.A;
}
bool isPrime(int x) {	//判断是不是大于3的素数
	if(x <= 2) return false;
	for(int i = 2; i * i<= x; i++)
		if(x % i == 0) return false;
	return true;
}
int gcd(int a,int b) {	//求最大公约数
	return b == 0 ? a : gcd(b,a%b);
}
int digit_sum(int A) {	//各个位数之和
	int sum = 0;
	while(A) {
		sum += A%10;
		A/=10;
	}
	return sum;
}
void DFS(int A, int sum, int rest_K) {	//A目标数 sum当前位数和 rest_k剩余k位
	if(rest_K == 0) {	//递归边界
		if(sum == m) {
			int n = digit_sum(A + 1);
			if(isPrime(gcd(m, n)))
				ans.push_back({n, A});
		}
	} else if(rest_K > 0) {
		for(int i = 0; i <= 9; i++) {
			//如果当前位为i，即时剩余的rest_k - 1位全部取9也无法达到m，则剪枝
			//当前位数和sum，当前位取i，已经超过了m也剪枝
			if(sum + i + (rest_K-1)*9 >= m && sum + i <= m)
				DFS(A*10 + i, sum+i, rest_K-1);
		}
	}
}
int main() {
	scanf("%d",&N);	//样例数
	for(int x = 1; x <= N; x++) {
		ans.clear();
		printf("Case %d\n",x);
		scanf("%d%d",&k,&m);	//k A的位数， m A各位数之和
		for(int i = 1; i <= 9; i++)	//i是当前位数
			DFS(i, i, k-1);
		if(ans.empty()) {
			printf("No Solution\n");
			continue;
		}
		sort(ans.begin(), ans.end(), cmp);
		for(int i = 0; i < ans.size(); i++) {
			printf("%d %d\n",ans[i].n, ans[i].A);
		}
	}
	return 0;
}

方法二:数学规律
按照尾数为99的数枚举，加一不进位的话，这两个数的各位之和相差的是1，无论如何也不会有不小于三的最小公约数
#include <iostream>
#include <vector>
#include <algorithm>
#include <set>
#include <map>
#include <cmath>
using namespace std;
int k, n,m,a,b,c,d,num;
bool f=false;
struct node {
	int n,a;
};
int gcd(int a,int b) {
	return b==0?a:gcd(b,a%b);
}
bool isPrime(int n) {
	if(n<=2)return false;
	int sq=(int)sqrt(n);
	for(int j=2; j<=sq; j++) {
		if(n%j==0)return false;
	}
	return true;
}
int sum(int n) {
	return n==0? 0: n%10 + sum(n/10);
}
bool cmp(node &a,node &b) {
	if(a.n!=b.n)return a.n<b.n;
	return a.a<b.a;
}
int main() {
	scanf("%d", &n);
	for(int i=0; i<n; i++) {
		scanf("%d %d",&k,&m);
		printf("Case %d\n",i+1);
		a=(int)pow(10,k-3);//-3
		b=a*10;
		vector<node> v;
		for(int x=a; x<b; x++) {
			c=x*100+99;
			d=sum(c+1);
			if(sum(c)==m && isPrime(gcd(d,m))) {
				v.push_back({d,c});
			}
		}
		if(v.size()==0) printf("No Solution\n");
		else {
			sort(v.begin(),v.end(),cmp);
			for(auto it:v) {
				printf("%d %d\n",it.n,it.a);
			}
		}
	}
	return 0;
}
```



### 1161(25 合并链表)

**Merging Linked Lists** (25分)

题目描述: 

Merging Linked Lists (25分)

Given two singly linked lists *L*1=*a*1→*a*2→⋯→*a**n*−1→*a**n* and *L*2=*b*1→*b*2→⋯→*b**m*−1→*b**m*. If *n*≥2*m*, you are supposed to reverse and merge the shorter one into the longer one to obtain a list like *a*1→*a*2→*b**m*→*a*3→*a*4→*b**m*−1⋯. For example, given one list being 6→7 and the other one 1→2→3→4→5, you must output 1→2→7→3→4→6→5.

 Input Specification:

Each input file contains one test case. For each case, the first line contains the two addresses of the first nodes of *L*1 and *L*2, plus a positive *N* (≤105) which is the total number of nodes given. The address of a node is a 5-digit nonnegative integer, and NULL is represented by `-1`.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is a positive integer no more than 105, and `Next` is the position of the next node. It is guaranteed that no list is empty, and the longer list is at least twice as long as the shorter one.

 Output Specification:

For each case, output in order the resulting linked list. Each node occupies a line, and is printed in the same format as in the input.

**Sample Input:**

```in
00100 01000 7
02233 2 34891
00100 6 00001
34891 3 10086
01000 1 02233
00033 5 -1
10086 4 00033
00001 7 -1
```

 **Sample Output:**

```out
01000 1 02233
02233 2 00001
00001 7 34891
34891 3 10086
10086 4 00100
00100 6 00033
00033 5 -1
```



```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
const int maxn=100010;
struct node {
	int address,data,next;
} list[maxn];
int main() {
	int n,st1,st2;
	vector<node> v1,v2,v;
	scanf("%d%d%d",&st1,&st2,&n);
	for(int i=0; i<n; i++) {
		int addrs;
		scanf("%d",&addrs);
		list[addrs].address=addrs;
		scanf("%d%d",&list[addrs].data,&list[addrs].next);
	}
	while(st1!=-1) {
		v1.push_back(list[st1]);
		st1=list[st1].next;
	}
	while(st2!=-1) {
		v2.push_back(list[st2]);
		st2=list[st2].next;
	}
	int len1=v1.size(),len2=v2.size(),index=0;
	if(len1>len2) {
		reverse(v2.begin(),v2.end());
		for(int i=0; i<=len1; i++) {
			if(i!=0&&i%2==0&&index<len2) {
				v.push_back(v2[index++]);
			}
			if(i<len1) v.push_back(v1[i]);
		}
	} else {
		reverse(v1.begin(),v1.end());
		for(int i=0; i<=len2; i++) {
			if(i!=0&&i%2==0&&index<len1) {
				v.push_back(v1[index++]);
			}
			if(i<len2) v.push_back(v2[i]);
		}
	}
	for(int i=0;i<v.size()-1;i++) printf("%05d %d %05d\n",v[i].address,v[i].data,v[i+1].address);
	printf("%05d %d -1",v[v.size()-1].address,v[v.size()-1].data);
	return 0;
}
```



### 1162(25 输出树的后缀表达式)

**Postfix Expression**(25分)

Given a syntax tree (binary), you are supposed to output the corresponding postfix expression, with parentheses reflecting the precedences of the operators.

 Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 20) which is the total number of nodes in the syntax tree. Then N lines follow, each gives the information of a node (the *i*-th line corresponds to the *i*-th node) in the format:

```
data left_child right_child
```

where `data` is a string of no more than 10 characters, `left_child` and `right_child` are the indices of this node's left and right children, respectively. The nodes are indexed from 1 to N. The NULL link is represented by −1. The figures 1 and 2 correspond to the samples 1 and 2, respectively.

![https://images.ptausercontent.com/4d1c4a98-33cc-45ff-820f-c548845681ba.JPG](C:\Users\lxb\Pictures\树.png)



 Output Specification:

For each case, print in a line the postfix expression, with parentheses reflecting the precedences of the operators.There must be no space between any symbols.

**Sample Input 1:**

```in
8
* 8 7
a -1 -1
* 4 1
+ 2 5
b -1 -1
d -1 -1
- -1 6
c -1 -1
```

**Sample Output 1:**

```out
(((a)(b)+)((c)(-(d))*)*)
```

 **Sample Input 2:**

```in
8
2.35 -1 -1
* 6 1
- -1 4
% 7 8
+ 2 3
a -1 -1
str -1 -1
871 -1 -1
```

**Sample Output 2:**

```out
(((a)(2.35)*)(-((str)(871)%))+)
```

```
#include<iostream>
#include<string>
using namespace std;
struct node {
	string data;
	int lc,rc;
} v[21];
string ans;
void postorder(int index) {
	bool flag=false;
	if((v[index].rc!=-1&&v[index].lc!=-1)||(v[index].lc==-1&&v[index].rc!=-1)||(v[index].lc==-1&&v[index].rc==-1)) {
		flag=true;
		ans+="(";
	}
	if(v[index].lc==-1&&v[index].rc!=-1) {
		ans+=v[index].data;
		postorder(v[index].rc);
	} else {
		if(v[index].lc!=-1)postorder(v[index].lc);
		if(v[index].rc!=-1)postorder(v[index].rc);
		ans+=v[index].data;
	}
	if(flag) ans+=")";
}
int main() {
	int n,root=1,notroot[21]= {0};
	cin>>n;
	for(int i=1; i<=n; i++) {
		cin>>v[i].data>>v[i].lc>>v[i].rc;
		if(v[i].lc!=-1) notroot[v[i].lc]=1;
		if(v[i].rc!=-1) notroot[v[i].rc]=1;
	}
	while(notroot[root]==1) root++;
	postorder(root);
	cout<<ans;
	return 0;
}
```



### 1163(30 判断dijkst的合法路径)

Dijkstra Sequence (30分)

Dijkstra's algorithm is one of the very famous greedy algorithms. It is used for solving the single source shortest path problem which gives the shortest paths from one particular source vertex to all the other vertices of the given graph. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later.

In this algorithm, a set contains vertices included in shortest path tree is maintained. During each step, we find one vertex which is not yet included and has a minimum distance from the source, and collect it into the set. Hence step by step an ordered sequence of vertices, let's call it **Dijkstra sequence**, is generated by Dijkstra's algorithm.

On the other hand, for a given graph, there could be more than one Dijkstra sequence. For example, both { 5, 1, 3, 4, 2 } and { 5, 3, 1, 2, 4 } are Dijkstra sequences for the graph, where 5 is the source. Your job is to check whether a given sequence is Dijkstra sequence or not.

 Input Specification:

Each input file contains one test case. For each case, the first line contains two positive integers *N**v* (≤103) and *N**e* (≤105), which are the total numbers of vertices and edges, respectively. Hence the vertices are numbered from 1 to *N**v*.

Then *N**e* lines follow, each describes an edge by giving the indices of the vertices at the two ends, followed by a positive integer weight (≤100) of the edge. It is guaranteed that the given graph is connected.

Finally the number of queries, *K*, is given as a positive integer no larger than 100, followed by *K* lines of sequences, each contains a permutationof the *N**v* vertices. It is assumed that the first vertex is the source for each sequence.

All the inputs in a line are separated by a space.

 Output Specification:

For each of the *K* sequences, print in a line `Yes` if it is a Dijkstra sequence, or `No` if not.

 **Sample Input:**

```in
5 7
1 2 2
1 5 1
2 3 1
2 4 1
2 5 2
3 5 1
3 4 1
4
5 1 3 4 2
5 3 1 2 4
2 3 4 5 1
3 2 1 5 4
```

 **Sample Output:**

```out
Yes
Yes
Yes
No
```



```
#include<cstdio>
#include<algorithm>
#include<vector>
using namespace std;
const int maxn=1001;
const int INF=0x7fffffff;
int g[maxn][maxn],d[maxn],nv,ne,k,index;
bool vis[maxn];
vector<int> path;
bool judge(int s) {
	fill(vis,vis+maxn,false);
	fill(d,d+maxn,INF);
	d[s]=0;
	for(int i=1; i<=nv; i++) {
		int u=-1,MIN=INF;
		for(int j=1; j<=nv; j++) {
			if(vis[j]==false&&d[j]<MIN) {
				MIN=d[j];
				u=j;
			}
		}
		if(u==-1) return true;
		if(d[path[index]]==MIN&&vis[path[index]]==false) {
			u=path[index];
			index++;
			vis[u]=true;
		} else return false;
		for(int v=1; v<=nv; v++) {
			if(vis[v]==false&&g[u][v]!=INF) {
				if(d[v]>d[u]+g[u][v]) d[v]=d[u]+g[u][v];
			}
		}
	}
}
int main() {
	scanf("%d%d",&nv,&ne);
	fill(g[0],g[0]+maxn*maxn,INF);
	for(int i=0; i<ne; i++) {
		int v1,v2;
		scanf("%d%d",&v1,&v2);
		scanf("%d",&g[v1][v2]);
		g[v2][v1]=g[v1][v2];
	}
	scanf("%d",&k);
	while(k--) {
		path.clear();
		path.resize(nv);
		for(int i=0; i<nv; i++) scanf("%d",&path[i]);
		index=0;
		if(judge(path[index])) printf("Yes\n");
		else printf("No\n");
	}
	return 0;
}
```



### 1164(20 图形输出 逻辑题 注意用string，getchar吸收换行符)

7-1 Good in C (20分)

When your interviewer asks you to write "Hello World" using C, can you do as the following figure shows?

 Input Specification:

Each input file contains one test case. For each case, the first part gives the 26 capital English letters A-Z, each in a 7×5 matrix of `C`'s and `.`'s. Then a sentence is given in a line, ended by a return. The sentence is formed by several words (no more than 10 continuous capital English letters each), and the words are separated by any characters other than capital English letters.

It is guaranteed that there is at least one word given.

 Output Specification:

For each word, print the matrix form of each of its letters in a line, and the letters must be separated by exactly one column of space. There must be no extra space at the beginning or the end of the word.

Between two adjacent words, there must be a single empty line to separate them. There must be no extra line at the beginning or the end of the output.

 Sample Input:

```in
..C..
.C.C.
C...C
CCCCC
C...C
C...C
C...C
CCCC.
C...C
C...C
CCCC.
C...C
C...C
CCCC.
.CCC.
C...C
C....
C....
C....
C...C
.CCC.
CCCC.
C...C
C...C
C...C
C...C
C...C
CCCC.
CCCCC
C....
C....
CCCC.
C....
C....
CCCCC
CCCCC
C....
C....
CCCC.
C....
C....
C....
CCCC.
C...C
C....
C.CCC
C...C
C...C
CCCC.
C...C
C...C
C...C
CCCCC
C...C
C...C
C...C
CCCCC
..C..
..C..
..C..
..C..
..C..
CCCCC
CCCCC
....C
....C
....C
....C
C...C
.CCC.
C...C
C..C.
C.C..
CC...
C.C..
C..C.
C...C
C....
C....
C....
C....
C....
C....
CCCCC
C...C
C...C
CC.CC
C.C.C
C...C
C...C
C...C
C...C
C...C
CC..C
C.C.C
C..CC
C...C
C...C
.CCC.
C...C
C...C
C...C
C...C
C...C
.CCC.
CCCC.
C...C
C...C
CCCC.
C....
C....
C....
.CCC.
C...C
C...C
C...C
C.C.C
C..CC
.CCC.
CCCC.
C...C
CCCC.
CC...
C.C..
C..C.
C...C
.CCC.
C...C
C....
.CCC.
....C
C...C
.CCC.
CCCCC
..C..
..C..
..C..
..C..
..C..
..C..
C...C
C...C
C...C
C...C
C...C
C...C
.CCC.
C...C
C...C
C...C
C...C
C...C
.C.C.
..C..
C...C
C...C
C...C
C.C.C
CC.CC
C...C
C...C
C...C
C...C
.C.C.
..C..
.C.C.
C...C
C...C
C...C
C...C
.C.C.
..C..
..C..
..C..
..C..
CCCCC
....C
...C.
..C..
.C...
C....
CCCCC
HELLO~WORLD!
```

Sample Output:

```out
C...C CCCCC C.... C.... .CCC.
C...C C.... C.... C.... C...C
C...C C.... C.... C.... C...C
CCCCC CCCC. C.... C.... C...C
C...C C.... C.... C.... C...C
C...C C.... C.... C.... C...C
C...C CCCCC CCCCC CCCCC .CCC.

C...C .CCC. CCCC. C.... CCCC.
C...C C...C C...C C.... C...C
C...C C...C CCCC. C.... C...C
C.C.C C...C CC... C.... C...C
CC.CC C...C C.C.. C.... C...C
C...C C...C C..C. C.... C...C
C...C .CCC. C...C CCCCC CCCC.
```







```
#include<iostream>
#include<cctype>
#include<vector>
using namespace std;
string a[30][10],str;
vector<int> word;
int main() {
	for(int i=0; i<26; i++) {
		for(int j=0; j<7; j++) {
			cin>>a[i][j];
		}
	}
	getchar();//因为cin会把换行符留在缓冲区，所以这里要将换行符忽略掉
	getline(cin,str);
	bool flag=false;
	for(int i=0; i<str.size(); i++) {
		if(isupper(str[i]) && i==str.size()-1)
			word.push_back(str[i]-'A');
		if(isupper(str[i]) && i!=str.size()-1) {
			word.push_back(str[i]-'A');
		} else {
			if(word.size()==0) cout<<"";
			else {
				if(flag) cout<<endl<<endl;
				for(int k=0; k<7; k++) {
					for(int j=0; j<word.size(); j++) {
						if(j!=0) cout<<" ";
						cout<<a[word[j]][k];
						flag=true;
					}
					if(k!=6)cout<<endl;
				}
				word.clear();
			}
		}
	}
	return 0;
}
```



### 1165(25 链表分段拼接)

Given a singly linked list *L*. Let us consider every *K* nodes as a **block** (if there are less than *K* nodes at the end of the list, the rest of the nodes are still considered as a block). Your job is to reverse all the blocks in *L*. For example, given *L* as 1→2→3→4→5→6→7→8 and *K* as 3, your output must be 7→8→4→5→6→1→2→3.

Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive *N* (≤105) which is the total number of nodes, and a positive *K* (≤*N*) which is the size of a block. The address of a node is a 5-digit nonnegative integer, and NULL is represented by −1.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is an integer, and `Next` is the position of the next node.

 Output Specification:

For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

 Sample Input:

```in
00100 8 3
71120 7 88666
00000 4 99999
00100 1 12309
68237 6 71120
33218 3 00000
99999 5 68237
88666 8 -1
12309 2 33218
```

Sample Output:

```out
71120 7 88666
88666 8 00000
00000 4 99999
99999 5 68237
68237 6 00100
00100 1 12309
12309 2 33218
33218 3 -1
```



```
#include<cstdio>
#include<vector>
using namespace std;
struct node{
	int addrs,data,next;
}list[100000];
int main(){
	int st,n,k,cnt,len;
	scanf("%d%d%d",&st,&n,&k);
	for(int i=0;i<n;i++){
		int addrs;
		scanf("%d",&addrs);
		list[addrs].addrs=addrs;
		scanf("%d%d",&list[addrs].data,&list[addrs].next);
	}
	vector<node> v;
	while(st!=-1){
		v.push_back(list[st]);
		st=list[st].next;
	}
	if(v.size()%k==0) cnt=v.size()/k;
	else cnt=v.size()/k+1;
	int pos;
	for(int i=0;i<cnt;i++){
		int j,x;
		if(i==0) j=v.size()-(cnt-1)*k;
		else j=k;
		if(i==0) pos=v.size()-j;
		for(x=pos;x<pos+j-1;x++) printf("%05d %d %05d\n",v[x].addrs,v[x].data,v[x+1].addrs);
		pos-=k;
		if(pos<0) printf("%05d %d -1\n",v[x].addrs,v[x].data);
		else printf("%05d %d %05d\n",v[x].addrs,v[x].data,v[pos].addrs);
	}
	return 0;
}
```



### 1166(25 图)

7-3 Summit (25分)

A **summit** (峰会) is a meeting of heads of state or government. Arranging the rest areas for the summit is not a simple job. The ideal arrangement of one area is to invite those heads so that everyone is a direct friend of everyone.

Now given a set of tentative arrangements, your job is to tell the organizers whether or not each area is all set.

 Input Specification:

Each input file contains one test case. For each case, the first line gives two positive integers N (≤ 200), the number of heads in the summit, and M, the number of friendship relations. Then M lines follow, each gives a pair of indices of the heads who are friends to each other. The heads are indexed from 1 to N.

Then there is another positive integer K (≤ 100), and K lines of tentative arrangement of rest areas follow, each first gives a positive number L (≤ N), then followed by a sequence of L distinct indices of the heads. All the numbers in a line are separated by a space.

 Output Specification:

For each of the K areas, print in a line your advice in the following format:

- if in this area everyone is a direct friend of everyone, and no friend is missing (that is, no one else is a direct friend of everyone in this area), print `Area X is OK.`.
- if in this area everyone is a direct friend of everyone, yet there are some other heads who may also be invited without breaking the ideal arrangement, print `Area X may invite more people, such as H.` where `H` is the smallest index of the head who may be invited.
- if in this area the arrangement is not an ideal one, then print `Area X needs help.` so the host can provide some special service to help the heads get to know each other.

Here `X` is the index of an area, starting from 1 to `K`.

 Sample Input:

```in
8 10
5 6
7 8
6 4
3 6
4 5
2 3
8 2
2 7
5 3
3 4
6
4 5 4 3 6
3 2 8 7
2 2 3
1 1
2 4 6
3 3 2 1
```

Sample Output:

```out
Area 1 is OK.
Area 2 is OK.
Area 3 is OK.
Area 4 is OK.
Area 5 may invite more people, such as 3.
Area 6 needs help.
```



```
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
const int maxn=205;
int main() {
	int n,m,g[maxn][maxn],k,num,vis[maxn];
	fill(g[0],g[0]+maxn*maxn,0);
	scanf("%d%d",&n,&m);
	for(int i=0; i<m; i++) {
		int v1,v2;
		scanf("%d%d",&v1,&v2);
		g[v1][v2]=g[v2][v1]=1;
	}
	scanf("%d",&k);
	for(int i=1; i<=k; i++) {
		fill(vis,vis+maxn,false);
		scanf("%d",&num);
		vector<int> v(num);
		int flag=1;
		for(int j=0; j<num; j++) {
			scanf("%d",&v[j]);
			vis[v[j]]=true;
		}
		for(int j=0; j<num-1; j++) {
			for(int x=j+1; x<num; x++) {
				if(flag==0) break;
				if(g[v[j]][v[x]]!=1) {
					flag=0;
					break;
				}
			}
		}
		if(flag==0) {
			printf("Area %d needs help.\n",i);
			continue;
		} else {
			for(int j=1; j<=n; j++) {
				if(vis[j]==false) {
					int x;
					for(x=0; x<v.size(); x++) {
						if(g[v[x]][j]==0) break;
					}
					if(x==v.size()) {
						printf("Area %d may invite more people, such as %d.\n",i,j);
						flag=0;
						break;
					}
				}
			}
			if(flag==1) printf("Area %d is OK.\n",i);
		}
	}
	return 0;
}
```



### 1167(30 用中序遍历序列建立“堆”树)

7-4 Cartesian Tree (30分)

A **Cartesian tree** is a binary tree constructed from a sequence of distinct numbers. The tree is heap-ordered, and an inorder traversal returns the original sequence. For example, given the sequence { 8, 15, 3, 4, 1, 5, 12, 10, 18, 6 }, the min-heap Cartesian tree is shown by the figure.

![CTree.jpg](https://images.ptausercontent.com/6a99f68a-6578-46e0-9232-fbf0adf3691f.jpg)

Your job is to output the level-order traversal sequence of the min-heap Cartesian tree.

 Input Specification:

Each input file contains one test case. Each case starts from giving a positive integer *N* (≤30), and then *N* distinct numbers in the next line, separated by a space. All the numbers are in the range of **int**.

 Output Specification:

For each test case, print in a line the level-order traversal sequence of the min-heap Cartesian tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the beginning or the end of the line.

 Sample Input:

```in
10
8 15 3 4 1 5 12 10 18 6
```

 Sample Output:

```out
1 3 5 8 4 6 15 10 12 18
```



```
#include<cstdio>
#include<vector>
#include<queue>
#include<algorithm>
using namespace std;
struct node {
	int v;
	node*lc,*rc;
};
int n,v[31],num=0;
node*create(int inl,int inr) {
	if((inl>inr)||inl<0||inr>=n) return NULL;
	node*root=new node;
	int *addrs=min_element(v+inl,v+inl+(inr-inl+1));
	int index=addrs-v;
	root->lc=create(inl,index-1);
	root->v=v[index];
	root->rc=create(index+1,inr);
	return root;
}
void levelorder(node*root) {
	queue<node*>q;
	q.push(root);
	while(!q.empty()) {
		node*t=q.front();
		q.pop();
		if(num!=0) printf(" ");
		printf("%d",t->v);
		num++;
		if(t->lc!=NULL) q.push(t->lc);
		if(t->rc!=NULL) q.push(t->rc);
	}
}
int main() {
	scanf("%d",&n);
	for(int i=0; i<n; i++) scanf("%d",&v[i]);
	node*root=create(0,n-1);
	levelorder(root);
	return 0;
}
```



