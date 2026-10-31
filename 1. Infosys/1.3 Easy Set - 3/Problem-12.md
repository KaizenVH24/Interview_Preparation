## 12. Vowel Multiply & Single Digit Digital Root

Given a string S, transform it according to the following rules:

1. For each vowel character (A, E, I, O, U, a, e, i, o, u) at index i (0-indexed):
   - Calculate res = i * 5.
   - Sum all even numbers starting from 1 up to res (i.e. 2 + 4 + 6 + ... <= res).
   - If the sum is more than a single digit, recursively compute its digital root (sum of digits) until it becomes a single digit (0-9).
   - Replace the vowel in S with this single-digit character.

2. Non-vowel characters remain unchanged.

Print the transformed string.

### Program

**Standard input / standard output**

### Input

A single line containing the string S.

### Output

Print the transformed string.

### Examples

**Example 1**

**Input:**
```text
apple
```

**Output:**
```text
0ppl2
```

The even-position sum at index 4 is 110, whose digital root is 2.

**Example 2**

**Input:**
```text
a
```

**Output:**
```text
0
```

Index 0 yields sum 0.

### Constraints

- 1 <= length(S) <= 10000
- S contains no whitespace

---

### 1. Plain-English Problem Breakdown

Imagine a word as a line of people, each standing on a numbered tile starting from zero. You walk down the line, and whenever you spot someone who is a "vowel" (A, E, I, O, U), you make them play a math game based on the number of the tile they are standing on.

**The Game:**

1. Multiply their tile number by 5.
2. Write down all the even numbers from 2 up to that new multiplied number, and add them all together to get a massive sum.
3. If that massive sum is a two-digit (or larger) number, keep adding its individual digits together until it shrinks down to a single digit (0 through 9).
4. Hand that person a sign with this final single digit, and tell them to hold it up instead of their original letter.

If the person is a consonant, you just walk right past them. Once you reach the end of the line, read the new word aloud.

### 2. Optimal Python Solution

We can optimize this heavily. Instead of writing a slow loop to add all the even numbers one by one, we can use a mathematical shortcut. The sum of the first `N` even numbers is always `N * (N + 1)`. Similarly, instead of repeatedly summing digits with a loop, we can use the "Modulo 9" math trick, which instantly calculates the digital root of any number in constant time.

**Python**

```python
import sys

def get_digital_root(n):
    # A mathematical trick: the digital root of a number is exactly equal 
    # to the number modulo 9. The only exceptions are 0 (which is 0) 
    # and multiples of 9 (which resolve to 9 instead of 0).
    if n == 0:
        return 0
    if n % 9 == 0:
        return 9
    return n % 9

def solve():
    # Read the input string
    s = sys.stdin.read().strip()
    if not s:
        return
        
    vowels = set("AEIOUaeiou")
    result = []
    
    for i, char in enumerate(s):
        if char in vowels:
            res = i * 5
            
            # Mathematical shortcut to sum even numbers up to 'res'
            # First, find out HOW MANY even numbers there are up to 'res'
            num_evens = res // 2
            
            # The sum of the first N even numbers is simply N * (N + 1)
            sum_even = num_evens * (num_evens + 1)
            
            # Get the single-digit root using the Modulo 9 trick
            single_digit = get_digital_root(sum_even)
            
            # Add the digit character to our result
            result.append(str(single_digit))
        else:
            # Consonants and other characters remain unchanged
            result.append(char)
            
    # Join the list into a final string and print
    print("".join(result))

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(L)$, where $L$ is the length of the string. We iterate through the string exactly once. Because we used pure mathematical formulas instead of loops to calculate the even sums and the digital roots, the operations inside the loop take $O(1)$ constant time.
- **Space Complexity:** $O(L)$ to store the `result` list before joining it back into a string.

### 3. Logic Walkthrough for a Layman

Let's test the logic with the word **"apple"**.

1. **'a' (Index 0):** It's a vowel! We multiply the index 0 by 5 to get `0`. How many even numbers are greater than 1 and less than or equal to 0? None. The sum is `0`. The digital root of 0 is `0`. We replace 'a' with `'0'`.
2. **'p' (Index 1), 'p' (Index 2), 'l' (Index 3):** Consonants. We ignore them and keep them as `'p'`, `'p'`, `'l'`.
3. **'e' (Index 4):** It's a vowel! We multiply the index 4 by 5 to get `20`.
   - **The Math Shortcut:** Instead of adding $2 + 4 + 6 + 8 \dots + 20$, we just realize there are exactly 10 even numbers between 1 and 20 (since $20 \div 2 = 10$). We multiply $10 \times 11 = 110$.
   - **The Magic 9 Trick:** We need the digital root of 110. $1 + 1 + 0 = 2$. Our math trick instantly computes this by doing $110 \pmod 9$. The remainder when dividing 110 by 9 is exactly `2`. We replace 'e' with `'2'`.
4. **Final Result:** We glue the pieces together to get **"0ppl2"**.

### 4. LeetCode Equivalent

This problem is a fusion of string traversal and a very famous pure-math LeetCode problem.

- **LeetCode 258. Add Digits** (Easy) - This LeetCode problem directly asks you to find the single-digit root of a number, teaching the exact $O(1)$ Modulo 9 math trick used above.
- **LeetCode 1456. Maximum Number of Vowels in a Substring of Given Length** (Medium) - Helps practice identifying and filtering vowels efficiently in Python.

### 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: In OOPs, what is the exact difference between Abstraction and Encapsulation?**

- **Answer:** **Encapsulation** is about *hiding* data to protect it. It binds the data (variables) and the code that manipulates it (methods) into a single capsule (a class), using private modifiers so outside code cannot accidentally corrupt it. **Abstraction** is about *hiding complexity*. It exposes only the essential, easy-to-use features to the user while keeping the messy internal mechanics hidden (like pressing the accelerator pedal in a car without needing to know how the fuel injection system works).

**Q2: What is a REST API?**

- **Answer:** A REST API (Representational State Transfer Application Programming Interface) is a standard set of rules that allows web applications to communicate with each other over the internet. It relies on standard HTTP methods (like `GET` to fetch data, `POST` to send new data, `PUT` to update data, and `DELETE` to remove data). REST APIs are stateless, meaning the server does not remember anything about the client between requests; every request must contain all the information needed to understand it.

**Q3: Explain the concepts of `Primary Key`, `Foreign Key`, and `Unique Key` with an example.**

- **Answer:**
  - A **Primary Key** uniquely identifies each row in a table (e.g., `Employee_ID`). It cannot be null, and there can only be one per table.
  - A **Unique Key** also ensures all values in a column are distinct (e.g., `Email_Address`), but unlike a primary key, it allows a single `NULL` value, and a table can have multiple unique keys.
  - A **Foreign Key** is a column in one table that links to the primary key of another table, creating a relationship. For example, a `Department_ID` column in the Employees table pointing to the primary key of a separate Departments table.
