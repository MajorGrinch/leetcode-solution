# 126. Word Ladder II

和[127. Word Ladder](127-Word-Ladder.md)差不多，但是要打印出所有的最短转换路径。因此，这题不再是记录个路径长度就行了，我们需要记录下沿途的单词变化路径以用来最后打印出来。

其他过程差不多，这里我们用一个`mutationMap`来记录单词变化的关系，以及`exist`来记录路径是否存在。因为是双向搜索，所以我们需要一个额外的`isBackward`来帮助分别变化路径。比如`cog -> dog`显然是从后向前搜索，那么对应的`mutationMap`里应该是`dog => [cog]`。

当另一方队列出现了当前队列衍生的单词时，我们就知道转换路径是存在的，所以`exist`置为`true`。但是我们并不急于break，因为这并不代表这是唯一的最短路径，我们还是记录下对应的单词变化。若是`wordSet`里面有这个衍生的单词的话，就记录下来对应的单词变化并且加入`forwardMutation`。因为不是唯一路径，所以我们不能立刻从`wordSet`里删去该单词，而是当前队列所有变化结束之后将所有的衍生词统一删除。

最后，我们就可以根据得到的单词变化表来搜索路径了。

```java
class Solution {
  public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    List<List<String>> res = new ArrayList<>();
    if (!wordSet.contains(endWord)) return res;

    wordSet.remove(beginWord);
    wordSet.remove(endWord);

    Set<String> forwardQueue = new HashSet<>();
    Set<String> backwardQueue = new HashSet<>();
    forwardQueue.add(beginWord);
    backwardQueue.add(endWord);
    boolean exist = false;
    Map<String, List<String>> mutationMap = new HashMap<>();
    boolean isBackward = true;

    while (!forwardQueue.isEmpty() && !backwardQueue.isEmpty()) {
      if (exist) {
        break;
      }
      if (forwardQueue.size() > backwardQueue.size()) {
        Set<String> temp = forwardQueue;
        forwardQueue = backwardQueue;
        backwardQueue = temp;
        isBackward = !isBackward;
      }
      Set<String> forwardMutation = new HashSet<>();
      for (String word : forwardQueue) {
        char[] w = word.toCharArray();
        for (int i = 0; i < w.length; i++) {
          char backup = w[i];
          for (char c = 'a'; c <= 'z'; c++) {
            if (c == backup) continue;
            w[i] = c;
            String mutation = new String(w);
            String parent = isBackward ? word : mutation;
            String child = isBackward ? mutation : word;
            if (backwardQueue.contains(mutation)) {
              exist = true;
              mutationMap.putIfAbsent(parent, new ArrayList<>());
              mutationMap.get(parent).add(child);
            }
            if (wordSet.contains(mutation)) {
              mutationMap.putIfAbsent(parent, new ArrayList<>());
              mutationMap.get(parent).add(child);
              forwardMutation.add(mutation);
            }
          }
          w[i] = backup;
        }
      }
      wordSet.removeAll(forwardMutation);
      forwardQueue = forwardMutation;
    }
    if (!exist) return res;

    getPaths(beginWord, endWord, mutationMap, new ArrayList<>(Arrays.asList(beginWord)), res);
    return res;
  }

  private void getPaths(
      String currWord,
      String endWord,
      Map<String, List<String>> mutationMap,
      List<String> path,
      List<List<String>> res) {
    if (currWord.equals(endWord)) {
      res.add(new ArrayList<>(path));
      return;
    }
    for (String word : mutationMap.getOrDefault(currWord, Collections.emptyList())) {
      path.add(word);
      getPaths(word, endWord, mutationMap, path, res);
      path.remove(path.size() - 1);
    }
  }
}
```