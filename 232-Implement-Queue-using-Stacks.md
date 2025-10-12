# 232. Implement Queue using Stacks

用栈来模拟实现一个队列。

这里我们需要两个栈，一个in，一个out。`in`专门用来存放新进来的元素，`out`专门存放准备出去的元素。

当`push`的时候，新进来的元素直接压入`in`栈。当`pop`的时候，要出去的元素从`out`弹出。但是如果`out`为空的话，那么就把`in`的所有元素依次弹出并压入`out`。那么原来在`in`栈顶的，现在就到了`out`的栈底，正好符合了队列先入先出的性质。再把`out`弹出元素，就实现了`pop`操作。

`peek`操作也就是看`out`的栈顶，如果`out`为空的话，继续把`in`的元素挪过来。`empty`的话就看是不是两个栈都是空。

```java
class MyQueue {

    private Deque<Integer> out, in;

    /** Initialize your data structure here. */
    public MyQueue() {
        in = new ArrayDeque<>();
        out = new ArrayDeque<>();
    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        in.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(out.isEmpty()){
            feedOut();
        }
        return out.pop();
    }

    /** Get the front element. */
    public int peek() {
        if(out.isEmpty()){
            feedOut();
        }
        return out.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return in.isEmpty() && out.isEmpty();
    }

    private void feedOut(){
        while(!in.isEmpty()){
            out.push(in.pop());
        }
    }
}
```

2025 update:
```java
class MyQueue {

    private Deque<Integer> s1;
    private Deque<Integer> s2;

    public MyQueue() {
        s1 = new ArrayDeque<>();
        s2 = new ArrayDeque<>();
    }

    public void push(int x) {
        s1.push(x);
    }

    public int pop() {
        if (s2.isEmpty()) {
            feedS2();
        }
        return s2.pop();
    }

    public int peek() {
        if (s2.isEmpty()) {
            feedS2();
        }
        return s2.peek();
    }

    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }

    private void feedS2() {
        while (!s1.isEmpty()) {
            s2.push(s1.pop());
        }
    }
}
```