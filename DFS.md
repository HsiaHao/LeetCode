## DFS:

- **Backtracking**

#### #. 687 Longest Univalue Path

**Description**:

Given the `root` of a binary tree, return *the length of the longest path, where each node in the path has the same value*. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```
Input: root = [5,4,5,1,1,5]
Output: 2
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

```
Input: root = [1,4,5,4,4,5]
Output: 2
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-1000 <= Node.val <= 1000`
- The depth of the tree will not exceed `1000`.

**Idea:**

- for a node:
  - it can be the top one:
    - count l+r
  - or not top one:
    - return 1+max(l,r) to top one
- The problem can be reduced to 
  - l = count(root.right, root.val)
  - r = count(root.left, soot.val)

**Code:**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.mx = 0
    def longestUnivaluePath(self, root: TreeNode) -> int:
        if not root: return 0
        self.count(root, root.val)
        return self.mx
    
    def count(self, root, val):
        if not root: return 0
        l = self.count(root.right, root.val)
        r = self.count(root.left, root.val)
        if (l+r)>self.mx:
            self.mx = l+r
        if root.val==val:
            return 1+max(l,r)
        else:
            return 0
```

---

#### #. 695 Max Area of Island

**Description**:Given a non-empty 2D array `grid` of 0's and 1's, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

Given the above grid, return `6`. Note the answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

```
[[0,0,0,0,0,0,0,0]]
```

Given the above grid, return `0`.

**Note:** The length of each dimension in the given `grid` does not exceed 50.

**Idea:**

- DFS
- change visited 1 to 0
- dfs go through every sides

**Code:**

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        max_area = 0
        n = len(grid)
        m = len(grid[0])
        
        for i in range(n):
            for j in range(m):
                if grid[i][j]==1:
                    max_area = max(max_area, self.dfs(i,j,grid))
        return max_area
    
    def dfs(self, i, j, grid) -> int:
        if i<0 or i>len(grid)-1 or j<0 or j>len(grid[0])-1:
            return 0
        
        if grid[i][j]==0: return 0
        
        grid[i][j] = 0
        count = 1
        count+= self.dfs(i+1,j,grid)
        count+= self.dfs(i,j+1,grid)
        count+= self.dfs(i-1,j,grid)
        count+= self.dfs(i,j-1,grid)
        
        return count
```

---

#### #. 200 Number of Island

**Description**:Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

**Idea:**

- dfs
- remove an island by dfs
- remove 1-> 0

**Code:**

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        n = len(grid)
        m = len(grid[0])
        count = 0
        for i in range(n):
            for j in range(m):
                if grid[i][j] == "1":
                    count+=1
                    self.dfs(grid, i, j)
                    print('count')
        return count
    
    def dfs(self, grid, i, j):
        if i<0 or i>len(grid)-1 or j<0 or j>len(grid[0])-1 or grid[i][j]=="0":
            return 
        grid[i][j]="0"
        self.dfs(grid, i+1, j)
        self.dfs(grid, i, j+1)
        self.dfs(grid, i-1, j)
        self.dfs(grid, i, j-1)
        
```

---

#### #. 547 Number of Provinces

**Description**:There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return *the total number of **provinces***.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
```

 

**Constraints:**

- `1 <= n <= 200`
- `n == isConnected.length`
- `n == isConnected[i].length`
- `isConnected[i][j]` is `1` or `0`.
- `isConnected[i][i] == 1`
- `isConnected[i][j] == isConnected[j][i]`

**Idea:**

- Simple DFS:
  - iterate through all cities 0..n-1
  - dfs check all neighbor cities

**Code:**

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        isVisited = [False for i in range(len(isConnected))]
        count = 0
        for i in range(len(isConnected)):
            if not isVisited[i]:
                count+=1
                self.dfs(i, isConnected, isVisited)
        return count
    
    def dfs(self, i, C, V):
        if V[i]: return
        V[i] = True
        # find all the neighbors
        nei = []
        for j in range(len(C[0])):
            if C[i][j]==1:
                nei.append(j)
        for n in nei:
            self.dfs(n, C, V)        
        
```

---

#### #. 130 Surrounded Regions

**Description**:Given an `m x n` matrix `board` containing `'X'` and `'O'`, *capture all regions surrounded by* `'X'`.

A region is **captured** by flipping all `'O'`s into `'X'`s in that surrounded region.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

```
Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
Explanation: Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
```

**Example 2:**

```
Input: board = [["X"]]
Output: [["X"]]
```

 

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`.

**Idea:**

- DFS:
- check all remaining 'O' by dfs
- mark remaining 'O' as 'T'
- flip other 'O' to 'X'

**Code:**

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        n = len(board)
        m = len(board[0])
        #check four borders
        for i in range(n):
            if board[i][0] == 'O':
                self.dfs(board, i, 0)
            if board[i][m-1]=='O':
                self.dfs(board, i, m-1)
        for i in range(m):
            if board[0][i]=='O':

                self.dfs(board, 0, i)
            if board[n-1][i]=='O':
                print('get', n-1, i)
                self.dfs(board, n-1, i)
                
        for i in range(n):
            for j in range(m):
                if board[i][j]=='T':
                    board[i][j] = 'O'
                else:
                    board[i][j] = 'X'
        
    def dfs(self,board, i, j):
        if i<0 or i>=len(board) or j<0 or j>=len(board[0]) or board[i][j]=='X' or board[i][j]=='T': return 
        board[i][j]='T'
        self.dfs(board, i+1, j)
        self.dfs(board, i, j+1)
        self.dfs(board, i-1, j)
        self.dfs(board, i, j-1)
        
        
        
        
        
        
```

---

#### #. 417 Pacific Atlantic Water Flow

**Description**: 

Given an `m x n` matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

**Note:**

1. The order of returned grid coordinates does not matter.
2. Both *m* and *n* are less than 150.

 

**Example:**

```
Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).
```

**Idea:**

- Find all the point where can be reached from the Oceans
- If a point can be found both by pacific and Atlantic, append it to the answer

**Code:**

```python
class Solution:
    def pacificAtlantic(self, matrix: List[List[int]]) -> List[List[int]]:
        if len(matrix)==0: return []
        n = len(matrix)
        m = len(matrix[0])
        p = [[False for i in range(m)] for j in range(n)]
        a = [[False for i in range(m)] for j in range(n)]
        #check 
        for i in range(n):
             self.dfs(i, 0, p, matrix[i][0], matrix)
             self.dfs(i, m-1, a, matrix[i][m-1], matrix)
        for j in range(m):
             self.dfs(0, j, p, matrix[0][j], matrix)
             self.dfs(n-1, j, a, matrix[n-1][j], matrix)
        
        res = []
        for i in range(n):
             for j in range(m):
                if p[i][j]==True and a[i][j]==True:
                    res.append([i,j])
        return res
    ## x, y-> coordinate
    ## v: visited array
    ## h: height
    ## m: matrix
    def dfs(self, x, y, v, h, m):
        if x<0 or x>len(m)-1 or y<0 or y>len(m[0])-1: return
        if v[x][y]: return
        if h>m[x][y]: return 
        dir_4 = [(1,0),(0,1),(-1,0),(0,-1)]     
        v[x][y]=True
        for d in dir_4:
             self.dfs(x+d[0], y+d[1], v, m[x][y], m)
```

---

#### #. Title 

**Description**:

**Idea:**

**Code:**

```python

```

---

