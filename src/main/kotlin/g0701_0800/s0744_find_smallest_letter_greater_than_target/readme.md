[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 744\. Find Smallest Letter Greater Than Target

Easy

You are given an array of characters `letters` that is sorted in **non-decreasing order**, and a character `target`. There are **at least two different** characters in `letters`.

Return _the smallest character in_ `letters` _that is lexicographically greater than_ `target`. If such a character does not exist, return the first character in `letters`.

**Example 1:**

**Input:** letters = ["c","f","j"], target = "a"

**Output:** "c"

**Explanation:** The smallest character that is lexicographically greater than 'a' in letters is 'c'.

**Example 2:**

**Input:** letters = ["c","f","j"], target = "c"

**Output:** "f"

**Explanation:** The smallest character that is lexicographically greater than 'c' in letters is 'f'.

**Example 3:**

**Input:** letters = ["x","x","y","y"], target = "z"

**Output:** "x"

**Explanation:** There are no characters in letters that is lexicographically greater than 'z' so we return letters[0].

**Constraints:**

*   <code>2 <= letters.length <= 10<sup>4</sup></code>
*   `letters[i]` is a lowercase English letter.
*   `letters` is sorted in **non-decreasing** order.
*   `letters` contains at least two different characters.
*   `target` is a lowercase English letter.

## Solution

```kotlin
class Solution {
    fun nextGreatestLetter(letters: CharArray, target: Char): Char {
        var start = 0
        var end = letters.size - 1
        // If target is greater than last element return first element of the array.
        if (letters[letters.size - 1] <= target) {
            return letters[start]
        }
        while (start < end) {
            val mid = start + (end - start) / 2
            if (letters[mid] <= target) {
                start = mid + 1
            } else {
                end = mid
            }
        }
        return letters[start]
    }
}
```