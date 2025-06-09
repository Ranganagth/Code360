# Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class Solution {

	public static int kDistinctChars(int k, String str) {
        if (str == null || str.length() == 0 || k == 0) return 0;

        HashMap<Character, Integer> map = new HashMap<>();
        int left = 0 , right = 0, maxLength = 0;

        while (right < str.length()) {
            char rightChar = str.charAt(right);
            map.put(rightChar, map.getOrDefault(rightChar, 0) + 1);

            while (map.size() > k) {
                char leftChar = str.charAt(left);
                map.put(leftChar, map.get(leftChar) - 1);
                if (map.get(leftChar) == 0) {
                    map.remove(leftChar);
                }
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
            right++;
        }
        return maxLength;
	}

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int T = scanner.nextInt();

        while (T-- > 0) {
            int K = scanner.nextInt();
            String str = scanner.next();
            System.out.println(kDistinctChars(K, str));
        }
        scanner.close();
    }
}

```