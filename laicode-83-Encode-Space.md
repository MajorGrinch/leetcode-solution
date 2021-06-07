# Laicode 83. Encode Space

给定一个字符串，对其中的空格进行encode变成`20%`。

那么我们首先遍历一下字符串统计有多少个空格，那么encode之后的长度就是字符串长度加上两倍的空格数量，因为每个空格都会额外带来2位长度。

双指针分别从原字符串和新字符串末尾一起向前，原字符串里遇到普通字符就拷贝，遇到空格就放`20%`这三个字符进去。两种情况新字符串的指针移动的位数不一样，注意区别。

Time complexity: O(n)

Space complexity: O(n)

```java
public class Solution {
  public String encode(String input) {
    if(input == null){
      return input;
    }
    int spaceCnt = 0;
    for(int i = 0; i < input.length(); i++){
      if(input.charAt(i) == ' ') spaceCnt++;
    }
    char[] sc = new char[input.length() + spaceCnt * 2];
    int j = sc.length - 1;
    for(int i = input.length() - 1; i >= 0; i--){
      if(input.charAt(i) == ' '){
        sc[j] = '%';
        sc[j - 1] = '0';
        sc[j - 2] = '2';
        j -= 3;
      }else{
        sc[j--] = input.charAt(i);
      }
    }
    return new String(sc);
  }
}
```