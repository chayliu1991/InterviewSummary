# [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

```
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for(const auto & num : nums)
        {
            int pos = std::abs(num) -1;
            if(nums[pos] > 0)
                nums[pos] = - nums[pos];
        }

        std::vector<int> res;
        for(int i = 0;i < nums.size();i++)
        {
            if(nums[i] > 0)
            {
                res.push_back(i+1);
            }
        }

        return res;
    }
};
```

# [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
		if(matrix.empty() || matrix[0].empty())
			return;
		int n = matrix.size();
		int i = 0,j = n -1;
		while(i < j)
		{
			for(int k = 0;k < n;k++)
				std::swap(matrix[i][k],matrix[j][k]);
			i++,j--;
		}
		
		for(int i = 0;i < n;i++)
		{
			for(int j = 0;j < i;j++)
			{
				std::swap(matrix[i][j],matrix[j][i]);				
			}
		}
    }
};
```

```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int temp = 0,n = matrix.size()-1;
        for(int row = 0;row <= n/2;row++)
        {
            for(int col = row;col < n-row;col++)
            {
                temp = matrix[col][n-row];
                matrix[col][n-row] = matrix[row][col];
                matrix[row][col] = matrix[n-col][row];
                matrix[n-col][row] = matrix[n-row][n-col];
                matrix[n-row][n-col] = temp;
            }
        }
    }
};
```

# [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int index = 0;
        for(int i = 0;i < nums.size();++i)
        {
            if(nums[i] != val)
                nums[index++] = nums[i];
        }
        return index;
    }
};
```

# [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
		int n = nums.size();
		if(n <= 1)
			return n;
		int index = 1;
		for(int i = 1;i < n;i++)
		{
			if(nums[i-1] != nums[i])
				nums[index++] = nums[i];
		}
		return index;
    }
};
```

# [80. 删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
		int n = nums.size();
		if(n <= 2)
			return n;
		int index = 2;
		for(int i = 2;i < n;i++)
		{
			if(nums[index-2] != nums[i])
				nums[index++] = nums[i];
		}
		return index;
    }
};
```

# [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map<int,int> hash;
        for(int i = 0;i < nums.size();i++)
        {
            if(hash.find(target-nums[i]) != hash.end())
                return {hash[target-nums[i]],i};
            hash[nums[i]] = i;
        }
        return {};
    }
};
```

# [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
		int n = nums.size();
        if(n < 3)
            return {};
		std::sort(nums.begin(),nums.end());
		std::vector<std::vector<int>> res;
		
		for(int i = 0;i < n;i++)
		{
			if(nums[i] > 0)
				break;
			if(i > 0 && nums[i] == nums[i-1])
				continue;
			int target = -nums[i];
			int left = i + 1,right = n - 1;
			while(left < right)
			{
				int sum = nums[left] + nums[right];
				if(sum == target)
				{
					res.emplace_back(std::vector<int>{nums[i],nums[left],nums[right]});
					left++,right--;
					while(left < right && nums[left] == nums[left-1])
						left++;
					while(left < right && nums[right] == nums[right+1])
						right--;
				}			
				else if(sum > target)
					right--;
				else
					left++;
			}
		}
		
		return res;
    }
};
```

# [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

```
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& mat, int r, int c) {
		int rows = mat.size(),cols = mat[0].size();
		if(rows * cols != r * c)
			return mat;
		vector<vector<int>> res(r,std::vector<int>(c));
		int k = 0;
		for(int i = 0;i < r;i++)
		{
			for(int j = 0;j < c;j++)
			{
				res[i][j] = mat[k/cols][k%cols];
				k++;
			}
		}
		return res;
    }
};
```

# [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

```
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
		if(intervals.empty())
			return {};
		std::vector<std::vector<int>> res;
		std::sort(intervals.begin(),intervals.end());
		res.emplace_back(intervals[0]);
		for(int i = 1;i < intervals.size();i++)
		{
			auto& curr = intervals[i];
			if(curr[0] <= res.back()[1])
				res.back()[1] = std::max(curr[1],res.back()[1]);
			else
				res.emplace_back(curr);
		}
		return res;
    }
};
```

# [435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

```
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
		if(intervals.empty())
			return 0;
		int res = 0;
		std::sort(intervals.begin(),intervals.end());
		int last = 0;
		for(int i = 1;i < intervals.size();i++)
		{
			auto& curr = intervals[i];
			if(curr[0] < intervals[last].back())
			{
				res += 1;
				if(curr[1] < intervals[last][1])
					last = i;
			}
			else
				last = i;
		}		
		return res;
    }
};
```

# [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

```
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
		if(points.empty())
			return 0;
		std::sort(points.begin(), points.end());
		int res = 1;
		int last_pos = points[0][1];
		for(int i = 1;i < points.size();i++)
		{
			if(points[i][0] <= last_pos)
				last_pos = std::min(points[i][1],last_pos);
			else
			{
				res++;
				last_pos = points[i][1];
			}
		}
		return res;
    }
};
```

# [57. 插入区间](https://leetcode-cn.com/problems/insert-interval/)

```
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
		intervals.emplace_back(newInterval);
		std::vector<std::vector<int>> res;
		std::sort(intervals.begin(),intervals.end());
		res.emplace_back(intervals[0]);
		for(int i = 1;i < intervals.size();i++)
		{
			auto& curr = intervals[i];
			if(curr[0] <= res.back()[1])
				res.back()[1] = std::max(curr[1],res.back()[1]);
			else
				res.emplace_back(curr);
		}
		return res;
    }
};

```

# [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

```
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty())
            return {};
      
        
		int dx[] = {0,1,0,-1},dy[] ={1,0,-1,0};
		int rows = matrix.size(),cols = matrix[0].size();
		std::vector<std::vector<bool>> visited(rows,std::vector<bool>(cols));
		int n = rows * cols;
		std::vector<int> res(n);
		
		int x = 0,y = 0,index = 0;		
        for (int i = 0; i < n; i++)
		{
            res[i] = matrix[x][y];
            visited[x][y] = true;
            int tx = x + dx[index], ty = y + dy[index];
            if (tx < 0 || tx >= rows || ty < 0 || ty >= cols || visited[tx][ty]) 		
                index = (index + 1) % 4;
        
            x += dx[index];
            y += dy[index];
        }
        return res;
    }
};
```

# [661. 图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

```
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& img) {
		if(img.empty() || img[0].empty())
			return {};
		int rows = img.size(),cols = img[0].size();
		std::vector<std::vector<int>> res(rows,std::vector<int>(cols));
		int dx[] = {-1,-1,-1,0,0,0,1,1,1},dy[] = {-1,0,1,-1,0,1,-1,0,1};
		for(int i = 0;i < rows;i++)
		{
			for(int j = 0;j < cols;j++)
			{
				int sum = 0;
				int count = 0; 
				for(int k = 0;k < 9;k++)
				{
					int x = i + dx[k],y = j + dy[k];
					if(x >= 0 && x < rows && y >= 0 && y <cols){
						sum += img[x][y];
						count ++;
					}
				}
				res[i][j] = sum / count;
			}
		}
		return res;
    }	
};
```

