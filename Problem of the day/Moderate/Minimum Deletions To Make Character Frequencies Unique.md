# Solution

```java
import java.util.*;

public class Solution {
	public static int minDeletions(String str) {
	    Map<Character, Integer> freqMap = new HashMap<>();

		for (char c : str.toCharArray()) {
			freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
		}

		Set<Integer> usedFrequencies = new HashSet<>();
		int deletions = 0;

		for (int freq : freqMap.values()) {
			while (freq > 0 && usedFrequencies.contains(freq)) {
				freq--;
				deletions++;
			}
			if (freq > 0) {
				usedFrequencies.add(freq);
			}
		}

		return deletions;
	}
}

```