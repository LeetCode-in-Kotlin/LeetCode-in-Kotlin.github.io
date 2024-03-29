[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2236\. Root Equals Sum of Children

Easy

You are given the `root` of a **binary tree** that consists of exactly `3` nodes: the root, its left child, and its right child.

Return `true` _if the value of the root is equal to the **sum** of the values of its two children, or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/04/08/graph3drawio.png)

**Input:** root = [10,4,6]

**Output:** true

**Explanation:** The values of the root, its left child, and its right child are 10, 4, and 6, respectively. 

10 is equal to 4 + 6, so we return true.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/04/08/graph3drawio-1.png)

**Input:** root = [5,3,1]

**Output:** false

**Explanation:** The values of the root, its left child, and its right child are 5, 3, and 1, respectively. 

5 is not equal to 3 + 1, so we return false.

**Constraints:**

* The tree consists only of the root, its left child, and its right child.
* `-100 <= Node.val <= 100`

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
    fun checkTree(root: TreeNode): Boolean {
        return root.left!!.`val` + root.right!!.`val` == root.`val`
    }
}
```