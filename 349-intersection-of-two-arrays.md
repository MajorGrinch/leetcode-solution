# 349. Intersection of Two Arrays

## Solution

### Approach 1: HashSet

**Algorithm:**
1. Add all elements from nums1 to a HashSet to eliminate duplicates
2. Iterate through nums2 and check if each element exists in the set
3. If it exists, add it to the result set and remove it from the original set to avoid duplicates
4. Convert the result set to an array

**Time Complexity:** O(n + m) where n is the length of nums1 and m is the length of nums2
**Space Complexity:** O(min(n, m)) for the HashSet

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resultSet = new HashSet<>();

        // Add all elements from nums1 to set
        for (int num : nums1) {
            set1.add(num);
        }

        // Check which elements from nums2 are in set1
        for (int num : nums2) {
            if (set1.contains(num)) {
                resultSet.add(num);
            }
        }

        // Convert set to array
        int[] result = new int[resultSet.size()];
        int index = 0;
        for (int num : resultSet) {
            result[index++] = num;
        }

        return result;
    }
}
```

### Approach 2: Two Pointers (with sorting)

**Algorithm:**
1. Sort both arrays
2. Use two pointers to traverse both arrays
3. If elements match, add to result and move both pointers
4. Otherwise, move the pointer with the smaller element
5. Skip duplicates in the result

**Time Complexity:** O(n log n + m log m) due to sorting
**Space Complexity:** O(1) excluding the output array

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);

        List<Integer> result = new ArrayList<>();
        int i = 0, j = 0;

        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] < nums2[j]) {
                i++;
            } else if (nums1[i] > nums2[j]) {
                j++;
            } else {
                // Found intersection
                if (result.isEmpty() || result.get(result.size() - 1) != nums1[i]) {
                    result.add(nums1[i]);
                }
                i++;
                j++;
            }
        }

        // Convert list to array
        int[] intersection = new int[result.size()];
        for (int k = 0; k < result.size(); k++) {
            intersection[k] = result.get(k);
        }

        return intersection;
    }
}
```

## Summary

- **Best Approach:** HashSet solution (Approach 1) is most straightforward and efficient
- **Key Insight:** Use a set to handle uniqueness requirement automatically
- **Edge Cases:**
  - Empty arrays (not possible per constraints)
  - No intersection (return empty array)
  - All elements are the same
