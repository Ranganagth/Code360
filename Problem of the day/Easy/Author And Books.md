# Intuition

Each row in the 2D array represents **one author**.

* The **first string** is the author’s name.
* The **remaining strings** are the books written by that author.

You must **format** this information into a numbered author list, and for each author, list their books using **alphabetical labels (A, B, C, …)** with proper indentation.

This is a **pure formatting problem**:

* No sorting
* No complex logic
* Just structured traversal and string construction

### Required Output Format (Strict)

For each author:

```
1. AuthorName
    A. Book1
    B. Book2
```

Rules:

* Author numbering starts from **1**
* Book labels start from **A**
* Each book line is indented by **4 spaces**
* Output is returned as a **String array**, one line per entry

---

# Approach

1. Use a dynamic list (`ArrayList<String>`) to store output lines.
2. Loop through authors using index `i`:

   * Add author line: `(i+1) + ". " + AuthorName`
3. For books:

   * Start character from `'A'`
   * For each book, append:

     ```
     "    " + letter + ". " + bookName
     ```
4. Convert the list to `String[]` and return.

---

# Complexity

- Time Complexity
	* **O(Total number of strings)**
  (Each author name and book is processed once)

- Space Complexity
	* **O(Output size)**
  Required to store formatted result

---

# Code

```java
import java.util.*;

public class Solution {
    public static String[] arrangeAuthors(String[][] S) {
        ArrayList<String> result = new ArrayList<>();

        for (int i = 0; i < S.length; i++) {
            // Author line
            result.add((i + 1) + ". " + S[i][0]);

            // Books
            char bookLabel = 'A';
            for (int j = 1; j < S[i].length; j++) {
                result.add("    " + bookLabel + ". " + S[i][j]);
                bookLabel++;
            }
        }

        return result.toArray(new String[0]);
    }
};

```

---

# Example Walkthrough

#### Input

```text
[
  ["ChetanBhagat", "TwoStates", "Revolution", "HalfGirlfriend"],
  ["JKRowling", "HarryPotter", "FantasticBeasts"]
]
```

#### Processing

* Author 1 → `"1. ChetanBhagat"`

  * Books → A, B, C
* Author 2 → `"2. JKRowling"`

  * Books → A, B

#### Output

```text
1. ChetanBhagat
    A. TwoStates
    B. Revolution
    C. HalfGirlfriend
2. JKRowling
    A. HarryPotter
    B. FantasticBeasts
```

---

### Key Takeaway

This problem tests:

* Clean iteration
* String formatting discipline
* Attention to indentation and ordering

No edge tricks. Just precision.
