# DSA Linked List - Set 1

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
![LeetCode Solved](https://img.shields.io/badge/dynamic/json?style=for-the-badge&label=Questions%20Solved&query=totalSolved&url=https://leetcode-stats-api.herokuapp.com/parthnarkar)

## 📋 Problems Included: 3
1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)
2. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)
3. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

---

### 1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

**Description:**  
Given the head of a singly linked list, reverse the list and return the reversed list.

**Approach:**  
We'll use three pointers: `prev`, `current`, and `next`. We traverse the list and reverse the links one by one until we reach the end.

**Java Code:**
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        while (current != null) {
            ListNode next = current.next;  // Store next node
            current.next = prev;            // Reverse the link
            prev = current;                 // Move prev forward
            current = next;                 // Move current forward
        }
        
        return prev;  // prev is the new head
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 2. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

**Description:**  
Given the head of a singly linked list, return the middle node. If there are two middle nodes, return the second middle node.

**Approach:**  
Use the slow and fast pointer technique. The slow pointer moves one step at a time, while the fast pointer moves two steps. When fast reaches the end, slow will be at the middle.

**Java Code:**
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;        // Move slow by 1
            fast = fast.next.next;   // Move fast by 2
        }
        
        return slow;  // slow is at the middle
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 3. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

**Description:**  
Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.

**Approach:**  
Create a dummy node to build the merged list. Compare nodes from both lists one by one and attach the smaller node to the result. Continue until both lists are exhausted.

**Java Code:**
```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                current.next = list1;
                list1 = list1.next;
            } else {
                current.next = list2;
                list2 = list2.next;
            }
            current = current.next;
        }
        
        // Attach remaining nodes
        if (list1 != null) {
            current.next = list1;
        } else {
            current.next = list2;
        }
        
        return dummy.next;
    }
}
```

**Time Complexity:** O(n + m)  
**Space Complexity:** O(1)

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

[![parth-builds-os Github Repo Footer](https://github.com/user-attachments/assets/4bef3a04-16ee-4484-a52c-4f31182e1916)](https://github.com/parthnarkar/parth-builds-os)
