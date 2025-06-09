# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {

	private static String getDigitFrequency(int n) {
		char[] digits = Integer.toString(n).toCharArray();
		Arrays.sort(digits);
		return new String(digits);
	}
	public static boolean reorderedPowerOf2(int n) {
		HashSet<String> powerOfTwoFrequencies = new HashSet<>();

		for ( int i = 0; i < 31; i ++) {
			int powerofTwo = 1 << i;
			powerOfTwoFrequencies.add(getDigitFrequency(powerofTwo));
		}

		String nFrequency = getDigitFrequency(n);

		return powerOfTwoFrequencies.contains(nFrequency);
		}

		public static void main(String[] args) {
			Scanner scanner = new Scanner(System.in);
			int T = scanner.nextInt();

			while (T-- > 0) {
				int n = scanner.nextInt();
				System.out.println(reorderedPowerOf2(n) ? 1 : 0);
			}
			scanner.close();
		}
}

```