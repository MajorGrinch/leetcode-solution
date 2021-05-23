# 127 Word Ladder

给定一个开始词，和一个结束词，和一个词汇表。规定每次转变只能改变一个字符且转变后的单词必须出现在词汇表里，问开始词需要多少步才能转化到结束词。如果不能，返回0。

Clarification:
+ 开始词和结束词长度一致，且和词汇表里所有词长度一致
+ 开始词不等于结束词
+ 词汇表里没有重复词汇
+ 结束词不一定在词汇表里

## BFS

这题依然是BFS，从开始词每次变化一个字符慢慢搜索。初始化步数为1，因为根据题意开始词本身就是一步。队列初始化后，放入开始词，每轮循环开始的时候队列里放着的都是当前步数能够产生的合理的单词。针对当前队列里的每个次进行变化，如果变化一步后的词就是结束词，那么直接返回步数。如果变化一步后的单词出现在词汇表里，那么就加入队列并且从词汇表里删除这个词以免重复搜索。

这样如果最后可以转换到结束词，就会被我们搜到并返回步数。如果转换不到，最后就搜不到，按照题意返回0即可。

```java
class Solution {
  public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;

    wordSet.remove(beginWord);
    wordSet.remove(endWord);

    Queue<String> queue = new ArrayDeque<>();
    queue.offer(beginWord);
    int step = 1;
    while (!queue.isEmpty()) {
      step++;
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        char[] word = queue.poll().toCharArray();
        for (int j = 0; j < word.length; j++) {
          char backup = word[j];
          for (char c = 'a'; c <= 'z'; c++) {
            if (c == backup) continue;
            word[j] = c;
            String mutation = new String(word);
            if (mutation.equals(endWord)) {
              return step;
            }
            if (wordSet.contains(mutation)) {
              queue.offer(mutation);
              wordSet.remove(mutation);
            }
            word[j] = backup;
          }
        }
      }
    }
    return 0;
  }
}
```

# Bi-directional BFS

普通的BFS会比较慢，我们可以采用双向BFS的方法加快搜索进度。

双向BFS，顾名思义从两头分别相向搜索。因此，我们需要两个队列来存放各自的搜索队列。大题思想还是一致的，稍微的区别在于如果一方产生的变换词出现在另一方的队列里，我们就直接返回。也因此，为了快速检索这个条件我们双向队列用`Set`实现而不用`Queue`。并且我们每次都从小的那一方搜索，这样可以缩小搜索范围。

如果在找到路径之前，有任意一方的队列为空了，说明就不可能找得到了，所以循环条件是两个队列都不为空。

```java
class Solution {
  public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    if (!wordSet.contains(endWord)) return 0;

    wordSet.remove(beginWord);
    wordSet.remove(endWord);

    Set<String> forwardQueue = new HashSet<>();
    Set<String> backwardQueue = new HashSet<>();

    forwardQueue.add(beginWord);
    backwardQueue.add(endWord);
    int step = 1;

    while (!forwardQueue.isEmpty() && !backwardQueue.isEmpty()) {
      if (forwardQueue.size() > backwardQueue.size()) {
        Set temp = forwardQueue;
        forwardQueue = backwardQueue;
        backwardQueue = temp;
      }
      step++;
      Set<String> forwardMutation = new HashSet<>();
      for (String word : forwardQueue) {
        char[] w = word.toCharArray();
        for (int i = 0; i < w.length; i++) {
          char backup = w[i];
          for (char c = 'a'; c <= 'z'; c++) {
            if (c == backup) continue;
            w[i] = c;
            String mutation = new String(w);
            if (backwardQueue.contains(mutation)) {
              return step;
            }
            if (wordSet.contains(mutation)) {
              forwardMutation.add(mutation);
              wordSet.remove(mutation);
            }
          }
          w[i] = backup;
        }
      }
      forwardQueue = forwardMutation;
    }
    return 0;
  }
}
```