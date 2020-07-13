<!--
 * @Author: Nettor
 * @Date: 2020-07-09 13:15:48
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-13 15:59:02
 * @Description: file content
-->

# Course Schedule

There are a total of numCourses courses you have to take, labeled from 0 to numCourses-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

**Example 1:**

```go
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```go
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

**Constraints:**

- The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
- You may assume that there are no duplicate edges in the input prerequisites.
- `1 <= numCourses <= 10^5`

## Solution

图的结构

[!](./img/1.png)

图有两个重要的指标： 入度和出度

这题需要判断 course 和前置 course 之间是否有矛盾，比如学 course 1 之前需要学 course 0，但学 course 0 之前又要学 course 1，这就很矛盾，到底是要先学 course 1 还是先学 course 0.

因此这题可以转化为判断有向图是否有环的问题，每个 course 都是一个图的节点，学 course 1 之前需要学 course 0 表示为节点 course 0 指向节点 course 1。

如果节点 course 0 指向节点 course 1，而节点 course 1 又反而指向节点 course 0，那就表示有环，course 0 和 course 1 不可能完成，返回 false。

检查是否有环可以使用 BFS，我们先统计所有节点的入度的数量，然后所有入度为 0 的节点入队，每出队一个节点 X，先判断 这个入度为 0 的节点 X 是不是另一个节点 Y 的入度，如果是，Y 节点的入度数减一，检查后如果 Y 入度数为 0，统计所有入度为 0 的节点的数量后把 Y 队，如此往复，直到队列中没有节点。

这时如果该有向图有环，那么统计所有入度为 0 的节点的数量这一步时，由于成环的这两个节点永远的入度永远不会变成 0，换句话说就是永远不会入队，因此所有入度为 0 的节点的数量会少于总的节点数量是不同的，我们可以根据这个判断是否有环

## Go Solution

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    if numCourses == 1 || len(prerequisites) == 0{
        return true
    }

    prLen := len(prerequisites)

    // counter for number of prerequisites
    // pCounter用于保存每个course的入度
    pCounter := make([]int,numCourses)
    for i := 0; i < prLen; i++{
        pCounter[prerequisites[i][0]]++
    }

    // store courses that have no prequisities
    // queue用于保存0入度的course
    queue := list.New()
    for i := 0; i < numCourses; i++{
        if pCounter[i] == 0{
            queue.PushBack(i)
        }
    }

    // number of courses that have no prerequisites
    // 0入度的course的数量
    numNoPre := queue.Len()

    // iterate over zero prequisites courses
    // 遍历0入度的course
    for queue.Len() != 0{
        top := queue.Back().Value.(int)
        queue.Remove(queue.Back())
        for i := 0; i < prLen; i++{
            // if a course's prerequisite can be satisfied by a course in queue
            // 如果一个queue的某个course是另一个course的入度
            if prerequisites[i][1] == top{
                // 该course的入度数减一
                pCounter[prerequisites[i][0]]--
                // 如果该course的入度数为零，增加numNoPre的数量，并且把该节点加入到queue
                if pCounter[prerequisites[i][0]] == 0{
                    numNoPre++
                    queue.PushBack(prerequisites[i][0])
                }

            }
        }
    }


    return numNoPre == numCourses

}
```
