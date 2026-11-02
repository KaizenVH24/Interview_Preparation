

# 9. Equal Partition Sum Swap Pair

Find a pair `(x,y)`, with `x` from `list1` and `y` from `list2`, whose swap makes the two array sums equal. Prefer a valid pair with even product over every valid pair with odd product. Within the preferred parity class, choose the pair whose `x` occurs earliest in `list1`. Print `x,y`, or -1 if none exists.

## Program

**Standard input / standard output**

### Input

- Line 1: Space-separated integers for `list1`
- Line 2: Space-separated integers for `list2`

### Output

Print the pair 'x,y' or -1.

### Examples

**Example 1**

**Input:**
```text id="a7w2kp"
4 1 2 1 1 2
3 6 3 3
```

**Output:**
```text id="m3x8vz"
4,6
```

The even-product pair is preferred.

**Example 2**

**Input:**
```text id="b9q4tc"
1 2
2 3
```

**Output:**
```text id="r5n1wd"
1,2
```

The swap balances both sums.

### Constraints

- `1 <= length(list1), length(list2) <= 100000`
- All values and both array sums fit in signed 32-bit integers

---

## 1. Plain-English Problem Breakdown

Imagine two brothers, Alex and Bob, each holding a bag of coins. They empty their bags onto a table. Alex realizes his coins total $20, while Bob's total $10. Alex has $10 more than Bob.

To make things perfectly fair ($15 each), Alex needs to give Bob a coin, and Bob needs to give Alex a coin in return. The net transfer from Alex to Bob must be exactly half the difference between their totals (in this case, $\$10 \div 2 = \$5$). So, Alex must give Bob a coin that is exactly $\$5$ larger than the coin Bob gives him.

The problem asks us to find all such valid coin swaps. From the valid swaps, we must apply a strict tie-breaking rule:

1. First, prioritize pairs where `x * y` is an **even** number.
2. If there are multiple even-product pairs (or only odd-product pairs), pick the pair where Alex's coin (`x`) appeared earliest in his original bag (his input list).

If no swap can balance the bags, print `-1`.

## 2. Optimal Python Solution

Because the lists can hold up to 100,000 numbers, we cannot use a nested loop to check every combination of $x$ and $y$ ($O(N^2)$). Instead, we use mathematical equations and a Hash Set to find matches instantly.

**Python**

```python id="f6k2yp"
import sys

def solve():
    # Read all inputs
    input_data = sys.stdin.read().strip().split('\n')
    if len(input_data) < 2:
        print("-1")
        return
        
    list1 = [int(i) for i in input_data[0].split()]
    list2 = [int(i) for i in input_data[1].split()]
    
    sum1 = sum(list1)
    sum2 = sum(list2)
    
    # If the difference between the sums is odd, a perfect swap is mathematically impossible
    # (because we can only transfer an integer amount, which must be half the difference)
    diff = sum1 - sum2
    if diff % 2 != 0:
        print("-1")
        return
        
    # The target difference we need between x and y
    # Since sum1 - x + y = sum2 - y + x
    # 2(x - y) = sum1 - sum2
    # x - y = (sum1 - sum2) / 2
    target_transfer = diff // 2
    
    # Store list2 in a Set for O(1) instant lookups
    set2 = set(list2)
    
    best_even_pair = None
    best_even_index = float('inf')
    best_odd_pair = None
    best_odd_index = float('inf')
    
    # Scan through list1 sequentially
    for i, x in enumerate(list1):
        # We need to find a 'y' such that x - y = target_transfer
        # Therefore, y = x - target_transfer
        y = x - target_transfer
        
        # Check if this specific 'y' exists in Bob's bag
        if y in set2:
            product = x * y
            if product % 2 == 0:
                # First valid even-product pair wins
                if best_even_pair is None:
                    best_even_pair = (x, y)
                    best_even_index = i
            else:
                # First valid odd-product pair wins
                if best_odd_pair is None:
                    best_odd_pair = (x, y)
                    best_odd_index = i
                    
    # Priority 1: Did we find an even-product pair?
    if best_even_pair:
        print(f"{best_even_pair[0]},{best_even_pair[1]}")
    # Priority 2: Did we find an odd-product pair?
    elif best_odd_pair:
        print(f"{best_odd_pair[0]},{best_odd_pair[1]}")
    # We found nothing
    else:
        print("-1")

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N + M)$ where $N$ and $M$ are the sizes of `list1` and `list2`. Summing the lists takes $O(N + M)$. Creating the set for `list2` takes $O(M)$. Iterating through `list1` takes $O(N)$, and the set lookup inside the loop is $O(1)$. This is vastly superior to the naive $O(N \times M)$ approach.
- **Space Complexity:** $O(M)$ to store `list2` inside a Hash Set.

## 3. Logic Walkthrough for a Layman

Let's use the Example 1 inputs:

- Alex's Bag: `[4, 1, 2, 1, 1, 2]` (Total = 11)
- Bob's Bag: `[3, 6, 3, 3]` (Total = 15)

1. **The Math:** The difference is $11 - 15 = -4$. Half the difference is $-2$. To balance the bags, Alex needs to give Bob a coin that is exactly **2 less** than the coin he receives.
2. **The Lookup:** Bob dumps his coins into a sorting bin (a Hash Set): `{3, 6}`.
3. **The Sweep:** Alex picks up his coins one by one in order:
   - **Coin 4:** Alex says, "I need Bob to give me a 6." Does Bob have a 6? Yes! Swap pair is `(4, 6)`. The product is $4 \times 6 = 24$ (Even). Since we read from left to right, this is our top even candidate.
   - **Coin 1:** Alex says, "I need Bob to give me a 3." Does Bob have a 3? Yes! Swap pair is `(1, 3)`. The product is $1 \times 3 = 3$ (Odd). This is our top odd candidate.
   - **Coin 2:** Alex needs a 4. Bob doesn't have a 4. Ignore.
   - (The rest are ignored because we already locked in the earliest candidates).
4. **The Verdict:** The problem forces us to prefer the even-product pair over the odd-product pair. So, the winner is `(4, 6)`.

## 4. LeetCode Equivalent

This is one of the most famous algorithmic math puzzles in competitive programming.

- **LeetCode 888. Fair Candy Swap** (Easy) - This is the exact 1:1 identical problem in terms of mathematical logic, but the Infosys version adds the "Even-product preference" twist.

## 5. Infosys Technical Interview Questions (Fresher Round)

Questions leveraging Hash Sets and Sets often lead to questions about searching, indexing, and underlying language implementations.

**Q1: What is the difference between Linear Search and Binary Search?**

- **Answer:** **Linear Search** starts at the beginning of an array and checks every single element one by one until it finds the target (Time Complexity: $O(N)$). It works on any unsorted data. **Binary Search** is much faster but *strictly requires the array to be sorted*. It repeatedly divides the search interval in half. If the target is less than the middle element, it ignores the right half and searches the left half (Time Complexity: $O(\log N)$).

**Q2: In Python, what is the difference between a `tuple` and a `list`?**

- **Answer:** The primary difference is mutability. A **`list`** uses square brackets `[]` and is mutable, meaning you can add, remove, or change elements after it is created. A **`tuple`** uses parentheses `()` and is immutable; once it is created, its contents can never be changed. Because tuples are immutable, they consume slightly less memory and can be used as keys in a dictionary (which lists cannot do).

**Q3: What does the `static` keyword mean in Java or C++?**

- **Answer:** The `static` keyword means that a variable or a method belongs to the *Class* itself, rather than to any specific object (instance) created from that class.
  - A static variable acts as a global counter shared by all objects (if one object changes it, it changes for everyone).
  - A static method can be called directly using the Class name (like `Math.pow()`) without needing to create an object first using the `new` keyword.

