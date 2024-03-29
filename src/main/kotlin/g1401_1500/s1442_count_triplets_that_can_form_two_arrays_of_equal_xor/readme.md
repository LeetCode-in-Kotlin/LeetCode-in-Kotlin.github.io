[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1442\. Count Triplets That Can Form Two Arrays of Equal XOR

Medium

Given an array of integers `arr`.

We want to select three indices `i`, `j` and `k` where `(0 <= i < j <= k < arr.length)`.

Let's define `a` and `b` as follows:

*   `a = arr[i] ^ arr[i + 1] ^ ... ^ arr[j - 1]`
*   `b = arr[j] ^ arr[j + 1] ^ ... ^ arr[k]`

Note that **^** denotes the **bitwise-xor** operation.

Return _the number of triplets_ (`i`, `j` and `k`) Where `a == b`.

**Example 1:**

**Input:** arr = [2,3,1,6,7]

**Output:** 4

**Explanation:** The triplets are (0,1,2), (0,2,2), (2,3,4) and (2,4,4)

**Example 2:**

**Input:** arr = [1,1,1,1,1]

**Output:** 10

**Constraints:**

*   `1 <= arr.length <= 300`
*   <code>1 <= arr[i] <= 10<sup>8</sup></code>

## Solution

```kotlin
class Solution {
    fun countTriplets(arr: IntArray): Int {
        var count = 0
        for (i in arr.indices) {
            var xor = arr[i]
            for (k in i + 1 until arr.size) {
                xor = xor xor arr[k]
                if (xor == 0) {
                    count += k - i
                }
            }
        }
        return count
    }
}
```