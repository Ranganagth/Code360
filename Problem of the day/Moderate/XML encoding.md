# Intuition

XML encoding follows a **preorder traversal** of the element tree.

For each element:

```
TAG Attributes 0 ChildElements/Value 0
```

Meaning:

1. Encode element tag.
2. Encode all attributes `(tag value)`.
3. Write `0` to indicate end of attributes.
4. Encode either:

   * child elements recursively, or
   * the element value.
5. Write final `0` to indicate end of element.

This naturally maps to **recursive DFS traversal**.

---

# Approach

Recursive encoding:

For each `Element`:

1. Append encoded tag using `tagToInt`.
2. For each attribute:

   ```
   tagMapping attributeValue
   ```
3. Append `0` (end of attributes).
4. If children exist:

   * recursively encode each child.
5. Else if value exists:

   * append value.
6. Append `0` (end of element).

Use `StringBuilder` to build the result efficiently.

---

# Complexity

* **Time complexity:** (O(N)) where N = total elements + attributes
* **Space complexity:** (O(H)) recursion stack (tree height)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

	public static String encodeXML(Element element, HashMap<String, Integer> tagToInt) {
		StringBuilder sb = new StringBuilder();
		encode(element, tagToInt, sb);
		return sb.toString().trim();
	}

	private static void encode(Element element, HashMap<String, Integer> map, StringBuilder sb) {

		// Encode element tag
		sb.append(map.get(element.tag)).append(" ");

		// Encode attributes
		for (Attribute attr : element.attribute) {
			sb.append(map.get(attr.tag)).append(" ");
			sb.append(attr.value).append(" ");
		}

		// End of attributes
		sb.append("0 ");

		// Encode children or value
		if (element.child.size() > 0) {
			for (Element child : element.child) {
				encode(child, map, sb);
			}
		} else if (element.value != null && element.value.length() > 0) {
			sb.append(element.value).append(" ");
		}

		// End of element
		sb.append("0 ");
	}
};

```

---

# Example walkthrough with explanation

XML:

```
<codingNinjas course="InterviewPreparation" type="IndividualCourse">
    <topic name="Aptitude">CourseContents</topic>
</codingNinjas>
```

Tag mapping:

```
codingNinjas → 1
topic → 2
course → 3
type → 4
name → 5
```

Encoding steps:

```
1                (codingNinjas)
3 InterviewPreparation
4 IndividualCourse
0                (end attributes)

2                (topic)
5 Aptitude
0                (end attributes)

CourseContents
0                (end topic)

0                (end codingNinjas)
```

Result:

```
1 3 InterviewPreparation 4 IndividualCourse 0 2 5 Aptitude 0 CourseContents 0 0
```
