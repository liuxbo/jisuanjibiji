---
title: '字符串相关'
date: 2020-05-07 19:31:12
tags: [c++]
published: true
hideInList: false
feature: 
isTop: false
---
* `scanf ("%s")`识别空格作为字符串结尾

  `      getchar`  `putchar` 用来输入输出单个字符

  `      gets` `puts` 用来输入输出一行字符串，`gets` 识别换行符\n作为输入结束  ，gets现在已经不支持了

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
