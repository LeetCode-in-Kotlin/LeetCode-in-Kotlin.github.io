[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2094\. Finding 3-Digit Even Numbers

Easy

You are given an integer array `digits`, where each element is a digit. The array may contain duplicates.

You need to find **all** the **unique** integers that follow the given requirements:

*   The integer consists of the **concatenation** of **three** elements from `digits` in **any** arbitrary order.
*   The integer does not have **leading zeros**.
*   The integer is **even**.

For example, if the given `digits` were `[1, 2, 3]`, integers `132` and `312` follow the requirements.

Return _a **sorted** array of the unique integers._

**Example 1:**

**Input:** digits = [2,1,3,0]

**Output:** [102,120,130,132,210,230,302,310,312,320]

**Explanation:** All the possible integers that follow the requirements are in the output array.

Notice that there are no **odd** integers or integers with **leading zeros**. 

**Example 2:**

**Input:** digits = [2,2,8,8,2]

**Output:** [222,228,282,288,822,828,882]

**Explanation:** The same digit can be used as many times as it appears in digits.

In this example, the digit 8 is used twice each time in 288, 828, and 882. 

**Example 3:**

**Input:** digits = [3,7,5]

**Output:** []

**Explanation:** No **even** integers can be formed using the given digits. 

**Constraints:**

*   `3 <= digits.length <= 100`
*   `0 <= digits[i] <= 9`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun findEvenNumbers(digits: IntArray): IntArray {
        val idx = IntArray(1)
        val result = IntArray(9 * 10 * 5)
        val digitMap = IntArray(10)
        for (digit in digits) {
            digitMap[digit]++
        }
        dfs(result, digitMap, idx, 0)
        return result.copyOfRange(0, idx[0])
    }

    private fun dfs(result: IntArray, digitMap: IntArray, idx: IntArray, `val`: Int) {
        var `val` = `val`
        if (`val` > 99) {
            result[idx[0]++] = `val`
            return
        }
        `val` *= 10
        for (i in 0..9) {
            if (digitMap[i] == 0 || `val` == 0 && i == 0 || `val` > 99 && i and 1 == 1) {
                continue
            }
            digitMap[i]--
            `val` += i
            dfs(result, digitMap, idx, `val`)
            `val` -= i
            digitMap[i]++
        }
    }
}
```