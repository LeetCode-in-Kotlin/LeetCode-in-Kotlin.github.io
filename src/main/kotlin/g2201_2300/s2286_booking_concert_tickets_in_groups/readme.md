[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2286\. Booking Concert Tickets in Groups

Hard

A concert hall has `n` rows numbered from `0` to `n - 1`, each with `m` seats, numbered from `0` to `m - 1`. You need to design a ticketing system that can allocate seats in the following cases:

*   If a group of `k` spectators can sit **together** in a row.
*   If **every** member of a group of `k` spectators can get a seat. They may or **may not** sit together.

Note that the spectators are very picky. Hence:

*   They will book seats only if each member of their group can get a seat with row number **less than or equal** to `maxRow`. `maxRow` can **vary** from group to group.
*   In case there are multiple rows to choose from, the row with the **smallest** number is chosen. If there are multiple seats to choose in the same row, the seat with the **smallest** number is chosen.

Implement the `BookMyShow` class:

*   `BookMyShow(int n, int m)` Initializes the object with `n` as number of rows and `m` as number of seats per row.
*   `int[] gather(int k, int maxRow)` Returns an array of length `2` denoting the row and seat number (respectively) of the **first seat** being allocated to the `k` members of the group, who must sit **together**. In other words, it returns the smallest possible `r` and `c` such that all `[c, c + k - 1]` seats are valid and empty in row `r`, and `r <= maxRow`. Returns `[]` in case it is **not possible** to allocate seats to the group.
*   `boolean scatter(int k, int maxRow)` Returns `true` if all `k` members of the group can be allocated seats in rows `0` to `maxRow`, who may or **may not** sit together. If the seats can be allocated, it allocates `k` seats to the group with the **smallest** row numbers, and the smallest possible seat numbers in each row. Otherwise, returns `false`.

**Example 1:**

**Input**

["BookMyShow", "gather", "gather", "scatter", "scatter"]

[[2, 5], [4, 0], [2, 0], [5, 1], [5, 1]]

**Output:** [null, [0, 0], [], true, false]

**Explanation:**

    BookMyShow bms = new BookMyShow(2, 5); // There are 2 rows with 5 seats each
    bms.gather(4, 0); // return [0, 0]
                      // The group books seats [0, 3] of row 0.
    bms.gather(2, 0); // return []
                      // There is only 1 seat left in row 0,
                      // so it is not possible to book 2 consecutive seats.
    bms.scatter(5, 1); // return True
                       // The group books seat 4 of row 0 and seats [0, 3] of row 1.
    bms.scatter(5, 1); // return False
                       // There is only one seat left in the hall. 

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= m, k <= 10<sup>9</sup></code>
*   `0 <= maxRow <= n - 1`
*   At most <code>5 * 10<sup>4</sup></code> calls **in total** will be made to `gather` and `scatter`.

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque

@Suppress("NAME_SHADOWING")
class BookMyShow(n: Int, private val m: Int) {
    private val n: Int

    // max number of seats in a row for some segment of the rows
    private val max: IntArray

    // total number of seats for some segment of the rows
    private val total: LongArray

    // number of rows with zero free places on the left and on the right
    // using this to quickly skip already zero rows
    // actual nodes are placed in [1,this.n], the first and last element only shows there the first
    // non-zero row
    private val numZerosRight: IntArray
    private val numZerosLeft: IntArray

    init {
        // make n to be a power of 2 (for simplicity)
        this.n = nextPow2(n)
        // segment tree for max number of seats in a row
        max = IntArray(this.n * 2 - 1)
        // total number of seats for a segment of the rows
        total = LongArray(this.n * 2 - 1)
        numZerosRight = IntArray(this.n + 2)
        numZerosLeft = IntArray(this.n + 2)
        // initialize max and total, for max we firstly set values to m
        // segments of size 1 are placed starting from this.n - 1
        max.fill(m, this.n - 1, this.n + n - 1)
        total.fill(m.toLong(), this.n - 1, this.n + n - 1)
        // calculate values of max and total for segments based on values of their children
        var i = this.n - 2
        var i1 = i * 2 + 1
        var i2 = i * 2 + 2
        while (i >= 0) {
            max[i] = Math.max(max[i1], max[i2])
            total[i] = total[i1] + total[i2]
            i--
            i1 -= 2
            i2 -= 2
        }
    }

    fun gather(k: Int, maxRow: Int): IntArray {
        // find most left row with enough free places
        val mostLeft = mostLeft(0, 0, n, k, maxRow + 1)
        if (mostLeft == -1) {
            return IntArray(0)
        }
        // get corresponding segment tree node
        var v = n - 1 + mostLeft
        val ans = intArrayOf(mostLeft, m - max[v])
        // update max and total for this node
        max[v] -= k
        total[v] -= k.toLong()
        // until this is a root of segment tree we update its parent
        while (v != 0) {
            v = (v - 1) / 2
            max[v] = Math.max(max[v * 2 + 1], max[v * 2 + 2])
            total[v] = total[v * 2 + 1] + total[v * 2 + 2]
        }
        return ans
    }

    private fun mostLeft(v: Int, l: Int, r: Int, k: Int, qr: Int): Int {
        if (l >= qr || max[v] < k) {
            return -1
        }
        if (l == r - 1) {
            return l
        }
        val mid = (l + r) / 2
        val left = mostLeft(v * 2 + 1, l, mid, k, qr)
        return if (left != -1) {
            left
        } else mostLeft(v * 2 + 2, mid, r, k, qr)
    }

    fun scatter(k: Int, maxRow: Int): Boolean {
        // find total number of free places in the rows [0; maxRow+1)
        var k = k
        val sum = total(0, 0, n, maxRow + 1)
        if (sum < k) {
            return false
        }
        var i = 0
        // to don't update parent for both of its children we use a queue
        val deque: Deque<Int> = ArrayDeque()
        while (k != 0) {
            i += numZerosRight[i] + 1
            var v = n - 1 + i - 1
            val spent = Math.min(k, max[v])
            k -= spent
            max[v] -= spent
            total[v] -= spent.toLong()
            if (max[v] == 0) {
                // update numZerosRight and numZerosLeft
                numZerosRight[i - numZerosLeft[i] - 1] += numZerosRight[i] + 1
                numZerosLeft[i + numZerosRight[i] + 1] += numZerosLeft[i] + 1
            }
            if (v != 0) {
                v = (v - 1) / 2
                // if we already have the parent node in the queue we don't need to update it
                if (deque.isEmpty() || deque.peekLast() != v) {
                    deque.addLast(v)
                }
            }
        }
        // update max and total
        while (deque.isNotEmpty()) {
            var v = deque.pollFirst()
            max[v] = Math.max(max[v * 2 + 1], max[v * 2 + 2])
            total[v] = total[v * 2 + 1] + total[v * 2 + 2]
            if (v != 0) {
                v = (v - 1) / 2
                // if we already have the parent node in the queue we don't need to update it
                if (deque.isEmpty() || deque.peekLast() != v) {
                    deque.addLast(v)
                }
            }
        }
        return true
    }

    // find sum of [ql, qr)
    private fun total(v: Int, l: Int, r: Int, qr: Int): Long {
        if (l >= qr) {
            return 0
        }
        if (r <= qr) {
            return total[v]
        }
        val mid = (l + r) / 2
        return total(v * 2 + 1, l, mid, qr) + total(v * 2 + 2, mid, r, qr)
    }

    companion object {
        private fun nextPow2(n: Int): Int {
            return if (n and n - 1 == 0) {
                n
            } else Integer.highestOneBit(n) shl 1
        }
    }
}
/*
 * Your BookMyShow object will be instantiated and called as such:
 * var obj = BookMyShow(n, m)
 * var param_1 = obj.gather(k,maxRow)
 * var param_2 = obj.scatter(k,maxRow)
 */
```