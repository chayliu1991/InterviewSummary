# [16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

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

# [43. 1～n 整数中 1 出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

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

# [44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

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



# [ 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

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

# [64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

```
class Solution {
public:
    int sumNums(int n) {
        n && (n+=sumNums(n-1));
        return n;
    }
};
```

