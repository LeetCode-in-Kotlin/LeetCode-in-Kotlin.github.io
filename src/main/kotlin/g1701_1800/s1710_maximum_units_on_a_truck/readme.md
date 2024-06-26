[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1710\. Maximum Units on a Truck

Easy

You are assigned to put some amount of boxes onto **one truck**. You are given a 2D array `boxTypes`, where <code>boxTypes[i] = [numberOfBoxes<sub>i</sub>, numberOfUnitsPerBox<sub>i</sub>]</code>:

*   <code>numberOfBoxes<sub>i</sub></code> is the number of boxes of type `i`.
*   <code>numberOfUnitsPerBox<sub>i</sub></code> is the number of units in each box of the type `i`.

You are also given an integer `truckSize`, which is the **maximum** number of **boxes** that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed `truckSize`.

Return _the **maximum** total number of **units** that can be put on the truck._

**Example 1:**

**Input:** boxTypes = \[\[1,3],[2,2],[3,1]], truckSize = 4

**Output:** 8

**Explanation:** There are: 

- 1 box of the first type that contains 3 units. 

- 2 boxes of the second type that contain 2 units each. 

- 3 boxes of the third type that contain 1 unit each. You can take all the boxes of the first and second types, and one box of the third type. The total number of units will be = (1 \* 3) + (2 \* 2) + (1 \* 1) = 8.

**Example 2:**

**Input:** boxTypes = \[\[5,10],[2,5],[4,7],[3,9]], truckSize = 10

**Output:** 91

**Constraints:**

*   `1 <= boxTypes.length <= 1000`
*   <code>1 <= numberOfBoxes<sub>i</sub>, numberOfUnitsPerBox<sub>i</sub> <= 1000</code>
*   <code>1 <= truckSize <= 10<sup>6</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun maximumUnits(boxTypes: Array<IntArray>, truckSize: Int): Int {
        var truckSize = truckSize
        boxTypes.sortWith { b1: IntArray, b2: IntArray -> Integer.compare(b2[1], b1[1]) }
        var maxUnits = 0
        var i = 0
        while (truckSize > 0 && i < boxTypes.size) {
            if (boxTypes[i][0] <= truckSize) {
                maxUnits += boxTypes[i][0] * boxTypes[i][1]
                truckSize -= boxTypes[i][0]
            } else {
                maxUnits += Math.min(truckSize, boxTypes[i][0]) * boxTypes[i][1]
                truckSize -= Math.min(truckSize, boxTypes[i][0])
            }
            i++
        }
        return maxUnits
    }
}
```