[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1109\. Corporate Flight Bookings

Medium

There are `n` flights that are labeled from `1` to `n`.

You are given an array of flight bookings `bookings`, where <code>bookings[i] = [first<sub>i</sub>, last<sub>i</sub>, seats<sub>i</sub>]</code> represents a booking for flights <code>first<sub>i</sub></code> through <code>last<sub>i</sub></code> (**inclusive**) with <code>seats<sub>i</sub></code> seats reserved for **each flight** in the range.

Return _an array_ `answer` _of length_ `n`_, where_ `answer[i]` _is the total number of seats reserved for flight_ `i`.

**Example 1:**

**Input:** bookings = \[\[1,2,10],[2,3,20],[2,5,25]], n = 5

**Output:** [10,55,45,25,25]

**Explanation:** 

Flight labels: 1 2 3 4 5 

Booking 1 reserved: 10 10 

Booking 2 reserved: 20 20 

Booking 3 reserved: 25 25 25 25 

Total seats: 10 55 45 25 25 Hence, answer = [10,55,45,25,25]

**Example 2:**

**Input:** bookings = \[\[1,2,10],[2,2,15]], n = 2

**Output:** [10,25]

**Explanation:** 

Flight labels: 1 2 

Booking 1 reserved: 10 10 

Booking 2 reserved: 15 

Total seats: 10 25 Hence, answer = [10,25]

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   <code>1 <= bookings.length <= 2 * 10<sup>4</sup></code>
*   `bookings[i].length == 3`
*   <code>1 <= first<sub>i</sub> <= last<sub>i</sub> <= n</code>
*   <code>1 <= seats<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun corpFlightBookings(bookings: Array<IntArray>, n: Int): IntArray {
        val ret = IntArray(n)
        for (booking in bookings) {
            ret[booking[0] - 1] += booking[2]
            if (booking[1] < n) {
                ret[booking[1]] -= booking[2]
            }
        }
        for (i in 1 until n) {
            ret[i] += ret[i - 1]
        }
        return ret
    }
}
```