# 1. Two Sum

Two Sum没必要解释了吧。。。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> idxMap = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
      int anotherIdx = idxMap.getOrDefault(target - nums[i], -1);
      if (anotherIdx != -1) {
        return new int[] {anotherIdx, i};
      }
      idxMap.put(nums[i], i);
    }
    return new int[] {-1, -1};
  }
}
```