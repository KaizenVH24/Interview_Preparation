# 2. Alex's Birthday Gift (Divisible Arrays)

Alex selected two integers N and K. Count the integer arrays a[1], a[2], ..., a[K] such that every a[i] is in [1, N] and a[i + 1] is divisible by a[i] for every 1 <= i < K.

Return the count modulo exactly 10000.

## Program

Standard input / standard output

### Input

Line 1: Two space-separated integers N and K.

### Output

Print a single integer denoting the total count of valid arrays modulo 10000.

### Examples

**Example 1**

**Input:**
```text
2 2
```

**Output:**
```text
3
```

Valid arrays of length 2 with elements in [1, 2]: [1, 1] (1%1==0), [1, 2] (2%1==0), [2, 2] (2%2==0). Total = 3.

**Example 2**

**Input:**
```text
3 2
```

**Output:**
```text
5
```

Valid arrays: [1, 1], [1, 2], [1, 3], [2, 2], [3, 3]. Total = 5.

### Constraints

- 1 <= N <= 2000
- 1 <= K <= 2000

## 1. Plain-English Problem Breakdown

Imagine you are building a chain of numbers. Your chain must be exactly $K$ links long, and every number you pick must be between $1$ and $N$.

However, there is a strict mathematical rule for locking the links together: **the next number you choose must be a perfect multiple of the previous number.**

- If your first number is 2, the next number could be 2, 4, 6, 8, etc.
- If your first number is 3, the next number could be 3, 6, 9, etc.

Your task is to figure out the total number of different valid chains you can possibly build. Because this final count can be a massive number, the problem asks you to output only the last four digits of the answer (which mathematically means taking the result modulo 10000).

## 2. Optimal Python Solution

To solve this efficiently without calculating trillions of combinations manually, we use **Dynamic Programming (DP)**. We will keep track of how many valid chains of a certain length end in a specific number, and build our way up to length $K$.

**Python**

```python
import sys

def solve():
    # Read input data
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    k = int(input_data[1])
    
    MOD = 10000
    
    # dp[x] will store the number of valid arrays of the current length ending with number 'x'.
    # For an array of length 1, there is exactly 1 valid array ending in 'x' (the number itself).
    dp = [1] * (n + 1)
    dp[0] = 0 # We ignore index 0 as numbers must be in [1, N]
    
    # We already have length 1. We now build the chain up to length K.
    for _ in range(1, k):
        new_dp = [0] * (n + 1)
        
        for x in range(1, n + 1):
            if dp[x] > 0:
                val = dp[x]
                # If a chain ends in 'x', the next number can be any multiple of 'x'
                # We iterate through all multiples of 'x' up to 'n'
                for multiple in range(x, n + 1, x):
                    new_dp[multiple] += val
                    
        # Apply modulo 10000 at the end of each length step to prevent massive numbers
        # This optimization saves execution time compared to applying it inside the inner loop
        dp = [val % MOD for val in new_dp]
        
    # Sum up all valid arrays of length K ending in any number from 1 to N
    total_valid_arrays = sum(dp) % MOD
    print(total_valid_arrays)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(K \cdot N \log N)$. The outer loop runs $K$ times. The inner loop iterates $N$ times, and the innermost multiples loop runs $N/x$ times. Mathematically, the sum of $N/x$ (Harmonic series) is bounded by $N \log N$. For the maximum constraints ($2000 \times 2000$), this translates to around $3 \times 10^7$ quick addition operations, which runs well within standard 1-2 second time limits.
- **Space Complexity:** $O(N)$. We only ever need two 1D arrays of size $N+1$ (`dp` and `new_dp`) in memory at any given time, making it highly memory efficient.

## 3. Logic Walkthrough for a Layman

1. **The Starting Point (Length 1):** If we want a chain of just 1 link with $N = 3$, there are exactly three valid chains: `[1]`, `[2]`, and `[3]`. We record this in a list: 1 chain ends in 1, 1 chain ends in 2, 1 chain ends in 3.
2. **Growing to Length 2:** We want to add a second link.
   - We look at our chain ending in `1`. The next number can be any multiple of 1 up to 3 (which are 1, 2, and 3). So, the chain `[1]` branches out into `[1, 1]`, `[1, 2]`, and `[1, 3]`.
   - We look at our chain ending in `2`. The next number must be a multiple of 2 (which is only 2). So `[2]` becomes `[2, 2]`.
   - We look at our chain ending in `3`. The multiple is only 3. So `[3]` becomes `[3, 3]`.
3. **The Tally:** For length 2, we sum up our new branches: `[1, 1]`, `[1, 2]`, `[1, 3]`, `[2, 2]`, `[3, 3]`. There are 5 valid chains!
4. **Repeating:** We simply repeat this "branching out to multiples" logic $K-1$ times. Whenever our tallies get dangerously large, we divide by 10000 and only keep the remainder, ensuring the computer doesn't lag from calculating million-digit numbers.

## 4. LeetCode Equivalent

This specific divisor-chain mathematical logic traces back to classic competitive programming platforms rather than LeetCode's standard library.

- **Codeforces 414B (Mashmokh and ACM):** This is an exact 1:1 replica of that famous problem.
- **LeetCode 368. Largest Divisible Subset** (Medium) - It uses the exact same core mathematical property (finding elements where $arr[i] \% arr[j] == 0$) and Dynamic Programming to build chains.

## 5. Infosys Technical Interview Questions (Fresher Round)

For algorithm questions relying heavily on loops, arrays, and optimization, interviewers usually shift to foundational Database and Python framework concepts.

**Q1: What is the difference between `CHAR` and `VARCHAR` data types in SQL?**

- **Answer:** Both store text strings, but they allocate memory differently. **`CHAR`** is fixed-length. If you define a column as `CHAR(50)` and only store "Hello" (5 characters), the database will pad it with 45 empty spaces, permanently consuming 50 bytes. **`VARCHAR`** (Variable Character) is dynamic. If you define it as `VARCHAR(50)` and store "Hello", it will strictly consume only 5 bytes plus a 1-byte length tracker, saving significant disk space.

**Q2: What do `*args` and `**kwargs` mean in Python functions?**

- **Answer:** They are used to pass a variable number of arguments to a function when you aren't sure how many inputs the user will provide.
  - `*args` (Non-Keyword Arguments) passes the inputs as a **Tuple** (e.g., `def add(*args):` allows you to call `add(1, 2, 3, 4)`).
  - `**kwargs` (Keyword Arguments) passes the inputs as a **Dictionary** with key-value pairs (e.g., `def profile(**kwargs):` allows you to call `profile(name="Vinay", age=21)`).

**Q3: What is the role of a DNS (Domain Name System) in Networking?**

- **Answer:** DNS acts as the phonebook of the internet. Humans access information online through readable domain names like `google.com`. However, web browsers communicate using Internet Protocol (IP) addresses (like `142.250.190.46`). A DNS translates the human-readable domain name you type into the URL bar into the machine-readable IP address so your browser can locate and load the correct web server.
