##**回文数练习**

题目：https://www.lintcode.com/problem/longest-palindromic-substring

题解：https://www.jiuzhang.com/solution/longest-palindromic-substring/



###中心线法出现的两个问题：
两个地方出现差错：
1、作为奇偶的回文数，起始点需要从(mid-1,mid+1)和(mid,mid+1)同时开始寻找；
2、end节点的查找错误，right本身就是其结束节点。

###动态规划法出现的问题：

进行数组计算的边界值出来，以及出现了重复计算的过程

```java
package com.jiuzhang2021.class2;

public class Solution0 {
	/**
	 * @param s: input string
	 * @return: a string as the longest palindromic substring
	 */
	public String longestPalindrome(String s) {

		if (s == null || s.length() == 0) {
			return "";
		}

		String result = "";
		int resLength = 0;

		int length = s.length();
		boolean[][] isPalindrome = new boolean[length][length];

		for (int i = length - 1; i >= 0; i--) {
			for (int j = i; j <= length - 1; j++) {
				String tmp = isPalindrome(s, i, j, isPalindrome);
				if (tmp.length() <= resLength) {
					continue;
				}
				result = tmp;
				resLength = result.length();
			}
		}

		return result;
	}

	private String isPalindrome(String s, int i, int j, boolean[][] isPalindrome) {

		boolean isPalindromeTmp = i == j || (i + 1 > j - 1 ? s.charAt(i) == s.charAt(j) :
				isPalindrome[i + 1][j - 1] && s.charAt(i) == s.charAt(j));
		isPalindrome[i][j] = isPalindromeTmp;
		if (isPalindromeTmp) {
			return s.substring(i, j + 1);
		}
		return "";
	}

}
```





### 字符串的小知识点



>Java中
>String demo = "Hello,world!";
>1.int length = demo.length(); //获取字符串的长度
>2.boolean equals = demo.equals("Hello,world"); // 比较两个字符串相等
>3.boolean contains = demo.contains("word"); // 是否包含子串
>4.String replace = demo.replace("Hello,", "Yeah@"); // 将指定字符串(或正则表达式)替换，返回替换后的结果
>5.char little = demo.charAt(5); // 查找字符串中索引为5的字符（索引从0开始）
>6.String trim = demo.trim(); // 将字符串左右空格去除，返回去除空格后的结果
>7.String concat = demo.concat("Great!"); // 拼接字符串，返回拼接结果
>8.char[] charArray = demo.toCharArray(); // 返回该字符串组成的字符数组
>9.String upperCase = demo.toUpperCase(); // 返回该字符串的大写形式
>10.String lowerCase = demo.toLowerCase(); // 返回该字符串的小写形式



>Python中
>s = "Hello,World"
>1.print(s[1]) # 'e', 取出某个位置的字符
>2.print(s[1:6]) # 'ello,' ，字符串切片
>3.print(len(s)) # 11, 返回字符串的长度
>4.print("e" in s) # True, 返回字符是否在字符串中
>5.print(s.lower()) # 'hello,world', 将字符串所有元素变为小写
>6.print(s.upper()) # 'HELLO,WORLD', 将字符串所有元素变为大写
>7.s += '...' # Hello,World... ，字符串拼接，在字符串后拼接另一个字符串
>8.print(s.find('lo')) # 3, 返回第一次找到指定字符串的起始位置（从左往右找）
>9.print(s.swapcase()) # hELLO,wORLD..., 将大小写互换
>10.print(s.split(',')) # ['Hello', 'World...'], 将字符串根据目标字符分割



### 排颜色

桶排序，以及相向指针使用

https://www.lintcode.com/problem/sort-colors-ii

https://www.jiuzhang.com/solutions/sort-colors-ii/



***left*** 和 ***right*** 用 `<= `进行比较，最终的结论

 >left指向右分区第一个开始位置
 >
 >right指向左分区最后一个位置
 >
 >left - right = 1

代码如下：

```java
package com.jiuzhang2021.class8;

import java.util.Arrays;

public class SolutionSortColors2 {
	/**
	 * @param colors: A list of integer
	 * @param k:      An integer
	 * @return: nothing
	 */
	public void sortColors2(int[] colors, int k) {
		sortByPartition(colors, 0, 0, colors.length - 1, k);
		Arrays.stream(colors).forEach(s -> System.out.print(s + ","));
	}

	private void sortByPartition(int[] colors, int kStart, int start, int end, int kEnd) {
		if (start >= end) return;
		if (kStart == kEnd) return;
		int left = start;
		int right = end;
		int mid = (kStart + kEnd) / 2;

		while (left <= right) {
			while (left <= right && colors[left] <= mid) {
				left++;
			}
			while (left <= right && colors[right] > mid) {
				right--;
			}
			if (left <= right) {
				changeColors(colors, left, right);
				left++;
				right--;
			}
		}

		sortByPartition(colors, kStart, start, left - 1, mid);
		sortByPartition(colors, mid + 1, left, end, kEnd);

	}

	private void changeColors(int[] colors, int left, int right) {
		int tmp = colors[left];
		colors[left] = colors[right];
		colors[right] = tmp;
	}

	public static void main(String[] args) {
		SolutionSortColors2 sol = new SolutionSortColors2();
		sol.sortColors2(
				new int[]{8, 1, 10, 1, 8, 8, 2, 4, 9, 3, 8, 1, 3, 3, 6, 2, 5, 1, 1, 7, 1, 1, 3, 9, 6, 4, 6, 6, 7, 2}, 10);
	}
}

```





ps：代码编写中遇到，mid中间值的取值问题

一开始的想法是将mid计算后，传入其中；发现在后半段计算过程中，丢失了mid的前缀数值，导致mid中间值取值错误；

错误代码如下：

```java
public class Solution {
  
  /*
  [8,1,10,1,8,8,2,4,9,3,8,1,3,3,6,2,5,1,1,7,1,1,3,9,6,4,6,6,7,2]
	10
  */
  
    /**
	 * @param colors: A list of integer
	 * @param k:      An integer
	 * @return: nothing
	 */
	public void sortColors2(int[] colors, int k) {
		int mid = k / 2;
		sortByPartition(colors, mid, 0, colors.length - 1, k);
		Arrays.stream(colors).forEach(s -> System.out.print(s + ","));
	}

	private void sortByPartition(int[] colors, int mid, int start, int end, int k) {
		if (start >= end) return;
		int left = start;
		int right = end;
		while (left < right) {
			while (left < right && colors[left] <= mid) {
				left++;
			}
			while (left < right && colors[right] > mid) {
				right--;
			}
			if (left < right) {
				changeColors(colors, left, right);
				left++;
				right--;
			}
		}

		if (left == end) return;
		if (right == start) return;

		int mid1 = mid / 2;
		int mid2 = (mid + k) / 2;
		
		if (colors[left] <= mid) {
			sortByPartition(colors, mid1, start, left, mid);
			sortByPartition(colors, mid2, left + 1, end, k);
		} else {
			sortByPartition(colors, mid1, start, left - 1, mid);
			sortByPartition(colors, mid2, left, end, k);
		}

	}

	private void changeColors(int[] colors, int left, int right) {
		int tmp = colors[left];
		colors[left] = colors[right];
		colors[right] = tmp;
	}

}
```





### 四数之和

两两相加求结果，最后进行结果汇总；

https://www.lintcode.com/problem/4sum-ii/

https://www.jiuzhang.com/solutions/4sum-ii



代码如下：

```python
class Solution:
    def fourSumCount(self, A, B, C, D):
        counter = {}
        for a in A:
            for b in B:
                counter[a + b] = counter.get(a + b, 0) + 1
        answer = 0
        for c in C:
            for d in D:
                answer += counter.get(-c - d, 0)
        return answer

```





###移动零

***同向快慢指针***：将非零值直接覆盖零的数值，然后将right左移动，最后将right移动过的位置全部复写0

https://www.lintcode.com/problem/move-zeroes/

https://www.jiuzhang.com/solution/move-zeroes



ps : 注意 `left++` 的位置，如果在`if (left != right)` 里面，***left*** 和 ***right*** 在都不是0的情况下，无法同时移动

代码：

```java
package com.jiuzhang2021.class8;

public class MoveZeroes {

	public void moveZeroes(int[] nums) {
		int left = 0;
		int right = 0;
		while (right < nums.length) {
			if (nums[right] != 0) {
				if (left != right) {
					nums[left] = nums[right];
				}
				left++;
			}
			right++;
		}
		while (left < nums.length) {
			if (nums[left] != 0) {
				nums[left] = 0;
			}
			left++;
		}
	}
}
```





### 队列章节

LinkedList 与 ArrayList 对比：

- 对于随机访问get和set，ArrayList绝对优于LinkedList，因为LinkedList要移动指针
- 对于新增和删除操作add和remove，在已经得到了需要新增和删除的元素位置的前提下，LinkedList可以在O(1)的时间内删除和增加元素，而ArrayList需要移动`增加或删除元素之后的所有元素`的位置，时间复杂度是O(n)的，因此LinkedList优势较大





### BFS（Bread First Search）



**适用场景：**

先序遍历通常使用递归方式来实现，即使使用非递归方式，也是借助栈来实现的，所以并不适合BFS，而层次遍历因为是一层一层的遍历，所以是BFS十分擅长的；边长一致的图是简单图，所以可以用BFS找最短路径；因为BFS只适用于简单图，边长不一致的图是无法通过BFS找到对短路径；矩阵连通块也是BFS可以处理的问题，求出最大块只需要维护一个最大值即可；选项F属于求所有方案问题，因此可以用BFS来处理，但是并不是唯一的解决方式。



###^ 异或操作

任何数异或0等于它本身，故 0 ^ 10 = 10，并且异或符合交换律（0 ^ 10 = 10 ^ 0）。
任何数异或它自己都是0，故 10 ^ 10 = 0。
3(011) ^ 7(111) = 4(100)，括号内为对应的二进制。
异或具有“知一得三”的性质，已知 A ^ B = C，一定能得到 A ^ C = B 和 B ^ C = A。





### **如果TSP问题中城市数量为10个，那么我最好用下列哪个范围来进行状态压缩呢？**

10个城市，也就是2^10 = 1024种状态。
在二进制中，长度为10的数字为：0 000 000 000 ~ 1 111 111 111，也就是`[0, 1023]`

由于0状态也可以忽略 `[1-2024)`





# HashMap和HashSet的区别？

首先得看java最基本的两种数据结构：数组和链表的区别：

数组易于快速读取（通过for循环），不便存储（数组长度有限制）；链表易于存储，不易于快速读取。

**哈希表的出现是为了解决链表访问不快速的弱点，哈希表也称散列表**。

HashSet是通过HashMap来实现的，HashMap的输入参数有Key、Value两个组成，在实现HashSet的时候，保持HashMap的Value为常量，相当于在HashMap中只对Key对象进行处理。

HashMap的底层是一个数组结构，数组中的每一项对应了一个链表，这种结构称“链表散列”的数据结构，即数组和链表的结合体；也叫散列表、哈希表。

想要了解HashMap和HashSet这样两个不同存储结构的区别，就得熟知他们的存储过程



**一、HashMap存储对象的过程**

1、对HashMap的Key调用hashCode()方法，返回int值，即对应的hashCode；

2、把此hashCode作为哈希表的索引，查找哈希表的相应位置，若当前位置内容为NULL，则把HashMap的Key、Value包装成Entry数组，放入当前位置；

3、若当前位置内容不为空，则继续查找当前索引处存放的链表，利用equals方法，找到Key相同的Entry数组，则用当前Value去替换旧的Value；

4、若未找到与当前Key值相同的对象，则把当前位置的链表后移（Entry数组持有一个指向下一个元素的引用），把新的Entry数组放到链表表头；

**二、HashSet存储对象的过程**

往HashSet添加元素的时候，HashSet会先调用元素的hashCode方法得到元素的哈希值 ，

然后通过元素 的哈希值经过移位等运算，就可以算出该元素在哈希表中 的存储位置。

情况1： 如果算出元素存储的位置目前没有任何元素存储，那么该元素可以直接存储到该位置上。

情况2： 如果算出该元素的存储位置目前已经存在有其他的元素了，那么会调用该元素的equals方法与该位置的元素再比较一次

，如果equals返回的是true，那么该元素与这个位置上的元素就视为重复元素，不允许添加，如果equals方法返回的是false，那么该元素运行添加。





**答案总结：HashSet和HashMap的区别**

| HashMap                          | HashSet                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| 实现了Map接口                    | 实现Set接口                                                  |
| 存储键值对                       | 仅存储对象                                                   |
| 调用put（）向map中添加元素       | 调用add（）方法向Set中添加元素                               |
| HashMap使用键（Key）计算Hashcode | HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false |



##### 冲突（Collision），是说两个不同的 key 经过哈希函数的计算后，得到了两个相同的值。解决冲突的方法，主要有两种：

1. 开散列法（Open Hashing）。是指哈希表所基于的数组中，每个位置是一个 Linked List 的头结点。这样冲突的 <key, value> 二元组，就都放在同一个链表中。
2. 闭散列法（Closed Hashing）。是指在发生冲突的时候，后来的元素，往下一个位置去找空位。



### 堆

堆是一棵满足如下性质的二叉树：
1、父节点的键值总是不大于它的孩子节点的键值（小顶堆）。
2、父节点的键值总是不小于它的孩子节点的键值（大顶堆）。
由于堆是一棵形态规则的二叉树，因此堆的父节点和孩子节点存在如下关系（根节点编号为0）：
设父节点的编号为 i, 则其左孩子节点的编号为2*i+1, 右孩子节点的编号为2*i+2
设孩子节点的编号为i, 则其父节点的编号为(i-1)/2
由于上面的性质，父节点一定比他的儿节点小（大），所以整个树的树根的值一定是最小（最大）的，那么我们就能在O(1)的时间内，获得整个堆的极值。
现在有没有发现堆和我们曾经遇到过的优先队列具有很相似的特点，那么二者究竟有何联系呢？



优先队列是一种抽象的数据类型，它和堆的关系类似于，List和数组、链表的关系一样；我们常常使用堆来实现优先队列，因此很多时候堆和优先队列都很相似，它们只是概念上的区分。
优先队列的应用场景十分的广泛，常见的应用有：

- Dijkstra’s algorithm（单源最短路问题中需要在邻接表中找到某一点的最短邻接边，这可以将复杂度降低。）
- Huffman coding（贪心算法的一个典型例子，采用优先队列构建最优的前缀编码树(prefixEncodeTree)）
- Prim’s algorithm for minimum spanning tree

在java，python中都已经有封装了的Priority Queue(Heaps)
优先队列是一个至少能够提供插入（Insert）和删除最小（DeleteMin）这两种操作的数据结构。对应于队列的操作，Insert相当于Enqueue，DeleteMin相当于Dequeue。
用堆实现优先的过程中，需要注意最大堆只能对应最大优先队列，最小堆则是对应最小优先队列。



基于 shiftup的版本 O(nlogn)

算法思路：
对于每个元素A[i]，比较A[i]和它的父亲结点的大小，如果小于父亲结点，则与父亲结点交换。
交换后再和新的父亲比较，重复上述操作，直至该点的值大于父亲。
时间复杂度分析：
对于每个元素都要遍历一遍，这部分是 O(n)
每处理一个元素时，最多需要向根部方向交换 logn次。
因此总的时间复杂度是 O(nlogn)



Java版本：

```java
public class Solution {
    /**
     * @param A: Given an integer array
     * @return: void
     */
    private void siftup(int[] A, int k) {
        while (k != 0) {
            int father = (k - 1) / 2;
            if (A[k] > A[father]) {
                break;
            }
            int temp = A[k];
            A[k] = A[father];
            A[father] = temp;
            
            k = father;
        }
    }
    
    public void heapify(int[] A) {
        for (int i = 0; i < A.length; i++) {
            siftup(A, i);
        }
    }
}
```

Python版本：

```python
class Solution:
    # @param A: Given an integer array
    # @return: void
    def siftup(self, A, k):
        while k != 0:
            father = (k - 1) // 2
            if A[k] > A[father]:
                break
            A[k], A[father] = A[father], A[k]
            k = father
            
    def heapify(self, A):
        for i in range(len(A)):
            self.siftup(A, i)
```



除了上面的代码，我们也可以使用更有效率的O(n)的算法。
基于 Siftdown 的版本 O(n)

算法思路：
初始选择最接近叶子的一个父结点，与其两个儿子中较小的一个比较，若大于儿子，则与儿子交换。
交换后再与新的儿子比较并交换，直至没有儿子。
再选择较浅深度的父亲结点，重复上述步骤。
时间复杂度分析
这个版本的算法，乍一看也是 O(nlogn)， 但是我们仔细分析一下，算法从第 n/2 个数开始，倒过来进行 siftdown。也就是说，相当于从 heap 的倒数第二层开始进行 siftdown 操作，倒数第二层的节点大约有 n/4 个， 这 n/4 个数，最多 siftdown 1次就到底了，所以这一层的时间复杂度耗费是 O(n/4)，然后倒数第三层差不多 n/8 个点，最多 siftdown 2次就到底了。所以这里的耗费是 O(n/8 * 2), 倒数第4层是 O(n/16 * 3)，倒数第5层是 O(n/32 * 4) ... 因此累加所有的时间复杂度耗费为：
T(n) = O(n/4) + O(n/8 * 2) + O(n/16 * 3) ...
然后我们用 2T - T 得到：
2 * T(n) = O(n/2) + O(n/4 * 2) + O(n/8 * 3) + O(n/16 * 4) ...
T(n) = O(n/4) + O(n/8 * 2) + O(n/16 * 3) ...

2 * T(n) - T(n) = O(n/2) +O (n/4) + O(n/8) + ...
= O(n/2 + n/4 + n/8 + ... )
= O(n)
因此得到 T(n) = 2 * T(n) - T(n) = O(n)



Java版本：

```java
public class Solution {
    /**
     * @param A: Given an integer array
     * @return: void
     */
    private void siftdown(int[] A, int k) {
        while (k * 2 + 1 < A.length) {
            int son = k * 2 + 1;   // A[i] 的左儿子下标。
            if (k * 2 + 2 < A.length && A[son] > A[k * 2 + 2])
                son = k * 2 + 2;     // 选择两个儿子中较小的。
            if (A[son] >= A[k])      
                break;
            
            int temp = A[son];
            A[son] = A[k];
            A[k] = temp;
            k = son;
        }
    }
    
    public void heapify(int[] A) {
        for (int i = (A.length - 1) / 2; i >= 0; i--) {
            siftdown(A, i);
        }
    }
}
```

Python版本：

```python
import sys
import collections
class Solution:
    # @param A: Given an integer array
    # @return: void
    def siftdown(self, A, k):
        while k * 2 + 1 < len(A):
            son = k * 2 + 1    #A[i]左儿子的下标
            if k * 2 + 2 < len(A) and A[son] > A[k * 2 + 2]:
                son = k * 2 + 2    #选择两个儿子中较小的一个
            if A[son] >= A[k]:
                break
                
            temp = A[son]
            A[son] = A[k]
            A[k] = temp
            k = son
    
    def heapify(self, A):
        for i in range((len(A) - 1) // 2, -1, -1):
            self.siftdown(A, i)
```





#### 堆排序

运用堆的性质，我们可以得到一种常用的、稳定的、高效的排序算法————堆排序。堆排序的时间复杂度为O(n*log(n))，空间复杂度为O(1)，堆排序的思想是：对于含有n个元素的无序数组nums, 构建一个堆(这里是小顶堆)heap，然后执行extractMin得到最小的元素，这样执行n次得到序列就是排序好的序列。
如果是降序排列则是小顶堆；否则利用大顶堆。

#### Trick

由于extractMin执行完毕后，最后一个元素last已经被移动到了root，因此可以将extractMin返回的元素放置于最后，这样可以得到sort in place的堆排序算法。
当然，如果不使用前面定义的heap，则可以手动写堆排序，由于堆排序设计到建堆和extractMin， 两个操作都公共依赖于siftDown函数，因此我们只需要实现siftDown即可。(trick:由于建堆操作可以采用siftUp或者siftDown，而extractMin是需要siftDown操作，因此取公共部分，则采用siftDown建堆)。

#### 升序堆排序（JAVA）

```java
public class Solution {
    private void siftdown(int[] A, int left, int right) {
        int k = left;
        while (k * 2 + 1 <= right) {
            int son = k * 2 + 1;
            if (son + 1 <= right && A[son] < A[son + 1]) {
                son = k * 2 + 2;
            }
            if (A[son] <= A[k]) {
                break;
            }
            int tmp = A[son];
            A[son] = A[k];
            A[k] = tmp;
            k = son;
        }
    }
    
    public void heapify(int[] A) {
        for (int i = (A.length - 1) / 2; i >= 0; i--) {
            siftdown(A, i, A.length - 1);
        }
    }
    
    void sortIntegers(int[] A) {
        heapify(A);
        for (int i = A.length - 1; i > 0; i--) {
            int tmp = A[0];
            A[0] = A[i];
            A[i] = tmp;
            siftdown(A, 0, i - 1);
        }
    }
}
```



### 同向双指针问题

因为数组是已经有序的，因此我们只需要O(n)的时间即可解决这个问题。但是这里有一个小问题，老师在视频中没有详细讲。为什么我们要让target取绝对值呢？如果不取会发生什么情况？如果取了答案会不会错？

![img](/Users/swk/九章算法2021班/10.2.png)

这里如果我们不将target置为其绝对值，会出现无法获得target的情况。因为我们的双指针移动过程中判断的都nums[j] - nums[i] 是否等于target。而如果j > i，那么nums[j] >= nums[i]，nums[j] -  nums[i] 一定不与target相等，因此会找不到解。



----



# 链表中点问题

## 问题描述

求一个链表的中点

## 问题分析

这个问题可能大家会觉得，WTF 这么简单有什么好做的？你可能的想法是：

> 先遍历一下整个链表，求出长度 L，然后再遍历一下链表找到第 L/2 的那个位置的节点。

但是在你抛出这个想法之后，面试官会追问你：`如果只允许遍历链表一次怎么办？`

可以看到这种 Follow up  并不是让你优化算法的时间复杂度，而是严格的限制了你遍历整个链表的次数。你可能会认为，这种优化有意义么？事实上是很有意义的。因为遍历一次这种场景，在真实的工程环境中会经常遇到，也就是我们常说的数据流问题（Data Stream Problem）。

## 数据流问题 Data Stream Problem

所谓的数据流问题，就是说，你需要设计一个在线系统，这个系统不断的接受一些数据，并维护这些数据的一些信息。比如这个问题就是在数据流中维护中点在哪儿。（维护中点的意思就是提供一个接口，来获取中点）

类似的一些数据流问题还有：

数据流中位数 http://www.lintcode.com/problem/data-stream-median/

数据流最大 K 项 http://www.lintcode.com/problem/top-k-largest-numbers-ii/

数据流高频 K 项 http://www.lintcode.com/problem/top-k-frequent-words-ii/

这类问题的特点都是，你`没有机会第二次遍历所有数据`。上述问题部分将在《九章算法强化班》中讲解。

## 用双指针算法解决链表中点问题

我们可以使用双指针算法来解决链表中点的问题，更具体的，我们可以称之为快慢指针算法。该算法如下：
 Java:

```
ListNode slow = head, fast = head.next;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}

return slow;
```

Python:

```
slow, fast = head, head.next
while fast != None and fast.next != None:
    slow = slow.next
    fast = fast.next.next

return slow
```

在上面的程序中，我们将快指针放在第二个节点上，慢指针放在第一个节点上，while 循环中每一次快指针走两步，慢指针走一步。这样当快指针走到头的时候，慢指针就在中点了。

快慢指针的算法，在下一小节的“带环链表”中，也用到了。

## 一个小练习

将上述代码改为提供接口的模式，即设计一个 class，支持两个函数，一个是 add(node) 加入一个节点，一个是 getMiddle() 求中间的那个节点。





### 低于O(n)的算法

​        在上一章中，我们讲到了一个O(logn)时间复杂度的算法，二分算法。只需要O(logn)的时间就可以成功进行搜索。而我们在第四章的时候，就已经讲到了，我们可以通过时间复杂度来推测算法，当我们发现需要时间复杂度为<O(N)的算法时，我们可以考虑使用二分搜索来解决问题。但是如果题目不能用二分搜索来解决，我们该使用什么算法来解决呢？
今天我们就来看一下几种同为logn时间复杂度的算法——快速幂算法、辗转相除法以及另外两种小于O(N)时间复杂度的算法——质因数分解和分块检索法。



#### 快速幂算法

`a^n % b` 问题



####辗转相除法

#####算法介绍

辗转相除法， 又名欧几里德算法， 是求最大公约数的一种方法。它的具体做法是：用**较大的数**除以**较小的数**，再用**除数**除以出现的**余数**（第一余数），再用**第一余数**除以出现的**余数**（第二余数），如此反复，直到最后余数是0为止。如果是求两个数的最大公约数，那么最后的除数就是这两个数的最大公约数。

#####代码

Java:

```java
public int gcd(int big, int small) {
    if (small != 0) {
        return gcd(small, big % small);
    } else {
        return big;
    }
}
```

Python:

```python
def gcd(big, small):
    if small != 0:
        return gcd(small, big % small)
    else:
        return big
```

C++:

```c++
int gcd(int big, int small) {
    if (small != 0) {
        return gcd(small, big % small);
    } else {
        return big;
    }
}
```



####两个排序数组的中位数

#####题目描述

在两个排序数组中，求他们合并到一起之后的中位数
 时间复杂度要求：O(log(n+m))，其中 n, m 分别为两个数组的长度

#####解法

1. 基于 FindKth 的算法。整体思想类似于 median of unsorted array 可以用 find kth from unsorted array 的解题思路。
2. 基于二分的方法。二分 median 的值，然后再用二分法看一下两个数组里有多少个数小于这个二分出来的值。



#####算法描述

1. 先将找中点问题转换为找第 k 小的问题，这里直接令`k = (n + m) / 2`。那么目标是在 logk = log((n+m)/2) = log(n+m) 的时间内找到A和B数组中从小到大第 k 个。
2. 比较 A 数组的第 k/2 小和 B 数组的第 k/2 小的数。谁小，就扔掉谁的前 k/2 个数。
3. 将目标寻找第 k 小修改为寻找第 (k-k/2) 小
4. 回到第 2 步继续做，直到 k == 1 或者 A 数组 B 数组里已经没有数了。

#####F.A.Q

**Q: 如何 O(1) 时间移除数组的前 k/2 个数？**

A: 给两个数组一个起始位置的标记参数（相当于一个offset，偏移位），把这个起始位置 + k/2 就可以了。

**Q: 不是让我们找中点么？怎么变成了找第 k 小？**

A: 找第 k 小如果能在 log(k) 的时间内解决，那么找中点就可以在 log( (n+m)/2 ) 的时间内解决。

**Q.如何证明谁的第 k/2 个数比较小就扔掉谁的前 k/2 个数这个理论？**

A: 直观的，我们看一个例子

```
A=[1,3,5,7]
B=[2,4,6,8]
```

假如我们要找第 4 小。也就是 k = 4。算法会去比较两个数组中第 2 小的数。也就是 A[1]=3 和 B[1]=4 这两个数的大小。然后会发现，3比较小，然后就决定扔掉 A 的前 k/2 = 2 个数。也就是，接下来，需要去找

```
A=[5,7]
B=[2,4,6,8]
```

中的第 k-k/2=2 小的数。这里我们扔掉了 [1,3]，扔掉的这些数中，一定不会包含我们要找的第 4 小的数——4。因为从位置上，他们在 A 和 B合并到一起之后，都会排在 4 的前面。

抽象的证明一下：

我们需要回顾一下 Merge Two Sorted Arrays 这道题目。算法的做法是，每一次比较两个数组中比较小的数，然后谁小，谁先被拿出来，放到最后的合并结果中。那么假设 A 和 B中 `A[k/2 - 1] <= B[k/2 - 1]`（反之同理）。我们会决定扔掉`A[0..k/2-1]`，因为这些数在 A 与 B 做简单的 Merge 的过程中，会优先于目标第 k 个数现出来。为什么？因为既然`A[k/2-1] <= B[k/2-1]`，那么当我们用最简单的 Merge Two Sorted Arrays 的算法一个个从A和B里拿数出来的时候，当 `A[k/2 - 1]` 出来的时候，`B[k/2 - 1]` 一定还没有被拿出来，那么此时A里出来了 k/2 个数，B里出来的数一定不够 k/2 个（因为第 k/2 个数都还没出来），所以加起来总共出来的数肯定不够k个，所以第k小的数一定还留在AB数组中。

因此我们证明了：扔掉较小的一部分的前 k/2 个数，不会扔掉要找的第 k 小的数。



----

#####基于二分的算法

#####算法描述

1. 我们需要先确定二分的上下界限，由于两个数组 A, B 均有序，所以下界为 `min(A[0], B[0])`，上界为 `max(A[A.length - 1], B[B.length - 1])`.
2. 判断当前上下界限下的 mid`(mid = (start + end) / 2)` 是否为我们需要的答案；这里我们可以分别对两个数组进行二分来找到两个数组中小于等于当前 mid 的数的个数`cnt1`与 `cnt2`，`sum = cnt1 + cnt2` 即为 A 跟 B 合并后小于等于当前mid的数的个数.
3. 如果` sum < k`，即中位数肯定不是` mid`，应该大于 `mid`，更新 `start` 为` mid`，否则更新 `end` 为` mid`，之后再重复第二步
4. 当不满足 `start + 1 < end` 这个条件退出二分循环时，再分别判断一下`start`跟 `end` ，最终返回符合要求的那个数即可

#####算法详解

如果对该算法有点疑问，我们下面来详细讲解一下：

- 这一题如果用二分法来做，其实就是一个二分答案的过程
- 首先我们已经得到了上下界限，那么答案必定是在这个上下界限中的，需要实现的就是从这个歌上下界限中找出答案
- 我们每次取的 mid，其实就是我们每次在假设答案为 mid，二分的过程就是不断的推翻这个假设，然后再假设新的答案
- 需要满足的条件为：
    - 上面算法描述中的 sum 需要等于 k，这里的 k = (A.length + B.length) / 2. 如果 sum < k，很明显当前的 mid 偏小，需要增大，否则就说明当前的 mid 偏大，需要缩小.
- 最终在 start 与 end 相邻的时候退出循环，判断 start 跟 end 哪个符合条件即可得到最终结果



### 二进制加法运算

a+b = a^b + (a&b) << 1

a^b 为不进位的加法原则；

(a&b) << 1 为进位的个数；



### 分解质因数

以 sqrt{n} 为时间复杂度的算法并不多见，最具代表性的就是分解质因数了。

####具体步骤

1. 记up = sqrt{n}，作为质因数k的上界, 初始化k=2。
2. 当k <= up 且 n不为1 时，执行步骤3，否则执行步骤4。
3. 当n被k整除时，不断整除并覆盖n，同时结果中记录k，直到n不能整出k为止。之后k自增，执行步骤2。
4. 当n不为1时，把n也加入结果当中，算法结束。

####几点解释

- 不需要判定k是否为质数，如果k不为质数，且能整出n时，n早被k的因数所除。故能整除n的k必是质数。
- 为何引入up？为了优化性能。当k大于up时，k已不可能整除n，除非k是n自身。也即为何步骤4判断n是否为1，n不为1时必是比up大的质数。
- 步骤2中，也判定n是否为1，这也是为了性能，当n已为1时，可早停。

***代码***

Java:

```java
public List<Integer> primeFactorization(int n) {
    List<Integer> result = new ArrayList<>();
    int up = (int) Math.sqrt(n);
    
    for (int k = 2; k <= up && n > 1; ++k) {
        while (n % k == 0) {
            n /= k;
            result.add(k);
        }
    }
    
    if (n > 1) {
        result.add(n);
    }
    
    return result;
}
```

Python:

```python
def primeFactorization(n):
    result = []
    up = int(math.sqrt(n));
    
    k = 2
    while k <= up and n > 1: 
        while n % k == 0:
            n //= k
            result.append(k)
        k += 1
            
    if n > 1:
        result.append(n)
        
    return result
```

C++:

```c++
vector<int> primeFactorization(int n) {
    vector<int> result;
    int up = (int)sqrt(n);
    
    for (int k = 2; k <= up && n > 1; ++k) {
        while (n % k == 0) {
            n /= k;
            result.push_back(k);
        }
    }
    
    if (n > 1) {
        result.push_back(n);
    }
    
    return result;
}
```

#####复杂度分析

- 最坏时间复杂度O(sqrt (n) )。当n为质数时，取到其最坏时间复杂度。
- 空间复杂度O(log(n)), 当n质因数很多时，需要空间大，但总不会多于O(log(n))个

#####延伸

质因数分解有一种更快的算法，叫做Pollard Rho快速因数分解。该算法时间复杂度为O(n^{1/4})，其理解起来稍有难度，有兴趣的同学可以进行自学，[参考链接](https://wenku.baidu.com/view/3db5c7a6ad51f01dc381f156.html)。



 

###外排序算法（External Sorting）
外排序算法是指在内存不够的情况下，如何对存储在一个或者多个大文件中的数据进行排序的算法。外排序算法通常是解决一些大数据处理问题的第一个步骤，或者是面试官所会考察的算法基本功。外排序算法是海量数据处理算法中十分重要的一块。
在学习这类大数据算法时，经常要考虑到内存、缓存、准确度等因素，这和我们之前见到的算法都略有差别。



外排序算法分为两个基本步骤：

- 将大文件切分为若干个个小文件，并分别使用内存排好序
- 使用K路归并算法（k-way merge）将若干个排好序的小文件合并到一个大文件中



分治 - 归并

第一步：文件拆分
根据内存的大小，尽可能多的分批次的将数据 Load 到内存中，并使用系统自带的内存排序函数（或者自己写个快速排序算法），将其排好序，并输出到一个个小文件中。比如一个文件有1T，内存有1G，那么我们就这个大文件中的内容按照 1G 的大小，分批次的导入内存，排序之后输出得到 1024 个 1G 的小文件。

第二步：K路归并算法
K路归并算法使用的是数据结构堆（Heap）来完成的，使用 Java 或者 C++ 的同学可以直接用语言自带的 PriorityQueue（C++中叫priority_queue）来代替。

我们将 K 个文件中的第一个元素加入到堆里，假设数据是从小到大排序的话，那么这个堆是一个最小堆（Min Heap）。每次从堆中选出最小的元素，输出到目标结果文件中，然后如果这个元素来自第 x 个文件，则从第 x 个文件中继续读入一个新的数进来放到堆里，并重复上述操作，直到所有元素都被输出到目标结果文件中。



***Follow up:*** 一个个从文件中读入数据，一个个输出到目标文件中操作很慢，如何优化？
如果我们每个文件只读入1个元素并放入堆里的话，总共只用到了 1024 个元素，这很小，没有充分的利用好内存。另外，单个读入和单个输出的方式也不是磁盘的高效使用方式。因此我们可以为输入和输出都分别加入一个缓冲（Buffer）。假如一个元素有10个字节大小的话，1024 个元素一共 10K，1G的内存可以支持约 100K 组这样的数据，那么我们就为每个文件设置一个 100K 大小的 Buffer， 每次需要从某个文件中读数据，都将这个 Buffer 装满。当然 Buffer 中的数据都用完的时候，再批量的从文件中读入。输出同理，设置一个 Buffer 来避免单个输出带来的效率缓慢。
那下面我们就来熟悉下两路归并和K路归并的算法。





###非递归的方式实现排列和组合类DFS

本章关键字：Combination（组合），Permutations（排列），Binary（二进制）。



#### 全子集问题

用非递归（Non-recursion / Iteration）的方式实现全子集问题，有两种方式：

- 进制转换（binary）
- 宽度优先搜索（Breadth-first Search）



基于进制转换的方法
思路就是使用一个 正整数的二进制表示 的第 i 位是 1 还是 0 来代表集合的第 i 个数取或者不取。因为从 0 到 2^n - 1 总共 2^n 个整数，正好对应集合的 2^n 个子集。
比如 {1，2，3} 的子集可以用 0 到 7 来表示。

```
0 -> 000 -> {}
1 -> 001 -> {3}
2 -> 010 -> {2}
3 -> 011 -> {2,3}
4 -> 100 -> {1}
5 -> 101 -> {1,3}
6 -> 110 -> {1,2}
7 -> 111 -> {1,2,3}
```

参考代码：
java代码：

```java
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        int n = nums.length;
        Arrays.sort(nums);
        for (int i = 0; i < (1 << n); i++) {
            List<Integer> subset = new ArrayList<Integer>();
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) != 0) {
                    subset.add(nums[j]);
                }
            }
            result.add(subset);
        }
        
        return result;
    }
}
```



基于 BFS 的方法
在 BFS 那节课的讲解中，我们很少提到用 BFS 来解决找所有的方案的问题。事实上 BFS 也是可以用来做这件事情的。
用 BFS 来解决该问题时，层级关系如下：

```
第一层: []
第二层: [1] [2] [3]
第三层: [1, 2] [1, 3], [2, 3]
第四层: [1, 2, 3]
```

每一层的节点都是上一层的节点拓展而来。



参考代码：
java代码：

```java
public class Solution {
    
    /*
     * @param nums: A set of numbers
     * @return: A list of lists
     */
    public List<List<Integer>> subsets(int[] nums) {
        // List vs ArrayList （google）
        List<List<Integer>> results = new LinkedList<>();
        
        if (nums == null) {
            return results; // 空列表
        }
        
        Arrays.sort(nums);
        
        // BFS
        Queue<List<Integer>> queue = new LinkedList<>();
        queue.offer(new ArrayList<Integer>());
        
        while (!queue.isEmpty()) {
            List<Integer> subset = queue.poll();
            results.add(subset);
            
            for (int i = 0; i < nums.length; i++) {
                if (subset.size() == 0 || subset.get(subset.size() - 1) < nums[i]) {
                    List<Integer> nextSubset = new ArrayList<Integer>(subset);
                    nextSubset.add(nums[i]);
                    queue.offer(nextSubset);
                }
            }
        }
        
        return results;
    }
}
```





#### 全排列问题

在学习全排列的解决方法之前，我们先来学习如何求 下一个排列。

问题：给定一个若干整数的排列，给出按整数大小进行字典序从小到大排序后的下一个排列。若没有下一个排列，则输出字典序最小的序列。
从末尾往左走，如果一直递增，例如 {...9,7,5} ，那么下一个排列一定会牵扯到左边更多的数，直到一个非递增数为止，例如 {...6,9,7,5} 。对于原数组的变化就只到 6 这里，和左侧其他数再无关系。6 这个位置会变成6右侧所有数中比 6 大的最小的数，而 6 会进入最后 3 个数中，且后 3 个数必是升序数组。
所以算法步骤如下：

- 从右往左遍历数组 nums，直到找到一个位置 i ，满足 nums[i] > nums[i - 1] 或者 i 为 0 。
- i 不为 0 时，用j再次从右到左遍历 nums ，寻找第一个 nums[j] > nums[i - 1] 。而后交换 nums[j] 和 nums[i - 1] 。注意，满足要求的 j 一定存在！且交换后 nums[i] 及右侧数组仍为降序数组。
- 将 nums[i] 及右侧的数组翻转，使其升序。

可能会有同学有些疑问：
Q：i为0怎么办？
A：i为0说明整个数组是降序的，直接翻转整个数组即可。

Q：有重复元素怎么办？
A：在遍历时只要严格满足 nums[i] > nums[i - 1] 和 nums[j] > nums[i - 1] 就不会有问题。

Q：元素过少是否要单独考虑？
A：当元素个数小于等于1个时，可以直接返回。



java参考代码：

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers that's next permuation
    */
    // 用于交换nums[i]和nums[j]
    public void swapItem(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    // 用于翻转nums[i]到nums[j]，包含两端的这一小段数组
    public void swapList(int[] nums, int i, int j) {
        while (i < j) {
            swapItem(nums, i, j);
            i ++; 
            j --;
        }
    }
    public void nextPermutation(int[] nums) {
        int len = nums.length;
        if ( len <= 1) {
            return;
        }
        int i = len - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) {
            i --;
        }
        if (i != 0) {
            int j = len - 1;
            while (nums[j] <= nums[i - 1]) {
                j--;
            }
            swapItem(nums, j, i-1);
        }
        swapList(nums, i, len - 1);
    }
}
```



python参考代码：

```python
class Solution:
    # 用于翻转nums[i]到nums[j]，包含两端的这一小段数组
    def swapList(self, nums, i, j):
        while i < j:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            j -= 1
            
    """
    @param nums: An array of integers
    @return: nothing
    """
    def nextPermutation(self, nums):
        n = len(nums)
        if n <= 1:
            return
        
        i = n-1
        while i > 0 and nums[i] <= nums[i-1]:
            i -= 1

        if i != 0:
            j = n-1
            while nums[j] <= nums[i-1]:
                j -= 1
            nums[j], nums[i-1] = nums[i-1], nums[j]
        self.swapList(nums, i, n-1)
```

lintCode : https://www.lintcode.com/problem/next-permutation/





####全排列代码问题

在学习了 下一个排列 的算法之后，对于全排列问题，我们只需要不断调用这个算法的函数就可以啦。
一些可以做得更细致的地方：
为了确定何时结束，建议在迭代前，先对输入nums数组进行升序排序，迭代到降序时，就都找完了。有心的同学可能还记得在 nextPermutation 当中，当且仅当数组完全降序，那么从右往左遍历的指针 i 最终会指向 0 。所以可以为 nextPermutation 带上布尔返回值，当 i 为 0 时，返回 false，表示找完了。要注意，排序操作在这样一个 NP 问题中，消耗的时间几乎可以忽略。
当数组长度为 1 时，nextPermutation 会直接返回 false ；当数组长度为 0 时， nextPermutation 中 i 会成为 -1 ，所以返回 false 的条件可以再加上 i 为 -1 。
Java中，如果输入类型是 int[] ，而输出类型是 List<List> ，要注意，并没有太好的方法进行类型转换，这是由于 int 是基本类型。建议还是自行手动复制，实际工作中还可使用 guava 库。



java示例代码：

```java
public class Solution {
    /*
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public List<List<Integer>> permute(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        
        boolean next = true;  // next 为 true 时，表示可以继续迭代
        while (next)  {
            List<Integer> current = new ArrayList<>();  // 进行数组复制
            for (int num : nums) {
                current.add(num);
            }
            
            result.add(current);
            next = nextPermutation(nums);
        }
        return result;
    }
    // 用于交换nums[i]和nums[j]
    public void swapItem(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    // 用于翻转nums[i]到nums[j]，包含两端的这一小段数组
    public void swapList(int[] nums, int i, int j) {
        while (i < j) {
            swapItem(nums, i, j);
            i ++; 
            j --;
        }
    }
    public boolean nextPermutation(int[] nums) {
        int len = nums.length;
        int i = len - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) {
            i--;
        }
        if (i <= 0) {
            return false;
        }
        
        int j = len - 1;
        while (nums[j] <= nums[i - 1]) {
            j--;
        }
        swapItem(nums, j, i - 1);
        
        swapList(nums, i, len - 1);
        
        return true;
    }
    
}
```



python示例代码：

```python
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """
    def permute(self, nums):
        # write your code here
        nums.sort()
        result = []


        hasNext = True  # hasNext 为 true 时，表示可以继续迭代
        while hasNext:
            current = list(nums)  # 进行数组复制
            result.append(current)
            hasNext = self.nextPermutation(nums)
        
        return result


    def swapList(self, nums, i, j):
        while i < j:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            j -= 1
            
    def nextPermutation(self, nums):
        n = len(nums)
        if n <= 1:
            return
        
        i = n - 1
        while i > 0 and nums[i] <= nums[i-1]:
            i -= 1


        if i <= 0:
            return False


        j = n-1
        while nums[j] <= nums[i-1]:
            j -= 1
        nums[j], nums[i-1] = nums[i-1], nums[j]
        
        self.swapList(nums, i, n-1)


        return True
```





####如何求一个排列是第几个？

题目：给出一个不含重复数字的排列，求这些数字的所有排列按字典序排序后该排列的编号，编号从1开始。例如排列 [1, 2, 4] 是第 1 个排列。

### 算法描述

只需计算有多少个排列在当前排列A的前面即可。如何算呢?举个例子，[3,7,4,9,1]，在它前面的必然是某位置i对应元素比原数组小，而i左侧和原数组一样。也即 [3,7,4,1,X] ， [3,7,1,X,X] ， [3,1或4,X,X,X] ， [1,X,X,X,X] 。
而第 i 个元素，比原数组小的情况有多少种，其实就是 A[i] 右侧有多少元素比 A[i] 小，乘上 A[i] 右侧元素全排列数，即 A[i] 右侧元素数量的阶乘。 i 从右往左看，比当前 A[i] 小的右侧元素数量分别为 1,1,2,1，所以最终字典序在当前 A 之前的数量为 1×1!+1×2!+2×3!+1×4!=39 ，故当前 A 的字典序为 40。

### 具体步骤：

用 permutation 表示当前阶乘，初始化为 1,result 表示最终结果，初始化为 0 。由于最终结果可能巨大，所以用 long 类型。
i从右往左遍历 A ，循环中计算 A[i] 右侧有多少元素比 A[i] 小，计为 smaller ，result += smaller * permutation。之后 permutation *= A.length - i ，为下次循环 i 左移一位后的排列数。
已算出多少字典序在 A 之前，返回 result + 1 。



### 参考代码

java代码：

```java
public class Solution {
    /**
     * @param A: An array of integers
     * @return: A long integer
     */
    public long permutationIndex(int[] A) {
        // write your code here
        long permutation = 1;
        long result = 0;
        for (int i = A.length - 2; i >= 0; --i) {
            int smaller = 0;
            for (int j = i + 1; j < A.length; ++j) {
                if (A[j] < A[i]) {
                    smaller++;
                }
            }
            result += smaller * permutation;
            permutation *= A.length - i;
        }
        return result + 1;
    }
}
```

python代码：

```python
class Solution:
    """
    @param A: An array of integers
    @return: A long integer
    """
    def permutationIndex(self, A):
        permutation = 1
        result = 0
        for i in range(len(A) - 2, -1, -1):
            smaller = 0
            for j in range(i + 1, len(A)):
                if A[j] < A[i]:
                    smaller += 1
            result += smaller * permutation
            permutation *= len(A) - i
        return result + 1
```

### 常见QA：

Q：为了找寻每个元素右侧有多少元素比自己小，用了O(n^2)的时间，能不能更快些？
A：可以做到O(nlogn)，但是很复杂，这是另外一个问题了，可以使用BST，归并排序或者线段树：http://www.lintcode.com/zh-cn/problem/count-of-smaller-number-before-itself/
Q：元素有重复怎么办？
A：好问题！元素有重复，情况会复杂的多。因为这会影响 A[i] 右侧元素的排列数，此时的排列数计算方法为总元素数的阶乘，除以各元素值个数的阶乘，例如 [1, 1, 1, 2, 2, 3] ，排列数为
6! ÷ (3! × 2! × 1!) 。为了正确计算阶乘数，需要用哈系表记录 A[i] 及右侧的元素值个数，并考虑到 A[i] 与右侧比其小的元素 A[k] 交换后，要把 A[k] 的计数减一。用该哈系表计算正确的阶乘数。而且要注意，右侧比 A[i]小 的重复元素值只能计算一次，不要重复计算！



