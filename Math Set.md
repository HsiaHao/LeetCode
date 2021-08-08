## Math Set

### 697 Degree of an Array

Given a non-empty array of non-negative integers `nums`, the **degree** of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of `nums`, that has the same degree as `nums`.

 

**Example 1:**

```
Input: nums = [1,2,2,3,1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
```



Code:

```python
class Solution:
    def findShortestSubArray(self, nums: List[int]) -> int:
        numMap = {}
        degree = 0
        firstSeen = {}
        res = 1000
        for i in range(len(nums)):
            if nums[i] in numMap:
                numMap[nums[i]]+=1
            else:
                numMap[nums[i]] = 1
                firstSeen[nums[i]] = i
                å
            if numMap[nums[i]]>degree:
                degree = numMap[nums[i]]
                mx = numMap[nums[i]]
                res = i-firstSeen[nums[i]]+1
            elif numMap[nums[i]]==mx:
                res = min(res, i-firstSeen[nums[i]]+1)
        return res
```

---

## 766 Teoplitz Matrix

Given an `m x n` `matrix`, return *`true` if the matrix is Toeplitz. Otherwise, return `false`.*

A matrix is **Toeplitz** if every diagonal from top-left to bottom-right has the same elements.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/04/ex1.jpg)

```
Input: matrix = [[1,2,3,4],[5,1,2,3],[9,5,1,2]]
Output: true
Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
```

```python
class Solution:
    def __init__(self):
        self.n = 0
        self.m = 0
    def isToeplitzMatrix(self, matrix: List[List[int]]) -> bool:
        self.n, self.m = len(matrix), len(matrix[0])
        
        # check the first row
        for i in range(self.m):
            x, y = 1, i+1
            curr = matrix[0][i]
            if not self.check(x, y, matrix, curr):
                return False
            
        # check the first column
        for i in range(self.n):
            curr = matrix[i][0]
            x, y = i+1, 1
            if not self.check(x, y, matrix, curr):
                return False
        return True
            
    def check(self, x, y, matrix, curr)->bool:
        while x<self.n and y<self.m:
                if curr==matrix[x][y]:
                    x+=1
                    y+=1
                else:
                    return False
        return True
```

