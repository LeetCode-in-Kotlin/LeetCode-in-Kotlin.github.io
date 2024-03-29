[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 975\. Odd Even Jump

Hard

You are given an integer array `arr`. From some starting index, you can make a series of jumps. The (1<sup>st</sup>, 3<sup>rd</sup>, 5<sup>th</sup>, ...) jumps in the series are called **odd-numbered jumps**, and the (2<sup>nd</sup>, 4<sup>th</sup>, 6<sup>th</sup>, ...) jumps in the series are called **even-numbered jumps**. Note that the **jumps** are numbered, not the indices.

You may jump forward from index `i` to index `j` (with `i < j`) in the following way:

*   During **odd-numbered jumps** (i.e., jumps 1, 3, 5, ...), you jump to the index `j` such that `arr[i] <= arr[j]` and `arr[j]` is the smallest possible value. If there are multiple such indices `j`, you can only jump to the **smallest** such index `j`.
*   During **even-numbered jumps** (i.e., jumps 2, 4, 6, ...), you jump to the index `j` such that `arr[i] >= arr[j]` and `arr[j]` is the largest possible value. If there are multiple such indices `j`, you can only jump to the **smallest** such index `j`.
*   It may be the case that for some index `i`, there are no legal jumps.

A starting index is **good** if, starting from that index, you can reach the end of the array (index `arr.length - 1`) by jumping some number of times (possibly 0 or more than once).

Return _the number of **good** starting indices_.

**Example 1:**

**Input:** arr = [10,13,12,14,15]

**Output:** 2

**Explanation:**

From starting index i = 0, we can make our 1st jump to i = 2 (since arr[2] is the smallest among arr[1],

arr[2], arr[3], arr[4] that is greater or equal to arr[0]), then we cannot jump any more.

From starting index i = 1 and i = 2, we can make our 1st jump to i = 3, then we cannot jump any more.

From starting index i = 3, we can make our 1st jump to i = 4, so we have reached the end.

From starting index i = 4, we have reached the end already.

In total, there are 2 different starting indices i = 3 and i = 4, where we can reach the end with some number of jumps.

**Example 2:**

**Input:** arr = [2,3,1,1,4]

**Output:** 3

**Explanation:**

From starting index i = 0, we make jumps to i = 1, i = 2, i = 3:

During our 1st jump (odd-numbered), we first jump to i = 1 because arr[1] is the smallest value in [arr[1],

arr[2], arr[3], arr[4]] that is greater than or equal to arr[0].

During our 2nd jump (even-numbered), we jump from i = 1 to i = 2 because arr[2] is the largest value in

[arr[2], arr[3], arr[4]] that is less than or equal to arr[1]. arr[3] is also the largest value, but 2 is a

smaller index, so we can only jump to i = 2 and not i = 3

During our 3rd jump (odd-numbered), we jump from i = 2 to i = 3 because arr[3] is the smallest value in

[arr[3], arr[4]] that is greater than or equal to arr[2].

We can't jump from i = 3 to i = 4, so the starting index i = 0 is not good.

In a similar manner, we can deduce that: From starting index i = 1, we jump to i = 4, so we reach the end.

From starting index i = 2, we jump to i = 3, and then we can't jump anymore.

From starting index i = 3, we jump to i = 4, so we reach the end.

From starting index i = 4, we are already at the end.

In total, there are 3 different starting indices i = 1, i = 3, and i = 4, where we can reach the end with

some number of jumps.

**Example 3:**

**Input:** arr = [5,1,3,4,2]

**Output:** 3

**Explanation:** We can reach the end from starting indices 1, 2, and 4.

**Constraints:**

*   <code>1 <= arr.length <= 2 * 10<sup>4</sup></code>
*   <code>0 <= arr[i] < 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    private lateinit var valToPos: IntArray

    fun oddEvenJumps(arr: IntArray): Int {
        val size = arr.size
        val odd = BooleanArray(size)
        val even = BooleanArray(size)
        valToPos = IntArray(100001)
        valToPos.fill(-1)
        valToPos[arr[size - 1]] = size - 1
        even[size - 1] = true
        odd[size - 1] = even[size - 1]
        var count = 1
        for (i in size - 2 downTo 0) {
            val curVal = arr[i]
            val maxS = findMaxS(curVal)
            val minL = findMinL(curVal)
            if (minL != -1 && even[minL]) {
                // System.out.println("find minL is true at: "+minL+" start from "+i);
                odd[i] = even[minL]
                count++
            }
            if (maxS != -1) {
                even[i] = odd[maxS]
            }
            valToPos[arr[i]] = i
        }
        return count
    }

    private fun findMaxS(`val`: Int): Int {
        for (i in `val` downTo 0) {
            if (valToPos[i] != -1) {
                return valToPos[i]
            }
        }
        return -1
    }

    private fun findMinL(`val`: Int): Int {
        for (i in `val`..100000) {
            if (valToPos[i] != -1) {
                return valToPos[i]
            }
        }
        return -1
    }
}
```