# Intuition

Convert the process into the Josephus traversal. You maintain a circular structure of remaining questions and repeatedly remove the next element by advancing **K steps** each time.

---

# Approach

Use an indexed structure for fast removals. An ArrayList works because N ≤ 3000 and O(N²) worst-case still fits in 1s.
Maintain an index pointer `idx`.
Each step:
`idx = (idx + k) % listSize`
Remove the element at `idx` and append it to the answer.
Continue until the list becomes empty.

---

# Complexity

* Time complexity: O(N²) due to ArrayList removals.
* Space complexity: O(N) for storage.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static ArrayList<Integer> theOrder(int n, int k) {
        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 1; i <= n; i++) list.add(i);

        ArrayList<Integer> ans = new ArrayList<>();
        int idx = 0;

        while (!list.isEmpty()) {
            idx = (idx + k) % list.size();
            ans.add(list.remove(idx));
        }

        return ans;
    }
};

```

---

# Example walkthrough with explanation

Example: N = 5, K = 3
List = [1,2,3,4,5], idx starts at 0
Step1: idx = (0+3) % 5 = 3 → remove 4
Step2: list=[1,2,3,5], idx=(3+3)%4=2 → remove 3
Step3: list=[1,2,5], idx=(2+3)%3=2 → remove 5
Step4: list=[1,2], idx=(2+3)%2=1 → remove 2
Step5: list=[1], idx=(1+3)%1=0 → remove 1
Final order: 4 3 5 2 1
