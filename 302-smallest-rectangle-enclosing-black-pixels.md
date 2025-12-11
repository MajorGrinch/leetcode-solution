# 302. Smallest Rectangle Enclosing Black Pixels

给定一个由 01 组成的二维数组，所有含有 1 的格子都是相连起来的。现在给定一个含有 1 的格子坐标，返回把所有 1 围起来的最小矩形面积。

其实很简单，用二分搜索分别去找矩形的上下左右四条边就好。怎么找？我们拿左边来举例子，矩形的左边肯定存在于 `[0, y]`这个范围内。二分搜索的时候，如果当前列 mid 是全 0 列，那么左边肯定就在 `(mid, y]` 里面，反之左边在 `[0, mid]`里面。

Time complexity: O(mlogn + nlogm)

Space complexity: O(1)

```java
class Solution {
    public int minArea(char[][] image, int x, int y) {
        int top = findTop(image, x, image.length - 1);
        int bottom = findBottom(image, 0, x);
        int left = findLeft(image, 0, y);
        int right = findRight(image, y, image[0].length - 1);
        return (top - bottom + 1) * (right - left + 1);
    }

    private int findLeft(char[][] img, int l, int r) {
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (isEmptyCol(img, mid)) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }

    private int findRight(char[][] img, int l, int r) {
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            if (isEmptyCol(img, mid)) {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        return isEmptyCol(img, r) ? l : r;
    }

    private int findTop(char[][] img, int l, int r) {
        while (r - l > 1) {
            int mid = l + (r - l) / 2;
            if (isEmptyRow(img, mid)) {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        return isEmptyRow(img, r) ? l : r;
    }

    private int findBottom(char[][] img, int l, int r) {
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (isEmptyRow(img, mid)) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }

    private boolean isEmptyCol(char[][] img, int col) {
        for (int r = 0; r < img.length; r++) {
            if (img[r][col] == '1') {
                return false;
            }
        }
        return true;
    }

    private boolean isEmptyRow(char[][] img, int row) {
        for (int c = 0; c < img[row].length; c++) {
            if (img[row][c] == '1') {
                return false;
            }
        }
        return true;
    }
}
```