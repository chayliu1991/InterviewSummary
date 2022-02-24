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
        for (int i = 0; i < s.size(); i += (2 * k)) {
            //@ 1. 每隔 2k 个字符的前 k 个字符进行反转
            //@ 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k );
                continue;
            }
            //@ 3. 剩余字符少于 k 个，则将剩余字符全部反转。
            reverse(s.begin() + i, s.end());
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
        stringstream ss(s);
        string res, str;
        while(ss >> str)
            res = str + ' ' + res;
        return res.substr(0, res.size() - 1);
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
		stack<int> sign;
		sign.push(1);
		int res = 0,num = 0,oper = 1; //@ oper 只取1或者-1 ，分别表示加上或者减去
		for(auto c : s)
		{
			if(c == ' ')
				continue;
			if(isdigit(c))
			{
				num = num * 10 + (c - '0');
				continue;
			}
			
			res += oper * num;
			num = 0;
			switch(c)
			{
				case '+':
					oper = sign.top();
					break;
				case '-':
					oper = -sign.top();
					break;
				case '(':    //@ 将括号的整体看成是一个数，结果统一取 '+' 或者取 '-'
					sign.push(oper);
					break;
				case ')':
					sign.pop();
			}	
		}
		res += oper * num;
		return res;
    }
};
```

# [227.基本计算器II](https://leetcode-cn.com/problems/basic-calculator-ii)

```
class Solution {
public:
    int calculate(string s) {
		stack<int> stk;
		int res = 0,n = 0;
		char oper = '+';  //@ 在字符串前人为添加 '+' 不影响计算结果
		for(int i = 0;i < s.length();++i)
		{
			char curr = s[i];
			if(isdigit(curr))
				n = n * 10 + (curr - '0');
			if((!isdigit(curr) && curr != ' ') || i == s.length()-1)
			{
				int prev = 0;
				switch(oper)
				{
					case '+':
						stk.push(n);
						break;
					case '-':
						stk.push(-n);
						break;
					case '*':
						prev = stk.top();
						stk.pop();
						stk.push(prev * n);
						break;
					case '/':
						prev = stk.top();
						stk.pop();
						stk.push(prev / n);
					break;					
				}
				
				oper = curr;  //@ 更新符号
				n = 0; //@ 重置0
			}
		}	
		
		while(!stk.empty())
		{
			res += stk.top();
			stk.pop();
		}			
		return res;		
    }
};
```

# [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
		//@ left是窗口左边界的前一个位置，始化为 -1 
		int res = 0,left = -1,n = s.length();
		unordered_map<int,int> dict;
		
		for(int i = 0;i < n;i++)
		{
			if(dict.count(s[i]) && dict[s[i]] > left)
				left = dict[s[i]];
			dict[s[i]] = i;
			res = max(res,i-left);
		}
		return res;
    }
};
```

# [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

```
class Solution {
public:
    int longestValidParentheses(string s)
    {	
        int res = 0;
        stack<int> sk;
        sk.push(-1);  //@ 无效位置
        for (int i = 0; i < s.length(); i++)
        {
            if (s[i] == '(')
                sk.push(i);
            else
            {
                sk.pop();
                if (sk.empty())
                    sk.push(i); //@ 上一个无效的位置
                else
                    res = max(res, i - sk.top());
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
    string findLongestWord(string s, vector<string>& d) {
        string best;
        for(auto const & str : d)
        {
            int l1 = best.size(),l2 = str.size();
            if(l1 > l2 || (l1 == l2 && best.compare(str) < 0))
                continue;
            if(isSubstr(s,str))
                best = str;
        }
        return best;
    }

    bool isSubstr(string& s,string str)
    {
        int i = 0,j = 0;
        while(i < s.length() && j < str.length()){
            if(s[i] == str[j])
                j++;
            i++;
        }
        return j == str.length();
    }
};
```

# [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

KMP：

```
class Solution {
public:
    vector<int> getnext(string str)
        {
            int len=str.size();
            vector<int> next;
            next.push_back(-1);//next数组初值为-1
            int j=0,k=-1;
            while(j<len-1)
            {
                if(k==-1||str[j]==str[k])//str[j]后缀 str[k]前缀
                {
                    j++;
                    k++;
                    next.push_back(k);
                }
                else
                {
                    k=next[k];
                }
            }
            return next;
        }
    int strStr(string haystack, string needle) {
        if(needle.empty())
            return 0;
        
        int i=0;//源串
        int j=0;//子串
        int len1=haystack.size();
        int len2=needle.size();
        vector<int> next;
        next=getnext(needle);
        while((i<len1)&&(j<len2))
        {
            if((j==-1)||(haystack[i]==needle[j]))
            {
                i++;
                j++;
            }
            else
            {
                j=next[j];//获取下一次匹配的位置
            }
        }
        if(j==len2)
            return i-j;
        
        return -1;
    }
};
```









