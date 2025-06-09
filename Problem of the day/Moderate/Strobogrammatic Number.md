# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
   
   	public static boolean isStrobogrammatic(String n){
    	Map<Character, Character> strobogrammaticMap = new HashMap<>();
		strobogrammaticMap.put('0', '0');
		strobogrammaticMap.put('1', '1');
		strobogrammaticMap.put('6', '9');
		strobogrammaticMap.put('9', '6');
		strobogrammaticMap.put('8', '8');

		int left = 0;
		int right = n.length() - 1;

		while (left <= right) {
			char leftChar = n.charAt(left);
			char rightChar = n.charAt(right);

			if(!strobogrammaticMap.containsKey(leftChar) ||
			strobogrammaticMap.get(leftChar) != rightChar) {
				return false;
			}

			left++;
			right--;
		}

		return true;
    }

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine().trim());

		while ( T-- > 0) {
			String n = br.readLine().trim();
			System.out.println(isStrobogrammatic(n));
		}
	}

}

```