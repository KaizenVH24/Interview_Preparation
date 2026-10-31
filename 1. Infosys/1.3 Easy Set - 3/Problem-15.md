## 15. Minimum Cost to Connect All Cables

You are given N cables of varying lengths. You need to connect all cables into a single cable. The cost of connecting two cables of length X and Y is X + Y. Find the minimum total cost to connect all N cables.

### Program

**Standard input / standard output**

### Input

Line 1: Integer N  
Line 2: N space-separated integers representing cable lengths

### Output

Print a single integer denoting minimum connection cost.

### Examples

**Example 1**

**Input:**
```text id="m5xq1v"
4
4 3 2 6
```

**Output:**
```text id="v7z2ak"
29
```

Connect 2 and 3 (cost 5) -> [4, 5, 6]. Connect 4 and 5 (cost 9) -> [6, 9]. Connect 6 and 9 (cost 15) -> Total = 5 + 9 + 15 = 29.

### Constraints

- 1 <= N <= 10^5
- 1 <= length[i] <= 10^6

---

### 1. Plain-English Problem Breakdown

Imagine you have several pieces of rope and you need to tie them all together into one giant rope. However, every time you tie two ropes together, you must pay a "tax" equal to their combined length.

If you tie your longest ropes together first, you will pay a massive tax on them. What's worse, that newly formed giant rope now goes back into the pile, and you will have to pay a massive tax on it *again* when you tie it to the next rope!

To keep your total tax as low as possible, your strategy must be greedy: **always find the two shortest ropes currently in your pile, and tie them together first.** You repeat this process until only one rope remains.

### 2. Optimal Python Solution

Because we need to repeatedly pull the smallest numbers from a constantly changing pile, using a standard array and sorting it over and over would be way too slow. The optimal data structure for this is a **Min-Heap** (Priority Queue).

**Python**

```python id="u2qv8k"
import sys
import heapq

def solve():
    # Read input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    
    # If there is only 1 cable, no connections are needed, cost is 0.
    if n == 1:
        print(0)
        return
        
    # Convert string inputs to integers
    cables = [int(x) for x in input_data[1:]]
    
    # Transform the list into a Min-Heap instantly
    heapq.heapify(cables)
    
    total_cost = 0
    
    # Continue connecting until only one massive cable remains
    while len(cables) > 1:
        # Pop the two smallest available cables
        first = heapq.heappop(cables)
        second = heapq.heappop(cables)
        
        # The cost to connect them
        current_cost = first + second
        total_cost += current_cost
        
        # Push the newly connected cable back into the heap
        heapq.heappush(cables, current_cost)
        
    print(total_cost)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N \log N)$. Turning the initial array into a heap takes $O(N)$ time. We then perform $N - 1$ merge operations. In each operation, we extract the minimums and insert the sum, which takes $O(\log N)$ time in a heap. For $10^5$ cables, this is lightning fast.
- **Space Complexity:** $O(N)$ to store the elements inside the heap structure.

### 3. Logic Walkthrough for a Layman

1. **The Setup:** We have cables of lengths `[4, 3, 2, 6]`. We throw them into a magical sorting box (the Min-Heap) which constantly pushes the smallest items to the top.
2. **First Tie:** The box hands us the two smallest ropes: `2` and `3`.
   - We tie them together to make a rope of length `5`.
   - We pay a tax of **5**.
   - We throw the new rope `5` back into the box.
3. **Second Tie:** The box now contains `[4, 5, 6]`. It hands us the smallest two: `4` and `5`.
   - We tie them together to make a rope of length `9`.
   - We pay a tax of **9**.
   - Our running total tax is now $5 + 9 = 14$.
   - We throw the `9` back into the box.
4. **Final Tie:** The box now contains `[6, 9]`. It hands us the only two left.
   - We tie them together to make a rope of length `15`.
   - We pay a tax of **15**.
   - Our running total tax is now $14 + 15 = 29$.
5. **The Result:** There is only one rope left, so we stop. The minimum possible cost is 29.

### 4. LeetCode Equivalent

This is a globally famous algorithmic problem that tests your knowledge of Priority Queues.

- **LeetCode 1167. Minimum Cost to Connect Sticks** (Medium - Premium) - Exact same problem.
- **LeetCode 1046. Last Stone Weight** (Easy) - Tests the exact same Min-Heap/Max-Heap push-and-pop logic.

### 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: What is a Priority Queue, and how is it different from a normal Queue?**

- **Answer:** A normal Queue follows the FIFO (First-In, First-Out) principle—whoever gets in line first is served first. A Priority Queue, however, serves elements based on their *priority* rather than their arrival time. Behind the scenes, it is usually implemented using a Heap data structure so that the element with the highest priority (e.g., the smallest number or largest number) is always instantly accessible at the front of the line.

**Q2: What is the OSI Model in Computer Networking?**

- **Answer:** The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes how computers communicate over a network. It is broken down into 7 layers, from physical hardware up to the user interface:
  1. Physical
  2. Data Link
  3. Network (where IP addresses live)
  4. Transport (where TCP/UDP live)
  5. Session
  6. Presentation
  7. Application (where HTTP and web browsers live).

**Q3: What are "Decorators" in Python?**

- **Answer:** A decorator is a design pattern in Python that allows you to modify or enhance the behavior of a function or class without permanently changing its source code. You place them above a function using the `@` symbol (like `@staticmethod`). Under the hood, a decorator takes a function as an input, wraps some extra code around it (like checking if a user is logged in before letting the function run), and returns the enhanced function.
