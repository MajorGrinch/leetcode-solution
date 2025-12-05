# 1804. Implement Trie II (Prefix Tree)

和[leetcode 208](208-implement-trie.md)类似，不过这里我们需要支持返回 word 个数，特定前缀的单词个数以及从字典树里删除单词。

用 `wordCount` 和 `prefixCount` 来记录该节点的单词个数，以该节点为前缀的单词个数。
+ insert 方法实现：每一个经过的节点 prefixCount 都要增加，此外最后节点的 `wordCount` 需要增加。
+ countWordsEqualTo 方法实现：正常查找，找到最后节点返回 `wordCount`。
+ countWordsStartingWith 方法实现：正常查找，找到最后节点返回 `prefixCount`。
+ erase 方法实现：和 insert 正好反过来。沿途节点的 prefixCount 都要减少，此外最后节点的 `wordCount`需要减少。

```java
class Trie {
    private Node root;

    public Trie() {
        root = new Node();
    }

    public void insert(String word) {
        Node node = root;
        node.prefixCount++;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            if (node.next[idx] == null) {
                node.next[idx] = new Node();
            }
            node = node.next[idx];
            node.prefixCount++;
        }
        node.wordCount++;
    }

    public int countWordsEqualTo(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            node = node.next[idx];
            if (node == null) {
                return 0;
            }
        }
        return node.wordCount;
    }

    public int countWordsStartingWith(String prefix) {
        Node node = root;
        for (int i = 0; i < prefix.length(); i++) {
            int idx = prefix.charAt(i) - 'a';
            node = node.next[idx];
            if (node == null) {
                return 0;
            }
        }
        return node.prefixCount;
    }

    public void erase(String word) {
        Node node = root;
        node.prefixCount--;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            node = node.next[idx];
            if (node == null) {
                return;
            }
            node.prefixCount--;
        }
        node.wordCount--;
    }

    class Node {
        int wordCount = 0;
        int prefixCount = 0;
        Node[] next = new Node[26];
    }
}
```