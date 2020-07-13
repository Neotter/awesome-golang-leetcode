<!--
 * @Author: Nettor
 * @Date: 2020-07-13 16:34:47
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-13 16:52:32
 * @Description: file content
-->

# Course Schedule II

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

**Example 1:**

```go
Input: 2, [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished
             course 0. So the correct course order is [0,1] .

```

**Example 2**

```go
Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
```

**Note:**

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.

## Solution

先看 207，看完 207 再看这题。

这题关键在于理解是入度为 0 的节点的意义。

入度为 0 的节点表示没有前置知识要学，既然没有前置的知识要学，那么当然可以排入课表啦。

假设一门课为 A，一但 A 排入了课表，那么表示 A 这门课已经学过了，那么所有需要 A 作为前置课程的其他课 B，C，D...等等，他们需要的学习的前置课程数量（入度）就可以减一。

假设 B，C，D...中有一门课的入度为 0，那意味着需要学习的前置课程数目已经全部学过了，那么当然可以排入课表.

因此在队列中呆着的节点都是的节点都是入度为 0 的节点，只要每次出队时加入到课表中，最后得到的就是结果了。

## Go Solution

```go
func findOrder(numCourses int, prerequisites [][]int) []int {
if numCourses == 0 {
return make([]int,0)
}

    pLen := len(prerequisites)
    res:= make([]int,numCourses)

    if pLen == 0{
        for i := 0; i < numCourses; i++{
            res[i] = i
        }
        return res
    }

    pCounter := make([]int,numCourses)

    // 计算每个course的入度
    for i := 0; i < pLen; i++{
        pCounter[prerequisites[i][0]]++
    }
    // 入度为0的courser入队
    queue := list.New()
    for i := 0; i < numCourses; i++{
        if pCounter[i] == 0{
            queue.PushBack(i)
        }
    }
    numNoPre := queue.Len()

    i := 0
    // 入度为0的节点出队
    for queue.Len() != 0{
        top := queue.Back().Value.(int)
        queue.Remove(queue.Back())
        // 入度为0，表示学这门course时没有需要学的前置课程，因此可以直接入队
        res[i] = top
        i++
        for i := 0; i < pLen; i++{
            if prerequisites[i][1]==top{
                pCounter[prerequisites[i][0]]--
                if pCounter[prerequisites[i][0]] == 0{
                    queue.PushBack(prerequisites[i][0])
                    numNoPre++
                }
            }
        }
    }
    if numNoPre == numCourses{
        return res
    } else {
        return make([]int,0)
    }

}

```
