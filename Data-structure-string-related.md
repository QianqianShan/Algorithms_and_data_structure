
### Strings

13 - Implement strStr()

For a given `source` string and a `target` string, you should output the first index(from 0) of `target` string in `source` string.

If target does not exist in source, just return -1.


Example: 

	Input:
    source = "abcdabcdefg" ，target = "bcd"
    
	Output: 1


Hashcode: 
    
    $$abcde = (a 31^4 + b 31^3 + c 31^2 + d 31^1 + e 31^0)$$


```python
# Python 1: basic implementation 
class Solution:
    """
    @param source: 
    @param target: 
    @return: return the index
    """
    def strStr(self, source, target):
        n_s = len(source)
        n_t = len(target)
        # corner case 
        # special case : '' == ''
        if source == target:
            return 0
        if n_s == 0 or n_t > n_s:
            return -1
        # if source is nonempty and target is empty 
        if n_s > 0 and n_t == 0:
            return 0
        
        # loop
        for i in range(n_s - n_t + 1):
            if source[i:(i + n_t)] == target:
                return i 
        return -1  

```


```python
Solution().strStr(source = "abcde", target="e")
```




    4




```python
# Python 2:  map strings to integers and record with a hashcode 
class Solution:
    """
    @param source: 
    @param target: 
    @return: return the index
    """
    def strStr(self, source, target):
        # compute hashcode of target 
        n_s = len(source)
        n_t = len(target)
        
        target_code = self.hashcode(target)
        
        for i in range(n_s - n_t + 1):
            source_code = self.hashcode(source[i:(i + n_t)])
            # double check by source[i:(i + n_t)] == target to make sure the target is in source 
            if source_code == target_code and source[i:(i + n_t)] == target:
                return i 
        return -1 

        
        
    def hashcode(self, string):
        code = 0
        for char in string:
            code = (code * 31 + ord(char)) % 10**6
        return code 
            

```


```python
Solution().strStr(source = "", target="")
```

    0





    0



594 -strStr II

Implement `strStr` function in `O(n + m)` time.

`strStr` returns the first index of the target string in a source string. The length of the target string is `m` and the length of the source string is `n`.

If target does not exist in source, just return -1.

Example: 

Input：source = "abcdef"， target = "bcd"

Output：1


```python
# Python: rabin-karp, use a quicker way to compute hashcode as shown in quick_hashcode function
class Solution:
    """
    @param: source: A source string
    @param: target: A target string
    @return: An integer as index
    """
    def strStr2(self, source, target):
        
        if source is None or target is None:
            return -1 
        # compute hashcode of target 
        n_s = len(source)
        n_t = len(target)
        
        base = 10**6
        
        target_code = self.hashcode(target, base)
        source_code = self.hashcode(source[:n_t], base)
        print('target',target_code)

        if source_code == target_code and source[:n_t] == target:
            return 0 
        
        power = 31**(n_t) % base
        
        for i in range(n_s - n_t):
            # 
            source_code = self.quick_hashcode(source_code, source[i], source[i + n_t], power, base)
            print('source',source_code)
            # double check by source[i:(i + n_t)] == target to make sure the target is in source 
            if source_code == target_code and source[(i + 1):(i + n_t + 1)] == target:
                return i + 1
#             if i % 1000 == 0:
#                 print(i)

        return -1 

        
        
    def hashcode(self, string, base):
        code = 0
        for char in string:
            code = (code * 31 + ord(char)) % base
        return code 
    
    def quick_hashcode(self, orig_code, removed_char, added_char, power, base):
        new_code = ((orig_code * 31 + ord(added_char)) % base - ord(removed_char) * power ) % base
        if new_code < 0:
            new_code += base 
        return new_code
        
            
```


```python
Solution().strStr2('abde', 'bd')
```

    target 3138
    source 3138





    1



415 - Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Example: 

Input: "A man, a plan, a canal: Panama"

Output: true


* Two pointers 



```python
class Solution:
    """
    @param s: A string
    @return: Whether the string is a valid palindrome
    """
    def isPalindrome(self, s):
        if len(s) == 0:
            return True
        # two pointer from two ends 
        start, end = 0, len(s) - 1
        while start < end:
            # ignore comma... which are not alphanumeric characters 
            # or use isdigit(), isalpha()
            while start < end and not s[start].isalnum():
                start += 1 
            while start < end and not s[end].isalnum():
                end -= 1 
            # compare the two characters 
            if s[start].lower() == s[end].lower():
                start += 1 
                end -= 1 
            else:
                return False 
        return True 

```


```python
Solution().isPalindrome('A man, a plan, a canal: Panama')
```




    True



627 - Longest Palindrome

* Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

* This is case sensitive, for example "Aa" is not considered a palindrome here.

* Hash table 

Example: 

Input : s = "abccccdd"

Output : 7


```python
class Solution:
    """
    @param s: a string which consists of lowercase or uppercase letters
    @return: the length of the longest palindromes that can be built
    """
    def longestPalindrome(self, s):
        if not s:
            return 0 
        # hash_table to be used to flag if the char has appeared even times 
        hash_table = {}
        for char in s:
            hash_table.setdefault(char, False)
            if hash_table[char]:
                del hash_table[char]
            else:
                hash_table[char] = True
        
        remove = len(hash_table)
        
        return len(s) - remove + 1 if remove > 0 else len(s)


        
```


```python
Solution().longestPalindrome('abccccdd')
```




    7



200 - Longest Palindromic Substring

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000,and there exists one unique longest palindromic substring.

Example: 

Input:"abcdzdcab"

Output:"cdzdc"


```python
# Python 1: start two pointers from each character of the string, both pointers start from the middle
# O(n^2) time 
class Solution:
    """
    @param s: input string
    @return: the longest palindromic substring
    """
    def longestPalindrome(self, s):
        # corner case 
        if not s: 
            return ''
        # initialize the longest substr 
        longest = ''
        # loop all positions as the middle character of a palindrome substr 
        for mid in range(len(s)):
            # mid for odd and even length substr
            for i in range(2):
                substr = self.find_palindrome(s, mid, mid + i)
                if len(substr) > len(longest):
                    longest = substr
        return longest
    
    
    def find_palindrome(self, s, left, right): 
        while left >=0 and right < len(s) and s[left] == s[right]:
            left -= 1 
            right += 1 
        return s[(left + 1):right]
        

```


```python
Solution().longestPalindrome('abcdzdcab')
```




    'cdzdc'




```python
# Python 2: dynamic programming 
class Solution:
    """
    @param s: input string
    @return: the longest palindromic substring
    """
    def longestPalindrome(self, s):
        if not s:
            return ''
        # set up 
        n = len(s)
        table = [[False] * n for _ in range(n)]
        
        # table[i][i] is always true (any character of iteself is palindrome)
        for i in range(n):
            table[i][i] = True 
        # table[i][i-1] is always invalid, set as True
        for i in range(1, n):
            table[i][i - 1] = True
        
        # if there is no palidrome longer than 1, then return the first character of s 
        longest = s[0]
        
        # loop over all possible lengths of a possible palidrome from 1 to n 
        for palidrome_len in range(1, n): 
            # find palidrome starting from 0, 1, ..., n, with length == palidrome_len 
            for start in range(n - palidrome_len):
                end = start + palidrome_len
                # s[start : (end + 1)] is palidrome (table[start][end] == True) only if 
                # s[start] == s[end] and the substr inbetween (start + 1, end -1) is palidrome
                table[start][end] = (s[start] == s[end]) and table[start + 1][end - 1]
                
                # s[start:end + 1] has (palidrome_len + 1) characters 
                if table[start][end] and palidrome_len + 1 > len(longest):
                    longest = s[start:(end + 1)]
        return longest
                
```


```python
Solution().longestPalindrome('ab')
```

    test
    False





    'a'




Python 3 , manacher's algorithm 

https://www.geeksforgeeks.org/manachers-algorithm-linear-time-longest-palindromic-substring-part-1/

https://segmentfault.com/a/1190000003914228

667 - Longest Palindromic Subsequence

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

* Dynamic programming 

Example: 

Input: "bbbab"

Output: 4

Key idea: 

1. $$f[i][j] = max(f[i+1][j], f[i][j-1], f[i+1][j-1] + 2 | s[i] = s[j])$$

2. Initialization: 

Any character itself is palindrome, $$f[i][i] = 1$$ 


If $s[i] = s[i+1]$,$$f[i][i+1] = 2$$

If $s[i] != s[i+1]$,$$f[i][i+1] = 1$$

3. loop over the length between i and j


```python
# Python 1: dynamic programming 
class Solution:
    """
    @param s: the maximum length of s is 1000
    @return: the longest palindromic subsequence's length
    """
    def longestPalindromeSubseq(self, s):
        n = len(s)
        # corner cases 
        if n == 0:
            return 0 
        if n == 1:
            return 1
        # initialization 
        table = [[0] * n for _ in range(n)]
        for i in range(n):
            table[i][i] = 1
        
        for i in range(n - 1):
            if s[i] == s[i + 1]:
                table[i][i + 1] = 2
            else:
                table[i][i + 1] = 1
        
        # loop over length between i and j 
        for length in range(3, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1 
                if s[i] == s[j]:
                    table[i][j] = max(table[i + 1][j], table[i][j - 1], table[i + 1][j - 1] + 2)
                else: 
                    table[i][j] = max(table[i + 1][j], table[i][j - 1])
        print(table)
        return table[0][n - 1]
        
            
            
        

```


```python
Solution().longestPalindromeSubseq("bbbab")
```

    [[1, 2, 3, 3, 4], [0, 1, 2, 2, 3], [0, 0, 1, 1, 3], [0, 0, 0, 1, 1], [0, 0, 0, 0, 1]]





    4




```python
# Python 2: memoization, time O(n^2)  
class Solution:
    """
    @param s: the maximum length of s is 1000
    @return: the longest palindromic subsequence's length
    """
    def longestPalindromeSubseq(self, s):
        self.s = s 
        n = len(s)
        
        if n == 0:
            return 0 
        
        if n == 1:
            return 1 
        
        self.table = [[0] * n for _ in range(n)]
        
#         for i in range(n):
#             for j in range(i, n):
#                 self.table[i][j] = -1 
                
        self.memo(0, n - 1)
        print(self.table)
        return self.table[0][n-1]
    
    def memo(self, i, j):
        # recursion ends with 
        # if already computed 
        if self.table[i][j] != 0:
            return 
        
        # if i == j 
        if i == j: 
            self.table[i][j] = 1
            return 
        
        # i = j - 1
        if i == (j - 1):
            self.table[i][j] = 2 if self.s[i] == self.s[j] else 1
            return 
                
        # recursion 
        self.memo(i, j - 1)
        self.memo(i + 1, j)
        self.memo(i + 1, j - 1)
        
        if self.s[i] == self.s[j]:
            self.table[i][j] = max(self.table[i][j - 1], self.table[i + 1][j], self.table[i + 1][j - 1] + 2)
        else: 
            self.table[i][j] = max(self.table[i][j - 1], self.table[i + 1][j])
  
```


```python
Solution().longestPalindromeSubseq("bbbab")
```

    [[0, 2, 3, 3, 4], [0, 1, 2, 2, 3], [0, 0, 1, 1, 3], [0, 0, 0, 1, 1], [0, 0, 0, 0, 0]]





    4




```python
import numpy as np
arr = np.arange(1000000)
%time for _ in range(10): my = arr ** 2 
```

    CPU times: user 17.3 ms, sys: 12.4 ms, total: 29.7 ms
    Wall time: 30.2 ms



```python
a = list(range(10))
a

b = a[5:8]
b[0] = 0
a

```




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



624 - Remove Substrings

Given a string `s` and a set of `n` substrings. You are supposed to remove every instance of those `n` substrings from `s` so that `s` is of the minimum length and output this minimum length.

Example: 

Input:
"ccdaabcdbb"

["ab","cd"]

Output:

2

Explanation: 
ccdaabcdbb -> ccdacdbb -> cabb -> cb (length = 2)




```python
# Python: loop through every possible places to remove the substring
from collections import deque 

class Solution:
    """
    @param: s: a string
    @param: dict: a set of n substrings
    @return: the minimum length
    """
    def minLength(self, s, dict):
        if not s:
            return -1 
        if not dict:
            return len(s)
        # initialization 
        min_len = len(s)
        # set to save the substrings that have been checked 
        checked_strs = set()
        
        queue = deque([s])
        while queue:
            cur_str = queue.popleft()
#             print(cur_str)
            for substr in dict:
                # find the first location (if any) of the substr appearance in s 
                loc = cur_str.find(substr)
                while loc != -1:
                    new_str = cur_str[0:loc] + cur_str[loc + len(substr):]
                    if new_str not in checked_strs:
                        queue.append(new_str)
                        checked_strs.add(new_str)
                        min_len = min(min_len, len(new_str))
                    # check if there are multiple appearances of the substr in cur_str
                    loc = cur_str.find(substr, loc + 1)
        return min_len        

```
