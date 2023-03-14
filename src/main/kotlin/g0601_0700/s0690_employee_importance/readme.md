[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 690\. Employee Importance

Medium

You have a data structure of employee information, including the employee's unique ID, importance value, and direct subordinates' IDs.

You are given an array of employees `employees` where:

*   `employees[i].id` is the ID of the <code>i<sup>th</sup></code> employee.
*   `employees[i].importance` is the importance value of the <code>i<sup>th</sup></code> employee.
*   `employees[i].subordinates` is a list of the IDs of the direct subordinates of the <code>i<sup>th</sup></code> employee.

Given an integer `id` that represents an employee's ID, return _the **total** importance value of this employee and all their direct and indirect subordinates_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/31/emp1-tree.jpg)

**Input:** employees = \[\[1,5,[2,3]],[2,3,[]],[3,3,[]]], id = 1

**Output:** 11

**Explanation:** Employee 1 has an importance value of 5 and has two direct subordinates: employee 2 and employee 3. They both have an importance value of 3. Thus, the total importance value of employee 1 is 5 + 3 + 3 = 11.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/31/emp2-tree.jpg)

**Input:** employees = \[\[1,2,[5]],[5,-3,[]]], id = 5

**Output:** -3

**Explanation:** Employee 5 has an importance value of -3 and has no direct subordinates. Thus, the total importance value of employee 5 is -3.

**Constraints:**

*   `1 <= employees.length <= 2000`
*   `1 <= employees[i].id <= 2000`
*   All `employees[i].id` are **unique**.
*   `-100 <= employees[i].importance <= 100`
*   One employee has at most one direct leader and may have several subordinates.
*   The IDs in `employees[i].subordinates` are valid IDs.

## Solution

```kotlin
import com_github_leetcode.Employee

/*
 *	// Definition for Employee.
 *	class Employee {
 *		var id:Int = 0
 *		var importance:Int = 0
 *		var subordinates:List<Int> = listOf()
 *	}
 */
class Solution {
    fun getImportance(employees: List<Employee?>, id: Int): Int {
        val map: MutableMap<Int, Employee> = HashMap()
        for (emp in employees) {
            map[emp!!.id] = emp
        }
        return calculateImportance(id, map)
    }

    private fun calculateImportance(id: Int, map: Map<Int, Employee>): Int {
        val employee = map[id]
        var sum = employee!!.importance
        for (sub in employee.subordinates) {
            sum += calculateImportance(sub, map)
        }
        return sum
    }
}
```