# Intuition

Split the list into two stable streams: one for even-valued nodes, one for odd-valued nodes. Traverse once, append each node to its appropriate stream without altering the original relative order. Finally, join the even stream in front of the odd stream.

---

# Approach

1. Maintain four pointers: `evenHead`, `evenTail`, `oddHead`, `oddTail`.
2. Walk through the original list node by node.
3. If a node’s value is even, attach it to the even list; otherwise attach it to the odd list.
4. After processing all nodes:
   • If no even nodes exist, result is the odd list.
   • If no odd nodes exist, result is the even list.
   • Otherwise, link `evenTail.next = oddHead`.
5. Terminate the tail of the odd list with `null`.

---

# Complexity

• **Time:** O(N), one linear pass.
• **Space:** O(1), reusing existing nodes, no auxiliary structures.

---

# Code

```java
public class Solution {
    public static Node segregateEvenOdd(Node head) {

        Node evenHead = null, evenTail = null;
        Node oddHead = null, oddTail = null;

        Node cur = head;

        while (cur != null) {
            if (cur.data % 2 == 0) {
                if (evenHead == null) {
                    evenHead = cur;
                    evenTail = cur;
                } else {
                    evenTail.next = cur;
                    evenTail = cur;
                }
            } else {
                if (oddHead == null) {
                    oddHead = cur;
                    oddTail = cur;
                } else {
                    oddTail.next = cur;
                    oddTail = cur;
                }
            }
            cur = cur.next;
        }

        if (evenHead == null) return oddHead;
        if (oddHead == null) return evenHead;

        evenTail.next = oddHead;
        oddTail.next = null;

        return evenHead;
    }
};

```

---

# Example Walkthrough

**Input list:**
`2 -> 1 -> 3 -> 5 -> 6 -> 4 -> 7`

**Traversal:**

• Node 2: even → even list becomes `2`
• Node 1: odd  → odd list becomes `1`
• Node 3: odd  → odd list becomes `1 -> 3`
• Node 5: odd  → odd list becomes `1 -> 3 -> 5`
• Node 6: even → even list becomes `2 -> 6`
• Node 4: even → even list becomes `2 -> 6 -> 4`
• Node 7: odd  → odd list becomes `1 -> 3 -> 5 -> 7`

**Final join:**
`even: 2 -> 6 -> 4`
`odd:  1 -> 3 -> 5 -> 7`

**Result:**
`2 -> 6 -> 4 -> 1 -> 3 -> 5 -> 7`
