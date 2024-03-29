[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1488\. Avoid Flood in The City

Medium

Your country has an infinite number of lakes. Initially, all the lakes are empty, but when it rains over the `nth` lake, the `nth` lake becomes full of water. If it rains over a lake which is **full of water**, there will be a **flood**. Your goal is to avoid the flood in any lake.

Given an integer array `rains` where:

*   `rains[i] > 0` means there will be rains over the `rains[i]` lake.
*   `rains[i] == 0` means there are no rains this day and you can choose **one lake** this day and **dry it**.

Return _an array `ans`_ where:

*   `ans.length == rains.length`
*   `ans[i] == -1` if `rains[i] > 0`.
*   `ans[i]` is the lake you choose to dry in the `ith` day if `rains[i] == 0`.

If there are multiple valid answers return **any** of them. If it is impossible to avoid flood return **an empty array**.

Notice that if you chose to dry a full lake, it becomes empty, but if you chose to dry an empty lake, nothing changes. (see example 4)

**Example 1:**

**Input:** rains = [1,2,3,4]

**Output:** [-1,-1,-1,-1]

**Explanation:** After the first day full lakes are [1]

After the second day full lakes are [1,2]

After the third day full lakes are [1,2,3]

After the fourth day full lakes are [1,2,3,4]

There's no day to dry any lake and there is no flood in any lake.

**Example 2:**

**Input:** rains = [1,2,0,0,2,1]

**Output:** [-1,-1,2,1,-1,-1]

**Explanation:** After the first day full lakes are [1]

After the second day full lakes are [1,2]

After the third day, we dry lake 2. Full lakes are [1]

After the fourth day, we dry lake 1. There is no full lakes.

After the fifth day, full lakes are [2].

After the sixth day, full lakes are [1,2].

It is easy that this scenario is flood-free. [-1,-1,1,2,-1,-1] is another acceptable scenario.

**Example 3:**

**Input:** rains = [1,2,0,1,2]

**Output:** []

**Explanation:** After the second day, full lakes are [1,2]. We have to dry one lake in the third day.

After that, it will rain over lakes [1,2]. It's easy to prove that no matter which lake you choose to dry in the 3rd day, the other one will flood.

**Constraints:**

*   <code>1 <= rains.length <= 10<sup>5</sup></code>
*   <code>0 <= rains[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.TreeSet

class Solution {
    fun avoidFlood(rains: IntArray): IntArray {
        val hm = HashMap<Int, Int>()
        val tree = TreeSet<Int>()
        val ans = IntArray(rains.size)
        var i = 0
        while (i < rains.size) {
            val rain = rains[i]
            if (rain != 0) {
                if (hm.containsKey(rain)) {
                    val mapVal = hm[rain]!!
                    if (tree.ceiling(mapVal) != null) {
                        ans[tree.ceiling(mapVal)] = rain
                        hm[rain] = i
                        tree.remove(tree.ceiling(mapVal))
                    } else {
                        return IntArray(0)
                    }
                } else {
                    hm[rain] = i
                }
                ans[i] = -1
            } else {
                tree.add(i)
            }
            i += 1
        }
        for (tr in tree) {
            ans[tr] = 1
        }
        return ans
    }
}
```