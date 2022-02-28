# 动态规划

## [10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

```
class Solution {
public:
    int fib(int n) {
        if(n == 0)
            return 0;
        if(n == 1)
            return 1;
        int left = 0,right = 1;
        for(int i = 2;i <= n;i++)
        {
            int tmp = (left + right) % 1000000007;
            left = right;
            right = tmp;
        }
        return right;
    }
};
```

##  [10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

```
class Solution {
public:
    int numWays(int n) {
        if(n < 0)
            return -1;
        if(n == 0 || n == 1)
            return 1;
        if(n == 2)
            return 2;
        int left = 1,right = 2;
        for(int i = 3;i <= n;i++)
        {
            int tmp = (left + right) % 1000000007;
            left = right;
            right = tmp;
        }
        return right;
    }
};
```

## [46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

```
class Solution {
public:
    int translateNum(int num) {
        std::string s = std::to_string(num);
		if(s.empty())
			return 0;
		int n = s.size();
		std::vector<int> dp(n+1,1);
		int prev = s[0] - '0';
		for(int i = 2;i <= n;i++)
		{
			int curr = s[i-1] - '0';
			if(prev == 1 || prev == 2 && curr < 6)
				dp[i] = dp[i-1] + dp[i-2];
			else
				dp[i] = dp[i-1];
            prev = curr;
		}
		return dp.back();
    }
};
```

## [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

```
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        if(s[0] == '0' || n == 0)
            return 0;
        if(n == 1)
            return 1;
        std::vector<int> dp(n+1,1);
        int prev = s[0] - '0';
        for(int i = 2;i <= n;i++)
        {
            int curr = s[i-1] - '0';
            if((prev == 0 || prev > 2) && curr == 0)
                return 0;
            if(prev == 1 || prev == 2 && curr < 7)
            {
                if(curr == 0)
                    dp[i] = dp[i-2];
                else
                     dp[i] = dp[i-1] + dp[i-2];
            }
            else
                dp[i] = dp[i-1];
            prev = curr;
        }
        return dp.back();
    }
};
```

## [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

```
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int n = s.length();
        std::vector<bool> dp(n+1);
        dp[0] = true;

        for(int i = 1;i <= n;i++)
        {
            for(int j = 0;j < i;j++)
            {
                auto curr = s.substr(j,i-j);
                if(dp[j] && std::find(wordDict.begin(),wordDict.end(),curr) != wordDict.end())
                {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp.back();
    }
};
```

## [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

```
class Solution {
public:
    int integerBreak(int n) {
        std::vector<int> dp(n+1,1);
        for(int i = 2;i <= n;i++)
        {
            for(int j = 1;j < i;j++)
            {
                dp[i] = std::max(dp[i],std::max(j*(i-j),dp[i-j]*j));
            }
        }
        return dp.back();
    }
};
```

## [49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

```
class Solution {
public:
    int nthUglyNumber(int n) {
        if(n <= 3)
            return n;
        std::vector<int> dp(n,1);
        int i = 0,j = 0,k = 0;
        for(int index = 1;index < n;index++)
        {
            int n2 = dp[i]*2,n3 = dp[j]*3,n5 = dp[k]*5;
            dp[index] = std::min(n2,std::min(n3,n5));
            if(n2 == dp[index])
                i++;
            if(n3 == dp[index])
                j++; 
            if(n5 == dp[index])
                k++;    
        }
        return dp.back();
    }
};
```

##  [413. 等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/)

```
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& nums) {
        size_t n = nums.size();
        if(n <= 2)
            return 0;

        std::vector<int> dp(n,0);
        for(size_t i = 2; i < n;++i)
        {
            if(nums[i-1] - nums[i -2] == nums[i] - nums[i-1])
                dp[i] = dp[i-1] + 1;
        }
        return std::accumulate(dp.begin(),dp.end(),0);
    }
};
```

## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

```
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.empty())
            return 0;
        
        size_t rows = grid.size(),cols = grid[0].size();
        std::vector<int> dp(cols,0);
        dp[0] = grid[0][0];
        for(size_t i = 1;i < cols;++i)
            dp[i] = grid[0][i] + dp[i-1];
        
        for(size_t i = 1;i < rows;++i)
        {
            for(size_t j = 0;j < cols;++j)
            {
                if(j == 0)
                    dp[j] += grid[i][0];
                else
                    dp[j] = std::min(dp[j-1],dp[j]) +  grid[i][j];
            }
        }
        return dp.back();
    }
};
```

## [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

```
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        if(triangle.empty() || triangle[0].empty())
            return 0;
        int rows = triangle.size(),cols = triangle[0].size();
        std::vector<int> dp(triangle[rows-1].begin(),triangle[rows-1].end());
        for(int i = rows-2;i >= 0;i--)
        {
            for(int j = 0;j < triangle[i].size();j++)
                dp[j] = std::min(dp[j],dp[j+1]) + triangle[i][j];
        }
        return dp.front();
    }
};
```

## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        std::vector<int> dp(n,1);

        for(int i=1;i < m;++i)
        {
            for(int j=1;j < n;++j)
            {
                dp[j] += dp[j-1];
            }
        }
        return dp.back();
    }
};
```

## [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if(obstacleGrid.empty() || obstacleGrid[0].empty())
            return 0;
        
        int rows = obstacleGrid.size(),cols = obstacleGrid[0].size();
        std::vector<int> dp(cols,0);
        dp[0] = (obstacleGrid[0][0] == 0);

        for(int i = 0;i < rows;++i)
        {
            for(int j = 0;j < cols;++j)
            {
                if(obstacleGrid[i][j] == 1)
                {
                    dp[j] = 0;
                    continue;
                }
                
                if(j-1 >= 0 && obstacleGrid[i][j-1] == 0)
                    dp[j] += dp[j-1];
            }
        }
        return dp.back();
    }
};
```

##  [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

```
class Solution {
public:
    int numSquares(int n) {
        if(n == 0)
            return 0;
        std::vector<int> dp(n+1,n+1);
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <= n;i++)
        {
            for(int j = 1;j * j <= i;j++)
            {
                dp[i] = std::min(dp[i],dp[i-j*j]+1);
            }
        }
        return dp.back();
    }
};
```


## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

```
class Solution {
public:
    int rob(vector<int>& nums) {
        size_t n = nums.size();

        if(n == 0)
            return 0;
        
        if(n == 1U)
            return nums[0];           
        
        int prev = 0,next = 0,res;
        for(size_t i = 0;i < n;++i)
        {
            res = max(prev + nums[i],next);
            prev = next;
            next =  res;
        }
        return res;
    }
};
```

## [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

```
class Solution {
public:
    int rob(vector<int>& nums) {
		if(nums.empty())
			return 0;

		int n = nums.size();
		if(n <= 2)
			return *max_element(nums.begin(),nums.end());
		
		return std::max(rob_range(nums,0,n-2),rob_range(nums,1,n-1));
    }
	
	int rob_range(const vector<int>& nums,int start,int end)
	{
		int first = nums[start],second = std::max(nums[start],nums[start+1]);
		for(int i = start + 2;i <= end;i++)
		{
			int tmp = second;
			second = std::max(first+nums[i],second);
			first = tmp;
		}
		return second;
	}
};
```

## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
		int n = prices.size();
		if(n <= 1)
			return 0;
		int res = 0,value = prices[0];
		for(int i = 1;i < n;i++)
		{
			value = std::min(value,prices[i]);
			res = std::max(res,prices[i]-value);				
		}
		return res;
    }
};
```

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {	
		int n = prices.size();
		if(n <= 1)
			return 0;
		
		std::vector<std::vector<int>> dp(n,std::vector<int>(2));
		dp[0][1] = -prices[0];
		for(int i = 1;i < n;i++)
        {
            dp[i][0] = std::max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1] = std::max(dp[i-1][1],-prices[i]);
         }
        return dp.back()[0];
    }
};
```

## [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
		int n = prices.size();
		if(n <= 1)
			return 0;
		int res = 0;
		for(int i = 1;i < n;i++)
		{
			int profit = prices[i] - prices[i-1];
			res += profit > 0 ? profit : 0;
		}
		return res;
    }
};
```

## [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        size_t n = prices.size();
        if(n < 2)
            return 0;
        
        std::vector<std::vector<int>>  dp_none(n,std::vector<int>(3,0));
        std::vector<std::vector<int>>  dp_hold(n,std::vector<int>(3,0));

        for(size_t i = 0;i < 3;++i)
            dp_hold[0][i] = -prices[0];
        
        for(size_t i = 1;i < n;++i)
        {
            //@ 持有股票，但是没有发生交易，应该挑选最低的买入价格
            dp_hold[i][0] = std::max(dp_hold[i-1][0],-prices[i]);
            for(size_t j = 1;j < 3;++j)
            {
                //@ 第 i 天发生 j 次交易，不持有股票：i-1 天发生 j 笔交易，或者 第i-1 天发生 j-1 笔交易，并且手里有股票在第 i 天卖出
                dp_none[i][j] = std::max(dp_none[i-1][j],dp_hold[i-1][j-1]+prices[i]);
                //@ 第 i 天发生 j 次交易，持有股票：i-1 天发生了 j 笔交易，那么再考虑今天是否买入
                dp_hold[i][j] = std::max(dp_hold[i-1][j],dp_none[i-1][j] - prices[i]);
            }
        }
        
        return dp_none[n-1][2]; //@ 最后一天，交易两次
    }
};
```

## [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
		int n = prices.size();
		if(n <= 1)
			return 0;
		
		if(k > n/2)
		{
			int res = 0;
			for(int i = 1;i < n;i++)
			{
				int profit = prices[i] - prices[i-1];
				res += profit > 0 ? profit : 0;
			}		
			return res;
		}
		
		std::vector<std::vector<int>> dp_none(n,std::vector<int>(k+1));
		std::vector<std::vector<int>> dp_hold(n,std::vector<int>(k+1));
		
		for(int i = 0;i <= k;i++)
			dp_hold[0][i] = -prices[0];
		
		for(int i = 1;i < n;i++)
		{
			dp_hold[i][0] = std::max(dp_hold[i-1][0],-prices[i]);
			for(int j = 1;j <= k;j++)
			{
                dp_none[i][j] = std::max(dp_none[i-1][j],dp_hold[i-1][j-1]+prices[i]);
 				dp_hold[i][j] = std::max(dp_hold[i-1][j],dp_none[i-1][j]-prices[i]);
			}
		}
		return dp_none[n-1][k];
    }
};
```

## [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if(n < 2)
            return 0;
        std::vector<int> dp_cold(n),dp_hold(n),dp_none(n);
        dp_hold[0] = -prices[0];
        for(int i = 1;i < n;i++)
        {
            dp_none[i] = std::max(dp_none[i-1],dp_hold[i-1]+prices[i]);
            dp_hold[i] = std::max(dp_hold[i-1],dp_cold[i-1]-prices[i]);           
            dp_cold[i] = std::max(dp_cold[i-1],dp_none[i-1]);
        }
        return dp_none.back();
    }
};
```

## [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

```
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        if(n < 2)
            return 0;
        
        int none = 0,hold = -prices[0];
        for(int i = 1;i < n;++i)
        {
            int temp = none;
            none = std::max(none,hold+prices[i]-fee);
            hold = std::max(hold,temp-prices[i]);
        }
        return none;
    }
};
```

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        if(n == 0)
            return 0;
        std::vector<int> dp(n,1);
        dp[0] = nums[0];
        for(int i = 1;i < n;++i)
        {
            if(dp[i-1] < 0)
                dp[i] = nums[i];
            else
                dp[i] = dp[i-1] + nums[i];
        }
        return *max_element(dp.begin(),dp.end());
    }
};
```


## [354. 俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/)

```
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        if(envelopes.empty())
            return 0;

        std::sort(envelopes.begin(),envelopes.end(),[](vector<int>& e1,vector<int>& e2){
            if(e1[0] == e2[0])
                return e1[1] >= e2[1];
            return e1[0] > e2[0];
        });

        int n = envelopes.size();
        int res = 1;
        vector<int> dp(n,1);
        for(int i = 1;i < n;++i)
        {
            for(int j = 0;j < i;++j)
            {
                if(envelopes[j][0] > envelopes[i][0] && envelopes[j][1] > envelopes[i][1])
                    dp[i] = std::max(dp[i],dp[j]+1);                
            }
            res = std::max(res,dp[i]);
        }
        return res;
    }
};
```

## [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

```
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        if(nums1.empty() || nums2.empty())
            return 0;
        
        int len1 = nums1.size(),len2= nums2.size();
        int res = 0;
        std::vector<std::vector<int>> dp(len1+1,std::vector<int>(len2+1,0));

        for(int i = 0;i < len1;++i)
        {
            for(int j = 0;j < len2;++j)
            {
                if(nums1[i] == nums2[j])
                {
                    dp[i+1][j+1] = dp[i][j] + 1;
                    res = std::max(res,dp[i+1][j+1]);
                }
            }
        }
        return res;
    }
};
```

## [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        if(n == 0)
            return 0;
        
        std::vector<int> dp_min(n),dp_max(n);
        dp_min[0] = nums[0];
        dp_max[0] = nums[0];

        for(int i = 1;i < n;++i)
        {
            dp_max[i] = std::max(nums[i] * dp_min[i-1],std::max(nums[i],nums[i]*dp_max[i-1]));
            dp_min[i] = std::min(nums[i] * dp_min[i-1],std::min(nums[i],nums[i]*dp_max[i-1]));
        }
        return *std::max_element(dp_max.begin(),dp_max.end());
    }
};
```

## [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

```
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int total = std::accumulate(nums.begin(),nums.end(),0);
        int max_value = *std::max_element(nums.begin(),nums.end());
        if(total & 0x01 || n < 2 || max_value > total/2)
            return false;

        int target = total/2;
        if(max_value == target)
            return true;
        std::vector<std::vector<bool>> dp(n,std::vector<bool>(target+1));
        for(int i = 0;i < n;i++)
            dp[i][nums[i]] = true;

        for(int i = 1;i < n;i++)
        {
            dp[i][0] = true;
            for(int j = 1;j <= target;j++)
            {
                if(j >= nums[i])
                    dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i]];
                else
                    dp[i][j] = dp[i-1][j];
            }
        }
        return dp.back().back();
    }
};
```

## [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

```
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int MAX = amount + 1;
        std::vector<int> dp(amount+1,MAX);
        dp[0] = 0;

        for(int i = 1;i <= amount;i++)
        {
            for(const auto coin : coins)
            {
                if(i >= coin)
                    dp[i] = std::min(dp[i],dp[i-coin]+1);
            }
        }
        return dp.back() == MAX ? -1 : dp.back();
    }
};
```

## [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

```
class Solution {
public:
    int change(int amount, vector<int>& coins) {
		vector<int> dp(amount+1,0);
        dp[0] = 1;
        for(const auto coin : coins)
        {   
            for(int i = coin;i <= amount;++i)
                dp[i] += dp[i-coin];
        }
        return dp.back();
    }
};
```

## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        auto matches = [&](int i,int j){
            if(i == 0)
                return false;
            if(p[j-1] == '.')
                return true;
            return s[i-1] == p[j -1];
        };

        std::vector<std::vector<int>> dp(m+1,std::vector<int>(n+1));
        dp[0][0] = true;

        for(int i = 0;i <= m;++i)
        {
            for(int j = 1;j <= n;++j)
            {
                if(p[j-1] == '*')
                {
                    dp[i][j] |= dp[i][j-2];
                    if(matches(i,j-1))
                        dp[i][j] |= dp[i-1][j];
                }
                else
                {
                    if(matches(i,j))
                        dp[i][j] |= dp[i-1][j-1];
                }
            }
        }
        return dp[m][n];
    }
};
```

## [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
		if(nums.empty())
			return 0;
		int n = nums.size();
		int res = 1;
		std::vector<int> dp(n,1);
		for(int i = 1;i < n;i++)
		{
			for(int j = 0;j < i;j++)
			{
				if(nums[j] < nums[i])
					dp[i] = std::max(dp[i],dp[j]+1);
			}
			res = std::max(res,dp[i]);
		}	
        return res;	
    }
};
```
## [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```
class Solution {
public:
    string longestPalindrome(string s) {
		int n = s.size();
		if(n < 2)
			return s;
		std::vector<std::vector<int>> dp(n,std::vector<int>(n));
		for(int i = 0;i < n;i++)
			dp[i][i] = true;
		int start = 0,max_length = 1;
		for(int length = 2;length <= n;length++) //@ 字串长度
		{
			for(int i = 0;i < n;i++)  //@ 左边界
			{
				int j = length + i - 1; //@ 有边界
				if(j >= n)
					break;
				if(s[i] != s[j])
					dp[i][j] = false;
				else
				{
					if(j - i < 3)
						dp[i][j] = true;
					else
						dp[i][j] = dp[i+1][j-1];
				}
				
				if(dp[i][j] && j - i + 1 > max_length){
					max_length = j - i + 1 ;
					start = i;
				}
			}		
		}
		
		return s.substr(start,max_length);
    }
};
```

中心扩展：

```
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.length();
        if(n < 2)
            return s;
        int start = 0,max_len = 0;
        for(int i= 0;i < n*2-1;i++)
        {
            int left = i >> 1;
            int right = left + i % 2;
            while(left >= 0 && right < n && s[left] == s[right])
            {
                if(right - left + 1 > max_len)
                {
                    max_len = right - left + 1;
                    start = left;
                }
                left--;
                right++;
            }          
        }
        return s.substr(start,max_len);
    }
};
```

## [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

```
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
		if(text1.empty() || text2.empty())
			return 0;
		int len1 = text1.length(),len2 = text2.length();
		std::vector<std::vector<int>> dp(len1+1,std::vector<int>(len2+1));
		for(int i = 1;i <= len1;i++)
		{
			for(int j = 1;j <= len2;j++)
			{
				if(text1[i-1] == text2[j-1])
					dp[i][j] = dp[i-1][j-1] + 1;
				else
					dp[i][j] = std::max(dp[i-1][j],dp[i][j-1]);
			}
		}
		return dp.back().back();
    }
};
```

## [19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)

```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        dp[0][0] = true;

        for(int j = 2; j <= n; j += 2)
            dp[0][j] = dp[0][j - 2] && p[j-1] == '*';

        for(int i = 1; i <= m; i++) 
        {
            for(int j = 1; j <= n; j++) 
            {
                if(p[j - 1] == '*') 
                {
                    if(dp[i][j - 2]) 
                        dp[i][j] = true;                            
                    else if(dp[i - 1][j] && (s[i-1] == p[j-2] || p[j-2] == '.')) 
                        dp[i][j] = true;   
                } 
                else 
                {
                    if(dp[i-1][j-1] && s[i-1] == p[j-1])
                        dp[i][j] = true;
                    else if(dp[i - 1][j - 1] && p[j - 1] == '.') 
                        dp[i][j] = true;
                }
            }
        }
        return dp[m][n];
    }
};
```

## [42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
		if(nums.empty())
			return 0;
		
		int res = INT_MIN,curr = 0;
		for(const auto n : nums)
		{
			if(curr > 0)
				curr += n;
			else
				curr = n;
			res = std::max(res,curr);
		}
		return res;
    }
};
```


## [47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

```
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int rows = grid.size(),cols = grid[0].size();
        std::vector<std::vector<int>> dp(rows,std::vector<int>(cols));
        dp[0][0] = grid[0][0];
        for(int i = 1;i < rows;i++)
            dp[i][0] = dp[i-1][0] + grid[i][0];
        for(int j =1;j < cols;j++)
            dp[0][j] = dp[0][j-1] + grid[0][j];
        
        for(int i = 1;i < rows;i++)
        {
            for(int j =1;j < cols;j++)
            {
                dp[i][j] = max(dp[i-1][j],dp[i][j-1]) + grid[i][j];
            }
        }
        return dp.back().back();
    }
};
```

- dp + 状态压缩

```
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if(grid.empty() || grid[0].empty())
            return 0;
        
        int rows = grid.size(),cols = grid[0].size();
        std::vector<int> dp(cols);
        for(int i = 0;i < rows;i++)
        {
            for(int j = 0;j < cols;j++)
            {
                if(i == 0 && j == 0)
                    dp[j] = grid[0][0];
                else if(i == 0)
                    dp[j] = grid[0][j] + dp[j-1] ;
                else if(j == 0)
                    dp[j] += grid[i][0];
                else 
                    dp[j] = max(dp[j-1],dp[j]) + grid[i][j];
            }
        }
        return dp.back();
    }
};
```

## [48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        std::vector<int> hash(256,-1);
        int res = 0,last = -1;
        for(int i = 0;i < s.length();i++)
        {
            last = std::max(last,hash[s[i]]);
            res = std::max(res,i-last);
            hash[s[i]] = i;
        }
        return res;
    }
};
```


## [60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

```
class Solution {
public:
    vector<double> dicesProbability(int n) {
        std::vector<double> dp(6,1/6.0);
        for(int i = 2;i <=n ;i++)
        {
            int total = 5 * i + 1;
            std::vector<double> tmp(total);
            for(int j = 0;j < dp.size();j++)
            {
                for(int k = 0;k < 6;k++)
                {
                    tmp[j+k] += dp[j]/6.0;
                }
            }
            dp = std::move(tmp);
        }
        return dp;
    }
};
```

## [63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

- 遍历

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() <= 1)
            return 0;
        int profit = INT_MIN,buy = prices[0];
        for(int i = 0;i < prices.size();i++)
        {
            buy = min(buy,prices[i]);
            profit = max(profit,prices[i] - buy);
        }
        return profit;
    }
};
```

- 单调栈

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() <= 1)
            return 0;
        std::stack<int> s;
        s.push(prices[0]);
        int profit = 0;
        for(int i = 1;i < prices.size();i++)
        {
            if(s.top() > prices[i])
            {
                s.pop();
                s.push(prices[i]);
            }
            else
            {
                profit = max(profit,prices[i] - s.top());
            }
        }
        return profit;
    }
};
```

- dp

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() <= 1)
            return 0;
        int curr = 0,profit = 0;
        for(int i = 1;i < prices.size();i++)
        {
            curr = max(curr,0) + prices[i] - prices[i-1];
            profit = max(profit,curr);
        }
        return profit;
    }
};
```

- dp

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if(n <= 1)
            return 0;
        
        std::vector<std::vector<int>> dp(n,std::vector<int>(2,0));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        for(int i = 1;i < n;i++)
        {
            dp[i][0] = max(dp[i-1][0],dp[i-1][1] + prices[i]);
            dp[i][1] = max(dp[i-1][1],-prices[i]);
        }
        return dp[n-1][0];
    }
};
```

# 搜索与回溯

## BFS 模板

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

## DFS 模板

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

## 回溯模板

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

## [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)

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

## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

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



## [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

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

## [542. 01 矩阵](https://leetcode-cn.com/problems/01-matrix/)

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


## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

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

## [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

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

## [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

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

## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

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
## [77. 组合](https://leetcode-cn.com/problems/combinations/)

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

## [78. 子集](https://leetcode-cn.com/problems/subsets/)

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

## [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

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

## [46. 全排列](https://leetcode-cn.com/problems/permutations/)

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

## [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

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

## [12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

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

## [17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

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

## [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

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

# 二分查找

## 口诀

- 标准查找带等号，非标查找不带等
- 找左定右，找右定左，找右加1免死
- 旋转要看旋转点，若有重复需去重

## [374. 猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

```
class Solution {
public:
    int guessNumber(int n) {
        int left = 1,right = n;
        while(left <= right)
        {
            int mid = left + ((right - left) >> 1);
            int res = guess(mid);
            if(res == 0)
                return mid;
            else if(res == -1)
                right = mid - 1;
            else
                left = mid + 1;
        }
        return -1;
    }
};
```

## [69. Sqrt(x)](https://leetcode-cn.com/problems/sqrtx/)

```
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 1)
            return x;
        
        int left = 1,right = x;
        while(left <= right)
        {
            int mid = left + ((right - left) >> 1);
            if(mid > x / mid)
                right = mid - 1;
            else
            {
                if((mid + 1) > x / (mid + 1))
                    return mid;
                else    
                    left = mid + 1;
            }
        }
        return -1;
    }
};
```

## [441. 排列硬币](https://leetcode-cn.com/problems/arranging-coins/)

```
class Solution {
public:
    int arrangeCoins(int n) {
        int left = 0,right = n;
        while(left <= right)
        {
            long mid = left + ((right - left) >> 1);
            long sum = mid * (mid + 1) >> 1;

            if(sum == n)
                return mid;
            else if(sum > n)
                right = mid - 1;
            else
                left = mid + 1;
        }
        return left - 1;
    }
};
```

## [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

```
class Solution {
public:
    bool isPerfectSquare(int num) {
        int left = 0,right = num;
        while(left <= right)
        {
            long mid = left + ((right - left) >> 1);
            long square = mid * mid;
            if(square == num)
                return true;
            else if(square > num)
                right = mid - 1;
            else
                left = mid + 1;
        }
        return false;
    }
};
```

## [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

```
class Solution {
public:
    int firstBadVersion(int n) {
        int left = 1,right = n;
        while(left < right)
        {
            int mid = left + ((right - left) >> 1);
            bool bad = isBadVersion(mid);
            if(bad)
                right = mid;
            else
                left = mid + 1;
        }
        return isBadVersion(right) ? right : -1;
    }
};
```

## [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

```
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int left = 0,right = nums.size() - 1;
        while(left < right)
        {
            int mid = left + ((right - left) >> 1);
            if(nums[mid] > nums[mid + 1])
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }
};
```

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty())
            return {-1,-1};
        int left = lower_bound(nums,target);
        int right = upper_bound(nums,target);
        if(left == -1 || right == -1)
            return {-1,-1};
        return {left,right};        
    }

    int lower_bound(vector<int>& nums, int target)
    {
        int left = 0,right = nums.size()-1;
        while(left < right)
        {
            int mid = left + ((right -left) >> 1);
            if(nums[mid] < target)
                left = mid + 1;
            else
                right = mid;
        }
        return nums[left] == target ? left : -1;
    }

    int upper_bound(vector<int>& nums, int target)
    {
        int left = 0,right = nums.size()-1;
        while(left < right)
        {
            int mid = left + ((right -left + 1) >> 1);
            if(nums[mid] > target)
                right = mid - 1;
            else
                left = mid;
        }
        return nums[right] == target ? right : -1;
    }
};
```

## [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

找好中点和旋转点：

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0,right = nums.size()-1;
        while(left <= right)
        {
            int mid = left + ((right - left) >> 1);
            if(nums[mid] == target)
                return mid;
                            
            if(nums[left] <= nums[mid])
            {
                if(nums[left] <= target && nums[mid] > target)
                    right = mid - 1;
                else
                    left = mid + 1;
            }
            else
            {
                if(nums[right] >= target && nums[mid] < target)
                    left = mid + 1;
                else
                    right = mid - 1;
            }
        }
        return -1;
    }
};
```

## [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

```
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int left = 0,right = nums.size()-1;
        while(left <= right)
        {
            int mid = left + (right-left)/2;
            if(nums[mid] == target)
                return true;
            
            if(nums[mid] == nums[left] && nums[mid] == nums[right])
            {
                left++;
                right--;
                continue;
            }

            if(nums[mid] >= nums[left])
            {
                if(target >= nums[left] && target <= nums[mid])
                    right = mid - 1;
                else    
                    left = mid + 1;
            }
            else
            {
                if(target >= nums[mid] && target <= nums[right])
                    left = mid + 1;
                else    
                    right = mid - 1;
            }
        }
        return false;
    }
};
```

## [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

```
class Solution {
public:
    int findMin(vector<int>& nums) {
		if(nums.empty())
			return -1;
		
		int left = 0,right = nums.size()-1;
		while(left < right)
		{
			int mid = left + (right - left) / 2;
			if(nums[mid] <= nums[right])
				right = mid;
			else
				left = mid + 1;
		}
		return nums[left];
    }
};
```

## [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

```
class Solution {
public:
    int findMin(vector<int>& nums) {
		if(nums.empty())
			return -1;
			
		int left = 0,right = nums.size()-1;
		while(left < right)
		{
			int mid = left + (right - left)/2;
			if(nums[mid] == nums[right] && nums[left] == nums[mid])
				++left,--right;
			else if(nums[mid] <= nums[right])
				right = mid;
			else
				left = mid +1;
		}
		return nums[left];
    }
};
```

## [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty())
            return false;
        int rows = matrix.size(),cols = matrix[0].size();
        int left = 0,right = rows * cols -1;
        while(left <= right)
        {
            int mid = left + (right-left)/2;
            int curr = matrix[mid/cols][mid%cols];
            if(curr == target)
                return true;
            else if(curr > target)
                right = mid - 1;
            else
                left = mid + 1;
        }
        return false;
    }
};
```

## [53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int res = 0;
        for(const auto n : nums)
        {
            if(n == target)
                res++;
        }
        return res;
    }
};
```

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty())
            return 0;
        int left = 0,right = nums.size() -1;
        while(left < right)
        {
            int mid = (left + right) >> 1;
            if(nums[mid] >= target)
                right = mid;
            else
                left = mid + 1;
        }

        if(nums[left] != target)
            return 0;
        int begin = left;
        left = 0,right = nums.size()-1;
        while(left < right)
        {
            int mid = (left + right + 1) >> 1;
            if(nums[mid] <= target)
                left = mid;
            else
                right = mid -1;
        }
        return right - begin + 1;
    }
};
```

## [53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int sum = std::accumulate(nums.begin(),nums.end(),0);
        int expect = (nums.size() * (nums.size()+1)) >> 1;
        return expect - sum;
    }
};
```

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left = 0,right = nums.size();
        while(left < right)
        {
            int mid = (left + right) >> 1;
            if(nums[mid] == mid)
                left = mid + 1;
            else 
                right = mid;
        }
        return left;
    }
};
```

# 贪心

## [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

```
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size();
        if(n < 2) return n;

        enum STATE {BEGIN = 0,UP,DOWN};
        STATE state = BEGIN;
        int max_len = 1;
        for(int i=1;i<n;i++)
        {
            switch(state)
            {
                case BEGIN:
                    if(nums[i-1] < nums[i])
                    {
                        state = UP;
                        max_len++;
                    }
                    else if(nums[i-1] > nums[i])
                    {
                        state = DOWN;
                        max_len++;
                    }
                    break;
                case UP:
                    if(nums[i-1] > nums[i])
                    {
                        state = DOWN;
                        max_len++;
                    }
                    break;
                case DOWN:
                if(nums[i-1] < nums[i])
                {
                    state = UP;
                    max_len++;
                }
                break;
            }
        } 
        return max_len;
    }
};
```

## [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

```
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int fives = 0,tens = 0;
        for(const auto bill : bills)
        {
            if(bill == 5)
                fives++;
            else if(bill == 10)
            {
                if(fives > 0)
                    fives--,tens++;   
                else    
                    return false;
            }
            else
            {
                if(fives > 0 && tens > 0)
                    fives--,tens--;
                else if(fives >= 3)
                    fives -= 3;
                else
                    return false;
            }
        }
        return true;
    }
};
```

## [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

```
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
		std::sort(people.begin(),people.end(),[](vector<int>& v1,vector<int>& v2)
		{
			if(v1[0] == v2[0])
				return v1[1] < v2[1];
			return v1[0] > v2[0];
		});

		vector<vector<int>> res;
		for(const auto p : people)
		{
			res.insert(res.begin()+ p[1],p);
		}
		return res;
    }
};
```

## [134. 加油站](https://leetcode-cn.com/problems/gas-station/)

```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
		int total_gas = std::accumulate(gas.begin(),gas.end(),0);
		int total_cost = std::accumulate(cost.begin(),cost.end(),0);

		if(total_cost > total_gas)
			return -1;
		int remain = 0,start = 0;
		for(int i = 0;i < gas.size();i++)
		{
			if(remain + gas[i] < cost[i])
			{
				remain = 0;
				start = i + 1;
			}
			else
				remain += gas[i] - cost[i];
		}
		return start;
    }
};
```

## [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

```
class Solution {
public:
   bool canJump(vector<int>& nums) 
   {
       int longest = 0;
	   int n = nums.size();

       for(int i = 0;i < n;i++)
       {
		   if(i <= longest)
		   {
			   longest = std::max(longest,i+nums[i]);
			   if(longest >= n-1)
			   	return true;
		   }
       }
       return false;
   }
};
```


## [316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)

```
class Solution {
public:
    string removeDuplicateLetters(string s) {
		unordered_map<char,int> remain_counter;
		for(const auto str : s)
			remain_counter[str]++;
		
		vector<char> S;
		for(const auto c : s)
		{
			if(find(S.begin(),S.end(),c) == S.end())
			{
				//@ 栈不为空，并且当前的字符小于栈尾元素，并且末尾这个字符后面还有机会出现
				while(!S.empty() && c < S.back() && remain_counter[S.back()] > 0)				
					S.pop_back();	
				S.push_back(c);
			}			
			remain_counter[c]--;
		}		
		return string(S.begin(),S.end());
    }
};
```



## [135. 分发糖果](https://leetcode-cn.com/problems/candy/)

```
class Solution {
public:
    int candy(vector<int>& ratings) {
		int n = ratings.size();
		if(n <= 1)
			return n;
		std::vector<int> dp(n,1);
		for(int i = 1;i < n;i++)
		{
			if(ratings[i] > ratings[i-1])
				dp[i] = dp[i-1] + 1;
		}
		
		for(int i = n-1;i > 0;i--)
		{
			if(ratings[i-1] > ratings[i])
				dp[i-1] = std::max(dp[i-1],dp[i]+1);
		}
		return std::accumulate(dp.begin(),dp.end(),0);
    }
};
```

## [14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

```
class Solution {
public:
    int cuttingRope(int n) {
        if(n == 2)
            return 1;
        if(n == 3)
            return 2;
        
        int res = 1;
        while(n > 4)
        {
            res *= 3;
            n -= 3;
        }
        return n * res;
    }
};
```

## [14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

```
class Solution {
public:
    int cuttingRope(int n) {
        if(n <= 3)
            return n - 1;
        if(n == 4)
            return 4;
        long res = 1;
        while(n > 4)
        {
            res *= 3;
            res %= 1000000007;
            n -= 3;
        }
        return (res * n) % 1000000007;
    }
};
```

# 树

## [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preorder(res,root);
        return res;
    }

    void preorder(vector<int>& vec,TreeNode* root)
    {
        if(root == nullptr)
            return;
        
        vec.emplace_back(root->val);
        preorder(vec,root->left);
        preorder(vec,root->right);
    }
};
```
```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
		s.push(root);
		while(!s.empty())
		{
			auto curr = s.top();
			s.pop();
			res.push_back(curr->val);
			if(curr->left)
				s.push(curr->left);
			if(curr->right)
				s.push(curr->right);		
		}
		
		std::reverse(res.begin(),res.end());
        return res;
    }
};
```
```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
        s.push(root);       
        while(!s.empty())
        {
            TreeNode* curr = s.top();
            s.pop();
            res.emplace_back(curr->val);
            if(curr->right)
                s.push(curr->right);
            if(curr->left)
                 s.push(curr->left);
        }
        return res;
    }
};
```

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
        s.push(root);       
        while(!s.empty())
        {
            TreeNode* curr = s.top();
			if(curr)
			{
				s.pop();
				if(curr->right)
					s.push(curr->right);
				if(curr->left)
					s.push(curr->left);
				s.push(curr);
				s.push(nullptr);
			}
			else
			{
				s.pop();
				curr = s.top();
				s.pop();
				res.emplace_back(curr->val);
			}
        }
        return res;
    }
};
```

## [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        std::vector<int> res;
        inorder(res,root);
        return res;
    }

    void inorder(std::vector<int>& vec,TreeNode* root)
    {
        if(root == nullptr)
            return;
        inorder(vec,root->left);
        vec.emplace_back(root->val);
        inorder(vec,root->right);
    }
};
```

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
        TreeNode* curr = root;
        while(curr != nullptr || !s.empty())
        {
            if(curr)
            {
                s.push(curr);
                curr = curr->left;
            }
            else
            {
                curr = s.top();
                s.pop();
                res.emplace_back(curr->val);
                curr = curr->right;
            }
        }
        return res;
    }
};
```

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
		if(root == nullptr)
			return {};
		
		std::vector<int> res;
		std::stack<TreeNode*> s;
		s.push(root);       
		while(!s.empty())
		{
			TreeNode* curr = s.top();
			if(curr)
			{
				s.pop();
				if(curr->right)
					s.push(curr->right);
				s.push(curr);
				s.push(nullptr);
				
				if(curr->left)
					s.push(curr->left);
			}
			else
			{
				s.pop();
				curr = s.top();
				s.pop();
				res.emplace_back(curr->val);
			}
		}
		return res;
    }
};
```

## [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        std::vector<int> res;
        postorder(res,root);
        return res;
    }

    void postorder(std::vector<int>& vec,TreeNode* root)
    {
        if(root == nullptr)
            return;
        postorder(vec,root->left);
        postorder(vec,root->right);
        vec.emplace_back(root->val);
    }
};
```

```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
		if(root == nullptr)
			return {};
		
		std::vector<int> res;
		std::stack<TreeNode*> s;
		s.push(root);       
		while(!s.empty())
		{
			TreeNode* curr = s.top();
			if(curr)
			{
				s.push(nullptr);				
				if(curr->right)
					s.push(curr->right);				
				if(curr->left)
					s.push(curr->left);
			}
			else
			{
				s.pop();
				curr = s.top();
				s.pop();
				res.emplace_back(curr->val);
			}
		}
		return res;
    }
};
```

## [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root == nullptr)
            return {};
        std::vector<vector<int>> res;
        std::queue<TreeNode*> q;
        q.push(root);
        std::vector<int> level;
        while(!q.empty())
        {
           size_t n = q.size();
           for(size_t i = 0;i < n;++i)
           {
                auto curr = q.front();
                q.pop();
                level.emplace_back(curr->val);
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right); 
           }
           res.emplace_back(std::move(level));             
        }
        return res;
    }
};
```

## [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(root == nullptr)
            return {};
        std::vector<std::vector<int>> res;
		std::queue<TreeNode*> q;
		q.push(root);
		bool l2r = true;
		while(!q.empty())
		{
			std::deque<int> dq;
			int n = q.size();
			while(n--)
			{
				auto node = q.front();
				q.pop();
				if(l2r)
					dq.emplace_back(node->val);
				else
					dq.emplace_front(node->val);
				if(node->left)
					q.push(node->left);
				if(node->right)
					q.push(node->right);
			}
			l2r = !l2r;
			res.emplace_back(dq.begin(),dq.end());
		}		
        return res;
    }
};
```

## [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

```
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
		if(root == nullptr)
			return -1;
		std::queue<TreeNode*> q;
		q.push(root);
		std::vector<std::vector<int>> layers;
		while(!q.empty())
		{
			int n = q.size();
			std::vector<int> layer;
			while(n--)
			{
				auto node = q.front();
				q.pop();
				layer.emplace_back(node->val);
				if(node->left)
					q.push(node->left);
				if(node->right)
					q.push(node->right);
			}
			layers.emplace_back(std::move(layer));
		}		
		return layers.back().front();
		}
};
```

## [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

```
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
		int sum = 0;
		inorder(root,sum);
		return root;
    }
	
	void inorder(TreeNode*node,int& sum)  //@ 必须是引用
	{
		if(node == nullptr)
			return;
		inorder(node->right,sum);
		sum += node->val;
		node->val = sum;
		inorder(node->left,sum);
	}
};
```

```
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        if(root == nullptr)
            return nullptr;
		
		std::stack<TreeNode*> s;
		s.push(root);
		int sum = 0;
		while(!s.empty())
		{
			auto p = s.top();
			if(p)
			{
				s.pop();
				if(p->left)
					s.push(p->left);		
				s.push(p);
				s.push(nullptr);				
				if(p->right)
					s.push(p->right);	
			}
			else
			{
				s.pop();
				auto node = s.top();
				s.pop();
				sum += node->val;
				node->val = sum;			
			}
		}
        return root;
    }
};
```

## [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

```
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
		vector<int>  res;
		if(root == nullptr)
			return res;
		queue<TreeNode*> q;
		q.push(root);
		while(!q.empty())
		{
			int size = q.size();
			for(int i = 0;i < size;++i)
			{
				auto t = q.front();
				q.pop();
				if(t->left)
					q.push(t->left);
				if(t->right)
					q.push(t->right);
				if(i == size -1)
					res.push_back(t->val);
			}
		}
		return res;
    }
};
```

## [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
		std::vector<int> vec;
		inorder(root,vec);
		for(int i = 1;i < vec.size();i++)
		{
			if(vec[i] <= vec[i-1])
				return false;
		}
		return true;
    }
	
	void inorder(TreeNode* root,std::vector<int>& res)
	{
		if(root == nullptr)
			return;
		inorder(root->left,res);
		res.emplace_back(root->val);
		inorder(root->right,res);
	}
};
```

## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

递归解法：

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr)
            return 0;
        return max(maxDepth(root->left),maxDepth(root->right)) + 1;
    }
};
```

非递归解法：

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr)
            return 0;
        int res = 0;
        queue<TreeNode* > q;
        q.push(root);
        while(!q.empty())
        {
            res ++;
            int size = q.size();
            for(int i=0;i<size;i++)
            {
                TreeNode* node = q.front();
                q.pop();
                if(node->left)
                    q.push(node->left);
                if(node->right)
                    q.push(node->right);
            }
        }
        return res;
    }
};
```

## [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(root == nullptr)
            return true;
        int left = _depth(root->left);
        int right = _depth(root->right);
        if(abs(left-right) > 1)
            return false;
        return isBalanced(root->left) && isBalanced(root->right);
    }

    int _depth(TreeNode* root)
    {
        if(root == nullptr)
            return 0;  
        return  max(_depth(root->left),_depth(root->right)) + 1;
    }
};
```

## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

```
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root == nullptr)
            return 0;
        int res = 0;
        depth(root,res);
        return res;
    }

    int depth(TreeNode* root,int& diameter)
    {
        if(root == nullptr)
            return 0;
        int L = depth(root->left,diameter);
        int R = depth(root->right,diameter);
        diameter = std::max(diameter,L+R);
        return std::max(L,R) + 1;
    }
};
```

## [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

中序遍历：

```
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        if(root == nullptr)
            return -1;
		vector<int> res;
		inorder(root,res);
        return res.size() >=k ?  res[k-1] : -1;		 
    }
	
	void inorder(TreeNode* root,vector<int>& res)
	{
		if(root == nullptr)
			return;
		inorder(root->left,res);
		res.push_back(root->val);
		inorder(root->right,res);
	}
};
```

二分法：

```
class Solution {
public:
    int numofNodes(struct TreeNode* root)
    {
        if(root == nullptr) 
            return 0;
        return 1 + numofNodes(root->left) + numofNodes(root->right);
    }

    int kthSmallest(struct TreeNode* root, int k){
        int cnt = numofNodes(root->left);
        if(k <= cnt) 
            return kthSmallest(root->left,k);
        else if(k > cnt+1) 
            return kthSmallest(root->right,k-cnt-1);
        return root->val; // k == cnt+ 1
    }
};
```

## [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

递归：

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
		if(root == nullptr)
			return root;
		if(root->val ==val)
			return root;
		if(root->val > val)
			return searchBST(root->left,val);
		if(root->val < val)
			return searchBST(root->right,val);
		
		return nullptr;
    }
};
```

迭代：

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root && root->val !=  val)
            root->val > val ? root = root->left : root = root->right;
        return root;
    }
};
```

栈迭代：

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        stack<TreeNode*> stk;
        if(root)
            stk.push(root);
        while(!stk.empty())
        {
            auto t = stk.top();
            stk.pop();
            if(t->val == val)
                return t;
            else if(t->val > val && t->left)
                stk.push(t->left);
            else if(t->val < val && t->right)  
                stk.push(t->right);
        }
        return nullptr;
    }
};
```

## [897. 递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)

```
class Solution {
public:
	void inorder(TreeNode* root,std::vector<TreeNode*>& nodes)
	{
		if(root == nullptr)
			return;
		
		inorder(root->left,nodes);
		nodes.push_back(root);
		inorder(root->right,nodes);
	}
	
    TreeNode* increasingBST(TreeNode* root) {
        if(root == nullptr)
			return nullptr;
		std::vector<TreeNode*> nodes;
		inorder(root,nodes);	
		for(int i = 1;i < nodes.size();++i)
		{
			nodes[i-1]->left = nullptr;
			nodes[i-1]->right = nodes[i];
		}
		nodes.back()->left = nodes.back()->right = nullptr;
        return nodes.front();
    }
};
```

## [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr || p == nullptr || q == nullptr)
			return nullptr;
		if(root->val > p->val && root->val > q->val)
			return lowestCommonAncestor(root->left,p,q);
		else if(root->val < p->val && root->val < q->val)
			return lowestCommonAncestor(root->right,p,q);
		return root;
    }
};
```

## [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

递归：

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if(root == nullptr)
            return false;
        if(root->left == nullptr && root->right == nullptr)
            return sum == root->val;
		sum -= root->val;
        return hasPathSum(root->left,sum) || hasPathSum(root->right,sum);
    }
};
```

迭代：

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        stack<pair<TreeNode*, int>> stk;
        if(root) stk.push(make_pair(root, root->val));
        while(stk.size()) {
            pair<TreeNode*, int> cur = stk.top(); stk.pop();
            if(!cur.first->left && !cur.first->right && cur.second == sum) return true;
            if(cur.first->left) stk.push(make_pair(cur.first->left, cur.second + cur.first->left->val));
            if(cur.first->right) stk.push(make_pair(cur.first->right, cur.second + cur.first->right->val));
        }
        return false;
    }
};
```

dfs：

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr)
            return false;
        return dfs(root,targetSum,0);
    }

    bool dfs(TreeNode* root, int targetSum,int curr)
    {
        if(root == nullptr)
            return false;
        curr += root->val;
        if(root->left == nullptr && root->right == nullptr && targetSum == curr)
            return true;        
        return dfs(root->left,targetSum,curr) || dfs(root->right,targetSum,curr);
    }
};
```

## [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

```
class Solution {
public:
    void dfs(TreeNode *root,vector<int> &path,vector<vector<int>>& res,int sum)  //@ sum 不能是引用
    {
        if(root == nullptr)
            return;
        path.push_back(root->val);
        if(root->val == sum && (root->left == nullptr && root->right == nullptr))
            res.push_back(path);		
		sum -= root->val;
        dfs(root->left,path,res,sum);
        dfs(root->right,path,res,sum);
        path.pop_back();
    }

    vector<vector<int>> pathSum(TreeNode* root, int sum) 
    {
        vector<int> path;
		vector<vector<int>> res;
        dfs(root,path,res,sum);
        return res;        
    }
};
```

## [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

```
class Solution {
public:
    int count(TreeNode* root, int sum)
    {
        if(root == nullptr)
            return 0;
        return (root->val == sum) + count(root->left,sum - root->val) + count(root->right,sum - root->val);
    }

    int pathSum(TreeNode* root, int sum) {
        if(root == nullptr)
            return 0;
        return count(root,sum) + pathSum(root->left,sum) + pathSum(root->right,sum);
    }
};
```

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

递归：

```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr)
            return nullptr;        
        TreeNode* left = invertTree(root->left);
        TreeNode* right = invertTree(root->right);
        root->left = right;
        root->right = left;
        return root;
    }
};
```

非递归：

```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr)
            return nullptr;
        
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            TreeNode* curr = q.front();
            q.pop();
            TreeNode* left = curr->left;
            curr->left = curr->right;
            curr->right = left;
            if(curr->left)
                q.push(curr->left);
            if(curr->right)
                q.push(curr->right);    
        }
        return root;
    }
};
```

## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

递归：

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
		if(root1 == nullptr)
			return root2;
		if(root2 == nullptr)
			return root1;
		root1->val += root2->val;
		root1->left = mergeTrees(root1->left,root2->left);
		root1->right = mergeTrees(root1->right,root2->right);
		return root1;
    }
};
```

栈迭代：

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1 == nullptr)
            return t2;
        stack<pair<TreeNode*,TreeNode*>> stk;
        if(t1 && t2)
            stk.push(make_pair(t1,t2));
        
        while(!stk.empty())
        {
            auto cur = stk.top();
            stk.pop();
            cur.first->val += cur.second->val;
            if(cur.second->left)
            {
                if(cur.first->left == nullptr)
                    cur.first->left = new TreeNode(0);  //如果左子树不存在，新建值为0的节点
                stk.push(make_pair(cur.first->left,cur.second->left));
            }
            if(cur.second->right)
            {
                if(cur.first->right == nullptr)
                    cur.first->right = new TreeNode(0);  //如果右子树不存在，新建值为0的节点
                stk.push(make_pair(cur.first->right,cur.second->right));
            }            
        }
        return t1;
    }
};
```

队列迭代：

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1 == nullptr)
            return t2;
        if(t2 == nullptr)
            return t1;
        queue<pair<TreeNode*,TreeNode*>> q;
        q.push(make_pair(t1,t2));
        while(!q.empty())
        {
            auto cur = q.front();
            q.pop();
            cur.first->val  += cur.second->val;      
            if(cur.second->left)
            {
                if(cur.first->left == nullptr)
                    cur.first->left = new TreeNode(0);  //如果左子树不存在，新建值为0的节点
                q.push(make_pair(cur.first->left,cur.second->left));
            }
            if(cur.second->right)
            {
                if(cur.first->right == nullptr)
                    cur.first->right = new TreeNode(0);  //如果右子树不存在，新建值为0的节点
                q.push(make_pair(cur.first->right,cur.second->right));
            }   
        }
        return  t1;
    }
};
```

## [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

```
class Solution {
public:
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
		if(root == nullptr)
			return false;
		return same(root,subRoot) || isSubtree(root->left,subRoot) || isSubtree(root->right,subRoot);
    }
	
	bool same(TreeNode* tree1, TreeNode* tree2)
	{
		if(tree1 == nullptr && tree2 == nullptr)
			return true;
		if(tree1 == nullptr || tree2 == nullptr)
			return false;
		if(tree1->val != tree2->val)
			return false;
		return same(tree1->left,tree2->left) && same(tree1->right,tree2->right);
	}	
};
```

## [671. 二叉树中第二小的节点](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

```
class Solution {
public:
    set<int> s;
    void dfs(TreeNode* root)
    {
        if(root == nullptr)
            return;
        s.insert(root->val);
        dfs(root->left);
        dfs(root->right);
    }

    int findSecondMinimumValue(TreeNode* root) {
        if(root == nullptr)
            return -1;
        
        dfs(root);
        auto it = s.begin();
        if(++it == s.end())
            return -1;
        else
            return *it;
    }
};
```

## [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr || root == p || root == q)
            return root;
        
        TreeNode* left = lowestCommonAncestor(root->left,p,q);
        TreeNode* right = lowestCommonAncestor(root->right,p,q);
        if(left && right)
            return root;
        return left ? left : right;
    }
};
```

## [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

递归：

```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr)
            return true;
        return symmetric(root->left,root->right);
    }

    bool symmetric(TreeNode* node1,TreeNode* node2)
    {
        if(node1 == nullptr && node2 == nullptr)
            return true;
        if(node1 == nullptr || node2 == nullptr)
            return false;
        if(node1->val != node2->val)
            return false;
        return symmetric(node1->left,node2->right) && symmetric(node1->right,node2->left);
    }
};
```

非递归：

```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr)
            return true;
        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);
        while(!q.empty())
        {
            TreeNode* L = q.front();
            q.pop();
            TreeNode* R = q.front();
            q.pop();
            if(L == nullptr && R == nullptr)
                continue;
            if(L == nullptr || R == nullptr || (L->val != R->val))
                return false;
            q.push(L->left);
            q.push(R->right);
            q.push(L->right);
            q.push(R->left);
        }
        return true;
    }
};
```

## [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

```
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        std::vector<int> vec;
        auto* curr = head;
        while(curr)
        {
            vec.push_back(curr->val);
            curr = curr->next;
        }
        return to_bst(vec);
    }
    
    TreeNode* to_bst(const std::vector<int>& vec)
    {
        if(vec.empty())
            return nullptr;
        int n = vec.size();
        if(n == 1)
            return new TreeNode(vec.front());
        TreeNode* root = new TreeNode(vec[n/2]);
        std::vector<int> lv(vec.begin(),vec.begin()+n/2);
        std::vector<int> rv(vec.begin()+n/2+1,vec.end());
        root->left = to_bst(lv);
        root->right = to_bst(rv);
        return root;
    }
};
```

快慢指针：

```
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if(head == nullptr)
            return nullptr;
        if(head->next == nullptr)
            return new TreeNode(head->val);
        
        //@ 快慢指针,pre指向slow节点的前一个节点
        ListNode *pre = head,*slow = head,*fast = head;
        while(fast && fast->next)
        {
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }

        pre->next = nullptr; //@ 链表切断,一分为二
        TreeNode* root= new TreeNode(slow->val);  //@ slow 作为中间节点可以作为根
        root->left = sortedListToBST(head); //@ 递归左子树
        root->right = sortedListToBST(slow->next); //@ 递归右子树
        return root;
    }
};
```

## [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)

```
class Solution {
public:
    bool isCompleteTree(TreeNode* root) {
		if(root == nullptr)
			return true;
		std::queue<TreeNode*> q;
		q.push(root);
		bool reach_null{false};
		while(!q.empty())
		{
			auto curr = q.front();
			q.pop();
			if(curr == nullptr)
			{
				reach_null = true;
				continue;
			}
			
			if(reach_null)
				return false;
			q.push(curr->left);
			q.push(curr->right);
		}
		return true;
    }
};
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() != inorder.size() || preorder.empty())
            return nullptr;
        int value = preorder[0];
        TreeNode* root = new TreeNode(value);
        auto pos = std::find(inorder.begin(),inorder.end(),value);
        std::vector<int> linor(inorder.begin(),pos);
        std::vector<int> rinor(pos+1,inorder.end());
        int left = linor.size();
        std::vector<int> lpre(preorder.begin()+1,preorder.begin()+1+left);
        std::vector<int> rpre(preorder.begin()+1+left,preorder.end());
        root->left = buildTree(lpre,linor);
        root->right = buildTree(rpre,rinor);
        return root;
    }
};
```

```
class Solution {
public:
    std::unordered_map<int,int> hash;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for(int i = 0;i < inorder.size();i++)
            hash[inorder[i]] = i;
        return build(preorder,inorder,0,preorder.size()-1,0,inorder.size()-1);
    }

    TreeNode* build(vector<int>& pre, vector<int>& inor,int pre_start,int pre_end,int inor_start,int inor_end)
    {
        if(pre_start > pre_end)
            return nullptr;
        TreeNode* root = new TreeNode(pre[pre_start]);
        if(pre_start == pre_end)
            return root;
		int pos = hash[pre[pre_start]];		        
        int len = pos - inor_start;
        root->left = build(pre,inor,pre_start+1,pre_start+len,inor_start,pos-1);
        root->right = build(pre,inor,pre_start+1+len,pre_end,pos+1,inor_end);
		return root;
    }
};
```

## [889. 根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

```
class Solution {
public:
    std::unordered_map<int,int> hash;

    TreeNode* constructFromPrePost(vector<int>& preorder, vector<int>& postorder) {
         for(int i = 0;i < postorder.size();i++)
            hash[postorder[i]] = i;
        return build(preorder,postorder,0,preorder.size()-1,0,postorder.size()-1);
    }

    TreeNode* build(const vector<int>& preorder, const vector<int>& postorder,int pre_start,int pre_end,int post_start,int post_end)
    {
        if(pre_start > pre_end)
            return nullptr;
        TreeNode* root = new TreeNode(preorder[pre_start]);
        if(pre_start == pre_end)
            return root;
        int pos = hash[preorder[pre_start+1]];
        int len = pos - post_start + 1;
        root->left = build(preorder,postorder,pre_start+1,pre_start+len,post_start,post_start+len-1);
        root->right = build(preorder,postorder,pre_start+1+len,pre_end,post_start+len,post_end-1);
        return root;
    }
};
```

## [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

````
class Solution {
public:
	std::unordered_map<int,int> hash;
	
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
		for(int i = 0;i < inorder.size();i++)
			hash[inorder[i]] = i;
		return build(inorder,postorder,0,inorder.size()-1,0,postorder.size()-1);
    }
	
	TreeNode* build(vector<int>& inor, vector<int>& post,int inor_start,int inor_end,int post_start,int post_end)
	{
		if(inor_start > inor_end)
			return nullptr;
		TreeNode* root = new TreeNode(post[post_end]);
        if(inor_start == inor_end)
            return root;
		int pos = hash[post[post_end]];
		int len = pos - inor_start -1;
		root->left = build(inor,post,inor_start,pos-1,post_start,post_start+len);
		root->right = build(inor,post,pos+1,inor_end,post_start+len+1,post_end-1);
		return root;
	}
};
````

## [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

递归：

```
class Solution {
public:
	vector<int> res;
    vector<int> postorder(Node* root) {
        if(root == nullptr)
			return res;
		for(auto i : root -> children)
            postorder(i);
        res.push_back(root -> val);
        return res;
    }
};
```

迭代：

```
class Solution {
public:
    vector<int> postorder(Node* root) {
        if(root == nullptr)
			return {};
		vector<int> res;
		stack<Node*> stk;
		stk.push(root);
		while(!stk.empty())
		{
			Node* t = stk.top();
			stk.pop();
			res.push_back(t->val);
			for(auto i : t->children)
				stk.push(i);			
		}
		reverse(res.begin(),res.end());
		return res;
    }
};
```

## [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

递归：

```
class Solution {
public:
	vector<int> res;
    vector<int> preorder(Node* root) {
        if(root == nullptr)
			return res;
		res.push_back(root->val);
		for(auto i : root->children)
			preorder(i);
		return res;
    }
};
```

迭代：

```
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> res;
		if(root == nullptr)
			return res;
		stack<Node*> stk;
		stk.push(root);
		while(!stk.empty())
		{
			auto t = stk.top();
			stk.pop();
            res.push_back(t->val);
			for(int i = t->children.size()-1;i>=0;--i)
				stk.push(t->children[i]);
		}
		return res;
    }
};
```

## [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

```
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
		if(root == nullptr)
			return res;
		queue<Node*> q;
		q.push(root);
		while(!q.empty())
		{
			vector<int> level;
			int size = q.size();
			for(int i=0;i<size;i++)
			{
				auto t = q.front();
				q.pop();
				level.push_back(t->val);
				for(const auto i : t->children)
					q.push(i);
			}
			res.push_back(move(level));
		}		
		return res;
    }
};
```

## [559. N叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

递归：

```
class Solution {
public:
    
    int maxDepth(Node* root) {
        if(root == nullptr)   
			return 0;
        int depth = 0;
        for(const auto i : root -> children)
            depth = max(maxDepth(i), depth);            
        return depth + 1;
    }
};
```

DFS：

```
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr)
			return 0;
		stack<pair<Node*,int>> stk;
		stk.push(make_pair(root,1));
		int depth = 1;
		while(!stk.empty())
		{
			auto t = stk.top();
			stk.pop();
			int dep = t.second;
			for(const auto i : t.first->children)
				stk.push(make_pair(i,dep+1));
			depth = max(depth,dep);		
		}
		return depth;
    }
};
```

BFS：

```
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr)
			return 0;
		
		queue<Node*> q;
		q.push(root);
		int depth = 0;
		while(!q.empty())
		{
			depth++;
			int size = q.size();
			for(int i = 0;i<size;i++)
			{
				auto t = q.front();
				q.pop();
				for(const auto i : t->children)
					q.push(i);
			}			
		}
		return depth;
    }
};
```

## [33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

```
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        return dfs(postorder,0, postorder.size() - 1);
    }

    bool dfs(std::vector<int>& post,int left,int right)
    {
        if(left >= right)
            return true;
        int root = post[right];
        int j = 0;
        while(j < right && post[j] < root)
            j++;
        
        for(int i = j;i < right;i++)
        {
            if(post[i] < root)
                return false;
        }
        return dfs(post,left,j-1) && dfs(post,j,right-1);
    }
};
```

## [54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

```
class Solution {
public:
    void inorder(TreeNode* root,std::vector<int>& vec)
    {
        if(root == nullptr)
            return;
        inorder(root->left,vec);
        vec.push_back(root->val);
        inorder(root->right,vec);
    } 

    int kthLargest(TreeNode* root, int k) {
        std::vector<int> vec;
        inorder(root,vec);

        if(vec.size() < k)
            return -1;
        std::reverse(vec.begin(),vec.end());
        return vec[k-1];
    }
};
```

## [55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr)
            return 0;
        return max(maxDepth(root->left),maxDepth(root->right)) + 1;
    }
};
```

# 排序

![](./img/quick_sort.gif)

- 通过一趟排序将要排序的数据分割成独立的两部分：分割点左边都是比它小的数，右边都是比它大的数
- 然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列

```
int partition(std::vector<int>& array, int left, int right)
{
	int value = array[left];
	while (left < right)
	{
		while (left < right && array[right] >= value)
			right--;
		if (left < right)
			array[left] = array[right];

		while(left < right && array[left] <= value)
			left++;
		if (left < right)
			array[right] = array[left];
	}
	array[left] = value;
	return left;
}

void quick_sort(std::vector<int>& array,int left,int right)
{
	if (left >= right)
		return;
	int pos = partition(array,left,right);
	quick_sort(array,left, pos-1);
	quick_sort(array, pos+1,right);
}
```

## 归并排序

![](./img/merge_sort.gif)

归并排序是建立在归并操作上的一种有效的排序算法，该算法是采用分治法的一个非常典型的应用。

```
void merge(std::vector<int>& nums, size_t left, size_t mid, size_t right)
{
	size_t i = left, j = mid + 1, k = 0;
	std::vector<int> vec(right - left + 1);

	while (i <= mid && j <= right)
	{
		if (nums[i] <= nums[j])
			vec[k++] = nums[i++];
		else
			vec[k++] = nums[j++];
	}

	while (i <= mid)
		vec[k++] = nums[i++];
	while (j <= right)
		vec[k++] = nums[j++];
	for (i = left, k = 0; i <= right;)
		nums[i++] = vec[k++];
}

void merge_sort(std::vector<int>& nums, size_t left, size_t right)
{
	if (left >= right)
		return;

	size_t mid = left + ((right - left) >> 1);
	merge_sort(nums, left, mid);
	merge_sort(nums, mid + 1, right);
	merge(nums, left, mid, right);
}
```
## 冒泡排序

![](./img/bubble_sort.gif)

- 冒泡排序是一种交换排序
- 重复地走访待排序的序列，依次比较两个相邻的元素，如果顺序是错误的就进行交换
- 当遍历一轮时候，不发生交换，可以提前结束

```
void bubble_sort(std::vector<int>& array)
{
	int n = array.size();
	for (int i = 0; i < n; i++)
	{
		bool swapped = false;
		for (int j = 0; j < n - 1; j++)
		{
			if (array[j] > array[j+1])
			{
				std::swap(array[j],array[j+1]);
				swapped = true;
			}
		}
		if (!swapped)
			break;
	}
}
```

## 选择排序

![](./img/select_sort.gif)

重复地走访待排序的序列，选择出最大的或者最小的元素放到合适位置上。

```
void select_sort(std::vector<int>& nums)
{
	for (size_t i = nums.size() - 1; i > 0; --i)
	{
		int pos = i;
		for (size_t j = 0; j < i; ++j)
		{
			if (nums[j] > nums[pos])
				pos = j;
		}
		if (pos != i)
			std::swap(nums[i], nums[pos]);
	}
}
```

## 插入排序

![](./img/insert_sort.gif)

```
void insert_sort(std::vector<int>& array)
{
  int n = array.size();
  for (int i = 1; i < n; i++)
  {
    int j = i - 1;
    int key = array[i];
    while (j >= 0 && array[j] > key) {
      array[j + 1] = array[j];
      j--;
    }
    array[j + 1] = key;
  }
}

```

## 希尔排序

![](./img/shell_sort.gif)

希尔(Shell)排序又称为缩小增量排序，它是一种插入排序。

```
void shell_sort(std::vector<int>& array)
{
  int len = array.size();
  for (int step = len / 2; step >= 1; step /= 2)
  {
    for (int i = 0; i < step; i++)
    {
      for (int j = step + i; j < len; j += step)
      {
        int key = array[j];
        int index = j - step;
        while (index >= 0 && array[index] > key)
        {
          array[index + step] = array[index];
          index -= step;
        }
        array[index + step] = key;
      }
    }
  }
}
```

## 堆排序

![](./img/heap_sort.gif)

```
void adjust(std::vector<int> &nums, size_t index, size_t size)
{
	//@ index 是根节点
	while (index * 2 + 1 < size) //@ 是否已经是叶子节点
	{
		size_t j = index * 2 + 1;
		if (j < size && j + 1 < size && nums[j + 1] > nums[j])   //@ j 是左孩子节点,j+1是右孩子节点     
			++j;
		if (nums[index] >= nums[j])
			break;
		std::swap(nums[index], nums[j]);
		index = j;
	}
}

void heapify(std::vector<int> &nums)
{
	size_t size = nums.size();
	//@ size/2 -1 是最后一个非叶子节点，它以下都是叶子节点，不用下沉了
	for (int i = size / 2 - 1; i >= 0; i--)
		adjust(nums, i, size);
}

void heap_sort(std::vector<int> &nums)
{
	size_t n = nums.size();
	heapify(nums);

	while (n > 1)
	{
		std::swap(nums[0], nums[--n]);
		adjust(nums, 0, n);
	}
}
```

## 基数排序

![](./img/radix_sort.gif)

将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。

```
void count_sort(std::vector<int>& nums, int exp)
{
	std::vector<int> range(10, 0); //@ [0 9] 区间
	for (const auto num : nums)
		range[num / exp % 10]++;

	std::vector<int> tmp(nums.size());

	for (int i = 1; i < 10; i++)
		range[i] += range[i - 1];

	for (int i = nums.size() - 1; i >= 0; i--)
	{
		int index = --range[nums[i] / exp % 10];
		tmp[index] = nums[i];
	}
	nums = std::move(tmp);
}

//@ 排序数组中不能有负数
void radix_sort(std::vector<int>& nums)
{
	int Max = INT_MIN;
	for (const auto num : nums)
		Max = std::max(Max, num);
	for (int exp = 1; Max / exp; exp *= 10)
		count_sort(nums, exp);
}
```

## 排序算法总结

![](./img/sort_summary.png)

排序算法分类：

- 交换排序：两两比较待排序的关键字，并交换不满足次序要求的那对数，直到整个表都满足次序要求为止
- 插入排序：每一趟将一个待排序的数字，按照其大小插入到有序队列的合适位置里，直到全部插入完成
- 选择排序：每趟从待排序的记录中选出关键字最小的记录，顺序放在已排序的记录序列末尾，直到全部排序结束为止

直接插入排序和希尔排序的比较：

- 直接插入排序是稳定的；而希尔排序是不稳定的
- 直接插入排序更适合于原始记录基本有序的集合。希尔排序的比较次数和移动次数都要比直接插入排序少，当N越大时，效果越明显
- 直接插入排序也适用于链式存储结构；希尔排序不适用于链式结构

## [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

```

class Solution {
public:
    ListNode* sortList(ListNode* head) {
		if(head == nullptr || head->next == nullptr)
			return head;
		ListNode* dummy = new ListNode(-1,head);
		ListNode* last_sorted = head,* curr = head->next;
		while(curr)
		{
			if(last_sorted->val <= curr->val)
				last_sorted = last_sorted->next;
			else{
				ListNode* prev = dummy;
				while(prev->next->val <= curr->val)
					prev = prev->next;
				last_sorted->next = curr->next;
				curr->next = prev->next;
				prev->next = curr;
			}
			curr = last_sorted->next;
		}
		return dummy->next;
    }
};
```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* slow = head,*fast = head->next;
        while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* mid = slow->next;
        slow->next = nullptr;
        return merge(sortList(head),sortList(mid));
    }

    ListNode* merge(ListNode* l1,ListNode* l2)
    {
        ListNode dummy(-1);
        ListNode* tail = &dummy;
        while(l1 && l2)
        {
            if(l1->val > l2->val)
                std::swap(l1,l2);
            tail->next = l1;
            l1 = l1->next;
            tail = tail->next;
        }
        if(l1)
            tail->next = l1;
        if(l2)
            tail->next = l2;
        return dummy.next;
    }
};
```

## [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

```
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        std::map<int,int> hash;
        for(auto num : nums)
            hash[num] ++;
        
        using DataPair = std::pair<int,int>;
        std::vector<DataPair> vec(hash.begin(),hash.end());
        std::sort(vec.begin(),vec.end(),[](const DataPair& p1,const DataPair& p2){return p1.second > p2.second;});
        
        std::vector<int> res;
        for(int i = 0;i < k;i++)
            res.emplace_back(vec[i].first);
        return res;
    }
};
```

## [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

```
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int curr = 0,left = 0,right = nums.size() - 1;
        while(curr <= right)
        {
            if(nums[curr] == 0)
                std::swap(nums[curr++],nums[left++]);
            else if(nums[curr] == 2)
                std::swap(nums[curr],nums[right--]);
            else
                curr++;
        }
    }
};
```

## [179. 最大数](https://leetcode-cn.com/problems/largest-number/)

```
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        if (std::all_of(nums.begin(), nums.end(), [](int x) { return x == 0; })) 
            return string("0");

        std::vector<std::string> strs(nums.size());
        std::transform(nums.begin(),nums.end(),strs.begin(),[](int num){return std::to_string(num);});
        std::sort(strs.begin(),strs.end(),[](const std::string& s1,const std::string& s2){return s1 + s2 > s2+ s1;});
        return std::accumulate(strs.begin(),strs.end(),std::string());
    }
};
```

## [324. 摆动排序 II](https://leetcode-cn.com/problems/wiggle-sort-ii/)

```
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        std::sort(nums.begin(),nums.end());
        size_t n = nums.size();
        size_t right = n,left = (n+1) >> 1;
        std::vector<int> tmp(nums);
        for(size_t i = 0;i < n;++i)
            nums[i] = (i & 1) ? tmp[--right] : tmp[--left];
    }
};
```

## [51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

```
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        std::vector<int> tmp(nums.size());
        return merge_sort(0, nums.size() - 1, nums, tmp);
    }
private:
    int merge_sort(int left, int right, std::vector<int>& nums, std::vector<int>& tmp) 
	{
        if (left >= right) 
			return 0;
        int m = (left + right) / 2;
        int res = merge_sort(left, m, nums, tmp) + merge_sort(m + 1, right, nums, tmp);
        int i = left, j = m + 1;
        for (int k = left; k <= right; k++)
            tmp[k] = nums[k];
        for (int k = left; k <= right; k++) 
        {
            if (i == m + 1)
                nums[k] = tmp[j++];
            else if (j == right + 1 || tmp[i] <= tmp[j])
                nums[k] = tmp[i++];
            else {
                nums[k] = tmp[j++];
                res += m - i + 1;
            }
        }
        return res;
    }
};
```

## [45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

```
class Solution {
public:
    string minNumber(vector<int>& nums) {
		if(nums.empty())
			return "";
		std::vector<std::string> strs;
		std::transform(nums.begin(), nums.end(), std::back_inserter(strs), [](const int x) {return std::to_string(x); });
		std::sort(strs.begin(),strs.end(),[](const std::string& s1,const std::string& s2){return s1+s2 < s2+s1;});
		std::string res;
		for(const auto& s: strs)
			res += s;
		return res;
    }
};
```

## [ 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

```
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        std::sort(nums.begin(),nums.end());
        int min_index = 0;
        while(nums[min_index] == 0)
            min_index++;
        for(int i = min_index;i < nums.size()-1;i++)
        {
            if(nums[i] == nums[i+1])
                return false;
        }
        return nums[4] - nums[min_index] <= 4;
    }
};
```

## [39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        std::sort(nums.begin(),nums.end());
        return nums[nums.size()/2];
    }
};
```

# 链表

## [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

```
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode dummy(-1);
        dummy.next = head;
        ListNode*prev = &dummy,*curr = head;
        while(curr)
        {
            if(curr->val == val)
                prev->next = curr->next;
            else
                prev = curr;
            curr = curr->next;
        }
        return dummy.next;
    }
};
```

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(l1 == nullptr)
            return l2;
        if(l2 == nullptr)
            return nullptr;
        int carry = 0;
        ListNode* dummy = new ListNode(-1);
        ListNode* curr = dummy;
        while(l1 || l2 || carry)
        {
            int v1 = l1 == nullptr ? 0 : l1->val;
            int v2 = l2 == nullptr ? 0 : l2->val;
            l1 = l1 == nullptr ? nullptr : l1->next;
            l2 = l2 == nullptr ? nullptr : l2->next;

            int tmp = v1 + v2 + carry;
            carry = tmp /10;
            ListNode* node = new ListNode(tmp%10);
            curr->next = node;
            curr = curr->next;            
        }
        return dummy->next;
    }
};
```

## [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1, s2;
        while (l1) {
            s1.push(l1 -> val);
            l1 = l1 -> next;
        }
        while (l2) {
            s2.push(l2 -> val);
            l2 = l2 -> next;
        }
        int carry = 0;
        ListNode* res = nullptr;
        while (!s1.empty() or !s2.empty() or carry != 0) {
            int a = s1.empty() ? 0 : s1.top();
            int b = s2.empty() ? 0 : s2.top();
            if (!s1.empty()) s1.pop();
            if (!s2.empty()) s2.pop();
            int cur = a + b + carry;
            carry = cur / 10;
            auto curnode = new ListNode(cur % 10);
            curnode -> next = res;
            res = curnode;
        }
        return res;
    }
};
```

## [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

```
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        if(head == nullptr)
            return head;
        ListNode* h_big = new ListNode(-1),*h_small = new ListNode(-1);
        ListNode* curr = head,*curr_big = h_big,*curr_small = h_small;
        while(curr)
        {
            if(curr->val >= x)
            {
                curr_big->next = curr;
                curr_big = curr_big->next;
            }
            else
            {
                curr_small->next = curr;
                curr_small = curr_small->next;
            }
            curr = curr->next;
        }
        curr_big->next = nullptr;
        curr_small->next = h_big->next;
        return h_small->next;
    }
};
```

## [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

```
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;

        ListNode* odd = head;
        ListNode* even_head = head->next;
        ListNode* even = even_head;
        while(even != nullptr && even->next != nullptr)
        {
            odd->next = even->next;
            odd = odd->next;
            even->next = odd->next;
            even = even->next;
        }
        odd->next = even_head;
        return head;
    }
};
```

```
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == nullptr)
            return head;
        ListNode* h_even = new ListNode(-1),*h_odd = new ListNode(-1);
        ListNode* p = head,*curr_even = h_even,*curr_odd = h_odd;
        bool even = true;
        while(p)
        {
            if(even)
            {
                curr_even->next = p;
                curr_even = curr_even->next;
            }
            else
            {
                curr_odd->next = p;
                curr_odd = curr_odd->next;
            }
            even = !even;
            p = p->next;
        }
        curr_odd->next = nullptr;
        curr_even->next = h_odd->next;
        return h_even->next;
    }
};
```

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* new_head = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return new_head;
    }
};
```

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* prev = nullptr,*curr = head;
        while(curr)
        {
            ListNode* tmp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = tmp;
        }
        return prev;
    }
};
```

## [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == nullptr || head->next == nullptr || k ==0)
            return head;
        int len = 1;
        ListNode* curr = head;
        while(curr->next)
        {
            len++;
            curr = curr->next;
        }
        
        if(k%len == 0)
            return head;

        curr->next = head;
        int last = len - k%len;
        while(last--)
            curr = curr->next;
        
        ListNode* new_head = curr->next;
        curr->next = nullptr;
        return new_head;
    }
};
```

## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
		ListNode* curr = dummy;
		while(l1 && l2)
		{
			auto &node = l1->val <= l2->val ? l1 : l2;
			curr->next = node;      
			curr = curr->next;
			node = node->next;			
		}
		curr->next = l1 == nullptr ? l2 : l1;
		return dummy->next;
    }
};
```

## [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode* new_head = head->next;
        head->next = swapPairs(new_head->next);
        new_head->next = head;
        return new_head;
    }
};
```

```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode(-1,head);
        ListNode* curr = dummy;
        while(curr->next && curr->next->next)
        {
            ListNode* node1 = curr->next;
            ListNode* node2 = curr->next->next;
            curr->next = node2;
            node1->next = node2->next;
            node2->next = node1;
            curr = node1;
        }
        return dummy->next;
    }
};
```

## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *pA = headA,*pB = headB;
        while(pA != pB)
        {
            pA = (pA == nullptr ? headB : pA->next);
            pB = (pB == nullptr ? headA : pB->next);
        }
        return pA;
    }
};
```

## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

```
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return true;
        
        std::vector<int> values;
        ListNode* curr = head;
        while(curr)
        {
            values.push_back(curr->val);
            curr = curr->next;
        }

        for(int i = 0,j = values.size()-1;i < j;i++,j--)
        {
            if(values[i] != values[j])
                return false;
        }
        return true;
    }
};
```

```
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return true;
        
        ListNode* left_end = find_end_half(head);
        ListNode* right_rev = reverse(left_end->next);
        ListNode* p1 = head,*p2 = right_rev;
        bool res = true;
        while(res && (p1 && p2))
        {
            if(p1->val != p2->val)
                res = false;
            
            p1 = p1->next;
            p2 = p2->next;
        }
        left_end->next = reverse(right_rev);
        return res;
    }

    ListNode* reverse(ListNode* head)
    {
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode*  new_head = reverse(head->next);
        head->next->next = head;
        head->next = nullptr;
        return new_head;
    }
    
    ListNode* find_end_half(ListNode* head)
    {
        ListNode *slow = head,*fast = head;
        while(fast->next && fast->next->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

## [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
		if(head == nullptr || head->next == nullptr)
            return head;
		ListNode* dummy = new ListNode(INT_MIN,head);
		ListNode* curr = head->next,*prev = head;
		while(curr)
		{
			if(curr->val != prev->val)
			{
				prev = prev->next;
				curr = curr->next;
			}
			else
			{
				while(curr && curr->val == prev->val)
					curr = curr->next;
				ListNode* p = dummy;
				while(p->next->val != prev->val)
					p = p->next;
				p->next = curr;
				prev = p;
			}		
		}
        return dummy->next;
    }
};
```

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        if(head->val != head->next->val)
            head->next = deleteDuplicates(head->next);
        else
        {
            ListNode* move = head->next;
            ListNode* prev = head;
            while(move && move->val == head->val){
                move = move->next;
                prev = prev->next;
            }
            return deleteDuplicates(prev);            
        }
        return head;
    }
};
```

## [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
            
        ListNode* curr = head;
        while(curr->next)
        {
            if(curr->val == curr->next->val)
                curr->next = curr->next->next;
            else
                curr = curr->next;
        }
        return head;
    }
};
```

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        if(head->val != head->next->val)
            head->next = deleteDuplicates(head->next);
        else
        {
            ListNode* move = head->next;
            while(move && head->val == move->val)
                move = move->next;
            return deleteDuplicates(move);
        }
        return head;
    }
};
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
		ListNode* fast = head;
		while(n--)
		{
			if(fast == nullptr)
				return nullptr;
			fast = fast->next;
		}
		if(fast == nullptr)
			return head->next;
		ListNode* slow = head;
		while(fast->next)
		{
			fast = fast->next;
			slow = slow->next;
		}
		slow->next = slow->next->next;
		return head;
    }
};
```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* slow = head,*fast = head->next;
        while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        } 

        ListNode* mid = slow->next;
        slow->next = nullptr;
        return merge(sortList(head),sortList(mid));
    }

     ListNode* merge(ListNode* l1, ListNode* l2)
     {
         ListNode node(-1);
         ListNode* dummy = &node,*curr = dummy;
         while(l1 && l2)
         {
            if(l1->val > l2->val)
                std::swap(l1,l2);
            curr->next = l1;
            l1 = l1->next;
            curr = curr->next;
         }

         curr->next = l1 ? l1 : l2;
         return dummy->next;
     }
};
```

## [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return false;
        ListNode* slow = head,*fast = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;

            if(fast == slow)
                return true;
        }
        return false;
    }
};
```

## [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return nullptr;
        ListNode*fast = head,* slow = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow)
            {
                fast = head;
                while(fast != slow)
                {
                    fast = fast->next;
                    slow = slow->next;
                }
                return fast;
            }

        }
        return nullptr;
    }
};
```

## [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```
class LRUCache {
public:
    int cap_;
    std::list<std::pair<int,int>> data_;
    using Iter = std::list<std::pair<int,int>>::iterator;
    std::unordered_map<int,Iter> dict_;

    LRUCache(int capacity) : cap_(capacity){
    }
    
    int get(int key) {
        if(dict_.find(key) == dict_.end())
            return -1;
        int res = dict_[key]->second;
        data_.erase(dict_[key]);
        data_.emplace_front(key,res);
        dict_[key] = data_.begin();
        return res;
    }
    
    void put(int key, int value) {
        if(dict_.find(key) != dict_.end())
            data_.erase(dict_[key]);          
        
        data_.emplace_front(key,value);
        dict_[key] = data_.begin();

        while(data_.size() > cap_)
        {
            dict_.erase(data_.back().first);
            data_.pop_back();
        }
    }
};
```

## [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

```
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* dummy = new ListNode(-1,head);
        ListNode* last_sorted = head;
        ListNode* curr = head->next;
        
        while (curr != nullptr) {
            if (last_sorted->val <= curr->val)
                last_sorted = last_sorted->next; //@ 如果顺序排列，插入到最后一个位置
            else 
            {
                ListNode* prev = dummy;    //@ 否则将 curr 节点插入到 list 的前面部分                
                while (prev->next->val <= curr->val)
                    prev = prev->next;
                
                //@ 此时 prev 指向的 node 是小于 curr 的最大值 
                last_sorted->next = curr->next;  //@ 首先记录 curr->next 的值
                curr->next = prev->next;        //@ 将 curr 插入到 prev 和 prev->next 之间
                prev->next = curr;              
            }  
            curr = last_sorted->next;
            //@ 向后移动考察值
        }
        
        return dummy->next; 
    }
};
```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```
class Solution {
public:
    ListNode* sortList(ListNode* head) 
    {
        if (nullptr == head || nullptr == head->next) 
            return head;
        auto slow = head, fast = head;
        while (fast->next && fast->next->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
             
        //@ 切链
        fast = slow->next;
        slow->next = nullptr;
        return merge(sortList(head), sortList(fast));
    }

private:
    ListNode* merge(ListNode* l1, ListNode* l2) 
    {
        ListNode *dummy = new ListNode(-1);
        ListNode *curr = dummy;
        while (l1 && l2) {
            auto &node = l1->val < l2->val ? l1 : l2;
            curr->next = node;
            curr = curr->next;
            node = node->next;
        }
        curr->next = l1 ? l1 : l2;
        return dummy->next;
    }
};
```

## [ 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

**思路**

- `std::verse`

```
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        std::vector<int> res;
        while(head != nullptr)
        {
            res.push_back(head->val);
            head = head->next;
        }
        std::reverse(res.begin(),res.end());
        return res;
    }
};
```


## [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

```
class Solution {
public:
    
    struct comp{
        bool operator()(ListNode* l1,ListNode* l2)
        {
            return l1->val > l2->val;
        }
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        std::priority_queue<ListNode*,std::vector<ListNode*>,comp> pq;
        for(const auto& l : lists)
        {
            if(l)
                pq.push(l);
        }
        
        ListNode node(-1);
        ListNode* dummy = &node,*curr = dummy;
        while(!pq.empty())
        { 
            ListNode* tmp = pq.top();
            pq.pop();
            curr->next = tmp;
            curr = curr->next;
            if(tmp->next)
                pq.push(tmp->next); 
        }
        return dummy->next;
    }
};
```



## [35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

```
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head == nullptr)
            return nullptr;
        
        std::unordered_map<Node*,Node*> hash;
        Node* curr = head;
        while(curr)
        {
            hash[curr] = new Node(curr->val);
            curr = curr->next;
        }

        curr = head;
        while(curr)
        {
            hash[curr]->next = hash[curr->next];
            hash[curr]->random = hash[curr->random];
            curr = curr->next;
        }
        return hash[head];
    }
};
```

```
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head == nullptr)
            return head;

        Node* curr = head;
        while(curr)
        {
            Node* node = new Node(curr->val);
            node->next = curr->next;
            curr->next = node;            
            curr = node->next;
        }

        curr = head;
        while(curr)
        {
            if(curr->random != nullptr)
                curr->next->random = curr->random->next;
            curr = curr->next->next;
        }

        curr = head->next;
        Node* prev = head;
        Node* res = head->next;
        while(curr->next != nullptr)
        {
            prev->next = prev->next->next;
            curr->next = curr->next->next;
            prev = prev->next;
            curr = curr->next;
        }
        prev->next = nullptr;
        return res;
    }
};
```

## [52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(!headA || !headB)
            return nullptr;
        ListNode* h1 = headA,*h2 = headB;
        while(h1 != h2)
        {
            h1 = (h1 != nullptr) ? h1->next : headB;
            h2 = (h2 != nullptr) ? h2->next : headA; 
        }
        return h1;
    }
};
```

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        std::unordered_set<ListNode *> s;
        ListNode*hA = headA,*hB = headB;
        while(hA)
        {
            s.insert(hA);
            hA = hA->next;
        }

        while(hB)
        {
            if(s.find(hB) != s.end())
                return hB;
            hB = hB->next;
        }
        return nullptr;
    }
};
```

# 双指针

## [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

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

## [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

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

## [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

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

## [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)

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

## [680. 验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)

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

## [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

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

## [57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

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

## [21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

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

## [57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

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

## [392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

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

# 数组

## [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

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

## [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

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

## [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

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

## [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

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

## [80. 删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

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

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

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

## [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
		std::sort(nums.begin(),nums.end());	
		vector<vector<int>> res;
		
		int n = nums.size();
		for(int i = 0;i < nums.size();i++)
		{	
			if(nums[i] > 0)
				break;
			if(i > 0 && nums[i] == nums[i-1])
				continue;
				
			int left = i + 1,right = n - 1;
			while(left < right)
			{
				int total = nums[i] + nums[left] + nums[right];
				if(total == 0){
					res.push_back({nums[i],nums[left],nums[right]});
					left++;
					right--;
					while(left < right && nums[left] == nums[left-1])
						left++;
					while(left < right && nums[right] == nums[right+1])
						right--;						
				}
				else if(total > 0)
					right--;
				else
					left++;				
			}
		}
		return res;
    }
};
```

## [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

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

## [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

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

## [435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

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

## [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

```
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
		if(points.empty())
			return 0;
		
		std::sort(points.begin(), points.end());
		std::vector<std::vector<int>> vec;
		vec.push_back(points[0]);
		for(int i = 1;i < points.size();i++)
		{
			auto curr = points[i];
			if(curr[0] <= vec.back()[1])
			{
				vec.back()[0] = std::max(curr[0],vec.back()[0]);
				vec.back()[1] = std::min(curr[1],vec.back()[1]);
			}
			else
				vec.push_back(points[i]);
		}
		return vec.size();
    }
};
```

## [57. 插入区间](https://leetcode-cn.com/problems/insert-interval/)

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

## [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

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

## [661. 图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

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

## [03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

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

## [04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

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

## [11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

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

## [50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

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

## [66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

```
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int n = a.size();
        vector<int> res(n,1);
        for(int i = 1;i < n;i++)
            res[i] = res[i-1] * a[i-1];
        int tmp = 1;
        for(int i = n-2;i >= 0;i--)
        {
            tmp *= a[i+1];
            res[i] *= tmp;
        }
        return res;
    }
};
```

# 数学

## [16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

```
class Solution {
public:
    double myPow(double x, int n) {
        long exp = n;
        if(exp < 0)
            return 1.0 / quick_pow(x,-exp);
        
        return quick_pow(x,n);
    }

    double quick_pow(double x,long n)
    {
        if(n == 0)
            return 1.0;
        if(n == 1)
            return x;
        
        double t = quick_pow(x,n >>1);
        double res = t * t;

        return (n & 1) ? res * x : res;
    }
};
```

## [43. 1～n 整数中 1 出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

```
class Solution {
public:
    int countDigitOne(int n) {
        int res = 0;
        long i = 1;
        while(n / i)
        {
            long high = n / (10 * i);
            long curr = (n / i) % 10;
            long low = n - (n / i) * i;

            if(curr == 0)
                res += high * i;
            else if(curr == 1)
                res += high * i + low + 1;
            else
                res += high * i + i;
            
            i *= 10;
        }
        return res;
    }
};
```

## [44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

```
class Solution {
public:
    int findNthDigit(int n) {
		long new_n = n;
		for(int width = 1;;width++)
		{
			if(width * pow(10,width) > new_n)
				return std::to_string(new_n/width)[new_n%width] - '0';
			new_n += pow(10,width);
		}
		return -1;
    }
};
```



## [ 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

```
class Solution {
public:
    int lastRemaining(int n, int m) {
        if(n == 1)
            return 0;
        int res;
        for(int i = 2;i <= n;i++)
        {
            res = (res + m) % i;
        }
        return res;
    }
};
```

## [64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

```
class Solution {
public:
    int sumNums(int n) {
        n && (n+=sumNums(n-1));
        return n;
    }
};
```

# 位运算

## [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

```
class Solution {
public:
    int hammingDistance(int x, int y) {
		int n = x ^ y;
		int res = 0;
		while(n)
		{
			res ++;
			n &= (n-1);
		}
		return res;
    }
};
```

## [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

```
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res;
        for(uint32_t i = 0;i < 32;i++)
        {
            res = (res << 1) | (n & 1);
            n >>= 1;
        }
        return res;
    }
};
```

## [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for(const auto& num : nums)
        {
            res ^= num;
        }
        return res;
    }
};
```

## [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)

```
class Solution {
public:
    bool isPowerOfFour(int n) {
        return n > 0 && 0 == (n & (n - 1)) && n%3 == 1;
    }
};
```

## [318. 最大单词长度乘积](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

```
class Solution {
public:
    int convert(const std::string& str)
    {
        int res = 0;
        for(auto c : str)
        {
            res |= 1 << (c - 'a');
        }
        return res;
    } 

    int maxProduct(vector<string>& words) {
        int n = words.size();
        std::vector<int> masks(n,0);
        for(int i = 0;i < n;i++)
        {
            masks[i] = convert(words[i]);
        }

        int res = 0;
        for(int i = 0;i < n;++i)
        {
            for(int j = 0;j < i;++j)
            {
                if((masks[i] & masks[j]) == 0 && words[i].length() * words[j].length() > res)
                {
                    res = words[i].length() * words[j].length();
                }
            }
        }
        return res;
    }
};
```

## [693. 交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/)

```
class Solution {
public:
    bool hasAlternatingBits(int n) {
        n = n ^ (n >> 1);
        return (n & (long(n) + 1)) == 0;
    }
};
```

## [476. 数字的补数](https://leetcode-cn.com/problems/number-complement/)

```
class Solution {
public:
    int findComplement(int num) {
        int i = 1;
        for(;i < num;i++)
            i <<= 1;
        return num ^ i;
    }
};
```

## [260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

```
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int xnor = 0;
        for(const auto num : nums)
            xnor ^= num;

        int mask = 1;
        while((mask & xnor) == 0)
            mask <<= 1;
        
        int a = 0,b = 0;
        for(auto const x : nums)
        {
            if(mask & x)
                a ^= x;
            else
                b ^= x;
        }
        return {a,b};
    }
};
```

## [15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

```
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while(n)
        {
            n &= (n-1);
            res++;
        }
        return res;
    }
};
```

## [56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

```
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
		if(nums.empty())
			return {};
			
		int XOR = 0;
		for(const auto n : nums)
			XOR ^= n;
		int uncommon = 1;
		for(int i = 0;i < 32;i++)
		{
			if((uncommon << i) & XOR)
			{
				uncommon <<= i;
				break;
			}				
		}
		
		int a = 0,b = 0;
		for(const auto n : nums)
		{
			if(uncommon & n)
				a ^= n;
			else
				b ^= n;
		}
		return {a,b};
    }
};
```

## [56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        std::unordered_map<int,int> hash;
        for(const auto n : nums)
        {
            hash[n]++;
            if(hash[n] == 3)
                hash.erase(n);
        }
        return hash.begin()->first;
    }
};
```

## [65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

```
class Solution {
public:
    int add(int a, int b) {
        while(b)
        {
            int tmp = a ^ b;
            int carry = (unsigned)(a&b) << 1;
            b = carry;
            a = tmp;
        }
        return a;
    }
};
```

# 栈和队列

## [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

```
class MyStack {
public:
    MyStack() {

    }
    
    void push(int x) {
        q.push(x);
    }
    
    int pop() {
        std::queue<int> tmp;
        while(q.size() > 1)
        {
            tmp.push(q.front());
            q.pop();
        }

        int res = q.front();
        q.pop();
        q = std::move(tmp);
        return res;
    }
    
    int top() {
        int res = pop();
        push(res);
        return res;
    }
    
    bool empty() {
        return q.empty();
    }

    std::queue<int> q;
};
```

## [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

```
class MyQueue {
public:
    MyQueue() {

    }
    
    void push(int x) {
        s_in.push(x);
    }
    
    int pop() {
        if(s_out.empty())
        {
            while(!s_in.empty())
            {
                s_out.emplace(s_in.top());
                s_in.pop();
            }
        }

        if(s_out.empty())
            return -1;
        int res = s_out.top();
        s_out.pop();
        return res;
    }
    
    int peek() {
        if(s_out.empty())
        {
            while(!s_in.empty())
            {
                s_out.emplace(s_in.top());
                s_in.pop();
            }
        }

        if(s_out.empty())
            return -1;
        int res = s_out.top();
        return res;
    }
    
    bool empty() {
        return s_in.empty() && s_out.empty();
    }

    std::stack<int> s_in,s_out;
};
```

## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

```
class MinStack {
public:
    MinStack() {

    }
    
    void push(int val) {
        s_data.push(val);
        if(s_min.empty() || s_min.top() > val)
            s_min.push(val);
        else
            s_min.push(s_min.top());

    }
    
    void pop() {
        s_data.pop();
        s_min.pop();
    }
    
    int top() {
        return s_data.top();
    }
    
    int getMin() {
        return s_min.top();
    }
    
private:
    std::stack<int> s_data;
    std::stack<int> s_min;
};
```

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```
class Solution {
public:
    bool isValid(string s) {
        std::unordered_map<char,char> pairs ={
            {'}','{'},
            {']','['},
            {')','('},
        };

        std::stack<char> sk;
        for(const auto c : s)
        {
            if(pairs.find(c) != pairs.end())
            {
                if(sk.empty() || sk.top() != pairs[c])
                    return false;
                sk.pop();
            }         
            else
                sk.push(c);
        }
        return sk.empty() ? true : false;
    }
};
```

## [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

```
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        if(temperatures.empty())
            return {};
        int n= temperatures.size();
        vector<int> res(n);
        std::stack<int> st;
        st.push(0);

        for(int i = 1;i < n;i++)
        {
            int curr = temperatures[i];
            if(curr > temperatures[st.top()])
            {
                while(!st.empty() && curr > temperatures[st.top()])
                {
                    res[st.top()] = i - st.top();
                    st.pop();                    
                }
            }
            
            st.push(i);

        }
        return res;
    }
};
```

## [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

```
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        struct compare{
            bool operator() (const ListNode* l1,const ListNode* l2)
            {
                return l1->val > l2->val;
            }
        };

        std::priority_queue<ListNode*,std::vector<ListNode*>,compare> pq;
        for(const auto list : lists)
        {
            if(list)
                pq.push(list);
        }

        ListNode dummy(-1);
        ListNode *p = &dummy,*curr = &dummy;
        while(!pq.empty())
        {
            ListNode* tmp = pq.top();
            pq.pop();
            curr->next = tmp;
            curr = curr->next;
            if(tmp->next)
                pq.push(tmp->next);
        }
        return dummy.next;
    }
};
```

## [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

```
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        if(k <= 0)
            return {};
        
        std::vector<int> res;
        std::deque<int> dq;
        for(int i = 0;i < k;i++)
        {
            while(!dq.empty() && nums[dq.back()] <= nums[i])
                dq.pop_back();
            dq.emplace_back(i);
        }
        res.emplace_back(nums[dq.front()]);

        for(int i = k;i < nums.size();i++)
        {
            while(!dq.empty() && i - dq.front() >= k)
                dq.pop_front();
             while(!dq.empty() && nums[dq.back()] <= nums[i])
                dq.pop_back();
            dq.emplace_back(i);
            res.emplace_back(nums[dq.front()]);
        }
        return res;
    }
};
```

## [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
           unsigned long size = heights.size();
        if (size == 1) {
            return heights[0];
        }


        heights.insert(heights.begin(),0);
        heights.push_back(0);

        std::stack<int> st;
        st.push(0);
        int res = 0;
        for(int i = 1;i < heights.size();i++)
        {
            while(!st.empty() && heights[i] < heights[st.top()])
            {
                int mid = st.top();
                st.pop();
                if(st.empty())
                    break;
                int h = heights[mid];
                int right = i-1;
                int left = st.top()+1;
                int w = right - left + 1;
                res = std::max(res,h*w);
            }
            st.push(i);
        }
        return res;
    }
};
```

## [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

```
class Solution {
public:
    int trap(vector<int>& height) {
        std::stack<int> st;
        st.push(0);
        int res = 0;
        for(int i = 1;i < height.size();i++)
        {
            while(!st.empty() && height[i] > height[st.top()])
            {
                int mid = st.top();
                st.pop();
                if(st.empty())
                    break;
                
                int left = st.top();
                int w = i - left - 1;
                int h = std::min(height[i],height[left]) - height[mid];
                res += w*h;
            }
            st.push(i);
        }
        return res;
    }
};
```

## [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

```
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        if(pushed.size() != popped.size())
            return false;
        std::stack<int> st;
        int j = 0; 
        for(const auto v : pushed)
        {
            st.emplace(v);
            while(!st.empty() && st.top() == popped[j])
            {
                st.pop();
                j++;
            }
        }
        return st.empty();
    }
};
```

## [59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

```
class MaxQueue {
public:
    MaxQueue() {

    }
    
    int max_value() {
        if(max_q.empty())
            return -1;
        return max_q.front();
    }
    
    void push_back(int value) {
		data_q.emplace(value);
		while(!max_q.empty() && max_q.back() <= value)
			max_q.pop_back();
		max_q.emplace_back(value);	
    }
    
    int pop_front() {
		if(data_q.empty())
			return -1;
		int res = data_q.front();
		data_q.pop();
		while(!max_q.empty() && max_q.front() == res)
			max_q.pop_front();
		return res;
    }
	
	std::queue<int> data_q;
	std::deque<int> max_q;
};
```

# 字符串

## [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

中心扩展：

- 中心点即 left 指针和 right 指针初始化指向的地方，可能是一个也可能是两个
- 中心点有 2 * len - 1 个，分别是 len 个单字符和 len - 1 个双字符
- left 和中心点有一个很明显的 2 倍关系的，right 可能和 left 指向同一个（中心点是偶数时），也可能往后移动一个（中心点是奇数时）

```
class Solution {
public:
    int countSubstrings(string s) {
		if(s.empty())
			return 0;
		int len = s.length();
		int res = 0;
		for(int center = 0;center < 2 *len - 1;center++)
		{
			int left = center / 2;
			int right = left + center%2; 
			
			while(left >= 0 && right < len && s[left] == s[right])
			{
				res++;
				left--;
				right++;
			}		
		}
		return res;
    }
};
```

## [541. 反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

```
class Solution {
public:
    string reverseStr(string s, int k) {
		for(int i = 0;i < s.length(); i += 2*k)
		{
			if(i+k <= s.length())
				std::reverse(s.begin()+i,s.begin()+i+k);
			else
				std::reverse(s.begin()+i,s.end());
		}
		return s;
    }
};
```

## [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

借助 stringstream ：

```
class Solution {
public:
    string reverseWords(string s) {
        std::stringstream ss(s);
        std::string res, tmp;
        while(ss >> tmp)
            res = tmp + ' ' + res;
        return res.substr(0, res.size() - 1);
    }
};
```

```
class Solution {
public:
    string reverseWords(string s) {
		while(!s.empty() && s.back() == ' ')
			s.pop_back();
		while(!s.empty() && s[0] == ' ')
			s.erase(0,1);
		if(s.empty())
			return "";
		std::string res;
		int left = 0;
		for(int i = 0; i< s.length();)
		{
			if(s[i] == ' '){
				res = s.substr(left,i-left) + " " + res;
				while(s[i] == ' ')
					i++;
				left = i;
			}
			
			i++;	
		}
		res = s.substr(left,s.length()-left) + " " + res;
		return res.substr(0,res.length()-1);
    }
};

```

原地翻转：

```
class Solution {
public:
    string reverseWords(string s) {
        reverse(s.begin(), s.end()); //@ 整体反转                       
        int start = 0, end = s.size() - 1;
        while (start < s.size() && s[start] == ' ') 
            start++;	//@ 首空格
        while (end >= 0 && s[end] == ' ') 
            end--;     //@ 尾空格
        if (start > end) 
            return "";  //@ 全是空格
        
        //@ 逐个单词翻转
        for (int curr = start; curr <= end;) 
        {   
            //@ 当前是空格的话，直接跳过
            while (s[curr] == ' '&& curr <= end) 
                curr++;
            //@ 当前不是空格的话，翻转单个词语
            int pos = curr;
            while (s[pos] != ' '&& pos <= end) 
                pos++;
            reverse(s.begin() + curr, s.begin() + pos);
            curr = pos;
        }
        
        //@ 处理中间冗余空格
        int tail = start;                                  
        for (int i = start; i <= end; i++) 
        {
            if (s[i] == ' ' && s[i - 1] == ' ') 
                continue;
            s[tail++] = s[i];
        }
        return s.substr(start, tail - start);
    }
};
```

倒叙：

```
class Solution {
public:
    string reverseWords(string s) {
        string res;
        int len = 0,curr = s.length() - 1;
        for(; curr >= 0;curr--)
        {
            if(s[curr] == ' ')
            {
                if(len)
                {
                    //@ curr 当前是空格，所以需要加1
                    res += s.substr(curr+1,len) + ' ';
                    len = 0;
                }
                continue;
            }
            len++;
        } 
        if(len)
        {
            res += s.substr(0,len);
            return res;
        }           
        else
            return res.substr(0,res.length()-1); //@ 之前多加了一个空格
    }
};
```

## [224.基本计算器](https://leetcode-cn.com/problems/basic-calculator)

```
class Solution {
public:
    int calculate(string s) {
		long res = 0,sign = 1,num = 0;
		std::stack<int> ops;
		for(const auto c : s)
		{
			if(isdigit(c))
				num = num * 10 + c - '0';
			else if(c == '+' || c == '-')
			{
				res += num * sign;
				sign = (c == '+' ? 1 : -1);
				num = 0;
			}
			else if(c == '(')
			{
				ops.emplace(res);
				ops.emplace(sign);
				res = 0;
				sign = 1;
				num = 0;
			}
			else if(c == ')')
			{
				res += num * sign;
				num = 0;
				res *= ops.top();
				ops.pop();
				res += ops.top();
				ops.pop();
			}			
		}
		
		res += sign * num;
		return res;
    }
};

```

## [227.基本计算器II](https://leetcode-cn.com/problems/basic-calculator-ii)

```
class Solution {
public:
    int calculate(string s) {
		std::vector<long> nums;
		char prev_sign = '+';		
		long num = 0;
		int len = s.length();
		for(int i = 0;i < len;i++)
		{
			if(isdigit(s[i]))	
				num = num * 10 + s[i] - '0';
				
			if(!isdigit(s[i]) && s[i] != ' ' || i == len-1)
			{
				switch(prev_sign)
				{
				case '+':
					nums.emplace_back(num);
					break;
				case '-':
					nums.emplace_back(-num);
					break;
				case '*':
					nums.back() *= num;
					break;
				default:
					nums.back() /= num;
					break;					
				}
				
				prev_sign = s[i];
				num = 0;
			}			
		}
		return std::accumulate(nums.begin(),nums.end(),0);
    }
};

```

## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
		int res = 0,left = -1;
		std::unordered_map<int,int> hash;
		for(int i = 0;i < s.length();i++)
		{	
			auto c = s[i];
			if(hash.count(c) && left <= hash[c])
				left = hash[c];
			hash[c] = i;
			res = std::max(res,i-left);
		}
		return res;
    }
};
```

## [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

```
class Solution {
public:
    int longestValidParentheses(string s) {		
		int res = 0;
		std::stack<int> sk;
		sk.push(-1);
		for(int i = 0;i < s.length();i++)
		{
			if(s[i] == '(')
				sk.push(i);
			else
			{
				sk.pop();
				if(sk.empty())
					sk.push(i);
				else
					res = std::max(i-sk.top(),res);
			}
		}
		return res;
    }
};
```

## [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

```
class Solution {
public:
    string findLongestWord(string s, vector<string>& dictionary) {
		std::string res;
		for(const auto& str : dictionary)
		{
			int l1 = res.length(),l2 = str.length();
			if(l1 > l2 || l1 == l2 && res.compare(str) < 0)
				continue;
			if(is_substr(s,str))
				res = str;		
		}
		return res;
    }
	
	bool is_substr(const std::string& s1,const std::string& s2)
	{
		int i = 0,j = 0;
		while(i < s1.length() && j < s2.length())
		{
			if(s1[i] == s2[j])
				j++;
			i++;
		}
		return j == s2.length();
	}
};
```

## [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

KMP：

```
class Solution {
public:
    std::vector<int> get_next(std::string& str)
    {
        int len = str.length();
        std::vector<int> res{-1};
        int j = 0,k = -1;
        while(j < len -1)
        {
            if(k == -1 || str[j] == str[k])
            {
                j++;
                k++;
                res.emplace_back(k);
            }
            else
                k = res[k];
        }
        return res;
    }

    int strStr(string haystack, string needle) {
        if(needle.empty())
            return 0;
        int i = 0,j = 0;
        int len1 = haystack.length(),len2 = needle.length();
        std::vector<int> next = get_next(needle);
        while(i < len1 && j < len2)
        {
            if(j == -1 || haystack[i] == needle[j])
                i++,j++;
            else
                j = next[j];
        }
        if(j == len2)
            return i - j;
        return -1;
    }
};
```

## [ 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

```
class Solution {
public:
    string replaceSpace(string s) {
        std::string res;
        for(const auto& c : s)
        {
            if(c == ' ')
                res += "%20";
            else
                res += c;
        }
        return res;
    }
};
```

## [20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

```
class Solution {
public:
    bool isNumber(string s) {
        while(!s.empty() && s[0] == ' ')
            s.erase(0,1);
        while(!s.empty() && s.back() == ' ')
            s.pop_back();
        if(s.empty())
            return false;
        bool dot = false,num = false,exp = false;
        int len = s.length();
        int last = len - 1;

        for(int i = 0;i < len;i++)
        {
            switch(s[i])
            {
                case '+':
                case '-':
                    if( (i > 0 && s[i-1] != 'e' && s[i-1] != 'E') || i == last)
                        return false;
                    break;
                case 'e':
                case 'E':
                    if(exp || !num || i == last)
                        return false;
                    exp = true;
                    break;
                case '.':
                    if(dot || exp || (i == last && !num))
                        return false;
                    dot = true;
                    break;
                default:
                    if(s[i] >= '0' && s[i] <= '9')
                        num = true;
                    else
                        return false;                
            }
        }
        return true;
    }
};
```


## [50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

```
class Solution {
public:
    char firstUniqChar(string s) {
        std::unordered_map<int,int> hash;
        for(const auto c : s)
            hash[c] ++;
        for(const auto c: s)
        {
            if(hash[c] == 1)
                return c;
        }
        return ' ';
    }
};
```

## [58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

```
class Solution {
public:
    string reverseWords(string s) {
        if(s.empty())
            return "";
        
        std::string res;
        int len = 0;
        for(int i = s.length() - 1; i >= 0;i--)
        {
            if(s[i] == ' ' && len)
            {
                res += s.substr(i+1,len) + ' ';
                len = 0;
                continue;
            }

            if(s[i] != ' ')
                len++;
        }

        if(len)
            res += (s.substr(0,len));
        
        while(res.back() == ' ')
            res.pop_back();
        return res;
    }
};
```

## [58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

```
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        std::reverse(s.begin(),s.begin()+n);
        std::reverse(s.begin()+n,s.end());
        std::reverse(s.begin(),s.end());
        return s;
    }
};
```

```
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        s += s;
        return s.substr(n,s.length()/2);
    }
};
```

## [67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

```
class Solution {
public:
    int strToInt(string str) {
        while(!str.empty() && str[0] == ' ')
            str.erase(0,1);
        while(!str.empty() && str.back() == ' ')
            str.pop_back();
        int sign = 1;
        if(str[0] == '-' || str[0] == '+')
        {
            if(str[0] == '-')                
                sign = -1;
            str.erase(0,1);
        }

        long res = 0;
        for(auto s : str)
        {
            if(!isdigit(s))
                break;
            
            res = res * 10 + s - '0';
            if(sign == 1 && res > INT_MAX)
                return INT_MAX;
            if(sign == -1 && res > INT_MAX)
                return INT_MIN;
        }
        return sign*res;
    }
};
```

# 哈希

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
		map<int,int> dict;
		for(int i = 0;i < nums.size();i++)
		{
			if(dict.find(target - nums[i]) != dict.end())
				return {dict[target - nums[i]],i};
			
			dict[nums[i]] = i;
		}
		return {};
    }
};
```

## [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
		std::set<int> hash(nums.begin(),nums.end());
		int res = 0,curr = 1;
		for(const auto n : hash)
		{
			if(hash.find(n-1)!= hash.end())
				curr += 1;
			else
				curr = 1;
			res = std::max(res,curr);
		}
		return res;		
    }
};
```

## [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```
class LRUCache {
	int cap_;
    list<pair<int, int>> record_;  //@ pair<key,value>
	using Iter = list<pair<int,int>>::iterator;	
    unordered_map<int, Iter> dict_;   
	
public:
    LRUCache(int capacity):cap_(capacity) {
    }
    
    int get(int key) {
		if(dict_.find(key) == dict_.end())
			return -1;
		int res = dict_[key]->second;
		record_.erase(dict_[key]);
		record_.push_front({key,res});
		dict_[key] = record_.begin();
		return res;
    }
    
    void put(int key, int value) {
		if(dict_.find(key) != dict_.end())		
			record_.erase(dict_[key]);
		
		record_.push_front({key,value});
		dict_[key] = record_.begin();
		
		if(record_.size() > cap_)
		{	
			dict_.erase(record_.back().first);
			record_.pop_back();
		}	
    }
};
```

## [380. 常数时间插入、删除和获取随机元素](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/)

```
class RandomizedSet {
    vector<int> data_;
    unordered_map<int, int> dict_;
	
public:
    RandomizedSet() {}
    bool insert(int val) {
        if (dict_.find(val) != dict_.end()) 
			return false;
        data_.push_back(val);
        dict_[val] = data_.size() - 1;
        return true;
    }
	
    bool remove(int val) {
        if (dict_.find(val) == dict_.end())
			return false;
        int num = data_.back();
        dict_[num] = dict_[val];
        data_[dict_[num]] = num;
        data_.pop_back();
        dict_.erase(val);
        return true;
    }
	
    int getRandom() {
        return data_[rand() % data_.size()];
    }
};
```

## [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

```
class Solution {
public:
    bool isIsomorphic(string s, string t) {
		std::unordered_map<int,int> hash_s,hash_t;
		for(int i = 0;i < s.length();i ++)
		{
			if(hash_s[s[i]] != hash_t[t[i]])
				return false;
			hash_s[s[i]] = hash_t[t[i]] = i + 1; //@ 防止混淆初始化时的 0
		}
		return true;
    }
};
```

## [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

```
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> dict;
        for(const auto num : nums)
            dict[num]++;
        
        using Data = std::pair<int,int>;
        vector<Data> vec(dict.begin(),dict.end());
        sort(vec.begin(),vec.end(),[](const Data& d1,const Data& d2){return d1.second > d2.second;});

        vector<int> res;
        for(int i = 0;i <k;i++)
            res.push_back(vec[i].first);
        return res;
    }
};
```
