# Intuition

To find both the minimum and maximum in a single pass, we can reduce the number of comparisons using **pairwise comparison** instead of checking each element individually.

---

# Approach

* If `n` is odd:
  * Start by initializing both `min` and `max` as the first element.
  * Traverse from index 1 with step size 2.
  
* If `n` is even:
  * Initialize `min` and `max` by comparing the first two elements.
  * Traverse from index 2 with step size 2.

During traversal:
* For each pair, compare the two elements:
  * First compare both elements to each other (1 comparison).
  * Then compare the smaller to `min` and the larger to `max` (2 comparisons).
  
* Total comparisons for n elements ≈ `3n/2` in worst-case, better than the naive 2n comparisons.

---

# Complexity Analysis 

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)

---

# Code (Java 8 Compatible)

```java
import java.util.*;

public class Solution {
    public static int sumOfMaxMin(int[] arr, int n) {
        if (n == 0) return 0;

        int min, max;
        int i;

        if (n % 2 == 0) {
            if (arr[0] < arr[1]) {
                min = arr[0];
                max = arr[1];
            } else {
                min = arr[1];
                max = arr[0];
            }
            i = 2;
        } else {
            min = max = arr[0];
            i = 1;
        }

        while (i < n - 1) {
            int a = arr[i];
            int b = arr[i + 1];

            if (a < b) {
                min = Math.min(min, a);
                max = Math.max(max, b);
            } else {
                min = Math.min(min, b);
                max = Math.max(max, a);
            }

            i += 2;
        }

        return min + max;
    }
}
```

---

### Example

**Input:**

```
arr = [-1, -4, 5, 8, 9, 3]
```

**Output:**

```
max = 9, min = -4 → sum = 5
```

---

# Solution in JavaScript 

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

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

function findSumOfMaxMin(arr, n) {
    if (n === 1) return arr[0] * 2;

    let maxElement = -Infinity;
    let minElement = Infinity;

    for (let i = 0; i < n; i++) {
        if (arr[i] > maxElement) {
            maxElement = arr[i];
        }
        if (arr[i] < minElement) {
            minElement = arr[i];
        }
    }

    return maxElement + minElement;
}


function main() {

    const T = parseInt(readLine());

    for (let t = 0; t < T; t++) {
        const N = parseInt(readLine());
        const arr = readLine().split(' ').map(Number);
        const result = findSumOfMaxMin(arr, N);
        console.log(result);
    }

}

```