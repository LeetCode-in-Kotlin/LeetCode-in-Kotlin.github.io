[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 558\. Logical OR of Two Binary Grids Represented as Quad-Trees

Medium

A Binary Matrix is a matrix in which all the elements are either **0** or **1**.

Given `quadTree1` and `quadTree2`. `quadTree1` represents a `n * n` binary matrix and `quadTree2` represents another `n * n` binary matrix.

Return _a Quad-Tree_ representing the `n * n` binary matrix which is the result of **logical bitwise OR** of the two binary matrixes represented by `quadTree1` and `quadTree2`.

Notice that you can assign the value of a node to **True** or **False** when `isLeaf` is **False**, and both are **accepted** in the answer.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

*   `val`: True if the node represents a grid of 1's or False if the node represents a grid of 0's.
*   `isLeaf`: True if the node is leaf node on the tree or False if the node has the four children.

    class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
    }

We can construct a Quad-Tree from a two-dimensional area using the following steps:

1.  If the current grid has the same value (i.e all `1's` or all `0's`) set `isLeaf` True and set `val` to the value of the grid and set the four children to Null and stop.
2.  If the current grid has different values, set `isLeaf` to False and set `val` to any value and divide the current grid into four sub-grids as shown in the photo.
3.  Recurse for each of the children with the proper sub-grid.

![](https://assets.leetcode.com/uploads/2020/02/11/new_top.png)

If you want to know more about the Quad-Tree, you can refer to the [wiki](https://en.wikipedia.org/wiki/Quadtree).

**Quad-Tree format:**

The input/output represents the serialized format of a Quad-Tree using level order traversal, where `null` signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list `[isLeaf, val]`.

If the value of `isLeaf` or `val` is True we represent it as **1** in the list `[isLeaf, val]` and if the value of `isLeaf` or `val` is False we represent it as **0**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/02/11/qt1.png) ![](https://assets.leetcode.com/uploads/2020/02/11/qt2.png)

**Input:** quadTree1 = \[\[0,1],[1,1],[1,1],[1,0],[1,0]] , quadTree2 = \[\[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]

**Output:** [[0,0],[1,1],[1,1],[1,1],[1,0]]

**Explanation:** quadTree1 and quadTree2 are shown above. You can see the binary matrix which is represented by each Quad-Tree. If we apply logical bitwise OR on the two binary matrices we get the binary matrix below which is represented by the result Quad-Tree. Notice that the binary matrices shown are only for illustration, you don't have to construct the binary matrix to get the result tree. ![](https://assets.leetcode.com/uploads/2020/02/11/qtr.png)

**Example 2:**

**Input:** quadTree1 = \[\[1,0]], quadTree2 = \[\[1,0]]

**Output:** [[1,0]]

**Explanation:** Each tree represents a binary matrix of size 1\*1. Each matrix contains only zero. The resulting matrix is of size 1\*1 with also zero.

**Constraints:**

*   `quadTree1` and `quadTree2` are both **valid** Quad-Trees each representing a `n * n` grid.
*   <code>n == 2<sup>x</sup></code> where `0 <= x <= 9`.

## Solution

```kotlin
/*
 * Definition for a QuadTree node.
 * class Node(var `val`: Boolean, var isLeaf: Boolean) {
 *     var topLeft: Node? = null
 *     var topRight: Node? = null
 *     var bottomLeft: Node? = null
 *     var bottomRight: Node? = null
 * }
 */
class Solution {
    fun intersect(quadTree1: Node?, quadTree2: Node?): Node? {
        if (quadTree1!!.isLeaf) {
            return if (quadTree1.`val`) quadTree1 else quadTree2
        }
        if (quadTree2!!.isLeaf) {
            return if (quadTree2.`val`) quadTree2 else quadTree1
        }
        val out = Node()
        val tl: Node? = intersect(quadTree1.topLeft, quadTree2.topLeft)
        val tr: Node? = intersect(quadTree1.topRight, quadTree2.topRight)
        val bl: Node? = intersect(quadTree1.bottomLeft, quadTree2.bottomLeft)
        val br: Node? = intersect(quadTree1.bottomRight, quadTree2.bottomRight)
        if ((
                tl!!.isLeaf &&
                    tr!!.isLeaf &&
                    bl!!.isLeaf &&
                    br!!.isLeaf && tl.`val` == tr.`val`
                ) && tr.`val` == bl.`val` && br.`val` == bl.`val`
        ) {
            out.isLeaf = true
            out.`val` = tl.`val`
        } else {
            out.topLeft = tl
            out.topRight = tr
            out.bottomLeft = bl
            out.bottomRight = br
        }
        return out
    }
}
```