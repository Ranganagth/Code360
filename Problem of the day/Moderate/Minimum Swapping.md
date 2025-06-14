# Intuition

Minimum swaps to move all `0`s to one side and all `1`s to the other.

There are **two possible target arrangements**:
1. All 0s on the left and 1s on the right
2. All 1s on the left and 0s on the right

We compute the number of swaps needed for both cases and return the **minimum**.

# Approach

#### **Case 1: Move 0s to the left**

* Iterate through the array.
* For each `0`, count how many `1`s are **before it** (those 1s will need to be swapped with the current 0).
* This gives the number of swaps needed to move 0s left.

#### **Case 2: Move 1s to the left**

* Similarly, for each `1`, count how many `0`s are **before it**.

# Complexity Analysis

* $O(N)$ per test case
* Efficient for `N ≤ 10^6`

# Code

```java
public class Solution {
    public static int minimumSwap(int[] arr, int n) {
        int swapsToMoveZerosLeft = 0;
        int countOnes = 0;

        for (int i = 0; i < n; i++) {
            if (arr[i] == 0) {
                swapsToMoveZerosLeft += countOnes;
            } else {
                countOnes++;
            }
        }

        int swapsToMoveOnesLeft = 0;
        int countZeros = 0;

        for (int i = 0; i < n; i++) {
            if (arr[i] == 1) {
                swapsToMoveOnesLeft += countZeros;
            } else {
                countZeros++;
            }
        }

        return Math.min(swapsToMoveZerosLeft, swapsToMoveOnesLeft);
    }
}

```

---

### **Example Walkthrough:**

For `arr = [1, 0, 0, 1, 1]`:

* Swaps to move 0s left:

  * For 0 at index 1 → 1 before it → +1
  * For 0 at index 2 → 1 before it → +1
  * Total = 2

* Swaps to move 1s left:

  * For 1 at index 0 → 0s before = 0
  * For 1 at index 3 → 0s before = 2 → +2
  * For 1 at index 4 → 0s before = 2 → +2
  * Total = 4

\=> Output = `min(2, 4) = 2`
