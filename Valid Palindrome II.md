### #680. Valid Palindrome II
**Description:**
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

**Example:**
Input: "abca"
Output: True

**Psudocode:**
- two pointer left and right of the string
- while through s, left+=1, right-=1, til right<left
- if s[left] not equal to s[right]
- check ( s[left+1:right+1] or s[left: right] )
    - if stil False: return False, else return True
- while loop end. return True

**Idea:**
- ther is a chance to remove a character
    - remove left or right and check again
```python
class Solution:
    count = 0
    def validPalindrome(self, s: str) -> bool:
        r = len(s)-1
        l = 0
        while r>l:
            if s[l]!=s[r]:
                if self.count==0:
                    self.count+=1
                    return self.validPalindrome(s[l+1:r+1]) or self.validPalindrome(s[l:r])
                else:
                    return False
            l+=1
            r-=1
        return True
```
--- 


