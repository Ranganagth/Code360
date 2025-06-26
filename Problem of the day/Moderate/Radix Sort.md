# Intuition:

Radix Sort works by sorting numbers **digit by digit**, starting from the **least significant digit (LSD)** to the **most significant digit (MSD)**. It uses a **stable sort** (usually **counting sort**) for each digit place.

---

# Approach:

1. Find the **maximum number** in the array to know the number of digits.
2. Apply **counting sort** for each digit (units, tens, hundreds, etc.).
3. Counting sort must be **stable** to maintain relative ordering from previous digits.

---

# Code:

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    static int[] radixSort(int n, int arr[]) {
        int max = getMax(arr, n);

        // Apply counting sort to each digit (exp = 1, 10, 100, ...)
        for (int exp = 1; max / exp > 0; exp *= 10) {
            countingSortByDigit(arr, n, exp);
        }

        return arr;
    }

    // Helper function to get the maximum value in arr[]
    private static int getMax(int[] arr, int n) {
        int max = arr[0];
        for (int i = 1; i < n; i++) {
            if (arr[i] > max)
                max = arr[i];
        }
        return max;
    }

    // Counting sort based on digit represented by exp
    private static void countingSortByDigit(int[] arr, int n, int exp) {
        int[] output = new int[n];
        int[] count = new int[10];

		// Count occurrences
        for (int i = 0; i < n; i++) {
            int digit = (arr[i] / exp) % 10;
            count[digit]++;
        }

		// Compute prefix sum to make it stable
        for (int i = 1; i < 10; i++) {
            count[i] += count[i - 1];
        }

		// Build output array (traverse from right to left to keep it stable)
        for (int i = n - 1; i >= 0; i--) {
            int digit = (arr[i] / exp) % 10;
            output[count[digit] - 1] = arr[i];
            count[digit]--;
        }

		// Copy output to original array
        for (int i = 0; i < n; i++) {
            arr[i] = output[i];
        }
    }
}

```

---

### Example:

```java
// Input: arr = [5, 4, 3, 2, 1]
// Output: [1, 2, 3, 4, 5]
```

---

This solution runs in **O(d \* (n + b))** time, where:

* `d` = number of digits (≤10 for ≤1e9),
* `n` = number of elements,
* `b` = base (10).

Efficient for the given constraints: up to $10^4$ elements and values ≤ $10^9$.