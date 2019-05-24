
190 - Next Permutation II


Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).


Input:1,2,3
    
Output:1,3,2

See https://en.wikipedia.org/wiki/Lexicographical_order and https://baike.baidu.com/item/%E5%AD%97%E5%85%B8%E5%BA%8F. 


* Permutation, DFS

Steps with an example: 

Current nums: 12642

* Find the first element, `a`, at index `i`,  that is smaller than the elements on its right: 1**2**642, a = 2, i = 1

* Find an element, `b`, at index `j`,that is smallest value bigger than `a` on `a`'s right:126**4**2, b = 4, j = 3

* Swap `a` and `b`: 14622

* Reverse elements after index `i`: 14226

Corner case: 

Current nums is already the largest: 321, reverse the current nums directly.


```python
class Solution:
    """
    @param nums: An array of integers
    @return: nothing
    """
    def nextPermutation(self, nums):
        n = len(nums)
        
        if n <= 1:
            return nums 
        
        # from right to left, find the last element index that is bigger than its right neighbor 
        i_ = n - 1
        
        # use i_ > 0 to avoid cases like [1, 1, 1, 1, ..., 1], to make sure i_ - 1 is valid ( >= 0 )
        while i_ > 0 and nums[i_ - 1] >= nums[i_]:
            i_ -= 1 
        # the first element index that is smaller than its right neighbor 
        i = i_ - 1 
        # print(i)
        if i < 0:
            left, right = 0, len(nums) - 1 
            while left < right:
                self.swap(nums, left, right)
                left += 1 
                right -= 1 
            return 

        # search from i + 1 to n - 1 to find the digits that is cloest to nums[i]
        j = i + 1 
        while j < n - 1 and nums[j] >= nums[j + 1] and nums[j + 1] > nums[i]:
            j += 1 
        # print(j)
        self.swap(nums, i, j)
        # print(nums)
        left, right = i + 1, len(nums) - 1 
        while left < right:
            self.swap(nums, left, right)
            left += 1 
            right -= 1 
        print(nums)
            
        
    def swap(self, nums, left, right):
            nums[left], nums[right] = nums[right], nums[left]

    
```


```python
p = Solution()
p.nextPermutation([1, 3, 2])
```

    [2, 1, 3]


52 - Next Permutation
 
Given a list of integers, which denote a permutation.

Find the next permutation in ascending order.


```python
# same as the above problem, the only difference is that it does require a return value instead of 
# finding the next permutation in-place 
class Solution:
    """
    @param nums: An array of integers
    @return: nothing
    """
    def nextPermutation(self, nums):
        n = len(nums)
        
        if n <= 1:
            return nums 
        
        # from right to left, find the last element that is bigger than its right neighbor 
        i_ = n - 1
        
        # use i_ > 0 to avoid cases like [1, 1, 1, 1, ..., 1], to make sure i_ - 1 is valid ( >= 0 )
        while i_ > 0 and nums[i_ - 1] >= nums[i_]:
            i_ -= 1 
        # the first element that is smaller than its right neighbor 
        i = i_ - 1 
        # print(i)
        if i < 0:
            left, right = 0, len(nums) - 1 
            while left < right:
                self.swap(nums, left, right)
                left += 1 
                right -= 1 
            return 

        # search from i + 1 to n - 1 to find the digits that is cloest to nums[i]
        j = i + 1 
        while j < n - 1 and nums[j] >= nums[j + 1] and nums[j + 1] > nums[i]:
            j += 1 
        # print(j)
        self.swap(nums, i, j)
        # print(nums)
        left, right = i + 1, len(nums) - 1 
        while left < right:
            self.swap(nums, left, right)
            left += 1 
            right -= 1 
        print(nums)
            
        
    def swap(self, nums, left, right):
            nums[left], nums[right] = nums[right], nums[left]

```

51 - Previous Permutation
 
Given a list of integers, which denote a permutation.

Find the previous permutation in ascending order.

Example: 
    
    Input:[1,3,2,3]
        
Output:[1,2,3,3]


```python
# the reverse process of next permutation above 
class Solution:
    """
    @param: nums: A list of integers
    @return: A list of integers that's previous permuation
    """
    def previousPermuation(self, nums):
        n = len(nums)
        if n <= 1:
            return nums 
        # find the last element index that is smaller than its right neighbor from right to left 
        i_ = n - 1
        while i_ > 0 and nums[i_ - 1] <= nums[i_]:

            i_ -= 1 
        
        # find the first element index that is bigger than its right neighbor  
        i = i_ - 1
        
        # if i < 0 (e.g., [1, 2, 3]), reverse the list  
        if i < 0:
            return nums[::-1]
        
        # reverse substrings from index + 1, so elements in [index+ 1: ] are now in ascending order 
        left, right = i + 1, len(nums) - 1 
        while left < right:
            self.swap(nums, left, right)
            left += 1
            right -= 1 
        
        # find the index of biggest element that is smaller than nums[i] after index (i + 1)  
        j = i + 1 
        while j < len(nums) - 1 and nums[j] >= nums[i]:
            j += 1 
        
        # swap 
        self.swap(nums, i, j)
        return nums 
            
        
    def swap(self, nums, left, right):
            nums[left], nums[right] = nums[right], nums[left]

```

197 - Permutation Index

Given a permutation which contains no repeated number, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.

Example: 
    
Input:[3,2,1]
    
Output:6


```python
class Solution:
    # @param {int[]} A an integer array
    # @return {long} a long integer
    def permutationIndex(self, A):
        result = 1
        factor = 1
        for i in range(len(A) - 1, -1, -1):
            rank = 0
            for j in range(i + 1, len(A)):
                if A[i] > A[j]:
                    rank += 1
            result += factor * rank
            factor *= len(A) - i
        return result

```

198 - Permutation Index II


Same as above but there may be duplicated numbers.


```python
class Solution:
    # @param {int[]} A an integer array
    # @return {long} a long integer
    def permutationIndexII(self, A):
        if A is None or len(A) == 0:
            return 0

        index, factor, multi_fact = 1, 1, 1
        counter = {}
        # iterate 
        for i in range(len(A) - 1, -1, -1):
            if A[i] not in counter:
                    counter[A[i]] = 0
                    
            counter[A[i]] += 1
            multi_fact *= counter[A[i]]
            rank = 0
            for j in range(i + 1, len(A)):
                if A[i] > A[j]:
                    rank += 1

            index += rank * factor / multi_fact
            factor *= (len(A) - i)

        return int(index)
```

15 - Permutations

Given a list of numbers, return all possible permutations.

You can assume that there is no duplicate numbers in the list.

* Permutation, DFS


```python
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """
    def permute(self, nums):
        results = []
        if nums is None:
            return [[]] 
        
        self.dfs(nums, [False]*len(nums), [], results)
        return results 
    
    
    def dfs(self, nums, visited, permutation, results):
        # append 
        if len(permutation) == len(nums):
            results.append(list(permutation))
            
        # recursion 
        for i in range(len(nums)):
            if visited[i]:
                continue 
            permutation.append(nums[i])
            visited[i] = True 
            self.dfs(nums, visited, permutation, results)
            visited[i] = False 
            permutation.pop()

```


```python
p = Solution()
p.permute([1, 3, 2])
```




    [[1, 3, 2], [1, 2, 3], [3, 1, 2], [3, 2, 1], [2, 1, 3], [2, 3, 1]]




```python
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """
    def permute(self, nums):
        if not nums:
            return [[]]
        
        results = []
        self.search(nums, [] , set(), results)
        return results 
        
        
    def search(self, nums, path, visited, results):
        if len(path) == len(nums):
            results.append(list(path))
            return 
        
        for i in range(len(nums)):
            if nums[i] not in visited:
                path.append(nums[i])
                visited.add(nums[i])
                self.search(nums, path, visited, results)
                visited.remove(nums[i])
                path.pop()
```


```python
p = Solution()
p.permute([1, 3, 2])
```




    [[1, 3, 2], [1, 2, 3], [3, 1, 2], [3, 2, 1], [2, 1, 3], [2, 3, 1]]



16 - Permutations II

Given a list of numbers with duplicate number in it. Find all unique permutations.

Input: [1,2,2]
    
Output:
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]


* DFS, recursion


```python
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """
    def permuteUnique(self, nums):
        results = []
        if nums is None:
            return results 
        # sort nums so the duplicated numbers (if any) are together
        nums.sort()
        # dfs 
        self.dfs(nums, [False]*len(nums), [], results)
        return results 
    
    
    def dfs(self, nums, visited, permutation, results):
        # append 
        if len(permutation) == len(nums):
            results.append(list(permutation))
            
        # recursion 
        for i in range(len(nums)):
            if visited[i]:
                continue 
            # remove duplicates     
            if i > 0 and nums[i] == nums[i - 1] and not visited[i - 1]: 
                continue
            permutation.append(nums[i])
            visited[i] = True 
            self.dfs(nums, visited, permutation, results)
            visited[i] = False 
            permutation.pop()
```

10 - String Permutation II

Given a string, find all permutations of it without duplicates.

* Permutation, DFS


```python
class Solution:
    """
    @param str: A string
    @return: all permutations
    """
    def stringPermutation2(self, str):
        if str is None:
            return [[]]
        results = []
        str = sorted(list(str))
        print(str)
        self.dfs(str, [False] * len(str), [], results)
        return results 

    def dfs(self, str, visited, permutation, results):
        if len(permutation) == len(str):
            results.append(''.join(permutation))
            
        # recursion 
        for i in range(len(str)):
            if visited[i]:
                continue 
            # remove duplicates 
            if (i > 0 and str[i] == str[i - 1] and not visited[i - 1]):
                continue 
            permutation.append(str[i])
            visited[i] = True 
            self.dfs(str, visited, permutation, results)
            visited[i] = False 
            permutation.pop()
            

```


```python
p = Solution()
p.stringPermutation2('abab')
```

    ['a', 'a', 'b', 'b']





    ['aabb', 'abab', 'abba', 'baab', 'baba', 'bbaa']



211 -  String Permutation

Given two strings, write a method to decide if one is a permutation of the other.


Example 1:

	Input:  "abcd", "bcad"
    
	Output:  True



```python
class Solution:
    """
    @param A: a string
    @param B: a string
    @return: a boolean
    """
    def Permutation(self, A, B):
        a = list(A)
        b = list(B)
        a.sort()
        b.sort() 
        
        return "".join(a) == "".join(b)

```


```python
Solution().Permutation("^&*#$@%@%@$%@$!*&*&*)))!)()())( **jsafhjhsajfhthisisa lint", "^&)!)(%))thijhsajfhs)())( **jsafh*#$@%@$!*&*&sa lint*@%@$i")
```

    []
    ['^']
    ['^', '&']
    ['^', '&', ')']
    ['^', '&', ')', '!']
    ['^', '&', ')', '!', ')']
    ['^', '&', ')', '!', ')', '(']
    ['^', '&', ')', '!', ')', '(', '%']
    ['^', '&', ')', '!', ')', '(', '%', ')']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't', '*']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't', '*', '@']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't', '*', '@', '%']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't', '*', '@', '%', '@']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't', '*', '@', '%', '@', '$']
    ['^', '&', ')', '!', ')', '(', '%', ')', ')', 't', 'h', 'i', 'j', 'h', 's', 'a', 'j', 'f', 'h', 's', ')', '(', ')', ')', '(', ' ', '*', '*', 'j', 's', 'a', 'f', 'h', '*', '#', '$', '@', '%', '@', '$', '!', '*', '&', '*', '&', 's', 'a', ' ', 'l', 'i', 'n', 't', '*', '@', '%', '@', '$', 'i']





    True




```python
# DFS: Takes very long as it finds all possible solutions, not recommended 
class Solution:
    """
    @param A: a string
    @param B: a string
    @return: a boolean
    """
    def Permutation(self, A, B):
        if not A and not B:
            return True 
        if not A or not B:
            return False 
        if len(A) != len(B):
            return False 
            
        visited = [False for _ in range(len(A))]
        return self.dfs(A, [], B, visited)
        
    def dfs(self, A, path, B, visited):
        print(path)
        if len(path) == len(B):
            if "".join(path) == B:
                return True 
        if len(path) > len(B):
            return 
        
        for i in range(len(A)):
            if visited[i]:
                continue
            if A[i] != B[len(path)]:
                continue 
            path.append(A[i])
            visited[i] = True 
            if self.dfs(A, path, B, visited):
                return True 
            else: 
                visited[i] = False 
                path.pop()
            

```


```python
# Can also use a prefix set to record all prefix possibilities 
```

425 - Letter **Combinations** of a Phone Number

Given a digit string excluded '0' and '1', return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

| 1 | 2 | 3 |
|---|---|---|
|---|ABC|DEF|
| 4 | 5 | 6 |
|GHI|JKL|MNO|
| 7| 8 | 9 |
|PQRS|TUV|WXYZ|


Example: 

Input: "23"

Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]


* Backtracking, recursion


```python
letters_dict = {
    '2':['a', 'b', 'c'], 
    '3':['d', 'e', 'f'],
    '4':['g', 'h', 'i'],
    '5':['j', 'k', 'l'],
    '6':['m', 'n', 'o'],
    '7':['p', 'q', 'r', 's'],
    '8':['t', 'u', 'v'],
    '9':['w', 'x','y','z']
}

class Solution:
    """
    @param digits: A digital string
    @return: all posible letter combinations
    """
    def letterCombinations(self, digits):
        if digits is None or len(digits) == 0:
            return []
        
        results = []
        self.dfs(digits, 0, "", results)
        return results 
    
    
    def dfs(self, digits, index, permutation, results):
        # append 
        if index == len(digits):
            results.append(''.join(permutation))
            return  # avoid unlimited running time 
        
        
        for letters in (letters_dict[digits[index]]):
                # string can not use append, use + 
            self.dfs(digits, index + 1, permutation + letters, results)
                
    

```


```python
p = Solution()
p.letterCombinations('23')
```




    ['ad', 'ae', 'af', 'bd', 'be', 'bf', 'cd', 'ce', 'cf']



33 - N-Queens

The n-queens puzzle is the problem of placing `n` queens on an `n×n` chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.'  indicate a queen and an empty space respectively.


Example: 

Input:4

Output:

[

  // Solution 1
  
  [".Q..",
  
   "...Q",
   
   "Q...",
   
   "..Q."
   
  ],
  
  // Solution 2
  
  [
  
  "..Q.",
  
   "Q...",
   
   "...Q",
   
   ".Q.."
   
  ]
]



```python
# Python 1:
class Solution:
    """
    @param: n: The number of queens
    @return: All distinct solutions
    """
    def solveNQueens(self, n):
        if not n:
            return []
        self.results = []
        self.search(n, 0, set(), set(), set(), [])
        return self.results
    
    
    def search(self, n, row, cols, diffs, sums, path):
        if row == n:
            self.results.append(self.draw_board(path))
            return 
        for col in range(n):
            if col in cols or (row - col) in diffs or (row + col) in sums:
                continue 
            cols.add(col)
            diffs.add(row - col)
            sums.add(row + col)
            path.append(col)
            self.search(n, row + 1, cols, diffs, sums, path)
            path.pop()
            sums.remove(row + col)
            diffs.remove(row - col)
            cols.remove(col)
            
            
    def draw_board(self, path):
        res = []
        for  col in path:
            temp = ["." for _ in range(len(path))]
            temp[col] = "Q"
            res.append("".join(temp))
        return res 
    
```


```python
# Python 2
class Solution:
    """
    @param: n: The number of queens
    @return: All distinct solutions
    """
    def solveNQueens(self, n):
        if n is None:
            return []
        
        results = []
        self.search(n, [], results)
        return results 
    
    # locations = 1, 3, 2, 4 means that the queen is at the col 1 in first row, col 3 in second row ... 
    def search(self, n, locations, results):
        row = len(locations)
        # if the n-th queen location has been found 
        if row == n:
#             print('results', locations)
            results.append(self.draw_board(locations)) # convert the numerical locations to chessboard 
            return 
        # dfs, for each row, check col 1 - n to see if it's a valid one 
        for col in range(n):
            if not self.is_valid(locations, row, col):
                continue 
            # if valid 
            locations.append(col)
            self.search(n, locations, results)
            locations.pop()
            
    def is_valid(self, locations, row, col):
        for prev_row, prev_col in enumerate(locations):
            if prev_col == col:
                return False
            if prev_row - prev_col == row - col or prev_row + prev_col == row + col:
                return False 
        return True 
        
    def draw_board(self, locations):
        n = len(locations)
        board = []
        for i in range(n):
            cur_row = ['Q' if j == locations[i] else '.' for j in range(n)]
            board.append(''.join(cur_row))  # convert the separate list elements into a single string
        return board 
    
```


```python
p = Solution()
p.solveNQueens(4)
```




    [['.Q..', '...Q', 'Q...', '..Q.'], ['..Q.', 'Q...', '...Q', '.Q..']]




```python
for r, c in enumerate([1, 3, 0, 1]):
    print('r', r, 'c', c)
```

    r 0 c 1
    r 1 c 3
    r 2 c 0
    r 3 c 1



```python
a = []
a.append([4])
a
```




    [[4]]



34 - N-Queens II

Now, instead outputting board configurations, return the total number of distinct solutions.

Example: 

Input: n=4

Output: 2

Explanation:


* Recursion, DFS


```python
# Python 1: dfs, for each row, check which position is eligible to put a queen 
# 1. cannot in the same column 
# 2. cannot in diagonal from left to right 
# 3. cannot in diagonal from right to left 
class Solution:
    """
    @param n: The number of queens.
    @return: The total number of distinct solutions.
    """
    def totalNQueens(self, n):
        if not n:
            return 0 
        
        self.count = 0 
        self.search(n, 0, set(), set(), set())
        return self.count
    
    
    def search(self, n, row, cols, diffs, sums):
        if row == n:
            self.count += 1 
            return 
        for col in range(n):
            if col in cols or (row - col) in diffs or (row + col) in sums:
                continue 
            cols.add(col)
            diffs.add(row - col)
            sums.add(row + col)
            self.search(n, row + 1, cols, diffs, sums)
            sums.remove(row + col)
            diffs.remove(row - col)
            cols.remove(col)
        
```


```python
class Solution:
    """
    @param: n: The number of queens
    @return: All distinct solutions
    """
    def totalNQueens(self, n):
        if n is None:
            return []
        
        self.count = 0
        self.search(n, [])
        return self.count 
    
    # locations = 1, 3, 2, 4 means that the queen is at the col 1 in first row, col 3 in second row ... 
    def search(self, n, locations):
        row = len(locations)
        # if the n-th queen location has been found 
        if row == n:
            self.count += 1
#             print('locations', locations)
            return # end of one recursion
        
        # dfs, for each row, check col 1 - n to see if it's a valid one 
        for col in range(n):
            if not self.is_valid(locations, row, col):
                continue 
            # if valid 
            locations.append(col)
            self.search(n, locations)
            locations.pop()
            
    def is_valid(self, locations, row, col):
        for prev_row, prev_col in enumerate(locations):
            if prev_col == col:
                return False
            if prev_row - prev_col == row - col or prev_row + prev_col == row + col:
                return False 
        return True 
        
#     def draw_board(self, locations):
#         n = len(locations)
#         board = []
#         for i in range(n):
#             cur_row = ['Q' if j == locations[i] else '.' for j in range(n)]
#             board.append(''.join(cur_row))  # convert the separate list elements into a single string
#         return board 
    
```


```python
p = Solution()
p.totalNQueens(4)
```

    locations [1, 3, 0, 2]
    locations [2, 0, 3, 1]





    2



828 -  Word Pattern

Given a pattern and a string `str`, find if `str` follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in `str`.

You may assume pattern contains only lowercase letters, and `str` contains lowercase letters separated by **a single space**.


Example: 

Input:  pattern = "abba" and str = "dog cat cat dog"

Output: true

Explanation:
The pattern of str is abba


```python
class Solution:
    """
    @param pattern: a string, denote pattern string
    @param teststr: a string, denote matching string
    @return: an boolean, denote whether the pattern string and the matching string match or not
    """
    def wordPattern(self, pattern, teststr):
        maps = {}
        used = set()
        split_str = teststr.split(" ")
        # print(split_str)
        if len(pattern) != len(split_str):
            return False 
        for i in range(len(pattern)):
            # print(maps)
            # print(used)
            # if pattern[i] has not been mapped and the i-th word is not used 
            if pattern[i] not in maps and split_str[i] not in used:
                maps[pattern[i]] = split_str[i]
                used.add(split_str[i])
            # if pattern[i] has not been mapped but i-th word has been mapped with other pattern
            elif pattern[i] not in maps and split_str[i] in used:
                return False 
            else:
                # if pattern[i] has been mapped but the mapped word is not the same as the i-th word 
                if maps[pattern[i]] != split_str[i]:
                    return False
                    
        return True 

```

829 - Word Pattern II

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here `follow` means a full match, such that there is a bijection between a letter in pattern and a non-empty substring in `str`.(i.e if `a` corresponds to `s`, then `b` cannot correspond to `s`. For example, given pattern = "ab", str = "ss", return false.)

Example: 

Given pattern = "abab", str = "redblueredblue", return true.

Given pattern = "aaaa", str = "asdasdasdasd", return true.

Given pattern = "aabb", str = "xyzabcxzyabc", return false.


* Backtracking


```python
class Solution:
    """
    @param pattern: a string,denote pattern string
    @param str: a string, denote matching string
    @return: a boolean
    """
    def wordPatternMatch(self, pattern, string):
        return self.is_match(pattern, string, {}, set())
    
    
    def is_match(self, pattern, string, maps, used):
        
        # check if string and pattern are empty 
        # important: when the input pattern is None, check if the string is consistent
        # note: not None == not False == not '' == not 0 == not [] == not {} == not () ~ False 
        # here pattern could be empty string, but empty string is not None 
        if not pattern:
            print('pattern',pattern is None)
            return not string  
                      
        # select a character in pattern and test for matched words in string 
        char = pattern[0]
        
        
        # when the char has been assigned with a substr in string, only need to skip the substr and continue  
        if char in maps:
            mapped_str = maps[char]
            # if the string doesn't start with mapped_word, cannot be matched, return false 
            if not string.startswith(mapped_str):
                return False
            
            # if they match, go directly to next step , no need to add the already added word to map
            return self.is_match(pattern[1:], string[(len(mapped_str)):], maps, used)
        
        # when char has NOT been assigned with a substr, dfs 
        for i in range(len(string)):
            test_str = string[:(i + 1)]
            
            # the words has been used before 
            if test_str in used:
                continue

            maps[char] = test_str 
            used.add(test_str) # cannot be paired with another character 
            if self.is_match(pattern[1:], string[(i + 1):], maps, used):
                return True 
            # backtracking if the current pattern pairing cannot return a True 
            used.remove(test_str)
            del maps[char]
        return False 
            

```


```python
Solution().wordPatternMatch('ab', 'redblue')

```

    pattern False
    pattern False
    pattern False
    pattern False
    pattern False
    pattern False





    True



121 - Word Ladder II


Given two words (start and end), and a dictionary, find all **shortest** transformation sequence(s) from start to end, such that:

* Only one letter can be changed at a time

* Each intermediate word must exist in the dictionary

Example: 

Given:

start = "hit"

end = "cog"

dict = ["hot","dot","dog","lot","log"]

Return

[
  ["hit","hot","dot","dog","cog"],
  
  ["hit","hot","lot","log","cog"]
]


* All words have the same length.

* All words contain only lowercase alphabetic characters.

* BFS, DFS, backtracking 



```python
### Python, first use BFS then use DFS

# Note: when finding shortest path, find the distances from END to START to make sure that all words selected in dfs 
#      future can reach END 
from collections import deque

class Solution:
    """
    @param: start: a string
    @param: end: a string
    @param: dict: a set of string
    @return: a list of lists of string
    """
    def findLadders(self, start, end, dict):
        # add start and end into the dictionary 
        dict.add(start)
        dict.add(end)
        
        # first do the bfs from end to start to find the shortest paths
        # distance is used to save 
        distance = {}
        self.bfs(end, start, distance, dict)
        print('distance', distance)
        # dfs 
        results = []
        self.dfs(end, distance, dict, [start], results)
        return results 
    
    def bfs(self, start, end, distance, dict):
        # the start itself 
        distance[start] = 0
        
        # queue 
        queue = deque([start])
        
        while queue:
            cur_word = queue.popleft()
            for neighbor in self.get_next_words(cur_word, dict):
                if neighbor in distance:
                    continue
                distance[neighbor] = distance[cur_word] + 1 
                queue.append(neighbor)
        return distance 
                
    def get_next_words(self, word, dict):
        next_words = set() # to avoid duplication
        for i in range(len(word)):
            for char in 'abcdefghijklmnopqrstuvwxyz':
                test_word = word[:i] + char + word[(i + 1):]
                if test_word in dict:
                    next_words.add(test_word)
        # remove word itself 
        next_words.remove(word)
        return next_words
    
    def dfs(self, end, distance, dict, path ,results):
        # return statement 
        if path[-1] == end:
            results.append(list(path))
            return
            
#         next_words = [words for words in distance if distance[words] == distance[path[-1]] - 1]
        next_words = self.get_next_words(path[-1], dict)
        next_words = [words for words in next_words if distance[words] == distance[path[-1]] - 1]
        print('cur words', path[-1])
        print('next_words', next_words)
        for word in next_words:
            path.append(word)
            self.dfs(end, distance, dict, path, results)
            path.pop()
        
        
```


```python
Solution().findLadders('hit', 'cog', {"hot","dot","dog","lot","log"})
# Solution().bfs('hit', 'dot',{},{"hot","dot","dog","lot","log"} )
```

    distance {'cog': 0, 'dog': 1, 'log': 1, 'dot': 2, 'lot': 2, 'hot': 3, 'hit': 4}
    cur words hit
    next_words ['hot']
    cur words hot
    next_words ['lot', 'dot']
    cur words lot
    next_words ['log']
    cur words log
    next_words ['cog']
    cur words dot
    next_words ['dog']
    cur words dog
    next_words ['cog']





    [['hit', 'hot', 'lot', 'log', 'cog'], ['hit', 'hot', 'dot', 'dog', 'cog']]



132 - Word Search II

Given a matrix of lower alphabets and a dictionary. Find all words in the dictionary that can be found in the matrix. A word can start from any position in the matrix and go left/right/up/down to the adjacent position. One character only be used once in one word. No same word in dictionary

* Trie, prefix tree

Given matrix:

doaf

agai

dcan

and dictionary:

{"dog", "dad", "dgdg", "can", "again"}

return {"dog", "dad", "can", "again"}


dog:

doaf

agai

dcan


dad:

doaf

agai

dcan


```python
# Python 1: use prefix_set 
DIRECTIONS = [(0, 1), (0, -1), (1, 0), (-1, 0)]

class Solution:
    """
    @param board: A list of lists of character
    @param words: A list of string
    @return: A list of string
    """
    def wordSearchII(self, board, words):
        if board is None or len(board) == 0 or len(board[0]) == 0:
            return []
            
        # dictionary 
        word_set = set(words)
        
        # prefix set 
        prefix_set = set()
        
        # update prefix_set  
        for word in word_set:
            for j in range(len(word)):
                prefix_set.add(word[ : j + 1])
        
        # search words starting from every possible position
        results = set()
        
        for i in range(len(board)):
            for j in range(len(board[0])):
                self.search(board, i, j, board[i][j], word_set, prefix_set, set([(i, j)]), results)
        return list(results)
        
        
    def search(self, board, x, y, substr, word_set, prefix_set, visited, results):
        
        # if the substr exists in word set 
        if substr in word_set:
            results.add(str(substr))
            
        # if the substr is not even a prefix of a word 
        if substr not in prefix_set:
            return 
        
        # if substr is in prefix_set but not a word in word set yet 
        for delta_x, delta_y in DIRECTIONS:
            new_x, new_y = x + delta_x, y + delta_y 
            if not self.is_valid(board, new_x, new_y):
                continue 
            if (new_x, new_y) in visited:
                continue 
            visited.add((new_x, new_y))
            self.search(board, new_x, new_y, substr + board[new_x][new_y], word_set, prefix_set, visited, results)
            visited.remove((new_x, new_y))
            
            
    def is_valid(self, board, x, y):
        return 0 <= x < len(board) and 0 <= y < len(board[0])
        
```


```python
Solution().wordSearchII(["doaf","agai","dcan"], ["dog","dad","dgdg","can","again"])
```




    ['dad', 'again', 'dog', 'can']




```python
a = ["doaf","agai","dcan"]
len(a)
a[0][3]
```




    'f'




```python
# Python 2: use trie , need to solve the get method 
DIRECTIONS = [(0, 1), (0, -1), (1, 0), (-1, 0)]

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_word = False 
        self.word = None 

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    # insert 'word' to Trie
    def add(self, word):
        node = self.root 
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_word = True 
        node.word = word 
    # find a word, not used in this problem     
    def find(self, word):
        node = self.root
        for char in word:
            node = node.children.get(char)
            if node is None:
                return None 
        return node 
            
        

        
class Solution:
    """
    @param board: A list of lists of character
    @param words: A list of string
    @return: A list of string
    """
    def wordSearchII(self, board, words):
        if board is None or len(board) == 0 or len(board[0]) == 0:
            return []
        
        # trie 
        trie = Trie()
        for word in words:
            trie.add(word)
         
        # search 
        results = set()
        for i in range(len(board)):
            for j in range(len(board[0])):
                self.search(board, i, j, trie.root.get(board[i][j]), set([(i, j)]), results)
        return list(results)
    
    def search(self, board, x, y, node, visited, results):
        if node is None:
            return 
        
        if node.is_word:
            results.add(node.word)
        
        for delta_x, delta_y in DIRECTIONS:
            new_x, new_y = x + delta_x, y + delta_y 
            if not self.is_valid(board, new_x, new_y):
                continue 
            if (new_x, new_y) in visited:
                continue 
            visited.add((new_x, new_y))
            self.search(board, new_x, new_y, node.children.get(board[new_x][new_y]), visited, results)
            visited.remove((new_x, new_y))
            
        
                
    def is_valid(self, board, x, y):
        return 0 <= x < len(board) and 0 <= y < len(board[0])
        
        
        
        

```

123 - Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.




```python
# Python: only true/false needed, no need to find all solutions, so we can stop as long as we find the word in board

DIRECTIONS = [(0, 1), (0, -1), (-1, 0), (1, 0)]
class Solution:
    """
    @param board: A list of lists of character
    @param word: A string
    @return: A boolean
    """
    def exist(self, board, word):
        if board is None or len(board) == 0 or len(board[0]) == 0:
            return False 
        
        prefix_set = set()
        for i in range(len(word)):
            prefix_set.add(word[ : i + 1])
        
        for i in range(len(board)):
            for j in range(len(board[0])):
                if self.search(board, i, j, board[i][j], word, prefix_set, set([(i, j)])):
                    return True 
        return False 
        
    def search(self, board, x, y, substr, word, prefix_set, visited):
        if substr == word:
            return True 

        
        for delta_x, delta_y in DIRECTIONS:
            new_x, new_y = x + delta_x, y + delta_y 
            if not self.is_valid(board, new_x, new_y):
                continue 
            if (new_x, new_y) in visited:
                continue 
            new_str = substr + board[new_x][new_y]
            
            if new_str in prefix_set:
                visited.add((new_x, new_y))
                if self.search(board, new_x, new_y, new_str, word, prefix_set, visited):
                    return True 
                else:
                    visited.remove((new_x, new_y))
        
    
    
    # is_valid 
    def is_valid(self, board, x, y):
        return 0 <= x < len(board) and 0 <= y < len(board[0])
```
