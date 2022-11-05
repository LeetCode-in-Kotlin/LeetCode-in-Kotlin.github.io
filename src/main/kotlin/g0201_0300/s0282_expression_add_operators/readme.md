[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 282\. Expression Add Operators

Hard

Given a string `num` that contains only digits and an integer `target`, return _**all possibilities** to insert the binary operators_ `'+'`_,_ `'-'`_, and/or_ `'*'` _between the digits of_ `num` _so that the resultant expression evaluates to the_ `target` _value_.

Note that operands in the returned expressions **should not** contain leading zeros.

**Example 1:**

**Input:** num = "123", target = 6

**Output:** ["1\*2\*3","1+2+3"]

**Explanation:** Both "1\*2\*3" and "1+2+3" evaluate to 6.

**Example 2:**

**Input:** num = "232", target = 8

**Output:** ["2\*3+2","2+3\*2"]

**Explanation:** Both "2\*3+2" and "2+3\*2" evaluate to 8.

**Example 3:**

**Input:** num = "3456237490", target = 9191

**Output:** []

**Explanation:** There are no expressions that can be created from "3456237490" to evaluate to 9191.

**Constraints:**

*   `1 <= num.length <= 10`
*   `num` consists of only digits.
*   <code>-2<sup>31</sup> <= target <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING", "kotlin:S107")
class Solution {
    fun addOperators(num: String, target: Int): List<String> {
        val res: MutableList<String> = ArrayList()
        if (num.length == 0 || java.lang.Long.valueOf(num) > Int.MAX_VALUE) {
            return res
        }
        val list = num.toCharArray()
        dfs(res, list, 0, 0, target, CharArray(2 * list.size - 1), 0, 1, '+', 0)
        return res
    }

    private fun dfs(
        res: MutableList<String>,
        list: CharArray,
        i: Int,
        j: Int,
        target: Int,
        arr: CharArray,
        `val`: Int,
        mul: Int,
        preOp: Char,
        join: Int
    ) {
        var j = j
        arr[j++] = list[i]
        val digit = 10 * join + (list[i].code - '0'.code)
        var result = `val` + mul * digit
        if (preOp == '-') {
            result = `val` - mul * digit
        }
        if (i == list.size - 1) {
            if (result == target) {
                val sb = StringBuilder()
                for (k in 0 until j) {
                    sb.append(arr[k])
                }
                res.add(sb.toString())
            }
            return
        }
        if (preOp == '+') {
            arr[j] = '+'
            dfs(res, list, i + 1, j + 1, target, arr, `val` + mul * digit, 1, '+', 0)
            arr[j] = '-'
            dfs(res, list, i + 1, j + 1, target, arr, `val` + mul * digit, 1, '-', 0)
            arr[j] = '*'
            dfs(res, list, i + 1, j + 1, target, arr, `val`, mul * digit, '+', 0)
            if (digit != 0) {
                dfs(res, list, i + 1, j, target, arr, `val`, mul, '+', digit)
            }
        } else {
            arr[j] = '+'
            dfs(res, list, i + 1, j + 1, target, arr, `val` - mul * digit, 1, '+', 0)
            arr[j] = '-'
            dfs(res, list, i + 1, j + 1, target, arr, `val` - mul * digit, 1, '-', 0)
            arr[j] = '*'
            dfs(res, list, i + 1, j + 1, target, arr, `val`, mul * digit, '-', 0)
            if (digit != 0) {
                dfs(res, list, i + 1, j, target, arr, `val`, mul, '-', digit)
            }
        }
    }
}
```