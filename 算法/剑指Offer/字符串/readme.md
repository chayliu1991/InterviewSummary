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

