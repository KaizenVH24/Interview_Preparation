# Largest Even Number from Alphanumeric String

## 1. Problem Statement

Given an alphanumeric string containing letters, digits, and special characters, extract all **unique digits** and find the largest even integer that can be formed using each unique extracted digit exactly once.

If no even number can be formed, return `-1`.

### Input Format

A single line containing the string `S`.

### Output Format

Print the largest possible even integer as a string, or `-1` if no even number can be formed.

### Examples

**Example 1**

Input:
```text
asdf@75483
```

Output:
```text
87534
```

Explanation:

- Unique digits: `{3, 4, 5, 7, 8}`
- Available even digits: `{4, 8}`
- Choose the smallest even digit, `4`, as the last digit.
- Arrange the remaining digits in descending order: `8, 7, 5, 3`.
- Result: `87534`.

**Example 2**

Input:
```text
infytq@13579
```

Output:
```text
-1
```

Explanation:

- Unique digits: `{1, 3, 5, 7, 9}`
- No even digit is available.
- An even number cannot be formed, so the output is `-1`.

### Constraints

- `1 <= len(S) <= 1000`
- The string contains ASCII characters.

---

## 2. Plain-English Problem Breakdown

Imagine you have a string containing letters, numbers, and special characters. Your task is to extract only the digits, remove duplicates, and arrange the remaining digits to form the largest possible even number.

For example:

```text
Input: asdf@75483
```

First, extract the digits and remove duplicates:

```text
{7, 5, 4, 8, 3}
```

An even number must end with one of these digits:

`0, 2, 4, 6, 8`

To maximize the resulting number, we need to:

1. Identify the available even digits.
2. Choose the smallest even digit for the last position.
3. Sort all remaining unique digits in descending order.
4. Concatenate the digits to form the final number.

Choosing the smallest available even digit for the last position leaves the largest possible digits at the beginning, maximizing the result.

If no even digit exists, the answer is `-1`.

---

## 3. Optimal Python Solution

```python
import sys


def solve():
    s = sys.stdin.read().strip()

    if not s:
        return

    # Extract all unique digits.
    unique_digits = {
        int(char) for char in s if '0' <= char <= '9'
    }

    # Identify the available even digits.
    even_digits = [d for d in unique_digits if d % 2 == 0]

    # An even number is impossible without an even digit.
    if not even_digits:
        print(-1)
        return

    # Place the smallest even digit at the end.
    smallest_even = min(even_digits)
    unique_digits.remove(smallest_even)

    # Arrange the remaining digits in descending order.
    remaining_sorted = sorted(unique_digits, reverse=True)

    # Construct the largest possible even number.
    result = ''.join(map(str, remaining_sorted)) + str(smallest_even)

    print(result)


if __name__ == "__main__":
    solve()
```

### How the Code Works

1. **Extract unique digits:** A set stores each digit only once.
2. **Find even digits:** Filter the set to identify digits divisible by `2`.
3. **Handle the impossible case:** If there are no even digits, print `-1`.
4. **Choose the final digit:** Select the smallest available even digit.
5. **Sort the remaining digits:** Arrange them in descending order.
6. **Build the result:** Concatenate the sorted digits with the selected even digit at the end.

### Why Choose the Smallest Even Digit?

The final digit determines whether the number is even. Once that condition is satisfied, the remaining digits should be arranged in descending order to maximize the number.

For example, with the unique digits `{3, 4, 5, 7, 8}`:

- Ending with `8` gives `75438`.
- Ending with `4` gives `87534`.

Since `87534 > 75438`, choosing `4` as the final digit produces the larger number.

---

## 4. Logic Walkthrough

Consider the input:

```text
asdf@75483
```

### Step 1: Extract Unique Digits

Ignore all letters and special characters.

```text
Unique digits = {3, 4, 5, 7, 8}
```

### Step 2: Identify Even Digits

The available even digits are:

```text
Even digits = {4, 8}
```

Since at least one even digit exists, an even number can be formed.

### Step 3: Select the Last Digit

Choose the smallest even digit:

```text
smallest_even = 4
```

Remove `4` from the set:

```text
Remaining digits = {3, 5, 7, 8}
```

### Step 4: Sort in Descending Order

```text
8, 7, 5, 3
```

### Step 5: Construct the Result

Concatenate the sorted digits and append `4`:

```text
8753 + 4 = 87534
```

Output:

```text
87534
```

---

## 5. Complexity Analysis

Let `N` be the length of the input string.

| Operation | Time Complexity |
|---|---|
| Scanning the input string | `O(N)` |
| Extracting unique digits | `O(N)` |
| Finding the smallest even digit | `O(1)` |
| Sorting the unique digits | `O(1)` |
| Constructing the result | `O(1)` |
| **Overall time complexity** | **`O(N)`** |
| **Auxiliary space complexity** | **`O(1)`** |

There are only ten possible decimal digits, from `0` to `9`. Therefore, the set can contain at most ten elements, regardless of the input length.

The algorithm is linear in the length of the input string, while the additional storage required for the unique digits is constant.

---

## 6. Related LeetCode Practice Problems

There is no exact one-to-one LeetCode equivalent, but these problems reinforce similar greedy and string-manipulation concepts.

1. [LeetCode 1903 — Largest Odd Number in String](https://leetcode.com/problems/largest-odd-number-in-string/) — Easy. Focuses on constructing a number that satisfies a parity condition.
2. [LeetCode 1323 — Maximum 69 Number](https://leetcode.com/problems/maximum-69-number/) — Easy. Focuses on greedy decisions to maximize a number.

**Key concepts:** String traversal, digit extraction, sets, sorting, greedy algorithms, and parity.

---

## 7. Infosys Technical Interview Questions

### Q1. What is the difference between a Dictionary and a Set in Python?

**Answer:**

Both are mutable collections, but they store data differently.

**Dictionary:**
- Stores key-value pairs.
- Keys must be unique and hashable.
- Useful for mapping one value to another.

Example:

```python
student = {
    "name": "Vinay",
    "age": 21
}
```

**Set:**
- Stores unique elements without key-value pairs.
- Useful for removing duplicates and performing set operations.

Example:

```python
numbers = {1, 2, 3, 3}
print(numbers)  # {1, 2, 3}
```

A set is particularly useful in this problem because it automatically removes duplicate digits.

### Q2. What are the ACID properties in DBMS?

**Answer:**

ACID represents four properties that help ensure reliable database transactions.

1. **Atomicity:** A transaction either completes entirely or has no effect.
2. **Consistency:** A transaction preserves database rules and constraints.
3. **Isolation:** Concurrent transactions behave according to the database's isolation guarantees.
4. **Durability:** Once a transaction is committed, its changes persist despite failures covered by the database's durability guarantees.

### Q3. What is the Software Development Life Cycle (SDLC)?

**Answer:**

The Software Development Life Cycle (SDLC) is a structured process used to plan, build, test, deploy, and maintain software.

Its common phases are:

1. **Requirement Analysis:** Understand user and business requirements.
2. **System Design:** Define the architecture, components, and data structures.
3. **Implementation:** Write the source code.
4. **Testing:** Identify defects and verify that the software meets requirements.
5. **Deployment:** Release the software into its target environment.
6. **Maintenance:** Fix defects, improve performance, and introduce updates.

The exact phases and their order may vary depending on the development methodology.

---

## 8. Key Takeaways

- Extract digits only; ignore letters and special characters.
- Use a set to eliminate duplicate digits.
- An even number must end with an even digit.
- Choose the smallest available even digit for the last position.
- Arrange all remaining digits in descending order.
- Return `-1` if no even digit exists.
- The algorithm runs in **`O(N)` time** with **`O(1)` auxiliary space** because there are only ten possible unique decimal digits.
