# Laicode 644. Common Elements In K Sorted Lists

给定k个升序的`List<Integer>`，求出他们共同的元素。

这题看上去好像有什么巧妙的方法，其实没有。就是简单的从输入的所有List里取出第一个，然后和剩下的每一个进行求共同元素。

具体过程是，第一个先和第二个求共同元素得到一个列表，这个列表再和第三个去求共同元素，然后继续。。。这样我们就可以得到所有List的共同元素了。

Time complexity: O(N) where N is the total number of elements in all lists.

Space complexity: O(N) in worst case.

```java
public class Solution {
  public List<Integer> commonElementsInKSortedArrays(List<List<Integer>> input) {
    if (input.size() == 0) return new ArrayList<>();
    List<Integer> res = input.get(0);
    for (int i = 1; i < input.size(); i++) {
      res = commonOfTwo(res, input.get(i));
    }
    return res;
  }

  private List<Integer> commonOfTwo(List<Integer> l1, List<Integer> l2) {
    List<Integer> res = new ArrayList<>();
    if (l1.size() == 0 || l2.size() == 0) {
      return res;
    }
    int i = 0, j = 0;
    while (i < l1.size() && j < l2.size()) {
      int cmp = Integer.compare(l1.get(i), l2.get(j));
      if (cmp == 0) {
        res.add(l1.get(i));
        i++;
        j++;
      } else if (cmp < 0) {
        i++;
      } else {
        j++;
      }
    }
    return res;
  }
}
```

## Save space

之前方法的空间复杂度有点高，我们有没有方法能把空间复杂度压下来呢？有，重复利用List。

有一个隐藏条件，我之前没有说出来——那就是最后共有元素的个数肯定不会超过任意List的长度。所以我们复用输入里的第一个List来放答案，每轮循环的共有元素都放在第一个List里，肯定够放的。

而`commonOfTwo`方法也要改一改，共有元素肯定不会比l1多，而且产生的进度也不可能大于l1对应的`i`指针进度。所以我们可以把共同答案放入l1，并且返回共同答案的个数。这样下一轮我知道l1该到哪儿停止，因为后面的元素都是无用的。

这样空间复杂度就被降至常量级了。

```java
public class Solution {
  public List<Integer> commonElementsInKSortedArrays(List<List<Integer>> input) {
    if (input.size() == 0) return new ArrayList<>();
    List<Integer> res = input.get(0);
    int resLen = res.size();
    for (int i = 0; i < input.size(); i++) {
      resLen = commonOfTwo(res, resLen, input.get(i));
    }
    return res.subList(0, resLen);
  }

  private int commonOfTwo(List<Integer> l1, int l1Size, List<Integer> l2) {
    if (l1.size() == 0 || l2.size() == 0) {
      return 0;
    }
    int i = 0, j = 0, k = 0;
    while (i < l1Size && j < l2.size()) {
      int cmp = Integer.compare(l1.get(i), l2.get(j));
      if (cmp == 0) {
        int num = l1.get(i);
        i++;
        j++;
        l1.set(k++, num);
      } else if (cmp < 0) {
        i++;
      } else {
        j++;
      }
    }
    return k;
  }
}
```