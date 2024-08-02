# LeetCode_py

## 数组和哈希

### 双指针法

#### **左右指针**

两个指针相向而行或者相背而行



#### 1. 二分查找

前提：1. 排序	2. 尽量无重复（若不然，可能会有多个答案）

##### [704. 二分查找](https://leetcode.cn/problems/binary-search/)	<font color='green'>简单</font>√

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1 

        while left <= right:
            middle = left + (right - left) // 2
            
            if nums[middle] > target:
                right = middle - 1  # 是mid - 1
            elif nums[middle] < target:
                left = middle + 1   # 是mid + 1
            else:
                return middle  
        return -1  
```



##### [35.搜索插入位置](https://leetcode.cn/problems/search-insert-position/description/)	<font color='green'>简单</font>√

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
            return right + 1	# 或者是l
```

问题：假设该值不在数组中，返回插入位置为什么是`right + 1`呢？

Ans：

根据 if-else 逻辑，right右边的都大于target，left左边的都小于target（此时属于数组中无target，找target插入位置）

我们要返回的下标应该是 

![db2697ca0abfae09ee7ab8ed556047c](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407142112597.jpg)





##### [34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)	<font color='yellow'>中等</font>√

二分法最重要的是<font color = 'red'>看最终左右指针会停在哪里。</font>

因为有序，势必left左边的都比target要小，right右边的都比target要大

```py
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        start = self.lower_bound(nums, target)  # 选择其中一种写法即可
        if start == len(nums) or nums[start] != target:
            return [-1, -1]  # nums 中没有 target
        # 如果 start 存在，那么 end 必定存在
        end = self.lower_bound(nums, target + 1) - 1
        return [start, end]

    # 闭区间写法
    def lower_bound(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1 
        while left <= right:  
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1  
            else:
                right = mid - 1  
        return left


```

根据 if-else 和 return 逻辑，left左边的势必都是比target小的，所以从left开始的值要么为target的第一个，要么是比target大的第一个



##### [LCR 172. 统计目标成绩的出现次数](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)	<font color='green'>简单</font>√

```py
class Solution:
    def countTarget(self, scores: List[int], target: int) -> int:
        start = self.backIndex(scores,target)
        if start == len(scores) or scores[start] != target:
            return 0
        end = self.backIndex(scores,target + 1) - 1
        return end - start + 1
        
    def backIndex(self,scores,target):
        l, r = 0, len(scores) - 1
        while l <= r:
            mid = l + (r - l) // 2
            if scores[mid] < target:
                l = mid + 1
            else:
                r = mid - 1
        return l

```





##### [69.x 的平方根](https://leetcode.cn/problems/sqrtx/)	<font color='green'>简单</font>√

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



##### [367.有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)	<font color='green'>简单</font>√

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



##### [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)	<font color='yellow'>中等</font>√

```py
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        ROWS, COLS = len(matrix), len(matrix[0])

        top, bot = 0, ROWS - 1
        while top <= bot:
            row = (top + bot) // 2
            if matrix[row][-1] < target:
                top = row + 1
            elif matrix[row][0] > target:
                bot = row - 1
            else:
                break
        
        if not (top <= bot):
            return False
            
        row = (top + bot) // 2
        l, r = 0, COLS - 1
        while l <= r:
            mid = (l + r) // 2
            if matrix[row][mid] > target:
                r = mid - 1
            elif matrix[row][mid] < target:
                l = mid + 1
            else:
                return True
        return False
```



##### [875. 爱吃香蕉的珂珂](https://leetcode.cn/problems/koko-eating-bananas/)	<font color='yellow'>中等</font>√

```py
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        l, r =  1, max(piles)
        res = r

        while l <= r:
            hours = 0
            k = (l + r) // 2
            for p in piles:
                hours += math.ceil(p / k)
            if hours <= h:
                r = k - 1
                res = min(res,k)
            else:
                l = k + 1

        return res
        
```



##### [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)		<font color='yellow'>中等</font>√

```py
class Solution:
    def findMin(self, nums: List[int]) -> int:
        l, r = 0, len(nums) - 1
        res = nums[0]

        while l <= r:
            if nums[l] < nums[r]:
                res = min(res,nums[l])
                break
            
            m = (l + r) // 2
            res = min(res,nums[m])
            if nums[m] >= nums[l]:
                l = m + 1
            else:
                r = m - 1
        return res
        
        
```



##### [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)			<font color='yellow'>中等</font>√

```py
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1

        while l <= r:
            m = (l + r) // 2
            if target == nums[m]:
                return m
            
            if nums[l] <= nums[m]:      # 整除会偏小，所以m是有可能等于l的
                if target > nums[m] or target < nums[l]:   
                    l = m + 1
                else:
                    r = m - 1
            else:
                if target > nums[r] or target < nums[m]:
                    r = m - 1
                else:
                    l = m + 1
        return -1
        
```



##### [981. 基于时间的键值存储](https://leetcode.cn/problems/time-based-key-value-store/)			<font color='yellow'>中等</font>√

```py
class TimeMap:

    def __init__(self):
        self.store = {}

    def set(self, key: str, value: str, timestamp: int) -> None:
        if key not in self.store:
            self.store[key] = []
        self.store[key].append([value,timestamp])

    def get(self, key: str, timestamp: int) -> str:
        res = ""
        values = self.store.get(key,[])

        l, r = 0, len(values) - 1
        while l <= r:
            mid = (l + r) // 2
            if values[mid][1] <= timestamp:
                res = values[mid][0]
                l = mid + 1
            else:
                r = mid - 1
        return res
        
```



##### [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)		<font color='red'>困难</font>√

```py
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        A, B = nums1, nums2
        total = len(A) + len(B)
        half = total // 2

        if len(B) < len(A):
            A, B = B, A
        
        l, r = 0, len(A) - 1
        while True:
            i = (l + r) // 2
            j = half - i - 2

            Aleft = A[i] if i >= 0 else float("-infinity")
            Bleft = B[j] if j >= 0 else float("-infinity")
            Aright = A[i + 1] if i+1 < len(A) else float("infinity")
            Bright = B[j + 1] if j+1 < len(B) else float("infinity")

            if Aleft <= Bright and Bleft <= Aright:
                if total % 2:
                    return min(Aright, Bright)
                else:
                    return (max(Aleft, Bleft) + min(Aright, Bright)) / 2
            elif Aleft > Bright:
                r = i - 1
            else:
                l = i + 1
```





#### 2. 两数之和/积

##### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)		<font color='yellow'>中等</font>√

[LCR 006. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/kLl5u1/)	√

[LCR 179. 查找总价格为目标值的两个商品](https://leetcode.cn/problems/he-wei-sde-liang-ge-shu-zi-lcof/)	√

输入数组为有序数组！

```py
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l, r = 0, len(numbers) - 1

        while l < r:
            curSum = numbers[l] + numbers[r]

            if curSum > target:
                r -= 1
            elif curSum < target:
                l += 1
            else:
                return [l + 1, r + 1]
```



##### [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)	<font color='green'>简单</font>√

只能额外开辟新数组，不能在原有基础上进行修改

```py
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        l, r = 0, len(nums) - 1
        res = []

        while l <= r:	# 此处应该是l <= r
            leftNum = nums[l]**2
            rightNum = nums[r]**2
            if leftNum < rightNum:
                res.append(rightNum)
                r -= 1
            else:
                res.append(leftNum)
                l += 1
        res = res[::-1]
        return res
```

注意几个细节：

1. 如果循环条件设置会`l < r`：那么最后一轮比较后只会添加较大的值，而较小的值并没有加入新数组
2. 可以先从大到小加入新数组后再反转（py万岁）



##### [15. 三数之和](https://leetcode.cn/problems/3sum/)	<font color='yellow'>中等</font>√

```py
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()

        for i,a in enumerate(nums):
            if i > 0 and nums[i-1] == a:
                continue
            l, r = i + 1, len(nums) - 1
            while l < r:
                threeSum = a + nums[l] + nums[r]
                if threeSum > 0:
                    r -= 1
                elif threeSum < 0:
                    l += 1
                else:
                    res.append([a,nums[l],nums[r]])
                    l += 1
                    while l < r and nums[l] == nums[l-1]:
                        l += 1
        
        return res
```



#### 3. 反转数组

##### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)	<font color='green'>简单</font>√

```py
class Solution:
    def reverseString(self, s: List[str]) -> None:
        l, r = 0, len(s) - 1

        while l < r:
            temp = s[l]
            s[l] = s[r]
            s[r] = temp
            l += 1
            r -= 1

```





#### 4. 回文串判断

##### [9. 回文数](https://leetcode.cn/problems/palindrome-number/)	<font color='green'>简单</font>√

法一：将int型转换为str型

```py
class Solution:
    def isPalindrome(self, x: int) -> bool:
        s = str(x)
        l, r = 0, len(s) - 1

        while l < r:
            if s[l] != s[r]:
                return False
            l += 1
            r -= 1
        return True
```

法二：计算x的倒序数

```py
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        cur = 0
        num = x
        while num != 0:
            cur = cur * 10 + num % 10
            num = num // 10		# 此处是整除！！！
        return cur == x
```





##### [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)	<font color='green'>简单</font>√

法一：

`isalnum()`函数

```py
class Solution:
    def isPalindrome(self, s: str) -> bool:
        newStr = ""

        for c in s:
            if c.isalnum():
                newStr += c.lower()		# 不区分大小写，所以要把字母全部小写进行比较
        return newStr == newStr[::-1]

        
```

法二：

双指针

```py
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:
            while l < r and not self.alphaNum(s[l]):	# 类记得要用self引用新建的方法
                l += 1
            while r > l and not self.alphaNum(s[r]):	# 是while！是while！是while！
                r -= 1
            if s[l].lower() != s[r].lower():	# 不区分大小写，所以要把字母全部小写进行比较
                return False
            l, r = l + 1, r - 1
        return True
    
    def alphaNum(self, c):
        return (ord('A') <= ord(c) <= ord('Z') or 
                ord('a') <= ord(c) <= ord('z') or 
                ord('0') <= ord(c) <= ord('9'))
```



#### 5. 应用

##### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)		<font color='yellow'>中等</font>√

```py
class Solution:
    def maxArea(self, heights: List[int]) -> int:
        res = 0
        l, r = 0, len(heights) - 1

        while l < r:
            area = (r - l) * min(heights[l],heights[r])
            res = max(res,area)
            if heights[l] < heights[r]:
                l += 1
            else:
                r -= 1
        return res
        
```

##### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)	<font color='red'>困难</font>√

```py
class Solution:
    def trap(self, height: List[int]) -> int:
        res = 0
        l, r = 0, len(height) - 1
        leftMax, rightMax = height[l], height[r]

        while l < r:
            if leftMax < rightMax:
                l += 1
                leftMax = max(leftMax,height[l])
                res += leftMax - height[l]
            else:
                r -= 1
                rightMax = max(rightMax,height[r])
                res += rightMax - height[r]
        return res



```





#### **快慢指针**

两个指针同向而行，一快一慢

技巧：1. 原地修改数组



##### [27.移除元素](https://leetcode.cn/problems/remove-element/)	<font color='green'>简单</font>√

```py
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        ans = 0

        l, r = 0, 0
        while r < len(nums):
            if nums[r] != val:
                nums[l] = nums[r]
                l += 1
                ans += 1
            r += 1
        return ans
```



##### [26.删除排序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)	<font color='green'>简单</font>√

```py
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        l, r = 1, 1
        flag = nums[0]

        while r < len(nums):
            if nums[r] != flag:
                nums[l] = nums[r]
                flag = nums[r]
                l += 1
            r += 1
        return l
```



##### [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)	<font color='yellow'>中等</font>√

```py
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        flag = nums[0]
        count = 1
        l, r = 1, 1

        while r < len(nums):
            if nums[r] == flag and count >= 2:
                r += 1
                continue

            if nums[r] == flag:
                count += 1
            else:
                count = 1
            nums[l] = nums[r]
            flag = nums[l]
            l += 1
            r += 1
        return l
```





##### [283.移动零](https://leetcode.cn/problems/move-zeroes/)	<font color='green'>简单</font>√

```py
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        l, r = 0, 0

        while r < len(nums):
            if nums[r] != 0:
                nums[l] = nums[r]
                l += 1
            r += 1
        
        while l < len(nums):
            nums[l] = 0
            l += 1
```



##### [844.比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)	<font color='green'>简单</font>√

```py
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        s = self.deleteBack(s)
        t = self.deleteBack(t)
        return s == t


    def deleteBack(self, s):
        res = ""
        pointer = 0

        for ch in s:
            if ch != '#':
                res += ch
                pointer += 1
            else:
                if pointer != 0:
                    pointer -= 1
                    res = res[0:pointer]
        return res


```



### 哈希

##### [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)	<font color='green'>简单</font>√

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



##### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)	<font color='green'>简单</font>√

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



##### [1. 两数之和](https://leetcode.cn/problems/two-sum/)	<font color='green'>简单</font>√

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

##### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)	<font color='green'>简单</font>√

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

##### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)		<font color='yellow'>中等</font>√

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
            # freq[i]可能会有多个；且返回值为list，所以不能将整个list都加入到res中，而是遍历元素加入
            for n in freq[i]:       
                res.append(n)
                if len(res) == k:
                    return res
        
        
```

##### [271. String Encode and Decode](https://neetcode.io/problems/string-encode-and-decode)		<font color='yellow'>中等</font>√

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

##### [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)		<font color='yellow'>中等</font>√

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



##### [36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)		<font color='yellow'>中等</font>√

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





##### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)		<font color='yellow'>中等</font>√

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



### 栈

##### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)	<font color='green'>简单</font>√

法一：

```py
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        map = {"}":"{", "]":"[", ")":"("}

        for ch in s:
            if ch in map:
                if stack and stack[-1] == map[ch]:
                    stack.pop()
                    continue
            stack.append(ch)
        return not stack	# 注意返回值

```

法二：

```py
class Solution:
    def isValid(self, s: str) -> bool:
        Map = {")": "(", "]": "[", "}": "{"}
        stack = []

        for c in s:
            if c not in Map:
                stack.append(c)
                continue
            if not stack or stack[-1] != Map[c]:
                return False
            stack.pop()

        return not stack
    
```



##### [155. 最小栈](https://leetcode.cn/problems/min-stack/)		<font color='yellow'>中等</font>√

```py
class MinStack:

    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        val = min(val, self.minStack[-1] if self.minStack else val)
        self.minStack.append(val)

    def pop(self) -> None:
        self.stack.pop()
        self.minStack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minStack[-1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```



##### [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)		<font color='yellow'>中等</font>√

```py
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        
        for t in tokens:
            if t == '+':
                stack.append(stack.pop() + stack.pop())
            elif t == '-':
                a, b = stack.pop(), stack.pop()
                stack.append(b - a)
            elif t == '*':
                stack.append(stack.pop() * stack.pop())
            elif t == '/':
                a, b = stack.pop(), stack.pop()
                stack.append(int(float(b) / a))		# 因为题目要求除法向0舍入，整数整除是对的，但在py中 -3//2=-2
            else:
                stack.append(int(t))
        return stack.pop()

```



##### [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)		<font color='yellow'>中等</font>√

```py
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res = []
        stack = []

        def backTrack(openN, closedN):
            if openN == closedN == n:
                res.append("".join(stack))	# 如何将整个栈的内容转化为一个值
                return
            
            if openN < n:	# 是if而不是elif
                stack.append("(")
                backTrack(openN + 1,closedN)
                stack.pop()
            
            if closedN < openN:	# 是if而不是elif
                stack.append(")")
                backTrack(openN,closedN + 1)
                stack.pop()
        
        backTrack(0,0)
        return res
```



##### [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)		<font color='yellow'>中等</font>√

[LCR 038. 每日温度](https://leetcode.cn/problems/iIQa4I/)		<font color='yellow'>中等</font>√

```py
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        ans = [0] * len(temperatures)
        stack = []

        for i,t in enumerate(temperatures):
            while stack and t > stack[-1][0]:
                stackT, stackIdx = stack.pop()
                ans[stackIdx] = i - stackIdx
            stack.append([t,i])
        return ans
```



##### [853. 车队](https://leetcode.cn/problems/car-fleet/)		<font color='yellow'>中等</font>√

法一：

```py
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        stack = []
        pair = [(p, s) for p, s in zip(position,speed)]

        for p, s in sorted(pair)[::-1]:
            stack.append((target - p) / s)
            if len(stack) >= 2 and stack[-1] <= stack[-2]:
                stack.pop()
        return len(stack)
            
```

法二：

```py
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        pair = [(p,s) for p,s in zip(position,speed)]
        stack = []

        for p,s in sorted(pair)[::-1]:
            h = (target - p) / s
            if stack and stack[-1] >= h:
                continue
            stack.append(h)
        return len(stack)
            
```



##### [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)		<font color='red'>困难</font>√

```py
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        maxArea = 0
        stack = []  # pair: (index, height)

        for i, h in enumerate(heights):
            start = i
            while stack and stack[-1][1] > h:
                index, height = stack.pop()
                maxArea = max(maxArea, height * (i - index))
                start = index
            stack.append((start, h))

        for i, h in stack:
            maxArea = max(maxArea, h * (len(heights) - i))
        return maxArea
```



### 滑动窗口

##### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)	<font color='green'>简单</font>√

```py
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        l, r = 0, 1
        res = 0

        while r < len(prices):
            if prices[l] < prices[r]:
                profit = prices[r] - prices[l]
                res = max(res, profit)
            else:
                l = r
            r += 1
        return res

```



##### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)	<font color='yellow'>中等</font>√

```py
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charSet = set()
        l = 0
        res = 0

        for r in range(len(s)):
            while s[r] in charSet:
                charSet.remove(s[l])
                l += 1
            charSet.add(s[r])
            res = max(res, r-l+1)
        return res
```



##### [424. 替换后的最长重复字符](https://leetcode.cn/problems/longest-repeating-character-replacement/)	<font color='yellow'>中等</font>√

```py
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}
        
        l = 0
        maxf = 0
        for r in range(len(s)):
            count[s[r]] = 1 + count.get(s[r], 0)
            maxf = max(maxf, count[s[r]])

            if (r - l + 1) - maxf > k:
                count[s[l]] -= 1
                l += 1

        return (r - l + 1)
```

```py
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}
        res = 0

        l = 0
        maxf = 0
        for r in range(len(s)):
            count[s[r]] = 1 + count.get(s[r],0)
            maxf = max(maxf, count[s[r]])

            if (r-l+1) - maxf > k:	# 用w'h
                count[s[l]] -= 1
                l += 1

            res = max(res, r-l+1)
        return res
```



##### [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)	<font color='yellow'>中等</font>

[LCR 014. 字符串的排列](https://leetcode.cn/problems/MPnaiL/)	<font color='yellow'>中等</font>

```py
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s1) > len(s2):
            return False
        
        s1Count, s2Count = [0] * 26, [0] * 26
        for i in range(len(s1)):
            s1Count[ord(s1[i]) - ord('a')] += 1
            s2Count[ord(s2[i]) - ord('a')] += 1
        
        matches = 0
        for i in range(26):
            matches += 1 if s1Count[i] == s2Count[i] else 0

        l = 0
        for r in range(len(s1), len(s2)):
            if matches == 26:
                return True

            index = ord(s2[r]) - ord("a")
            s2Count[index] += 1
            if s1Count[index] == s2Count[index]:
                matches += 1
            elif s1Count[index] + 1 == s2Count[index]:
                matches -= 1

            index = ord(s2[l]) - ord("a")
            s2Count[index] -= 1
            if s1Count[index] == s2Count[index]:
                matches += 1
            elif s1Count[index] - 1 == s2Count[index]:
                matches -= 1
            l += 1

        return matches == 26       
```



##### [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)		<font color='red'>困难</font>

```py
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if t == "":
            return ""
        
        countT, window = {}, {}
        for c in t:
            countT[c] = 1 + countT.get(c,0)
        
        have, need = 0, len(countT)
        res, resLen = [-1, -1], float('infinity')
        l = 0

        for r in range(len(s)):
            c = s[r]
            window[c] = 1 + window.get(c,0)

            if c in countT and countT[c] == window[c]:
                have += 1
            
            while have == need:
                if r-l+1 < resLen:
                    res = [l, r]
                    resLen = r-l+1
                
                window[s[l]] -= 1
                if s[l] in countT and window[s[l]] + 1 == countT[s[l]]:
                    have -= 1
                l += 1
        l, r = res
        return s[l:r+1] if resLen != float('infinity') else ''
```

