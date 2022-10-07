[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 43\. Multiply Strings

Medium

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Note:** You must not use any built-in BigInteger library or convert the inputs to integer directly.

**Example 1:**

**Input:** num1 = "2", num2 = "3"

**Output:** "6"

**Example 2:**

**Input:** num1 = "123", num2 = "456"

**Output:** "56088"

**Constraints:**

*   `1 <= num1.length, num2.length <= 200`
*   `num1` and `num2` consist of digits only.
*   Both `num1` and `num2` do not contain any leading zero, except the number `0` itself.

## Solution

```kotlin
class Solution {
    private fun getIntArray(s: String): IntArray {
        val chars = s.toCharArray()
        val arr = IntArray(chars.size)
        for (i in chars.indices) {
            arr[i] = chars[i] - '0'
        }
        return arr
    }

    private fun convertToStr(res: IntArray, i: Int): String {
        var index = i
        val chars = StringBuffer()
        while (index < res.size) {
            chars.append(res[index].toString())
            index++
        }
        return chars.toString()
    }

    fun multiply(num1: String, num2: String): String {
        val arr1 = getIntArray(num1)
        val arr2 = getIntArray(num2)
        val res = IntArray(arr1.size + arr2.size)
        var index = arr1.size + arr2.size - 1
        for (i in arr2.indices.reversed()) {
            var k = index--
            for (j in arr1.indices.reversed()) {
                res[k] += arr2[i] * arr1[j]
                k--
            }
        }
        index = arr1.size + arr2.size - 1
        var carry = 0
        for (i in index downTo 0) {
            val temp = res[i] + carry
            res[i] = temp % 10
            carry = temp / 10
        }
        var i = 0
        while (i < res.size && res[i] == 0) {
            i++
        }
        return if (i > index) {
            "0"
        } else {
            convertToStr(res, i)
        }
    }
}
```