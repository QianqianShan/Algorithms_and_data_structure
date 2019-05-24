
## Bit Manipulation


Bits in python https://wiki.python.org/moin/BitManipulation


```python
# bits representation of integer 
format(5, 'b')
```




    '101'




```python
format(-5, 'b')
```




    '-101'




```python
format(4, 'b')
```




    '100'




```python
"{0:b}".format(5)
```




    '101'




```python
int("101", 2)
```




    5



236 -  Swap Bits

Write a program to swap odd and even bits in an integer with as few instructions as possible (e.g., bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, and so on).


Input: 0

Output: 0

Explanation:
0 = 0(2) -> 0(2) = 0


```python
# the following two hexdecimal numbers: 

# 0xaaaaaaaa has 1 at all even positions 

# 0x55555555 has 1 at all odd positions 
```


```python
format(0xaaaaaaaa, 'b')
```




    '10101010101010101010101010101010'




```python
format(0x55555555, 'b')
```




    '1010101010101010101010101010101'




```python
class Solution:
    """
    @param: x: An integer
    @return: An integer
    """
    def swapOddEvenBits(self, x):
        x_binary = format(x, 'b')
        # x & 0xaaaaaaaa) >> 1 keeps all the 1s at event positions and right shift by one 
        # (x & 0x55555555) << 1 keeps all the 1s at odd positions and left shift by one 
        return ( ((x & 0xaaaaaaaa) >> 1) | ((x & 0x55555555) << 1) )

```

1 - A + B Problem

Write a function that add two numbers A and B.




```python
# Python
class Solution:
    """
    @param a: An integer
    @param b: An integer
    @return: The sum of a and b 
    """
    def aplusb(self, a, b):
        print('orig a', format(a, 'b'))
        print('orig b', format(b, 'b'))
        if a >= 0 and b >= 0:
            return self.postive_sum(a, b)
        else: 
            return a + b 
        
    def positive_sum(self, a, b):

        while b != 0 :
            _a = a ^ b 
            print('_a', format(_a, 'b'))
            _b = (a & b) << 1 
            print('_b', format(_b, 'b'))
            a = _a 
            print('a', format(a, 'b'))
            b = _b 
            print('b', format(b, 'b'))
        return a 
        

```


```python
2**32
```




    4294967296




```python
Solution().aplusb(100, -100)
```

    orig a 1100100
    orig b -1100100
    _a 11111111111111111111111111111000
    _b 1000
    a 11111111111111111111111111111000
    b 1000
    _a 11111111111111111111111111110000
    _b 10000
    a 11111111111111111111111111110000
    b 10000
    _a 11111111111111111111111111100000
    _b 100000
    a 11111111111111111111111111100000
    b 100000
    _a 11111111111111111111111111000000
    _b 1000000
    a 11111111111111111111111111000000
    b 1000000
    _a 11111111111111111111111110000000
    _b 10000000
    a 11111111111111111111111110000000
    b 10000000
    _a 11111111111111111111111100000000
    _b 100000000
    a 11111111111111111111111100000000
    b 100000000
    _a 11111111111111111111111000000000
    _b 1000000000
    a 11111111111111111111111000000000
    b 1000000000
    _a 11111111111111111111110000000000
    _b 10000000000
    a 11111111111111111111110000000000
    b 10000000000
    _a 11111111111111111111100000000000
    _b 100000000000
    a 11111111111111111111100000000000
    b 100000000000
    _a 11111111111111111111000000000000
    _b 1000000000000
    a 11111111111111111111000000000000
    b 1000000000000
    _a 11111111111111111110000000000000
    _b 10000000000000
    a 11111111111111111110000000000000
    b 10000000000000
    _a 11111111111111111100000000000000
    _b 100000000000000
    a 11111111111111111100000000000000
    b 100000000000000
    _a 11111111111111111000000000000000
    _b 1000000000000000
    a 11111111111111111000000000000000
    b 1000000000000000
    _a 11111111111111110000000000000000
    _b 10000000000000000
    a 11111111111111110000000000000000
    b 10000000000000000
    _a 11111111111111100000000000000000
    _b 100000000000000000
    a 11111111111111100000000000000000
    b 100000000000000000
    _a 11111111111111000000000000000000
    _b 1000000000000000000
    a 11111111111111000000000000000000
    b 1000000000000000000
    _a 11111111111110000000000000000000
    _b 10000000000000000000
    a 11111111111110000000000000000000
    b 10000000000000000000
    _a 11111111111100000000000000000000
    _b 100000000000000000000
    a 11111111111100000000000000000000
    b 100000000000000000000
    _a 11111111111000000000000000000000
    _b 1000000000000000000000
    a 11111111111000000000000000000000
    b 1000000000000000000000
    _a 11111111110000000000000000000000
    _b 10000000000000000000000
    a 11111111110000000000000000000000
    b 10000000000000000000000
    _a 11111111100000000000000000000000
    _b 100000000000000000000000
    a 11111111100000000000000000000000
    b 100000000000000000000000
    _a 11111111000000000000000000000000
    _b 1000000000000000000000000
    a 11111111000000000000000000000000
    b 1000000000000000000000000
    _a 11111110000000000000000000000000
    _b 10000000000000000000000000
    a 11111110000000000000000000000000
    b 10000000000000000000000000
    _a 11111100000000000000000000000000
    _b 100000000000000000000000000
    a 11111100000000000000000000000000
    b 100000000000000000000000000
    _a 11111000000000000000000000000000
    _b 1000000000000000000000000000
    a 11111000000000000000000000000000
    b 1000000000000000000000000000
    _a 11110000000000000000000000000000
    _b 10000000000000000000000000000
    a 11110000000000000000000000000000
    b 10000000000000000000000000000
    _a 11100000000000000000000000000000
    _b 100000000000000000000000000000
    a 11100000000000000000000000000000
    b 100000000000000000000000000000
    _a 11000000000000000000000000000000
    _b 1000000000000000000000000000000
    a 11000000000000000000000000000000
    b 1000000000000000000000000000000
    _a 10000000000000000000000000000000
    _b 10000000000000000000000000000000
    a 10000000000000000000000000000000
    b 10000000000000000000000000000000
    _a 0
    _b 100000000000000000000000000000000
    a 0
    b 100000000000000000000000000000000
    _a 100000000000000000000000000000000
    _b 0
    a 100000000000000000000000000000000
    b 0





    4294967296



179 - Update Bits

Given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to set all bits between i and j in N equal to M (e g , M becomes a substring of N start from i to j)

Input: 
    
    N=(10000000000)2
    
    M=(10101)2 
    
    i=2
    
    j=6
    
Output: N=(10001010100)2


```python
# Java  for reference 
class Solution {
    /**
     *@param n, m: Two integer
     *@param i, j: Two bit positions
     *return: An integer
     */
    public int updateBits(int n, int m, int i, int j) {
        // write your code here
        int max = ~0; /* All 1’s */
        // 1’s through position j, then 0’s
        if (j == 31)
            j = max;
        else
            j = (1 << (j + 1)) - 1;
        int left = max - j;
        // 1’s after position i
        int right = ((1 << i) - 1);
        // 1’s, with 0s between i and j
        int mask = left | right;
        // Clear i through j, then put m in there
        return ((n & mask) | (m << i));
    }
}
```


```python
format((1 << 32) - 1, 'b')
```




    '11111111111111111111111111111111'



142 - O(1) Check Power of 2



```python
class Solution:
    """
    @param n: An integer
    @return: True or false
    """
    def checkPowerOf2(self, n):
        print(format(n, 'b'))
        print(format(n - 1, 'b'))
        return n > 0 and n & (n - 1) == 0 


```


```python
Solution().checkPowerOf2(9)
```

    1001
    1000





    False



181 - Flip Bits

Determine the number of bits required to flip if you want to convert integer n to integer m.




```python
class Solution:
    """
    @param a: An integer
    @param b: An integer
    @return: An integer
    """
    def bitSwapRequired(self, a, b):
        # first find the bits where are different for a and b then count 
        new = a ^ b 
        # count number of 1s in new 
        count = 0 
        # compare 
        for i in range(32):
            if (new & 1 << i):
                count += 1 
        return count
```

17 - Subsets

Given a set of distinct integers, return all possible subsets.

Can also be done with DFS 



```python

class Solution:
    """
    @param nums: A set of numbers
    @return: A list of lists
    """
    def subsets(self, nums):
        if nums is None:
            return []
        # sort 
        nums.sort()
        results = []
        
        n = len(nums)
        
        # enumerate all 2^n cases 
        for i in range(1 << n):
            subset = []
            # decide what elements to be included for the i-th case 
            # suppose i in binary form is 10010, then the 2nd and 5th elements will be included 
            for j in range(n):
                if (i & (1 << j)) != 0:
                    subset.append(nums[j])
            results.append(list(subset))
        return results
                    
                    
        
#         results = []
#         # dfs with backtracking 
#         self.dfs(nums, 0, [], results)
#         return results
    
    
#     def dfs(self, nums, index, subset, results):
#         # 
#         results.append(list(subset))
        
#         for i in range(index, len(nums)):
#             subset.append(nums[i])
#             self.dfs(nums, i + 1, subset, results)
#             subset.pop()  # back track 
        
```

#### Bitwise Opearation

Bitwise operations can usually perform faster computation. 

365 - Count 1 in Binary

Count how many 1 in binary representation of a 32-bit integer.


Input: 32

Output: 1

Explanation:

32(100000), return 1.


```python
# Python 1: bitwise AND for each bit 
class Solution:
    """
    @param: num: An integer
    @return: An integer
    """
    def countOnes(self, num):
        count = 0 
        # compare 
        for i in range(32):
            if (num & 1 << i):
                count += 1 
        return count

```


```python
Solution().countOnes(-1)
```


```python
# Python 2: use a property of bits representation of integeres: 
# e.g.: the rightmost 1 in a num was converted to 0, the 0's after this 1 becomes 1 for num - 1
class Solution:
    """
    @param: num: An integer
    @return: An integer
    """
    def countOnes(self, num):
        # boundary: for num = -1, bits representation is 111111111....11111, first cut it to 32-bit 
        if num < 0:
            num &= (1 << 32) - 1
        print(format(num, 'b'))
        count = 0 
        while num != 0:
            num &= num - 1
            count += 1 
        return count
        
```


```python
Solution().countOnes(-1)
```

82 - Single Number

Given 2*n + 1 numbers, every numbers occurs twice except one, find it.


* Property: a ^ b ^ b = a 



```python
class Solution:
    """
    @param A: An integer array
    @return: An integer
    """
    def singleNumber(self, A):
        
        res = 0
        for num in A:
            res ^= num 
        return res 


```


```python
Solution().singleNumber([1, 2, 2, 3, 3, 4, 4])
```




    1



83 -  Single Number II

Given 3*n + 1 non-negative integer, every numbers occurs triple times except one, find it.




```python
# IDEA: 
# 1. ones set the bits positions when there are x times with x % 3 = 1 of the past as 1s 
# 2. twos set the bits positions when there are x times with x % 3 = 2 of the past as 1s 
# 3. ones and twos must have 1s in different positions 
# 4. (ones ^ A[i]) update ones such that if position j of A[i] and ones are both 1, set position j as 0 
#                                        if position j of A[i] is 0 and ones is 1, keep position j of ones as 1 
#                                        if position j of A[i] is 1 and ones is 0, set position j of ones as 1 
#                                        if position j of A[i] and ones are both 0, set position j as 0 
# NOT twos set all positions that are not appearing x times with x % 3 == 2 (that is appear x with x % 3 == 1 or 
#  x % 3 == 0) 

# the & makes sure the the positions set as 1 have appeared x times with x % 3 == 1 

# similar analysis for twos 
```


```python
class Solution:
    """
    @param A: An integer array
    @return: An integer
    """
    def singleNumberII(self, A):
        ones = twos = 0 
        for i in range(len(A)):
            ones = (ones ^ A[i]) & ~twos 
            print('ones',format(ones, 'b'))
            
            twos = (twos ^ A[i]) & ~ones 
            print('twos',format(twos, 'b'))
        return ones 

```


```python
Solution().singleNumberII([1,1,2,3,3,3,2,2,4,1, 30, 30, 30, 80, 80, 80])
```

    ones 1
    twos 0
    ones 0
    twos 1
    ones 10
    twos 1
    ones 0
    twos 10
    ones 1
    twos 0
    ones 10
    twos 1
    ones 0
    twos 11
    ones 0
    twos 1
    ones 100
    twos 1
    ones 100
    twos 0
    ones 11010
    twos 100
    ones 0
    twos 11010
    ones 100
    twos 0
    ones 1010100
    twos 0
    ones 100
    twos 1010000
    ones 100
    twos 0





    4



84 - Single Number III


Given 2*n + 2 numbers, every numbers occurs twice except two, find them.


Example 1:
    
Input: 
        
        [1,2,2,3,4,4,5,3]
        
Output: 
        
        [1,5]


```python
# IDEA: find a way to split the two numbers which appear once into two separate XOR operations
```


```python
class Solution:
    """
    @param A: An integer array
    @return: An integer array
    """
    def singleNumberIII(self, A):
        diff = 0 
        for num in A:
            diff ^= num
        # get the last 1 in diff 
        diff &= -diff
        
        # the final diff is x ^ y if x and y are the two numbers which appear once each
        # so diff have value 1 with positions where one of them have 1 while the other have 0 
        results = [0, 0]
        
        for num in A:
            if num & diff == 0:
                results[0] ^= num 
            else:
                results[1] ^= num
        return results 

```


```python
Solution().singleNumberIII([1,2,2,3,4,4,5,3])

```




    [1, 5]


