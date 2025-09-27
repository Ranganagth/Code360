# Intuition

We want to assign points:

* **Perfect hit** (correct color, correct position) → +2 points.
* **Pseudo hit** (color exists in original but wrong position, not already counted as perfect) → +1 point.

Important rule:
A color slot already counted as a **perfect hit** cannot count again as a pseudo hit.

---

# Approach

1. **Step 1:** Traverse both strings.

   * If `original[i] == guess[i]`, mark it as a **perfect hit** → add 2 points.
   * Otherwise, store the **remaining characters** from both `original` and `guess` into maps (frequency counters).
2. **Step 2:** Count pseudo hits.

   * For each color left in `guessMap`, if it also exists in `originalMap`, then pseudo hits contributed = `min(freq in guess, freq in original)`.
   * Add those pseudo hits to the score (+1 per hit).

---

# Complexity

* **Time:** `O(1)` since length is always `4`, but generalized it’s `O(n)`.
* **Space:** `O(1)` for frequency maps, at most 4 characters.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int slotScore(String original, String guess) {
        int score = 0;
        Map<Character, Integer> originalMap = new HashMap<>();
        Map<Character, Integer> guessMap = new HashMap<>();

        // Step 1: count perfect hits and build frequency maps for leftovers
        for (int i = 0; i < 4; i++) {
            if (original.charAt(i) == guess.charAt(i)) {
                score += 2; // perfect hit
            } else {
                // store frequencies of unmatched chars
                originalMap.put(original.charAt(i), originalMap.getOrDefault(original.charAt(i), 0) + 1);
                guessMap.put(guess.charAt(i), guessMap.getOrDefault(guess.charAt(i), 0) + 1);
            }
        }

        // Step 2: count pseudo hits
        for (char c : guessMap.keySet()) {
            if (originalMap.containsKey(c)) {
                score += Math.min(guessMap.get(c), originalMap.get(c));
            }
        }

        return score;
    }
};

```

---

## Example Walkthrough

Original = `"RGYB"`, Guess = `"YGRR"`

* Compare slot by slot:

  * Slot 0: `R` vs `Y` → no
  * Slot 1: `G` vs `G` → perfect hit (+2)
  * Slot 2: `Y` vs `R` → no
  * Slot 3: `B` vs `R` → no

Remaining:

* `originalMap = {R:1, Y:1, B:1}`
* `guessMap = {Y:1, R:2}`

Pseudo hits:

* `Y` → min(1,1) = 1 (+1)
* `R` → min(1,2) = 1 (+1)

Total = `2 + 1 + 1 = 4`.

---
