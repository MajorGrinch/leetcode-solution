# 103. Binary Tree Zigzag Level Order Traversal

这个二叉树Zigzag的遍历，需要用到双端队列，以及一个方向标记。

我们用`towardsRight`来表示当前层是否是向右输出，然后用一个双端队列来存放每一层的元素。存放顺序都是从左到右，只不过分情况讨论，
+ 如果`towardsRight`为`true`的话，表示向右输出。那么我们就不断地从队列头部拿取元素放入当前层的list里，左右非空孩子依次放入队尾。
+ 如果`towardsRight`为`false`的话，表示向左输出。那么我们就不断地从队列尾部拿取元素放入当前层的list里，右左非空孩子依次放入队头。

每层结束之后，把当前层的list放入答案，并且改变方向。最后就可以得到zigzag level order。

Time complexity: O(N)

Space complexity: O(N), because the deque size could be up to N/2.

```java
class Solution {
  public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
      return res;
    }
    Deque<TreeNode> deque = new ArrayDeque<>();
    deque.offer(root);
    boolean towardsRight = true;
    while (!deque.isEmpty()) {
      List<Integer> levelList = new ArrayList<>();
      int size = deque.size();
      if (towardsRight) {
        for (int i = 0; i < size; i++) {
          TreeNode curr = deque.pollFirst();
          levelList.add(curr.val);
          if (curr.left != null) {
            deque.offerLast(curr.left);
          }
          if (curr.right != null) {
            deque.offerLast(curr.right);
          }
        }
      } else {
        for (int i = 0; i < size; i++) {
          TreeNode curr = deque.pollLast();
          levelList.add(curr.val);
          if (curr.right != null) {
            deque.offerFirst(curr.right);
          }
          if (curr.left != null) {
            deque.offerFirst(curr.left);
          }
        }
      }
      towardsRight = !towardsRight;
      res.add(levelList);
    }
    return res;
  }
}
```