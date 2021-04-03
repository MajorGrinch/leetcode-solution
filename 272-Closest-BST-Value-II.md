# 272. Closest Binary Search Tree Value II

给定一个BST，找到离`target`最近的`k`个值，返回List。

我们思考一下，BST有什么性质？中序遍历是升序。这样我们就可以中序遍历整个BST，在遍历途中记录用一个Queue来记录我们遇到的元素，那么这个Queue队顶到队尾是升序的。如果当前Queue的元素个数不到`k`，那么就持续把当前节点值加入Queue。如果已经有k个了，那么我们就把当前元素和队列顶部的元素比较看谁离target比较近。如果当前元素比较近，那就把队列顶部弹出，当前元素入队尾。如果队列顶部比较近，那么也就没有继续中序遍历的必要了，因为后面元素只会越来越远。

这里我们在调用的时候可以用一个`LinkedList`来传给Queue，返回的时候也返回它，因为它同时实现了Queue和List这两个接口。

Time complexity: O(N)

Space complexity: O(max(k, H)). H is height.

```java
class Solution {
  public List<Integer> closestKValues(TreeNode root, double target, int k) {
    LinkedList<Integer> res = new LinkedList<>();
    inorder(root, target, k, res);
    return res;
  }

  private void inorder(TreeNode root, double target, int k, Queue<Integer> res) {
    if (root == null) {
      return;
    }
    inorder(root.left, target, k, res);
    if (res.size() < k) {
      res.offer(root.val);
    } else {
      int val = root.val;
      if (Math.abs(val - target) < Math.abs(res.peek() - target)) {
        res.poll();
        res.offer(val);
      } else {
        return;
      }
    }
    inorder(root.right, target, k, res);
  }
}
```