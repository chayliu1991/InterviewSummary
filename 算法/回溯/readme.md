# [77. 组合](https://leetcode-cn.com/problems/combinations/)

```
class Solution {
public:
	vector<vector<int>> res;
	vector<int> path;
	
	void backtrace(int n, int k,int curr_index)
	{	
		if(path.size() == k)
		{
			res.push_back(path);
			return;
		}
		
		for(int i = curr_index;i <= n;i++)
		{
			path.push_back(i);
			backtrace(n,k,i+1);
			path.pop_back();
		}
	}
		
    vector<vector<int>> combine(int n, int k) {
		backtrace(n,k,1);
		return res;
    }
};
```

# [78. 子集](https://leetcode-cn.com/problems/subsets/)

```
class Solution {
public:
	vector<vector<int>> res;
	vector<int> path;
	
	void backtrace(vector<int>& nums,int curr_index)
	{
		res.push_back(path);
		for(int i = curr_index;i < nums.size();i++)
		{
			path.push_back(nums[i]);
			backtrace(nums,i+1);
			path.pop_back();
		}		
	}
	
    vector<vector<int>> subsets(vector<int>& nums) {
		backtrace(nums,0);
		return res;
    }
};
```

# [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

```
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        std::vector<std::vector<int>> res;
        std::vector<int> path;
        backtrack(candidates,target,res,path,0);
        return res;
    }

    void backtrack(vector<int>& candidates, int target,std::vector<std::vector<int>>& res,std::vector<int>& path,int curr)
    {
        int sum = std::accumulate(path.begin(),path.end(),0);
        if(sum > target)
            return;

        if(sum == target)
        {
            res.emplace_back(path);
            return;
        }

        for(int i = curr;i < candidates.size();i++)
        {
            path.emplace_back(candidates[i]);
            backtrack(candidates,target,res,path,i); //@ 元素可以重复
            path.pop_back();
        }
    }
};
```

# [46. 全排列](https://leetcode-cn.com/problems/permutations/)

```
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        std::vector<std::vector<int>> res;
        backtrack(nums,res,0);
        return res;
    }

    void backtrack(vector<int>& nums,std::vector<std::vector<int>>& res,int curr)
    {
        if(curr == nums.size())
        {
            res.emplace_back(nums);
            return;
        }
        
        for(int i = curr;i < nums.size();i++)
        {
            std::swap(nums[curr],nums[i]);
            backtrack(nums,res,curr+1);
            std::swap(nums[curr],nums[i]);
        }
    }
};
```

# [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

```
class Solution {
private:
    vector<vector<int>> res;
	vector<int> path;
	
	
    void backtrace (vector<int>& nums, vector<int>& path, vector<bool>& used) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            // 这里理解used[i - 1]非常重要 
            // used[i - 1] == true，说明同一树支nums[i - 1]使用过 
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) { 
                continue;
            }
            if (used[i] == false) {
                used[i] = true;
                path.push_back(nums[i]);
                backtrace(nums, path, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }

public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);        
        backtrace(nums, path, used);
        return res;

    }
};
```

# [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

```
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if(board.empty() || board[0].empty())
            return false;
        for(int i = 0;i < board.size();i++)
        {
            for(int j = 0;j < board[0].size();j++)
            {
                if(board[i][j] == word[0] && backtrack(board,word,i,j,0))
                    return true;
            }
        }
        return false;
    }

    bool backtrack(vector<vector<char>>& board, string word,int i,int j,int curr)
    {
        if(i < 0 || j < 0 || i >= board.size() || j >= board[i].size() || board[i][j] != word[curr])
            return false;
        if(curr == word.length()-1)
            return true;
        int dx[] = {1,0,-1,0},dy[] = {0,-1,0,1};
        char tmp = board[i][j];
        board[i][j] = '.';
        for(int k = 0;k < 4;k++)
        {
            int x = dx[k] + i,y = dy[k] + j;
            if(backtrack(board,word,x,y,curr + 1))
                return true;
        }
        board[i][j] = tmp;
        return false;
    }
};
```

