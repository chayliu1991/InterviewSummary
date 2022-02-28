# [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

```
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        std::vector<int> res;
        int left = 0,right = numbers.size()-1;
        while(left < right)
        {
            if(numbers[left] + numbers[right] == target)
                return {left+1,right+1};
            else if(numbers[left] + numbers[right] > target)
                right--;
            else
                left++;
        }
        return res;
    }
};
```

# [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1,j = n-1,last = m+n-1;

        //@ 条件应该包含0
		while(i >= 0 && j >= 0)
			nums1[last--] = nums1[i] >= nums2[j] ? nums1[i--] : nums2[j--];

		while(j >= 0)
			nums1[last--] = nums2[j--];
    }
};
```

# [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

```
class Solution {
public:
    string minWindow(string s, string t) {
        if(s.length() < t.length())
            return "";
        
        std::vector<int> needs(128,0),windows(128,0);
        int left = 0,right = 0;
        int match_len = 0,min_len = INT_MAX;
        std::string res;
        for(auto c : t)
            needs[c]++;
        
        while(right < s.length())
        {
            char rc = s[right];
            windows[rc]++;

            if(needs[rc] && windows[rc] <= needs[rc])
                match_len++;
            
            while(left <= right && match_len == t.length())
            {
                if(min_len > right - left + 1)
                {
                    min_len = right - left + 1;
                    res = s.substr(left,min_len);
                }

                char lc = s[left];
                windows[lc]--;
                if(needs[lc] && windows[lc] < needs[lc])
                    match_len--;
                left++;
            }
            ++right;
        }
        return res;
    }
};
```

# [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)

```
class Solution {
public:
    bool judgeSquareSum(int c) {
        long left = 0,right = sqrt(c);
        while(left <= right)
        {
            long square = left * left + right * right;
            if(square == c)
                return true;
            else if(square > c)
                right--;
            else
                left++;
        }
        return false;
    }
};
```

# [680. 验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)

```
class Solution {
public:
    bool validPalindrome(string s) {
        int left = 0,right = s.length() -1;
        while(left < right)
        {
            if(s[left] == s[right])
                left++,right--;
            else
                return valid(s,left+1,right) || valid(s,left,right-1);
        }
        return true;
    }

    bool valid(const std::string& str,int i,int j)
    {
        while(i <= j)
        {
            if(str[i++] != str[j--])
                return false;
        }
        return true;
    }
};
```

# [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

```
class Solution {
public:
    int maxArea(vector<int>& height) {
        if(height.empty())
            return 0;
        int left = 0,right = height.size()-1;
        int res = 0;
        while(left < right)
        {
            int curr = std::min(height[right],height[left]) * (right-left);
            res = std::max(res,curr);
            if(height[right] < height[left]) 
                right --;
            else
                left++;
        }
        return res;
    }
};
```

# [57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

```
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
		int left = 1,right = 1;
		std::vector<std::vector<int>> res;
		int sum = 0;
		while(left <= target / 2)
		{
			if(sum == target)
			{	
				std::vector<int> path;
				for(int i = left;i < right;i++)
					path.emplace_back(i);
				res.emplace_back(path);
				sum -= left;
				left++;
			}
			else if(sum > target)
			{
				sum -= left;
				left++;
			}
			else
			{
				sum += right;
				right++;				
			}
		}
		return res;
    }
};
```

# [21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

```
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int n = nums.size();
        if(n <= 1)
            return nums;
        int left = 0,right = n -1;
        while(left < right)
        {
            if(nums[left] & 0x01 && left < right)
                left++;
            if(!(nums[right] & 0x01) && left < right)
                right--;
            if(left < right)      
                std::swap(nums[left],nums[right]);       
        }
        return nums;
    }
};
```

# [57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        if(nums.empty())
            return {};
        int left = 0,right = nums.size()-1;
        while(left < right)
        {
            if(nums[left] + nums[right] == target)
                return {nums[left],nums[right]};
            else if(nums[left] + nums[right] > target)
                right--;
            else    
                left++;
        }
        return {};
    }
};
```

# [392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

```
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int i = 0,j = 0;
		int m = s.length(),n = t.length();
		while(i < m && j < n)
		{
			if(s[i] == t[j])
				i++;
			j++;
		}
		return i == m;
    }
};
```