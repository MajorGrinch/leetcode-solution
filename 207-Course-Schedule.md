# 207. Course Schedule

给定一定数量的课程从0开始标号，以及这些课程之间的前置关系。计算是否可以完成所有课程。

这题可以用拓扑的思想来解决。我们新建一个`Course`类包含它被哪些课程标为前置`takeBefore`，以及它自己还剩多少前置课程需要完成`leftPreAmount`。初始化`Course[]`数组之后，根据前置关系来对`takeBefore`和`leftPreAmount`进行修改来构建拓扑关系。

初始化队列为空，并且把所有没有前置课程的课号加入到队列，用`taken`来记录目前为止已经上了多少课。不断地从对列中取出元素，代表完成这门课。把这门课所有的后置课程的`leftPreAmount`全部减1，并且如果满足`leftPreAmount == 0`的话就入队。重复这个过程直到队列为空。

最后如果`taken == numCourses`，就代表可以上完，否则就不可以。

Time complexity: O(E + V) where E is the numCourse and V is the length of prerequisites. Because we only access each edge twice and each node will be enqued and dequed once.

Space complexity: O(E + V).

```java
class Solution {
  public boolean canFinish(int numCourses, int[][] prerequisites) {
    if (numCourses == 0 || prerequisites.length == 0) return true;

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
    for (int i = 0; i < courses.length; i++) {
      if (courses[i].leftPreAmount == 0) queue.offer(i);
    }
    int taken = 0;
    while (!queue.isEmpty()) {
      Course curr = courses[queue.poll()];
      taken++;
      for (int c : curr.takeBefore) {
        if (--courses[c].leftPreAmount == 0) queue.offer(c);
      }
    }
    return taken == numCourses;
  }

  class Course {
    List<Integer> takeBefore = new ArrayList<>();
    int leftPreAmount = 0;
  }
}
```