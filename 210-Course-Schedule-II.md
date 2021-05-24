# 210. Course Schedule II

和[207. Course Schedule](207-Course-Schedule.md)类似，但是这里如果可以完成的话要给出具体的完成顺序。

Clarification:
+ 不能上完就返回空数组
+ 不存在自依赖
+ 给的依赖关系都是合理的，不存在超出`numCourses`情况

其实区别不大，我们只要用一个额外的数组`ordering`记录下顺序就好。`taken`每次自增之前，先把当前的课程写入`ordering`对应的位置。最后如果上完了的话，就返回`ordering`，没上完就返回空数组。

```java
class Solution {
  public int[] findOrder(int numCourses, int[][] prerequisites) {
    int[] empty = new int[0];
    if (numCourses == 0) return empty;

    Course[] courses = new Course[numCourses];
    for (int i = 0; i < numCourses; i++) {
      courses[i] = new Course();
    }
    for (int[] pre : prerequisites) {
      int u = pre[1];
      int v = pre[0];
      courses[u].takeBefore.add(v);
      courses[v].leftPreAmount++;
    }

    Queue<Integer> queue = new ArrayDeque<>();
    for (int i = 0; i < numCourses; i++) {
      if (courses[i].leftPreAmount == 0) {
        queue.offer(i);
      }
    }
    int taken = 0;
    int[] ordering = new int[numCourses];
    while (!queue.isEmpty()) {
      int curr = queue.poll();
      ordering[taken++] = curr;
      for (int c : courses[curr].takeBefore) {
        if (--courses[c].leftPreAmount == 0) {
          queue.offer(c);
        }
      }
    }
    return taken == numCourses ? ordering : empty;
  }

  class Course {
    int leftPreAmount = 0;
    List<Integer> takeBefore = new ArrayList<>();
  }
}
```