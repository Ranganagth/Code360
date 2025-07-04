# Intuition

We need to simulate a smart autocomplete feature that:
1. Tracks how frequently sentences are typed.
2. Suggests top 3 matches based on frequency and prefix.
3. Updates records when a sentence ends (with `#`).

To efficiently find sentences with a common prefix, a **Trie** is ideal. To track frequencies and sorting by frequency & lexicographical order, use **HashMaps** and custom comparators.

---

# Approach

1. **Data Structures**:
   * **TrieNode**: To store prefix hierarchy.
   * **Map\<String, Integer> freqMap**: To store sentence frequency.

2. **Trie Insertion**:
   * For each sentence, insert into the Trie and update frequency.

3. **Input Handling**:
   * If input is `#`: update frequency, reset input state.
   * Else: add char to current query and find top 3 matches.

4. **Searching Top Matches**:
   * Traverse the Trie to the prefix node.
   * From that node, DFS all possible sentences and return top 3 by:
     * Highest frequency
     * Lexicographically smaller string if tie

---

# Complexity Analysis

* **Insert Sentence**: O(L), where L is length of sentence
* **Get Suggestions**: O(M log M), where M is the number of matching sentences
* **Overall**:

  * Time per `input(char)`: O(P + M log M) where P = length of prefix, M = matching nodes
  * Space: O(N \* L) for Trie and Map

---
# Code

```java
import java.util.*;

public class AutocompleteSystem {
    
    private static class TrieNode {
        Map<Character, TrieNode> children = new HashMap<>();
        Set<String> sentences = new HashSet<>();
    }

    private final TrieNode root = new TrieNode();
    private final Map<String, Integer> freqMap = new HashMap<>();
    private StringBuilder currentQuery = new StringBuilder();

    public AutocompleteSystem(String[] sentences, int[] times) {
        for (int i = 0; i < sentences.length; i++) {
            freqMap.put(sentences[i], times[i]);
            insertToTrie(sentences[i]);
        }
    }

    private void insertToTrie(String sentence) {
        TrieNode node = root;
        for (char ch : sentence.toCharArray()) {
            node.children.putIfAbsent(ch, new TrieNode());
            node = node.children.get(ch);
            node.sentences.add(sentence);
        }
    }

    public List<String> input(char c) {
        if (c == '#') {
            String fullSentence = currentQuery.toString();
            freqMap.put(fullSentence, freqMap.getOrDefault(fullSentence, 0) + 1);
            insertToTrie(fullSentence);
            currentQuery = new StringBuilder();
            return new ArrayList<>();
        }

        currentQuery.append(c);
        String prefix = currentQuery.toString();
        TrieNode node = root;
        for (char ch : prefix.toCharArray()) {
            if (!node.children.containsKey(ch)) {
                return new ArrayList<>();
            }
            node = node.children.get(ch);
        }

        PriorityQueue<String> pq = new PriorityQueue<>(
            (a, b) -> {
                int freqA = freqMap.get(a), freqB = freqMap.get(b);
                if (freqA != freqB) return freqB - freqA;
                return a.compareTo(b);
            }
        );

        pq.addAll(node.sentences);
        List<String> result = new ArrayList<>();
        int count = 0;
        while (!pq.isEmpty() && count < 3) {
            result.add(pq.poll());
            count++;
        }

        return result;
    }
}
```

---

### **Sample Usage**

```java
AutocompleteSystem obj = new AutocompleteSystem(
    new String[]{"i want to join faang", "icecream", "i love coding ninjas"},
    new int[]{4, 3, 1}
);

System.out.println(obj.input('i')); // [i want to join faang, icecream, i love coding ninjas]
System.out.println(obj.input('s')); // []
System.out.println(obj.input('#')); // []
```

---
### **Example Walkthrough**

Input:

```
Sentences = ["i want to join faang", "icecream", "i love coding ninjas"]
Times     = [4, 3, 1]
```

* Typing `i`: returns all 3
* Typing `s`: no matches → `[]`
* Typing `#`: end sentence → add `"is"` to Trie

---
