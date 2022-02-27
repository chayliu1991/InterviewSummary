# [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preorder(res,root);
        return res;
    }

    void preorder(vector<int>& vec,TreeNode* root)
    {
        if(root == nullptr)
            return;
        
        vec.emplace_back(root->val);
        preorder(vec,root->left);
        preorder(vec,root->right);
    }
};
```
```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
		s.push(root);
		while(!s.empty())
		{
			auto curr = s.top();
			s.pop();
			res.push_back(curr->val);
			if(curr->left)
				s.push(curr->left);
			if(curr->right)
				s.push(curr->right);		
		}
		
		std::reverse(res.begin(),res.end());
        return res;
    }
};
```
```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
        s.push(root);       
        while(!s.empty())
        {
            TreeNode* curr = s.top();
            s.pop();
            res.emplace_back(curr->val);
            if(curr->right)
                s.push(curr->right);
            if(curr->left)
                 s.push(curr->left);
        }
        return res;
    }
};
```

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
        s.push(root);       
        while(!s.empty())
        {
            TreeNode* curr = s.top();
			if(curr)
			{
				s.pop();
				if(curr->right)
					s.push(curr->right);
				if(curr->left)
					s.push(curr->left);
				s.push(curr);
				s.push(nullptr);
			}
			else
			{
				s.pop();
				curr = s.top();
				s.pop();
				res.emplace_back(curr->val);
			}
        }
        return res;
    }
};
```

# [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        std::vector<int> res;
        inorder(res,root);
        return res;
    }

    void inorder(std::vector<int>& vec,TreeNode* root)
    {
        if(root == nullptr)
            return;
        inorder(vec,root->left);
        vec.emplace_back(root->val);
        inorder(vec,root->right);
    }
};
```

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        
        std::vector<int> res;
        std::stack<TreeNode*> s;
        TreeNode* curr = root;
        while(curr != nullptr || !s.empty())
        {
            if(curr)
            {
                s.push(curr);
                curr = curr->left;
            }
            else
            {
                curr = s.top();
                s.pop();
                res.emplace_back(curr->val);
                curr = curr->right;
            }
        }
        return res;
    }
};
```

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
		if(root == nullptr)
			return {};
		
		std::vector<int> res;
		std::stack<TreeNode*> s;
		s.push(root);       
		while(!s.empty())
		{
			TreeNode* curr = s.top();
			if(curr)
			{
				s.pop();
				if(curr->right)
					s.push(curr->right);
				s.push(curr);
				s.push(nullptr);
				
				if(curr->left)
					s.push(curr->left);
			}
			else
			{
				s.pop();
				curr = s.top();
				s.pop();
				res.emplace_back(curr->val);
			}
		}
		return res;
    }
};
```

# [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(root == nullptr)
            return {};
        std::vector<int> res;
        postorder(res,root);
        return res;
    }

    void postorder(std::vector<int>& vec,TreeNode* root)
    {
        if(root == nullptr)
            return;
        postorder(vec,root->left);
        postorder(vec,root->right);
        vec.emplace_back(root->val);
    }
};
```

```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
		if(root == nullptr)
			return {};
		
		std::vector<int> res;
		std::stack<TreeNode*> s;
		s.push(root);       
		while(!s.empty())
		{
			TreeNode* curr = s.top();
			if(curr)
			{
				s.push(nullptr);				
				if(curr->right)
					s.push(curr->right);				
				if(curr->left)
					s.push(curr->left);
			}
			else
			{
				s.pop();
				curr = s.top();
				s.pop();
				res.emplace_back(curr->val);
			}
		}
		return res;
    }
};
```

# [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root == nullptr)
            return {};
        std::vector<vector<int>> res;
        std::queue<TreeNode*> q;
        q.push(root);
        std::vector<int> level;
        while(!q.empty())
        {
           size_t n = q.size();
           for(size_t i = 0;i < n;++i)
           {
                auto curr = q.front();
                q.pop();
                level.emplace_back(curr->val);
                if(curr->left)
                    q.push(curr->left);
                if(curr->right)
                    q.push(curr->right); 
           }
           res.emplace_back(std::move(level));             
        }
        return res;
    }
};
```

# [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(root == nullptr)
            return {};
        std::vector<std::vector<int>> res;
		std::queue<TreeNode*> q;
		q.push(root);
		bool l2r = true;
		while(!q.empty())
		{
			std::deque<int> dq;
			int n = q.size();
			while(n--)
			{
				auto node = q.front();
				q.pop();
				if(l2r)
					dq.emplace_back(node->val);
				else
					dq.emplace_front(node->val);
				if(node->left)
					q.push(node->left);
				if(node->right)
					q.push(node->right);
			}
			l2r = !l2r;
			res.emplace_back(dq.begin(),dq.end());
		}		
        return res;
    }
};
```

# [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

```
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
		if(root == nullptr)
			return -1;
		std::queue<TreeNode*> q;
		q.push(root);
		std::vector<std::vector<int>> layers;
		while(!q.empty())
		{
			int n = q.size();
			std::vector<int> layer;
			while(n--)
			{
				auto node = q.front();
				q.pop();
				layer.emplace_back(node->val);
				if(node->left)
					q.push(node->left);
				if(node->right)
					q.push(node->right);
			}
			layers.emplace_back(std::move(layer));
		}		
		return layers.back().front();
		}
};
```

# [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

```
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
		int sum = 0;
		inorder(root,sum);
		return root;
    }
	
	void inorder(TreeNode*node,int& sum)  //@ 必须是引用
	{
		if(node == nullptr)
			return;
		inorder(node->right,sum);
		sum += node->val;
		node->val = sum;
		inorder(node->left,sum);
	}
};
```

```
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        if(root == nullptr)
            return nullptr;
		
		std::stack<TreeNode*> s;
		s.push(root);
		int sum = 0;
		while(!s.empty())
		{
			auto p = s.top();
			if(p)
			{
				s.pop();
				if(p->left)
					s.push(p->left);		
				s.push(p);
				s.push(nullptr);				
				if(p->right)
					s.push(p->right);	
			}
			else
			{
				s.pop();
				auto node = s.top();
				s.pop();
				sum += node->val;
				node->val = sum;			
			}
		}
        return root;
    }
};
```

# [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

```
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
		vector<int>  res;
		if(root == nullptr)
			return res;
		queue<TreeNode*> q;
		q.push(root);
		while(!q.empty())
		{
			int size = q.size();
			for(int i = 0;i < size;++i)
			{
				auto t = q.front();
				q.pop();
				if(t->left)
					q.push(t->left);
				if(t->right)
					q.push(t->right);
				if(i == size -1)
					res.push_back(t->val);
			}
		}
		return res;
    }
};
```

# [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
		std::vector<int> vec;
		inorder(root,vec);
		for(int i = 1;i < vec.size();i++)
		{
			if(vec[i] <= vec[i-1])
				return false;
		}
		return true;
    }
	
	void inorder(TreeNode* root,std::vector<int>& res)
	{
		if(root == nullptr)
			return;
		inorder(root->left,res);
		res.emplace_back(root->val);
		inorder(root->right,res);
	}
};
```

# [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

递归解法：

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr)
            return 0;
        return max(maxDepth(root->left),maxDepth(root->right)) + 1;
    }
};
```

非递归解法：

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr)
            return 0;
        int res = 0;
        queue<TreeNode* > q;
        q.push(root);
        while(!q.empty())
        {
            res ++;
            int size = q.size();
            for(int i=0;i<size;i++)
            {
                TreeNode* node = q.front();
                q.pop();
                if(node->left)
                    q.push(node->left);
                if(node->right)
                    q.push(node->right);
            }
        }
        return res;
    }
};
```

# [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(root == nullptr)
            return true;
        int left = _depth(root->left);
        int right = _depth(root->right);
        if(abs(left-right) > 1)
            return false;
        return isBalanced(root->left) && isBalanced(root->right);
    }

    int _depth(TreeNode* root)
    {
        if(root == nullptr)
            return 0;  
        return  max(_depth(root->left),_depth(root->right)) + 1;
    }
};
```

# [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

```
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root == nullptr)
            return 0;
        int res = 0;
        depth(root,res);
        return res;
    }

    int depth(TreeNode* root,int& diameter)
    {
        if(root == nullptr)
            return 0;
        int L = depth(root->left,diameter);
        int R = depth(root->right,diameter);
        diameter = std::max(diameter,L+R);
        return std::max(L,R) + 1;
    }
};
```

# [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

中序遍历：

```
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        if(root == nullptr)
            return -1;
		vector<int> res;
		inorder(root,res);
        return res.size() >=k ?  res[k-1] : -1;		 
    }
	
	void inorder(TreeNode* root,vector<int>& res)
	{
		if(root == nullptr)
			return;
		inorder(root->left,res);
		res.push_back(root->val);
		inorder(root->right,res);
	}
};
```

二分法：

```
class Solution {
public:
    int numofNodes(struct TreeNode* root)
    {
        if(root == nullptr) 
            return 0;
        return 1 + numofNodes(root->left) + numofNodes(root->right);
    }

    int kthSmallest(struct TreeNode* root, int k){
        int cnt = numofNodes(root->left);
        if(k <= cnt) 
            return kthSmallest(root->left,k);
        else if(k > cnt+1) 
            return kthSmallest(root->right,k-cnt-1);
        return root->val; // k == cnt+ 1
    }
};
```

# [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

递归：

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
		if(root == nullptr)
			return root;
		if(root->val ==val)
			return root;
		if(root->val > val)
			return searchBST(root->left,val);
		if(root->val < val)
			return searchBST(root->right,val);
		
		return nullptr;
    }
};
```

迭代：

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root && root->val !=  val)
            root->val > val ? root = root->left : root = root->right;
        return root;
    }
};
```

栈迭代：

```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        stack<TreeNode*> stk;
        if(root)
            stk.push(root);
        while(!stk.empty())
        {
            auto t = stk.top();
            stk.pop();
            if(t->val == val)
                return t;
            else if(t->val > val && t->left)
                stk.push(t->left);
            else if(t->val < val && t->right)  
                stk.push(t->right);
        }
        return nullptr;
    }
};
```

# [897. 递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)

```
class Solution {
public:
	void inorder(TreeNode* root,std::vector<TreeNode*>& nodes)
	{
		if(root == nullptr)
			return;
		
		inorder(root->left,nodes);
		nodes.push_back(root);
		inorder(root->right,nodes);
	}
	
    TreeNode* increasingBST(TreeNode* root) {
        if(root == nullptr)
			return nullptr;
		std::vector<TreeNode*> nodes;
		inorder(root,nodes);	
		for(int i = 1;i < nodes.size();++i)
		{
			nodes[i-1]->left = nullptr;
			nodes[i-1]->right = nodes[i];
		}
		nodes.back()->left = nodes.back()->right = nullptr;
        return nodes.front();
    }
};
```

# [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr || p == nullptr || q == nullptr)
			return nullptr;
		if(root->val > p->val && root->val > q->val)
			return lowestCommonAncestor(root->left,p,q);
		else if(root->val < p->val && root->val < q->val)
			return lowestCommonAncestor(root->right,p,q);
		return root;
    }
};
```

# [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

递归：

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if(root == nullptr)
            return false;
        if(root->left == nullptr && root->right == nullptr)
            return sum == root->val;
		sum -= root->val;
        return hasPathSum(root->left,sum) || hasPathSum(root->right,sum);
    }
};
```

迭代：

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        stack<pair<TreeNode*, int>> stk;
        if(root) stk.push(make_pair(root, root->val));
        while(stk.size()) {
            pair<TreeNode*, int> cur = stk.top(); stk.pop();
            if(!cur.first->left && !cur.first->right && cur.second == sum) return true;
            if(cur.first->left) stk.push(make_pair(cur.first->left, cur.second + cur.first->left->val));
            if(cur.first->right) stk.push(make_pair(cur.first->right, cur.second + cur.first->right->val));
        }
        return false;
    }
};
```

dfs：

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr)
            return false;
        return dfs(root,targetSum,0);
    }

    bool dfs(TreeNode* root, int targetSum,int curr)
    {
        if(root == nullptr)
            return false;
        curr += root->val;
        if(root->left == nullptr && root->right == nullptr && targetSum == curr)
            return true;        
        return dfs(root->left,targetSum,curr) || dfs(root->right,targetSum,curr);
    }
};
```

# [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

```
class Solution {
public:
    void dfs(TreeNode *root,vector<int> &path,vector<vector<int>>& res,int sum)  //@ sum 不能是引用
    {
        if(root == nullptr)
            return;
        path.push_back(root->val);
        if(root->val == sum && (root->left == nullptr && root->right == nullptr))
            res.push_back(path);		
		sum -= root->val;
        dfs(root->left,path,res,sum);
        dfs(root->right,path,res,sum);
        path.pop_back();
    }

    vector<vector<int>> pathSum(TreeNode* root, int sum) 
    {
        vector<int> path;
		vector<vector<int>> res;
        dfs(root,path,res,sum);
        return res;        
    }
};
```

# [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

```
class Solution {
public:
    int count(TreeNode* root, int sum)
    {
        if(root == nullptr)
            return 0;
        return (root->val == sum) + count(root->left,sum - root->val) + count(root->right,sum - root->val);
    }

    int pathSum(TreeNode* root, int sum) {
        if(root == nullptr)
            return 0;
        return count(root,sum) + pathSum(root->left,sum) + pathSum(root->right,sum);
    }
};
```

# [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

递归：

```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr)
            return nullptr;        
        TreeNode* left = invertTree(root->left);
        TreeNode* right = invertTree(root->right);
        root->left = right;
        root->right = left;
        return root;
    }
};
```

非递归：

```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr)
            return nullptr;
        
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            TreeNode* curr = q.front();
            q.pop();
            TreeNode* left = curr->left;
            curr->left = curr->right;
            curr->right = left;
            if(curr->left)
                q.push(curr->left);
            if(curr->right)
                q.push(curr->right);    
        }
        return root;
    }
};
```

# [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

递归：

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
		if(root1 == nullptr)
			return root2;
		if(root2 == nullptr)
			return root1;
		root1->val += root2->val;
		root1->left = mergeTrees(root1->left,root2->left);
		root1->right = mergeTrees(root1->right,root2->right);
		return root1;
    }
};
```

栈迭代：

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1 == nullptr)
            return t2;
        stack<pair<TreeNode*,TreeNode*>> stk;
        if(t1 && t2)
            stk.push(make_pair(t1,t2));
        
        while(!stk.empty())
        {
            auto cur = stk.top();
            stk.pop();
            cur.first->val += cur.second->val;
            if(cur.second->left)
            {
                if(cur.first->left == nullptr)
                    cur.first->left = new TreeNode(0);  //如果左子树不存在，新建值为0的节点
                stk.push(make_pair(cur.first->left,cur.second->left));
            }
            if(cur.second->right)
            {
                if(cur.first->right == nullptr)
                    cur.first->right = new TreeNode(0);  //如果右子树不存在，新建值为0的节点
                stk.push(make_pair(cur.first->right,cur.second->right));
            }            
        }
        return t1;
    }
};
```

队列迭代：

```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1 == nullptr)
            return t2;
        if(t2 == nullptr)
            return t1;
        queue<pair<TreeNode*,TreeNode*>> q;
        q.push(make_pair(t1,t2));
        while(!q.empty())
        {
            auto cur = q.front();
            q.pop();
            cur.first->val  += cur.second->val;      
            if(cur.second->left)
            {
                if(cur.first->left == nullptr)
                    cur.first->left = new TreeNode(0);  //如果左子树不存在，新建值为0的节点
                q.push(make_pair(cur.first->left,cur.second->left));
            }
            if(cur.second->right)
            {
                if(cur.first->right == nullptr)
                    cur.first->right = new TreeNode(0);  //如果右子树不存在，新建值为0的节点
                q.push(make_pair(cur.first->right,cur.second->right));
            }   
        }
        return  t1;
    }
};
```

# [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

```
class Solution {
public:
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
		if(root == nullptr)
			return false;
		return same(root,subRoot) || isSubtree(root->left,subRoot) || isSubtree(root->right,subRoot);
    }
	
	bool same(TreeNode* tree1, TreeNode* tree2)
	{
		if(tree1 == nullptr && tree2 == nullptr)
			return true;
		if(tree1 == nullptr || tree2 == nullptr)
			return false;
		if(tree1->val != tree2->val)
			return false;
		return same(tree1->left,tree2->left) && same(tree1->right,tree2->right);
	}	
};
```

# [671. 二叉树中第二小的节点](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

```
class Solution {
public:
    set<int> s;
    void dfs(TreeNode* root)
    {
        if(root == nullptr)
            return;
        s.insert(root->val);
        dfs(root->left);
        dfs(root->right);
    }

    int findSecondMinimumValue(TreeNode* root) {
        if(root == nullptr)
            return -1;
        
        dfs(root);
        auto it = s.begin();
        if(++it == s.end())
            return -1;
        else
            return *it;
    }
};
```

# [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr || root == p || root == q)
            return root;
        
        TreeNode* left = lowestCommonAncestor(root->left,p,q);
        TreeNode* right = lowestCommonAncestor(root->right,p,q);
        if(left && right)
            return root;
        return left ? left : right;
    }
};
```

# [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

递归：

```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr)
            return true;
        return symmetric(root->left,root->right);
    }

    bool symmetric(TreeNode* node1,TreeNode* node2)
    {
        if(node1 == nullptr && node2 == nullptr)
            return true;
        if(node1 == nullptr || node2 == nullptr)
            return false;
        if(node1->val != node2->val)
            return false;
        return symmetric(node1->left,node2->right) && symmetric(node1->right,node2->left);
    }
};
```

非递归：

```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr)
            return true;
        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);
        while(!q.empty())
        {
            TreeNode* L = q.front();
            q.pop();
            TreeNode* R = q.front();
            q.pop();
            if(L == nullptr && R == nullptr)
                continue;
            if(L == nullptr || R == nullptr || (L->val != R->val))
                return false;
            q.push(L->left);
            q.push(R->right);
            q.push(L->right);
            q.push(R->left);
        }
        return true;
    }
};
```

# [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

```
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        std::vector<int> vec;
        auto* curr = head;
        while(curr)
        {
            vec.push_back(curr->val);
            curr = curr->next;
        }
        return to_bst(vec);
    }
    
    TreeNode* to_bst(const std::vector<int>& vec)
    {
        if(vec.empty())
            return nullptr;
        int n = vec.size();
        if(n == 1)
            return new TreeNode(vec.front());
        TreeNode* root = new TreeNode(vec[n/2]);
        std::vector<int> lv(vec.begin(),vec.begin()+n/2);
        std::vector<int> rv(vec.begin()+n/2+1,vec.end());
        root->left = to_bst(lv);
        root->right = to_bst(rv);
        return root;
    }
};
```

快慢指针：

```
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if(head == nullptr)
            return nullptr;
        if(head->next == nullptr)
            return new TreeNode(head->val);
        
        //@ 快慢指针,pre指向slow节点的前一个节点
        ListNode *pre = head,*slow = head,*fast = head;
        while(fast && fast->next)
        {
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }

        pre->next = nullptr; //@ 链表切断,一分为二
        TreeNode* root= new TreeNode(slow->val);  //@ slow 作为中间节点可以作为根
        root->left = sortedListToBST(head); //@ 递归左子树
        root->right = sortedListToBST(slow->next); //@ 递归右子树
        return root;
    }
};
```

# [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)

```
class Solution {
public:
    bool isCompleteTree(TreeNode* root) {
		if(root == nullptr)
			return true;
		std::queue<TreeNode*> q;
		q.push(root);
		bool reach_null{false};
		while(!q.empty())
		{
			auto curr = q.front();
			q.pop();
			if(curr == nullptr)
			{
				reach_null = true;
				continue;
			}
			
			if(reach_null)
				return false;
			q.push(curr->left);
			q.push(curr->right);
		}
		return true;
    }
};
```

# [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() != inorder.size() || preorder.empty())
            return nullptr;
        int value = preorder[0];
        TreeNode* root = new TreeNode(value);
        auto pos = std::find(inorder.begin(),inorder.end(),value);
        std::vector<int> linor(inorder.begin(),pos);
        std::vector<int> rinor(pos+1,inorder.end());
        int left = linor.size();
        std::vector<int> lpre(preorder.begin()+1,preorder.begin()+1+left);
        std::vector<int> rpre(preorder.begin()+1+left,preorder.end());
        root->left = buildTree(lpre,linor);
        root->right = buildTree(rpre,rinor);
        return root;
    }
};
```

```
class Solution {
public:
    std::unordered_map<int,int> hash;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for(int i = 0;i < inorder.size();i++)
            hash[inorder[i]] = i;
        return build(preorder,inorder,0,preorder.size()-1,0,inorder.size()-1);
    }

    TreeNode* build(vector<int>& pre, vector<int>& inor,int pre_start,int pre_end,int inor_start,int inor_end)
    {
        if(pre_start > pre_end)
            return nullptr;
        TreeNode* root = new TreeNode(pre[pre_start]);
        if(pre_start == pre_end)
            return root;
		int pos = hash[pre[pre_start]];		        
        int len = pos - inor_start;
        root->left = build(pre,inor,pre_start+1,pre_start+len,inor_start,pos-1);
        root->right = build(pre,inor,pre_start+1+len,pre_end,pos+1,inor_end);
		return root;
    }
};
```

# [889. 根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

```
class Solution {
public:
    std::unordered_map<int,int> hash;

    TreeNode* constructFromPrePost(vector<int>& preorder, vector<int>& postorder) {
         for(int i = 0;i < postorder.size();i++)
            hash[postorder[i]] = i;
        return build(preorder,postorder,0,preorder.size()-1,0,postorder.size()-1);
    }

    TreeNode* build(const vector<int>& preorder, const vector<int>& postorder,int pre_start,int pre_end,int post_start,int post_end)
    {
        if(pre_start > pre_end)
            return nullptr;
        TreeNode* root = new TreeNode(preorder[pre_start]);
        if(pre_start == pre_end)
            return root;
        int pos = hash[preorder[pre_start+1]];
        int len = pos - post_start + 1;
        root->left = build(preorder,postorder,pre_start+1,pre_start+len,post_start,post_start+len-1);
        root->right = build(preorder,postorder,pre_start+1+len,pre_end,post_start+len,post_end-1);
        return root;
    }
};
```

# [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

````
class Solution {
public:
	std::unordered_map<int,int> hash;
	
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
		for(int i = 0;i < inorder.size();i++)
			hash[inorder[i]] = i;
		return build(inorder,postorder,0,inorder.size()-1,0,postorder.size()-1);
    }
	
	TreeNode* build(vector<int>& inor, vector<int>& post,int inor_start,int inor_end,int post_start,int post_end)
	{
		if(inor_start > inor_end)
			return nullptr;
		TreeNode* root = new TreeNode(post[post_end]);
        if(inor_start == inor_end)
            return root;
		int pos = hash[post[post_end]];
		int len = pos - inor_start -1;
		root->left = build(inor,post,inor_start,pos-1,post_start,post_start+len);
		root->right = build(inor,post,pos+1,inor_end,post_start+len+1,post_end-1);
		return root;
	}
};
````

# [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

递归：

```
class Solution {
public:
	vector<int> res;
    vector<int> postorder(Node* root) {
        if(root == nullptr)
			return res;
		for(auto i : root -> children)
            postorder(i);
        res.push_back(root -> val);
        return res;
    }
};
```

迭代：

```
class Solution {
public:
    vector<int> postorder(Node* root) {
        if(root == nullptr)
			return {};
		vector<int> res;
		stack<Node*> stk;
		stk.push(root);
		while(!stk.empty())
		{
			Node* t = stk.top();
			stk.pop();
			res.push_back(t->val);
			for(auto i : t->children)
				stk.push(i);			
		}
		reverse(res.begin(),res.end());
		return res;
    }
};
```

# [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

递归：

```
class Solution {
public:
	vector<int> res;
    vector<int> preorder(Node* root) {
        if(root == nullptr)
			return res;
		res.push_back(root->val);
		for(auto i : root->children)
			preorder(i);
		return res;
    }
};
```

迭代：

```
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> res;
		if(root == nullptr)
			return res;
		stack<Node*> stk;
		stk.push(root);
		while(!stk.empty())
		{
			auto t = stk.top();
			stk.pop();
            res.push_back(t->val);
			for(int i = t->children.size()-1;i>=0;--i)
				stk.push(t->children[i]);
		}
		return res;
    }
};
```

# [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

```
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
		if(root == nullptr)
			return res;
		queue<Node*> q;
		q.push(root);
		while(!q.empty())
		{
			vector<int> level;
			int size = q.size();
			for(int i=0;i<size;i++)
			{
				auto t = q.front();
				q.pop();
				level.push_back(t->val);
				for(const auto i : t->children)
					q.push(i);
			}
			res.push_back(move(level));
		}		
		return res;
    }
};
```

# [559. N叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

递归：

```
class Solution {
public:
    
    int maxDepth(Node* root) {
        if(root == nullptr)   
			return 0;
        int depth = 0;
        for(const auto i : root -> children)
            depth = max(maxDepth(i), depth);            
        return depth + 1;
    }
};
```

DFS：

```
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr)
			return 0;
		stack<pair<Node*,int>> stk;
		stk.push(make_pair(root,1));
		int depth = 1;
		while(!stk.empty())
		{
			auto t = stk.top();
			stk.pop();
			int dep = t.second;
			for(const auto i : t.first->children)
				stk.push(make_pair(i,dep+1));
			depth = max(depth,dep);		
		}
		return depth;
    }
};
```

BFS：

```
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr)
			return 0;
		
		queue<Node*> q;
		q.push(root);
		int depth = 0;
		while(!q.empty())
		{
			depth++;
			int size = q.size();
			for(int i = 0;i<size;i++)
			{
				auto t = q.front();
				q.pop();
				for(const auto i : t->children)
					q.push(i);
			}			
		}
		return depth;
    }
};
```

