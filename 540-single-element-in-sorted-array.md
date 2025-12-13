# 540. Single Element in a Sorted Array

## Solution

### Approach: Binary Search

The key insight is that in a sorted array where every element appears twice except one:
- **Before the single element**: Each pair starts at an even index (0, 2, 4, ...) with pattern [i] == [i+1]
- **After the single element**: Each pair starts at an odd index, with pattern [i] == [i-1]

We use binary search to find where this pattern changes.

### Algorithm
1. Use binary search on the array
2. At each mid point, check if mid is part of a pair
3. Determine which half contains the single element based on:
   - If mid is even and nums[mid] == nums[mid+1], single element is on the right
   - If mid is even and nums[mid] != nums[mid+1], single element is on the left
   - If mid is odd, adjust accordingly

### Java Implementation

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            // Make sure mid is at the start of a pair
            // If mid is odd, move it back to even index
            if (mid % 2 == 1) {
                mid--;
            }

            // Check if the pair starts at mid
            if (nums[mid] == nums[mid + 1]) {
                // The single element is on the right side
                left = mid + 2;
            } else {
                // The single element is on the left side (including mid)
                right = mid;
            }
        }

        return nums[left];
    }
}
```

### Alternative Implementation (Without Mid Adjustment)

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            // Determine if we should compare with left or right neighbor
            boolean halvesAreEven = (right - mid) % 2 == 0;

            if (nums[mid] == nums[mid + 1]) {
                if (halvesAreEven) {
                    left = mid + 2;
                } else {
                    right = mid - 1;
                }
            } else if (nums[mid] == nums[mid - 1]) {
                if (halvesAreEven) {
                    right = mid - 2;
                } else {
                    left = mid + 1;
                }
            } else {
                // Found the single element
                return nums[mid];
            }
        }

        return nums[left];
    }
}
```

my code:

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l = 0;
        int r = nums.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (mid % 2 == 0) {
                if (nums[mid] == nums[mid + 1]) {
                    l = mid + 2;
                } else {
                    r = mid;
                }
            } else {
                if (nums[mid - 1] == nums[mid]) {
                    l = mid + 1;
                } else {
                    r = mid;
                }
            }
        }
        return nums[l];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(log n) - Binary search on the array
- **Space Complexity**: O(1) - Only using constant extra space

## Test Cases

```java
public class Test {
    public static void main(String[] args) {
        Solution sol = new Solution();

        // Test case 1
        int[] nums1 = {1,1,2,3,3,4,4,8,8};
        System.out.println(sol.singleNonDuplicate(nums1)); // Output: 2

        // Test case 2
        int[] nums2 = {3,3,7,7,10,11,11};
        System.out.println(sol.singleNonDuplicate(nums2)); // Output: 10

        // Test case 3 - single element at start
        int[] nums3 = {1,2,2,3,3};
        System.out.println(sol.singleNonDuplicate(nums3)); // Output: 1

        // Test case 4 - single element at end
        int[] nums4 = {1,1,2,2,3};
        System.out.println(sol.singleNonDuplicate(nums4)); // Output: 3

        // Test case 5 - only one element
        int[] nums5 = {1};
        System.out.println(sol.singleNonDuplicate(nums5)); // Output: 1
    }
}
```

## Key Insights
1. The problem requires O(log n) which hints at binary search
2. We exploit the pattern that pairs break at the single element
3. By always ensuring we check at even indices, we can determine which half to search
4. The single element disrupts the even/odd pairing pattern
