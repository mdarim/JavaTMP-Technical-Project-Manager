Here is a **Java program to find and print the *longest contiguous increasing subsequence*** (i.e., longest *strictly* increasing subarray) from an array — not just its length but the actual elements.

A **contiguous increasing subsequence** means every element in the subsequence must be adjacent in the original array and strictly increasing from one to the next.

---

## 📌 Explanation

The idea is:

1. Traverse the array once and keep track of the start of the current increasing segment.
2. If `arr[i] > arr[i-1]`, we’re continuing an increasing run.
3. If not, we compare the current run length against the longest found so far.
4. At the end, the longest run is printed.

This runs in **O(n)** time. ([GeeksforGeeks][1])

---

## ✅ Java Code (Print Longest Contiguous Increasing Subsequence)

```java
import java.util.ArrayList;
import java.util.List;

public class LongestContiguousIncreasing {

    public static List<Integer> longestContiguousIncreasing(int[] arr) {
        int n = arr.length;
        if (n == 0) return new ArrayList<>();

        int start = 0;
        int maxLen = 1;
        int maxStart = 0;

        // Loop through the array and find longest contiguous increasing segment
        for (int i = 1; i < n; i++) {
            if (arr[i] <= arr[i - 1]) {
                int currentLen = i - start;
                if (currentLen > maxLen) {
                    maxLen = currentLen;
                    maxStart = start;
                }
                start = i;
            }
        }

        // Check last segment
        if (n - start > maxLen) {
            maxLen = n - start;
            maxStart = start;
        }

        // Build result subsequence
        List<Integer> result = new ArrayList<>();
        for (int i = maxStart; i < maxStart + maxLen; i++) {
            result.add(arr[i]);
        }

        return result;
    }

    public static void main(String[] args) {
        int[] arr = {5, 6, 3, 0, 7, 8, 9, 1, 2};
        List<Integer> longest = longestContiguousIncreasing(arr);
        System.out.println("Longest contiguous increasing subsequence: " + longest);
    }
}
```

---

## 🧪 Sample

For input:

```
{5, 6, 3, 0, 7, 8, 9, 1, 2}
```

Output:

```
Longest contiguous increasing subsequence: [0, 7, 8, 9]
```

Explanation: `0 → 7 → 8 → 9` is the longest run where each element is greater than the one before it **and contiguous**. ([Sanfoundry][2])

---

## 🧠 Notes

* This finds the **longest *contiguous*** run, not the general LIS (which can skip elements).
* Time complexity is **O(n)** — efficient for large arrays.
* You can easily modify it to return only the length if needed.

---

If you want the same logic but returning *just the length*, let me know! 😊

[1]: https://www.geeksforgeeks.org/longest-increasing-subarray/?utm_source=chatgpt.com "Longest increasing subarray - GeeksforGeeks"
[2]: https://www.sanfoundry.com/java-program-print-longest-subarray-increasing/?utm_source=chatgpt.com "Java Program to Print the Longest Increasing Subarray - Sanfoundry"
