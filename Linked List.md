## Leetcode 2

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // Define a pointer that points to the head pointer and returns the result
        ListNode dummyHead = new ListNode(0);
        
        // Define a portable pointer that points to the sum of two values
        ListNode cur = dummyHead;
        
        int carry = 0;
        
        while (l1 != null || l2 != null) {
            int sum = carry;
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            } 
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }
           
            carry = sum / 10;
            
            // Create and assign the sum value to a new pointer because cur.next = sum is invalid
            cur.next = new ListNode(sum % 10);
            cur = cur.next;
        }
        if (carry > 0) {
            cur.next = new ListNode(carry);
        }
        return dummyHead.next;
    }
}
```

## Leetcode 19
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode fast = dummyHead;
        ListNode slow = dummyHead;
        
        for (int i=0; i < n+1; i++) {
            fast = fast.next;    
        }
        
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        
        slow.next = slow.next.next;        
        return dummyHead.next;
    }
}
```

## Leetcode 21
```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummyHead = new ListNode(0);
        ListNode cur = dummyHead;
        
        while (list1 != null && list2 != null) {
            
            if (list1.val < list2.val) {
                cur.next = list1;
                list1 = list1.next;
            }
            else {
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next;
        }
        
        if (list1 != null) {
            cur.next = list1;
            list1 = list1.next;
        }
        
        if (list2 != null) {
            cur.next = list2;
            list2 = list2.next;
        }
        
        return dummyHead.next;        
    }
}
```


## Leetcode 23
Method 1: Follow Leetcode 21; Brute Force
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        
        for (int i=1; i < lists.length; i++) {
            lists[0] = mergeTwoLists(lists[0], lists[i]);
        }
        return lists[0];
    }
    
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummyHead = new ListNode(0);
        ListNode cur = dummyHead;
        
        while (list1 != null && list2 != null) {
            
            if (list1.val < list2.val) {
                cur.next = list1;
                list1 = list1.next;
            }
            else {
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next;
        }
        
        if (list1 != null) {
            cur.next = list1;
            list1 = list1.next;
        }
        
        if (list2 != null) {
            cur.next = list2;
            list2 = list2.next;
        }
        
        return dummyHead.next;        
    }
}
```

Method 2: Priority Queue
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists==null || lists.length==0) return null;
        
        PriorityQueue<ListNode> queue= new PriorityQueue<ListNode>(lists.length, (a,b)-> a.val-b.val);
        
        ListNode dummy = new ListNode(0);
        ListNode tail=dummy;
        
        for (ListNode node:lists)
            if (node!=null)
                queue.add(node);
            
        while (!queue.isEmpty()){
            tail.next=queue.poll();
            tail=tail.next;
            
            if (tail.next!=null)
                queue.add(tail.next);
        }
        return dummy.next;
    }
}
```

## Leetcode 24
Method 1: Recursion
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode temp = head.next;
        head.next = swapPairs(temp.next);
        temp.next = head;
        return temp;
    }
}
```

Method 2: Iteration
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = dummy;
        
        while (cur.next != null && cur.next.next != null) {
            ListNode first = cur.next;
            ListNode second = cur.next.next;
            first.next = second.next;
            cur.next = second;
            cur.next.next = first;
            cur = cur.next.next;
        }
        return dummy.next;
    }
}
```

## Leetcode 25
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null) return head;
        
        ListNode end = head;
        for (int i=0; i < k-1; i++) {
            end = end.next;
            if (end == null) return head;
        }
        
        ListNode newNode = end.next;
        ListNode newList = reverseList(head, end);
        head.next = reverseKGroup(newNode, k);
        return newList;
    }
    
    public ListNode reverseList(ListNode start, ListNode end) {
        ListNode tmp = null;
        while (tmp != end) {
            ListNode next = start.next;
            start.next = tmp;
            tmp = start;
            start = next;
        }
        return tmp;
    }   
}
```
