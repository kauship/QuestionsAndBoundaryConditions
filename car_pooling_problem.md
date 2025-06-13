**Problem: Car Pooling (Medium Difficulty)**

You are given a list of trips, where `trip[i] = [numPassengers, from, to]` represents a carpooling trip:

- `numPassengers` passengers must be picked up at `from` and dropped off at `to`.
- You have a car with a fixed `capacity`.

Return **true** if you can successfully pick up and drop off all passengers without exceeding the car's capacity at any time, otherwise return **false**.

---

### Java Function Signature

```java
public boolean carPooling(int[][] trips, int capacity)
```

---

### Java Solution

```java
import java.util.*;

public class CarPooling {
    public boolean carPooling(int[][] trips, int capacity) {
        TreeMap<Integer, Integer> timeline = new TreeMap<>();

        for (int[] trip : trips) {
            int numPassengers = trip[0];
            int from = trip[1];
            int to = trip[2];

            timeline.put(from, timeline.getOrDefault(from, 0) + numPassengers);
            timeline.put(to, timeline.getOrDefault(to, 0) - numPassengers);
        }

        int currentPassengers = 0;
        for (int passengerChange : timeline.values()) {
            currentPassengers += passengerChange;
            if (currentPassengers > capacity) {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {
        CarPooling cp = new CarPooling();
        System.out.println(cp.carPooling(new int[][]{{2, 1, 5}, {3, 3, 7}}, 4)); // false
        System.out.println(cp.carPooling(new int[][]{{2, 1, 5}, {3, 5, 7}}, 3)); // true
    }
}
```

---

### Test Cases

**Test Case 1: Basic Overlapping Violation**

```java
int[][] trips = {
    {2, 1, 5},
    {3, 3, 7}
};
int capacity = 4;
// Expected: false
```

**Test Case 2: Non-overlapping Trips**

```java
int[][] trips = {
    {2, 1, 5},
    {3, 5, 7}
};
int capacity = 3;
// Expected: true
```

**Test Case 3: Exact Capacity Used at Peak**

```java
int[][] trips = {
    {2, 0, 5},
    {1, 3, 7},
    {2, 5, 9}
};
int capacity = 3;
// Expected: true
```

**Test Case 4: Drop-off and Pickup at Same Time**

```java
int[][] trips = {
    {3, 2, 5},
    {3, 5, 7}
};
int capacity = 3;
// Expected: true
```

**Test Case 5: Multiple Drops Before Pickup**

```java
int[][] trips = {
    {2, 0, 3},
    {2, 3, 6},
    {1, 6, 8}
};
int capacity = 2;
// Expected: true
```

**Test Case 6: Zero Capacity**

```java
int[][] trips = {
    {1, 2, 3}
};
int capacity = 0;
// Expected: false
```

**Test Case 7: Large Number of Small Trips with High Frequency**

```java
int[][] trips = new int[1000][];
for (int i = 0; i < 1000; i++) {
    trips[i] = new int[]{1, i, i + 2};
}
int capacity = 1;
// Expected: true
```

**Test Case 8: Stress Capacity at Single Point**

```java
int[][] trips = {
    {1, 1, 10},
    {1, 2, 10},
    {1, 3, 10},
    {1, 4, 10},
    {1, 5, 10}
};
int capacity = 5;
// Expected: true
```

**Test Case 9: Peak Exceeds Capacity Temporarily**

```java
int[][] trips = {
    {2, 0, 5},
    {2, 2, 7},
    {2, 3, 9}
};
int capacity = 5;
// Expected: false
```

**Test Case 10: Unordered Trips**

```java
int[][] trips = {
    {3, 5, 10},
    {1, 0, 2},
    {2, 2, 5}
};
int capacity = 3;
// Expected: true
```

