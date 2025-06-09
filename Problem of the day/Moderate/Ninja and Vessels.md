# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {

    static class State{
        int x, y, steps;

        State(int x, int y, int steps) {
            this.x = x;
            this.y = y;
            this.steps = steps;
        }

        @Override
        public int hashCode() {
            return Objects.hash(x, y);
        }

        @Override
        public boolean equals(Object obj) {
            if (this == obj) return true;
            if (!(obj instanceof State)) return false;
            State other = (State) obj;
            return this.x == other.x && this.y == other.y;
        }
    }

    public static int ninjaAndVessels(int m, int n, int w) {
        if (w > Math.max(m, n)) return -1;
        if (w % gcd(m, n) != 0) return -1;

        Queue<State> queue = new LinkedList<>();
        Set<State> visited = new HashSet<>();

        queue.add(new State(0, 0, 0));
        visited.add(new State(0, 0, 0));

        while (!queue.isEmpty()) {
            State curr = queue.poll();

            if (curr.x == w || curr.y == w) return curr.steps;

            List<State> nextStates = Arrays.asList(
                new State(m, curr.y, curr.steps + 1),
                new State(curr.x, n, curr.steps + 1),
                new State(0, curr.y, curr.steps + 1),
                new State(curr.x, 0, curr.steps + 1),

                new State(curr.x - Math.min(curr.x, n - curr.y), curr.y + Math.min(curr.x, n - curr.y), curr.steps + 1),
                new State(curr.x + Math.min(curr.y, m - curr.x), curr.y - Math.min(curr.y, m - curr.x), curr.steps + 1)
            );

            for (State next : nextStates) {
                if (!visited.contains(next)) {
                    visited.add(next);
                    queue.add(next);
                }
            }
        } 

        return -1;
    }

    private static int gcd(int a, int b) {
        if (b == 0) return a;
        return gcd(b, a % b);
    }
}

```