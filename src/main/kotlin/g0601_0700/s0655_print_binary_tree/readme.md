[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 655\. Print Binary Tree

Medium

Given the `root` of a binary tree, construct a **0-indexed** `m x n` string matrix `res` that represents a **formatted layout** of the tree. The formatted layout matrix should be constructed using the following rules:

*   The **height** of the tree is `height` and the number of rows `m` should be equal to `height + 1`.
*   The number of columns `n` should be equal to <code>2<sup>height+1</sup> - 1</code>.
*   Place the **root node** in the **middle** of the **top row** (more formally, at location `res[0][(n-1)/2]`).
*   For each node that has been placed in the matrix at position `res[r][c]`, place its **left child** at <code>res[r+1][c-2<sup>height-r-1</sup>]</code> and its **right child** at <code>res[r+1][c+2<sup>height-r-1</sup>]</code>.
*   Continue this process until all the nodes in the tree have been placed.
*   Any empty cells should contain the empty string `""`.

Return _the constructed matrix_ `res`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/print1-tree.jpg)

**Input:** root = [1,2]

**Output:** 
    
    [["","1",""],
     ["2","",""]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/03/print2-tree.jpg)

**Input:** root = [1,2,3,null,4]

**Output:** 

    [["","","","1","","",""], 
     ["","2","","","","3",""], 
     ["","","4","","","",""]]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[1, 2<sup>10</sup>]</code>.
*   `-99 <= Node.val <= 99`
*   The depth of the tree will be in the range `[1, 10]`.

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.LinkedList
import kotlin.math.pow

/*
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun printTree(root: TreeNode?): List<MutableList<String>> {
        val result: MutableList<MutableList<String>> = LinkedList()
        val height = if (root == null) 1 else getHeight(root)
        val columns = (2.0.pow(height.toDouble()) - 1).toInt()
        val row: MutableList<String> = ArrayList()
        for (i in 0 until columns) {
            row.add("")
        }
        for (i in 0 until height) {
            result.add(ArrayList(row))
        }
        populateResult(root, result, 0, height, 0, columns - 1)
        return result
    }

    private fun populateResult(
        root: TreeNode?,
        result: List<MutableList<String>>,
        row: Int,
        totalRows: Int,
        i: Int,
        j: Int,
    ) {
        if (row == totalRows || root == null) {
            return
        }
        result[row][(i + j) / 2] = root.`val`.toString()
        populateResult(root.left, result, row + 1, totalRows, i, (i + j) / 2 - 1)
        populateResult(root.right, result, row + 1, totalRows, (i + j) / 2 + 1, j)
    }

    private fun getHeight(root: TreeNode?): Int {
        return if (root == null) {
            0
        } else {
            1 + getHeight(root.left).coerceAtLeast(getHeight(root.right))
        }
    }
}
```