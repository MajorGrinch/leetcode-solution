# Laicode 50. Tweaked Identical Binary Trees

判断两个二叉树是否可以通过无限次将自身的左右子树对调从而变成一样的树，同[leetcode 951](951-Flip-Equivalent-Binary-Tree.md)


```java
public class Solution {
    public boolean isTweakedIdentical(TreeNode one, TreeNode two) {
        if(one == null && two == null){
            return true;
        }else if(one == null || two == null){
            return false;
        }else if(one.key != two.key){
            return false;
        }
        return (isTweakedIdentical(one.left, two.left) && isTweakedIdentical(one.right, two.right)) ||
            (isTweakedIdentical(one.left, two.right) && isTweakedIdentical(one.right, two.left));
    }
}
```