# INFYTQ Special 4 and 7 Number Extraction

## 1. Problem Statement

Given a sequence of comma-separated single-digit integers, locate the unique digit `4` and the unique digit `7`. The digit `4` always occurs before the digit `7`.

Calculate two values:

- **Num1:** The sum of all digits outside the inclusive segment from `4` to `7`.
- **Num2:** The integer formed by concatenating all digits from `4` through `7`, including both endpoints.

Print the sum of `Num1` and `Num2`.

### Input Format

A single line containing comma-separated single-digit integers.

### Output Format

Print the integer result:

`Num1 + Num2`

### Examples

**Example 1**

Input:
```text
3,1,6,4,2,3,7,2
```

Output:
```text
4249
```

Explanation:

- Digits outside the segment: `3, 1, 6, 2`
- `Num1 = 3 + 1 + 6 + 2 = 12`
- Digits inside the segment: `4, 2, 3, 7`
- `Num2 = 4237`
- Final result: `12 + 4237 = 4249`

**Example 2**

Input:
```text
4,7,1
```

Output:
```text
48
```

Explanation:

- Digits outside the segment: `1`
- `Num1 = 1`
- Digits inside the segment: `4, 7`
- `Num2 = 47`
- Final result: `1 + 47 = 48`

### Constraints

- `3 <= n <= 18`, where `n` is the number of digits.
- Every token is a single digit from `0` through `9`.
- Exactly one token is `4` and exactly one token is `7`.
- The digit `4` occurs before the digit `7`.
- The result fits in a signed 64-bit integer.

---

## 2. Plain-English Problem Breakdown

Imagine a sequence of single-digit numbers separated by commas. The digit `4` marks the beginning of a segment, and the digit `7` marks its end.

Your task is to divide the sequence into two groups.

### Inner Segment

Take all the digits starting from `4` and ending at `7`, including both digits. Concatenate them to form a single integer.

For example:

```text
4,2,3,7 -> 4237
```

### Outer Segment

Take all the digits before `4` and after `7`. Add these digits normally.

For example:

```text
3,1,6,4,2,3,7,2
```

The outer digits are `3, 1, 6, 2`.

Their sum is:

```text
3 + 1 + 6 + 2 = 12
```

Finally, add the outer sum to the integer formed by the inner segment:

```text
12 + 4237 = 4249
```

---

## 3. Optimal Python Solution

The following solution reads input from standard input and prints the result to standard output.

```python
import sys


def solve():
    input_data = sys.stdin.read().strip()

    if not input_data:
        return

    tokens = input_data.split(',')

    idx_4 = tokens.index('4')
    idx_7 = tokens.index('7')

    # Sum all digits outside the inclusive 4-to-7 segment.
    num1 = sum(
        int(tokens[i])
        for i in range(len(tokens))
        if i < idx_4 or i > idx_7
    )

    # Concatenate the digits inside the segment.
    num2 = int("".join(tokens[idx_4:idx_7 + 1]))

    # Print the final result.
    print(num1 + num2)


if __name__ == "__main__":
    solve()
```

### How the Code Works

1. **Read the input:** Read the entire input as a string.
2. **Split the input:** Use `split(',')` to convert the comma-separated digits into a list of strings.
3. **Find the boundaries:** Use `index('4')` and `index('7')` to find the positions of the two digits.
4. **Calculate Num1:** Sum the digits whose indices are before `4` or after `7`.
5. **Calculate Num2:** Use list slicing and `"".join()` to concatenate the digits from `4` through `7`.
6. **Print the answer:** Add `Num1` and `Num2`.

---

## 4. Logic Walkthrough

Consider the input:

```text
3,1,6,4,2,3,7,2
```

### Step 1: Split the Input

```python
tokens = ['3', '1', '6', '4', '2', '3', '7', '2']
```

### Step 2: Locate the Digits

```python
idx_4 = 3
idx_7 = 6
```

Python uses zero-based indexing, so `4` is at index `3` and `7` is at index `6`.

### Step 3: Calculate Num1

Select all digits before index `3` and after index `6`.

```text
3, 1, 6, 2
```

Calculate their sum:

```text
Num1 = 3 + 1 + 6 + 2
Num1 = 12
```

### Step 4: Calculate Num2

Use the slice `tokens[3:7]` to include both boundary digits.

```python
segment = tokens[3:7]
num2 = int("".join(segment))
```

The resulting segment is:

```text
4, 2, 3, 7 -> 4237
```

Therefore:

```text
Num2 = 4237
```

### Step 5: Calculate the Final Result

```text
Result = Num1 + Num2
       = 12 + 4237
       = 4249
```

Output:

```text
4249
```

---

## 5. Complexity Analysis

Let `N` be the number of digits in the input.

| Operation | Time Complexity |
|---|---|
| Splitting the input | `O(N)` |
| Locating `4` and `7` | `O(N)` |
| Summing the outer digits | `O(N)` |
| Concatenating the inner segment | `O(N)` |
| **Overall time complexity** | **`O(N)`** |
| **Space complexity** | **`O(N)`** |

The algorithm is efficient because it processes the sequence a constant number of times.

Given the maximum of 18 digits, the execution time and memory usage are negligible.

---

## 6. Related LeetCode Practice Problems

There is no exact one-to-one LeetCode equivalent for this specific INFYTQ problem. However, the following problems reinforce related concepts.

1. [LeetCode 1512 — Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/) — Easy. Practices array traversal and counting.
2. [LeetCode 1859 — Sorting the Sentence](https://leetcode.com/problems/sorting-the-sentence/) — Easy. Practices string splitting, manipulation, and joining.

**Key concepts to practise:** String parsing, array indexing, slicing, concatenation, and conditional summation.

---

## 7. Infosys Technical Interview Questions

### Q1. What are the four pillars of Object-Oriented Programming (OOP)?

**Answer:**

The four pillars of OOP are:

1. **Encapsulation:** Bundling data and the methods that operate on it within a class, while controlling access to the internal state.
2. **Abstraction:** Hiding implementation details and exposing only the essential functionality.
3. **Inheritance:** Allowing a class to derive properties and behaviours from another class, promoting code reuse.
4. **Polymorphism:** Allowing the same interface or method call to exhibit different behaviours depending on the object or implementation.

### Q2. What is the difference between a Primary Key and a Unique Key in SQL?

**Answer:**

Both constraints enforce uniqueness, but they differ in their rules and usage.

| Feature | Primary Key | Unique Key |
|---|---|---|
| Uniqueness | Required | Required |
| `NULL` values | Not permitted | Depends on the database system |
| Number per table | Only one primary key constraint | Multiple unique constraints are allowed |
| Purpose | Identifies each row | Enforces uniqueness on a column or combination of columns |

**Important:** Handling of `NULL` values in unique constraints varies between database systems. For example, PostgreSQL normally permits multiple `NULL` values in a unique column, while SQL Server generally permits only one `NULL` in a single-column unique constraint.

### Q3. What is the difference between an Array and a Linked List?

**Answer:**

An **array** stores elements in indexed positions, typically in contiguous memory for conventional arrays. A **linked list** stores elements in nodes connected through references or pointers.

| Feature | Array | Linked List |
|---|---|---|
| Memory layout | Typically contiguous | Nodes may occupy different memory locations |
| Access by index | `O(1)` | `O(N)` |
| Search | `O(N)` for an unsorted array | `O(N)` |
| Insert/delete at the beginning | Usually `O(N)` for a conventional array | `O(1)` when the head is available |
| Insert/delete at a known position | May require shifting elements | `O(1)` pointer updates once the position is reached |
| Memory overhead | Usually lower | Extra memory for links or pointers |

Arrays are useful when fast indexed access is important. Linked lists are useful when insertions and deletions at known positions are frequent and their pointer-management overhead is acceptable.

---

## 8. Key Takeaways

- Locate the unique digits `4` and `7` to identify the required segment.
- Include both `4` and `7` when extracting the segment.
- Sum only the digits outside that segment.
- Use `"".join()` to concatenate the inner digits into an integer.
- Use `tokens[idx_4:idx_7 + 1]` to include the ending digit in Python slicing.
- The solution runs in **`O(N)` time** and uses **`O(N)` space**.
- Be prepared to explain string manipulation, array indexing, OOP principles, SQL constraints, and array-versus-linked-list trade-offs in a technical interview.

