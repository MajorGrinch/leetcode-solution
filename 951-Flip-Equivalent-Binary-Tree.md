# 951. Flip Equivalent Binary Trees

判断两个二叉树是否可以通过无限次将自身的左右子树对调从而变成一样的树。

其实这题和判断两个数是否相同，或者两个数是否镜面对称差不多。既然可以交换左右子树，那么判断的时候就有如下情况，
+ `root1`和`root2`都为`null`，那么当然相同。
+ 有且只有一个是`null`，那么当然不同
+ 都不为`null`的话
  + 他们两值不一样，不同。
  + 他们值一样的话，那就将对调和不对调左右孩子的情况都查看一下，看看是否能够相等


Time complexity: O(N^2)，时间复杂度这里我们好好分析一番。因为我们对于两个节点要检查四个情况，root1.left == root2.left, root1.right == root2.right, root1.left == root2.right, root1.right == root2.left。所以递归的时候，每层都会有4个分支。假设题目输入的树是平衡树，那么该平衡树会有log2(N)层，我们的递归也会进行那么多层。那么时间复杂度就是4^log2(N) = N^2。

Space complexity: O(H), where H is the height.

```java
class Solution {
    public boolean flipEquiv(TreeNode root1, TreeNode root2) {
        if(root1 == null && root2 == null){
            return true;
        }else if(root1 == null || root2 == null){
            return false;
        }else if(root1.val != root2.val){
            return false;
        }
        boolean nonFlip = flipEquiv(root1.left, root2.left) && flipEquiv(root1.right, root2.right);
        boolean flip = flipEquiv(root1.left, root2.right) && flipEquiv(root1.right, root2.left);
        return nonFlip || flip;
    }
}
```