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





















