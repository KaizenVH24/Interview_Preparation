# 3. Minimum Platforms Required

Given the arrival and departure times of N trains at one railway station on the same day, find the minimum number of platforms needed so that no train waits. Times use valid 24-hour HHMM notation. A train occupies its platform through its departure time: if one train arrives at exactly the time another departs, the two trains require different platforms.

## Program

Standard input / standard output

### Input

- Line 1: Integer n (number of trains)
- Line 2: n space-separated integers representing arrival times
- Line 3: n space-separated integers representing departure times

### Output

Print a single integer denoting the minimum platforms needed.

### Examples

**Example 1**

**Input:**
```text
6
900 940 950 1100 1500 1800
910 1200 1120 1130 1900 2000
```

**Output:**
```text
3
```

At 11:00, the trains with intervals 09:40-12:00 and 09:50-11:20 are still present, and the 11:00-11:30 train has arrived. The maximum occupancy is 3, so 3 platforms are required.

### Constraints

- 1 <= n <= 100000
- Every time is a valid same-day HHMM value from 0000 through 2359, with minutes from 00 through 59
- arrival[i] <= departure[i] for every train
- An arrival at time T is processed before a departure at time T

## 1. Plain-English Problem Breakdown

Imagine standing on a bridge overlooking a railway station, simply counting the number of trains currently parked at the platforms.

You don't need to track the specific name of a train or map it to a specific platform. All you care about is the total volume of traffic at any given minute. Every time a train arrives, the station needs one more platform. Every time a train departs, the station frees up one platform.

If you separate all the arrival times and departure times into two separate timelines and organize them from earliest to latest, you can track the station's capacity exactly like a tally counter. The core catch is the tie-breaker: if Train A departs at 11:00 and Train B arrives at exactly 11:00, the station manager must assign Train B a completely different platform because Train A is still physically occupying its space until 11:01. Therefore, in the event of a time tie, arrivals must always be processed before departures.

## 2. Optimal Python Solution

We use the "Two Pointers" technique. By decoupling and sorting the arrivals and departures, we can solve this in $O(N \log N)$ time, completely avoiding nested loops.

**Python**

```python
import sys

def solve():
    # Read all input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    
    # Extract the arrival and departure times as integer lists
    # Since HHMM format scales numerically (e.g., 940 < 1100), 
    # we can safely treat them as standard integers for comparisons.
    arr = [int(x) for x in input_data[1 : n + 1]]
    dep = [int(x) for x in input_data[n + 1 : 2 * n + 1]]
    
    # Sort both arrays independently
    arr.sort()
    dep.sort()
    
    platforms_needed = 0
    max_platforms = 0
    
    # Use two pointers to traverse the timelines
    i = 0  # Pointer for arrivals
    j = 0  # Pointer for departures
    
    while i < n and j < n:
        # If the next event is an arrival (or a tie where arrival happens at the exact same minute)
        if arr[i] <= dep[j]:
            platforms_needed += 1
            # Update our all-time record of simultaneous trains
            if platforms_needed > max_platforms:
                max_platforms = platforms_needed
            i += 1
        # If the next event is a departure
        else:
            platforms_needed -= 1
            j += 1
            
    print(max_platforms)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N \log N)$ where $N$ is the number of trains. Sorting the arrival and departure arrays takes $O(N \log N)$ time. The `while` loop traverses the arrays using two pointers, taking $O(N)$ time. The sorting step dictates the overall speed.
- **Space Complexity:** $O(N)$ because we are storing the $N$ arrival times and $N$ departure times in memory arrays.

## 3. Logic Walkthrough for a Layman

1. **The Separation:** We don't look at a train's specific schedule (like arriving at 9:00 and leaving at 9:10). We rip the schedule apart into a master list of just Arrivals and a master list of just Departures.
2. **The Sorting:** We order both lists from morning to night.
   - Arrivals: `[900, 940, 950, 1100, 1500, 1800]`
   - Departures: `[910, 1120, 1130, 1200, 1900, 2000]`
3. **The Two Pointers (The Station Master):** We place one finger on the first arrival (`900`) and one finger on the first departure (`910`).
   - Which happens first? `900` is earlier. A train rolls in. We need **1** platform. We move our arrival finger to `940`.
   - Which happens next? `910` (departure) is earlier than `940` (arrival). A train leaves. Platforms needed drops to **0**. We move our departure finger to `1120`.
   - Next? `940` arrives. Platforms = **1**. Arrival finger moves to `950`.
   - Next? `950` arrives. Platforms = **2**. Arrival finger moves to `1100`.
   - Next? `1100` arrives. Platforms = **3**.
4. **The Tie-Breaker Rule:** If our arrival finger and departure finger point to the exact same time (e.g., both are `1120`), our code strictly processes the arrival first (`<=` logic), pushing the platform count up momentarily before bringing it back down, ensuring we don't accidentally crash two trains into the same platform.
5. **The Result:** Throughout the entire day, the platform count peaked at 3. We print 3.

## 4. LeetCode Equivalent

This is one of the most famous interval problems in technical interviews and serves as a fundamental benchmark for array manipulation.

- **LeetCode 253. Meeting Rooms II** (Medium - Premium) - Exact same problem logic, phrased as booking conference rooms instead of train platforms.

## 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: In Python, what is the exact difference between `append()` and `extend()` when working with lists?**

- **Answer:** **`append()`** adds its entire argument as a *single* element to the end of a list. If you append a list `[2, 3]` to `[1]`, you get a nested list `[1, [2, 3]]`. **`extend()`** iterates over its argument and adds each element individually to the end of the list. If you extend `[1]` with `[2, 3]`, you get a flat list `[1, 2, 3]`.

**Q2: What is the difference between a Clustered and a Non-Clustered Index in SQL?**

- **Answer:** A **Clustered Index** determines the actual physical sorting order of the data rows on the hard drive (like a phonebook sorted alphabetically). Because the data can only be physically sorted one way, a table can only have exactly *one* clustered index (usually the Primary Key). A **Non-Clustered Index** is a separate data structure stored away from the main table that contains pointers back to the physical rows (like an index at the back of a textbook). A table can have multiple non-clustered indexes to speed up various queries.

**Q3: Explain the difference between the Waterfall model and the Agile model in Software Engineering.**

- **Answer:** **Waterfall** is a traditional, linear, and sequential approach. You must completely finish one phase (like Design) before moving to the next (like Coding). It is rigid, making it very difficult and expensive to handle changing client requirements late in the project. **Agile** is an iterative and flexible approach. Development is broken down into short cycles called "Sprints" (usually 2 weeks). A working, incomplete version of the software is frequently delivered to the client for continuous feedback, making it highly adaptable to changes.
