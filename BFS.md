## BFS

![截圖 2021-03-11 上午3.07.30](/Users/xiayinghao/Desktop/ScreenCut/截圖 2021-03-11 上午3.07.30.png)

- Concept:
  - 走完所有路的第i步
  - 找出最短路近
    - 如果目標很深->耗時
  - **Queue!!**
  - Usually is not BFS
  - **Loop+Queue**
    - Collections.deque
    - Queue.popleft()
  - Queue-> FIFO
- Problem Scope
  - Level Order

#### 0111. Minumum Depth of Binary Tree

- Idea:
  - which step?
    - the stepof previous level+1
    - The first level is 1
  - How to use BFS?
    - end when find the answer(minimum steps)
    - Queue -> append(root, 1)
    - Pop from Queue
    - Check whether stop 
      - if yes: return
      - if not: append new () to Queue
  - Collections.deque()
    - q = collections.deque([]) //**put a list in it**
    - Duque.append()
    - Duque.popleft()

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        q = deque([(root,1)])
        # q = q.append()
        while q:
            node, dep = q.popleft()
            print( node.val, dep)
            if not node.left and not node.right:
                return dep
            else:
                if node.left:
                    q.append((node.left, dep+1))
                if node.right:
                    q.append((node.right, dep+1))
        return -1
            
```

#### #1091. Shortest Path in Binary Matrix

**Description**:

Given an `n x n` binary matrix `grid`, return *the length of the shortest **clear path** in the matrix*. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

- All the visited cells of the path are `0`.
- All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

**Idea:**

- BFS
  - Collections.deque + while loop
  - check 8 directions 
  - set the box have been checked to 1 for preventing checking that again

**Code:**

```python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        if not grid: return 0
        if grid[0][0]==1: return -1
        q = collections.deque([])
        q.append(((0,0),1))
        dirs = [ (1,0), (0,1), (1,-1), (1,1), (-1,0), (0,-1), (-1,-1), (-1,1) ]
        n = len(grid)
        while q:
            coord, step = q.popleft()
            if coord[0]==n-1 and coord[1]==n-1:
                return step
            for dir in dirs:
                i = coord[0]+dir[0]
                j = coord[1]+dir[1]
                if 0<=i and i<n and j>=0 and j<n:
                    if grid[i][j]==0:
                        q.append( ( (i,j), step+1 ) )
                        grid[i][j]=1
        return -1   
```

---

#### #. 279 Perfect Squares

**Description**:

Given an integer `n`, return *the least number of perfect square numbers that sum to* `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Example 1:**

```
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
```

**Example 2:**

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

**Idea:**

- BFS
- Queue -> (n, step) 
  - n: current number
  - Step: how many step?
- isVisited:
  - 走過不要再走

**Code:**

```python
class Solution:
    def numSquares(self, n: int) -> int:
        square_nums = []
        for i in range(n+1):
            square_nums.append(i**2)
            
        isVisited = [False]*(n+1)
        q = collections.deque()
        q.append((n, 0))
        isVisited[n] = True
        while q:
            n, step = q.popleft()
            if n==0:
                return step
            for squar in square_nums:
                if squar>n:
                    break
                if not isVisited[n-squar]:
                    q.append((n - squar, step+1))
                    isVisited[n-squar] = True
        return -1
```

---

#### #. 127 Word Ladder

**Description**:

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words such that:

- The first word in the sequence is `beginWord`.
- The last word in the sequence is `endWord`.
- Only one letter is different between each adjacent pair of words in the sequence.
- Every word in the sequence is in `wordList`.

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the **number of words** in the **shortest transformation sequence** from* `beginWord` *to* `endWord`*, or* `0` *if no such sequence exists.*

**Idea:**

- BFS:
  - store all the possible word for one step
  - remind the check method!!
  - if check word by word will cause time limit 

**Code:**

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        wordList = set(wordList)
        isVisited = [False]*len(wordList)
        s = beginWord
        e = endWord
        q = collections.deque()
        q.append((s,0))
        while q:
            curr, step = q.popleft()
            if curr==e: return step+1
            for i in range(len(curr)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    next_word = curr[:i] + c + curr[i+1:]
                    if next_word in wordList:
                        q.append((next_word, step+1))
                        wordList.remove(next_word)
        return 0
```

---



#### #. Title 

**Description**:

**Idea:**

**Code:**

```python

```

---

