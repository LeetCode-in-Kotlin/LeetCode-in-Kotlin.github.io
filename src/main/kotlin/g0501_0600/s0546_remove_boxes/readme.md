[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 546\. Remove Boxes

Hard

You are given several `boxes` with different colors represented by different positive numbers.

You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (i.e., composed of `k` boxes, `k >= 1`), remove them and get `k * k` points.

Return _the maximum points you can get_.

**Example 1:**

**Input:** boxes = [1,3,2,2,2,3,4,3,1]

**Output:** 23

**Explanation:** [1, 3, 2, 2, 2, 3, 4, 3, 1] ----> [1, 3, 3, 4, 3, 1] (3\*3=9 points) ----> [1, 3, 3, 3, 1] (1\*1=1 points) ----> [1, 1] (3\*3=9 points) ----> [] (2\*2=4 points)

**Example 2:**

**Input:** boxes = [1,1,1]

**Output:** 9

**Example 3:**

**Input:** boxes = [1]

**Output:** 1

**Constraints:**

*   `1 <= boxes.length <= 100`
*   `1 <= boxes[i] <= 100`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    // dp for memoization
    lateinit var dp: Array<Array<IntArray>>
    fun removeBoxes(boxes: IntArray): Int {
        val n = boxes.size
        dp = Array(n + 1) {
            Array(n + 1) {
                IntArray(
                    n + 1,
                )
            }
        }
        return get(boxes, 0, boxes.size - 1, 0)
    }

    operator fun get(boxes: IntArray, i: Int, j: Int, streak: Int): Int {
        var i = i
        var streak = streak
        if (i > j) {
            return 0
        }

        // first we traverse till the adjacent values are different
        while (i + 1 <= j && boxes[i] == boxes[i + 1]) {
            i++
            streak++
        }
        // memoization
        if (dp[i][j][streak] > 0) {
            return dp[i][j][streak]
        }
        // we calculate the ans here which is streak (length of similar elements) and move
        // forward to the remaining block through recursion
        var ans = (streak + 1) * (streak + 1) + get(boxes, i + 1, j, 0)
        // also another way we can choose is to choose the inner elements first then the outer similar elements can be combined to get even
        // larger value
        for (k in i + 1..j) {
            // we traverse from k (i has moved from 0 to just before the beginning of different elements) and keep searching for same value as
            // in i. after that the middle elements (between i+1 and k-1) are sent to differnt partition and from k to j(ending) we send the updated streak
            if (boxes[i] == boxes[k]) {
                ans = Math.max(ans, get(boxes, i + 1, k - 1, 0) + get(boxes, k, j, streak + 1))
            }
        }
        // return ans here
        return ans.also { dp[i][j][streak] = it }
    }
}
```