# 链表

## 链表排序

### [148. 排序链表](https://leetcode.cn/problems/sort-list/)

中等

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

// 冒泡排序（超时）
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode *node_i = head, *tail = nullptr;
        while(node_i != nullptr) {
            ListNode *node_j = head;
            while(node_j != nullptr && node_j->next != tail) {
                if(node_j->val > node_j->next->val) {
                    int temp = node_j->val;
                    node_j->val = node_j->next->val;
                    node_j->next->val = temp;
                }
                node_j = node_j->next;
            }
            tail = node_j;
            node_i = node_i->next;
        }
        return head;
    }
};

// 选择排序（超时）
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode *node_i = head;
        while(node_i != nullptr) {
            ListNode *node_j = node_i->next, *temp = node_i;
            while(node_j != nullptr) {
                if(temp->val > node_j->val) {
                    temp = node_j;
                }
                node_j = node_j->next;
            }
            if(node_i != temp) {
                int num = node_i->val;
                node_i->val = temp->val;
                temp->val = num;
            }
            node_i = node_i->next;
        }
        return head;
    }
};

// 插入排序（超时）
class Solution {
public:
    ListNode* insertionSort(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }

        ListNode* dummyHead = new ListNode(-1);
        dummyHead->next = head;
        ListNode* sortedList = head;
        ListNode* cur = head->next;

        while (cur) {
            if (sortedList->val <= cur->val) {
                // 将 cur 插入到 sortedList 之后
                sortedList = sortedList->next;
            } else {
                ListNode* prev = dummyHead;
                while (prev->next->val <= cur->val) {
                    prev = prev->next;
                }
                // 将 cur 插入到链表中间
                sortedList->next = cur->next;
                cur->next = prev->next;
                prev->next = cur;
            }
            cur = sortedList->next;
        }

        return dummyHead->next;
    }
    
    ListNode* sortList(ListNode* head) {
        return insertionSort(head);
    }
};

// 快速排序（超时）
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        return quickSort(head, nullptr);
    }

    ListNode* partition(ListNode* left, ListNode* right) {
        // 左闭右开，区间没有元素或者只有一个元素，直接返回第一个节点
        if (left == right || left->next == right) {
            return left;
        }
        // 选择头节点为基准节点
        int pivot = left->val;
        // 使用 nodeI, nodeJ 双指针，保证 nodeI 之前的节点值都小于基准节点值，
        // nodeI 与 nodeJ 之间的节点值都大于等于基准节点值
        ListNode* nodeI = left;
        ListNode* nodeJ = left->next;

        while (nodeJ != right) {
            // 发现一个小于基准值的元素
            if (nodeJ->val < pivot) {
                // 因为 nodeI 之前节点都小于基准值，所以先将 nodeI 向右移动一位
                // 此时 nodeI 节点值大于等于基准节点值
                nodeI = nodeI->next;
                // 将小于基准值的元素 nodeJ 与当前 nodeI 换位，
                // 换位后可以保证 nodeI 之前的节点都小于基准节点值
                int temp = nodeI->val;
                nodeI->val = nodeJ->val;
                nodeJ->val = temp;
            }
            nodeJ = nodeJ->next;
        }
        // 将基准节点放到正确位置上
        int temp = left->val;
        left->val = nodeI->val;
        nodeI->val = temp;
        return nodeI;
    }

    ListNode* quickSort(ListNode* left, ListNode* right) {
        if (left == right || left->next == right) {
            return left;
        }
        ListNode* pi = partition(left, right);
        quickSort(left, pi);
        quickSort(pi->next, right);
        return left;
    }
};

// 归并排序
class Solution {
public:
    ListNode* merge(ListNode* left, ListNode* right) {
        // 归并环节
        ListNode* dummyHead = new ListNode(-1);
        ListNode* cur = dummyHead;
        while (left && right) {
            if (left->val <= right->val) {
                cur->next = left;
                left = left->next;
            } else {
                cur->next = right;
                right = right->next;
            }
            cur = cur->next;
        }

        if (left) {
            cur->next = left;
        } else if (right) {
            cur->next = right;
        }

        return dummyHead->next;
    }

    ListNode* mergeSort(ListNode* head) {
        // 分割环节
        if (!head || !head->next) {
            return head;
        }

        // 快慢指针找到中心链节点
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // 断开左右链节点
        ListNode* leftHead = head;
        ListNode* rightHead = slow->next;
        slow->next = nullptr;

        // 归并操作
        return merge(mergeSort(leftHead), mergeSort(rightHead));
    }
    
    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

// 冒泡排序（超时）
class Solution {
    public ListNode sortList(ListNode head) {
        ListNode node_i = head, tail = null;
        while(node_i != null) {
            ListNode node_j = head;
            while(node_j != null && node_j.next != tail) {
                if(node_j.val > node_j.next.val) {
                    int temp = node_j.val;
                    node_j.val = node_j.next.val;
                    node_j.next.val = temp;
                }
                node_j = node_j.next;
            }
            tail = node_j;
            node_i = node_i.next;
        }
        return head;
    }
}

// 选择排序（超时）
class Solution {
    public ListNode sortList(ListNode head) {
        ListNode node_i = head;
        while(node_i != null) {
            ListNode node_j = node_i.next, temp = node_i;
            while(node_j != null) {
                if(temp.val > node_j.val) {
                    temp = node_j;
                }
                node_j = node_j.next;
            }
            if(node_i != temp) {
                int num = node_i.val;
                node_i.val = temp.val;
                temp.val = num;
            }
            node_i = node_i.next;
        }
        return head;
    }
}

// 插入排序（超时）
class Solution {
    public ListNode insertionSort(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode dummyHead = new ListNode(-1);
        dummyHead.next = head;
        ListNode sortedList = head;
        ListNode cur = head.next;

        while (cur != null) {
            if (sortedList.val <= cur.val) {
                // 将 cur 插入到 sortedList 之后
                sortedList = sortedList.next;
            } else {
                ListNode prev = dummyHead;
                while (prev.next.val <= cur.val) {
                    prev = prev.next;
                }
                // 将 cur 插入到链表中间
                sortedList.next = cur.next;
                cur.next = prev.next;
                prev.next = cur;
            }
            cur = sortedList.next;
        }

        return dummyHead.next;
    }
    
    public ListNode sortList(ListNode head) {
        return insertionSort(head);
    }
}

// 快速排序（超时）
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        return quickSort(head, null);
    }

    public ListNode quickSort(ListNode left, ListNode right) {
        if (left == right || left.next == right) {
            return left;
        }
        ListNode pivot = partion(left, right);
        quickSort(left, pivot);
        quickSort(pivot.next, right);
        return left;
    }

    public ListNode partion(ListNode left, ListNode right) {
        if (left == right || left.next == right) {
            return left;
        }
        // 选择头节点为基准节点
        int pivot = left.val;
        // 使用 nodeI, nodeJ 双指针，保证 nodeI 之前的节点值都小于基准节点值，
        // nodeI 与 nodeJ 之间的节点值都大于等于基准节点值
        ListNode nodeI = left;
        ListNode nodeJ = left.next;

        while (nodeJ != right) {
            // 发现一个小于基准值的元素
            if (nodeJ.val < pivot) {
                // 因为 nodeI 之前的节点都小于基准值，所以先将 nodeI 向右移动一位
                // 此时 nodeI 节点值大于等于基准节点值
                nodeI = nodeI.next;
                // 将小于基准值的元素 nodeJ 与当前 nodeI 换位，
                // 换位后可以保证 nodeI 之前的节点都小于基准节点值
                int temp = nodeI.val;
                nodeI.val = nodeJ.val;
                nodeJ.val = temp;
            }
            nodeJ = nodeJ.next;
        }
        // 将基准节点放到正确位置上
        int temp = left.val;
        left.val = nodeI.val;
        nodeI.val = temp;
        return nodeI;
    }
}

// 归并排序
class Solution {
    public ListNode merge(ListNode left, ListNode right) {
        // 归并环节
        ListNode dummyHead = new ListNode(-1);
        ListNode cur = dummyHead;
        while (left != null && right != null) {
            if (left.val <= right.val) {
                cur.next = left;
                left = left.next;
            } else {
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }

        if (left != null) {
            cur.next = left;
        } else if (right != null) {
            cur.next = right;
        }

        return dummyHead.next;
    }

    public ListNode mergeSort(ListNode head) {
        // 分割环节
        if (head == null || head.next == null) {
            return head;
        }

        // 快慢指针找到中心链节点
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // 断开左右链节点
        ListNode leftHead = head;
        ListNode rightHead = slow.next;
        slow.next = null;

        // 归并操作
        return merge(mergeSort(leftHead), mergeSort(rightHead));
    }

    public ListNode sortList(ListNode head) {
        return mergeSort(head);
    }
}
```



### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

简单

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *preHead = new ListNode(0, nullptr);
        ListNode *temp = preHead;
        while(list1 != nullptr && list2 != nullptr) {
            if(list1->val < list2->val) {
                temp->next = list1;
                list1 = list1->next;
            } else {
                temp->next = list2;
                list2 = list2->next;
            }
            temp = temp->next;
        }
        temp->next = list1 != nullptr ? list1 : list2;
        return preHead->next;
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode preHead = new ListNode(0, null);
        ListNode temp = preHead;
        while(list1 != null && list2 != null) {
            if(list1.val < list2.val) {
                temp.next = list1;
                list1 = list1.next;
            } else {
                temp.next = list2;
                list2 = list2.next;
            }
            temp = temp.next;
        }
        temp.next = list1 != null ? list1 : list2;
        return preHead.next;
    }
}
```



### [147. 对链表进行插入排序](https://leetcode.cn/problems/insertion-sort-list/)

中等

给定单个链表的头 `head` ，使用 **插入排序** 对链表进行排序，并返回 *排序后链表的头* 。

**插入排序** 算法的步骤:

1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

下面是插入排序算法的一个图形示例。部分排序的列表(黑色)最初只包含列表中的第一个元素。每次迭代时，从输入数据中删除一个元素(红色)，并就地插入已排序的列表中。

对链表进行插入排序。

![img](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/04/sort1linked-list.jpg)

```
输入: head = [4,2,1,3]
输出: [1,2,3,4]
```

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }

        ListNode* dummyHead = new ListNode(-1);
        dummyHead->next = head;
        ListNode* sortedList = head;
        ListNode* cur = head->next;

        while (cur) {
            if (sortedList->val <= cur->val) {
                // 将 cur 插入到 sortedList 之后
                sortedList = sortedList->next;
            } else {
                ListNode* prev = dummyHead;
                while (prev->next->val <= cur->val) {
                    prev = prev->next;
                }
                // 将 cur 插入到链表中间
                sortedList->next = cur->next;
                cur->next = prev->next;
                prev->next = cur;
            }
            cur = sortedList->next;
        }

        return dummyHead->next;
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode dummyHead = new ListNode(-1);
        dummyHead.next = head;
        ListNode sortedList = head;
        ListNode cur = head.next;

        while (cur != null) {
            if (sortedList.val <= cur.val) {
                // 将 cur 插入到 sortedList 之后
                sortedList = sortedList.next;
            } else {
                ListNode prev = dummyHead;
                while (prev.next.val <= cur.val) {
                    prev = prev.next;
                }
                // 将 cur 插入到链表中间
                sortedList.next = cur.next;
                cur.next = prev.next;
                prev.next = cur;
            }
            cur = sortedList.next;
        }

        return dummyHead.next;
    }
}
```



### [23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

困难

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

C++版本

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
// 方法一：顺序合并
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode *ans = nullptr;
        for (size_t i = 0; i < lists.size(); ++i) {
            ans = mergeTwoLists(ans, lists[i]);
        }
        return ans;
    }
};

// 方法二：分治合并
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* merge(vector <ListNode*> &lists, int l, int r) {
        if (l == r) return lists[l];
        if (l > r) return nullptr;
        int mid = (l + r) >> 1;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return merge(lists, 0, lists.size() - 1);
    }
};

// 方法三：使用优先队列合并
class Solution {
public:
    struct Status {
        int val;
        ListNode *ptr;
        bool operator < (const Status &rhs) const {
            return val > rhs.val;
        }
    };

    priority_queue <Status> q;

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        for (auto node: lists) {
            if (node) q.push({node->val, node});
        }
        ListNode head, *tail = &head;
        while (!q.empty()) {
            auto f = q.top(); q.pop();
            tail->next = f.ptr; 
            tail = tail->next;
            if (f.ptr->next) q.push({f.ptr->next->val, f.ptr->next});
        }
        return head.next;
    }
};
```

Java版本

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

// 方法一：顺序合并
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode ans = null;
        for (int i = 0; i < lists.length; ++i) {
            ans = mergeTwoLists(ans, lists[i]);
        }
        return ans;
    }

    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
}

// 方法二：分治合并
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        return merge(lists, 0, lists.length - 1);
    }

    public ListNode merge(ListNode[] lists, int l, int r) {
        if (l == r) {
            return lists[l];
        }
        if (l > r) {
            return null;
        }
        int mid = (l + r) >> 1;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
}

// 方法三：使用优先队列合并
class Solution {
    class Status implements Comparable<Status> {
        int val;
        ListNode ptr;

        Status(int val, ListNode ptr) {
            this.val = val;
            this.ptr = ptr;
        }

        public int compareTo(Status status2) {
            return this.val - status2.val;
        }
    }

    PriorityQueue<Status> queue = new PriorityQueue<Status>();

    public ListNode mergeKLists(ListNode[] lists) {
        for (ListNode node: lists) {
            if (node != null) {
                queue.offer(new Status(node.val, node));
            }
        }
        ListNode head = new ListNode(0);
        ListNode tail = head;
        while (!queue.isEmpty()) {
            Status f = queue.poll();
            tail.next = f.ptr;
            tail = tail.next;
            if (f.ptr.next != null) {
                queue.offer(new Status(f.ptr.next.val, f.ptr.next));
            }
        }
        return head.next;
    }
}
```



