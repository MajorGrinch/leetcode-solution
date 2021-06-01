# 212. Word Search II

和[79. Word Search](79-Word-Search.md)类似，但是我们要找到所有存在的单词并返回。

这题当然可以针对每个单词进去搜一遍，但是那样太耗费时间了，因此我们需要优化一下。借助字典树，根据已有的单词列表构建字典树来给深搜剪枝。当我们深搜路上遇到字典树的分支尽头了，那说明当前搜索形成的路径无法匹配任何给定的单词，再搜下去是毫无意义的，就直接返回。如果目前的节点正好在字典树里有对应的单词，就说明目前搜索路径形成了一个给定的单词，加入答案。继续借助字典树该分支，往四周搜索。不过搜之前，先把这个点标记为`#`以表明这个点搜过了，别重复了。因此，刚开始搜索的时候也先检查当前点是否为`#`以避免重复。

Time complexity: O(m * n * 3^L), L is the max length of word. This is only for worst case. In actual running, trie will help prune the search tree significantly.

Space complexity: O(N) where N is the total number of letters in the dictionary.

```java
class Solution {
  public List<String> findWords(char[][] board, String[] words) {
    List<String> res = new ArrayList<>();
    if (board.length == 0 || board[0].length == 0 || words.length == 0) {
      return res;
    }

    TrieNode root = buildTrie(words);
    int m = board.length, n = board[0].length;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        dfs(board, i, j, root, res);
      }
    }
    return res;
  }

  private void dfs(char[][] board, int r, int c, TrieNode p, List<String> res) {
    if (r < 0 || r >= board.length || c < 0 || c >= board[0].length || board[r][c] == '#') {
      return;
    }

    char ch = board[r][c];
    int nextIndex = ch - 'a';
    if (p.next[nextIndex] == null) return;

    p = p.next[nextIndex];
    if (p.word != null) {
      res.add(p.word);
      p.word = null;
    }

    board[r][c] = '#';

    dfs(board, r + 1, c, p, res);
    dfs(board, r - 1, c, p, res);
    dfs(board, r, c + 1, p, res);
    dfs(board, r, c - 1, p, res);

    board[r][c] = ch;
  }

  private TrieNode buildTrie(String[] words) {
    TrieNode root = new TrieNode();
    for (String word : words) {
      TrieNode p = root;
      for (int i = 0; i < word.length(); i++) {
        int nextIndex = word.charAt(i) - 'a';
        if (p.next[nextIndex] == null) p.next[nextIndex] = new TrieNode();
        p = p.next[nextIndex];
      }
      p.word = word;
    }
    return root;
  }

  class TrieNode {
    String word;
    TrieNode[] next = new TrieNode[26];
  }
}
```