# 算法与数据结构（leetcode等）



#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

leetcode上的该题【滑动窗口可直接求解】



以下是：**大神的单循环题解**

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        k, res, c_dict = -1, 0, {}
        for i, c in enumerate(s):
            if c in c_dict and c_dict[c] > k:  # 字符c在字典中 且 上次出现的下标大于当前长度的起始下标
                k = c_dict[c]
                c_dict[c] = i
            else:
                c_dict[c] = i
                res = max(res, i-k)
        return res
```





### 滚动数组技巧

​																	**二维数组 -> 滚动处理**

```java
        d[i][j] = d[i - 1][j] + d[i][j - 1];
        d[i % 2][j] = d[(i - 1) % 2][j] + d[i % 2][j - 1];
        d[j] += d[j - 1];

```



### 树状数组

url:https://www.cnblogs.com/xenny/p/9739600.html

