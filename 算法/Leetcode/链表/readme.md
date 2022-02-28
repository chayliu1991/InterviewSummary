# [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

```
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode dummy(-1);
        dummy.next = head;
        ListNode*prev = &dummy,*curr = head;
        while(curr)
        {
            if(curr->val == val)
                prev->next = curr->next;
            else
                prev = curr;
            curr = curr->next;
        }
        return dummy.next;
    }
};
```

# [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(l1 == nullptr)
            return l2;
        if(l2 == nullptr)
            return nullptr;
        int carry = 0;
        ListNode* dummy = new ListNode(-1);
        ListNode* curr = dummy;
        while(l1 || l2 || carry)
        {
            int v1 = l1 == nullptr ? 0 : l1->val;
            int v2 = l2 == nullptr ? 0 : l2->val;
            l1 = l1 == nullptr ? nullptr : l1->next;
            l2 = l2 == nullptr ? nullptr : l2->next;

            int tmp = v1 + v2 + carry;
            carry = tmp /10;
            ListNode* node = new ListNode(tmp%10);
            curr->next = node;
            curr = curr->next;            
        }
        return dummy->next;
    }
};
```

# [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1, s2;
        while (l1) {
            s1.push(l1 -> val);
            l1 = l1 -> next;
        }
        while (l2) {
            s2.push(l2 -> val);
            l2 = l2 -> next;
        }
        int carry = 0;
        ListNode* res = nullptr;
        while (!s1.empty() or !s2.empty() or carry != 0) {
            int a = s1.empty() ? 0 : s1.top();
            int b = s2.empty() ? 0 : s2.top();
            if (!s1.empty()) s1.pop();
            if (!s2.empty()) s2.pop();
            int cur = a + b + carry;
            carry = cur / 10;
            auto curnode = new ListNode(cur % 10);
            curnode -> next = res;
            res = curnode;
        }
        return res;
    }
};
```

# [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

```
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        if(head == nullptr)
            return head;
        ListNode* h_big = new ListNode(-1),*h_small = new ListNode(-1);
        ListNode* curr = head,*curr_big = h_big,*curr_small = h_small;
        while(curr)
        {
            if(curr->val >= x)
            {
                curr_big->next = curr;
                curr_big = curr_big->next;
            }
            else
            {
                curr_small->next = curr;
                curr_small = curr_small->next;
            }
            curr = curr->next;
        }
        curr_big->next = nullptr;
        curr_small->next = h_big->next;
        return h_small->next;
    }
};
```

# [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

```
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;

        ListNode* odd = head;
        ListNode* even_head = head->next;
        ListNode* even = even_head;
        while(even != nullptr && even->next != nullptr)
        {
            odd->next = even->next;
            odd = odd->next;
            even->next = odd->next;
            even = even->next;
        }
        odd->next = even_head;
        return head;
    }
};
```

```
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == nullptr)
            return head;
        ListNode* h_even = new ListNode(-1),*h_odd = new ListNode(-1);
        ListNode* p = head,*curr_even = h_even,*curr_odd = h_odd;
        bool even = true;
        while(p)
        {
            if(even)
            {
                curr_even->next = p;
                curr_even = curr_even->next;
            }
            else
            {
                curr_odd->next = p;
                curr_odd = curr_odd->next;
            }
            even = !even;
            p = p->next;
        }
        curr_odd->next = nullptr;
        curr_even->next = h_odd->next;
        return h_even->next;
    }
};
```

# [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* new_head = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return new_head;
    }
};
```

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* prev = nullptr,*curr = head;
        while(curr)
        {
            ListNode* tmp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = tmp;
        }
        return prev;
    }
};
```

# [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == nullptr || head->next == nullptr || k ==0)
            return head;
        int len = 1;
        ListNode* curr = head;
        while(curr->next)
        {
            len++;
            curr = curr->next;
        }
        
        if(k%len == 0)
            return head;

        curr->next = head;
        int last = len - k%len;
        while(last--)
            curr = curr->next;
        
        ListNode* new_head = curr->next;
        curr->next = nullptr;
        return new_head;
    }
};
```

# [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
		ListNode* curr = dummy;
		while(l1 && l2)
		{
			auto &node = l1->val <= l2->val ? l1 : l2;
			curr->next = node;      
			curr = curr->next;
			node = node->next;			
		}
		curr->next = l1 == nullptr ? l2 : l1;
		return dummy->next;
    }
};
```

# [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode* new_head = head->next;
        head->next = swapPairs(new_head->next);
        new_head->next = head;
        return new_head;
    }
};
```

```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode(-1,head);
        ListNode* curr = dummy;
        while(curr->next && curr->next->next)
        {
            ListNode* node1 = curr->next;
            ListNode* node2 = curr->next->next;
            curr->next = node2;
            node1->next = node2->next;
            node2->next = node1;
            curr = node1;
        }
        return dummy->next;
    }
};
```

# [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *pA = headA,*pB = headB;
        while(pA != pB)
        {
            pA = (pA == nullptr ? headB : pA->next);
            pB = (pB == nullptr ? headA : pB->next);
        }
        return pA;
    }
};
```

# [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

```
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return true;
        
        std::vector<int> values;
        ListNode* curr = head;
        while(curr)
        {
            values.push_back(curr->val);
            curr = curr->next;
        }

        for(int i = 0,j = values.size()-1;i < j;i++,j--)
        {
            if(values[i] != values[j])
                return false;
        }
        return true;
    }
};
```

```
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return true;
        
        ListNode* left_end = find_end_half(head);
        ListNode* right_rev = reverse(left_end->next);
        ListNode* p1 = head,*p2 = right_rev;
        bool res = true;
        while(res && (p1 && p2))
        {
            if(p1->val != p2->val)
                res = false;
            
            p1 = p1->next;
            p2 = p2->next;
        }
        left_end->next = reverse(right_rev);
        return res;
    }

    ListNode* reverse(ListNode* head)
    {
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode*  new_head = reverse(head->next);
        head->next->next = head;
        head->next = nullptr;
        return new_head;
    }
    
    ListNode* find_end_half(ListNode* head)
    {
        ListNode *slow = head,*fast = head;
        while(fast->next && fast->next->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

# [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
		if(head == nullptr || head->next == nullptr)
			return head;
		ListNode* curr = head;
		while(curr && curr->next)
		{
			if(curr->val != curr->next->val)
				curr = curr->next;
			else{
				curr->next = curr->next->next;
			}
		}
		return head;
    }
};
```

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* dummy = new ListNode(-1,head);
        ListNode* curr = head,*prev = dummy;
        while(curr)
        {
            while(curr->next && curr->val == curr->next->val)
                curr = curr->next;
            if(prev->next != curr)
                prev->next = curr->next;
            else
                prev = prev->next;
            curr = curr->next;
        }
        return dummy->next;
    }
};
```

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        if(head->val != head->next->val)
            head->next = deleteDuplicates(head->next);
        else
        {
            ListNode* move = head->next;
            ListNode* prev = head;
            while(move && move->val == head->val){
                move = move->next;
                prev = prev->next;
            }
            return deleteDuplicates(prev);            
        }
        return head;
    }
};
```

# [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
            
        ListNode* curr = head;
        while(curr->next)
        {
            if(curr->val == curr->next->val)
                curr->next = curr->next->next;
            else
                curr = curr->next;
        }
        return head;
    }
};
```

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        if(head->val != head->next->val)
            head->next = deleteDuplicates(head->next);
        else
        {
            ListNode* move = head->next;
            while(move && head->val == move->val)
                move = move->next;
            return deleteDuplicates(move);
        }
        return head;
    }
};
```

# [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
		ListNode* fast = head;
		while(n--)
		{
			if(fast == nullptr)
				return nullptr;
			fast = fast->next;
		}
		if(fast == nullptr)
			return head->next;
		ListNode* slow = head;
		while(fast->next)
		{
			fast = fast->next;
			slow = slow->next;
		}
		slow->next = slow->next->next;
		return head;
    }
};
```

# [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* slow = head,*fast = head->next;
        while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        } 

        ListNode* mid = slow->next;
        slow->next = nullptr;
        return merge(sortList(head),sortList(mid));
    }

     ListNode* merge(ListNode* l1, ListNode* l2)
     {
         ListNode node(-1);
         ListNode* dummy = &node,*curr = dummy;
         while(l1 && l2)
         {
            if(l1->val > l2->val)
                std::swap(l1,l2);
            curr->next = l1;
            l1 = l1->next;
            curr = curr->next;
         }

         curr->next = l1 ? l1 : l2;
         return dummy->next;
     }
};
```

# [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return false;
        ListNode* slow = head,*fast = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;

            if(fast == slow)
                return true;
        }
        return false;
    }
};
```

# [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return nullptr;
        ListNode*fast = head,* slow = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow)
            {
                fast = head;
                while(fast != slow)
                {
                    fast = fast->next;
                    slow = slow->next;
                }
                return fast;
            }

        }
        return nullptr;
    }
};
```

# [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```
class LRUCache {
public:
    int cap_;
    std::list<std::pair<int,int>> data_;
    using Iter = std::list<std::pair<int,int>>::iterator;
    std::unordered_map<int,Iter> dict_;

    LRUCache(int capacity) : cap_(capacity){
    }
    
    int get(int key) {
        if(dict_.find(key) == dict_.end())
            return -1;
        int res = dict_[key]->second;
        data_.erase(dict_[key]);
        data_.emplace_front(key,res);
        dict_[key] = data_.begin();
        return res;
    }
    
    void put(int key, int value) {
        if(dict_.find(key) != dict_.end())
            data_.erase(dict_[key]);          
        
        data_.emplace_front(key,value);
        dict_[key] = data_.begin();

        while(data_.size() > cap_)
        {
            dict_.erase(data_.back().first);
            data_.pop_back();
        }
    }
};
```

# [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

```
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (head == nullptr || head->next == nullptr)
            return head;
        
        ListNode* dummy = new ListNode(-1,head);
        ListNode* last_sorted = head;
        ListNode* curr = head->next;
        
        while (curr != nullptr) {
            if (last_sorted->val <= curr->val)
                last_sorted = last_sorted->next; //@ 如果顺序排列，插入到最后一个位置
            else 
            {
                ListNode* prev = dummy;    //@ 否则将 curr 节点插入到 list 的前面部分                
                while (prev->next->val <= curr->val)
                    prev = prev->next;
                
                //@ 此时 prev 指向的 node 是小于 curr 的最大值 
                last_sorted->next = curr->next;  //@ 首先记录 curr->next 的值
                curr->next = prev->next;        //@ 将 curr 插入到 prev 和 prev->next 之间
                prev->next = curr;              
            }  
            curr = last_sorted->next;
            //@ 向后移动考察值
        }
        
        return dummy->next; 
    }
};
```

# [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```
class Solution {
public:
    ListNode* sortList(ListNode* head) 
    {
        if (nullptr == head || nullptr == head->next) 
            return head;
        auto slow = head, fast = head;
        while (fast->next && fast->next->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
             
        //@ 切链
        fast = slow->next;
        slow->next = nullptr;
        return merge(sortList(head), sortList(fast));
    }

private:
    ListNode* merge(ListNode* l1, ListNode* l2) 
    {
        ListNode *dummy = new ListNode(-1);
        ListNode *curr = dummy;
        while (l1 && l2) {
            auto &node = l1->val < l2->val ? l1 : l2;
            curr->next = node;
            curr = curr->next;
            node = node->next;
        }
        curr->next = l1 ? l1 : l2;
        return dummy->next;
    }
};
```







