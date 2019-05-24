
### Linked List

35 - Reverse Linked List

Example: 
    
Input: 1->2->3->null
    
Output: 3->2->1->null


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
    @param head: n
    @return: The new head of reversed linked list.
    """
    def reverse(self, head):
        if head is None:
            return head 
        cur, prev = head, None 
        while cur:
            # cur.next -> prev, cur -> cur.next, prev -> cur, the cur is the prev of next step 
            cur.next, cur, prev = prev, cur.next, cur 
        return prev 

```

450 - Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list `k` at a time and return its modified list.

If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.
Only constant memory is allowed.

Example: 

Input:

list = 1->2->3->4->5->null

k = 2

Output:

2->1->4->3->5


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
    @param head: a ListNode
    @param k: An integer
    @return: a ListNode
    """
    def reverseKGroup(self, head, k):
        if not head or k == 1:
            return head 
        dummy = ListNode(-1)
        dummy.next = head 
        prev = dummy 
        
        while prev.next:
            kth = prev 
            for _ in range(k):
                kth = kth.next 
                if kth is None:
                    return dummy.next 
            reverse_ = self.reverse(prev.next, kth)
            prev.next = reverse_[0]
            prev = reverse_[1]
        return dummy.next 
    
    def reverse(self, start, end):
        next_node = end.next 
        cur, prev = start, next_node 
        while cur != next_node:
            cur.next, cur, prev = prev, cur.next, cur
        return [prev, start]

        
  
```

451 -  Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

Example: 

Input: 

1->2->3->4->null


Output: 

2->1->4->3->null


```python
# Python 1: most intuitive way: record prev.next, prev.next.next, prev.next.next.next as a, b ,c 
#           a = b, b = a, a = c
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: a ListNode
    @return: a ListNode
    """
    def swapPairs(self, head):
        if head is None:
            return head 

        
        dummy = ListNode(-1)
        dummy.next = head
        prev = dummy
 
        while prev.next and prev.next.next:
            a = prev.next
            b = a.next
            c = b.next 
            
            prev.next = b 
            b.next = a 
            a.next = c 
            
            # update prev
            prev = a 
        return dummy.next

```


```python
# Python 2: more compact way 

"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    """
    @param head: a ListNode
    @return: a ListNode
    """
    def swapPairs(self, head):
        if head is None:
            return head 

        
        dummy = ListNode(-1)
        dummy.next = head
        prev = dummy
 
        while prev.next and prev.next.next:
            a = prev.next
            b = a.next
            # the first two items swap two elements, a will be the second second after swap, update its 
            # next attribute as b.attribute
            # Originally prev.next (a) = 1, prev.next.next (b) = 2, b.next = 3, now a, b swapped, 1.next = b.next = 3
            prev.next, prev.next.next, a.next = b, a, b.next 
            
            prev = a 
        return dummy.next

```

102 - Linked List Cycle

Check two pointers section for ways to solve it with two pointers. 

Given a linked list, determine if it has a cycle in it.




```python
# Python 1: use a set to record visited nodes 
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
        visited = set()
        node = head 
        while node:
            if node not in visited:
                visited.add(node)
                node = node.next 
            else:
                return True 
        return False 
            
        
```


```python
# Python 2: two pointers 
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

103 - Linked List Cycle II

Given a linked list, return the node where the cycle begins.

If there is no cycle, return null.


```python
# Python 1: use set, see Two pointers notebook for a more complicated method 
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
        if head is None:
            return None
        visited = set()
        node = head 
        while node:
            if node in visited:
                return node 
            else:
                visited.add(node)
                node = node.next 
        return None 

```
