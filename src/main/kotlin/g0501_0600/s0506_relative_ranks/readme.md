[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 506\. Relative Ranks

Easy

You are given an integer array `score` of size `n`, where `score[i]` is the score of the <code>i<sup>th</sup></code> athlete in a competition. All the scores are guaranteed to be **unique**.

The athletes are **placed** based on their scores, where the <code>1<sup>st</sup></code> place athlete has the highest score, the <code>2<sup>nd</sup></code> place athlete has the <code>2<sup>nd</sup></code> highest score, and so on. The placement of each athlete determines their rank:

*   The <code>1<sup>st</sup></code> place athlete's rank is `"Gold Medal"`.
*   The <code>2<sup>nd</sup></code> place athlete's rank is `"Silver Medal"`.
*   The <code>3<sup>rd</sup></code> place athlete's rank is `"Bronze Medal"`.
*   For the <code>4<sup>th</sup></code> place to the <code>n<sup>th</sup></code> place athlete, their rank is their placement number (i.e., the <code>x<sup>th</sup></code> place athlete's rank is `"x"`).

Return an array `answer` of size `n` where `answer[i]` is the **rank** of the <code>i<sup>th</sup></code> athlete.

**Example 1:**

**Input:** score = [5,4,3,2,1]

**Output:** ["Gold Medal","Silver Medal","Bronze Medal","4","5"]

**Explanation:** The placements are [1<sup>st</sup>, 2<sup>nd</sup>, 3<sup>rd</sup>, 4<sup>th</sup>, 5<sup>th</sup>].

**Example 2:**

**Input:** score = [10,3,8,9,4]

**Output:** ["Gold Medal","5","Bronze Medal","Silver Medal","4"]

**Explanation:** The placements are [1<sup>st</sup>, 5<sup>th</sup>, 3<sup>rd</sup>, 2<sup>nd</sup>, 4<sup>th</sup>].

**Constraints:**

*   `n == score.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>0 <= score[i] <= 10<sup>6</sup></code>
*   All the values in `score` are **unique**.

## Solution

```kotlin
class Solution {
    fun findRelativeRanks(score: IntArray): Array<String> {
        fun mapPlace(score: Int, sortedScores: IntArray): String {
            val place = sortedScores.lastIndex - sortedScores.binarySearch(score)
            return when {
                place > 2 -> "${place + 1}"
                place == 0 -> "Gold Medal"
                place == 1 -> "Silver Medal"
                place == 2 -> "Bronze Medal"
                else -> throw NoWhenBranchMatchedException()
            }
        }

        val sortedScores = score.clone().apply { sort() }
        return score.map { mapPlace(it, sortedScores) }.toTypedArray()
    }
}
```