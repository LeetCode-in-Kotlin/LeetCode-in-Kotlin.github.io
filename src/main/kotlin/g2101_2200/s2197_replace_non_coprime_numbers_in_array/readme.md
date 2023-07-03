[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2197\. Replace Non-Coprime Numbers in Array

Hard

You are given an array of integers `nums`. Perform the following steps:

1.  Find **any** two **adjacent** numbers in `nums` that are **non-coprime**.
2.  If no such numbers are found, **stop** the process.
3.  Otherwise, delete the two numbers and **replace** them with their **LCM (Least Common Multiple)**.
4.  **Repeat** this process as long as you keep finding two adjacent non-coprime numbers.

Return _the **final** modified array._ It can be shown that replacing adjacent non-coprime numbers in **any** arbitrary order will lead to the same result.

The test cases are generated such that the values in the final array are **less than or equal** to <code>10<sup>8</sup></code>.

Two values `x` and `y` are **non-coprime** if `GCD(x, y) > 1` where `GCD(x, y)` is the **Greatest Common Divisor** of `x` and `y`.

**Example 1:**

**Input:** nums = [6,4,3,2,7,6,2]

**Output:** [12,7,6]

**Explanation:**

- (6, 4) are non-coprime with LCM(6, 4) = 12. Now, nums = [**12**,3,2,7,6,2].

- (12, 3) are non-coprime with LCM(12, 3) = 12. Now, nums = [**12**,2,7,6,2].

- (12, 2) are non-coprime with LCM(12, 2) = 12. Now, nums = [**12**,7,6,2].

- (6, 2) are non-coprime with LCM(6, 2) = 6. Now, nums = [12,7,**6**].

There are no more adjacent non-coprime numbers in nums.

Thus, the final modified array is [12,7,6].

Note that there are other ways to obtain the same resultant array. 

**Example 2:**

**Input:** nums = [2,2,1,1,3,3,3]

**Output:** [2,1,1,3]

**Explanation:**

- (3, 3) are non-coprime with LCM(3, 3) = 3. Now, nums = [2,2,1,1,**3**,3].

- (3, 3) are non-coprime with LCM(3, 3) = 3. Now, nums = [2,2,1,1,**3**].

- (2, 2) are non-coprime with LCM(2, 2) = 2. Now, nums = [**2**,1,1,3].

There are no more adjacent non-coprime numbers in nums.

Thus, the final modified array is [2,1,1,3].

Note that there are other ways to obtain the same resultant array. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   The test cases are generated such that the values in the final array are **less than or equal** to <code>10<sup>8</sup></code>.

## Solution

```kotlin
class Solution {
    fun replaceNonCoprimes(nums: IntArray): List<Int> {
        val res = ArrayList<Int>()
        var i = 1
        res.add(nums[0])
        while (i < nums.size) {
            val first = res[res.size - 1]
            val second = nums[i]
            val gcd = gcd(first, second)
            if (gcd > 1) {
                val lcm = first.toLong() * second.toLong() / gcd
                if (res.isNotEmpty()) {
                    res.removeAt(res.size - 1)
                }
                res.add(lcm.toInt())
                recursivelyCheck(res)
            } else {
                res.add(second)
            }
            i++
        }
        return res
    }

    private fun gcd(a: Int, b: Int): Int {
        if (a > b) {
            return gcd(b, a)
        }
        return if (b % a == 0) {
            a
        } else gcd(b % a, a)
    }

    private fun recursivelyCheck(list: ArrayList<Int>) {
        if (list.size < 2) {
            return
        }
        val a = list.removeAt(list.size - 1)
        val b = list.removeAt(list.size - 1)
        val gcd = gcd(a, b)
        if (gcd > 1) {
            val lcm = a.toLong() * b.toLong() / gcd
            list.add(lcm.toInt())
            recursivelyCheck(list)
        } else {
            list.add(b)
            list.add(a)
        }
    }
}
```