# 84. Largest Rectangle in Histogram

给定一个柱状图，求出里面最大的矩形面积。

直观上来说，为了求出这个柱状图里最大的矩形面积，我们需要遍历每一个柱形然后向左向右找到比自己矮的柱形作为边界来求出面积。这样子，我们最后需要O(n^2)的复杂度可以求出其中最大的矩形。

有没有更好的思路呢？想想看，我们在向左向右寻找比自己矮的柱形时，这个步骤可以被优化一下。我们用一个栈来存储横轴坐标，然后从左向右遍历横轴坐标，每次都要把当前的坐标压栈。但是呢，我们要维护一个性质，栈内自栈底到栈顶的所有坐标对应的柱形是呈严格升序的，没有重复高度。这样做有什么好处呢？这样子，每当被较矮的柱形截断时，我们可以及时计算好之前部分里面的最大矩形。

如何计算之前部分里最大矩形？不断地取栈顶坐标和当前坐标比。如果当前柱形不比栈顶坐标对应的柱形高的话，那么我们就需要弹出栈顶坐标并且计算以这个柱形为“中心”的最大矩形面积。我给“中心”打上引号，因为这个柱形并不一定真的是它对应矩形的正中间。该矩形左边界在哪儿？弹出后新的栈顶坐标就是左边界，因为栈顶坐标对应的柱形肯定比它矮。右边界在哪儿？右边界就是当前柱形，因为当前柱形也不比这个柱形高。所以以这个柱形为“中心”的最大矩形的宽就可以通过`i - stack.peek() - 1`计算出来了。

那如果当前柱形等于栈顶柱形咋办？宽度不就少算了么？不担心，当前坐标入栈之后，以后肯定还会把它弹出来然后计算以它为中心的最大矩形面积的，到时候不就算出来了。

栈内先放个-1来初始化左边界。需要注意的是，遍历完柱形图之后如果栈里面还有多余元素的话，我们还要接着重复弹出并计算的过程。

Time complexity: O(n) because each index only get pushed and poped once.

Space complexity: O(n).

```java
class Solution {
  public int largestRectangleArea(int[] heights) {
    if (heights == null || heights.length == 0) {
      return 0;
    }
    Deque<Integer> ascendingIndices = new ArrayDeque<>();
    ascendingIndices.push(-1);
    int maxArea = 0;
    for (int i = 0; i < heights.length; i++) {
      while (ascendingIndices.peek() != -1 && heights[ascendingIndices.peek()] >= heights[i]) {
        int h = heights[ascendingIndices.pop()];
        int width = i - ascendingIndices.peek() - 1;
        int currArea = h * width;
        maxArea = Math.max(maxArea, currArea);
      }
      ascendingIndices.push(i);
    }
    while (ascendingIndices.peek() != -1) {
      int h = heights[ascendingIndices.pop()];
      int width = heights.length - ascendingIndices.peek() - 1;
      maxArea = Math.max(maxArea, h * width);
    }
    return maxArea;
  }
}
```

2021年写的题解不知道叽里咕噜的在说什么，这里重新解释一下。

我们遍历每一个 i，然后每一个 i 都需要入栈。但是，这个 stack 保持一个性质，从栈底到栈顶的 index 对应的高度，保持升序。如果当前栈顶 index 对应的高度高于当前 i 的高度，那么我们需要不断弹出直到低于 i 的高度为止。那么这个性质会带来一个好处，是什么呢？stack 里面相邻的两个元素对应的坐标 i1 和 i2，height[i1] < height[i2]，并且任意 (i1, i2) 之间的坐标 `i`都满足 height[i] >= height[i2]。

那么我们遍历 i 的时候，就有两种情况
1. 当前的 height[i] 比栈顶的 index 对应的高度要高，性质不会被破坏，i 正常入栈。
2. 当前的 height[i] 比栈顶的 index 对应的高度要矮，性质会被破坏，我们要弹出所有不矮于 height[i] 的坐标。每当弹出栈顶的时候，以栈顶 index 对应的高度为高的矩形，右边界是当前的 i，左边界是次栈顶对应的 index，both exclusive。计算对应的矩形面积并且试图更新 global max。

自带的数组需要进行扩充，左右各放置一个边界。左边边界高度为-1，右边边界高度为 0。为什么左边界要为-1 呢？因为要比右边界更低，这样不会左边界就不会从栈里面弹出去了。

2025 code:

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] histogram = new int[heights.length + 2];
        histogram[0] = -1;
        for (int i = 0; i < heights.length; i++) {
            histogram[i + 1] = heights[i];
        }
        histogram[histogram.length - 1] = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        int res = 0;
        for (int i = 0; i < histogram.length; i++) {
            while (!stack.isEmpty() && histogram[stack.peek()] >= histogram[i]) {
                int h = histogram[stack.pop()];
                int l = i - 1 - stack.peek();
                res = Math.max(res, h * l);
            }
            stack.push(i);
        }
        return res;
    }
}
```