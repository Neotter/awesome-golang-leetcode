<!--
 * @Author: Nettor
 * @Date: 2020-06-24 13:43:02
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-24 21:01:19
 * @Description: file content
-->

# Game of Life

According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

**Example:**

```go
Input:
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output:
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

**Follow up:**

1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

# Solution

因为 board 上所有的 cell 的出生和死亡是**同时发生**的，所以如果按照顺序直接遍历每个 cell（原地（in place）地修改状态，前一个 cell 的状态修改完成后，轮到下一个 cell 需要修改状态时，此时下一个 cell 会参考前一个 cell 的状态来进行修改，这就导致了前后两个 cell 的出生和死亡**不是同时发生**。
因此解决方法是增加一个维度的空间，保存还没修改前整个 board 的状态信息，参考未修改前的 board 的状态信息推算修改后的状态信息。

如果按照从左到右从上到下的顺序遍历修改:

```go
× ● ×
● ● × ->
● × ×
第一行
● ● ×    ● ● ×    ● ● ×
● ● × -> ● ● × -> ● ● × ->
● × ×    ● × ×    ● × ×
第二行
● ● ×    ● ● ×    ● ● ×
× ● × -> × ● × -> × ● × ->
● × ×    ● × ×    ● × ×
第三行
● × ×    ● × ×    ● × ×
× ● × -> × ● × -> × ● ×
× × ×    × × ×    × × ×
```

如果参考没修改前的状态同时修改 cell:

```go
以未修改前的board做参考
× ● ×
● ● × ->
● × ×
第一行
● ● ×    ● ● ×    ● ● ×
● ● × -> ● ● × -> ● ● × ->
● × ×    ● × ×    ● × ×
第二行
● ● ×    ● ● ×    ● ● ×
● ● × -> ● ● × -> ● ● × ->
● × ×    ● × ×    ● × ×
第三行
● × ×    ● × ×    ● × ×
● ● × -> ● ● × -> ● ● ×
● × ×    ● ● ×    ● ● ×
```

从以上可以看出两种修改状态的方式是不同的，要符合题目要求同时改变状态就必须是后一种。

目前的 board 里每个 element 里面只记录了 cell 的死和活（0、1）两个状态（1bit），我们可以通过增大每个 element 为 2bit，通过记录状态的变化来解。也可以创建多一个二维数组来解，不过这样就要用到而外的空间了（不是 in place）。

| 状态转换 | bit | int |
| -------- | --- | --- |
| 死 <- 死 | 00  | 0   |
| 死 <- 活 | 01  | 1   |
| 活 <- 死 | 10  | 2   |
| 活 <- 活 | 11  | 3   |

首先先遍历 board，根据每个 cell 本身和其 neighbors 的状态，原地（in place）地更新整个 board。
让 board 的每个 cell 的值表示的是前后状态的转换。
比如，一个 cell 原先是 live，并且 2<=neighbors lives<4, 根据题目的规则 2，那么他状态转换是**活 <- 活**，所以更新这个 cell 的值为 11（bit）也就是 3（int）。

而要计算一个 cell 的 neighbor 的存活数量只用把周围的 neighbor 的状态转换和 01 按位与`&1`就可以了，因为 2bit 状态转化的前一位表示的转换后，后一位表示的是转换前。按位与计算过后只保留的转换前的值。

这种解法妙就妙在接着只用把整个 board 的 cell 向右位移一位（>>1）就能得到最终的状态了。

一个格子的上下左右对角线的格子索引可以通过当前格子的索引循环加上这个数组获取`neighbors := [...][2]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}, {-1, -1}, {1, 1}, {-1, 1}, {1, 1}}`获取。

## Go Solution

```go
func gameOfLife(board [][]int)  {
    r, c := len(board), len(board[0])
    neighbors := [...][2]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}, {-1, -1}, {1, 1}, {-1, 1}, {1, -1}}
    for i := 0; i < r; i++ {
        for j := 0; j < c; j++ {
            lives := 0
            for _, neighbor := range neighbors{
                if 0 <= i + neighbor[0] && i + neighbor[0] < r && 0 <= j + neighbor[1] && j+neighbor[1]<c{
                    lives += board[i+neighbor[0]][j+neighbor[1]] & 1
                }
            }
            //这里只有board[i][j] = 3 和= 2 是因为
            //当board[i][j] == 1 && lives < 2 && lives >= 4，
            //状态转换是死 <- 活，对应的值还是1，所以board[i][j]
            //不需要变，同理当board[i][j] == 0 && lives != 3的
            //时候状态转换是死 <- 死，对应的值还是0，board[i][j]不需要变。
            if board[i][j] == 1 && 2 <= lives && lives < 4{
                board[i][j] = 3
            }
            if board[i][j] == 0 && lives == 3{
                board[i][j] = 2
            }
        }
    }
    for i := 0; i < r; i++ {
        for j := 0; j < c; j++ {
            board[i][j] >>=1
        }
    }
}
```

Golang 要点：

- 多维数组的初始化,例如[3][3]int{{1,2,3}{4,5,6}},数组内要用{}这个括号把内容括起来。不想计算初始化数组大小的话也可以用...代替数组的大小变成[...][...]int{{1,2,3}{4,5,6}}。

- Go 不能连续使用关系运算符，比如`1 <= lives <= 2`这样写是错的。

- for range 循环，range 之前只有一个接收变量的的话，只能取出 index，两个接收变量的话，第一个是 index，第二个是 value。
