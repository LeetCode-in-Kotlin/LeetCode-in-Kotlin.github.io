[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2736\. Maximum Sum Queries

Hard

You are given two **0-indexed** integer arrays `nums1` and `nums2`, each of length `n`, and a **1-indexed 2D array** `queries` where <code>queries[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

For the <code>i<sup>th</sup></code> query, find the **maximum value** of `nums1[j] + nums2[j]` among all indices `j` `(0 <= j < n)`, where <code>nums1[j] >= x<sub>i</sub></code> and <code>nums2[j] >= y<sub>i</sub></code>, or **\-1** if there is no `j` satisfying the constraints.

Return _an array_ `answer` _where_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query._

**Example 1:**

**Input:** nums1 = [4,3,1,2], nums2 = [2,4,9,5], queries = \[\[4,1],[1,3],[2,5]]

**Output:** [6,10,7]

**Explanation:** 

For the 1st query <code>x<sub>i</sub> = 4</code> and <code>y<sub>i</sub> = 1</code>, we can select index `j = 0` since `nums1[j] >= 4` and `nums2[j] >= 1`. The sum `nums1[j] + nums2[j]` is 6, and we can show that 6 is the maximum we can obtain. 

For the 2nd query <code>x<sub>i</sub> = 1</code> and <code>y<sub>i</sub> = 3</code>, we can select index `j = 2` since `nums1[j] >= 1` and `nums2[j] >= 3`. The sum `nums1[j] + nums2[j]` is 10, and we can show that 10 is the maximum we can obtain. 

For the 3rd query <code>x<sub>i</sub> = 2</code> and <code>y<sub>i</sub> = 5</code>, we can select index `j = 3` since `nums1[j] >= 2` and `nums2[j] >= 5`. The sum `nums1[j] + nums2[j]` is 7, and we can show that 7 is the maximum we can obtain. 

Therefore, we return `[6,10,7]`.

**Example 2:**

**Input:** nums1 = [3,2,5], nums2 = [2,3,4], queries = \[\[4,4],[3,2],[1,1]]

**Output:** [9,9,9]

**Explanation:** For this example, we can use index `j = 2` for all the queries since it satisfies the constraints for each query.

**Example 3:**

**Input:** nums1 = [2,1], nums2 = [2,3], queries = \[\[3,3]]

**Output:** [-1]

**Explanation:** There is one query in this example with <code>x<sub>i</sub></code> = 3 and <code>y<sub>i</sub></code> = 3. For every index, j, either nums1[j] < <code>x<sub>i</sub></code> or nums2[j] < <code>y<sub>i</sub></code>. Hence, there is no solution.

**Constraints:**

*   `nums1.length == nums2.length`
*   `n == nums1.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>9</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>x<sub>i</sub> == queries[i][1]</code>
*   <code>y<sub>i</sub> == queries[i][2]</code>
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.TreeMap

class Solution {
    private fun update(map: TreeMap<Int, Int>, num: Int, sum: Int) {
        var entry = map.floorEntry(num)
        while (entry != null && entry.value <= sum) {
            map.remove(entry.key)
            val x = entry.key
            entry = map.floorEntry(x)
        }
        entry = map.ceilingEntry(num)
        if (entry == null || entry.value < sum) map.put(num, sum)
    }

    private fun queryVal(map: TreeMap<Int, Int>, num: Int): Int {
        val (_, value) = map.ceilingEntry(num) ?: return -1
        return value
    }

    fun maximumSumQueries(nums1: IntArray, nums2: IntArray, queries: Array<IntArray>): IntArray {
        val n = nums1.size
        val m = queries.size
        val v: MutableList<IntArray> = ArrayList()
        for (i in 0 until n) {
            v.add(intArrayOf(nums1[i], nums2[i]))
        }
        v.sortWith(
            Comparator { a: IntArray, b: IntArray ->
                a[0] - b[0]
            }
        )
        val ind: MutableList<Int> = ArrayList()
        for (i in 0 until m) ind.add(i)
        ind.sortWith(Comparator { a: Int?, b: Int? -> queries[b!!][0] - queries[a!!][0] })
        val values = TreeMap<Int, Int>()
        var j = n - 1
        val ans = IntArray(m)
        for (i in ind) {
            val a = queries[i][0]
            val b = queries[i][1]
            while (j >= 0 && v[j][0] >= a) {
                update(values, v[j][1], v[j][0] + v[j][1])
                j--
            }
            ans[i] = queryVal(values, b)
        }
        return ans
    }
}
```