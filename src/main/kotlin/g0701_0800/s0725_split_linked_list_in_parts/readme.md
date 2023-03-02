[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 725\. Split Linked List in Parts

Medium

Given the `head` of a singly linked list and an integer `k`, split the linked list into `k` consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return _an array of the_ `k` _parts_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg)

**Input:** head = [1,2,3], k = 5

**Output:** [[1],[2],[3],[],[]]

**Explanation:** The first element output[0] has output[0].val = 1, output[0].next = null. The last element output[4] is null, but its string representation as a ListNode is [].

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg)

**Input:** head = [1,2,3,4,5,6,7,8,9,10], k = 3

**Output:** [[1,2,3,4],[5,6,7],[8,9,10]]

**Explanation:** The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.

**Constraints:**

*   The number of nodes in the list is in the range `[0, 1000]`.
*   `0 <= Node.val <= 1000`
*   `1 <= k <= 50`

## Solution

```kotlin
import com_github_leetcode.ListNode
import java.util.Objects

/*
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
@Suppress("NAME_SHADOWING")
class Solution {
    fun splitListToParts(head: ListNode?, k: Int): Array<ListNode?> {
        var head = head
        val len = getLength(head)
        val aveSize = len / k
        var extra = len % k
        val result = arrayOfNulls<ListNode>(k)
        for (i in 0 until k) {
            result[i] = head
            var aveSizeTmp = aveSize
            aveSizeTmp += if (extra-- > 0) 1 else 0
            var aveSizeTmp2 = aveSizeTmp
            while (aveSizeTmp-- > 0) {
                head = Objects.requireNonNull(head)?.next
            }
            if (result[i] != null) {
                var tmp = result[i]
                while (tmp!!.next != null && aveSizeTmp2-- > 1) {
                    tmp = tmp.next
                }
                tmp.next = null
            }
        }
        return result
    }

    private fun getLength(root: ListNode?): Int {
        var len = 0
        var tmp = root
        while (tmp != null) {
            len++
            tmp = tmp.next
        }
        return len
    }
}
```