# 744. Find Smallest Letter Greater Than Target

二分搜索逼近答案。loop invariant是，第一个比target大的字母一定在[l, r]区间里。

如果letters[mid] <= target，那么第一个比target大的字母肯定在[mid + 1, r]。如果letter[mid] > target，那么第一个比target大的字母肯定在[l, mid]。如此维护循环不变量。

最后[l, r]区间逼近到长度为1或者0，直接看letters[l]是不是比target大，是的话就返回。不是的话，根据题意返回首字母。

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