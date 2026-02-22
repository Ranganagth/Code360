# Intuition

If extra space were allowed, separate row and column arrays could mark which rows and columns contain zero.
To satisfy the **in-place constraint**, use the **first row and first column as markers**.

* First row marks which columns must become zero.
* First column marks which rows must become zero.

Additionally, track whether the first row or first column themselves originally contained a zero.

---

# Approach

1. Check whether the first row contains any `0`.
2. Check whether the first column contains any `0`.
3. Traverse the remaining matrix:

   * if `matrix[i][j] == 0`
   * mark:

     ```
     matrix[i][0] = 0
     matrix[0][j] = 0
     ```
4. Zero rows based on first column markers.
5. Zero columns based on first row markers.
6. Finally zero the first row/column if required.

---

# Complexity

* **Time complexity:** $(O(N \times M))$
* **Space complexity:** $(O(1))$

---

# Code

```java
import java.io.*;
import java.util.*;

public class Solution {
    public static void setZeros(int matrix[][]) {

        int n = matrix.length;
        int m = matrix[0].length;

        boolean firstRowZero = false;
        boolean firstColZero = false;

        // check first row
        for (int j = 0; j < m; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }

        // check first column
        for (int i = 0; i < n; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }

        // mark rows and columns
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        // zero rows
        for (int i = 1; i < n; i++) {
            if (matrix[i][0] == 0) {
                for (int j = 1; j < m; j++) {
                    matrix[i][j] = 0;
                }
            }
        }

        // zero columns
        for (int j = 1; j < m; j++) {
            if (matrix[0][j] == 0) {
                for (int i = 1; i < n; i++) {
                    matrix[i][j] = 0;
                }
            }
        }

        // zero first row if needed
        if (firstRowZero) {
            for (int j = 0; j < m; j++) {
                matrix[0][j] = 0;
            }
        }

        // zero first column if needed
        if (firstColZero) {
            for (int i = 0; i < n; i++) {
                matrix[i][0] = 0;
            }
        }
    }
};

```

---

# Example walkthrough with explanation

Input:

```
[1 2 3]
[4 0 6]
[7 8 9]
```

Marking phase:

```
matrix[1][0] = 0
matrix[0][1] = 0
```

Markers:

```
[1 0 3]
[0 0 6]
[7 8 9]
```

Zero row 1:

```
[1 0 3]
[0 0 0]
[7 8 9]
```

Zero column 1:

```
[1 0 3]
[0 0 0]
[7 0 9]
```

Result:

```
[1 0 3]
[0 0 0]
[7 0 9]
```
