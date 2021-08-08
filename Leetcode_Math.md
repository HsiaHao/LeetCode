## Math 

### No.  204 Count Primes

#### Description:

Count the number of prime numbers less than a non-negative number, `n`.

#### Idea: 

- notPrime array
- set $$notPrime[x]$$ = True when find an prime $$j$$ where x from j*j to n
- The reason from $$j*j$$ is $$j*(j-1)$$ must be processed before

```python
class Solution:
    def countPrimes(self, n: int) -> int:
        if n<3: return 0
        notPrime = [False for i in range(n)]
        count = 0
        for i in range(2, n):
            if notPrime[i]:
                continue
            count+=1
            for j in range(i*i, n, i):
                notPrime[j] = True
        return count
```





### No. 504 Base 7

#### Description:

Given an integer `num`, return *a string of its **base 7** representation*.

#### Idea: 

- Num divide by seven
- the reverse order of the remainder is the solution

```python
class Solution:
    def convertToBase7(self, num: int) -> str:
        minus = False
        if num<0:
            num = -num
            minus = True
            
        mul = 1
        res = 0
        while num>0:
            x = num%7
            res = res + x*mul
            mul *= 10
            num = num // 7
    
        return str(res) if minus==False else '-'+str(res)
```

---

### No.  405 Convert a number to Hexadecimal

#### Description:

Given an integer `num`, return *a string representing its hexadecimal representation*. For negative integers, [two’s complement](https://en.wikipedia.org/wiki/Two's_complement) method is used.

All the letters in the answer string should be lowercase characters, and there should not be any leading zeros in the answer except for the zero itself.

**Note:** You are not allowed to use any built-in library method to directly solve this problem.

#### Idea: 

- Negativer value: return two's complement form
  - num+2^32^ 
- Num -> %16 and /16

```python
class Solution:
    def toHex(self, num: int) -> str:
        h = '0123456789abcdef'
        res = []
        if num<0:
            num+=2**32
        while num>0:
            x = num%16
            res.insert(0, h[x])
            num = num//16
        ans = ''
        for c in res:
            ans+=c
        return ans if ans else '0'
```

---

### No. 168 Excel Sheet Colume Title 

#### Description:Given an integer `columnNumber`, return *its corresponding column title as it appears in an Excel sheet*.

For example:

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

#### Idea: 

- Like Base 26

- But strat from 1

- Problem; 

  - 26%26=0 however, it needs to be represented by 'Z' instead of 'A0'

  - Solution: 
    - x = (columnNumber-1)%26
    - columnNumber = (columnNumber-1) //26

- The solution makes the input 26 output 25 as the remainder

- we can +1 to this 25+1

- and columnNumber  = (columnNumber-1)//26 only effects when the remainder is 0

```python
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        e = ['Z']
        for i in range(26):
            e.append(chr(ord('A')+i))
        
        res = []
        while columnNumber>0:
            x = (columnNumber-1) % 26
            res.insert(0, e[x+1])
            columnNumber = (columnNumber-1) // 26
        ans = ''
        for c in res:
            ans+=c
        return ans
```

#### Recursion:

```python
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        if columnNumber==0: return ''
        
        return  self.convertToTitle((columnNumber-1)//26)+chr((columnNumber-1)%26 + ord('A'))
```



### No.  172 Factorial Trailing Zeroes

#### Description:

Given an integer `n`, return *the number of trailing zeroes in `n!`*.

**Follow up:** Could you write a solution that works in logarithmic time complexity?

#### Idea: 

- How many 5 in n?
- if n=5: 1,2,3,4,5-> only one 5
- if n=25 therte are  5, 10, 15, 20 ,25 has 5. But 25 has two 5.
- So if the remainder n/5 is greater than 5, that means there are more 5 in there

```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        # if there are 10, or 2*5 there will be a zero
        count = 0
        while n>0:
            x = n//5
            count+=x
            n = n//5
        return count      	
```

---

### No. 67 Add Binary

#### Description:

Given two binary strings `a` and `b`, return *their sum as a binary string*.

#### Idea: 

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        if a=='0' and b=='0': return '0'
        x = int(a)
        y = int(b)
        carry = 0
        res = []
        while x>0 or y>0 or carry>0:
            i, j = 0, 0
            if x>0:
                i = x%10
                x = x//10
            if y>0:
                j = y%10
                y = y//10
            carry = carry+i+j
            res.insert(0, str(carry%2))
            carry = carry//2
        ans = ''
        for c in res:
            ans+=c
        return ans
```



### No. 415 Add Strings

#### Description:

Given two non-negative integers, `num1` and `num2` represented as string, return *the sum of* `num1` *and* `num2` *as a string*.

You must solve the problem without using any built-in library for handling large integers (such as `BigInteger`). You must also not convert the inputs to integers directly.

#### Idea: 

- use variable carry to calculate every digit

```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        carry = 0
        i, j = len(num1), len(num2)
        ans = ''
        while i>0 or j>0 or carry>0:
            if i>0:
                carry += int(num1[i-1])
                i-=1
            if j>0:
                carry += int(num2[j-1])
                j-=1
            ans = str(carry%10) + ans
            carry = carry//10
        return ans
```

---

### No. 462. Minimum Moves to Equal Array Elements II

#### Description:

#### Given an integer array `nums` of size `n`, return *the minimum number of moves required to make all array elements equal*.

In one move, you can increment or decrement an element of the array by `1`.

Test cases are designed so that the answer will fit in a **32-bit** integer.

#### Idea: 

- the sum of all the distances from n to the median is the solution 
- sort takes $$O(nlogn)$$

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        nums.sort()
        n = len(nums)
        mid = n //2
        
        ans = 0
        for n in nums:
            ans+=abs(n-nums[mid])
        return ans
```

#### update:

- using quick sort $$O(n)$$

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        i = self.findMid(nums, 0, len(nums)-1, len(nums)//2)
        ans = 0
        for n in nums:
            ans+=abs(n - i)
        return ans
        
    def findMid(self, arr, l, r, k):
        i = self.partition(arr, l, r)
        if i==k:
            return arr[i]
        if i>k:
            return self.findMid(arr, l, i-1, k)
        return self.findMid(arr, i+1, r, k)
    
    def partition(self, arr, l, r):
        x = arr[r]
        i = l
        for j in range(l, r):
            if arr[j]<=x:
                arr[i], arr[j] = arr[j], arr[i]
                i+=1
        arr[r], arr[i] = arr[i], arr[r]
        return i
```

#### Review Quick Select:

- Partition: return i where the pivot is the i largest number in the array
- findKLargest: if i==kL return the arr[i]
- Otherwise, make the search range smaller.

#### Proof of Average O(n)

- C = n+n/2+n/4+...=2n = O(n)

---

### No. 169 majority Elements

#### Description:

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

#### Idea: 

- Voting 

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 1
        curr = nums[0]
        for i in range(1, len(nums)):
            if curr==nums[i]:
                count+=1
            else:
                if count==0:
                    count+=1
                    curr=nums[i]
                else:
                    count-=1
        return curr
```



### No. 367 Perfect Square

#### Description:Given a **positive** integer *num*, write a function which returns True if *num* is a perfect square else False.

**Follow up:** **Do not** use any built-in library function such as `sqrt`.

#### Idea: 

- Make the possible range of square root smaller by dividing 2
- Ex: num = 49
- 49//2=24->24**2>49
- 24//2 = 12->12**2>49
- 12//2 = 6->6**2<49
- The possible square root range is 6 to 12

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        r = num
        while r**2>num:
            r = r//2
            
        for i in range(r, 2*r+1):
            if i**2==num:
                return True
        return False
```

#### Idea2: Binary Search

- Use bunary search to make the range smaller
- Initial: l, r = 0, nums

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        #binary search
        l, r= 0, num
        while l<r:
            mid = (l+r)//2
            if mid**2==num:
                return True
            if mid**2>num:
                r = mid-1
            else:
                l = mid+1
        return l**2==num
```

---

### No. 326 Power of Three

#### Description:Given an integer `n`, return *`true` if it is a power of three. Otherwise, return `false`*.

An integer `n` is a power of three, if there exists an integer `x` such that `n == 3x`.

#### Idea: 

- Divide num by three and check the remainder

```python
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        if n==0: return False
        while n!=1:
            if n%3!=0:
                return False
            n = n//3
        return True
```

---

### No. 238 Product of Array Except Self

#### Description:

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

#### Idea: 

- Go through the array and store the product with the previous result
- Ex: nums[1,2,3,4,5,0] -> arr[1, 1, 1 2 , 1 2 3 , 1 2 3 4  , 1 2 3 4 5]
- Then, go back: [1 2 3 4 * 0, 1 2 3 * 0 5, ...]

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        arr = []
        curr = 1
        for n in nums:
            arr.append(curr)
            curr *= n
        
        tmp = 1
        for i in range(len(nums)-1, -1, -1):
            arr[i]*=tmp
            tmp *= nums[i]
        return arr
```

---

### No. 628 Maximum Product of Three Numbers

#### Description: 

Given an integer array `nums`, *find three numbers whose product is maximum and return the maximum product*.

 

#### Idea:

- There are two possible solution:
  - The greatest three number or the smallest two number with the greatest number

```python
class Solution:
    def maximumProduct(self, nums: List[int]) -> int:
        # two - one + or three +
        # find the greatest three and the smallest two
        # p1>p2>p3
        p1, p2, p3 = -10000, -10000, -10000
        #m1>m2
        m1, m2 = 10000, 10000
        for n in nums:
            if n>p1:
                p3 = p2
                p2 = p1
                p1 = n
            elif n>p2:
                p3 = p2
                p2 = n
            elif n>p3:
                p3 = n
            
            if n<m2:
                m1 = m2
                m2 = n
            elif n<m1:
                m1 = n

        return max(m1*m2*p1, p1*p2*p3)
```

---

