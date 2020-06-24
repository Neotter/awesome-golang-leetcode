<!--
 * @Author: Nettor
 * @Date: 2020-06-18 15:33:26
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-24 13:00:16
 * @Description: file content
-->

# Generate Parentheses

## Go Solution

```go
func generateParenthesis(n int) []string {
    var ret = make([]string,0,n)
    if n == 0 {
        return ret
    }
    backtracking(&ret,n,0,0,"")
    return ret
}

func backtracking(ret *[]string, n int, left int, right int, temp string){
    if right == n {
        *ret = append(*ret,temp)
        return
    }
    if left < n {
        backtracking(ret, n, left+1, right, temp+"(")
    }
    if right < left{
        backtracking(ret, n, left, right+1, temp+")")
    }
}
```
