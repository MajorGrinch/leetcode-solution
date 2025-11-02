# 437. Path Sum III

给定一个二叉树，找出所有上下走向、路径和等于 targetSum 的路径，并且返回个数。

这题需要用前缀和的概念来解决。二叉树从根节点到每一个叶子节点形成的路径，我们都可以看作为一个数组。假如一个二叉树有 10 个叶子节点，那么就有10 个路径形成的数组。在这 10 个数组里找到子数组，他们的和是 targetSum 就可以。怎么样？是不是瞬间明白问题的本质了？

我们自顶向下遍历二叉树，沿途记录目前的路径和 currSum，以及所有路径和出现的次数prefixSumMap。对于每一层处理的时候，还是经典三个问题：
+ 我当前层需要做什么？以当前节点为终点的子数组，有几个和是 targetSum？
+ 我希望左右孩子给我什么？以他们里面节点为终点的子数组，有多少个和是 targetSum，返回给我。
+ 我向上返回什么？以我这个子树里任意节点为终点的子数组，有多少个和是 targetSum，返回上去。

沿途记录前缀和出现的次数，这样子就方便我们计算以当前节点为终点、和为 targetSum 的子数组个数。

Time complexity: O(N)

Space complexity: O(N)

```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> prefixSumMap = new HashMap<>();
        prefixSumMap.put(0L, 1);
        return helper(root, 0, targetSum, prefixSumMap);
    }

    private int helper(TreeNode node, long currSum, int targetSum, Map<Long, Integer> prefixSumMap) {
        if (node == null) {
            return 0;
        }
        currSum += node.val;
        int res = prefixSumMap.getOrDefault(currSum - targetSum, 0);
        prefixSumMap.put(currSum, prefixSumMap.getOrDefault(currSum, 0) + 1);
        res += helper(node.left, currSum, targetSum, prefixSumMap)
                + helper(node.right, currSum, targetSum, prefixSumMap);
        prefixSumMap.put(currSum, prefixSumMap.getOrDefault(currSum, 0) - 1);
        return res;
    }
}
```