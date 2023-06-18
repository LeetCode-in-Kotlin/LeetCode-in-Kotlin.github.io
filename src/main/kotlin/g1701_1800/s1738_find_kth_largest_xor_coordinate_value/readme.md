[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1738\. Find Kth Largest XOR Coordinate Value

Medium

You are given a 2D `matrix` of size `m x n`, consisting of non-negative integers. You are also given an integer `k`.

The **value** of coordinate `(a, b)` of the matrix is the XOR of all `matrix[i][j]` where `0 <= i <= a < m` and `0 <= j <= b < n` **(0-indexed)**.

Find the <code>k<sup>th</sup></code> largest value **(1-indexed)** of all the coordinates of `matrix`.

**Example 1:**

**Input:** matrix = \[\[5,2],[1,6]], k = 1

**Output:** 7

**Explanation:** The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.

**Example 2:**

**Input:** matrix = \[\[5,2],[1,6]], k = 2

**Output:** 5

**Explanation:** The value of coordinate (0,0) is 5 = 5, which is the 2nd largest value.

**Example 3:**

**Input:** matrix = \[\[5,2],[1,6]], k = 3

**Output:** 4

**Explanation:** The value of coordinate (1,0) is 5 XOR 1 = 4, which is the 3rd largest value.

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 1000`
*   <code>0 <= matrix[i][j] <= 10<sup>6</sup></code>
*   `1 <= k <= m * n`

## Solution

```kotlin
class Solution {
    fun kthLargestValue(matrix: Array<IntArray>, k: Int): Int {
        var t = 0
        val rows: Int = matrix.size
        val cols: Int = matrix[0].size
        val prefixXor: Array<IntArray> = Array(rows + 1) { IntArray(cols + 1) }
        val array = IntArray(rows * cols)
        for (r in 1..rows) {
            for (c in 1..cols) {
                prefixXor[r][c] = (
                    matrix[r - 1][c - 1]
                        xor prefixXor[r - 1][c]
                        xor prefixXor[r][c - 1]
                        xor prefixXor[r - 1][c - 1]
                    )
                array[t++] = prefixXor[r][c]
            }
        }
        val target: Int = array.size - k
        quickSelect(array, 0, array.size - 1, target)
        return array[target]
    }

    private fun quickSelect(array: IntArray, left: Int, right: Int, target: Int): Int {
        if (left == right) {
            return left
        }
        val pivot: Int = array[right]
        var j: Int = left
        var k: Int = right - 1
        while (j <= k) {
            if (array[j] < pivot) {
                j++
            } else if (array[k] > pivot) {
                k--
            } else {
                swap(array, j++, k--)
            }
        }
        swap(array, j, right)
        return if (j == target) {
            j
        } else if (j > target) {
            quickSelect(array, left, j - 1, target)
        } else {
            quickSelect(array, j + 1, right, target)
        }
    }

    private fun swap(array: IntArray, i: Int, j: Int) {
        val tmp: Int = array[i]
        array[i] = array[j]
        array[j] = tmp
    }
}
```