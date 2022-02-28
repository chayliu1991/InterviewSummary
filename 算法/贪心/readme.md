# [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

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

# [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/)

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

# [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

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

# [134. 加油站](https://leetcode-cn.com/problems/gas-station/)

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

# [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

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


# [316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)

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



# [135. 分发糖果](https://leetcode-cn.com/problems/candy/)

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

# [14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

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

# [14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

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
