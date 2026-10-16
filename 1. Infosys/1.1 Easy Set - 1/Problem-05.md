# Pronic Substring Extractor

## 1. Problem Statement

A **pronic number**, also known as a heteromecic number, is a number that can be expressed as the product of two consecutive non-negative integers.

Mathematically,

\[
P = n \times (n + 1), \quad n \geq 0
\]

Examples of pronic numbers include:

`0, 2, 6, 12, 20, 30, 42, 56, 72, 90, 110, ...`

Given a numeric string `S`, find all unique contiguous substrings whose integer values are pronic numbers.

Return the identified pronic numbers in ascending order. If no pronic number is found, print `-1`.

**Important:** Substrings with leading zeros represent the same numeric value. For example, `"02"` and `"2"` both represent the number `2` and must not be treated as distinct results.

### Input Format

A single line containing a numeric string `S`.

### Output Format

Print all unique pronic numbers in ascending order, separated by spaces. If no pronic number is found, print `-1`.

### Examples

**Example 1**

Input:
```text
93012630
```

Output:
```text
0 2 6 12 30 930
```

Explanation:

The string contains contiguous substrings representing the pronic numbers `0`, `2`, `6`, `12`, `30`, and `930`.

For example:

- `0 = 0 × 1`
- `2 = 1 × 2`
- `6 = 2 × 3`
- `12 = 3 × 4`
- `30 = 5 × 6`
- `930 = 30 × 31`

The unique results are printed in ascending order.

**Example 2**

Input:
```text
111
```

Output:
```text
-1
```

Explanation:

The possible substring values are `1` and `11`. Neither is a pronic number, so the output is `-1`.

### Constraints

- `1 <= length(S) <= 9`
- `S` contains decimal digits only.
- Leading zeros in a substring do not create a distinct numeric value.

---

## 2. Plain-English Problem Breakdown

Imagine the input string is a sequence of digits, and you need to identify every number that can be formed by cutting out a continuous section of that string.

For example, given:

```text
123
```

The possible substrings are:

```text
1, 12, 123, 2, 23, 3
```

Each substring is converted into an integer and checked to determine whether it is pronic.

A number is pronic if it equals the product of two consecutive non-negative integers.

For example:

\[
30 = 5 \times 6
\]

Therefore, `30` is a pronic number.

### The Approach

1. Generate every possible non-empty contiguous substring.
2. Convert each substring into an integer.
3. Check whether the integer is pronic.
4. Store the pronic numbers in a set to eliminate duplicates.
5. Sort the unique results in ascending order.
6. Print the results, or `-1` if none are found.

---

## 3. Optimal Python Solution

Since the input string contains at most nine digits, generating every possible substring is efficient.

```python
import sys
import math


def is_pronic(num):
    n = math.isqrt(num)
    return n * (n + 1) == num


def solve():
    s = sys.stdin.read().strip()

    if not s:
        return

    length = len(s)
    pronic_numbers = set()

    # Generate every non-empty contiguous substring.
    for i in range(length):
        for j in range(i + 1, length + 1):
            num = int(s[i:j])

            if is_pronic(num):
                pronic_numbers.add(num)

    # Print unique pronic numbers in ascending order.
    if pronic_numbers:
        print(*sorted(pronic_numbers))
    else:
        print(-1)


if __name__ == "__main__":
    solve()
```

### How the Code Works

**Step 1: Generate substrings**

The nested loops generate all non-empty contiguous substrings using the slice `s[i:j]`.

**Step 2: Convert substrings to integers**

The expression `int(s[i:j])` converts each substring into an integer. Leading zeros are automatically ignored when converting to an integer.

**Step 3: Check whether the number is pronic**

The function `is_pronic()` uses the integer square root to find a candidate value of `n`.

For a number `num`, let:

\[
n = \lfloor \sqrt{\text{num}} \rfloor
\]

The number is pronic if:

\[
n(n+1) = \text{num}
\]

**Step 4: Remove duplicates**

A set ensures that each pronic numeric value appears only once, even if multiple substrings represent the same number.

**Step 5: Sort and print**

Sort the unique values in ascending order. If the set is empty, print `-1`.

---

## 4. Logic Walkthrough

Consider the input:

```text
93012630
```

### Step 1: Generate Substrings

The algorithm generates every possible contiguous substring, including single digits and the entire string.

Some examples include:

```text
9, 93, 930, 3, 30, 0, 12, 126, ...
```

There are at most 45 non-empty substrings for a string of length nine.

### Step 2: Check for Pronic Numbers

Each substring is converted to an integer and tested.

For example:

| Substring | Integer | Pronic? | Reason |
|---|---:|---|---|
| `"0"` | 0 | Yes | `0 × 1 = 0` |
| `"2"` | 2 | Yes | `1 × 2 = 2` |
| `"6"` | 6 | Yes | `2 × 3 = 6` |
| `"12"` | 12 | Yes | `3 × 4 = 12` |
| `"30"` | 30 | Yes | `5 × 6 = 30` |
| `"93"` | 93 | No | No consecutive non-negative integers multiply to 93 |
| `"930"` | 930 | Yes | `30 × 31 = 930` |

### Step 3: Remove Duplicates

If the same pronic value appears in multiple substrings, the set stores it only once.

For example, `"02"` and `"2"` both represent the same integer, `2`.

### Step 4: Sort the Results

The unique pronic numbers are sorted in ascending order.

```text
0 2 6 12 30 930
```

Output:

```text
0 2 6 12 30 930
```

---

## 5. Complexity Analysis

Let `L` be the length of the input string.

| Operation | Complexity |
|---|---|
| Generating all substrings | `O(L²)` substrings |
| Converting and checking substrings | `O(L³)` overall in a general variable-length analysis |
| Storing unique pronic values | `O(L²)` upper bound on the number of candidates |
| Sorting unique results | `O(L² log L)` as a conservative bound |
| **Overall time complexity** | **`O(L³)`** |
| **Auxiliary space complexity** | **`O(L²)`** |

The cubic time bound accounts for the cost of converting and checking substrings whose lengths can be up to `L`. Since `L <= 9`, the input is tiny, and this approach easily satisfies the constraints.

---

## 6. Related LeetCode Practice Problems

There is no exact one-to-one LeetCode equivalent, but the following problems reinforce related substring and numeric-processing concepts.

1. [LeetCode 2269 — Find the K-Beauty of a Number](https://leetcode.com/problems/find-the-k-beauty-of-a-number/) — Easy. Extracts substrings from a number and checks a mathematical condition.
2. [LeetCode 647 — Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/) — Medium. Focuses on identifying properties of contiguous substrings.

**Key concepts:** Substring generation, integer conversion, mathematical validation, sets, and sorting.

---

## 7. Infosys Technical Interview Questions

### Q1. What does it mean that Python strings are immutable?

**Answer:**

String immutability means that the contents of an existing string object cannot be changed after it is created.

For example:

```python
name = "Infy"
name = name + "sys"
print(name)
```

Output:

```text
Infysys
```

The concatenation creates a new string object rather than modifying the original string object in place. The variable `name` is then assigned to the new result.

### Q2. What is the difference between `break`, `continue`, and `pass`?

**Answer:**

- **`break`:** Immediately terminates the nearest enclosing loop.
- **`continue`:** Skips the remaining statements in the current iteration and moves to the next iteration.
- **`pass`:** Performs no operation. It is useful as a placeholder when Python syntax requires a statement.

Example:

```python
for i in range(5):
    if i == 1:
        continue
    if i == 3:
        break
    print(i)
```

Output:

```text
0
2
```

### Q3. What is a Foreign Key in a Database?

**Answer:**

A foreign key is a column or group of columns that references a candidate key, usually the primary key, in the same table or another table.

It helps establish relationships between records and enforce referential integrity.

For example, an `Orders` table might contain a `customer_id` foreign key that references the `id` column in a `Customers` table.

This prevents an order from referencing a customer that does not exist, subject to the database's foreign-key rules and any configured actions.

---

## 8. Key Takeaways

- A pronic number has the form `n × (n + 1)`, where `n >= 0`.
- Generate every non-empty contiguous substring.
- Convert each substring to an integer before checking its value.
- Use `math.isqrt()` to test for pronic numbers without floating-point precision issues.
- Use a set to eliminate duplicate numeric values.
- Sort the unique results in ascending order.
- Print `-1` if no pronic number is found.
- With a maximum input length of nine digits, exhaustive substring generation is a practical and efficient solution.

