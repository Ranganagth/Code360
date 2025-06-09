# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
	public static int planTown(int m, int n, char rides[][]){
		int[] prevRow = new int[1 << n];
		Arrays.fill(prevRow, -1);
		prevRow[0] = 0;

		for (int row = 0; row < m; row++) {
			int[] currRow = new int[1 << n];
			Arrays.fill(currRow, -1);

			for (int mask = 0; mask < (1 << n); mask++) {
				if(!isValid(mask, rides[row])) continue;

				for (int prevMask = 0; prevMask < (1 << n); prevMask++) {
					if(prevRow[prevMask] == -1) continue;

					if((mask & (prevMask << 1)) != 0 || (mask & (prevMask >> 1)) != 0) {
						continue;
					}

					int houseCount = Integer.bitCount(mask);
					currRow[mask] = Math.max(currRow[mask], prevRow[prevMask] + houseCount);
				}
			}
			prevRow = currRow;
		}

		int maxHouses = 0;
		for (int count : prevRow) {
			maxHouses = Math.max(maxHouses, count);
		}
		return maxHouses;
	}

	private static boolean isValid(int mask, char[] row) {
		int n = row.length;

		for(int col = 0; col < n; col++) {
			if((mask & (1 << col)) > 0 && row[col] == 'T') {
				return false;
			}
		}

		if((mask & (mask << 1)) > 0) {
			return false;
		}

		return true;
	}

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int T = scanner.nextInt();

		while (T-- > 0) {
			int M = scanner.nextInt();
			int N = scanner.nextInt();
			char[][] rides = new char[M][N];

			for (int i = 0; i < M; i++) {
				for(int j = 0; j < N; j++) {
					rides[i][j] = scanner.next().charAt(0);
				}
			}

			System.out.println(planTown(M,N, rides));
		}
		scanner.close();
	}
}

```