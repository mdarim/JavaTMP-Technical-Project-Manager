Here’s a simple and common **Java method to check if two strings are anagrams** — that is, they contain the exact same characters in any order.

### ✅ Java Method (Using Sorting)

```java
import java.util.Arrays;

public class AnagramChecker {
    public static boolean areAnagrams(String s1, String s2) {
        // if lengths are different, they can't be anagrams
        if (s1.length() != s2.length()) {
            return false;
        }

        // convert both strings to char arrays
        char[] a1 = s1.toCharArray();
        char[] a2 = s2.toCharArray();

        // sort the arrays
        Arrays.sort(a1);
        Arrays.sort(a2);

        // check if sorted arrays are equal
        return Arrays.equals(a1, a2);
    }

    public static void main(String[] args) {
        System.out.println(areAnagrams("listen", "silent")); // true
        System.out.println(areAnagrams("hello", "world"));   // false
    }
}
```

### 🔍 How It Works

1. **Check lengths first** — if they differ, the strings cannot be anagrams. ([Baeldung on Kotlin][1])
2. **Convert to character arrays** — to work with individual characters. ([programiz.com][2])
3. **Sort both arrays** — anagrams will sort to the same sequence. ([programiz.com][2])
4. **Compare sorted arrays** — if they match, the original strings are anagrams. ([Baeldung on Kotlin][1])

---

### 📌 Alternative Efficient Approach (Counting Characters)

Instead of sorting (O(n log n)), you can count character frequencies in linear time. For ASCII strings, for example:

```java
public static boolean areAnagramsCount(String a, String b) {
    if (a.length() != b.length()) return false;

    int[] counts = new int[256]; // for ASCII
    for (int i = 0; i < a.length(); i++) {
        counts[a.charAt(i)]++;
        counts[b.charAt(i)]--;
    }

    for (int count : counts) {
        if (count != 0) return false;
    }
    return true;
}
```

This version avoids sorting and checks character counts directly. ([stackoverflow.com][3])

---

If you want, I can also show a version that **ignores whitespace and case differences** (e.g., “Listen” and “Silent”).

[1]: https://www.baeldung.com/java-strings-anagrams?utm_source=chatgpt.com "Check if Two Strings Are Anagrams in Java"
[2]: https://www.programiz.com/java-programming/examples/check-if-two-strings-are-anagram?utm_source=chatgpt.com "Java Program to Check if two strings are anagram"
[3]: https://stackoverflow.com/questions/36180897/checking-if-two-strings-are-anagram-of-each-other-using-basic-java?utm_source=chatgpt.com "Checking if two Strings are anagram of each other using ..."
