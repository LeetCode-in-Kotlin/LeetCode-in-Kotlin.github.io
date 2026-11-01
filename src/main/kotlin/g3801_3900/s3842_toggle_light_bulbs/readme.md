[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3842\. Toggle Light Bulbs

Easy

You are given an array `bulbs` of integers between 1 and 100.

There are 100 light bulbs numbered from 1 to 100. All of them are switched off initially.

For each element `bulbs[i]` in the array `bulbs`:

*   If the <code>bulbs[i]<sup>th</sup></code> light bulb is currently off, switch it on.
*   Otherwise, switch it off.

Return the list of integers denoting the light bulbs that are on in the end, **sorted** in **ascending** order. If no bulb is on, return an empty list.

**Example 1:**

**Input:** bulbs = [10,30,20,10]

**Output:** [20,30]

**Explanation:**

*   The <code>bulbs[0] = 10<sup>th</sup></code> light bulb is currently off. We switch it on.
*   The <code>bulbs[1] = 30<sup>th</sup></code> light bulb is currently off. We switch it on.
*   The <code>bulbs[2] = 20<sup>th</sup></code> light bulb is currently off. We switch it on.
*   The <code>bulbs[3] = 10<sup>th</sup></code> light bulb is currently on. We switch it off.
*   In the end, the 20<sup>th</sup> and the 30<sup>th</sup> light bulbs are on.

**Example 2:**

**Input:** bulbs = [100,100]

**Output:** []

**Explanation:**

*   The <code>bulbs[0] = 100<sup>th</sup></code> light bulb is currently off. We switch it on.
*   The <code>bulbs[1] = 100<sup>th</sup></code> light bulb is currently on. We switch it off.
*   In the end, no light bulb is on.

**Constraints:**

*   `1 <= bulbs.length <= 100`
*   `1 <= bulbs[i] <= 100`

## Solution

```kotlin
class Solution {
    fun toggleLightBulbs(bulbs: MutableList<Int>): MutableList<Int> {
        val on = BooleanArray(101)
        for (b in bulbs) {
            on[b] = !on[b]
        }
        val ans: MutableList<Int> = ArrayList<Int>()
        for (i in 1..100) {
            if (on[i]) {
                ans.add(i)
            }
        }
        return ans
    }
}
```