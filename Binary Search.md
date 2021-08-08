## Binary Search

### Note:

Basic Code: -> the minimum l, such that the condition is true

```python
nums = [1,2,2,2,4,4,5]
target = 2
l = 0
r = len(nums) #Remember!!
while l<r:
    mid = (l+r)//2
    if nums[mid]<=target:
        l = mid+1
    else:
        r = mid-1
print('index: ',r)
```

- nums = [1,2,2,2,4,4,5]
- Lowerbound(x)
  - First index of i, such that nums[i] >=x
    - **if find the same value-> return the minimum i**
    - **else return the minimum index i, where nums[i]>value**
  - Ex: 
    - lower_bound(nums, 2) -> index 1
    - lower_bound(nums, 3) -> index 4 
- Upperbound(x)
  - First index of i, such that nums[i]>x
    - **if find the value, return the minimum i such that nums[i]>x**
    - **return the index len(nums) when all the elements are smaller than the value**
  - Ex:
    - Upper_bound(nums, 2) -> 4 
    - Upper_bound(nums, 5) -> 7 (does not exist)

```python
def lower_bound(nums, val, l, r):
  while l<r:
    mid = l+(r-l)//2
    if nums[mid]>=val: ##### include the lower bound ####
      r=m								### new range [l, m)
    else:
      l = m+1						### new range [m+1, r)
  return l
```

```python
def upper_bound(nums, val, l, r):
  while l<r:
    mid = l+(r-l)//2
    if nums[mid]>val: ##### not include the lower bound ####
      r=m							### new range [l, m)
    else:
      l = m+1					### new range [m+1, r)
  return l
```



### 6. #69. Sqrt(x)
**Description**:

Given a non-negative integer `x`, compute and return *the square root of* `x`.Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

**Example**:

```
Input: x = 4
Output: 2
```

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

**Key Points:**

- Binary Search
  - **Remember** 
    - While h>=l (must have equal)
    - mid = (l+h)//2 #get middle with integer 
    - l = mid+1
    - h = mid-1

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x==0: return 0
        l = 0
        h = x
        while h>=l:
            mid = (l+h)//2
            if mid*mid<=x and (mid+1)*(mid+1)>x:
                return mid
            elif mid*mid>x:
                h = mid-1
            else:
                l = mid+1
        return False
                
            
```

### 1. #744. Find Smallest Letter Greater Than Target

**Description**:

Given a list of sorted characters `letters` containing only lowercase letters, and given a target letter `target`, find the smallest element in the list that is larger than the given target.

**Example**:

```
Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"
```

**Key Points:**

- Binary Search:

**Bad Version**  

```python
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        l = 0
        r = len(letters)-1
        while l<=r:
            mid = (l+r)//2
            print(mid)
            if ord(target)>ord(letters[mid]):
                l = mid+1
            elif ord(target)==ord(letters[mid]):
                if (mid+1)>=len(letters):
                    return letters[0]
                if ord(target)<ord(letters[mid+1]):
                    return letters[mid+1]
                l=mid+1
            else:
                if ord(target)>=ord(letters[mid-1]):
                    return letters[mid]
                r = mid-1
        if r==-1:
            return letters[0]
        if r==len(letters)-1:
            return letters[0]
        else:
            return letters[r]
                    
```

**Good Version**

Binary search -> a position for inserting a number! 

- deal with the special cases first!

- Ex: 1,3,5,7,9 -> find 2 -> index 1
- Ex: 1,2,5,7,9 -> find 2 -> index 1
- Ex: 1,2,2,7,9 -> find 2 -> index 2
- For this problem, we want the the one is bigger than target
- Thus, return **letters[r+1]**

```python
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        l = 0
        r = len(letters)-1
        if ord(target)<ord(letters[0]):
            return letters[0]
        if ord(target)>=ord(letters[-1]):
            return letters[0]
        
        while l<=r:
            mid = (l+r)//2
            print(mid)
            if ord(target)>=ord(letters[mid]):
                l = mid+1
            else:
                r = mid-1
        return letters[r+1]
```



### 1. #540. Single Element in a Sorted Array

**Description**:

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

**Example**:

**Example 1:**

```
Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2
```

**Example 2:**

```
Input: nums = [3,3,7,7,10,11,11]
Output: 10
```

**Key Points:**

- Check the length of the subarray:
  - if length%2==0 -> even number-> no single value-> ababdon
  - Else-> single value is in there!-> keep the subset and abandon the other side

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        if len(nums)==1: return nums[0]
        l = 0
        h = len(nums)
        n = len(nums)
        while l<=h:
            mid = (l+h)//2
            if mid==n-1: return nums[n-1]
            if nums[mid]==nums[mid+1]:
                if (n-mid+2)%2==0:
                    #keep left
                    h = mid-1
                else:
                    #keep right
                    l = mid+2
            else:
                if nums[mid]!=nums[mid-1]:
                    return nums[mid]
                else:
                    if (mid+1)%2==0:
                        l= mid+1
                    else:
                        h = mid-1
        return nums[r]
```

### 1. #278. First Bad Version

**Description**:

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example**:

**Example 1:**

```
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

**Example 2:**

```
Input: n = 1, bad = 1
Output: 1
```

**Key Points:**

```python
class Solution:
    def firstBadVersion(self, n):
        if isBadVersion(1)==True: return 1
        l = 0
        r = n
        while l<=r:
            mid = (l+r)//2
            if isBadVersion(mid):
                if not isBadVersion(mid-1):
                    return mid
                else:
                    r = mid-1
            else:
                l = mid+1
        return r
```

### 1. #153. Find Minimum in Rotated Sorted Array 

**Description**:

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums`, return *the minimum element of this array*.

**Example**:

**Example 1:**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Key Points:**

- If the array is rotated, nums[mid]>nums[r]
- -> abandon left part when nums[mid]>nums[r]
- -> abandon right part otherwise
- **Note** When abanding left part; l = mid, since nums[mid] could be the answer.

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        if nums[-1]>nums[0]: return nums[0]
        l = 0
        r = len(nums)-1
        while l<r:
            mid = (l+r)//2
            print(mid)
            if nums[mid]<nums[0]:
                r = mid
            else:
                l = mid+1
        return nums[l]
```

### 1. #34. Find First and Last Position of Element in Sorted Array

**Description**:

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

**Follow up:** Could you write an algorithm with `O(log n)` runtime complexity?

**Example**:

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]
```

**Key Points:**

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if len(nums)==0:
            return [-1,-1]
        if len(nums)==1:
            if nums[0]==target:
                return [0,0]
            else:
                return [-1,-1]
        l, r = 0, len(nums)-1
        first=-1
        # Find first lower_bound()
        while l<r:
            mid = (l+r)//2
            if nums[mid]>=target:
                r = mid
                if target==nums[mid]: first = mid
            else:
                l = mid+1
        if nums[l]==target:
            first=l
        else:
            first=-1
        
        # Find last  upper_bound
        l, r = 0, len(nums)
        last=-1
        while l<r:
            mid = l+(r-l)//2
            if nums[mid]>target:
                r = mid 
            else:
                l = mid+1
        print(l)
        if (l-1)>=0 and nums[l-1]==target:
            last=l-1
        else:
            last=-1 
        
        return [first, last]
            
            
```

