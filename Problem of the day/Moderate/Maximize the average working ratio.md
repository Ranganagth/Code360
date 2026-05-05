# Intuition

Each extra bulb increases both **WORKS and TOTAL by 1**.

Gain from assigning one bulb to a shop:
$$
\Delta = \frac{w+1}{t+1} - \frac{w}{t}
$$

Greedy rule:

* Always assign next bulb to the shop with **maximum marginal gain**

---

# Approach

1. Use **max heap (priority queue)** based on gain:
   $$
   gain(w,t) = \frac{w+1}{t+1} - \frac{w}{t}
   $$
2. Push all shops into heap
3. Repeat `extra` times:

   * Pop shop with max gain
   * Update `(w, t) → (w+1, t+1)`
   * Push back with updated gain
4. Compute final average

---

# Complexity

* **Time complexity:**
  $$O((N + EXTRA)\log N)$$

* **Space complexity:**
  $$O(N)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 
import java.util.ArrayList;

public class Solution {

	static class Shop {
		int w, t;

		Shop(int w, int t) {
			this.w = w;
			this.t = t;
		}
	}

	private static double gain(int w, int t) {
		return (double)(w + 1) / (t + 1) - (double)w / t;
	}

	public static double maxAverageWorkingRatio(ArrayList<ArrayList<Integer>> bulbs, int n, int extra) {

		PriorityQueue<Shop> pq = new PriorityQueue<>(
			(a, b) -> Double.compare(gain(b.w, b.t), gain(a.w, a.t))
		);

		for (ArrayList<Integer> b : bulbs) {
			pq.offer(new Shop(b.get(0), b.get(1)));
		}

		while (extra-- > 0) {
			Shop s = pq.poll();
			s.w++;
			s.t++;
			pq.offer(s);
		}

		double sum = 0.0;

		while (!pq.isEmpty()) {
			Shop s = pq.poll();
			sum += (double)s.w / s.t;
		}

		return sum / n;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Bulbs:
[2/3], [3/5], [2/2]
Extra = 2

Step 1:
Compute gains → choose best shop

Step 2:
Assign bulb → update ratio

Step 3:
Repeat

Final:
(3/4 + 4/6 + 2/2) / 3 = 0.80556

---

## Example 2:

Bulbs:
[1/2], [2/4], [4/8]

Assign greedily based on gain

Final average = 0.58889
