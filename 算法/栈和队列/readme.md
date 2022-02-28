# [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

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

# [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

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

# [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

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

# [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

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

# [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

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

# [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

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

# [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

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

# [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

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

# [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

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

# [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

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



