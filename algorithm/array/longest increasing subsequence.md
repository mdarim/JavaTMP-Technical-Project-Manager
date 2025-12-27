Below is a **detailed explanation** and a **Java implementation** that returns the **longest increasing subsequence (LIS)** of an array — not just its length, but the actual subsequence — with step-by-step reasoning.

---

## 🔍 What Is the Longest Increasing Subsequence (LIS)?

Given an array of numbers, the *Longest Increasing Subsequence (LIS)* is the longest subsequence (not necessarily contiguous) such that all its elements are **strictly increasing**.

For example:

```
Input:  [10, 22, 9, 33, 21, 50, 41, 60]
Output: [10, 22, 33, 50, 60]
Explanation: This is the longest strictly increasing subsequence.
Length = 5
```

This example and explanation are based on standard LIS definitions. ([GeeksforGeeks][1])

---

## 🧠 Algorithm Overview (Dynamic Programming + Reconstruction)

### **Idea**

1. Use a **DP array** `dp[i]` that stores the length of the longest increasing subsequence ending at index `i`.
2. Maintain a **parent pointer** `prev[i]` to reconstruct the actual subsequence later.
3. After processing all elements, find the **maximum length** and use the `prev` array to backtrack the subsequence.

This approach runs in **O(n²)** time, which is fine for moderate lengths (e.g., up to a few thousand elements). ([GeeksforGeeks][2])

---

## 📌 Step-by-Step Explanation

### 1. **Initialization**

* For every index `i`, set `dp[i] = 1` because the LIS at least includes itself.
* Set `prev[i] = -1` (no predecessor by default).

### 2. **Fill DP**

* For each `i` from 1 to `n−1`:

    * For each `j` from 0 to `i−1`:

        * If `arr[j] < arr[i]` and `dp[j] + 1 > dp[i]`, then update:

            * `dp[i] = dp[j] + 1`
            * `prev[i] = j`

### 3. **Find LIS Length and End Index**

* Track the index `maxIndex` where `dp[i]` is largest.

### 4. **Reconstruct Subsequence**

* Start from `maxIndex` and follow `prev` back to the start of LIS.
* Reverse the collected values — that gives the LIS in correct order.

---

## ✅ Full Java Code (Returns Actual LIS)

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class LongestIncreasingSubsequence {

    public static List<Integer> findLIS(int[] arr) {
        int n = arr.length;
        int[] dp = new int[n];      // dp[i] stores LIS length ending at i
        int[] prev = new int[n];    // prev[i] stores index of predecessor of arr[i]

        // Base case: each element itself is an increasing subsequence
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
            prev[i] = -1;
        }

        // Fill dp and prev arrays
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (arr[j] < arr[i] && dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    prev[i] = j;
                }
            }
        }

        // Find the index of the maximum LIS length
        int maxLen = 0;
        int maxIndex = 0;
        for (int i = 0; i < n; i++) {
            if (dp[i] > maxLen) {
                maxLen = dp[i];
                maxIndex = i;
            }
        }

        // Reconstruct the LIS by tracing predecessors
        List<Integer> lis = new ArrayList<>();
        int current = maxIndex;
        while (current != -1) {
            lis.add(arr[current]);
            current = prev[current];
        }

        // The sequence was built backwards, so reverse it
        Collections.reverse(lis);
        return lis;
    }

    public static void main(String[] args) {
        int[] arr = {10, 22, 9, 33, 21, 50, 41, 60};
        List<Integer> lis = findLIS(arr);
        System.out.println("LIS: " + lis);
    }
}
```

---

## 🧪 Example Output

For input:

```
{ 10, 22, 9, 33, 21, 50, 41, 60 }
```

Your program will output:

```
LIS: [10, 22, 33, 50, 60]
```

---

## 📌 Time & Space Complexity

| Approach               | Time      | Space |
|------------------------|-----------|-------|
| DP with reconstruction | **O(n²)** | O(n)  |

* `O(n²)` because of the nested loops comparing every pair of elements. ([GeeksforGeeks][2])

---

## 🧠 Notes

* This returns one of the possible LIS (there may be multiple LIS for the same array).
* If you only need **length**, you can skip the `prev` reconstruction and return `maxLen`. ([GeeksforGeeks][1])
* For very large `n`, an optimized approach using binary search yields **O(n log n)** time. ([ويكيبيديا][3])

---

If you want, I can also provide the **O(n log n)** Java solution that’s faster for large arrays and also reconstructs the sequence! 🚀

[1]: https://www.geeksforgeeks.org/java-program-for-longest-increasing-subsequence/?utm_source=chatgpt.com "Java Program for Longest Increasing Subsequence - GeeksforGeeks"
[2]: https://www.geeksforgeeks.org/longest-increasing-subsequence-dp-3/?utm_source=chatgpt.com "Longest Increasing Subsequence (LIS) - GeeksforGeeks"
[3]: https://en.wikipedia.org/wiki/Longest_increasing_subsequence?utm_source=chatgpt.com "Longest increasing subsequence"

