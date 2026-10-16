# Largest Number Formed from Array

## 1. Problem Statement

Given an array of non-negative integers `arr`, arrange the elements such that they form the **largest possible number** when concatenated as strings.

Return the result as a string because the resulting number may be very large.

**Special Case:** If all elements are zero, return `"0"` instead of a string containing multiple zeros.

### Input Format

- **Line 1:** An integer `n`, representing the size of the array.
- **Line 2:** `n` space-separated non-negative integers.

### Output Format

Print the largest concatenated number as a string.

### Examples

**Example 1**

Input:
```text
5
3 30 34 5 9
```

Output:
```text
9534330
```

Explanation:

The numbers are arranged to form the largest possible concatenation: `9`, `5`, `34`, `3`, `30`.

Notice that `3` comes before `30` because `"330"` is greater than `"303"`.

**Example 2**

Input:
```text
2
0 0
```

Output:
```text
0
```

Explanation:

All elements are zero, so the result is `"0"` rather than `"00"`.

### Constraints

- `1 <= n <= 10^5`
- `0 <= arr[i] <= 10^9`

---

## 2. Plain-English Problem Breakdown

Imagine you have several blocks, each containing a number. Your task is to arrange these blocks in a row so that the resulting number is as large as possible.

You cannot simply sort the numbers in descending numerical order.

For example, consider `9` and `34`:

- Placing `34` before `9` produces `349`.
- Placing `9` before `34` produces `934`.

Since `934 > 349`, the number `9` must come before `34`.

The key challenge is determining the correct order for every pair of numbers. We must also handle the special case where all the elements are zero.

---

## 3. Optimal Python Solution

The following solution reads input from standard input and prints the result to standard output, making it suitable for competitive programming assessments.

```python
import sys
from functools import cmp_to_key


def compare(a, b):
    if a + b > b + a:
        return -1
    elif a + b < b + a:
        return 1
    return 0


def solve():
    input_data = sys.stdin.read().split()

    if not input_data:
        return

    n = int(input_data[0])
    arr = input_data[1:n + 1]

    arr.sort(key=cmp_to_key(compare))

    largest_num = "".join(arr)

    if largest_num[0] == "0":
        print("0")
    else:
        print(largest_num)


if __name__ == "__main__":
    solve()
```

### How the Comparator Works

The `compare(a, b)` function decides which of two numbers should appear first.

- If `a + b > b + a`, place `a` before `b`.
- If `a + b < b + a`, place `b` before `a`.
- If both combinations are equal, either order is acceptable.

The return values `-1`, `1`, and `0` tell Python's sorting mechanism how the two strings should be ordered.

### Why Convert Numbers to Strings?

The problem involves concatenation rather than arithmetic. Treating the numbers as strings makes it possible to compare combinations such as `"330"` and `"303"` directly.

### Why Use `cmp_to_key()`?

Python's `sort()` method does not directly accept a custom two-argument comparison function. The `cmp_to_key()` utility converts the comparator into a key function that Python's sorting mechanism can use.

### Why Check the First Character?

After sorting and concatenating, if the first character is `"0"`, every element must be zero. Therefore, we print `"0"` instead of the full string of zeros.

---

## 4. Logic Walkthrough

Consider the input:

```text
5
3 30 34 5 9
```

### Step 1: Read the Input

Store the array elements as strings:

```python
arr = ["3", "30", "34", "5", "9"]
```

### Step 2: Compare Pairs

For two strings `a` and `b`, compare their possible concatenations.

For example, with `"3"` and `"30"`:

```text
a + b = "330"
b + a = "303"
```

Since `"330" > "303"`, `"3"` should appear before `"30"`.

Similarly, comparing `"9"` and `"34"`:

```text
a + b = "934"
b + a = "349"
```

Since `"934" > "349"`, `"9"` should appear before `"34"`.

### Step 3: Sort the Array

Using the custom comparator, the array becomes:

```python
["9", "5", "34", "3", "30"]
```

### Step 4: Concatenate the Elements

```python
largest_num = "".join(arr)
```

Result:

```text
9534330
```

### Step 5: Handle the All-Zero Case

If the concatenated result begins with `"0"`, print `"0"` to avoid leading zeros.

---

## 5. Complexity Analysis

Let:

- `N` be the number of elements.
- `K` be the maximum number of digits in an element.

| Operation | Complexity |
|---|---|
| Reading and storing the input | `O(NK)` |
| Sorting with a custom comparator | `O(N log N × K)` |
| Concatenating the strings | `O(NK)` |
| **Overall time complexity** | **`O(N log N × K)`** |
| **Space complexity** | **`O(NK)`** |

Since each element is at most `10^9`, `K <= 10`. Therefore, `K` is bounded by a small constant under the given constraints, making the practical time complexity approximately **`O(N log N)`**.

The space complexity accounts for storing the input strings, the concatenated result, and the sorting overhead.

---

## 6. LeetCode Equivalent

**Problem:** [LeetCode 179 — Largest Number](https://leetcode.com/problems/largest-number/)

- **Difficulty:** Medium
- **Core concepts:** String manipulation, custom sorting, comparators, greedy ordering.
- **Key learning:** Numerical descending order does not necessarily produce the largest concatenated number.

---

## 7. Infosys Technical Interview Questions

These questions cover common programming and computer science fundamentals that may be discussed during a fresher technical interview.

### Q1. What is the difference between method overloading and method overriding?

**Answer:**

**Method Overloading** refers to defining multiple methods with the same name but different parameter lists. In languages such as Java, this is an example of compile-time polymorphism.

Example:
```java
int add(int a, int b);
int add(int a, int b, int c);
```

**Method Overriding** occurs when a subclass provides its own implementation of an inherited method with a compatible signature. It supports runtime polymorphism.

### Q2. What is the difference between `DELETE` and `TRUNCATE` in SQL?

**Answer:**

| Feature | `DELETE` | `TRUNCATE` |
|---|---|---|
| Purpose | Removes selected rows or all rows | Removes all rows |
| `WHERE` clause | Supported | Not supported |
| SQL category | DML | Commonly classified as DDL |
| Row-level filtering | Yes | No |
| Performance | Often slower for removing all rows | Often faster for removing all rows |
| Rollback | Depends on the database and transaction context | Depends on the database and transaction context |

**Important:** Do not claim that `TRUNCATE` can never be rolled back. Its transactional behavior depends on the database system.

### Q3. What is the difference between a list and a tuple in Python?

**Answer:**

The primary difference is **mutability**.

**List:**
- Mutable: elements can be added, removed, or modified.
- Uses square brackets `[]`.
- Suitable for collections that change over time.

Example:
```python
my_list = [1, 2, 3]
my_list[0] = 5
```

**Tuple:**
- Immutable: its elements cannot be reassigned after creation.
- Uses parentheses `()`.
- Suitable for fixed collections of values.

Example:
```python
my_tuple = (1, 2, 3)
```

Tuples can be more memory-efficient than lists for comparable collections, although the exact performance and memory differences depend on the use case.

---

## 8. Key Takeaways

- Never sort the elements only by their numerical values.
- Convert the elements to strings before comparing concatenations.
- Use `a + b` versus `b + a` to determine the correct ordering.
- Use `cmp_to_key()` to apply a custom comparator in Python.
- Return `"0"` when all elements are zero.
- Use `sys.stdin.read()` for efficient input handling in competitive programming.
- Remember the LeetCode equivalent: **Problem 179 — Largest Number**.
