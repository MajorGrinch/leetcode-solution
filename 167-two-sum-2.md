# Laicode 167. Two Sum II - Input Array Is Sorted

还是 two sum 问题，但是给定的数组是有序的。那就两个指针 l 和 r，分别从两头往中间走。他们的和小于 target 就 l++，大于 target 就 r--。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0;
        int r = numbers.length - 1;
        while(l < r) {
            int sum = numbers[l] + numbers[r];
            if(sum == target) {
                return new int[] {l + 1, r + 1};
            } else if(sum < target) {
                l++;
            } else {
                r--;
            }
        }
        return new int[] {-1, -1};
    }
}
```