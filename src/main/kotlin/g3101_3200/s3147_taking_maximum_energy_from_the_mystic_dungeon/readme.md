[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3147\. Taking Maximum Energy From the Mystic Dungeon

Medium

In a mystic dungeon, `n` magicians are standing in a line. Each magician has an attribute that gives you energy. Some magicians can give you negative energy, which means taking energy from you.

You have been cursed in such a way that after absorbing energy from magician `i`, you will be instantly transported to magician `(i + k)`. This process will be repeated until you reach the magician where `(i + k)` does not exist.

In other words, you will choose a starting point and then teleport with `k` jumps until you reach the end of the magicians' sequence, **absorbing all the energy** during the journey.

You are given an array `energy` and an integer `k`. Return the **maximum** possible energy you can gain.

**Example 1:**

**Input:** energy = [5,2,-10,-5,1], k = 3

**Output:** 3

**Explanation:** We can gain a total energy of 3 by starting from magician 1 absorbing 2 + 1 = 3.

**Example 2:**

**Input:** energy = [-2,-3,-1], k = 2

**Output:** -1

**Explanation:** We can gain a total energy of -1 by starting from magician 2.

**Constraints:**

*   <code>1 <= energy.length <= 10<sup>5</sup></code>
*   `-1000 <= energy[i] <= 1000`
*   `1 <= k <= energy.length - 1`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumEnergy(energy: IntArray, k: Int): Int {
        var max = Int.MIN_VALUE
        val n = energy.size
        for (i in n - 1 downTo n - k) {
            var en = 0
            var j = i
            while (j >= 0) {
                en += energy[j]
                max = max(en, max)
                j -= k
            }
        }
        return max
    }
}
```