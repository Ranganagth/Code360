# Intuition:

- `READ4(char[] buffer)` reads up to 4 characters from a file and returns the actual number of characters read.
- Implement `READ(Reader4 obj, char[] buf, int n)` to read **up to `n` characters** from the file using repeated calls to `READ4`.
- The state (e.g., file pointer) is maintained internally by `Reader4` object — so the `read` function can be called multiple times across test cases.

---

# Approach:

- Call `READ4` repeatedly until:
  - We have read `n` characters in total, or
  - `READ4` returns less than 4 (meaning file ends).
- Copy the characters read from `READ4` into `buf`.

---

# Code:

```java
public class Solution {
  public static int read(Reader4 obj, char[] buf, int n) {
    char[] temp = new char[4];
    int totalRead = 0;

    while (totalRead < n) {
      int charsRead = obj.read4(temp);

      if (charsRead == 0) break; // EOF reached

      for (int i = 0; i < charsRead && totalRead < n; i++) {
        buf[totalRead++] = temp[i];
      }
    }

    return totalRead;
  }
}
```

---

### **Key Points:**

- We use a `temp` buffer to store each call to `read4()`.
- Copy only up to `n` characters to `buf`.
- Stop reading early if `read4()` returns fewer than 4 characters (file end).

---

### **Example Execution:**

For FILE = `abcdefgh`, first call `read(buf, 6)`:
- read4() → "abcd" → copy 4
- read4() → "efgh" → copy 2 more → total = 6

For second call `read(buf, 4)`:
- remaining characters = "gh" → read4() → only 2 → copy 2

This approach handles all such edge cases.
