

# 8. Matrix 4-in-a-Row Consecutive Number Finder

Given an R x C integer matrix, search horizontally, vertically, and diagonally (both principal and anti-diagonals) for any number that repeats at least 4 consecutive times in a line.

Collect all such numbers that repeat 4 consecutive times and return the minimum among them. If no number repeats 4 consecutive times, return -1.

## Program

**Standard input / standard output**

### Input

- Line 1: Two space-separated integers `R` and `C`.
- Next `R` lines: `C` space-separated integers for each row.

### Output

Print the minimum repeated number or -1.

### Examples

**Example 1**

**Input:**
```text
4 4
1 1 1 1
2 3 4 5
6 7 8 9
0 0 0 0
```

**Output:**
```text
0
```

Row 0 has '1' repeating 4 times. Row 3 has '0' repeating 4 times. Candidates = {0, 1}. Minimum = 0.

### Constraints

- `4 <= R, C <= 100`

---

## 1. Plain-English Problem Breakdown

Imagine playing a giant game of Connect-4 on a grid of numbers. You are looking for any number that manages to line up at least 4 of its identical copies in a row. This line can go straight across (horizontal), straight down (vertical), or diagonally in both directions.

Because the grid can be large, you might find multiple different numbers that successfully got 4-in-a-row (for example, the number `1` might have a horizontal line, and the number `0` might have a vertical line). Your goal is to gather all the winning numbers into a list, pick the mathematically smallest number among them, and print it. If absolutely no number gets 4-in-a-row, you print `-1`.

## 2. Optimal Python Solution

Instead of tracing complex paths, we can just treat every single cell as a potential "starting point" for a 4-in-a-row sequence. From each cell, we only need to check exactly 4 directions: Right, Down, Diagonal-Right, and Diagonal-Left. (We don't need to check Up or Left, because those sequences would have already been found when we were visiting the earlier cells).

**Python**

```python
import sys

def solve():
    # Read all inputs from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    r = int(input_data[0])
    c = int(input_data[1])
    
    # Reconstruct the 2D matrix
    idx = 2
    matrix = []
    for _ in range(r):
        row = []
        for _ in range(c):
            row.append(int(input_data[idx]))
            idx += 1
        matrix.append(row)
        
    candidates = set()
    
    # Traverse every cell in the matrix
    for i in range(r):
        for j in range(c):
            val = matrix[i][j]
            
            # Check Horizontal (Right) -> Make sure we have 3 spaces to the right
            if j + 3 < c:
                if val == matrix[i][j+1] == matrix[i][j+2] == matrix[i][j+3]:
                    candidates.add(val)
            
            # Check Vertical (Down) -> Make sure we have 3 spaces down
            if i + 3 < r:
                if val == matrix[i+1][j] == matrix[i+2][j] == matrix[i+3][j]:
                    candidates.add(val)
                    
            # Check Diagonal Right-Down (\) -> 3 spaces right AND 3 spaces down
            if i + 3 < r and j + 3 < c:
                if val == matrix[i+1][j+1] == matrix[i+2][j+2] == matrix[i+3][j+3]:
                    candidates.add(val)
                    
            # Check Anti-Diagonal Left-Down (/) -> 3 spaces left AND 3 spaces down
            if i + 3 < r and j - 3 >= 0:
                if val == matrix[i+1][j-1] == matrix[i+2][j-2] == matrix[i+3][j-3]:
                    candidates.add(val)
                    
    # Print the minimum candidate, or -1 if the set is empty
    if candidates:
        print(min(candidates))
    else:
        print("-1")

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(R \times C)$ where $R$ is rows and $C$ is columns. We visit each cell exactly once and perform at most 4 quick equality checks. For the maximum constraints of $100 \times 100$, this totals 10,000 cell visits and executes in less than 5 milliseconds.
- **Space Complexity:** $O(R \times C)$ to store the 2D matrix in memory. The `candidates` set uses $O(U)$ space where $U$ is the number of unique winning numbers, which is negligible.

## 3. Logic Walkthrough for a Layman

1. **The Starting Point:** We start at the very top-left corner of the grid (let's say it's row 0, column 0, and the number is `1`).
2. **The 4 Checks:** From this `1`, we look ahead like a radar:
   - Can we go 3 steps Right? If yes, are they all `1`?
   - Can we go 3 steps Down? If yes, are they all `1`?
   - Can we go 3 steps Down-Right? If yes, are they all `1`?
   - Can we go 3 steps Down-Left? (No, we are at the left edge, so we skip this check).
3. **The Sweep:** We move to the next number in the row and do the exact same 4 checks. We sweep through the entire board reading left-to-right, top-to-bottom.
4. **The Collection:** Every time one of our radar checks successfully hits four identical numbers in a row, we throw that number into a "winners basket" (a Set, so duplicate wins by the same number are ignored).
5. **The Final Verdict:** Once we finish sweeping the bottom-right corner, we look in our basket. If it has `{1, 0}`, we print the smallest one (`0`). If the basket is empty, we print `-1`.

## 4. LeetCode Equivalent

This simulates standard grid traversal combined with game-board logic.

- **LeetCode 1275. Find Winner on a Tic Tac Toe Game** (Easy) - Shares the exact same row/column/diagonal sequence-checking mechanics.
- **LeetCode 79. Word Search** (Medium) - Applies similar horizontal/vertical grid searching logic.

## 5. Infosys Technical Interview Questions (Fresher Round)

Matrix problems test your understanding of multi-dimensional data structures. Expect the interviewer to transition into nested loops, algorithmic limits, and core web architecture.

**Q1: What is a RESTful Web Service?**

- **Answer:** A RESTful Web Service is a lightweight, scalable architectural style for building APIs. It uses standard HTTP protocols (GET, POST, PUT, DELETE) to allow different software applications to communicate over the internet. It is "stateless," meaning the server does not store any information about the client's past requests—every request must contain all the necessary data to process it independently.

**Q2: In Database Management, what are Primary Keys and Foreign Keys?**

- **Answer:** A **Primary Key** is a specific column in a database table that uniquely identifies every single row (like a Student ID). It cannot contain duplicate values and cannot be `NULL`. A **Foreign Key** is a column in one table that links directly to the Primary Key of another table. It is used to establish relationships between tables (for example, a `Department_ID` in the Students table that points to the main Departments table) to maintain referential integrity.

**Q3: What is the difference between a `List` and a `Set` in Python?**

- **Answer:** Both are built-in data collections, but they behave differently. A **List** is ordered (maintains the exact sequence you inserted elements) and allows duplicate values. It uses square brackets `[]`. A **Set** is unordered (it doesn't keep track of insertion sequence) and strictly prohibits duplicate values. It uses curly braces `{}` and is highly optimized for quickly checking if an item exists ($O(1)$ lookup time).

