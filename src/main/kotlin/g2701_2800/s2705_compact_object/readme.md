[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2705\. Compact Object

Medium

Given an object or array `obj`, return a **compact object**. A **compact object** is the same as the original object, except with keys containing **falsy** values removed. This operation applies to the object and any nested objects. Arrays are considered objects where the indices are keys. A value is considered **falsy** when `Boolean(value)` returns `false`.

You may assume the `obj` is the output of `JSON.parse`. In other words, it is valid JSON.

**Example 1:**

**Input:** obj = [null, 0, false, 1]

**Output:** [1]

**Explanation:** All falsy values have been removed from the array.

**Example 2:**

**Input:** obj = {"a": null, "b": [false, 1]}

**Output:** {"b": [1]}

**Explanation:** obj["a"] and obj["b"][0] had falsy values and were removed.

**Example 3:**

**Input:** obj = [null, 0, 5, [0], [false, 16]]

**Output:** [5, [], [16]]

**Explanation:** obj[0], obj[1], obj[3][0], and obj[4][0] were falsy and removed.

**Constraints:**

*   `obj is a valid JSON object`
*   <code>2 <= JSON.stringify(obj).length <= 10<sup>6</sup></code>

## Solution

```typescript
type Obj = Record<any, any>

function compactObject(obj: Obj): Obj {
    if (Array.isArray(obj)) {
        let retArr = []
        obj.forEach((e, idx) => {
            if (e) {
                retArr.push(compactObject(e))
            }
        })
        return retArr
    } else if (obj !== null && typeof obj === 'object') {
        let retObj = {}
        for (const key of Object.keys(obj)) {
            if (obj[key]) {
                retObj[key] = compactObject(obj[key])
            }
        }
        return retObj
    }
    return obj
}

export { compactObject }
```