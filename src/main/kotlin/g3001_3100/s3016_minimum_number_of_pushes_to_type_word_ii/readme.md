[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3016\. Minimum Number of Pushes to Type Word II

Medium

You are given a string `word` containing lowercase English letters.

Telephone keypads have keys mapped with **distinct** collections of lowercase English letters, which can be used to form words by pushing them. For example, the key `2` is mapped with `["a","b","c"]`, we need to push the key one time to type `"a"`, two times to type `"b"`, and three times to type `"c"` _._

It is allowed to remap the keys numbered `2` to `9` to **distinct** collections of letters. The keys can be remapped to **any** amount of letters, but each letter **must** be mapped to **exactly** one key. You need to find the **minimum** number of times the keys will be pushed to type the string `word`.

Return _the **minimum** number of pushes needed to type_ `word` _after remapping the keys_.

An example mapping of letters to keys on a telephone keypad is given below. Note that `1`, `*`, `#`, and `0` do **not** map to any letters.

![](https://assets.leetcode.com/uploads/2023/12/26/keypaddesc.png)

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/12/26/keypadv1e1.png)

**Input:** word = "abcde"

**Output:** 5

**Explanation:** The remapped keypad given in the image provides the minimum cost. 

"a" -> one push on key 2 

"b" -> one push on key 3 

"c" -> one push on key 4 

"d" -> one push on key 5 

"e" -> one push on key 6 

Total cost is 1 + 1 + 1 + 1 + 1 = 5. 

It can be shown that no other mapping can provide a lower cost.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/12/26/keypadv2e2.png)

**Input:** word = "xyzxyzxyzxyz"

**Output:** 12

**Explanation:** The remapped keypad given in the image provides the minimum cost. 

"x" -> one push on key 2 

"y" -> one push on key 3 

"z" -> one push on key 4 Total cost is 1 * 4 + 1 * 4 + 1 * 4 = 12 

It can be shown that no other mapping can provide a lower cost. 

Note that the key 9 is not mapped to any letter: it is not necessary to map letters to every key, but to map all the letters.

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/12/27/keypadv2.png)

**Input:** word = "aabbccddeeffgghhiiiiii"

**Output:** 24

**Explanation:** The remapped keypad given in the image provides the minimum cost. 

"a" -> one push on key 2 

"b" -> one push on key 3 

"c" -> one push on key 4 

"d" -> one push on key 5 

"e" -> one push on key 6 

"f" -> one push on key 7 

"g" -> one push on key 8 

"h" -> two pushes on key 9 

"i" -> one push on key 9 

Total cost is 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 2 * 2 + 6 * 1 = 24.

It can be shown that no other mapping can provide a lower cost.

**Constraints:**

*   <code>1 <= word.length <= 10<sup>5</sup></code>
*   `word` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minimumPushes(word: String): Int {
        val count = IntArray(26)
        val l = word.length
        for (i in 0 until l) {
            ++count[word[i].code - 'a'.code]
        }
        var j = 8
        var result = 0
        while (true) {
            var mi = 0
            for (i in 0..25) {
                if (count[mi] < count[i]) {
                    mi = i
                }
            }
            if (count[mi] == 0) {
                break
            }
            result += (j / 8) * count[mi]
            count[mi] = 0
            ++j
        }
        return result
    }
}
```