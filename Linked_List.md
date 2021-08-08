### No. 160 Intersection of two linked list

#### Description: 

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![img](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

It is **guaranteed** that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

#### Idea:

- The length of A is a+c
- The length of B is b+c
- we know that after the intersection node, the length must be the same
- Thus, we only need to find the length of A, B and remove the first i node and compare the rest of the nodes

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        # find the length of the two linklist
        tmpA, tmpB = headA, headB
        lenA, lenB = 0, 0
        while headA or headB:
            if headA:
                lenA+=1
                headA=headA.next
            if headB:
                lenB+=1
                headB = headB.next
        headA, headB = tmpA, tmpB
        if lenA>lenB:
            for i in range(lenA-lenB):
                headA = headA.next
        if lenB>lenA:
            for i in range(lenB-lenA):
                headB = headB.next

        for i in range(lenB):
            if headA==headB:
                return headA
            headA = headA.next
            headB = headB.next
        return None
```

---

### No. 206 Reverse Linked List

#### Description:

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

#### Idea: 

- Previous, current, next

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        pre, curr, nxt = None, head, None
        
        while curr:
            nxt = curr.next
            curr.next = pre
            pre = curr
            curr = nxt
        return pre
```

### No. 21 Merge Two Sorted Lists

#### Description:

Merge two sorted linked lists and return it as a **sorted** list. The list should be made by splicing together the nodes of the first two lists.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

#### Idea: 

- check one by one

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        node = ListNode()
        head = node
        while l1 or l2:
            if l1 and l2:
                if l1.val<l2.val:
                    node.next = l1
                    node = node.next
                    l1 = l1.next
                else:
                    node.next = l2
                    node = node.next
                    l2=l2.next
            if l1 and not l2:
                node.next = l1
                node = node.next
                l1=l1.next
            if l2 and not l1:
                node.next = l2å
                node = node.next
                l2 = l2.next
        return head.next
```

---

### No. 83. Remove Duplicates from Sorted List

#### Description:

Easy

2656151Add to ListShare

Given the `head` of a sorted linked list, *delete all duplicates such that each element appears only once*. Return *the linked list **sorted** as well*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
Input: head = [1,1,2]
Output: [1,2]
```

#### Idea:

- Curr: the current node
- Remove all the duplicate 

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        ite = head
        while ite:
            curr = ite
            while ite and curr.val==ite.val:
                ite = ite.next
            curr.next = ite
            ite = curr.next
        return head
            
```

### No. 19. Remove Nth Node From End of List

#### Description:

Medium

5646317Add to ListShare

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

#### Idea: 

- count the length of the list
- we know which one we want to remove from the head

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        count = 0
        ite = head
        while ite:
            count+=1
            ite = ite.next
        
        count = count-n
        
        # count==0 means we need to remove the first item
        if count==0:
            return head.next
        
        ite = head
        for i in range(count-1):
            ite = ite.next
        if ite.next.next:
            ite.next = ite.next.next
        else:
            ite.next = None
        return head
```

---

### No. 24 Swap Nodes in Pairs

#### Description:

#### Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

#### Idea: 

- Create a dummy node for return
- if pre->curr->nxt->tmp and we want to swap curr and nxt
-  Pre.next = nxt / nxt.next = curr / curr.next = tmp
- Update: pre = curr / curr = tmp

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    temp = 1
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode()
        dummy.next = head
        
        pre, curr = dummy, head
        while curr and curr.next:
            nxt = curr.next
            tmp = nxt.next
            pre.next = nxt
            nxt.next = curr
            curr.next = tmp
            
            pre = curr
            curr = tmp
        return dummy.next
```



### No. 445 Add Two Numbers II

#### Description:

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

  

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/09/sumii-linked-list.jpg)

#### Idea: 

- Reverse the Linked lists
- add each node one by one

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        # reverse the linked lists
        
        l1 = self.reverse(l1)
        l2 = self.reverse(l2)

        res = ListNode()
        curr = 0
        while l1 or l2 or curr:
            node = ListNode()
            if l1:
                curr += l1.val
                l1 = l1.next
            if l2:
                curr += l2.val
                l2 = l2.next
            node.val = curr%10
            curr = curr//10
            
            node.next = res.next
            res.next = node

        return res.next
    
    def reverse(self, head: ListNode)->ListNode:
        curr, nxt = head, head.next
        if not nxt:
            return head
        while nxt.next:
            tmp = nxt.next
            nxt.next = curr
            if curr == head:
                curr.next = None
            curr = nxt
            nxt = tmp
        nxt.next = curr
        if curr==head:
            curr.next = None
        return nxt
```



#### Idea:

- store values in stacks

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        # store values in stacks
        s1 = self.stack(l1)
        s2 = self.stack(l2)
        
        curr = 0
        res = ListNode()
        while s1 or s2 or curr:
            if len(s1)!=0:
                curr+=s1.pop()
            if len(s2)!=0:
                curr+=s2.pop()
            node = ListNode(curr%10, None)
            curr = curr//10
            node.next = res.next
            res.next = node
        return res.next
        
    def stack(self, head):
        l = []
        while head:
            l.append(head.val)
            head = head.next
        return l
```



### No. 234

#### Description:

Given the `head` of a singly linked list, return `true` if it is a palindrome.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

#### Idea: 

- count the length of the linked list
- store the first half values of the linked list  in a stack
- compare the rest part of the linked list with the values in the stack

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        ite = head
        # the length of the linked list
        l = 0
        while ite:
            l+=1
            ite = ite.next
        odd = False
        if l%2!=0: 
            odd = True
        
        #store half of the valus in a stack
        l = l//2
        s = []
        for i in range(l):
            s.append(head.val)
            head = head.next
        
        # if the length is odd, ignore the center value
        if odd: head = head.next
            
        # compare the rest of the linked list with the values in the stack
        while head:
            if head.val!=s.pop(): return False
            head = head.next
        return True
```

#### Idea:

- cut the linked list in half 
- reverse one of them and compare the values

### No. 725 Split Linked List in Parts

#### Description:

Given a (singly) linked list with head node `root`, write a function to split the linked list into `k` consecutive linked list "parts".

The length of each part should be as equal as possible: no two parts should have a size differing by more than 1. This may lead to some parts being null.

The parts should be in order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal parts occurring later.

Return a List of ListNode's representing the linked list parts that are formed.

Examples 1->2->3->4, k = 5 // 5 equal parts [ [1], [2], [3], [4], null ]

**Example 1:**

```
Input:
root = [1, 2, 3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The input and each element of the output are ListNodes, not arrays.
For example, the input root has root.val = 1, root.next.val = 2, \root.next.next.val = 3, and root.next.next.next = null.
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but it's string representation as a ListNode is [].
```

#### Idea: 

- Count = the length of the linked list
- R = count%k
- n = count//k
- we need r parts which contains (n+1) nodes and k-r parts which contains n nodes
- since count = r*(n+1) + (k-r)*n

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def splitListToParts(self, root: ListNode, k: int) -> List[ListNode]:
        count = 0
        ite = root
        while ite:
            count+=1
            ite = ite.next
        
        remain = count%k
        n = count//k
        pre = None
        res = []
        for i in range(remain):
            res.append(root)
            for j in range(n+1):
                pre = root
                root = root.next
            if pre:
                pre.next = None
                

        for i in range(k-remain):
            res.append(root)
            for j in range(n):
                if root:
                    pre = root
                    root = root.next
            if pre:
                pre.next = None
        return res
```

### No. 328 Odd Even Linked List

Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return *the reordered list*.

The **first** node is considered **odd**, and the **second** node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

#### Idea:

- create two node odd and even
- Connect each odd node to odd even to even
- then connect odd with even

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        odd = ListNode()
        even = ListNode()
        ite = head
        res = odd
        even_head = even
        while ite:
            nxt = ite.next
            
            if ite:
                odd.next = ite
                odd = odd.next
            if nxt:
                even.next = nxt
                even = even.next
                
            if nxt:
                ite = nxt.next
            else:
                break
        even.next = None  
        
        odd.next = even_head.next
        return res.next
```

