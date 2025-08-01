## 76. Minimum Window Substring[[Link](https://leetcode.com/problems/minimum-window-substring/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/minimum-window-with-characters)]

```python
# time & space: O(n)
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if t == "":
            return ""
        
        count_t, window = {}, {} # char -> count
        # we need to use count_t hashmap to compare with s
        for c in t:
            count_t[c] = 1 + count_t.get(c, 0)
        
        have, need = 0, len(count_t)
        res, res_len = [-1, -1], float('inf') # one store the result pointer, one store the
        l = 0
        for r in range(len(s)):
            c = s[r]
            window[c] = 1 + window.get(c, 0)

            if c in count_t and window[c] == count_t[c]:
                have += 1
            
            while have == need:
                # update the result (record l, r pointers), r - l + 1=lenght of current window
                if (r - l + 1) < res_len:
                    res = [l, r]
                    res_len = (r - l + 1)
                # pop from the left of the window
                window[s[l]] -= 1
                if s[l] in count_t and window[s[l]] < count_t[s[l]]:
                    have -= 1
                l += 1

        l, r = res # this is the shortes length pointers
        return s[l: r + 1] if res_len != float('inf') else ""
```

## 209. Minimum Size Subarray Sum[[Link](https://leetcode.com/problems/minimum-size-subarray-sum/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/minimum-size-subarray-sum)]

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l, total = 0, 0
        length = float("inf")

        for r in range(len(nums)):
            total += nums[r]
            while total >= target:
                length = min(length, r - l + 1)
                total -= nums[l]
                l += 1
        
       return length if length != float('inf') else 0
```

## 242. Valid Anagram[[Link](https://leetcode.com/problems/valid-anagram/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/is-anagram)]
- hint: constructure two hashmaps and compare if they are equal
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        
        count_s, count_t = {}, {}

        for i in range(len(s)):
            count_s[s[i]] = 1 + count_s.get(s[i], 0)
            count_t[t[i]] = 1 + count_t.get(t[i], 0)
        
        return count_s == count_t

# or
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        
        return sorted(s) == sorted(t)
```

## 49. *Group Anagrams[[Link](https://leetcode.com/problems/group-anagrams/description/?envType=study-plan-v2&envId=top-interview-150)]

- video explaination[[Link](https://neetcode.io/problems/anagram-groups)]
- hint: to store the count pattern as the key in hash table
```python
# time: O(m * n)
# spcae: O(m) extra space. O(m * n) space for the output list.
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        res = defaultdict(list)
        for s in strs:
            count = [0] * 26 # count pattern
            for c in s:
                count[ord(c) - ord('a')] += 1
            res[tuple(count)].append(s) # conver to tuple because list is mutable so cannot be the key
        return list(res.values())
```

## 567. Permutation in String[[Link](https://leetcode.com/problems/permutation-in-string/description/)]

- video explaination[[Link](https://neetcode.io/problems/permutation-string)]
```python
# brute-force
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        s1 = sorted(s1)

        for i in range(len(s2)):
            for j in range(i, len(s2)):
                sub_str = s2[i:j + 1]
                sub_str = sorted(sub_str)
                if s1 == sub_str:
                    return True
        return False

# sliding window
# time: O(n); space: O(1)
# 1
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s1) > len(s2):
            return False
        
        s1_count, s2_count = [0] * 26, [0] * 26
        for i in range(len(s1)):
            s1_count[ord(s1[i]) - ord('a')] += 1
            s2_count[ord(s2[i]) - ord('a')] += 1
        
        # at this stage, if two are equal, immediate return
        if s1_count == s2_count:
            return True
        
        for i in range(len(s1), len(s2)):
            # sliding part: add and remove s2
            # add
            s2_count[ord(s2[i]) - ord('a')] += 1
            # remove the leftmost
            s2_count[ord(s2[i - len(s1)]) - ord('a')] -= 1
            if s1_count == s2_count:
                return True
        return False
```

## 438. Find All Anagrams in a String[[Link](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)]

- video explaination[[Link](https://neetcode.io/solutions/find-all-anagrams-in-a-string)]

```python
# time: O(n + m); space: O(1) since at most 26 charaters
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        if len(p) > len(s):
            return []

        p_count, s_count = {}, {}
        for i in range(len(p)):
            p_count[p[i]] = 1 + p_count.get(p[i], 0)
            s_count[s[i]] = 1 + s_count.get(s[i], 0)
        
        res = []
        if p_count == s_count: # at this time, if two hash map equals, means start index 0 can be added
            res = [0]
        
        l = 0
        for r in range(len(p), len(s)):
            s_count[s[r]] = 1 + s_count.get(s[r], 0)
            s_count[s[l]] -= 1

            if s_count[s[l]] == 0:
                s_count.pop(s[l]) # pop the key
            l += 1

            if p_count == s_count:
                res.append(l)
        
        return res
```