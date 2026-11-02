# 4. Wine Trading in Gergovia

Gergovia consists of N wine traders in a single line. Each inhabitant i has a demand A[i]. If A[i] > 0, the inhabitant wants to buy A[i] bottles of wine. If A[i] < 0, the inhabitant wants to sell -A[i] bottles of wine. It is guaranteed that sum(A[i]) == 0.

Transporting one bottle of wine between adjacent houses takes 1 unit of work. Find the minimum total work needed to satisfy all wine demands.

## Program

Standard input / standard output

### Input

- Line 1: Integer N (number of houses)
- Line 2: N space-separated integers representing demands A[i]

### Output

Print a single integer denoting minimum total work units required.

### Examples

**Example 1**

**Input:**
```text id="7j3xpf"
5
5 -4 1 -3 1
```

**Output:**
```text id="g8k2va"
9
```

Positive values are buyers and negative values are sellers. House 2 sells 4 bottles to House 1 for 4 units of work. House 4 sells its 3 bottles as follows: 1 to House 1 for 3 units, 1 to House 3 for 1 unit, and 1 to House 5 for 1 unit. Total work is 4 + 3 + 1 + 1 = 9.

### Constraints

- 2 <= N <= 10^5
- -10^6 <= A[i] <= 10^6
- sum(A[i]) == 0

## 1. Plain-English Problem Breakdown

Imagine a single, long street of houses. Some houses desperately want to buy wine (positive numbers), and some houses have excess wine they want to sell (negative numbers). The total amount of excess wine in the town perfectly matches the total demand.

Moving one bottle of wine to a next-door neighbor costs 1 unit of energy. Your goal is to find the absolute minimum energy required to settle all debts.

Instead of trying to figure out which specific house should sell to which specific buyer, you can look at the street as a series of border crossings. If you draw a line between House 1 and House 2, you can ask: *"Does the left side of this line have a net shortage or a net surplus?"* If the left side has a total shortage of 5 bottles, it is a physical law that exactly 5 bottles **must** cross that specific border from the right side to the left side. By simply walking down the street, keeping a running tally of the net balance, and adding up the absolute size of that balance at every single gap between houses, you instantly calculate the total work required.

## 2. Optimal Python Solution

This is a classic Greedy algorithm problem that is solved perfectly using a single-pass Prefix Sum approach.

**Python**

```python id="7kpz2n"
import sys

def solve():
    # Read all input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    demands = [int(x) for x in input_data[1 : n + 1]]
    
    total_work = 0
    current_balance = 0
    
    # Walk through the street, updating the running balance of wine
    for demand in demands:
        current_balance += demand
        # The absolute value of the current balance dictates exactly 
        # how many bottles must cross the boundary to the next house
        total_work += abs(current_balance)
        
    print(total_work)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N)$ where $N$ is the number of houses. We only iterate through the array of demands exactly once. For $10^5$ houses, this runs in roughly a millisecond.
- **Space Complexity:** $O(N)$ to store the parsed list of demands. The actual work logic uses $O(1)$ constant space (just the `total_work` and `current_balance` variables).

## 3. Logic Walkthrough for a Layman

Let's trace Example 1: `[5, -4, 1, -3, 1]`

1. **House 1:** Needs 5 bottles. Our running balance is `5`. This means 5 bottles must cross the gap from House 2 to House 1. We add 5 to our Work Total. (Work = 5)
2. **House 2:** Has 4 extra bottles to sell (-4). We add this to our running balance: $5 - 4 = 1$. The left side of the street still needs 1 bottle to be perfectly settled. That 1 bottle must cross the gap from House 3 to House 2. We add 1 to our Work Total. (Work = 6)
3. **House 3:** Needs 1 bottle. Running balance becomes $1 + 1 = 2$. Two bottles must cross the next gap. We add 2 to our Work Total. (Work = 8)
4. **House 4:** Has 3 extra bottles to sell (-3). Running balance becomes $2 - 3 = -1$. The left side now has a surplus of 1 bottle, which means 1 bottle must cross the gap heading right towards House 5. We add 1 to our Work Total. (Work = 9)
5. **House 5:** Needs 1 bottle. Running balance becomes $-1 + 1 = 0$. The street is perfectly settled.
6. Our final Minimum Work Total is **9**.

## 4. LeetCode Equivalent

While this specific scenario is famously known as the SPOJ problem "GERGOVIA", it maps closely to array prefix-sum manipulation problems on LeetCode.

- **LeetCode 1769. Minimum Number of Operations to Move All Balls to Each Box** (Medium) - Shares the core concept of counting operations based on spatial distances in a 1D array.
- **LeetCode 462. Minimum Moves to Equal Array Elements II** (Medium) - Deals with minimizing distance costs in a 1D plane using absolute differences.

## 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: In SQL, what is the exact difference between `DELETE`, `TRUNCATE`, and `DROP`?**

- **Answer:**
  - **`DELETE`** is a DML command that removes specific rows based on a `WHERE` clause. It logs every individual deletion, making it slower, but allowing you to undo (`ROLLBACK`) the action.
  - **`TRUNCATE`** is a DDL command that instantly wipes all rows from a table. It does not log individual row deletions (so it is incredibly fast) and keeps the empty table structure intact, but it cannot be rolled back.
  - **`DROP`** is a DDL command that completely deletes the table's data, its structure, its indexes, and its permissions from the database permanently.

**Q2: In Java or general OOPs, what is the difference between `final`, `finally`, and `finalize()`?**

- **Answer:**
  - **`final`** is a keyword used to declare a constant variable (cannot be modified), a method that cannot be overridden by child classes, or a class that cannot be inherited.
  - **`finally`** is a block used in exception handling (`try-catch-finally`). Code inside the `finally` block is guaranteed to execute whether an error occurs or not, typically used to close database connections or file streams.
  - **`finalize()`** is a method called by the Garbage Collector right before an object is destroyed to free up memory. (Note: It is deprecated in modern Java).

**Q3: What is the difference between a "Cookie" and a "Session" in Web Development?**

- **Answer:** Both track user data, but they store it in different places. A **Cookie** stores data on the client's side (directly in the user's web browser). It has a defined expiration date and can be modified or viewed by the user, making it unsafe for sensitive data. A **Session** stores data securely on the server's side. The server generates a unique "Session ID", gives it to the user's browser (usually as a tiny cookie), and uses that ID to look up the user's secure details in its own database. Sessions end as soon as the user closes the browser or logs out.
