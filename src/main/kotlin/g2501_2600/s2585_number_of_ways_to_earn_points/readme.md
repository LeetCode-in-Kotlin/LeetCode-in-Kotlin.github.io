[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2585\. Number of Ways to Earn Points

Hard

There is a test that has `n` types of questions. You are given an integer `target` and a **0-indexed** 2D integer array `types` where <code>types[i] = [count<sub>i</sub>, marks<sub>i</sub>]</code> indicates that there are <code>count<sub>i</sub></code> questions of the <code>i<sup>th</sup></code> type, and each one of them is worth <code>marks<sub>i</sub></code> points.

Return _the number of ways you can earn **exactly**_ `target` _points in the exam_. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note** that questions of the same type are indistinguishable.

*   For example, if there are `3` questions of the same type, then solving the <code>1<sup>st</sup></code> and <code>2<sup>nd</sup></code> questions is the same as solving the <code>1<sup>st</sup></code> and <code>3<sup>rd</sup></code> questions, or the <code>2<sup>nd</sup></code> and <code>3<sup>rd</sup></code> questions.

**Example 1:**

**Input:** target = 6, types = \[\[6,1],[3,2],[2,3]]

**Output:** 7

**Explanation:** You can earn 6 points in one of the seven ways: 

- Solve 6 questions of the 0<sup>th</sup> type: 1 + 1 + 1 + 1 + 1 + 1 = 6 
- Solve 4 questions of the 0<sup>th</sup> type and 1 question of the 1<sup>st</sup> type: 1 + 1 + 1 + 1 + 2 = 6 
- Solve 2 questions of the 0<sup>th</sup> type and 2 questions of the 1<sup>st</sup> type: 1 + 1 + 2 + 2 = 6 
- Solve 3 questions of the 0<sup>th</sup> type and 1 question of the 2<sup>nd</sup> type: 1 + 1 + 1 + 3 = 6 
- Solve 1 question of the 0<sup>th</sup> type, 1 question of the 1<sup>st</sup> type and 1 question of the 2<sup>nd</sup> type: 1 + 2 + 3 = 6 
- Solve 3 questions of the 1<sup>st</sup> type: 2 + 2 + 2 = 6 - Solve 2 questions of the 2<sup>nd</sup> type: 3 + 3 = 6

**Example 2:**

**Input:** target = 5, types = \[\[50,1],[50,2],[50,5]]

**Output:** 4

**Explanation:** You can earn 5 points in one of the four ways: 

- Solve 5 questions of the 0<sup>th</sup> type: 1 + 1 + 1 + 1 + 1 = 5
- Solve 3 questions of the 0<sup>th</sup> type and 1 question of the 1<sup>st</sup> type: 1 + 1 + 1 + 2 = 5 
- Solve 1 questions of the 0<sup>th</sup> type and 2 questions of the 1<sup>st</sup> type: 1 + 2 + 2 = 5 
- Solve 1 question of the 2<sup>nd</sup> type: 5

**Example 3:**

**Input:** target = 18, types = \[\[6,1],[3,2],[2,3]]

**Output:** 1

**Explanation:** You can only earn 18 points by answering all questions.

**Constraints:**

*   `1 <= target <= 1000`
*   `n == types.length`
*   `1 <= n <= 50`
*   `types[i].length == 2`
*   <code>1 <= count<sub>i</sub>, marks<sub>i</sub> <= 50</code>

## Solution

```kotlin
class Solution {
    fun waysToReachTarget(target: Int, types: Array<IntArray>): Int {
        val kMod = 1000000007
        val dp = Array(types.size + 1) { IntArray(target + 1) }
        dp[0][0] = 1
        for (i in 1..types.size) {
            val count = types[i - 1][0]
            val mark = types[i - 1][1]
            for (j in 0..target) for (solved in 0..count) if (j - solved * mark >= 0) {
                dp[i][j] += dp[i - 1][j - solved * mark]
                dp[i][j] %= kMod
            }
        }
        return dp[types.size][target]
    }
}
```