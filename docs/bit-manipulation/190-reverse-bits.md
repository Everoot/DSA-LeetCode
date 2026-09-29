---
tags:
    - Divide and Conquer
    - Bit Manipulation
    - Top Interviews
---

# [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/)

Reverse bits of a given 32 bits unsigned integer.

**Note:**

- Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two's_complement). Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

 

**Example 1:**

```
Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
```

**Example 2:**

```
Input: n = 11111111111111111111111111111101
Output:   3221225471 (10111111111111111111111111111111)
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
```

 **Constraints:**

- The input must be a **binary string** of length `32`

**Follow up:** If this function is called many times, how would you optimize it?



**Solution:**

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;
        // n:     -> 
        // result:    <-
        for (int i = 0; i < 32; i++) {
            result = result << 1;    // shift all bit one position left
            // 0101 << 1    = 1010
            int bit = (n & 1); // // Extract the least significant bit of n
            result = result | bit;
            // Perform bitwise OR on result with the extracted bit 
            // and store the result back in result
            n = n >> 1;
        }
        return result;
    }
}

// TC: O(logn)
// SC: O(1)
```



`<<`: Left Shift Operator

```java
int a = 5; // binary representation is 0101
int result = a << 2; // result is 10100, which is 20 in decimal
```

The left shift operator `<<` shifts the bits of its left-hand operand to the left by the number of positions specified by its right-hand operand. Vacant positions are filled with zeros.


`>>`: Signed Right Shift

```java
int a = -20; // negative numbers are represented in two's complement form
int result = a >> 2; // result maintains the sign, giving -5
```

The signed right shift operator `>>` shifts the bits of its left-hand operand to the right by the number of positions specified by its right-hand operand. Vacant positions are filled with the sign bit.



`>>>`: Unsigned Right Shift

```java
int a = -20;
int result = a >>> 2; // result is a large number because the sign bit is not propagated
```

The unsigned right shift operator `>>>` shifts the bits of its left-hand operand to the right by the number of positions specified by its right-hand operand. Vacant positions are filled with zeros, regardless of the sign of the initial number.





---

```java
class Solution {
    public int reverseBits(int n) {
       int result = 0;
       for (int i = 0; i < 32; i++){
        result = result << 1;
        int bit = (n & 1);
        System.out.println(
            "bit = " + bit +
            ", n = " +
            String.format("%32s", Integer.toBinaryString(n))
                  .replace(' ', '0')
        );  
        result = result | bit;
        System.out.println(
            "result = " +
            String.format("%32s", Integer.toBinaryString(result))
                  .replace(' ', '0')
        );

        n = n >> 1;
       } 

       return result;
    }
}
```



```
bit = 0, n = 00000010100101000001111010011100
result = 00000000000000000000000000000000
bit = 0, n = 00000001010010100000111101001110
result = 00000000000000000000000000000000
bit = 1, n = 00000000101001010000011110100111
result = 00000000000000000000000000000001
bit = 1, n = 00000000010100101000001111010011
result = 00000000000000000000000000000011
bit = 1, n = 00000000001010010100000111101001
result = 00000000000000000000000000000111
bit = 0, n = 00000000000101001010000011110100
result = 00000000000000000000000000001110
bit = 0, n = 00000000000010100101000001111010
result = 00000000000000000000000000011100
bit = 1, n = 00000000000001010010100000111101
result = 00000000000000000000000000111001
bit = 0, n = 00000000000000101001010000011110
result = 00000000000000000000000001110010
bit = 1, n = 00000000000000010100101000001111
result = 00000000000000000000000011100101
bit = 1, n = 00000000000000001010010100000111
result = 00000000000000000000000111001011
bit = 1, n = 00000000000000000101001010000011
result = 00000000000000000000001110010111
bit = 1, n = 00000000000000000010100101000001
result = 00000000000000000000011100101111
bit = 0, n = 00000000000000000001010010100000
result = 00000000000000000000111001011110
bit = 0, n = 00000000000000000000101001010000
result = 00000000000000000001110010111100
bit = 0, n = 00000000000000000000010100101000
result = 00000000000000000011100101111000
bit = 0, n = 00000000000000000000001010010100
result = 00000000000000000111001011110000
bit = 0, n = 00000000000000000000000101001010
result = 00000000000000001110010111100000
bit = 1, n = 00000000000000000000000010100101
result = 00000000000000011100101111000001
bit = 0, n = 00000000000000000000000001010010
result = 00000000000000111001011110000010
bit = 1, n = 00000000000000000000000000101001
result = 00000000000001110010111100000101
bit = 0, n = 00000000000000000000000000010100
result = 00000000000011100101111000001010
bit = 0, n = 00000000000000000000000000001010
result = 00000000000111001011110000010100
bit = 1, n = 00000000000000000000000000000101
result = 00000000001110010111100000101001
bit = 0, n = 00000000000000000000000000000010
result = 00000000011100101111000001010010
bit = 1, n = 00000000000000000000000000000001
result = 00000000111001011110000010100101
bit = 0, n = 00000000000000000000000000000000
result = 00000001110010111100000101001010
bit = 0, n = 00000000000000000000000000000000
result = 00000011100101111000001010010100
bit = 0, n = 00000000000000000000000000000000
result = 00000111001011110000010100101000
bit = 0, n = 00000000000000000000000000000000
result = 00001110010111100000101001010000
bit = 0, n = 00000000000000000000000000000000
result = 00011100101111000001010010100000
bit = 0, n = 00000000000000000000000000000000
result = 00111001011110000010100101000000
```

