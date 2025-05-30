[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2115\. Find All Possible Recipes from Given Supplies

Medium

You have information about `n` different recipes. You are given a string array `recipes` and a 2D string array `ingredients`. The <code>i<sup>th</sup></code> recipe has the name `recipes[i]`, and you can **create** it if you have **all** the needed ingredients from `ingredients[i]`. Ingredients to a recipe may need to be created from **other** recipes, i.e., `ingredients[i]` may contain a string that is in `recipes`.

You are also given a string array `supplies` containing all the ingredients that you initially have, and you have an infinite supply of all of them.

Return _a list of all the recipes that you can create._ You may return the answer in **any order**.

Note that two recipes may contain each other in their ingredients.

**Example 1:**

**Input:** recipes = ["bread"], ingredients = \[\["yeast","flour"]], supplies = ["yeast","flour","corn"]

**Output:** ["bread"]

**Explanation:** We can create "bread" since we have the ingredients "yeast" and "flour".

**Example 2:**

**Input:** recipes = ["bread","sandwich"], ingredients = \[\["yeast","flour"],["bread","meat"]], supplies = ["yeast","flour","meat"]

**Output:** ["bread","sandwich"]

**Explanation:** 

We can create "bread" since we have the ingredients "yeast" and "flour". 

We can create "sandwich" since we have the ingredient "meat" and can create the ingredient "bread".

**Example 3:**

**Input:** recipes = ["bread","sandwich","burger"], ingredients = \[\["yeast","flour"],["bread","meat"],["sandwich","meat","bread"]], supplies = ["yeast","flour","meat"]

**Output:** ["bread","sandwich","burger"]

**Explanation:** 

We can create "bread" since we have the ingredients "yeast" and "flour". 

We can create "sandwich" since we have the ingredient "meat" and can create the ingredient "bread". 

We can create "burger" since we have the ingredient "meat" and can create the ingredients "bread" and "sandwich".

**Constraints:**

*   `n == recipes.length == ingredients.length`
*   `1 <= n <= 100`
*   `1 <= ingredients[i].length, supplies.length <= 100`
*   `1 <= recipes[i].length, ingredients[i][j].length, supplies[k].length <= 10`
*   `recipes[i], ingredients[i][j]`, and `supplies[k]` consist only of lowercase English letters.
*   All the values of `recipes` and `supplies` combined are unique.
*   Each `ingredients[i]` does not contain any duplicate values.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun findAllRecipes(
        recipes: Array<String>,
        ingredients: List<List<String>>,
        supplies: Array<String>,
    ): List<String> {
        val indegree: MutableMap<String, Int> = HashMap()
        val supplySet: MutableSet<String> = HashSet()
        val adj: MutableMap<String, MutableSet<String>> = HashMap()
        supplySet.addAll(supplies)
        for (recipe in recipes) {
            indegree[recipe] = 0
        }
        for (i in recipes.indices) {
            val recipe = recipes[i]
            var numberOfDependencies = 0
            for (ingredient in ingredients[i]) {
                if (!supplySet.contains(ingredient)) {
                    adj.computeIfAbsent(ingredient) { _: String? -> HashSet() }.add(recipe)
                    numberOfDependencies++
                }
            }
            indegree[recipe] = numberOfDependencies
        }
        val q: Queue<String> = LinkedList()
        for ((key, value) in indegree) {
            if (value == 0) {
                q.add(key)
            }
        }
        val res: MutableList<String> = ArrayList()
        while (q.isNotEmpty()) {
            val recipe = q.remove()
            res.add(recipe)
            if (adj.containsKey(recipe)) {
                for (dep in adj[recipe]!!) {
                    indegree[dep] = indegree[dep]!! - 1
                    if (indegree[dep] == 0) {
                        q.add(dep)
                    }
                }
            }
        }
        return res
    }
}
```