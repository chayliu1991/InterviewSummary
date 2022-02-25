# 二分查找

口诀：

- 标准查找带等号，非标查找不带等
- 找左定右，找右定左，找右注意死循环
- 旋转要看旋转点，若有重复需去重

标准二分查找：

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

寻找左边界：

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

寻找右边界：

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
- [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
- [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
- [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)
- [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)
- [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
- [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
- [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)


# 贪心

- [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)
- [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)
- [134. 加油站](https://leetcode-cn.com/problems/gas-station/)
- [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)
- [135. 分发糖果](https://leetcode-cn.com/problems/candy/)


# 二叉树

- [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
- [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)
- [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
- [897. 递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)
- [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
- [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)
- [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)
- [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
- [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)
- [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)
- [26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
- [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)
- [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)
- [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)
- [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
- [889. 根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)
- [37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

# 链表

遍历：

```
//@ 遍历链表
ListNode* p = head;
while(p!=NULL)
    p = p->next;

//@ 找到最后一个节点
if(!head) return head;
ListNode* p = head;
while(p->next!=NULL){
    p = p->next;
}

//@ 记录前一个
ListNode* p_before = NULL;
ListNode* p = head;
while(p!=NULL){
    p_before = p;
    p = p->next;
}

//@ 在 head 之前加上一个节点来简化计算
 ListNode* dummy = new ListNode(-1,head);
```

前后指针:

```
//@ l1在前，l2在后，让l2先走n步，当退出第二个循环时，l2位空，l1为倒数第n个节点
ListNode* l1 = head;
ListNode* l2 = head;
while(n-->0){
    l2 = l2->next;
}
while(l2 !=NULL){
    l1 = l1->next;
    l2 = l2->next;
}

//@ 利用快慢指针可以找到链表的中间节点，l1每次向前走一格，l2每次向前走两个
ListNode* l1 = head;
ListNode* l2 = head;
while (l2 != NULL && l2->next!= NULL) {
    l1 = l1->next;
    l2 = l2->next->next;
} 
```

- [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

- [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

- [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

- [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

- [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

- [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

- [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

- [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

- [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

- [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

- [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

- [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

- [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

- [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

- [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

- [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

- [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

- [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

- [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

# 排序

时间复杂度和稳定性：

| 排序算法 | 平均时间复杂度  | 稳定性  |   备注   |
| :--: | :------: | :--: | :----: |
| 插入排序 |  O(n^2)  |  稳定  | n 小时较好 |
| 希尔排序 | O(n^1.3) | 不稳定  | n 大时较好 |
| 选择排序 |  O(n^2)  | 不稳定  | n 小时较好 |
| 堆排序  | O(nlogn) | 不稳定  | n 大时较好 |
| 冒泡排序 |  O(n^2)  |  稳定  | n 小时较好 |
| 快速排序 | O(nlogn) | 不稳定  | n 大时较好 |
| 归并排序 | O(nlogn) |  稳定  | n 大时较好 |

直接插入排序和希尔排序的比较：

- 直接插入排序是稳定的；而希尔排序是不稳定的
- 直接插入排序更适合于原始记录基本有序的集合。希尔排序的比较次数和移动次数都要比直接插入排序少，当N越大时，效果越明显
- 直接插入排序也适用于链式存储结构；希尔排序不适用于链式结构




- [41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)


# 搜索与回溯

BFS 需要借助队列完成。
- [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces/)
- [46. 全排列](https://leetcode-cn.com/problems/permutations/)
- [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)
- [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)
- [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
- [12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
- [38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)
- [46. 全排列](https://leetcode-cn.com/problems/permutations/)
- [78. 子集](https://leetcode-cn.com/problems/subsets/)
- [77. 组合](https://leetcode-cn.com/problems/combinations/)


# 双指针

- [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)
- [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)
- [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)
- [680. 验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)


# 哈希

- [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)
- [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)


# 数组

- [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

- [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

- [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

- [80. 删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

- [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

- [435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

- [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

- [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

- [57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

- [ 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

- [44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

- #### [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

# 位运算

- [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)
- [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)


# 字符串

- [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)
- [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)
- [224.基本计算器](https://leetcode-cn.com/problems/basic-calculator)
- [227.基本计算器II](https://leetcode-cn.com/problems/basic-calculator-ii)
- [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
- [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)
- [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)
- [20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)
- [67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

# 栈和队列

- [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
- [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
- [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)
- [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
- [59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

