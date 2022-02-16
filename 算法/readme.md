# 二分查找

口诀：

- 标准查找带等号，非标查找不带等
- 找左定右，找右定左，找右注意死循环
- 旋转要看旋转点，若有重复需去重

## 标准二分查找

```
int search(std::vector<int> nums,int target)
{
	int left = 0,right = nums.size() - 1;
	while(left <= right)  //@ 1,终止条件： left > right,即 left = right + 1
	{
		int mid = left + ((right - left) >> 1);
		if(nums[mid] == target)
			return mid;
		else if(nums[mid] > target)
			right = mid - 1;
		else
			left = mid + 1;
	}
	return -1;
}
```

## 寻找左边界

将区间 `[l, r]` 划分成 `[l, mid]` 和 `[mid + 1, r]` :

```
int search(std::vector<int> nums,int target)
{
	int left = 0,right = nums.size() - 1;
	while(left < right)  //@ 1,终止条件： left = right
	{
		int mid = left + ((right - left) >> 1);
		else if(nums[mid] >= target)
			right = mid;
		else
			left = mid + 1;
	}
	return nums[left] >= target ? left : -1;
}
```

## 寻找右边界

将区间 `[l, r]` 划分成 `[l, mid - 1]` 和 `[mid, r]` ：

```
int search(std::vector<int> nums,int target)
{
	int left = 0,right = nums.size() - 1;
	while(left < right)  //@ 1,终止条件： left = right
	{
		//@ 向上取整，在left与right相差为1时，left可以取到right而不会造成死循环
		int mid = left + ((right - left + 1) >> 1);
		else if(nums[mid] <= target)
			left = mid;
		else
			right = mid - 1;
	}
	return nums[right] <= target ? right : -1;
}
```

- [53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
- [53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
- [69. Sqrt(x)](https://leetcode-cn.com/problems/sqrtx/)
- [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
- [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
- [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)


# 动态规划

- [19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)
- [46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)
- [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)
- [48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
- [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)
- [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)
- [49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)
- [60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)
- [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)
- [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)
- [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)
- [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

