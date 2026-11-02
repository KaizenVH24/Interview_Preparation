

# 7. Longest Subsequence with Non-Zero Bitwise XOR

Given an array of non-negative integers `arr` of length `N`, find the maximum length of a subsequence whose elements have bitwise XOR strictly greater than 0. A subsequence preserves input order and may omit elements. If no such subsequence exists, return 0.

## Program

**Standard input / standard output**

### Input

- Line 1: Integer `N`
- Line 2: `N` space-separated integers

### Output

Print the length of the longest valid subsequence.

### Examples

**Example 1**

**Input:**
```text
4
2 4 5 7
```

**Output:**
```text
4
```

XOR of all elements $2 \oplus 4 \oplus 5 \oplus 7 = 4 \neq 0$. Longest subsequence = 4.

### Constraints

- `1 <= N <= 10^5`
- `0 <= arr[i] <= 10^9`

---

## 1. Plain-English Problem Breakdown

Imagine you have a group of people, and each person holds a number. We are combining their numbers using a special math operation called **XOR** (Exclusive OR).

XOR has a magical balancing property: if you XOR a number with itself, it cancels out to 0 (e.g., $5 \oplus 5 = 0$). If you XOR a number with 0, it stays exactly the same (e.g., $5 \oplus 0 = 5$).

The problem asks for the largest group of people you can pick so that their combined XOR score is *not* 0. There is a massive logical shortcut here:

1. First, check the combined score of *everyone*. If it's already greater than 0, your job is done! You can pick everyone (length $N$).
2. What if everyone's combined score exactly balances out to 0? Thanks to the XOR property, if you kick out just **one** person who holds a number greater than 0, the balance is instantly broken, and the remaining group's score will become exactly that person's number. So, the maximum group size is simply everyone minus one (length $N - 1$).
3. The only time this fails is if literally everyone in the group is holding a 0. In that case, no matter who you pick, the score is always 0. The answer is 0.

## 2. Optimal Python Solution

We do not need to check combinations or subarrays. We simply compute the total XOR of the entire array and check if any non-zero elements exist.

**Python**

```python
import sys

def solve():
    # Read all input from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    # Extract the array of numbers
    arr = [int(x) for x in input_data[1 : n + 1]]
    
    total_xor = 0
    all_zero = True
    
    # Calculate the total XOR and check if all elements are strictly 0
    for num in arr:
        total_xor ^= num
        if num > 0:
            all_zero = False
            
    # Case 1: Every single element is 0. Impossible to get XOR > 0.
    if all_zero:
        print(0)
    # Case 2: The total XOR is already > 0. The whole array is valid.
    elif total_xor > 0:
        print(n)
    # Case 3: Total XOR is 0, but there are positive numbers.
    # Removing just ONE non-zero number makes the remaining XOR > 0.
    else:
        print(n - 1)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N)$ where $N$ is the number of elements. We iterate through the array exactly once to compute the bitwise XOR. For $N = 10^5$, this is practically instantaneous.
- **Space Complexity:** $O(N)$ to store the array from the input. The algorithm itself uses $O(1)$ constant space.

## 3. Logic Walkthrough for a Layman

Let's use the logic on the array `[2, 4, 5, 7]`.

1. **The Full Test:** We XOR everything together: $2 \oplus 4 \oplus 5 \oplus 7$.
   - $2 \oplus 4 = 6$
   - $6 \oplus 5 = 3$
   - $3 \oplus 7 = 4$
2. **The Decision:** The total XOR is 4, which is greater than 0. Since the whole group works perfectly, we don't need to remove anyone. The max length is 4.

Let's look at another array: `[5, 5]`.

1. **The Full Test:** We XOR everything together: $5 \oplus 5 = 0$. The total is 0, so taking everyone doesn't work.
2. **The Rescue:** Is everyone a 0? No, there are 5s. If we remove just one `5`, the remaining array is `[5]`. The XOR of `[5]` is 5, which is greater than 0. By removing exactly one element, we fixed it. The max length is $2 - 1 = 1$.

## 4. LeetCode Equivalent

This problem perfectly captures the mathematical trickery of Bit Manipulation categories on LeetCode.

- **LeetCode 136. Single Number** (Easy) - Teaches the fundamental $A \oplus A = 0$ trick that proves why removing one element from a 0-balanced array instantly flips the XOR to > 0.
- **LeetCode 1310. XOR Queries of a Subarray** (Medium) - Applies similar prefix-XOR logic to sub-segments.

## 5. Infosys Technical Interview Questions (Fresher Round)

Bitwise operators are a favorite for testing low-level data understanding, which naturally leads to questions about memory allocation and database optimizations.

**Q1: What are the unique properties of the Bitwise XOR (`^`) operator?**

- **Answer:** XOR has two magical properties used heavily in programming logic:
  1. **Self-Cancellation:** Any number XORed with itself is exactly zero ($A \oplus A = 0$).
  2. **Identity:** Any number XORed with zero remains unchanged ($A \oplus 0 = A$). Because of this, if you XOR a list of numbers where every number appears twice except for one, all the duplicates cancel each other out, leaving only the unique number.

**Q2: In SQL, what is the difference between `UNION` and `UNION ALL`?**

- **Answer:** Both operators combine the results of two or more `SELECT` queries into a single column. However, **`UNION`** automatically scans the combined list and removes any duplicate rows (which takes extra processing time). **`UNION ALL`** blindly pastes the lists together, keeping all duplicates. Because `UNION ALL` skips the deduplication step, it is much faster.

**Q3: What is the difference between Stack Memory and Heap Memory?**

- **Answer:** **Stack Memory** is used for static memory allocation. It is highly organized, incredibly fast, and strictly follows LIFO (Last-In, First-Out). It stores local variables and function calls, and its size is limited. **Heap Memory** is used for dynamic memory allocation (like creating objects or arrays whose size might change). It is much larger and less organized. Data in the Heap stays there until the Garbage Collector deletes it or the programmer manually frees it.

