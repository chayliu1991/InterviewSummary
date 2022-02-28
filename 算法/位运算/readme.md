# [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

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

# [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

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

# [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

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

# [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)

```
class Solution {
public:
    bool isPowerOfFour(int n) {
        return n > 0 && 0 == (n & (n - 1)) && n%3 == 1;
    }
};
```

# [318. 最大单词长度乘积](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

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

# [693. 交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/)

```
class Solution {
public:
    bool hasAlternatingBits(int n) {
        n = n ^ (n >> 1);
        return (n & (long(n) + 1)) == 0;
    }
};
```

# [476. 数字的补数](https://leetcode-cn.com/problems/number-complement/)

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

# [260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

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

# [15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

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

# [56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

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

# [56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

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

# [65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

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

