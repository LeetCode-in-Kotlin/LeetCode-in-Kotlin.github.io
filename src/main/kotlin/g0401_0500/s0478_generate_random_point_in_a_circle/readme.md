[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 478\. Generate Random Point in a Circle

Medium

Given the radius and the position of the center of a circle, implement the function `randPoint` which generates a uniform random point inside the circle.

Implement the `Solution` class:

*   `Solution(double radius, double x_center, double y_center)` initializes the object with the radius of the circle `radius` and the position of the center `(x_center, y_center)`.
*   `randPoint()` returns a random point inside the circle. A point on the circumference of the circle is considered to be in the circle. The answer is returned as an array `[x, y]`.

**Example 1:**

**Input** ["Solution", "randPoint", "randPoint", "randPoint"] [[1.0, 0.0, 0.0], [], [], []]

**Output:** [null, [-0.02493, -0.38077], [0.82314, 0.38945], [0.36572, 0.17248]]

**Explanation:** 

    Solution solution = new Solution(1.0, 0.0, 0.0); 
    solution.randPoint(); // return [-0.02493, -0.38077] 
    solution.randPoint(); // return [0.82314, 0.38945] 
    solution.randPoint(); // return [0.36572, 0.17248]

**Constraints:**

*   <code>0 < radius <= 10<sup>8</sup></code>
*   <code>-10<sup>7</sup> <= x_center, y_center <= 10<sup>7</sup></code>
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `randPoint`.

## Solution

```kotlin
import java.util.Random

@Suppress("kotlin:S2245")
class Solution(private val radius: Double, private val xCenter: Double, private val yCenter: Double) {
    private val random: Random = Random()
    fun randPoint(): DoubleArray {
        var x = getCoordinate(xCenter)
        var y = getCoordinate(yCenter)
        while (getDistance(x, y) >= radius * radius) {
            x = getCoordinate(xCenter)
            y = getCoordinate(yCenter)
        }
        return doubleArrayOf(x, y)
    }

    private fun getDistance(x: Double, y: Double): Double {
        return (xCenter - x) * (xCenter - x) + (yCenter - y) * (yCenter - y)
    }

    private fun getCoordinate(center: Double): Double {
        return center - radius + random.nextDouble() * 2 * radius
    }
}

/*
 * Your Solution object will be instantiated and called as such:
 * var obj = Solution(radius, x_center, y_center)
 * var param_1 = obj.randPoint()
 */
```