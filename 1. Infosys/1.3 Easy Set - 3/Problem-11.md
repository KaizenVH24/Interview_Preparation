## 11. Max Continuous Zeroes in Base-B Representation

Given a decimal integer N and a base B (2 <= B <= 16), convert N into base B representation and find the length of the longest continuous run of consecutive '0' characters in the converted string.

### Program

**Standard input / standard output**

### Input

Line 1: Integer N (decimal number)  
Line 2: Integer B (base)

### Output

Print the maximum length of consecutive zeroes in base B representation of N.

### Examples

**Example 1**

**Input:**
```text
68
2
```

**Output:**
```text
3
```

68 is 1000100 in base 2.

**Example 2**

**Input:**
```text
8
2
```

**Output:**
```text
3
```

8 is 1000 in base 2.

### Constraints

- 1 <= N <= 10^9
- 2 <= B <= 16

---

### 1. Plain-English Problem Breakdown

Imagine you are translating a number from our standard human counting system (Base-10) into a computer's counting system (like Base-2 binary, or Base-16 hexadecimal).

When you translate the number, it becomes a sequence of new digits. Your goal is simply to find the longest unbroken streak of zeros in that new sequence.

You don't even need to physically build the new word or know the alphabetical letters for hexadecimal (like A, B, C). All you need to do is repeatedly divide the number by your target base. Every time the division leaves a remainder of exactly `0`, that represents a zero in the new system. You just count how many times you get a `0` remainder in a row, and keep track of your highest score.

### 2. Optimal Python Solution

We can solve this purely mathematically without ever constructing a string. This saves memory and runs much faster.

**Python**

```python
import sys

def solve():
    # Read all input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    b = int(input_data[1])
    
    max_zeroes = 0
    current_zeroes = 0
    
    # Repeatedly divide N by the target base B
    while n > 0:
        remainder = n % b
        
        if remainder == 0:
            # We found a zero, increase our current streak
            current_zeroes += 1
            # Update the all-time record if this streak is the longest
            if current_zeroes > max_zeroes:
                max_zeroes = current_zeroes
        else:
            # The streak is broken by a non-zero digit
            current_zeroes = 0
            
        # Move to the next digit
        n = n // b
        
    print(max_zeroes)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** `O(\log_B N)`. In every step of the `while` loop, we divide `N` by the base `B`. For a number as large as `10^9` and the smallest base `B = 2`, this loop runs at most ~30 times. This is instantaneous (`O(1)` in practical terms).
- **Space Complexity:** `O(1)`. We do not create any arrays or strings. We only use two integer variables (`max_zeroes` and `current_zeroes`) to keep track of the count, meaning it takes virtually zero memory.

### 3. Logic Walkthrough for a Layman

1. **The Setup:** We want to convert 68 to Base-2. We grab our counters: `current_streak = 0` and `record_streak = 0`.
2. **First Division:** We divide 68 by 2. The remainder is 0.
   - It's a zero! `current_streak` becomes 1. `record_streak` updates to 1.
   - Our new number is 34.
3. **Second Division:** We divide 34 by 2. The remainder is 0.
   - Another zero! `current_streak` becomes 2. `record_streak` updates to 2.
   - Our new number is 17.
4. **The Streak Breaker:** We divide 17 by 2. The remainder is 1.
   - It's not a zero. The streak is broken! `current_streak` drops back to 0. `record_streak` stays at 2.
   - Our new number is 8.
5. **The Comeback:** As we continue dividing 8, 4, and 2, we hit three remainders of 0 in a row. Our `current_streak` reaches 3, which beats our old record! `record_streak` becomes 3.
6. Once the number hits 0, we stop and print our `record_streak` of 3.

### 4. LeetCode Equivalent

This mathematically translates to standard consecutive-element counting problems and binary bitwise operations.

- **LeetCode 485. Max Consecutive Ones** (Easy) - Same exact logic, just counting 1s instead of 0s.
- **LeetCode 868. Binary Gap** (Easy) - Focuses on the distance between 1s in a binary representation, which inherently involves counting the 0s in between.

### 5. Infosys Technical Interview Questions (Fresher Round)

Questions dealing with base conversions usually trigger interviewers to ask about memory, data types, and fundamental loop structures.

**Q1: What is the exact difference between a `while` loop and a `for` loop?**

- **Answer:** A **`for` loop** is used when you know exactly how many times you want to iterate in advance (like looping through every item in a list of 10 elements). A **`while` loop** is used when the number of iterations is unknown, and the loop must continue running until a specific condition becomes false (like dividing a number until it reaches zero).

**Q2: What is an API (Application Programming Interface)?**

- **Answer:** An API is a set of rules and protocols that allows two completely different software applications to talk to each other. For example, when you use a food delivery app, the app doesn't have its own maps system; it uses the Google Maps API to fetch the location data and display it on your screen. It acts as a middleman delivering a request and returning the response.

**Q3: Python is an "Interpreted" language. What does that mean, and how does it make Python platform-independent?**

- **Answer:** Being interpreted means Python code is not directly converted into machine-specific hardware code by a compiler. Instead, it is translated into an intermediate "bytecode". The Python Virtual Machine (PVM), which reads this bytecode line-by-line, handles the final execution. Because you can install the PVM on Windows, Mac, or Linux, you can write Python code on a Mac and run the exact same file on Windows without changing anything.
