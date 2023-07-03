[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2196\. Create Binary Tree From Descriptions

Medium

You are given a 2D integer array `descriptions` where <code>descriptions[i] = [parent<sub>i</sub>, child<sub>i</sub>, isLeft<sub>i</sub>]</code> indicates that <code>parent<sub>i</sub></code> is the **parent** of <code>child<sub>i</sub></code> in a **binary** tree of **unique** values. Furthermore,

*   If <code>isLeft<sub>i</sub> == 1</code>, then <code>child<sub>i</sub></code> is the left child of <code>parent<sub>i</sub></code>.
*   If <code>isLeft<sub>i</sub> == 0</code>, then <code>child<sub>i</sub></code> is the right child of <code>parent<sub>i</sub></code>.

Construct the binary tree described by `descriptions` and return _its **root**_.

The test cases will be generated such that the binary tree is **valid**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/02/09/example1drawio.png)

**Input:** descriptions = \[\[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]

**Output:** [50,20,80,15,17,19]

**Explanation:** The root node is the node with value 50 since it has no parent.

The resulting binary tree is shown in the diagram. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/02/09/example2drawio.png)

**Input:** descriptions = \[\[1,2,1],[2,3,0],[3,4,1]]

**Output:** [1,2,null,null,3,4]

**Explanation:** The root node is the node with value 1 since it has no parent.

The resulting binary tree is shown in the diagram. 

**Constraints:**

*   <code>1 <= descriptions.length <= 10<sup>4</sup></code>
*   `descriptions[i].length == 3`
*   <code>1 <= parent<sub>i</sub>, child<sub>i</sub> <= 10<sup>5</sup></code>
*   <code>0 <= isLeft<sub>i</sub> <= 1</code>
*   The binary tree described by `descriptions` is valid.

## Solution

```kotlin
import com_github_leetcode.TreeNode

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
    fun createBinaryTree(descriptions: Array<IntArray>): TreeNode? {
        val map: MutableMap<Int, Data> = HashMap()
        for (description in descriptions) {
            var data = map[description[0]]
            if (data == null) {
                data = Data()
                data.node = TreeNode(description[0])
                data.isHead = true
                map[description[0]] = data
            }
            var childData = map[description[1]]
            if (childData == null) {
                childData = Data()
                childData.node = TreeNode(description[1])
                map[childData.node!!.`val`] = childData
            }
            childData.isHead = false
            if (description[2] == 1) {
                data.node!!.left = childData.node
            } else {
                data.node!!.right = childData.node
            }
        }
        for ((_, value) in map) {
            if (value.isHead) {
                return value.node
            }
        }
        return null
    }

    private class Data {
        var node: TreeNode? = null
        var isHead = false
    }
}
```