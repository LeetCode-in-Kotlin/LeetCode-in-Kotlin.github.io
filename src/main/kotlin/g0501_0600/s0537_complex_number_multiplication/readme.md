[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 537\. Complex Number Multiplication

Medium

A [complex number](https://en.wikipedia.org/wiki/Complex_number) can be represented as a string on the form <code>"**real**+**imaginary**i"</code> where:

*   `real` is the real part and is an integer in the range `[-100, 100]`.
*   `imaginary` is the imaginary part and is an integer in the range `[-100, 100]`.
*   <code>i<sup>2</sup> == -1</code>.

Given two complex numbers `num1` and `num2` as strings, return _a string of the complex number that represents their multiplications_.

**Example 1:**

**Input:** num1 = "1+1i", num2 = "1+1i"

**Output:** "0+2i"

**Explanation:** (1 + i) \* (1 + i) = 1 + i2 + 2 \* i = 2i, and you need convert it to the form of 0+2i.

**Example 2:**

**Input:** num1 = "1+-1i", num2 = "1+-1i"

**Output:** "0+-2i"

**Explanation:** (1 - i) \* (1 - i) = 1 + i2 - 2 \* i = -2i, and you need convert it to the form of 0+-2i.

**Constraints:**

*   `num1` and `num2` are valid complex numbers.

## Solution

```kotlin
class Solution {
    fun complexNumberMultiply(num1: String, num2: String): String {
        val countReal: Int
        val countImagine: Int
        val arr1 = IntArray(2)
        val arr2 = IntArray(2)
        arr1[0] = num1.substring(0, num1.indexOf("+")).toInt()
        arr1[1] = num1.substring(num1.indexOf("+") + 1, num1.length - 1).toInt()
        arr2[0] = num2.substring(0, num2.indexOf("+")).toInt()
        arr2[1] = num2.substring(num2.indexOf("+") + 1, num2.length - 1).toInt()
        countReal = arr1[0] * arr2[0] - arr1[1] * arr2[1]
        countImagine = arr1[0] * arr2[1] + arr1[1] * arr2[0]
        return countReal.toString() + "+" + countImagine + "i"
    }
}
```