# Intuition

Each customer appears exactly twice:

* first occurrence → arrival
* second occurrence → departure

Maintain:

* currently occupied computers
* whether a customer is using a computer or had already left due to no availability

When a customer arrives:

* if a computer is free → allocate
* else → increment rejected count and mark that customer as "rejected" so that their departure does not free any computer later

---

# Approach

1. Maintain:

   * `boolean[] usingComputer` → customer currently occupying a computer
   * `boolean[] rejected` → customer who left without getting a computer
2. Traverse the array:

   * If first occurrence:

     * if `occupied < k` → assign computer
     * else → mark rejected and increment answer
   * If second occurrence:

     * if customer was using computer → free one computer
3. Return the number of rejected customers.

---

# Complexity

* **Time:** (O(n))
* **Space:** (O(n))

---

# Code

```java
import java.util.*;
import java.io.*;
import java.util.ArrayList;

public class Solution 
{
	public static int countCustomers(ArrayList<Integer> arr, int k) 
    {
		int n = arr.size();
        int maxId = 0;
        for (int v : arr) maxId = Math.max(maxId, v);

        boolean[] using = new boolean[maxId + 1];
        boolean[] rejected = new boolean[maxId + 1];
        boolean[] seen = new boolean[maxId + 1];

        int occupied = 0;
        int lost = 0;

        for (int id : arr)
        {
            if (!seen[id]) // arrival
            {
                seen[id] = true;

                if (occupied < k)
                {
                    using[id] = true;
                    occupied++;
                }
                else
                {
                    rejected[id] = true;
                    lost++;
                }
            }
            else // departure
            {
                if (using[id])
                {
                    occupied--;
                    using[id] = false;
                }
            }
        }

        return lost;
	}
};

```

---

# Example walkthrough with explanation

For `arr = [1,2,3,2,3,1]`, `k = 2`:

* 1 arrives → gets computer (occupied=1)
* 2 arrives → gets computer (occupied=2)
* 3 arrives → no computer → rejected (lost=1)
* 2 departs → occupied=1
* 3 departs → ignored (was rejected)
* 1 departs → occupied=0

Rejected customers = **1**.
