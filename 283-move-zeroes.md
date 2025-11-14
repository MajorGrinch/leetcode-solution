# 283. Move Zeroes

给定一个数组，要求把所有 0 元素移到最后，非 0 元素保持相对位置不变。

其实很简单，设置一个挡板 `l`，物理意义是 `[0, l)` 都是非 0 元素。对数组遍历一次，遇到非 0 元素就放入 `[0, l)`范围内。结束后，`[l, len)`全放 0 就可以。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int l = 0;
        for (int n : nums) {
            if (n != 0) {
                nums[l++] = n;
            }
        }
        while (l < nums.length) {
            nums[l++] = 0;
        }
    }
}
```