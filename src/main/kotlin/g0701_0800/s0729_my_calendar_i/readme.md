[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 729\. My Calendar I

Medium

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **double booking**.

A **double booking** happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

Implement the `MyCalendar` class:

*   `MyCalendar()` Initializes the calendar object.
*   `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a **double booking**. Otherwise, return `false` and do not add the event to the calendar.

**Example 1:**

**Input**

["MyCalendar", "book", "book", "book"]

[[], [10, 20], [15, 25], [20, 30]]

**Output:** [null, true, false, true]

**Explanation:**

    MyCalendar myCalendar = new MyCalendar(); 
    myCalendar.book(10, 20); // return True 
    myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event. 
    myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.

**Constraints:**

*   <code>0 <= start < end <= 10<sup>9</sup></code>
*   At most `1000` calls will be made to `book`.

## Solution

```kotlin
import java.util.TreeSet


class MyCalendar {
    internal class Meeting(val start: Int, val end: Int) : Comparable<Meeting?> {
        override operator fun compareTo(other: Meeting?): Int {
            return start - other!!.start
        }
    }

    private val meetings: TreeSet<Meeting> = TreeSet()

    fun book(start: Int, end: Int): Boolean {
        val meetingToBook = Meeting(start, end)
        val prevMeeting = meetings.floor(meetingToBook)
        val nextMeeting = meetings.ceiling(meetingToBook)
        if ((prevMeeting == null || prevMeeting.end <= meetingToBook.start) &&
            (nextMeeting == null || meetingToBook.end <= nextMeeting.start)
        ) {
            meetings.add(meetingToBook)
            return true
        }
        return false
    }
}

/*
 * Your MyCalendar object will be instantiated and called as such:
 * var obj = MyCalendar()
 * var param_1 = obj.book(start,end)
 */
```