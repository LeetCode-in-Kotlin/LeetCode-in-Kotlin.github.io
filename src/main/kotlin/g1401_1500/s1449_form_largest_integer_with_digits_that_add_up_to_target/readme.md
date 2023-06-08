[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1449\. Form Largest Integer With Digits That Add up to Target

Hard

Given an array of integers `cost` and an integer `target`, return _the **maximum** integer you can paint under the following rules_:

*   The cost of painting a digit `(i + 1)` is given by `cost[i]` (**0-indexed**).
*   The total cost used must be equal to `target`.
*   The integer does not have `0` digits.

Since the answer may be very large, return it as a string. If there is no way to paint any integer given the condition, return `"0"`.

**Example 1:**

**Input:** cost = [4,3,2,5,6,7,2,5,5], target = 9

**Output:** "7772"

**Explanation:** The cost to paint the digit '7' is 2, and the digit '2' is 3. Then cost("7772") = 2\*3+ 3\*1 = 9. You could also paint "977", but "7772" is the largest number.

**Digit cost** 
  
1 -> 4 
    
2 -> 3 
    
3 -> 2 

4 -> 5 

5 -> 6 

6 -> 7 

7 -> 2 

8 -> 5 

9 -> 5

**Example 2:**

**Input:** cost = [7,6,5,5,5,6,8,7,8], target = 12

**Output:** "85"

**Explanation:** The cost to paint the digit '8' is 7, and the digit '5' is 5. Then cost("85") = 7 + 5 = 12.

**Example 3:**

**Input:** cost = [2,4,6,2,4,6,4,4,4], target = 5

**Output:** "0"

**Explanation:** It is impossible to paint any integer with total cost equal to target.

**Constraints:**

*   `cost.length == 9`
*   `1 <= cost[i], target <= 5000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun largestNumber(cost: IntArray, target: Int): String {
        var target = target
        val dp = Array(10) { IntArray(5001) }
        dp[0].fill(-1)
        for (i in 1..cost.size) {
            for (j in 1..target) {
                if (cost[i - 1] > j) {
                    dp[i][j] = dp[i - 1][j]
                } else {
                    var temp = if (dp[i - 1][j - cost[i - 1]] == -1) -1 else 1 + dp[i - 1][j - cost[i - 1]]
                    val t = if (dp[i][j - cost[i - 1]] == -1) -1 else 1 + dp[i][j - cost[i - 1]]
                    temp = if (t != -1 && temp == -1) {
                        t
                    } else {
                        Math.max(t, temp)
                    }
                    if (dp[i - 1][j] == -1) {
                        dp[i][j] = temp
                    } else if (temp == -1) {
                        dp[i][j] = dp[i - 1][j]
                    } else {
                        dp[i][j] = Math.max(temp, dp[i - 1][j])
                    }
                }
            }
        }
        if (dp[9][target] == -1) {
            return "0"
        }
        var i = 9
        val result = StringBuilder()
        while (target > 0) {
            if (target - cost[i - 1] >= 0 && dp[i][target - cost[i - 1]] + 1 == dp[i][target] ||
                (
                    target - cost[i - 1] >= 0 &&
                        dp[i - 1][target - cost[i - 1]] + 1 == dp[i][target]
                    )
            ) {
                result.append(i)
                target -= cost[i - 1]
            } else {
                i--
            }
        }
        return result.toString()
    }
}
```