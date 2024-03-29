[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1103\. Distribute Candies to People

Easy

We distribute some number of `candies`, to a row of **`n = num_people`** people in the following way:

We then give 1 candy to the first person, 2 candies to the second person, and so on until we give `n` candies to the last person.

Then, we go back to the start of the row, giving `n + 1` candies to the first person, `n + 2` candies to the second person, and so on until we give `2 * n` candies to the last person.

This process repeats (with us giving one more candy each time, and moving to the start of the row after we reach the end) until we run out of candies. The last person will receive all of our remaining candies (not necessarily one more than the previous gift).

Return an array (of length `num_people` and sum `candies`) that represents the final distribution of candies.

**Example 1:**

**Input:** candies = 7, num\_people = 4

**Output:** [1,2,3,1]

**Explanation:** 

On the first turn, ans[0] += 1, and the array is [1,0,0,0]. 

On the second turn, ans[1] += 2, and the array is [1,2,0,0]. 

On the third turn, ans[2] += 3, and the array is [1,2,3,0]. 

On the fourth turn, ans[3] += 1 (because there is only one candy left), and the final array is [1,2,3,1].

**Example 2:**

**Input:** candies = 10, num\_people = 3

**Output:** [5,2,3]

**Explanation:**

On the first turn, ans[0] += 1, and the array is [1,0,0]. 

On the second turn, ans[1] += 2, and the array is [1,2,0]. 

On the third turn, ans[2] += 3, and the array is [1,2,3]. 

On the fourth turn, ans[0] += 4, and the final array is [5,2,3].

**Constraints:**

*   1 <= candies <= 10^9
*   1 <= num\_people <= 1000

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun distributeCandies(candies: Int, numPeople: Int): IntArray {
        var candies = candies
        val candiesDistribution = IntArray(numPeople)
        var count = 1
        while (candies > 0) {
            for (i in 0 until numPeople) {
                if (candies >= count) {
                    candiesDistribution[i] += count
                    candies -= count
                    count++
                } else if (candies > 0) {
                    candiesDistribution[i] += candies
                    candies -= candies
                }
                if (candies == 0) {
                    return candiesDistribution
                }
            }
        }
        return candiesDistribution
    }
}
```