# Intuition

A deque supports insertion and deletion from both ends.
To simulate a **stack (LIFO)**:

* Treat **front of deque as stack top**
* Push → add to front
* Pop → remove from front
* Top → peek front

---

# Approach

Use `ArrayDeque`:

* `push(x)` → `addFirst(x)`
* `pop()` → `pollFirst()`
* `top()` → `peekFirst()`
* `isEmpty()` → `isEmpty()`
* `size()` → `size()`

---

# Complexity

* **All operations:** $O(1)$

---
# Code

```java id="k9x3zp"
import java.util.*;
import java.io.*;

public class Stack {

	Deque<Integer> dq;

	Stack() {
		dq = new ArrayDeque<>();
	}

	public boolean push(int x) {
		dq.addFirst(x);
		return true;
	}

	public int pop() {
		if (dq.isEmpty()) return -1;
		return dq.pollFirst();
	}

	public int top() {
		if (dq.isEmpty()) return -1;
		return dq.peekFirst();
	}

	public boolean isEmpty() {
		return dq.isEmpty();
	}

	public int size() {
		return dq.size();
	}
};

```

---

# Example walkthrough

Operations:

```text
push(10) → [10]
push(20) → [20,10]
```

```text
top() → 20
pop() → 20
top() → 10
```
