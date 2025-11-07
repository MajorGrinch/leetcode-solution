# 1740. Find Distance in a Binary Tree

给定一个二叉树，计算两个点之间的距离。

constraints:
+ 二叉树非空且无重复值
+ 这两个点肯定存在

## Approach 1. Find LCA and get distance separately

我们可以先找 LCA，然后从 LCA 分别计算和这两个点的距离，相加就可以。找 LCA 可以参考[Leetcode 236](236-LCA.md)。至于计算距离，我们在遍历的时候还是三件事：
+ 我希望左右孩子返回给我什么？他们离给定值的距离，如果他们里面没有给定值就返回 -1。
+ 我当前层需要做什么？计算当前节点和 val 的距离。如果自己就是 val，那直接返回 0。如果左右孩子都返回 -1，说明 val 不在我这个子树里，那么距离自然也就是 -1。如果任意孩子返回非-1，那么我就需要根据返回的结果加 1 作为我和val 的距离。
+ 我需要向上返回什么？当然就是当前节点和 val 的距离。

Time complexity: O(N)，要遍历 3 次二叉树。

Space complexity: O(H)，H 是二叉树的层数，由此引来的调用栈。

```java
class Solution {
    public int findDistance(TreeNode root, int p, int q) {
        TreeNode lca = findLCA(root, p, q);
        int distance1 = getDistance(lca, p);
        int distance2 = getDistance(lca, q);
        return distance1 + distance2;
    }

    private int getDistance(TreeNode node, int val) {
        if (node == null) {
            return -1;
        }
        if (node.val == val) {
            return 0;
        }
        int left = getDistance(node.left, val);
        int right = getDistance(node.right, val);
        if (left == -1 && right == -1) {
            return -1;
        }
        return Math.max(left, right) + 1;
    }

    private TreeNode findLCA(TreeNode node, int p, int q) {
        if (node == null || node.val == p || node.val == q) {
            return node;
        }
        TreeNode left = findLCA(node.left, p, q);
        TreeNode right = findLCA(node.right, p, q);
        if (left != null && right != null) {
            return node;
        }
        return left == null ? right : left;
    }
}
```

## Approach 2. Find LCA and record depth in a single traversal

上述方法需要遍历二叉树 3 次，那么我们能不能简单一点？能，一边遍历，一边记录节点的深度，方便回头计算。想想看，假如 LCA 在第 3 层，p 在 5 层，然后 q 在 8 层，那么很显然 pq 的距离就是(5 - 4) + (8 - 3)。

我们用一个变量 steps 来记录当前节点的深度，另外用一个三元组 threeDepth 分别记录 LCA 的深度，p 的深度和 q 的深度。

那么我们在遍历的时候需要思考这三件事情：
+ 我希望左右孩子给我什么？当然还是他们里面是否找到 p 或 q。
+ 我当前层需要做什么？计算 LCA 的候选节点并且记录深度。如果当前节点是 p 或者 q 的话，那么当前节点必然是LCA候选节点。但是我们不着急返回，我们先记录深度再继续向左右孩子探索。为什么？因为我们需要找到 p 和 q 各自的深度。等探索完左右孩子了，如果当前节点是 p 或 q 的话，直接返回当前节点作为候选节点。如果当前节点非 p 非 q，那么就看左右孩子。如果左右孩子都各自返回非空的候选节点，那么当前节点也还是候选节点，并且更新 LCA 的深度。如果左右孩子任意一个返回了空结果，那么就正常按照 LCA 的逻辑来处理。
+ 我需要向上返回什么？LCA 候选节点。

当 p 或者 q 任意一个是 LCA 的时候，LCA的深度就不会被记录。所以最后计算距离的时候，我们需要额外的判定一下。这样我们只遍历一次整个树，就找到了距离。

```java
class Solution {
    public int findDistance(TreeNode root, int p, int q) {
        int[] depth = { -1, -1, -1 };
        findLCA(root, p, q, 0, depth);
        if (depth[0] == -1) {
            // p or q is LCA
            return Math.abs(depth[1] - depth[2]);
        } else {
            // another node is LCA
            return depth[1] + depth[2] - 2 * depth[0];
        }
    }

    private TreeNode findLCA(TreeNode node, int p, int q, int steps, int[] threeDepth) {
        if (node == null) {
            return null;
        }
        if (node.val == p) {
            threeDepth[1] = steps;
        }
        if (node.val == q) {
            threeDepth[2] = steps;
        }
        TreeNode left = findLCA(node.left, p, q, steps + 1, threeDepth);
        TreeNode right = findLCA(node.right, p, q, steps + 1, threeDepth);
        if (node.val == p || node.val == q) {
            return node;
        }
        if (left != null && right != null) {
            threeDepth[0] = steps;
            return node;
        }
        return left == null ? right : left;
    }
}
```