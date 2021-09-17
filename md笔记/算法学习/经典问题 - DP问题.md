### Unique Path

\#坐标型 \#dp\[i][j]

```python

```



### 数字三角形

\#坐标型 

```python
class SolutionTriangle:
    """
    @param triangle: a list of lists of integers
    @return: An integer, minimum path sum
    """

    @staticmethod
    def minimumTotal(self, triangle):
        n = len(triangle)
        dp = [[0] * n for _ in range(n)]

        dp[0][0] = triangle[0][0]
        for i in range(1, n):
            dp[i][0] = dp[i - 1][0] + triangle[i][0]
            dp[i][i] = dp[i - 1][i - 1] + triangle[i][i]

        for i in range(2, n):
            for j in range(1, i):
                dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j]) + triangle[i][j]

        return min(dp[n - 1])

```



### 跳跃游戏

 \#坐标型 \#dp[i]

```python

```



### 背包问题

\#dp\[i][j] \#boolean \#背包型 \#滚动数组

```python
    def backPack(self, m, A):
        n = len(A)

        # state i个物品，在j质量限制下，最多能装多少质量物品
        dp = [[False] * (m + 1) for _ in range(2)]

        # initialize dp[0][0]都为true dp[0][1.. m]都为false
        dp[0][0] = True
        for i in range(1, m + 1):
            dp[0][i] = False

        for i in range(1, n + 1):
            for j in range(0, m + 1):
                # function
                if j < A[i - 1]:
                    dp[i % 2][j] = dp[(i - 1) % 2][j]
                else:
                    dp[i % 2][j] = dp[(i - 1) % 2][j] or dp[(i - 1) % 2][j - A[i - 1]]

        # answer dp[n][i]中首先为true的
        for i in range(m, -1, -1):
            if dp[n % 2][i]:
                return i
        return -1
```



\#dp\[i][j] \#int \#背包型

```python
    def backPack(self, m, A):
        n = len(A)

        # state i个物品，在j质量限制下，最多能装多少质量物品
        # initialize dp[0][0.. m]都为0
        dp = [[0] * (m + 1) for _ in range(n + 1)]

        for i in range(1, n + 1):
            for j in range(0, m + 1):
                # function

                if j < A[i - 1]:
                    dp[i][j] = dp[i - 1][j]
                else:
                    dp[i][j] = max(
                        dp[i - 1][j],
                        dp[i - 1][j - A[i - 1]] + A[i - 1]
                    )

        # answer n个物品，m质量下，所能装满的最大值
        return dp[n][m]
```



### 通配符匹配 · Wildcard Matching

\#dp\[i][j] \#坐标型

```java
	public boolean isMatch(String s, String p) {
		int n = s.length();
		int m = p.length();

		boolean[][] dp = new boolean[n + 1][m + 1];
		for (int i = 0; i < n + 1; i++) {
			Arrays.fill(dp[i], false);
		}

		dp[0][0] = true;
		for (int j = 1; j < m + 1; j++) {
			if (p.charAt(j - 1) == '*') {
				dp[0][j] = dp[0][j - 1];
			}
		}

		for (int i = 1; i < n + 1; i++) {
			for (int j = 1; j < m + 1; j++) {
				if (p.charAt(j - 1) == '*') {
					dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
				} else {
					if (p.charAt(j - 1) == '?'
							|| p.charAt(j - 1) == s.charAt(i - 1)) {
						dp[i][j] = dp[i - 1][j - 1];
					}
				}
			}
		}
		return dp[n][m];
	}
```



### 最长增加子串～Longest Increasing Subsequence

\#dp[i] \#接龙型 \#坐标型

```python

```



#### FollowUp ~ 找到最大的subSequence

\#倒推法

```python

```



\#正推记录法

```java

```



### Longset Continuous Increasing Subseuqence II

\#dp\[i][j] \#接龙型

```python

```



***【接龙型需要严格找到递增的数列】***



