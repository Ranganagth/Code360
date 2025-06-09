# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
	public static ArrayList<Integer> getFrequency(String str, ArrayList<String> words, int n) {
		ArrayList<Integer> result = new ArrayList<>();

		String[] tokens = str.split("\\s+");

		HashMap<String, Integer> freq = new HashMap<>();

		for (String word : tokens) {
			freq.put(word, freq.getOrDefault(word, 0) + 1);
		}

		for (String word : words) {
			result.add(freq.getOrDefault(word, 0));
		}
		return result;
	}
}

```