# 150. Evaluate Reverse Polish Notation

给定一个数组的token记录着逆波兰式，我们要计算这个逆波兰式的结果。这就是遇到操作符就弹出操作数，计算出结果压回去。遇到操作数，就入栈。很简单。

不过我们要注意的是，操作数有可能是负数，所以如何判定一个`token`是操作符还是数字呢？那么`token.length() == 1 && Character.isDigit(token.charAt(0))`就可以推断出`token`是操作符。那么反过来，就可以推断出这是个数字。

Time complexity: O(n)

Space complexity: O(n)

```java
class Solution {
  public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String token : tokens) {
      char ch = token.charAt(0);
      if (token.length() > 1 || Character.isDigit(ch)) {
        stack.push(Integer.valueOf(token));
        continue;
      }
      int op2 = stack.pop(), op1 = stack.pop();
      switch (ch) {
        case '+':
          stack.push(op1 + op2);
          break;
        case '-':
          stack.push(op1 - op2);
          break;
        case '*':
          stack.push(op1 * op2);
          break;
        case '/':
          stack.push(op1 / op2);
          break;
        default:
          break;
      }
    }
    return stack.peek();
  }
}
```