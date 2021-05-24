# 269. Alien Dictionary

给定一个按照字典序排序的外星字符数组，求出它字典序。

这题还是拓扑的思路。新建`Vertex`类来记录每个字符以及有多少字符排在它前面`inDegree`，以及哪些字符在它后面`outVertexes`。建立`Map<Character, Vertex> alphabet`来建立字符到对应`Vertex`的映射。

通过对字符串数组遍历来寻找潜在的字典序关系。具体来说就是相邻两个字符串找到首个不同的字符，那么前一个字符串这个位置的字符肯定在字典序上就比后一个字符串这个位置的字符要小，后一个字符的`inDegree`就得+1，前一个字符的`outVertexes`就把后一个字符的vertex加进去。余下的部分的就不用再看了。

接着，我们从所有的字符vertex里找出`inDegree`为0的入队，他们是字典序最小的一批。可能有多个`inDegree`为0的字符，他们之间的顺序我们不一定清楚，但是他们都是有可能的。只有明显的自相矛盾才会导致没有答案，比如`["a", "b", "a"]`。

用一个`cnt`来记录处理多少个字符了，用`ordering`来构建字典序。从队列取出一个字符，加入`ordering`，然后cnt自增。把所有在当前字符的`outVertexes`里的字符节点的`inDegree`全部自减，如果自减之后有0的话就入队。重复该过程知道队列为空。如果`cnt`等于字符总数，也就是`alphabet`的大小，那么就返回`ordering`。否则，就代表没处理完则返回空串。

```java
class Solution {
  public String alienOrder(String[] words) {
    Map<Character, Vertex> alphabet = new HashMap<>();
    for (String word : words) {
      for (int i = 0; i < word.length(); i++) {
        if (!alphabet.containsKey(word.charAt(i))) {
          alphabet.put(word.charAt(i), new Vertex(word.charAt(i)));
        }
      }
    }

    for (int i = 0; i < words.length - 1; i++) {
      String former = words[i], latter = words[i + 1];
      if (former.length() > latter.length() && former.startsWith(latter)) {
        return "";
      }
      int idx = 0;
      while (idx < former.length() && idx < latter.length()) {
        if (former.charAt(idx) != latter.charAt(idx)) {
          char u = former.charAt(idx);
          char v = latter.charAt(idx);
          alphabet.get(u).outVertexes.add(alphabet.get(v));
          alphabet.get(v).inDegree++;
          break;
        }
        idx++;
      }
    }

    Queue<Vertex> queue = new ArrayDeque<>();
    for (Map.Entry<Character, Vertex> entry : alphabet.entrySet()) {
      if (entry.getValue().inDegree == 0) {
        queue.offer(entry.getValue());
      }
    }
    StringBuilder ordering = new StringBuilder();
    int cnt = 0;
    while (!queue.isEmpty()) {
      Vertex curr = queue.poll();
      ordering.append(curr.c);
      cnt++;
      for (Vertex v : curr.outVertexes) {
        if (--v.inDegree == 0) {
          queue.offer(v);
        }
      }
    }
    return cnt == alphabet.size() ? ordering.toString() : "";
  }

  class Vertex {
    char c;
    int inDegree = 0;
    List<Vertex> outVertexes = new ArrayList<>();

    Vertex(char c) {
      this.c = c;
    }
  }
}
```