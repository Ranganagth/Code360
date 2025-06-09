# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static String isPossible(String str, int n) {
        int [][] direction = {{0,1}, {1,0}, {0, -1}, {-1, 0}};
        int x = 0, y = 0, dir = 0;

        for (char ch: str.toCharArray()) {
            switch (ch) {
                case 'G' -> {
                    x += direction[dir][0];
                    y += direction[dir][1];
                }
                case 'L' -> dir = (dir + 3) % 4;
                case 'R' -> dir = (dir + 1) % 4;
            }
        }
        return ((x == 0 && y == 00) || dir != 0) ? "True" : "False";
    }

    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(System.in)) {
            int T = scanner.nextInt();
            while (T-- > 0) {
                int N = scanner.nextInt();
                String str = scanner.next();
                System.out.println(isPossible(str, N));
            }
        }
    }
}

```