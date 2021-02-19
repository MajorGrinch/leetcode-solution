# 268. Missing Number

给定一个长度为`n`的数组含有数字`[0, n]`的无重复元素数组，返回数组中缺少的那个属于`[0, n]`的元素。

## HashSet Approach

用一个HashSet来盛放数组里的所有元素，然后从[0, n]依次检查，谁不在HashSet里就返回谁。插入集合的平均复杂度是O(N)，最糟复杂度是O(N^2)。依次检查的平均复杂度是O(N)，最糟复杂度是O(N^2)。

Time complexity: O(N) in average, O(N^2) in worst case.

Space complexity: O(N) due to the hash set.

```java
class Solution {
  public int missingNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
      return -1;
    }
    Set<Integer> numSet = new HashSet<>();
    for (int n : nums) {
      numSet.add(n);
    }
    for (int i = 0; i <= nums.length; i++) {
      if (!numSet.contains(i)) {
        return i;
      }
    }
    return -1;
  }
}
```

## Array Approach

除了用HashSet来记录数字出现的频率外，我们可以用一个数组来记录对应[0, n]的出现次数。题目给的是数字范围是[0, n]，那么我们就开个长度为`n + 1`的数组来记录`[0, n]`数字出现的频率，这个其实就是个简易的HashSet。这个简易的HashSet没有最糟情况，插入和查询单个元素复杂度永远是O(1)。

Time complexity: O(N)

Space complexity: O(N) because of the array.

```java
class Solution {
  public int missingNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
      return -1;
    }
    int[] freq = new int[nums.length + 1];
    for (int n : nums) freq[n]++;
    for (int i = 0; i <= nums.length; i++) {
      if (freq[i] == 0) {
        return i;
      }
    }
    return -1;
  }
}
```

## Arithmetic Sequence Approach

`[0, n]`所有数字的和，根据等差数列的计算公式是`(n+1) * n / 2`。我们再算出个实际的和是多少，相减就可以得到缺的那个数了。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
  public int missingNumber(int[] nums) {
    int n = nums.length;
    int expectSum = (n * (1 + n)) / 2;
    int realSum = 0;
    for (int num : nums) realSum += num;
    return expectSum - realSum;
  }
}
```

## Bitwise operation Approach

巧用位运算，利用异或操作。我们都知道 x ^ x = 0，既然数组涵盖了`[0, n]`里的n个元素，那么让`[0, n]`所有的`n + 1`个元素全部和数组里的n个元素异或一下，最后缺的那个元素就显现出来了。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
  public int missingNumber(int[] nums) {
    int res = nums.length;
    for (int i = 0; i < nums.length; i++) {
      res ^= i ^ nums[i];
    }
    return res;
  }
}
```