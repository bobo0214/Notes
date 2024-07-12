# LeetCode_py

## 数组和哈希

### 二分查找

##### [704. 二分查找](https://leetcode.cn/problems/binary-search/)	<font color='green'>简单</font>

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1 

        while left <= right:
            middle = left + (right - left) // 2
            
            if nums[middle] > target:
                right = middle - 1  
            elif nums[middle] < target:
                left = middle + 1  
            else:
                return middle  
        return -1  
```



##### [35.搜索插入位置](https://leetcode.cn/problems/search-insert-position/description/)	<font color='green'>简单</font>

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        # 注意前两个if都是开区间，如果都是闭区间没办法通过用例
        if target > nums[right]:
            return right + 1
        elif target < nums[left]:
            return left
        else:
            while left <= right:
                mid = left + (right - left) // 2
                if nums[mid] > target:
                    right = mid - 1
                elif nums[mid] < target:
                    left = mid + 1
                else:
                    return mid
            return right + 1
```



##### [34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)	<font color='yellow'>中等</font>



##### [69.x 的平方根](https://leetcode.cn/problems/sqrtx/)	<font color='green'>简单</font>

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        left, right = 0, x

        while left <= right:
            mid = left + (right - left) // 2
            if mid*mid <= x:
                left = mid + 1
                ans = mid
            elif mid*mid > x:
                right = mid - 1
            else:
                return mid
        return ans

```



##### [367.有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)	<font color='green'>简单</font>

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        left, right = 0, num

        while left <= right:
            mid = left + (right - left) // 2
            if mid*mid > num:
                right = mid - 1
            elif mid*mid < num:
                left = mid + 1
            else:		# 注意，python中的布尔值是：True，False（首字母大写）
                return True
        return False
```



### 双指针法

##### [27.移除元素](https://leetcode.cn/problems/remove-element/)	<font color='green'>简单</font>

##### [26.删除排序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)	<font color='green'>简单</font>

##### [283.移动零](https://leetcode.cn/problems/move-zeroes/)	<font color='green'>简单</font>

##### [844.比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)	<font color='green'>简单</font>

##### [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)	<font color='green'>简单</font>



### 哈希

##### [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)	<font color='green'>简单</font>

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashset = set()

        # set的添加是add()，删除是remove()
        for n in nums:
            if n in hashset:
                return True
            hashset.add(n)
        return False
```



##### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)	<font color='green'>简单</font>

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)	# 此处使用dict的get函数，防止初始时无对应key值
            countT[t[i]] = 1 + countT.get(t[i], 0)	# dict也使用[]
        return countS == countT
    
    # 上面那个for循环等同于
        for ch in s:
            countS[ch] = 1 + countS.get(ch,0)
        for ch in t:    
            countT[ch] = 1 + countT.get(ch,0)
```

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
        
```



##### [1. 两数之和](https://leetcode.cn/problems/two-sum/)	<font color='green'>简单</font>

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}

        for i,n in enumerate(nums):  # enumerate()函数
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff],i]
            prevMap[n] = i		# 字典key-value的对应写法，append方法是list中的
```

##### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)	<font color='green'>简单</font>

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = defaultdict(list)			#defaultdict()函数

        for s in strs:
            count = [0] * 26		# count对每个词来说都是新的
            for c in s:
                count[ord(c) - ord("a")] += 1
            ans[tuple(count)].append(s)
        return list(ans.values())
```

##### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)		<font color='yellow'>中等</font>

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]

        for n in nums:
            count[n] = 1 + count.get(n,0)
        for n,i in count.items():
            freq[i].append(n)
        
        res = []
        for i in range(len(freq)-1,0,-1):   # range(*,*,*)
            for n in freq[i]:       # freq[i]可能会有多个
                res.append(n)
                if len(res) == k:
                    return res
        
        
```

##### [271. String Encode and Decode](https://neetcode.io/problems/string-encode-and-decode)		<font color='yellow'>中等</font>

```python
class Solution:

    def encode(self, strs: List[str]) -> str:
        res = ""
        for s in strs:
            res += str(len(s)) + '#' + s    # 不能int+字符串，只能字符串+字符串
        return res


    def decode(self, s: str) -> List[str]:
        res = []
        i = 0

        while i < len(s):
            j = i
            while s[j] != '#':
                j = j + 1
            length = int(s[i:j])
            res.append(s[j+1:j+1+length])
            i = j + 1 + length
        
        return res

```

##### [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)		<font color='yellow'>中等</font>

```py
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res = [1] * len(nums)   # 需要初始化

        prefix = 1
        for i in range(len(nums)):
            res[i] = prefix
            prefix *= nums[i]
        postfix = 1
        for i in range(len(nums) - 1,-1,-1):
            res[i] *= postfix
            postfix *= nums[i]
        return res
        
```



##### [36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)		<font color='yellow'>中等</font>

```py
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        rows = defaultdict(set)
        cols = defaultdict(set)
        squares = defaultdict(set)

        for r in range(9):
            for c in range(9):
                if board[r][c] == '.':      # 别忘记处理'.'
                    continue
                if (board[r][c] in rows[r] or board[r][c] in cols[c]
                    or board[r][c] in squares[(r//3,c//3)]):
                    return False
                rows[r].add(board[r][c])
                cols[c].add(board[r][c])
                squares[(r//3,c//3)].add(board[r][c])
        return True
```





##### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)		<font color='yellow'>中等</font>

```py
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums)
        longest = 0

        for n in nums:
            if n-1 not in numSet:
                length = 0
                while n+length in numSet:       # 很聪明的n+length（n不变，length变）
                    length += 1
                longest = max(length,longest)
        return longest
        
```

