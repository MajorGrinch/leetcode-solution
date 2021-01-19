# 50. Pow(x, n)

## Solution 1

经典的分治思想。

n是偶数的话，`x^n` 可以由 `x^(n/2) * x^(n/2)` 计算得出。`n` 是奇数的话，`x^n` 则由 `x^(n/2) * x^(n/2) * x` 所得。

Corner case:
+ n 是负数怎么办？转化为正数，最后被1除。
+ 注意 n 是 `-2147483648`，int的范围是 `-2147483648 ~ 2147483647`。

```java
class Solution {
    public double myPow(double x, int n) {
        if(n >= 0){
            return pow(x, n);
        }else{
            long longN = n;
            return 1 / pow(x, -longN);
        }
    }

    private double pow(double x, long n){
        if(n == 0){
            return 1.0;
        }
        double halfRes = pow(x, n / 2);
        if(n % 2 == 1){
            return halfRes * halfRes * x;
        }else{
            return halfRes * halfRes;
        }
    }
}
```

## Solution 2

利用二进制的思路，如果题目明确禁止使用分治思想来解决，则可以使用这个思路。

把 `n` 想成二进制，比如 `5^10` == `5^b1010` == `5^b1000` * `5^b0010`。

这样我们可以把 5^10 用两个5的2的整数幂次方相乘来表示。也就是任意的 x^n，我都可以用若干个x的2的整数幂次方相乘来表示。

设置一个 accumulator 来记录5的2的整数幂次方，比如 5^1, 5^2, 5^4, 5^8, 5^16 等等。那么最后 5^n 一定可以通过这些5的2的整数幂次方相乘得到。

那么`5^n`具体是哪些5的2的整数幂次方相乘得到的呢？accumulator初始化为 `5^1`，prodduct初始化为`1.0 == 5^0`。

不断循环到n的二进制没有1为止，也就是n == 0。当前轮我们取n的最低位，看看是不是1。是1的话说明这里是一个5的2的整数幂次方，所以需要乘以accumulator。如果不是1的话，说明这里不是，则不需要累乘。做完这些操作之后，对accumulator进行累乘，也就是平方，这样 `5^1`才能变成`5^2`,`5^4`等等。同时对n进行右移操作，更新最低位直至n == 0。

```java
class Solution2 {
    public double myPow(double x, int n) {
        double product = 1.0;
        double accumulator = x;
        long N = n;
        N = Math.abs(N);
        while(N > 0){
            if(N % 2 == 1){
                product *= accumulator;
            }
            accumulator *= accumulator;
            N = N >> 1;
        }
        if(n >=0){
            return product;
        }else{
            return 1 / product;
        }
    }
}
```