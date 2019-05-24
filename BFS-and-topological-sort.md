
## BFS and Topological sorting 

137 - Clone Graph

Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

How we serialize an undirected graph:http://www.lintcode.com/help/graph/

Nodes are labeled uniquely.

We use `#` as a separator for each node, and `,` as a separator for node label and each neighbor of the node.

return a deep copied graph.



As an example, consider the serialized graph {0,1,2#1,2#2,2}.

The graph has a total of three nodes, and therefore contains three parts as separated by #.

First node is labeled as 0. Connect node 0 to both nodes 1 and 2.

Second node is labeled as 1. Connect node 1 to node 2.

Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.

Visually, the graph looks like the following:


* BFS




```python

   1
  / \
 /   \ 
0 --- 2
     / \
     \_/ 
```


```python
使用宽度优先搜索 BFS 的版本。

第一步：找到所有的点
第二步：复制所有的点，将映射关系存起来
第三步：找到所有的边，复制每一条边

```


```python
"""
Definition for a undirected graph node
class UndirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []
"""

from collections import deque 
class Solution:
    """
    @param: node: A undirected graph node
    @return: A undirected graph node
    """
    def cloneGraph(self, node):
        if not node:
            return node 
        # root node 
        root = node 
        
        # find all graph nodes and store them in a set 
        all_nodes = self.get_nodes(node)
        
        # store each node and its corresponding label, neighbors 
        mapping = {}
        # initialize a graph node for each node 
        for node in all_nodes:
            mapping[node] = UndirectedGraphNode(node.label)

        # copy neighbors (edges) from old ones 
        for nd in all_nodes:
            new_node = mapping[nd]
            # add neighbors info to new nodes 
            for neighbor in nd.neighbors:
                # neighbor graph node 
                new_neighbor = mapping[neighbor]
                new_node.neighbors.append(new_neighbor)
        return mapping[root]
        
        # get all nodes with node as root 
    def get_nodes(self, node):
        queue = deque([node])
        result = set([node])
        while queue:
            nd = queue.popleft()
            for neighbor in nd.neighbors:
                if neighbor not in result:
                    result.add(neighbor)
                    queue.append(neighbor)
        return result
        
        
```

7 - Serialize and Deserialize Binary Tree

Design an algorithm and write code to serialize and deserialize a binary tree. Writing the tree to a file is called 'serialization' and reading back from the file to reconstruct the exact same binary tree is 'deserialization'.

* Binary tree , two pointers

Example: 
Input:
Binary tree {3,9,20,#,#,15,7},  denote the following structure:

	  3 
	 / \
	9  20
	  /  \
	 15   7
     
     



```python
from collections import deque 

class Solution:
    """
    @param root: An object of TreeNode, denote the root of the binary tree.
    This method will be invoked first, you should design your own algorithm 
    to serialize a binary tree which denote by a root node to a string which
    can be easily deserialized by your own "deserialize" method later.
    """
    def serialize(self, root):
        # special case
        if root is None:
            return ''
        
        # BFS 
        queue = deque([root])
        result = []  # save the serialized result 
        while queue: 
            node = queue.popleft() 
            result.append(str(node.val) if node else '#') # save as string to be consistent with '#'
            if node: 
                queue.append(node.left)
                queue.append(node.right)
        return ' '.join(result)

    """
    @param data: A string serialized by your serialize method.
    This method will be invoked second, the argument data is what exactly
    you serialized at method "serialize", that means the data is not given by
    system, it's given by your own serialize method. So the format of data is
    designed by yourself, and deserialize it here as you serialize it in 
    "serialize" method.
    """
    def deserialize(self, data):
        # special case , None or ""
        if not data:
            return None 
        
        # unwrap the node values from the data string 
        result = [TreeNode(int(val)) if val != '#' else None for val in data.split()]
        
        # initialize the two pointers, slow points to parent node, fast points to left child  
        slow, fast = 0, 1
        nodes = [result[0]]  # initialize the nodes with the root node 
        # use two pointers, slow pointer points to the node, fast pointer points to the left and right child 
        
        # note: the length of nodes is dynamic 
        while slow < len(nodes):
            # read the node 
            node = nodes[slow]
            
            slow += 1 
            
            node.left = result[fast]
            node.right = result[fast + 1]
            
            fast += 2 
            
            # update nodes list for slow pointer 
            if node.left:
                nodes.append(node.left)
            if node.right:
                nodes.append(node.right)
        return result[0]
```


```python
''.join(['1', '2'])
```




    '12'




```python
''.join(['1', '2']).split()
```




    ['12']




```python
' '.join(['1', '2'])
```




    '1 2'




```python
' '.join(['1', '2']).split()
```




    ['1', '2']



69 - Binary Tree Level Order Traversal

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level). 

   

* BFS, binary tree, queue, DFS 


```python

Example, given binary tree 

    3
    
   / \
   
  9  20
    /  \
   15   7 

return its level order traversal as:

[
  [3],
  [9,20],
  [15,7]
]
```


```python
# Python 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
from collections import deque 

class Solution:
    """
    @param root: A Tree
    @return: Level order a list of lists of integer
    """
    def levelOrder(self, root):
        # corner case 
        if root is None:
            return []
        result = []
        queue = deque([root])
        while queue: 
            level_vals = []
            size = len(queue)
            for _ in range(size):
                new = queue.popleft()
                level_vals.append(new.val)
                if new.left is not None:
                    queue.append(new.left)
                if new.right is not None:
                    queue.append(new.right)
            result.append(level_vals)
        return result
                

```

70 - Binary Tree Level Order Traversal II

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

Example: 
    
Input:

{1,2,3}

Output:

[[2,3],[1]]


```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
from collections import deque 

class Solution:
    """
    @param root: A tree
    @return: buttom-up level order a list of lists of integer
    """
    def levelOrderBottom(self, root):
        if not root:
            return []
        # add node to queue 
        queue = deque([root])
        results = []
        while queue:
            size = len(queue)
            level_vals = []
            for _ in range(size):
                node = queue.popleft()
                # add node.val to list 
                level_vals.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            results.append(level_vals)

        # reverse 
        left, right = 0, len(results) - 1
        while left < right:
            results[left], results[right] = results[right], results[left]
            left += 1 
            right -= 1
    
        return results
        # return list(reversed(results))
        
            

```

71 - Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).




```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""
from collections import deque 

class Solution:
    """
    @param root: A Tree
    @return: A list of lists of integer include the zigzag level order traversal of its nodes' values.
    """
    def zigzagLevelOrder(self, root):
        if not root:
            return []
        level = 0
        queue = deque([root])
        results = []
        while queue:
            size = len(queue)
            level_vals = []
            for _ in range(size):
                node = queue.popleft()
                # append value 
                level_vals.append(node.val)
                
                # add next level nodes to queue 
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            if level % 2 == 1:
                results.append(level_vals[::-1])
            else:
                results.append(level_vals)
                
            # update level 
            level += 1 
        return results
        
```

433 - Number of Islands

Given a boolean 2D matrix, 0 is represented as the sea, 1 is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.

Find the number of islands.

* BFS, union find 



```python
Example: 
    
Input:
[
  [1,1,0,0,0],
  [0,1,0,0,1],
  [0,0,0,1,1],
  [0,0,0,0,0],
  [0,0,0,0,1]
]

Output:
3
```


```python
# Python 

from collections import deque

class Solution:
    """
    @param grid: a boolean 2D matrix
    @return: an integer
    """
    def numIslands(self, grid):
        if not grid:
            return 0 
        # counter of islands 
        count = 0 
        
        # loop over all elements 
        for i in range(len(grid)): # row 
            for j in range(len(grid[0])):
                # only run when grid[i][j] is 1 
                if grid[i][j]:
                    
                    # set grid[i][j] neighbors and neighbors of neighbors to be 0 
                    self.bfs_neighbors(grid, i, j)
                    
                    # set grid[i][j] to be 0 
                    grid[i][j] = 0
                    count += 1 

        return count 
    
    
    # bfs_neighbors find the neighbors of grid[i][j] and neighbors of the neighbors
    # that are 1 and set them to be 0 / False
    def bfs_neighbors(self, grid, i, j):
        # creat a list of tuples, note: queue.append((tuples)), append tuple elements directly, not [(tuples)]
        queue = deque([(i, j)])          
        
        while queue: 
            i, j = queue.popleft()
            for delta_x, delta_y in ([1, 0], [0, 1], [-1, 0], [0, -1]):
                neighbor_x, neighbor_y = i + delta_x, j + delta_y
                # tell if the neighbor x y coordinates are valid (within the matrix and = 1)
                valid_ind = self.is_valid(grid, neighbor_x, neighbor_y)
                # if it's not one 
                if not valid_ind:
                    continue  # next iteration of the for loop, ignore the rest 
                
                # if it's one, set it to zero  and continue to check the neighbors of neighbor_x, neighbor_y
                queue.append((neighbor_x, neighbor_y))
                grid[neighbor_x][neighbor_y] = 0
                
    # tell if the neighbor x y coordinates are valid (within the matrix and = 1)
    def is_valid(self, grid, i, j):
        return int(0 <= i < len(grid) and 0 <= j < len(grid[0]) and grid[i][j])
            
        

```


```python
p = Solution()
p.numIslands([[1,1,0,0,0],[0,1,0,0,1],[0,0,0,1,1],[0,0,0,0,0],[0,0,0,0,1]])
```




    3




```python
x = [1,1,0,0,0],[0,1,0,0,1],[0,0,0,1,1],[0,0,0,0,1]
x[1][0]
print(len(x))
print(len(x[0]))
```

    4
    5


598 - Zombie in Matrix

Given a 2D grid, each cell is either a wall `2`, a zombie `1` or people `0` (the number zero, one, two).Zombies can turn the nearest people(up/down/left/right) into zombies every day, but can not through wall. How long will it take to turn all people into zombies? Return -1 if can not turn all people into zombies.


```python
# Python: 
# 1. find all zombies (==1) at the beginning 
# 2. check up/down/right/left each each zombie, turn locations (==0) to 1 and add them to new zombies
# 3. repeat step 2 on new zombies to affect more zombies
from collections import deque 
class Solution:
    """
    @param grid: a 2D integer grid
    @return: an integer
    """
    def zombie(self, grid):
        if not grid:
            return -1 
        count = 0 
        start_zombies = []
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    start_zombies.append((i, j))
                    
        delta = [[0, 1], [0, -1], [-1, 0], [1, 0]]
        
        queue = deque(start_zombies)
        while queue:
            size = len(queue)
            count += 1 
            new_zombie = []

            for _ in range(size):
                cur_zombie = queue.popleft()

                for delta_x, delta_y in delta:
                    new_x, new_y = cur_zombie[0] + delta_x, cur_zombie[1] + delta_y
                    if self.is_person(grid, new_x, new_y):
                        grid[new_x][new_y] = 1 
                        new_zombie.append((new_x, new_y))
            queue = deque(new_zombie) 
        # check if there are still 0s in grid 
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 0:
                    return -1
        return count - 1 
        
        
    # is_person
    def is_person(self, grid, x, y):
        return 0 <= x < len(grid) and 0 <= y < len(grid[0]) and grid[x][y] == 0

```

611 -  Knight Shortest Path

Given a knight in a chessboard (a binary matrix with 0 as empty and 1 as barrier) with a source position, find the shortest path to a destination position, return the length of the route.
Return -1 if destination cannot be reached.


* Dynamic programming, BFS


```python
If the knight is at (x, y), he can get to the following positions in one step:

(x + 1, y + 2)
(x + 1, y - 2)
(x - 1, y + 2)
(x - 1, y - 2)
(x + 2, y + 1)
(x + 2, y - 1)
(x - 2, y + 1)
(x - 2, y - 1)


Input:
    
[[0,0,0],
 [0,0,0],
 [0,0,0]]


source = [2, 0] destination = [2, 2] 

Output: 2
    
Explanation:
    
[2,0]->[0,1]->[2,2]
```


```python
# Python 1: BFS 
"""
Definition for a point.
class Point:
    def __init__(self, a=0, b=0):
        self.x = a
        self.y = b
"""

class Solution:
    """
    @param grid: a chessboard included 0 (false) and 1 (true)
    @param source: a point
    @param destination: a point
    @return: the shortest path 
    """
    def shortestPath(self, grid, source, destination):
        row_num = len(grid)
        col_num = len(grid[0])
        delta = [(1, 2), (1, -2), (-1, 2), (-1, -2), (2, 1), (2, -1), (-2, 1), (-2, -1)]
        # distance to source 
        d_to_source = {(source.x, source.y):0} 
        
        # points to be evaluated 
        queue = collections.deque([(source.x, source.y)])  # list of tuples 
        
        while queue: 
            cur_x, cur_y = queue.popleft()
            # check if the current position equals destination 
            if (cur_x, cur_y )== (destination.x, destination.y):
                return d_to_source[(x, y)]
            
            # loop through neighbors of cur_position 
            for delta_x, delta_y in delta: 
                neighbor_x, neighbor_y = cur_x + delta_x, cur_y + delta_y 
                if (neighbor_x, neighbor_y) in d_to_source or not self.is_valid(row_num, col_num, neighbor_x, neighbor_y, grid):
                    continue 
                queue.append((neighbor_x, neighbor_y))
                d_to_source[(neighbor_x, neighbor_y)] = d_to_source[(cur_x, cur_y)] + 1 
                if (neighbor_x, neighbor_y) == (destination.x, destination.y):
                    return d_to_source[(neighbor_x, neighbor_y)]
                
                
    def is_valid(self, row_num, col_num, x, y, grid):
        return 0 <= x < row_num and 0 <= y < col_num and not grid[x][y]
        
    
    
            

```


```python
DIRECTIONS = [
    (-2, -1), (-2, 1), (-1, 2), (1, 2),
    (2, 1), (2, -1), (1, -2), (-1, -2),
]

class Solution:
        
    """
    @param grid: a chessboard included 0 (false) and 1 (true)
    @param source: a point
    @param destination: a point
    @return: the shortest path 
    """
    def shortestPath(self, grid, source, destination):
        queue = collections.deque([(source.x, source.y)])
        distance = {(source.x, source.y): 0}

        while queue:
            x, y = queue.popleft()
            if (x, y) == (destination.x, destination.y):
                return distance[(x, y)]
            for dx, dy in DIRECTIONS:
                next_x, next_y = x + dx, y + dy
                if (next_x, next_y) in distance:
                    continue
                if not self.is_valid(next_x, next_y, grid):
                    continue
                distance[(next_x, next_y)] = distance[(x, y)] + 1
                queue.append((next_x, next_y))
        return -1
        
    def is_valid(self, x, y, grid):
        n, m = len(grid), len(grid[0])

        if x < 0 or x >= n or y < 0 or y >= m:
            return False
            
        return not grid[x][y]
```

127 - Topological Sorting


Given an directed graph, a topological order of the graph nodes is defined as follow:

For each directed edge A -> B in graph, A must before B in the order list.

The first node in the order can be any node in the graph with no nodes direct to it.

Find any topological order for the given graph.


* Topological sort, BFS, DFS


```python
"""
Definition for a Directed graph node
class DirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []
"""
from collections import deque 

class Solution:
    """
    @param: graph: A list of Directed graph node
    @return: Any topological order for the given graph.
    """
    def topSort(self, graph):
        if not graph:
            return graph 
        
        indegrees = self.get_indegree(graph)
        queue = deque([x for x in indegrees if indegrees[x] == 0])
        results = []
        while queue:
            cur = queue.popleft()
            results.append(cur)
            for neighbor in cur.neighbors:
                indegrees[neighbor] -= 1 
                if indegrees[neighbor] == 0:
                    queue.append(neighbor)
        return results 
        
        
    def get_indegree(self, graph):
        indegrees = {x:0 for x in graph}
        for node in graph:
            for neighbor in node.neighbors:
                indegrees[neighbor] += 1 
        return indegrees 

```

615 - Course Schedule

There are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example, to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?


Example: 

Input: n = 2, prerequisites = [[1,0],[0,1]] 

Output: false


Input: n = 2, prerequisites = [[1,0]] 


Output: true


* Topological sort, BFS


```python
# Python: BFS 

from collections import deque
class Solution:
    """
    @param: numCourses: a total of n courses
    @param: prerequisites: a list of prerequisite pairs
    @return: true if can finish all courses or false
    """
    def canFinish(self, numCourses, prerequisites):
        
        # [1, 0] ~ i, j ~ to, froms 
        neighbors = {x:[] for x in range(numCourses)}
        indegree = {x:0 for x in range(numCourses)}
        
        # update neighbors and indegree 
        for to, froms in prerequisites:
            neighbors[froms].append(to)
            indegree[to] += 1 
        
        # topological sort 
        zero_indegree = deque([x for x in range(numCourses) if indegree[x] == 0])
        count = 0
        while zero_indegree:
            cur_node = zero_indegree.popleft()
            count += 1 
            for neighbor in neighbors[cur_node]:
                if cur_node in neighbors[neighbor]:
                    return False # a cycle 
                indegree[neighbor] -= 1 
                if (indegree[neighbor] == 0):
                    zero_indegree.append(neighbor)
        return count == numCourses
            
        

```


```python
from collections import deque 
class Solution:
    """
    @param: numCourses: a total of n courses
    @param: prerequisites: a list of prerequisite pairs
    @return: true if can finish all courses or false
    """
    def canFinish(self, numCourses, prerequisites):
        # coner case 
        if not prerequisites:
            return True  
        # compute indegrees and neighbors 
        neighbors = {x:[] for x in range(numCourses)}
        indegrees = {x:0 for x in range(numCourses)}
        for to, froms in prerequisites:
            neighbors[froms].append(to)
            indegrees[to] += 1 
#         print(neighbors)
#         print(indegrees)
        # topological sort 
        count = 0
        zero_indegrees = deque([x for x in indegrees if indegrees[x] == 0])
        print(zero_indegrees)
        if len(zero_indegrees) == 0:
            return False 
        while zero_indegrees:
            cur_node = zero_indegrees.popleft()
            count += 1 
            for neighbor in neighbors[cur_node]:
                indegrees[neighbor] -= 1 
                if indegrees[neighbor] == 0:
                    zero_indegrees.append(neighbor)
        # tell if count == numCourses 
        return numCourses == count

```

616 - Course Schedule II

There are a total of n courses you have to take, labeled from 0 to n - 1.
Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Example: 

Input: n = 4, prerequisites = [1,0],[2,0],[3,1],[3,2]] 


Output: [0,1,2,3] or [0,2,1,3]

* BFS, topological sort 


```python

from collections import deque
class Solution:
    """
    @param: numCourses: a total of n courses
    @param: prerequisites: a list of prerequisite pairs
    @return: true if can finish all courses or false
    """

    def findOrder(self, numCourses, prerequisites):
        # [1, 0] ~ i, j ~ to, froms 
        neighbors = {x:[] for x in range(numCourses)}
        indegree = {x:0 for x in range(numCourses)}
        
        # update neighbors and indegree 
        for to, froms in prerequisites:
            neighbors[froms].append(to)
            indegree[to] += 1 
        
        # topological sort 
        zero_indegree = deque([x for x in range(numCourses) if indegree[x] == 0])
        result = []
        while zero_indegree:
            cur_node = zero_indegree.popleft()
            result.append(cur_node)
            for neighbor in neighbors[cur_node]:
                if cur_node in neighbors[neighbor]:
                    return [] # a cycle 
                indegree[neighbor] -= 1 
                if indegree[neighbor] == 0:
                    zero_indegree.append(neighbor)
        if len(result) == numCourses:
            return result
        return []
            
```


```python
p = Solution()
x = p.findOrder(100, [[6,27],[83,9],[10,95],[48,67],[5,71],[18,72],[7,10],[92,4],[68,84],[6,41],[82,41],[18,54],[0,2],[1,2],[8,65],[47,85],[39,51],[13,78],[77,50],[70,56],[5,61],[26,56],[18,19],[35,49],[79,53],[40,22],[8,19],[60,56],[48,50],[20,70],[35,12],[99,85],[12,75],[2,36],[36,22],[21,15],[98,1],[34,94],[25,41],[65,17],[1,56],[43,96],[74,57],[19,62],[62,78],[50,86],[46,22],[10,13],[47,18],[20,66],[83,66],[51,47],[23,66],[87,42],[25,81],[60,81],[25,93],[35,89],[65,92],[87,39],[12,43],[75,73],[28,96],[47,55],[18,11],[29,58],[78,61],[62,75],[60,77],[13,46],[97,92],[4,64],[91,47],[58,66],[72,74],[28,17],[29,98],[53,66],[37,5],[38,12],[44,98],[24,31],[68,23],[86,52],[79,49],[32,25],[90,18],[16,57],[60,74],[81,73],[26,10],[54,26],[57,58],[46,47],[66,54],[52,25],[62,91],[6,72],[81,72],[50,35],[59,87],[21,3],[4,92],[70,12],[48,4],[9,23],[52,55],[43,59],[49,26],[25,90],[52,0],[55,8],[7,23],[97,41],[0,40],[69,47],[73,68],[10,6],[47,9],[64,24],[95,93],[79,66],[77,21],[80,69],[85,5],[24,48],[74,31],[80,76],[81,27],[71,94],[47,82],[3,24],[66,61],[52,13],[18,38],[1,35],[32,78],[7,58],[26,58],[64,47],[60,6],[62,5],[5,22],[60,54],[49,40],[11,56],[19,85],[65,58],[88,44],[86,58]])
print(len(x))
y = set(x)
print(len(y))
```

    36
    36


611 -  Knight Shortest Path
Given a knight in a chessboard (a binary matrix with 0 as empty and 1 as barrier) with a source position, find the shortest path to a destination position, return the length of the route.

Return -1 if destination cannot be reached.


* BFS, dynamic programming 





```python

If the knight is at (x, y), he can get to the following positions in one step:

(x + 1, y + 2)
(x + 1, y - 2)
(x - 1, y + 2)
(x - 1, y - 2)
(x + 2, y + 1)
(x + 2, y - 1)
(x - 2, y + 1)
(x - 2, y - 1)



Input:
[[0,0,0],
 [0,0,0],
 [0,0,0]]

source = [2, 0] destination = [2, 2] 

Output: 2
    
Explanation:
[2,0]->[0,1]->[2,2]
```


```python
# Python 1: BFS 

"""
Definition for a point.
class Point:
    def __init__(self, a=0, b=0):
        self.x = a
        self.y = b
"""

class Solution:
    """
    @param grid: a chessboard included 0 (false) and 1 (true)
    @param source: a point
    @param destination: a point
    @return: the shortest path 
    """
    def shortestPath(self, grid, source, destination):
        n_row = len(grid)
        n_col = len(grid[0])
        
        # changes each step can make 
        delta = [(1, 2), (1, -2), (-1, 2), (-1, -2), (2, 1), (2, -1), (-2, 1), (-2, -1)]
        
        # distance to source dictionary 
        d_to_source = {(source.x, source.y):0}
        
        # queue to save the positions to be evaluated 
        queue = collections.deque([(source.x, source.y)])
        
        while queue: 
            cur_x, cur_y = queue.popleft()
            if (cur_x, cur_y) == (destination.x, destination.y):
                return d_to_source[(cur_x, cur_y)]
            for delta_x, delta_y in delta:
                neighbor_x, neighbor_y = cur_x + delta_x, cur_y + delta_y 
                if (neighbor_x, neighbor_y) in d_to_source or not self.is_valid(n_row, n_col, grid, neighbor_x, neighbor_y):
                    continue 
                queue.append((neighbor_x, neighbor_y))
                d_to_source[(neighbor_x, neighbor_y)] = d_to_source[(cur_x, cur_y)] + 1 
        return -1 
    def is_valid(self, grid, x, y):
        return 0 <= x < len(grid) and 0 <= y < len(grid[0]) and not grid[x][y]
    
            

```

605 - Sequence Reconstruction

Check whether the original sequence `org` can be uniquely reconstructed from the sequences in `seqs`. The `org` sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 10^4. 

Reconstruction means building a shortest common supersequence of the sequences in `seqs` (i.e., a shortest sequence so that all sequences in `seqs` are subsequences of it). Determine whether there is only one sequence that can be reconstructed from `seqs` and it is the `org` sequence.

* Topological sort, BFS


```python
Example 1: 
Input: org = [1,2,3], seqs = [[1,2],[1,3],[2,3]]
    
Output: true
    
Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].



Example 2: 
    
Input:org = [1,2,3], seqs = [[1,2],[1,3]]
Output: false
Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also
a valid sequence that can be reconstructed.
```


```python
# Python : build graph and check indegre separately (or if, for example 5 --> 2 appears more than once
# in two different sequences, indegree cannot tell it and will add indegree twice)

# 1. build key of each node for neighbors (build_graph) 
from collections import deque 
class Solution:
    """
    @param org: a permutation of the integers from 1 to n
    @param seqs: a list of sequences
    @return: true if it can be reconstructed only one or false
    """
    def sequenceReconstruction(self, org, seqs):
        
        if len(seqs) == 1 and len(seqs[0]) == 1:
            return seqs[0] == org
        # build a graph from seqs 
        neighbors = self.build_graph(seqs)
        indegree = self.get_indegree(neighbors)
        
        # topological sort 
        topo_sort = self.topological_sort(neighbors, indegree)
        return topo_sort == org 
        
        
    def build_graph(self, seqs):
        # two dictionaries 
        # 1. save neighbors 
        # 2. save indegrees 
        neighbors = {}
        
        # add neighbors node, do this separately to avoid missing nodes at the end of each seqs 
        for i in range(len(seqs)):
            for j in range(len(seqs[i])):
                if seqs[i][j] not in neighbors:
                    neighbors[seqs[i][j]] = set()
        
            
        for i in range(len(seqs)):
            for j in range(len(seqs[i]) - 1):
                froms = seqs[i][j]
                to = seqs[i][j + 1]
                neighbors[froms].add(to)
#             for key, value in neighbors.items():
#                 print(key, value)
        return neighbors 

    def get_indegree(self, graph):
        indegree = {x:0 for x in graph}
        for node in graph: 
            for neighbor in graph[node]:
                indegree[neighbor] += 1
        return indegree
            



    
    def topological_sort(self, neighbors, indegree):
        zero_indegree = deque([x for x in indegree if indegree[x] == 0])
        if len(zero_indegree) > 1:
            return None 
        
        result = []
       # print(zero_indegree)
        while zero_indegree:
            if len(zero_indegree) > 1:
                return None 
            cur_node = zero_indegree.popleft()
            result.append(cur_node)
            for neighbor in neighbors[cur_node]:
                indegree[neighbor] -= 1 
                if (indegree[neighbor] == 0):
                    zero_indegree.append(neighbor)
#        print(result)
        return result 
                    
            
            
```


```python
p = Solution()
p.sequenceReconstruction([5,3,2,4,1], [[5,3,2,4],[4,1],[1],[3],[2,4], [1000000000]])
```




    False




```python
# Slightly optimization of the above code with setdefault in build_graph
from collections import deque 
class Solution:
    """
    @param org: a permutation of the integers from 1 to n
    @param seqs: a list of sequences
    @return: true if it can be reconstructed only one or false
    """
    def sequenceReconstruction(self, org, seqs):
        if len(org) == 0 and len(seqs[0]) == 0:
            return True
            
        if len(org) == 1 and len(seqs) == 1:
            return org == seqs[0]
        # dictionary for neighbors and indegrees 
        graph = self.build_graph(seqs)
        # print(graph)

        indegrees = self.get_indegree(graph)
        # print(indegrees)
        # topological sorting 
        return self.topological_sort(graph, indegrees) == org 

    def build_graph(self, seqs):
        graph = {}
        for i in range(len(seqs)):
            for j in range(len(seqs[i]) - 1):
                graph.setdefault(seqs[i][j], set())
                graph[seqs[i][j]].add(seqs[i][j + 1])
            if len(seqs[i]) > 0:
                graph.setdefault(seqs[i][len(seqs[i]) - 1], set())
        return graph 
    
    def get_indegree(self, graph):
        indegrees = {x:0 for x in graph}
        for node in graph:
            for neighbor in graph[node]:
                indegrees[neighbor] += 1 
        return indegrees
        
    def topological_sort(self, graph, indegrees):
        queue = deque([x for x in indegrees if indegrees[x] == 0])
        results = []
        while queue:
            if len(queue) > 1:
                # no way to get a unique sorting 
                return results
            cur_val = queue.popleft()
            results.append(cur_val)
            for neighbor in graph[cur_val]:
                indegrees[neighbor] -= 1 
                if indegrees[neighbor] == 0:
                    queue.append(neighbor)
        return results
            
```

120 -  Word Ladder

Given two words (start and end), and a dictionary, find the length of shortest transformation sequence from start to end, such that:

Only one letter can be changed at a time

Each intermediate word must exist in the dictionary


Example: 
    
Given:
    
start = "hit"

end = "cog"

dict = ["hot","dot","dog","lot","log"]

As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",

return its length 5.


* BFS 


```python
# Python 1: use a queue and a set (visited), use a steps counter to count the steps 
from collections import deque 
class Solution:
    """
    @param: start: a string
    @param: end: a string
    @param: dict: a set of string
    @return: An integer
    """
    def ladderLength(self, start, end, dict):
        dict.add(end)
        queue = deque([start])
        visited = set([start])  # use set can avoid the time limit problem , set is implemented by hash table O(1)
        steps = 0
        while queue:
            steps += 1 
            for _ in range(len(queue)):
                cur_node = queue.popleft()
                #print(cur_node)
                if cur_node == end:
                    return steps
                new_words = self.get_next_words(cur_node, dict)
                print(new_words)
                for word in new_words:
                    if  word in visited:
                        continue
                        
                    queue.append(word)
                    visited.add(word)
        return 0
                        
        
        
    def get_next_words(self, word, dict):
        new_words = set()
        
        for i in range(len(word)):
            for char in 'abcdefghijklmnopqrstuvwxyz':
                new = word[0:i] + char + word[(i+1):]
                if new in dict:
                    new_words.add(new)
        return list(new_words)

```


```python
# Python 2: use queue and a dictionary to record each word as key and the corresponding distance to start as value
from collections import deque 
class Solution:
    """
    @param: start: a string
    @param: end: a string
    @param: dict: a set of string
    @return: An integer
    """
    def ladderLength(self, start, end, dict):
        if not dict or not start or not end:
            return 0 
        # add end to dict 
        dict.add(end)
        d_to_start = {start:1}
        queue = deque([start])
        while queue:
            cur_word = queue.popleft()
            if cur_word == end:
                return d_to_start[cur_word]
            next_words = self.get_next_words(cur_word, dict)
            for word in next_words:
                if word not in d_to_start:
                    d_to_start[word] = d_to_start[cur_word] + 1
                    queue.append(word)
        return 0 
    
    def get_next_words(self, word, dict):
        words = set()
        for i in range(len(word)):
            for char in 'abcdefghijklmnopqrstuvwxyz':
                try_word = word[0:i] + char + word[i + 1:]
                if try_word in dict:
                    words.add(try_word)
        return words 
```


```python
p = Solution()
p.ladderLength('hit', 'cog', set(["hot","dot","dog","lot","log"]))
```

    ['hot']
    ['hot', 'dot', 'lot']
    ['hot', 'dot', 'lot', 'dog']
    ['hot', 'dot', 'log', 'lot']
    ['dog', 'cog', 'dot', 'log']
    ['dog', 'cog', 'log', 'lot']





    5



242 - Convert Binary Tree to Linked Lists by Depth

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth `D`, you'll have `D` linked lists).
                                                                                                     
                                                                                                     
Example: 

Input: {1,2,3,4}

Output: 

    [
        1->null,
        2->3->null,
        4->null
    ]


```python
# Python 1: BFS 
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        this.val = val
        this.left, this.right = None, None
Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
"""
from collections import deque 

class Solution:
    # @param {TreeNode} root the root of binary tree
    # @return {ListNode[]} a lists of linked list
    def binaryTreeToLists(self, root):
        if not root:
            return []
        results = []
        queue = deque([root])
        while queue:
            dummy_node = ListNode(-1)
            prev_node = dummy_node

            size = len(queue)

            for _ in range(size):
                cur_node = queue.popleft()
                list_node = ListNode(cur_node.val)
                prev_node.next = list_node
                prev_node = list_node
                if cur_node.left:
                    queue.append(cur_node.left)
                if cur_node.right:
                    queue.append(cur_node.right)
            results.append(dummy_node.next)
        return results
                
```


```python
# Python 2: DFS 
class Solution:
    # @param {TreeNode} root the root of binary tree
    # @return {ListNode[]} a lists of linked list
    def binaryTreeToLists(self, root):
        # Write your code here
        result = []
        self.dfs(root, 1, result)
        return result

    def dfs(self, root, depth, result):
        if root is None:
            return
        node = ListNode(root.val)
        if len(result) < depth:
            result.append(node)
        else:
            node.next = result[depth-1]
            result[depth-1] = node
        
        self.dfs(root.right, depth + 1, result)
        self.dfs(root.left, depth + 1, result)
```

618 - Search Graph Nodes

Given a undirected graph, a node and a target, return the nearest node to given node which value of it is target, return NULL if you can't find.

There is a mapping store the nodes' values in the given parameters.


```python
Example: 
    
Input:
    
Graph: {1,2,3,4#2,1,3#3,1#4,1,5#5,4}
 
Values: [3,4,5,50,50]
 
node: 1
 
target: 50
 
Output:
4
        
Explanation:
2------3  5
 \     |  | 
  \    |  |
   \   |  |
    \  |  |
      1 --4
Give a node 1, target is 50

there a hash named values which is [3,4,10,50,50], represent:
        
Value of node 1 is 3
Value of node 2 is 4
Value of node 3 is 10
Value of node 4 is 50
Value of node 5 is 50

Return node 4
```


```python
"""
Definition for a undirected graph node
class UndirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []
"""

from collections import deque 

class Solution:
    """
    @param: graph: a list of Undirected graph node
    @param: values: a hash mapping, <UndirectedGraphNode, (int)value>
    @param: node: an Undirected graph node
    @param: target: An integer
    @return: a node
    """
    def searchNode(self, graph, values, node, target):        
        # find the nodes with its value == target 
        ends = [nd for nd in values if values[nd] == target]
        # corner case 
        if node in ends:
            return node 
        
        queue = deque([node])
        while queue:
            cur_node = queue.popleft()
            if cur_node in ends:
                return cur_node 
            for neighbor in cur_node.neighbors:
                queue.append(neighbor)
        return None

```

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
a = 'abcdefg'
a.find('ef')
```




    4




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


```python
Solution().minLength("abcabd", ["ab","abcd"])
```

    abcabd
    cabd
    abcd
    cd
    





    0



531 - Six Degrees

Six degrees of separation is the theory that everyone and everything is six or fewer steps away, by way of introduction, from any other person in the world, so that a chain of "a friend of a friend" statements can be made to connect any two people in a maximum of six steps.

Given a friendship relations, find the degrees of two people, return -1 if they can not been connected by friends of friends.

Input: {1,2,3#2,1,4#3,1,4#4,2,3} and s = 1, t = 4 

Output: 2

Explanation:

    1------2-----4
     \          /
      \        /
       \--3--/


```python
"""
Definition for Undirected graph node
class UndirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []
"""

from collections import deque 

class Solution:
    """
    @param: graph: a list of Undirected graph node
    @param: s: Undirected graph node
    @param: t: Undirected graph nodes
    @return: an integer
    """
    def sixDegrees(self, graph, s, t):
        if not graph or not s or not t:
            return -1 
        queue = deque([s])
        d_to_s = {s:0}
        while queue:
            node = queue.popleft()
            if node == t:
                return d_to_s[node]
            
            for neighbor in node.neighbors:
                if neighbor not in d_to_s:
                    d_to_s[neighbor] = d_to_s[node] + 1 
                    queue.append(neighbor)
        return -1
        

```

178 - Graph Valid Tree

Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

Input: 

n = 5, edges = [[0, 1], [0, 2], [0, 3], [1, 4]]

Output: true.


* Union find 

* BFS, DFS


```python
满足三个条件中的两个即为树：

1. 连通

2.  n - 1 条边

3. 无环
```


```python
# Python 1: BFS, check condition 1 and 2 
from collections import deque

class Solution:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """
    def validTree(self, n, edges):
        if not n or n == 0:
            return False 
        
        # one single node n = 1, edges = [] is also a tree 
        
        
        # condition 2 
        if len(edges) != n - 1:
            return False 
        
        # condition 1, check if one can traverse all nodes from one node 
        queue = deque([0])
        visited = set()
        while queue:
            node = queue.popleft()
            visited.add(node)
            for nd1, nd2 in edges:
                if nd1 == node and nd2 not in visited:
                    queue.append(nd2)
                if nd2 == node and nd1 not in visited:
                    queue.append(nd1)
        return len(visited) == n
            
        
        

```


```python
# Python 2: union find, check condition 2 and 3 
class Solution:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """
    def validTree(self, n, edges):
        if not n or n == 0:
            return False 
        
        # one single node n = 1, edges = [] is also a tree 
        
        
        # condition 2 
        if len(edges) != n - 1:
            return False 
        
        # condition 3
        self.father = {i:i for i in range(n)}
        self.size = n 

        for nd1, nd2 in edges:
            self.union(nd1, nd2)
            
        return self.size == 1
        
    def union(self, a, b):
        root_a = self.find(a)
        root_b = self.find(b)
        if root_a != root_b:
            self.size -= 1
            self.father[root_a] = root_b
        
    def find(self, node):
        path = []
        while node != self.father[node]:
            path.append(node)
            node = self.father[node]
            
        for n in path:
            self.father[n] = node
            
        return node
        
        

```

574 - Build Post Office

Given a 2D grid, each cell is either an house 1 or empty 0 (the number zero, one), find the place to build a post office, the distance that post office to all the house sum is smallest. Return the smallest distance. Return -1 if it is not possible.

You can pass through house and empty.

You only build post office on an empty.

The distance between house and the post office is Manhattan distance


Example: 

Input：[[0,1,0,0],[1,0,1,1],[0,1,0,0]]

Output： 6

Explanation：

Placing a post office at (1,1), the distance that post office to all the house sum is smallest.


```python
class Solution:
    """
    @param grid: a 2D grid
    @return: An integer
    """
    def shortestDistance(self, grid):

```
