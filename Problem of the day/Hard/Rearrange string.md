# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {

	static class CharCount {
		char character;
		int count;

		CharCount(char character, int count) {
			this.character = character;
			this.count = count;
		}
	}

    public static String reArrangeString(String s) {
        int n = s.length();
		if (n == 0) {
			return "";
		}

		Map<Character, Integer> freqMap = new HashMap<>();
		for (char c : s.toCharArray()) {
			freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
		}

		PriorityQueue<CharCount> maxHeap = new PriorityQueue<>((a, b) -> b.count - a.count);
		int maxFreq = 0;
		for (Map.Entry<Character, Integer> entry: freqMap.entrySet()) {
			maxHeap.add(new CharCount(entry.getKey(), entry.getValue()));
			maxFreq = Math.max(maxFreq, entry.getValue());
		}

		if (maxFreq > (n + 1) / 2) {
			return "not possible";
		}

		StringBuilder result = new StringBuilder();
		CharCount prevCharCount = null;

		while (!maxHeap.isEmpty()) {
			CharCount current = maxHeap.poll();

			result.append(current.character);
			current.count--;

			if (prevCharCount != null && prevCharCount.count > 0) {
				maxHeap.add(prevCharCount);
			}

			prevCharCount = current;
		}

		if (result.length() != n) {
			return "not possible";
		}

		return result.toString();
    }

}

```