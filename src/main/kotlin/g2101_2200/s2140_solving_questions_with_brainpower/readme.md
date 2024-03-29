[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2140\. Solving Questions With Brainpower

Medium

You are given a **0-indexed** 2D integer array `questions` where <code>questions[i] = [points<sub>i</sub>, brainpower<sub>i</sub>]</code>.

The array describes the questions of an exam, where you have to process the questions **in order** (i.e., starting from question `0`) and make a decision whether to **solve** or **skip** each question. Solving question `i` will **earn** you <code>points<sub>i</sub></code> points but you will be **unable** to solve each of the next <code>brainpower<sub>i</sub></code> questions. If you skip question `i`, you get to make the decision on the next question.

*   For example, given `questions = \[\[3, 2], [4, 3], [4, 4], [2, 5]]`:
    *   If question `0` is solved, you will earn `3` points but you will be unable to solve questions `1` and `2`.
    *   If instead, question `0` is skipped and question `1` is solved, you will earn `4` points but you will be unable to solve questions `2` and `3`.

Return _the **maximum** points you can earn for the exam_.

**Example 1:**

**Input:** questions = \[\[3,2],[4,3],[4,4],[2,5]]

**Output:** 5

**Explanation:** The maximum points can be earned by solving questions 0 and 3. 

- Solve question 0: Earn 3 points, will be unable to solve the next 2 questions 

- Unable to solve questions 1 and 2 

- Solve question 3: Earn 2 points 
  
Total points earned: 3 + 2 = 5. There is no other way to earn 5 or more points.

**Example 2:**

**Input:** questions = \[\[1,1],[2,2],[3,3],[4,4],[5,5]]

**Output:** 7

**Explanation:** The maximum points can be earned by solving questions 1 and 4. 

- Skip question 0 

- Solve question 1: Earn 2 points, will be unable to solve the next 2 questions 

- Unable to solve questions 2 and 3 

- Solve question 4: Earn 5 points 
  
Total points earned: 2 + 5 = 7. There is no other way to earn 7 or more points.

**Constraints:**

*   <code>1 <= questions.length <= 10<sup>5</sup></code>
*   `questions[i].length == 2`
*   <code>1 <= points<sub>i</sub>, brainpower<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun mostPoints(questions: Array<IntArray>): Long {
        val n = questions.size
        val memo = LongArray(n)
        memo[n - 1] = questions[n - 1][0].toLong()
        for (i in n - 2 downTo 0) {
            if (i + questions[i][1] + 1 < n) {
                memo[i] = Math.max(memo[i + 1], questions[i][0] + memo[i + questions[i][1] + 1])
            } else {
                memo[i] = Math.max(memo[i + 1], questions[i][0].toLong())
            }
        }
        return memo[0]
    }
}
```