# 1513. Number of Substrings With Only 1s

给定一个只有01字符的字符串，返回有多少个只有1的子串。

`1`算一个，`11`算三个因为有两个`1`和一个`11`。同理`111`算6个，因为有3个`1`，2个`11`，还有1一个`111`。以此类推，返回答案。

这题怎么解？很简单，我们观察一下`111`算6个，因为有3个`1`，2个`11`，还有1一个`111`。那么也就是说当前的连续为1的子串长度的从1到最后的累加之和就是当前子串含有1的子串个数。知道这个方法了，接下来也就简单了。

不过本题需要注意答案是有可能爆int的，所以需要额外检查。


```java
class Solution {
    public int numSub(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int count = 0;
        int res = 0;
        int mod = (int) 1e9 + 7;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '1') {
                count++;
            } else {
                count = 0;
            }
            res = (res + count) % mod;
        }
        return res;
    }
}
```