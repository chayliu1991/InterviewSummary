# [40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

```
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(arr.empty())
            return {};
        
        std::sort(arr.begin(),arr.end());
        if(arr.size() < k)
            return {};
        return std::vector<int>(arr.begin(),arr.begin()+k);
    }
};
```

# [41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

```
class MedianFinder {
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        decrease_max.push(num);
        increase_min.push(decrease_max.top());
        decrease_max.pop();

        while(decrease_max.size() < increase_min.size())
        {
            decrease_max.push(increase_min.top());
            increase_min.pop();
        }
    }
    
    double findMedian() {
        return decrease_max.size() > increase_min.size() ? (double) decrease_max.top() : (decrease_max.top() + increase_min.top()) * 0.5;
    }

private:
    std::priority_queue<int,std::vector<int>,std::greater<int>> increase_min;
    std::priority_queue<int,std::vector<int>,std::less<int>> decrease_max;
};
```

# [45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

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

# [ 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

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