# Greedy Algorithm

### 1. #406 QUeue Reconstruction by Height(Medium)
**Description**:
You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

**Ex:**

    Input:
    [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]
    
    Output:
    [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]

**Key Point**
- Greedy Algorithm
    - sort height -> short to high (h)
    - if people have same height-> sort by k
- After sorting
    - insert the people in a list by k
    - the higher person will be inserted first
    - then, inserting short people in the list will not influence higher people
```python
class Solution:
    def take_second(elem):
        return elem[1]
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people = sorted(people, key = lambda x: (-x[0], x[1]))
        # height: big->small # numbers: samll to big
        # higher one will not infulence shorter one
        # numbers need to insert from the smallest
        res = []
        for p in people:
            res.insert(p[1], p)
        return res
```

**Note: The key parameter in sorted can choose the factor for sorting**. 

---

### 2. #121 Best Time To Buy and Sell Stock
**Description**
You are given an array prices where prices[i] is the price of a given stock on the ith day.
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

**Example**
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Key Points:**
- Greedy Algorithm
    - for loop throght every price in prices
    - if find a lower price, update lowerest_price
    - else count the diff (current_price) - (lowerst_price)
    - when finish, get the biggest diff
 - Ideas
    - only update lowerst without cosidering the prvious case
    - since if there is a lowest price, it is better to buy the current price than the previous one
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_p = 100000
        res = 0
        for p in prices:
            if (p-min_p)<=0:
                min_p = p
            else:
                x = p - min_p
                if x>res:
                    res = x
        return res
```
---
### 3. #121 Best Time to Buy and Sell Stock II (Easy)
**Description**
You are given an array prices for which the ith element is the price of a given stock on day i.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).


**Example**
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
**Key Points:**
- Greedy Algorithm
    - for loop through all the prices:
    - if the current price is smaller than the current_min: add the diff to the answer
    - else update the curr_min
    - return res
 - Ideas
    - sell when selling can earn
    - It will be optimal since you can buy that again (update the min_curr)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        last = prices[0]
        for p in prices:
            if p - last >0:
                res+=(p-last)
                last = p
            else:
                last = p
        return res
```
---
### 4. #605. Can Place Flowers
**Description**
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule.


**Example**
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true

Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
**Key Points:**
- Ideas: 
    - Iterate through all points
    - Let p be the point we are currently checking
    - if the two point before and after p are 0: set p=1
    - count the maximum places can be planted
    - if cnt>n: return True
```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        if len(flowerbed)==1:
            if flowerbed[0]==0:
                return n<=1
            else:
                return n==0
        pre = 0
        cnt=0
        i=0
        for i in range(0,len(flowerbed)-1):
            if pre==0 and flowerbed[i]==0 and flowerbed[i+1]==0:
                cnt +=1
                flowerbed[i]=1
                print(i, 'valid')
            pre = flowerbed[i]
        if pre==0 and flowerbed[i+1]==0:
            cnt+=1
        return cnt>=n
```
### 5. #392. Is Subsequence
**Description**
Given two strings s and t, check if s is a subsequence of t.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).


**Example**
Input: s = "abc", t = "ahbgdc"
Output: true
Input: s = "axc", t = "ahbgdc"
Output: false
**Key Points:**
- Ideas: 
    - compare all the characteristic and move the index of s
    - util the index of s == len(s)
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if len(s)==0: return True
        i=0
        for c in t:
            if c==s[i]:
                i+=1
            if i==len(s):
                return True
        return False
 
```
### 6. #665. None Decreasing Array
**Description**



**Example**

**Key Points:**
- there are two possible condition
- Case 1: 1,7,3,4
    - 7->3 is wrong
    - we need to change 7->1
- Case 2: 4,7,3,9
    - 7->3 is wrong
    - we cannot change 7->1
    - we can only change 3->7
- for case1, nums[i+1], whech is 3, is bigger that nums[i-1]
    - we want the numsber be as small as possible
    - we can change nums[i] to nums[i+1]-> makes it smaller but it is also bigger than nums[i-1]
- for case2, nums[i+1] is less than nums[i-1]
    - we cannot set nums[i] = nums[i+1], since it will make nums[i] smaller than nums[i-1]
    - Thus, we can only set nums[i+1] = nums[i]
```python
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        check = 0
        for i in range(len(nums)-1):
            if nums[i]>nums[i+1]:
                if i==0:
                    pre = 0
                else:
                    pre = nums[i-1]
                if check==0:
                    if pre>nums[i+1]:
                        nums[i+1] = nums[i]
                    else:
                        nums[i] = nums[i+1]
                    check=1
                else:
                    return False
        return True
                
        
 
```
### 6. #665. None Decreasing Array
**Description**



**Example**

**Key Points:**

```python

 
```
### 6. #52. Maximum Subarray (Easy)
**Description**
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
**Example**
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

**Key Points:**
- Dyanamic programming
    - if the total of previous <0:
        - ababdon it and pre = nums[i]
    - else:
        - it can make the current status bigger
        - add it!
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        pre = nums[0]
        l = [pre]
        for i in range(1, len(nums)):
            if pre>0:
                pre += nums[i]
            else:
                pre = nums[i]
            l.append(pre)
        return max(l)
                
```
---
### 6. #763. Partition Labels
**Description**
A string S of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.


**Example**
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
**Key Points:**
- Greedy Algorithm
    - store the farest value of each character in a dictionary
    - update farest length of the partition label by checking the dictionary
    - if reach the farest index, append the value to a list and find the other partition label.
```python
class Solution:
    def partitionLabels(self, S: str) -> List[int]:
        dic = {}
        # record the last position for every char
        for i in range(len(S)):
            dic[S[i]] = i
        l = []
        max_len = 0
        last=-1 # Note: the first length of the label need to be added one
        for i in range(len(S)):
            if dic[S[i]]>max_len:
                max_len = dic[S[i]]
            if i==max_len:
                print(i, last)
                l.append(max_len-last)
                last = max_len
                max_len=0
        
        return l
```
### 6. #665. None Decreasing Array
**Description**
**Example**

**Key Points:**

```python
```



















