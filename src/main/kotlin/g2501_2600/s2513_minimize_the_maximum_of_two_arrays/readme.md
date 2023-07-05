[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2513\. Minimize the Maximum of Two Arrays

Medium

We have two arrays `arr1` and `arr2` which are initially empty. You need to add positive integers to them such that they satisfy all the following conditions:

*   `arr1` contains `uniqueCnt1` **distinct** positive integers, each of which is **not divisible** by `divisor1`.
*   `arr2` contains `uniqueCnt2` **distinct** positive integers, each of which is **not divisible** by `divisor2`.
*   **No** integer is present in both `arr1` and `arr2`.

Given `divisor1`, `divisor2`, `uniqueCnt1`, and `uniqueCnt2`, return _the **minimum possible maximum** integer that can be present in either array_.

**Example 1:**

**Input:** divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3

**Output:** 4

**Explanation:**

We can distribute the first 4 natural numbers into arr1 and arr2. 

arr1 = [1] and arr2 = [2,3,4]. 

We can see that both arrays satisfy all the conditions. 

Since the maximum value is 4, we return it.

**Example 2:**

**Input:** divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1

**Output:** 3

**Explanation:** 

Here arr1 = [1,2], and arr2 = [3] satisfy all conditions. 

Since the maximum value is 3, we return it.

**Example 3:**

**Input:** divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2

**Output:** 15

**Explanation:** 

Here, the final possible arrays can be arr1 = [1,3,5,7,9,11,13,15], and arr2 = [2,6]. 

It can be shown that it is not possible to obtain a lower maximum satisfying all conditions.

**Constraints:**

*   <code>2 <= divisor1, divisor2 <= 10<sup>5</sup></code>
*   <code>1 <= uniqueCnt1, uniqueCnt2 < 10<sup>9</sup></code>
*   <code>2 <= uniqueCnt1 + uniqueCnt2 <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) {
            a
        } else gcd(b, a % b)
    }

    fun minimizeSet(divisor1: Int, divisor2: Int, uniqueCnt1: Int, uniqueCnt2: Int): Int {
        var divisor2 = divisor2
        var lo: Long = 1
        var hi = 10e10.toInt().toLong()
        if (divisor2 == 0) {
            divisor2 = 1
        }
        val lcm = divisor1.toLong() * divisor2.toLong() / gcd(divisor1, divisor2)
        while (lo < hi) {
            val mi = lo + (hi - lo) / 2
            val cnt1 = (mi - mi / divisor1).toInt()
            val cnt2 = (mi - mi / divisor2).toInt()
            val cntAll = (mi - mi / lcm).toInt()
            if (cnt1 < uniqueCnt1 || cnt2 < uniqueCnt2 || cntAll < uniqueCnt1 + uniqueCnt2) {
                lo = mi + 1
            } else {
                hi = mi
            }
        }
        return lo.toInt()
    }
}
```