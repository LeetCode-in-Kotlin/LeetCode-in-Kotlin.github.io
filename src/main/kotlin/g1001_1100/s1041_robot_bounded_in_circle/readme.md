[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1041\. Robot Bounded In Circle

Medium

On an infinite plane, a robot initially stands at `(0, 0)` and faces north. Note that:

*   The **north direction** is the positive direction of the y-axis.
*   The **south direction** is the negative direction of the y-axis.
*   The **east direction** is the positive direction of the x-axis.
*   The **west direction** is the negative direction of the x-axis.

The robot can receive one of three instructions:

*   `"G"`: go straight 1 unit.
*   `"L"`: turn 90 degrees to the left (i.e., anti-clockwise direction).
*   `"R"`: turn 90 degrees to the right (i.e., clockwise direction).

The robot performs the `instructions` given in order, and repeats them forever.

Return `true` if and only if there exists a circle in the plane such that the robot never leaves the circle.

**Example 1:**

**Input:** instructions = "GGLLGG"

**Output:** true

**Explanation:** The robot is initially at (0, 0) facing the north direction.

"G": move one step. Position: (0, 1). Direction: North.

"G": move one step. Position: (0, 2). Direction: North.

"L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: West.

"L": turn 90 degrees anti-clockwise. Position: (0, 2). Direction: South.

"G": move one step. Position: (0, 1). Direction: South.

"G": move one step. Position: (0, 0). Direction: South.

Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (0, 2) --> (0, 1) --> (0, 0).

Based on that, we return true.

**Example 2:**

**Input:** instructions = "GG"

**Output:** false

**Explanation:** The robot is initially at (0, 0) facing the north direction.

"G": move one step. Position: (0, 1). Direction: North.

"G": move one step. Position: (0, 2). Direction: North.

Repeating the instructions, keeps advancing in the north direction and does not go into cycles.

Based on that, we return false.

**Example 3:**

**Input:** instructions = "GL"

**Output:** true

**Explanation:** The robot is initially at (0, 0) facing the north direction.

"G": move one step. Position: (0, 1). Direction: North.

"L": turn 90 degrees anti-clockwise. Position: (0, 1). Direction: West.

"G": move one step. Position: (-1, 1). Direction: West.

"L": turn 90 degrees anti-clockwise. Position: (-1, 1). Direction: South.

"G": move one step. Position: (-1, 0). Direction: South.

"L": turn 90 degrees anti-clockwise. Position: (-1, 0). Direction: East.

"G": move one step. Position: (0, 0). Direction: East.

"L": turn 90 degrees anti-clockwise. Position: (0, 0). Direction: North.

Repeating the instructions, the robot goes into the cycle: (0, 0) --> (0, 1) --> (-1, 1) --> (-1, 0) --> (0, 0).

Based on that, we return true.

**Constraints:**

*   `1 <= instructions.length <= 100`
*   `instructions[i]` is `'G'`, `'L'` or, `'R'`.

## Solution

```kotlin
class Solution {
    fun isRobotBounded(instructions: String): Boolean {
        val dir = arrayOf(intArrayOf(0, 1), intArrayOf(-1, 0), intArrayOf(0, -1), intArrayOf(1, 0))
        var i = 0
        var x = 0
        var y = 0
        for (s in instructions.indices) {
            if (instructions[s] == 'L') {
                i = (i + 1) % 4
            } else if (instructions[s] == 'R') {
                i = (i + 3) % 4
            } else {
                x += dir[i][0]
                y += dir[i][1]
            }
        }
        return x == 0 && y == 0 || i != 0
    }
}
```