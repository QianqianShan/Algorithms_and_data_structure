
## Binary Tree & Tree-based DFS

66 - Binary Tree Preorder Traversal

Given a binary tree, return the preorder traversal of its nodes' values.

The first data is the root node, followed by the value of the left and right son nodes, and "#" indicates that there is no child node.
The number of nodes does not exceed 20.


Example: 
    
    Input：{1,2,3}
    
Output：[1,2,3]

Explanation:
    
         1
        / \
       2   3


```python
# Python 1: recursion 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: A Tree
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        if root is None:
            return []
        
        return self.preorder_traverse(root)
    
    def preorder_traverse(self, root):
        if root is None:
            return []
        
        return [root.val] + self.preorder_traverse(root.left) + self.preorder_traverse(root.right)
```


```python
# Python 2: non-recursion 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: A Tree
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        if root is None:
            return []
            
        stack = [root]
        preorder = []
        
        while stack:
            cur_node = stack.pop()
            preorder.append(cur_node.val)
            if cur_node.right:
                stack.append(cur_node.right)
            if cur_node.left:
                stack.append(cur_node.left)
        return preorder 
        
```

67 - Binary Tree Inorder Traversal

Given a binary tree, return the inorder traversal of its nodes' values.

* DFS, binary tree, recursion, divide and conquer, in-order/inorder 


```python
# Python 1: divide and conquer 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        if root is None:
            return []
        return self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
```


```python
# Python 2: traversal 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        if root is None:
            return []
        # create a dummy node so the root node can be treated as the way as as other nodes 
        dummy_node = TreeNode(-1)
        dummy_node.right = root 
        #stack to save the nodes to be backtracked 
        stack = [dummy_node]  # stack top is always the current iterator 
        result = []
        while stack:
            cur_node = stack.pop()
            if cur_node.right:
                cur_node = cur_node.right 
                # go deep to the left most node 
                while cur_node:
                    stack.append(cur_node)
                    cur_node = cur_node.left
            if stack:
                # only collect the stack top value, not pop out 
                result.append(stack[-1].val)
        return result
        
```

68 - Binary Tree Postorder Traversal

Given a binary tree, return the postorder traversal of its nodes' values.


Example: 

Input：{1,2,3}

Output：[2,3,1]

Explanation:  

      1
     / \
    2   3



```python
# Python 1: recursion 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: A Tree
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        if root is None:
            return []
        return self.postorderTraversal(root.left) + self.postorderTraversal(root.right) + [root.val]

```


```python
# Python 2: non-recursion with two stacks 
# https://www.youtube.com/watch?v=qT65HltK2uE
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: A Tree
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        if root is None:
            return []
        stack1 = [root]
        stack2= []
        postorder = []

        while len(stack1) > 0:
            cur_node = stack1.pop()
            stack2.append(cur_node)
            if cur_node.left:
                stack1.append(cur_node.left)
            if cur_node.right:
                stack1.append(cur_node.right)
            
        while len(stack2) > 0:
            cur_node = stack2.pop()
            postorder.append(cur_node.val)
        return postorder
```

915 - Inorder Predecessor in BST


Given a binary search tree and a node in it, find the in-order predecessor of that node in the BST.

Example: 
    
Input: root = {2,1}, p = 2
    
Output: 1


```python
# Python: iteratively find the cloesest number to p.val with its value < p.val 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param p: the given node
    @return: the in-order predecessor of the given node in the BST
    """
    def inorderPredecessor(self, root, p):
        if root is None:
            return None 
        predec = None
        
        while root:
            if root.val >= p.val:
                root = root.left 
            else:
                # root.val < p.val 
                if predec is None or root.val > predec.val:
                    predec = root 
                root = root.right 
        return predec 
        
```

448 - Inorder Successor in BST

Given a binary search tree (See Definition) and a node in it, find the in-order successor of that node in the BST.

If the given node has no in-order successor in the tree, return null.

Example: 

Input: tree = {2,1,3}, node value = 1

Output: 2

Explanation: 

      2
     / \
    1   3


```python
# Python 1: iteration
"""
Definition for a binary tree node.
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
"""


class Solution:
    """
    @param: root: The root of the BST.
    @param: p: You need find the successor node of p.
    @return: Successor of p.
    """
    def inorderSuccessor(self, root, p):
        if root is None:
            return None 
        succ = None 
        while root:
            if root.val < p or root == p:
                root = root.right 
            else:
                if succ is None or succ.val > root.val:
                    succ = root 
                root = root.left 
        return succ 

```


```python
# Python 2: traverse 
class Solution:
    """
    @param: root: The root of the BST.
    @param: p: You need find the successor node of p.
    @return: Successor of p.
    """
    def inorderSuccessor(self, root, p):
        if root == None:
            return None
        if root.val <= p.val:
            return self.inorderSuccessor(root.right, p)
        
        left = self.inorderSuccessor(root.left, p)
        if left != None:
            return left
        else:
            return root
```

97 - Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

* Binary tree, recursion, divide and conquer 


```python
Input: tree = {1,2,3,#,#,4,5}
Output: 3
Explanation: Like this:
   1
  / \                
 2   3                
    / \                
   4   5
```


```python
# Python 1 : traverse, set depth as a class instance and return it at the end 

"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: An integer
    """
#     def __init___(self):
#         self.depth = 0

    def maxDepth(self, root):
        if (root is None):
            return 0
        self.depth = 0 
        self.traverse(root, 1)
        return self.depth
    
    def traverse(self, cur_node, cur_depth):
        if cur_node is None:
            return 
        
        self.depth = max(self.depth, cur_depth)
        self.traverse(cur_node.left, cur_depth + 1)
        self.traverse(cur_node.right, cur_depth + 1)
```


```python
# Python 2: divide and conquer, no global variable, has a return value (not record depth in global variable) 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: An integer
    """

    def maxDepth(self, root):
        if root is None:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1 
```

155 - Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.




```python
# Python 1: traverse 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: The root of binary tree
    @return: An integer
    """
    def minDepth(self, root):
        if root is None:
            return 0 
        self.min_depth = sys.maxsize
        self.get_height(root, 1)
        return self.min_depth 
    
    def get_height(self, root, cur_height):
        if root is None:
            return 
        if root.left is None and root.right is None:
            self.min_depth = min(self.min_depth, cur_height)
            return cur_height
        self.get_height(root.left, cur_height + 1)
        self.get_height(root.right, cur_height + 1)
                
        

```


```python
# Python 2: divide and conquer 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: The root of binary tree
    @return: An integer
    """
    def minDepth(self, root):
        if root is None:
            return 0 
        
        min_height, _ = self.get_height(root)
        return min_height
    
    def get_height(self, root):
        if root is None:
            return 0, 0 
        
        left_min, left_height = self.get_height(root.left)
        right_min, right_height = self.get_height(root.right)
        
        min_height = min(left_min + 1, right_min + 1)
        height = max(left_height, right_height) + 1 
        return min_height, height
```

93 - Balanced Binary Tree
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.


* Binary tree, divide and conquer, recursion



Input: tree = {3,9,20,#,#,15,7}
	Output: true
	
	Explanation:
	This is a balanced binary tree.
		  3  
		 / \                
		9  20                
		  /  \                
		 15   7 


```python
# Python: balanced ~ left subtree and right subtree are balanced, abs(left height - right height) <= 1 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: True if this Binary tree is Balanced, or false.
    """
    def isBalanced(self, root):
        # special case 
        if root is None:
            return True 
        # tell if tree with root is balanced 
        balanced_ind, _ = self.get_height(root)

        return balanced_ind
    
    def get_height(self, root):
        if root is None:
            return True, 0 
        left_balanced, left_height = self.get_height(root.left)
        if not left_balanced:
            return False, 0
        right_balanced, right_height = self.get_height(root.right)
        if not right_balanced:
            return False, 0
        
        balanced_ind = abs(left_height - right_height) <= 1
        height = max(left_height, right_height) + 1 
        return balanced_ind, height

```


```python
# Python 2: another divide and conquer 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: True if this Binary tree is Balanced, or false.
    """
    def isBalanced(self, root):
        # corner case 
        if root is None:
            return True 
        # tell if the left subtree is balanced 
        if not self.isBalanced(root.left):
            return False 
        
        # tell if the right subtree is balanced 
        if not self.isBalanced(root.right):
            return False
        # if both left and right subtrees are balanced, check if the tree with current root is balanced 
        return abs(self.get_height(root.left) - self.get_height(root.right)) <= 1 
    
    def get_height(self, root):
        if root is None:
            return 0 
        # the height of a tree = max height of each subtree + 1 (root)
        return max(self.get_height(root.left), self.get_height(root.right)) + 1 
```

95 - Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys less than the node's key.

* The right subtree of a node contains only nodes with keys greater than the node's key.

* Both the left and right subtrees must also be binary search trees.

* A single node tree is a BST


* Binary tree, Binary search tree, divide and conquer, recursion


```python
Example 1:
	Input:  For the following binary tree（only one node）:
	-1
	Output：true
	
Example 2:
	Input:  For the following binary tree:
	
	
	  2
	 / \
	1   4
	   / \
	  3   5
		
	Output: true
```


```python
# Python 1: in order traverse 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: True if the binary tree is BST, or false
    """
    def isValidBST(self, root):
        if root is None:
            return True 
        # two global variables to record the previous node value and if the tree is valid 
        self.is_valid = True 
        self.prev_node = None 
        self.check_valid(root)
        return self.is_valid 
        
    
    def check_valid(self, root):
        if root is None:
            return 
        
        self.check_valid(root.left)
        if self.prev_node is not None and self.prev_node >= root.val:
            self.is_valid = False 
            return
        self.prev_node = root.val 
        self.check_valid(root.right)

        
        


```


```python
# Python 2: divide and conquer 
# for each subtree, record its max and min val, 
# compare its max of left subtree with root
# compare its min of right subtree with root 
# Update the current tree's min and max 

"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of binary tree.
    @return: True if the binary tree is BST, or false
    """
    def isValidBST(self, root):
        if root is None:
            return True 
        is_valid, _, _ = self.check_valid(root)
        return is_valid 
        
    def check_valid(self, root):
        if root is None:
            return True, None, None 
        # check if left/right subtrees are balanced and record their corresponding
        # max, min values 
        is_valid_left, left_max, left_min = self.check_valid(root.left)
        is_valid_right, right_max, right_min = self.check_valid(root.right)
        
        # check if the two subtrees are balanced 
        if not is_valid_left or not is_valid_right:
            return False, None, None 
        # check the current tree 
        # if left subtree max >= root.val 
        if left_max is not None and left_max >= root.val:
            return False, None, None 
        # if right subtree min <= root.val 
        if right_min is not None and right_min <= root.val:
            return False, None, None 
        # if all conditions above are not satisfied, the current tree is balanced, update its values     
        min_val = left_min if left_min is not None else root.val 
        max_val = right_max if right_max is not None else root.val 
        return True, max_val, min_val  
        

        

```

900 -  Closest Binary Search Tree Value

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

* Binary tree, binary search tree 


```python
Example: 
    
Input: root = {5,4,9,2,#,8,10} and target = 6.124780
               
Output: 5
```


```python
# Python 1: iteration, find the lowerbound and upper bound of the target 
# lower bound: 
# start from the root
# 1. if target  < root.val, then the lower bound must be on the left subtree (if any)
# search the left subtree until finding a node value smaller than target 
# check the right subtree of the node to see if there is other values, x, such that node.val < x < target



# upper bound: 
# similar
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param target: the given target
    @return: the value in the BST that is closest to the target
    """
    def closestValue(self, root, target):
        # corner case 
        if not root or not target:
            return None 
        # get the two nodes which have values as lower and upper bounds 
        lower_node = self.get_lower_bound(root, target)
        upper_node = self.get_upper_bound(root, target)
        
        if lower_node is None:
            return upper_node.val 
        
        if upper_node is None:
            return lower_node.val
        
        return lower_node.val if (target - lower_node.val) < (upper_node.val - target) else upper_node.val

    
    
    def get_lower_bound(self, root, target):
        # find lower bound 
        # special case 
        if root is None: 
            return None
        
        if target < root.val:
            return self.get_lower_bound(root.left, target)
        
        # if target >= root.val 
        lower = self.get_lower_bound(root.right, target)
        return lower if lower is not None else root
    
    
    def get_upper_bound(self, root, target):
        if root is None:
            return None 
        
        if target > root.val: 
            return self.get_upper_bound(root.right, target)
        
        # target <= root.val 
        upper = self.get_upper_bound(root.left, target)
        return upper if upper is not None else root  
        

```


```python
# Python 2: with iteration 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param target: the given target
    @return: the value in the BST that is closest to the target
    """
    def closestValue(self, root, target):
        # corner case 
        if not root or not target:
            return None 
        
        lower, upper = None, None 
        while root:
            if root.val > target:
                upper = root
                root = root.left
            elif root.val < target:
                lower = root 
                root = root.right
            else:
                return root.val 
        # if lower is not available         
        if not lower:
            return upper.val
        if not upper:
            return lower.val 
        # if both lower and upper exist 
        return upper.val if target - lower.val > upper.val - target else lower.val

```

596 - Minimum Subtree

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

* DFS, binary tree 



```python
Input:
     1
   /   \
 -5     2
 / \   /  \
0   2 -4  -5 

Output:1
```


```python
# Python 1: use divide and conquer to find the sum values, use traversal to find the min_sum node position 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of binary tree
    @return: the root of the minimum subtree
    """
    def findSubtree(self, root):
        self.min_sum = sys.maxsize 
        self.min_sum_node = None 
        self.find_sum(root)
        return self.min_sum_node 

          
    def find_sum(self, root):
        if root is None:
            return 0
        left_sum = self.find_sum(root.left)
        right_sum = self.find_sum(root.right)
        root_sum = left_sum + root.val + right_sum
        if root_sum < self.min_sum:
            self.min_sum = root_sum
            self.min_sum_node = root 
        return root_sum 

        
        

```


```python
# Python 2: use pure divide and conquer, use a helper function to not only compute sums, but also record nodes 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: the root of binary tree
    @return: the root of the minimum subtree
    """
    def findSubtree(self, root):
        if not root:
            return None 
        min_node, _, _ = self.helper(root)
        return min_node 
        
        
    # return three values, the min_sum for all subtrees rooted at root, corresponding node,
    # the root_sum 
    def helper(self, root):
        if root is None:
            return None, sys.maxsize, 0
            
        left_min_node, left_min, left_sum = self.helper(root.left)
        right_min_node, right_min, right_sum = self.helper(root.right)
        root_sum = left_sum + root.val + right_sum 
        
        min_sum = min(left_min, right_min, root_sum)
        # compare 
        if min_sum == left_min:
            return left_min_node, left_min, root_sum 
        elif min_sum == right_min:
            return right_min_node, right_min, root_sum 
        else:
            return root, root_sum, root_sum 
    
 
```

597 - Subtree with Maximum Average

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.



Input：

{1,-5,11,1,2,4,-2}

Output：11

Explanation:

The tree is look like this:

        1
      /   \
    -5     11
    / \   /  \
    1   2 4    -2 

The average of subtree of 11 is 4.3333, is the maximun.


```python
# Python 1: traversal 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: the root of binary tree
    @return: the root of the maximum average of subtree
    """
    def findSubtree2(self, root):
        if not root:
            return None 
        self.max_ave = - sys.maxsize 
        self.max_ave_node = None 
        self.find_average(root)
        return self.max_ave_node 
    
    def find_average(self, root):
        if root is None:
            return 0, 0 
        left_ave, left_size = self.find_average(root.left)
        right_ave, right_size = self.find_average(root.right)
        
        ave, size = (left_ave * left_size + right_ave * right_size + root.val) / (left_size + right_size + 1), left_size + right_size + 1
        if self.max_ave < ave:
            self.max_ave = ave
            self.max_ave_node = root 
        return ave, size 

```


```python
# Python 2: divide and conquer 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
import sys 
class Solution:
    """
    @param root: the root of binary tree
    @return: the root of the maximum average of subtree
    """
    def findSubtree2(self, root):
        if not root:
            return None 
        max_ave_node, _, _ , _= self.find_average(root)
        return max_ave_node 
    
    def find_average(self, root):
        if root is None:
            return None, None, None, 0 
        if root.left is None and root.right is None:
            return root, root.val, root.val, 1 
        
        left_max_node, left_max_ave, left_ave, left_size = self.find_average(root.left)
        right_max_node, right_max_ave, right_ave, right_size = self.find_average(root.right)
        
        # make sure the above values are not None for arithmetic operations 
        left_ave  = left_ave if left_ave is not None else 0
        left_size = left_size if left_size is not None else 0 
        left_max_ave = left_max_ave if left_max_ave is not None else -sys.maxsize
        
        right_ave = right_ave if right_ave is not None else 0 
        right_size = right_size if right_size is not None else 0 
        right_max_ave = right_max_ave if right_max_ave is not None else -sys.maxsize 
        
    
         # compute ave, size    
        ave = (left_ave * left_size + right_ave * right_size + root.val) / (left_size + right_size + 1)
        size = left_size + right_size + 1 
        
        
        
        cur_max_ave = max(left_max_ave, right_max_ave, ave)
        
        if cur_max_ave == left_max_ave:
            return left_max_node, left_max_ave, ave, size 
        elif cur_max_ave == right_max_ave:
            return right_max_node, right_max_ave, ave, size 
        else:
            return root, ave, ave, size 
            
```

453 -  Flatten Binary Tree to Linked List

Flatten a binary tree to a fake "linked list" in pre-order traversal.

Here we use the right pointer in TreeNode as the next pointer in ListNode.

* DFS, binary tree, divide and conquer 


```python
Input:
{1,2,5,3,4,#,6}
     1
    / \
   2   5
  / \   \
 3   4   6
Output:
{1,#,2,#,3,#,4,#,5,#,6}
1
\
 2
  \
   3
    \
     4
      \
       5
        \
         6
```


```python
# Python: divide and conquer 
# think about 
#    1
#  /   \
# 2    3
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""


class Solution:
    # keeps updating last_node , think about {2, 1, 3}
    last_node = None
    
    """
    @param root: a TreeNode, the root of the binary tree
    @return: nothing
    """
    def flatten(self, root):
        if root is None:
            return
        # key part: last_node is the parent node of the current node, set last_node left as None 
        # move the current node as the right child of last_node and set the current node as parent of next 
        if self.last_node is not None:
            self.last_node.left = None
            self.last_node.right = root
            
        self.last_node = root
        right = root.right
        self.flatten(root.left)
        self.flatten(right)
```


```python
# Python 2: divide and conquer 
class Solution:
    """
    @param root: a TreeNode, the root of the binary tree
    @return: nothing
    """
    def flatten(self, root):
        self.helper(root)
        
    # restructure and return last node in preorder
    def helper(self, root):
        if root is None:
            return None
        # the last node in left subtree after flattening     
        left_last = self.helper(root.left)
        # the last node in right subtree after flattening 
        right_last = self.helper(root.right)
        
        # connect the last node of left subtree to the right 
        if left_last is not None:
            left_last.right = root.right
            root.right = root.left
            root.left = None
        # if the right last node is not None, return it for connection     
        if right_last is not None:
            return right_last
        # if the left last node is not None 
        if left_last is not None:
            return left_last
        # if both root.left and root.right are None (the helper will return None) 
        return root
```

480 -  Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

* Binary tree 


```python
Input:

   1
 /   \
2     3
 \
  5

Output:


[
  "1->2->5",
  "1->3"
]
```


```python
# Python 1: traversal 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
class Solution:

    """
    @param root: the root of the binary tree
    @return: all root-to-leaf paths
    """
    def binaryTreePaths(self, root):
        if root is None:
            return []
        result = []
        path = [str(root.val)]
#         print(path)
        self.helper(root, path, result)
        return result 
    
    def helper(self, root, path, result):
        if root is None:
            return 
        
        if root.left is None and root.right is None:
            result.append('->'.join(path))
            
        if root.left is not None:
            # append, O(1), if use path + root.left.val, create a new list, O(n)
            path.append(str(root.left.val))
            self.helper(root.left, path, result)
            # backtrack
            path.pop()

        if root.right is not None:
            path.append(str(root.right.val)) 
            self.helper(root.right, path, result)
            path.pop()

```


```python
# Python 2: divide and conquer 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of the binary tree
    @return: all root-to-leaf paths
    """
    def binaryTreePaths(self, root):
        if root is None:
            return []
            
        if root.left is None and root.right is None:
            return [str(root.val)]

        paths = []
        for path in self.binaryTreePaths(root.left):
            paths.append(str(root.val) + '->' + path)
        
        for path in self.binaryTreePaths(root.right):
            paths.append(str(root.val) + '->' + path)
            
        return paths
```


```python
# Python: DFS 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of the binary tree
    @return: all root-to-leaf paths
    """
    def binaryTreePaths(self, root):
        if root is None:
            return []
            
        result = []
        self.dfs(root, [str(root.val)], result)
        return result

    def dfs(self, node, path, result):
        
        if node.left is None and node.right is None:
            result.append('->'.join(path))
            return
        
        if node.left:
            path.append(str(node.left.val))
            self.dfs(node.left, path, result);
            path.pop()
        
        if node.right:
            path.append(str(node.right.val))
            self.dfs(node.right, path, result)
            path.pop()

```


```python
# DFS
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of the binary tree
    @return: all root-to-leaf paths
    """
    def binaryTreePaths(self, root):
        if root is None:
            return []
            
        result = []
        self.dfs(root, [], result)
        return result

    def dfs(self, node, path, result):
        path.append(str(node.val))
        
        if node.left is None and node.right is None:
            result.append('->'.join(path))
            path.pop()
            return

        if node.left:
            self.dfs(node.left, path, result);
        
        if node.right:
            self.dfs(node.right, path, result)

        path.pop()
```

376 - Binary Tree Path Sum

Given a binary tree, find all paths that sum of the nodes in the path equals to a given number `target`.

A valid path is from root node to any of the leaf nodes.


```python
Example: 
    
Input:
{1,2,4,2,3}
5
Output: [[1, 2, 2],[1, 4]]
Explanation:
The tree is look like this:
	     1
	    / \
	   2   4
	  / \
	 2   3
```


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""


class Solution:
    """
    @param: root: the root of binary tree
    @param: target: An integer
    @return: all valid paths
    """
    def binaryTreePathSum(self, root, target):
        if not root or not target:
            return []
        all_paths = []
        self.find_paths(root, [root.val], all_paths)
        # print(all_paths)
        results = []
        for path in all_paths:
            # print('path', path)
            if sum(path) == target:
                results.append(path)
        return results 
    
    def find_paths(self, root, path, results):
        if root.left is None and root.right is None:
            # deep copy
            results.append(path[:])
            return 
        
        if root.left:
            path.append(root.left.val)
            self.find_paths(root.left, path, results)
            path.pop()
            
        if root.right:
            path.append(root.right.val)
            self.find_paths(root.right, path, results)
            path.pop()
        return results 
        
```

246 - Binary Tree Path Sum II

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.




```python
# Python 1: dfs, for each path, check if there exists subsets sum == target 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""


class Solution:
    """
    @param: root: the root of binary tree
    @param: target: An integer
    @return: all valid paths
    """
    def binaryTreePathSum2(self, root, target):
        if not root:
            return []
        results = []
        self.dfs(root, [root.val], results, target)
        return results 
    
    def dfs(self, root, path, results, target):
        # print('path', path)
  
        residuals = target
        for i in range(len(path) - 1, -1, -1):
            residuals -= path[i]
            # print('res', residuals)
            if residuals == 0:
                results.append(path[i:])
                # print('true', path[i:])
        if root.left is None and root.right is None:
            return 
        if root.left:
            path.append(root.left.val)
            self.dfs(root.left, path, results, target)
            path.pop()
            
        if root.right:
            path.append(root.right.val)
            self.dfs(root.right, path, results, target)
            path.pop()
        
            

```


```python
# Python 2: a more concise way, different return conditions
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
class Solution:
    # @param {TreeNode} root the root of binary tree
    # @param {int} target an integer
    # @return {int[][]} all valid paths
    def binaryTreePathSum2(self, root, target):
        # Write your code here
        result = []
        path = []
        if root is None:
            return result
        self.dfs(root, path, result,  target)
        return result

    def dfs(self, root, path, result, target):
        if root is None:
            return
        path.append(root.val)
        residuals = target
        for i in range(len(path) - 1, -1, -1):
            residuals -= path[i]
            if residuals == 0:
                result.append(path[i:])

        self.dfs(root.left, path, result, target)
        self.dfs(root.right, path, result, target)

        path.pop()
```

472 - Binary Tree Path Sum III

Give a binary tree, and a target number, find all path that the sum of nodes equal to target, the path could be start and end at any node in the tree.

Example: 
    
Input:
tree = {1,2,3,4}

target = 6

Output: 

  [
  [2, 4],
  
  [2, 1, 3],
  
  [3, 1, 2],
  
  [4, 2]
]


        1
       / \
      2   3
     /
    4



```python
"""
Definition of ParentTreeNode:
class ParentTreeNode:
    def __init__(self, val):
        self.val = val
        self.parent, self.left, self.right = None, None, None
"""
class Solution:
    # @param {ParentTreeNode} root the root of binary tree
    # @param {int} target an integer
    # @return {int[][]} all valid paths
    def binaryTreePathSum3(self, root, target):
        # Write your code here
        results = []
        self.dfs(root, target, results)
        return results

    def dfs(self, root, target, results):
        if root is None:
            return
        
        path = []
        self.findSum(root, None, target, path, results)

        self.dfs(root.left, target, results)
        self.dfs(root.right, target, results)

    def findSum(self, root, father, target, path, results):
        path.append(root.val)
        target -= root.val

        if target == 0:
            results.append(path[:])

        if root.parent not in [None, father]:
            self.findSum(root.parent, root, target, path, results)

        if root.left not in [None, father]:
            self.findSum(root.left, root, target, path, results)

        if root.right not in [None, father]:
            self.findSum(root.right, root, target, path, results)

        path.pop()

```

94 - Binary Tree Maximum Path Sum

Given a binary tree, find the maximum path sum.

The path may start and end at any node in the tree.


Example 2:

	Input:  For the following binary tree:

      1
     / \
    2   3
		
	Output: 6


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
import sys
class Solution:
    """
    @param root: The root of binary tree.
    @return: An integer
    """
    def maxPathSum(self, root):
        if not root:
            return 
        max_sum, _ = self.helper(root)
        return max_sum 
    
    def helper(self, root):
        if root is None:
            return -sys.maxsize, 0
        
        left_sum_max, left_sum_subtree = self.helper(root.left)
        right_sum_max, right_sum_subtree = self.helper(root.right)
        
        sum_max = max(left_sum_max, right_sum_max, left_sum_subtree + root.val + right_sum_subtree)
        sum_root = max(left_sum_subtree + root.val, right_sum_subtee + root.val, 0)
        return sum_max, sum_root
        
```

902 - Kth Smallest Element in a BST

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.


Example: 

Given root = {1,#,2}, k = 2, return 2.


* DFS, binary search tree , in-order / inorder  



```python
# Python: iteration of in-order traversal to the k-th value 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param k: the given k
    @return: the kth smallest element in BST
    """
    def kthSmallest(self, root, k):
        if root is None:
            return None 
        dummy_node = TreeNode(-1)
        dummy_node.right = root 
        
        # stack to store nodes to be backtracked 
        stack = [dummy_node]
        
        for _ in range(k):
            cur_node = stack.pop()
            if cur_node.right:
                cur_node = cur_node.right
                while cur_node:
                    stack.append(cur_node)
                    cur_node = cur_node.left
            if not stack:
                break
        return stack[-1].val
        
        
        
 
```


```python
# python 2: use a counter to count which is the k-th 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param k: the given k
    @return: the kth smallest element in BST
    """
    def kthSmallest(self, root, k):
        if not root:
            return -1 
        
        dummy_node = TreeNode(-1)
        dummy_node.right = root 
        
        stack = [dummy_node]
        count = -1 
        while stack:
            cur_node = stack.pop()
            # print(cur_node.val)
            count +=1 
            if count == k:
                return cur_node.val 
            if cur_node.right:
                cur_node = cur_node.right
                while cur_node:
                    stack.append(cur_node)
                    # print('curnode val', cur_node.val)
                    cur_node = cur_node.left
            
        return -1         
                
```

901 - Closest Binary Search Tree Value II

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

* Stack, DFS, binary tree


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param target: the given target
    @param k: the given k
    @return: k values in the BST that are closest to the target
    """
    def closestKValues(self, root, target, k):
        if root is None:
            return []
        ordered_vals = self.in_order_traverse(root, target)
        # print('ordered_vals', ordered_vals)
        # binary search 
        if len(ordered_vals) == 1:
            return ordered_vals
        left, right = 0, len(ordered_vals) - 1 
        while left + 1 < right:
            mid = (left + right) // 2 
            if ordered_vals[mid] < target:
                left = mid 
            else:
                right = mid 
        
        for _ in range(k):
            if self.is_left_closer(ordered_vals, left, right, target):
                left -= 1 
            else:
                right +=1 
        return ordered_vals[left + 1: right]
    
    def is_left_closer(self, ordered_vals, left, right, target):
        if left < 0:
            return False 
        if right > len(ordered_vals) - 1:
            return True 
        return (target - ordered_vals[left]) < (ordered_vals[right] - target)
                
        
        
        
    def in_order_traverse(self, root, target):
        results = []
        dummy = TreeNode(None)
        dummy.right = root 
        stack = [dummy]
        while stack:
            cur_node = stack.pop()
            # print('cur node', cur_node.val)
            if cur_node.val is not None:
                results.append(cur_node.val)
            if cur_node.right:
                cur_node = cur_node.right
                while cur_node:
                    stack.append(cur_node)
                    cur_node = cur_node.left
        return results
    
```


```python
# Python 2: same idea, another way to implement, first in-order traversal, then use the the k closet numbers methods 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the given BST
    @param target: the given target
    @param k: the given k
    @return: k values in the BST that are closest to the target
    """
    def closestKValues(self, root, target, k):
        if root is None:
            return None
        result = self.in_order_traversal(root)
        return self.k_closest_numbers(result, target, k)
    
    def in_order_traversal(self, root):
        if root is None:
            return []
        # create a dummy node so the root node can be treated as the way as as other nodes 
        dummy_node = TreeNode(-1)
        dummy_node.right = root 
        #stack to save the nodes to be backtracked 
        stack = [dummy_node]  # stack top is always the current iterator 
        result = []
        while stack:
            cur_node = stack.pop()
            if cur_node.right:
                cur_node = cur_node.right 
                while cur_node:
                    stack.append(cur_node)
                    cur_node = cur_node.left
            if stack:
                # only collect the stack top value, not pop out 
                result.append(stack[-1].val)
                
        return result 
    
    def k_closest_numbers(self, A, target, k):
        # write two functions: one does binary search for the closet number, the other does compare
        # of which side has a closer number 
        if len(A) < k: 
            return A
        if len(A) == 0: 
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
            if (A[mid] < target):
                left = mid 
            else: 
                right = mid
        if (A[left] >= target):
            return left 
        else: 
            return right
    
    def is_left_closer(self, A, left, right, target):
        if (left < 0):
            return False 
        if (right >= len(A)):
            return True
        if (target - A[left]) <= (A[right] - target): 
            return True
        return False


        

```


```python
# Python 2: treate tree as array, solution version from 
# https://www.jiuzhang.com/solution/closest-binary-search-tree-value-ii/#tag-highlight-lang-python
class Solution:
    """
    @param root: the given BST
    @param target: the given target
    @param k: the given k
    @return: k values in the BST that are closest to the target
    """
    def closestKValues(self, root, target, k):
        if root is None or k == 0:
            return []
            
        lower_stack = self.get_stack(root, target)
        upper_stack = list(lower_stack)
        if lower_stack[-1].val < target:
            self.move_upper(upper_stack)
        else:
            self.move_lower(lower_stack)
        
        result = []
        for i in range(k):
            if self.is_lower_closer(lower_stack, upper_stack, target):
                result.append(lower_stack[-1].val)
                self.move_lower(lower_stack)
            else:
                result.append(upper_stack[-1].val)
                self.move_upper(upper_stack)
                
        return result
        
    def get_stack(self, root, target):
        stack = []
        while root:
            stack.append(root)
            if target < root.val:
                root = root.left
            else:
                root = root.right
                
        return stack
        
    def move_upper(self, stack):
        if stack[-1].right:
            node = stack[-1].right
            while node:
                stack.append(node)
                node = node.left
        else:
            node = stack.pop()
            while stack and stack[-1].right == node:
                node = stack.pop()
                
    def move_lower(self, stack):
        if stack[-1].left:
            node = stack[-1].left
            while node:
                stack.append(node)
                node = node.right
        else:
            node = stack.pop()
            while stack and stack[-1].left == node:
                node = stack.pop()
                
    def is_lower_closer(self, lower_stack, upper_stack, target):
        if not lower_stack:
            return False
            
        if not upper_stack:
            return True
            
        return target - lower_stack[-1].val < upper_stack[-1].val - target
```

88 - Lowest Common Ancestor of a Binary Tree

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

* Assume two nodes are exist in tree.


Example: 

Input：{1},1,1

Output：1

Explanation：

 For the following binary tree（only one node）:
         1
         
 LCA(1,1) = 1


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""


class Solution:
    """
    @param: root: The root of the binary search tree.
    @param: A: A TreeNode in a Binary.
    @param: B: A TreeNode in a Binary.
    @return: Return the least common ancestor(LCA) of the two nodes.
    """
    def lowestCommonAncestor(self, root, A, B):
        if root is None:
            return None 
        common_ancestor = self.find_ancestor(root, A, B)
        return common_ancestor 
    
    def find_ancestor(self, root, A, B):
        # check if root is None
        if root is None:
            return root 
        
        # if root is not None, check if root is either A or B 
        if root == A or root == B:
            return root 
        
        # if root is neighter A nor B, check if there are A or B in left/right subtree  
        at_left = self.find_ancestor(root.left, A, B)
        at_right = self.find_ancestor(root.right, A, B)
        
        # if both at_left and at_right are not None,
        if at_left and at_right:
            return root 
        # if on only one side 
        return at_left if at_left else at_right
 

```

474 - Lowest Common Ancestor II

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute parent which point to the father of itself. The root's parent is `null`.


```python
Input:
      4
     / \
    3   7
       / \
      5   6
and 3,5
Output: 4
Explanation:LCA(3, 5) = 4
```


```python
# First find all ancestor nodes of A, then compare B with the ancestors  
"""
Definition of ParentTreeNode:
class ParentTreeNode:
    def __init__(self, val):
        self.val = val
        self.parent, self.left, self.right = None, None, None
"""


class Solution:
    """
    @param: root: The root of the tree
    @param: A: node in the tree
    @param: B: node in the tree
    @return: The lowest common ancestor of A and B
    """
    def lowestCommonAncestorII(self, root, A, B):
        if root is None:
            return root 
        common_ancestor = self.find_ancestor(root, A, B)
        return common_ancestor 
    
    def find_ancestor(self, root, A, B):
        # use hash set to save for quick search 
        a_parent = {}
        while A != root:
            a_parent[A] = True
            A = A.parent
            
        while B != root:
            if B in a_parent:
                return B 
            B = B.parent
            
        return root 
            



```

578 - Lowest Common Ancestor III


Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.
Return null if LCA does not exist.


* DFS, binary tree 

* node A or node B may not exist in tree.




```python
Input: 
{4, 3, 7, #, #, 5, 6}
3 5
5 6
6 7 
5 8
Output: 
4
7
7
null
Explanation:
  4
 / \
3   7
   / \
  5   6

LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7
LCA(5, 8) = null

```


```python
# Python 1: find each node and its path (if any) first, then compare the paths for the first common node 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        this.val = val
        this.left, this.right = None, None
"""

class Solution:
    """
    @param: root: The root of the binary tree.
    @param: A: A TreeNode
    @param: B: A TreeNode
    @return: Return the LCA of the two nodes.
    """
    def lowestCommonAncestor3(self, root, A, B):
        if root is None:
            return root
        a_exist, a_ancestors = self.find_node(root, [root], A)
        # print(a_exist, a_ancestors)

        b_exist, b_ancestors = self.find_node(root, [root], B)
        # print(b_exist, b_ancestors)
        
        if not a_exist or not b_exist:
            return None 
        
        a_len = len(a_ancestors)
        b_len = len(b_ancestors)
        
        if a_len > b_len:
            while len(a_ancestors) > b_len:
                a_ancestors.pop()
        if a_len < b_len:
            while len(b_ancestors) > a_len:
                b_ancestors.pop()
                
        while a_ancestors is not None and b_ancestors is not None and a_ancestors[-1] != b_ancestors[-1]:
            a_ancestors.pop()
            b_ancestors.pop()
        
        if a_ancestors is None:
            return None 
        return a_ancestors[-1]
    
    def find_node(self, root, path, node):
        #     
        if root == node:
            return True, path
            
        if root.left is None and root.right is None:
            return False, [] 

        
        if root.left is not None:
            path.append(root.left)
            self.find_node(root.left, path, node)
            if path[-1] == node:
                return True, path
            path.pop()
    
        if root.right is not None:
            path.append(root.right)
            self.find_node(root.right, path, node)
            if path[-1] == node:
                return True, path
            path.pop()
        return False, []

```


```python
# Python 2: use a helper function to help decide if A and B exist with returned tuple 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        this.val = val
        this.left, this.right = None, None
"""

class Solution:
    """
    @param: root: The root of the binary tree.
    @param: A: A TreeNode
    @param: B: A TreeNode
    @return: Return the LCA of the two nodes.
    """
    def lowestCommonAncestor3(self, root, A, B):
        if root is None:
            return root
        a_found, b_found, parent = self.helper(root, A, B)
        if a_found and b_found:
            return parent
        else: 
            return None


    def helper(self, root, A, B):
        if root is None:
            return False, False, None
            
        left_a_found, left_b_found, left_node = self.helper(root.left, A, B)
        right_a_found, right_b_found, right_node = self.helper(root.right, A, B)
        
        a_found = left_a_found or right_a_found or root == A
        b_found = left_b_found or right_b_found or root == B
        
        if root == A or root == B:
            return a_found, b_found, root

        if left_node is not None and right_node is not None:
            return a_found, b_found, root
        if left_node is not None:
            return a_found, b_found, left_node
        if right_node is not None:
            return a_found, b_found, right_node

        return a_found, b_found, None
    
    
    def helper(self, root, A, B):
        if root is None:
            return False, False, None
        
        left_a_found, left_b_found, left_node = self.helper(root.left, A, B)
        right_a_found, right_b_found, right_node = self.helper(root.left, A, B)
        
        a_found = left_a_found or right_a_found or root == A 
        b_found = left_b_found or right_b_found or root == B 
        
        if root == A or root == B:
            return a_found, b_found, root 
        
        if left_node is not None and right_node is not None:
            return a_found, b_found, root 
        
        if left_node is not None:
            return a_found, b_found, left_node 
        if right_node is not None:
            return a_found, b_found, right_node
        return a_found, b_found, None 
```

86 - Binary Search Tree Iterator

Design an iterator over a binary search tree with the following rules:

Elements are visited in ascending order (i.e. an in-order traversal)

`next()` and `hasNext()` queries run in O(1) time in average.

* Binary tree, binary search tree


```python
For the following binary search tree, in-order traversal by using iterator is [1, 6, 10, 11, 12]

   10
 /    \
1      11
 \       \
  6       12
```


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None

Example of iterate a tree:
iterator = BSTIterator(root)
while iterator.hasNext():
    node = iterator.next()
    do something for node 
"""


class BSTIterator:
    """
    @param: root: The root of binary tree.
    """
    def __init__(self, root):
        # stack and cur_node to store the iterated nodes and the cur_node to be iterated 
        self.stack = []
        self.cur_node = root 

    """
    @return: True if there has next node, or false
    """
    def hasNext(self, ):
        return self.cur_node is not None or len(self.stack) > 0

    """
    @return: return next node
    """
    def next(self, ):
        # go to the left most node
        while self.cur_node:
            self.stack.append(self.cur_node)
            self.cur_node = self.cur_node.left
        # record the node to be returned 
        next_node = self.stack.pop()
        # if the next_node has right child, move cur_node to it for the following iteration 
        if next_node.right:
            self.cur_node = next_node.right
        return next_node
        

```


```python
# Python 2: another method 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None

Example of iterate a tree:
iterator = BSTIterator(root)
while iterator.hasNext():
    node = iterator.next()
    do something for node 
"""


class BSTIterator:
    """
    @param: root: The root of binary tree.
    """
    def __init__(self, root):
        self.stack = []
        while root != None:
            self.stack.append(root)
            root = root.left

    """
    @return: True if there has next node, or false
    """
    def hasNext(self):
        return len(self.stack) > 0

    """
    @return: return next node
    """
    def next(self):
        node = self.stack[-1]
        if node.right is not None:
            n = node.right
            while n != None:
                self.stack.append(n)
                n = n.left
        else:
            n = self.stack.pop()
            while self.stack and self.stack[-1].right == n:
                n = self.stack.pop()
        
        return node
```

595 - Binary Tree Longest Consecutive Sequence

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from `parent` to `child` (cannot be the reverse).


Input:


     1
      \
       3
      / \
     2   4
          \
           5
           
Output:3


Explanation:
Longest consecutive sequence path is 3-4-5, so return 3.


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of binary tree
    @return: the length of the longest consecutive sequence path
    """
    def longestConsecutive(self, root):

        return self.helper(root, None, 0)
    
    def helper(self, root, parent, length):
        if root is None:
            return length
        if parent != None and root.val == parent.val + 1:
            length += 1 
        else: 
            length = 1 
        return max(length, max(self.helper(root.left, root, length)), self.helper(root.right, root, length))
        

```

614 -  Binary Tree Longest Consecutive Sequence II

Given a binary tree, find the length(number of nodes) of the longest consecutive sequence(Monotonic and adjacent node values differ by 1) path.
The path could be start and end at any node in the tree

Example: 
    
Input:
{1,2,0,3}

Output:

    4

Explanation:

       1
      / \
     2   0
    /
    3

0-1-2-3


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: the root of binary tree
    @return: the length of the longest consecutive sequence path
    """
    def longestConsecutive2(self, root):
        max_length, _ , _ = self.helper(root)
        return max_length
    
    def helper(self, root):
        if root is None:
            return 0, 0, 0
        left_max_len, left_max_down, left_max_up = self.helper(root.left)
        right_max_len, right_max_down, right_max_up = self.helper(root.right)
        
        down = 0
        up = 0 
        
        if root.left is not None and root.left.val + 1 == root.val:
            down = max(down, left_max_down + 1)
        if root.left is not None and root.left.val - 1 == root.val:
            up = max(up, left_max_up + 1)
        if root.right is not None and root.right.val + 1 == root.val:
            down = max(down, right_max_down + 1)
            
        if root.right is not None and root.right.val - 1 == root.val:
            up = max(up, right_max_up + 1)
            
        length = down + 1 + up 
        length = max(length, max(left_max_len, right_max_len))
        
        return length, down, up
        

```
