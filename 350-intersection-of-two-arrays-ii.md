# 350. Intersection of Two Arrays II

AI太好用了你们知道吗？

## Solution

### Approach 1: HashMap (Frequency Count)

**Algorithm:**
1. Create a HashMap to count the frequency of elements in nums1
2. Iterate through nums2 and check if the element exists in the map with count > 0
3. If found, add it to result and decrement the count in the map
4. This ensures we only include elements min(count1, count2) times

**Time Complexity:** O(n + m) where n is the length of nums1 and m is the length of nums2
**Space Complexity:** O(min(n, m)) for the HashMap

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        // Optimize: use smaller array for the map
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }

        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> result = new ArrayList<>();

        // Count frequency of each element in nums1
        for (int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        // Find intersection with nums2
        for (int num : nums2) {
            if (map.containsKey(num) && map.get(num) > 0) {
                result.add(num);
                map.put(num, map.get(num) - 1);
            }
        }

        // Convert list to array
        int[] intersection = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            intersection[i] = result.get(i);
        }

        return intersection;
    }
}
```

my code:

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }

        Map<Integer, Integer> freq1 = new HashMap<>();
        List<Integer> res = new ArrayList<>();

        for (int n : nums1) {
            freq1.put(n, freq1.getOrDefault(n, 0) + 1);
        }
        for (int n : nums2) {
            int count = freq1.getOrDefault(n, 0);
            if (count > 0) {
                res.add(n);
                freq1.put(n, --count);
            }
        }
        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

### Approach 2: Two Pointers (with Sorting)

**Algorithm:**
1. Sort both arrays
2. Use two pointers to traverse both arrays
3. If elements match, add to result and move both pointers
4. Otherwise, move the pointer with the smaller element

**Time Complexity:** O(n log n + m log m) due to sorting
**Space Complexity:** O(1) excluding the output array (or O(log n + log m) for sorting)

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
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
                result.add(nums1[i]);
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

### Approach 3: Stream API (Java 8+)

**Algorithm:**
- Use Java Streams with frequency map for a concise solution

**Time Complexity:** O(n + m)
**Space Complexity:** O(n)

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Long> map = Arrays.stream(nums1)
            .boxed()
            .collect(Collectors.groupingBy(e -> e, Collectors.counting()));

        return Arrays.stream(nums2)
            .filter(num -> map.getOrDefault(num, 0L) > 0)
            .map(num -> {
                map.put(num, map.get(num) - 1);
                return num;
            })
            .toArray();
    }
}
```

## Follow-up Questions

### Q1: What if the given array is already sorted? How would you optimize your algorithm?

**Answer:** Use the Two Pointers approach (Approach 2) without sorting step.
**Time Complexity:** O(n + m)
**Space Complexity:** O(1) excluding output

### Q2: What if nums1's size is small compared to nums2's size? Which algorithm is better?

**Answer:** HashMap approach (Approach 1) is better.

- Build the HashMap from the smaller array (nums1)
- Iterate through the larger array (nums2) to find matches
- Space complexity is O(n) where n is the size of smaller array
- This minimizes memory usage

The code in Approach 1 already implements this optimization by swapping arrays if nums1 is larger.

### Q3: What if elements of nums2 are stored on disk, and memory is limited?

**Answer:** Use different strategies based on constraints:

**Strategy 1: External Sorting + Two Pointers**
```java
// Pseudocode
class Solution {
    public int[] intersect(int[] nums1, File nums2File) {
        // 1. Sort nums1 in memory
        Arrays.sort(nums1);

        // 2. External sort nums2 on disk if not sorted
        // 3. Stream nums2 from disk in chunks
        List<Integer> result = new ArrayList<>();
        BufferedReader reader = new BufferedReader(new FileReader(nums2File));

        int i = 0;
        String line;
        while ((line = reader.readLine()) != null) {
            int num2 = Integer.parseInt(line);

            while (i < nums1.length && nums1[i] < num2) {
                i++;
            }

            if (i < nums1.length && nums1[i] == num2) {
                result.add(num2);
                i++;
            }
        }

        return result.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

**Strategy 2: HashMap with Disk Streaming**
```java
// If nums1 fits in memory
class Solution {
    public int[] intersect(int[] nums1, File nums2File) {
        // Build frequency map from nums1
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        // Stream nums2 from disk
        List<Integer> result = new ArrayList<>();
        BufferedReader reader = new BufferedReader(new FileReader(nums2File));

        String line;
        while ((line = reader.readLine()) != null) {
            int num = Integer.parseInt(line);
            if (map.containsKey(num) && map.get(num) > 0) {
                result.add(num);
                map.put(num, map.get(num) - 1);
            }
        }

        return result.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

**Key Considerations:**
- If nums1 fits in memory → Use HashMap approach
- If neither fits in memory → Use external sorting + two pointers
- Minimize disk I/O by reading sequentially
- Consider using memory-mapped files for better performance

## Summary

- **Best General Approach:** HashMap (Approach 1) - O(n + m) time and space efficient
- **Best for Sorted Arrays:** Two Pointers (Approach 2) without sorting - O(n + m) time, O(1) space
- **Best for Memory-Constrained:** Streaming with HashMap or External Sort
- **Key Difference from Problem 349:** Include duplicates based on min frequency in both arrays
- **Edge Cases:**
  - No intersection (return empty array)
  - One array is empty (return empty array)
  - All elements are duplicates
