<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

# serval list problem

## 关于单链表中的环
1. **给一个单链表，判断是否右环存在**  [leetcode 141](https://leetcode.com/problems/linked-list-cycle/)
   对于这个问题我们可以采用“快慢指针”的方法。就是有两个指针fast和slow，开始的时候两个指针都指向链表头head，然后在每一步操作中slow向前走一步即：slow = slow->next，而fast每一步向前两步即：fast = fast->next->next。
   
   
   
   二者一定能够在环上相遇，并且此时slow还没有绕环一圈，也就是说一定是在slow走完第一圈之前相遇。
   
   
   
   也就是说，slow每次向前走一步，fast向前追了两步，因此每一步操作后fast到slow的距离缩短了1步，这样继续下去就会使得两者之间的距离逐渐缩小：...、5、4、3、2、1、0 -> 相遇。又因为在同一个环中fast和slow之间的距离不会大于换的长度，因此
   
   到二者相遇的时候slow一定还没有走完一周（或者正好走完以后，这种情况出现在开始的时候fast和slow都在环的入口处）。
   
   ```c++
   /**
    * Definition for singly-linked list.
    * struct ListNode {
    *     int val;
    *     ListNode *next;
    *     ListNode(int x) : val(x), next(NULL) {}
    * };
    */
   class Solution {
   public:
       bool hasCycle(ListNode *head) {
           ListNode* p = head,*q = head;
           while(q && p&& p->next){
               p = p->next->next;
               q = q->next;
               if(q == p)
                   return true;
           }
           return false;
       }
   };
   ```

2. **如果存在环，找出环的入口点**  [leetcode 142](https://leetcode.com/problems/linked-list-cycle-ii)

   Slow pointer 每次移动 1 步, fast pointer 每次移动 2 步, 从 head 同时出发.
   记环的入口 index 为 $e$(entrance), 第一次相遇时 index 为 $m$(meeting), 环的长度为 $l$ (loop length).
   则当 slow pointer 到入口时, fast pointer 比 slow pointer 快了 emodlemodl 步, 则
   $$
   m = e + [l - (e \bmod l)].
   $$
   相遇后把 fast pointer 扔掉, 同时让另一个 slow pointer 从 head 开始跑, 则新 slow pointer 到入口时, 比老的慢
   $$
   [(m + e) - e] \bmod l = 0.
   $$

   ```c++
   /**
    * Definition for singly-linked list.
    * struct ListNode {
    *     int val;
    *     ListNode *next;
    *     ListNode(int x) : val(x), next(NULL) {}
    * };
    */
   class Solution {
   public:
       ListNode *detectCycle(ListNode *head) {
           if(!head || !(head->next))
               return nullptr;
           ListNode* fast = head,*slow = head;
           while(slow && fast && fast->next){
               slow = slow->next;
               fast = fast->next->next;
               if(fast == slow){
                   fast = head;
                   while(fast != slow){
                       slow = slow->next;
                       fast = fast->next;
                   }
                   return slow;
               }
                 
           }
           return nullptr;
          
       }
   };
   ```

   

   
