##Math Related Summary

**常见题型** 
- pow(x, n)
- isPrime(x)
- countPrimeNumberLessthan(x)
- findAllPerfectSqureNum(x, y)
- sqrt (x) 


> **pow(x, n)** 利用二分查找降低复杂度到 O(logn)
	-  **注意 正负数指数n**
	-  **注意 integer overflow**

```java
public double pow(double x, int n) {
	if (n < 0) {
		//To avoid integer overflow
		//Math.abs(Integer.MIN_VALUE) is greater than Integer.MAX_VALUE
		n = -(n + 1);
		return pow(1/x, n) * 1/ x;// 传递性 x^n = x^(n+1) * 1/x;
	}
	if(n == 0) return 1;
	
	double res = pow(x, n / 2);
	if (n % 2 == 0) {
		return res * res;
	}else {
		return x * res * res;
	}
}
```



> **isPrime(x)** 利用二分查找降低复杂度到 O(sqrt(n))
	-  2 is the smallest prime number
	-  num % 2 == 0 is **not** prime number
	-  all odd numbers are prime number
	-  if the input X can be divided by any factor in [3, sqrt(X)], it's **not** prime number  
	- Why to sqrt(X)?
	  *if x is a prime number, it can be factored into two factors a and b, like x = a \* b, if both a and b are greater than sqrt(X), a*b will be greater than X.  So at least one of those factors must be less or equal to the square root of n, and to check if n is prime, we only need to test for factors less than or equal to the square root.*

```java
public boolean isPrime(int num) {
	if (num == 2) return true;
	if (num < 2 || num % 2 == 0) return false;
	//i * i <= num to avoid using sqrt func, much faster
	//i += 2 skip all even numbers
	for (int i = 3; i * i <= num; i += 2) {
		if (num % i == 0) {
			return false;
		}
	}
	return true;
}
```

> **countPrimeNumberLessthan(X)** 利用sieve of Eratosthenes 把时间复杂度降低到O(n) 
	- Detail explanation is http://www.algolist.net/Algorithms/Number_theoretic/Sieve_of_Eratosthenes
	- 简单来说
	- mark 2 ~ sqrt(X) 中每个质数的倍数，然后找出剩余的数
	- i * i + i, +2i, +3i ... = i * (i + 1) 都可以被 i 整除，所以都不是质数

```java
public int countPrimeNumberLessthan(int num) {
	boolean[] nonPrime = new boolean[num + 1];
	//mark non-prime number as true
	for (int i = 2; i * i< num; i++) {
		if(!nonPrime[i]) {
			for (int j = i * i; j < num; j += i) {
				nonPrime[j] = true;
			}
		}
	}
	//count prime number
	for (int i = 2; i < num; i++) {
		res = !nonPrime[i] ? res + 1 : res;
	}
	return res;
}
```

> **findAllPerfectSqureNum(x, y) where 1 <= x <= y <= 1,000,000,000** 利用sqrt 的等效性降低循环区间 O(sqrt(y) - sqrt(x))
	- 若果一个数 在 [sqrt(x), sqrt(y)] 之间，那么他们的平方数一定在[x, y]区间

```java
public List<Integer> perfectSqureNum (int x, int y) {
	int start = Math.floor(Math.sqrt(x));
	int end = Math.ceil(Math.sqrt(y));
	List<Integer> res = new LinkedList<>();
	for (int i = start + 1; i < end; i++) {
		res.add(i);
	}
	return res;
}
```


> **mySqrt(X)** 手动实现求平方根，利用二分查找实现O(logn)
	- 考点就是二分查找
	- 注意边界条件
	
```java
public int mySqrt(int num) {
	if (num == 0 || num == 1) {
		return num;
	}
	
	int low = 0, high = num;
	while (low <= high) {
		int mid = low + (high - low) / 2;
		if (mid == num / mid) {
			return mid;
		}else if (mid > num / mid) {
			high = mid - 1;
		}else {
			low = mid + 1;
		}
	}
	
	return high;
}
```