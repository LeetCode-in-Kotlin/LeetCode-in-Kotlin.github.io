[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3259\. Maximum Energy Boost From Two Drinks

Medium

You are given two integer arrays `energyDrinkA` and `energyDrinkB` of the same length `n` by a futuristic sports scientist. These arrays represent the energy boosts per hour provided by two different energy drinks, A and B, respectively.

You want to _maximize_ your total energy boost by drinking one energy drink _per hour_. However, if you want to switch from consuming one energy drink to the other, you need to wait for _one hour_ to cleanse your system (meaning you won't get any energy boost in that hour).

Return the **maximum** total energy boost you can gain in the next `n` hours.

**Note** that you can start consuming _either_ of the two energy drinks.

**Example 1:**

**Input:** energyDrinkA = [1,3,1], energyDrinkB = [3,1,1]

**Output:** 5

**Explanation:**

To gain an energy boost of 5, drink only the energy drink A (or only B).

**Example 2:**

**Input:** energyDrinkA = [4,1,1], energyDrinkB = [1,1,3]

**Output:** 7

**Explanation:**

To gain an energy boost of 7:

*   Drink the energy drink A for the first hour.
*   Switch to the energy drink B and we lose the energy boost of the second hour.
*   Gain the energy boost of the drink B in the third hour.

**Constraints:**

*   `n == energyDrinkA.length == energyDrinkB.length`
*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>1 <= energyDrinkA[i], energyDrinkB[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxEnergyBoost(energyDrinkA: IntArray, energyDrinkB: IntArray): Long {
        var a0: Long = 0
        var a1: Long = 0
        var b0: Long = 0
        var b1: Long = 0
        val n = energyDrinkA.size
        for (i in 0 until n) {
            a1 = max((a0 + energyDrinkA[i]), b0)
            b1 = max((b0 + energyDrinkB[i]), a0)
            a0 = a1
            b0 = b1
        }
        return max(a1, b1)
    }
}
```