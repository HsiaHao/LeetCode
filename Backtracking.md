## Backtracking:

#### #. 17 Letter Combination of a Phone Number

**Description**: Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

 

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```

 

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

**Idea:**

- DFS run through all the possible solutions
- backtracking: remove previous record

**Code:**

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        str_l = {'1':"", '2':"abc",'3':"def",'4':"ghi",'5':"jkl",'6':"mno",'7':"pqrs",'8':"tuv",'9':"wxyz" }
        res = []
        self.comb("", str_l, digits,res)
        return res
    
    def comb(self,prefix, str_l, digits, res):
        if len(prefix)==len(digits):
            res.append(prefix)
            return
        current_d = digits[len(prefix)]
        print(current_d)
        print(str_l[current_d])
        for c in str_l[current_d]:
            self.comb(prefix+c, str_l, digits,res)
            prefix.replace(c,'')
            
```

---



#### #. 93. Restore IP address

**Description**:

```
Output: ["1.1.1.1"]
```

**Example 4:**

```
Input: s = "010010"
Output: ["0.10.0.10","0.100.1.0"]
```

**Example 5:**

```
Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

**Constraints:**

- `0 <= s.length <= 3000`
- `s` consists of digits only.

**Idea:**

- dfs run through all possible solutions

**Code:**

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []
        ans = ""
        self.dfs(res, s, 0, 0, ans)
        return res
    
    def dfs(self, res, s, index, count, ans):
        if count==4 and len(ans)-1==len(s)+3:
            res.append(ans[:-1])
        if count>=4: return 
        
        for i in range(1,4):
            if index+i<=len(s):
                tmp= s[index:index+i]
                if int(tmp)>=0 and int(tmp)<=255 and ( len(tmp)>1 and tmp[0]!='0' or len(tmp)==1 ):
                    tmp+='.'
                    ans+=tmp
                    self.dfs(res, s, index+i, count+1, ans)
                    x = len(tmp)
                    ans = ans[:len(ans)-x]
```

---

#### #. Word Search

**Description**:Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Note:** There will be some test cases with a `board` or a `word` larger than constraints to test if your solution is using pruning.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

 

**Constraints:**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consists of only lowercase and uppercase English letters.

**Idea:**

- DFS search all the possible combination from (i,j)

**Code:**

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        n = len(board)
        m = len(board[0])
        isVisited = [[False for i in range(m)] for j in range(n)]

        for i in range(n):
            for j in range(m):
                if self.dfs(board, word, isVisited, i, j, 0):
                    return True
        return False
    
    def dfs(self, board, word, isVisited, i, j, index):
        if index==len(word): return True
        
        if i<0 or i>=len(board) or j<0 or j>=len(board[0]) or index>=len(word) or board[i][j]!=word[index] or isVisited[i][j]:
            return False
        isVisited[i][j]=True
        
        for d in [(1,0),(0,-1),(-1,0),(0,1)]:
            if self.dfs(board, word, isVisited, i+d[0], j+d[1], index+1):
                return True
        
        isVisited[i][j]=False
        return False
```

---

#### #. 257 Binary Tree Paths

**Description**:Given the `root` of a binary tree, return *all root-to-leaf paths in **any order***.

A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

**Example 2:**

```
Input: root = [1]
Output: ["1"]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `-100 <= Node.val <= 100`

**Idea:**

- DFS search for all possible paths
- Base Case: when not node.right and not node.left

**Code:**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        res = []
        self.dfs(root, res, str(root.val))
        return res
    
    def dfs(self, node, res, path):
        if not node.right and not node.left:
            res.append(path)
            return
        tmp = path
        if node.right:
            path = path+"->"+str(node.right.val)
            self.dfs(node.right, res, path)
        
        path = tmp
        
        if node.left:
            path = path+"->"+str(node.left.val)
            self.dfs(node.left, res,  path)
        
        path = tmp
```

---

#### #. 46 Permutation

**Description**:

Given an array `nums` of distinct integers, return *all the possible permutations*. You can return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

 

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

**NOTE** : 

- When passing a value, we should not use list.append() to pass
- -> it will return None
- Use list +[a], it will return a list type
- do not pass a list to dfs
- -> it will pass the adress and it will change all the values once one of them is changed.
- return list[:] will return a new list, not the address of the list

**Code:**

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        visited = [False for i in range(len(nums))]
        self.dfs(nums, res, visited, [])
        return res
    
    def dfs(self, nums, res, visited, sub):
        if len(sub)==len(nums):
            res.append(sub)
            return
        for i in range(len(nums)):
            if not visited[i]:
                visited[i] = True
                self.dfs(nums, res, visited ,sub+[nums[i]])
                visited[i] = False
```

---

#### #. 47 Permutation || -> Duplicate elements

**Description**:Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.*

 

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**Idea:**

- Use counter to prevent use duplicates

**Code:**

```python
from collections import Counter
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res = []
        visited = [False for i in range(len(nums))]
        self.dfs(Counter(nums),nums, res, [], visited)
        return res
    
    def dfs(self, counter, nums, res, sub, visited):
        if len(nums)==len(sub):
            if not sub in res:
                res.append(sub[:])
            return
        for i in counter:
            if counter[i]>0:
                counter[i]-=1
                sub.append(i)
                self.dfs(counter, nums, res, sub, visited)
                sub.pop()
                counter[i]+=1
```

---

#### #. 77 Combinations

**Description**:

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.

You may return the answer in **any order**.

 

**Example 1:**

```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**Idea:**

- shrink the range of nums to prevent duplicate elements
- Do not need visited to track the path
- since we have shrink the nums space already

**Code:**

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        nums = [i for i in range(1,n+1)]
        self.dfs(res, nums, k, [])
        return res
    
    def dfs(self, res, nums, k, sub):
        if len(sub)==k:
            res.append(sub[:])
            return
        
        for i in range(len(nums)):
            sub.append(nums[i])
            self.dfs(res, nums[i+1:], k, sub)
            sub.pop()
```

---

#### #. 39 Combination Sum

**Description**:

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

 

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Idea:**

- smaller the space for dfs
- ->candidates[i:] 
- cannot choose the previous one to avoid dupicates 

**Code:**

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        self.dfs(candidates, target, 0, [], res)
        return res
    
    def dfs(self, candidates, target, curr_sum, sub, res):
        if curr_sum==target:
            res.append(sub[:])
        if curr_sum>=target:
            return
        
        for i in range(len(candidates)):
            sub.append(candidates[i])
            self.dfs(candidates[i:], target, curr_sum+candidates[i], sub, res)
            sub.pop()
```

---

#### #. 40 Combination Sum II

**Description**:

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

 

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Idea:**

- Sort the array first
- then if we check the same element as last run, we can skip ot to avoid duplicates

**Code:**

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        res = []
        self.dfs(candidates, target, res, [], 0, -100000)
        return res
    
    def dfs(self, candidates, target, res, sub, curr_sum, last):
        if curr_sum==target:
            res.append(sub[:])
        if curr_sum>target:
            return
        for i in range(len(candidates)):
            if candidates[i]!=last:
                sub.append(candidates[i])
                curr_sum+=candidates[i]
                self.dfs(candidates[i+1:], target, res, sub, curr_sum, last)
                last = candidates[i]
                curr_sum-=candidates[i]
                sub.pop()
```

---

#### #. 216 Combination Sum III

**Description**:

Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return *a list of all possible valid combinations*. The list must not contain the same combination twice, and the combinations may be returned in any order.

 

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```

**Idea:**

- check all possible solution by dfs
- smaller the range by pass i+1 to next recursion
- stop when find the answer or  impossible to find

**Code:**

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        self.dfs(1 ,k, n, res, [])
        return res
    
    def dfs(self, begin, k, n, res, tmp):
        if len(tmp)==k and sum(tmp)==n:
            res.append(tmp[:])
            return
        if len(tmp)>k or sum(tmp)>n or begin >=10:
            return
        
        for i in range(begin,10):
            tmp.append(i)
            self.dfs(i+1 ,k, n, res, tmp)
            tmp.pop()
```

---

#### #. 78 Subsets

**Description**:

Given an integer array `nums` of **unique** elements, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Idea:**

- dfs store all the possible combination
- pass nums[i+1:] to prevent duplicates set

**Code:**

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.dfs(res, nums, [])
        return res
    
    def dfs(self, res, nums, temp):
        res.append(temp[:])
        if len(nums)==0:
            return
        
        for i in range(len(nums)):
            temp.append(nums[i])
            self.dfs(res, nums[i+1:], temp)
            temp.pop()
            
```

---

#### #. Title 

**Description**:

**Idea:**

**Code:**

```python

```

---

