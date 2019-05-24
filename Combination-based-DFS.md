
## Combination-based DFS

17 - Subsets

Given a set of distinct integers, return all possible subsets.

* DFS, recursion, iteration 


```python
Input: [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```


```python
# Python 2: DFS, backtracking, first find what to add next, then find 
# what to add next to the selected one ... continue until nothing can 
# be found, backtrack

class Solution:
    """
    @param nums: A set of numbers
    @return: A list of lists
    """
    def subsets(self, nums):
        if not nums:
            return [[]]
        # sort the nums first 
        nums.sort()
        results = []
        self.dfs(0, [], nums, results)
        return results 
        
    def dfs(self, start_index, subset, nums, results):

        results.append(list(subset))
        
        for i in range(start_index, len(nums)):
            subset.append(nums[i])
            self.dfs(i + 1, subset, nums, results)
            subset.pop()
        
```


```python
p = Solution()
p.subsets([1, 2, 3])
```




    [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]



18 - Subsets II

Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).


* DFS, backtracking, recursion



```python
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```


```python
class Solution:
    """
    @param nums: A set of numbers.
    @return: A list of lists. All valid subsets.
    """
    def subsetsWithDup(self, nums):
        if nums is None:
            return []
        nums.sort()
        results = []
        self.dfs(nums, 0, [], results)
        return results 
    
    
    def dfs(self, nums, index, subset, results):
        results.append(list(subset))
        print('results', results)
        
        print('index', index)
        for i in range(index, len(nums)):
            print('i', i)
            # tell if nums[i] has been added before (duplicated)
            if i != 0 and nums[i] == nums[i - 1] and i > index:
                continue
            # dfs recursion 
            subset.append(nums[i])
            self.dfs(nums, i + 1, subset, results)
            subset.pop()
        

```


```python
p = Solution()
p.subsetsWithDup([2, 2])
```

    results [[]]
    index 0
    i 0
    results [[], [2]]
    index 1
    i 1
    results [[], [2], [2, 2]]
    index 2
    i 1





    [[], [2], [2, 2]]



426 - Restore IP Addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

(Your task is to add three dots to this string to make it a valid IP address. Return all possible IP address.)


Example: 

Input: "25525511135"

Output: ["255.255.11.135", "255.255.111.35"]


```python
class Solution:
    # @param s, a string
    # @return a list of strings
    def restoreIpAddresses(self, s):
        results = []
        self.dfs(s, 0, '', results)
        return results 

    def dfs(s, sub_parts, sub_ip, results):
        if sub_parts == 4 and len(s) == 0: 
            # the first element of sub_ip is . 
            results.append(sub_ip[1:])                         
            return
        
        for i in range(1, 4):                               
            if i <= len(s):                                 
                if int(s[ :i]) <= 255:
                    self.dfs(s[i: ], sub_parts + 1, sub_ip + '.' + s[ :i])
                if s[0] == '0':
                    break                      
```

152 - Combinations

Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

Example: 

Given n = 4 and k = 2, a solution is:

[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4]
]


* DFS, backtracking 


```python
# Python: select all subsets that have length k 
class Solution:
    """
    @param n: Given the range of numbers
    @param k: Given the numbers of combinations
    @return: All the combinations of k numbers out of 1..n
    """
    def combine(self, n, k):
        if n < k or not n:
            return []
        results = []
        self.dfs(n, k, [], results)
        return results 
        
    def dfs(self, n, k, sub_str, results):
        if len(sub_str) == k:
            results.append(sub_str[:])
            return 
        start_val = sub_str[-1] + 1 if len(sub_str) > 0 else 1 
        for i in range(start_val, n + 1):
            sub_str.append(i)
            self.dfs(n, k, sub_str, results)
            sub_str.pop()
        

```


```python
p = Solution()
p.combine(3, 2)
```




    [[1, 2], [1, 3], [2, 3]]



135 - Combination Sum


Given a set of candidate numbers (`C`) and a target number (`T`), find all unique combinations in `C` where the candidate numbers sums to `T`.

The same repeated number may be chosen from `C` unlimited number of times.

All numbers (including target) will be positive integers.

Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).

The solution set must not contain duplicate combinations.

* backtracking, DFS


Example: 

Given candidate set [2,3,6,7] and target 7, a solution set is:

[7]

[2, 2, 3]



```python
# Python 1: add an extra constraint (target), start from i instead of i + 1 for dfs recursion 
#           as each element can be used repeated
class Solution:
    """
    @param candidates: A list of integers
    @param target: An integer
    @return: A list of lists of integers
    """
    def combinationSum(self, candidates, target):
        if len(candidates) == 0 or target == 0:
            return [] # as all numbers are positive  (if target ==0, can only be empty set)

        results = []
        # sort the elements in candidates 
        candidates = sorted(list(candidates))
        
        self.dfs(candidates, target, 0, [], results)
        return results
    
    
    def dfs(self, candidates, target, start_index, subset,results):
#         print('sum subset', sum(subset))
        if sum(subset) == target:
#             print('true')
            # deep copy, needed!!!  
            results.append(list(subset))
        if sum(subset) > target:
            return
            
        for i in range(start_index, len(candidates)):
            # stop iteration if candidates[i] itself > target 
            if candidates[i] > target:
                return
            # remove duplicated elements 
            if i != 0 and candidates[i] == candidates[i - 1] and i > start_index:
                continue
            subset.append(candidates[i])
            self.dfs(candidates, target, i ,subset, results)  # start_index is i instead of i + 1
            subset.pop()
        

```


```python
x = {3, 2, 1}
x = sorted(list(x))
print(x)
type(x)
x[2]
```

    [1, 2, 3]





    3




```python
p = Solution()
p.combinationSum([2,2,3], 5)
```




    [[2, 3]]




```python
# Python 2:change the target value dynamically according to subset 
class Solution:
    """
    @param candidates: A list of integers
    @param target: An integer
    @return: A list of lists of integers
    """
    def combinationSum(self, candidates, target):
        if not candidates or not target:
            return []
        
        candidates.sort()
        
        results = []
        self.dfs(candidates, 0, [], target, results)
        return results 
        
        
    def dfs(self, candidates, start_index, subset, target, results):
        if target == 0:
            results.append(list(subset))
        # a return condition 
        if target < 0:
            return 
 
        for i in range(start_index, len(candidates)):
            
            if i != 0 and candidates[i] == candidates[i - 1] and i > start_index:
                continue 
            
            subset.append(candidates[i])
            # print(subset)
            # print('target', target - candidates[i])
            self.dfs(candidates, i, subset, target - candidates[i], results)
            subset.pop()
        

```


```python
# Python 3, use dynamic target value 
# 1. result append condition 
# 2. no need to add condition 'if sum(subset) > target      return' 
class Solution:
    """
    @param candidates: A list of integers
    @param target: An integer
    @return: A list of lists of integers
    """
    def combinationSum(self, candidates, target):
        if len(candidates) == 0 or target == 0:
            return [] # as all numbers are positive  (if target ==0, can only be empty set)

        results = []
        # remove duplicated elements from the very beginning as the iteration in dfs function 
        # can start from the elements iteself (already allow duplication for as many times)
        candidates = set(candidates)
        # sort the elements in candidates 
        candidates = sorted(list(candidates))
        
        self.dfs(candidates, target, 0, [], results)
        return results
    
    def dfs(self, candidates, target, start_index, subset,results):
#         print('sum subset', sum(subset))
        if target == 0:
#             print('true')
            # deep copy, needed!!!  
            results.append(list(subset))

            
        for i in range(start_index, len(candidates)):
            # stop iteration if candidates[i] itself > target, target could be < 0  
            if candidates[i] > target:
                return
            subset.append(candidates[i])
            self.dfs(candidates, target - candidates[i], i ,subset, results)  # start_index is i instead of i + 1
            subset.pop()
        
```


```python
p = Solution()
p.combinationSum([2,2,3], 5)
```




    [[2, 3]]



153 - Combination Sum II

Given a collection of candidate numbers (`C`) and a target number (`T`), find all unique combinations in `C` where the candidate numbers sums to `T`.

Each number in `C` may only be used once in the combination.

All numbers (including target) will be positive integers.

Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).

The solution set must not contain duplicate combinations.

* backtracking, DFS, duplicated elements 


```python
Given candidate set [10,1,6,7,2,1,5] and target 8,

A solution set is:

[
  [1,7],
  [1,2,5],
  [2,6],
  [1,1,6]
]
```


```python
# Python: 
class Solution:
    """
    @param num: Given the candidate numbers
    @param target: Given the target number
    @return: All the combinations that sum to target
    """
    def combinationSum2(self, num, target):
        if target == 0 or len(num) == 0:
            return []
        
        # sort 
        num.sort()
        
        results = []
        self.dfs(num, target, 0, [], results)
        return results
    
    def dfs(self, num, target, start_index, subset, results):
        if sum(subset) == target and subset == sorted(subset):
          #  print('results', results)
            results.append(list(subset))
        if sum(subset) > target:
            return 
            
        for i in range(start_index, len(num)):
            if num[i] > target:
                return
            if i != 0 and num[i] == num[i - 1] and i > start_index:
                continue
            subset.append(num[i])
            self.dfs(num, target, i + 1, subset, results)  # NOTE: it's i + 1 !!!!!
#             print('start index', num[start_index])
#             print('start index + 1', num[start_index + 1])
            subset.pop()

        
   
```


```python
p = Solution()
p.combinationSum2([29,19,14,33,11,5,9,23,23,33,12,9,25,25,12,21,14,11,20,30,17,19,5,6,6,5,5,11,12,25,31,28,31,33,27,7,33,31,17,13,21,24,17,12,6,16,20,16,22,5], 28)
```




    [[5, 5, 5, 6, 7],
     [5, 5, 5, 13],
     [5, 5, 6, 6, 6],
     [5, 5, 6, 12],
     [5, 5, 7, 11],
     [5, 5, 9, 9],
     [5, 6, 6, 11],
     [5, 6, 17],
     [5, 7, 16],
     [5, 9, 14],
     [5, 11, 12],
     [5, 23],
     [6, 6, 7, 9],
     [6, 6, 16],
     [6, 9, 13],
     [6, 11, 11],
     [6, 22],
     [7, 9, 12],
     [7, 21],
     [9, 19],
     [11, 17],
     [12, 16],
     [14, 14],
     [28]]



89 - k Sum

Given `n` distinct positive integers, integer `k` (`k <= n`) and a number target.

Find `k` numbers where sum is target. Calculate how many solutions there are?

Example: 

Input:
List = [1,2,3,4]

k = 2

target = 5

Output: 2

Explanation: 1 + 4 = 2 + 3 = 5


```python
# Python 1: dynamic programming 
class Solution:
    """
    @param A: An integer array
    @param k: A positive integer (k <= length(A))
    @param target: An integer
    @return: An integer
    """
    def kSum(self, A, k, target):
        if not A:
            return 0 
        n = len(A)
        dp = [
            [[0] * (target + 1) for _ in range(k + 1)],
            [[0] * (target + 1) for _ in range(k + 1)],
        ]
        
        # dp[i][j][s]
        # 前 i 个数里挑出 j 个数，和为 s
        dp[0][0][0] = 1
        for i in range(1, n + 1):
            dp[i % 2][0][0] = 1
            for j in range(1, min(k + 1, i + 1)):
                for s in range(1, target + 1):
                    dp[i % 2][j][s] = dp[(i - 1) % 2][j][s]
                    if s >= A[i - 1]:
                        dp[i % 2][j][s] += dp[(i - 1) % 2][j - 1][s - A[i - 1]]
                        
        return dp[n % 2][k][target]

```


```python
# Python 2: use count to record the number of unique combinations 
class Solution:
    """
    @param A: An integer array
    @param k: A positive integer (k <= length(A))
    @param target: An integer
    @return: An integer
    """
    def kSum(self, A, k, target):
        if not A:
            return 0 
        self.count = 0 
        self.dfs(A, k, target, 0, [])
        return self.count
    
    def dfs(self, A, k, target, start_index, subset):
        if len(subset) == k and sum(subset) == target:
            self.count += 1 
        if len(subset) > k or sum(subset) > target:
            return 
            
        for i in range(start_index, len(A)):
            subset.append(A[i])
            self.dfs(A, k, target, i + 1, subset)
            subset.pop()
        return 

```


```python
# Python 3: use return a list of different combinations as well as count 
class Solution:
    """
    @param A: An integer array
    @param k: A positive integer (k <= length(A))
    @param target: An integer
    @return: An integer
    """
    def kSum(self, A, k, target):
        if not A:
            return 0 
        results = []
        self.dfs(A, k, target, 0, [], results)
        return len(results)
    
    def dfs(self, A, k, target, start_index, subset, results):
        if len(subset) == k and sum(subset) == target:
            results.append(list(subset))
        if len(subset) > k or sum(subset) > target:
            return 
            
        for i in range(start_index, len(A)):
            subset.append(A[i])
            self.dfs(A, k, target, i + 1, subset, results)
            subset.pop()
        return 

```

90 - k Sum II

Given `n` unique postive integers, number `k` (1<=k<=n) and target.

Find all possible `k` integers where their sum is target.


```python
class Solution:
    """
    @param: A: an integer array
    @param: k: a postive integer <= length(A)
    @param: targer: an integer
    @return: A list of lists of integer
    """
    def kSumII(self, A, k, target):
        if not A:
            return 0 
        results = []
        self.dfs(A, k, target, 0, [], results)
        return results
    
    def dfs(self, A, k, target, start_index, subset, results):
        if len(subset) == k and sum(subset) == target:
            results.append(list(subset))
        if len(subset) > k or sum(subset) > target:
            return 
            
        for i in range(start_index, len(A)):
            subset.append(A[i])
            self.dfs(A, k, target, i + 1, subset, results)
            subset.pop()
        return 

```

680 - Split String

Give a string, you can choose to split the string after one character or two adjacent characters, and make the string to be composed of only one character or two characters. Output all possible results.

Input: "123"

Output: [["1","2","3"],["12","3"],["1","23"]]

* DFS



```python
# Python 1: naive way: at each step, decide to add the next one or two characters in to the splited list 

class Solution:
    """
    @param: : a string to be split
    @return: all possible split string array
    """

    def splitString(self, s):
        if len(s) == 0:
            return [[]]
        results = []
        self.dfs(s, 0, [], results)
        return results 
    
    def dfs(self, s, start_index, splited_str, results):
        # append the one splitting way to results 
        if start_index == len(s):
            results.append(list(splited_str))
            
            
        if start_index <= len(s) - 1:
            splited_str.append(s[start_index])
            self.dfs(s, start_index + 1, splited_str, results)
            splited_str.pop()
        
        if start_index <= len(s) - 2:
            splited_str.append(s[start_index:(start_index + 2)])
            self.dfs(s, start_index + 2, splited_str, results)
            splited_str.pop()

```


```python
x = 'sleep'
len(x)
```




    5




```python
p = Solution()
p.splitString('123')
```




    [['1', '2', '3'], ['1', '23'], ['12', '3']]




```python
# Python 2: use more compact code (with a for loop in dfs function)
class Solution:
    """
    @param: : a string to be split
    @return: all possible split string array
    """

    def splitString(self, s):
        if len(s) == 0:
            return [[]]
        results = []
        self.dfs(s, 0, [], results)
        return results 
    
    def dfs(self, s, start_index, splited_str, results):
        # append 
        if start_index == len(s):
            results.append(list(splited_str))
            
            
        for i in range(2):
            if start_index <= len(s) - i:
                splited_str.append(s[start_index:(start_index + i + 1)])
                self.dfs(s, start_index + i + 1, splited_str, results)
                splited_str.pop()

```

136 - Palindrome Partitioning

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

Example: 
    
Given s = "aab", return:

[
  ["aa","b"],
  ["a","a","b"]
]

* DFS, backtracking 

搜索的时间复杂度：O(答案总数 * 构造每个答案的时间)

举例：Subsets问题，求所有的子集。子集个数一共 2^n，每个集合的平均长度是 O(n) 的，所以时间复杂度为 O(n * 2^n)，同理 Permutations 问题的时间复杂度为：O(n * n!)

动态规划的时间复杂度：O(状态总数 * 计算每个状态的时间复杂度)

举例：triangle，数字三角形的最短路径，状态总数约 O(n^2) 个，计算每个状态的时间复杂度为 O(1)——就是求一下 min。所以总的时间复杂度为 O(n^2)

用分治法解决二叉树问题的时间复杂度：O(二叉树节点个数 * 每个节点的计算时间)

举例：二叉树最大深度。二叉树节点个数为 N，每个节点上的计算时间为 O(1)。总的时间复杂度为 O(N)

https://www.jiuzhang.com/qa/2994/



```python
# dfs 
class Solution:
    """
    @param: s: A string
    @return: A list of lists of string
    """
    def partition(self, s):
        if not s:
            return [[]]
        results = []
        self.dfs(s, 0, [], results)
        return results 
        
        
    def dfs(self, s, start_index, sub_str, results):
        # start_index indicates the current pointer position, if already iterated on all characters, return 
        if start_index == len(s):
            results.append(sub_str[:])
        
        for i in range(len(s) - start_index):
            new_str = s[start_index:(start_index + i + 1)]
            # first tell if the new sub_str is palindrome, if yes, append it to continue to search
            if self.is_palindrome(new_str):
                sub_str.append(new_str)
                self.dfs(s, start_index + i + 1, sub_str, results)
                sub_str.pop()
                
    def is_palindrome(self, s):
        if len(s) == 1:
            return True 
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                return False 
            left += 1 
            right -= 1 
        return True 
    
```


```python
p = Solution()
p.partition('a')
```




    [['a']]



108 - Palindrome Partitioning II

Given a string s, cut s into some substrings such that every substring is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

Example:
    
Input: "aab"
    
Output: 1
    
Explanation: Split "aab" once, into "aa" and "b", both palindrome.


```python
# Python: from tail to head, with i < j, first tell the cuts needed of string s[j:], 
# then tell if s[i:j+1] is palindrome or not, use this info to update cuts[i] (cuts
# needed for string s[i:])
class Solution:
    """
    @param s: A string
    @return: An integer
    """
    def minCut(self, s):
        n = len(s)
        
        cuts = []
        table = [[False for x in range(n)] for x in range(n)]
        
        # initialize cuts 
        for i in range(n + 1):
            cuts.append(n - 1 - i)
        
        # update table[i][j] if s[i:(j + 1)] is palindrome 
        for i in reversed(range(n)):
            for j in range(i, n):
                # s[i:j+1] is palindrome if s[(i+1):(j)] is palindrom and s[i] == s[j] or 
                # j - i = 0 , 1 so at most one character between i and j 
                if s[i] == s[j] and (j - i < 2 or table[i + 1][j - 1]):
                    table[i][j] = True 
                    cuts[i] = min(cuts[i], cuts[j + 1] + 1)
        return cuts[0]

        
        

```

192 - Wildcard Matching

Implement wildcard pattern matching with support for '?' and '*'.

`'?'` Matches any single character.

`'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

Examples: 
    
isMatch("aa","a") → false

isMatch("aa","aa") → true

isMatch("aaa","aa") → false

isMatch("aa", "*") → true

isMatch("aa", "a*") → true

isMatch("ab", "?*") → true

isMatch("aab", "c*a*b") → false


* Backtracking, dynamic programming, DFS, memoization search



```python
# Python 1: DFS, O(n^2)  
class Solution:
    """
    @param s: A string 
    @param p: A string includes "?" and "*"
    @return: is Match?
    """
    def isMatch(self, source, pattern):
        return self.is_match_helper(source, 0, pattern, 0, {})
    
    def is_match_helper(self, source, i, pattern, j, memo):
        # memoization
        if (i, j) in memo:
            return memo[(i, j)]
        
        # if  source is empty
        if len(source) == i:
            # every character in pattern should be "*"
            for index in range(j, len(pattern)):
                if pattern[index] != '*':
                    return False
            return True
        
        # length of two strings not the same, cannot be matched 
        if len(pattern) == j:
            return False
        
        # discuss two cases: if pattern[j] is * or not 
        if pattern[j] != '*':
            # the current character matches and the following characters match
            matched = self.is_match_char(source[i], pattern[j]) and 
            self.is_match_helper(source, i + 1, pattern, j + 1, memo)
        else: 
            # * matches one character of source or an empty character 
            matched = self.is_match_helper(source, i + 1, pattern, j, memo) or 
            self.is_match_helper(source, i, pattern, j + 1, memo)
        # update memo[(i, j)]
        memo[(i, j)] = matched 
        return matched 
    
    def is_match_char(self, s, p):
        return s == p or p == '?'  # deal with the ? case here 
```


```python
# Python 2: dynamic programming 
class Solution:
    """
    @param s: A string 
    @param p: A string includes "?" and "*"
    @return: is Match?
    """
    def isMatch(self, s, p):
        
        # replace possible multiple * with one *, e.g., a***b --> a*b
        duplicated = False 
        new_index = 0 
        new_p = []
        for i in range(len(p)):
            if p[i] == "*":
                if not duplicated:
                    new_p.append("*")
                    new_index += 1 
                    duplicated = True 
            else:
                new_p.append(p[i])
                new_index += 1 
                duplicated = False 

        p = "".join(new_p)

        T = [[False for _ in range(len(p) + 1)] for _ in range(len(s) + 1)]
        
        T[0][0] = True 
        
        if len(p) > 0 and p[0] == "*":
            T[0][1] = True 
            
        for i in range(1, len(s) + 1):
            for j in range(1, len(p) + 1):
                if s[i - 1] == p[j - 1] or p[j - 1] == "?":
                    T[i][j] = T[i - 1][j - 1]
                elif p[j - 1] == "*":
                    T[i][j] = T[i - 1][j] or T[i][j - 1]
                else:
                    T[i][j] = False
        return T[len(s)][len(p)]
                
                    
            

```


```python
# Python 3: dynamic programming 
class Solution:
    """
    @param s: A string
    @param p: A string includes "?" and "*"
    @return: A boolean
    """
    def isMatch(self, s, p):
        n = len(s)
        m = len(p)
        # f is (n + 1) lists of [False, False ..., False] of length (m + 1)
        f = [[False] * (m + 1) for i in range(n + 1)]
        
        # initialize f[0][0] as True 
        f[0][0] = True
        
        # if source is empty, pattern contains only *
        if n == 0 and p.count('*') == m:
            return True
        
        # loop over all elements  
        for i in range(0, n + 1):
            for j in range(0, m + 1):
                if i > 0 and j > 0:
                    f[i][j] |= f[i-1][j-1] and (s[i-1] == p[j-1] or p[j - 1] in ['?', '*'])

                if i > 0 and j > 0:
                    f[i][j] |= f[i - 1][j] and p[j - 1] == '*'
                    
                # i = 0, j > 0, f[i][j] is initialized as False for any i, j except i = 0, j= 0 
                if j > 0:
                    f[i][j] |= f[i][j - 1] and p[j - 1] == '*'


        return f[n][m]
```


```python
[[False] * (1 + 1) for i in range(2 + 1)]
```




    [[False, False], [False, False], [False, False]]



154 -  Regular Expression Matching

Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.

'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).


The function prototype should be:

`bool isMatch(string s, string p)`

Examples: 

isMatch("aa","a") → false

isMatch("aa","aa") → true

isMatch("aaa","aa") → false

isMatch("aa", "a*") → true

isMatch("aa", ".*") → true

isMatch("ab", ".*") → true

isMatch("aab", "c*a*b") → true


```python
f = False
f |= True and False
f
```




    False




```python
# Dynamic programming 
class Solution:
    """
    @param s: A string 
    @param p: A string includes "." and "*"
    @return: A boolean
    """
    def isMatch(self, s, p):
        
        T = [[False for _ in range(0, len(p) + 1)] for _ in range(0, len(s) + 1)]
        
        # initialization 
        T[0][0] = True 
        
        # a*, a*b*, a*b*c* 
        for i in range(1, len(p) + 1):
            if p[i - 1] == "*":
                T[0][i] = T[0][i - 2]
        
        
        for i in range(1, len(s) + 1):
            for j in range(1, len(p) + 1):
                if s[i - 1] == p[j - 1] or p[j - 1] == ".":
                    T[i][j] = T[i - 1][j - 1]
                elif p[j - 1] == "*":
                    T[i][j] = T[i][j - 2]
                    if s[i - 1] == p[j - 2] or p[j - 2] == ".":
                        T[i][j] = T[i - 1][j] or T[i][j]
                else:
                    T[i][j] = False 
        # print(T)
        return T[len(s)][len(p)]
                
```

107 -  Word Break

Given a string `s` and a dictionary of words `dict`, determine if `s` can be break into a space-separated sequence of one or more dictionary words.

Example 1:

	Input:  "lintcode", ["lint", "code"]
    
	Output:  true



```python
# Findout all possible solutions, then tell if there solutions list is empty. Takes more time 
class Solution:
    """
    @param: s: A string
    @param: dict: A dictionary of words dict
    @return: A boolean
    """
    def wordBreak(self, s, dict):
        if len(s) == 0:
            return True 
        paritions = self.dfs(s, dict, {})
        
        return len(paritions) > 0
    
    def dfs(self, s, dict, memo):
        if s in memo:
            return memo[s]
        partitions = []
        
        for i in range(0, len(s) - 1):
            prefix = s[ :i + 1]
            # print('prefix', prefix)
            if prefix not in dict:
                continue 
            sub_paritions = self.dfs(s[i + 1: ], dict, memo)
            # print(sub_paritions)
            for partition in sub_paritions:
                partitions.append(prefix + " " + partition)
        if s in dict:
            partitions.append(s)
        memo[s] = partitions
        return partitions 
        
```


```python
# Python 2: dynamic programming 
class Solution:
    """
    @param: s: A string
    @param: dict: A dictionary of words dict
    @return: A boolean
    """
    def wordBreak(self, s, dict):
        dp = [False for _ in range(len(s) + 1)]
        dp[0] = True 
        
        for i in range(1, len(s) + 1):
            for k in range(i):
                if dp[k] and s[k:i] in dict:
                    dp[i] = True 
        return dp[len(s)]
```


```python
# Python 3: dynamic programming, take the max possible length of word into account to save time for very long string 
class Solution:
    """
    @param: s: A string
    @param: dict: A dictionary of words dict
    @return: A boolean
    """
    def wordBreak(self, s, dict):
        if len(s) == 0:
            return True 
        if len(dict) == 0:
            return False 
            
        dp = [False for _ in range(len(s) + 1)]
        dp[0] = True 
        
        max_len = max([len(w) for w in dict])
        for i in range(1, len(s)+ 1):
            for j in range(1, min(i, max_len) + 1):
                if not dp[i - j]:
                    continue 
                if s[i - j: i] in dict:
                    dp[i] = True

        return dp[len(s)]
```

582 - Word Break II


Given a string `s` and a dictionary of words `dict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

Example

Gieve s = leetcode,

dict = ["de", "ding", "co", "code", "leet"].

A solution is ["leet code", "leet co de"].



```python
class Solution:
    """
    @param: s: A string
    @param: wordDict: A set of words.
    @return: All possible sentences.
    """
    def wordBreak(self, s, wordDict):
        if len(s) == 0:
            return []
        if len(wordDict) ==0 :
            return []
        
        return self.dfs(s, wordDict, {})
    
    def dfs(self, s, wordDict, memo):
        # if the partition of s has been computed before, pull the results directly
        if s in memo:
            return memo[s]
            
        partitions = []
        
        # split s 
        for i in range(0, len(s) - 1):
            prefix = s[ :i + 1]
            if prefix not in wordDict:
                continue
            # partition the rest of the string 
            sub_partitions = self.dfs(s[i + 1: ], wordDict, memo)
            
            # add the partitions of the rest of string 
            for partition in sub_partitions:
                partitions.append(prefix + " " + partition)
        # case when s itself is in the dictionary        
        if s in wordDict:
            partitions.append(s)
            
        memo[s] = partitions
        return partitions
```


```python
Solution().wordBreak('leetcode',  ["de", "ding", "co", "code", "leet", 'leetcode'])
```




    ['leet co de', 'leet code', 'leetcode']




```python
a = [""]
len(a)
a == ''
```




    False



683 - Word Break III

Give a dictionary of words and a sentence with all whitespace removed, return the number of sentences you can form by inserting whitespaces to the sentence so that each word can be found in the dictionary.


```python
# Python 1: same as wordBreak II but change the return value to length of parition solutions 
class Solution:
    """
    @param: : A string
    @param: : A set of word
    @return: the number of possible sentences.
    """
        
    def wordBreak3(self, s, wordDict):
        if len(s) == 0:
            return 0
        if len(wordDict) ==0:
            return 0
        # change to lower case 
        s = s.lower()
        wordDict = [x.lower() for x in wordDict]

        return len(self.dfs(s, wordDict, {}))
    
    def dfs(self, s, wordDict, memo):
        # if the partition of s has been computed before, pull the results directly
        if s in memo:
            return memo[s]
            
        partitions = []
        
        # split s 
        for i in range(0, len(s) - 1):
            prefix = s[ :i + 1]
            if prefix not in wordDict:
                continue
            # partition the rest of the string 
            sub_partitions = self.dfs(s[i + 1: ], wordDict, memo)
            
            # add the partitions of the rest of string 
            for partition in sub_partitions:
                partitions.append(prefix + " " + partition)
        # case when s itself is in the dictionary        
        if s in wordDict:
            partitions.append(s)
            
        memo[s] = partitions
        return partitions
```

$$dp[i][j] = \sum_{k=i}^{j} dp[i][k] * dp[k+1][i]$$


```python
# Python 2: dynamic programming 
class Solution:
    """
    @param: : A string
    @param: : A set of word
    @return: the number of possible sentences.
    """

    def wordBreak3(self, s, dict):
        
        if not s or not dict:
            return 0
            
        n, hash_set = len(s), set()
        
        s = s.lower()
        for word in dict:
            hash_set.add(word.lower())
            
        counts = [[0] * n for _ in range(n)]
        for i in range(n):
            for j in range(i, n):
                sub = s[i : j + 1]
                if sub in hash_set:
                    counts[i][j] = 1
                    
        for i in range(n):
            for j in range(i, n):
                for k in range(i, j):
                    counts[i][j] += counts[i][k] * counts[k + 1][j]
                    
        return counts[0][-1]
```

652 -  Factorization

A non-negative numbers can be regarded as product of its factors.

Write a function that takes an integer `n` and return all possible combinations of its factors.

Elements in a combination `(a1, a2, … , ak)` must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
The solution set must not contain duplicate combination.

Example: 

Input: 8

Output: [[2,2,2],[2,4]]

Explanation:

8 = 2 x 2 x 2 = 2 x 4


```python
class Solution:
    """
    @param n: An integer
    @return: a list of combination
    """
    def getFactors(self, n):
        if n <= 1:
            return n 
        results = []
        self.dfs(2, n, [], results)
        return results 
        
    def dfs(self, start, remain, factors, results):
        # return when remain == 1 and factors list includes multiple values (not n itself) 
        if remain == 1 and len(factors) > 1:
            results.append(list(factors))
         
        
        for i in range(start, remain):
            # iterate up to i**2 <= remain to make sure the list is ascending order 
            if i > remain / i:
                break
            if remain % i == 0:
                factors.append(i)
                self.dfs(i, remain // i, factors, results)
                factors.pop()
        # e.g., n = 8 --> dfs(2, 8) --> dfs(2, 4) --> dfs(2, 2), start == remain, but was excluded  from for loop
        # e.g., n = 8 --> dfs(2, 8) --> dfs(2, 4) --> [2, 4]
        if remain >= start:
            factors.append(remain)
            self.dfs(remain, 1, factors, results)
            factors.pop()
            
```

427 - Generate Parentheses

Given `n`, and there are `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Example: 

Input: 3

Output: ["((()))", "(()())", "(())()", "()(())", "()()()"]


```python
# Python 1: use dfs 
class Solution:
    """
    @param n: n pairs
    @return: All combinations of well-formed parentheses
    """
    def generateParenthesis(self, n):
        if not n or n == 0:
            return []
        results = []
        self.dfs(n, [], results)
        return results 
    
    def dfs(self, n, subset, results):
        
        # number of left parentheses 
        left_num = sum([x == "(" for x in subset])
        
        # append to results if its a valid combination 
        if len(subset) == n * 2 and left_num == n:
            results.append("".join(subset))
         
        # invalid combination: # of left parentheses < # of right parentheses 
        if left_num < len(subset) - left_num:
            return 
        
        
        # number of left parentheses should be smaller than n 
        if left_num <= n:
            subset.append("(")
            self.dfs(n, subset, results)
            subset.pop()
        
        # number of right parentheses should be smaller than n 
        if len(subset) - left_num <= n:
            subset.append(")")
            self.dfs(n, subset, results)
            subset.pop()
    
```


```python
# Python 2: dfs in another way 
class Solution:
    # @param an integer
    # @return a list of string
    # @draw a decision tree when n == 2, and you can understand it!
    def generateParenthesis(self, n):
        if n == 0:
            return []
        
        results = []
        self.dfs(n , n, "", results)
        return results 
    
    def dfs(self, left_num_need, right_num_need, subset, results):
        if left_num_need > right_num_need:
            return 
            
        if left_num_need == 0 and right_num_need == 0:
            results.append(str(subset))
        
        if left_num_need > 0:
            self.dfs(left_num_need - 1, right_num_need, subset + "(", results)
            
        if right_num_need > 0:
            self.dfs(left_num_need, right_num_need - 1, subset + ")", results)
        
```

780 - Remove Invalid Parentheses

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

The input string may contain letters other than the parentheses ( and ).


Example: 

Input:
"(a)())()"

Output:
 ["(a)()()", "(a())()"]
 
 Input:
")(" 

Output:
 [""]


```python
class Solution:
    def removeInvalidParentheses(self, s):
        results = []
        left, right = self.removed_counts(s)
        self.dfs(s, left, right, 0, results)
        return results
        
        
    
    def dfs(self, s, left, right, start, results):
        if left == 0 and right == 0 and self.is_valid(s):
            results.append(s)
            return
        
        for i in range(start, len(s)):
            # remove duplicates 
            if i > start and s[i] == s[i-1]:
                continue
            # remove illegal left parentheses 
            if s[i] == '(' and left > 0:
                self.dfs(s[ :i]+s[i + 1: ], left - 1, right, i, results)
            # remove illegal right parantheses 
            if s[i] == ')' and right > 0:
                self.dfs(s[ :i]+s[i + 1:], left, right - 1, i, results)
            # if s[i] is neither "(" nor ")", jump to the next i loop 
                
    # tell if all parentheses are legal 
    def is_valid(self, s):
        left, right = self.removed_counts(s)
        return left == 0 and right == 0 
    
    # count how many illegal left/right parentheses 
    # eg. ')(', illegal left and right parentheses are 1, 1 
    def removed_counts(self, s):
        left, right = 0, 0 
        for char in s:
            if char == "(":
                left += 1 
            if char == ")":
                if left > 0:
                    left -= 1 
                else:
                    right += 1 
        return left, right 
```

196 -  Missing Number

Given an array contains `N` numbers of `0 .. N`, find which number doesn't exist in the array.

Example: 

Input:[0,1,3]

Output:2


```python
# Python 1: use an external set 
class Solution:
    """
    @param nums: An array of integers
    @return: An integer
    """
    def findMissing(self, nums):
        if nums is None:
            return -1 
        n = len(nums)
        
        # use set to romove element in O(1) 
        all_nums = set([i for i in range(n + 1)])
        
        for num in nums:
            all_nums.remove(num)
            
        if len(all_nums) == 1:
            return list(all_nums)[0]      

```

570 - Find the Missing Number II

Giving a string with numbers in range `[1, n]` in random order, but miss `1` number.Find that number.

Example: 

Input: n = 20 and str = 19201234567891011121314151618

Output: 17

Explanation:
19'20'1'2'3'4'5'6'7'8'9'10'11'12'13'14'15'16'18


```python
class Solution:
    """
    @param n: An integer
    @param str: a string with number from 1-n in random order and miss one number
    @return: An integer
    """
    def findMissing2(self, n, string):
        # use exists[1:n+1] to store the status of if 1, ..,n exists 
        exists = [False for _ in range(n + 1)]
        return self.dfs(n, string, 0, exists)
    
    def dfs(self, n, string, start_index, exists):
        # when iterated to the end of string 
        if start_index == len(string):
            results = []
            # check which number is missing 
            for i in range(1, n + 1):
                if not exists[i]:
                    results.append(i)
            # if everything is correct, there should be only one number in results 
            return results[0] if len(results) == 1 else -1 
        
        # numbers are always in [1, n]
        if string[start_index] == '0':
            return -1 
        
        # search the next number, can be one digit or two digits 
        for i in [1, 2]:
            if start_index + i <= len(string):
                num = int(string[start_index:(start_index + i)])
                # if the num is between [1, n] and doesn't exist yet 
                if num >= 1 and num <= n and not exists[num]:
                    exists[num] = True
                    next_vals = self.dfs(n, string, start_index + i, exists)
                    if next_vals != -1:
                        return next_vals
                    # backtrack 
                    exists[num] = False
        return -1 

```

#### An unrelated max swap question 

1095  Maximum Swap

Given a non-negative integer. You could choose to swap two digits of it. Return the maximum valued number you could get.

Example: 

Input: 2736

Output: 7236

Explanation: Swap the number 2 and the number 7.


```python
class Solution:
    def maximumSwap(self, num):
        new_num = num 

        num = list(str(num))
        
        for i in range(len(num) - 1):
            for j in range(i + 1, len(num)):
                if int(num[j]) > int(num[i]):
                    swapped = int("".join(num[ :i] + [num[j]] + num[i + 1 : j] + [num[i]] + num[j + 1: ]))
                    if swapped > new_num:
                        print(swapped)
                        new_num = swapped 
        return new_num 
        
```


```python

```
