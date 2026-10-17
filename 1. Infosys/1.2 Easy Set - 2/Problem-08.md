## Non-overlapping Longest Equal Prefix and Suffix

Given a string S, find the length of the longest substring which is simultaneously a prefix and a suffix of S, with the condition that the prefix and suffix do NOT overlap.
If no such non-overlapping prefix-suffix exists, return 0.

**Program**

`Standard input / standard output`

### Input

A single line containing string S.

### Output

Print the length of the longest non-overlapping prefix-suffix.

### Examples

**Example 1**

**Input:** `abcdabc`

**Output:** `3`

The border abc does not overlap.

**Example 2**

**Input:** `ababa`

**Output:** `1`

Only a is equal and non-overlapping.

### Constraints

- 1 <= length(S) <= 100000
- S contains no whitespace

---

### 1. Plain-English Problem Breakdown

Imagine reading a word from left to right (the prefix), and then reading that same word from right to left (the suffix). Your goal is to find the longest identical sequence of letters that appears at both the very beginning and the very end of the word.

However, there is a strict rule: **they cannot share the same letters in the middle.** They must be two completely independent, non-overlapping blocks of text.

For example, in `"abcdabc"`, the prefix `"abc"` and the suffix `"abc"` match perfectly, and since they are on opposite sides of the `"d"`, they do not overlap. The answer is 3.

But in `"ababa"`, the longest matching prefix and suffix is `"aba"` (length 3). However, the word is only 5 letters long, meaning the `"a"` in the middle is being shared by both the prefix and the suffix. This is illegal. The next best match is just `"a"` (length 1), which fits perfectly at both ends without touching.

### 2. Optimal Python Solution

A naive approach (checking every possible length from the middle down to 1) would take \(O(N^2)\) time, which will time out for 100,000 characters. The optimal solution uses the **LPS (Longest Prefix Suffix) array** concept from the famous KMP (Knuth-Morris-Pratt) string matching algorithm.

**Python**

```python
import sys

def solve():
    # Read the input string
    s = sys.stdin.read().strip()
    if not s:
        return
        
    n = len(s)
    # If the string is too short to have non-overlapping parts, return 0
    if n < 2:
        print(0)
        return
        
    # Array to store the length of the longest prefix-suffix for every substring
    lps = [0] * n
    
    length = 0  # Length of the previous longest prefix suffix
    i = 1
    
    # Step 1: Build the standard LPS array
    while i < n:
        if s[i] == s[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            # If there is a mismatch, fallback to the previous longest prefix-suffix
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
                
    # Step 2: Extract the longest prefix-suffix for the entire string
    lps_len = lps[-1]
    
    # Step 3: Enforce the non-overlapping condition
    # If the prefix-suffix overlaps (length > n // 2), 
    # we use the LPS array to gracefully fall back to the next longest valid prefix-suffix
    while lps_len > n // 2:
        lps_len = lps[lps_len - 1]
        
    print(lps_len)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** \(O(N)\), where \(N\) is the length of the string. We loop through the string exactly once to build the memory array, and at most \(N/2\) times in the final fallback loop. This runs in milliseconds even for \(10^5\) characters.
- **Space Complexity:** \(O(N)\) because we create an integer array `lps` of the same length as the string to store the fallback states.

### 3. Logic Walkthrough for a Layman

1. **The Memory Tracker (LPS Array):** Instead of blindly checking prefixes against suffixes over and over, we walk through the word just once from left to right. As we walk, we keep a "memory tracker" that records: *"Up to this current letter, what is the longest matching prefix and suffix?"*
2. **The Smart Fallback:** If we are building a match (like `"aba"`...) and suddenly the next letter ruins the pattern, we don't start over from scratch! Our memory tracker tells us exactly where to look next to salvage a smaller matching pattern.
3. **The Final Check:** By the time we reach the end of the word, our memory tracker gives us the absolute longest prefix-suffix for the whole word.
4. **Fixing the Overlap:** Let's say for `"ababa"`, the tracker says the longest match is 3 (`"aba"`). We check the math: \(3 > 5 \div 2\). It overlaps! Because of the mathematical properties of strings, the next best non-overlapping match is literally guaranteed to be the prefix-suffix *of our current overlapping match*.
5. We look up the memory tracker's value for the 3rd letter (`"aba"`), which tells us the next longest match is 1 (`"a"`). Since 1 is less than or equal to \(2.5\), we accept it and print 1.

### 4. LeetCode Equivalent

This is a direct variation of standard KMP pattern matching algorithms.

- **LeetCode 1392. Longest Happy Prefix** (Hard) - This problem asks you to find the longest prefix-suffix, but allows overlaps. The solution provided above solves this exact LeetCode problem in Step 1, and then adds the Step 3 loop to handle the specific Infosys non-overlapping constraint.

### 5. Infosys Technical Interview Questions (Fresher Round)

For algorithm-heavy string questions, the interviewer will often pivot to basic networking or foundational database design to ensure you have a well-rounded IT baseline.

**Q1: In Computer Networking, what is the difference between TCP and UDP?**

- **Answer:** **TCP (Transmission Control Protocol)** is connection-oriented and highly reliable. It guarantees that data packets are delivered in the correct order without errors (used in emails and web browsing). **UDP (User Datagram Protocol)** is connectionless and faster, but unreliable. It just sends the packets without checking if they arrived or in what order (used in live video streaming or online gaming where speed matters more than occasional lost pixels).

**Q2: What is Normalization in a Database?**

- **Answer:** Normalization is the process of organizing data in a database to reduce redundancy (duplicate data) and improve data integrity. It involves dividing large tables into smaller, related tables and linking them using relationships (Primary and Foreign keys). This ensures that if a piece of information needs to be updated, it only needs to be updated in one place.

**Q3: Explain the difference between a Stack and a Queue.**

- **Answer:** Both are linear data structures, but they process elements differently. A **Stack** follows the **LIFO** (Last-In, First-Out) principle, like a stack of plates—you can only add or remove from the top. A **Queue** follows the **FIFO** (First-In, First-Out) principle, like a line of people at a ticket counter—the first person to join the line is the first one to be served.
