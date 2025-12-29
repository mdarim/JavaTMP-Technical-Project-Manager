Here are a few **Java examples to rotate an array by `N` elements** — either to the left or right. Rotating means circularly shifting elements; e.g., rotating left by `2` on `{1,2,3,4,5}` gives `{3,4,5,1,2}`. ([Sanfoundry][1])

---

## 👉 1. Simple Rotation (Left) — Naive Way

```java
public class RotateArrayLeft {
    // Rotate left by d positions
    public static void rotateLeft(int[] arr, int d) {
        int n = arr.length;
        d = d % n; // handle d > n

        for (int i = 0; i < d; i++) {
            int first = arr[0];
            for (int j = 0; j < n - 1; j++) {
                arr[j] = arr[j + 1];
            }
            arr[n - 1] = first;
        }
    }

    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        int n = 2; // rotate by 2
        
        System.out.println("Before: " + java.util.Arrays.toString(array));
        rotateLeft(array, n);
        System.out.println("After:  " + java.util.Arrays.toString(array));
    }
}
```

**Output:**

```
Before: [1, 2, 3, 4, 5]
After:  [3, 4, 5, 1, 2]
```

✔ Works even if `d > array.length` because of the `% n`. ([GeeksforGeeks][2])

---

## 👉 2. Simple Rotation (Right) — Naive Way

```java
public class RotateArrayRight {
    // Rotate right by d positions
    public static void rotateRight(int[] arr, int d) {
        int n = arr.length;
        d = d % n; // handle d > n

        for (int i = 0; i < d; i++) {
            int last = arr[n - 1];
            for (int j = n - 1; j > 0; j--) {
                arr[j] = arr[j - 1];
            }
            arr[0] = last;
        }
    }

    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        int n = 2; // rotate by 2
        
        System.out.println("Before: " + java.util.Arrays.toString(array));
        rotateRight(array, n);
        System.out.println("After:  " + java.util.Arrays.toString(array));
    }
}
```

**Output:**

```
Before: [1, 2, 3, 4, 5]
After:  [4, 5, 1, 2, 3]
```

✔ Shifts each element to the right circularly. ([Newtum][3])

---

## 👉 3. More Efficient Left Rotation Using Temporary Array

If you want **O(n) time**, you can use a temporary array:

```java
public class RotateArrayLeftEfficient {
    public static void rotateLeft(int[] arr, int d) {
        int n = arr.length;
        d = d % n;

        int[] temp = new int[n];
        int index = 0;

        // Copy elements from arr[d] to end
        for (int i = d; i < n; i++) {
            temp[index++] = arr[i];
        }
        // Copy first d elements
        for (int i = 0; i < d; i++) {
            temp[index++] = arr[i];
        }
        System.arraycopy(temp, 0, arr, 0, n);
    }

    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        rotateLeft(array, 2);
        System.out.println(java.util.Arrays.toString(array));
    }
}
```

**Output:**

```
[3, 4, 5, 1, 2]
```

This avoids shifting one-by-one. ([GeeksforGeeks][4])

---

## 👉 4. Advanced Efficient Rotation (Juggling / Reversal)

If you care about efficiency with **O(n) time & O(1) extra space**, algorithms like **Juggling** or **Reversal** are preferred (best for large arrays). ([GeeksforGeeks][5])

---

### Summary

| Approach            | Left / Right | Time   | Space |
|---------------------|--------------|--------|-------|
| Naive loop          | Both         | O(n·d) | O(1)  |
| Temp array          | Both         | O(n)   | O(n)  |
| Juggling / Reversal | Both         | O(n)   | O(1)  |

---

If you want, I can also show **generic utility methods** (e.g., returning new rotated array, or rotating `List<T>`).

[1]: https://www.sanfoundry.com/java-program-rotate-array/?utm_source=chatgpt.com "Array Rotation Program in Java - Sanfoundry"
[2]: https://www.geeksforgeeks.org/java-program-for-array-rotation/?utm_source=chatgpt.com "Java Program to Left Rotate the Elements of an Array - GeeksforGeeks"
[3]: https://blog.newtum.com/rotate-an-array-in-java/?utm_source=chatgpt.com "Rotate an Array in Java - Newtum"
[4]: https://www.geeksforgeeks.org/dsa/how-to-left-or-right-rotate-an-array-in-java/?utm_source=chatgpt.com "How to Left or Right rotate an Array in Java? - GeeksforGeeks"
[5]: https://www.geeksforgeeks.org/array-rotation/?utm_source=chatgpt.com "Rotate an Array by d - Counterclockwise or Left - GeeksforGeeks"

