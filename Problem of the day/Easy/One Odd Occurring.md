# Intuition

We are told that:

* Every number occurs an **even** number of times,
* Except **one** number that occurs an **odd** number of times.

We can exploit the properties of the **XOR (^)** operator:

1. **a ^ a = 0** → XOR of two same numbers is 0
2. **a ^ 0 = a** → XOR with 0 gives the same number
3. XOR is **commutative and associative** → order doesn’t matter

So, if we XOR all numbers together, all pairs will cancel out, leaving only the number that occurs an odd number of times.

---

# Approach

1. Initialize a variable `res = 0`.
2. Traverse the array:

   * For each element `num`, do `res = res ^ num`.
3. After the loop ends, `res` will hold the number that occurs an odd number of times.

---

# Complexity

* **Time Complexity:** O(N)
  (We traverse the array once.)
* **Space Complexity:** O(1)
  (No extra space used.)

---

# Code

```java
public class Solution {
    public static int missingNumber(int n, int[] arr) {
        int res = 0;
        for (int num : arr) {
            res ^= num;  // XOR operation
        }
        return res;
    }
}
```

---

## Example Walkthrough

### Input:

```
n = 9
arr = [4, 5, 6, 5, 6, 9, 9, 4, 4]
```

### Process:

```
res = 0
res = 0 ^ 4 = 4
res = 4 ^ 5 = 1
res = 1 ^ 6 = 7
res = 7 ^ 5 = 2
res = 2 ^ 6 = 4
res = 4 ^ 9 = 13
res = 13 ^ 9 = 4
res = 4 ^ 4 = 0
res = 0 ^ 4 = 4
```

**Output:**

```
4
```

**Answer:** 4
