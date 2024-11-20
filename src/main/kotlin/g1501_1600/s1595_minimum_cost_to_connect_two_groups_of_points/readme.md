[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1595\. Minimum Cost to Connect Two Groups of Points

Hard

You are given two groups of points where the first group has <code>size<sub>1</sub></code> points, the second group has <code>size<sub>2</sub></code> points, and <code>size<sub>1</sub> >= size<sub>2</sub></code>.

The `cost` of the connection between any two points are given in an <code>size<sub>1</sub> x size<sub>2</sub></code> matrix where `cost[i][j]` is the cost of connecting point `i` of the first group and point `j` of the second group. The groups are connected if **each point in both groups is connected to one or more points in the opposite group**. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.

Return _the minimum cost it takes to connect the two groups_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex1.jpg)

**Input:** cost = \[\[15, 96], [36, 2]]

**Output:** 17

**Explanation:** The optimal way of connecting the groups is:

1--A

2--B

This results in a total cost of 17.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/03/ex2.jpg)

**Input:** cost = \[\[1, 3, 5], [4, 1, 1], [1, 5, 3]]

**Output:** 4

**Explanation:** The optimal way of connecting the groups is:

1--A

2--B

2--C

3--A

This results in a total cost of 4.

Note that there are multiple points connected to point 2 in the first group and point A in the second group.

This does not matter as there is no limit to the number of points that can be connected. We only care about the minimum total cost.

**Example 3:**

**Input:** cost = \[\[2, 5, 1], [3, 4, 7], [8, 1, 2], [6, 2, 4], [3, 8, 8]]

**Output:** 10

**Constraints:**

*   <code>size<sub>1</sub> == cost.length</code>
*   <code>size<sub>2</sub> == cost[i].length</code>
*   <code>1 <= size<sub>1</sub>, size<sub>2</sub> <= 12</code>
*   <code>size<sub>1</sub> >= size<sub>2</sub></code>
*   `0 <= cost[i][j] <= 100`

## Solution

```kotlin
class Solution {
    fun connectTwoGroups(cost: List<List<Int>>): Int {
        // size of set 1
        val m = cost.size
        // size of set 2
        val n = cost[0].size
        val mask = 1 shl m
        // min cost to connect nodes in set 1 (of different states);
        var record = IntArray(mask)
        record.fill(Int.MAX_VALUE)
        // since we use record to get the min cost of connecting nodes in set 1
        // we shall go through nodes in set 2 one by one, to make sure they are connected
        // base case:
        record[0] = 0
        for (col in 0 until n) {
            val tmpRecord = IntArray(mask)
            tmpRecord.fill(Int.MAX_VALUE)
            // try connection with each of the node in set 1
            for (row in 0 until m) {
                for (msk in 0 until mask) {
                    // the new min cost should be based on the cost record of connecting previous
                    // node in set 2;
                    val newMask = msk or (1 shl row)
                    if (record[msk] != Int.MAX_VALUE) {
                        tmpRecord[newMask] = Math.min(tmpRecord[newMask], record[msk] + cost[row][col])
                    }
                    // if row nodes in this state has not been connected yet, and the msk is
                    // achievable by connecting the current node
                    // then check whether connect the current node multiple times will benefit the
                    // cost
                    if (msk and (1 shl row) == 0 && tmpRecord[msk] != Int.MAX_VALUE) {
                        tmpRecord[newMask] = Math.min(
                            tmpRecord[newMask],
                            tmpRecord[msk] + cost[row][col],
                        )
                    }
                }
            }
            // use tmpRecord to update record
            record = tmpRecord
        }
        return record[(1 shl m) - 1]
    }
}
```