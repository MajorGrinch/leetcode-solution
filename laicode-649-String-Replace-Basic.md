# Laicode 649. String Replace (basic)

给定一个字符串`input`，进行模式匹配和替换。

Clarification:
+ 给定的三个字符串都不为null
+ source不为空

用[laicode 85](laicode-85-Determine-Substring.md)的Rabin-Karp算法来先找到所有的occurrence，计算出新的字符串长度，然后逐一拷贝过去。遇到occurrence了就整个把target拷贝过去。

有一点需要注意：字符串里有`aaaa`，要匹配`aa`的话，要注意只能匹配两次，rolling hash容易给匹配成多次。加入occurrence的时候就需要注意检查。

Time complexity: O(M + N)，子串匹配需要O(M + N)的复杂度，替换只需一次遍历就好。

Space complexity: 具体看到底要有多少被替换。

```java
public class Solution {
  public String replace(String input, String source, String target) {
    char[] inputArr = input.toCharArray();
    char[] sc = source.toCharArray();
    char[] tc = target.toCharArray();
    int multiplier = 1;
    for(int i = 0; i < sc.length - 1; i++) {
      multiplier *= 26;
    }
    int sourceHash = getHash(sc, 0, sc.length);
    int prevHash = 0;
    List<Integer> occurList = new ArrayList<>();
    for(int i = 0; i < inputArr.length - sc.length + 1; i++) {
      int currHash;
      if(i == 0) {
        currHash = getHash(inputArr, 0, sc.length);
      } else {
        currHash = getRollingHash(prevHash, multiplier, inputArr[i - 1], inputArr[i + sc.length - 1]);
      }
      if(currHash == sourceHash && equalString(inputArr, i, sc, 0, sc.length)) {
        if(occurList.isEmpty()) {
          occurList.add(i);
        } else if(i - occurList.get(occurList.size() - 1) >= sc.length){
          occurList.add(i);
        }
      }
      prevHash = currHash;
    }
    if(occurList.isEmpty()) {
      return input;
    }
    int newLength = (tc.length - sc.length) * occurList.size() + inputArr.length;
    char[] res = new char[newLength];
    int k = 0;
    for(int i = 0; i < inputArr.length; i++) {
      if(occurList.contains(i)) {
        for(int j = 0; j < tc.length; j++) {
          res[k++] = tc[j];
        }
        i += sc.length - 1;
      } else {
        res[k++] = inputArr[i];
      }
    }
    return new String(res);
  }

  private boolean equalString(char[] s1, int s1Start, char[] s2, int s2Start, int offset) {
    for(int i = 0; i < offset; i++) {
      if(s1[s1Start + i] != s2[s2Start + i]) {
        return false;
      }
    }
    return true;
  }

  private int getHash(char[] sc, int start, int offset) {
    int hash = 0;
    for(int i = start; i < start + offset; i++) {
      hash = hash * 26 + sc[i] - 'a';
    }
    return hash;
  }

  private int getRollingHash(int prevHash, int multiplier, char removed, char added) {
    return (prevHash - (removed - 'a') * multiplier ) * 26 + (added - 'a');
  }
}
```