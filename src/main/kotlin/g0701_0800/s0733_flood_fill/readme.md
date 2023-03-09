[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 733\. Flood Fill

Easy

An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `color`. You should perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.

To perform a **flood fill**, consider the starting pixel, plus any pixels connected **4-directionally** to the starting pixel of the same color as the starting pixel, plus any pixels connected **4-directionally** to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `color`.

Return _the modified image after performing the flood fill_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)

**Input:** image = \[\[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2

**Output:** [[2,2,2],[2,2,0],[2,0,1]]

**Explanation:** From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color. Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.

**Example 2:**

**Input:** image = \[\[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0

**Output:** [[0,0,0],[0,0,0]]

**Explanation:** The starting pixel is already colored 0, so no changes are made to the image.

**Constraints:**

*   `m == image.length`
*   `n == image[i].length`
*   `1 <= m, n <= 50`
*   <code>0 <= image[i][j], color < 2<sup>16</sup></code>
*   `0 <= sr < m`
*   `0 <= sc < n`

## Solution

```kotlin
class Solution {
    fun floodFill(image: Array<IntArray>, sr: Int, sc: Int, newColor: Int): Array<IntArray> {
        val o = image[sr][sc]
        helper(image, sr, sc, newColor, o)
        return image
    }

    private fun helper(img: Array<IntArray>, r: Int, c: Int, n: Int, o: Int) {
        if (r >= img.size || c >= img[0].size || r < 0 || c < 0 || img[r][c] == n || img[r][c] != o) {
            return
        }
        img[r][c] = n
        helper(img, r + 1, c, n, o)
        helper(img, r - 1, c, n, o)
        helper(img, r, c + 1, n, o)
        helper(img, r, c - 1, n, o)
    }
}
```