# Laicode 279. Sort With 3 Stacks

用MergeSort的思想。

```java
public class Solution {
  public void sort(LinkedList<Integer> s1) {
    LinkedList<Integer> s2 = new LinkedList<Integer>();
    LinkedList<Integer> s3 = new LinkedList<Integer>();
    // Write your solution here.
    mergeSort(s1, s2, s3, s1.size());
  }

  private void mergeSort(LinkedList<Integer> s1, LinkedList<Integer> s2, LinkedList<Integer> s3, int s1Len) {
    if(s1Len <= 1) return;

    int s2Len = s1Len / 2;
    for(int i = 0; i < s2Len; i++) s2.push(s1.pop());

    s1Len -= s2Len;
    mergeSort(s1, s2, s3, s1Len);
    mergeSort(s2, s1, s3, s2Len);

    int i = 0;
    int j = 0;
    while(i < s1Len && j < s2Len) {
      if(s1.peek() <= s2.peek()) {
        s3.push(s1.pop());
        i++;
      } else {
        s3.push(s2.pop());
        j++;
      }
    }
    while(i++ < s1Len) s3.push(s1.pop());
    while(j++ < s2Len) s3.push(s2.pop());

    while(!s3.isEmpty()) {
      s1.push(s3.pop());
    }
  }
}
```