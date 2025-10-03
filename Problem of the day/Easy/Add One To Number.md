# Intuition

We’re simulating the **addition of 1** to the number represented as an array of digits:

* Start from the **last digit** (least significant).
* If it’s less than 9, just add 1 and we’re done.
* If it’s 9, make it 0 and carry 1 to the next digit.
* If we finish looping with a carry left, add `1` at the start.

Also, the result must **not contain leading zeros**.

---

# Approach

1. Remove **leading zeros** from input.
2. Traverse digits from **right to left**:

   * If digit < 9 → increment it, return result.
   * Else set digit = 0, and carry continues.
3. After loop:

   * If carry is left, insert `1` at start.
4. Return the array without leading zeros.

---

# Complexity

* **Time:** O(N) (single pass over digits)
* **Space:** O(1) extra (excluding output storage)

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static ArrayList<Integer> addOneToNumber(ArrayList<Integer> arr) {
        // Step 1: Remove leading zeros
        while (arr.size() > 1 && arr.get(0) == 0) {
            arr.remove(0);
        }

        int n = arr.size();

        // Step 2: Traverse from rightmost digit
        for (int i = n - 1; i >= 0; i--) {
            int val = arr.get(i);
            if (val < 9) {
                arr.set(i, val + 1);
                return arr; // No further carry
            }
            arr.set(i, 0); // Set to 0, carry will go to next left digit
        }

        // Step 3: If all digits were 9 (e.g., 999 → 1000)
        arr.add(0, 1);

        return arr;
    }

    // For local testing
    public static void main(String[] args) {
        ArrayList<Integer> arr1 = new ArrayList<>(Arrays.asList(1, 2, 3));
        System.out.println(addOneToNumber(arr1)); // [1,2,4]

        ArrayList<Integer> arr2 = new ArrayList<>(Arrays.asList(9, 9));
        System.out.println(addOneToNumber(arr2)); // [1,0,0]

        ArrayList<Integer> arr3 = new ArrayList<>(Arrays.asList(0, 2));
        System.out.println(addOneToNumber(arr3)); // [3]
    }
};

```

---

This solution handles:

* Normal increments
* Carry propagation
* Removing leading zeros
* Edge cases like all 9’s (`[9,9,9] → [1,0,0,0]`), single zero (`[0] → [1]`), and leading zeros (`[0,0,1,2] → [1,3]`).

---
