# Intuition

We are given two linked lists of equal length `N`. Our goal is to insert nodes of the **second linked list** into the **first linked list** at alternate positions.

So for:
```
List1: 1 -> 3 -> 5
List2: 2 -> 4 -> 6
```

After merging:
```
Result: 1 -> 2 -> 3 -> 4 -> 5 -> 6
```

---

# Approach

1. Use two pointers `first` and `second` to traverse both linked lists.
2. For each node in `first`, insert a node from `second` right after it.
3. Update pointers accordingly: save `first.next` and `second.next` before overwriting them.
4. The second list is **exhausted** into the first one; we don’t need to return anything if we modify in-place.

### Edge Cases

- Either of the heads is `null`.
- Only one element in each list.
- Large lists (ensure O(N) time).

---

# Complexity Analysis

- **Time Complexity:** O(N), where N is the number of nodes in either list.
- **Space Complexity:** O(1), in-place pointer manipulation.

---

# Code

```java
class Node {
    int data;
    Node next;

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

public class Solution {

    public static void merge(Node head1, Node head2) {
        Node first = head1, second = head2;

        // Loop until either list ends
        while (first != null && second != null) {
            // Save next pointers
            Node f_next = first.next;
            Node s_next = second.next;

            // Insert second node into first list
            first.next = second;
            second.next = f_next;

            // Move both pointers ahead
            first = f_next;
            second = s_next;
        }
    }

    // Optional: Utility to print list (for testing)
    public static void printList(Node head) {
        while (head != null) {
            System.out.print(head.data + " ");
            head = head.next;
        }
        System.out.println();
    }
}
```

---

### **Sample Test Case Execution**

#### Input
```
List1: 1 -> 3 -> 5
List2: 2 -> 4 -> 6
```

#### Call
```java
merge(head1, head2);
printList(head1); // Output: 1 2 3 4 5 6
```

---
