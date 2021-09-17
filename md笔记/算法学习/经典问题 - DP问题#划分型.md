划分型动态规划本质上是一种前缀型动态规划

一般情况下，它的状态都定义为诸如“前i个字符”，“前i个数字”等。



## 107 · 单词拆分 I

```python
def wordBreak(s, wordSet):
    if not s:
        return True

    max_word_length = 0
    for word in wordSet:
        max_word_length = max(max_word_length, len(word))

    n = len(s)
    dp = [False] * (n + 1)
    # init
    dp[0] = True

    # function
    # dp[i] = dp[i-l] and word in wordSet
    for i in range(1, n + 1):
        for length in range(1, max_word_length + 1):
            if i < length:
                break
            if not dp[i - length]:
                continue
            word = s[i - length:i]
            if word in wordSet:
                dp[i] = True
                break

    return dp[n]
```

https://www.lintcode.com/problem/word-break/



## 512 · 解码方法

```python
def numDecodings(self, s):
    if not s:
        return 0
    n = len(s)
    if n == 1:
        return self.decode_ok(s)

    dp = [0, 0, 0]
    dp[0] = 1
    dp[1] = self.decode_ok(s[0])

    for i in range(2, n + 1):
        dp[i % 3] = dp[(i - 1) % 3] * self.decode_ok(s[i - 1]) + \
                    + dp[(i - 2) % 3] * self.decode_ok(s[i - 2: i])
    return dp[n % 3]


def decode_ok(self, sub_str):
    code = int(sub_str)
    if len(sub_str) == 1 and 1 <= code <= 9:
        return 1
    if len(sub_str) == 2 and 10 <= code <= 26:
        return 1
    return 0
```

https://www.lintcode.com/problem/decode-ways/



## 437 · 书籍复印

```python
def copyBooks(self, pages, k):
    if not pages or not k:
        return 0

    n = len(pages)

    sums = [0] * (n + 1)
    for i in range(1, n + 1):
        sums[i] = sums[i - 1] + pages[i - 1]

    # init
    dp = [[sums[n]] * (k + 1) for _ in range(n + 1)]
    for j in range(k + 1):
        dp[0][j] = 0

    for i in range(n + 1):
        dp[i][1] = sums[i]

    for i in range(1, n + 1):
        for j in range(2, k + 1):
            for prev in range(1, i + 1):
                dp[i][j] = min(dp[i][j], max(dp[prev][j - 1],
                                             sums[i] - sums[prev]))

    return dp[n][k]
```



通过倒序的方式，最后一个人的工作量不断的上涨的，逐渐逼近与前面工作量的最大值。
***注意：***当超过最大值后，两个接近值仍需要比较一次，确认最小的工作时长

```python
def copyBooks(self, pages, k):
    if not pages or not k:
        return 0

    n = len(pages)

    sums = [0] * (n + 1)
    for i in range(1, n + 1):
        sums[i] = sums[i - 1] + pages[i - 1]

    # init
    dp = [[sums[n]] * (k + 1) for _ in range(n + 1)]
    for j in range(k + 1):
        dp[0][j] = 0

    for i in range(n + 1):
        dp[i][1] = sums[i]

    for i in range(1, n + 1):
        for j in range(2, k + 1):
            dp[i][j] = self.get_least_workload(dp, i, j - 1, sums)

    return dp[n][k]
```

```python
# 倒序查询优化
def get_least_workload(self, dp, workload, workers, sums):
    pages = workload
    result = sums[workload] - sums[pages]
    while dp[pages][workers] > sums[workload] - sums[pages] and pages >= 2:
        pages -= 1
        result = min(result,
                     max(dp[pages][workload], sums[workload] - sums[pages]))

    return result
```

```python
# 二分答案集方法
def get_least_workload(self, dp, workload, workers, sums):
    start, end = 0, workload
    while start + 1 < end:
        mid = start + (end - start) / 2
        if dp[mid][workers] == sums[workload] - sums[mid]:
            return sums[workload] - sums[mid]
        if dp[mid][workers] > sums[workload] - sums[mid]:
            end = mid
        else:
            start = mid

    result = sums[workload]
    result = min(result, max(dp[start][workers], sums[workload] - sums[start]))
    result = min(result, max(dp[end][workers], sums[workload] - sums[end]))

    return result
```

https://www.lintcode.com/problem/copy-books/





### Follow Up

***该题内部的prev O(n) 运算可以优化到O(1)*** 

