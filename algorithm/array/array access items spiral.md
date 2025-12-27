To **access the items of a Java 2D array in *spiral order*** (i.e., traverse the matrix clockwise from the top-left, then right → down → left → up, shrinking inward), you typically use **four boundary markers** — `top`, `bottom`, `left`, and `right` — and move within those boundaries until all elements are visited. ([www.tpointtech.com][1])

---

### ✅ Spiral Order Traversal (Return Elements in Spiral Order)

Here’s a clean Java method that takes a `int[][] matrix` and returns all elements in spiral order:

```java
import java.util.ArrayList;
import java.util.List;

public class SpiralMatrix {
    public static List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();
        if (matrix == null || matrix.length == 0) {
            return result;
        }

        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {
            // Traverse left → right on the top row
            for (int j = left; j <= right; j++) {
                result.add(matrix[top][j]);
            }
            top++;

            // Traverse top → bottom on the right column
            for (int i = top; i <= bottom; i++) {
                result.add(matrix[i][right]);
            }
            right--;

            // Traverse right → left on the bottom row (if still valid)
            if (top <= bottom) {
                for (int j = right; j >= left; j--) {
                    result.add(matrix[bottom][j]);
                }
                bottom--;
            }

            // Traverse bottom → top on the left column (if still valid)
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    result.add(matrix[i][left]);
                }
                left++;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        System.out.println(spiralOrder(matrix)); 
        // Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
    }
}
```

---

### 🧠 How It Works

The idea is to treat the 2D array as **shrinking layers**:

1. **Top row:** move left → right
2. **Right column:** move top → bottom
3. **Bottom row:** move right → left (only if not already visited)
4. **Left column:** move bottom → top (only if still valid)

After each direction, you adjust the corresponding boundary (`top++`, `right--`, `bottom--`, `left++`). Continue until all elements are processed. ([www.tpointtech.com][1])

---

### 🧾 Notes

✔ This works for any rectangular matrix (not just square). ([www.tpointtech.com][1])
✔ The `if` guards (`top <= bottom`, `left <= right`) prevent duplicates when the matrix has uneven dimensions. ([www.tpointtech.com][1])
✔ Time complexity: **O(n × m)** where `n` and `m` are rows and columns. ([www.tpointtech.com][1])

---

If you want code that **fills** a matrix in spiral form from a 1D array (instead of reading one), I can share that too! Just let me know.

[1]: https://www.tpointtech.com/spirally-traversing-a-matrix-in-java?utm_source=chatgpt.com "Spirally Traversing a Matrix in Java - Tpoint Tech"
