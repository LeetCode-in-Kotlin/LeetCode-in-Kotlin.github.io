[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 832\. Flipping an Image

Easy

Given an `n x n` binary matrix `image`, flip the image **horizontally**, then invert it, and return _the resulting image_.

To flip an image horizontally means that each row of the image is reversed.

*   For example, flipping `[1,1,0]` horizontally results in `[0,1,1]`.

To invert an image means that each `0` is replaced by `1`, and each `1` is replaced by `0`.

*   For example, inverting `[0,1,1]` results in `[1,0,0]`.

**Example 1:**

**Input:** image = \[\[1,1,0],[1,0,1],[0,0,0]]

**Output:** [[1,0,0],[0,1,0],[1,1,1]]

**Explanation:** First reverse each row: [[0,1,1],[1,0,1],[0,0,0]]. Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]

**Example 2:**

**Input:** image = \[\[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]

**Output:** [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]

**Explanation:** First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]]. Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]

**Constraints:**

*   `n == image.length`
*   `n == image[i].length`
*   `1 <= n <= 20`
*   `images[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun flipAndInvertImage(image: Array<IntArray>): Array<IntArray> {
        val m = image.size
        val n = image[0].size
        val result = Array(m) { IntArray(n) }
        for (i in 0 until m) {
            val flipped = reverse(image[i])
            result[i] = invert(flipped)
        }
        return result
    }

    private fun invert(flipped: IntArray): IntArray {
        val result = IntArray(flipped.size)
        for (i in flipped.indices) {
            if (flipped[i] == 0) {
                result[i] = 1
            } else {
                result[i] = 0
            }
        }
        return result
    }

    private fun reverse(nums: IntArray): IntArray {
        var i = 0
        var j = nums.size - 1
        while (i < j) {
            val tmp = nums[i]
            nums[i] = nums[j]
            nums[j] = tmp
            i++
            j--
        }
        return nums
    }
}
```