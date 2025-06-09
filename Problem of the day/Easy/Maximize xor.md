# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {

    public static int maxXor(int L, int R) {
        int xor = L ^ R;

        int maxXor = 0;
        while (xor > 0) {
            maxXor = (maxXor << 1) | 1;
            xor >>= 1;
        }
        return maxXor;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine().trim());
        while (T-- > 0) {
            String[] range = br.readLine().trim().split(" ");
            int L = Integer.parseInt(range[0]);
            int R = Integer.parseInt(range[1]);
            System.out.println(maxXor(L, R));
        }
    }
}
```