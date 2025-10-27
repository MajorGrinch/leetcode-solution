# 8. String to Integer

根据题目要求，跳过leading zero，可能会有正负号，一直读到不是数字为止。只读取第一段连续数字，之后所有内容都可以抛弃。爆int的话就取int两边的极值。

注意，这题还有可能爆long，所以不能用long存答案，得用double！

```java
class Solution {
    public int myAtoi(String s) {
        if (s.length() == 0) {
            return 0;
        }
        boolean positive = true;
        char[] sc = s.toCharArray();
        int i = 0;
        while (i < sc.length && sc[i] == ' ') {
            // skip the leading space
            i++;
        }
        if (i == sc.length) {
            return 0;
        }
        if (sc[i] == '+') {
            i++;
        } else if (sc[i] == '-') {
            positive = false;
            i++;
        }

        double res = 0;

        while (i < sc.length && isDigit(sc[i])) {
            res = res * 10 + sc[i] - '0';
            i++;
        }

        if (positive) {
            return res > Integer.MAX_VALUE ? Integer.MAX_VALUE : (int) res;
        } else {
            res = -res;
            return res < Integer.MIN_VALUE ? Integer.MIN_VALUE : (int) res;
        }
    }

    private boolean isDigit(char c) {
        return '0' <= c && c <= '9';
    }
}
```