# Intuition

Process the list in **groups of size K**:

* Traverse K nodes
* Compute their sum
* Insert a new node with that sum after the group

If remaining nodes < K:

* Still compute sum
* Insert one final node

---

# Approach

1. Initialize `curr = head`
2. While traversing:

   * Count up to K nodes
   * Compute sum
   * Keep track of last node of this group
3. Insert new node after the group:

   * `last.next = new Node(sum)`
4. Connect inserted node to next part of list
5. Continue from next node after inserted node

---

# Complexity

* **Time complexity:**
  $$O(N)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {
	public static Node getListAfterAddingNodes(Node head, int k) {

		if (head == null) return head;

		Node curr = head;

		while (curr != null) {

			int sum = 0;
			int count = 0;

			Node temp = curr;
			Node prev = null;

			// Traverse K nodes or till end
			while (temp != null && count < k) {
				sum += temp.data;
				prev = temp;
				temp = temp.next;
				count++;
			}

			// Insert new node after group
			Node newNode = new Node(sum);
			prev.next = newNode;
			newNode.next = temp;

			// Move to next group
			curr = temp;
		}

		return head;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Input:
1 → 2 → 3 → 4 → 5 → 6 → 7

K = 3

Group 1:
1 + 2 + 3 = 6
→ Insert after 3

List:
1 → 2 → 3 → 6 → ...

Group 2:
4 + 5 + 6 = 15
→ Insert after 6

List:
... → 4 → 5 → 6 → 15 → ...

Remaining:
7 → sum = 7
→ Insert after 7

Final:
1 → 2 → 3 → 6 → 4 → 5 → 6 → 15 → 7 → 7

---

## Example 2:

Input:
0 → 6 → 1 → 5

K = 2

Group 1:
0 + 6 = 6 → insert

Group 2:
1 + 5 = 6 → insert

Final:
0 → 6 → 6 → 1 → 5 → 6
