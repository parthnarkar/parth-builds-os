# DSA Linked List - Set 2

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
![LeetCode Solved](https://img.shields.io/badge/dynamic/json?style=for-the-badge&label=Questions%20Solved&query=totalSolved&url=https://leetcode-stats-api.herokuapp.com/parthnarkar)

## 📋 Problems Included: 3
1. [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)
2. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)
3. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

---

### 1. [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

**Description:**  
There is a singly-linked list and you are given a node that needs to be deleted. You will not be given access to the head of the list. Delete the given node from the linked list.

**Approach:**  
Since we don't have access to the previous node, we can't change the previous node's next pointer. Instead, we copy the value of the next node into the current node and then delete the next node.

**Java Code:**
```java
class Solution {
    public void deleteNode(ListNode node) {
        // Copy the value of the next node to current node
        node.val = node.next.val;
        
        // Skip the next node
        node.next = node.next.next;
    }
}
```

**Time Complexity:** O(1)  
**Space Complexity:** O(1)

---

### 2. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

**Description:**  
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each node contains a single digit. Add the two numbers and return the sum as a linked list.

**Approach:**  
Traverse both lists simultaneously, adding corresponding digits along with any carry from the previous addition. Create new nodes for the result list. Continue until both lists are exhausted and there's no carry left.

**Java Code:**
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        int carry = 0;
        
        while (l1 != null || l2 != null || carry != 0) {
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
            current.next = new ListNode(sum % 10);
            current = current.next;
        }
        
        return dummy.next;
    }
}
```

**Time Complexity:** O(max(n, m))  
**Space Complexity:** O(max(n, m))

---

### 3. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

**Description:**  
Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Approach:**  
Use two pointers with a gap of n nodes between them. Move both pointers until the first pointer reaches the end. The second pointer will be just before the node to be deleted. Use a dummy node to handle edge cases like removing the head.

**Java Code:**
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode first = dummy;
        ListNode second = dummy;
        
        // Move first pointer n+1 steps ahead
        for (int i = 0; i <= n; i++) {
            first = first.next;
        }
        
        // Move both pointers until first reaches the end
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        
        // Remove the nth node from end
        second.next = second.next.next;
        
        return dummy.next;
    }
}
```

**Time Complexity:** O(n)  
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
**More DSA sets coming soon, Stay Tuned! 📘**
