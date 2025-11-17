# 270. Closest Binary Search Tree Value

给定一个BST和一个target值，找到离`target`最近的值。如果有两个值离 target 一样近，选小的那个。

那么其实就是在BST里找`target`，如果能找到那就返回这个值。如果找不到，在途中记录离`target`最近的值最后返回就好。需要注意的是，如果有两个值离 target 一样近，那我们需要选小的那个。

Time complexity: O(H). H is the height.

Space complexity: O(1).

```java
class Solution {
    public int closestValue(TreeNode root, double target) {
        int res = root.val;
        while (root != null) {
            if (Math.abs(root.val - target) < Math.abs(res - target)) {
                res = root.val;
            } else if(Math.abs(root.val - target) == Math.abs(res - target) && root.val < res) {
                res = root.val;
            }
            if (root.val == target) {
                break;
            } else if (root.val < target) {
                root = root.right;
            } else {
                root = root.left;
            }
        }
        return res;
    }
}
```