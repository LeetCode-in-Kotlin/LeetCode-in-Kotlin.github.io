[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1691\. Maximum Height by Stacking Cuboids

Hard

Given `n` `cuboids` where the dimensions of the <code>i<sup>th</sup></code> cuboid is <code>cuboids[i] = [width<sub>i</sub>, length<sub>i</sub>, height<sub>i</sub>]</code> (**0-indexed**). Choose a **subset** of `cuboids` and place them on each other.

You can place cuboid `i` on cuboid `j` if <code>width<sub>i</sub> <= width<sub>j</sub></code> and <code>length<sub>i</sub> <= length<sub>j</sub></code> and <code>height<sub>i</sub> <= height<sub>j</sub></code>. You can rearrange any cuboid's dimensions by rotating it to put it on another cuboid.

Return _the **maximum height** of the stacked_ `cuboids`.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/10/21/image.jpg)**

**Input:** cuboids = \[\[50,45,20],[95,37,53],[45,23,12]]

**Output:** 190

**Explanation:**

Cuboid 1 is placed on the bottom with the 53x37 side facing down with height 95.

Cuboid 0 is placed next with the 45x20 side facing down with height 50.

Cuboid 2 is placed next with the 23x12 side facing down with height 45.

The total height is 95 + 50 + 45 = 190.

**Example 2:**

**Input:** cuboids = \[\[38,25,45],[76,35,3]]

**Output:** 76

**Explanation:**

You can't place any of the cuboids on the other.

We choose cuboid 1 and rotate it so that the 35x3 side is facing down and its height is 76.

**Example 3:**

**Input:** cuboids = \[\[7,11,17],[7,17,11],[11,7,17],[11,17,7],[17,7,11],[17,11,7]]

**Output:** 102

**Explanation:**

After rearranging the cuboids, you can see that all cuboids have the same dimension.

You can place the 11x7 side down on all cuboids so their heights are 17.

The maximum height of stacked cuboids is 6 \* 17 = 102.

**Constraints:**

*   `n == cuboids.length`
*   `1 <= n <= 100`
*   <code>1 <= width<sub>i</sub>, length<sub>i</sub>, height<sub>i</sub> <= 100</code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun maxHeight(cuboids: Array<IntArray>): Int {
        for (a in cuboids) {
            a.sort()
        }
        Arrays.sort(
            cuboids
        ) { a: IntArray, b: IntArray ->
            if (a[0] != b[0]) {
                return@sort a[0] - b[0]
            } else if (a[1] != b[1]) {
                return@sort a[1] - b[1]
            }
            a[2] - b[2]
        }
        var ans = 0
        val dp = IntArray(cuboids.size)
        for (i in cuboids.indices) {
            dp[i] = cuboids[i][2]
            for (j in 0 until i) {
                if (cuboids[i][0] >= cuboids[j][0] &&
                    cuboids[i][1] >= cuboids[j][1] && cuboids[i][2] >= cuboids[j][2]
                ) {
                    dp[i] = Math.max(dp[i], cuboids[i][2] + dp[j])
                }
            }
            ans = Math.max(ans, dp[i])
        }
        return ans
    }
}
```