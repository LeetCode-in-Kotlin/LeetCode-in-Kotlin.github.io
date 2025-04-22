[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 770\. Basic Calculator IV

Hard

Given an expression such as `expression = "e + 8 - a + 5"` and an evaluation map such as `{"e": 1}` (given in terms of `evalvars = ["e"]` and `evalints = [1]`), return a list of tokens representing the simplified expression, such as `["-1*a","14"]`

*   An expression alternates chunks and symbols, with a space separating each chunk and symbol.
*   A chunk is either an expression in parentheses, a variable, or a non-negative integer.
*   A variable is a string of lowercase letters (not including digits.) Note that variables can be multiple letters, and note that variables never have a leading coefficient or unary operator like `"2x"` or `"-x"`.

Expressions are evaluated in the usual order: brackets first, then multiplication, then addition and subtraction.

*   For example, `expression = "1 + 2 * 3"` has an answer of `["7"]`.

The format of the output is as follows:

*   For each term of free variables with a non-zero coefficient, we write the free variables within a term in sorted order lexicographically.
    *   For example, we would never write a term like `"b*a*c"`, only `"a*b*c"`.
*   Terms have degrees equal to the number of free variables being multiplied, counting multiplicity. We write the largest degree terms of our answer first, breaking ties by lexicographic order ignoring the leading coefficient of the term.
    *   For example, `"a*a*b*c"` has degree `4`.
*   The leading coefficient of the term is placed directly to the left with an asterisk separating it from the variables (if they exist.) A leading coefficient of 1 is still printed.
*   An example of a well-formatted answer is `["-2*a*a*a", "3*a*a*b", "3*b*b", "4*a", "5*c", "-6"]`.
*   Terms (including constant terms) with coefficient `0` are not included.
    *   For example, an expression of `"0"` has an output of `[]`.

**Note:** You may assume that the given expression is always valid. All intermediate results will be in the range of <code>[-2<sup>31</sup>, 2<sup>31</sup> - 1]</code>.

**Example 1:**

**Input:** expression = "e + 8 - a + 5", evalvars = ["e"], evalints = [1]

**Output:** ["-1\*a","14"]

**Example 2:**

**Input:** expression = "e - 8 + temperature - pressure", evalvars = ["e", "temperature"], evalints = [1, 12]

**Output:** ["-1\*pressure","5"]

**Example 3:**

**Input:** expression = "(e + 8) \* (e - 8)", evalvars = [], evalints = []

**Output:** ["1\*e\*e","-64"]

**Constraints:**

*   `1 <= expression.length <= 250`
*   `expression` consists of lowercase English letters, digits, `'+'`, `'-'`, `'*'`, `'('`, `')'`, `' '`.
*   `expression` does not contain any leading or trailing spaces.
*   All the tokens in `expression` are separated by a single space.
*   `0 <= evalvars.length <= 100`
*   `1 <= evalvars[i].length <= 20`
*   `evalvars[i]` consists of lowercase English letters.
*   `evalints.length == evalvars.length`
*   `-100 <= evalints[i] <= 100`

## Solution

```kotlin
import java.util.Collections

class Solution {
    internal inner class Node {
        var mem: MutableMap<List<String>, Int> = HashMap()
        fun update(cur: List<String>, cnt: Int) {
            Collections.sort(cur)
            mem[cur] = mem.getOrDefault(cur, 0) + cnt
        }

        fun add(cur: Node): Node {
            val ans = Node()
            for (key1 in mem.keys) {
                ans.update(key1, mem[key1]!!)
            }
            for (key2 in cur.mem.keys) {
                ans.update(key2, cur.mem[key2]!!)
            }
            return ans
        }

        fun sub(cur: Node): Node {
            val ans = Node()
            for (key1 in mem.keys) {
                ans.update(key1, mem[key1]!!)
            }
            for (key2 in cur.mem.keys) {
                ans.update(key2, -cur.mem[key2]!!)
            }
            return ans
        }

        fun mul(cur: Node): Node {
            val ans = Node()
            for (key1 in mem.keys) {
                for (key2 in cur.mem.keys) {
                    val next: MutableList<String> = ArrayList()
                    next.addAll(key1)
                    next.addAll(key2)
                    ans.update(next, mem[key1]!! * cur.mem[key2]!!)
                }
            }
            return ans
        }

        fun evaluate(vars: Map<String, Int>): Node {
            val ans = Node()
            for (cur in mem.keys) {
                var cnt = mem[cur]!!
                val free: MutableList<String> = ArrayList()
                for (tmp in cur) {
                    if (vars.containsKey(tmp)) {
                        cnt *= vars[tmp]!!
                    } else {
                        free.add(tmp)
                    }
                }
                ans.update(free, cnt)
            }
            return ans
        }

        fun toList(): List<String> {
            val ans: MutableList<String> = ArrayList()
            val keys: List<List<String>> = ArrayList(mem.keys)
            Collections.sort(
                keys,
            ) { a: List<String>, b: List<String> ->
                if (a.size != b.size) {
                    return@sort b.size - a.size
                }
                for (i in a.indices) {
                    if (a[i].compareTo(b[i]) != 0) {
                        return@sort a[i].compareTo(b[i])
                    }
                }
                0
            }
            for (key in keys) {
                if (mem[key] == 0) {
                    continue
                }
                var cur = "" + mem[key].toString()
                for (data in key) {
                    cur += "*"
                    cur += data
                }
                ans.add(cur)
            }
            return ans
        }
    }

    private fun make(cur: String): Node {
        val ans = Node()
        val tmp: MutableList<String> = ArrayList()
        if (Character.isDigit(cur[0])) {
            ans.update(tmp, Integer.valueOf(cur))
        } else {
            tmp.add(cur)
            ans.update(tmp, 1)
        }
        return ans
    }

    private fun getNext(expression: String, start: Int): Int {
        var end = start
        while (end < expression.length && Character.isLetterOrDigit(expression[end])) {
            end++
        }
        return end - 1
    }

    private fun getPriority(a: Char): Int {
        if (a == '+' || a == '-') {
            return 1
        } else if (a == '*') {
            return 2
        }
        return 0
    }

    private fun helper(numS: ArrayDeque<Node>, ops: ArrayDeque<Char>): Node {
        val b = numS.removeLast()
        val a = numS.removeLast()
        val op = ops.removeLast()
        if (op == '*') {
            return a.mul(b)
        } else if (op == '+') {
            return a.add(b)
        }
        return a.sub(b)
    }

    fun basicCalculatorIV(expression: String, evalvars: Array<String>, evalints: IntArray): List<String> {
        val ans: List<String> = ArrayList()
        if (expression.isEmpty()) {
            return ans
        }
        val vars: MutableMap<String, Int> = HashMap()
        for (i in evalvars.indices) {
            vars[evalvars[i]] = evalints[i]
        }
        val n = expression.length
        val numS = ArrayDeque<Node>()
        val ops = ArrayDeque<Char>()
        var i = 0
        while (i < n) {
            val a = expression[i]
            if (Character.isLetterOrDigit(a)) {
                val end = getNext(expression, i)
                val cur = expression.substring(i, end + 1)
                i = end
                val now = make(cur)
                numS.add(now)
            } else if (a == '(') {
                ops.add(a)
            } else if (a == ')') {
                while (ops.last() != '(') {
                    numS.add(helper(numS, ops))
                }
                ops.removeLast()
            } else if (a == '+' || a == '-' || a == '*') {
                while (ops.isNotEmpty() && getPriority(ops.last()) >= getPriority(a)) {
                    numS.add(helper(numS, ops))
                }
                ops.add(a)
            }
            i++
        }
        while (ops.isNotEmpty()) {
            numS.add(helper(numS, ops))
        }
        return numS.last().evaluate(vars).toList()
    }
}
```