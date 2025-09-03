# Intuition

* The robot has 2 actions:

  * `A`: move forward/backward, double speed.
  * `R`: reverse direction, reset speed to `±1`.
* We want the **minimum number of instructions** to reach exactly `target`.
* This is essentially a **shortest path search** in the state-space `(position, speed)`.

---

# Approach 1: DP

We want the minimum number of instructions to reach `target`.

* Suppose we move forward with **k accelerations (A)** from the start:

  * Position reached = `2^k - 1`
  * Speed = `2^k`

Two cases:

1. **Exact hit**
   If `2^k - 1 == target` → answer = `k`.

2. **Overshoot**
   If `2^k - 1 > target`:

   * We overshoot, then must reverse.
   * One option: go `k` steps forward, then reverse (1 step), then solve recursively for `(2^k - 1 - target)`.
   * Cost = `k + 1 + dp(2^k - 1 - target)`.

3. **Undershoot**
   If `2^(k-1) - 1 < target < 2^k - 1`:

   * We almost reached but not enough.
   * Strategy: move `(k-1)` times, then reverse, then move `j` steps backward, then reverse again.
   * Formula:
     `dp(target) = (k - 1) + 1 + j + 1 + dp(target - (2^(k-1) - 1) + (2^j - 1))`
   * We try all `j < k-1` to minimize cost.

---

# Complexity

* DP memoizes results for each target.
* For each target, we try at most `log(target)` splits.
* Complexity: **O(T log T)** where `T` = target.

---

# Code

```java
import java.util.*;

public class Solution {
    private static Map<Integer, Integer> memo = new HashMap<>();

    public static int findShortestLen(int target) {
        return dp(target);
    }

    private static int dp(int target) {
        if (memo.containsKey(target)) return memo.get(target);

        int k = 1;
        while ((1 << k) - 1 < target) {
            k++;
        }

        int exact = (1 << k) - 1;
        // Case 1: exact hit
        if (exact == target) {
            memo.put(target, k);
            return k;
        }

        // Case 2: overshoot
        int ans = k + 1 + dp(exact - target);

        // Case 3: undershoot
        for (int j = 0; j < k - 1; j++) {
            int pos = (1 << (k - 1)) - 1; // forward k-1 times
            int back = (1 << j) - 1;     // backward j times
            int remain = target - pos + back;
            ans = Math.min(ans, (k - 1) + 1 + j + 1 + dp(remain));
        }

        memo.put(target, ans);
        return ans;
    }
}
```

---

## **Walkthrough**

Example: `target = 6`

* Smallest `k` such that `(2^k - 1) >= 6` is `k = 3`, exact = 7.
* `7 > 6` → overshoot case:

  * Option 1: `k+1+dp(7-6)` = `3+1+dp(1)`

    * `dp(1) = 1` → cost = 5
* Undershoot case:

  * Try `k-1 = 2`, pos = 3,
    For j=0: remain = 6 - 3 + 0 = 3 → cost = 2+1+0+1+dp(3)=4+2=6
    For j=1: remain = 6 - 3 + 1 = 4 → cost = 2+1+1+1+dp(4)=5+3=8
  * Min = 5.
    Answer: 5.

---


# Approach 2: BFS (Shortest Path in State Space)

* Each **state**: `(position, speed)`.
* Start state: `(0, 1)` with step count = 0.
* Transitions:

  * `A`: `(pos + speed, speed * 2)`.
  * `R`: `(pos, speed > 0 ? -1 : 1)`.
* Use **BFS queue** to explore step-by-step.
* Stop when `pos == target`.

We also need to bound the search:

* Position can go negative, but no need to go too far.
* Bound positions to `[0, 2*target]`.

  * Because if we overshoot by more than target, reversing is always suboptimal.

---

# Complexity

* Each state `(pos, speed)` visited once.
* Position bound: `O(2*target)`.
* Speed bound: also limited by doubling, but effectively `O(log(target))`.
* So BFS complexity \~ `O(target * log target)`.
* Works well for target ≤ 10^4 (per constraints).

---

# Code

```java
import java.util.*;

public class Solution {
    static class State {
        int pos, speed;
        State(int p, int s) {
            pos = p; speed = s;
        }
    }

    public static int findShortestLen(int target) {
        Queue<State> queue = new LinkedList<>();
        Queue<Integer> steps = new LinkedList<>();
        Set<String> visited = new HashSet<>();

        queue.add(new State(0, 1));
        steps.add(0);
        visited.add("0,1");

        int bound = target * 2;

        while (!queue.isEmpty()) {
            State curr = queue.poll();
            int step = steps.poll();

            if (curr.pos == target) return step;

            // Action A
            int newPos = curr.pos + curr.speed;
            int newSpeed = curr.speed * 2;
            String keyA = newPos + "," + newSpeed;
            if (Math.abs(newPos) <= bound && !visited.contains(keyA)) {
                visited.add(keyA);
                queue.add(new State(newPos, newSpeed));
                steps.add(step + 1);
            }

            // Action R
            int revSpeed = curr.speed > 0 ? -1 : 1;
            String keyR = curr.pos + "," + revSpeed;
            if (!visited.contains(keyR)) {
                visited.add(keyR);
                queue.add(new State(curr.pos, revSpeed));
                steps.add(step + 1);
            }
        }
        return -1; // theoretically never reached
    }
}
```

---

## Walkthrough

Example: `target = 2`

* Start: `(0,1)` step=0
* `A`: `(1,2)` step=1
* From `(1,2)`:

  * `A`: `(3,4)` step=2
  * `R`: `(1,-1)` step=2
* From `(1,-1)`:

  * `A`: `(0,-2)` step=3
  * `R`: `(1,1)` step=3
* Continue until `(2,*)` found at step=4.
  Answer = 4.

---
