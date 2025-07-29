# Intuition

To safely transmit secret information over insecure channels, we'll encode the message using a Caesar cipher with a +3 ASCII shift and replace spaces with `'!'`. This ensures that the message structure 
is preserved but notreadable to an unintended recipient.

For decoding, we simply reverse the shift and replace `'!'` with a space.

---

# Approach

* Traverse each character in the input string:
  * If the character is a space, replace it with `'!'`.
  * Otherwise, add 3 to the ASCII value and append it to the result.

* For decoding:
  * If the character is `'!'`, convert it back to a space.
  * Otherwise, subtract 3 from the ASCII value and append it.

After encoding and decoding, compare the decoded result with the original message to verify successful transmission.

---

# Complexity

* **Time complexity**:
  $O(n)$ for both encoding and decoding, where $n$ is the length of the input string.

* **Space complexity**:
  $O(n)$ to store intermediate strings.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    // Encode the secret message with Caesar cipher and replacing spaces with '!'
    public static String encode(String secretInformation) {
        StringBuilder encoded = new StringBuilder();
        for (char ch : secretInformation.toCharArray()) {
            if (ch == ' ') {
                encoded.append('!');
            } else {
                encoded.append((char)(ch + 3)); // Caesar cipher shift +3
            }
        }
        return encoded.toString();
    }

    // Decode the message by reversing Caesar cipher and replacing '!' back to space
    public static String decode(String encodedInformation) {
        StringBuilder decoded = new StringBuilder();
        for (char ch : encodedInformation.toCharArray()) {
            if (ch == '!') {
                decoded.append(' ');
            } else {
                decoded.append((char)(ch - 3)); // Reverse Caesar cipher
            }
        }
        return decoded.toString();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = Integer.parseInt(sc.nextLine());
        for (int i = 0; i < t; i++) {
            String input = sc.nextLine();
            String encoded = encode(input);
            String decoded = decode(encoded);
            if (decoded.equals(input)) {
                System.out.println("Transmission successful");
            } else {
                System.out.println("Transmission unsuccessful");
            }
        }
    }
}

```

---
