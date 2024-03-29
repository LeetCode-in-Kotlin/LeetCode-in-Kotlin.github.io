[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2167\. Minimum Time to Remove All Cars Containing Illegal Goods

Hard

You are given a **0-indexed** binary string `s` which represents a sequence of train cars. `s[i] = '0'` denotes that the <code>i<sup>th</sup></code> car does **not** contain illegal goods and `s[i] = '1'` denotes that the <code>i<sup>th</sup></code> car does contain illegal goods.

As the train conductor, you would like to get rid of all the cars containing illegal goods. You can do any of the following three operations **any** number of times:

1.  Remove a train car from the **left** end (i.e., remove `s[0]`) which takes 1 unit of time.
2.  Remove a train car from the **right** end (i.e., remove `s[s.length - 1]`) which takes 1 unit of time.
3.  Remove a train car from **anywhere** in the sequence which takes 2 units of time.

Return _the **minimum** time to remove all the cars containing illegal goods_.

Note that an empty sequence of cars is considered to have no cars containing illegal goods.

**Example 1:**

**Input:** s = "**11**00**1**0**1**"

**Output:** 5

**Explanation:** 

One way to remove all the cars containing illegal goods from the sequence is to 

- remove a car from the left end 2 times. Time taken is 2 \* 1 = 2. 

- remove a car from the right end. Time taken is 1. 

- remove the car containing illegal goods found in the middle. Time taken is 2. This obtains a total time of 2 + 1 + 2 = 5. An alternative way is to 

- remove a car from the left end 2 times. Time taken is 2 \* 1 = 2. 

- remove a car from the right end 3 times. Time taken is 3 \* 1 = 3. This also obtains a total time of 2 + 3 = 5. 
  
5 is the minimum time taken to remove all the cars containing illegal goods. There are no other ways to remove them with less time.

**Example 2:**

**Input:** s = "00**1**0"

**Output:** 2

**Explanation:** One way to remove all the cars containing illegal goods from the sequence is to 

- remove a car from the left end 3 times. Time taken is 3 \* 1 = 3. 
  
This obtains a total time of 3.

Another way to remove all the cars containing illegal goods from the sequence is to 

- remove the car containing illegal goods found in the middle. Time taken is 2. This obtains a total time of 2. 
  
Another way to remove all the cars containing illegal goods from the sequence is to 

- remove a car from the right end 2 times. Time taken is 2 \* 1 = 2. 
  
This obtains a total time of 2. 

2 is the minimum time taken to remove all the cars containing illegal goods. 

There are no other ways to remove them with less time.

**Constraints:**

*   <code>1 <= s.length <= 2 * 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```kotlin
class Solution {
    fun minimumTime(s: String): Int {
        val n = s.length
        val sum = IntArray(n + 1)
        for (i in 0 until n) {
            sum[i + 1] = sum[i] + (s[i].code - '0'.code)
        }
        if (sum[n] == 0) {
            return 0
        }
        var res = s.length
        var min = Int.MAX_VALUE
        for (end in 0 until n) {
            min = Math.min(min, end - 2 * sum[end] + n - 1)
            res = Math.min(res, min + 2 * sum[end + 1] - end)
        }
        return res
    }
}
```