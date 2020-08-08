---
title: 'c++刷题常用函数'
date: 2020-05-07 21:13:37
tags: [c++]
published: true
hideInList: false
feature: 
isTop: false
---
* `math.h`头文件函数

  `fabs(double x)`  取绝对值 ，

  `floor(double x)`和`ceil(double x)` ，分别为向上取整和向下取整，返回double型

  `pow(double r,double p)` 用于返回r^p

  `sqrt(double x)` ,返回算数平方根

  `log(double x)` ,返回以自然对数为底的对数，用换底公式求具体对数

  `sin(double x)` `cos(double x)` `tan(double x)` 

  `asin(double x)` `acos(double x)` `atan(double x)` 

  `round(double x)` 将x四舍五入，返回为double型

* `algorithm`头文件下的函数，加`using namespace std;`

  `max(x,y)` `min(x,y)`  `abs(x)` 返回x的绝对值，x必须为整数

  `swap(x,y)` 

  `reverse(it,it2)` `reverse(a,a+4)`  ,将数组元素反转

  `next_permutation(a,a+...)` ,给出一个序列在全排列中的下一个序列，该函数在到达全排列的最后一个时会返回false

  `fill(a,a+4,233)` ,赋相同值，对于`G[maxn] [maxn]`二维数组`fill(G[0],G[0]+maxn*maxn,INF);`

  `sort(首元素地址，尾元素地址的下一个地址，比较函数（非必填)）`,无比较函数，默认递增排序。

  `sort(a, a+n, greater());`  从大到小排序(要用`iostream`头文件)

  `lower_bound(first,last,val)`  `upper_bound(first,last,val)` ,用于有序数组或容器，前者用来寻找[first,last)范围内第一个值大于等于val的元素的位置，后者寻找第一个值大于val的元素位置.若是数组，这两个函数返回的是地址，如`int* right=upper_bound(num, num+n, k)`，返回num数组中第一个大于k的元素所在地址，其下标=`right-num`

  `max_element(a, a+len)`返回序列中最大元素地址（迭代器）,可用其减去数组首地址(即数组名)获取其下标,若要获取该元素值，直接在前面加一个*,表示取地址即可

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

3.`stoi`如果遇到的是⾮法输⼊（⽐如`stoi("123.4")`，123.4不是⼀个int型变量）： 1.会⾃动截取最前⾯的     数字，直到遇到不是数字为⽌ (所以说如果是浮点型，会截取前⾯的整数部分) 2.如果最前⾯不是数字，会运⾏时发⽣错误 

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


  

