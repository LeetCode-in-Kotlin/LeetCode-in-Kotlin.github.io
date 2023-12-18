[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2848\. Points That Intersect With Cars

Easy

You are given a **0-indexed** 2D integer array `nums` representing the coordinates of the cars parking on a number line. For any index `i`, <code>nums[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> where <code>start<sub>i</sub></code> is the starting point of the <code>i<sup>th</sup></code> car and <code>end<sub>i</sub></code> is the ending point of the <code>i<sup>th</sup></code> car.

Return _the number of integer points on the line that are covered with **any part** of a car._

**Example 1:**

**Input:** nums = \[\[3,6],[1,5],[4,7]]

**Output:** 7

**Explanation:** All the points from 1 to 7 intersect at least one car, therefore the answer would be 7.

**Example 2:**

**Input:** nums = \[\[1,3],[5,8]]

**Output:** 7

**Explanation:** Points intersecting at least one car are 1, 2, 3, 5, 6, 7, 8. There are a total of 7 points, therefore the answer would be 7.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `nums[i].length == 2`
*   <code>1 <= start<sub>i</sub> <= end<sub>i</sub> <= 100</code>

## Solution

```kotlin
class Solution {
    fun numberOfPoints(nums: List<List<Int>>): Int {
        var min = 101
        var max = 0
        val count = IntArray(102)
        for (list in nums) {
            val num1 = list[0]
            val num2 = list[1]
            if (num1 < min) {
                min = num1
            }
            if (num2 > max) {
                max = num2
            }
            count[num1]--
            count[num2 + 1]++
        }
        var result = 0
        var balance = 0
        while (min <= max) {
            balance += count[min]
            if (balance < 0) {
                result++
            }
            min++
        }
        return result
    }
}
```