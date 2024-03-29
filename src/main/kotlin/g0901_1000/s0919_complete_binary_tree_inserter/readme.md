[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 919\. Complete Binary Tree Inserter

Medium

A **complete binary tree** is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.

Design an algorithm to insert a new node to a complete binary tree keeping it complete after the insertion.

Implement the `CBTInserter` class:

*   `CBTInserter(TreeNode root)` Initializes the data structure with the `root` of the complete binary tree.
*   `int insert(int v)` Inserts a `TreeNode` into the tree with value `Node.val == val` so that the tree remains complete, and returns the value of the parent of the inserted `TreeNode`.
*   `TreeNode get_root()` Returns the root node of the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/03/lc-treeinsert.jpg)

**Input** ["CBTInserter", "insert", "insert", "get\_root"] [[[1, 2]], [3], [4], []]

**Output:** [null, 1, 2, [1, 2, 3, 4]]

**Explanation:**

    CBTInserter cBTInserter = new CBTInserter([1, 2]); 
    cBTInserter.insert(3); // return 1 
    cBTInserter.insert(4); // return 2 
    cBTInserter.get\_root(); // return [1, 2, 3, 4]

**Constraints:**

*   The number of nodes in the tree will be in the range `[1, 1000]`.
*   `0 <= Node.val <= 5000`
*   `root` is a complete binary tree.
*   `0 <= val <= 5000`
*   At most <code>10<sup>4</sup></code> calls will be made to `insert` and `get_root`.

## Solution

```kotlin
import com_github_leetcode.TreeNode
import java.util.LinkedList
import java.util.Queue

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
class CBTInserter(root: TreeNode?) {
    private val q: Queue<TreeNode>
    private val head: TreeNode

    init {
        q = LinkedList()
        head = root!!
        addToQueue()
    }

    private fun addToQueue() {
        val hlq: Queue<TreeNode> = LinkedList()
        hlq.add(head)
        while (hlq.isNotEmpty()) {
            var size = hlq.size
            while (size-- > 0) {
                val poll: TreeNode = hlq.poll()
                q.add(poll)
                if (poll.left != null) {
                    hlq.add(poll.left)
                }
                if (poll.right != null) {
                    hlq.add(poll.right)
                }
            }
        }
    }

    fun insert(`val`: Int): Int {
        val nn = TreeNode(`val`)
        deleteFullNode()
        val peek: TreeNode = q.peek()
        if (peek.left == null) {
            peek.left = nn
        } else {
            peek.right = nn
        }
        q.add(nn)
        return peek.`val`
    }

    private fun deleteFullNode() {
        while (q.isNotEmpty()) {
            val peek: TreeNode = q.peek()
            if (peek.left != null && peek.right != null) {
                q.poll()
            } else {
                break
            }
        }
    }

    // get_root()
    fun getRoot(): TreeNode {
        return head
    }
}

/*
 * Your CBTInserter object will be instantiated and called as such:
 * var obj = CBTInserter(root)
 * var param_1 = obj.insert(`val`)
 * var param_2 = obj.get_root()
 */
```