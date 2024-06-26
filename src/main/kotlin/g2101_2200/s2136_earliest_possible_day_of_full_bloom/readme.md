[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2136\. Earliest Possible Day of Full Bloom

Hard

You have `n` flower seeds. Every seed must be planted first before it can begin to grow, then bloom. Planting a seed takes time and so does the growth of a seed. You are given two **0-indexed** integer arrays `plantTime` and `growTime`, of length `n` each:

*   `plantTime[i]` is the number of **full days** it takes you to **plant** the <code>i<sup>th</sup></code> seed. Every day, you can work on planting exactly one seed. You **do not** have to work on planting the same seed on consecutive days, but the planting of a seed is not complete **until** you have worked `plantTime[i]` days on planting it in total.
*   `growTime[i]` is the number of **full days** it takes the <code>i<sup>th</sup></code> seed to grow after being completely planted. **After** the last day of its growth, the flower **blooms** and stays bloomed forever.

From the beginning of day `0`, you can plant the seeds in **any** order.

Return _the **earliest** possible day where **all** seeds are blooming_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/21/1.png)

**Input:** plantTime = [1,4,3], growTime = [2,3,1]

**Output:** 9

**Explanation:** The grayed out pots represent planting days, colored pots represent growing days, and the flower represents the day it blooms. 

One optimal way is: 

On day 0, plant the 0<sup>th</sup> seed. The seed grows for 2 full days and blooms on day 3. 

On days 1, 2, 3, and 4, plant the 1<sup>st</sup> seed. The seed grows for 3 full days and blooms on day 8.

On days 5, 6, and 7, plant the 2<sup>nd</sup> seed. The seed grows for 1 full day and blooms on day 9. 

Thus, on day 9, all the seeds are blooming.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/21/2.png)

**Input:** plantTime = [1,2,3,2], growTime = [2,1,2,1]

**Output:** 9

**Explanation:** The grayed out pots represent planting days, colored pots represent growing days, and the flower represents the day it blooms. 

One optimal way is: 

On day 1, plant the 0<sup>th</sup> seed. The seed grows for 2 full days and blooms on day 4. 

On days 0 and 3, plant the 1<sup>st</sup> seed. The seed grows for 1 full day and blooms on day 5. 

On days 2, 4, and 5, plant the 2<sup>nd</sup> seed. The seed grows for 2 full days and blooms on day 8. 

On days 6 and 7, plant the 3<sup>rd</sup> seed. The seed grows for 1 full day and blooms on day 9. 

Thus, on day 9, all the seeds are blooming.

**Example 3:**

**Input:** plantTime = [1], growTime = [1]

**Output:** 2

**Explanation:** On day 0, plant the 0<sup>th</sup> seed. The seed grows for 1 full day and blooms on day 2. 

Thus, on day 2, all the seeds are blooming.

**Constraints:**

*   `n == plantTime.length == growTime.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= plantTime[i], growTime[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import java.util.Collections

class Solution {
    fun earliestFullBloom(plantTime: IntArray, growTime: IntArray): Int {
        val n = plantTime.size
        if (n == 1) {
            return plantTime[0] + growTime[0]
        }
        val arr = arrayOfNulls<Seed>(n)
        for (i in 0 until n) {
            arr[i] = Seed(plantTime[i], growTime[i])
        }
        arr.sortWith(Collections.reverseOrder())
        var ans = arr[0]!!.plantTime + arr[0]!!.growTime
        var lastPlantDay = arr[0]!!.plantTime
        for (i in 1 until n) {
            val currBloomDay = lastPlantDay + arr[i]!!.plantTime + arr[i]!!.growTime
            ans = Math.max(ans, currBloomDay)
            lastPlantDay += arr[i]!!.plantTime
        }
        return ans
    }

    internal class Seed(var plantTime: Int, var growTime: Int) : Comparable<Seed> {
        override fun compareTo(other: Seed): Int {
            return growTime - other.growTime
        }
    }
}
```