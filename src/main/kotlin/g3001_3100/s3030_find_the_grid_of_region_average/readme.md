[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3030\. Find the Grid of Region Average

Medium

You are given a **0-indexed** `m x n` grid `image` which represents a grayscale image, where `image[i][j]` represents a pixel with intensity in the range`[0..255]`. You are also given a **non-negative** integer `threshold`.

Two pixels `image[a][b]` and `image[c][d]` are said to be **adjacent** if `|a - c| + |b - d| == 1`.

A **region** is a `3 x 3` subgrid where the **absolute difference** in intensity between any two **adjacent** pixels is **less than or equal to** `threshold`.

All pixels in a **region** belong to that region, note that a pixel **can** belong to **multiple** regions.

You need to calculate a **0-indexed** `m x n` grid `result`, where `result[i][j]` is the **average** intensity of the region to which `image[i][j]` belongs, **rounded down** to the nearest integer. If `image[i][j]` belongs to multiple regions, `result[i][j]` is the **average** of the **rounded down average** intensities of these regions, **rounded down** to the nearest integer. If `image[i][j]` does **not** belong to any region, `result[i][j]` is **equal to** `image[i][j]`.

Return _the grid_ `result`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/12/21/example0corrected.png)

**Input:** image = \[\[5,6,7,10],[8,9,10,10],[11,12,13,10]], threshold = 3

**Output:** [[9,9,9,9],[9,9,9,9],[9,9,9,9]]

**Explanation:** There exist two regions in the image, which are shown as the shaded areas in the picture. The average intensity of the first region is 9, while the average intensity of the second region is 9.67 which is rounded down to 9. The average intensity of both of the regions is (9 + 9) / 2 = 9. As all the pixels belong to either region 1, region 2, or both of them, the intensity of every pixel in the result is 9. Please note that the rounded-down values are used when calculating the average of multiple regions, hence the calculation is done using 9 as the average intensity of region 2, not 9.67.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/12/21/example1corrected.png)

**Input:** image = \[\[10,20,30],[15,25,35],[20,30,40],[25,35,45]], threshold = 12

**Output:** [[25,25,25],[27,27,27],[27,27,27],[30,30,30]]

**Explanation:** There exist two regions in the image, which are shown as the shaded areas in the picture. The average intensity of the first region is 25, while the average intensity of the second region is 30. The average intensity of both of the regions is (25 + 30) / 2 = 27.5 which is rounded down to 27. All the pixels in row 0 of the image belong to region 1, hence all the pixels in row 0 in the result are 25. Similarly, all the pixels in row 3 in the result are 30. The pixels in rows 1 and 2 of the image belong to region 1 and region 2, hence their assigned value is 27 in the result.

**Example 3:**

**Input:** image = \[\[5,6,7],[8,9,10],[11,12,13]], threshold = 1

**Output:** [[5,6,7],[8,9,10],[11,12,13]]

**Explanation:** There does not exist any region in image, hence result[i][j] == image[i][j] for all the pixels.

**Constraints:**

*   `3 <= n, m <= 500`
*   `0 <= image[i][j] <= 255`
*   `0 <= threshold <= 255`

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun resultGrid(image: Array<IntArray>, threshold: Int): Array<IntArray> {
        val n = image.size
        val m = image[0].size
        val intensity = Array(n) { IntArray(m) }
        val count = Array(n) { IntArray(m) }
        for (i in 1 until n - 1) {
            for (j in 1 until m - 1) {
                var regionPossible = true
                var regionSum = 0
                val r0c0 = image[i - 1][j - 1]
                val r0c1 = image[i - 1][j]
                val r0c2 = image[i - 1][j + 1]
                val r1c0 = image[i][j - 1]
                val r1c1 = image[i][j]
                val r1c2 = image[i][j + 1]
                val r2c0 = image[i + 1][j - 1]
                val r2c1 = image[i + 1][j]
                val r2c2 = image[i + 1][j + 1]
                regionSum += (r0c0 + r0c1 + r0c2 + r1c0 + r1c1 + r1c2 + r2c0 + r2c1 + r2c2)
                if (abs((r0c0 - r0c1)) > threshold || abs((r0c0 - r1c0)) > threshold || abs(
                        (r0c1 - r0c0),
                    ) > threshold || abs((r0c1 - r1c1)) > threshold || abs((r0c1 - r0c2)) > threshold || abs(
                        (r0c2 - r0c1),
                    ) > threshold || abs((r0c2 - r1c2)) > threshold || abs((r1c0 - r1c1)) > threshold || abs(
                        (r1c2 - r1c1),
                    ) > threshold || abs((r2c0 - r2c1)) > threshold || abs((r2c0 - r1c0)) > threshold || abs(
                        (r2c1 - r2c0),
                    ) > threshold || abs((r2c1 - r1c1)) > threshold || abs((r2c1 - r2c2)) > threshold || abs(
                        (r2c2 - r2c1),
                    ) > threshold || abs((r2c2 - r1c2)) > threshold
                ) {
                    regionPossible = false
                }
                if (regionPossible) {
                    regionSum /= 9
                    for (k in -1..1) {
                        for (l in -1..1) {
                            intensity[i + k][j + l] += regionSum
                            count[i + k][j + l]++
                        }
                    }
                }
            }
        }
        for (i in 0 until n) {
            for (j in 0 until m) {
                if (count[i][j] == 0) {
                    intensity[i][j] = image[i][j]
                } else {
                    intensity[i][j] = intensity[i][j] / count[i][j]
                }
            }
        }
        return intensity
    }
}
```