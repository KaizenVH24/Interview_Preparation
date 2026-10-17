## Minimize Unique Elements after K Deletions

You are given an integer array arr and an integer K representing the number of deletions allowed. You can delete any K elements from the array.
Find the minimum number of unique elements remaining in the array after removing exactly K elements.

**Program**

`Standard input / standard output`

### Input

Line 1: Integer K (number of elements to delete) Line 2: Space-separated integers representing the array

### Output

Print the minimum number of unique elements remaining.

### Examples

**Example 1**

**Input:** `4 1 1 1 2 2 3 3 4 5 6`

**Output:** `3`

Frequencies: 1:3, 2:2, 3:2, 4:1, 5:1, 6:1. Remove 4, 5, 6 (3 deletions) and one from 2 or 3 (1 deletion). Unique remaining: {1, 2, 3} -> 3 unique elements.

### Constraints

- 1 <= K <= len(arr) <= 10^5

---

### 1. Plain-English Problem Breakdown

Imagine you have several jars of different colored candies, and you are allowed to eat exactly `K` candies in total. Your goal is to completely empty as many jars as possible so that the number of jars with at least one candy left is as small as possible.

To do this efficiently, which jars should you target first?

You should target the jars that have the *fewest* candies. If a jar only has 1 candy, it only costs you 1 "eat" to eliminate that color completely. If you waste your `K` eats on a jar holding 100 candies, you might not even empty it, meaning that color still exists. The strategy is purely greedy: always eliminate the rarest elements first.

### 2. Optimal Python Solution

We use a Hash Map (Counter) to count the frequencies, sort those frequencies from smallest to largest, and subtract them from our allowed `K` deletions until we run out of `K`.

**Python**

```python id="rm8nqv"
import sys
from collections import Counter

def solve():
    # Read all input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    k = int(input_data[0])
    # The remaining elements form our array
    arr = input_data[1:] 
    
    # Count how many times each element appears
    # Example: ['1', '1', '2', '2', '2'] -> {'1': 2, '2': 3}
    frequencies = Counter(arr)
    
    # Extract just the frequency counts and sort them from smallest to largest
    # Example counts: [2, 3]
    counts = sorted(frequencies.values())
    
    total_unique = len(counts)
    removed_unique = 0
    
    # Greedily remove elements starting with the lowest frequency
    for count in counts:
        if k >= count:
            k -= count  # "Spend" our allowed deletions
            removed_unique += 1 # We completely eliminated one unique element
        else:
            # Not enough deletions left to completely remove the next element
            break
            
    # Print the remaining number of unique elements
    print(total_unique - removed_unique)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** \(O(N \log N)\) where \(N\) is the number of elements in the array. Counting the frequencies takes \(O(N)\) time. Sorting the frequency values takes \(O(U \log U)\) time, where \(U\) is the number of *unique* elements. In the worst case, \(U = N\), so sorting dominates the time complexity.
- **Space Complexity:** \(O(N)\) to store the frequency dictionary and the list of counts.

### 3. Logic Walkthrough for a Layman

1. **Take Inventory:** First, we look at the array and count how many times each number appears. If our array is `[4, 5, 6, 1, 1, 1, 2, 2, 3, 3]`, our inventory says: 4 appears 1 time, 5 appears 1 time, 6 appears 1 time, 2 appears 2 times, 3 appears 2 times, and 1 appears 3 times. We currently have **6** unique numbers.
2. **Line Up by Rarity:** We strip away the actual numbers and just look at the counts. We line these counts up from smallest to largest: `[1, 1, 1, 2, 2, 3]`.
3. **The Eraser (Greedy Logic):** We have \(K = 4\) erasers.
   - We look at the first group (size 1). We use 1 eraser. \(K\) is now 3. We successfully eliminated a unique number!
   - We look at the second group (size 1). We use 1 eraser. \(K\) is now 2. Eliminated another!
   - We look at the third group (size 1). We use 1 eraser. \(K\) is now 1. Eliminated a third!
   - We look at the fourth group (size 2). We only have 1 eraser left, so we can't completely wipe this group out. We stop.
4. **Final Tally:** We started with 6 unique numbers. We completely wiped out 3 of them. \(6 - 3 = 3\). There are exactly 3 unique numbers left.

### 4. LeetCode Equivalent

This is a direct mapping to a popular LeetCode Medium problem that frequently appears in OA (Online Assessment) rounds.

- **LeetCode 1481. Least Number of Unique Integers after K Removals** (Medium)

### 5. Infosys Technical Interview Questions (Fresher Round)

Since this solution uses a "Greedy" approach, interviewers will often test your knowledge of algorithm design paradigms and backend basics.

**Q1: What is the difference between a Greedy Algorithm and Dynamic Programming?**

- **Answer:** A **Greedy Algorithm** builds a solution step-by-step by always choosing the next option that offers the most immediate, obvious benefit (the "local optimum"). It does not look back or reconsider its choices. **Dynamic Programming (DP)**, on the other hand, breaks a problem down into overlapping subproblems, solves each one, remembers the answers, and uses them to guarantee the absolute best overall solution (the "global optimum"). Greedy is faster, but DP is safer for complex paths.

**Q2: What is the `__init__` method in Python?**

- **Answer:** The `__init__` method is a reserved method in Python classes, known as the constructor. It is automatically called the exact moment you create a new object (instance) from a class. It is primarily used to initialize the object's attributes with default or user-provided values.

**Q3: What is Database Indexing, and why is it used?**

- **Answer:** An index in a database is a data structure (usually a B-Tree) that improves the speed of data retrieval operations. Think of it like the index at the back of a textbook—instead of scanning every single page (a "full table scan") to find a topic, the database looks at the index to instantly find the exact disk location of the row. While indexing makes reading `SELECT` queries much faster, it slightly slows down `INSERT`, `UPDATE`, and `DELETE` queries because the index itself must also be updated.
