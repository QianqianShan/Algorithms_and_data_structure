
## Data structure: stack, queue, hash and heap 

* See differences of hash table, hash set and hash map at https://www.quora.com/What-is-the-difference-between-HashSet-HashMap-and-hash-table-How-do-they-behave-in-a-multi-threaded-environment

495 - Implement Stack

Implement a stack. You can use any data structure inside a stack except stack itself to implement it.

Input:

push(1)

pop()

push(2)

top()  // return 2

pop()

isEmpty() // return true

push(3)

isEmpty() // return false


* Stack 


```python
class Stack:
    """
    @param: x: An integer
    @return: nothing
    """
    # initialize an empty list 
    def __init__(self):
        self.stack = []
        
    def push(self, x):
        self.stack.append(x)

    """
    @return: nothing
    """
    def pop(self):
        if not self.isEmpty():
            last_element = self.stack[-1]
            self.stack = self.stack[:len(self.stack) - 1]
            return last_element
        else:
            return None 

    """
    @return: An integer
    """
    def top(self):
        if not self.isEmpty():
            return self.stack[-1]

    """
    @return: True if the stack is empty
    """
    def isEmpty(self):
        return len(self.stack) == 0

```

494 - Implement Stack by Two Queues

Implement a stack by two queues. The queue is first in first out (FIFO). That means you can not directly pop the last element in a queue.

* Queue, stack 


```python
# 1. push elements to q1 
# 2. when popping out an element, keeping popping from q1 and append to q1 until there is only one element left 
#   in q1 (the one needs to be popped)
# 3. swap q1 and q2 
from collections import deque 

class Stack:
    """
    @param: x: An integer
    @return: nothing
    """
    def __init__(self):
        self.queue1 = deque()
        self.queue2 = deque()
        
    def push(self, x):
        # push new elements to queue1 
        self.queue1.append(x)

    """
    @return: nothing
    """
    def pop(self):
        if self.isEmpty():
            return 
        
        # pop all except the last elements in queue1 to queue2, pop, then swap queue1 and queue2 
        while len(self.queue1) > 1:
            self.queue2.append(self.queue1.popleft())
        # pop the last element in queue 1 
        self.queue1.popleft()
        
        # swap queue1 and queue2 
        self.queue1, self.queue2 = self.queue2, self.queue1
        

    """
    @return: An integer
    """
    def top(self):
        if self.isEmpty():
            return 
        return self.queue1[-1]


    """
    @return: True if the stack is empty
    """
    def isEmpty(self):
        return len(self.queue1) == 0

```

224 - Implement Three Stacks by Single Array

Implement three stacks by single array.

You can assume the three stacks has the same size and big enough, you don't need to care about how to extend it if one of the stack is full.


Example: 
    
ThreeStacks(5)  // create 3 stacks with size 5 in single array. stack index from 0 to 2

push(0, 10) // push 10 in the 1st stack.

push(0, 11)

push(1, 20) // push 20 in the 2nd stack.

push(1, 21)

pop(0) // return 11

pop(1) // return 21

peek(1) // return 20

push(2, 30)  // push 30 in the 3rd stack.

pop(2) // return 30

isEmpty(2) // return true

isEmpty(0) // return false



```python
class ThreeStacks:
    """
    @param: size: An integer
    """
    def __init__(self, size):
        self.size = size 
        self.stack = [[] for _ in range(3)]

    """
    @param: stackNum: An integer
    @param: value: An integer
    @return: nothing
    """
    def push(self, stackNum, value):
        self.stack[stackNum].append(value)
        if len(self.stack[stackNum]) > self.size:
            self.stack[stackNum].pop()

    """
    @param: stackNum: An integer
    @return: the top element
    """
    def pop(self, stackNum):
        return self.stack[stackNum].pop()


    """
    @param: stackNum: An integer
    @return: the top element
    """
    def peek(self, stackNum):
        if self.isEmpty(stackNum):
            return
        return self.stack[stackNum][-1]
        

    """
    @param: stackNum: An integer
    @return: true if the stack is empty else false
    """
    def isEmpty(self, stackNum):
        return len(self.stack[stackNum]) == 0

```

##### Implement queue by linkedlist 


```python
# define a queuenode class 
class QueueNode:
    # initialization 
    def __init__(self, value):
        self.val = value 
        self.next = None 
    
    
class Queue:
    def __init__(self):
        # a dummy head node 
        self.dummy = QueueNode(-1)
        # a tail node is the same as the dummy node at the initialization 
        self.tail = self.dummy
        
    # add an element with push()
    def push(self, val):
        # create a new queue node 
        new_node = QueueNode(val)
        # if the first element 
        if self.dummy.next is None:
            self.dummy.next = new_node 
        print('dummy',self.dummy.next.val)
        print('newnode', new_node)
        self.tail.next = new_node 
        self.tail = new_node 
        print('tail', self.tail.val)
        
    def pop(self):
        pop_val = self.dummy.next.val
        print('popval', pop_val)
        self.dummy.next = self.dummy.next.next 
        if self.dummy.next is None:
            self.tail = self.dummy
        return pop_val 
    
    def top(self):
        return self.dummy.next.val
    
    def isEmpty(self):
        return self.dummy.next is None 
```


```python
a = Queue()
a.push(1)
a.push(2)
a.push(5)
a.pop()
a.isEmpty()
```

    dummy 1
    newnode <__main__.QueueNode object at 0x7fa8c0514898>
    tail 1
    dummy 1
    newnode <__main__.QueueNode object at 0x7fa8c0514438>
    tail 2
    dummy 1
    newnode <__main__.QueueNode object at 0x7fa8c050ff60>
    tail 5
    popval 1





    False



40 - Implement Queue by Two Stacks

As the title described, you should only use two stacks to implement a queue's actions.

The queue should support `push`(element), `pop()` and `top()` where `pop` is pop the first(a.k.a front) element in the queue.

Both `pop` and `top` methods should return the value of first element.


* Stack, queue 


```python
# 1. stack1 is used to append pushed element 
# 2. stack2 is used to pop out element 
class MyQueue:
    
    def __init__(self):
        # push elements to stack1, pop from stack 2 
        self.stack1 = []
        self.stack2 = []

    """
    @param: element: An integer
    @return: nothing
    """
    def push(self, element):
        self.stack1.append(element)

    """
    @return: An integer
    """
    def pop(self):
        while len(self.stack2) == 0:
            self.move_elements()
        return self.stack2.pop()

    """
    @return: An integer
    """
    def top(self):
        while len(self.stack2) == 0:
            self.move_elements()
        return self.stack2[-1]
    
    def move_elements(self):
        while len(self.stack1) > 0:
            self.stack2.append(self.stack1.pop())

```


```python
a = MyQueue()
a.push(1)
a.push(2)
a.stack1
a.pop()

```




    1




```python
# or move all current elements in stack 1 to stack 2 when stack2 is empty 
class MyQueue:
    
    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    """
    @param: element: An integer
    @return: nothing
    """
    def push(self, element):
        self.stack1.append(element)


    """
    @return: An integer
    """
    def pop(self):
        if len(self.stack2) == 0:
            self.move_elements()
        return self.stack2.pop()
    """
    @return: An integer
    """
    def top(self):
        if len(self.stack2) == 0:
            return self.stack1[0]
        else: 
            return self.stack2[-1]
        
    def move_elements(self):
        while len(self.stack1) > 0:
            self.stack2.append(self.stack1.pop())

```

955 - Implement Queue by Circular Array
 
Implement queue by circulant array. You need to support the following methods:

`CircularQueue(n)`: initialize a circular array with size `n` to store elements

`boolean isFull()`: return true if the array is full

`boolean isEmpty()`: return true if there is no element in the array

`void enqueue(element)`: add an element to the queue

`int dequeue()`: pop an element from the queue

it's guaranteed in the test cases we won't call enqueue if it's full and we also won't call dequeue if it's empty. So it's ok to raise exception in scenarios described above.


```python
class CircularQueue:
    def __init__(self, n):
        # initialize array 
        self.circular_array = [0] * n
        # pointers to queue head and tail 
        self.head = 0
        self.tail = 0 
        
        # record the number of elements 
        self.size = 0
    """
    @return:  return true if the array is full
    """
    def isFull(self):
        return self.size == len(self.circular_array)

    """
    @return: return true if there is no element in the array
    """
    def isEmpty(self):
        return self.size == 0 

    """
    @param element: the element given to be added
    @return: nothing
    """
    def enqueue(self, element):
        if self.isFull():
            raise RuntimeError('Queue is full.')
        # find tail location 
        self.tail = (self.head + self.size) % len(self.circular_array)
        self.circular_array[self.tail] = element 
        self.size += 1 

    """
    @return: pop an element from the queue
    """
    def dequeue(self):
        if self.isEmpty():
            raise RuntimeError('Queue is empty')
        # record the element 
        pop_val = self.circular_array[self.head]
        # update head 
        self.head = (self.head + 1) % len(self.circular_array)
        self.size -= 1 
        return pop_val 

```

128 - Hash Function


In data structure Hash, hash function is used to convert a string (or any other type) into an integer smaller than hash size and bigger or equal to zero. The objective of designing a hash function is to `"hash" the key as unreasonable as possible`. A good hash function can avoid collision as less as possible. A widely used hash function algorithm is using a magic number `33`, consider any string as a 33 based big integer like follow:
 
             $hashcode("abcd") = (ascii(a) * 33^3 + ascii(b) * 33^2 + ascii(c) *33 + ascii(d)) % HASH_SIZE$

                             $ = (97* 333 + 98 * 332 + 99 * 33 +100) % HASH_SIZE$

                             $ = 3595978 % HASH_SIZE$

here `HASH_SIZE` is the capacity of the hash table (you can assume a hash table is like an array with index 0 ~ HASH_SIZE-1).

Given a string as a key and the size of hash table, return the hash value of this key.f

* hash 


Properties of modular: https://www.quora.com/What-are-all-the-properties-of-modulo-which-can-be-used-in-programming-competitions


```python
class Solution:
    """
    @param key: A string you should hash
    @param HASH_SIZE: An integer
    @return: An integer
    """
    def hashCode(self, key, HASH_SIZE):
        result = 0 
        for i in range(len(key)):
            result = (result * 33 % HASH_SIZE + ord(key[i])) % HASH_SIZE
        return result

```


```python
Solution().hashCode('abcd', 1000)
```




    978




```python
help(ord)
```

    Help on built-in function ord in module builtins:
    
    ord(c, /)
        Return the Unicode code point for a one-character string.
    


129 -  Rehashing

The size of the hash table is not determinate at the very beginning. If the total size of keys is too large (e.g. size >= capacity / 10), we should double the size of the hash table and rehash every key. 

* Hash function


```python
# Python 1: add each new conflicted node at the beginning 
"""
Definition of ListNode
class ListNode(object):

    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""
class Solution:
    """
    @param hashTable: A list of The first node of linked list
    @return: A list of The first node of linked list which have twice size
    """
    def rehashing(self, hashTable):
        size = 2 * len(hashTable)
        
        # initialize new hashtable 
        new_hash = [None] * size 
        
        # loop through the old hashTable to re-arrange the key, value pairs 
        for node in hashTable:
            # loop through (possible) linkedlist 
            while node is not None:
                self.add_node(node.val, new_hash)
                node = node.next 
        return new_hash
    
    def add_node(self, value, hash_table):
        loc = value % len(hash_table)
        if hash_table[loc] is None: 
            hash_table[loc] = self.new_list_node(value)
        else: 
            cur_node = hash_table[loc]
            new_node = self.new_list_node(value)
            new_node.next = cur_node 
            hash_table[loc] = new_node 
            
    def new_list_node(self, value):
        return ListNode(value)
```


```python
# Python 2: add each new conflicted node at the end of the linked list  
# Definition of ListNode
"""
Definition of ListNode
class ListNode(object):

    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""
class Solution:
    """
    @param hashTable: A list of The first node of linked list
    @return: A list of The first node of linked list which have twice size
    """
    def rehashing(self, hashTable):
        # new size 
        new_size = 2 * len(hashTable)
        # new hash table 
        new_hash = [None for _ in range(new_size)]
        
        # check each node 
        for node in hashTable:
            while node is not None:
                self.add_node(node.val, new_hash)
                node = node.next 
        return new_hash
        
    def add_node(self, value, hash_table):
        # hashcode 
        index = value % len(hash_table)
        
        # if no values have been added at index 
        if hash_table[index] is None:
            new_node = ListNode(value)
            hash_table[index] = new_node
        else:
            new_node = ListNode(value)
            prev_node = hash_table[index]
            # find the tail of linked list 
            while prev_node.next is not None:
                prev_node = prev_node.next
            # append new node at tail 
            prev_node.next = new_node
            
            
```

130 - Heapify

Given an integer array, heapify it into a min-heap array.

For a heap array `A`, `A[0]` is the root of heap, and for each `A[i]`, `A[i * 2 + 1]` is the left child of `A[i]` and `A[i * 2 + 2]` is the right child of `A[i]`.

Example: 

Given [3,2,1,4,5], return [1,2,3,4,5] or any legal heap array.

* Heap



```python
# Python 1: O(n log n), siftup
# import sys 
# import collections 

class Solution:
    """
    @param: A: Given an integer array
    @return: nothing
    """
    def heapify(self, A):
        # do siftup for each element 
        for i in range(len(A)):
            self.siftup(A, i)
        return A 
    
    def siftup(self, A, index):
        # comparasion starts from index = 1 as parent is always before index (when index = 1, parent is at 0)
        while index != 0: 
            parent = (index - 1) // 2
            if A[parent] > A[index]:
                A[index], A[parent] = A[parent], A[index]
                # loop until index = 0
                index = parent
                print('index', index)
            else:
                break
                
            
        

```


```python
Solution().heapify([3, 2, 1, 4, 1])
```

    index 0
    index 0
    index 1





    [1, 1, 2, 4, 3]




```python
# Python 2: O(n), siftdown 
class Solution:
    """
    @param: A: Given an integer array
    @return: nothing
    """
    def heapify(self, A):
        # do siftup for each element 
        for i in range(len(A) - 1, -1, -1):
            self.siftdown(A, i)
        return A 
    
    def siftdown(self, A, index):
        
        # when A[index] has at least one child (is a parent node)
        while (index * 2 + 1) < len(A):
            left_child = index * 2 + 1 
            # if there is also a right child 
            if left_child + 1 < len(A) and A[left_child] > A[left_child + 1]:
                min_child = left_child + 1
            else: 
                min_child = left_child 
            if A[min_child] >= A[index]:
                break
            A[index], A[min_child] = A[min_child], A[index]
            
            # update index
            index = min_child
            
  
```

642 - Moving Average from Data Stream
 
Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.


Example: 

MovingAverage m = new MovingAverage(3);

m.next(1) = 1 // return 1.00000

m.next(10) = (1 + 10) / 2 // return 5.50000

m.next(3) = (1 + 10 + 3) / 3 // return 4.66667

m.next(5) = (10 + 3 + 5) / 3 // return 6.00000


* Data stream, queue 




```python
from collections import deque 
class MovingAverage:
    """
    @param: size: An integer
    """
    def __init__(self, size):
        # do intialization
        self.queue = deque([])
        # the window size 
        self.size = size 
        
        # sum
        self.sum = 0
        

    """
    @param: val: An integer
    @return:  
    """
    def next(self, val):
        if len(self.queue) < self.size:
            self.sum += val
        else: 
            pop_val = self.queue.popleft()
            self.sum -= pop_val
            self.sum += val
        # append val in both cases 
        self.queue.append(val)
        return self.sum / len(self.queue)
        
```

209 - First Unique Character in a String


Find the first unique character in a given string. You can assume that there is at least one unique character in the string.


Example 1:
    
	Input: "abaccdeff"
        
	Output:  'b'
	
	Explanation:
        
	There is only one 'b' and it is the first one.



```python
# Python: use queue to save the characters and loop to check if the new character is the same as the 
# first element in a queue 


class Solution:
    """
    @param str: str: the given string
    @return: char: the first unique character in a given string
    """
    def firstUniqChar(self, string):
        if len(string) == 0 or string is None:
            return '' 
        hash_count = {}
        for char in string:
            if char in hash_count:
                hash_count[char] += 1 
            else:
                hash_count[char] = 1
#         print(hash_count)
        # iterate from the first char to make sure it's the FIRST unique 
        for key, val in hash_count.items():
            if hash_count[key] == 1:
                return key
        # if no char appears only once 
        return ''
            

```


```python
Solution().firstUniqChar('aab')
```

    {'a': 2, 'b': 1}





    'b'




```python
hash_count = {x:0 for x in 'abc'}
hash_count['a']
'a' in hash_count
```




    True



685 - First Unique Number in Data Stream

To give a continuous stream of data, write a function that returns the first unique number (including the terminating number) when the terminating number arrives. If the terminating number is not found, return -1.

Example: 

Input: 
[1, 2, 2, 1, 3, 4, 4, 5, 6]

5

Output: 3


```python
# Use a linked list to record current unique nums in sequence (easier to remove from any position of the linkedlist)
# Use a hash table to record each num's prev node to easier deletion 
class ListNode(object):

    def __init__(self, val, next=None):
        self.val = val
        self.next = next

class DataStream(object):
    def __init__(self):
        self.head = ListNode(-1)
        self.tail = self.head 
        # hash table to save num:prev_node pairs 
        self.hash_table = {}
        # set to save duplicates numbers to be used to tell if a future num is unique or not 
        self.duplicates = set()
    
    # remove num from linked list and hash table 
    def remove(self, num):
        # prev node of node with value as num 
        prev = self.hash_table[num]
        # delete node with value as num 
        prev.next = prev.next.next 
        # delete num from hash table 
        del self.hash_table[num]
        
        # update hash table: assign prev node to key = prev.next.val if prev.next is not None 
        if prev.next is not None:
            self.hash_table[prev.next.val] = prev 
        else:
            self.tail = prev 
            
    # add a num to hash table and linked list         
    def add(self, num):
        # tell if the num appears more than twice (has been deleted from hash table and added to duplicates set) 
        if num in self.duplicates:
            return 
         
        # if num has appeared once 
        if num in self.hash_table:
            self.remove(num)
            self.duplicates.add(num)
        else:
            # if num appears for the first time 
            node = ListNode(num)
            self.hash_table[num] = self.tail
            self.tail.next = node 
            self.tail = node 
        # print(self.hash_table)
        # print(self.duplicates)
        
    # first unique is the current head.next value     
    def first_unique(self):
        if self.head.next is not None:
            return self.head.next.val 
        return -1 

class Solution:
    """
    @param nums: a continuous stream of numbers
    @param number: a number
    @return: returns the first unique number
    """
    def firstUniqueNumber(self, nums, number):
        ds = DataStream()
        for i in range(len(nums)):
            ds.add(nums[i])
            if nums[i] == number:
                return ds.first_unique()
        return -1 

```


```python

```

657 - Insert Delete GetRandom O(1)

Design a data structure that supports all following operations in average O(1) time.

`insert(val)`: Inserts an item val to the set if not already present.

`remove(val)`: Removes an item val from the set if present.

`getRandom`: Returns a random element from current set of elements. Each element must have the same probability of being returned.
 

Example:
    
// Init an empty set.

RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.

randomSet.insert(1);

// Returns false as 2 does not exist in the set.

randomSet.remove(2);


* Hash table



```python
import random

class RandomizedSet:
    
    def __init__(self):
        # array to save values 
        self.res = []
        # dictionary to save index and values 
        self.res_dict = {}

    """
    @param: val: a value to the set
    @return: true if the set did not already contain the specified element or false
    """
    def insert(self, val):
        if val not in self.res_dict:
            self.res.append(val)
            self.res_dict[val] = len(self.res) - 1
            return True 
        return False

    """
    @param: val: a value from the set
    @return: true if the set contained the specified element or false
    """
    def remove(self, val):
        if val in self.res_dict:
            # find its index 
            index = self.res_dict[val]
            # overwrite res[index] with the last element
            self.res[index] = self.res[-1]
            
            # update the key val pair for dict, now res[index] is the original last element of res 
            self.res_dict[self.res[index]] = index
            # pop from res 
            self.res.pop()
            # delete from res_dict 
            del self.res_dict[val]
            return True 
        return False 
            
        

    """
    @return: Get a random element from the set
    """
    def getRandom(self):
        return self.res[random.randint(0, len(self.res) - 1)]



```

526 - Load Balancer

Implement a load balancer for web servers. It provide the following functionality:

Add a new server to the cluster => `add(server_id)`.

Remove a bad server from the cluster => `remove(server_id)`.

Pick a server in the cluster randomly with equal probability => `pick()`.

At beginning, the cluster is empty. When `pick()` is called you need to randomly return a server_id in the cluster.


Example: 

Input:

  add(1)
  
  add(2)
  
  add(3)
  
  pick()
  
  pick()
  
  pick()
  
  pick()
  
  remove(1)
  
  pick()
  
  pick()
  
  pick()
  
Output:

  1
  2
  1
  3
  2
  3
  3
  
Explanation: The return value of pick() is random, it can be either 2 3 3 1 3 2 2 or other.


```python
import random 
class LoadBalancer:
    def __init__(self):
        self.hash = {}
        self.servers = []
        
        
    """
    @param: server_id: add a new server to the cluster
    @return: nothing
    """
    def add(self, server_id):
        if server_id not in self.hash:
            self.servers.append(server_id)
            self.hash[server_id] = len(self.servers) - 1 



    """
    @param: server_id: server_id remove a bad server from the cluster
    @return: nothing
    """
    def remove(self, server_id):
        print('before',self.hash)
        print('before',self.servers)
        if server_id in self.hash:
            index = self.hash[server_id]
            self.hash[self.servers[-1]] = index 
            self.servers[index] = self.servers[-1]
            del self.hash[server_id]

            
            self.servers.pop()
        print('after', self.hash)
        print('after', self.servers)

    """
    @return: pick a server in the cluster randomly with equal probability
    """
    def pick(self):
        return self.servers[random.randint(0, len(self.servers) - 1)]
        
```


```python
p = LoadBalancer()
p.add(23)
p.add(56)
p.remove(56)
p.add(10)
p.remove(23)
p.remove(10)
p.add(600)
p.add(700)
p.remove(700)
```

    before {23: 0, 56: 1}
    before [23, 56]
    after {23: 0}
    after [23]
    before {23: 0, 10: 1}
    before [23, 10]
    after {10: 0}
    after [10]
    before {10: 0}
    before [10]
    after {}
    after []
    before {600: 0, 700: 1}
    before [600, 700]
    after {600: 0}
    after [600]


612 - K Closest Points

Given some points and an origin point in two-dimensional space, find k points which are nearest to the origin.
Return these points sorted by distance, if they are same in distance, sorted by the x-axis, and if they are same in the x-axis, sorted by y-axis.

Input:

points = [[4,6],[4,7],[4,4],[2,5],[1,1]], origin = [0, 0], k = 3 

Output: [[1,1],[2,5],[4,4]]

* Heap


```python
# Python: use dictionary/hash and sort, use count as key to include duplicated (x, y) pairs

"""
Definition for a point.
class Point:
    def __init__(self, a=0, b=0):
        self.x = a
        self.y = b
"""

class Solution:
    """
    @param points: a list of points
    @param origin: a point
    @param k: An integer
    @return: the k closest points
    """
    def kClosest(self, points, origin, k):
        if points is None or len(points) == 0:
            return [[]]
        distance_dict = {}
        count = 0
        for point in points:
#             print(point)
            # math.sqrt
            distance_dict[count] = [(point[0] - origin[0])**2 + (point[1] - origin[1])**2, point[0], point[1]]
            count += 1 
        distance_dict = {k:v for k, v in sorted(distance_dict.items(), key = lambda x:(x[1]))}
        return [list(x)[1:3] for _, x in distance_dict.items()][0:k]
        

```


```python

Solution().kClosest([[0,9],[138,429],[115,359],[115,359],[-30,-102],[230,709],[-150,-686],[-135,-613],[-60,-248],[-161,-481],[207,639],[23,79],[-230,-691],[-115,-341],[92,289],[60,336],[-105,-467],[135,701],[-90,-394],[-184,-551],[150,774]], [100, 71], 10)
```




    [[23, 79],
     [0, 9],
     [-30, -102],
     [92, 289],
     [60, 336],
     [115, 359],
     [115, 359],
     [-60, -248],
     [138, 429],
     [-115, -341]]




```python
# Python: priority queue, use minHeap 
import heapq

"""
Definition for a point.
class Point:
    def __init__(self, a=0, b=0):
        self.x = a
        self.y = b
"""

class Solution:
    """
    @param points: a list of points
    @param origin: a point
    @param k: An integer
    @return: the k closest points
    """
    def kClosest(self, points, origin, k):
        self.heap = []
        # compute distance 
        for point in points:
            dist = (point.x - origin.x)**2 + (point.y - origin.y)**2
            # use (-dist, -point.x, -point.y) tuple as heap item, so the smallest / farest point 
            # could be poped out first 
            heapq.heappush(self.heap, (-dist, -point.x, -point.y))
            
            if len(self.heap) > k:
                heapq.heappop(self.heap)
        # retrieve results from heap
        res = []
        while len(self.heap) > 0:
            _, x, y = heapq.heappop(self.heap)
            res.append(Point(-x, -y))
        # re-order the points in ascending order 
        res.reverse()
        return res

```


```python
import heapq
help(heapq.heappush)
```

    Help on built-in function heappush in module _heapq:
    
    heappush(...)
        heappush(heap, item) -> None. Push item onto heap, maintaining the heap invariant.
    


544 - Top k Largest Numbers

Given an integer array, find the top `k` largest numbers in it.

Example: 

Input: [3, 10, 1000, -99, 4, 100] and k = 3

Output: [1000, 100, 10]

* Priority queue, heap



```python
# Python: use minHeap
# can also first sort the array and then output the top k 
import heapq
class Solution:
    """
    @param nums: an integer array
    @param k: An integer
    @return: the top k largest numbers in array
    """
    def topk(self, nums, k):
        if len(nums) == 0:
            return []
        heap = []
        for i in range(len(nums)):
            heapq.heappush(heap, nums[i])
            if len(heap) > k:
                heapq.heappop(heap)
        res = []
        while len(heap) > 0:
            res.append(heapq.heappop(heap))
           # print(res)
        res.reverse()
        return res
    
```


```python
Solution().topk([1, 100, 90, 20], 3)
```




    [100, 90, 20]




```python
# Python: use built-in functions 
import heapq

class Solution:
    '''
    @param {int[]} nums an integer array
    @param {int} k an integer
    @return {int[]} the top k largest numbers in array
    '''
    def topk(self, nums, k):
        # heapify
        heapq.heapify(nums)
        # nlargest 
        topk = heapq.nlargest(k, nums)
        topk.sort()
        topk.reverse()
        return topk
```

606 - Kth Largest Element II

Find K-th largest element in an array. and N is much larger than k.

Example: 

Input:[9,3,2,4,8], 3

Output:4


```python
from heapq import heappush, heapop 
class Solution:
    """
    @param nums: an integer unsorted array
    @param k: an integer from 1 to n
    @return: the kth largest element
    """
    def kthLargestElement2(self, nums, k):
        if not nums or not k:
            return -1 
        heap = []
        for i in range(len(nums)):
            heappush(heap, nums[i])
            if len(heap) > k:
                heappop(heap)
        return heappop(heap)

```

545 - Top k Largest Numbers II

Implement a data structure, provide two interfaces:

`add(number)` Add a new number in the data structure.

`topk()` Return the top k largest numbers in this data structure. k is given when we create the data structure.


```python
from heapq import heappush, heappop 
class Solution:
    """
    @param: k: An integer
    """
    def __init__(self, k):
        self.heap = []
        self.size = k 
        

    """
    @param: num: Number to be added
    @return: nothing
    """
    def add(self, num):
        heappush(self.heap, num)
        if len(self.heap) > self.size:
            heappop(self.heap)

    """
    @return: Top k element
    """
    def topk(self):
        return sorted(self.heap, reverse = True)

```

613 - High Five

There are two properties in the node student id and scores, to ensure that each student will have at least 5 points, find the average of 5 highest scores for each person.

Example: 

Input: 
[[1,91],[1,92],[2,93],[2,99],[2,98],[2,97],[1,60],[1,58],[2,100],[1,61]]

Output:

1: 72.40

2: 97.40




```python
'''
Definition for a Record
class Record:
    def __init__(self, id, score):
        self.id = id
        self.score = score
'''
from heapq import heappush, heappop 

class Solution:
    # @param {Record[]} results a list of <student_id, score>
    # @return {dict(id, average)} find the average of 5 highest scores for each person
    # <key, value> (student_id, average_score)
    def highFive(self, results):
        ids = set()
        for record in results:
            id_ = record.id 
            ids.add(id_)
        average = {id_ : 0 for id_ in ids}
        ids = list(ids)
        for i in range(len(ids)):
            heap = []
            for j in range(len(results)):
                if results[j].id != ids[i]:
                    continue 
                heappush(heap, results[j].score)
                if len(heap) > 5:
                    heappop(heap)
            average[ids[i]] = sum(heap) / len(heap)
        return average 
        

        
```

4 - Ugly Number II

Ugly number is a number that only have factors 2, 3 and 5.

Design an algorithm to find the `n-th` ugly number. The first 10 ugly numbers are 1, 2, 3, 4, 5, 6, 8, 9, 10, 12...

* Heap, priority queue 


```python
# Python: use the properties of minHeap: always pop the min value first 
from heapq import heappush, heappop
class Solution:
    """
    @param n: An integer
    @return: return a  integer as description.
    """
    def nthUglyNumber(self, n):
        heap = [1]
        visited = set([1])
        for _ in range(n):
            cur_min = heappop(heap)
            for factor in [2, 3, 5]:
                new_val = cur_min * factor
                if new_val not in visited:
                    visited.add(new_val)
                    heappush(heap, new_val)
        return cur_min
        

```


```python
Solution().nthUglyNumber(10)
```




    12



104 -  Merge K Sorted Lists

Merge k sorted linked lists and return it as one sorted list.

Analyze and describe its complexity.


Example 1:

	Input:   [2->4->null,null,-1->null]
    
	Output:  -1->2->4->null
    
* Priority queue, heap, divide and conquer, linked list 


```python
# Python 1: use heap 

from heapq import heappop, heappush

"""
Definition of ListNode
class ListNode(object):

    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""
class Solution:
    """
    @param lists: a list of ListNode
    @return: The head of one sorted list.
    """
    def mergeKLists(self, lists):
        if not lists:
            return None 
        # initialize head and tail of the new ListNode
        head = tail = ListNode(-1)
        heap = []
        # count is used to record an index to be used to differentiate nodes with the same values 
        # so when making comparisons, it's comparing the first two elements in the tuple pushed to heap:
        # node.val, then index, NOT the third element, ListNode 
        count = 0
        # first add (at most) k elements from each ListNode to the heap 
        for node in lists:
            # if the list is not null 
            if node:
                heappush(heap, (node.val, count, node))
                count += 1 

        # start iterating 
        while heap: 
            # pop a node with min val 
            cur_node = heappop(heap)[2]
            # add to new ListNode 
            tail.next = cur_node
            tail = tail.next
            # add the next node following the popped node to heap 
            if tail.next:
                print(tail.next.val)
                heappush(heap, (tail.next.val, count, tail.next))
                count += 1
        return head.next
    

```

134 - LRU Cache


Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: `get` and `set`.

`get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.

`set(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.


Example: 

Input:

LRUCache(2)

set(2, 1)

set(1, 1)

get(2)

set(4, 1)

get(1)

get(2)

Output:

[1,-1,1]


* Linked list 



```python
# Python 1: use single linked list and a hash table (to save the prev node as value of each node as key)


# define the linkednode class 
class LinkedNode:
    
    def __init__(self, key=None, value=None, next=None):
        self.key = key
        self.value = value
        self.next = next

class LRUCache:

    # @param capacity, an integer
    def __init__(self, capacity):
        # hash table to save the current node key and its prev node 
        self.hash = {}
        # head and tail of linked list 
        self.head = LinkedNode()  # dummy node 
        self.tail = self.head
        self.capacity = capacity
        
    # @param key, an integer
    # @param value, an integer
    # @return nothing
    def set(self, key, value):
        # if the key exists, overwrite it and move it to the tail 
        if key in self.hash:
            self.kick(self.hash[key])
            # update value of the key  
            # hash[key].next means the next of key's prev, which is key 
            self.hash[key].next.value = value
        else:
            # if key doesn't exist in hash, append a node to tail and update tail 
            # first check if the hash reaches capacity 
            self.append(LinkedNode(key, value))
            if len(self.hash)  > self.capacity:
                self.pop_head()
            print('hash', self.hash)


            
        
    # @return an integer
    def get(self, key):
        if key not in self.hash:
            print('get', key, 'return',-1)
            return -1
        val = self.hash[key].next.value
        self.kick(self.hash[key])
        print('get', key, 'return',val)
        return val

    
    # remove node: change "prev->node->next...->tail" to "prev->next->...->tail -> node 
    def kick(self, prev):
        node = prev.next
        print('kick', node)
#         print('tail', self.tail)
        if node == self.tail:
            print('node = tail')
            return
         # update linked list 
        prev.next = node.next
        # update hash table 
        if node.next is not None:
            self.hash[node.next.key] = prev
            node.next = None
        # append needs to be done right after the udpate so hash[key].next is node 
        self.append(node)

    # update hash table and append node to tail 
    def append(self, node):
        self.hash[node.key] = self.tail
        self.tail.next = node
        self.tail = node
    
    # pop the head of the linked list 
    # dummy -> 1 -> 2 -> 3 ... to dummy -> 2 -> 3 ... 
    def pop_head(self):
        # delete the current head.next.key ~ head.key pair  (1 ~ dummy pair)
        del self.hash[self.head.next.key]
        # update the head.next from 1 to 2 
        self.head.next = self.head.next.next 
        # update the new head.next ~ head pair (2 ~ dummy)
        # current head.next.key = 2, head.key = None, head.val = None, head.next.key = 2
        self.hash[self.head.next.key] = self.head
        


        
```

601 - Flatten 2D Vector

Implement an iterator to flatten a 2d vector.




```python
from collections import deque  
class Vector2D(object):

    # @param vec2d {List[List[int]]}
    def __init__(self, vec2d):
        self.queue = deque([])
        i = 0
        while i < len(vec2d):
            j = 0 
            while j < len(vec2d[i]):
                self.queue.append(vec2d[i][j])
                j += 1 
            i += 1 
        



    # @return {int} a next element
    def next(self):
        return self.queue.popleft()
        



    # @return {boolean} true if it has next element
    # or false
    def hasNext(self):
        return len(self.queue) > 0


# Your Vector2D object will be instantiated and called as such:
# i, v = Vector2D(vec2d), []
# while i.hasNext(): v.append(i.next())
```

124 - Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Example: 

Given [100, 4, 200, 1, 3, 2], the longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.


```python
# First sort, then iterate with one for loop, O(nlongn)
class Solution:
    """
    @param num: A list of integers
    @return: An integer
    """
    def longestConsecutive(self, num):
        num.sort()
        lower = num[0]
        max_len = 1 
        cur_len = 1 
        for n in num:
            if n - lower == 0:
                continue 
            elif n - lower == 1:
                cur_len += 1 
            else:
                # n - lower > 1, not consecutive any more 
                # reset cur_len 
                cur_len = 1 
            # update max_len 
            if cur_len > max_len:
                max_len = cur_len 
            lower = n 
        return max_len 

```


```python
# first use a hash table to count all elements in num, then iterate to two directions from each element
# as we delete each iterated element, the time complexity is O(n)
class Solution:
    """
    @param num: A list of integers
    @return: An integer
    """
    def longestConsecutive(self, num):
        hash_table = {}
        for n in num:
            hash_table.setdefault(n, 0)
            hash_table[n] += 1 
        
        for n in num:
            if key in hash_table:
                cur_len = 1 
                del hash_table[key]
                lower, upper = key - 1, key + 1 
                while lower in hash_table:
                    del hash_table[lower]
                    lower -= 1 
                    cur_len += 1 
                while upper in hash_table:
                    del hash_table[upper]
                    upper += 1 
                    cur_len += 1 
                if max_len < cur_len:
                    max_len = cur_len 
        return max_len 
                
```

551 -  Nested List Weight Sum

Given a nested list of integers, return the sum of all integers in the list weighted by their depth. Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Example: 

Input: the list [1,[4,[6]]], 

Output: 27. 

Explanation:
one 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4 * 2 + 6 * 3 = 27

* dfs



```python
"""

class NestedInteger(object):
    def isInteger(self):
        # @return {boolean} True if this NestedInteger holds a single integer,
        # rather than a nested list.

    def getInteger(self):
        # @return {int} the single integer that this NestedInteger holds,
        # if it holds a single integer
        # Return None if this NestedInteger holds a nested list

    def getList(self):
        # @return {NestedInteger[]} the nested list that this NestedInteger holds if it holds a nested list
        # Return None if this NestedInteger holds a single integer
"""


class Solution(object):
    # @param {NestedInteger[]} nestedList a list of NestedInteger Object
    # @return {int} an integer
    def depthSum(self, nestedList):
        if len(nestedList) == 0:
            return 0 
        stack = []
        sums = 0 
        for lists in nestedList:
            stack.append((lists, 1))
        
        while stack:
            lists, depth = stack.pop()
            if lists.isInteger():
                sums += depth * lists.getInteger()
            else:
                for i in lists.getList():
                    stack.append((i, depth + 1))
        return sums 
            
    
```

528 - Flatten Nested List Iterator

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Example: 

Input: list = [[1,1],2,[1,1]]

Output: [1,1,2,1,1]


```python
"""
class NestedInteger(object):
    def isInteger(self):
        # @return {boolean} True if this NestedInteger holds a single integer,
        # rather than a nested list.

    def getInteger(self):
        # @return {int} the single integer that this NestedInteger holds,
        # if it holds a single integer
        # Return None if this NestedInteger holds a nested list

    def getList(self):
        # @return {NestedInteger[]} the nested list that this NestedInteger holds,
        # if it holds a nested list
        # Return None if this NestedInteger holds a single integer
"""

class NestedIterator(object):

    def __init__(self, nestedList):
        stack = []
        self.flatten = []
        for lists in nestedList:
            stack.append(lists)

        while stack:
            last_list = stack.pop()
            if not last_list.isInteger():
                for element in last_list.getList():
                    stack.append(element)
            else:
                self.flatten.append(last_list.getInteger())
        print(self.flatten)

        
    # @return {int} the next element in the iteration
    def next(self):
        if self.hasNext():
            return self.flatten.pop()
        else:
            return None 
        
    # @return {boolean} true if the iteration has more element or false
    def hasNext(self):
        return len(self.flatten) > 0 


# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```

575 - Decode String

Given an expression `s` contains numbers, letters and brackets. Number represents the number of repetitions inside the brackets(can be a string or another expression)．Please expand expression to be a string.

Example:

Input: S = 3[2[ad]3[pf]]xyz

Output: "adadpfpfpfadadpfpfpfadadpfpfpfxyz"


```python
class Solution:
    """
    @param s: an expression includes numbers, letters and brackets
    @return: a string
    """
    def expressionExpand(self, s):
        if s is None:
            return None 
        if len(s) == 0:
            return ""
            
        stack = []
        
        for char in s:
            
            # find the first ] from left to right 
            if char != ']':
                stack.append(char)
                continue 
            
            # now char == ']', expand contents within []
            # extract chars in [] in reversed order 
            sub_str = []
            while stack and stack[-1] != '[':
                sub_str.append(stack.pop())
            
            # ignore '['
            stack.pop()
            
            # record the number of repeats of sub_str 
            rep = 0 
            base = 1  # rep can be more than 10 (with two digits)
            while stack and stack[-1].isdigit():
                rep += (int(stack.pop())) * base
                base *= 10 
            stack.append(''.join(reversed(sub_str)) * rep)
            
        return ''.join(stack)
                
```

540 - Zigzag Iterator

Given two 1-d vectors, implement an iterator to return their elements alternately.


Example: 

Input: v1 = [1, 2] and v2 = [3, 4, 5, 6]

Output: [1, 3, 2, 4, 5, 6]



```python
from collections import deque 
class ZigzagIterator:
    """
    @param: v1: A 1d vector
    @param: v2: A 1d vector
    """
    def __init__(self, v1, v2):
        n1, n2 = len(v1), len(v2)
        loc1, loc2 = 0, 0 
        self.results = deque()
        while loc1 < n1 and loc2 < n2:
            self.results.append(v1[loc1])
            self.results.append(v2[loc2])
            loc1 += 1 
            loc2 += 1 
        while loc1 < n1:
            self.results.append(v1[loc1])
            loc1 += 1 
        while loc2 < n2:
            self.results.append(v2[loc2])
            loc2 += 1 
        

    """
    @return: An integer
    """
    def next(self):
        if self.hasNext():
            return self.results.popleft()
        else:
            return None 

    """
    @return: True if has next
    """
    def hasNext(self):
        return len(self.results) > 0


# Your ZigzagIterator object will be instantiated and called as such:
# solution, result = ZigzagIterator(v1, v2), []
# while solution.hasNext(): result.append(solution.next())
# Output result
```

541 -  Zigzag Iterator II

Follow up Zigzag Iterator: What if you are given k 1d vectors? How well can your code be extended to such cases? The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic".

Example: 
    
Input: k = 3
    
vecs = [
    [1,2,3],
    
    [4,5,6,7],
    
    [8,9],
]

Output: [1,4,8,2,5,9,3,6,7]


```python
from collections import deque 
class ZigzagIterator2:
    """
    @param: vecs: a list of 1d vectors
    """
    def __init__(self, vecs):
        self.queue = deque([vec for vec in vecs if vec])
        

    """
    @return: An integer
    """
    def next(self):
        cur_vec = self.queue.popleft()
        value = cur_vec.pop(0)
        if cur_vec:
            self.queue.append(cur_vec)
        return value 

    """
    @return: True if has next
    """
    def hasNext(self):
        return len(self.queue) > 0 

# Your ZigzagIterator2 object will be instantiated and called as such:
# solution, result = ZigzagIterator2(vecs), []
# while solution.hasNext(): result.append(solution.next())
# Output result
```




```python

```
