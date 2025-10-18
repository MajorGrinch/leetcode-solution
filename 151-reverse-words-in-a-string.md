# 151. Reverse words in a String

这题其实很简单，先把字符串去除多余的空格，leading space, trailing space and only one space between words. 参照[Laicode 281](laicode-281-Remove-Spaces.md).

去除多余的空格之后，再进行单词反转。这部分可以参照[Laicode 84](laicode-84-reverse-words-in-a-sentence-i.md).

```java
class Solution {
    public String reverseWords(String s) {
        if (s == null || s.length() <= 1) {
            return s;
        }
        char[] sc = s.toCharArray();
        int slow = 0;
        for (int i = 0; i < sc.length; i++) {
            if (sc[i] == ' ') {
                if (slow == 0) {
                    // leading space
                    continue;
                }
                if (sc[i - 1] == ' ') {
                    // multiple space between words
                    continue;
                }
                sc[slow++] = sc[i];
            } else {
                sc[slow++] = sc[i];
            }
        }
        if (sc[slow - 1] == ' ') {
            // remove trailing space
            slow--;
        }
        reverse(sc, 0, slow - 1);
        int fast = 0;
        while (fast < slow) {
            int begin = fast;
            while (fast < slow && sc[fast] != ' ') {
                fast++;
            }
            reverse(sc, begin, fast - 1);
            fast++;
        }

        return new String(sc, 0, slow);
    }

    private void reverse(char[] sc, int l, int r) {
        while (l < r) {
            swap(sc, l++, r--);
        }
    }

    private void swap(char[] sc, int i, int j) {
        char temp = sc[i];
        sc[i] = sc[j];
        sc[j] = temp;
    }
}
```