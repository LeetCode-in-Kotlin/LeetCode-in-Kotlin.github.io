[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1115\. Print FooBar Alternately

Medium

Suppose you are given the following code:

class FooBar { public void foo() { for (int i = 0; i < n; i++) { print("foo"); } } public void bar() { for (int i = 0; i < n; i++) { print("bar"); } } }

The same instance of `FooBar` will be passed to two different threads:

*   thread `A` will call `foo()`, while
*   thread `B` will call `bar()`.

Modify the given program to output `"foobar"` `n` times.

**Example 1:**

**Input:** n = 1

**Output:** "foobar"

**Explanation:** There are two threads being fired asynchronously. One of them calls foo(), while the other calls bar(). "foobar" is being output 1 time.

**Example 2:**

**Input:** n = 2

**Output:** "foobarfoobar"

**Explanation:** "foobar" is being output 2 times.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
import java.util.concurrent.Semaphore

class FooBar(private val n: Int) {
    private val fooSemaphore: Semaphore
    private val barSemaphore: Semaphore

    init {
        fooSemaphore = Semaphore(1)
        barSemaphore = Semaphore(1)
        try {
            barSemaphore.acquire()
        } catch (ignored: InterruptedException) {
            // ignored
        }
    }

    @Throws(InterruptedException::class)
    fun foo(printFoo: Runnable) {
        for (i in 0 until n) {
            fooSemaphore.acquire()
            // printFoo.run() outputs "foo". Do not change or remove this line.
            printFoo.run()
            barSemaphore.release()
        }
    }

    @Throws(InterruptedException::class)
    fun bar(printBar: Runnable) {
        for (i in 0 until n) {
            barSemaphore.acquire()
            // printBar.run() outputs "bar". Do not change or remove this line.
            printBar.run()
            fooSemaphore.release()
        }
    }
}
```