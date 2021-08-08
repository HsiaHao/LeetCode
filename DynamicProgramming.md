

## Dynamic Programming 

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

- Dynamic Programming
- Min(dp[i], dp[squar-i]+1) (for all square numbers)
  - the best solution for i is find a square less than it and check 

**Code:**

```python
class Solution:
    def numSquares(self, n: int) -> int:
        s = int(sqrt(n))
        dp = [100000 for i in range(n+1)]
        
        square_nums = []
        for i in range(n+1):
            square_nums.append(i**2)
            
        dp[0] = 0
        for i in range(1,n+1):
            for squar in square_nums:
                if i<squar:
                    break
                dp[i] = min(dp[i], dp[i-squar]+1)
        return dp[n]
            
```

---

#### #. 70 Climbing Stairs

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

 

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Idea:**

- dp[n] = dp[n-1]+dp[n-2]
- For climbing to the n steps, the only two possible ways are climb one step from the n-1 step or climb 2 steps from the n-2 step.

**Code:** Space O(n) Time O(n)

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [0 for i in range(n+1)]
        dp[0], dp[1] = 1, 1
        for i in range(2,n+1):
            dp[i] = dp[i-2]+dp[i-1]
        return dp[n]
```

**Update Code:**Space O(1) Time O(n)

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        pre1, pre2 = 1, 1
        if n==0 or n==1:
            return 1
        for i in range(2, n+1):
            curr = pre1+pre2
            pre1 = pre2
            pre2 = curr
        return curr
```



---

#### #. 198 House Robber

**Description**:You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Idea:**

- dp[n] = max(dp[n-1], dp[n-2]+nums[n])
- at position n, we can rob the nth house and get the maximumm from dp[n-2]
- or we can just get max n-1 and ignore n

**Code:** Time: O(n) Space: O(n)

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n==1:
            return nums[0]
        dp = [0 for i in range(n)]
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2,n):
            dp[i] = max(dp[i-1], nums[i]+dp[i-2])
        
        return dp[n-1]
```

**Code:** Time: O(n) Space O(1)

**Note:**

* pre1 = pre2 before pre2 = curr
* curr = pre2 (if n==2 just return curr)
* for the last run, we can return pre2 or curr since they are the same
* However, if n==2, we will not jump input the for loop, whch means we will not have any value in curr. In this case, we just can return pre2 without any error.

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n==1:
            return nums[0]
        pre1, pre2 = nums[0], max(nums[0], nums[1])
        for i in range(2, n):
            curr = max(nums[i]+pre1, pre2)
            pre1 = pre2
            pre2 = curr
        return pre2
```

---

#### #. 213 House Robber II

**Description**:You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Idea:**

- as robber I, we can solve by DP
- Circle problem: choose the first one or not
- the solution is Max(nums[:n-1], nums[1:])
- 

**Code:**

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n==1:
            return nums[0]
        return max(self.help_rob(nums[:n-1]), self.help_rob(nums[1:]))
    
    def help_rob(self, nums)-> int:
        n = len(nums)
        if n==1:
            return nums[0]
        pre1, pre2 = nums[0], max(nums[0], nums[1])
        curr = pre2
        for i in range(2, n):
            curr = max(pre1+nums[i], pre2)
            pre1 = pre2
            pre2 = curr
        return curr
```

- However, using slicing is copying the entire array. It cost O(n) space.
- We can use index first and last to indicate the array we want to avoid wasting the space.
- When use DP technique, we can set the initial value in various way. We can start from dp[2] or dp[0].

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n==1:
            return nums[0]
        # the answer is either choose the first one or the last one
        return max(self.help(nums, 1, n), self.help(nums, 0, n-1))
    
    def help(self, nums, first, last):
        n = len(nums)
        if n==1:
            return nums[0]
        pre1, pre2 = 0, 0
        for idx in range(first, last):
            curr = max(nums[idx] + pre1, pre2)
            pre1 = pre2
            pre2 = curr
        return pre2
```

---

#### #. 64 Minimum Path Sum

**Description**:Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Idea:** the minimum path sum for grid_i_j is the min(grid_i-1_j, grid_i_j-1) + grid_i_j

**Code:**

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        row = len(grid)
        col = len(grid[0])
        
        #initial row values
        for i in range(1, row):
            grid[i][0] += grid[i-1][0]
        #initial colume values
        for i in range(1, col):
            grid[0][i] += grid[0][i-1]
        #calculate grid[i][j]
        for i in range(1, row):
            for j in range(1, col):
                grid[i][j] = min(grid[i-1][j], grid[i][j-1]) + grid[i][j]
        return grid[row-1][col-1]
```

---

#### #. 62. Unique Paths

**Description**:A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Idea:** The number of ways for grid_i_j is either from left or top

Thus, grid_i_j = grid_i-1_j+grid_i_j-1

**Code:**

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for i in range(n)] for j in range(m)]
        # initialize row values
        for i in range(m):
            dp[i][0] = 1
        # initialize colume values
        for i in range(n):
            dp[0][i] = 1
        # calculate dp[i][j] = dp[i-1][j]+dp[i][j-1]
        
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i-1][j]+dp[i][j-1]
        return dp[m-1][n-1]
```

---

#### #. 303. Range Sum Query - Immutable

**Description**:Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**Idea:** Create an dp array where dp[i] indicates the sum from nums[0] to nums[n]

When we need to calculate the sum of a subarray, we can return dp[right] - dp[left-1]

**Code:**

```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.dp = nums
        for i in range(1, len(nums)):
            self.dp[i] += self.dp[i-1]

    def sumRange(self, left: int, right: int) -> int:
        if left==0:
            return self.dp[right]
        return self.dp[right] - self.dp[left-1]


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(left,right)
```

---

#### #. 413. Arithmetic Slices

**Description**:An integer array is called arithmetic if it consists of **at least three elements** and if the difference between any two consecutive elements is the same.

- For example, `[1,3,5,7,9]`, `[7,7,7,7]`, and `[3,-1,-5,-9]` are arithmetic sequences.

Given an integer array `nums`, return *the number of arithmetic **subarrays** of* `nums`.

A **subarray** is a contiguous subsequence of the array.

**Idea:**

dp[i] indicates the number of arithmetic array which end with nums[i]

dp[i] = dp[i-1]+1

- 0, 1, 2 ,3, 4 ,5 ,6
- -> 0,1,2
- ->0,1,2,3 / 1,2,3
- ->0,1,2,3,4/1,2,3,4/2,3,4

**Code:**

```python
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        n = len(nums)
        if n<3:
            return 0
        dp = [0 for i in range(n)]
        for i in range(2, n):
            if nums[i] - nums[i-1]== nums[i-1]-nums[i-2]:
                dp[i] = dp[i-1]+1
        ans = 0
        for n in dp:
            ans+=n
        return ans
```

---

#### #. 343. Integer Break

**Description**:Given an integer `n`, break it into the sum of `k` **positive integers**, where `k >= 2`, and maximize the product of those integers.

Return *the maximum product you can get*.

**Idea:** 

- dp[i] is the maximum product for i
- for a value n, the maximum product we can get is j (1..n)(i-j)
- the value (i-j) can also be decomposed and the maximum product is dp[i-j]

**Code:** Time: O(n^2^)

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [0 for i in range(n+1)]
        dp[0], dp[1], dp[2] = 0, 1, 1
        for i in range(3, n+1):
            for j in range(1, i):
                dp[i] = max(dp[i], max(j*(i-j), j*dp[i-j]))
        return dp[n]
```

**Another solution**

- For n >6: the soluton becomes dp[n] = dp[n-3] *3
- This is because the value need to be decomposed into 3 base

**Code: ** TIme: O(n)

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [1 for i in range(n+1)]
        v = [0, 0, 1, 2, 4, 6, 9]
        if n<len(v):
            return v[n]
        for i in range(len(v)):
            dp[i] = v[i]
        for i in range(7, n+1):
            dp[i] = 3 * dp[i-3]
        return dp[n]
```

- Since we only need the previous three values from the dp table, we can reduce the Space to O(1)

---

#### #. 279. Perfect Squares

**Description**:Given an integer `n`, return *the least number of perfect square numbers that sum to* `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Idea:**

- dp[i] indicates the least number of perfect square for i
- for any value n, dp[n] is j^2^ *dp[n-j^2^]
- Try each j and find the minimum number of perfect squares

**Code:** Time O(n^2^) 

```python
import math
class Solution:
    def numSquares(self, n: int) -> int:
        if n<3:
            return n
        dp = [1000 for i in range(n+1)]
        dp[0], dp[1], dp[2] = 0, 1, 2
        for i in range(3,n+1):
            for j in range(int(math.sqrt(i)), 0, -1):
                count = 1 + dp[i-j**2]
                if count<dp[i]:
                    dp[i] = count
        return dp[n]
```



---

#### #. 91. Decode Ways

**Description**:

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

**Idea:**

- dp[i] indicates the number of decode ways
- for any value i, if s[i]!=0, we at least have dp[i-1] decode ways
- and if s[i-1:i+1] is between 09 and 26, we have extra dp[i-2] ways

**Code:** TIme: O(n)

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        n = len(s)
        dp = [0 for i in range(n+1)]
        
        if s[0]=='0':
            return 0
        
        dp[0] = 1
        for i in range(1, n+1):
            if s[i-1]!='0':
                dp[i] = dp[i-1]
            if i!=1 and s[i-2:i]<'27' and s[i-2:i]>'09':
                dp[i] += dp[i-2]
        return dp[n]         
```

---

#### #. 300. Longest Increasing Subsequence

**Description**:

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Idea:**

- dp[i] indicates the length of the longest subsequence from nums[:i] including nums[i]
- for any value i, we go through all the values in dp[:i-1]
- if we find a value j where nums[j]<nums[i]
- we know we can append nums[i] in this subsequence.
- Thus, the length of the subsequence becomes dp[j]+1
- we update dp[i] once we found a greater possible value
- Finally, we piclk the greatest number in dp as our answer.

**Code:** Time: O(n^2^)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [1 for i in range(n)]
        
        for i in range(1, n):
            for j in range(0, i):
                if nums[j]<nums[i]:
                    dp[i] = max(dp[i], 1+dp[j])
        return max(dp)
```

Update Code: Time O(nlogn)

 **idea**

- [花花](https://www.youtube.com/watch?v=l2rCz7skAlk)
- bisect_left -> binary search (lower bound)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = []
        for n in nums:
            i = bisect_left(dp, n)
            if i==len(dp):
                dp.append(n)
            else:
                dp[i] = n
        return len(dp)
```



---

#### #. 646. Maximum Length of Pair Chain

**Description**:

You are given an array of `n` pairs `pairs` where `pairs[i] = [lefti, righti]` and `lefti < righti`.

A pair `p2 = [c, d]` **follows** a pair `p1 = [a, b]` if `b < c`. A **chain** of pairs can be formed in this fashion.

Return *the length longest chain which can be formed*.

You do not need to use up all the given intervals. You can select pairs in any order.

**Idea:**

- dp[i] = the maximum length oh pair chain
- dp[i] = max(dp[i], dp[j]+1)
- if pairs $[i][0]$ >pairs$[j][1]$ , we know that it can form a pair chain whose length is dp[j]+1

**Code:** Time O(n^2^) -> Dynamic Programming

```python
from operator import itemgetter, attrgetter
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        pairs = sorted(pairs, key=itemgetter(1))
        dp = [1 for i in range(len(pairs))]
        for i in range(1, len(pairs)):
            for j in range(0, i):
                if pairs[i][0]>pairs[j][1]:
                    dp[i] = max(dp[i], dp[j]+1)
        return max(dp)
```

**Idea:** Greedy

- Earlist Finish Time -> O(n)

```python
from operator import itemgetter, attrgetter
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        # sorted by finish time
        pairs = sorted(pairs, key=itemgetter(1))
        # f: finish time
        f = pairs[0][1]
        sum = 1
        for pair in pairs:
            if pair[0]>f:
                sum+=1
                f = pair[1]
        return sum
```



---

#### #. 376. Wiggle Subsequence

**Description**:A **wiggle sequence** is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

- For example, `[1, 7, 4, 9, 2, 5]` is a **wiggle sequence** because the differences `(6, -3, 5, -7, 3)` alternate between positive and negative.
- In contrast, `[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.

A **subsequence** is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array `nums`, return *the length of the longest **wiggle subsequence** of* `nums`

**Idea:**

- DP works -> O(n^2^)
- Greedy faster -> O(n)
  - count all nums[i] - nums[i-1] store in dp[i]
  - if sign(sp[i]) != sign[dp[i-1]] -> count+=1
  - !! Special case: nums[i] - nums[i-1]=0: Ignore them

**Code:**

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        dp = [nums[i] - nums[i-1] for i in range(1, len(nums)) if nums[i]-nums[i-1]!=0]
        if not dp: return 1
        count = 2
        for i in range(1, len(dp)):
            if dp[i]>0 and dp[i-1]<0 or dp[i]<0 and dp[i-1]>0:
                count+=1
        return count
```

---

#### #.1143. Longest Common Subsequence

**Description**:Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Idea:**

- $dp[i][j]$ indicates the longest common subsequence of text1[0..i] and text2[0..j].
- $dp[i][j]$ = $dp[i-1][j-1]$ +1 if text1[i]==text2[j]
- else = max($dp[i-1][j]$, $dp[i][j-1]$) 

**Code:** Time O(n^2^)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        n = len(text1)
        m = len(text2)
        dp = [[0 for i in range(n+1)] for j in range(m+1)]
        
        for i in range(1, m+1):
            for j in range(1, n+1):
                if text2[i-1]==text1[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[m][n]
```

---

#### #. 416. Partition Equal Subset Sum

**Description**:Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Idea:**

- we know that if we can find a subset whose sum is sum(nums)/2, we know this array can be partitioned
- Target = sum(nums)//2
- $dp[i]$ is true if we can find the sum of a subarray is i
- if we can find dp[target] is true, return true
- initial dp[0] = false

**Code:**

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        target = sum(nums)
        if target%2!=0: return False
        target = target//2
        dp = [False for i in range(target+1)]
        dp[0] = True
        for n in nums:
            for i in range(target, n-1, -1):
                dp[i] = dp[i] or dp[i-n]
        return dp[target]
```

---

#### #. 494 Target Sum

**Description**:

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Idea:**

- DFS - Time Limit

**Code:**

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        def helper(index, count):
            if index==len(nums):
                if count==target:
                    return 1
                else:
                    return 0
            return helper(index+1, count+nums[index]) + helper(index+1, count-nums[index])
        return helper(0, 0)
```

**Idea2**: Dynamic Programming

- we assume we have two subarray positive, negative
- p = sum(positive)
- n = sum(negative)
- if p-n==target, wer find the answer
- we know p+n = sum
- p - n  = p - (sum-p) = 2p-sum = target
- p = (target+sum)/2
- Thus, if we can find a subset whose sum is (target+sum) / 2. that is one of the answer
- DP process:
  - $$dp[i][j]$$  = the total weight is j by using the first i items
  - $$dp[i][j]$$ =
    - (ignore item i when nums[i]>j)  $$dp[i-1][j]$$
    - (include item i when nums[i]<=j) $$dp[i-1][j] + dp[i-1][j-nums[i]]$$

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        n = len(nums)
        x = (sum(nums)+target)
        if x%2!=0:
            return 0
        else:
            x = x//2
        dp = [[0 for i in range(x+1)] for j in range(n+1)]
        # 0 weight can be formed by the empty subset
        for i in range(n+1):
            dp[i][0] = 1
            
        for i in range(1, n+1):
            for j in range(x+1):
                if nums[i-1]>j:
                    dp[i][j] = dp[i-1][j]
                elif nums[i-1]<=j:
                    dp[i][j] = dp[i-1][j] + dp[i-1][j-nums[i-1]]
        print(dp)
        return dp[-1][-1]
        
```



---

#### #. \474. Ones and Zeroes

**Description**:You are given an array of binary strings `strs` and two integers `m` and `n`.

Return *the size of the largest subset of `strs` such that there are **at most*** `m` `0`*'s and* `n` `1`*'s in the subset*.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

**Idea:**

- $$dp[i][j][k]$$: with first i strings, the largest number of subset such that there are at most j ones and k zeroes
- Initial: $$dp[0][j][k]$$ = 0
- $$dp[i][j][k]$$ = $$max (dp[i-1][j][k], dp[i-1][j-ones][k-zeros]+1)$$ 



**Code:** Time: $$O(len(strs)*mn)$$  Space: $$O(len(strs)*mn)$$ 

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [ [ [0 for i in range(m+1)] for j in range(n+1) ] for k in range(len(strs)+1) ]
        # dp[i][j][k]: i: first i strings / j: within j ones / k: with k zeroes
        
        for i in range(1, len(strs)+1):
            s = strs[i-1]
            ones, zeroes = 0, 0
            for c in s:
                if c=='1':
                    ones+=1
                if c=='0':
                    zeroes+=1
            for j in range(0, n+1):
                for k in range(0, m+1):
                    if ones<=j and zeroes<=k:
                        dp[i][j][k] = max(dp[i-1][j][k], dp[i-1][j-ones][k-zeroes]+1)
                    else:
                        dp[i][j][k] = dp[i-1][j][k]
        return dp[-1][-1][-1]
```

**Code update**:  Space: $$O(mn)$$ 

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [ [0 for i in range(m+1)] for j in range(n+1) ]
        # dp[i][j]: i ones / j zeroes
        
        for s in strs:
            ones, zeroes = 0, 0
            for c in s:
                if c=='1':
                    ones+=1
                if c=='0':
                    zeroes+=1
            for i in range(n, ones-1, -1):
                for j in range(m, zeroes-1, -1):
                    dp[i][j] = max(dp[i][j], dp[i-ones][j-zeroes]+1)
        return dp[-1][-1]
```



#### No. 322. Coin Change

#### Description:

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

#### Idea:

- $$dp[i]$$: the fewest number of coins such that the sum is i
- Initial: set all the values to amount+1 except dp[0] = 0

#### Code: Time: O(len(coins) * amount) Space: O(amount)

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        #dp[i]: the fewest amount of coins when amount is i
        dp = [amount+1 for i in range(amount+1)]
        dp[0] = 0
        
        for i in range(amount+1):
            for c in coins:
                if c<=i:
                    dp[i] = min(dp[i], dp[i-c]+1)
        return dp[-1] if dp[-1]!=amount+1 else -1
```

---

#### No. 518 Coin Change 2

#### Description:

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

#### Idea:

- $$dp[i][j]$$ : the number of combination that make up j bu using the first i coins
- $$dp[0][0]$$ = 0, other $$dp[0][j] = 1$$
- if $$j<coins[j], dp[i][j] = dp[i-1][j]$$
- else = $$dp[i][j-coins[i-1]] + dp[i-1][j]$$

#### Code: Time: Space

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # dp[i][j]: the number of combination that make up j by using the first i coins
        n = len(coins)
        dp = [[0 for i in range(amount+1)] for j in range(n+1)]
        dp[0][0] = 1
        
        for i in range(1, n+1):
            for j in range(0, amount+1):
                if j<coins[i-1]:
                    dp[i][j] = dp[i-1][j]
                else:
                    dp[i][j] = dp[i][j-coins[i-1]] + dp[i-1][j]
        return dp[-1][-1]
```

---

#### No. 

#### Idea:

#### Code: Time: Space

```python

```

---



