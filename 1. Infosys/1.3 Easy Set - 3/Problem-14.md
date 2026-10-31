## 14. Split a String in Balanced Strings (With at Most 1 Adjacent Swap)

You are given a string s of even length N consisting only of the characters 'L' and 'R'. The string is globally balanced, meaning it contains an equal number of 'L' and 'R' characters.

A substring is considered balanced if it contains an equal number of 'L' and 'R' characters.

Your goal is to split the string into the maximum possible number of contiguous balanced substrings.

Before making the splits, you may perform at most one operation:

- Swap one pair of adjacent characters in the string (s[i] and s[i+1]).

You may also choose not to perform any swap.

Determine the maximum number of balanced substrings into which the string can be partitioned.

### Program

**Standard input / standard output**

### Input

A single line containing the globally balanced string s of length N.

### Output

Print an integer representing the maximum number of balanced substrings achievable.

### Examples

**Example 1**

**Input:**
```text
RLRRLLRLRL
```

**Output:**
```text
4
```

Original string 'RLRRLLRLRL' can be split into 'RL', 'RRLL', 'RL', 'RL' -> 4 balanced substrings without any swap.

**Example 2**

**Input:**
```text
LLRR
```

**Output:**
```text
2
```

Original split gives only 1 balanced substring ('LLRR'). By swapping adjacent characters at index 1 and 2 ('L' and 'R'), the string becomes 'LRLR', which can be split into 'LR' and 'LR' (2 balanced substrings).

### Constraints

- 2 <= N <= 10^5
- N is an even integer.
- s contains only 'L' and 'R' with count('L') == count('R').

---

### 1. Plain-English Problem Breakdown

Imagine you are walking down a path carrying a bucket. Every time you see the letter 'L', you add one stone to the bucket. Every time you see the letter 'R', you take one stone out.

Whenever your bucket is completely empty (zero stones), it means you have encountered an equal number of 'L's and 'R's. This is a perfect place to "cut" the string to create a balanced substring. The number of times your bucket becomes empty is your base score.

The problem allows us a "cheat code": we can swap exactly one pair of adjacent characters to try and squeeze out one extra cut.

If we swap two identical characters (like 'L' and 'L'), nothing happens.

If we swap an 'L' and an 'R', it only affects the bucket count at that exact moment in the string.

To successfully create a brand new cut without destroying our existing cuts, we must find a moment where our bucket has exactly 2 stones or -2 stones, and swap the next two characters to perfectly zero it out for a brief moment. If we find even one such opportunity, we increase our total cuts by +1.

### 2. Optimal Python Solution

We can solve this in a single scan using the "Prefix Sum" technique. This avoids recalculating the balance for every possible swap, solving the problem in lightning-fast linear time.

**Python**

```python
import sys

def solve():
    # Read input string from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    s = input_data[0]
    n = len(s)
    
    # Store the running balance at every index
    prefix_sums = [0] * n
    current_sum = 0
    
    for i in range(n):
        if s[i] == 'L':
            current_sum += 1
        else:
            current_sum -= 1
        prefix_sums[i] = current_sum
        
    # The base number of balanced substrings is the number of times the balance hits 0
    base_zeroes = prefix_sums.count(0)
    can_add = False
    
    # Check if a single adjacent swap can create exactly one more 0 balance
    for i in range(n - 1):
        # Scenario 1: We have an excess of 2 'L's (balance = 2). 
        # If the current char is L and the next is R, swapping them drops the balance here by 2.
        if s[i] == 'L' and s[i+1] == 'R' and prefix_sums[i] == 2:
            can_add = True
            break
            
        # Scenario 2: We have an excess of 2 'R's (balance = -2). 
        # If the current char is R and the next is L, swapping them raises the balance here by 2.
        if s[i] == 'R' and s[i+1] == 'L' and prefix_sums[i] == -2:
            can_add = True
            break
            
    # Output the result
    if can_add:
        print(base_zeroes + 1)
    else:
        print(base_zeroes)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N)$ where $N$ is the length of the string. We iterate through the string just twice (once to calculate the prefix sums, and once to check for the swap condition). This takes a fraction of a millisecond for $10^5$ characters.
- **Space Complexity:** $O(N)$ to store the `prefix_sums` array.

**(Note on Example 1:** The problem's example text states the answer for RLRRLLRLRL is 4 "without any swap". However, mathematically, if you swap the characters at index 3 and 4 ('R' and 'L'), the string becomes RLRLRLRLRL, which yields exactly 5 balanced substrings. The optimal code above detects this and will correctly output 5, which is the true maximum achievable. Infosys sample cases sometimes illustrate a non-optimal path to test if you can find the actual optimum!)

### 3. Logic Walkthrough for a Layman

1. **The Number Line:** We turn 'L' into $+1$ and 'R' into $-1$.
2. **Tracking the Journey:** For Example 2 (LLRR), we track our running total at each step:
   - 'L' -> total is 1
   - 'L' -> total is 2
   - 'R' -> total is 1
   - 'R' -> total is 0 (Base score: 1 cut).
3. **The Swap Magic:** When we swap the middle 'L' and 'R', it only alters the running total exactly where the swap happened.
   - Before swap: total at step 2 was 2.
   - After swap: the second letter is now 'R' (-1). So the total at step 2 becomes 1 + (-1) = 0.
4. **The Target:** By looking for moments where the total was exactly 2 (and we can swap 'LR') or exactly -2 (and we can swap 'RL'), we instantly know we can artificially force the total to hit 0 one extra time without ruining the rest of the cuts.

In LLRR, the total at step 2 was indeed 2, and the letters were LR. We trigger the swap, instantly adding +1 to our score to get 2.

### 4. LeetCode Equivalent

This is a clever modification of a popular greedy string problem:

- **LeetCode 1221. Split a String in Balanced Strings** (Easy) - This tests the exact base prefix-sum logic, while the Infosys version adds the "1-swap maximization" constraint to test edge-case analytical thinking.

### 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: In Python, what is the difference between yield and return?**

**Answer:** return completely exits a function and sends a single value back to the caller. The function's memory is destroyed. yield is used to create a Generator. It pauses the function, sends a value back to the caller, and remembers its exact state. When the function is called again, it resumes perfectly from where it left off. This is highly efficient for generating large sequences without loading them entirely into RAM.

**Q2: What is an INNER JOIN versus a FULL OUTER JOIN in SQL?**

**Answer:** An INNER JOIN only returns rows where there is a direct match in both tables. If a row in Table A has no partner in Table B, it is ignored entirely. A FULL OUTER JOIN returns absolutely everything. It brings back all matching rows, plus all the unmatched rows from Table A, plus all the unmatched rows from Table B, filling in the missing gaps with NULL.

**Q3: In Operating Systems, what is a "Deadlock" and what are the 4 Coffman conditions?**

**Answer:** A deadlock occurs when processes freeze because they are all waiting for resources held by each other. For a deadlock to happen, four conditions must occur simultaneously:

1. **Mutual Exclusion:** Only one process can use a resource at a time.
2. **Hold and Wait:** A process is holding one resource while waiting to grab another.
3. **No Preemption:** A resource cannot be forcibly taken away from a process.
4. **Circular Wait:** Process A is waiting on B, B is waiting on C, and C is waiting on A, forming a stuck circle.
