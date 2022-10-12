[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 117\. Populating Next Right Pointers in Each Node II

Medium

Given a binary tree

struct Node { int val; Node \*left; Node \*right; Node \*next; }

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

**Input:** root = [1,2,3,4,5,null,7]

**Output:** [1,#,2,3,#,4,5,7,#]

**Explanation:** Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

**Example 2:**

**Input:** root = []

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 6000]`.
*   `-100 <= Node.val <= 100`

**Follow-up:**

*   You may only use constant extra space.
*   The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

## Solution

```kotlin
import com_github_leetcode.left_right.Node
import java.util.LinkedList
import java.util.Queue

/*
 * Definition for a Node.
 * class Node(var `val`: Int) {
 *     var left: Node? = null
 *     var right: Node? = null
 *     var next: Node? = null
 * }
 */
class Solution {
    fun connect(root: Node?): Node? {

        if (root == null) return null

        val bfsQueue: Queue<Node> = LinkedList()

        bfsQueue.offer(root)
        root.next = null

        var temp: Node?
        var prev: Node?

        while (!bfsQueue.isEmpty()) {

            val size = bfsQueue.size
            prev = null

            for (j in 0 until size) {

                temp = bfsQueue.poll()
                if (prev != null) prev.next = temp
                if (temp!!.left != null) bfsQueue.offer(temp.left)
                if (temp.right != null) bfsQueue.offer(temp.right)
                prev = temp
            }
        }

        return root
    }
}
```