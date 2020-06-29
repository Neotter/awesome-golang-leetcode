<!--
 * @Author: Nettor
 * @Date: 2020-06-29 15:46:34
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-29 18:10:07
 * @Description: file content
-->

# Valid Sudoku

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

- Each row must contain the digits 1-9 without repetition.
- Each column must contain the digits 1-9 without repetition.
- Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

![img](./img/1.png)

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

**Example 1:**

```go
Input:
[
    ["5","3",".",".","7",".",".",".","."],
    ["6",".",".","1","9","5",".",".","."],
    [".","9","8",".",".",".",".","6","."],
    ["8",".",".",".","6",".",".",".","3"],
    ["4",".",".","8",".","3",".",".","1"],
    ["7",".",".",".","2",".",".",".","6"],
    [".","6",".",".",".",".","2","8","."],
    [".",".",".","4","1","9",".",".","5"],
    [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

**Example 2:**

```go
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with
the 5 in the top left corner being modified to 8.
Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

## Solution

数独的规则：

- 每一行必须是数字 1~9 且不重复
- 每一列必须是数字 1~9 且不重复
- 每一个小九宫格（互不交叉，总共九个小九宫格）必须是数字 1~9 且不重复

可以用 map 做成的 set 解，检查行和检查列挺简单的，按行或列扫描时判断当前 `board[i][j]` 是否存在于 map 中，如果不存在就加入，如果存在直接返回 false。

这里有个小技巧就是可以把按行扫描和按列扫描都塞到同一个 for 循环中，实现方法是把`board[i][j]`（按行）的索引互换一下，变成`board[j][i]`（按列），具体看代码。

另一个问题是如何扫描每个九宫格

注意这时我们可以索引`i,j`的意思变成第 i 个九宫格的第 j 格子

我们观察一下九宫格**行号**的规律：

```go
  - - -   - - -   - - -
| 0 0 0 | 0 0 0 | 0 0 0 |
| 1 1 1 | 1 1 1 | 1 1 1 |
| 2 2 2 | 2 2 2 | 2 2 2 |
  - - -   - - -   - - -
| 3 3 3 | 3 3 3 | 3 3 3 |
| 4 4 4 | 4 4 4 | 4 4 4 |
| 5 5 5 | 5 5 5 | 5 5 5 |
  - - -   - - -   - - -
| 6 6 6 | 6 6 6 | 6 6 6 |
| 7 7 7 | 7 7 7 | 7 7 7 |
| 8 8 8 | 8 8 8 | 8 8 8 |
  - - -   - - -   - - -
```

可以看出，对于第 i 个九宫格内的格子，每过 3 个格子行号增加 1，对于每 3 个九宫格之间的格子，每过 3 个九宫格，行号同时加 3.

举个例子，第 0 个九宫格的第 0 1 2 个格子的行号都为零，第 3 4 5 个九宫格因为经过 0 1 2 三个格子，按照每 3 个格子行号行号增加 1 的规则，所以要加 1，变成都为 1.

第 3 个九宫格的因为跨过了第 0 1 2 这三个九宫格（注意区分第 n 九宫格和第 n 个九宫格的第 m 个格子的表述），按照**每 3 个九宫格行号同时加 3**的规则，所以在

```go
0 0 0
1 1 1
2 2 2
```

的基础上同时加 3，变成

```go
3 3 3
4 4 4
5 5 5
```

因此行号可以表示为**i/3\*3 + i/3**

我们再观察一下九宫格**列号**的规律：

```go
  - - -   - - -   - - -
| 0 1 2 | 3 4 5 | 6 7 8 |
| 0 1 2 | 3 4 5 | 6 7 8 |
| 0 1 2 | 3 4 5 | 6 7 8 |
  - - -   - - -   - - -
| 0 1 2 | 3 4 5 | 6 7 8 |
| 0 1 2 | 3 4 5 | 6 7 8 |
| 0 1 2 | 3 4 5 | 6 7 8 |
  - - -   - - -   - - -
| 0 1 2 | 3 4 5 | 6 7 8 |
| 0 1 2 | 3 4 5 | 6 7 8 |
| 0 1 2 | 3 4 5 | 6 7 8 |
  - - -   - - -   - - -
```

可以看出对于第 i 个九宫格内的格子，列号按 3 个格子为周期，每次增加 1. 周期的数学表示就是取余数， 因此列可以表示为 `i%3\*3+j%3`。

## Go Solution

```go
func isValidSudoku(board [][]byte) bool {
    for i:=0;i<9;i++{
        row := make(map[byte]int,0)
        col := make(map[byte]int,0)
        cube := make(map[byte]int,0)
        for j:=0;j<9;j++{
            //检查行
            if _, ok := row[board[i][j]]; board[i][j] != '.' && !ok{
                row[board[i][j]] = 1
            } else if board[i][j] != '.' && ok{
                return false
            }
            //检查列
            if _, ok := col[board[j][i]];board[j][i] != '.' && !ok{
                col[board[j][i]] = 1
            } else if board[j][i] != '.' && ok{
                return false
            }
            //检查九宫格
            // 行号+偏移量
            RowIndex := 3 * (i / 3) + j / 3;
            // 列号+偏移量
            ColIndex := 3 * (i % 3) + j % 3;
            //每个小九宫格，总共9个
            if _, ok := cube[board[RowIndex][ColIndex]]; board[RowIndex][ColIndex] != '.' && !ok {
                cube[board[RowIndex][ColIndex]] = 1
            } else if board[RowIndex][ColIndex] != '.' && ok{
                return false
            }
        }
    }
    return true
}
```
