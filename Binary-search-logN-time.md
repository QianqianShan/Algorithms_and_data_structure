
## Binary search and log N algorithm

14 - First Position of Target

For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity.


```python
class Solution:
    """
    @param nums: The integer array.
    @param target: Target to find.
    @return: The first position of target. Position starts from 0.
    """
    def binarySearch(self, nums, target):
        if nums is None or len(nums) == 0:
            return -1 
        left, right = 0, len(nums) - 1 
        while left + 1 < right: 
            mid = (left + right) // 2 
            if nums[mid] < target:
                left = mid 
            else:
                right = mid
        if nums[left] == target:
            return left 
        if nums[right] == target:
            return right 
        return -1 

```

141 - Sqrt(x)

Implement `int` sqrt(int x), 	return the largest integer y that y*y <= x. 




```python
class Solution:
    """
    @param x: An integer
    @return: The sqrt of x
    """
    def sqrt(self, x):
        if x == 0:
            return 0 
        left, right = 0, x 
        while left + 1 < right: 
            mid = (left + right) // 2
            if mid**2 > x:
                right = mid 
            else:
                left = mid 
        if right**2 <= x:
            return right
        else:
            return left 

```

586 - Sqrt(x) II

Implement `double` sqrt(double x) and x >= 0.

Compute and return the square root of x.


```python
class Solution:
    """
    @param x: a double
    @return: the square root of x
    """
    def sqrt(self, x):
        if x == 0:
            return 0
        small_ind = True if x < 1 else False 
        if small_ind:
            x = 1 / x 
  
        left, right = 0, x 
        while right - left > 1e-10:
            mid = (left + right) / 2 
            if mid**2 > x:
                right = mid 
            else: 
                left = mid 
        return left if not small_ind else 1 / left 

```


```python
Solution().sqrt(0.1)
```




    0.31622776601787617



366 - Fibonacci

Find the Nth number in Fibonacci sequence.

A Fibonacci sequence is defined as follow:

The first two numbers are 0 and 1.
The i th number is the sum of i-1 th number and i-2 th number.
The first ten numbers in Fibonacci sequence is:

0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ...

* Recursion


```python
# O(n)
class Solution:
    """
    @param n: an integer
    @return: an ineger f(n)
    """
    def fibonacci(self, n):
        # write your code here
        f1 = 0 
        f2 = 1 
        if (n == 1):
            return 0
        if (n == 2):
            return 1
        for i in range(n-2):
            f3 = f1 + f2
            f1 = f2
            f2 = f3
            # or conveniently use 
            #f1, f2 = f2, f1 + f2
        return f3
```

457 - Classical Binary Search

Find any position of a target number in a sorted array. Return -1 if target does not exist.

* Binary search


```python
# Python : use left, right , mid , binary search 

class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        if nums is None or len(nums) == 0:
            return -1 
            
        left, right = 0, len(nums) - 1 
        while left + 1 < right:
            mid = (left + right) // 2 
            if nums[mid] > target:
                right = mid 
            else:
                left = mid 
        if nums[right] == target:
            return right 
        elif nums[left] == target:
            return left 
        else:
            return -1 
                

```


```python
# Python: use recursion 
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def findPosition(self, nums, target):
        # write your code here
        if (len(nums) == 0):
            return -1
            
        return self.binarySearch(nums, 0, len(nums) - 1, target)
    
    def binarySearch(self, nums, left, right, target):
        # end conditions 
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
        # if nums[left] and nums[right] are not equal target and they are already next to each other
        # and neither equals to the target 
        if left+1 >= right:
            return -1
        # recursion 
        mid = (left + right) // 2 
        if nums[mid] > target:
            return self.binary_search(nums, left, mid, target)
        else:
            return self.binary_search(nums, mid, right, target)

```

458 - Last Position of Target

* Find the last position of a target number in a sorted array. Return -1 if target does not exist.

* Binary search 


```python
# Python 
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def lastPosition(self, nums, target):
        # write your code here
        if (len(nums) == 0):
            return -1
        left, right = 0, len(nums) - 1
        while ((left + 1) < right):
            mid = (left + right) // 2 
            if (nums[mid] > target):
                right = mid
            elif (nums[mid] < target):
                left = mid 
            else: 
                left = mid  # set left as mid so the search continues to the right to find the last position 
        # first check the right position as we are looking for the LAST position         
        if (nums[right] == target):
            return right 
        if (nums[left] == target):
            return left
        return -1 
```

585 - Maximum Number in Mountain Sequence

* Given a mountain sequence of n integers which increase firstly and then decrease, find the mountain top.
* Binary search 



```python
class Solution:
    """
    @param nums: a mountain sequence which increase firstly and then decrease
    @return: then mountain top
    """
    def mountainSequence(self, nums):
        if nums is None or len(nums) == 0: 
            return None
        
        left, right = 0, len(nums) - 1 
        while (left + 1) < right:
            mid = (left + right) // 2 
            if (nums[mid] < nums[mid + 1]):
                left = mid 
            else:
                right = mid 
        return max(nums[left], nums[right])
```

428 - Pow(x, n)
* Implement pow(x, n). (n is an integer.)
* Binary search, divide and conquer 


Note: `n` could be NEGATIVE!


```python
# Python  1, non recursive way  
class Solution:
    """
    @param x {float}: the base number
    @param n {int}: the power number
    @return {float}: the result
    """
    def myPow(self, x, n):
        if n == 0:
            return 1 
        if n > 0:
            return self.power(x, n)
        else:
            return  self.power(1 / x, -n)
        
    def power(self, x, n):
        res = 1
        temp = x 
        # temp: x, x^2, x^4, x^8,....., x^(n // 2)
        while n > 0: 
            if n % 2 == 1:
                res *= temp 
            temp *= temp
            n //= 2 
        return res
```


```python
# Python 2: recursion, use (1/x)^-n instead of 1 / x^-n when n is negative to avoid overflow (x^n could be very big)
class Solution:
    """
    @param x {float}: the base number
    @param n {int}: the power number
    @return {float}: the result
    """
    def myPow(self, x, n):
        if n == 0:
            return 1 
        if n > 0:
            return self.power(x, n)
        else:
            return  self.power(1 / x, -n)
        
    def power(self, x, n):
        if n == 1:
            return x 
        if n % 2 == 0: 
            return self.power(x, n // 2)**2 
        else: 
            return self.power(x, n // 2)**2 * x 

```


```python
Solution().myPow(2.000, -2147483648)
```




    0.0



159 - Find Minimum in Rotated Sorted Array

* Suppose a sorted array is rotated at some pivot unknown to you beforehand.

* (i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

* Find the minimum element.

* Binary search 


```python
class Solution:
    """
    @param nums: a rotated sorted array
    @return: the minimum number in the array
    """
    def findMin(self, nums):
        left, right = 0, len(nums) - 1 
        while (left + 1) < right: 
            mid = (left + right) // 2 
            if nums[mid] <= nums[right]:
                right = mid 
            else: 
                left = mid 
        return min(nums[left], nums[right])
#         if (nums[left] < nums[right]):
#             return nums[left] 
#         return nums[right]
```

160 - Find Minimum in Rotated Sorted Array II

Same as 159 but array may contain duplicates. 


```python
class Solution:
    """
    @param nums: a rotated sorted array
    @return: the minimum number in the array
    """
    def findMin(self, nums):
        left, right = 0, len(nums) - 1 
        while left + 1 < right:
            mid = (left + right) // 2 
            if nums[mid] < nums[right]:
                # nums[mid] is smaller than nums[right], nums[mid] is possible the min, right = mid not mid - 1 
                right = mid 
            elif nums[mid] > nums[right]:
                # if nums[mid] is bigger then another value, nums[mid] cannot be min, so left = mid + 1 
                # use left = mid will also work 
                left = mid + 1 
            else:
                right = right - 1
        return nums[left] if nums[left] < nums[right] else nums[right] 

```

140 - Fast Power

* Calculate the $a^n % b$ where a, b and n are all 32bit positive integers.

* Divide and conquer, recursion


$a^{(1010)2} = a^{(1000)2} + a^{(10)2}$


```python
# Python 1, recursion
class Solution:
    """
    @param a: A 32bit integer
    @param b: A 32bit integer
    @param n: A 32bit integer
    @return: An integer
    """
    def fastPower(self, a, b, n):
        # write your code here
        if (n == 0):
            return 1 % b 
        if (n == 1):
            return a % b 
        tmp = self.fastPower(a, b, n // 2)
        if (n % 2 == 0):
            return (tmp * tmp) % b 
        if (n % 2 == 1):
            return (tmp * tmp * a) % b
```


```python
# Python 2: another version of recursion 
class Solution:
    """
    @param a: A 32bit integer
    @param b: A 32bit integer
    @param n: A 32bit integer
    @return: An integer
    """
    def fastPower(self, a, b, n):
        if n == 0:
            return 1 % b
        return self.power(a, b, n)
        
    def power(self, a, b, n):
        if n == 1:
            return a % b 
        
        if n % 2 == 1:
            return (self.power(a, b, n // 2)**2 % b * a) % b
        else:
            return self.power(a, b, n // 2)**2 % b 

```




    0



75 - Find Peak Element

* There is an integer array which has the following features:

* The numbers in adjacent positions are different. $A[0] < A[1] && A[A.length - 2] > A[A.length - 1]$.

* We define a position `P` is a peak if:

   $A[P] > A[P-1] && A[P] > A[P+1]$

* Find a peak element in this array. Return the index of the peak.

* Binary search, array 


```python
class Solution:
    """
    @param A: An integers array.
    @return: return any of peek positions.
    """
    def findPeak(self, A):
        if A is None or len(A) == 0:
            return -1 
        
        left, right = 0, len(A) - 1 
        while left + 1 < right: 
            mid = (left + right) // 2 
            if A[mid + 1] > A[mid]:
                left = mid 
            else:
                right = mid 
        return left if A[left] > A[right] else right 
            
            
```

457 - Classical Binary Search

Find any position of a target number in a sorted array. Return -1 if target does not exist.




```python
# Python binary search 
# left + 1 < right 
# mid = (start + end) // 2 
# tell A[mid] ? target 
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def findPosition(self, nums, target):
        # boundary conditions 
        if len(nums) == 0:
            return -1 
        # initialization 
        left, right = 0, len(nums) - 1 
        while (left + 1) < right: 
            mid = (left + right) // 2 
            if nums[mid] > target: 
                right = mid 
            elif nums[mid] == target:
                return mid 
            else: 
                left = mid
        
        # tell if left or right = target 
        if (nums[left] == target):
            return left 
        elif (nums[right] == target):
            return right 
        else: 
            return -1
```

28 - Search a 2D Matrix

Write an efficient algorithm that searches for a value in an m x n matrix.

This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row. For example, 

 [1, 3, 5, 7],
 
 [10, 11, 16, 20],
 
 [23, 30, 34, 50]


* Binary search $O(log(mn))$

Idea: Treat the matrix as a set of sorted arrays and do extra conversion of matrix row/col indices to array index 


```python
class Solution:
    """
    @param matrix: matrix, a list of lists of integers
    @param target: An integer
    @return: a boolean, indicate whether matrix contains target
    """
    def searchMatrix(self, matrix, target):
        if matrix is None or len(matrix) == 0 or len(matrix[0]) == 0:
            return False 
        nrow = len(matrix)
        ncol = len(matrix[0])
        
        left, right = 0, nrow * ncol - 1  
        while left + 1 < right:
            mid = (left + right) // 2 
            row = mid // ncol 
            col = mid % ncol 
            if matrix[row][col] < target:
                left = mid 
            else:
                right = mid 
        if matrix[left // ncol][left % ncol] == target or matrix[right // ncol][right % ncol] == target:
            return True 
        return False
        
```

38 - Search a 2D Matrix II

Write an efficient algorithm that searches for a value in an m x n matrix, return the occurrence of it.

This matrix has the following properties:

Integers in each row are sorted from left to right.

Integers in each column are sorted from up to bottom.

No duplicate integers in each row or column. For example, 

[1, 3, 5, 7],
 
[2, 4, 7, 8],
      
[3, 5, 9, 10]


target = 3

Output:2


* O(m+n) time and O(1) extra space


      

##### Idea: start from top-right or bottom left corner: 

* If the corner number is bigger than target, then can drop the current column

* If the coner number is smaller than target, drop the current row

* If the coner number == target, record it and drop the current row AND column


```python
class Solution:
    """
    @param matrix: A list of lists of integers
    @param target: An integer you want to search in matrix
    @return: An integer indicate the total occurrence of target in the given matrix
    """
    def searchMatrix(self, matrix, target):
        if matrix is None or len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        count = 0 
        row, col = len(matrix), len(matrix[0])
        start_row = 0 
        start_col = col - 1
       # print(start_col)
        while start_col >= 0 and start_row < row:
           #  print(start_col)
           # print(start_row)
            if matrix[start_row][start_col] > target: 
                start_col -= 1 
                print('start_col {}'.format(start_col))
            elif matrix[start_row][start_col] == target:
                count += 1 
                start_row += 1
                start_col -= 1
               # print('count {}'.format(count))
            else: 
                start_row += 1 
        return count
```

8 - Rotate String

Given a string(Given in the way of char array) and an offset, rotate the string by offset in place. (rotate from left to right)

* Rotate in-place with O(1) extra memory.


* String 


```python
class Solution:
    """
    @param str: An array of char
    @param offset: An integer
    @return: nothing
    """
    def rotateString(self, s, offset):
        # boundary conditions 
        if (len(s) == 0):
            return " "
        
        # if offset if bigger the string length 
        if (offset > len(s)):
            offset = offset % len(s)
        # first do overall reverse 
        self.reverse(s, 0, len(s) - 1)
        
        # reverse the two segments back respectively 
        self.reverse(s, 0, offset - 1)
        self.reverse(s, offset, len(s) - 1)

        
        
    def reverse(self, s ,start, end):
        while (start < end):
            s[start], s[end] = s[end], s[start]
            start += 1 
            end -= 1
        
```

39 - Recover Rotated Sorted Array

Given a rotated sorted array, recover it to sorted array in-place.

* 三步翻转法




```python
# Python 
class Solution:
    """
    @param nums: An integer array
    @return: nothing
    """
    def recoverRotatedSortedArray(self, nums):
        # write your code here
        # first find the location of the number which is bigger than the next one 
        for index in range(len(nums) - 1):  # loop up to len(nums) - 1 -1 to avoid overflow of index + 1
            if (nums[index] > nums[index + 1]):
                break
            index += 1
        self.reverse(nums, 0, index)
        self.reverse(nums, index + 1, len(nums) - 1)
        self.reverse(nums, 0, len(nums) - 1)
    
    def reverse(self, nums ,start, end):
        while (start < end):
            nums[start], nums[end] = nums[end], nums[start]
            start += 1 
            end -= 1
        
```

74 - First Bad Version

The code base version is an integer start from 1 to n. One day, someone committed a bad version in the code case, so it caused this version and the following versions are all failed in the unit tests. Find the first bad version.

You can call isBadVersion to help you determine which version is the first bad one. The details interface can be found in the code's annotation part.


Example: 

Given n = 5:

    isBadVersion(3) -> false
    
    isBadVersion(5) -> true
    
    isBadVersion(4) -> true

Here we are 100% sure that the 4th version is the first bad version.

* Binary search 



```python
# Python 
#class SVNRepo:
#    @classmethod
#    def isBadVersion(cls, id)
#        # Run unit tests to check whether verison `id` is a bad version
#        # return true if unit tests passed else false.
# You can use SVNRepo.isBadVersion(10) to check whether version 10 is a 
# bad version.
class Solution:
    """
    @param n: An integer
    @return: An integer which is the first bad version.
    """
    def findFirstBadVersion(self, n):
        # write your code here
        left, right = 1, n   # use location index  1 - n 
        while (left + 1) < right:
            mid = (left + right) // 2 
            if SVNRepo.isBadVersion(mid): 
                right = mid
            else:
                left = mid
        if SVNRepo.isBadVersion(left):
            return left
        return right
```

460 - Find K Closest Elements

Given `target`, a non-negative integer `k` and an integer array `A` sorted in ascending order, find the k closest numbers to target in A, sorted in ascending order by the difference between the number and target. Otherwise, sorted in ascending order by number if the difference is same. For example, 

Input: A = [1, 2, 3], target = 2, k = 3

Output: [2, 1, 3]

* Binary search, two pointers 



```python
class Solution:
    """
    @param A: an integer array
    @param target: An integer
    @param k: An integer
    @return: an integer array
    """
    def kClosestNumbers(self, A, target, k):
        # write two functions: one does binary search for the closet number, the other does compare
        # of which side has a closer number 
        if (len(A) < k): 
            return A
        if (len(A) == 0): 
            return []
            
        # initialize left and right from binary search 
        right = self.find_upper_closest(A, target)
        left = right - 1 
        
        res = []
        # collect k elements closest to target 
        for _ in range(k):
            indicator =  self.is_left_closer(A, left, right, target)
            if indicator == True:
                res.append(A[left])
                left -= 1 
            else: 
                res.append(A[right])
                right += 1
        return res 
         
         
    def find_upper_closest(self, A, target):
        left, right = 0, len(A) - 1 
        while (left + 1) < right: 
            mid = (left + right) // 2 
            if A[mid] < target:
                left = mid 
            else: 
                right = mid
        if A[left] >= target:
            return left 
        else: 
            return right
    
    def is_left_closer(self, A, left, right, target):
        # first make sure the indices are within range before comparison
        if left < 0:
            return False 
        if right >= len(A):
            return True
        if target - A[left] <= A[right] - target: 
            return True
        return False




```

62 - Search in Rotated Sorted Array

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

* O(logN) time

* Binary search 


```python
# Python: use one binary search
# 1. Add additional condition to tell if the mid is on which segment   
class Solution:
    """
    @param A: an integer rotated sorted array
    @param target: an integer to be searched
    @return: an integer
    """
    def search(self, A, target):
        # special cases 
        if (len(A) == 0):
            return -1 
        left, right = 0, len(A) - 1 
        while (left + 1) < right:
            mid = (left + right) // 2
            # first discuss where is the mid 
            if (A[mid] > A[left]):
                if A[left] <= target <= A[mid]:
                    right = mid
                else: 
                    left = mid 
            else:
                if A[mid] <= target <= A[right]:
                    left = mid 
                else: 
                    right = mid 
        if A[left] == target:
            return left 
        if A[right] == target:
            return right 
        return -1 
                
            

```


```python
p = Solution()
p.search([1, 2, 3, 4, 9], 9)
```




    4




```python
# Python 2: use two binary search 
# 1. First find the min value index 
# 2. Decide which segment the segment could be in and do another binary search 
class Solution:
    """
    @param A: an integer rotated sorted array
    @param target: an integer to be searched
    @return: an integer
    """
    def search(self, A, target):
        # special cases 
        if (len(A) == 0):
            return -1 
        min_index = self.find_min_index(A)
        print(min_index)
        # tell the relationship of the target and min(A) and max(A)
        # special case 
        #if (A[min_index] > target or A[min_index - 1] < target):
        #    return -1
        
        # binary search 
        if (A[min_index] <= target <= A[len(A) - 1]):
            #print('In range')
            return self.binary_search(A, min_index, len(A) - 1, target)
        else:
            return self.binary_search(A, 0, min_index - 1, target)

    
    
    '''
    # O(n)
    def find_min_index(self, A):
        index = 0 
        for i in range(len(A)):
            if (A[i] < A[index]):
                index = i
        return index 
    '''
    
    # O(log n)
    def find_min_index(self, A):
        left, right = 0, len(A) - 1 
        while (left + 1) < right:
            mid = (left + right) // 2 
            if (A[mid] <= A[right]): 
                right = mid 
            else:
                left = mid 
        if (A[left] <= A[right]):
            return left 
        return right 
            
    
    def binary_search(self, A, left, right, target):
        while (left + 1) < right:
            mid = (left + right) // 2 
            if (A[mid] <= target):
                left = mid 
            else: 
                right = mid 
        if (A[left] == target):
            return left 
        if (A[right] == target):
            return right 
        return -1 
                

                
            

```


```python
p = Solution()
# p.search([6,8,9,1,3,5], 5)
# p.binary_search([4, 3], 1, 1, 3)
p.find_min_index([6,8,9,1,3,5])

```




    3



63 - Search in Rotated Sorted Array II

Follows 62, what if duplicates are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

Example: 

Input:
[3,4,4,5,7,0,1,2]

4

Output:

true



```python
# brute force 

# first use set() to remove duplicates, then apply binary search 
```

447 - Search in a Big Sorted Array

Given a big sorted array with non-negative integers sorted by non-decreasing order. The array is so big so that you can not get the length of the whole array directly, and you can only access the kth number by ArrayReader.get(k) (or ArrayReader->get(k) for C++).

Find the first index of a target number. Your algorithm should be in O(log k), where k is the first index of the target number.

Return -1, if the number doesn't exist in the array.

* Binary search 


#### Binary search Idea: first find the first element that is no smaller than target , then use binary search 


```python
# Python 1 : binary search 
"""
Definition of ArrayReader
class ArrayReader(object):
    def get(self, index):
        # return the number on given index, 
        # return 2147483647 if the index is invalid.
"""
class Solution:
    """
    @param: reader: An instance of ArrayReader.
    @param: target: An integer
    @return: An integer which is the first index of target.
    """
    def searchBigSortedArray(self, reader, target):
        # find a position whose number >= target
        right = 1 
        while reader.get(right) < target:
            right *= 2
        
        # do binary search 
        left, right = right // 2, right 
        while (left + 1) < right:
            mid = (left + right) // 2 
            vals = reader.get(mid)
            ###################################################
            # Note: the question asks for the FIRST position, 
            # so only left = mid when vals < target, 
            # when vals = target, let right = mid to make sure 
            # the first appearance is included 
            #################################################
            if vals < target:  
                left = mid
            else:
                right = mid 
        if reader.get(left) == target:
            return left 
        if reader.get(right) == target:
            return right 
        return -1 
        
            
        
            
        
        
```


```python
# Python 2 : use exponential backoff 倍增法
"""
Definition of ArrayReader
class ArrayReader(object):
    def get(self, index):
        # return the number on given index, 
        # return 2147483647 if the index is invalid.
"""
class Solution:
    """
    @param: reader: An instance of ArrayReader.
    @param: target: An integer
    @return: An integer which is the first index of target.
    """
    def searchBigSortedArray(self, reader, target):
        first_val = read.get(0)
        if first_val == target:
            return 0 
        if first_val > target:
            return -1 
        
        # jump from position 0 with exponenetially increasing step sizes when value < target
        # jump from the first position with value >= target from above back with exponentially decreasing steps
        index, steps = 0, 1 
        while steps > 0:
            while steps > 0 and read.get(index + steps) >= target:
                steps /= 2 
            index += steps
            steps *= 2 
        if reader.get(index + 1) == target:
            return index + 1 
        return -1 

```

462 - Total Occurrence of Target

Given a target number and an integer array sorted in ascending order. Find the total number of occurrences of target in the array.


```python
# Python 1: two pointers 
class Solution:
    """
    @param A: A an integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def totalOccurrence(self, A, target):
        # corner case 
        if A is None or len(A) == 0:
            return 0 
        
        # two pointers 
        left, right = 0, len(A) - 1 
        while left < right and A[left] < target:
            left += 1 
        while left < right and A[right] > target:
            right -= 1 
        if A[left] == target and A[right] == target:
            return right - left + 1
        return 0
        
```


```python
# Python 2: binary search, find the first occurence and then count 
class Solution:
    """
    @param A: A an integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def totalOccurrence(self, A, target):
        # corner case 
        if A is None or len(A) == 0:
            return 0 
            
        # binary search 
        left, right = 0, len(A) - 1 
        while left + 1 < right: 
            mid = (left + right) // 2 
            if A[mid] < target:
                left = mid 
            else:
                right = mid
        # count the total occurences
        count = 0 
        if A[left] == target: 
            start = left 
        elif A[right] == target:
            start = right
        else:
            return count 
        # special case : A = [1, 1, 1, ,1, 1, ..., 1]
        while start < len(A) and A[start] == target:
            count += 1 
            start += 1 
        return count 

```

61 - Search for a Range

Given a sorted array of n integers, find the starting and ending position of a given target value.

If the target is not found in the array, return [-1, -1].


```python
class Solution:
    """
    @param A: an integer sorted array
    @param target: an integer to be inserted
    @return: a list of length 2, [index1, index2]
    """
    def searchRange(self, A, target):
        if len(A) == 0 or A is None:
            return [-1, -1]
        return self.binary_search(A, target)

    def binary_search(self, A, target):
        result = []
        for i in range(2):
            # reset left and right pointers for each loop 
            left, right = 0, len(A) - 1 

            while left + 1 < right:
                mid = (left + right) // 2 
                if i == 0:
                    if A[mid] < target:
                        left = mid 
                    else:
                        right = mid 
                else:
                    if A[mid] > target:
                        right = mid
                    else:
                        left = mid
            if i == 0:
                if A[left] == target:
                    start = left 
                elif A[right] == target:
                    start = right 
                else:
                    start = -1 
                result.append(start)
            else:
                if A[right] == target:
                    end = right 
                elif A[left] == target:
                    end = left 
                else:
                    end = -1 
                print(end)
                result.append(end)
        return result
                
        
                    


```

459 - Closest Number in Sorted Array

Given a target number and an integer array A sorted in ascending order, find the index i in A such that A[i] is closest to the given target.

Return -1 if there is no element in the array.


```python
# find upper closest and then compare it and its previous value to target to see which one is closer to target 

class Solution:
    """
    @param A: an integer array sorted in ascending order
    @param target: An integer
    @return: an integer
    """
    def closestNumber(self, A, target):
        if A is None or len(A) == 0:
            return -1 
        
        left, right = 0, len(A) - 1 
        
        while left + 1 < right:
            mid = (left + right) // 2 
            if A[mid] < target:
                left = mid 
            else:
                right = mid 
        if (A[right] - target) > (target - A[left]):
            return left 
        else:
            return right

        # if A[left] >= target:
        #     # make sure (left - 1) > 0 
        #     if left > 1 and (A[left] - target) > (target - A[left - 1]):
        #         return left -1 
        #     else:
        #         return left 
        # else:
        #     if (A[right] - target) > (target - A[left]):
        #         return left 
        #     else:
        #         return right 
        
        
```

235 - Prime Factorization

Prime factorize a given integer.

Example: 
Input: 10
    
Output: [2, 5]


```python
# only need to loop up to sqrt(n)
class Solution:
    """
    @param num: An integer
    @return: an integer array
    """
    def primeFactorization(self, num):
        
        # 1. find max k: sqrt(n)
        # 2. loop through k 
        # 3. check if the updated n value is 1 or not 
        
        loopEnd = int(math.sqrt(num))
        results = []  # save values
        
        k = 2 # ignore trivial case k = 1
        
        # while loop tells if n = 1, if n = 1, can stop early 
        while k <= loopEnd and num > 1:
            if (num % k == 0):
                num //= k
                results.append(k)
                k = 1 # reset k 
            k += 1
        
        # tell the case when n > 1, happens when num itself is prime  
        if (num > 1):
            results.append(num)
        return res
```

254 - Drop Eggs

There is a building of` `n floors. If an egg drops from the `k` th floor or above, it will break. If it's dropped from any floor below, it will not break.

You're given two eggs, Find `k` while minimize the number of drops for the worst case. Return the number of drops in the worst case.


Reference: 

http://datagenetics.com/blog/july22012/index.html

https://www.zhihu.com/question/19690210

https://blog.csdn.net/joylnwang/article/details/6769160


```python
import math
class Solution:
    """
    @param n: An integer
    @return: The sum of a and b
    """
    def dropEggs(self, n):
        k = int((-1 + math.sqrt(1 + 8 *n)) / 2)
        if k * (k + 1) /2 == n:
            return k 
        else:
            return k + 1 
        
        
 


```


```python
# Python 2: use recursion 
import functools
@functools.lru_cache(maxsize=None)
def f(n, m):
    if n == 0:
        return 0
    if m == 1:
        return n

    ans = min([max([f(i - 1, m - 1), f(n - i, m)]) for i in range(1, n + 1)]) + 1
    return ans

print(f(100, 2))

```

    14



```python
f(100, 2)
```

414 - Divide Two Integers

Divide two integers without using multiplication, division and mod operator.

If it will overflow(exceeding 32-bit signed integer representation range), return 2147483647



```python
import sys

class Solution:
    """
    @param dividend: the dividend
    @param divisor: the divisor
    @return: the result
    """
    def divide(self, dividend, divisor):
        INT_MAX = sys.maxsize
        if dividend == 0:
            return 0 
        if divisor == 0:
            return INT_MAX
        negative_indicator = dividend > 0 and divisor < 0 or dividend < 0 and divisor > 0 
        
        dividend = abs(dividend)
        divisor = abs(divisor)
            

        answer, shift = 0, 31
        while shift >= 0:
            if dividend >= divisor << shift:
                dividend -= divisor << shift
                answer += 1 << shift 
            shift -= 1 
        if negative_indicator:
            answer = - answer
        return answer
        

```


```python
Solution().divide(2147483647, 1)
```




    2147483647



600 - Smallest Rectangle Enclosing Black Pixels

An image is represented by a binary matrix with `0` as a white pixel and `1` as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location `(x, y)` of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.


Example: 

Input：["0010","0110","0100"]，x=0，y=2

Output：6


```python
class Solution:
    """
    @param image: a binary matrix with '0' and '1'
    @param x: the location of one of the black pixels
    @param y: the location of one of the black pixels
    @return: an integer
    """
    def minArea(self, image, x, y):
        if image is None or len(image) == 0 or len(image[0]) == 0:
            return 0 
        nrow = len(image)
        ncol = len(image[0])
        
        positions = [[0, y], [y, ncol - 1], [0, x], [x, nrow - 1]]
        
        # find the smallest column number with 1
        start, end = positions[0]
        while start + 1 < end: 
            mid = (start + end) // 2 
            if self.check_ones_col(image, mid):
                end = mid
            else:
                start = mid 
#         if self.check_ones_row(image, start):
#             left = start 
#         elif self.check_ones_row(image, end):
#             left = end
                
        left = start if self.check_ones_col(image, start) else end  
        
        # the largest column number with 1 
        start, end = positions[1]
        while start + 1 < end: 
            mid = (start + end) // 2 
            if self.check_ones_col(image, mid):
                start = mid 
            else:
                end = mid 
                
        right = end if self.check_ones_col(image, end) else start 
        
        
        # find the smallest row number with 1
        start, end = positions[2]
        while start + 1 < end: 
            mid = (start + end) // 2 
            if self.check_ones_row(image, mid):
                end = mid
            else:
                start = mid 
                
        low = start if self.check_ones_row(image, start) else end  
        # the largest row number with 1 
        start, end = positions[3]
        while start + 1 < end: 
            mid = (start + end) // 2 
            if self.check_ones_row(image, mid):
                start = mid 
            else:
                end = mid 
        up = end if self.check_ones_row(image, end) else start 
        
        return (up - low + 1) * (right - left + 1)
    
    # check if there are 1s for the given col of image 
    def check_ones_col(self, image, col):
        for i in range(len(image)):
            if image[i][col] == '1':
                return True 
        return False 
    
    
    def check_ones_row(self, image, row):
        for i in range(len(image[0])):
            if image[row][i] == '1':
                return True 
        return False 
        
        
        
        
        
                    
            
        

```


```python
Solution().minArea(["0010","0110","0100"], 0, 2)
```




    6



617 - Maximum Average Subarray II

Given an array with positive and negative numbers, find the maximum average subarray which length should be greater or equal to given length `k`.

Example: 

Input:

[1,12,-5,-6,50,3]

3

Output:

15.667 as (-6 + 50 + 3) / 3 = 15.667

#### Binary search on answer with prefix sum 

1. Bianry search on average, average must be in [min(nums), max(nums)]

2. For each searched average, minus the average from each value of the array, find subarrays such that len(subarray) >= k and sum(subarray), if the sum > 0, it indicates that the max average could be bigger than the current guess 


_Different from the maximum subarray, the check_subarray needs to record prefix_sum at each position as we need to trace at least k steps back when computing the min_prefix_sum_


```python
class Solution:
    """
    @param nums: an array with positive and negative numbers
    @param k: an integer
    @return: the maximum average
    """
    def maxAverage(self, nums, k):
        if not nums:
            return 0 
        
        lower, upper = min(nums), max(nums)
        
        while upper - lower > 1e-5:
            average = (upper + lower) / 2
            if self.check_subarray(nums, k, average):
                lower = average
            else: 
                upper = average
        # lower approx = upper 
        return lower 
     
    # check if there are subarrays that have sum > 0 
    # if yes, which means the max possible average can be bigger than the current one 
    def check_subarray(self, nums, k, average):
        # new_nums = [nums[i] - average for i in range(len(nums))]
        # prefix_sum = [0]
        # for num in new_nums:
        #     prefix_sum.append(prefix_sum[-1] + num)
        prefix_sum = [0]
        for num in nums:
            prefix_sum.append(prefix_sum[-1] + num - average)
            
        min_prefix_sum = 0 
        
        # prefix_sum[i] is the prefix sum of the first i elements as prefix_sum is initialized with 0 
        for i in range(k, len(nums) + 1):
            if prefix_sum[i] - min_prefix_sum >= 0:
                return True 
            min_prefix_sum = min(min_prefix_sum, prefix_sum[i - k + 1])
        return False 

```


```python
def check_subarray(self, nums, k, average):
        prefix_sum = [0]
        for num in nums:
            prefix_sum.append(prefix_sum[-1] + num - average)
            
        min_prefix_sum = 0
        for i in range(k, len(nums) + 1):
            if prefix_sum[i] - min_prefix_sum >= 0:
                return True
            min_prefix_sum = min(min_prefix_sum, prefix_sum[i - k + 1])
            
        return False
```


```python

```


```python

```
