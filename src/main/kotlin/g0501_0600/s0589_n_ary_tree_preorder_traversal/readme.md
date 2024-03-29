[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 589\. N-ary Tree Preorder Traversal

Easy

Given the `root` of an n-ary tree, return _the preorder traversal of its nodes' values_.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

**Input:** root = [1,null,3,2,4,null,5,6]

**Output:** [1,3,5,6,2,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

**Input:** root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]

**Output:** [1,2,3,6,7,11,14,4,8,12,5,9,13,10]

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>4</sup></code>
*   The height of the n-ary tree is less than or equal to `1000`.

**Follow up:** Recursive solution is trivial, could you do it iteratively?

## Solution

```kotlin
import com_github_leetcode.Node

/*
 * Definition for a Node.
 * class Node(var `val`: Int) {
 *     var children: List<Node?> = listOf()
 * }
 */
class Solution {
    fun preorder(root: Node?): List<Int> {
        val res: MutableList<Int> = ArrayList()
        preorderHelper(res, root)
        return res
    }

    private fun preorderHelper(res: MutableList<Int>, root: Node?) {
        if (root == null) {
            return
        }
        res.add(root.`val`)
        for (node in root.neighbors) {
            preorderHelper(res, node)
        }
    }
}
```