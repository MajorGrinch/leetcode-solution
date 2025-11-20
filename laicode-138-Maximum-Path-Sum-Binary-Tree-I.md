# Laicode. 138. Maximum Path Sum Binary Tree I

给定一个二叉树，得到最大的从一个叶子节点到另一个叶子节点的路径节点和。

那么针对这个二叉树的递归，我们还是思考三件事情：
+ 我希望左右孩子给我什么？给出他们到他们底下叶子节点最大的路径。
+ 我当前层需要做什么？算出是否有经过当前节点的最大路径和
+ 我需要向上返回什么？返回我到达底下叶子节点的最大路径。

搞清楚这三件事情之后，这题就很好实现了。globalMax来记录全局最大路径和，初始化为int最小值。我们用`maxPathToLeaf`来计算当前`root`到底下叶子节点的最大路径和，并且在途中顺手计算是否有经过当前`root`的路径可以替代globalMax。要想取代目前的globalMax，仅仅路径和大事不够的，还得满足当前节点不能是叶子节点，以及它左右孩子都不能为空。

`maxPathToLeaf`在返回的时候，需要返回的当前节点到底下叶子节点的最大路径和。如果左孩子为空，那么返回右孩子那边传回来的路径和外加自己。如果左孩子不为空但是右孩子为空，那么就返回左孩子那里传回来的路径和外加自己。如果左右孩子都不空，选大的那个外加自己返回给上层。

`maxPathSum`里对整个树的`root`进行调用，这样就可以求出整个树里的最大路径和。

Time complexity: O(N). We only access each node once.

Space complexity: O(H). H is the height.

```java
public class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = new int[] {Integer.MIN_VALUE};
    maxPathToLeaf(root, res);
    return res[0];
  }

  private int maxPathToLeaf(TreeNode root, int[] globalMax) {
    if (root == null) return 0;
    int leftMaxToLeaf = maxPathToLeaf(root.left, globalMax);
    int rightMaxToLeaf = maxPathToLeaf(root.right, globalMax);
    int currMaxPathSum = leftMaxToLeaf + root.key + rightMaxToLeaf;
    if (root.left != null && root.right != null && currMaxPathSum > globalMax[0]) {
      globalMax[0] = currMaxPathSum;
    }
    if (root.left == null) {
      return root.key + rightMaxToLeaf;
    } else if (root.right == null) {
      return root.key + leftMaxToLeaf;
    } else {
      return Math.max(leftMaxToLeaf, rightMaxToLeaf) + root.key;
    }
  }
}
```

也可以这样写

```java
public class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = {Integer.MIN_VALUE};
    helper(root, res);
    return res[0];
  }

  private int helper(TreeNode node, int[] res) {
    if(node == null) {
      return 0;
    }
    int left = helper(node.left, res);
    int right = helper(node.right, res);
    if(node.left != null && node.right != null) {
      res[0] = Math.max(res[0], left + right + node.key);
      return Math.max(left, right) + node.key;
    }
    return node.left == null ? right + node.key : left + node.key;
  }
}
```