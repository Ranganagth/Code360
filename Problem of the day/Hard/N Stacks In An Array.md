# Intuition

Multiple stacks must share a single array without fixed partitioning.

Use:

* A single array for values
* A **free list** to reuse empty indices
* Each stack behaves like a linked list inside the array

---

# Approach

Maintain:

1. `arr[]` → actual values
2. `top[]` → top index of each stack
3. `next[]` → next index (for both stack links + free list)
4. `free` → starting index of free list

Initialization:

* All indices are free → chain using `next[]`

### Push:

* Take index from `free`
* Update `free = next[free]`
* Insert element:

  * `arr[index] = x`
  * `next[index] = top[m-1]`
  * `top[m-1] = index`

### Pop:

* If stack empty → return -1
* Get top index
* Move top pointer
* Add index back to free list

---

# Complexity

* **Time complexity:**
  $$O(1)$$ per operation

* **Space complexity:**
  $$O(S + N)$$

---

# Code

```Java
import java.util.*;
import java.io.*;

public class NStack {

    int[] arr;
    int[] top;
    int[] next;
    int free;

    public NStack(int N, int S) {

        arr = new int[S];
        top = new int[N];
        next = new int[S];

        Arrays.fill(top, -1);

        for (int i = 0; i < S - 1; i++) {
            next[i] = i + 1;
        }
        next[S - 1] = -1;

        free = 0;
    }

    public boolean push(int x, int m) {

        if (free == -1) return false;

        int index = free;
        free = next[index];

        arr[index] = x;

        next[index] = top[m - 1];
        top[m - 1] = index;

        return true;
    }

    public int pop(int m) {

        if (top[m - 1] == -1) return -1;

        int index = top[m - 1];
        top[m - 1] = next[index];

        next[index] = free;
        free = index;

        return arr[index];
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

N = 3, S = 6

Initial:
free = 0 → 1 → 2 → 3 → 4 → 5

push(10,1):

* index = 0
* stack1 → [10]

push(20,1):

* index = 1
* stack1 → [10,20]

push(30,2):

* index = 2
* stack2 → [30]

pop(1):

* returns 20

pop(2):

* returns 30

---

## Example 2:

Single stack

push(15), push(25)
pop → 25
push(30)
pop → 30
