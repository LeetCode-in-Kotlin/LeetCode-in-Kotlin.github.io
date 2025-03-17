[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3245\. Alternating Groups III

Hard

There are some red and blue tiles arranged circularly. You are given an array of integers `colors` and a 2D integers array `queries`.

The color of tile `i` is represented by `colors[i]`:

*   `colors[i] == 0` means that tile `i` is **red**.
*   `colors[i] == 1` means that tile `i` is **blue**.

An **alternating** group is a contiguous subset of tiles in the circle with **alternating** colors (each tile in the group except the first and last one has a different color from its **adjacent** tiles in the group).

You have to process queries of two types:

*   <code>queries[i] = [1, size<sub>i</sub>]</code>, determine the count of **alternating** groups with size <code>size<sub>i</sub></code>.
*   <code>queries[i] = [2, index<sub>i</sub>, color<sub>i</sub>]</code>, change <code>colors[index<sub>i</sub>]</code> to <code>color<sub>i</sub></code>.

Return an array `answer` containing the results of the queries of the first type _in order_.

**Note** that since `colors` represents a **circle**, the **first** and the **last** tiles are considered to be next to each other.

**Example 1:**

**Input:** colors = [0,1,1,0,1], queries = \[\[2,1,0],[1,4]]

**Output:** [2]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-14-44.png)**

First query:

Change `colors[1]` to 0.

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-20-25.png)

Second query:

Count of the alternating groups with size 4:

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-25-02-2.png)![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-24-12.png)

**Example 2:**

**Input:** colors = [0,0,1,0,1,1], queries = \[\[1,3],[2,3,0],[1,5]]

**Output:** [2,0]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-35-50.png)

First query:

Count of the alternating groups with size 3:

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-37-13.png)![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-from-2024-06-03-20-36-40.png)

Second query: `colors` will not change.

Third query: There is no alternating group with size 5.

**Constraints:**

*   <code>4 <= colors.length <= 5 * 10<sup>4</sup></code>
*   `0 <= colors[i] <= 1`
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   `queries[i][0] == 1` or `queries[i][0] == 2`
*   For all `i` that:
    *   `queries[i][0] == 1`: `queries[i].length == 2`, `3 <= queries[i][1] <= colors.length - 1`
    *   `queries[i][0] == 2`: `queries[i].length == 3`, `0 <= queries[i][1] <= colors.length - 1`, `0 <= queries[i][2] <= 1`

## Solution

```kotlin
import java.util.BitSet

class Solution {
    fun numberOfAlternatingGroups(colors: IntArray, queries: Array<IntArray>): MutableList<Int?> {
        val n = colors.size
        val set = BitSet()
        val bit = BIT(n)
        for (i in 0..<n) {
            if (colors[i] == colors[getIndex(i + 1, n)]) {
                add(set, bit, n, i)
            }
        }
        val ans: MutableList<Int?> = ArrayList<Int?>()
        for (q in queries) {
            if (q[0] == 1) {
                if (set.isEmpty) {
                    ans.add(n)
                } else {
                    val size = q[1]
                    val res = bit.query(size)
                    ans.add(res[1] - res[0] * (size - 1))
                }
            } else {
                val i = q[1]
                var color = colors[i]
                if (q[2] == color) {
                    continue
                }
                val pre = getIndex(i - 1, n)
                if (colors[pre] == color) {
                    remove(set, bit, n, pre)
                }
                val next = getIndex(i + 1, n)
                if (colors[next] == color) {
                    remove(set, bit, n, i)
                }
                colors[i] = colors[i] xor 1
                color = colors[i]
                if (colors[pre] == color) {
                    add(set, bit, n, pre)
                }
                if (colors[next] == color) {
                    add(set, bit, n, i)
                }
            }
        }
        return ans
    }

    private fun add(set: BitSet, bit: BIT, n: Int, i: Int) {
        if (set.isEmpty) {
            bit.update(n, 1)
        } else {
            update(set, bit, n, i, 1)
        }
        set.set(i)
    }

    private fun remove(set: BitSet, bit: BIT, n: Int, i: Int) {
        set.clear(i)
        if (set.isEmpty) {
            bit.update(n, -1)
        } else {
            update(set, bit, n, i, -1)
        }
    }

    private fun update(set: BitSet, bit: BIT, n: Int, i: Int, v: Int) {
        var pre = set.previousSetBit(i)
        if (pre == -1) {
            pre = set.previousSetBit(n)
        }
        var next = set.nextSetBit(i)
        if (next == -1) {
            next = set.nextSetBit(0)
        }
        bit.update(getIndex(next - pre + n - 1, n) + 1, -v)
        bit.update(getIndex(i - pre, n), v)
        bit.update(getIndex(next - i, n), v)
    }

    private fun getIndex(index: Int, mod: Int): Int {
        val result = if (index >= mod) index - mod else index
        return if (index < 0) index + mod else result
    }

    private class BIT(n: Int) {
        var n: Int = n + 1
        var tree1: IntArray = IntArray(n + 1)
        var tree2: IntArray = IntArray(n + 1)

        fun update(size: Int, v: Int) {
            var i = size
            while (i > 0) {
                tree1[i] += v
                tree2[i] += v * size
                i -= i and -i
            }
        }

        fun query(size: Int): IntArray {
            var count = 0
            var sum = 0
            var i = size
            while (i < n) {
                count += tree1[i]
                sum += tree2[i]
                i += i and -i
            }
            return intArrayOf(count, sum)
        }
    }
}
```