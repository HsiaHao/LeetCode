### Tree

---

## 104 Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

#### Idea:

- the max depth for a node is either the depth of left subtree tree ot right subtree +1

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: TreeNode, depth=0) -> int:
        if not root: return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right))+1
```



## 110 Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of *every* node differ in height by no more than 1.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

### Idea:

- count depth of left subtree and right subtree
- l is false or right is false-> it's false
- if abs(l-r)>1-> its false.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        return self.help(root)!=-1
    
    # if the tree is balanced, count the max depth of the tree
    # otherwise, return -1
    def help(self, root) -> int:
        if not root: return 0
        
        l = self.help(root.left)
        r = self.help(root.right)
        if abs(l-r)>1 or l==-1 or r==-1:
            return -1
        else:
            return max(l, r)+1
```



### No.543 Diameter of Binary Tree

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3is the length of the path [4,2,1,3] or [5,2,1,3].
```

#### Idea:

- count the max depth for a node 
- the max depth for a node is either the max depth of its right subtree or left subtree
- when return to the previous layer, the value need to add 1 since there will be another edge in between

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    res = 0
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.maxDepth(root)
        return self.res
    
    def maxDepth(self, root):
        if not root: return 0
        
        l = self.maxDepth(root.left)
        r = self.maxDepth(root.right)
        
        self.res = max(self.res, l+r)
        
        return max(l,r)+1
```

## 226 Invert Binary Tree

Given the `root` of a binary tree, invert the tree, and return *its root*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

#### idea:

- for every node, switch left and right subtree

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root: return None
        
        left_sub = self.invertTree(root.left)
        right_sub = self.invertTree(root.right)
        
        root.left = right_sub
        root.right = left_sub
        
        return root
```

## 617 Merge Two Binary Trees

You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return *the merged tree*.

**Note:** The merging process must start from the root nodes of both trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```

#### Idea:

- four different cases
- for each node: return when the subtrees have merged

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1 and not t2: return None
        if not t1 and t2: return t2
        if not t2 and t2: return t1
        if t1 and t2:
            t1.val += t2.val
            l = self.mergeTrees(t1.left, t2.left)
            r = self.mergeTrees(t1.right, t2.right)
            t1.left = l
            t1.right = r
        return t1
```



## 112 Path Sum

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
```

#### Idea:

- if find a leaf where the leaf has no right and left subtree
- check the sum==targetSum

#### Code:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    res = False
    def hasPathSum(self, root: TreeNode, targetSum: int) -> bool:
        if not root: return False
        
        def check(root, s):
            if not root:
                return 
            s = s+root.val
            if not root.left and not root.right:
                if s==targetSum:
                    self.res = True
                return
            check(root.left, s)
            check(root.right, s)
            
        check(root, 0)
        
        return self.res
```



## 437. Path Sum III

Given the `root` of a binary tree and an integer `targetSum`, return *the number of paths where the sum of the values along the path equals* `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```

#### Idea:

- Create a stack (first-in-last-out) 
- Append node value to the stack and check the val
- onece find there is no next node, pop a node value and check the other side

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    res = 0
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        def findTarget(root, cur_sum, path):
            if not root: return
            
            cur_sum+=root.val
            path.append(root.val)

            if cur_sum==targetSum:
                self.res+=1
                
            tmp = cur_sum
            for i in range(len(path)-1):
                cur_sum-=path[i]
                if cur_sum==targetSum: self.res+=1
            cur_sum = tmp
            findTarget(root.left, cur_sum, path)
            
            if root.left:
                path.pop()
            
            findTarget(root.right, cur_sum, path)
            if root.right:
                path.pop()
            
        findTarget(root, 0, [])
        return self.res
```

## 572 Subtree of Another Tree

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of` subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

#### Idea:

- create a function for compare the subtree from a specific root
- itereate through all the node of the tree

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: TreeNode, subRoot: TreeNode) -> bool:
        if not root: return False
        
        if self.startFromRoot(root, subRoot)==True:
            return True
        else:
            return self.isSubtree(root.right, subRoot) or self.isSubtree(root.left, subRoot)
        
        
    def startFromRoot(self, root, subRoot):
        if not root and not subRoot: return True
        if not root and subRoot: return False
        if root and not subRoot: return False
        
        if root.val!=subRoot.val: return False
            
        return self.startFromRoot(root.right, subRoot.right) and self.startFromRoot(root.left, subRoot.left) 
```



## 404 Sum of Left Leaves

Given the `root` of a binary tree, return the sum of all left leaves.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 24
Explanation: There are two left leaves in the binary tree, with values 9 and 15 respectively.
```

#### Idea:

- when the node is left node and it is a leaf
- add it to res

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    res = 0
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        
        self.addLeft(root)
        
        return self.res
        
    def addLeft(self, root):
        if not root:
            return 
        
        if root.left and not root.left.left and not root.left.right:
            self.res+=root.left.val
        
        self.addLeft(root.left)
        self.addLeft(root.right)
```

## 111 Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root: return 0
        
        q = deque([(1, root)])
        
        while q:
            dep, root = q.popleft()
            if not root.left and not root.right:
                return dep
            if root.left:
                q.append((dep+1, root.left))
            if root.right:
                q.append((dep+1, root.right))
        return -1
            
```

## 404 Sum of Left Leaves

Given the `root` of a binary tree, return the sum of all left leaves.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 24
Explanation: There are two left leaves in the binary tree, with values 9 and 15 respectively.
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    res = 0
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        
        self.addLeft(root)
        
        return self.res
        
    def addLeft(self, root):
        if not root:
            return 
        
        if root.left and not root.left.left and not root.left.right:
            self.res+=root.left.val
        
        self.addLeft(root.left)
        self.addLeft(root.right)
```

## 687 Longest Univalue Path

Given the `root` of a binary tree, return *the length of the longest path, where each node in the path has the same value*. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```
Input: root = [5,4,5,1,1,5]
Output: 2
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.mx = 0
    def longestUnivaluePath(self, root: TreeNode) -> int:
        if not root: return 0

        self.count(root, root.val)
        return self.mx
        
    def count(self, root, pre)->int:
        if not root: return 0
        
        curr = 1
        r=self.count(root.right, root.val)
        l=self.count(root.left, root.val)
        curr = curr
        
        if curr+r+l-1>self.mx:
            self.mx=curr-1+r+l
        if root.val==pre:
            return curr+max(l, r)
        else:
            return 0
```

## 337 House Robber III

he thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the `root` of the binary tree, return *the maximum amount of money the thief can rob **without alerting the police***.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

#### Idea:

- either rob the root or not rob the root
- return the max value between the above two cases

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    
    nodeMap = {}
    
    def rob(self, root: TreeNode) -> int:
        if not root: return 0
        if root in self.nodeMap:
            return self.nodeMap[root]
        
        val = root.val
        if root.right: 
            val+=self.rob(root.right.right)
            val+=self.rob(root.right.left)
        if root.left: 
            val+=self.rob(root.left.right)
            val+=self.rob(root.left.left)
        
        val2 = self.rob(root.right) + self.rob(root.left)
        
        self.nodeMap[root] = max(val, val2)
        
        return max(val, val2)
```

## 671 Second Minimum Node in a Binary Tree

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

 

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/15/smbt1.jpg)

```
Input: root = [2,2,5,null,null,5,7]
Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # min1<min2
    min1 = 10000000000
    min2 = 10000000000
    def findSecondMinimumValue(self, root: TreeNode) -> int:
        
        self.help(root)
        print(self.min1, self.min2)
        if self.min1==10000000000 or self.min2==10000000000:
            return -1
        return self.min2 if self.min1!=self.min2 else -1
        
    def help(self, root):
        if not root: return 
        
        if root.val<self.min1:
            
            self.min2 = self.min1
            self.min1 = root.val
        elif root.val<self.min2 and root.val!= self.min1:
            self.min2 = root.val

        self.help(root.left)
        self.help(root.right)
```

