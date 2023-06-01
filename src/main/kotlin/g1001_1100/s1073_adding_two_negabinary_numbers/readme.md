[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1073\. Adding Two Negabinary Numbers

Medium

Given two numbers `arr1` and `arr2` in base **\-2**, return the result of adding them together.

Each number is given in _array format_: as an array of 0s and 1s, from most significant bit to least significant bit. For example, `arr = [1,1,0,1]` represents the number `(-2)^3 + (-2)^2 + (-2)^0 = -3`. A number `arr` in _array, format_ is also guaranteed to have no leading zeros: either `arr == [0]` or `arr[0] == 1`.

Return the result of adding `arr1` and `arr2` in the same format: as an array of 0s and 1s with no leading zeros.

**Example 1:**

**Input:** arr1 = [1,1,1,1,1], arr2 = [1,0,1]

**Output:** [1,0,0,0,0]

**Explanation:** arr1 represents 11, arr2 represents 5, the output represents 16.

**Example 2:**

**Input:** arr1 = [0], arr2 = [0]

**Output:** [0]

**Example 3:**

**Input:** arr1 = [0], arr2 = [1]

**Output:** [1]

**Constraints:**

*   `1 <= arr1.length, arr2.length <= 1000`
*   `arr1[i]` and `arr2[i]` are `0` or `1`
*   `arr1` and `arr2` have no leading zeros

## Solution

```kotlin
class Solution {
    fun addNegabinary(arr1: IntArray, arr2: IntArray): IntArray {
        val len1 = arr1.size
        val len2 = arr2.size
        val reverseArr1 = IntArray(len1)
        for (i in len1 - 1 downTo 0) {
            reverseArr1[len1 - i - 1] = arr1[i]
        }
        val reverseArr2 = IntArray(len2)
        for (i in len2 - 1 downTo 0) {
            reverseArr2[len2 - i - 1] = arr2[i]
        }
        val sumArray = IntArray(len1.coerceAtLeast(len2) + 2)
        System.arraycopy(reverseArr1, 0, sumArray, 0, len1)
        for (i in sumArray.indices) {
            if (i < len2) {
                sumArray[i] += reverseArr2[i]
            }
            if (sumArray[i] > 1) {
                sumArray[i] -= 2
                sumArray[i + 1]--
            } else if (sumArray[i] == -1) {
                sumArray[i] = 1
                sumArray[i + 1]++
            }
        }
        var resultLen = sumArray.size
        for (i in sumArray.indices.reversed()) {
            if (sumArray[i] == 0) {
                resultLen--
            } else {
                break
            }
        }
        if (resultLen == 0) {
            return intArrayOf(0)
        }
        val result = IntArray(resultLen)
        for (i in resultLen - 1 downTo 0) {
            result[resultLen - i - 1] = sumArray[i]
        }
        return result
    }
}
```