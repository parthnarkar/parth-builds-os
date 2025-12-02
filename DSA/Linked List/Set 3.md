# DSA Linked List - Set 3

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
![LeetCode Solved](https://img.shields.io/badge/dynamic/json?style=for-the-badge&label=Questions%20Solved&query=totalSolved&url=https://leetcode-stats-api.herokuapp.com/parthnarkar)

## 📋 Problems Included: 3
1. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)
2. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)
3. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) ![Hard](https://img.shields.io/badge/Hard-ef4743?style=flat-square)

---

### 1. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

**Description:**  
Given the heads of two singly linked lists, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

**Approach:**  
Use two pointers starting at each head. When a pointer reaches the end, redirect it to the other list's head. Both pointers will meet at the intersection point or both will become null if there's no intersection. This works because both pointers travel the same total distance.

**Java Code:**
```java
class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        
        ListNode pointerA = headA;
        ListNode pointerB = headB;
        
        // Traverse both lists
        while (pointerA != pointerB) {
            // When reaching end, switch to other list's head
            pointerA = (pointerA == null) ? headB : pointerA.next;
            pointerB = (pointerB == null) ? headA : pointerB.next;
        }
        
        return pointerA; // Either intersection node or null
    }
}
```

**Time Complexity:** O(n + m)  
**Space Complexity:** O(1)

---

### 2. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

**Description:**  
Given the head of a linked list, determine if the linked list has a cycle in it. A cycle exists if there is some node in the list that can be reached again by continuously following the next pointer.

**Approach:**  
Use Floyd's Cycle Detection Algorithm (Tortoise and Hare). Use two pointers: slow moves one step at a time, fast moves two steps. If there's a cycle, fast will eventually meet slow. If fast reaches null, there's no cycle.

**Java Code:**
```java
class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;           // Move one step
            fast = fast.next.next;      // Move two steps
            
            if (slow == fast) {         // Cycle detected
                return true;
            }
        }
        
        return false; // No cycle
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 3. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) ![Hard](https://img.shields.io/badge/Hard-ef4743?style=flat-square)

**Description:**  
Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list. If the number of nodes is not a multiple of k, then left-over nodes at the end should remain as is.

**Approach:**  
First, check if there are at least k nodes remaining. If yes, reverse those k nodes and recursively process the rest. Use three pointers to reverse: previous, current, and next. Connect the reversed group with the previous and next groups properly.

**Java Code:**
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) return head;
        
        // Check if there are k nodes remaining
        ListNode current = head;
        int count = 0;
        while (current != null && count < k) {
            current = current.next;
            count++;
        }
        
        // If we have k nodes, reverse them
        if (count == k) {
            ListNode prev = null;
            ListNode curr = head;
            ListNode next = null;
            count = 0;
            
            // Reverse k nodes
            while (curr != null && count < k) {
                next = curr.next;
                curr.next = prev;
                prev = curr;
                curr = next;
                count++;
            }
            
            // Recursively reverse the rest and connect
            if (next != null) {
                head.next = reverseKGroup(next, k);
            }
            
            return prev; // prev is the new head of reversed group
        }
        
        return head; // Less than k nodes, return as is
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n/k) for recursion stack

---

## 🔗 Connect With Me

[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/parth.builds)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/parthnarkar)
[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/parthnarkar)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
[![Email](https://img.shields.io/badge/-Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:parthnarkarofficial@gmail.com)
[![Twitter](https://img.shields.io/badge/-Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/parthnarkar)
[![Discord](https://img.shields.io/badge/-Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com/users/parth_narkar)

### ⭐ Found this helpful? Give this Repo a STAR!
**More DSA sets coming soon, Stay Tuned! 📘**
