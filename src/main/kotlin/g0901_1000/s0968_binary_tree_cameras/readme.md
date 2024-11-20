[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 968\. Binary Tree Cameras

Hard

You are given the `root` of a binary tree. We install cameras on the tree nodes where each camera at a node can monitor its parent, itself, and its immediate children.

Return _the minimum number of cameras needed to monitor all nodes of the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

**Input:** root = [0,0,null,0,0]

**Output:** 1

**Explanation:** One camera is enough to monitor all nodes if placed as shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_02.png)

**Input:** root = [0,0,null,0,null,0,null,null,0]

**Output:** 2

**Explanation:** At least two cameras are needed to monitor all nodes of the tree. The above image shows one of the valid configurations of camera placement.

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `Node.val == 0`

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
    private var cameras = 0
    fun minCameraCover(root: TreeNode?): Int {
        cameras = 0
        if (minCameras(root) == -1) {
            // root needs a camera
            cameras++
        }
        return cameras
    }

    //  States =>
    // -1 : Node needs a camera
    //  0 : Node has a camera placed
    //  1 : Node is covered somehow
    private fun minCameras(root: TreeNode?): Int {
        if (root == null) {
            return 1
        }
        val leftChildState = minCameras(root.left)
        val rightChildState = minCameras(root.right)
        // One of the two or both children need a camera
        if (leftChildState == -1 || rightChildState == -1) {
            // installed
            cameras++
            return 0
        }
        // One of the two or both children have a camera placed
        return if (leftChildState == 0 || rightChildState == 0) {
            // gets covered by the children
            1
        } else {
            -1
        }
        // needs a camera
    }
}
```