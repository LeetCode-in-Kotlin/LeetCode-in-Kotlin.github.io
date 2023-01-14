[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 514\. Freedom Trail

Hard

In the video game Fallout 4, the quest **"Road to Freedom"** requires players to reach a metal dial called the **"Freedom Trail Ring"** and use the dial to spell a specific keyword to open the door.

Given a string `ring` that represents the code engraved on the outer ring and another string `key` that represents the keyword that needs to be spelled, return _the minimum number of steps to spell all the characters in the keyword_.

Initially, the first character of the ring is aligned at the `"12:00"` direction. You should spell all the characters in `key` one by one by rotating `ring` clockwise or anticlockwise to make each character of the string key aligned at the `"12:00"` direction and then by pressing the center button.

At the stage of rotating the ring to spell the key character `key[i]`:

1.  You can rotate the ring clockwise or anticlockwise by one place, which counts as **one step**. The final purpose of the rotation is to align one of `ring`'s characters at the `"12:00"` direction, where this character must equal `key[i]`.
2.  If the character `key[i]` has been aligned at the `"12:00"` direction, press the center button to spell, which also counts as **one step**. After the pressing, you could begin to spell the next character in the key (next stage). Otherwise, you have finished all the spelling.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/ring.jpg)

**Input:** ring = "godding", key = "gd"

**Output:** 4

**Explanation:** For the first key character 'g', since it is already in place, we just need 1 step to spell this character. For the second key character 'd', we need to rotate the ring "godding" anticlockwise by two steps to make it become "ddinggo". Also, we need 1 more step for spelling. So the final output is 4.

**Example 2:**

**Input:** ring = "godding", key = "godding"

**Output:** 13

**Constraints:**

*   `1 <= ring.length, key.length <= 100`
*   `ring` and `key` consist of only lower case English letters.
*   It is guaranteed that `key` could always be spelled by rotating `ring`.

## Solution

```kotlin
class Solution {
    fun findRotateSteps(ring: String, key: String): Int {
        val indexs: Array<MutableList<Int>?> = arrayOfNulls<MutableList<Int>?>(26)
        for (i in 0 until ring.length) {
            val num = ring[i].code - 'a'.code
            if (indexs[num] == null) {
                indexs[num] = ArrayList()
            }
            indexs[num]!!.add(i)
        }
        val dp = Array(101) { IntArray(101) }
        return find(ring, 0, key, 0, dp, indexs)
    }

    private fun find(
        ring: String,
        i: Int,
        key: String,
        j: Int,
        cache: Array<IntArray>,
        indexs: Array<MutableList<Int>?>
    ): Int {
        if (j == key.length) {
            return 0
        }
        if (cache[i][j] != 0) {
            return cache[i][j]
        }
        var ans = Int.MAX_VALUE
        val targets: List<Int>? = indexs[key[j].code - 'a'.code]
        for (t in targets!!) {
            var delta = Math.abs(i - t)
            delta = Math.min(delta, Math.abs(ring.length - delta))
            ans = Math.min(ans, 1 + delta + find(ring, t, key, j + 1, cache, indexs))
        }
        cache[i][j] = ans
        return ans
    }
}
```