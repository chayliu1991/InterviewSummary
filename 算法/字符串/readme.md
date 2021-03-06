# [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

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

# [541. 反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

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

# [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

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

# [224.基本计算器](https://leetcode-cn.com/problems/basic-calculator)

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

# [227.基本计算器II](https://leetcode-cn.com/problems/basic-calculator-ii)

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

# [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

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

# [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

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

# [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

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

# [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

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

# [ 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

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

# [20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

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


# [50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

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

# [58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

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

# [58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

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

# [67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

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









