

# 6. Summer Array Even-Odd Rearrangement Swaps

Given an array of integers `arr` of length `N`, find the minimum number of adjacent swaps required to rearrange the array such that all even numbers appear before all odd numbers (i.e. all even elements on the left, all odd elements on the right). The relative internal order of even elements among themselves and odd elements among themselves should be preserved (stable).

## Program

**Standard input / standard output**

### Input

- Line 1: Integer `N`
- Line 2: `N` space-separated integers

### Output

Print the minimum number of adjacent swaps required.

### Examples

**Example 1**

**Input:**
```text
4
1 2 3 4
```

**Output:**
```text
3
```

There are three odd-before-even inversions.

**Example 2**

**Input:**
```text
4
2 4 1 3
```

**Output:**
```text
0
```

Already partitioned.

### Constraints

- `1 <= N <= 10^5`
- `-10^9 <= arr[i] <= 10^9`

---

## 1. Plain-English Problem Breakdown

Imagine you have a line of students, some wearing blue shirts (even numbers) and some wearing red shirts (odd numbers). Your goal is to move all the blue shirts to the front of the line and all the red shirts to the back.

However, you can only swap two students if they are standing right next to each other. Furthermore, you cannot mess up the original order of the blue-shirted students among themselves, nor the red-shirted students among themselves.

Every time a red-shirted student (an odd number) is standing in front of a blue-shirted student (an even number), they are in the wrong place relative to each other. To fix this, you must swap them. Because you are only doing adjacent swaps, the total number of swaps you must perform is exactly equal to the total number of times an odd number appears before an even number in the original line.

## 2. Optimal Python Solution

We do not need to physically perform the swaps or write a complicated sorting algorithm. Since an even number must hop over every single odd number that precedes it to reach the front, the total number of swaps is simply the sum of all "odd numbers seen so far" for every even number we encounter.

**Python**

```python
import sys

def solve():
    # Read all inputs from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    
    # Process the array element by element
    # We don't actually need to store the whole array in memory if we don't want to,
    # but storing it is fine within N=10^5 constraints.
    arr = [int(x) for x in input_data[1 : n + 1]]
    
    odd_count = 0
    total_swaps = 0
    
    # Traverse the array from left to right
    for num in arr:
        if num % 2 != 0:
            # We found an odd number. Increase our tally.
            odd_count += 1
        else:
            # We found an even number. 
            # To get to its correct position on the left, this even number 
            # must hop over every single odd number we have seen so far.
            total_swaps += odd_count
            
    print(total_swaps)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N)$ where $N$ is the number of elements. We loop through the array exactly once, performing simple arithmetic. For $10^5$ elements, this runs in a tiny fraction of a millisecond.
- **Space Complexity:** $O(N)$ to store the array. The swap-counting logic itself runs in $O(1)$ constant extra space.

## 3. Logic Walkthrough for a Layman

Let's use **Example 1**: `[1, 2, 3, 4]`.

1. **The Setup:** We keep a tally called `odd_count` starting at 0, and a tally called `total_swaps` starting at 0. We walk from left to right.
2. **Step 1:** We look at `1`. It's an odd number. We increase our `odd_count` to 1.
3. **Step 2:** We look at `2`. It's an even number. To move `2` to the front, it has to physically swap places with every odd number currently blocking it. How many odd numbers are blocking it? Just 1 (the number `1`). We add `odd_count` (which is 1) to our `total_swaps`. `total_swaps` is now 1.
4. **Step 3:** We look at `3`. It's an odd number. We increase our `odd_count` to 2.
5. **Step 4:** We look at `4`. It's an even number. How many odd numbers are currently standing in front of it? Well, `1` and `3` are in front of it. So our `odd_count` is 2. `4` must hop over both of them. We add 2 to our `total_swaps`. `total_swaps` is now 3.
6. **The Result:** We finished the line. The absolute minimum number of adjacent swaps required is 3.

## 4. LeetCode Equivalent

This specific logic is a simplified 1D application of "counting inversions".

- **LeetCode 2136. Minimum Swaps to Group All 1's Together II** (Medium) - While dealing with a circular array, the core concept of counting the opposing element to calculate swap/move distance is highly related.
- **GeeksForGeeks / InterviewBit:** "Minimum Swaps required to group all 1s together" uses this exact $O(N)$ counting logic.

## 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: What is a View in SQL?**

- **Answer:** A View is a "virtual table" based on the result-set of an SQL statement. It doesn't actually store data on the hard drive itself; instead, it saves a complex query (like joining 4 different tables) under a simple name. When a user runs `SELECT * FROM MyView`, the database executes that complex query in the background. It is heavily used for security (hiding sensitive columns from certain users) and simplifying complex queries for reporting.

**Q2: In Python, how is memory managed, and what is the Global Interpreter Lock (GIL)?**

- **Answer:** Python manages memory automatically using **Reference Counting** (tracking how many variables point to an object) and a **Garbage Collector** to clean up objects that are no longer used. The **GIL (Global Interpreter Lock)** is a mutex that protects access to Python objects, preventing multiple threads from executing Python bytecodes at once. This means even on a multi-core processor, standard Python multithreading can only execute one thread at a time, making it bad for CPU-heavy tasks but perfectly fine for I/O-heavy tasks (like web scraping).

**Q3: Explain the difference between `==` and `equals()` in Java.**

- **Answer:** In Java, the `==` operator is used to compare primitives (like `int` or `char`), and for Objects, it strictly checks **Memory Reference Equality** (do both variables point to the exact same physical object in memory?). The `.equals()` method is used to check **Value Equality** (do the two separate objects contain the exact same data?), which is why you must always use `.equals()` to compare Strings in Java.

