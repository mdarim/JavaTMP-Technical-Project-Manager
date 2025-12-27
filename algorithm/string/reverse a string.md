Here’s an **efficient Java function to reverse a string without using any built-in reverse methods**, by converting it to a character array and swapping characters **in place** (so it only uses (O(n)) time and (O(n)) space for the array, but no extra temporary string or built-in reverse). ([GeeksforGeeks][1])

```java
public static String reverseString(String input) {
    // convert string to char array so we can modify it
    char[] chars = input.toCharArray();

    int left = 0;
    int right = chars.length - 1;

    while (left < right) {
        // swap characters at left and right
        char temp = chars[left];
        chars[left] = chars[right];
        chars[right] = temp;

        left++;
        right--;
    }

    // convert back to a new String
    return new String(chars);
}
```

### How it works

* We convert the input string into a **mutable `char[]`** so we can change characters without creating new Strings. ([Instance of Java][2])
* Use **two pointers**: one starting at the beginning (`left`) and one at the end (`right`).
* Swap the characters at these two positions, and move the pointers toward the center until they meet.
* Finally, we construct a new `String` from the modified character array.

### Example usage

```java
public static void main(String[] args) {
    String original = "Hello World";
    String reversed = reverseString(original);
    System.out.println(reversed); // prints: dlroW olleH
}
```

### Why this is efficient

* **Time complexity:** (O(n)) — you scan the array once.
* **Space complexity:** (O(n)) strictly because Java strings are immutable, so converting to and from a `char[]` is needed. ([GeeksforGeeks][1])

If you want a version that *doesn’t convert to a character array at all* (e.g., using recursion), I can show that too!

[1]: https://www.geeksforgeeks.org/reverse-a-string-in-java/?utm_source=chatgpt.com "Reverse a String in Java - GeeksforGeeks"
[2]: https://www.instanceofjava.com/2014/12/reverse-string-without-using-string.html?utm_source=chatgpt.com "Reverse a string without using string function in java - InstanceOfJava"

Here’s a **Java recursive solution that reverses a string *in-place*** — meaning we *don’t build intermediate strings with `substring()` on every recursive call — by using a **helper function on a `char[]`** and swapping characters as the recursion goes down and up. This is similar to how you would reverse an array recursively. ([GeeksforGeeks][1])

```java
public class ReverseRecursive {

    public static String reverseStringInPlace(String input) {
        if (input == null) {
            return null;
        }

        // Convert to mutable char array
        char[] chars = input.toCharArray();

        // Call helper to reverse in place
        reverseHelper(chars, 0, chars.length - 1);

        // Build and return a new String
        return new String(chars);
    }

    private static void reverseHelper(char[] arr, int left, int right) {
        // Base case: when pointers cross, stop
        if (left >= right) {
            return;
        }

        // Swap characters
        char temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;

        // Recurse on the "inner" part of the array
        reverseHelper(arr, left + 1, right - 1);
    }

    public static void main(String[] args) {
        String original = "Hello World!";
        String reversed = reverseStringInPlace(original);
        System.out.println(reversed); // !dlroW olleH
    }
}
```

### **How it works**

1. Convert the immutable `String` to a mutable `char[]`.
2. Use a **recursive helper function** with two indices (`left`, `right`).
3. Swap `arr[left]` and `arr[right]`.
4. Recur with `left + 1` and `right - 1` until they meet or cross.
5. Finally, build a new `String` from the reversed array. ([GeeksforGeeks][1])

### **Why this is efficient**

* **Time complexity:** (O(n)) — each character is swapped once.
* **Auxiliary space:** (O(n)) for the recursion call stack and the `char[]` due to Java’s immutable `String`. ([GeeksforGeeks][1])

This gives you a **recursive in-place reversal** that avoids building new strings on every recursive call (unlike simple recursive with `substring()`), but still adheres to the constraint of not using any “reverse” built-in. If you want a version that *prints* the reversed string using recursion (without building any arrays), I can show that too!

[1]: https://www.geeksforgeeks.org/dsa/reverse-an-array-using-recursion/?utm_source=chatgpt.com "Reverse an Array using Recursion - GeeksforGeeks"
