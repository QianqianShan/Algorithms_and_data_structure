
## Two Pointers Algorithm 

415 - Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Input: "A man, a plan, a canal: Panama"

Output: true

Explanation: "amanaplanacanalpanama"

* Two pointers 



```python
class Solution:
    """
    @param s: A string
    @return: Whether the string is a valid palindrome
    """
    def isPalindrome(self, s):
        # write your code here
        left, right = 0, len(s) - 1 
        while (left < right):
            while (left < right and not s[left].isalpha() and not s[left].isdigit()):
                left += 1 
            while (left < right and not s[right].isalpha() and not s[right].isdigit()):
                right -= 1 
            if (s[left].lower() != s[right].lower()):
                return False 
            # if the two characters are equal, move forward 
            left += 1 
            right -= 1 
        return True 


```

891 - Valid Palindrome II

Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

* Two pointers 

#### Idea: use two pointers from the two sides of the string, at the position where s[left] != s[right],  try to delete either s[left] or s[right] to see if the rest could be palindrome 


```python
# Python 1: use a helper variable count to directly check if it's palindrome after deleting one element 
class Solution:
    """
    @param s: a string
    @return: nothing
    """
    def validPalindrome(self, s):
        # number of characters can be removed 
        count = 1 
        left, right = 0, len(s) - 1 
        # print(count)
        while (left < right):
            while (left < right and not s[left].isdigit() and not s[left].isalpha()):
                left += 1 
            while (left < right and not s[right].isdigit() and not s[right].isalpha()):
                right -= 1 
            while (count > 0 and s[left] != s[right]):
                if (s[left + 1] == s[right]):
                    left += 1 
                    count -= 1 
                elif (s[left] == s[right - 1]):
                    right -= 1 
                    count -= 1
            if (count == 0 and s[left] != s[right]):
                return False
            left += 1
            right -= 1
            
        return True 

```


```python
p = Solution()
p.validPalindrome(
 "ognfjhgbjhzkqhzadmgqbwqsktzqwjexqvzjsopolnmvnymbbzoofzbbmynvmnloposjzvqxejwqztksqwbqgmdazhqkzhjbghjfno")
# p.validPalindrome('aba')
```




    False




```python
# Python 2: split the problem into two parts, tell if it's palindrome and tell if the two characters at the pointer 
# locations are the same 
class Solution:
    """
    @param s: a string
    @return: nothing
    """
    def validPalindrome(self, s):
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                break
            left += 1
            right -= 1
            
        if left >= right:
            return True
            
        return self.is_palindrome(s, left + 1, right) or self.is_palindrome(s, left, right - 1)

    def is_palindrome(self, s, left, right):
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
            
        return True
```


```python
# My Python version: use the previous isPalindrome function and pass sliced string to it  
class Solution:
    """
    @param s: a string
    @return: nothing
    """
    def validPalindrome(self, s):
        left, right = 0, len(s) - 1 
        
        # find the left and right positions where the two are not equal 
        # 
        while (left < right and s[left] == s[right]):
            left += 1 
            right -= 1 
            #print('left', left)
            #print('right', right)
        if (left >= right):
            return True 
        #print(s[left : (right - 1)])
        #print(s[(left+1) : right])
        
        # note for string slicing, the element with index after : is NOT included 
        return self.isPalindrome(s[left : (right)]) or self.isPalindrome(s[(left+1) : (right + 1)]) # use brackets

    def isPalindrome(self, s):
        # write your code here
        left, right = 0, len(s) - 1 
        while (left < right):
            while (left < right and not s[left].isalpha() and not s[left].isdigit()):
                left += 1 
            while (left < right and not s[right].isalpha() and not s[right].isdigit()):
                right -= 1 
            if (s[left].lower() != s[right].lower()):
                return False 
            # if the two characters are equal, move forward 
            left += 1 
            right -= 1 
        return True 

```


```python
p = Solution()
p.validPalindrome("abccbaa")
```




    True



608 - Two Sum II - Input array is sorted

Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.


* Two pointers 

* Hash table 


```python
# Python 1: two pointers 
class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def twoSum(self, numbers, target):
        # first sort the numbers 
        # print(type(numbers))
        if len(numbers) == 0:
            return [-1, -1]
        # two pointers 
        left, right = 0, len(numbers) - 1 
        # not binary search, no mid value is going to be calculated, so don't have to use  left + 1 < right 
        while left < right:
            if numbers[left] + numbers[right] > target:
                right -= 1 
            elif numbers[left] + numbers[right] < target:
                left += 1 
            else:
                return [left + 1, right + 1] # index starts from 1 for this problem 

        # not found after the while loop 
        return [-1, -1]    
        
```


```python
# python 2: use while loops inside while loop
class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def twoSum(self, numbers, target):
        if not numbers:
            return [-1, -1]

        # two pointers
        left, right = 0, len(numbers) - 1 
        while left < right:
            while left < right and numbers[left] + numbers[right] > target:
                right -= 1 
            while left < right and numbers[left] + numbers[right] < target:
                left += 1 
            if left < right and numbers[left] + numbers[right] == target:
                return [left + 1, right + 1]

        return [-1, -1]
```

56 - Two Sum


Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are zero-based.

* Two pointers : O(nlogn) + O(n)  (sort + traverse) 

* Hash table : O(n) time 


```python
# Python 1: use hash table 

class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def twoSum(self, numbers, target):
        if not numbers:
            return [-1, -1]
        # a dictionary to save location as key and value as val  
        hash_table = {}
        for i, num in enumerate(numbers): 
            # first tell if num + x = target with x already in hash table 
            if target - num in hash_table:
                return [hash_table[target - num], i]
            # add num, i pair to hash table, do it after the if condition to avoid cases like
            # num + num = target 
            hash_table.setdefault(num, i)

        return [-1, -1]
            
        
            
```


```python
p = Solution()
p.twoSum([1, 2, 7, 9], 9)
```




    [1, 2]




```python
p = Solution()
p.twoSum([2,3], 5)
```




    [0, 1]




```python
# Python 2: first sort then use two pointers, take special care for the case when two elements equal to each other 
#           and their sum == target 

class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def twoSum(self, numbers, target):
        if not numbers:
            return [-1, -1]
        orig = numbers[:]
        # sort 
        numbers.sort()
        
        # two pointers
        left, right = 0, len(numbers) - 1 
        while left < right:
            while left < right and numbers[left] + numbers[right] > target:
                right -= 1 
            while left < right and numbers[left] + numbers[right] < target:
                left += 1 
            if left < right and numbers[left] + numbers[right] == target:
                if numbers[left] == numbers[right]:
                    return sorted([i for i, x in enumerate(orig) if x == numbers[left]])[:2]
                return sorted([orig.index(numbers[left]), orig.index(numbers[right])])
        return [-1, -1]
                

```


```python
# Python 3: use two pointers 
# 1. sort the array 
# 2. two pointers from two sides of the array 
class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def twoSum(self, numbers, target):
        # first sort the numbers 
        # print(type(numbers))
        if (len(numbers) == 0):
            return [-1, -1]
       
        # first save the old locations of each number 
        hash_table = {}
        for i in range(len(numbers)):
            hash_table[numbers[i]] = i
        
        # numbers = sorted(numbers)
        orig_numbers = numbers[:] # do a full slice for a NEW object to avoid reference 
        print(orig_numbers)
        
         # sort
        '''
        Note that numbers.sort() will sort numbers internally and return a Nonetype, 
        so numbers = numbers.sort() will not be sorted numbers. 
        Use: 
        numbers.sort() 
        or 
        numbers = sorted(numbers)
        '''
        
        numbers.sort()
        # print(type(numbers))
        # two pointers 
        left, right = 0, len(numbers) - 1 
        # not binary search, no mid value is going to be calculated, so don't have to use  left + 1 < right 
        while (left < right):
            if ((numbers[left] + numbers[right]) > target):
                right -= 1 
            elif ((numbers[left] + numbers[right]) < target):
                left += 1 
            else:
                # in case of duplicated numbers that cannot be handled by hash table 
                if (numbers[left] == numbers[right]):
                    return [x for x in range(len(orig_numbers)) if orig_numbers[x] == numbers[left]]
                else:
                    return sorted([hash_table[numbers[left]], hash_table[numbers[right]]])
        # not found after the while loop 
        return [-1, -1]    
        
```


```python
p = Solution()
p.twoSum([0,4,3,0], 0)
```

    [0, 4, 3, 0]
    0
    0
    tt





    [0, 3]



533 - Two Sum - Closest to target

Given an array `nums` of `n` integers, find two integers in nums such that the sum is closest to a given number, target.

Return the absolute value of difference between the sum of the two integers and the target.




```python
import sys 
class Solution:
    """
    @param nums: an integer array
    @param target: An integer
    @return: the difference between the sum and the target
    """
    def twoSumClosest(self, nums, target):
        if not nums:
            return -1 
        nums.sort()
        left, right = 0, len(nums) - 1 
        diff = sys.maxsize
        while left < right:
            if nums[left] + nums[right] > target:
                diff = min(nums[left] + nums[right] - target, diff) 
                right -= 1 
            elif nums[left] + nums[right] < target:
                diff = min(target - (nums[left] + nums[right]), diff)
                left += 1 
            else:
                diff = 0
                return diff 
        return diff 
        
            
```

443 - Two Sum - Greater than target

Given an array of integers, find how many pairs in the array such that their sum is bigger than a specific target number. Please return the number of pairs.


```python
class Solution:
    """
    @param nums: an array of integer
    @param target: An integer
    @return: an integer
    """
    def twoSum2(self, nums, target):
        if not nums:
            return 0 
        nums.sort()
        count = 0 
        left, right = 0, len(nums) - 1 
        while left < right:
            if left < right and nums[left] + nums[right] <= target:
                left += 1 
            if left < right and nums[left] + nums[right] > target:
                count += right - left
                right -= 1 
        return count 
        

```


```python
Solution().twoSum2([1,0,-1], 0)
```




    1



607 - Two Sum III - Data structure design

Design and implement a TwoSum class. It should support the following operations: add and find.

`add` - Add the number to an internal data structure.

`find` - Find if there exists any pair of numbers which sum is equal to the value.

Example:

add(1); add(3); add(5);

find(4) // return true

find(7) // return false

* Data structure design , hash table

* Think about which one uses more frequently, `add` or `find`, then consider the complexity for designing 


```python
# dictionary basics 
a = {"new":1, "old":2}
for num in a:
    print(num)

```

    new
    old



```python
# Python 
class TwoSum:
    """
    @param number: An integer
    @return: nothing
    """
    def __init__(self):
        self.count = {} # initialize a dictionary to save the nums and their counts, can also use a set 
        
    def add(self, number):
        self.count.setdefault(number, 0)
        self.count[number] += 1 
#         if number not in self.count:
#             self.count[number] = 1
#         else:
#             self.count[number] += 1 
        

    """
    @param value: An integer
    @return: Find if there exists any pair of numbers which sum is equal to the value.
    """
    def find(self, value):
        # use hash map, make sure to look for PAIRS of data  
        for i in self.count:
            if value - i in self.count and (value - i != i or self.count[i] > 1):
                print(i)
                return True 
        return False
```


```python
p = TwoSum();
p.add(2)
p.add(3)
print(p.count)
print(p.find(4))
p.find(5)
p.find(6)
p.add(3)
p.find(6)
p.count
```

    {2: 1, 3: 1}
    False
    2
    3





    {2: 1, 3: 2}



587 - Two Sum - Unique pairs

Given an array of integers, find how many unique pairs in the array such that their sum is equal to a specific target number. Please return the number of pairs. For example,

Input: nums = [1,1,2,45,46,46], target = 47 

Output: 2

Explanation:

1 + 46 = 47

2 + 45 = 47


* Hash table, two pointers


```python
# Hash table 
class Solution:
    """
    @param nums: an array of integer
    @param target: An integer
    @return: An integer
    """
    def twoSum6(self, nums, target):
        if not nums:
            return 0 
        hash_table = {}
        for num in nums:
            hash_table.setdefault(num, 0)
            hash_table[num] += 1 
        count = 0
        for num in hash_table:
            if target - num in hash_table and hash_table[num] > 0 and (target - num != num or hash_table[num] > 1):
                count += 1 
                hash_table[target - num] = 0
                hash_table[num] = 0 
        return count 
                

```


```python
p = Solution()
p.twoSum6([7,11,11,1,2,3,4], 22)
# p.twoSum6([1,1,2,45,46,46], 47)
```

    hash {7: 1, 11: 2, 1: 1, 2: 1, 3: 1, 4: 1}





    1




```python
# Python 2: Two pointers
# 1. sort 
# 2. search 
class Solution:
    """
    @param nums: an array of integer
    @param target: An integer
    @return: An integer
    """
    def twoSum6(self, nums, target):
        if not nums:
            return 0 
        nums.sort()
        count = 0 
        left, right = 0, len(nums) - 1 
        while left < right:
            # move to possible pairs with nums[left] + nums[right] == target 
            while left < right and nums[left] + nums[right] > target:
                right -= 1 
            while left < right and nums[left] + nums[right] < target:
                left += 1 
            # when nums[left] + nums[right] == target 
            if left < right and nums[left] + nums[right] == target:
                count += 1 
                # deal with possible duplicated numbers 
                while left < right and nums[left + 1] == nums[left]:
                    left += 1 
                while left < right and nums[right - 1] == nums[right]:
                    right -= 1 
                # general case, keep search pairs 
                left += 1 
                right -=1 
        return count 

          
        
```


```python
p = Solution()
p.twoSum6([7,11,11,11, 11,1,2,3,4], 22)
p.twoSum6([1,1,2,45,46,46], 47)



```




    2



609 - Two Sum - Less than or equal to target

Given an array of integers, find how many pairs in the array such that their sum is less than or equal to a specific target number. Please return the number of pairs.

Example:

Input: nums = [2, 7, 11, 15], target = 24. 

Output: 5. 

Explanation:

2 + 7 < 24

2 + 11 < 24

...


```python
class Solution:
    """
    @param nums: an array of integer
    @param target: an integer
    @return: an integer
    """
    def twoSum5(self, nums, target):
        if not nums:
            return 0 
        nums.sort()
        count = 0 
        left, right = 0, len(nums) - 1 
        while left < right:
            while left < right and nums[left] + nums[right] > target:
                right -= 1 
            if left < right and nums[left] + nums[right] <= target:
                count += right - left
                left += 1 
        return count 
                
```

610 - Two Sum - Difference equals to target

Given an array of integers, find two numbers that their difference equals to a target value.

Index1 must be less than index2. Please note that your returned answers (both index1 and index2) are NOT zero-based.

* Two pointers, hash table 

#### Idea: use two pointers in sorted array 


```python
# Python 1: 
# 1. two pointers 
# 2. save the original numbers location info and then sort 
class Solution:
    """
    @param nums: an array of Integer
    @param target: an integer
    @return: [index1 + 1, index2 + 1] (index1 < index2)
    """
    def twoSum7(self, nums, target):
        # special case 
        if (len(nums) == 0 or len(nums) == 1):
            return []
        # in case target is negative 
        target = abs(target)
        # sort 
        orig_nums = nums[:]
        nums.sort()
        fast = 1 # fast pointer starts from the second location 
        for slow in range(len(nums)): 
            # note fast is NOT reset to a number next to slow (saves a lot time)
            while (fast < len(nums) - 1) and (nums[fast] - nums[slow] < target):
                fast += 1 
            if (nums[fast] - nums[slow] == target):
                if (target != 0):
                    return sorted([[x for x in range(len(nums)) if orig_nums[x] == nums[slow]][0] + 1, 
                              [x for x in range(len(nums)) if orig_nums[x] == nums[fast]][0] + 1])
                elif (target ==0) and fast != slow:
                    return sorted([[x for x in range(len(nums)) if orig_nums[x] == nums[slow]][0] + 1, 
                              [x for x in range(len(nums)) if orig_nums[x] == nums[fast]][1] + 1])
        return [-1, -1]

```


```python
p = Solution()
p.twoSum7([2,7,15,24], 5)
p.twoSum7([1, 0, 1], 0)

# nums = [1, 2, 3, 4, 5, 5]
# [x for x in range(len(nums)) if nums[x] == 5][0]  + 1
```




    [1, 3]




```python
nums = [1, 5, 3, 6, 9, 7, 8]
new = [(num, i) for i, num in enumerate(nums)]
new
```




    [(1, 0), (5, 1), (3, 2), (6, 3), (9, 4), (7, 5), (8, 6)]




```python
#Python solution: created 
# use  nums = [(num, i) for i, num in enumerate(nums)] to save nums location info 

class Solution:
    """
    @param nums {int[]} n array of Integer
    @param target {int} an integer
    @return {int[]} [index1 + 1, index2 + 1] (index1 < index2)
    """
    def twoSum7(self, nums, target):
        # Write your code here
        nums = [(num, i) for i, num in enumerate(nums)]
        target = abs(target)    
        n, indexs = len(nums), []
    
        nums = sorted(nums, key=lambda x: x[0])

        j = 0
        for i in range(n):
            if i == j:
                j += 1
            while j < n and nums[j][0] - nums[i][0] < target:
                j += 1
            if j < n and nums[j][0] - nums[i][0] == target:
                indexs = [nums[i][1] + 1, nums[j][1] + 1]

        if indexs[0] > indexs[1]:
            indexs[0], indexs[1] = indexs[1], indexs[0]

        return indexs
```

57 - 3Sum three sum 

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Example: 

Input:[-1,0,1,2,-1,-4]


Output:	[[-1, 0, 1],[-1, -1, 2]]



* Two pointers 

#### Idea: loop through one variable and see (a + b = target = -c)

Make sure to remove dupicates on the iteration a step and on the two sum step. 


```python
# Python: use two pointers 

class Solution:
    """
    @param numbers: Give an array numbers of n integer
    @return: Find all unique triplets in the array which gives the sum of zero.
    """
    def threeSum(self, numbers):
        if (len(numbers) < 3):
            return []
        res = []
        numbers.sort()
        for i in range(len(numbers)):
            if (numbers[i] == numbers[i - 1]) and i > 0:  # when i = 0, i - 1 = -1 
                continue
            target = -numbers[i]
            
            # newNumbers must exclude the previous looped target value:
            # for each pair that satifies a + b = -c, there must be another pair that 
            # satisfies a + c = -b (when -b is the new target), repeated
            newNumbers = numbers[(i + 1):]
            goodList = self.twoSum6(newNumbers, target)
            # append the numbers together if there are any returned lists from twoSum6 function 
            if (len(goodList) > 0):
                for j in range(len(goodList)):
                    res.append([numbers[i]] + goodList[j])
        return res
    # nums is already sorted in the above function 
    def twoSum6(self, nums, target):
        res = []
        left, right = 0, len(nums) - 1 
        while (left < right):
            if (nums[left] + nums[right] == target):
                res.append([nums[left], nums[right]])
                # avoid cases such as [11, 11, 11, 11] for target = 22
                # move to the closet to center location which equals nums[left] or nums[right] 
                while (left < right and nums[right] == nums[right - 1]):
                    right -= 1 
                while (left < right and nums[left] == nums[left + 1]):
                    left += 1
                                # move to the next possible set 
                left += 1 
                right -= 1
            elif (nums[left] + nums[right] > target):
                right -= 1
            elif (nums[left] + nums[right] < target):
                left += 1
        return res
            
        
        
        
```


```python
p = Solution()
p.threeSum([1,0,-1,-1,-1,-1,0,1,1,1])
```




    [[-1, 0, 1]]




```python
class Solution:
    """
    @param numbers: Give an array numbers of n integer
    @return: Find all unique triplets in the array which gives the sum of zero.
    """
    def threeSum(self, numbers):
        if not numbers or len(numbers) < 3:
            return []
        # sort 
        numbers.sort()
        print(numbers)
        results = []
        # loop a with a + b + c = 0 
        for i in range(len(numbers) - 2):
            # duplicates 
            if numbers[i] == numbers[i - 1] and i > 0:
                continue
            two_sums = self.two_sum(numbers[i + 1 :], -numbers[i])
            if len(two_sums) > 0:
                for j in range(len(two_sums)):
                    results.append([numbers[i]] + two_sums[j])
        return results
            
    def two_sum(self, numbers, target):
        left, right = 0, len(numbers) - 1 
        result = []
        while left < right:
            while left < right and numbers[left] + numbers[right] > target: 
                right -= 1 
            while left < right and numbers[left] + numbers[right] < target:
                left += 1 
            if left < right and numbers[left] + numbers[right] == target:
                result.append([numbers[left], numbers[right]])
                # duplicates 
                while left < right and numbers[left + 1] == numbers[left]:
                    left += 1 
                while left < right and numbers[right - 1] == numbers[right]:
                    right -= 1 
                left += 1 
                right -= 1
        return result
```


```python
p = Solution()
p.threeSum([1,0,-1,-1,-1,-1,0,1,1,1])`
```

    [-1, -1, -1, -1, 0, 0, 1, 1, 1, 1]
    -1
    number -1
    [[0, 1]]
    0
    1





    [[-1, 0, 1]]



59 - 3Sum Closest

Given an array `S` of `n` integers, find three integers in `S` such that the sum is closest to a given number, target. Return the sum of the three integers.

You may assume that each input would have exactly one solution.

Example: 

Input:

[2,7,11,15],3


Output:

20

Explanation:

2+7+11=20


```python
# python 1: fix one number of the three numbers, iterate the other two 
import sys
class Solution:
    """
    @param numbers: Give an array numbers of n integer
    @param target: An integer
    @return: return the sum of the three integers, the sum closest target.
    """
    def threeSumClosest(self, numbers, target):
        if not numbers or len(numbers) < 3:
            return None 
        if len(numbers) == 3:
            return sum(numbers)
        numbers.sort()
        closest_sum = sys.maxsize 
        for i in range(len(numbers)):
            num = numbers[i]
            left, right = i + 1, len(numbers) - 1 
            while left < right:
                new_sum = numbers[i] + numbers[left] + numbers[right]
                if abs(new_sum - target) < abs(closest_sum - target):
                    closest_sum = new_sum 
                if new_sum < target:
                    left += 1
                if new_sum > target:
                    right -= 1 
                if new_sum == target:
                    return new_sum
        return closest_sum
        
```


```python
# python 2: convert to a two sum closest problem 
import sys
class Solution:
    """
    @param numbers: Give an array numbers of n integer
    @param target: An integer
    @return: return the sum of the three integers, the sum closest target.
    """
    def threeSumClosest(self, numbers, target):
        if not numbers or len(numbers) < 3:
            return None 
        if len(numbers) == 3:
            return sum(numbers)
        numbers.sort()
        print(numbers)
        self.closest_sum = sys.maxsize 
        for i in range(len(numbers) - 2):
            # remove numbers[i] 
            new_sum = numbers[i] + self.two_sum_closest(numbers[:i] + numbers[i+1:], target - numbers[i])

#             new_sum = numbers[i] + self.two_sum_closest(numbers[i+1:], target - numbers[i])
#             print('numbers i', numbers[i])
#             print(new_sum)
            self.closest_sum = new_sum if abs(new_sum - target) < abs(self.closest_sum - target) else self.closest_sum
        return self.closest_sum
        
        
        
    def two_sum_closest(self, nums, target):

        left, right = 0, len(nums) - 1 
        diff = sys.maxsize 
        res = sys.maxsize 
        while left < right:
            if nums[left] + nums[right] > target:
                if nums[left] + nums[right] - target < diff:
                    diff = min(nums[left] + nums[right] - target, diff) 
                    res = nums[left] + nums[right] 
                right -= 1 
            elif nums[left] + nums[right] < target:
                if target - (nums[left] + nums[right]) < diff:
                    diff = min(target - nums[left] + nums[right], diff) 
                    res = nums[left] + nums[right] 
                left += 1 
            else:
                diff = 0
                return nums[left] + nums[right]
        return res
        
```


```python
Solution().threeSumClosest([1,2,33,23,2423,33,23,1,7,6,8787,5,33,2,3,-23,-54,-67,100,400,-407,-500,-35,-8,0,0,7,6,0,1,2,-56,-89,24,2], 148)
```

    [-500, -407, -89, -67, -56, -54, -35, -23, -8, 0, 0, 0, 1, 1, 1, 2, 2, 2, 2, 3, 5, 6, 6, 7, 7, 23, 23, 24, 33, 33, 33, 100, 400, 2423, 8787]





    147



58 - 4Sum

Given an array `S` of `n` integers, are there elements `a, b, c`, and `d` in `S` such that `a + b + c + d = target`?

Find all `unique` quadruplets in the array which gives the sum of target.

Elements in a quadruplet (a,b,c,d) must be in non-descending order. (ie, a ≤ b ≤ c ≤ d)
The solution set must not contain duplicate quadruplets.

Example: 

Input:[1,0,-1,0,-2,2],0

Output:

[[-1, 0, 0, 1]
,[-2, -1, 1, 2]
,[-2, 0, 0, 2]]



```python
class Solution:
    """
    @param numbers: Give an array
    @param target: An integer
    @return: Find all unique quadruplets in the array which gives the sum of zero
    """
    def fourSum(self, numbers, target):
        if not numbers or len(numbers) < 4:
            return []
        results = []
        numbers.sort()
        for i in range(len(numbers) - 3):
            if i > 0 and numbers[i] == numbers[i - 1]:
                continue 
            for j in range(i + 1, len(numbers) - 2):
                if j > i + 1 and numbers[j] == numbers[j - 1]:
                    continue 
                new_target = target - numbers[i] - numbers[j]
                two_sums = self.two_sum(numbers, j + 1, len(numbers) - 1, new_target)
                if len(two_sums) > 0:
                    for last_two in two_sums:
                        results.append([numbers[i], numbers[j]] + last_two)
        return results 
    
    def two_sum(self, numbers, left, right, target):
        if right - left < 1:
            return []
        res = []
        while left < right:
            while left < right and numbers[left] + numbers[right] < target:
                left += 1 
            while left < right and numbers[left] + numbers[right] > target:
                right -= 1 
            if left < right and numbers[left] + numbers[right] == target:
                res.append([numbers[left], numbers[right]])
                while left < right and numbers[left + 1] == numbers[left]:
                    left +=1 
                while left < right and numbers[right] == numbers[right - 1]:
                    right -= 1 
                left += 1 
                right -= 1 
        return res 
                   

```


```python
Solution().fourSum([1,0,-1,0,-2,2], -2)
```




    [[-2, -1, 0, 1]]



521 - Remove Duplicate Numbers in Array

Given an array of integers, remove the duplicate numbers in it.

You should:

Do it in place in the array.

Move the unique numbers to the front of the array.

Return the total number of the unique numbers.

Example:

Input:

nums = [1,3,1,4,4,2]

Output:

[1,3,4,2,?,?]

* Two pointers 



```python
# Python : O(nlog n) for sort, O(1) space 
class Solution:
    """
    @param nums: an array of integers
    @return: the number of unique integers
    """
    def deduplication(self, nums):
        if not nums:
            return 0 
        nums.sort()
        slow, fast = 0, 0
        while fast < len(nums) - 1:
            if nums[fast + 1] != nums[fast]:
                slow += 1 
                nums[slow] = nums[fast + 1]
            fast += 1 
        return slow + 1
```


```python
p = Solution()
p.deduplication([1,3,1,4,4,2])
# p.deduplication([1])
```




    4




```python
# Python 2: hash set, use count to be the index of nums to update nums in-place 
# O(n) time, O(n) space
class Solution:
    """
    @param nums: an array of integers
    @return: the number of unique integers
    """
    def deduplication(self, nums):
        if not nums:
            return 0
        hash_set = set()
        count = 0 
        for num in nums:
            if num not in hash_set:
                hash_set.add(num)
                nums[count] = num 
                count += 1 
        return len(hash_set)

                
```

604 - Window Sum

Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the sum of the element inside the window at each moving.

Input：array = [1,2,7,8,5], k = 3

Output：[10,17,20]

Explanation：

1 + 2 + 7 = 10

2 + 7 + 8 = 17

7 + 8 + 5 = 20

* Two pointers 


```python
# Python 1: sliding window 
# 1. compute the window sum for the first time 
# 2. Add a following value and remove the first value to obtain the window sum for next position 
class Solution:
    """
    @param nums: a list of integers.
    @param k: length of window.
    @return: the sum of the element inside the window at each moving.
    """
    def winSum(self, nums, k):
        # corner cases 
        if not nums:
            return []
        if len(nums) < k:
            return sum(nums)
        window_sum = sum(nums[0:k])
        results = [window_sum]
        for i, num in enumerate(nums):
            if i < (k):
                continue
            window_sum = window_sum + num - nums[i - k]
            results.append(window_sum)
        return results 
            
        
       
```


```python
p = Solution()
p.winSum([1,2,7,7,2], 3)
# p.winSum([], 0)
```




    [10, 16, 16]




```python
# brute force: too slow 
class Solution:
    """
    @param nums: a list of integers.
    @param k: length of window.
    @return: the sum of the element inside the window at each moving.
    """
    def winSum(self, nums, k):
        if k <= 0:
            return []
        if len(nums) < k:
            return []
        slow, fast = 0, k
        res = [] # save results 
        while fast <= len(nums):
            res.append(sum(nums[slow:(fast)])) 
            slow += 1 
            fast += 1
        return res 
        

```


```python
p = Solution()
p.winSum([1,2,7,7,2], 3)
p.winSum([], 0)
```




    []



228 -  Middle of Linked List

Find the middle node of a linked list.

* Linked list, data stream 

* Traverse ONLY one time , O(n/2)



```python
# Python 
# use two pointers: the slow pointer moves one step each time and the fast pointer moves two steps each time 

"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: the head of linked list.
    @return: a middle node of the linked list
    """
    def middleNode(self, head):
        
        # special case 
        if head is None:
            return None 
        # initialization 
        slow = head 
        fast = slow.next 
        
        # traverse using fast pointer 
        while fast is not None and fast.next is not None:
            # so the pointer can jump two nodes each time 
            slow = slow.next 
            fast = fast.next.next 
        return slow 
        
```

539 - Move Zeroes

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

* Two pointers



```python
# Use two pointers 
# slow moves together with fast only when elements non zero
# slow stops at the position with 0 until it swaps with fast 
# fast loops to the last element 
class Solution:
    """
    @param nums: an integer array
    @return: nothing
    """
    def moveZeroes(self, nums):
        slow, fast = 0, 0
        while fast < len(nums): # loop through all values 
            if nums[fast] != 0:
                nums[slow], nums[fast] = nums[fast], nums[slow]
                slow += 1 
               #  print(nums)
            fast += 1 
        return nums
```


```python
# [0,1,0,3,12]
p = Solution()
p.moveZeroes([5,4,3,2,1])
# p.moveZeroes([0,1,0,3,12])
p.moveZeroes([-1,2,-3,4,0,1,0,-2,0,0,1])

```




    [-1, 2, -3, 4, 1, -2, 1, 0, 0, 0, 0]




```python
# partition: CANNOT keep the relative order of non-negative numbers 
class Solution:
    """
    @param nums: an integer array
    @return: nothing
    """
    def moveZeroes(self, nums):
        if not nums:
            return nums 
        left, right = 0, len(nums) - 1 
        while left <= right:
            while left <= right and  nums[left] > 0:
                left += 1 
            while left <= right and nums[right] == 0:
                right -= 1 
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
        return nums 
```

463 - Sort Integers

Given an integer array, sort it in ascending order. Use selection sort, bubble sort, insertion sort or any O(n2) algorithm.

* Sort, merge sort, quick sort  




```python
# Python 1: standard 
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        A.sort()
```


```python
# selection sort 选择排序
# iterate over each element, find the smallest number in the rest array that is smaller than the current element 
# and swap 
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        for i in range(len(A)):
            t = i 
            # find the smallest in the rest 
            for j in range(i + 1, len(A)):
                if A[j] < A[t]:
                    t = j 
            if t != i:
                A[i], A[t] = A[t], A[i]

```

Insertion sort 
https://qianqianshan.com/post/introduction-to-algorithms/#pf3


```python
# insertion sort 
# see algorithm at https://qianqianshan.com/post/introduction-to-algorithms/#pf3
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        for j in range(1, len(A)):
            key = A[j]
            i = j - 1 
            while i >= 0  and A[i] > key:
                A[i + 1] = A[i]
                i -= 1 
            A[i + 1] = key

        return A 
```


```python
# bubble sort 
class Solution:
    # @param {int[]} A an integer array
    # @return nothing
    def sortIntegers(self, A):
        for i in range(0, len(A) - 1):
            for j in range(0, len(A) - 1):
                if A[j] > A[j + 1]:
                    A[j], A[j + 1] = A[j + 1], A[j]
```


```python
# Python 2: Merge sort 
class Solution:
    """
    @param A: an integer array
    @return: nothing
    """
    def sortIntegers(self, A):
        if len(A) == 0:
            return A
        
        # a list to save sorted values 
        result = [0] * len(A)
        self.merge_sort(A, 0, len(A) - 1, result)
        return result
    
    def merge_sort(self, A, start, end, result):
        if (start >= end):
            return 
        self.merge_sort(A, start, (start + end)//2, result)
        self.merge_sort(A, (start + end)//2 + 1, end, result)
        self.merge(A, start, end, result)
        
    def merge(self, A, start, end, result):
        mid = (start + end) // 2
        # initialization
        # sorted array from index left to mid 
        left = start
        # sorted array from mid + 1 to end 
        right = mid + 1 
        
        # index of result
        index = start   
        
        # only when both left and right are smaller than their 
        # upper limit, the comparison can be done without errors on index range
        while left <= mid and right <= end:
            if A[left] < A[right]:
                result[index] = A[left]
                left += 1 
            else: 
                result[index] = A[right]
                right += 1 
            index += 1 
                
        # add the (possible) rest of results back, only one while statement will be executed 
        while left <= mid:
            result[index] = A[left]
            index += 1
            left += 1
        while right <= end:
            result[index] = A[right]
            index += 1
            right += 1
            
        # copy result to A 
        for i in range(start, end + 1):
            A[i] = result[i]
        

```


```python
p = Solution()
p.sortIntegers([3,2,1,4,5])

```




    [1, 2, 3, 4, 5]



464 - Sort Integers II

Given an integer array, sort it in ascending order. Use quick sort, merge sort, heap sort or any O(nlogn) algorithm.

* Quick sort, merge sort 

Quick sort https://qianqianshan.com/post/introduction-to-algorithms/


```python
# Python 1: Solution based on quick sort at https://qianqianshan.com/post/introduction-to-algorithms/
class Solution:
    def sortIntegers2(self, A):
        if len(A) == 0:
            return A
        start, end = 0, len(A) - 1
        self.quick_sort(A, start, end)
        return A
            
            

    def quick_sort(self, A, left, right):
        if left >= right:
            return
        pivot_loc = self.partition(A, left, right)
        self.quick_sort(A, left, pivot_loc - 1)
        self.quick_sort(A, pivot_loc + 1, right)
        
    def partition(self, A, start, end):
        #pivotVal = A[(start + end) // 2]
        #A[0], A[(start + end) // 2] = A[(start + end) // 2], A[0]
        pivot_val = A[start]
        i = start 
        for j in range(start + 1, end + 1):
            if A[j] <= pivot_val:
                i += 1
                A[i], A[j] = A[j], A[i]
        A[start], A[i] = A[i], A[start]
        return i 
        
```


```python
# Python 2: standard solution 
class Solution:
    """
    @param A: an integer array
    @return: nothing
    """
    def sortIntegers2(self, A):
        self.quick_sort(A, 0, len(A) - 1)
        return A 
    
    def quick_sort(self, A, start, end):
        if start >= end:
            return 
        left, right = start, end 
        # key point 1: pivot is the value, not the index
        pivot = A[(start + end) // 2];
        
        # key point 2: every time you compare left & right, it should be 
        # left <= right not left < right
        while left <= right:
            while left <= right and A[left] < pivot:
                left += 1 
            while left <= right and A[right] > pivot:
                right -= 1 
            if left <= right:
                A[left], A[right] = A[right], A[left]
                left += 1 
                right -= 1 
            
        
        # now left > right, further sort the smaller sets 
        self.quick_sort(A, start, right)
        self.quick_sort(A, left, end)
            
```


```python
p = Solution()
p.sortIntegers2([3,2,1,4,5,0])
```




    [0, 1, 2, 3, 4, 5]



31 - Partition Array

Given an array nums of integers and an int k, partition the array (i.e move the elements in "nums") such that:

All elements < k are moved to the left

All elements >= k are moved to the right

Return the partitioning index, i.e the first index i nums[i] >= k.

Example:

Input:

[3,2,2,1],2

Output:1

Explanation:

the real array is[1,2,2,3].So return 1

* Two pointers, sort 



```python
class Solution:
    """
    @param nums: The integer array you should partition
    @param k: An integer
    @return: The index after partition
    """
    def partitionArray(self, nums, k):
        if len(nums) == 0:
            return 0
        left = 0
        for right in range(len(nums)):
            if nums[right] < k: 
                # as the pivot value is NOT chosen from nums, so we first do the swap and then add 1 to left pointer 
                nums[left], nums[right] = nums[right], nums[left]
                left += 1 
        return left

        

```


```python
p = Solution()
p.partitionArray([1, 2, 8,4,3], 4)
```




    3




```python
# Use while loop 
class Solution:
    """
    @param nums: The integer array you should partition
    @param k: An integer
    @return: The index after partition
    """
    def partitionArray(self, nums, k):
        if not nums:
            return 0 
        left, right = 0, len(nums) - 1 
        while left <= right:
            while left <= right and nums[left] < k:
                left += 1 
            # note the difference with the above quick sort >= instead of > 
            while left <= right and nums[right] >= k:
                right -= 1
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1 
                right -= 1 
        return left

```

625 - Partition Array II

Partition an unsorted integer array into three parts:

* The front part < low
* The middle part >= low & <= high
* The tail part > high

Return any of the possible solutions.

Example: 

Input:

[4,3,4,1,2,3,1,2]

2

3

Output:

[1,1,2,3,2,3,4,4]

Explanation:

[1,1,2,2,3,3,4,4] is also a correct answer, but [1,2,1,2,3,3,4,4] is not



```python
# Python 1: 
class Solution:
    """
    @param nums: an integer array
    @param low: An integer
    @param high: An integer
    @return: nothing
    """
    def partition2(self, nums, low, high):
        if not nums:
            return nums 
        left, right = 0, len(nums) - 1 
        i = 0 
        # note right changes as looping over i in the while statement 
        while i <= right:
            if nums[i] < low:
                nums[i], nums[left] = nums[left], nums[i]
                left += 1 
                # also moves i one step forward 
                i += 1 
            elif nums[i] > high:
                nums[i], nums[right] = nums[right], nums[i]
                right -=1 
                # i doesn't change 
            else:
                # if low <= nums[i] <= high
                i += 1 
        return nums 

```


```python
# Python 2: partition twice 
class Solution:
    """
    @param nums: an integer array
    @param low: An integer
    @param high: An integer
    @return: nothing
    """
    def partition2(self, nums, low, high):
        if not nums:
            return nums 
        # first split the data into two parts < low, >= low 
        left, right = 0, len(nums) - 1 
        while left <= right:
            while left <= right and nums[left] < low:
                left += 1 
            while left <= right and nums[right] >= low:
                right -=1 
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left +=1 
                right -=1 
                
        # reset right value , split data from left to right into two parts <= high, > high
        right = len(nums) - 1 
        
        while left <= right:
            while left <= right and nums[left] <= high:
                left += 1 
            while left <= right and nums[right] > high:
                right -= 1 
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1 
                right -= 1 
        return nums
            

```

148 - Sort Colors

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Example: 

Given [1, 0, 1, 2],

sort it in-place to [0, 1, 1, 2].




```python
class Solution:
    """
    @param nums: A list of integer which is 0, 1 or 2 
    @return: nothing
    """
    def sortColors(self, nums):
        if not nums:
            return []
        colors = [0, 1, 2]
        left, right = 0, len(nums) - 1 
        for color in colors:
            while left <= right:
                while left <= right and nums[left] == color:
                    left += 1 
                while left <= right and nums[right] > color:
                    right -= 1 
                if left <= right:
                    nums[left], nums[right] = nums[right], nums[left]
                    left += 1 
                    right -= 1 
            right = len(nums) - 1 
        return nums 
        

```

143 - Sort Colors II

Given an array of n objects with k different colors (numbered from 1 to k), sort them so that objects of the same color are adjacent, with the colors in the order 1, 2, ... k.

Example: 

Given colors=[3, 2, 2, 1, 4], k=4, your code should sort colors in-place to [1, 2, 2, 3, 4].

* Sort, partition, two pointers

* O(k * n)


```python
# partition 
class Solution:
    """
    @param colors: A list of integer
    @param k: An integer
    @return: nothing
    """
    def sortColors2(self, colors, k):
        if not colors:
            return colors 
        # partition k times 
        left, right = 0, len(colors) - 1 
        
        # iterate over color i 
        for i in range(1, k + 1):
            while left < right:
                while left < right and colors[left] <= i:
                    left += 1 
                while left < right and colors[right] > i:
                    right -= 1 
                if left < right:
                    colors[left], colors[right] = colors[right], colors[left]
            # left is now the first position with value > i 
            # reset right 
            right = len(colors) - 1 
        return colors 
                
            

```


```python
class Solution:
    """
    @param colors: A list of integer
    @param k: An integer
    @return: nothing
    """
    def sortColors2(self, colors, k):
        if (len(colors) == 0):
            return []
        # do partition k times `
        start, end = 0, len(colors) - 1
        for color_i in range(1, k + 1):
            # do partition of color_i and all the others 
            start = self.partitionArray(colors, start, end, color_i)
        return colors 
    
    def partitionArray(self, nums, start, end, pivotVal):
        i = start 
        for j in range(start, end + 1):
            if nums[j] == pivotVal:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
        return i 
```


```python
p = Solution()
p.sortColors2([2,1,1,2,2, 3, 2, 1, 4], 4)
```




    [1, 1, 1, 2, 2, 2, 2, 3, 4]



5 - Kth Largest Element

Find K-th largest element in an array.

Example: 

Input:

n = 1, nums = [1,3,4,2]

Output:
4

* Sort 



#### Convert to the (len(A) - k)-th element when A is sorted in ascending order 

###### Two different cases of left and right in partition, so three different cases for the return statements in parition function 

1. 1, 3, 2 (right), 5, 7(left), 6, 9 
2. 1, 3, 2(right), 5(left), 7, 7


```python
class Solution:
    # @param k & A a integer and an array
    # @return ans a integer
    def kthLargestElement(self, k, A):
        if not A or k < 1 or k > len(A):
            return None
        return self.partition(A, 0, len(A) - 1, len(A) - k)
        
    def partition(self, nums, start, end, k):
        """
        During the process, it's guaranteed start <= k <= end
        """
        if start == end:
            return nums[k]
            
        left, right = start, end
        pivot = nums[(start + end) // 2]
        
        # where there is only one element left == right 
        while left <= right:
            while left <= right and nums[left] < pivot:
                left += 1
            while left <= right and nums[right] > pivot:
                right -= 1
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left, right = left + 1, right - 1
                
        # left is not bigger than right
        if k <= right:
            return self.partition(nums, start, right, k)
        if k >= left:
            return self.partition(nums, left, end, k)
        # if nums[right], nums[k], nums[left]
        return nums[k]
```

461 - Kth Smallest Numbers in Unsorted Array

Find the kth smallest number in an unsorted integer array.

Example: 

Input: [3, 4, 1, 2, 5], k = 3

Output: 3


```python
class Solution:
    """
    @param k: An integer
    @param nums: An integer array
    @return: kth smallest element
    """
    def kthSmallest(self, k, nums):
        if not nums:
            return -1 
        # the k-th smallest is at position k - 1 in sorted nums 
        return self.partition(nums, 0, len(nums) - 1, k - 1)
    
    def partition(self, nums, start, end, k):
        if start == end:
            return nums[start]
        left, right = start, end 
        pivot = nums[(left + right) // 2] 

        while left <= right:
            while left <= right and nums[left] < pivot:
                left += 1 
            while left <= right and nums[right] > pivot:
                right -= 1 
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1 
                right -= 1 
        # print('right', right)
        # print('left', left)
        if k  <= right:
            return self.partition(nums, start, right, k)
        if k  >= left:
            return self.partition(nums, left, end, k)
        return nums[k]


```

382 - Triangle Count

Given an array of integers, how many three numbers can be found in the array, so that we can build an triangle whose three edges length is the three numbers that we find?

Example: 

Input: [3, 4, 6, 7]

Output: 3

Explanation:

They are (3, 4, 6), 
         (3, 6, 7),
         (4, 6, 7)
         
         

#### Idea: first sort the array to have a <= b <= c <= d ...

As long as sum of  two smaller values > a bigger value in the array, they can build triangluar. 


```python
class Solution:
    """
    @param S: A list of integers
    @return: An integer
    """
    def triangleCount(self, S):
        if not S:
            return 0 
        S.sort()
        count = 0 
        for i in range(len(S) - 1, 1, -1):
            print('i', i)
            left, right = 0, i - 1 
            while left < right:
                while left < right and S[left] + S[right] <= S[i]:
                    left += 1 
                if left < right and S[left] + S[right] > S[i]:
                    # add counts that works with the current S[right] as one of the numbers
                    # eg. [1, 2, 3, 4, 5], i = 5, S[left] = 2, S[right] = 4, add two pairs [2, 4], [3, 4]
                    count += right - left 
                    # move right to a smaller value and check if there are other possibilities 
                    # check if [2, 3] qualifies
                    right -= 1
                    print('count', count)
        return count 
                    
                    

```


```python
Solution().triangleCount([4,4,4,4])
```

    i 3
    count 2
    count 3
    i 2
    count 4





    4



894 - Pancake Sorting

Given an unsorted array, sort the given array. You are allowed to do only following operation on array.

`flip(arr, i)`: Reverse array from 0 to i

Unlike a traditional sorting algorithm, which attempts to sort with the fewest comparisons possible, the goal is to sort the sequence in as few reversals as possible.


```python
# Python: first find the max element index, flip it to the beginning, flip it again to the end 
class Solution:
    """
    @param array: an integer array
    @return: nothing
    """
    def pancakeSort(self, array):
        cur_size = len(array)
        while cur_size > 1:
            # find index of max in array[0], ..., array[cur_size - 1]
            max_ind = self.find_max(array, cur_size)
            # move the max element to end of current array if not already at the end 
            if max_ind != cur_size - 1:
                # move the max element to index = 0
                self.flip(array, max_ind)
                # move the max element to the last posistion
                self.flip(array, cur_size - 1)
            cur_size -= 1 
        return array
    def flip(self, array, i):
        start = 0
        while start < i:
            array[start], array[i] = array[i], array[start]
            start += 1 
            i -= 1 
    def find_max(self, array, n):
        max_ind = 0 
        for i in range(n):
            if array[i] > array[max_ind]:
                max_ind = i
        return max_ind
            


```


```python
Solution().pancakeSort([6,11,10,12,7,23,20])
```




    [6, 7, 10, 11, 12, 20, 23]



380 - Intersection of Two Linked Lists

Write a program to find the node at which the intersection of two singly linked lists begins.

* If the two linked lists have no intersection at all, return `null`.

* The linked lists must retain their original structure after the function returns.

* You may assume there are no cycles anywhere in the entire linked structure.


###### Assumption: if there is an intersection, then all elements after the beginning of the intersection will be the same for two linked lists.


```python
# first find the lengths of two linked list then compare 
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param headA: the first list
    @param headB: the second list
    @return: a ListNode
    """
    def getIntersectionNode(self, headA, headB):
        if headA is None or headB is None:
            return None 
        # find length of both linked lists 
        node_A = headA 
        node_B = headB 
        length_A = 0 
        length_B = 0 
        while node_A is not None:
            length_A += 1 
            node_A = node_A.next 
        while node_B is not None:
            length_B += 1 
            node_B = node_B.next 

        # compare 
        node_A = headA 
        node_B = headB 
        while length_A > length_B:
            node_A = node_A.next 
            length_A -= 1 
        while length_A < length_B:
            node_B = node_B.next 
            length_B -= 1 
        # now two lists have the same length, compare their nodes 
        while node_A is not None and node_A is not node_B:
            node_A = node_A.next 
            node_B = node_B.next 
        if node_A is None:
            return None 
        return node_A 
            

```

102 - Linked List Cycle

Given a linked list, determine if it has a cycle in it.

	Example 1:
		Input: 21->10->4->5,  then tail connects to node index 1(value 10).
		Output: true
		
	Example 2:
		Input: 21->10->4->5->null
		Output: false



```python
# use two pointers, if there is a cycle, fast and slow pointers will equal at some point 
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: The first node of linked list.
    @return: True if it has a cycle, or false
    """
    def hasCycle(self, head):    
        if not head:
            return False 
        slow, fast = head, head.next 
        while fast is not None and fast.next is not None:
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast:
                return True 
        return False 


```


```python
# python 3: use three pointers 
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: The first node of linked list.
    @return: True if it has a cycle, or false
    """
    def hasCycle(self, head):    
        if not head:
            return False 
        slow, mid, fast = head, head, head.next 
        while fast is not None and fast.next is not None:
            slow = slow.next 
            mid = mid.next.next
            fast = fast.next.next 
            if slow == fast or slow == mid:
                return True 
        return False 


```

103 - Linked List Cycle II

Given a linked list, return the node where the cycle begins.

If there is no cycle, return null.


```python
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: The first node of linked list.
    @return: The node where the cycle begins. if there is no cycle, return null
    """
    def detectCycle(self, head):
        if not head:
            return None 
        slow, fast = head, head
        while fast is not None and fast.next is not None:
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast:
                slow = head
                while slow != fast:
                    slow = slow.next 
                    fast = fast.next 
                return slow 
        return None 

```


```python
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: The first node of linked list.
    @return: The node where the cycle begins. if there is no cycle, return null
    """
    def detectCycle(self, head):
        if not head:
            return None 
        slow, fast = head, head 
        while fast is not None and fast.next is not None:
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast:
                while head != slow:
                    head = head.next 
                    slow = slow.next 
                return head 
        return None 

```
