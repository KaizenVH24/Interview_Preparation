# Bracket Sequence Mismatch Position

## 1. Problem Statement

Given a non-empty string containing only parentheses `()`, curly brackets `{}`, and square brackets `[]`, identify the position of the first bracket mismatch using standard stack-based matching.

Follow these rules:

1. Scan the string from left to right.
2. When an opening bracket is encountered, push it and its 1-based position onto a stack.
3. When a closing bracket is encountered:
   - If the stack is empty, the closing bracket is unmatched. Print its position.
   - If the closing bracket does not match the stack's top opening bracket, print the closing bracket's position.
   - Otherwise, remove the matching opening bracket from the stack.
4. If no closing mismatch occurs but opening brackets remain after processing the entire string, print the position of the earliest unmatched opening bracket.
5. If the sequence is balanced, print `0`.

### Input Format

A single line containing the bracket string `S`.

### Output Format

Print `0` if the sequence is balanced. Otherwise, print the 1-based position of the first mismatch according to the rules above.

### Examples

**Example 1**

Input:
```text
{([])}
```

Output:
```text
0
```

Explanation:

Every opening bracket has a corresponding closing bracket in the correct order. Therefore, the sequence is balanced.

**Example 2**

Input:
```text
{[[]])}
```

Output:
```text
6
```

Explanation:

The bracket at position `6` is `)`, but the most recently opened unmatched bracket is `[`. Since the brackets do not match, the first closing mismatch occurs at position `6`.

### Constraints

- `1 <= length(S) <= 100000`
- `S` contains only bracket characters: `(`, `)`, `{`, `}`, `[` and `]`.

---

## 2. Plain-English Problem Breakdown

Imagine opening and closing boxes inside one another. Every box must be closed in the reverse order in which it was opened.

For example:

```text
{[()]}
```

This sequence is valid because the innermost bracket closes first, followed by the surrounding brackets.

There are three possible outcomes.

### Case 1: Incorrect Closing Bracket

If a closing bracket does not match the most recently opened bracket, report the position of that closing bracket.

Example:

```text
{[)}
```

At position `3`, the closing bracket `)` does not match the most recently opened bracket `[`. Therefore, the output is `3`.

### Case 2: Unmatched Opening Bracket

If the string ends while some opening brackets remain unmatched, report the position of the earliest unmatched opening bracket.

Example:

```text
{[(
```

The opening brackets at positions `1`, `2`, and `3` remain unmatched. The earliest one is at position `1`, so the output is `1`.

### Case 3: Balanced Sequence

If every bracket matches correctly and no opening brackets remain, print `0`.

Example:

```text
([])
```

Output:

```text
0
```

---

## 3. Optimal Python Solution

The following solution uses a stack to track opening brackets and their positions. It reads input from standard input and prints the result to standard output.

```python
import sys


def solve():
    s = sys.stdin.read().strip()

    if not s:
        return

    stack = []
    matching_bracket = {
        ')': '(',
        '}': '{',
        ']': '['
    }

    for i, char in enumerate(s):
        position = i + 1

        if char in "({[":
            stack.append((char, position))

        else:
            if not stack:
                print(position)
                return

            top_char, top_position = stack.pop()

            if top_char != matching_bracket[char]:
                print(position)
                return

    # Handle unmatched opening brackets.
    if stack:
        print(stack[0][1])
    else:
        print(0)


if __name__ == "__main__":
    solve()
```

### How the Code Works

1. **Initialize the stack:** Store each opening bracket along with its 1-based position.
2. **Define matching pairs:** Use a dictionary to map each closing bracket to its corresponding opening bracket.
3. **Scan the string:** Process each character from left to right.
4. **Handle opening brackets:** Push the bracket and its position onto the stack.
5. **Handle closing brackets:** If the stack is empty or the top bracket does not match, print the current position and stop.
6. **Check remaining brackets:** If the stack is not empty after processing the string, print the position of the earliest unmatched opening bracket.
7. **Print zero:** If no mismatch exists and the stack is empty, print `0`.

---

## 4. Logic Walkthrough

Consider the input:

```text
{[[]])}
```

### Step 1: Process the Opening Brackets

The stack stores each opening bracket and its position.

| Position | Character | Stack after processing |
|---:|:---:|---|
| 1 | `{` | `[('{', 1)]` |
| 2 | `[` | `[('{', 1), ('[', 2)]` |
| 3 | `[` | `[('{', 1), ('[', 2), ('[', 3)]` |
| 4 | `]` | `[('{', 1), ('[', 2)]` |
| 5 | `]` | `[('{', 1)]` |

At position `4`, `]` correctly matches `[`. At position `5`, the next `]` does not match the `{` currently at the top of the stack.

Therefore, the first mismatch occurs at position `6`, where the character is `)`.

Output:

```text
6
```

### Step 2: Understand the Unmatched Opening Case

Consider:

```text
{[(
```

After processing all characters, the stack contains:

```python
[('{', 1), ('[', 2), ('(', 3)]
```

The earliest unmatched opening bracket is at the bottom of the stack, at position `1`.

Output:

```text
1
```

### Step 3: Understand the Balanced Case

Consider:

```text
{([])}
```

Every closing bracket matches the stack's top opening bracket. The stack is empty at the end.

Output:

```text
0
```

---

## 5. Complexity Analysis

Let `N` be the length of the input string.

| Operation | Complexity |
|---|---|
| Scanning the string | `O(N)` |
| Stack push and pop operations | `O(1)` per operation |
| Checking the earliest unmatched opening bracket | `O(1)` |
| **Overall time complexity** | **`O(N)`** |
| **Auxiliary space complexity** | **`O(N)`** |

In the worst case, the string contains only opening brackets, requiring the stack to store all `N` characters and their positions.

The algorithm is optimal in time complexity because every character may need to be examined.

---

## 6. LeetCode Equivalent

**[LeetCode 20 — Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)**

- **Difficulty:** Easy
- **Core concepts:** Stack, string traversal, matching pairs.
- **Similarity:** Both problems use a stack to verify that brackets are correctly matched and nested.
- **Difference:** This problem requires the exact 1-based position of a mismatch and specifies how to handle unmatched opening brackets.

---

## 7. Infosys Technical Interview Questions

### Q1. What are the real-world applications of a Stack?

**Answer:**

A stack follows the **Last-In, First-Out (LIFO)** principle. The last element inserted is the first element removed.

Common applications include:

1. **Function call stack:** Tracks active function calls and their execution state.
2. **Undo operations:** Reverses the most recent action first.
3. **Browser navigation:** Helps manage navigation history.
4. **Expression evaluation:** Evaluates arithmetic expressions and checks operator precedence.
5. **Bracket matching:** Verifies whether brackets are correctly nested.
6. **Backtracking:** Supports algorithms such as depth-first search.

### Q2. What is the difference between `==` and `is` in Python?

**Answer:**

- `==` checks **value equality**: whether two objects contain equal values.
- `is` checks **object identity**: whether two references point to the same object.

Example:

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(a == b)  # True
print(a is b)  # False
```

Both lists contain the same values, but they are separate objects.

### Q3. What is the difference between a Compiler and an Interpreter?

**Answer:**

A **compiler** translates source code into another representation, often machine code or bytecode, before the resulting program or code is executed.

An **interpreter** executes a program by interpreting its instructions or an intermediate representation at runtime.

| Feature | Compiler | Interpreter |
|---|---|---|
| Main role | Translates source code into another representation | Executes code through runtime interpretation |
| Execution model | Often involves a separate compilation step | Often involves interpretation during execution |
| Error reporting | May report compilation errors before execution | May report runtime errors as execution proceeds |
| Examples | C and C++ commonly use native-code compilation | Python commonly uses bytecode and a virtual machine |

Modern language implementations may combine compilation and interpretation, so the distinction is not always absolute.

---

## 8. Key Takeaways

- Use a stack to track opening brackets and their positions.
- Always compare a closing bracket with the most recently opened unmatched bracket.
- If a closing bracket has no matching stack top, report its position immediately.
- If opening brackets remain at the end, report the earliest unmatched opening bracket.
- Use 1-based indexing when reporting positions.
- Print `0` only when the sequence is balanced.
- The solution runs in **`O(N)` time** and requires **`O(N)` auxiliary space**.
- Practise LeetCode 20 to strengthen your understanding of stack-based bracket matching.

