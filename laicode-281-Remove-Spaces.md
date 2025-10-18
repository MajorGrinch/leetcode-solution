# Laicode 281. Remove Spaces

去除字符串里的leading, trailing and duplicated space，单词之间只留下1个空格。

那么依然还是双指针的思路，slow指针的含义是[0, slow)记录着当前删去指定空格之后的字符串。分类讨论，
+ 当前sc[i]字符是空格的话
  + slow为0的话，说明正式的字符串还没开始，所以这是leading空格，跳过
  + slow不为0，但是sc[i - 1]为空格的话，说明这是个连续的空格，跳过
  + 如果不满足以上两个条件，说明这是个单词之间的首个空格，放到slow，slow后移
+ 当前sc[i]字符不是空格的话，放到slow，slow后移

以上流程做完以后，我们只能保证字符串头部没有多余的空格，以及每个单词后面最多只有1个空格。但是根据题意，最后一个单词后面不能跟空格，所以最后检查一下`slow - 1`的位置是不是空格，是的话就去掉。

Time complexity: O(n)

Space complexity: O(n)

```java
public class Solution {
  public String removeSpaces(String input) {
    if (input == null || input.length() == 0) {
      return input;
    }
    int slow = 0;
    char[] sc = input.toCharArray();
    for (int i = 0; i < sc.length; i++) {
      if (sc[i] == ' ') {
        if (slow != 0 && sc[i - 1] != ' ') {
          sc[slow++] = sc[i];
        }
      } else {
        sc[slow++] = sc[i];
      }
    }
    if (slow > 0 && sc[slow - 1] == ' ') slow--;
    return new String(sc, 0, slow);
  }
}
```


2025 code:

```java
public class Solution {
  public String removeSpaces(String input) {
    if(input.length() == 0) {
      return input;
    }
    char[] sc = input.toCharArray();
    int slow = 0;
    for(int i = 0; i < sc.length; i++) {
      if(sc[i] == ' ') {
        if(slow == 0) {
          // leading space
          continue;
        }
        if(sc[i - 1] == ' ') {
          // multiple spaces between words
          continue;
        }
        // only space between words
        sc[slow++] = sc[i];
      } else {
        sc[slow++] = sc[i];
      }
    }
    if(slow > 0 && sc[slow-1] == ' ') {
      slow--;
    }
    return new String(sc, 0, slow);
  }
}
```