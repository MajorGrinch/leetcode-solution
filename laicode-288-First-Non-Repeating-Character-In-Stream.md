# Laicode 288. First Non-Repeating Character In Stream

给定一个无限的字符输入流，实现读入操作和得到首个未重复字符操作。要求得到首个未重复字符操作的复杂度为O(1)。

这题我们用一个Deque来按序记录目前为止所有的non-repeating char和Map来记录目前为止遇到的所有字符出现的次数。我们要维护一个性质，Deque的首个字符一定是non-repeating的。这样，当我们调用`firstNonRepeating`的时候直接从队列头部取出元素就好。

当我们读入新的字符时，首先得到这字符出现过的次数。如果没有出现过，那么就把它放入Deque的尾部，并且在Map里记录它出现过1次。如果出现过大于一次，那么这个字符可能之前在Deque里，也可能不在。我们不用管这么多，我们只要保证Deque的首个字符只出现过一次就好，所以就在Deque不为空的时候检查Deque首字符是否只出现过一次。如果出现过多次，就弹出，一直到首字符只出现过一次或者队列为空为止。

Time complexity: firstNonRepeating, O(1).

```java
public class Solution {
  Deque<Character> uniqueDeque = new ArrayDeque<>();
  Map<Character, Integer> countMap = new HashMap<>();

  public Solution() {}

  public void read(char ch) {
    int count = countMap.getOrDefault(ch, 0);
    countMap.put(ch, ++count);
    if (count == 1) {
      uniqueDeque.offerLast(ch);
    } else {
      while (!uniqueDeque.isEmpty() && countMap.get(uniqueDeque.peekFirst()) > 1)
        uniqueDeque.pollFirst();
    }
  }

  public Character firstNonRepeating() {
    if (!uniqueDeque.isEmpty()) {
      return uniqueDeque.peekFirst();
    }
    return null;
  }
}
```