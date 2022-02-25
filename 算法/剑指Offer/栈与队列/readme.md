# [09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

**思路**

- 一个入栈，一个出栈

```
class CQueue {
public:
    CQueue() {

    }
    
    void appendTail(int value) {
        s_in.push(value);
    }
    
    int deleteHead() {
        if(s_in.empty() && s_out.empty())
            return -1;
        
        if(s_out.empty())
        {
            while(!s_in.empty())
			{
				s_out.push(s_in.top());
				s_in.pop();
			}
        }
		
		auto res = s_out.top();
		s_out.pop();
		return res;
    }

    stack<int> s_in,s_out;
};
```

# [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

```
class Solution {
public:
    bool isValid(string s) {
        std::unordered_map<char,char> pairs =
        {
            {')','('},
            {']','['},
            {'}','{'}
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

# [30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

思路：

- 一个数据栈，一个最小栈

```
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {

    }
    
    void push(int x) {
        if(s_data.empty())
        {
            s_min.push(x);
        }
        else
        {
            if(s_min.top() <= x)
                s_min.push(s_min.top());
            else
                s_min.push(x);
        }

        s_data.push(x);
    }
    
    void pop() {
        s_min.pop();
        s_data.pop();
    }
    
    int top() {
        return s_data.top();
    }
    
    int min() {
        return s_min.top();
    }

    stack<int> s_min,s_data;
};

```

# [59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

```
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        if(nums.empty())
            return {};
        
        std::deque<int> dq;
        for(int i = 0;i < k;i++)
        {
            while(!dq.empty() && nums[i] >= nums[dq.back()])
                dq.pop_back();
            dq.push_back(i);
        }

        std::vector<int> res{nums[dq.front()]};
        for(int i = k;i < nums.size();i++)
        {
            while(!dq.empty() && nums[i] >= nums[dq.back()])
                dq.pop_back();
            dq.push_back(i);

            while(i - dq.front() >= k)
                dq.pop_front();

            res.push_back(nums[dq.front()]);
        }
        return res;
    }
};
```

# [59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

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

