## Maximum Subarray Sum (Kadane's Algorithm)

Given an integer array arr of size N, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Program**

`Standard input / standard output`

### Input

Line 1: Integer N Line 2: N space-separated integers

### Output

Print the maximum subarray sum.

### Examples

**Example 1**

**Input:** `9 -2 1 -3 4 -1 2 1 -5 4`

**Output:** `6`

Contiguous subarray [4, -1, 2, 1] has the largest sum = 6.

### Constraints

- 1 <= N <= 10^5
- -10^9 <= arr[i] <= 10^9

---

### 1. Plain-English Problem Breakdown

Imagine you are playing a board game where you walk forward step-by-step. Some tiles give you points (positive numbers), and some tiles take points away (negative numbers). Your goal is to find the most profitable continuous streak of tiles on the board.

If you hit a patch of negative tiles that drains your score so heavily that you actually have a negative total, that entire streak has become a liability. You would be better off throwing away that history and starting completely fresh from the next tile. Kadane's Algorithm is just the mathematical version of this exact logic.

### 2. Optimal Python Solution

This problem is the hallmark use case for **Kadane's Algorithm**, which solves it in a single pass without needing nested loops.

**Python**

```python id="kdn53a"
import sys

def solve():
    # Read all input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    # Convert string inputs to integers
    arr = [int(x) for x in input_data[1:]]
    
    # Initialize both the current streak and maximum overall with the first element
    current_sum = arr[0]
    max_sum = arr[0]
    
    # Start iterating from the second element
    for num in arr[1:]:
        # Core Kadane's Logic: 
        # Do I attach this number to my existing streak, or is my streak 
        # so bad that I should just start a new streak with this number alone?
        current_sum = max(num, current_sum + num)
        
        # Did this new current_sum break our all-time record?
        max_sum = max(max_sum, current_sum)
        
    print(max_sum)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** \(O(N)\), where \(N\) is the number of elements in the array. The algorithm only requires a single scan of the array from left to right, making it incredibly fast even for \(10^5\) inputs.
- **Space Complexity:** \(O(N)\) strictly to store the input array in memory. The actual algorithmic logic uses \(O(1)\) constant space (just the `current_sum` and `max_sum` variables).

### 3. Logic Walkthrough for a Layman

1. **The Setup:** Let's take the array `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`. We start on the first tile (`-2`). Our current score is -2, and our all-time highest record is -2.
2. **The Decision (Tile 2):** We step on `1`. If we add it to our previous score of -2, we get -1. But if we just drop our past baggage and start a new streak starting with `1`, our score is 1. Since 1 is better than -1, we drop the past. Current score: 1. Record highest: 1.
3. **The Dip (Tile 3):** We step on `-3`. Current score becomes -2. We don't drop the past because starting fresh would mean starting at -3, and -2 is slightly better. Record highest remains 1.
4. **The Comeback (Tile 4):** We step on `4`. Our current score with baggage is 2 (from -2 + 4). But starting fresh gives us exactly 4. We drop the past again. Current score: 4. Record highest updates to 4!
5. **Riding the Wave:** We step on `-1` (score: 3), then `2` (score: 5, new record!), then `1` (score: 6, new record!).
6. By making this simple "keep the past or start fresh" decision at every single step, we effortlessly track the highest possible peak of 6.

### 4. LeetCode Equivalent

This is one of the most fundamental DSA questions asked in interviews globally.

- **LeetCode 53. Maximum Subarray** (Medium)

### 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: What is a Binary Search Tree (BST)?**

- **Answer:** A Binary Search Tree is a node-based data structure where each node has at most two children (left and right). Its defining property is that for any given node, all values in its left subtree are strictly smaller than the node's value, and all values in its right subtree are strictly greater. This makes searching for data extremely fast, taking \(O(\log N)\) time on average.

**Q2: What is the difference between "Pass by Value" and "Pass by Reference"?**

- **Answer:** **Pass by Value** means a copy of the actual data is passed to a function. If the function modifies that data, the original variable outside the function is totally unaffected (used for integers/strings in most languages). **Pass by Reference** means the memory address of the original data is passed. If the function changes the data, the original variable outside the function is modified as well (common for lists, arrays, and objects).

**Q3: What is a "Deadlock" in Operating Systems?**

- **Answer:** A deadlock is a situation where two or more processes are completely stuck because they are each waiting for a resource that the other process is currently holding. For example, Process A locks File 1 and needs File 2 to finish, while Process B locks File 2 and needs File 1 to finish. Neither can proceed, resulting in a frozen system until the OS intervenes.
