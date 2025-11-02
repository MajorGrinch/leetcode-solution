# 113. Path Sum II

给定一个二叉树，找出所有root-to-leaf路径和等于 targetSum 的路径，并且返回。

这次和[112. Path Sum](112-path-sum.md)是一样的，只不过需要记录所有的路径。

那么针对这个二叉树的递归，我们还是思考三件事情：
+ 我希望左右孩子给我什么？不需要给我什么，我只想向左右孩子搜寻。
+ 我当前层需要做什么？判断当前node 是否是叶子节点，
  + 是叶子节点的话，就看当前node 值是否等于剩余的 targetSum。如果等于的话，那这个叶子节点到 root 的路径就是一个答案，我们记录下来。如果不等，那就不用记录，直接返回。
  + 不是叶子节点的话，就看它有没有到叶子节点的路径和等于 targetSum - node.val 的。
+ 我需要向上返回什么？正如我不需要左右孩子给我什么，我也不会向上返回什么。退出前记得把沿途记录的path的末尾元素给去掉。

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        pathSum(root, new ArrayList<>(), targetSum, res);
        return res;
    }

    private void pathSum(TreeNode node, List<Integer> path, int target, List<List<Integer>> res) {
        path.add(node.val);
        if (isLeaf(node)) {
            if (target == node.val) {
                res.add(new ArrayList<>(path));
            }
            path.remove(path.size() - 1);
            return;
        }

        target -= node.val;
        if (node.left != null) {
            pathSum(node.left, path, target, res);
        }
        if (node.right != null) {
            pathSum(node.right, path, target, res);
        }
        path.remove(path.size() - 1);
    }

    private boolean isLeaf(TreeNode node) {
        return node.left == null && node.right == null;
    }
}
```