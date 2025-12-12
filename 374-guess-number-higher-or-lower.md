# 374. Guess Number Higher or Lower

二分法猜数字，这个太经典了，没啥好说的。

Time complexity: O(logN)

Space complexity: O(1)

```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int l = 1;
        int r = n;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            int cmp = guess(mid);
            if (cmp == 0) {
                return mid;
            } else if (cmp < 0) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return -1;
    }
}
```