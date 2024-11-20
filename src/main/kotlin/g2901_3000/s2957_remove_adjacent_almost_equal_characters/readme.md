[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2957\. Remove Adjacent Almost-Equal Characters

Medium

You are given a **0-indexed** string `word`.

In one operation, you can pick any index `i` of `word` and change `word[i]` to any lowercase English letter.

Return _the **minimum** number of operations needed to remove all adjacent **almost-equal** characters from_ `word`.

Two characters `a` and `b` are **almost-equal** if `a == b` or `a` and `b` are adjacent in the alphabet.

**Example 1:**

**Input:** word = "aaaaa"

**Output:** 2

**Explanation:** We can change word into "a**<ins>c</ins>**a<ins>**c**</ins>a" which does not have any adjacent almost-equal characters. 

It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 2.

**Example 2:**

**Input:** word = "abddez"

**Output:** 2

**Explanation:** We can change word into "**<ins>y</ins>**bd<ins>**o**</ins>ez" which does not have any adjacent almost-equal characters. 

It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 2.

**Example 3:**

**Input:** word = "zyxyxyz"

**Output:** 3

**Explanation:** We can change word into "z<ins>**a**</ins>x<ins>**a**</ins>x**<ins>a</ins>**z" which does not have any adjacent almost-equal characters.

It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 3.

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun removeAlmostEqualCharacters(word: String): Int {
        var count = 0
        val wordArray = word.toCharArray()
        for (i in 1 until wordArray.size) {
            if (abs((wordArray[i].code - wordArray[i - 1].code).toDouble()) <= 1) {
                count++
                wordArray[i] =
                    if ((
                        i + 1 < wordArray.size &&
                            (wordArray[i + 1] != 'a' && wordArray[i + 1] != 'b')
                        )
                    ) {
                        'a'
                    } else {
                        'z'
                    }
            }
        }
        return count
    }
}
```