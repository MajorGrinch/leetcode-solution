# 938. Range Sum of BST

给定一个BST，再给定一个范围，求出这个BST在这个闭区间范围内所有节点值的和。

那其实就是遍历一下整个BST，途中遇到符合条件的就记录下来。思考三件事情，
+ 我希望左右子树返回什么？我希望他们返回他们子树内符合条件的节点值的和。
+ 当前节点要干什么？我希望得到以我为根的子树所有满足条件的节点值之和。当前节点在区间内，就计入，不在就不计入。
+ 我需要向上返回什么？我要返回以我为根的子树所有满足条件的节点值之和。

Time complexity: O(N)

Space complexity: O(H)

```java
class Solution {
    public int rangeSumBST(TreeNode root, int low, int high) {
        if(root == null){
            return 0;
        }
        int sum = 0;
        if(low <= root.val && root.val <= high){
            sum = root.val;
        }
        return sum + rangeSumBST(root.left, low, high) + rangeSumBST(root.right, low, high);
    }
}
```