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
        ListNode dummy(-1);
        ListNode* curr = &dummy;
        int carry = 0;
        
         for(ListNode *p1 =l1,*p2 = l2;p1 || p2;)
        {
            int v1 = (p1 == nullptr ? 0 : p1->val);
            int v2 = (p2 == nullptr ? 0 : p2->val);
            int sum = v1 + v2 + carry;
            carry = sum/10;
            ListNode* node = new ListNode(sum%10);
            curr->next = node;
            curr = curr->next;
            p1 = (p1 == nullptr ? nullptr : p1->next);
            p2 = (p2 == nullptr ? nullptr : p2->next);
        }
        
        if(carry)
        {
            ListNode* node = new ListNode(carry);
            curr->next = node;
        }

        return dummy.next;
    }
};
```

# [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

```
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode node1(-1),node2(-1);
        ListNode* hsmall = &node1,*hbig = &node2;
        ListNode* curr_small = hsmall,*curr_big = hbig,*curr = head;
        while(curr)
        {
            if(curr->val < x)
            {
				curr_small->next = curr;
                curr_small = curr_small->next;
            }
            else
            {
                curr_big->next = curr;
                curr_big = curr_big->next; 
            }
            curr = curr->next;
        }
        curr_big->next = nullptr;
        curr_small->next = hbig->next;
        return hsmall->next;
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
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode dummy(-1);
        ListNode* curr = &dummy;
        while(list1 && list2)
        {
            if(list1->val <= list2->val)
            {
                curr->next = list1;
                list1 = list1->next;
            }
            else
            {
                curr->next = list2;
                list2 = list2->next;
            }
            curr = curr->next;
        }
        curr->next = list1 ? list1 : list2;
        return dummy.next;
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
        
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* prev = &dummy,*curr = head;
        ListNode* left = &dummy,*right = head;
        while(curr && curr->next)
        {
            left = curr->next;
            right = curr->next->next;

            curr->next->next = curr;
            curr->next = right;
            prev->next = left;

            prev = curr;
            curr = right;
        }
        return dummy.next;
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
        
        ListNode dummy(-1);
        dummy.next = head;
        ListNode* prev = &dummy;
        while(prev && prev->next)
        {
            ListNode* curr = prev->next;
            if(curr->next == nullptr || curr->val != curr->next->val)
                prev = curr;
            else
            {
                while(curr->next && curr->val == curr->next->val)
                    curr = curr->next;
                prev->next = curr->next;
            }
        }
        return dummy.next;
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
            slow = slow->next;
            fast = fast->next;
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
        ListNode* slow = head,*fast = head->next->next;
        while(fast && fast->next)
        {
            if(fast == slow)
                return true;
            slow = slow->next;
            fast = fast->next->next;
        }
        return false;
    }8uh
};
```

# [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head,*slow = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;

            //@ 链表有环
            if(fast == slow)
            {
                fast = head;
                while(fast != slow)
                {
                    slow = slow->next;
                    fast = fast->next;
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
	int cap_;
    list<pair<int, int>> record_;  //@ pair<key,value>
	using Iter = list<pair<int,int>>::iterator;	
    unordered_map<int, Iter> dict_;   
	
public:
    LRUCache(int capacity):cap_(capacity) {
    }
    
    int get(int key) {
		if(dict_.find(key) == dict_.end())
			return -1;
		int res = dict_[key]->second;
		record_.erase(dict_[key]);
		record_.push_front({key,res});
		dict_[key] = record_.begin();
		return res;
    }
    
    void put(int key, int value) {
		if(dict_.find(key) != dict_.end())		
			record_.erase(dict_[key]);
		
		record_.push_front({key,value});
		dict_[key] = record_.begin();
		
		if(record_.size() > cap_)
		{	
			dict_.erase(record_.back().first);
			record_.pop_back();
		}	
    }
};
```







