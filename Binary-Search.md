# Binary Search Topic

## CH

二分搜索其实很简单，假定一个循环不变量loop invariant，并且让你的每一轮循环去维持这个循环不变量。

就拿278来说，find first bad version。

我们的循环不变量(loop invariant)是first bad version出现在[l, r]区间里。

如果mid is not bad verison，那么first bad version肯定出现在[mid + 1, r]。如果mid is bad version，那么first bad version肯定出现在[l, mid]。这样，每一轮循环的始末，loop invariant都是成立的。

最后循环终止，l == r，我也知道我的loop invariant依然是成立的，所以我知道[l, r]这个长度为1的区间里装的这个版本就是first bad version。