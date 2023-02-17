[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 654\. Maximum Binary Tree

Medium

You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

1.  Create a root node whose value is the maximum value in `nums`.
2.  Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
3.  Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

Return _the **maximum binary tree** built from_ `nums`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

**Input:** nums = [3,2,1,6,0,5]

**Output:** [6,3,5,null,2,0,null,null,1]

**Explanation:** The recursive calls are as follow:

- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].

    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].

        - Empty array, so no child.

        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].

            - Empty array, so no child.

            - Only one element, so child is a node with value 1.

    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].

        - Only one element, so child is a node with value 0.

        - Empty array, so no child.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

**Input:** nums = [3,2,1]

**Output:** [3,null,2,null,1]

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `0 <= nums[i] <= 1000`
*   All integers in `nums` are **unique**.

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
    fun constructMaximumBinaryTree(nums: IntArray): TreeNode? {
        return mbt(nums, 0, nums.size - 1)
    }

    private fun mbt(nums: IntArray, l: Int, r: Int): TreeNode? {
        if (l > r || l >= nums.size || r < 0) {
            return null
        }
        if (l == r) {
            return TreeNode(nums[r])
        }
        var max = Int.MIN_VALUE
        var maxidx = 0
        for (i in l..r) {
            if (nums[i] > max) {
                max = nums[i]
                maxidx = i
            }
        }
        val root = TreeNode(max)
        root.left = mbt(nums, l, maxidx - 1)
        root.right = mbt(nums, maxidx + 1, r)
        return root
    }
}
```