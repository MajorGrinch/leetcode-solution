# 649. String Replace (basic)

给定一个字符串`input`，进行模式匹配和替换。

Clarification:
+ 给定的三个字符串都不为null
+ source不为空

用[laicode 85](laicode-85-Determine-Substring.md)的Rabin-Karp算法来先找到所有的occurrence，计算出新的字符串长度，然后逐一拷贝过去。遇到occurrence了就整个把target拷贝过去。

Time complexity: O(M + N)，子串匹配需要O(M + N)的复杂度，替换只需一次遍历就好。

Space complexity: 具体看到底要有多少被替换。

```java
public class Solution {
  public String replace(String input, String source, String target) {
    if (input.length() == 0) {
      return input;
    }
    char[] inArray = input.toCharArray();
    char[] srcArray = source.toCharArray();
    char[] tarArray = target.toCharArray();
    int multiplier = 1;
    for (int i = 0; i < srcArray.length - 1; i++) multiplier *= 26;
    int srcHash = getSubstrHash(srcArray, 0, srcArray.length);
    List<Integer> occrList = new ArrayList<>();
    int prevHash = 0;
    for (int i = 0; i < inArray.length - srcArray.length + 1; i++) {
      int currHash =
          i == 0
              ? getSubstrHash(inArray, 0, srcArray.length)
              : rollingHash(prevHash, multiplier, inArray[i - 1], inArray[i + srcArray.length - 1]);
      if (currHash == srcHash) {
        boolean equal = true;
        for (int j = 0; j < srcArray.length; j++) {
          if (srcArray[j] != inArray[i + j]) {
            equal = false;
            break;
          }
        }
        if (equal) occrList.add(i);
      }
      prevHash = currHash;
    }
    int num = occrList.size();
    int newSize = inArray.length + (tarArray.length - srcArray.length) * num;
    char[] newInputArray = new char[newSize];
    int start = 0, i = 0;
    while(i < inArray.length){
      if (occrList.contains(i)) {
        for (int j = 0; j < tarArray.length; j++) newInputArray[start + j] = tarArray[j];
        start += tarArray.length;
        i += srcArray.length;
      } else {
        newInputArray[start++] = inArray[i++];
      }
    }
    return new String(newInputArray);
  }

  private int getSubstrHash(char[] sc, int start, int offset) {
    int hash = 0;
    for (int i = start; i < start + offset; i++) {
      hash = hash * 26 + sc[i] - 'a';
    }
    return hash;
  }

  private int rollingHash(int prev, int multiplier, char oldChar, char newChar) {
    int base = 26;
    return (prev - (oldChar - 'a') * multiplier) * base + newChar - 'a';
  }
}
```