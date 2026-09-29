---
tags:
    - Math
    - String
    - Stack
    - Google
    - Top Interviews
---

# [224. Basic Calculator](https://leetcode.com/problems/basic-calculator/)

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return *the result of the evaluation*.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

 

**Example 1:**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2:**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```



**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
- `s` represents a valid expression.
- `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
- `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
- There will be no two consecutive operators in the input.
- Every number and running calculation will fit in a signed 32-bit integer.



Solution:

```java
/*
 * This function evaluates a mathematical expression represented as a string.
 * The expression may contain:
 *   - Non-negative integers
 *   - Operators: '+', '-', '*', '/'
 *   - Parentheses '(', ')'
 * The goal is to correctly compute the value of the expression, respecting the
 * precedence of operations and handling nested parentheses through recursion.
 *
 * Approach:
 * 1. Parse the expression character by character.
 * 2. Use a stack to handle intermediate results for addition and subtraction.
 * 3. For multiplication and division, immediately compute the result with the
 *    previous value from the stack.
 * 4. Recursively handle parentheses by evaluating sub-expressions.
 * 5. Return the final result after processing all characters.
 */

class Solution {
    /*
     * This function evaluates a mathematical expression represented as a string.
     * It uses a stack to handle intermediate results and recursion for parentheses.
     */
    public int calculate(String s) {
        // Convert the string into a character array and call the helper function
        return helper(new StringBuilder(s));
    }

    /*
     * Helper function to evaluate the expression. This function uses a stack to handle
     * addition and subtraction, and evaluates parentheses recursively.
     */
    private int helper(StringBuilder s){
        Stack<Integer> stack = new Stack<>();
        int curNumber = 0;
        char preOperation = '+';

        while(s.length() > 0){
            char ch = s.charAt(0);
            s.deleteCharAt(0); 
            // Move forward by deleting the character we've just processed


            // Build the current number
            if (Character.isDigit(ch)){
                curNumber = curNumber * 10 + (int) (ch - '0');
            }

            // If we encounter an opening parenthesis, recursively calculate the sub expression
            if (ch == '('){
                curNumber = helper(s); // Solve the sub-expression recurisively
            }


            // If we encounter an operator or reach the end of the string
            // process the last number
            if (!Character.isDigit(ch) && ch != ' ' || s.length() == 0){
                if (preOperation == '+'){
                    stack.push(curNumber);
                }else if (preOperation == '-'){
                    stack.push(-curNumber);
                }else if (preOperation == '*'){
                    stack.push(stack.pop() * curNumber);
                }else if (preOperation == '/'){
                    stack.push(stack.pop() / curNumber);
                }

                // Reset the number and update the sign for the next operation
                curNumber = 0;
                preOperation = ch;
            }

            // If we encounter a closing parenthesis, return the result of the subexpression
            if (ch == ')'){
                break;
            }
        }

        // Sum up the stack to get the result of this expression
        int result = 0;
        while(!stack.isEmpty()){
            result = result + stack.pop(); // Sum all values in the stack
        }

        return result;
    }
}

// TC: O(n)
// SC: O(n)

```







String -> int

```java
String s = "458";

int n = 0;

for (int i = 0; i < s.length(); i++){
  char c = s.charAt(i);
  n = 10 * n + (c - '0');
}
```



`+` `-`

```java
int calculate(String s){
  Stack<Integer> stk = new Stack<>();
  
  int num = 0;
  
  char sign = '+';
  
  for (int i = 0; i < s.length(); i++){
    char c = s.charAt(i);
    if (Character.isDigit(c)){
      num = 10 * num + (c - '0');
    }
    
    if (c == '+' || c == '-' || i == s.length() - 1){
      switch(sign){
        case '+':
          stk.push(num);
          break;
        case '-':
          stk.push(-num);
          break;
      }
      sign = c;
      num = 0;
    }
  }
  
  int res = 0;
  while(!stk.isEmpty()){
    res = res + stk.pop();
  }
  
  return res;
}
```



`*` , `/`:

```java
for (int i = 0; i < s.size(); i++){
  char c = s[i];
  if (isdigit(c)){
    num = 10 * num + (c - '0');
  }
  
  if (!isdigit(c) || i == s.size() - 1){
    switch(sign){
        int pre;
      case '+':
        stk.push(num);
        break;
      case '-';
        stk.push(-num);
        break;
      case '*':
        pre = stk.top();
        stk.pop();
        stk.push(pre * num);
        break;
      case '/':
        pre = stk.top();
        stk.pop();
        stk.push(pre / num);
        break;
    }
    sign = c;
    num - 0;
  }
}
```

乘除法优先于加减法体现在, 乘除法可以和栈顶的数结合, 而加减法只能把自己放入栈.



`(`, `)`:

```java
```

括号具有递归



```java
class Solution {
    public int calculate(String s) {

        Stack<Integer> stack = new Stack<Integer>();
        int operand = 0;
        int result = 0; // For the on-going result
        int sign = 1;  // 1 means positive, -1 means negative

        for (int i = 0; i < s.length(); i++) {

            char ch = s.charAt(i);
            if (Character.isDigit(ch)) {

                // Forming operand, since it could be more than one digit
                operand = 10 * operand + (int) (ch - '0');

            } else if (ch == '+') {

                // Evaluate the expression to the left,
                // with result, sign, operand
                result += sign * operand;

                // Save the recently encountered '+' sign
                sign = 1;

                // Reset operand
                operand = 0;

            } else if (ch == '-') {

                result += sign * operand;
                sign = -1;
                operand = 0;

            } else if (ch == '(') {

                // Push the result and sign on to the stack, for later
                // We push the result first, then sign
                stack.push(result);
                stack.push(sign);

                // Reset operand and result, as if new evaluation begins for the new sub-expression
                sign = 1;
                result = 0;

            } else if (ch == ')') {

                // Evaluate the expression to the left
                // with result, sign and operand
                result += sign * operand;

                // ')' marks end of expression within a set of parenthesis
                // Its result is multiplied with sign on top of stack
                // as stack.pop() is the sign before the parenthesis
                result *= stack.pop();

                // Then add to the next operand on the top.
                // as stack.pop() is the result calculated before this parenthesis
                // (operand on stack) + (sign on stack * (result from parenthesis))
                result += stack.pop();

                // Reset the operand
                operand = 0;
            }
        }
        return result + (sign * operand);
    }
}
```



```java
class Solution {
    public int calculate(String s) {
        Deque<Integer> stack = new ArrayDeque<>();

        int result = 0; // 当前计算结果

        int num = 0; // 当前正在读的数字

        int sign = 1; // 当前数字的符号(1表示+, -1表示 -)
        int n = s.length();

        for (int i = 0; i < n; i++){
            char ch = s.charAt(i);

            if (Character.isDigit(ch)){
                // 构建数字(考虑多位数)
                num = 10 * num + (int)(ch - '0');
            }else if (ch == '+'){
                // 把前面的数字计算进result
                result = result + sign * num;

                sign = 1; // 下一个数是正数

                num = 0;
            }else if (ch == '-'){
                result = result + sign * num;
                sign = -1; // 下一个数是负数
                num = 0;
            }else if (ch == '('){
                // 保存当前状态(结果和符号)到栈
                stack.offerLast(result);
                stack.offerLast(sign);
                // 重置, 开始计算括号内的内容
                sign = 1;
                result = 0;
            }else if (ch == ')'){
                // 把当前num加入结果
                result = result + sign * num;
                result = result * stack.pollLast(); // 乘以括号前的括号
                result = result + stack.pollLast(); // 加上括号前的结果
                num = 0;
            }
        }

        // 处理最后一个数字
        result = result + sign * num;
        return result;
    }
}
// TC: O(n)
// SC: O(n)
```



```java
class Solution {
    public int calculate(String s) {
        Deque<Integer> stack = new ArrayDeque<>();

        int result = 0; // 当前计算结果

        int num = 0; // 当前正在读的数字

        int sign = 1; // 当前数字的符号(1表示+, -1表示 -)
        int n = s.length();

        for (int i = 0; i < n; i++){
            char ch = s.charAt(i);

            if (Character.isDigit(ch)){
                // 构建数字(考虑多位数)
                num = 10 * num + (int)(ch - '0');
            }else if (ch == '+'){
                // 把前面的数字计算进result
                result = result + sign * num;

                sign = 1; // 下一个数是正数

                num = 0;
            }else if (ch == '-'){
                result = result + sign * num;
                sign = -1; // 下一个数是负数
                num = 0;
            }else if (ch == '('){
                // 保存当前状态(结果和符号)到栈
                stack.offerLast(result);
                stack.offerLast(sign);
                // 重置, 开始计算括号内的内容
                sign = 1;
                result = 0;
            }else if (ch == ')'){
                // 把当前num加入结果
                result = result + sign * num;
                result = result * stack.pollLast(); // 乘以括号前的括号
                result = result + stack.pollLast(); // 加上括号前的结果
                num = 0;
            }
        }

        // 处理最后一个数字
        result = result + sign * num;
        return result;
    }
}
// TC: O(n)
// SC: O(n)
```

