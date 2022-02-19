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
- [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
- [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
- [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)
- [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)


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
ListNode dummy(-1);
dummy.next = head;
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
- [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)
- [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

- [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
- [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)
- [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
- [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)
- 







