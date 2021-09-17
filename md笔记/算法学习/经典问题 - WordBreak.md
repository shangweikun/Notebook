##WordBreak

###单词拆分问题 - I

\#DPS \#记忆化搜索 \#可行性

https://www.lintcode.com/problem/word-break/

本题解题难点在于记忆化的使用：

​	～ 记忆化表示的含义为：***index 后的字符串，是否为字典串***（可以拆为多个word）



其中，在编写代码过程中，考虑：**通过dict来做循环匹配**｜ **subStr做循环匹配**

【实际过程中，subStr的循环要远小于dict字典的大小】



最后，***剪枝操作***

【判断当前循环中的subStr，是否已经超过了最大的dict中最大单词的长度，排除后续无效循环】

​	～ 通过maxWordLength，来实现对dfs的优化

```java
    wordSet [li, nt, lint, Code]
               lintCode
         /                \           ...             \
(x) l | intCode           (✓) li |ntCode         (✓) lint | Code
                    /           \
          (x) li | n | tCode   (✓) li | nt | Code
```



```java
/**
	 * @param s:       A string
	 * @param wordSet: A dictionary of words dict
	 * @return: A boolean
	 */
	public boolean wordBreak(String s, Set<String> wordSet) {

		if (wordSet.contains(s)) {
			return true;
		}

		if (s.length() == 0) {
			return true;
		}

		int maxWordLength = getMaxLength(wordSet);

		return dfsIsWords(s,
				0,
				wordSet,
				maxWordLength,
				new HashMap<Integer, Boolean>());

	}

	private int getMaxLength(Set<String> wordSet) {
		int result = Integer.MIN_VALUE;
		for (String word : wordSet
		) {
			result = Math.max(word.length(), result);
		}
		return result;
	}
	private boolean dfsIsWords(String s,
	                           int index,
	                           Set<String> wordSet,
	                           int maxWordLength,
	                           Map<Integer, Boolean> memo) {
		if (s.length() == index) {
			return true;
		}

		memo.put(index, false);
		for (int i = 0; i < s.length() - index; i++) {

			if (i + 1 > maxWordLength) {
				break;
			}

			String subStr = s.substring(index, index + i + 1);
			if (!wordSet.contains(subStr)) {
				continue;
			}
			if (dfsIsWords(s,
					index + i + 1,
					wordSet,
					maxWordLength,
					memo)) {
				memo.put(index, true);    //只寻找可行性，无需继续
				break;
			}
		}

		return memo.get(index);
	}
```



###单词拆分问题 - II

\#DFS \#具体方案

https://www.lintcode.com/problem/word-break-ii/



本题通过，需要通过剪枝优化进行操作 ***pruning***

【pruning的逻辑本质就是一个**wordBreak - I**的代码】

```java
/*
 * @param s: A string
 * @param wordDict: A set of words.
 * @return: All possible sentences.
 */
	public List<String> wordBreak(String s,
	                              Set<String> wordDict) {

		Map<Integer, Boolean> memo = new HashMap<>();
		List<String> result = new ArrayList<>();

		if (s.length() == 0) {
			result.add(s);
			return result;
		}

		int maxWordLength = getMaxLength(wordDict);
		dfsFindWords(s,
				0,
				wordDict,
				maxWordLength,
				"",
				result,
				memo);
		return result;

	}

	private int getMaxLength(Set<String> wordSet) {
		int result = Integer.MIN_VALUE;
		for (String word : wordSet
		) {
			result = Math.max(word.length(), result);
		}
		return result;
	}

	private void dfsFindWords(String s,
	                          int index,
	                          Set<String> wordDict,
	                          int maxWordLength,
	                          String sentence,
	                          List<String> result,
	                          Map<Integer, Boolean> memo) {
		if (s.length() == index) {
			result.add(sentence.trim());
			return;
		}

		for (int i = 0; i < s.length() - index; i++) {

			if (i + 1 > maxWordLength) {
				break;
			}
			String subStr = s.substring(index, index + i + 1);
			if (!wordDict.contains(subStr)) {
				continue;
			}

			//pruning
			if (!isPossibleSentence(s,
					index + i + 1,
					wordDict,
					maxWordLength,
					memo)) {
				continue;
			}

			dfsFindWords(s,
					index + i + 1,
					wordDict,
					maxWordLength,
					sentence + subStr + " ",
					result,
					memo);
		}
	}

	private boolean isPossibleSentence(String s,
	                                   int index,
	                                   Set<String> wordDict,
	                                   int maxWordLength,
	                                   Map<Integer, Boolean> memo) {
		if (memo.containsKey(index)) {
			return memo.get(index);
		}

		if (s.length() == index) {
			return true;
		}

		memo.put(index, false);
		for (int i = 0; i < s.length() - index; i++) {
			if (i + 1 > maxWordLength) {
				break;
			}
			String subStr = s.substring(index, index + i + 1);
			if (!wordDict.contains(subStr)) {
				continue;
			}
			if (isPossibleSentence(s,
					index + i + 1,
					wordDict,
					maxWordLength,
					memo)) {
				memo.put(index, true);
				break;
			}

		}

		return memo.get(index);
	}
```



由 ***wordBreak - III*** 方案总数变化而来的

```java
/**
 * @param s:    A string
 * @param wordDict: A set of word
 * @return: the number of possible sentences.
 */
	public List<String> wordBreak2(String s, Set<String> wordDict) {

		if (s.length() == 0) {
			List<String> result = new ArrayList<>();
			result.add(s);
			return result;
		}

		int maxWordLength = getMaxLength(wordDict);

		return dfsSearchWords(s.toLowerCase(),
				0,
				wordDict,
				maxWordLength,
				new HashMap<>());

	}


	private int getMaxLength(Set<String> wordSet) {
		int result = Integer.MIN_VALUE;
		for (String word : wordSet
		) {
			result = Math.max(word.length(), result);
		}
		return result;
	}

	private List<String> dfsSearchWords(String s,
	                                    int index,
	                                    Set<String> wordDict,
	                                    int maxWordLength,
	                                    Map<Integer, List<String>> memo) {
		if (memo.containsKey(index)) {
			return memo.get(index);
		}

		if (s.length() == index) {
			return Collections.singletonList("");
		}

		List<String> indexList = new ArrayList<>();
		memo.put(index, indexList);
		for (int i = 0; i < s.length() - index; i++) {
			if (i + 1 > maxWordLength) {
				break;
			}
			String subStr = s.substring(index, index + i + 1);
			if (!wordDict.contains(subStr)) {
				continue;
			}
			List<String> subList = dfsSearchWords(
					s,
					index + i + 1,
					wordDict,
					maxWordLength,
					memo
			);
			handleIndexList(subStr, indexList, subList);
		}

		return memo.get(index);
	}

	private void handleIndexList(String prefix,
	                             List<String> indexList,
	                             List<String> subList) {
		for (String subStr : subList
		) {
			indexList.add((prefix + " " + subStr).trim());
		}
	}
```



### 单词拆分问题 - III

\#DPS \#记忆化搜索 \#方案总数

本题本质是一个wordBreak的基本变型

【注意在记忆化存储时，一定要注意 ***方案的累加*** 可能性】

```java
/*
	Dict[Co, der, de, d, er, e, r]
						Coder
						 ||
					  Co ｜ der
				/          \          \
		(1) Co der.   (1) Co d er.   (1) Co d e r.
*/
```



```java
/**
 * @param s:    A string
 * @param dict: A set of word
 * @return: the number of possible sentences.
 */
	public int wordBreak3(String s, Set<String> dict) {

		if (s.length() == 0) {
			return 0;
		}

		int maxWordLength = getMaxLength(dict);
		Set<String> lowerDict = lowerDictHandle(dict);


		return dfsSearchWords(s.toLowerCase(),
				0,
				lowerDict,
				maxWordLength,
				new HashMap<>());

	}

	private Set<String> lowerDictHandle(Set<String> dict) {
		Set<String> lowerDict = new HashSet<>();
		for (String word : dict) {
			lowerDict.add(word.toLowerCase());
		}
		return lowerDict;
	}

	private int getMaxLength(Set<String> wordSet) {
		int result = Integer.MIN_VALUE;
		for (String word : wordSet
		) {
			result = Math.max(word.length(), result);
		}
		return result;
	}

	private int dfsSearchWords(String s,
	                           int index,
	                           Set<String> wordDict,
	                           int maxWordLength,
	                           Map<Integer, Integer> memo) {
		if (memo.containsKey(index)) {
			return memo.get(index);
		}

		if (s.length() == index) {
			return 1;
		}

		memo.put(index, 0);
		for (int i = 0; i < s.length() - index; i++) {
			if (i + 1 > maxWordLength) {
				break;
			}
			String subStr = s.substring(index, index + i + 1);
			if (!wordDict.contains(subStr)) {
				continue;
			}
			memo.put(index,
					memo.get(index) + dfsSearchWords(s,
							index + i + 1,
							wordDict,
							maxWordLength,
							memo));
		}

		return memo.get(index);
	}
```

