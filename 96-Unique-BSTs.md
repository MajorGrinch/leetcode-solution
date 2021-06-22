# 96. Unique Binary Search Trees

## Recurisve Approach

给定一个数字`n`，问`n`个节点最多可以生成多少个不同的BST。

那么这题我们可以先把大问题划成子问题再去解决。

base case是`n`小于等于1的时候，最多只能生成1个BST。

一般情况下，假设这些节点编号`1` ~ `n`，`n`个节点不同BST的数量为`f(n)`。如何求出`f(n)`呢？那么我取出任意节点`x`，`x`左边有`x - 1`个节点，`x`右边有`n - x`个节点。他们各自有对应数量的BST，`f(x - 1)`和`f(n - x)`。那么对于`x`作为根节点的情况，就有`f(x - 1) * f(n - x)`种可能。那么我们枚举`1 ~ n`所有节点作为根节点的情况，对他们左右子树情况进行计算并向下递归。

Time complexity: O(n!)

Space complexity: O(n)

```java
class Solution {
  public int numTrees(int n) {
    if (n <= 1) {
      return 1;
    }
    int num = 0;
    for (int leftNodes = 1; leftNodes <= n; leftNodes++) {
      num += numTrees(leftNodes - 1) * numTrees(n - leftNodes);
    }
    return num;
  }
}
```

## Dynamic Programming Approach

上述递归过程包含了大量的重复计算，比如`f(3)`可能就被重复计算了很多次，所以我们用动态规划来解决重复计算这一问题。

`numBST[i]`表示`i`个节点可以生成多少个不同的BST，那么很显然base case`numBST[0] = 1`。我们要从节点数1开始一直算到`n`，每轮循环里做的事情和递归方法里一样，都是对每个节点做根节点的情况进行累加。但是因为之前的`numBST[0...i)`都已经算出来了，所以我们就可以直接调用答案。

Time complexity: O(n^2)

Space complexity: O(n)

```java
class Solution {
  public int numTrees(int n) {
    if (n <= 1) {
      return 1;
    }
    int[] numBST = new int[n + 1];
    numBST[0] = 1;
    for (int i = 1; i <= n; i++) {
      int num = 0;
      for (int j = 1; j <= i; j++) {
        num += numBST[j - 1] * numBST[i - j];
      }
      numBST[i] = num;
    }
    return numBST[n];
  }
}

```