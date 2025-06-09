# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {

    static class Event implements Comparable<Event> {
        int x, y1, y2, type;
        public Event(int x, int y1, int y2, int type) {
            this.x = x;
            this.y1 = y1;
            this.y2 = y2;
            this.type = type;
        }

        public int compareTo(Event other) {
            return Integer.compare(this.x, other.x);
        }
    }

    private static final int MOD = 1_000_000_007;

    public static int rectangleArea(int n, int[][] rectangles)  {
        List<Event> events = new ArrayList<>();

        for (int[] rect : rectangles) {
            int x1 = rect[0], y1 = rect[1], x2 = rect[2], y2 = rect[3];

            events.add(new Event(x1, y1, y2, 1));
            events.add(new Event(x2, y1, y2, -1));
            }

            Collections.sort(events);

            TreeMap<int[], Integer> active = new TreeMap<>((a, b) -> {
                if (a[0] != b[0]) return a[0] - b[0];
                return a[1] - b[1];
            });

            int prevX = events.get(0).x;
            long area = 0;

            for (Event event : events) {
                int currX = event.x;
                long ySum = mergeIntervals(active.keySet());
                area = (area + (currX - prevX) * ySum) % MOD;

                int[] interval = new int[]{event.y1, event.y2};

                if (event.type == 1) {
                    active.put(interval, active.getOrDefault(interval, 0) + 1);
                } else {
                    int count = active.get(interval);
                    if (count == 1) active.remove(interval);
                    else active.put(interval, count - 1);
                }
                prevX = currX;
            }

            return (int) area;

    }

    private static long mergeIntervals(Set<int[]> intervals) {
        long total = 0;
        int prevStart = -1, prevEnd = -1;
        for (int[] interval : intervals) {
            if (interval[0] > prevEnd) {
                total += prevEnd - prevStart;
                prevStart = interval[0];
                prevEnd = interval[1];
            } else {
                prevEnd = Math.max(prevEnd, interval[1]);
            }
        }
        total += prevEnd - prevStart;
        return total;
    }
}

```