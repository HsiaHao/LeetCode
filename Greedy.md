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
    - sort height -> short to high h
    - if people have same height-> sort by k
- After sorting
    - insert the people in a list by k
    - the higher person will be inserted first
    - then, when insert short people in the list will not influence higher people
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

---



