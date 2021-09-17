###**\#匹配型 \#dp\[i][j]**

匹配型动态规划问题，一般是解决***两个字符串*** 问题

模型为 ***dp\[i][j]*** ，一般可以通过 ***滚动数组*** 进行优化



##192 · 通配符匹配 · Wildcard Matching

\#dp\[i][j] \#坐标型

```python
def isMatch(self, s, p):
    n, m = len(s), len(p)
    dp = [[False] * (m + 1) for _ in range(n + 1)]

    dp[0][0] = True
    for j in range(1, m + 1):
        dp[0][j] = dp[0][j - 1] and p[j - 1] == "*"

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if p[j - 1] == "*":
                dp[i][j] = dp[i - 1][j] or dp[i][j - 1]
            else:
                dp[i][j] = dp[i - 1][j - 1] and self.is_match(s[i - 1], p[j - 1])
    return dp[n][m]


def is_match(self, string, pattern):
    if pattern == "?" or string == pattern:
        return True
    return False
```

https://www.lintcode.com/problem/wildcard-matching/



## 77 · 最长公共子序列

```python
def longestCommonSubsequence(self, A, B):
    if not A or not B:
        return 0

    n, m = len(A), len(B)
    dp = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
            if A[i - 1] == B[j - 1]:
                dp[i][j] = max(dp[i][j], dp[i - 1][j - 1] + 1)
    return dp[n][m]
```

https://www.lintcode.com/problem/longest-common-subsequence/



##119 · 编辑距离

```python
def minDistance(self, word1, word2):
    if not word1:
        return len(word2)
    if not word2:
        return len(word1)

    n, m = len(word1), len(word2)
    dp = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(n + 1):
        dp[i][0] = i
    for j in range(m + 1):
        dp[0][j] = j

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + 1
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = min(dp[i][j], dp[i - 1][j - 1])
            else:
                dp[i][j] = min(dp[i][j], dp[i - 1][j - 1] + 1)

    return dp[n][m]
```

https://www.lintcode.com/problem/edit-distance/



## 154 · 正则表达式匹配

```python
def isMatch(self, s, p):
    n, m = len(s), len(p)
    dp = [[False] * (m + 1) for _ in range(n + 1)]

    dp[0][0] = True
    for j in range(1, m + 1):
        if p[j - 1] == "*":
            dp[0][j] = dp[0][j - 2]

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if p[j - 1] == "*":
                dp[i][j] = dp[i][j - 2]
                if p[j - 2] == s[i - 1] or p[j - 2] == ".":
                    dp[i][j] |= dp[i - 1][j]
            else:
                if p[j - 1] == s[i - 1] or p[j - 1] == ".":
                    dp[i][j] = dp[i - 1][j - 1]
    return dp[n][m]
```

https://www.lintcode.com/problem/regular-expression-matching/

