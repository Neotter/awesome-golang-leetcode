# LeetCode No.206: everse Linked List

# Question

Reverse a singly linked list.

# Example 1

```Python
Input: 1 -> 2 -> 3 -> 4 -> 5 -> NULL
Output: 5 -> 4 -> 3 -> 2 -> 1 -> NULL
```

# Solution

Zero round:

|       | curr | next |     |     |     | prev |
| ----- | ---- | ---- | --- | --- | --- | ---- |
| Input | 1 -> | 2 -> | 3-> | 4-> | 5-> | Null |

First round end:

|       |      | prev | curr | next |     |     |      |
| ----- | ---- | ---- | ---- | ---- | --- | --- | ---- |
| Input | Null | <-1  | 2 -> | 3->  | 4-> | 5-> | Null |

Second round end:

|       |      |      | prev | curr | next |      |      |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Input | Null | <- 1 | <- 2 | 3 -> | 4 -> | 5 -> | Null |

......
