# 229. Majority Element II

和[169. Majority Element](169-Majority-Element.md)类似，只不过这要求次数超过1/3的。

首先我们可以得知，这个数组里最多只有两个这样的数。那么我们就用两个变量`candidate1`和`candidate2`来记录两个候选数字，以及`count1`和`count2`来记录他们各自出现的次数。

我们继续沿用voting algorithm，
+ 如果目前 `nums[i]` 是任意 candidate，那么对应的次数就 +1。
+ 如果目前 `nums[i]` 不是任何 candidate，那就再看如果目前`count1`或者`count2`是否为0，是的话就重置对应的候选数字为当前数字。
+ 如果不满足以上两个条件，即 `nums[i]` 不是任何 candidate，目前的 candidate 次数也都不为 0，那么两个候选数字的count一起自减。为什么要两个count都自减呢？因为这样子我们相当于从数组中去除了一个三元组，我们知道这三元组组成的数组没有Majority Number。所以从数组里去掉这个三元组，对最后的答案没有影响。

这题有两个地方需要注意，
1. 这题不保证输入的数组里有Majority Number，所以遍历完一次之后，我们对两个候选数字还再要检查一下。
2. 初始化两个 candidate 需要确保两个 candidate 不一样。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if (nums.length == 0) {
            return res;
        }
        if (nums.length == 1) {
            res.add(nums[0]);
            return res;
        }
        int candidate1 = nums[0];
        int count1 = 1;
        int candidate2 = 0;
        int count2 = 0;
        int i = 1;
        while (i < nums.length) {
            if (nums[i] == candidate1) {
                count1++;
                i++;
            } else {
                candidate2 = nums[i];
                count2 = 1;
                i++;
                break;
            }
        }
        while (i < nums.length) {
            if (nums[i] == candidate1) {
                count1++;
            } else if (nums[i] == candidate2) {
                count2++;
            } else if (count1 == 0) {
                candidate1 = nums[i];
                count1++;
            } else if (count2 == 0) {
                candidate2 = nums[i];
                count2++;
            } else {
                count1--;
                count2--;
            }
            i++;
        }
        count1 = 0;
        count2 = 0;
        for (int num : nums) {
            if (num == candidate1) {
                count1++;
            } else if (num == candidate2) {
                count2++;
            }
        }
        if (count1 > nums.length / 3) {
            res.add(candidate1);
        }
        if (count2 > nums.length / 3) {
            res.add(candidate2);
        }
        return res;
    }
}
```