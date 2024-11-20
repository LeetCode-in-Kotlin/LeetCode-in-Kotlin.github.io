[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1298\. Maximum Candies You Can Get from Boxes

Hard

You have `n` boxes labeled from `0` to `n - 1`. You are given four arrays: `status`, `candies`, `keys`, and `containedBoxes` where:

*   `status[i]` is `1` if the <code>i<sup>th</sup></code> box is open and `0` if the <code>i<sup>th</sup></code> box is closed,
*   `candies[i]` is the number of candies in the <code>i<sup>th</sup></code> box,
*   `keys[i]` is a list of the labels of the boxes you can open after opening the <code>i<sup>th</sup></code> box.
*   `containedBoxes[i]` is a list of the boxes you found inside the <code>i<sup>th</sup></code> box.

You are given an integer array `initialBoxes` that contains the labels of the boxes you initially have. You can take all the candies in **any open box** and you can use the keys in it to open new boxes and you also can use the boxes you find in it.

Return _the maximum number of candies you can get following the rules above_.

**Example 1:**

**Input:** status = [1,0,1,0], candies = [7,5,4,100], keys = \[\[],[],[1],[]], containedBoxes = \[\[1,2],[3],[],[]], initialBoxes = [0]

**Output:** 16

**Explanation:** You will be initially given box 0. You will find 7 candies in it and boxes 1 and 2.

Box 1 is closed and you do not have a key for it so you will open box 2. You will find 4 candies and a key to box 1 in box 2.

In box 1, you will find 5 candies and box 3 but you will not find a key to box 3 so box 3 will remain closed.

Total number of candies collected = 7 + 4 + 5 = 16 candy.

**Example 2:**

**Input:** status = [1,0,0,0,0,0], candies = [1,1,1,1,1,1], keys = \[\[1,2,3,4,5],[],[],[],[],[]], containedBoxes = \[\[1,2,3,4,5],[],[],[],[],[]], initialBoxes = [0]

**Output:** 6

**Explanation:** You have initially box 0. Opening it you can find boxes 1,2,3,4 and 5 and their keys.

The total number of candies will be 6.

**Constraints:**

*   `n == status.length == candies.length == keys.length == containedBoxes.length`
*   `1 <= n <= 1000`
*   `status[i]` is either `0` or `1`.
*   `1 <= candies[i] <= 1000`
*   `0 <= keys[i].length <= n`
*   `0 <= keys[i][j] < n`
*   All values of `keys[i]` are **unique**.
*   `0 <= containedBoxes[i].length <= n`
*   `0 <= containedBoxes[i][j] < n`
*   All values of `containedBoxes[i]` are unique.
*   Each box is contained in one box at most.
*   `0 <= initialBoxes.length <= n`
*   `0 <= initialBoxes[i] < n`

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun maxCandies(
        status: IntArray,
        candies: IntArray,
        keys: Array<IntArray>,
        containedBoxes: Array<IntArray>,
        initialBoxes: IntArray,
    ): Int {
        var collectedCandies = 0
        val boxes: Queue<Int> = LinkedList()
        for (box in initialBoxes) {
            boxes.offer(box)
        }
        var unseen = 0
        while (boxes.isNotEmpty()) {
            if (unseen == boxes.size) {
                break
            }
            val curBox = boxes.poll()
            if (status[curBox] == 0) {
                unseen++
                boxes.offer(curBox)
            } else {
                unseen = 0
                // collect candies
                collectedCandies += candies[curBox]
                // open keys
                for (key in keys[curBox]) {
                    status[key] = 1
                }
                // collect contained boxes
                for (box in containedBoxes[curBox]) {
                    boxes.offer(box)
                }
            }
        }
        return collectedCandies
    }
}
```