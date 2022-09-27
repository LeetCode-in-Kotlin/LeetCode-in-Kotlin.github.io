[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 66\. Plus One

Easy

You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the <code>i<sup>th</sup></code> digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return _the resulting array of digits_.

**Example 1:**

**Input:** digits = [1,2,3]

**Output:** [1,2,4]

**Explanation:** 

The array represents the integer 123. 

Incrementing by one gives 123 + 1 = 124. 

Thus, the result should be [1,2,4].

**Example 2:**

**Input:** digits = [4,3,2,1]

**Output:** [4,3,2,2]

**Explanation:** 

The array represents the integer 4321. 

Incrementing by one gives 4321 + 1 = 4322. 

Thus, the result should be [4,3,2,2].

**Example 3:**

**Input:** digits = [9]

**Output:** [1,0]

**Explanation:** 

The array represents the integer 9. 

Incrementing by one gives 9 + 1 = 10. 

Thus, the result should be [1,0].

**Constraints:**

*   `1 <= digits.length <= 100`
*   `0 <= digits[i] <= 9`
*   `digits` does not contain any leading `0`'s.

## Solution

```kotlin
class Solution {
    fun plusOne(digits: IntArray): IntArray {
        val num = 1
        var carry = 0
        var sum: Int
        for (i in digits.indices.reversed()) {
            sum = if (i == digits.size - 1) {
                digits[i] + carry + num
            } else {
                digits[i] + carry
            }
            carry = sum / 10
            digits[i] = sum % 10
        }
        if (carry != 0) {
            val ans = IntArray(digits.size + 1)
            ans[0] = carry
            System.arraycopy(digits, 0, ans, 1, ans.size - 1)
            return ans
        }
        return digits
    }
}
```