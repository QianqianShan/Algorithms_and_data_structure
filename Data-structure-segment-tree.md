
## Segment Tree


Segment tree supports queries like:

querying which of the stored segments contain a given point, max /min / sum of some intervals ...

https://en.wikipedia.org/wiki/Segment_tree

201 - Segment Tree Build

The structure of Segment Tree is a binary tree with each node has two attributes start and end denote an segment / interval.



```python
Example: 
    
Input:start = 0,end = 3
Output: 
	               [0,  3]
	             /        \
	      [0,  1]           [2, 3]
	      /     \           /     \
	   [0, 0]  [1, 1]     [2, 2]  [3, 3]
```


```python
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end):
        self.start, self.end = start, end
        self.left, self.right = None, None
"""


class Solution:
    """
    @param: start: start value.
    @param: end: end value.
    @return: The root of Segment Tree.
    """
    def build(self, start, end):
        return self.build_helper(start, end)
    
    def build_helper(self, start, end):
        # if start > end, returns null, e.g. [10, 4]
        if start > end: 
            return
        
        root = SegmentTreeNode(start, end)
        
        if start == end:
            return root
        
        # if start != end, root will have children 
        mid = (start + end) // 2 
        root.left = self.build_helper(start, mid)
        root.right = self.build_helper(mid + 1, end)
        
        return root 
        
        

```

439 - Segment Tree Build II

Implement a build method with a given array, so that we can create a corresponding segment tree with every node value represent the corresponding `interval max value` in the array, return the `root` of this segment tree.




```python
Example: 
Input: [3,2,1,4]
Explanation:
The segment tree will be
          [0,3](max=4)
          /          \
       [0,1]         [2,3]    
      (max=3)       (max=4)
      /   \          /    \    
   [0,0]  [1,1]    [2,2]  [3,3]
  (max=3)(max=2)  (max=1)(max=4)
```


```python
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end, max):
        self.start, self.end, self.max = start, end, max
        self.left, self.right = None, None
"""

class Solution:
    """
    @param A: a list of integer
    @return: The root of Segment Tree
    """
    def build(self, A):
        return self.build_helper(0, len(A) - 1, A)
    
    def build_helper(self, start, end, A):
        if start > end: 
            return 
        # max is not known yet, use A[start], as A[start] is the max when start == end  
        root = SegmentTreeNode(start, end, A[start])
        if start == end:
            return root 
        
        # if start < end 
        mid = (start + end) // 2
        root.left = self.build_helper(start, mid, A)
        root.right = self.build_helper(mid + 1, end, A)
        # update max 
        root.max = max(root.left.max, root.right.max)
        print(root.max)
        return root 
```

202 - Segment Tree Query


For an integer array (index from 0 to n-1, where n is the size of this array), in the corresponding SegmentTree, each node stores an extra attribute max to denote the maximum number in the interval of the array (index from start to end).

Design a `query` method with three parameters `root`, `start` and `end`, find the maximum number in the interval `[start, end]` by the given root of segment tree.


```python
# Python 1: use root.left and root.right to tell which child to go for recursion (make sure there is overlap with
#  [start, end])
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end, max):
        self.start, self.end, self.max = start, end, max
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: The root of segment tree.
    @param start: start value.
    @param end: end value.
    @return: The maximum number in the interval [start, end]
    """
    def query(self, root, start, end):
        return self.query_helper(root, start, end)
    
    def query_helper(self, root, start, end):
        # if the [start, end] >= [root.start, root.end]
        if (start <= root.start and end >= root.end):
            return root.max 
        
        # split 
        res = -sys.maxsize 
        mid = (root.start + root.end) // 2 
        # check left child as long as mid >= start 
        if mid >= start: 
            res = max(res, self.query_helper(root.left, start, end)) # still use [start, end] instead of [, mid]
        # check right child as long as mid + 1 < end 
        if mid + 1 <= end:
            res = max(res, self.query_helper(root.right, start, end))
        return res 
```


```python
# Python 2: just return the max of iteration on left and right child, set the non-overlapped root interval 
# to have max = -Inf
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end, max):
        self.start, self.end, self.max = start, end, max
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: The root of segment tree.
    @param start: start value.
    @param end: end value.
    @return: The maximum number in the interval [start, end]
    """
    def query(self, root, start, end):
        return self.query_helper(root, start, end)
    
    def query_helper(self, root, start, end):
        # if the [start, end] >= [root.start, root.end]
        if (start <= root.start and end >= root.end):
            return root.max 
            
        # if no overlap with the current root.start, root.end     
        if end < root.start or start > root.end:
            return -sys.maxsize
        return max(self.query_helper(root.left, start, end), self.query_helper(root.right, start, end))
        
```

203 - Segment Tree Modify

For a Maximum Segment Tree, which each node has an extra value max to store the maximum value in this node's interval.

Implement a `modify` function with three parameter root, index and value to change the node's value with `[start, end] = [index, index]` to the new given value. Make sure after this change, every node in segment tree still has the max attribute with the correct value.


```python
# Python: note that the updated value can be smaller / bigger than the original value, 
#  so one has to find the new max value from childs 
"""
Definition of SegmentTreeNode:
class SegmentTreeNode:
    def __init__(self, start, end, max):
        self.start, self.end, self.max = start, end, max
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of segment tree.
    @param index: index.
    @param value: value
    @return: nothing
    """
    def modify(self, root, index, value):
        if root is None:
            return root 
        return self.modify_helper(root, index, value)
    
    def modify_helper(self, root, index, value):
        
        # end of update, no child
        if root.start == root.end == index:
            # assign the new value as root.max for [index, index] position
            root.max = value
            return
        
        # find which side of the binary tree to go
        mid = (root.start + root.end) // 2 
        
        if index <= mid:
            self.modify_helper(root.left, index, value)

        if index >= mid + 1: 
            self.modify_helper(root.right, index, value)
        
        # update the max 
        root.max = max(root.left.max, root.right.max)

        return

```

206 - Interval Sum


Given an integer array (index from `0` to `n-1`, where `n` is the size of this array), and an query list. Each query has two integers `[start, end]`. For each query, calculate the sum number between index start and end in the given array, return the result list.


Examle: 

Input: array = [1,2,7,8,5],  

queries = [(0,4),(1,2),(2,4)]


Output: [23,9,20]



```python

class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end


class segmentTreeNode:
    def __init__(self, start, end, sum):
        self.start = start 
        self.end = end
        self.left = self.right = None
        self.sum = sum
        
class Solution:
    """
    @param A: An integer list
    @param queries: An query list
    @return: The result list
    """
    def intervalSum(self, A, queries):
        root = self.build(0, len(A) - 1, A)
        results = []
        for interval in queries:
            results.append(self.query(root, interval.start, interval.end))
            
        return results
        
        
    def build(self, start, end, A):
        if start > end:
            return 
        
        root = segmentTreeNode(start, end, A[start])
        if start == end: 
            return root 

        mid = (start + end) // 2 
        root.left = self.build(start, mid, A)
        root.right = self.build(mid + 1, end, A)
        root.sum = root.left.sum + root.right.sum
        
        return root 
    
    
    def query(self, root, start, end):
        if root.start >= start and root.end <= end: 
            return root.sum
        if end < root.start or start > root.end:
            return 0
        return self.query(root.left, start, end) + self.query(root.right, start, end)
    
#     # another way to do query 
#     def query(self, root, start, end):
        
#         if root.start >= start and root.end <= end:
#             return root.sum 
#         if start > root.end or end < root.start:
#             return 0 
#         # note mid = (ROOT.start + ROOT.end)// 2
#         mid = (root.start + root.end) // 2 
#         leftsum = rightsum = 0
#         if mid >= start:
#             # mid is COMPUTED BY ROOT, use [start, mid] will ruin the original [interval]
#             # eg. [start, end] = [0, 0], if used mid, it may become [0, 3], WRONG!
#             leftsum = self.query(root.left, start, end)
#         if mid + 1 <= end:
#             rightsum = self.query(root.right, start, end)
#         return leftsum + rightsum 
        
        
        
                
```


```python
# Solution().intervalSum([1,2,7,8,5], [Interval(1,2),Interval(0,4),Interval(2,4)])
a = Solution().build(0, 4, [1,2,7,8,5])
a
Solution().query(a, 1, 4)
```




    22



207 - Interval Sum II

Given an integer array in the construct method, implement two methods `query(start, end)` and `modify(index, value)`:

For `query(start, end)`, return the sum from index start to index end in the given array.

For `modify(index, value)`, modify the number in the given index to value


```python
class segmentTreeNode:
    def __init__(self, start, end, sum):
        self.start, self.end = start, end 
        self.left, self.right = None, None 
        self.sum = sum 
        
        
class Solution:
    """
    @param: A: An integer array
    """
    def __init__(self, A):
        # build segment tree node and record the root node 
        self.root = self.build(0, len(A) - 1, A)

    """
    @param: start: An integer
    @param: end: An integer
    @return: The sum from start to end
    """
    def query(self, start, end):
        return self.query_helper(self.root, start, end)
    
    def query_helper(self, root, start, end):
        if root.start >= start and root.end <= end:
            return root.sum 
        
        if end < root.start or start > root.end: 
            return 0 
        
        return self.query_helper(root.left, start, end) + self.query_helper(root.right, start, end)

    """
    @param: index: An integer
    @param: value: An integer
    @return: nothing
    """
    def modify(self, index, value):
        return self.modify_helper(self.root, index, value)
    
    def modify_helper(self, root, index, value): 
        
        if root.start == root.end == index:
            root.sum = value
            return 
            
        mid = (root.start + root.end) // 2 
        # update the corresponding child 
        if mid >= index: 
            self.modify_helper(root.left, index, value)
        else: 
            self.modify_helper(root.right, index, value)
        # update the sum at root
        root.sum = root.left.sum + root.right.sum

        
        
    def build(self, start, end, A):
        if start > end: 
            return 
        # create new node 
        root = segmentTreeNode(start, end, A[start])
        if start == end:
            return root 
        
        # if start < end 
        mid = (start + end) // 2 
        root.left = self.build(start, mid, A)
        root.right = self.build(mid + 1, end, A)
        root.sum = root.left.sum + root.right.sum
        return root 
```


```python
a = Solution([1,2,7,8,5])
a.root.left.left.sum
a.query(0, 2)
```




    10



249 - Count of Smaller Number before itself

Give you an integer array (index from `0` to `n-1`, where n is the size of this array, data value from 0 to 10000) . For each element Ai in the array, count the number of element before this element `Ai` is smaller than it and return count number array.

Example: 
    
For array [1,2,7,8,5], return [0,1,2,3,2]


* Segment tree, binary indexed tree




```python
# Python 1:  pay attention to the query and modify index: it's A[i] intead of i 

'''
Time EXCEEDED with the sorting (save space, spend more time) at the beginning, see next version without sorting
'''
class segmentTreeNode:
    def __init__(self, start, end, count):
        self.start = start 
        self.end = end 
        self.left = self.right = None 
        self.count = 0
        
class Solution:
    """
    @param A: an integer array
    @return: A list of integers includes the index of the first number and the index of the last number
    """
    def countOfSmallerNumberII(self, A):
        # map A to an array with its order to save segment tree size 
        sorted_A = sorted(A)
        A = [sorted_A.index(x) for x in A]
        print('A', A)
        
        results = []
        # initialize a segment tree with .count = 0  
        root = self.build(0, len(A) - 1)
        
        for i in range(len(A)):
#             print('Ai', A[i])
            results.append(self.query(root, 0, A[i] - 1))
            # modify, note modification here is orig.val + 1, not == 1  
            self.modify(root, A[i], 1)
            
#             print('root.left.count',root.left.count)
        return results 
        
    def build(self, start, end):
        if start > end:
            return 
        
        root = segmentTreeNode(start, end, 0)
        if start == end:
            return root 
        mid = (start + end) // 2 
        root.left = self.build(start, mid)
        root.right = self.build(mid + 1, end)
        return root 
        
    def query(self,root, start, end):
        if root.start >= start and root.end <= end:
            return root.count
        
        if root.start > end or root.end < start: 
            return 0 
        return self.query(root.left, start, end) + self.query(root.right, start, end)
        
    def modify(self,root, index, value):
        
        if root.start == root.end == index: 
            root.count += value
            return 
        
        mid = (root.start + root.end) // 2 
        
        # if on the left child 
        if mid >= index: 
            self.modify(root.left, index, value)
        else:
            self.modify(root.right, index, value)
            
        # update root.count 
        root.count += value
            
        
        
        

        
```


```python
a = Solution().countOfSmallerNumberII(
[73,82,74,12,25,0,33,46,79,90,6,97,18,84,34,54,64,5,54,44,74,95,90,24,70,94,12,41,79,88,48,82,89,100,33,3,23,21,90,50,26,3,4,21,67,24,59,62,9,78,60,40,4,40,7,5,54,38,68,66])
a
```

    A [42, 48, 43, 10, 18, 0, 20, 28, 46, 53, 7, 58, 12, 50, 22, 31, 37, 5, 31, 27, 43, 57, 53, 16, 41, 56, 10, 26, 46, 51, 29, 48, 52, 59, 20, 1, 15, 13, 53, 30, 19, 1, 3, 13, 39, 16, 34, 36, 9, 45, 35, 24, 3, 24, 8, 5, 31, 23, 40, 38]





    [0,
     1,
     1,
     0,
     1,
     0,
     3,
     4,
     7,
     9,
     1,
     11,
     3,
     11,
     6,
     8,
     9,
     1,
     9,
     8,
     14,
     20,
     19,
     5,
     14,
     23,
     3,
     10,
     20,
     24,
     13,
     23,
     27,
     33,
     8,
     1,
     7,
     7,
     32,
     18,
     11,
     1,
     3,
     9,
     26,
     12,
     26,
     27,
     6,
     35,
     28,
     20,
     3,
     21,
     7,
     5,
     30,
     23,
     39,
     38]




```python
# Python 2: build segment tree with range (min(A), max(A)), takes more space, save time 
class segmentTreeNode:
    def __init__(self, start, end, count):
        self.start = start 
        self.end = end 
        self.left = self.right = None 
        self.count = 0
        
class Solution:
    """
    @param A: an integer array
    @return: A list of integers includes the index of the first number and the index of the last number
    """
    def countOfSmallerNumberII(self, A):
        # map A to an array with its order to save segment tree size 
        # sorted_A = sorted(A)
        # A = [sorted_A.index(x) for x in A]
        if not A or len(A) == 0:
            return []

        results = []
        # initialize a segment tree with .count = 0 
        root = self.build(0, max(A))
        for i in range(len(A)):
            results.append(self.query(root, 0, A[i] - 1))
            # modify, note modification here is orig.val + 1, not == 1  
            self.modify(root, A[i], 1)
            
        return results 
        
    def build(self, start, end):
        if start > end:
            return 
        
        root = segmentTreeNode(start, end, 0)
        if start == end:
            return root 
        mid = (start + end) // 2 
        root.left = self.build(start, mid)
        root.right = self.build(mid + 1, end)
        return root 
        
    def query(self,root, start, end):
        if root.start >= start and root.end <= end:
            return root.count
        
        if root.start > end or root.end < start: 
            return 0 
        return self.query(root.left, start, end) + self.query(root.right, start, end)
        
    def modify(self,root, index, value):
#         print('root.start', root.start)
#         print('root.end', root.end)
#         print('indx', index)
        
        if root.start == root.end == index: 
            root.count += value
            return 
        
        mid = (root.start + root.end) // 2 
        
        # if on the left child 
        if mid >= index: 
            self.modify(root.left, index, value)
        else:
            self.modify(root.right, index, value)
            
        # update root.count 
        root.count += value
            
        
        
        

        
```


```python
a = Solution().countOfSmallerNumberII(
[73,82,74,12,25,0,33,46,79,90,6,97,18,84,34,54,64,5,54,44,74,95,90,24,70,94,12,41,79,88,48,82,89,100,33,3,23,21,90,50,26,3,4,21,67,24,59,62,9,78,60,40,4,40,7,5,54,38,68,66])
a
```

    root.start 0
    root.end 100
    indx 73
    root.start 51
    root.end 100
    indx 73
    root.start 51
    root.end 75
    indx 73
    root.start 64
    root.end 75
    indx 73
    root.start 70
    root.end 75
    indx 73
    root.start 73
    root.end 75
    indx 73
    root.start 73
    root.end 74
    indx 73
    root.start 73
    root.end 73
    indx 73
    root.start 0
    root.end 100
    indx 82
    root.start 51
    root.end 100
    indx 82
    root.start 76
    root.end 100
    indx 82
    root.start 76
    root.end 88
    indx 82
    root.start 76
    root.end 82
    indx 82
    root.start 80
    root.end 82
    indx 82
    root.start 82
    root.end 82
    indx 82
    root.start 0
    root.end 100
    indx 74
    root.start 51
    root.end 100
    indx 74
    root.start 51
    root.end 75
    indx 74
    root.start 64
    root.end 75
    indx 74
    root.start 70
    root.end 75
    indx 74
    root.start 73
    root.end 75
    indx 74
    root.start 73
    root.end 74
    indx 74
    root.start 74
    root.end 74
    indx 74
    root.start 0
    root.end 100
    indx 12
    root.start 0
    root.end 50
    indx 12
    root.start 0
    root.end 25
    indx 12
    root.start 0
    root.end 12
    indx 12
    root.start 7
    root.end 12
    indx 12
    root.start 10
    root.end 12
    indx 12
    root.start 12
    root.end 12
    indx 12
    root.start 0
    root.end 100
    indx 25
    root.start 0
    root.end 50
    indx 25
    root.start 0
    root.end 25
    indx 25
    root.start 13
    root.end 25
    indx 25
    root.start 20
    root.end 25
    indx 25
    root.start 23
    root.end 25
    indx 25
    root.start 25
    root.end 25
    indx 25
    root.start 0
    root.end 100
    indx 0
    root.start 0
    root.end 50
    indx 0
    root.start 0
    root.end 25
    indx 0
    root.start 0
    root.end 12
    indx 0
    root.start 0
    root.end 6
    indx 0
    root.start 0
    root.end 3
    indx 0
    root.start 0
    root.end 1
    indx 0
    root.start 0
    root.end 0
    indx 0
    root.start 0
    root.end 100
    indx 33
    root.start 0
    root.end 50
    indx 33
    root.start 26
    root.end 50
    indx 33
    root.start 26
    root.end 38
    indx 33
    root.start 33
    root.end 38
    indx 33
    root.start 33
    root.end 35
    indx 33
    root.start 33
    root.end 34
    indx 33
    root.start 33
    root.end 33
    indx 33
    root.start 0
    root.end 100
    indx 46
    root.start 0
    root.end 50
    indx 46
    root.start 26
    root.end 50
    indx 46
    root.start 39
    root.end 50
    indx 46
    root.start 45
    root.end 50
    indx 46
    root.start 45
    root.end 47
    indx 46
    root.start 45
    root.end 46
    indx 46
    root.start 46
    root.end 46
    indx 46
    root.start 0
    root.end 100
    indx 79
    root.start 51
    root.end 100
    indx 79
    root.start 76
    root.end 100
    indx 79
    root.start 76
    root.end 88
    indx 79
    root.start 76
    root.end 82
    indx 79
    root.start 76
    root.end 79
    indx 79
    root.start 78
    root.end 79
    indx 79
    root.start 79
    root.end 79
    indx 79
    root.start 0
    root.end 100
    indx 90
    root.start 51
    root.end 100
    indx 90
    root.start 76
    root.end 100
    indx 90
    root.start 89
    root.end 100
    indx 90
    root.start 89
    root.end 94
    indx 90
    root.start 89
    root.end 91
    indx 90
    root.start 89
    root.end 90
    indx 90
    root.start 90
    root.end 90
    indx 90
    root.start 0
    root.end 100
    indx 6
    root.start 0
    root.end 50
    indx 6
    root.start 0
    root.end 25
    indx 6
    root.start 0
    root.end 12
    indx 6
    root.start 0
    root.end 6
    indx 6
    root.start 4
    root.end 6
    indx 6
    root.start 6
    root.end 6
    indx 6
    root.start 0
    root.end 100
    indx 97
    root.start 51
    root.end 100
    indx 97
    root.start 76
    root.end 100
    indx 97
    root.start 89
    root.end 100
    indx 97
    root.start 95
    root.end 100
    indx 97
    root.start 95
    root.end 97
    indx 97
    root.start 97
    root.end 97
    indx 97
    root.start 0
    root.end 100
    indx 18
    root.start 0
    root.end 50
    indx 18
    root.start 0
    root.end 25
    indx 18
    root.start 13
    root.end 25
    indx 18
    root.start 13
    root.end 19
    indx 18
    root.start 17
    root.end 19
    indx 18
    root.start 17
    root.end 18
    indx 18
    root.start 18
    root.end 18
    indx 18
    root.start 0
    root.end 100
    indx 84
    root.start 51
    root.end 100
    indx 84
    root.start 76
    root.end 100
    indx 84
    root.start 76
    root.end 88
    indx 84
    root.start 83
    root.end 88
    indx 84
    root.start 83
    root.end 85
    indx 84
    root.start 83
    root.end 84
    indx 84
    root.start 84
    root.end 84
    indx 84
    root.start 0
    root.end 100
    indx 34
    root.start 0
    root.end 50
    indx 34
    root.start 26
    root.end 50
    indx 34
    root.start 26
    root.end 38
    indx 34
    root.start 33
    root.end 38
    indx 34
    root.start 33
    root.end 35
    indx 34
    root.start 33
    root.end 34
    indx 34
    root.start 34
    root.end 34
    indx 34
    root.start 0
    root.end 100
    indx 54
    root.start 51
    root.end 100
    indx 54
    root.start 51
    root.end 75
    indx 54
    root.start 51
    root.end 63
    indx 54
    root.start 51
    root.end 57
    indx 54
    root.start 51
    root.end 54
    indx 54
    root.start 53
    root.end 54
    indx 54
    root.start 54
    root.end 54
    indx 54
    root.start 0
    root.end 100
    indx 64
    root.start 51
    root.end 100
    indx 64
    root.start 51
    root.end 75
    indx 64
    root.start 64
    root.end 75
    indx 64
    root.start 64
    root.end 69
    indx 64
    root.start 64
    root.end 66
    indx 64
    root.start 64
    root.end 65
    indx 64
    root.start 64
    root.end 64
    indx 64
    root.start 0
    root.end 100
    indx 5
    root.start 0
    root.end 50
    indx 5
    root.start 0
    root.end 25
    indx 5
    root.start 0
    root.end 12
    indx 5
    root.start 0
    root.end 6
    indx 5
    root.start 4
    root.end 6
    indx 5
    root.start 4
    root.end 5
    indx 5
    root.start 5
    root.end 5
    indx 5
    root.start 0
    root.end 100
    indx 54
    root.start 51
    root.end 100
    indx 54
    root.start 51
    root.end 75
    indx 54
    root.start 51
    root.end 63
    indx 54
    root.start 51
    root.end 57
    indx 54
    root.start 51
    root.end 54
    indx 54
    root.start 53
    root.end 54
    indx 54
    root.start 54
    root.end 54
    indx 54
    root.start 0
    root.end 100
    indx 44
    root.start 0
    root.end 50
    indx 44
    root.start 26
    root.end 50
    indx 44
    root.start 39
    root.end 50
    indx 44
    root.start 39
    root.end 44
    indx 44
    root.start 42
    root.end 44
    indx 44
    root.start 44
    root.end 44
    indx 44
    root.start 0
    root.end 100
    indx 74
    root.start 51
    root.end 100
    indx 74
    root.start 51
    root.end 75
    indx 74
    root.start 64
    root.end 75
    indx 74
    root.start 70
    root.end 75
    indx 74
    root.start 73
    root.end 75
    indx 74
    root.start 73
    root.end 74
    indx 74
    root.start 74
    root.end 74
    indx 74
    root.start 0
    root.end 100
    indx 95
    root.start 51
    root.end 100
    indx 95
    root.start 76
    root.end 100
    indx 95
    root.start 89
    root.end 100
    indx 95
    root.start 95
    root.end 100
    indx 95
    root.start 95
    root.end 97
    indx 95
    root.start 95
    root.end 96
    indx 95
    root.start 95
    root.end 95
    indx 95
    root.start 0
    root.end 100
    indx 90
    root.start 51
    root.end 100
    indx 90
    root.start 76
    root.end 100
    indx 90
    root.start 89
    root.end 100
    indx 90
    root.start 89
    root.end 94
    indx 90
    root.start 89
    root.end 91
    indx 90
    root.start 89
    root.end 90
    indx 90
    root.start 90
    root.end 90
    indx 90
    root.start 0
    root.end 100
    indx 24
    root.start 0
    root.end 50
    indx 24
    root.start 0
    root.end 25
    indx 24
    root.start 13
    root.end 25
    indx 24
    root.start 20
    root.end 25
    indx 24
    root.start 23
    root.end 25
    indx 24
    root.start 23
    root.end 24
    indx 24
    root.start 24
    root.end 24
    indx 24
    root.start 0
    root.end 100
    indx 70
    root.start 51
    root.end 100
    indx 70
    root.start 51
    root.end 75
    indx 70
    root.start 64
    root.end 75
    indx 70
    root.start 70
    root.end 75
    indx 70
    root.start 70
    root.end 72
    indx 70
    root.start 70
    root.end 71
    indx 70
    root.start 70
    root.end 70
    indx 70
    root.start 0
    root.end 100
    indx 94
    root.start 51
    root.end 100
    indx 94
    root.start 76
    root.end 100
    indx 94
    root.start 89
    root.end 100
    indx 94
    root.start 89
    root.end 94
    indx 94
    root.start 92
    root.end 94
    indx 94
    root.start 94
    root.end 94
    indx 94
    root.start 0
    root.end 100
    indx 12
    root.start 0
    root.end 50
    indx 12
    root.start 0
    root.end 25
    indx 12
    root.start 0
    root.end 12
    indx 12
    root.start 7
    root.end 12
    indx 12
    root.start 10
    root.end 12
    indx 12
    root.start 12
    root.end 12
    indx 12
    root.start 0
    root.end 100
    indx 41
    root.start 0
    root.end 50
    indx 41
    root.start 26
    root.end 50
    indx 41
    root.start 39
    root.end 50
    indx 41
    root.start 39
    root.end 44
    indx 41
    root.start 39
    root.end 41
    indx 41
    root.start 41
    root.end 41
    indx 41
    root.start 0
    root.end 100
    indx 79
    root.start 51
    root.end 100
    indx 79
    root.start 76
    root.end 100
    indx 79
    root.start 76
    root.end 88
    indx 79
    root.start 76
    root.end 82
    indx 79
    root.start 76
    root.end 79
    indx 79
    root.start 78
    root.end 79
    indx 79
    root.start 79
    root.end 79
    indx 79
    root.start 0
    root.end 100
    indx 88
    root.start 51
    root.end 100
    indx 88
    root.start 76
    root.end 100
    indx 88
    root.start 76
    root.end 88
    indx 88
    root.start 83
    root.end 88
    indx 88
    root.start 86
    root.end 88
    indx 88
    root.start 88
    root.end 88
    indx 88
    root.start 0
    root.end 100
    indx 48
    root.start 0
    root.end 50
    indx 48
    root.start 26
    root.end 50
    indx 48
    root.start 39
    root.end 50
    indx 48
    root.start 45
    root.end 50
    indx 48
    root.start 48
    root.end 50
    indx 48
    root.start 48
    root.end 49
    indx 48
    root.start 48
    root.end 48
    indx 48
    root.start 0
    root.end 100
    indx 82
    root.start 51
    root.end 100
    indx 82
    root.start 76
    root.end 100
    indx 82
    root.start 76
    root.end 88
    indx 82
    root.start 76
    root.end 82
    indx 82
    root.start 80
    root.end 82
    indx 82
    root.start 82
    root.end 82
    indx 82
    root.start 0
    root.end 100
    indx 89
    root.start 51
    root.end 100
    indx 89
    root.start 76
    root.end 100
    indx 89
    root.start 89
    root.end 100
    indx 89
    root.start 89
    root.end 94
    indx 89
    root.start 89
    root.end 91
    indx 89
    root.start 89
    root.end 90
    indx 89
    root.start 89
    root.end 89
    indx 89
    root.start 0
    root.end 100
    indx 100
    root.start 51
    root.end 100
    indx 100
    root.start 76
    root.end 100
    indx 100
    root.start 89
    root.end 100
    indx 100
    root.start 95
    root.end 100
    indx 100
    root.start 98
    root.end 100
    indx 100
    root.start 100
    root.end 100
    indx 100
    root.start 0
    root.end 100
    indx 33
    root.start 0
    root.end 50
    indx 33
    root.start 26
    root.end 50
    indx 33
    root.start 26
    root.end 38
    indx 33
    root.start 33
    root.end 38
    indx 33
    root.start 33
    root.end 35
    indx 33
    root.start 33
    root.end 34
    indx 33
    root.start 33
    root.end 33
    indx 33
    root.start 0
    root.end 100
    indx 3
    root.start 0
    root.end 50
    indx 3
    root.start 0
    root.end 25
    indx 3
    root.start 0
    root.end 12
    indx 3
    root.start 0
    root.end 6
    indx 3
    root.start 0
    root.end 3
    indx 3
    root.start 2
    root.end 3
    indx 3
    root.start 3
    root.end 3
    indx 3
    root.start 0
    root.end 100
    indx 23
    root.start 0
    root.end 50
    indx 23
    root.start 0
    root.end 25
    indx 23
    root.start 13
    root.end 25
    indx 23
    root.start 20
    root.end 25
    indx 23
    root.start 23
    root.end 25
    indx 23
    root.start 23
    root.end 24
    indx 23
    root.start 23
    root.end 23
    indx 23
    root.start 0
    root.end 100
    indx 21
    root.start 0
    root.end 50
    indx 21
    root.start 0
    root.end 25
    indx 21
    root.start 13
    root.end 25
    indx 21
    root.start 20
    root.end 25
    indx 21
    root.start 20
    root.end 22
    indx 21
    root.start 20
    root.end 21
    indx 21
    root.start 21
    root.end 21
    indx 21
    root.start 0
    root.end 100
    indx 90
    root.start 51
    root.end 100
    indx 90
    root.start 76
    root.end 100
    indx 90
    root.start 89
    root.end 100
    indx 90
    root.start 89
    root.end 94
    indx 90
    root.start 89
    root.end 91
    indx 90
    root.start 89
    root.end 90
    indx 90
    root.start 90
    root.end 90
    indx 90
    root.start 0
    root.end 100
    indx 50
    root.start 0
    root.end 50
    indx 50
    root.start 26
    root.end 50
    indx 50
    root.start 39
    root.end 50
    indx 50
    root.start 45
    root.end 50
    indx 50
    root.start 48
    root.end 50
    indx 50
    root.start 50
    root.end 50
    indx 50
    root.start 0
    root.end 100
    indx 26
    root.start 0
    root.end 50
    indx 26
    root.start 26
    root.end 50
    indx 26
    root.start 26
    root.end 38
    indx 26
    root.start 26
    root.end 32
    indx 26
    root.start 26
    root.end 29
    indx 26
    root.start 26
    root.end 27
    indx 26
    root.start 26
    root.end 26
    indx 26
    root.start 0
    root.end 100
    indx 3
    root.start 0
    root.end 50
    indx 3
    root.start 0
    root.end 25
    indx 3
    root.start 0
    root.end 12
    indx 3
    root.start 0
    root.end 6
    indx 3
    root.start 0
    root.end 3
    indx 3
    root.start 2
    root.end 3
    indx 3
    root.start 3
    root.end 3
    indx 3
    root.start 0
    root.end 100
    indx 4
    root.start 0
    root.end 50
    indx 4
    root.start 0
    root.end 25
    indx 4
    root.start 0
    root.end 12
    indx 4
    root.start 0
    root.end 6
    indx 4
    root.start 4
    root.end 6
    indx 4
    root.start 4
    root.end 5
    indx 4
    root.start 4
    root.end 4
    indx 4
    root.start 0
    root.end 100
    indx 21
    root.start 0
    root.end 50
    indx 21
    root.start 0
    root.end 25
    indx 21
    root.start 13
    root.end 25
    indx 21
    root.start 20
    root.end 25
    indx 21
    root.start 20
    root.end 22
    indx 21
    root.start 20
    root.end 21
    indx 21
    root.start 21
    root.end 21
    indx 21
    root.start 0
    root.end 100
    indx 67
    root.start 51
    root.end 100
    indx 67
    root.start 51
    root.end 75
    indx 67
    root.start 64
    root.end 75
    indx 67
    root.start 64
    root.end 69
    indx 67
    root.start 67
    root.end 69
    indx 67
    root.start 67
    root.end 68
    indx 67
    root.start 67
    root.end 67
    indx 67
    root.start 0
    root.end 100
    indx 24
    root.start 0
    root.end 50
    indx 24
    root.start 0
    root.end 25
    indx 24
    root.start 13
    root.end 25
    indx 24
    root.start 20
    root.end 25
    indx 24
    root.start 23
    root.end 25
    indx 24
    root.start 23
    root.end 24
    indx 24
    root.start 24
    root.end 24
    indx 24
    root.start 0
    root.end 100
    indx 59
    root.start 51
    root.end 100
    indx 59
    root.start 51
    root.end 75
    indx 59
    root.start 51
    root.end 63
    indx 59
    root.start 58
    root.end 63
    indx 59
    root.start 58
    root.end 60
    indx 59
    root.start 58
    root.end 59
    indx 59
    root.start 59
    root.end 59
    indx 59
    root.start 0
    root.end 100
    indx 62
    root.start 51
    root.end 100
    indx 62
    root.start 51
    root.end 75
    indx 62
    root.start 51
    root.end 63
    indx 62
    root.start 58
    root.end 63
    indx 62
    root.start 61
    root.end 63
    indx 62
    root.start 61
    root.end 62
    indx 62
    root.start 62
    root.end 62
    indx 62
    root.start 0
    root.end 100
    indx 9
    root.start 0
    root.end 50
    indx 9
    root.start 0
    root.end 25
    indx 9
    root.start 0
    root.end 12
    indx 9
    root.start 7
    root.end 12
    indx 9
    root.start 7
    root.end 9
    indx 9
    root.start 9
    root.end 9
    indx 9
    root.start 0
    root.end 100
    indx 78
    root.start 51
    root.end 100
    indx 78
    root.start 76
    root.end 100
    indx 78
    root.start 76
    root.end 88
    indx 78
    root.start 76
    root.end 82
    indx 78
    root.start 76
    root.end 79
    indx 78
    root.start 78
    root.end 79
    indx 78
    root.start 78
    root.end 78
    indx 78
    root.start 0
    root.end 100
    indx 60
    root.start 51
    root.end 100
    indx 60
    root.start 51
    root.end 75
    indx 60
    root.start 51
    root.end 63
    indx 60
    root.start 58
    root.end 63
    indx 60
    root.start 58
    root.end 60
    indx 60
    root.start 60
    root.end 60
    indx 60
    root.start 0
    root.end 100
    indx 40
    root.start 0
    root.end 50
    indx 40
    root.start 26
    root.end 50
    indx 40
    root.start 39
    root.end 50
    indx 40
    root.start 39
    root.end 44
    indx 40
    root.start 39
    root.end 41
    indx 40
    root.start 39
    root.end 40
    indx 40
    root.start 40
    root.end 40
    indx 40
    root.start 0
    root.end 100
    indx 4
    root.start 0
    root.end 50
    indx 4
    root.start 0
    root.end 25
    indx 4
    root.start 0
    root.end 12
    indx 4
    root.start 0
    root.end 6
    indx 4
    root.start 4
    root.end 6
    indx 4
    root.start 4
    root.end 5
    indx 4
    root.start 4
    root.end 4
    indx 4
    root.start 0
    root.end 100
    indx 40
    root.start 0
    root.end 50
    indx 40
    root.start 26
    root.end 50
    indx 40
    root.start 39
    root.end 50
    indx 40
    root.start 39
    root.end 44
    indx 40
    root.start 39
    root.end 41
    indx 40
    root.start 39
    root.end 40
    indx 40
    root.start 40
    root.end 40
    indx 40
    root.start 0
    root.end 100
    indx 7
    root.start 0
    root.end 50
    indx 7
    root.start 0
    root.end 25
    indx 7
    root.start 0
    root.end 12
    indx 7
    root.start 7
    root.end 12
    indx 7
    root.start 7
    root.end 9
    indx 7
    root.start 7
    root.end 8
    indx 7
    root.start 7
    root.end 7
    indx 7
    root.start 0
    root.end 100
    indx 5
    root.start 0
    root.end 50
    indx 5
    root.start 0
    root.end 25
    indx 5
    root.start 0
    root.end 12
    indx 5
    root.start 0
    root.end 6
    indx 5
    root.start 4
    root.end 6
    indx 5
    root.start 4
    root.end 5
    indx 5
    root.start 5
    root.end 5
    indx 5
    root.start 0
    root.end 100
    indx 54
    root.start 51
    root.end 100
    indx 54
    root.start 51
    root.end 75
    indx 54
    root.start 51
    root.end 63
    indx 54
    root.start 51
    root.end 57
    indx 54
    root.start 51
    root.end 54
    indx 54
    root.start 53
    root.end 54
    indx 54
    root.start 54
    root.end 54
    indx 54
    root.start 0
    root.end 100
    indx 38
    root.start 0
    root.end 50
    indx 38
    root.start 26
    root.end 50
    indx 38
    root.start 26
    root.end 38
    indx 38
    root.start 33
    root.end 38
    indx 38
    root.start 36
    root.end 38
    indx 38
    root.start 38
    root.end 38
    indx 38
    root.start 0
    root.end 100
    indx 68
    root.start 51
    root.end 100
    indx 68
    root.start 51
    root.end 75
    indx 68
    root.start 64
    root.end 75
    indx 68
    root.start 64
    root.end 69
    indx 68
    root.start 67
    root.end 69
    indx 68
    root.start 67
    root.end 68
    indx 68
    root.start 68
    root.end 68
    indx 68
    root.start 0
    root.end 100
    indx 66
    root.start 51
    root.end 100
    indx 66
    root.start 51
    root.end 75
    indx 66
    root.start 64
    root.end 75
    indx 66
    root.start 64
    root.end 69
    indx 66
    root.start 64
    root.end 66
    indx 66
    root.start 66
    root.end 66
    indx 66





    [0,
     1,
     1,
     0,
     1,
     0,
     2,
     3,
     6,
     8,
     0,
     10,
     2,
     10,
     5,
     7,
     8,
     0,
     8,
     7,
     13,
     19,
     18,
     4,
     13,
     22,
     2,
     9,
     19,
     23,
     12,
     22,
     26,
     32,
     7,
     0,
     6,
     6,
     31,
     17,
     10,
     0,
     2,
     8,
     25,
     11,
     25,
     26,
     5,
     34,
     27,
     19,
     2,
     20,
     6,
     4,
     29,
     22,
     38,
     37]




```python
# Python 3: use BIT 
class Solution:
    """
    @param A: an array
    @return: total of reverse pairs
    """

    def countOfSmallerNumberII(self, A):
        if not A or len(A) == 0:
            return []
        # remove duplicates by dictionary  
       # self.A = list(dict.fromkeys(A))
    # make sure that every value >= 1 
        self.A = [x + 1 for x in A]
        
        
        # map A to their corresponding locations in the array (not necessary, but can simplify the numbers)
#         sorted_A = sorted(A)
#         self.A = [sorted_A.index(x) + 1 for x in self.A]
#         self.max = len(self.A) 
        self.max = max(self.A)
    
        
        self.C = [0] * (self.max + 1)
        self.C[0] = self.max + 1
        results = []
        for i in range(len(self.A)):
#             results.append(self.sum(self.max) - self.sum(self.A[i]))
            results.append(self.sum(self.A[i] - 1))
            self.add(self.A[i], 1)
        return results
            
        
        
    def lowbit(self, i):
        return i & (-i)
    
    def add(self, index, val):
        
        while index <= self.max: 
            self.C[index] += val
            index += self.lowbit(index)
            
    def sum(self, index):
        res = 0 
        while index > 0: 
            res += self.C[index]
            index -= self.lowbit(index)
        return res 
            
        

   
```


```python
a = Solution().countOfSmallerNumberII([73,82,74,12,25,0,33,46,79,90,6,97,18,84,34,54,64,5,54,44,74,95,90,24,70,94,12,41,79,88,48,82,89,100,33,3,23,21,90,50,26,3,4,21,67,24,59,62,9,78,60,40,4,40,7,5,54,38,68,66])
a
```

    [102, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19
    20
    21
    22
    23
    24
    25
    26
    27
    28
    29
    30
    31
    32
    33
    34
    35
    36
    37
    38
    39
    40
    41
    42
    43
    44
    45
    46
    47
    48
    49
    50
    51
    52
    53
    54
    55
    56
    57
    58
    59





    [0,
     1,
     1,
     0,
     1,
     0,
     3,
     4,
     7,
     9,
     1,
     11,
     3,
     11,
     6,
     8,
     9,
     1,
     9,
     8,
     14,
     20,
     19,
     5,
     14,
     23,
     3,
     10,
     20,
     24,
     13,
     23,
     27,
     33,
     8,
     1,
     7,
     7,
     32,
     18,
     11,
     1,
     3,
     9,
     26,
     12,
     26,
     27,
     6,
     35,
     28,
     20,
     3,
     21,
     7,
     5,
     30,
     23,
     39,
     38]




```python
a = [1, 2, 3, 4]
a + 1
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-17-bf98811c06e5> in <module>
          1 a = [1, 2, 3, 4]
    ----> 2 a + 1
    

    TypeError: can only concatenate list (not "int") to list

