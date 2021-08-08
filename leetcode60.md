## Leetcode 60

# LinkedList 

- [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
  - [x] 7/23 

- [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
- [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)
  - [x] 7/24
  - Curr: the node is on the new linked list
  - Tmp: find the next node which has different value from curr node

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        curr = head
        while curr:
            tmp = curr.next
            # find a node whose value is different from the value of the current node
            while tmp and tmp.val==curr.val:
                tmp = tmp.next
            curr.next = tmp
            curr = curr.next
        return head
```



- [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)
  - [x] 7/24    
  - Two different cases:
    - if we find duplicates, we just ignore them
    - Else, which means we didn't find any duplicates, we should connect our node to it
    - and check whether we should terminate the process

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        
        dummy = ListNode()
        curr = dummy
        ite = head
        while curr:
            if ite.next and ite.next.val==ite.val:
                while ite.next and ite.next.val==ite.val:
                    ite = ite.next 
                if ite.next:
                    ite = ite.next
                else:
                    curr.next = None
                    curr = curr.next
            else:
                # no next value
                if not ite.next:
                    curr.next = ite
                    curr = curr.next.next
                else:
                    curr.next = ite
                    curr = curr.next
                    ite = ite.next
        return dummy.next
```



- [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
  - For each iteration, we calculate the total value of this digit from l1, l2 and the carrier.
  - Since we don't know the length of each LInkedList, we should continue check while one of l1, l2, and tmp is not empty

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        tmp = 0
        res = ListNode()
        ite = res
        while l1 or l2 or tmp:
            if l1:
                tmp+=l1.val
                l1 = l1.next
            if l2:
                tmp+=l2.val
                l2 = l2.next
            ite.next = ListNode(tmp%10)
            ite = ite.next
            tmp = tmp//10
        return res.next
```



# Stack

- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
- [x] 7/24

```python
class Solution:
    def isValid(self, s: str) -> bool:
        p = {'(':')','[':']', '{':'}'}
        l = []
        for c in s:
            if c in p:
                l.append(c)
            else:
                if p[l.pop()]!=c:
                    return False
        if l:
            return False
        else:
            return True
```

- [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
- [x] 7/24

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



# Heap, PriorityQueue

- [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
  - [x] 7/24
- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
  - [x] 7/26

- [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
  - [x] 7/26

# HashMap

- [Two Sum](https://leetcode.com/problems/two-sum/)
  - [x] 7/26
- [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
- [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
  - [x] 7/24
- [Unique Email Addresses](https://leetcode.com/problems/unique-email-addresses/)
- [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)
- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

# Graph, BFS, DFS

- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
  - [x] 7/24
- [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
  - [x] 7/29
- [Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
  - Not visible
- [Word Ladder](https://leetcode.com/problems/word-ladder/)
  - [x] 7/29
- [Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)
  - [x] 7/25

# Tree, BT, BST

- [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
- [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
- [Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)
- [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)
- [Path Sum](https://leetcode.com/problems/path-sum/)
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
- [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

# Sort

Check out [Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms). Understand in which data set radix sort or insertion sort are better than general heap/merge sort. Go each of sorting algorithms and understand pros and cons.

# Dynamic Programming

- [Paint Fence](https://leetcode.com/problems/paint-fence/)
- [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
- [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- [Unique Paths](https://leetcode.com/problems/unique-paths/)
  - [x] 7/23
- [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/) 
  - [x] 7/24
- [House Robber](https://leetcode.com/problems/house-robber/)
- [House Robber II](https://leetcode.com/problems/house-robber-ii/)
- [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
- [Word Break](https://leetcode.com/problems/word-break/)
- [Coin Change](https://leetcode.com/problems/coin-change/)

# Binary Search

- [Search Insert Position](https://leetcode.com/problems/search-insert-position/)
- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
- [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

# Recursion

- [Pow(x, n)](https://leetcode.com/problems/powx-n/)
- [K-th Symbol in Grammar](https://leetcode.com/problems/k-th-symbol-in-grammar/)
- [Split BST](https://leetcode.com/problems/split-bst/)

# Sliding Window

- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

# Greedy + Backtracking

- [Permutations](https://leetcode.com/problems/permutations/)
- [Subsets](https://leetcode.com/problems/subsets/)
- [Combination Sum](https://leetcode.com/problems/combination-sum/)
- [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

# Others

I could not put these problems in above categories, but good to solve.

- [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
- [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)
- [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)
- [Is Subsequence](https://leetcode.com/problems/is-subsequence/)
- [Next Permutation](https://leetcode.com/problems/next-permutation/)
- [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
- [ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)