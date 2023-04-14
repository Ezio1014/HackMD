Note
===

###### tags: `Leetcode`

* 參考資料1：[解 LeetCode 之前](https://ithelp.ithome.com.tw/articles/10213928)

### 13. Roman to Integer
* 思路：先思考羅馬數字的用途，他是拿來計數而不是拿來計算的，因此由右至左相加搭配左邊的符號如果小於右邊的符號則是減法來做即可。

```
def test():
    a = 'MMMDCCCLXXXVIII'
    if a.rfind('M') != -1:
    b = a.rfind('M')
    print(b)
```

### 5. Longest Palindromic Substring
* 思路：要先思考迴文成立的條件，再考慮條件之後拆分成迴文的字數是單數還是雙數來作判別。

[解法連結](https://www.youtube.com/watch?v=XYQecbcd6_c&t=2s)

```
str_s = ''
maxLength = 0
for i in range(len(s)):
    # odd
    l, r = i, i
    while l >= 0 and r < len(s) and s[l] == s[r]:
        if (r - l + 1) > maxLength:
            str_s = s[l: r + 1]
            maxLength = len(str_s)
        l -= 1
        r += 1
    # even
    l, r = i, i + 1
    while l >= 0 and r < len(s) and s[l] == s[r]:
        if (r - l + 1) > maxLength:
            str_s = s[l: r + 1]
            maxLength = len(str_s)
        l -= 1
        r += 1
```
            
### 11. Container With Most Water
 * 思路：
    1. 取兩面牆，一個高一個低，因其中要裝滿水所以會以低牆的高度為基準(阿不然就會溢出)
    2. 當使用雙指標一個頭一個尾來計算時，如果內縮高牆則不管下一個更高還是更低所裝的容量都會變小，因此應該要移動低牆才有機會獲得更大的容量
    3. 這樣做才可以排除移動高牆這種沒有必要的步驟，減少時間複雜度
    4. 另外兩面牆一樣高時先預設移動左邊的牆，反正移哪一個都不確定變大變小
```
def test(height: int):
    max_c = 0
    l, r = 0, len(height)-1
    while l != r:
        #大於現有的容積才替換
        if max_c < min(height[l],height[r])*(r-l):
            max_c = min(height[l], height[r]) * (r - l)
        #內縮條件
        if height[l] <= height[r]:
            l += 1
        else:
            r -= 1
```

### 15. 3Sum
* 思路：參考11. Container With Most Water的作法，要先讓指針的移動有意義。
* 做法1(Time Limit Exceeded)：
    1. 先對array做排序(由小至大或由大至小都沒差，這裡的做法是由小到大)
    2. 再來就可以做一個迴圈來固定數字n1 = array[i]，再來n2 = array[i+1]為左指針、n3 = array[len(array)-1]為右指針
    3. 接下來只要設好判別指針移動的while條件式即可
```
list_fl = []
nums.sort()
for i in range(len(nums)):
    l, r = i + 1, len(nums) - 1
    d = {}
    while l < r and 0 <= nums[i] + nums[r - 1] + nums[r]:
        if nums[l] + nums[r] > -nums[i]:
            r -= 1
        elif nums[l] + nums[r] < -nums[i]:
            l += 1

        if [nums[i], nums[l], nums[r]] not in list_fl:
            list_fl.append([nums[i], nums[l], nums[r]])
            l += 1
```
leetcode soultion:(思路基本一樣，不過人家的就是比較快QQ，之後再來研究，初步判斷是去重複這個步驟有差異)
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        ans = []
        n = len(nums)
        for i in range(n-2):
            x = nums[i]
            if x + nums[i+1] + nums[i+2] > 0:
                break
            if i > 0 and nums[i - 1] == x:
                continue
            if x + nums[n-2] + nums[n-1] < 0:
                continue
            j = i + 1
            k = n - 1
            while j < k:
                s = x + nums[j] + nums[k]
                if s > 0:
                    k -= 1
                elif s < 0:
                    j += 1
                else:
                    ans.append([x, nums[j], nums[k]])
                    j += 1
                    while j < k and nums[j] == nums[j - 1]:
                        j += 1
                    k -= 1
                    while k > j and nums[k] == nums[k + 1]:
                        k -= 1
        return ans 
```

* 做法2：將這個題目看做是TwoSum的進階，有一個動態的Target n1，再來n2跟n3就是使用Two的解法，不過還要注意去除重複的問題，Time Limit Exceeded超多次，最後再把一些不必要查找的條件列出來後過了，不過RunTime就很感人QQ。

```
list_fl = []
nums.sort()
for i in range(len(nums)):
    d = {}
    #如果頭nums[i]+尾兩個 nums[len(nums)-2],nums[len(nums)-1]還是負的就直接跳出
    if nums[i] + nums[len(nums)-2] + nums[len(nums)-1] < 0:
        continue
    #經過排序(升冪)後，如果nums[i]已經大於零代表後面都無解不用再跑了
    if nums[i] > 0:
        break
    for j in range(i + 1,len(nums)):
        n3 = -nums[i] - nums[j]
        if n3 in d and [nums[i],nums[j],n3] not in list_fl:
            list_fl.append([nums[i],nums[j],n3])
        d[nums[j]] = j
```
