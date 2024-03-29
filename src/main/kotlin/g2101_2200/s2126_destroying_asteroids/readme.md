[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2126\. Destroying Asteroids

Medium

You are given an integer `mass`, which represents the original mass of a planet. You are further given an integer array `asteroids`, where `asteroids[i]` is the mass of the <code>i<sup>th</sup></code> asteroid.

You can arrange for the planet to collide with the asteroids in **any arbitrary order**. If the mass of the planet is **greater than or equal to** the mass of the asteroid, the asteroid is **destroyed** and the planet **gains** the mass of the asteroid. Otherwise, the planet is destroyed.

Return `true` _if **all** asteroids can be destroyed. Otherwise, return_ `false`_._

**Example 1:**

**Input:** mass = 10, asteroids = [3,9,19,5,21]

**Output:** true

**Explanation:** One way to order the asteroids is [9,19,5,3,21]: 

- The planet collides with the asteroid with a mass of 9. New planet mass: 10 + 9 = 19 

- The planet collides with the asteroid with a mass of 19. New planet mass: 19 + 19 = 38 

- The planet collides with the asteroid with a mass of 5. New planet mass: 38 + 5 = 43 

- The planet collides with the asteroid with a mass of 3. New planet mass: 43 + 3 = 46

- The planet collides with the asteroid with a mass of 21. New planet mass: 46 + 21 = 67 
  
All asteroids are destroyed.

**Example 2:**

**Input:** mass = 5, asteroids = [4,9,23,4]

**Output:** false

**Explanation:** The planet cannot ever gain enough mass to destroy the asteroid with a mass of 23. 

After the planet destroys the other asteroids, it will have a mass of 5 + 4 + 9 + 4 = 22. 

This is less than 23, so a collision would not destroy the last asteroid.

**Constraints:**

*   <code>1 <= mass <= 10<sup>5</sup></code>
*   <code>1 <= asteroids.length <= 10<sup>5</sup></code>
*   <code>1 <= asteroids[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun asteroidsDestroyed(mass: Int, asteroids: IntArray): Boolean {
        return helper(mass.toLong(), 0, asteroids)
    }

    private fun helper(mass: Long, startIndex: Int, asteroids: IntArray): Boolean {
        var mass = mass
        var smallOrEqualIndex = partition(mass, startIndex, asteroids)
        if (smallOrEqualIndex < startIndex) {
            return false
        }
        if (smallOrEqualIndex >= asteroids.size - 1) {
            return true
        }
        for (i in startIndex..smallOrEqualIndex) {
            mass += asteroids[i].toLong()
        }
        return helper(mass, ++smallOrEqualIndex, asteroids)
    }

    private fun partition(mass: Long, startIndex: Int, asteroids: IntArray): Int {
        val length = asteroids.size
        var smallOrEqualIndex = startIndex - 1
        for (i in startIndex until length) {
            if (asteroids[i] <= mass) {
                smallOrEqualIndex++
                swap(asteroids, i, smallOrEqualIndex)
            }
        }
        return smallOrEqualIndex
    }

    private fun swap(array: IntArray, i: Int, j: Int) {
        val tmp = array[i]
        array[i] = array[j]
        array[j] = tmp
    }
}
```