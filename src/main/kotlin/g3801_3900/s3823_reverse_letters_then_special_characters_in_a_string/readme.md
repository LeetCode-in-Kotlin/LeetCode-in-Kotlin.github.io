[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3823\. Reverse Letters Then Special Characters in a String

Easy

You are given a string `s` consisting of lowercase English letters and special characters.

Your task is to perform these **in order**:

*   **Reverse** the **lowercase letters** and place them back into the positions originally occupied by letters.
*   **Reverse** the **special characters** and place them back into the positions originally occupied by special characters.

Return the resulting string after performing the reversals.

**Example 1:**

**Input:** s = ")ebc#da@f("

**Output:** "(fad@cb#e)"

**Explanation:**

*   The letters in the string are `['e', 'b', 'c', 'd', 'a', 'f']`:
    *   Reversing them gives `['f', 'a', 'd', 'c', 'b', 'e']`
    *   `s` becomes `")fad#cb@e("`
*   The special characters in the string are `[')', '#', '@', '(']`:
    *   Reversing them gives `['(', '@', '#', ')']`
    *   `s` becomes `"(fad@cb#e)"`

**Example 2:**

**Input:** s = "z"

**Output:** "z"

**Explanation:**

The string contains only one letter, and reversing it does not change the string. There are no special characters.

**Example 3:**

**Input:** s = "!@#$%^&\*()"

**Output:** ")(\*&^%$#@!"

**Explanation:**

The string contains no letters. The string contains all special characters, so reversing the special characters reverses the whole string.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of lowercase English letters and the special characters in `"!@#$%^&*()"`.

## Solution

```kotlin
class Solution {
    fun reverseByType(s: String): String {
        val arr = s.toCharArray()
        var low = 0
        var high = arr.size - 1
        while (low < high) {
            if (arr[low] >= 'a' && arr[low] <= 'z' && arr[high] >= 'a' && arr[high] <= 'z') {
                val t = arr[low]
                arr[low] = arr[high]
                arr[high] = t
                low++
                high--
            } else if (arr[low] < 'a' || arr[low] > 'z') {
                low++
            } else {
                high--
            }
        }
        var i = 0
        var j = arr.size - 1
        while (i < j) {
            if ((arr[i] < 'a' || arr[i] > 'z') && (arr[j] < 'a' || arr[j] > 'z')) {
                val t = arr[i]
                arr[i] = arr[j]
                arr[j] = t
                i++
                j--
            } else if (arr[i] >= 'a' && arr[i] <= 'z') {
                i++
            } else {
                j--
            }
        }
        return String(arr)
    }
}
```