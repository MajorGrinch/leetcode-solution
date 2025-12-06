# 744. Find Smallest Letter Greater Than Target

二分搜索逼近答案。loop invariant是，第一个比target大的字母一定在[l, r]区间里，那么我们首先要排除一个情况就是数组里没有元素比 target 大。排除掉这个元素之后，再进行二分搜索。

如果letters[mid] <= target，那么第一个比target大的字母肯定在[mid + 1, r]。如果letter[mid] > target，那么第一个比target大的字母肯定在[l, mid]。如此维护循环不变量。

最后[l, r]区间逼近到长度为1，直接返回 letters[l]就行了。为什么？因为数组里一定存在，那么最后l 和 r 停在的那个元素就是答案。

Time complexity: O(logN)

Space complexity: O(1)

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        if(letters == null){
            return ' ';
        }
        return firstGreaterThan(letters, target);
    }

    private char firstGreaterThan(char[] letters, char target){
        int l = 0, r = letters.length - 1;
        while(l < r){
            int mid = l + (r-l >>1);
            if(letters[mid] <= target){
                l = mid + 1;
            }else{
                r = mid;
            }
        }
        return letters[l] > target ? letters[l] : letters[0];
    }
}
```

2025:

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int n = letters.length;
        if (letters[n - 1] <= target) {
            return letters[0];
        }
        int l = 0;
        int r = n - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (letters[mid] > target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return letters[l];
    }
}
```