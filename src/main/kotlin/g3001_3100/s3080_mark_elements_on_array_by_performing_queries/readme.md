[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3080\. Mark Elements on Array by Performing Queries

Medium

You are given a **0-indexed** array `nums` of size `n` consisting of positive integers.

You are also given a 2D array `queries` of size `m` where <code>queries[i] = [index<sub>i</sub>, k<sub>i</sub>]</code>.

Initially all elements of the array are **unmarked**.

You need to apply `m` queries on the array in order, where on the <code>i<sup>th</sup></code> query you do the following:

*   Mark the element at index <code>index<sub>i</sub></code> if it is not already marked.
*   Then mark <code>k<sub>i</sub></code> unmarked elements in the array with the **smallest** values. If multiple such elements exist, mark the ones with the smallest indices. And if less than <code>k<sub>i</sub></code> unmarked elements exist, then mark all of them.

Return _an array answer of size_ `m` _where_ `answer[i]` _is the **sum** of unmarked elements in the array after the_ <code>i<sup>th</sup></code> _query_.

**Example 1:**

**Input:** nums = [1,2,2,1,2,3,1], queries = \[\[1,2],[3,3],[4,2]]

**Output:** [8,3,0]

**Explanation:**

We do the following queries on the array:

*   Mark the element at index `1`, and `2` of the smallest unmarked elements with the smallest indices if they exist, the marked elements now are <code>nums = [**<ins>1</ins>**,<ins>**2**</ins>,2,<ins>**1**</ins>,2,3,1]</code>. The sum of unmarked elements is `2 + 2 + 3 + 1 = 8`.
*   Mark the element at index `3`, since it is already marked we skip it. Then we mark `3` of the smallest unmarked elements with the smallest indices, the marked elements now are <code>nums = [**<ins>1</ins>**,<ins>**2**</ins>,<ins>**2**</ins>,<ins>**1**</ins>,<ins>**2**</ins>,3,**<ins>1</ins>**]</code>. The sum of unmarked elements is `3`.
*   Mark the element at index `4`, since it is already marked we skip it. Then we mark `2` of the smallest unmarked elements with the smallest indices if they exist, the marked elements now are <code>nums = [**<ins>1</ins>**,<ins>**2**</ins>,<ins>**2**</ins>,<ins>**1**</ins>,<ins>**2**</ins>,**<ins>3</ins>**,<ins>**1**</ins>]</code>. The sum of unmarked elements is `0`.

**Example 2:**

**Input:** nums = [1,4,2,3], queries = \[\[0,1]]

**Output:** [7]

**Explanation:** We do one query which is mark the element at index `0` and mark the smallest element among unmarked elements. The marked elements will be <code>nums = [**<ins>1</ins>**,4,<ins>**2**</ins>,3]</code>, and the sum of unmarked elements is `4 + 3 = 7`.

**Constraints:**

*   `n == nums.length`
*   `m == queries.length`
*   <code>1 <= m <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= index<sub>i</sub>, k<sub>i</sub> <= n - 1</code>

## Solution

```kotlin
@Suppress("kotlin:S1871")
class Solution {
    fun unmarkedSumArray(nums: IntArray, queries: Array<IntArray>): LongArray {
        val l = nums.size
        val orig = IntArray(l)
        for (i in 0 until l) {
            orig[i] = i
        }
        var x = 1
        while (x < l) {
            val temp = IntArray(l)
            val teor = IntArray(l)
            var y = 0
            while (y < l) {
                var s1 = 0
                var s2 = 0
                while (s1 + s2 < 2 * x && y + s1 + s2 < l) {
                    if (s2 >= x || y + x + s2 >= l) {
                        temp[y + s1 + s2] = nums[y + s1]
                        teor[y + s1 + s2] = orig[y + s1]
                        s1++
                    } else if (s1 >= x) {
                        temp[y + s1 + s2] = nums[y + x + s2]
                        teor[y + s1 + s2] = orig[y + x + s2]
                        s2++
                    } else if (nums[y + s1] <= nums[y + x + s2]) {
                        temp[y + s1 + s2] = nums[y + s1]
                        teor[y + s1 + s2] = orig[y + s1]
                        s1++
                    } else {
                        temp[y + s1 + s2] = nums[y + x + s2]
                        teor[y + s1 + s2] = orig[y + x + s2]
                        s2++
                    }
                }
                y += 2 * x
            }
            for (i in 0 until l) {
                nums[i] = temp[i]
                orig[i] = teor[i]
            }
            x *= 2
        }
        val change = IntArray(l)
        for (i in 0 until l) {
            change[orig[i]] = i
        }
        val mark = BooleanArray(l)
        val m = queries.size
        var st = 0
        var sum: Long = 0
        for (num in nums) {
            sum += num.toLong()
        }
        val out = LongArray(m)
        for (i in 0 until m) {
            val a = queries[i][0]
            if (!mark[change[a]]) {
                mark[change[a]] = true
                sum -= nums[change[a]].toLong()
            }
            val b = queries[i][1]
            var many = 0
            while (many < b) {
                if (st == l) {
                    out[i] = sum
                    break
                }
                if (!mark[st]) {
                    mark[st] = true
                    sum -= nums[st].toLong()
                    many++
                }
                st++
            }
            out[i] = sum
        }
        return out
    }
}
```