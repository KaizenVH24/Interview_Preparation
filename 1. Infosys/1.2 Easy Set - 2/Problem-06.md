# First Non-Repeating Character

Given a string S containing lowercase English alphabets, find and return the first non-repeating character in S. If every character in S appears more than once, return '#'.

## Program

Standard input / standard output

### Input

A single line containing the string S.

### Output

Print the first non-repeating character or '#'.

### Examples

#### Example 1

**Input:** `programming`

**Output:** `p`

'p' appears only once and is at index 0.

#### Example 2

**Input:** `aabbcc`

**Output:** `#`

Every character repeats, so the required sentinel is '#'.

### Constraints

- 1 <= len(S) <= 10^5

---

## 1. Plain-English Problem Breakdown

Imagine you are checking the guest list for a party, and everyone is assigned a single lowercase letter as their ID. You want to find the very first person in the line whose ID is completely unique—meaning nobody else at the entire party has that same letter.

If you scan the whole line and realise that every single person shares their ID letter with at least one other person, you hand out a special `#` badge indicating there are no unique IDs.

## 2. Optimal Python Solution

To handle a string of up to 100,000 characters efficiently, we cannot compare every letter against every other letter (which would take too long). Instead, we use a Hash Map (Python Dictionary) to count occurrences in one quick pass.

**Python**

```python
import sys
from collections import Counter

def solve():
    # Read the input string
    s = sys.stdin.read().strip()
    if not s:
        return
        
    # Counter automatically creates a dictionary of character frequencies
    # Example for "aabbcc": {'a': 2, 'b': 2, 'c': 2}
    # Example for "programming": {'p': 1, 'r': 2, 'o': 1, 'g': 2, 'a': 1, 'm': 2, 'i': 1, 'n': 1}
    char_counts = Counter(s)
    
    # Iterate through the original string to maintain the left-to-right order
    for char in s:
        # The first character we see that has a count of exactly 1 is our answer
        if char_counts[char] == 1:
            print(char)
            return
            
    # If the loop finishes and we found no unique character, print '#'
    print("#")

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** \(O(N)\), where \(N\) is the length of the string. We loop through the string exactly twice: once to count the frequencies, and once to check the counts. For \(10^5\) characters, this runs in a fraction of a millisecond.
- **Space Complexity:** \(O(1)\) constant space. Even though we create a dictionary, there are only 26 possible lowercase English letters. The dictionary will never grow larger than 26 key-value pairs, meaning memory usage is incredibly tiny and strictly bounded.

## 3. Logic Walkthrough for a Layman

1. **The Tally Phase (Pass 1):** We walk down the string from left to right and keep a tally on a clipboard of how many times we see each letter. If the word is "programming", our clipboard says: p=1, r=2, o=1, g=2, a=1, m=2, i=1, n=1.
2. **The Verification Phase (Pass 2):** We start at the beginning of the string *again*.
3. **The Check:** We look at the first letter, 'p'. We check our clipboard. The tally for 'p' is exactly 1.
4. **The Result:** Because we are reading from left to right, 'p' is guaranteed to be the *first* non-repeating character. We print it and stop the program immediately. If the word was "aabbcc", we would check 'a' (tally 2), then 'b' (tally 2), then 'c' (tally 2). Since we finish the word without finding a tally of 1, we print `#`.

## 4. LeetCode Equivalent

This is one of the most frequently asked basic hashing questions across all major service-based company interviews.

- **LeetCode 387. First Unique Character in a String** (Easy) - Identical logic, but asks you to return the numeric index instead of the character itself.

## 5. Infosys Technical Interview Questions (Fresher Round)

Since you used a Hash Map / Dictionary to solve this efficiently, the interviewer will likely test your knowledge on how they work, along with standard OS and Database concepts.

**Q1: How does a Hash Map (or Dictionary in Python) look up data so fast?**

- **Answer:** A Hash Map uses a "Hash Function". When you give it a key (like a character), the hash function instantly calculates a mathematical integer based on that key. This integer acts as the exact memory address (index) where the value is stored. Instead of searching through data one by one, the hash map jumps directly to that memory address in \(O(1)\) constant time.

**Q2: In Operating Systems, what is the difference between a Process and a Thread?**

- **Answer:** A **Process** is a heavy-weight, independent program in execution (like opening Google Chrome). It has its own dedicated memory space, so if one process crashes, it doesn't crash others. A **Thread** is a lightweight sub-task *inside* a process (like a single tab within Google Chrome). Threads share the same memory and resources of their parent process, making them faster to create and switch between, but a crash in one thread can bring down the whole process.

**Q3: What is the difference between DDL and DML commands in SQL?**

- **Answer:** **DDL (Data Definition Language)** commands deal with the structure or schema of the database itself. Examples include `CREATE`, `ALTER`, and `DROP` (e.g., creating a whole new table). **DML (Data Manipulation Language)** commands deal with modifying the actual data inside those tables. Examples include `INSERT`, `UPDATE`, and `DELETE` (e.g., updating a user's password).
