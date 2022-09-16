[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 378\. Kth Smallest Element in a Sorted Matrix

Medium

Given an `n x n` `matrix` where each of the rows and columns is sorted in ascending order, return _the_ <code>k<sup>th</sup></code> _smallest element in the matrix_.

Note that it is the <code>k<sup>th</sup></code> smallest element **in the sorted order**, not the <code>k<sup>th</sup></code> **distinct** element.

You must find a solution with a memory complexity better than <code>O(n<sup>2</sup>)</code>.

**Example 1:**

**Input:** matrix = \[\[1,5,9],[10,11,13],[12,13,15]], k = 8

**Output:** 13

**Explanation:** The elements in the matrix are [1,5,9,10,11,12,13,<ins>**13**</ins>,15], and the 8<sup>th</sup> smallest number is 13

**Example 2:**

**Input:** matrix = \[\[-5]], k = 1

**Output:** -5

**Constraints:**

*   `n == matrix.length == matrix[i].length`
*   `1 <= n <= 300`
*   <code>-10<sup>9</sup> <= matrix[i][j] <= 10<sup>9</sup></code>
*   All the rows and columns of `matrix` are **guaranteed** to be sorted in **non-decreasing order**.
*   <code>1 <= k <= n<sup>2</sup></code>

**Follow up:**

*   Could you solve the problem with a constant memory (i.e., `O(1)` memory complexity)?
*   Could you solve the problem in `O(n)` time complexity? The solution may be too advanced for an interview but you may find reading [this paper](http://www.cse.yorku.ca/~andy/pubs/X+Y.pdf) fun.

## Solution

```kotlin
class Solution {
    fun kthSmallest(matrix: Array<IntArray>?, k: Int): Int {
        if (matrix == null || matrix.size == 0) {
            return -1
        }
        var start = matrix[0][0]
        var end = matrix[matrix.size - 1][matrix[0].size - 1]
        // O(log(max-min)) time
        while (start + 1 < end) {
            val mid = start + (end - start) / 2
            if (countLessEqual(matrix, mid) < k) {
                // look towards end
                start = mid
            } else {
                // look towards start
                end = mid
            }
        }

        // leave only with start and end, one of them must be the answer
        // try to see if start fits the criteria first
        return if (countLessEqual(matrix, start) >= k) {
            start
        } else {
            end
        }
    }

    // countLessEqual
    // O(n) Time
    private fun countLessEqual(matrix: Array<IntArray>, target: Int): Int {
        // binary elimination from top right
        var row = 0
        var col: Int = matrix[0].size - 1
        var count = 0
        while (row < matrix.size && col >= 0) {
            if (matrix[row][col] <= target) {
                // get the count in current row
                count += col + 1
                row++
            } else if (matrix[row][col] > target) {
                // eliminate the current col
                col--
            }
        }
        return count
    }
}
```