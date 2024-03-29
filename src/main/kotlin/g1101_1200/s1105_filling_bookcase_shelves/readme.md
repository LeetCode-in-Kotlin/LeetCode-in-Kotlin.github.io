[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1105\. Filling Bookcase Shelves

Medium

You are given an array `books` where <code>books[i] = [thickness<sub>i</sub>, height<sub>i</sub>]</code> indicates the thickness and height of the <code>i<sup>th</sup></code> book. You are also given an integer `shelfWidth`.

We want to place these books in order onto bookcase shelves that have a total width `shelfWidth`.

We choose some of the books to place on this shelf such that the sum of their thickness is less than or equal to `shelfWidth`, then build another level of the shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down. We repeat this process until there are no more books to place.

Note that at each step of the above process, the order of the books we place is the same order as the given sequence of books.

*   For example, if we have an ordered list of `5` books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth book on the last shelf.

Return _the minimum possible height that the total bookshelf can be after placing shelves in this manner_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/06/24/shelves.png)

**Input:** books = \[\[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelf\_width = 4

**Output:** 6

**Explanation:** 

The sum of the heights of the 3 shelves is 1 + 3 + 2 = 6. 

Notice that book number 2 does not have to be on the first shelf.

**Example 2:**

**Input:** books = \[\[1,3],[2,4],[3,2]], shelfWidth = 6

**Output:** 4

**Constraints:**

*   `1 <= books.length <= 1000`
*   <code>1 <= thickness<sub>i</sub> <= shelfWidth <= 1000</code>
*   <code>1 <= height<sub>i</sub> <= 1000</code>

## Solution

```kotlin
class Solution {
    fun minHeightShelves(books: Array<IntArray>, shelfWidth: Int): Int {
        val n = books.size
        val dp = IntArray(n + 1)
        dp.fill(Int.MAX_VALUE)
        dp[0] = 0
        for (i in 1..n) {
            var widthLeft = shelfWidth
            var maxH = 0
            for (j in i - 1 downTo 0) {
                widthLeft -= books[j][0]
                maxH = Math.max(maxH, books[j][1])
                if (widthLeft >= 0) {
                    dp[i] = Math.min(dp[i], maxH + dp[j])
                }
            }
        }
        return dp[n]
    }
}
```