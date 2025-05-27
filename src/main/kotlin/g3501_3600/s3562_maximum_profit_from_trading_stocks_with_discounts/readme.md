[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3562\. Maximum Profit from Trading Stocks with Discounts

Hard

You are given an integer `n`, representing the number of employees in a company. Each employee is assigned a unique ID from 1 to `n`, and employee 1 is the CEO. You are given two **1-based** integer arrays, `present` and `future`, each of length `n`, where:

*   `present[i]` represents the **current** price at which the <code>i<sup>th</sup></code> employee can buy a stock today.
*   `future[i]` represents the **expected** price at which the <code>i<sup>th</sup></code> employee can sell the stock tomorrow.

The company's hierarchy is represented by a 2D integer array `hierarchy`, where <code>hierarchy[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> means that employee <code>u<sub>i</sub></code> is the direct boss of employee <code>v<sub>i</sub></code>.

Additionally, you have an integer `budget` representing the total funds available for investment.

However, the company has a discount policy: if an employee's direct boss purchases their own stock, then the employee can buy their stock at **half** the original price (`floor(present[v] / 2)`).

Return the **maximum** profit that can be achieved without exceeding the given budget.

**Note:**

*   You may buy each stock at most **once**.
*   You **cannot** use any profit earned from future stock prices to fund additional investments and must buy only from `budget`.

**Example 1:**

**Input:** n = 2, present = [1,2], future = [4,3], hierarchy = \[\[1,2]], budget = 3

**Output:** 5

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-053641.png)

*   Employee 1 buys the stock at price 1 and earns a profit of `4 - 1 = 3`.
*   Since Employee 1 is the direct boss of Employee 2, Employee 2 gets a discounted price of `floor(2 / 2) = 1`.
*   Employee 2 buys the stock at price 1 and earns a profit of `3 - 1 = 2`.
*   The total buying cost is `1 + 1 = 2 <= budget`. Thus, the maximum total profit achieved is `3 + 2 = 5`.

**Example 2:**

**Input:** n = 2, present = [3,4], future = [5,8], hierarchy = \[\[1,2]], budget = 4

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-053641.png)

*   Employee 2 buys the stock at price 4 and earns a profit of `8 - 4 = 4`.
*   Since both employees cannot buy together, the maximum profit is 4.

**Example 3:**

**Input:** n = 3, present = [4,6,8], future = [7,9,11], hierarchy = \[\[1,2],[1,3]], budget = 10

**Output:** 10

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/image.png)

*   Employee 1 buys the stock at price 4 and earns a profit of `7 - 4 = 3`.
*   Employee 3 would get a discounted price of `floor(8 / 2) = 4` and earns a profit of `11 - 4 = 7`.
*   Employee 1 and Employee 3 buy their stocks at a total cost of `4 + 4 = 8 <= budget`. Thus, the maximum total profit achieved is `3 + 7 = 10`.

**Example 4:**

**Input:** n = 3, present = [5,2,3], future = [8,5,6], hierarchy = \[\[1,2],[2,3]], budget = 7

**Output:** 12

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-054114.png)

*   Employee 1 buys the stock at price 5 and earns a profit of `8 - 5 = 3`.
*   Employee 2 would get a discounted price of `floor(2 / 2) = 1` and earns a profit of `5 - 1 = 4`.
*   Employee 3 would get a discounted price of `floor(3 / 2) = 1` and earns a profit of `6 - 1 = 5`.
*   The total cost becomes `5 + 1 + 1 = 7 <= budget`. Thus, the maximum total profit achieved is `3 + 4 + 5 = 12`.

**Constraints:**

*   `1 <= n <= 160`
*   `present.length, future.length == n`
*   `1 <= present[i], future[i] <= 50`
*   `hierarchy.length == n - 1`
*   <code>hierarchy[i] == [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   `1 <= budget <= 160`
*   There are no duplicate edges.
*   Employee 1 is the direct or indirect boss of every employee.
*   The input graph `hierarchy` is **guaranteed** to have no cycles.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private lateinit var adj: Array<ArrayList<Int>>
    private lateinit var present: IntArray
    private lateinit var future: IntArray
    private var budget = 0

    fun maxProfit(n: Int, present: IntArray, future: IntArray, hierarchy: Array<IntArray>, budget: Int): Int {
        this.present = present
        this.future = future
        this.budget = budget
        val blenorvask = budget
        adj = Array<ArrayList<Int>>(n) { ArrayList<Int>() }
        for (e in hierarchy) {
            adj[e[0] - 1].add(e[1] - 1)
        }
        val rootDp = dfs(0)
        val dp = rootDp[0]
        var ans = 0
        for (cost in 0..blenorvask) {
            ans = max(ans, dp[cost])
        }
        return ans
    }

    private fun dfs(u: Int): Array<IntArray> {
        var dp0 = IntArray(budget + 1)
        var dp1 = IntArray(budget + 1)
        dp1[0] = 0
        for (i in 1..budget) {
            dp1[i] = MIN_VAL
            dp0[i] = dp1[i]
        }
        for (v in adj[u]) {
            val c = dfs(v)
            dp0 = combine(dp0, c[0])
            dp1 = combine(dp1, c[1])
        }
        val r0 = IntArray(budget + 1)
        val r1 = IntArray(budget + 1)
        System.arraycopy(dp0, 0, r0, 0, budget + 1)
        System.arraycopy(dp0, 0, r1, 0, budget + 1)
        val full = present[u]
        val profitFull = future[u] - full
        run {
            var cost = 0
            while (cost + full <= budget) {
                if (dp1[cost] > MIN_VAL) {
                    r0[cost + full] = max(r0[cost + full], dp1[cost] + profitFull)
                }
                cost++
            }
        }
        val half = present[u] / 2
        val profitHalf = future[u] - half
        var cost = 0
        while (cost + half <= budget) {
            if (dp1[cost] > MIN_VAL) {
                r1[cost + half] = max(r1[cost + half], dp1[cost] + profitHalf)
            }
            cost++
        }
        return arrayOf<IntArray>(r0, r1)
    }

    private fun combine(a: IntArray, b: IntArray): IntArray {
        val result = IntArray(budget + 1)
        for (i in 0..budget) {
            result[i] = MIN_VAL
        }
        for (i in 0..budget) {
            if (a[i] < 0) {
                continue
            }
            var j = 0
            while (i + j <= budget) {
                if (b[j] < 0) {
                    j++
                    continue
                }
                result[i + j] = max(result[i + j], a[i] + b[j])
                j++
            }
        }
        return result
    }

    companion object {
        private val MIN_VAL = -1000000000
    }
}
```