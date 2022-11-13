[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 307\. Range Sum Query - Mutable

Medium

Given an integer array `nums`, handle multiple queries of the following types:

1.  **Update** the value of an element in `nums`.
2.  Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

*   `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
*   `void update(int index, int val)` **Updates** the value of `nums[index]` to be `val`.
*   `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**Example 1:**

**Input** ["NumArray", "sumRange", "update", "sumRange"] [[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]

**Output:** [null, 9, null, 8]

**Explanation:** NumArray numArray = new NumArray([1, 3, 5]); numArray.sumRange(0, 2); // return 1 + 3 + 5 = 9 numArray.update(1, 2); // nums = [1, 2, 5] numArray.sumRange(0, 2); // return 1 + 2 + 5 = 8

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   `-100 <= nums[i] <= 100`
*   `0 <= index < nums.length`
*   `-100 <= val <= 100`
*   `0 <= left <= right < nums.length`
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `update` and `sumRange`.

## Solution

```kotlin
class NumArray(private val nums: IntArray) {
    private var sum = 0

    init {
        for (num in nums) {
            sum += num
        }
    }

    fun update(index: Int, `val`: Int) {
        sum -= nums[index] - `val`
        nums[index] = `val`
    }

    fun sumRange(left: Int, right: Int): Int {
        var sumRange = 0
        if (right - left < nums.size / 2) {
            // Array to sum is less than half
            for (i in left..right) {
                sumRange += nums[i]
            }
        } else {
            // Array to sum is more than half
            // Better to take total sum and substract the numbers not in range
            sumRange = sum
            for (i in 0 until left) {
                sumRange -= nums[i]
            }
            for (i in right + 1 until nums.size) {
                sumRange -= nums[i]
            }
        }
        return sumRange
    }
}

/*
 * Your NumArray object will be instantiated and called as such:
 * var obj = NumArray(nums)
 * obj.update(index,`val`)
 * var param_2 = obj.sumRange(left,right)
 */
```