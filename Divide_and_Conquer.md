

## Divide and Conquer 

### 1. #241 Different Ways to Add Parentheses

**Description:**

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.

**Example 1:**

```
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```

**Example 2:**

```
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

**Idea:**

- Lamda function with dictionary
- find the operation '+, -, *'
- calculate the left part s[:i] and right part s[i+1:]
- return all the possible combinations
- calculate all the combination of left and right
- return ..

**Code**

```python
class Solution:
    def diffWaysToCompute(self, input: str) -> List[int]:
        ops = {'+': lambda x, y: x + y,
           '-': lambda x, y: x - y,
           '*': lambda x, y: x * y}
        def ways(s):
            ans = []
            for i in range(len(s)):
                if s[i] in ['+','-','*']:
                    l_dfs = ways(s[:i])
                    r_dfs = ways(s[i+1:])
                    for j in range(len(l_dfs)):
                        for k in range(len(r_dfs)):
                            ans.append(ops[s[i]](l_dfs[j], r_dfs[k]))
            if not ans:
                ans.append(int(s))
            return ans
        
        return ways(input)
                        
```

### 2. #95. Unique Binary Search Tree II

**Description:**

Given an integer `n`, return *all the structurally unique **BST'**s (binary search trees), which has exactly* `n` *nodes of unique values from* `1` *to* `n`. Return the answer in **any order**.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
Input: n = 3
Output: [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

**Example 2:**

```
Input: n = 1
Output: [[1]]
```



**Idea:**

- Divide the problem into small problems
  - Choose a head
  - Find all possible combination of the left part
  - Find all possible combination of the right part
  - (repeat all numbers)
- Ex: if n=3 [1, 2, 3]
  - Let 1 be the head, right part [2, 3] and left part: None
  - Let 2 be the head, right part[2] and left part: [3]
  - Let 3 be the head, right [1, 2], left: None

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:

        def ways(a, b):
            res = []
            if b-a<0:
                return [None]
            for i in range(a,b+1):
                l = ways(a,i-1)
                r = ways(i+1, b)
                for l_n in l:
                    for r_n in r:
                        head = TreeNode(i)
                        head.left = l_n
                        head.right = r_n
                        res.append(head)
            return res
        return ways(1, n)
```

Time complexity: O(3^n)

Space complexity: O(3^n)

**#Improve: Dynamic Programming**

