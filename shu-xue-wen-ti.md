---
title: '数学问题'
date: 2020-05-07 21:15:56
tags: [c++]
published: true
hideInList: false
feature: 
isTop: false
---
* 求最大公约数,(最小公倍数：a和b的最大公约数为d，则a和b的最小公倍数为ab/d)

  ```
  long long gcd(long long a, long long b) {
       return b == 0 ? abs(a) : gcd(b, a %b);
   }
  ```

* 分数化简

  ```
  struct fraction {
  	ll up, down;
  }a, b;
  fraction reduction(fraction result) {
  	if (result.down < 0) {
  		result.up = -result.up;
  		result.down = -result.down;
  	}
  	if (result.up == 0) result.down = 1;
  	else {
  		ll gcdvalue = gcd(abs(result.up), abs(result.down));
  		result.up /= gcdvalue;
  		result.down /= gcdvalue;
  	}
  	return result;
  }
  ```

* 四则运算

  ```
  fraction add(fraction f1, fraction f2) {
  	fraction result;
  	result.up = f1.up * f2.down + f1.down * f2.up;
  	result.down = f1.down * f2.down;
  	return reduction(result);
  }
  ```

* 分数输出

  ```
  void showresult(fraction r) {
  	r = reduction(r);
  	if (r.up < 0) printf("(");
  	if (r.down == 1) printf("%lld", r.up);
  	else if (abs(r.up) < r.down) printf("%lld/%lld", r.up, r.down);
  	else if (abs(r.up) > r.down) printf("%lld %lld/%lld", r.up / r.down, abs(r.up) % r.down, r.down);
  	if (r.up < 0) printf(")");
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
  ```

  