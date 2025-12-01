# 208. Implement Trie

不知道该说什么，字典树，按部就班实现就好。

```java
class Trie {

    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            if (node.next[idx] == null) {
                node.next[idx] = new TrieNode();
            }
            node = node.next[idx];
        }
        node.word = word;
    }

    public boolean search(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            if (node.next[idx] == null) {
                return false;
            }
            node = node.next[idx];
        }
        return node.word != null;
    }

    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for (int i = 0; i < prefix.length(); i++) {
            int idx = prefix.charAt(i) - 'a';
            if (node.next[idx] == null) {
                return false;
            }
            node = node.next[idx];
        }
        return true;
    }

    class TrieNode {
        String word;
        TrieNode[] next = new TrieNode[26];
    }
}
```