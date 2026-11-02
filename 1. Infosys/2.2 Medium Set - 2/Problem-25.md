

# 10. Matrix Zero Replacement by Divisible Row/Column Sum

Count the zero and nonzero cells before making any change. If zeros do not strictly outnumber nonzeros, print the matrix unchanged. Otherwise, process zero cells in row-major order. For each processed zero, compute its current row and column sums. Use the larger sum, or the column sum on a tie, and replace the zero by the smallest positive X that makes that chosen sum plus X divisible by 3. Stop immediately once nonzeros outnumber zeros, then print the whole matrix.

## Program

**Standard input / standard output**

### Input

- Line 1: Two integers `M` and `N` (rows and columns).
- Next `M` lines: `N` space-separated integers for each row.

### Output

Print the modified matrix row by row.

### Examples

**Example 1**

**Input:**
```text id="v8n2qx"
3 3
0 0 1
0 2 0
0 0 0
```

**Output:**
```text id="k4m7pt"
2 3 1
1 2 0
0 0 0
```

Three row-major replacements make nonzeros outnumber zeros.

**Example 2**

**Input:**
```text id="w5r9yb"
1 1
0
```

**Output:**
```text id="c2j6zf"
3
```

The smallest positive value divisible by 3 is 3.

### Constraints

- `1 <= M, N <= 100`
- `0 <= matrix[i][j] <= 10^4`

---

## 1. Plain-English Problem Breakdown

Imagine a grid of numbers. Your first task is to take a quick census: how many cells contain exactly `0`, and how many contain a number greater than 0? If the grid doesn't have more `0`s than actual numbers, you refuse to do anything and just print the original grid.

However, if `0`s dominate the board, you begin a repair process reading left-to-right, top-to-bottom. Whenever you encounter a `0`, you do the following:

1. Add up all the numbers currently in that specific row.
2. Add up all the numbers currently in that specific column.
3. Compare the two totals. Pick the larger one (if they are tied, pick the column total).
4. Now, find the absolute smallest positive integer (1, 2, or 3) you can add to that chosen total so that it becomes perfectly divisible by 3.
5. Replace the `0` with that small integer.
6. Immediately after replacing the `0`, update your census. The moment the actual numbers finally outnumber the `0`s, you drop your tools and immediately print the board exactly as it currently looks.

## 2. Optimal Python Solution

Because we must update row and column sums dynamically (the numbers we inject change the sums for future `0`s), we can maintain running arrays of row sums and column sums to keep calculations instantaneous.

**Python**

```python id="q6t3mz"
import sys

def solve():
    # Read all input data
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    m = int(input_data[0])
    n = int(input_data[1])
    
    # Reconstruct the matrix
    idx = 2
    matrix = []
    zero_count = 0
    nonzero_count = 0
    
    # Track the sum of each row and column dynamically
    row_sums = [0] * m
    col_sums = [0] * n
    
    for r in range(m):
        row = []
        for c in range(n):
            val = int(input_data[idx])
            row.append(val)
            idx += 1
            
            # Census tracking
            if val == 0:
                zero_count += 1
            else:
                nonzero_count += 1
                
            # Initial sum calculation
            row_sums[r] += val
            col_sums[c] += val
            
        matrix.append(row)
        
    # Rule 1: If zeros do not strictly outnumber nonzeros, do nothing.
    if zero_count <= nonzero_count:
        for row in matrix:
            print(" ".join(map(str, row)))
        return
        
    # Rule 2: Process in row-major order (left-to-right, top-to-bottom)
    for r in range(m):
        for c in range(n):
            if matrix[r][c] == 0:
                # Get current sums
                r_sum = row_sums[r]
                c_sum = col_sums[c]
                
                # Pick the larger sum (or column sum on a tie)
                if r_sum > c_sum:
                    chosen_sum = r_sum
                else:
                    chosen_sum = c_sum
                    
                # Find the smallest positive integer X (1, 2, or 3) 
                # such that (chosen_sum + X) is divisible by 3.
                remainder = chosen_sum % 3
                if remainder == 0:
                    x = 3
                elif remainder == 1:
                    x = 2
                else:  # remainder == 2
                    x = 1
                    
                # Replace the zero with X
                matrix[r][c] = x
                
                # Dynamically update our trackers
                row_sums[r] += x
                col_sums[c] += x
                zero_count -= 1
                nonzero_count += 1
                
                # Rule 3: Stop IMMEDIATELY once nonzeros strictly outnumber zeros
                if nonzero_count > zero_count:
                    for final_row in matrix:
                        print(" ".join(map(str, final_row)))
                    return
                    
    # Fallback in case we process the whole matrix (though the loop should break first)
    for row in matrix:
        print(" ".join(map(str, row)))

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(M \times N)$ where $M$ is rows and $N$ is columns. We scan the matrix once to set it up, and scan it again to process the zeroes. Because we maintain `row_sums` and `col_sums` arrays, finding the sum for a zero takes $O(1)$ time instead of recalculating the entire row and column every time ($O(M+N)$). For a maximum $100 \times 100$ grid, execution is practically instant.
- **Space Complexity:** $O(M \times N)$ to hold the 2D matrix structure. The dynamic tracking arrays take a negligible $O(M + N)$ space.

## 3. Logic Walkthrough for a Layman

Let's use **Example 2**: A tiny `1x1` grid with just a single `0`.

1. **The Census:** Zeroes = 1, Non-zeroes = 0. Do zeroes strictly outnumber non-zeroes? Yes (1 > 0). The repair begins.
2. **The First Zero:** We look at the first (and only) `0`.
3. **The Sums:** The row sum is `0`. The column sum is `0`. They are tied. We pick the column sum (`0`).
4. **The Divisibility Math:** We need to find the smallest positive number ($X > 0$) that, when added to `0`, creates a number cleanly divisible by 3.
   - Try $X = 1$: $0 + 1 = 1$. Not divisible by 3.
   - Try $X = 2$: $0 + 2 = 2$. Not divisible by 3.
   - Try $X = 3$: $0 + 3 = 3$. Perfectly divisible by 3!
5. **The Replacement:** We replace the `0` with `3`.
6. **The Check:** We recount. Zeroes = 0, Non-zeroes = 1. Non-zeroes now strictly outnumber zeroes (1 > 0).
7. **The Halt:** We immediately drop our tools and print the final grid: `3`.

## 4. LeetCode Equivalent

This problem is a classic "Simulation" problem that tests your ability to follow extremely precise, multi-step instructions without cutting logical corners.

- **LeetCode 1572. Matrix Diagonal Sum** (Easy) - Tests basic matrix iteration and sum tracking.
- **LeetCode 73. Set Matrix Zeroes** (Medium) - Features the exact same requirement to track row/column states dynamically to avoid "chain reactions" from modifying the board in real-time.

## 5. Infosys Technical Interview Questions (Fresher Round)

Grid and matrix tracking logic naturally leads into questions about data structures and memory boundaries.

**Q1: What is the difference between an Array and a List in Python?**

- **Answer:** In standard Python, people use the term "List" (created with `[]`). A Python List is dynamic—it can grow or shrink automatically, and it can hold multiple different data types at once (like `[1, "hello", 3.14]`). A true "Array" in computer science (which you can import in Python using the `array` module or `NumPy`) is static. It strictly requires all elements to be of the exact same data type (like all integers), making it much more memory-efficient and faster for heavy mathematical computations.

**Q2: Can you explain the concept of a "Foreign Key" constraint?**

- **Answer:** A Foreign Key constraint is a database rule that links a column in one table directly to the Primary Key of another table. It strictly enforces "Referential Integrity". This means if Table B has a Foreign Key pointing to a record in Table A, you absolutely cannot delete the record in Table A until you deal with the child record in Table B. It prevents "orphan" records.

**Q3: What are the main differences between TCP and UDP protocols?**

- **Answer:** Both are Transport Layer protocols used for transmitting data over the internet.
  - **TCP (Transmission Control Protocol):** Connection-oriented. It establishes a strict connection before sending data. It guarantees delivery, checks for errors, and ensures packets arrive in the correct order. It is reliable but slower (used for Web Browsing, Emails, File Transfers).
  - **UDP (User Datagram Protocol):** Connectionless. It simply fires the data at the destination without checking if the receiver is ready or if the data actually arrived. It is unreliable but incredibly fast (used for Live Video Streaming, Online Gaming, VoIP calls).

