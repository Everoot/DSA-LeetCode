---
tags:
    - Math
    - Recursion
    - Top Interviews
---



# [50. Pow(x, n)](https://leetcode.com/problems/powx-n/)

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xn`).

 

**Example 1:**

```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**

```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**

```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```



**Solution:**

```java
class Solution {
    public double myPow(double x, int n) {
        if (n == 0){
            return 1;
        }
        long N = (long) n;
        if (N < 0){
            N = -1 * N;
            x = 1.0 / x;
        }

        double result = 1;

        while (N != 0){
            if (N % 2 == 1){
                result = result * x;
                N = N - 1;
            }

            x = x * x;
            N = N / 2;
        }

        return result;
    }
}
```



How to quickly calculate $x^8$?
$$
x \rightarrow x^2 \rightarrow x^4 \rightarrow x^8
$$
Start from $x$, repeatedly square it three times to get $x^8$.



How to quicky calculate $x^{15}$?

During the process $x \rightarrow x^2 \rightarrow x^4 \rightarrow x^8$, multiply all these numbers together to get $x^{15}$
$$
x \cdot x^2 \cdot x^4 \cdot x^8 = x^{15}
$$

如何快速计算$x^{13}$?

During the process $x \rightarrow x^2 \rightarrow x^4 \rightarrow x^8$的过程中, 把$x, x^4, x^8$想乘, 即可得到$x^{13}$.



如何知道要把哪些数相乘?
$$
13 = 1101_{(2)}
$$
把n看成二进制数, 从低到高遍历二进制数, 遇到1就和x的幂相乘.



如何遍历二进制数中的1?
$$
1101 \rightarrow 110 \rightarrow 11 \rightarrow 1
$$
不断右移二进制数, 右移前看最低位是否为1.

如何处理$n < 0$的情况?

把$n$变成$-n$, 把x变成$\frac{1}{x}$, 就转换成了$n \geq 0$的情况.

⚠部分语言需要注意，当$n=−2^{31}$ 时，$−n=2^{31}$ 比 32 位整数的最大值还大，溢出了。可以转成 64 位整数解决。

 


```java
class Solution {
    public double myPow(double x, int n) { // 2, 10
       double ans = 1; 
       long N = n; // 10 
       if (N < 0){ // x^-N = (1/x)^N
        N = -N;
        x = 1/x;
       } 

       while(N != 0){ // 从低到高枚举 n 的每个比特位// 1010
        if ((N & 1) == 1){ // 这个比特位是 1      N& 1 比的末尾   
            ans = ans * x; // 把 x 乘到 ans 中   //ans = 1 / 1*4 // 4 * 16 = 64
        }

        x = x * x; // x 自身平方 // x = 4 // 16 // 
        N = N >> 1; // 继续枚举下一个比特位 // 101 // 10 // 1 // 0
       }

       return ans;
    }
}

// TC: O(log|n|)
// SC: O(1)
```

> `long N = n`
>
> 要使用整数类型(byte, short, int, long) 才能位运算



方法二: 递归

当$n < 0$时, $x^n= (x^{-1})^{-n}=(\frac{1}{x})^{-n}$.

当$n \geq 0$时, 我们有
$$
x^n = \left\{\begin{align*}
& (x^{\frac{n}{2}})^2, n \ mod \ 2 = 0 \\
& (x^\left \lfloor \frac{n}{2} \right \rfloor)^2 \cdot x, n \ mod \ 2 = 1
\end{align*}\right.
$$
 根据上式递归计算$x^n$.

递归边界: $x^0 =1$.

```java
class Solution {
    public double myPow(double x, int n) {
        if (n < 0){
            return pow(1 / x, -(long) n);
        }

        return pow(x, n);
    }

    private double pow(double x, long n){
        if (n == 0){
            return 1;
        }

        double res = pow(x, n / 2);
        res = res * res;
        if (n % 2 != 0){
            res = res * x;
        }

        return res;
    }
}

// TC: O(log∣n∣)。
// SC: O(log∣n∣)。递归需要 O(log∣n∣) 的栈开销。
```





```java
class Solution {
    public double myPow(double x, int n) {
        return helper(x, (long) n);
    }
  
         // base case
       // 1: 0^0   0^1232 = 0
       // 2: 1^0 = 1      2^0 = 1
       // 3: 1^134 =1
       // 4: -1^    if (n % 2 == 0)  / n % 2 != 0
       // 5. Max Integer min Integer overflow 

    private double helper(double x, long n){
        // base case
        if (x == 0){
            return 0;
        }else if (x == 1){
            return 1;
        }else if (x == -1){
            if (n % 2 == 0){
                return 1;
            }else{
                return -1;
            }
        }else if (n == 0){
            return 1;
        }else if (n == 1){
            return x;
        }

        if (n < 0){
            x = 1/x;
            n = -n;
        }

        double result = helper(x, n/2);

        if (result >= Integer.MAX_VALUE/2){
            return 0;
        }

        if (n % 2 == 0){
            return (result * result);
        }else {
            return (result * result * x);
        }
    }   
}
}
// TC : O(logn)
// SC : O(logn)
//               pow (2, 5)
//                /     \
//             2^2       2^2
//            /  \
//           2^1
//
```



Laioffer 13 a to the power of b

Evaluate a to the power of b, assuming both a and b are integers and b is non-negative.

**Examples**

- power(2, 0) = 1
- power(2, 3) = 8
- power(0, 10) = 0
- power(-2, 5) = -32

**Corner Cases**

- In this question, we assume $0^0$ = 1.
- What if the result is overflowed? We can assume the result will not be overflowed when we solve this problem on this online judge.



## **Fast Power Algorithm Recursive** 

Intuition

Assuming we have got the result of $x^n$, how can we get $x^{2*n}$ ? Obviously we do not need to multiply $x$ for another $n$ times. Using the formula $(x^n)^2 = x^{2*n}$, we can get $x^{2*n}$ at the cost of only one computation. Using this optimization, we can reduce the time complexity of our algorithm.



Algorithm 

Assume we have got the result of $x^{\frac{n}{2}}$, and now we want to get the result of $x^n$. Let `A` be result of $x^{\frac{n}{2}}$, we can talk about $x^n$ based on the parity of $n$ respectively. If $n$ is even, we can use the formula $(x^n)^2 = x^{(2*n)}$ to get $x^n = A*A$. If $n$ is odd, then $A*A = x^{n-1}$. Intuitively, we need to multiply another $x$ to the result, so $x^n = A * A * x$. This approach can be easily implemented using recursion. We call this method "Fast Power", because we only need at most $O(\log n)$ computations get $x^n$

 

Primitive Data Types

A primitive data type specifies the size and type of variable values, and it has no additional methods.

There are eight primitive data types in Java:

| Data Type | Size    | Description                                                  |
| --------- | ------- | ------------------------------------------------------------ |
| byte      | 1 byte  | Store whole numbers from -128 to 127                         |
| short     | 2 bytes | Store whole numbers from -32768 to 32767                     |
| int       | 4 bytes | Store whole numbers from -2147483648 to 2147483647           |
| long      | 8 bytes | Stores whole numbers from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| float     | 4 bytes | Stores fractional numbers. Sufficient for storing 6 to 7 decimal digits |
| double    | 8 bytes | Stores fractional numbers. Sufficient for storing 15 decimal digits |
| boolean   | 1 bit   | Store true or false values                                   |
| char      | 2 bytes | Stores a single character/letter or ASCII values             |





For the LeetCode it has test cases, x = 2, n = -2147483648. You have to transform the data type. Which means you have to avoid overflow.



## Fast Power Algorithm Iterative

Intuition 

Using the formula $x^{a+b} = x^a x^b$, we can write $n$ as a sum of positive integers, $n = \sum_i{b_i}$. If we can get the result of $x^{b_i}$ quickly, the total time for computing $x^n$ will be reduced.

Algorithm 

We can use the binary representation of $n$ to better understand the problem. Let the binary representation of $n$ to be $b_1, b_2, ..., b_{length\_limit}$, from the Least Significant Bit(LSB) to the Most Significant Bit(MSB). For the $i$ th bit, if $b_i =1$, it means we need to multiply the result by $x^{2^i}$ . 

It seems to have no improvement with this representation, since $\sum_i b_i2^i =n$. But using the formula $(x^n)^2 = x^{2n}$ we mentioned above, we can see some differences. Initially $x^1 = x$ , and for each $i >1$, we can use the result of $x^{2^{i-1}}$ to get $x^{2^i}$ in one step. Since the number of $b_i$ is at most $O(logn)$, we can get all $x^{2^i}$ in $O(\log n)$ time. After that, for all $i$ s that satisfy $b_i =1$, we can multiply $x^{2^i}$ to the result. This also requires $O(\log n)$ time.

Using fast power recursively or iteratively are actually taking different paths towards the same goal. For more information about fast power algorithm, you can visit its wiki https://en.wikipedia.org/wiki/Exponentiation_by_squaring .



Binary Search

$a = 3, b = 5$

$3^5 = 3 \times 3 \times 3 \times 3 \times 3$

$i = 5, 5 \ \% \ 2 = 1, result = 1 \times 3 = 3, current\_result = 3^1 $

$i = 4, 4 \ \% \ 2 = 0, result = 3 , current\_result=3^2  $

$i = 3, 3 \ \% \ 2 = 1, result = 3 \times 3^2 = 3^3 , current\_result = 3^2$

$i = 2, 2 \ \% \ 1 = 0, result = 3^3, current\_result = 3^2 \times 3^2 = 3^4 $

$i = 1, 1 \ \% \ 2 = 1, result = 3^3 \times 3^2 =3^5   , current\_result=3^4$

$i = 0, stop$

````java
/*13. a to the power of b
Evaluate a to the power of b, assuming both a and b are integers and b is non-negative.

Examples

power(2, 0) = 1
power(2, 3) = 8
power(0, 10) = 0
power(-2, 5) = -32
Corner Cases

In this question, we assume 0^0 = 1.
What if the result is overflowed? We can assume the result will not be overflowed when we solve this problem on this online judge.

Leetcode(50)

50. Pow(x, n)

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., $x^n$).

 **Example 1:**

```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**

```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**

```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```


*/

public class AToThePowerOfB13{
	public static void main(String[] args){
		double  a1 = 2;
		int b1 = 0;
		double a2 = 2;
		int b2 = 3;
		double a3 = 0;
		int b3 = 10;
		double a4 = -2;
		int b4 = 5;
		double a5 = 2;
		int b5 = -2;
		double a6 = 2;
		int b6 = -2147483648;

		/*
		
		System.out.println("Method 1: ");
		System.out.println(power(a1,b1));
		System.out.println(power(a2,b2));
		System.out.println(power(a3,b3));
		System.out.println(power(a4,b4));
		System.out.println(power(a5,b5));
		System.out.println(power(a6,b6));
		*/
		// Stackover
		
		/*	
		System.out.println("Method 2: ");	
		System.out.println(power2(a1,b1));
		System.out.println(power2(a2,b2));
		System.out.println(power2(a3,b3));
		System.out.println(power2(a4,b4));
		System.out.println(power2(a5,b5));
		System.out.println(power2(a6,b6));
		// Time limit (over time)
		*/
	


		System.out.println("Method 3: ");	
		System.out.println(power3(a1,b1));
		System.out.println(power3(a2,b2));
		System.out.println(power3(a3,b3));
		System.out.println(power3(a4,b4));
		System.out.println(power3(a5,b5));
		System.out.println(power3(a6,b6));

		System.out.println("Method 4: ");	
		System.out.println(power4(a1,b1));
		System.out.println(power4(a2,b2));
		System.out.println(power4(a3,b3));
		System.out.println(power4(a4,b4));
		System.out.println(power4(a5,b5));
		System.out.println(power4(a6,b6));
	}


	public static double power(double a, int b){
		// base case
		double B = b;
		if (a == 0){
			return 0;
		}

		if (B == 0 || a == 1){
			return 1;
		}

		if (B < 0){
			a = 1/a;
			B = -B;
		}

		// recursive 
		// p(a,b) = p(a,b-1)*a
		
		double result = power(a,(int) B-1) * a; // StackOverflowError

		return result; 
		
	}
	/* Time complexity: O(n)
	 *
	 * Space complexity: O(n)
	 */

	public static double power2(double a, int b){ // Time Limit Exceeded
						      // Stack over
		// base case
		long B = b;
		if (a == 0){
			return 0;
		}
		
		if (B == 0 || a == 1){
			return 1;	
		}

		if (B < 0){
			a = 1/a;
			B = -B;
		}

		double result = a;

		for (int i = 1; i < B; i++){
			result = result * a;
		}

		return result;

	} 
	/* Time complexity: O(n)
	 *
	 * Space complexity: O(1)
	 */

	public static double power3(double a, int b){
		// base case
		double B = b;
		if (a == 0){
			return 0;
		} 
		
		if (B == 0 || a == 1){
			return 1;
		}

		if (B < 0){
			a = 1/a;
			B = -B;
		}


		// recursive 
		double result = power3(a, (int) B/2);
		
		if (B % 2 == 0){
			result = result * result;
		} else {
			result = result * result * a;
		}	

		return result;
		
	}


	/* Time complexity: O(logn);
	 *
	 *
	 * Space complexity: O(logn);
	 */


	public static double power4(double a, int b){
		// base case 
		long b = b;
		if (a == 0){
			return 0;
		}

		if (b == 0 || a == 1){
			return 1;
		}

		if (b < 0){
			a = 1/a;
			b = -b;
		}

		// iterative			
		double result = 1;
		double current_product = a;

		for (long i = b; i > 0; i /= 2){
			if ((i % 2) == 1){
				result = result * current_product;
			}
			current_product = current_product * current_product;
		}

		return result;
				
	}

	/* time complexity: o(logn). 
	 * space complexity: o(1);
	 */

}

````



