[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 989\. Add to Array-Form of Integer

Easy

The **array-form** of an integer `num` is an array representing its digits in left to right order.

*   For example, for `num = 1321`, the array form is `[1,3,2,1]`.

Given `num`, the **array-form** of an integer, and an integer `k`, return _the **array-form** of the integer_ `num + k`.

**Example 1:**

**Input:** num = [1,2,0,0], k = 34

**Output:** [1,2,3,4]

**Explanation:** 1200 + 34 = 1234

**Example 2:**

**Input:** num = [2,7,4], k = 181

**Output:** [4,5,5]

**Explanation:** 274 + 181 = 455

**Example 3:**

**Input:** num = [2,1,5], k = 806

**Output:** [1,0,2,1]

**Explanation:** 215 + 806 = 1021

**Constraints:**

*   <code>1 <= num.length <= 10<sup>4</sup></code>
*   `0 <= num[i] <= 9`
*   `num` does not contain any leading zeros except for the zero itself.
*   <code>1 <= k <= 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun addToArrayForm(num: IntArray, k: Int): List<Int> {
        var k = k
        val result = ArrayList<Int>()
        var carry = 0
        for (i in num.indices.reversed()) {
            val temp = num[i] + k % 10 + carry
            result.add(temp % 10)
            carry = temp / 10
            k /= 10
        }
        while (k > 0) {
            val t = k % 10 + carry
            result.add(t % 10)
            carry = t / 10
            k /= 10
        }
        if (carry == 1) {
            result.add(1)
        }
        result.reverse()
        return result
    }
}
```