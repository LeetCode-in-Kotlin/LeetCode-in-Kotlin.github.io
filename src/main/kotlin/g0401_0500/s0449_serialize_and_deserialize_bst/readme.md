[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 449\. Serialize and Deserialize BST

Medium

Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

**Example 1:**

**Input:** root = [2,1,3]

**Output:** [2,1,3]

**Example 2:**

**Input:** root = []

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 10<sup>4</sup></code>
*   The input tree is **guaranteed** to be a binary search tree.

## Solution

```kotlin
import com_github_leetcode.TreeNode

/*
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */

class Codec {
    private var cur = 0

    // Encodes a tree to a single string.
    fun serialize(root: TreeNode?): String {
        val sb = StringBuilder()
        serializeDfs(root, sb)
        return sb.toString()
    }

    private fun serializeDfs(root: TreeNode?, sb: StringBuilder) {
        if (root == null) {
            sb.append(SPLIT)
            return
        }
        sb.append((root.`val` + MIN).toChar())
        serializeDfs(root.left, sb)
        serializeDfs(root.right, sb)
    }

    // Decodes your encoded data to tree.
    fun deserialize(data: String): TreeNode? {
        cur = 0
        return deserializeDFS(data.toCharArray())
    }

    private fun deserializeDFS(data: CharArray): TreeNode? {
        if (data[cur] == SPLIT) {
            cur++
            return null
        }
        val node = TreeNode(data[cur++].code - MIN)
        node.left = deserializeDFS(data)
        node.right = deserializeDFS(data)
        return node
    }

    companion object {
        private const val SPLIT = 0.toChar()
        private const val MIN = 1
    }
}

/*
 * Your Codec object will be instantiated and called as such:
 * val ser = Codec()
 * val deser = Codec()
 * val tree: String = ser.serialize(root)
 * val ans = deser.deserialize(tree)
 * return ans
 */
```