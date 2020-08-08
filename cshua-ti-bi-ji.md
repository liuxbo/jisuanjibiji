---
title: 'c++(PAT)刷题笔记汇总'
date: 2020-05-06 21:20:37
tags: [c++]
published: true
hideInList: false
feature: 
isTop: true
---




### 1. c++字符串数组的\0问题

```
#include<stdio.h>
int main() {
	char str1[8] = { 'd','e','d','g','o' };
	char str2[5] = { 'd','e','d','g','o' };
	printf("%d\n", sizeof(str1));
	printf("%d\n", sizeof(str2));
	return 0;
}
```

![](C:\Users\lxb\Pictures\Camera Roll\KG1V`[}F4D7YE5L_NIJ289O.png)

对于字符串数组，当定义时直接对其赋值，无论元素是否占满数组，其储存的字符个数都是数组的元素个数，未占满时会自动用\0补全

```
#include<stdio.h>
#include<string.h>
int main() {
	char str[14];
	for (int i = 0; i < 5; i++) {
		str[i] = getchar();
	}
	puts(str);
	return 0;
}
```

输出会乱码

![image-20191205103056228](C:\Users\lxb\AppData\Roaming\Typora\typora-user-images\image-20191205103056228.png)

使用`getchar`要在输入的每个字符串后面加\0,例如

```
#include<stdio.h>
#include<string.h>
int main() {
	char str[14];
	for (int i = 0; i < 5; i++) {
		str[i] = getchar();
	}
	str[5] = '\0';
	puts(str);
	return 0;
}
```



### 2.数组相关问题

* 反转一个一维数组

  ```
  #include <cstdio>
  int main() {
  	int a[5] = { 5,2,3,6,9 };
  	for (int i = 0, j = 4; i <= j; i++, j--) {
  		int temp = a[i];
  		a[i] = a[j];
  		a[j] = temp;
  	}
  	for(int i=0;i<5;i++)
  	printf("%d", a[i]);
  	return 0;
  }
  ```

* 找二维(或三维)数组中的“块”(比如相邻的若干个数组元素1)

  用BFS，要设置增量数组，

  对于一维的情况，可以设置

  ```
  int X[4]={0,0,1,-1};
  int Y[4]={1,-1,0,0};
  ```

  以便于访问上下左右四个位置

  对于三维的情况，可以设置

  ```
  int X[6]={0,0,0,0,1,-1};
  int Y[6]={0,0,1,-1,0,0};
  int Z[6]={1,-1,0,0,0,0};
  ```

  对应前后左右上下6个位置


### 3.字符串问题

* `scanf ("%s")`识别空格作为字符串结尾

  `      getchar`  `putchar` 用来输入输出单个字符

* `cin` 读入字符串时，以空格为分隔符，如果想读入一整行字符串，用`getline(cin,s)`，注意前面如果输入数字n，一定要注意`scanf("%d\n",&n);` 这样写，防止getline读入错误

* 判断两个字符串是否一样时可以用`strcmp()==0`，该函数其中一个参数可以为字符数组的名称

* 使⽤ `stoi()` 、 `stod()`  可以将字符串 string 转化为对应的 int 型、 double 型变量

  ```
  #include <iostream>
  #include <string>
  using namespace std;
  int main() {
   string str = "123";
   int a = stoi(str);
   cout << a;//输出123
   str = "123.44";
   double b = stod(str);
   cout << b;//输出123.44
   return 0;
  }
  ```

  `stoi`如果遇到的是⾮法输⼊（⽐如`stoi("123.4")`，123.4不是⼀个int型变量）： 1.会⾃动截取最前⾯的     数字，直到遇到不是数字为⽌ (所以说如果是浮点型，会截取前⾯的整数部分) 2.如果最前⾯不是数字，会运⾏时发⽣错误 

  `stod`如果是⾮法输⼊： 1.会⾃动截取最前⾯的浮点数，直到遇到不满⾜浮点数为⽌ 2.如果最前⾯不是数字或者⼩数点，会运⾏时发⽣错误 3.如果最前⾯是⼩数点，会⾃动转化后在前⾯补0 

  不仅有`stoi`、`stod`两种，相应的还有：

   `stof (string to float)`

   `stold (string to long double) `

  `stol (string to long)` 

  `stoll (string to long long)`

   `stoul (string to unsigned long) `

  `stoull (string to unsigned long long)`

* `to_string()`将数值转化为字符串。返回对应的字符串。头文件同样为`include<string>`

* `printf("%s\n", (s1 + s2).c_str());`  如果想⽤`printf`输出string，需要加⼀ 个`.c_str()`

* 字符串形式的两个数字相加

  ```
  string add(string s1, string s2) {
  	string s = s1;
  	int carry = 0;
  	for (int i = s1.size() - 1; i >= 0; i--) {
  		s[i] = ((s1[i] - '0') + (s2[i] - '0') + carry) % 10 + '0';
  		carry = ((s1[i] - '0') + (s2[i] - '0') + carry) / 10;
  	}
  	if (carry > 0) s = '1' + s;
  	return s;
  }
  ```

  




* 涉及到的题目:

  A1035，A1077,A1082,A1093,A1061，**A1073**(科学计数法)，A1077(公共后缀)，**A1082**,
  
  A1112, A1152, A1140  A1136(回文串、字符串) 、 A1132(20 水题)

###  4.中序序列和 后序或先序或层序 搭配， 确定一棵二叉树

* 先由后序序列或先序序列确定根节点

* 利用在先序或后序序列找到的根节点的值，确定中序序列中根节点的位置

* 确定左子树、右子树的 先序/后序  和中序序列（左子树所有节点的个数只能在中序序列中得知）

* 分别向左孩子和右孩子递归构建

  注意递归时区间的书写格式

###  5. 映射问题(hash 、map、散列...)

* 直接开数组，将数组下标与下标对应的内容作为映射,ASCII码表有128个字符，对于一些题目可以直接开`hashtable[128]`数组，关于其下标，可以用以下函数获得

  ```
  int change(char c){
      if(c>='0'&&c<='9') return c-'0';
      if(c>='a'&&c<='z') return c-'a'+10;
      if(c>='A'&&c<='Z') return c-'A'+36;
  }
  ```

  

* 字符串hash

  * ~~~
    for(int i = 0; i < 3; i++)  id = 26 * id + (name[i] - 'A'); //大写字母字符串映射为整数
    ~~~

* 涉及到的题目

  A1084,A1092,A1041,A1050，A1048  **A1129(25 set的应用 结构体内运算符重载)**
  
  A1145(25 hash 平方探查)

### 6. `STL`容器

* vector

  定义一个m行n列的数组 `vector<vector<int> > b(m, vector<int>(n));`

  ```
  vector<int> vec1(4,1);              //vec1的内容为1,1,1,1
  vector<int> vec1{ 1, 2, 3, 4, 5, 6 };       //vec1内容1,2，3,4,5,6
  ```

  ```
  vector<int> vec(&arr[1], &arr[4]); //将arr[1]~arr[4]范围内的元素作为vec的初始值
  ```

  ```
  vector<int> vec(arr, arr + 5);   //将arr数组的元素用于初始化vec向量
  //说明：当然不包括arr[4]元素，末尾指针都是指结束元素的下一个元素，
  //这个主要是为了和vec.end()指针统一。
  ```

* priority_queue

   `priority_queue<int,vector < int > ,greater< int> >`表示数字越小的优先级越大，优先放在队首

  `priority_queue<int,vector < int > ,less< int> >`表示数字越大的优先级越大，优先放在队首，

  该容器只能通过top()访问队首元素，push()进队，pop()出队，empty()检空

* String

   `string &insert(int p0, int n, char c);`//在p0处插入n个字符c

   `string &insert(int p0,const string &s);`//在p0位置插入字符串s

   `void insert(iterator it, const_iterator first, const_iterator last);`  //在it处插入从first开始至last-1的所有字符

* 异同

  只有`vector`和`string`支持`*(it+i)`的访问方式 ,`set`只能通过迭代器访问(即*it)，

  `map` 、`string`和`vector`支持下标访问和迭代器访问。

  map与set内部都会自动递增排序，(这里指map的key），并且set的元素值与map的key在其各自内部都是唯一的，包括数字和字

* set、unordered_map

   `s.find(value) `,返回集合s中值为value的迭代器

* map 、unordered_map

  使用count，返回的是被查找元素的个数。如果有，返回1；否则，返回0。注意，map中不存在相同元素，所以返回值只能是1或0。count(key值)

  遍历map中所有的元素`for (auto it : mp) `,mp为map容器变量名，用`it.first`和`it.second`访问每个key和相应的value

  find() erase() size() clear()

* 注意

  begin()函数返回一个迭代器,指向字符串的第一个元素.

  end()函数返回一个迭代器，指向字符串的末尾(最后一个字符的下一个位置).

  rbegin()返回一个逆向迭代器，指向字符串的最后一个字符。

  rend()函数返回一个逆向迭代器，指向字符串的开头（第一个字符的前一个位置）。
  
  vector数组可以直接用==判断两个数组是否相同
  
  string类型可以直接用<  或 > 或== 直接进行字典序的比较，而char数组必须用strcmp比较
  
  vector使用下标访问前注意先resize一下
  
  set的内部排序可以重载运算符(以存放结构体变量为例)
  
  ```
  struct node{
  	int id,fre;
  };
  bool operator < (const node &a,const node &b){
  	return a.fre!=b.fre? a.fre>b.fre : a.id<b.id;
  }
  //set存放的node型变量就会定义的方式排序
  ```
  
  
  
  可以考虑 ⽤ unordered_map （或者 unordered_set ）缩短代码运⾏时间、提⾼代码效率
  
  

A1039,A1047,A1063,A1060,A1100,A1054，A1071,A1022 A1120 ,A1149(map、vector)

A1121 (25 map、set应用)   A1137(25 map 排序)    A1139(30 逻辑题 unordered_map)

###  7. 输入输出问题

`scanf`中 ，`long long `型 ：` scanf("%lld",&n)` , `double `型：`scanf("lf",&n)`

`printf`中，`long long` 型：`printf("%lld",n)`   , `double` 型：`printf("%f",n)` 

若`getline(cin, str);`前有`scanf("%d%*c", &n);`输入，可以用%**c忽略一个字符型(比如换行),或者%* *d忽略一个数字型。也可以用getchar()吸收掉字符

`scanf("%*c%d",&n);可以吸收掉上一行末尾的的换行符`

若输入格式为数字`空格` 字符`空格` 数字，可以写成`scanf("%d %c %d")` ,因为空格也算字符。

注意string类型用printf输出时一定要用c_str()转化一下

`sscanf(a, "%lf", &temp);` 从左到右，将**字符数组a**以浮点数的形式存到double类型temp中；

`sprintf(b, "%.2f", temp);` 从右到左，将double类型temp以保留两位小数的形式存到**字符数组b**中



### 8. 提取数中的元素

``` 
while (b != 0) {
		if (b % 10 == db) pb = pb * 10 + db;
		b = b / 10;
	}
提取b中的重复数字组成新的数字 b=38633 ，db为3，新数字为333

//计算某个数所有位上的数字之和  比如数字123的和为6
int getsum(int n){
	int sum=0;
	while(n!=0){
		sum+=n%10;
		n/=10;
	}
	return sum;
}
```



### 9. 常用函数

* `math.h`头文件函数

  `fabs(double x)`  取绝对值 ，

  `floor(double x)`和`ceil(double x)` ，分别为向下取整和向上取整，返回double型

  `pow(double r,double p)` 用于返回r^p

  `sqrt(double x)` ,返回算数平方根

  `log(double x)` ,返回以自然对数为底的对数，用换底公式求具体对数

  `sin(double x)` `cos(double x)` `tan(double x)` 

  `asin(double x)` `acos(double x)` `atan(double x)` 

  `round(double x)` 将x四舍五入，返回为double型

* `algorithm`头文件下的函数，加`using namespace std;`

  `max(x,y)` `min(x,y)`  

  `abs(x)` 返回x的绝对值，x必须为整数

  `swap(x,y)` 

  `reverse(it,it2)` `reverse(a,a+4)`  ,将数组元素反转

  `next_permutation(a,a+...)` ,给出一个序列在全排列中的下一个序列，该函数在到达全排列的最后一个时会返回false

  `fill(a,a+4,233)` ,赋相同值，对于`G[maxn] [maxn]`二维数组`fill(G[0],G[0]+maxn*maxn,INF);`

  `sort(首元素地址，尾元素地址的下一个地址，比较函数（非必填)）`,无比较函数，默认递增排序。

  `sort(a, a+n, greater());`  从大到小排序(要用`iostream`头文件)

  `lower_bound(first,last,val)`  `upper_bound(first,last,val)` ,用于有序数组或容器，前者用来寻找[first,last)范围内第一个值大于等于val的元素的位置，后者寻找第一个值大于val的元素位置.若是数组，这两个函数返回的是地址，如`int* right=upper_bound(num, num+n, k)`，返回num数组中第一个大于k的元素所在地址，其下标=`right-num`

  `max_element(a, a+len)`返回序列中最大元素地址（迭代器）,可用其减去数组首地址(即数组名)获取其下标,若要获取该元素值，**直接在前面加一个*,表示取地址即可**

  `min_element(begin, end)` 返回序列中最小元素地址

* `string.h` 头文件(`cstring`头文件)

  `memset(数组名,-1或0,sizeof(数组名))`

  `strlen(字符数组)` 得到字符数组中第一个\0前的字符个数

  `strcmp(字符数组1，字符数组2) `返回两个字符串大小比较结果，按字典序。字符数组1<2 ,返回负整数;1=2，返回0;1>2，返回正整数

  `strcpy(字符数组1，字符数组2)`，把字符数组2复制给字符数组1，包括\0

  `strcat(字符数组1，字符数组2)` ,把2接在1后面

* `cctype`头文件

  1.不仅仅能判断字⺟，还能判断数字、⼩写字⺟、⼤写字⺟等

  `isalpha `字⺟（包括⼤写、⼩写）

  ` islower` （⼩写字⺟）

  ` isupper `（⼤写字⺟）

   `isalnum` （字⺟⼤写⼩写+数字）

  ` isblank `（space和 \t ）

  ` isspace `（ space 、 \t 、 \r 、 \n ）

  `isdigit`(数字)

  2 .`tolower (char c)`和 `toupper(char c)` 将某个字符转为⼩写或⼤写

* `string`

  `str.insert(pos,str1)` 在原字符串的pos号位插入一个新的字符串str1

  `str.erase(first,last)` first为需要删除的区间的起始迭代器，last是区间末尾迭代器的下一个地址

  `str.erase(pos,length)` pos为需要开始删除的起始位置，length是删除的字符个数

  `str.clear()` 清空string中的数据

  `str.substr(pos,len)` 返回从pos号开始，长度是len 的子串

  `string::npos`是一个常数，可以作为find函数匹配失败的返回值

  `str.find(str1,pos)`  从str的pos号位开始匹配str1，返回str1第一次出现的位置

  `str.find(str1)`  从str的pos号位开始匹配str1，返回str1第一次出现的位置

  



### 10.进制转换
* 十进制转化为其他进制

```
1.数组形式
int z[maxn],num=0;
do{
z[num++]=n%b;
n/=b;
}while(n!=0);
2.数字形式(将十进制数n转化为任意b进制数，将结果存在数组中)
vector change(int n,int b){
	vector<int> v;
	int ans=0;
	while(n!=0){
		v.push_back(n%b);
		n/=b;
	}//注意在数组中存储的顺序(下标从0开始)是反的
	return ans;
}
3.
void change(int n,int b){
	if(n>0){
		int i=0;
		change(n/b,b);
		a.push_back(n%b);
	}
	else return;
}
```

* 其他进制转化为十进制

  ```
  for(int i=0;i<v.size();i++) ans+=v[i]*pow(b,i);
  ```

  

* 给定⼀个数值和⼀个进制，将它转化为10进制。转化过程中可能产⽣溢出,数值中只有小写字母和数字

  ```
  long long convert(string n, long long radix) {
   long long sum = 0;
   int index = 0, temp = 0;
   for (auto it = n.rbegin(); it != n.rend(); it++) {
     temp = isdigit(*it) ? *it - '0' : *it - 'a' + 10;
     sum += temp * pow(radix, index++);
   }
   return sum;
  }
  ```

  

### 11. `ASCLL` 码表

A-Z对应十进制65-90

a-z对应十进制97-122



### 12.回文串

```
bool judge(int arr[],int index){//index为数组元素个数
    for(int i = 0; i <index / 2; i++) {
       if(arr[i] != arr[index-i-1]) {
           return false;
     }
   }
   return true;
}

```



### 13.数字、字符数组相互转换

```
1.数字存到数组中
void to_array(int n,int a[]){
    int j=0;
    while(n){
        a[j++]=n%10;
        n/=10;
    }
}
2.数组变为数字
int exp=0;
for(int i=0;i<len;i++){
    exp=exp*10+(str[i]-'0');
}
或

```



### 14 排序、找最值

* ```
  int best=0;
  for (int j = 0; j < 4; j++) {
  	if (a[j] < a[best])
  	best = j;
  }
  找出数组a中最小元素的位置
  ```

* 结构体内重载运算符

  运算符重载的格式如下：

  函数类型 operator 运算符名称（形参表列）{对运算符的重载处理},例如：
  
  ```
  struct node {
  	int id;
  	int freq;
  	bool operator < (const node& a) const {
  		return (freq != a.freq) ? freq > a.freq:id < a.id;
  	}
  };
  ```
  
  
  
* 使用sort()对char数组排序

  正确方法：

  ```
  #include<cstdio>
  #include<cstring>
  #include<algorithm>
  using namespace std;
  char name[3][4] = {"wu","jia","jun"};//二维数组保存n个字符串 
  bool cmp(int a,int b){
      return strcmp(name[a],name[b]) < 0;
  }
  int main(){
      int arr[3] = {0,1,2};//与字符串数组下标一一对应 
      sort(arr,arr+3,cmp);//排列的实际是标号 ，这么做更快 
      for(int i = 0;i <3 ;i++){
          printf("%s ",name[arr[i]]);
      }
  }
  ```

  错误方法：

  ```
  #include<cstdio>
  #include<algorithm>
  #include<cstring>
  using namespace std;
  char stu[6][6] = { "ahda","jwnd","djsh","yrhw","JWJs" };
  bool cmp(int a, int  b) { return strcmp(stu[a], stu[b]) < 0; }
  int main() {
  	sort(stu, stu + 5, cmp);
  	for (int i = 0; i < 5; i++)
  		printf("%s", stu[i]);
  	return 0;
  }
  ```

* ```
   testee[0].rank=1;
    	for(int k=1;k<total;k++){
    		if(testee[k].grade==testee[k-1].grade) testee[k].rank=testee[k-1].rank;
    		else testee[k].rank=k+1;
    	}
  确保排名为 1 1 3 3 4...的形式
  ```

* A1062 ,A1075,A1012,A1016,A1025,A1028,A1055,A1075,A1083,A1080,A1095,A1109,A1141(注意1080中cmp的写法和使用)

  思路：

  定义结构体，cmp函数，初始化结构体内的变量，for循环中边输入边处理

  排序时注意是否能用学号等信息作为数组下标，因为排序后下标会改变，所以可以在结构体中设置id变量

  平均数作为最后成绩可以不用除，直接用总和比较

  有些题要找出有效记录(比如常出现的配对问题)

* 各种排序算法

   * 选择排序
   
     ```
     void selectsort(){
         for(int i=0;i<n;i++){
             int k=i;
             for(int j=k;j<n;j++){
                 if(A[j]<A[k]){
                     k=j;
                 }
             }
             int temp=A[K];
             A[k]=A[i];
             A[i]=temp;
         }
     }
     ```
   
     
   
   * 插入排序
   
     ```
     void insertSort(){
        for(int i=2;i<=n;i++){
           int temp=a[i],j=i;
           while(j>1&&a[j-1]>temp){
              a[j]=a[j-1];
              j--;
           }
           a[j]=temp;
        }
     }
     ```
   
   * 归并排序 (此处为2路归并) (合并两个有序序列时，注意其中一个数列先扫描完的情况)
   
     ```
     void merge(int A[],int L1,int R1,int L2,int R2){
     //将数组A的[L1,R1]与[L2,R2]区间合并为有序区间(此处L2=R1+1)
         int i=L1,j=L2,temp[maxn],index=0;
         while(i<=R1&&j<=R2){
         if(A[i]<=A[j]) temp[index++]=A[i++];
         else temp[index++]=A[j++];
         }
         while(i<=R1) temp[index++]=A[i++];
         while(j<=R2) temp[index++]=A[j++];
         for(i=0;i<index;i++) A[L1+i]=temp[i];
     }
     void mergeSort(int A[],int left,int right){
         if(left<right){
             int mid=(left+right)/2;
             mergeSort(A,left,mid);
             mergeSort(A,mid+1,right);
             merge(A,left,mid,mid+1,right);
         }
     }
     ```
   
   * 快排(递归法)
   
     ```
     int partition(int A[],int left,int right){
         int temp=A[left];
         while(left<right){
             while(right>left&&A[right]>temp) right--;
             A[left]=A[right];
             while(left<right&&A[left]<=temp) left++;
             A[right]=A[left];
         }
         A[left]=temp;
         return left;
     }
     void quicksort(int A[],int left,int right){
         if(left<right){
             int pos=partition(A,left,right);
             quicksort(A,left,pos-1);
             quciksort(A,pos+1,right);
         }
     }
     ```
   
     快排(改进后)
   
     生成随机数需要添加stdlib.h与time.h头文件，mian函数内开头加上srand((unsigned)time(NULL));
   
     ```
     int partition(int A[],int left,int right){//只改进该函数
         int p=(int)(round(1.0*rand()/RAND_MAX*(right-left)+left));
         //生成[left,right]范围内的随机数
         swap(A[left],A[p]);
         int temp=A[left];
         while(left<right){
             while(right>left&&A[right]>temp) right--;
             A[left]=A[right];
             while(left<right&&A[left]<=temp) left++;
             A[right]=A[left];
         }
         A[left]=temp;
         return left;
     }
     void quicksort(int A[],int left,int right){//该函数不变
         if(left<right){
             int pos=partition(A,left,right);
             quicksort(A,left,pos-1);
             quciksort(A,pos+1,right);
         }
     }
     ```
   
   * 堆排序
   
     ```
     void downadjust(int low, int high) {
     	int i = low, j = 2 * i;
     	while (j <= high) {
     		if (j + 1 <= high && heap[j + 1] > heap[j]) {
     			j = j + 1;
     		}
     		if (heap[i] < heap[j]) {
     			swap(heap[i], heap[j]);
     			i = j;
     			j = 2 * i;
     		}
     		else {
     			break;
     		}
     	}
     }
     void upadjust(int low, int high) {
     	int i = high, j = i / 2;
     	while (j >= low) {
     		if (heap[i] < heap[j]) {
     			swap(heap[i], heap[j]);
     			i = j;
     			j = i / 2;
     		}
     		else {
     			break;
     		}
     	}
     }
     void createheap() {//建堆
     	for (int i = n / 2; i >= 1; i--) {
     		downadjust(i, n);
     	}
     }
     void heapsort(){
         createheap();
         for(int i=n;i>1;i--){
             swap(heap[1],heap[i]);
             upadjust(1,i-1);
         }
     }
     ```
   
   * 拓扑排序
   
     ```
     vector<int> G[maxn]; //邻接表
     int n,m,indegree[maxn]; //顶点数，入度 
   bool topologicalSort(){
     	int num=0;
     	queue<int> q;
     	for(int i=0;i<n;i++){
     		if(indegree[i])==0 q.push(i);
     	}
     	while(!q.empty()){
     		int u=q.front();//取队首结点
     		q.pop();
     		for(int i=0;i<G[u].size();i++){
     			int v=G[u][i];//u的后继结点
     			indegree[v]--;
     			if(indegree[v]==0) q.push(v); 
     		} 
     		G[u].clear();//清空u的所有出边，如无必要可不写
     		num++; 
     	}
     	if(num==n) return true;//加入拓扑排序的顶点数为n，说明拓扑排序成功
     	else return false; //顶点数小于n，失败 
     }
     ```
     
     
   
   

### 15 计算时长

* 对已知的起止时间，不断将起始时间加1，判断是否到达终止时间

  ```
  void get_ans(int on, int off, int& time, int& money) {
  	temp = rec[on];
	while (temp.dd < rec[off].dd || temp.hh < rec[off].hh || temp.mm < rec[off].mm) {
  		time++;
  		money += toll[temp.hh];
  		temp.mm++;
  		if (temp.mm >= 60) {
  			temp.mm = 0;
  			temp.hh++;
  		}
  		if (temp.hh >= 24) {
  			temp.hh = 0;
  			temp.dd++;
  		}
  	}
  }
  ```
  
* 对于`hh:mm:ss`类型的时间处理：可以`hh * 3600 + mm * 60 + ss` 转化成秒，方便处理




### 16.数据类型问题

* 且C++把所有⾮零值解释为 true ，零值解 释为 false ～所以直接赋值⼀个数字给 `bool` 变量也是可以的，它会⾃动根据 int 值是不是零来决定 给 `bool` 变量赋值 true 还是 false ～

```
bool flag = true;//flag为1，若是flase则为0
bool flag2 = -2; // flag2为true
bool flag3 = 0; // flag3为false
```





### 17.栈、队列、链表

在使用栈的pop()和top()函数前必须使用empty()判断栈是否非空。

栈：A1051

记得清空栈

```
while(!st.empty()){
    st.pop();
}
```



队列：A1056

链表：A1074，A1032，A1052, A1097 A1133(链表  重新排列)

```
for(int i=0;i<n;i++){
		scanf("%d",&address);
		scanf("%d%d",&node[address].key,&node[address].next);
		node[address].address=address;
	}
```



当结点的地址是比较小的整数(比如5位数的整数),可以使用静态链表，没有必要建立动态链表。

注意初始化，排除无效结点(可以结合sort，cmp中针对order)。用count计数有效结点

某些情况下的最后一个结点要特殊处理(比如令next为-1)。

注意链表结构体元素order的使用

### 18.贪心

A1070 ,**A1033** ,A1037,A1067 A1125(简单贪心 排序)

### 19 二分

A1085,A1010,A1044 A1048

注意二分法的几种写法,以及非严格递增序列的处理,可以使用`lower_bound`和`upper_bouned`

* 查找给定的数x

  ```
  int binarySearch(int A[],int left,int right,int x){
      int mid;
      while(left<=right){
          mid=(left+right)/2;
          if(A[mid]==x) return mid;
          else if(A[mid]>x) right=mid-1;
          else left=mid+1;
      }
      return -1;//查找失败，返回-1
  }
  ```

  

* 求第一个大于等于x的元素的位置

  ```
  int lower_bound(int A[],int left,int right,int x){
      int mid;
      while(left<right){
          mid=(left+right)/2;
          if(A[mid]>=x) right=mid;
          else left=mid+1;
      }
      return left;
  }
  ```

  

* 求第一个大于x的元素的位置

  ```
  int upper_bound(int A[],int left,int right,int x){
      int mid;
      while(left<right){
          mid=(left+right)/2;
          if(A[mid]>x) right=mid;
          else left=mid+1;
      }
      return left;
  }
  ```

  

### 20 two points

A1085,A1089,A1029



### 21 数学问题
* 求最大公约数,(最小公倍数：a和b的最大公约数为d，则a和b的最小公倍数为ab/d)

  ```
  long long gcd(long long a, long long b) {
       return b == 0 ? abs(a) : gcd(b, a %b);
   }
  ```

* 分数化简

  ```
  struct node{
  	long long up, down;
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
  ```

* 分数的四则运算

  ```
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
  ```

* 分数输出

  ```
  void showres(node res){
  	res=reduction(res);
  	if(res.down==1) printf("%lld\n",res.up);
  	else if(abs(res.up)>abs(res.down)){
  		printf("%lld %lld/%lld\n",res.up/res.down,abs(res.up%res.down),res.down);
  	}else printf("%lld/%lld\n",res.up,res.down);
  }
  ```
  

  
* 判断是否为素数

  ```
  bool isprime(int n) {
  	if (n <= 1) return false;
      int sqr = sqrt(1.0 * n);
  	for (int i = 2; i <= sqr; i++) {
  		if (n % i == 0) return false;
  	}
  	return true;
  }
  
  ```

* 建立素数表

  ```
  
  int prime[maxn], pnum=0;
  void find_prime() {
  	for (int i = 1; i < maxn; i++) {
  		if (isprime(i)) {
  			prime[pnum++] = i;
  		}
  	}
  }
  ```

  

* 大整数运算

  * 以字符串方式存储、运算
  
    ```
    string add(string s1, string s2) {//加法运算，十进制，两个数位数相同
    	string s = s1;
    	int carry = 0;
    	for (int i = s1.size() - 1; i >= 0; i--) {
    		s[i] = ((s1[i] - '0') + (s2[i] - '0') + carry) % 10 + '0';
    		carry = ((s1[i] - '0') + (s2[i] - '0') + carry) / 10;
    	}
    	if (carry > 0) s = '1' + s;
    	return s;
    }
    ```
  
  
  
  * 以结构体方式存储运算
  
    ```
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
  		a.d[i] = str[a.len - 1 - i]-'0';//整数的高位会变成数组的低位
    	}
    	return a;
    }
    
    bign add(bign a, bign b) {//加法
    	bign c;
    	int carry=0;
    	for (int i = 0; i < a.len||i<b.len; i++) {
    		int temp = a.d[i] +b.d[i] + carry;
    		c.d[c.len++] = temp % 10;
    		carry = temp / 10;
    	}
    	if(carry != 0) {
    		c.d[c.len++] = carry;
    	}
    	return c;
    }
    
    bign sub(bign a, bign b) {//加法
    	bign c;
    	for (int i = 0; i < a.len||i<b.len; i++) {
    	    if(a.d[i]<b.d[i]){//如果不够减
    	       a.d[i+1]--;//向高位结尾
    	       a.d[i]+=10;//当前位加10
    	    }
    	    c.d[c.len++]=a.d[i]-b.d[i];
    	}
    	while(c.len-1>=1&&c.d[c.len-1]==0) c.len--;
    	//去除高位的0，同时至少保留一位最低位
    	return c;
    }
    
    bign multi(bign a, int b) {//乘法
    	bign c;
    	int carry=0;
    	for (int i = 0; i < a.len; i++) {
    		int temp = a.d[i] * b + carry;
    		c.d[c.len++] = temp % 10;
    		carry = temp / 10;
    	}
    	while (carry != 0) {//乘法的进位可能不止一位
    		c.d[c.len++] = carry % 10;
    		carry /= 10;
    	}
    	return c;
    }
    
    bign divide(bign a,int b,int& r){//r为余数
        bign c;
        c.len=a.len;
        for(int i=a.len-1;i>=0;i--){//从高位开始
           r=r*10+a.d[i];//和上一位遗留的余数组合
           if(r<b) c.d[i]=0;//不够除,改位为0
           else{
              c.d[i]=r/b;//商
              r=r%b; //获得新的余数
           }
        }
        while(c.len-1>=1&&c.d[c.len-1]==0)  c.len--;//去除高位的0，同时至少保留一位最低位
        return c;
    }
    ```
  
    
  
  



A1069,A1049,A1088,A1015,A1078,A1096,A1023,A1024


### 22.入门模拟

A1042(洗扑克牌) ，A1046(环形两点间距离) ，A1065(两数相加判断大小)， A1002(多项式相加)，A1009(多项式相乘)，A1011，A1006(签到签离) ，A1036(找最值)，A1031(输出图形)，A1019(回文串)，A1027(进制转换)，A1058(加法进位)，A1061(字符串问题)，***A1073(科学计数法)***，A1001(a+b)，A1005,A1035,A1077(公共后缀)，**A1082(用汉语读数字)**

### 23.技巧、逻辑

A1093,A1101, A1113 A1117(逻辑)  A1148(狼人杀 找到两个狼人)

### 24 DFS BFS

* DFS用递归实现，A1103

  ```
  void DFS(int index, int nowk, int sum, int facsum) {
  	if (sum == n && nowk == k) {
  		if (facsum > maxfacsum) {
  			ans = temp;//更新最优序列
  			maxfacsum = facsum;
  		}
  		return;
  	}
  	if (nowk > k || sum > n) return;
  	if (index - 1 >= 0) {
  		temp.push_back(index);
  		DFS(index, nowk + 1, sum + fac[index], facsum + index);
  		temp.pop_back();
  		DFS(index - 1, nowk, sum, facsum);
  	}
  }
  ```

  

* BFS用队列实现    A1091

  ```
  void BFS(int s){
      queue<int> q;
      q.push(s);
      while(!q.empty()){
          取出队首元素top;
          访问队首元素top;
          将队首元素出队;
          将top的下一层结点中未曾入队的结点全部入队，并设置为已入队;
      }
  }
  ```

  ```
  int BFS(int z,int x,int y){
  	int total=0;
  	Node.x = x;
  	Node.y = y;
  	Node.z = z;
  	inq[z][x][y] = true;
  	queue<node> q;
  	q.push(Node);
  	while (!q.empty()) {
  		node topp = q.front();
  		q.pop();
  		total++;
  		int newx, newy, newz;
  		for (int i = 0; i < 6; i++) {
  			newz = topp.z + Z[i];
  			newx = topp.x + X[i];
  			newy = topp.y + Y[i];
  			if (judge(newz, newx, newy)) {
  				Node.x = newx;
  				Node.y = newy;
  				Node.z = newz;
  				q.push(Node);
  				inq[Node.z][Node.x][Node.y] = true;
  			}
  		}
  	}
  	if (total >= T) return total;
  	else return 0;
  }
  ```

  

### 25 树

A1020,A1086,A1102 , A1151(二叉树LCA)，A1143(二叉搜索树LCA) 、A1119(前序和后序求中序)

A1123(30 AVL 、层序遍历、判断是否是完全二叉树) 、 A1110(判断是否为完全二叉树)

A1127(30 中序后序建树，dfs，输出z字形层序遍历)   A1130(dfs二叉树 输出中缀表达式)

A1155(30 完全二叉树  判断大顶堆小顶堆 dfs 打印路径)    A1147(30  判断大顶堆小顶堆 后序遍历)

A1135(30 判断红黑树 递归判断)

* 二叉树

  

  * 存储结构

    ```
    struct node{
        typename data;
        node* lchild;
        node* rchild;
    }//动态
    struct node{
      typename data;
        int left,right;
  }//静态
    
    //对于不是二叉树的树
    struct node{
        typename data;
        vector<int> child;
    }
    ```
  
* 新建结点
  
  ```
    node* newNode(int v){
        node* root=new node;
        Node->data=v;
        Node->lchild=Node->rchild=NULL;
        return Node;
    }
  ```
  
* 中序遍历

  ```
    void inorder(node* root){
        if(root==NULL) return;
        inorder(root->lchild);
        printf("%d",root->data);
        inorder(root->rchild);
    }
  ```

  * 二叉树的层序遍历(BFS)

    ```
    1.void layerorder(node* root) {
    	queue<node*> q;
        q.push(root);
    	while (!q.empty()) {
    	node* now = q.front();
    		q.pop();
    		printf("%d", now->data);
    		if (now->lchild != NULL) q.push(now->lchild);
    	if (now->rchild != NULL) q.push(now->rchild);
    	}
    }
    
    2.//层序遍历用当前队列的元素个数统计每层节点
    vector<int> level[31];
    struct node{
    	int v;
    	node* left,*right;
    };
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
    
    3.//层序遍历用结构体统计每层结点
    vector<int> level[35];
    struct node{
    	int v,depth;
    	node* left,*right;
    };
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
    ```

  * 用已知的两个序列构建唯一二叉树

    ```
    node* create(int postL, int postR, int inL, int inR) {//后序加中序
    	if (postL > postR) return NULL;
    	node* root = new node;
    	root->data = postorder[postR];
    	int k;
    	for (int i = inL; i <= inR; i++) {
    		if (inorder[i] == root->data) {
    			k = i;
    		break;
    		}
    	}
    	int leafnum = k - inL;
    	root->lchild = create(postL, postL + leafnum - 1, inL, k - 1);
    	root->rchild = create(postL + leafnum, postR - 1, k + 1, inR);
    	return root;
    }
    
    ```

    

    层序加中序建树

    ```
    typedef struct node{				//结点定义 
    	int data;
    	struct node *right,*left;
    }Node;
    vector<int> inor,layor;				//中序和层序序列 
    unordered_map<int,int> inmp,laymp;	//序列与其下标的映射，（建立映射可以减少递归时候查找序列的次数） 
    Node *creat(int il,int ir){			//建树函数 ，il表示中序序列的左端，ir便是中序序列的右端 
    	if(il>ir) return NULL;			//如果树中序列长度为0，表示到了空结点 
    	int min=INF,id;					//min表示中序序列中的元素在层序中下标最小的那个，其中序的下标 
    	for(int i=il;i<=ir;i++){		//遍历中序序列，找出对应层序中下边最小的元素 
    		if(laymp[inor[i]]<min){
    			min=laymp[inor[i]];
    		}
    	}
    	Node *root=new node;			
    	root->data=layor[min];			//中序序列中 层序中下标最小的便是根 
    	id=inmp[root->data];			//获得根在中序的位置，划分为左子树和右子树 
    	root->left=creat(il,id-1);		//递归左子树 
    	root->right=creat(id+1,ir);		//递归右子树 
    	return root;					//返回当前子树的根 
    }
    void preorder(Node *root){			//递归的先序遍历函数 
    	if(root==NULL) return;
    	printf("%d ",root->data);
    	preorder(root->left);
    	preorder(root->right);
    }
    int main(void){
    
    	int n;
    	cin>>n;
    	inor.resize(n); layor.resize(n);//读入元素个数并初始化 
    	for(int i=0;i<n;i++){			//读入中序序列并完成映射 
    		cin>>inor[i];
    		inmp[inor[i]]=i;
    } 
    	for(int i=0;i<n;i++){			//读入层序序列并完成映射 
    	cin>>layor[i];
    		laymp[layor[i]]=i;
    	} 
    	Node *root=creat(0,n-1);		//建树 
    	preorder(root);					//先序遍历，验证树的结构 
    	return 0;
    } 
    ```

    

    前序加后序建树

    ```
    node* create(int prel,int prer,int postl,int postr){//1119
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
    ```

    

* 静态二叉树(如果题目中直接给出结点序号之间的父子关系，可以用静态)
  
    ```
  struct node{
        typename data;
        int lchild;
        int rchild;
    }Node[maxn];
  ```
  
  
  
* 二叉树反转(静态)
  
  ```
    void postorder(int root) {//注意是后序遍历
  	if (root == -1) return;
    	postorder(Node[root].lchild);
  	postorder(Node[root].rchild);
    	swap(Node[root].lchild, Node[root].rchild);
    }
  ```
  
  * 完全二叉树
  
    
  
    1.判断是否是完全二叉树
  
    判断是不是完全⼆叉树，就看在出现了⼀个孩⼦为空的结点之后是否还会出现孩⼦结点不为空的结 点，如果出现了就不是完全⼆叉树。
  
    ```
    int iscomplete = 1, after = 0;
    vector<int> levelorder(node* root) {//层序遍历中
    	queue<node*> q;
    	vector<int> v;
    	q.push(root);
    	while (!q.empty()) {
    		node* temp = q.front();
    		q.pop();
    		v.push_back(temp->data);
    		if (temp->lchild != NULL) {
    			if (after) iscomplete = 0;
    			q.push(temp->lchild);
    		}
    	else after = 1;
    		if (temp->rchild != NULL) {
    		if (after) iscomplete = 0;
    			q.push(temp->rchild);
    	}
    		else after = 1;
    }
    	return v;
    }
    ```
  
    
  
    2.给出⼀个n表示有n个结点，这n个结点为0~n-1，给出这n个结点的左右孩⼦，求问这棵树是 不是完全⼆叉树
  
     分析：递归出最⼤的下标值，完全⼆叉树⼀定把前⾯的下标充满： 最⼤的下标值 == 最⼤的节点数； 不完全⼆叉树前满⼀定有位置是空，会往后挤： 最⼤的下标值 > 最⼤的节点数
  
    3.完全二叉搜索树，将待插入数据a[]从小到大排序，利用中序建树，存放到CBT数组中，依次输出CBT数组的元素即为完全二叉搜索树的层序遍历序列
  
    ```
    void inorder(int root){
    	if(root>n) return;
    	inorder(root*2);
    	CBT[root]=a[index++];
    	inorder(root*2+1);
    }
    ```
  
    
  
  * 注意点
  
    * 完全二叉树的存储中，如果根节点下标为1，则该树中任何一个结点i，其左孩子编号为2i，右孩子编号为2i+1，父亲节点为下取整(i/2);如果根节点下标为0，对树中某结点i,父亲结点为下取整((i-1)/2);左孩子为2i+1，右孩子为2i+2
    * 函数参数中，对指针指向的结点内容进行修改是不需要加引用的，。如果函数中需要新建结点，即对二叉树的结构做出修改，就需要加引用(如insert，)，如果是修改当前已有结点的内容，或者是遍历树，就不需要加引用。
    * 无论是先序还是后序，都必须知道中序序列才能唯一确定一棵树
    * 对于题目中给出左右孩子结点编号的情况，没有被当做孩子结点的结点编号即为根结点，可以用一个数组来判断
  
* 普通树

  深度遍历，递归边界为无孩子结点，根节点深度为0

  ```
  void DFS(int index,int depth){
  	if(Node[index].child.size()==0){
  		ans+=Node[index].data*p*pow((1+r/100),depth);
  		return;
  	}
  	for(int i=0;i<Node[index].child.size();i++){
  		DFS(Node[index].child[i],depth+1);
  	}
  }
  ```

  

    广度遍历

  ```
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
    }`
  ```

  

  二叉查找树BST

  * 存储结构

    ```
    struct node{
    	int data;
    	node* lchild;
    	node* rchild;
    };
    ```

    

  * 插入

    ```
    void insert(node*& root, int x) {
    	if (root == NULL) {
    		root = new node;
    		root->data = x;
    		root->lchild = NULL;
    		root->rchild = NULL;
    		return;
    	}
    	if (x < root->data) insert(root->lchild, x);
    	else insert(root->rchild, x);
    }
    ```

    

  * 完全二叉查找树

    用数组存放完全二叉查找树时，可以先将待插入的权值递增排列，然后用中序遍历的方式将其插入到树中，并且要注意结点序号的关系

    ```
    void inorder(int root) {
    	if (root > n) return;
    	inorder(root * 2);
    	CBT[root] = number[index++];
    	inorder(root * 2 + 1);
    }
    ```

    

  * 注意点：对二叉查找树进行中序遍历，其结果是有序的

* 平衡二叉树(AVL)

  A1066

  任意结点左子树与右子树的高度之差的绝对值不超过1

  某结点左子树与右子树的高度之差成为该结点的平衡因子。

  在对AVL进行插入操作时，只要把最靠近插入结点的失衡结点调整到正常，路径上的所有结点就都会平衡。

  * 存储结构

    ```
    struct node {
    	int data, height;
    	node* lchild, * rchild;
    };
    ```
    
    
    
  * 生成一个新结点

    ```
    node* newNode(int v) {
    	node* Node = new node;
    	Node->v = v;
    	Node->height = 1;
    	Node->lchild = Node->rchild = NULL;
    	return Node;
    }
    ```

  * 获取结点root所在子树当前高度

    ```
    int getheight(node* root) {
        if (root == NULL) return 0;
    	return root->height;
    }
    ```

    

  * 更新高度

    ```
    void updateheight(node* root) {
    	root->height = max(getheight(root->lchild), getheight(root->rchild)) + 1;
    }
    ```

    

    

  * 计算平衡因子

    ```
    int getbalancefactor(node* root) {
    	return getheight(root->lchild) - getheight(root->rchild);
    	//注意是左减右
    }
    ```

  * 左旋

    ```
    void L(node*& root) {
    	node* temp = root->rchild;
    	root->rchild = temp->lchild;
    	temp->lchild = root;
    	updateheight(root);
    	updateheight(temp);
    	root = temp;
    }
    ```

  * 右旋

    ```
    void R(node*& root) {
    	node* temp = root->lchild;
    	root->lchild = temp->rchild;
    	temp->rchild = root;
    	updateheight(root);
    	updateheight(temp);
    	root = temp;
    }
    ```
    
    
    
  * 建树

    ```
    void insert(node*& root, int v) {
    	if (root == NULL) {
    		root = newnode(v);
    		return;
    	}
    	if (root->data > v) {
    		insert(root->lchild, v);
    		updateheight(root);
    		if (getbalance(root) == 2) {
    			if (getbalance(root->lchild) == 1) {
    				R(root);
    			}
    			else if (getbalance(root->lchild) == -1) {
    				L(root->lchild);
    				R(root);
    			}
    		}
    	}
    	else {
    		insert(root->rchild, v);
    		updateheight(root);
    		if (getbalance(root) == -2) {
    			if (getbalance(root->rchild) == -1) {
    				L(root);
    			}
    			else if (getbalance(root->rchild) == 1) {
    				R(root->rchild);
    				L(root);
    			}
    		}
    	}
    }
    ```

  

* 堆(完全二叉树(大顶堆、小顶堆))，下面以大顶堆为例

  * 向下调整

    ```
    void downadjust(int low, int high) {
    	int i = low, j = 2 * i;
    	while (j <= high) {
    		if (j + 1 <= high && heap[j + 1] > heap[j]) {
    			j = j + 1;
    		}
    		if (heap[i] < heap[j]) {
    			swap(heap[i], heap[j]);
    			i = j;
    			j = 2 * i;
    		}
    		else {
    			break;
    		}
    	}
    }
    ```

  * 删除堆顶元素

    ```
    void deletetop() {
    	heap[1] = heap[n--];
    	downadjust(1, n);
    }
    ```

  * 向上调整

    ```
    void upadjust(int low, int high) {
    	int i = high, j = i / 2;
    	while (j >= low) {
    		if (heap[i] < heap[j]) {
    			swap(heap[i], heap[j]);
    			i = j;
    			j = i / 2;
    		}
    		else {
    			break;
    		}
    	}
    }
    ```

  * 插入元素

    ```
    void insert(int x) {
    	heap[++n] = x;
    	upadjust(1, n);
    }
    ```

* 红黑树

    定义：红黑树是每个节点都带有颜色属性的平衡二叉查找树 ，颜色为红色或黑色。除了二叉查找树一般要求以外，对于任何有效的红黑树增加了如下的额外要求:

    （1） 节点是要么红色或要么是黑色。

    （2） 根一定是黑色节点。

    （3） 每个叶子结点都带有两个空的黑色结点（称之为NIL节点，它又被称为黑哨兵）。

    （4） 每个红色节点的两个子节点都是黑色（或者说从每个叶子到根的所有路径上不能有两个连续的红色节点）。

    （5） 从任一节点到它所能到达得叶子节点的所有简单路径都包含相同数目的黑色节点

    ```
    void create(node*&root,int v){//创建BST
    	if(root==NULL){
    		root=new node;
    		root->v=v;
    		root->left=root->right=NULL;
    		return;
    	}
    	if(abs(v)<=abs(root->v)) create(root->left,v);
    	else create(root->right,v);
    }
    //负点权代表红，正点权代表黑
    bool judge1(node*root){//判断红结点的孩子结点是否都为黑
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
    bool judge2(node*root){//判断从任意结点到叶⼦结点的路径中，黑色结点的个数是否相同
    	if(root==NULL) return true;
    	int l=getnum(root->left);
    	int r=getnum(root->right);
    	if(l!=r) return false;
    	return judge2(root->left)&&judge2(root->right);
    }
    ```

    

    1135(30 判断红黑树 递归判断)

### 26 并查集

 A1107 A1114 A1118

* 初始化

  ```
  for(int i=0;i<N;i++){//初始化，千万不要忘了
      father[i]=i;
  }
  ```

  

  

* 找根结点

  ```
  int findfather(int x){//普通版找根节点
      while(x!=father[x]){
        x=father[x];
      }
      return x;
  }
  
  int findfather(int x){//路径压缩版找根结点(非递归写法)
      int a=x;
      while(x!=father[x]) x=father[x];
      while(a!=father[a]){
          int z=a;
          a=father[a];//a回溯到其父亲节点
          father[z]=x;//将原先的结点a的父亲节点改为根节点
      }
      return x;
  }
  
  int findfather(int v){//路径压缩版找根结点(递归写法)
  	if(father[v]==v) return v;
  	else{
  		int F=findfather(father[v]);
  		father[v]=F;//把路径上所有结点的父节点都变为根节点
  		return F;
  	}
  }
  ```

* 合并两个集合

  ```
  void union(int a,int b){
      int fatherA=findfather(a);
      int fatherB=findfather(b);
      if(fatherA!=fatherB) father[fatherA]=fatherB;
      //可以自己设定，比如序号小的为根节点
  }
  ```

### 27 图

  A1013 A1021, A1034 **A1076** A1003 **A1018** A1030 A1072 A1087，A1126(欧拉图)  A1134(图  结点与边的问题)

  A1146(判断是否为拓扑排序序列 )    A1154(25 图  边的两端点的判断 )    A1142(25 无向完全图 最大子图  两点相连)     

A1150(25 判断循环图 输出最小路径)    A1131(30 DFS 、unordered_map邻接矩阵、 难题  )

* 存储方式

  邻接表或邻接矩阵
  
  G[maxn] [maxn] 或者vector< vector <int> > G 或vector<int> G[maxn] 等等
  
  ```
  struct node{
     int id;
     int layer;
  }
  vector<node> adj[amxn];
  ```
  
  
  
* 深度遍历

  ```
  void DFS(u) {//访问顶点u所在的连通块
  	vis[u] = true;
  	......
  	for (从u出发能到达的所有顶点v) {
          ......
  		if (vis[i] == false) {
  			DFS(v);
  		}
  	}
  }
  void DFStrave(G) {//遍历图G
  	for (G的所有顶点u) {
  		if (vis[u] == false) {
  			......
  			DFS(u);
  		    ......
  		}
  	}
  }
  ```

* 广度遍历

  ```
  BFS(u){
     queue q;
     inq[u]=true;
     while(q非空){
       取出队首元素u进行访问；
       for(从u出发可以到达的所有顶点v){
         if(inq[v]==false){
           将v入队;
         inq[v]=true;
         }
       }
  }
  BFStrave(G){
     for(G的所有结点u){
        if(inq[u]==false){
           BFS(u);
        }
     }
  }
  ```

* 判断是否为连通图的两种方法

  * 深度搜索记录访问结点的数量，如果记录的访问的节点数等于总结点数，则为连通图

    ```
    void dfs(int index) {
     visit[index] = true;
     cnt++;
     for (int i = 0; i < v[index].size(); i++)
     if (visit[v[index][i]] == false)
     dfs(v[index][i]);
    }
    ```

    

  * 深度搜索记录连通块数量

  ```
  for(int j=1;j<=n;j++){
  	if(vis[j]==false){
  		DFS(j);
  		block++;
  	}
  }
  ```

* 最短路径dijkstra

  * dijkstra函数内首先进行初始化

    包括fill(vis,vis+maxn,false);//某个结点是否访问
    	fill(d,d+maxn,INF);//到某个结点的最短路径
    	fill(w,w+maxn,0);//到某个结点路径上的总点权(一般为最大值)
    	fill(pt,pt+maxn,0);//路径上的结点个数
    	fill(num,num+maxn,0);//最短路径条数，

    ​     for(int i=0;i<n;i++) pre[i]=i;//每个结点的前驱结点设为其本身

    最后在main函数中还有对临接矩阵或临接链表的初始化，即设为INF

  * 输出路径

    ```
    void printpath(int v){
    	if(v==0){//到达起始节点，开始输出，并逐层返回
    		cout<<inttostring[v];
    		return;
    	}
    	printpath(pre[v]);
    	cout<<"->"<<inttostring[v];
    }
    ```

  * dijkstra示例代码

    ```
    void dijkst(int s){
    	fill(vis,vis+maxn,false);
    	fill(d,d+maxn,INF);
    	fill(w,w+maxn,0);
    	fill(pt,pt+maxn,0);
    	fill(num,num+maxn,0);
        for(int i=0;i<n;i++) pre[i]=i;
    	d[s]=0;
    	w[s]=weight[s];
    	num[s]=1;
    	for(int i=0;i<n;i++){
    		int u=-1,MIN=INF;
    		for(int j=0;j<n;j++){
    			if(vis[j]==false&&d[j]<MIN){
    				u=j;
    				MIN=d[u];
    			}
    		}
    		if(u==-1) return;
    		vis[u]=true;
    		for(int v=0;v<n;v++){
    			if(vis[v]==false&&G[u][v]!=INF){
    				if(d[v]>d[u]+G[u][v]){
    					d[v]=d[u]+G[u][v];
    					w[v]=w[u]+weight[v];	
    					num[v]=num[u];
    					pt[v]=pt[u]+1;
    					pre[v]=u;
    				}else if(d[v]==d[u]+G[u][v]){
    					num[v]+=num[u];
    					if(w[v]<w[u]+weight[v]){
    						w[v]=w[u]+weight[v];
    						pt[v]=pt[u]+1;
    						pre[v]=u;
    					}else if(w[v]==weight[v]+w[u]){
    						double avgfun1=w[v]*1.0/pt[v];
    						double avgfun2=(weight[v]+w[u])*1.0/(pt[u]+1);
    						if(avgfun2>avgfun1){
    							pt[v]=pt[u]+1;
    							pre[v]=u;
    						}
    					}
    				}
    			}
    		}
    	}
    }
    ```

* 哈密顿问题

  A1122(哈密顿回路 set)

* 笔记

  * 连通的、且边数为N-1的具有N个结点的图一定是棵树。在这N个结点中选择合适的根节点，使树的高度最大的办法是：先任意选择一个结点，从该结点出发遍历整个树，获取能达到的最深的结点(记为集合A)，然后从集合A中任意一个结点出发遍历整个树，获取能达到的最深的结点(记为集合B)，集合A与集合B的并集即为所求的使树的高度最大的根结点。

  * 删除图中某个顶点及其相连的边时，不用真的删除，可以在DFS访问到该顶点时返回即可,如下

    ```
    void DFS(int v){
    	if(v==deletepoint) return;//访问到删除的结点时返回
    	vis[v]=true;
    	for(int i=0;i<G[v].size();i++){
    		if(vis[G[v][i]]==false){
    			DFS(G[v][i]);
    		}
    	}
    }
    ```

  * 结点标号为字符串时，可以用map进行和数字间的转化

  * 对于每个结点只能访问一次的情况，可以优先考虑BFS(A1076)

  * 不论是DFS、BFS、djkst都要先初始化传入参数(即起始结点)的相关信息

  * 删除图中某个点i，可以设置该结点vis[i]=true实现“删除”


### 28 动态规划

* 最大连续子序列和  A1007

  状态转移方程:`dp[i]=max{a[i],dp[i-1]+a[i]}`

  注意边界初始条件

  ```
  #include<cstdio>
  const int maxn=10010;
  int a[maxn],dp[maxn],s[maxn]={0};//s数组记录最大连续子序列的起始元素
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
      dp[0]=a[0];//边界初始条件--------------------------------------------------------
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
  		if(dp[i]>dp[k]){//题目要求输出i，j最小的序列，所以这里的判断是>而不是>=
  			k=i;
  		}
  	}
  	printf("%d %d %d",dp[k],a[s[k]],a[k]);
  	return 0;
  }
  ```

* 最长不下降子序列 (LIS) A1045

  状态转移方程:`dp[i]=max{1,dp[j]+1}`
  
  如果存在a[i]之前的元素a[j] (j<i)，使得a[j]<=a[i]并且dp[j]+1>dp[i],那么就把a[i]跟在以a[j]为结尾的LIS后面，形成一条更长的不下降子序列(令dp[i]=dp[j]+1)
  
  如果a[i]之前的元素都比a[i]大，那么a[i]就只好自己形成一条LIS，长度为1
  
  ```
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
  		dp[i]=1;//边界初始条件--------------------------------------
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
  ```
  
* 最长公共子序列 A1045

  * 经典LCS模型，两个序列的元素匹配必须一一对应：

  ​      状态转移方程:如果a[i]==b[j],`dp[i] [j]=dp[i-1] [j-1]+1`,   

  ​      如果a[i]!=b[j],`dp[i] [j]=max{dp[i-1] [j],dp[i] [j-1]}`

  * 本题中允许公共部分产生重复元素

    状态转移方程修改为:

    如果a[i]==b[j],`dp[i] [j]=max{dp[i-1] [j],dp[i] [j-1]}+1`   

    如果a[i]!=b[j],`dp[i] [j]=max{dp[i-1] [j],dp[i] [j-1]}`

  ```
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
      for(int i=0;i<m;i++) dp[i][0]=0;//边界起始条件
      for(int i=0;i<L;i++) dp[0][i]=0;//边界起始条件
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

* 最长回文子串 A1040

  dp[i] [j]表示s[i]至s[j]表示的子串是否为回文子串

  状态转移方程: 如果s[i]==s[j],`dp[i] [j]=dp[i+1] [j-1]`

  如果s[i]!=s[j], `dp[i] [j]=0`

​       边界：`dp[i] [i] =1;dp[i] [i+1]=(s[i]==s[i+1])? 1 : 0`

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
    	for(int i=0;i<len;i++){//边界初始化
    		dp[i][i]=1;
    		if(i<len-1){
    			if(str[i]==str[i+1]){
    				dp[i][i+1]=1;
    				ans=2;//初始化时注意最长回文子串的长度
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
* 背包问题

  * 01背包 A1068

    dp[i] [v]=max{dp[i-1] [v],dp[i-1] [v-w[i]]+c[i]}(1<=i<=n, w[i]<=v<=V)

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

    

### 29 分块  (实时查询问题)

  A1057

某非负整数序列元素的取值范围为0到N，则每块中元素个数是根号N(向下取整)，块数是根号N(向上取整)

例如所有元素都不超过100000，则可以将序列分为317块，每块316个元素，并用table[]数组记录每个元素出现的次数，用block[]数组记录每块中的元素个数

### 30 树状数组(BIT) (实时查询)

`#define lowbit(i) ((i) & (-i))`

C[i]的覆盖长度是`lowbit(i)`,树状数组的下标必须从1开始

```
int getsum(int x)//返回前x个整数之和{
   int sum=0;
   for(int i=x;i>0;i-=lowbit(i)){
      sum+=c[i];
   }
   return sum;
}
```

```
void update(int x,int v){
   for(int i=x;i<=N;i+=lowbit(i)){
      c[i]+=v;
   }
}
```



### 31 可怕模拟

A1017(25 )   A1153( 25 模拟 排序引用传参 vector unordered_map)

A1014(30 模拟  难题)

### 32 n皇后

A1128(20)

### 33.堆

堆是一棵完全二叉树，可以用heap[1]~heap[n]数组存储，

* 堆排序

  ```
  //大顶堆为例
  void downadjust(int low, int high) {
  	int i = low, j = 2 * i;
  	while (j <= high) {
  		if (j + 1 <= high && heap[j + 1] > heap[j]) {
  			j = j + 1;
  		}
  		if (heap[i] < heap[j]) {
  			swap(heap[i], heap[j]);
  			i = j;
  			j = 2 * i;
  		}
  		else {
  			break;
  		}
  	}
  }
  void createheap() {//建堆
  	for (int i = n / 2; i >= 1; i--) {
  		downadjust(i, n);
  	}
  }
  void heapsort(){
      createheap();
      for(int i=n;i>1;i--){
          swap(heap[1],heap[i]);
          upadjust(1,i-1);
      }
  }
  ```

  

* 删除堆顶元素

  ```
  void deleteTop(){
      heap[1]=heap[n--];//用最后一个元素覆盖堆顶元素，并让元素个数减一
      downadjust(1,n);
  }
  ```

  



* 在堆的最后插入新元素

  ```
  void upadjust(int low, int high) {//
  	int i = high, j = i / 2;
  	while (j >= low) {
  		if (heap[i] < heap[j]) {
  			swap(heap[i], heap[j]);
  			i = j;
  			j = i / 2;
  		}
  		else {
  			break;
  		}
  	}
  }
  void insert(int x){
      heap[++n]=x;
      upadjust(1,n);
  }
  ```

  

# 注意

1.= 与 ==   

2.变量初始化赋值时有时必须赋值为0或1

3.注意全局变量与局部变量

4.`s1[i]-'0'`，字符数组变为整型数字

5.`s1[i]>='0' && s1[1]<='9'`，字符数组中寻找0到9之间的数字

6.注意输出结果为0时的特殊情况，隐藏的测试点等

7.段错误很可能是数组越界，可能是定义的数组长度不符合题目限制的最大范围

8.注意if 和else if 的搭配使用， 注意函数传入值node* root ，下面对应root=new node,而不是node*root=new node,输出格式`printf("\n")`,`printf(" ")`是否正确

9.如果想要在 Dev-Cpp ⾥⾯使⽤C++11, 在菜单栏中⼯具-编译选项-编译器-编译时加⼊ -std=c++11 这句命令即可

11.有时并不一定真的要实现出正确的算法，最后不管用什么方式，只要能输出正确的格式就好

12.注意设置bool型的变量用于判断

13.注意输入的例子中可能有无效的数据，要找出有效的数据进行处理

14.`cmp`中写按字母大小排序，注意写成`strcmp()<0`......的形式

15.看题目中的数据是整数还是浮点数，注意题目中数据的保留位数

16.注意是使用while还是if，不要忘了break或continue，注意区分是用break还是continue

17.`long long k=(long long)p*num[i]`;    若新声明的变量为long long 型，右边为int乘int型，最好在右边带上强制转换(long long)，其他情况也这样吧....

18.注意是多个if并列 还是if与else if结合使用

19.int上限为`0x7fffffff`, 即2^31 -1,可以用`const int  INF=0x7fffffff`,也可以写成`(1<<31)-1`

​     int下限为-2^31。const double eps=1e-8(即10的-8次方，注意是数字1不是字母l)，const double     INF=1e12(即10^12)

20.字符数组变为数字时要注意减'0',`res.d[i] = str[res.len - 1 - i]-'0'`

21.函数的参数前有时需要加引用&

22.操作放在花括号内还是花括号外，注意条件的判断

23.注意DFS的下一层进行DFS时，要写DFS(i,height+1)  不要写height++

24.涉及到排序问题时，可以考虑使用set或map内部自动排序的容器，也可以先把题目中给出的数据先排序再处理

25.提交时出现“运行时发生错误”的情况，一般是开的数组太小，没有达到题目要求的最大值

26.不要忘了一些算法的初始化，比如使用并查集千万不要忘了初始化`father[i]=i`

27一些题目处理的是五位数的格式，(比如id号)，所以输出时注意`printf("%05d")`

28.四舍五入为整数`int(v[temp].gm * 0.4 + v[temp].gf * 0.6 + 0.5);`

​      **在最后加上0.5并强制转换为int型即为四舍五入**

29.A1139邻接矩阵的巧妙写法

```
unordered_map<int ,bool> isfri;
isfri[abs(stoi(a))*10000+abs(stoi(b))]=isfri[abs(stoi(b))*10000+abs(stoi(a))]=true;
10000为超过最大范围的整数
```

30.题目中数据量大的时候最好不用`cin`和`cout`

31.数据量大时sort的cmp函数可以用引用传值，以防超时

 比如

```
bool cmp2(cosnt node& a,const node& b){
	return a.id<b.id;
}
```

32.题目中同时有int型和浮点型格式的数据，最好统一定义成浮点型

33. 某个测试点不过有可能是数据类型有问题，比如需要强制类型转换等等，换成数据表示范围更大的64位数据类型

* 层序遍历中队列里存放的是结点的地址，动态的就是node*类型，静态的就是root(int型)结点编号

* 将图当做树去处理时，注意无向图是双向的，可能会回溯已经访问的结点，要注意避免；在统计图的边权和或者点权和等信息时，为了避免重复访问或者累加，可以在访问后将点权或边权设为0