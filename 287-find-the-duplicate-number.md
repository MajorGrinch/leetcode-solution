# 287. Find the Duplicate Number

## HashSet Solution

很简单，用一个 HashSet 来存放已经遇到的元素。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        Set<Integer> met = new HashSet<>();
        for (int num : nums) {
            if (met.contains(num)) {
                return num;
            }
            met.add(num);
        }
        return -1;
    }
}
```

## Cycle Detection Solution

既然题目说了，有一个长度为 n+1 的数组，含有 `[1, n]`里面的数，而且只有一个数是有重复的，那么我们可以把它想象成一个 linked list。怎么想象？每一个元素的值，就是下一个元素的坐标。比如，`[2, 1, 3, 4, 2]`，就可以抽象成 `2 -> 3 -> 4 -> 2 -> previous 3`。linked list 找到环的起点，参照(leetcode 142)[142-Linked-List-Cycle-2.md]。

那么有一个问题，这个数字一定可以抽象成一个带环的链表吗？当然，因为根据题目要求，有 n + 1 个元素，他们的范围是 `[1, n]`，且只有一个数字是重复的。根据题意，要么所有数字都是同一个值，要么就是只有一个数字出现了 2 次。我们可以分类讨论一下，
+ 所有数字都是同一个值，比如 `[3, 3, 3, 3, 3, 3]`。你说可以么？
+ 只有一个数字出现了两次，其他数字都是 `[1, n]`之间。假如，数字 6 出现了 2 次，那么就会有 2 个元素指向下标为 6 的地方。我们按照链表顺序走，其中一个 6 带领我们来到了下标为 6 的地方，下标为 6 的元素会指向下一个元素，然后迟早指到另一个 6，再走回来。为什么一定会走到另一个 6？因为 `[1, n]` 之间 6 出现了两次，其他数字都出现一次，那么所有的 `[1, n]` 下标都会被访问 1 次，所以另一个 6 一定会被访问到。

Time complexity: O(N)

Space complexity: O(1)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[nums[0]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```