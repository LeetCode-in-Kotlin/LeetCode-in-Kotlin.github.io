[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1603\. Design Parking System

Easy

Design a parking system for a parking lot. The parking lot has three kinds of parking spaces: big, medium, and small, with a fixed number of slots for each size.

Implement the `ParkingSystem` class:

*   `ParkingSystem(int big, int medium, int small)` Initializes object of the `ParkingSystem` class. The number of slots for each parking space are given as part of the constructor.
*   `bool addCar(int carType)` Checks whether there is a parking space of `carType` for the car that wants to get into the parking lot. `carType` can be of three kinds: big, medium, or small, which are represented by `1`, `2`, and `3` respectively. **A car can only park in a parking space of its** `carType`. If there is no space available, return `false`, else park the car in that size space and return `true`.

**Example 1:**

**Input** ["ParkingSystem", "addCar", "addCar", "addCar", "addCar"] [[1, 1, 0], [1], [2], [3], [1]]

**Output:** [null, true, true, false, false]

**Explanation:** 

ParkingSystem parkingSystem = new ParkingSystem(1, 1, 0);

parkingSystem.addCar(1); // return true because there is 1 available slot for a big car

parkingSystem.addCar(2); // return true because there is 1 available slot for a medium car 

parkingSystem.addCar(3); // return false because there is no available slot for a small car 

parkingSystem.addCar(1); // return false because there is no available slot for a big car. It is already occupied.

**Constraints:**

*   `0 <= big, medium, small <= 1000`
*   `carType` is `1`, `2`, or `3`
*   At most `1000` calls will be made to `addCar`

## Solution

```kotlin
class ParkingSystem(big: Int, medium: Int, small: Int) {
    private val slots = IntArray(3)

    init {
        slots[0] = big
        slots[1] = medium
        slots[2] = small
    }

    fun addCar(carType: Int): Boolean {
        return if (carType == 1) {
            if (slots[0] > 0) {
                slots[0]--
                true
            } else {
                false
            }
        } else if (carType == 2) {
            if (slots[1] > 0) {
                slots[1]--
                true
            } else {
                false
            }
        } else {
            if (slots[2] > 0) {
                slots[2]--
                true
            } else {
                false
            }
        }
    }
}
/*
 * Your ParkingSystem object will be instantiated and called as such:
 * var obj = ParkingSystem(big, medium, small)
 * var param_1 = obj.addCar(carType)
 */
```