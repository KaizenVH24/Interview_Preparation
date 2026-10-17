# Alternate Even and Odd Digits by Special Character Parity

You are given a string S containing at least one special character, at least one even digit, and at least one odd digit.

1. Count special characters, defined as characters that are not alphanumeric.
2. Collect even digits in their original left-to-right order and collect odd digits in their original left-to-right order.
3. If the special-character count is even, alternate the collected digits starting with even. If it is odd, alternate starting with odd.
4. When one parity group runs out, append the remaining digits from the other group in their original relative order.

## Program

Standard input / standard output

### Input

A single line containing the string S.

### Output

Print the resulting reordered digits string.

### Examples

#### Example 1

**Input:** `A5c67r21i@p#8t`

**Output:** `652781`

Special characters = {'@', '#'} -> count is 2 (even). Even digits: [6, 2, 8], Odd digits: [5, 7, 1]. Starting with even: 6, 5, 2, 7, 8, 1 -> '652781'.

### Constraints

- 1 <= length(S) <= 100000
- S contains only printable ASCII characters
- S contains at least one non-alphanumeric character, one even digit, and one odd digit
- A special character is any ASCII character outside A-Z, a-z, and 0-9

---

## 1. Plain-English Problem Breakdown

Imagine you have two separate decks of cards: one containing only even numbers, and the other containing only odd numbers. You also have a secret code—a string of random characters.

Your first task is to count every "special symbol" in that code (anything that is not a letter or a number, like `@`, `#`, or `$`).

- If you counted an **even** number of symbols, you start building your final sequence by placing an **even** card first.
- If you counted an **odd** number of symbols, you start with an **odd** card first.

Once you know who goes first, you simply take turns (alternate) pulling one card from the even deck, then one from the odd deck, over and over like a zipper. If one deck runs out of cards before the other, you just attach the rest of the remaining deck to the very end of your sequence.

## 2. Optimal Python Solution

To process 100,000 characters efficiently, we will do a single scan of the string to categorize everything, and then a quick merge.

**Python**

```python
import sys

def solve():
    # Read the input string
    s = sys.stdin.read().strip()
    if not s:
        return
        
    special_count = 0
    evens = []
    odds = []
    
    # Single pass to categorize characters
    for char in s:
        if not char.isalnum():
            # Not a letter or number -> Special character
            special_count += 1
        elif char.isdigit():
            # Categorize digits into even and odd lists
            if int(char) % 2 == 0:
                evens.append(char)
            else:
                odds.append(char)
                
    # Determine the starting and secondary lists based on special_count parity
    if special_count % 2 == 0:
        first_group = evens
        second_group = odds
    else:
        first_group = odds
        second_group = evens
        
    result = []
    idx1, idx2 = 0, 0
    len1, len2 = len(first_group), len(second_group)
    
    # Alternate appending from both groups
    while idx1 < len1 or idx2 < len2:
        if idx1 < len1:
            result.append(first_group[idx1])
            idx1 += 1
        if idx2 < len2:
            result.append(second_group[idx2])
            idx2 += 1
            
    # Join the final list into a string and print
    print("".join(result))

if __name__ == "__main__":
    solve()
```

**Complexity Analysis:**

- **Time Complexity:** \(O(N)\), where \(N\) is the length of the string. We loop through the string exactly once to separate the characters, and then loop once more to merge the digits. The `isalnum()` and `isdigit()` string methods run in constant \(O(1)\) time.
- **Space Complexity:** \(O(N)\) because we are storing the extracted digits in separate `evens` and `odds` lists, and building a `result` array. The maximum memory consumed will easily fit within standard limits.

## 3. Logic Walkthrough for a Layman

1. **The Sorting Hat (Categorization):** We read the input string `"A5c67r21i@p#8t"` one letter at a time.
   - When we see `@` and `#`, we recognize them as symbols and update our symbol counter to 2.
   - When we see `6`, `2`, and `8`, we push them into our "Evens" line.
   - When we see `5`, `7`, and `1`, we push them into our "Odds" line.
   - We completely ignore the alphabet letters.
2. **The Coin Toss (Parity Check):** We check our symbol counter. We found exactly 2 symbols. Because 2 is an even number, the "Evens" line gets to go first.
3. **The Zipper Merge:** We stand at the front of both lines.
   - We pull the first even (`6`), then the first odd (`5`).
   - Then the second even (`2`), then the second odd (`7`).
   - Then the third even (`8`), then the third odd (`1`).
4. **The Cleanup:** If the odds line had more people (say, an extra `3` and `9`), we would just append `"39"` to the end. Since both lines were exactly 3 digits long, we perfectly zip them up and print `"652781"`.

## 4. LeetCode Equivalent

This specific string-parsing structure is an InfyTQ signature style, but the exact "zipper merge" logic is a very common LeetCode pattern.

- **LeetCode 1768. Merge Strings Alternately** (Easy) - Teaches the exact alternating two-pointer logic used to zip the two arrays together.
- **LeetCode 2149. Rearrange Array Elements by Sign** (Medium) - Segregates positives and negatives, then perfectly alternates them back into a single array.

## 5. Infosys Technical Interview Questions (Fresher Round)

For logic and string parsing questions, Infosys interviewers usually pivot to asking about how the web works and fundamental database retrieval concepts.

**Q1: What is the exact difference between an `INNER JOIN` and a `LEFT JOIN` in SQL?**

- **Answer:** An **INNER JOIN** returns only the rows where there is a match in *both* tables. If a record in Table A has no corresponding match in Table B, it gets dropped. A **LEFT JOIN** returns *all* the records from the left table (Table A), and any matching records from the right table (Table B). If there is no match for a row on the left side, SQL simply fills the right side's columns with `NULL`.

**Q2: How does Garbage Collection work in Python?**

- **Answer:** Python primarily uses **Reference Counting** for garbage collection. Every object in memory keeps a tally of how many variables are pointing to it. When that count drops to zero (e.g., the variable is overwritten or the function ends), Python automatically deletes the object and frees the memory. It also has a secondary cyclic garbage collector to detect and clean up objects that reference each other (circular references).

**Q3: What is the difference between an HTTP `GET` request and a `POST` request?**

- **Answer:** A **GET** request is used to retrieve data from a server (like loading a webpage). The data you send is visible in the URL, so it should never be used for sensitive information like passwords. It is also idempotent (calling it multiple times doesn't change anything). A **POST** request is used to submit or send data to the server (like submitting a login form). The data is hidden securely inside the request body, not the URL.
