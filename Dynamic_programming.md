
## Dynamic programming

* Backpack 

92 - Backpack

Given `n` items with size `Ai`, an integer `m` denotes the size of a backpack. How full you can fill this backpack?

You can not divide any item into small pieces.



Example 1:
    
	Input:  
        
        A = [3,4,8,5], backpack size m = 10
        
	Output:  9


```python
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param A: Given n items with size A[i]
    @return: The maximum size
    """
    def backPack(self, m, A):
        table = [[False] * (m + 1) for _ in range(len(A) + 1)]
        
        # put zero element with size no bigger than zero 
        table[0][0] = True
        
        # table[0][j] == False, loop through each element in A for each j = 1, ..., m
        for i in range(1, len(A) + 1): 
            table[i][0] = True
            for j in range(1, m + 1):
                if j >= A[i - 1]:
                    table[i][j] = table[i - 1][j] or table[i - 1][j - A[i - 1]]
                else:
                    table[i][j] = table[i - 1][j]
                    
        for j in range(m, -1, -1):
            if table[len(A)][j]:
                return j
        return 0 

```


```python
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param A: Given n items with size A[i]
    @return: The maximum size
    """
    def backPack(self, m, A):
        n = len(A)
        table = [[0] * (m + 1) for _ in range(n + 1)]
        
        for i in range(n):
            for j in range(1, m + 1):
                table[i + 1][j] = table[i][j]
                if j >= A[i]:
                    table[i + 1][j] = max(table[i + 1][j], table[i][j - A[i]] + A[i])
        return table[n][m]
```


```python
Solution().backPack(10, [3, 4, 8, 5])
```




    9




```python
# Python 2: use intervals 
```


```python

```

125 -  Backpack II

Given `n` items with size `Ai` and value `Vi`, and a backpack with size `m`. What's the maximum value can you put into the backpack?

Example: 

Given 4 items with size [2, 3, 5, 7] and value [1, 5, 2, 4], and a backpack with size 10. The maximum value is 9.


```python
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param A: Given n items with size A[i]
    @param V: Given n items with value V[i]
    @return: The maximum value
    """
    def backPackII(self, m, A, V):
        n = len(A)
        table = [[0] * (m + 1) for _ in range(n + 1)]
        
        for i in range(n):
            for j in range(m + 1):
                table[i + 1][j] = table[i][j]
                if j >= A[i]:
                    table[i + 1][j] = max(table[i + 1][j], table[i][j - A[i]] + V[i])
            
        return table[n][m]

```


```python
class Solution:
    # @param m: An integer m denotes the size of a backpack
    # @param A & V: Given n items with size A[i] and value V[i]
    def backPackII(self, m, A, V):
        table = [0 for i in range(m + 1)]
        n = len(A)`
        for i in range(n):
            for j in range(m, A[i]-1, -1):
                table[j] = max(table[j] , table[j - A[i]] + V[i])
        return table[m]
                
```


```python

```
