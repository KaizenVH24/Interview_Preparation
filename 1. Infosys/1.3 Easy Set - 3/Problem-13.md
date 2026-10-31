## 13. Sort Characters By Frequency

Sort a string's characters by decreasing frequency. When two characters have equal frequency, the character with the smaller ASCII code comes first. Print each character repeated by its frequency.

### Program

**Standard input / standard output**

### Input

A single line containing the string s.

### Output

Print the frequency-sorted string.

### Examples

**Example 1**

**Input:**
```text
tree
```

**Output:**
```text
eert
```

e has frequency 2; r precedes t on the tie.

**Example 2**

**Input:**
```text
cccaaa
```

**Output:**
```text
aaaccc
```

a precedes c on the frequency tie.

### Constraints

- 1 <= length(s) <= 500000
- s contains only English letters and decimal digits

---

### 1. Plain-English Problem Breakdown

Imagine you are given a massive pile of Scrabble tiles. Your job is to organize them into a new word based on a strict set of rules:

1. **The Popularity Contest:** You must group identical tiles together into stacks and arrange the stacks from tallest (most frequent) to shortest.
2. **The Tie-Breaker:** If two stacks are the exact same height (for example, you have three 'c's and three 'a's), you arrange them in alphabetical (ASCII) order. Since 'a' comes before 'c', the 'a' stack goes first.
3. **The Final Readout:** Once the stacks are in order, you write down every letter in every stack from left to right to form your final answer.

### 2. Optimal Python Solution

To handle 500,000 characters rapidly, we use a Hash Map (Python `Counter`) to tally the characters, and then apply a custom sorting rule using Python's `lambda` functions.

**Python**

```python
import sys
from collections import Counter

def solve():
    # Read the input string
    s = sys.stdin.read().strip()
    if not s:
        return
        
    # Count the frequency of every character
    # Example for 'tree': {'t': 1, 'r': 1, 'e': 2}
    freq = Counter(s)
    
    # Sort the unique characters based on our specific rules:
    # Rule 1: Highest frequency first -> We use negative frequency (-freq[x]) to sort descending
    # Rule 2: ASCII value ascending -> We use the character itself (x) as the tie-breaker
    sorted_unique_chars = sorted(freq.keys(), key=lambda x: (-freq[x], x))
    
    # Rebuild the string by multiplying each character by its frequency
    result = []
    for char in sorted_unique_chars:
        result.append(char * freq[char])
        
    # Join the list into a single string and print
    print("".join(result))

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N)$, where $N$ is the length of the string. Tallying the characters takes $O(N)$. Sorting takes $O(U \log U)$ where $U$ is the number of unique characters. Since the string only contains alphanumeric characters, $U$ is strictly capped at 62 (26 lowercase + 26 uppercase + 10 digits). Thus, sorting takes $O(1)$ constant time. Rebuilding the string takes $O(N)$. Overall time is incredibly fast and dominated purely by reading the string.
- **Space Complexity:** $O(N)$ to build the final output string and store the characters. The frequency dictionary takes $O(1)$ space because it holds at most 62 key-value pairs.

### 3. Logic Walkthrough for a Layman

1. **The Tally:** We look at `"cccaaa"`. Our tally sheet says: `'c'` appears 3 times, `'a'` appears 3 times.
2. **The Sorting Rules:** We take our unique letters (`'c'` and `'a'`) and apply a strict mathematical sorting formula consisting of two parts: `(-Frequency, Character)`.
   - For `'c'`, the score is `(-3, 'c')`.
   - For `'a'`, the score is `(-3, 'a')`.
3. **The Tie-Breaker in Action:** Python sorts lists by looking at the first part of the score. Both have `-3`, so it's a tie. Python then automatically looks at the second part of the score. Since `'a'` is alphabetically lower (has a smaller ASCII number) than `'c'`, Python decides `'a'` should come first.
4. **The Reassembly:** The computer says the final order of unique characters is `['a', 'c']`. We grab the tally sheet, print `'a'` 3 times, then print `'c'` 3 times. We get `"aaaccc"`.

### 4. LeetCode Equivalent

This is a direct, famous LeetCode problem with a slight added strictness on the tie-breaker.

- **LeetCode 451. Sort Characters By Frequency** (Medium) - The exact same problem, though LeetCode allows *any* tie-breaker for same frequencies, whereas Infosys explicitly demands ASCII ordering.

### 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: In SQL, what is the difference between the `WHERE` clause and the `HAVING` clause?**

- **Answer:** Both are used to filter data, but at different stages. The **`WHERE`** clause filters raw rows *before* any grouping happens. It cannot be used with aggregate functions like `SUM()` or `COUNT()`. The **`HAVING`** clause is used to filter aggregated data *after* a `GROUP BY` clause has organized the data into groups.

**Q2: What is Polymorphism in Object-Oriented Programming (OOPs)?**

- **Answer:** Polymorphism (meaning "many forms") allows a single action or method to behave differently depending on the object that calls it. The two main types are **Compile-time Polymorphism** (Method Overloading: same method name, different parameters in the same class) and **Run-time Polymorphism** (Method Overriding: a child class providing a specific, different implementation of a method that exists in its parent class).

**Q3: When browsing the web, what do HTTP Status Codes like 200, 404, and 500 mean?**

- **Answer:** These are standard server responses.
  - **200 OK:** The request was completely successful, and the server delivered the data.
  - **404 Not Found:** The server was reached, but the specific page or resource you requested doesn't exist.
  - **500 Internal Server Error:** You did everything right, but the server crashed or encountered a bug while trying to process your request.
