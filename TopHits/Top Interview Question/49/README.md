<!--
 * @Author: Nettor
 * @Date: 2020-06-20 21:55:34
 * @LastEditors: Nettor
 * @LastEditTime: 2020-06-20 22:00:16
 * @Description: file content
-->

# Group Anagrams

```go
func groupAnagrams(strs []string) [][]string {
    set := make(map[string][]string)
    for index,_ := range strs{
        str := SortString(strs[index])
        set[str] = append(set[str],strs[index])
    }
    res := make([][]string,0)
    for _,value := range set{
        res = append(res,value)
    }
    return res
}

type sortRunes []rune

func (s sortRunes) Less(i, j int) bool {
    return s[i] < s[j]
}

func (s sortRunes) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

func (s sortRunes) Len() int {
    return len(s)
}

func SortString(s string) string {
    r := []rune(s)
    sort.Sort(sortRunes(r))
    return string(r)
}
```

- 用 SortString 排序再放到一个 map 中,再从 map 中取出每个 key 对应的 value 放到[][]string 中就行了.
- Runes 实现的 SortString 比较满,或许直接比较 byte 比较快

改进过后:

```go
func groupAnagrams(strs []string) [][]string {
    set := make(map[string][]string)
    for index,_ := range strs{
        str := SortString(strs[index])
        set[str] = append(set[str],strs[index])
    }
    res := make([][]string,0)
    for _,value := range set{
        res = append(res,value)
    }
    return res
}

type sortBytes []byte

func (s sortBytes) Less(i, j int) bool {
    return s[i] < s[j]
}

func (s sortBytes) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

func (s sortBytes) Len() int {
    return len(s)
}

func SortString(s string) string {
    r := []byte(s)
    sort.Sort(sortBytes(r))
    return string(r)
}
```
