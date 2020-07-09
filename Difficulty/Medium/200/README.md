<!--
 * @Author: Nettor
 * @Date: 2020-07-08 17:26:49
 * @LastEditors: Nettor
 * @LastEditTime: 2020-07-09 10:30:56
 * @Description: file content
-->

# Number of Islands

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```go
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**

```go
Input:
11000
11000
00100
00011

Output: 3
```

## Solution

BFS,每次把一个元素入队，遍历上下左右四个元素，如果有 1 就继续入队并且标记 visited 直到队中没有元素，这时意味着已经没有相连的陆地，岛屿加一。

再次找到 visited 中没有走过的路径，入队继续以上步骤。

## Go Solution

```go
func numIslands(grid [][]byte) int {
    res := 0
    if len(grid) == 0 || len(grid[0]) == 0 {
        return 0
    }
    row := len(grid)
    col := len(grid[0])
    // 用于记录是否遍历
    visited := make([][]bool,row)
    for i:=0; i<row; i++{
        visited[i] = make([]bool,col)
    }

    // 记录每次需要遍历的队列
    queue := list.New()
    // 上下左右
    dirX := [...]int{-1,0,1,0}
    dirY := [...]int{0,1,0,-1}
    // 上到下从左到右开始遍历
    for i:= 0; i<row; i++{
        for j:=0; j<col; j++{
            // 已经访问跳过或者不是1(陆地)
            if grid[i][j] == '0' || visited[i][j]{
                continue
            }
            res++
            // 入队
            // i*col+j 代表矩阵中第几个元素,依照列来算
            queue.PushBack(i*col+j)
            visited[i][j] = true
            for queue.Len() != 0 {
                t := queue.Front().Value.(int)
                queue.Remove(queue.Front())
                for k:=0; k<4; k++{
                    // 看上下左右是否有1相邻
                    x := t/col+dirX[k]
                    y := t%col+dirY[k]
                    if x<0 || x>=row || y<0 || y>=col || grid[x][y] == '0' || visited[x][y]{
                        continue
                    }
                    visited[x][y] = true
                    // 把当前节点相邻的所有1都入队
                    queue.PushBack(x*col+y)
                }
            }
        }
    }
    return res
}
```
