# Intuition

This is a **topological sorting problem**.

From adjacent words, derive ordering rules:

* compare two words
* first differing character gives an edge:

  $$
  u → v  (u comes before v)
 $$ 
Build a graph and perform **topological sort**.

To ensure **lexicographically smallest order**, use:

* **PriorityQueue (min-heap)** instead of normal queue

Also detect **invalid cases**:

* prefix issue: `"abc"` before `"ab"` → invalid
* cycle in graph

---

# Approach

1. Initialize graph and indegree for all characters.
2. Compare adjacent words:

   * find first mismatch → add edge
   * if no mismatch and first word longer → invalid
3. Perform Kahn’s BFS topological sort:

   * use min-heap for lexicographical order
4. If result size != total chars → cycle → return ""

---

# Complexity

* **Time complexity:**
$$
O(C + E log C)
$$
* **Space complexity:**
$$
O(C + E)
$$

# Code

```java
import java.util.*;
import java.io.*;

public class Solution 
{
    public static String alienOrder(ArrayList<String> words, int n)
    {
        Map<Character, Set<Character>> graph = new HashMap<>();
        Map<Character, Integer> indegree = new HashMap<>();

        // Initialize all characters
        for (String word : words) {
            for (char c : word.toCharArray()) {
                graph.putIfAbsent(c, new HashSet<>());
                indegree.putIfAbsent(c, 0);
            }
        }

        // Build graph
        for (int i = 0; i < n - 1; i++) {

            String w1 = words.get(i);
            String w2 = words.get(i + 1);

            int len = Math.min(w1.length(), w2.length());
            boolean found = false;

            for (int j = 0; j < len; j++) {
                char c1 = w1.charAt(j);
                char c2 = w2.charAt(j);

                if (c1 != c2) {
                    if (!graph.get(c1).contains(c2)) {
                        graph.get(c1).add(c2);
                        indegree.put(c2, indegree.get(c2) + 1);
                    }
                    found = true;
                    break;
                }
            }

            // invalid prefix case
            if (!found && w1.length() > w2.length()) {
                return "";
            }
        }

        // Topological sort using min-heap
        PriorityQueue<Character> pq = new PriorityQueue<>();

        for (char c : indegree.keySet()) {
            if (indegree.get(c) == 0) {
                pq.offer(c);
            }
        }

        StringBuilder result = new StringBuilder();

        while (!pq.isEmpty()) {
            char curr = pq.poll();
            result.append(curr);

            for (char nei : graph.get(curr)) {
                indegree.put(nei, indegree.get(nei) - 1);
                if (indegree.get(nei) == 0) {
                    pq.offer(nei);
                }
            }
        }

        // cycle check
        if (result.length() != indegree.size()) {
            return "";
        }

        return result.toString();
    }
}
```
