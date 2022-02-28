# BFS 模板

BFS 算法本质上就是从一个图的起点出发开始搜索找到目标终点完成搜索。

BFS 的整个解决分成下边几个步骤：

- 起点入队列
- 以队列非空为循环条件，进行节点扩散（将所有队列节点出队（同时判断出队节点是否为目标节点），获取其邻接结点）
- 判断获取的节点是否已被遍历，未被遍历节点入队

```
int BFS(start,target){
     Queue q; 
     Set visited: 
     int step = 0; 

     q.add(start);
     visited.add(start);
     while(q not empty) {
         int sz = q.size();

         for(int i =0 ; i < sz; i++) 
		 {
             cur = q.front();
			 q.pop();
             if(cur is target) {
                 return;
             }

             for(Node n:cur.adjs) 
			 {
                 if(n is not int visited) {
                     visitd.add(n);
                     q.add(n);
                 }
             }
         }
     }
 }
```

# DFS 模板

深度优先搜索（DFS）类似于树的先序遍历。在搜索时会尽可能的沿着一条所有路径进行搜索，直到该条路径上所有节点搜索完成，然后切换到另一条路径上进行搜索，直到图的所有节点全部都被遍历。

深度优先搜索整个过程可以分成如下步骤：

- 判断终止条件
- 对节点进行访问并加入到访问集合中
- 以当前节点的邻接结点为起点，通过递归向更深层次进行搜索

在实现上DFS一般使用递归或栈来实现。

```
 set visited;
 void DFS(start) {     
     if(shoud be end) //@ 结束条件
         return;     
     visited.add(start);
     //@ 递归向更深次进行遍历
     for(Node n:start.adjs) 
     {
         if(n is not visited)
             DFS(n);   
     }
 }
```

# 回溯模板

**DFS 不需要回复状态，回溯需要恢复状态。**

```
vector<vector<string>> result;
vector<string> path; 

void backtrace(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtrace(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

# [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

```
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        if(isConnected.empty())
            return 0;
        int n = isConnected.size();
        int res = 0;
        std::vector<bool> visited(n);
        for(int i = 0;i < n;i++)
        {
            if(!visited[i])
            {
                res++;
                std::queue<int> q;
                q.emplace(i);
                while(!q.empty())
                {
                    auto city = q.front();
                    q.pop();
                    visited[city] = true;
                    for(int j = 0;j < n;j++)
                    {
                        if(isConnected[city][j] == 1 && !visited[j])
                            q.emplace(j);
                    }
                }
            }
        }
        return res; 
    }
};
```

```
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
         if(isConnected.empty())
            return 0;
        int n = isConnected.size();
        int res = 0;
        std::vector<bool> visited(n);
        for(int i = 0;i < n;i++)
        {
            if(!visited[i])
            {
                res++;
                dfs(isConnected,visited,i);
            }
        }
        return res;
    }

    void dfs(vector<vector<int>>& isConnected,std::vector<bool> &visited,int curr)
    {
        int n = isConnected.size();
        for(int i = 0;i < n;i++)
        {
            if(isConnected[curr][i] && !visited[i])
            {
                visited[i] = true;
                dfs(isConnected,visited,i);
            }
        }
    }
};
```

# [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

````
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
		if(grid.empty())
			return 0;
		int res = 0;
		int dx[] = {0,1,-1,0},dy[] = {1,0,0,-1};
		int rows = grid.size(),cols = grid[0].size();
		std::vector<std::vector<bool>> visited(rows,std::vector<bool>(cols));
		for(int i = 0;i < rows;i++)
		{
			for(int j = 0;j < cols;j++)
			{
				if(grid[i][j] == '1' && !visited[i][j])
				{
					visited[i][j] = true;
					std::queue<std::pair<int,int>> q;
					q.push({i,j});
					res += 1;
					
					while(!q.empty())
					{
						auto land = q.front();
						q.pop();
						int x = land.first,y = land.second;
						for(int i = 0; i < 4;i++)
						{
							int m = x + dx[i],n = y + dy[i];
							if(m >=0 && m < rows && n >= 0 && n < cols && grid[m][n] == '1' && !visited[m][n])
							{
								visited[m][n] = true;
								q.push({m,n});
							}
						}
						
					}
				}
			}
		}
		return res;
    }
};
````

```
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
		if(grid.empty() || grid[0].empty())
			return 0;
		
		int res = 0;
		int rows = grid.size(),cols = grid[0].size();
		for(int i = 0;i < rows;i++)
		{
			for(int j = 0;j < cols;j++)
			{
				if(grid[i][j] == '1'){
					res += 1;
					dfs(grid,i,j);
				}
			}
		}		
		return res;
    }
	
	void dfs(vector<vector<char>>& grid,int i,int j)
	{
		if(i < 0 || j < 0 || i >= grid.size() || j >= grid[0].size() || grid[i][j] == '0')
			return;
		grid[i][j] = '0';
		int dx[] = {1,0,-1,0},dy[] = {0,1,0,-1};
		for(int k = 0;k < 4;k++)
		{
			int x = dx[k] + i,y = dy[k] + j;
			dfs(grid,x,y);
		}
	}
};
```



# [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

```
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
		if(grid.empty())
			return 0;
		int res = 0;
		int dx[] = {0,1,-1,0},dy[] = {1,0,0,-1};
		int rows = grid.size(),cols = grid[0].size();
		std::vector<std::vector<bool>> visited(rows,std::vector<bool>(cols));
		for(int i = 0;i < rows;i++)
		{
			for(int j = 0;j < cols;j++)
			{
				if(grid[i][j] == 1 && !visited[i][j])
				{
					int curr = 0;
					visited[i][j] = true;
					std::queue<std::pair<int,int>> q;
					q.push({i,j});
			
					while(!q.empty())
					{
						curr++;
						auto land = q.front();
						q.pop();
						int x = land.first,y = land.second;
						for(int i = 0; i < 4;i++)
						{
							int m = x + dx[i],n = y + dy[i];
							if(m >=0 && m < rows && n >= 0 && n < cols && grid[m][n] == 1 && !visited[m][n])
							{
								visited[m][n] = true;
								q.push({m,n});
							}
						}
						
					}
					res = std::max(res,curr);
				}
			}
		}
		return res;
    }
};
```

```
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        if(grid.empty())
            return 0;
        int res  = 0;
        for(int i = 0;i < grid.size();i++)
        {
            for(int j = 0;j < grid[0].size();j++)
            {
                if(grid[i][j] == 1)
                    res = max(res,dfs(grid,i,j));
            }
        }
        return res;
    }

    int dfs(vector<vector<int>>& grid,int i,int j)
    {
        if(i < 0 || j <0 || i >= grid.size() || j >= grid[0].size() || grid[i][j] == 0)
            return 0;

        grid[i][j] = 0;
        int count = 1;
		int dx[] = {1,0,-1,0} ,dy[] = {0,1,0,-1};
        for(int k = 0;k < 4;k++)
		{
			int x = dx[k] + i,y = dy[k] + j;
			count += dfs(grid,x,y);
		}
        return count;
    }
};
```

# [542. 01 矩阵](https://leetcode-cn.com/problems/01-matrix/)

```
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
		if(mat.empty())
			return {};
		int dx[] = {0,1,-1,0},dy[] = {1,0,0,-1};
		int rows = mat.size(),cols = mat[0].size();
		std::vector<std::vector<bool>> visited(rows,std::vector<bool>(cols));
		std::vector<std::vector<int>> res(rows,std::vector<int>(cols)); 	
        queue<pair<int,int>> q;
		for(int i = 0;i < rows;i++)
		{
			for(int j = 0;j < cols;j++)
			{
				if(mat[i][j] == 0)
				{
					q.emplace(i,j);
					visited[i][j] = true;
				}
			}
		}
		
		while(!q.empty())
        {
            auto it= q.front();
            q.pop();
            int row = it.first,col = it.second;
            for(int k = 0;k < 4;k++)
            {
                int x = row + dx[k];
                int y = col + dy[k];
                if(x >= 0 && x < rows && y >= 0 && y < cols && !visited[x][y])
                {
                    res[x][y] = res[row][col] + 1;
                    q.emplace(x,y);
                    visited[x][y] = true;
                }
            }
        }
        return res;
    }
};
```


# [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
		if(digits.empty())
			return {};
		std::string path;
		std::vector<std::string> res;
		dfs(digits,path,res,0);	
        return res;
    }
	
	void dfs(const std::string& digits,std::string& path,std::vector<std::string>& res,int index)
	{
		std::unordered_map<char,string> dict = {
        {'2', "abc"}, {'3', "def"}, {'4', "ghi"}, {'5', "jkl"}, {'6', "mno"},
        {'7', "pqrs"}, {'8', "tuv"}, {'9', "wxyz"}};
	
		if(index == digits.size())
		{
			res.push_back(path);
			return;
		}
		
		for(int i = 0;i < dict[digits[index]].size();i++)
        {
            path.push_back(dict[digits[index]][i]);
            dfs(digits,path,res,index+1);
            path.pop_back();
        }
	}	
};
```

# [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

```
class Solution {
public:
	void dfs(vector<vector<char>>& board,int i,int j)
	{
		if(i < 0 || j < 0 || i >= board.size() || j >= board[0].size() || board[i][j] != 'O')
			return;	
		
		board[i][j] = 'A';
		vector<vector<int>> dir{{1,0},{-1,0},{0,1},{0,-1}};
		for(const auto d : dir)
			dfs(board,i+d[0],j+d[1]);		
	}

    void solve(vector<vector<char>>& board) {
        if(board.empty())
			return;
		int m = board.size(),n = board[0].size();
		for(int i = 0;i < m;i++)
		{
			dfs(board,i,0);
			dfs(board,i,n-1);
		}
		
		for(int i = 1;i < n -1;i++)
		{
			dfs(board,0,i);
			dfs(board,m-1,i);
		}
		
		for(int i = 0;i < m;i++)
		{
			for(int j = 0;j < n;j++)
			{
				if(board[i][j] == 'A')
					board[i][j] = 'O';
				else if(board[i][j] == 'O')
					board[i][j] = 'X';
			}
		}		
    }
};
```

# [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

```
class Solution {
public:
    std::vector<std::vector<bool>> Visited;

    int islandPerimeter(vector<vector<int>>& grid) {
        if(grid.empty())
            return 0;
        int rows = grid.size(),cols = grid[0].size();
        Visited = std::vector<std::vector<bool>> (rows,std::vector<bool>(cols));

        int res = 0;
        for(int i = 0;i < rows;i++)
        {
            for(int j = 0;j < cols;j++)
            {
                if(grid[i][j] == 1 && !Visited[i][j])
                    res += dfs(grid,rows,cols,i,j);
            }
        }
        return res;
    }

    int dfs(vector<vector<int>>& grid,int rows,int cols,int i,int j)
    {
        if(i < 0 || j < 0 || i >= rows || j >= cols || grid[i][j] == 0)
            return 1;
        if(Visited[i][j])
            return 0;
        Visited[i][j] = true;

        int res  = 0;
        int dx[] = {1,0,-1,0},dy[] = {0,1,0,-1};
        for(int k = 0;k < 4;k++)
        {
            int x = dx[k] + i,y =dy[k]+j;
            res += dfs(grid,rows,cols,x,y);
        }

        return res;
    }
};
```

# [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

```
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        if(n <= 0)
            return {};
        
        std::vector<string> res;
        dfs(res,"",n,0,0);
        return res;
    }

    void dfs(vector<string>& res,string path,int n,int lp,int rp) 
    {
        //@ dfs 的条件是右括号数量不能大于左括号，左、右括号的长度都不能大于n
        if(rp > lp || rp > n || lp > n)
            return;
        if(rp == lp && lp == n)
            res.emplace_back(path);
        dfs(res,path+'(',n,lp+1,rp);
        dfs(res,path+')',n,lp,rp+1);
    }
};
```
# [77. 组合](https://leetcode-cn.com/problems/combinations/)

```
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        vector<int> path;
        backtrack(n,k,res,path,1);
        return res;
    }

    void backtrack(int n, int k,vector<vector<int>>& res, vector<int>& path,int curr)
    {
        if(path.size() > k)
            return;
        if(path.size() == k){
            res.emplace_back(path);
            return;
        }
        
        for(int i = curr;i <= n;i++)
        {
            path.emplace_back(i);
            backtrack(n,k,res,path,i+1);
            path.pop_back();
        }
    }

};
```

# [78. 子集](https://leetcode-cn.com/problems/subsets/)

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        if(nums.empty())
            return {};
        vector<int> path;
        vector<vector<int>> res;
        backtrack(nums,res,path,0);
        return res;
    }

    void backtrack(vector<int>& nums,vector<vector<int>>& res,vector<int>& path,int curr)
    {
        if(curr > nums.size())
            return;
        res.emplace_back(path);
        for(int i = curr;i < nums.size();i++)
        {
            path.emplace_back(nums[i]);
            backtrack(nums,res,path,i+1);
            path.pop_back();
        }
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

# [12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

```
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if(board.empty() || word.empty())
            return false;
        int rows = board.size(),cols = board[0].size();
        for(int i = 0;i < rows;i++)
        {
            for(int j = 0;j < cols;j++)
            {
                if(board[i][j] == word[0])
                {
                    if(backtrack(board,word,rows,cols,0,i,j))
                        return true;
                }
            }
        }
        return false;
    }

    bool backtrack(vector<vector<char>>& board,string word,int rows,int cols,int len,int i,int j)
    {
        if(i < 0 || i >= rows || j < 0 || j >= cols || board[i][j] != word[len])
            return false;

        if(len == word.length()-1)
            return true;

        char tmp = board[i][j];
        board[i][j] = '\0';
        int dx[] = {1,0,-1,0},dy[] = {0,1,0,-1};
        for(int k = 0;k < 4;k++)
        {
            int x = dx[k] + i,y = dy[k] + j;
            if(backtrack(board,word,rows,cols,len+1,x,y))
                return true;
        }
        board[i][j]  = tmp;
        return false;
    }
};
```

# [17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

```
class Solution {
public:
    vector<int> printNumbers(int n) {
        std::vector<int> res;
        std::string str(n,'0');
        back_trace(res,str,0,n);
        return res;
    }

    void back_trace(std::vector<int>& res,std::string& str,int idx,int n)
    {
        if(idx == n)
        {
            int val = std::stoi(str);
            if(val)
                res.push_back(val);
            return;
        }

        for(char i = '0';i <= '9';i++)
        {
            str[idx] = i;
            back_trace(res,str,idx + 1,n);
        }
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

