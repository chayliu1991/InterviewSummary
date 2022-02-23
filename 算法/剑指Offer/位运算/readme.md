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

