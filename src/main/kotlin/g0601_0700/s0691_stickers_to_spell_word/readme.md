[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 691\. Stickers to Spell Word

Hard

We are given `n` different types of `stickers`. Each sticker has a lowercase English word on it.

You would like to spell out the given string `target` by cutting individual letters from your collection of stickers and rearranging them. You can use each sticker more than once if you want, and you have infinite quantities of each sticker.

Return _the minimum number of stickers that you need to spell out_ `target`. If the task is impossible, return `-1`.

**Note:** In all test cases, all words were chosen randomly from the `1000` most common US English words, and `target` was chosen as a concatenation of two random words.

**Example 1:**

**Input:** stickers = ["with","example","science"], target = "thehat"

**Output:** 3

**Explanation:** We can use 2 "with" stickers, and 1 "example" sticker. After cutting and rearrange the letters of those stickers, we can form the target "thehat". Also, this is the minimum number of stickers necessary to form the target string.

**Example 2:**

**Input:** stickers = ["notice","possible"], target = "basicbasic"

**Output:** -1 Explanation: We cannot form the target "basicbasic" from cutting letters from the given stickers.

**Constraints:**

*   `n == stickers.length`
*   `1 <= n <= 50`
*   `1 <= stickers[i].length <= 10`
*   `1 <= target.length <= 15`
*   `stickers[i]` and `target` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    // count the characters of every sticker
    private lateinit var counts: Array<IntArray>

    // For each character, save the sticker index which has this character
    private val map: HashMap<Char, HashSet<Int>> = HashMap()
    private val cache: HashMap<Int, Int> = HashMap()
    fun minStickers(stickers: Array<String>, target: String): Int {
        counts = Array(stickers.size) { IntArray(26) }
        for (i in 0..25) {
            map[('a'.code + i).toChar()] = HashSet()
        }
        for (i in stickers.indices) {
            for (c in stickers[i].toCharArray()) {
                counts[i][c.code - 'a'.code]++
                map[c]!!.add(i)
            }
        }
        val res = dp(0, target)
        return if (res > target.length) {
            -1
        } else {
            res
        }
    }

    private fun dp(bits: Int, target: String): Int {
        val len = target.length
        if (bits == (1 shl len) - 1) {
            // all bits are 1
            return 0
        }
        if (cache.containsKey(bits)) {
            return cache[bits]!!
        }
        var index = 0
        // find the first bit which is 0
        for (i in 0 until len) {
            if (bits and (1 shl i) == 0) {
                index = i
                break
            }
        }
        // In worst case, each character use 1 sticker. So, len + 1 means impossible
        var res = len + 1
        for (key in map[target[index]]!!) {
            val count = counts[key].clone()
            var mask = bits
            for (i in index until len) {
                if (mask and (1 shl i) != 0) {
                    // this bit has already been 1
                    continue
                }
                val c = target[i]
                if (count[c.code - 'a'.code] > 0) {
                    count[c.code - 'a'.code]--
                    mask = mask or (1 shl i)
                }
            }
            val `val` = dp(mask, target) + 1
            res = res.coerceAtMost(`val`)
        }
        cache[bits] = res
        return res
    }
}
```