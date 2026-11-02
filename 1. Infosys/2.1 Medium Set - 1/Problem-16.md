# 1. RPG Monster Defeat Quest

While playing an RPG game, you are assigned to complete a challenging quest with n monsters. Each monster i is described by two integers: power[i] (experience required to defeat it) and bonus[i] (experience gained upon defeating it).

To defeat monster i, your current experience points must be strictly greater than or equal to power[i]. If you attempt to fight a monster without sufficient experience, you lose immediately. When you defeat monster i, your experience points increase by bonus[i].

You can fight the monsters in any arbitrary order. You are given an initial experience point e. Find and return the maximum number of monsters you can defeat.

## Program

Standard input / standard output

### Input

- Line 1: Integer n (number of monsters)
- Line 2: Integer e (initial experience)
- Line 3: n space-separated integers representing power array
- Line 4: n space-separated integers representing bonus array

### Output

Print a single integer denoting the maximum number of monsters defeated.

### Examples

**Example 1**

**Input:**
```text
2
123
78 130
10 0
```

**Output:**
```text
2
```

Initial exp = 123. Monster 0 requires power 78 <= 123. Defeating it gives bonus 10 -> new exp = 133. Now fight Monster 1 (power 130 <= 133). Defeating it gives bonus 0 -> total defeated = 2.

**Example 2**

**Input:**
```text
3
100
101 100 304
100 1 524
```

**Output:**
```text
2
```

Initial exp = 100. Fight Monster 1 (power 100 <= 100) -> new exp = 101. Next fight Monster 0 (power 101 <= 101) -> new exp = 201. Monster 2 requires power 304 > 201, cannot defeat. Total = 2.

### Constraints

- 1 <= n <= 100000
- 0 <= e <= 1000000000
- 1 <= power[i] <= 1000000000
- 0 <= bonus[i] <= 1000000000
- The power and bonus lines each contain exactly n integers

## 1. Plain-English Problem Breakdown

Imagine you are playing an RPG video game. You start with a certain amount of experience points (XP). You are standing in a field with a bunch of monsters, and you get to choose the exact order in which you fight them.

Every time you defeat a monster, you gain its XP bonus, making you stronger for the next fight. Because you want to defeat as many monsters as possible, there is absolutely no reason to challenge a high-level boss first and risk losing. Your best strategy is purely "Greedy": you should relentlessly grind the weakest available monsters first to harvest their XP, continuously leveling up until you are strong enough to take on the harder ones.

If you sort all the monsters from weakest to strongest and fight them in that order, you guarantee the maximum possible number of victories.

## 2. Optimal Python Solution

We combine the power and bonus arrays into pairs, sort them based on the power requirement, and simulate the fights.

**Python**

```python
import sys

def solve():
    # Read all input from standard input efficiently
    input_data = sys.stdin.read().split()
    if not input_data:
        return
        
    n = int(input_data[0])
    e = int(input_data[1])
    
    # Extract the power and bonus arrays
    power = [int(x) for x in input_data[2 : 2 + n]]
    bonus = [int(x) for x in input_data[2 + n : 2 + 2 * n]]
    
    # Pair each monster's power and bonus together: [(power1, bonus1), (power2, bonus2), ...]
    monsters = list(zip(power, bonus))
    
    # Sort the monsters primarily by their required power (weakest first)
    monsters.sort(key=lambda x: x[0])
    
    defeated_count = 0
    
    # Iterate through the sorted monsters and fight them one by one
    for req_power, xp_bonus in monsters:
        if e >= req_power:
            # We have enough XP to win. Defeat it and claim the bonus.
            e += xp_bonus
            defeated_count += 1
        else:
            # The moment we hit a monster we can't defeat, we stop.
            # Since the array is sorted, we know we can't defeat any monsters after this one either.
            break
            
    print(defeated_count)

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** $O(N \log N)$ where $N$ is the number of monsters. Pairing the arrays and iterating through them takes $O(N)$ time, but sorting the monsters takes $O(N \log N)$ time, which dictates the overall speed. For $N = 100,000$, this executes in a fraction of a second.
- **Space Complexity:** $O(N)$ to store the pairs of power and bonus in the `monsters` list.

## 3. Logic Walkthrough for a Layman

Let's use **Example 2**:

- Initial XP = `100`
- Required Powers = `[101, 100, 304]`
- XP Bonuses = `[100, 1, 524]`

1. **The Pairing:** We link the stats to form specific monsters: Monster A `(101, 100)`, Monster B `(100, 1)`, and Monster C `(304, 524)`.
2. **The Sorting Hat:** We organize the monsters from easiest to hardest based on their required power:
   - First: `(100, 1)`
   - Second: `(101, 100)`
   - Third: `(304, 524)`
3. **The Arena (Fight 1):** We face the first monster requiring `100` power. Our XP is `100`. It's a tie, which means we win! We absorb its `1` XP bonus. Our new XP is `101`. (Defeated: 1)
4. **Fight 2:** We face the second monster requiring `101` power. Our XP is exactly `101`. We win again! We absorb its massive `100` XP bonus. Our new XP is `201`. (Defeated: 2)
5. **Fight 3:** We face the third monster requiring `304` power. Our XP is only `201`. We are too weak and lose immediately. We exit the arena.
6. **The Result:** We successfully defeated 2 monsters.

## 4. LeetCode Equivalent

This specific greedy logic is incredibly famous in competitive programming.

- **Codeforces 230A (Dragons):** This is the exact 1:1 identical problem, famously phrased as Kirito fighting dragons to steal their strength.
- **LeetCode 826. Most Profit Assigning Work** (Medium) - Uses the exact same "pair up, sort by difficulty, and iterate" logic.

## 5. Infosys Technical Interview Questions (Fresher Round)

For algorithm questions involving arrays and sorting, expect the interviewer to test your deeper understanding of memory management and OOP design.

**Q1: In Python, what is the difference between a Shallow Copy and a Deep Copy?**

- **Answer:** A **Shallow Copy** creates a new object, but inserts *references* (memory pointers) to the items contained in the original. If you change a nested object (like a list inside a list) in the copy, it also changes in the original. A **Deep Copy** creates a completely new, independent clone of the object and all of its nested objects. Modifying the deep copy has absolutely zero effect on the original.

**Q2: What is the difference between an Abstract Class and an Interface in Object-Oriented Programming?**

- **Answer:** An **Interface** acts as a strict contract; it only contains method signatures with no actual code body (100% abstraction), forcing the child class to write all the logic. An **Abstract Class** can be a mix of both; it can have blank abstract methods, but it can also have fully coded methods that child classes can inherit and use directly. (Note: Python handles interfaces via abstract base classes with only abstract methods).

**Q3: In a Database, what is a Stored Procedure?**

- **Answer:** A Stored Procedure is a prepared SQL code that you can save directly on the database server so it can be reused over and over again. Instead of a frontend application sending a massive block of SQL queries across the network, it just sends a single command to execute the stored procedure. This reduces network traffic, improves performance, and enhances security against SQL injection.
