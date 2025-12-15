### Problem Summary:

You are given an array of integers. A **mountain subarray** is defined as one that:

1. Strictly **increases** to a peak.
2. Then strictly **decreases** after the peak.
   You need to find the **length of the longest mountain** subarray.

---

# Intuition:

To qualify as a mountain, an element must have:
* At least one increasing element before it.
* At least one decreasing element after it.

So, we can:
1. Traverse the array and find **upward slopes** before each index.
2. Find **downward slopes** after each index.
3. For every element that has both slopes (`up[i] > 0 && down[i] > 0`), we compute the length of the mountain: `up[i] + down[i] + 1`.

---

# Approach:

1. Create two arrays:
   * `up[i]`: Number of increasing elements ending at `i`.
   * `down[i]`: Number of decreasing elements starting at `i`.
2. Traverse the array from left to right to fill `up[i]`.
3. Traverse from right to left to fill `down[i]`.
4. Loop through each index:
   * If `up[i] > 0 && down[i] > 0`, then compute mountain length and track the maximum.

---

# Complexity Analysis:

* **Time:** `O(N)` — One pass for `up`, one for `down`, and one to compute result.
* **Space:** `O(N)` — For `up[]` and `down[]`.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int longestMountain(int[] arr, int n) {
        if (n < 3) return 0;

        int[] up = new int[n];
        int[] down = new int[n];

        // Build 'up' array
        for (int i = 1; i < n; i++) {
            if (arr[i] > arr[i - 1]) {
                up[i] = up[i - 1] + 1;
            }
        }

        // Build 'down' array
        for (int i = n - 2; i >= 0; i--) {
            if (arr[i] > arr[i + 1]) {
                down[i] = down[i + 1] + 1;
            }
        }

        int maxLen = 0;
        for (int i = 0; i < n; i++) {
            if (up[i] > 0 && down[i] > 0) {
                maxLen = Math.max(maxLen, up[i] + down[i] + 1);
            }
        }

        return maxLen;
    }
}
```

---

# Example Walkthrough:

**Input:** `arr = [1, 3, 1, 4]`
**up\[]:**    `[0, 1, 0, 1]`
**down\[]:**  `[0, 1, 0, 0]`

At index 1 → `up[1] = 1`, `down[1] = 1`
→ Mountain of length `1 + 1 + 1 = 3`

Final Answer: `3`

---

### Solution (JavaScript)

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function longestMountainSubarray(N, arr) {
    if (N < 3) {
        return 0;
    }

    let maxLength = 0;

    for (let i = 1; i < N - 1; i++) {
        if (arr[i] > arr[i - 1] && arr[i] > arr[i + 1]) {
            let left = i - 1;
            let right = i + 1;

            while (left > 0 && arr[left] > arr[left - 1]) {
                left--;
            }

            while (right < N - 1 && arr[right] > arr[right + 1]) {
                right++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }
    }

    return maxLength;
}


process.stdin.resume();
process.stdin.setEncoding('ascii');

var input_stdin = "";
var input_stdin_array = "";
var input_currentline = 0;

process.stdin.on('data', function (data) {
    input_stdin += data;
});

process.stdin.on('end', function () {
    input_stdin_array = input_stdin.split("\n");
    main();
});

function readLine() {
    return input_stdin_array[input_currentline++];
}


function main() {

    let T = parseInt(readLine());
    for (let t = 0; t < T; t++) {
        let N = parseInt(readLine());
        const arr = readLine().replace(/\s+$/g, '').split(' ').map(arrTemp => parseInt(arrTemp));

        let result = longestMountainSubarray(N, arr);
        console.log(result);
    }

}

```