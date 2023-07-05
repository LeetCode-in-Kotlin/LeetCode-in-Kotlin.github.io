[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2512\. Reward Top K Students

Medium

You are given two string arrays `positive_feedback` and `negative_feedback`, containing the words denoting positive and negative feedback, respectively. Note that **no** word is both positive and negative.

Initially every student has `0` points. Each positive word in a feedback report **increases** the points of a student by `3`, whereas each negative word **decreases** the points by `1`.

You are given `n` feedback reports, represented by a **0-indexed** string array `report` and a **0-indexed** integer array `student_id`, where `student_id[i]` represents the ID of the student who has received the feedback report `report[i]`. The ID of each student is **unique**.

Given an integer `k`, return _the top_ `k` _students after ranking them in **non-increasing** order by their points_. In case more than one student has the same points, the one with the lower ID ranks higher.

**Example 1:**

**Input:** positive\_feedback = ["smart","brilliant","studious"], negative\_feedback = ["not"], report = ["this student is studious","the student is smart"], student\_id = [1,2], k = 2

**Output:** [1,2]

**Explanation:** Both the students have 1 positive feedback and 3 points but since student 1 has a lower ID he ranks higher.

**Example 2:**

**Input:** positive\_feedback = ["smart","brilliant","studious"], negative\_feedback = ["not"], report = ["this student is not studious","the student is smart"], student\_id = [1,2], k = 2

**Output:** [2,1]

**Explanation:** 

- The student with ID 1 has 1 positive feedback and 1 negative feedback, so he has 3-1=2 points. 

- The student with ID 2 has 1 positive feedback, so he has 3 points. Since student 2 has more points, [2,1] is returned.

**Constraints:**

*   <code>1 <= positive_feedback.length, negative_feedback.length <= 10<sup>4</sup></code>
*   `1 <= positive_feedback[i].length, negative_feedback[j].length <= 100`
*   Both `positive_feedback[i]` and `negative_feedback[j]` consists of lowercase English letters.
*   No word is present in both `positive_feedback` and `negative_feedback`.
*   `n == report.length == student_id.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   `report[i]` consists of lowercase English letters and spaces `' '`.
*   There is a single space between consecutive words of `report[i]`.
*   `1 <= report[i].length <= 100`
*   <code>1 <= student_id[i] <= 10<sup>9</sup></code>
*   All the values of `student_id[i]` are **unique**.
*   `1 <= k <= n`

## Solution

```kotlin
import java.util.PriorityQueue

@Suppress("NAME_SHADOWING")
class Solution {
    fun topStudents(
        positiveFeedback: Array<String>,
        negativeFeedback: Array<String>,
        report: Array<String>,
        studentId: IntArray,
        k: Int
    ): List<Int> {
        var k = k
        val feedback = HashMap<String, Int>()
        for (s in positiveFeedback) {
            feedback[s] = 3
        }
        for (s in negativeFeedback) {
            feedback[s] = -1
        }
        val pq = PriorityQueue { a: Student, b: Student ->
            val result = Integer.compare(a.points, b.points)
            if (result == 0) Integer.compare(a.id, b.id) else -result
        }
        for (i in report.indices) {
            val split = report[i].split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
            var sum = 0
            for (subStr in split) {
                sum += feedback.getOrDefault(subStr, 0)
            }
            pq.offer(Student(studentId[i], sum))
        }
        val result: MutableList<Int> = ArrayList()
        while (pq.isNotEmpty() && k-- > 0) {
            result.add(pq.poll().id)
        }
        return result
    }

    private class Student(var id: Int, var points: Int)
}
```