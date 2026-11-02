# 5. Maximum Vacation Days with K Cancellations

You are given a schedule of N days represented as a binary array where 1 represents a free day and 0 represents a busy workday with obligations. You have special permission to cancel at most K busy obligations (converting at most K '0's into '1's).

Find the maximum number of consecutive vacation days (contiguous subarray of '1's) you can enjoy.

## Program

Standard input / standard output

### Input

- Line 1: Integer N (number of days) and Integer K (max cancellations)
- Line 2: N space-separated integers (0 or 1)

### Output

Print the maximum consecutive vacation days.

### Examples

**Example 1**

**Input:**
```text
11 2
1 1 0 1 1 0 0 1 1 1 1
```

**Output:**
```text
8
```

Indices 3 through 10 form an eight-day window.

**Example 2**

**Input:**
```text
5 0
1 1 0 1 1
```

**Output:**
```text
2
```

No cancellation is allowed.

### Constraints

- 1 <= N <= 10^5
- 0 <= K <= N

## 1. Plain-English Problem Breakdown

Imagine you are looking at your calendar for the next few weeks. The days are marked either as holidays (1s) or busy workdays (0s). You have a special "sick leave" quota that allows you to cancel exactly $K$ busy workdays and turn them into holidays.

Your goal is to find the absolute longest continuous stretch of holidays you can create. To do this, you don't need to guess random combinations. You just need to slide a "window" across your calendar. As long as the window contains $K$ or fewer workdays, you can technically take that entire window off. If your window accidentally swallows too many workdays, you simply shrink it from the left side until you are back within your allowed sick leave quota.

## 2. Optimal Python Solution

This problem is solved using the **Sliding Window (Two Pointers)** technique. It allows us to scan the array exactly once, dynamically expanding and shrinking our window of vacation days.

**Python**

```python id="2kgq3m"
import sys

def solve():
    # Read all inputs from standard input
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    k = int(input_data[1])
    
    # Extract the binary array of days
    arr = [int(x) for x in input_data[2 : 2 + n]]
    
    left = 0
    zero_count = 0
    max_vacation = 0
    
    # Move the 'right' pointer to expand the window day by day
    for right in range(n):
        if arr[right] == 0:
            zero_count += 1
            
        # If we exceed our cancellation limit, shrink the window from the left
        while zero_count > k:
            if arr[left] == 0:
                zero_count -= 1
            left += 1
            
        # Calculate the current valid window size and update the record
        current_window = right - left + 1
        if current_window > max_vacation:
            max_vacation = current_window
            
    print(max_vacation)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N)$, where $N$ is the number of days. Both the `left` and `right` pointers only ever move forward. In the worst-case scenario, each element is visited at most twice (once by `right` and once by `left`), making the execution time strictly linear and well within limits for $N = 10^5$.
- **Space Complexity:** $O(N)$ to store the array of days in memory. The sliding window logic itself only uses $O(1)$ constant extra space.

## 3. Logic Walkthrough for a Layman

Let's use **Example 1**: `[1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1]` with $K = 2$ sick leaves.

1. **The Setup:** We place two fingers (a Left finger and a Right finger) at the very start of the calendar.
2. **Expanding the Window:** We drag our Right finger forward. It covers a `1` (holiday), another `1` (holiday), and then hits a `0` (workday). We use 1 sick leave. Our window size is 3.
3. **Hitting the Limit:** We keep moving the Right finger. It hits another `0`. We use our 2nd sick leave. Our window stretches safely.
4. **Breaking the Limit:** The Right finger moves again and hits a third `0`. Our window now has 3 workdays, but we only have 2 sick leaves! This window is illegal.
5. **The Correction (Shrinking):** To make the window legal again, we drag our Left finger forward. We keep moving it until it passes over one of the `0`s we swallowed earlier. Once the Left finger spits a `0` out of the window, we get that sick leave back. The window is legal again, and we resume moving the Right finger.
6. **The Result:** By continuously recording the size of the window every time it is legal, we eventually capture the massive 8-day stretch at the end of the array.

## 4. LeetCode Equivalent

This is a globally standardized sliding window problem that frequently appears in Online Assessments for product and service companies alike.

- **LeetCode 1004. Max Consecutive Ones III** (Medium) - The exact same problem logic, just phrased as flipping bits instead of taking vacations.

## 5. Infosys Technical Interview Questions (Fresher Round)

**Q1: In Computer Networking, what is the difference between a MAC Address and an IP Address?**

- **Answer:** A **MAC (Media Access Control) Address** is a permanent, physical hardware address assigned to a device's network card by the manufacturer. It operates at the Data Link layer and ensures data reaches the correct physical machine on a local network. An **IP (Internet Protocol) Address** is a logical, temporary address assigned by your router or ISP. It operates at the Network layer and determines how data is routed to you across the global internet. (Think of the MAC address as your Aadhaar card, and the IP address as your current mailing address).

**Q2: How does a Hash Table resolve "Collisions"?**

- **Answer:** A collision happens when a hash function assigns two different keys to the exact same memory index. The two most common ways to resolve this are:
  1. **Chaining:** Instead of storing just a value at the index, the index holds a Linked List. If multiple keys hash to the same index, they are simply appended to that linked list.
  2. **Open Addressing (Linear Probing):** If the target index is already full, the hash table simply checks the very next available slot (index + 1) and places the data there.

**Q3: Explain the concept of "Inheritance" in OOPs. Does Java support Multiple Inheritance?**

- **Answer:** Inheritance allows a new class (Child/Derived class) to inherit the properties and methods of an existing class (Parent/Base class), promoting code reusability. Java does **not** support multiple inheritance with classes (a child class cannot `extend` more than one parent class) to avoid the "Diamond Problem" (ambiguity if both parents have a method with the same name). However, Java achieves multiple inheritance conceptually through **Interfaces**, as a class can `implement` multiple interfaces simultaneously.
