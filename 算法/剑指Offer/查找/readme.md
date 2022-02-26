# [03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

```
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        std::unordered_map<int,int> dict;
        for(const auto n : nums)
        {
            if(dict.find(n) == dict.end())
                dict[n] = 1;
            else
                return n;
        }
        return -1;
    }
};
```

# [04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

- 二叉搜索树

```
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty())
            return false;
        
        int rows = matrix.size(),cols = matrix[0].size();
        int i = 0,j = cols - 1;
        while(i < rows && j >= 0)
        {
            if(matrix[i][j] == target)
                return true;
            else if(matrix[i][j] > target)
                j--;
            else 
                i++;
        }
        return false;
    }
};
```
-  二分查找
```
class Solution {
public:

    bool binary_search(vector<int>& vec,int target)
    {
        int left = 0,right = vec.size() - 1;
        while(left <= right)
        {
            int mid = (left + right) >> 1;
            if(vec[mid] == target)
                return true;
            if(vec[mid] > target)
                right = mid - 1;
            else
                left = mid + 1;
        }
        return false;
    }

    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty())
            return false;
        for(int i = 0;i < matrix.size();i++)
        {
            bool res = binary_search(matrix[i],target);
            if(res)
                return true;
        }
        return false;
    }
};
```

# [11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

- 遍历

```
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if(numbers.empty())
            return -1;
        int res = numbers[0];
        for(const auto n : numbers)
        {
            if(res > n)
            {
                res = n;
                break;
            }
        }
        return res;
    }
};
```

- 二分查找

```
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if(numbers.empty())
            return -1;
        int left = 0,right = numbers.size() -1;
        while(left < right)
        {
            int mid = (left +  right) >> 1;
            if(numbers[mid] > numbers[right])
                left = mid + 1;
            else if(numbers[mid] < numbers[right])
                right = mid;
            else
                right--;
        }
        return numbers[left];
    }
};
```

# [50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

- 遍历+哈希

```
class Solution {
public:
    char firstUniqChar(string s) {
        std::unordered_map<char,int> hash;
        for(const auto c : s)
            hash[c]++;
        for(const auto c : s)
        {
            if(hash[c] == 1)
                return c;
        }
        return ' ';
    }
};
```



