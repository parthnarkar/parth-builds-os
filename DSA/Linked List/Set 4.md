# DSA Linked List - Set 4

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
[![LeetCode Profile](https://img.shields.io/badge/LeetCode-ParthNarkar-FFA116?style=for-the-badge&logo=leetcode)](https://leetcode.com/u/parthnarkar/)
![LeetCode Solved](https://img.shields.io/badge/dynamic/json?style=for-the-badge&label=Questions%20Solved&query=totalSolved&url=https://leetcode-stats-api.herokuapp.com/parthnarkar)

## 📋 Problems Included: 4
1. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)
2. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)
3. [Rotate List](https://leetcode.com/problems/rotate-list/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)
4. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

---

### 1. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) ![Easy](https://img.shields.io/badge/Easy-00b8a3?style=flat-square)

**Description:**  
Given the head of a singly linked list, return true if it is a palindrome or false otherwise.

**Approach:**  
Use slow and fast pointers to find the middle of the list. Reverse the second half of the list. Compare the first half with the reversed second half. Optionally restore the list to its original state.

**Java Code:**
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;
        
        // Find the middle of the list
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // Reverse the second half
        ListNode secondHalf = reverse(slow);
        ListNode firstHalf = head;
        
        // Compare both halves
        while (secondHalf != null) {
            if (firstHalf.val != secondHalf.val) {
                return false;
            }
            firstHalf = firstHalf.next;
            secondHalf = secondHalf.next;
        }
        
        return true;
    }
    
    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 2. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

**Description:**  
Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

**Approach:**  
Use Floyd's Cycle Detection Algorithm. First, detect if a cycle exists using slow and fast pointers. If they meet, reset one pointer to head and move both one step at a time. They will meet at the cycle's starting point.

**Java Code:**
```java
class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) return null;
        
        ListNode slow = head;
        ListNode fast = head;
        
        // Detect if cycle exists
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            
            if (slow == fast) {
                // Cycle detected, find the start
                slow = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow; // Starting point of cycle
            }
        }
        
        return null; // No cycle
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 3. [Rotate List](https://leetcode.com/problems/rotate-list/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

**Description:**  
Given the head of a linked list, rotate the list to the right by k places.

**Approach:**  
First, find the length of the list and connect the tail to head to form a circle. Calculate the effective rotations needed (k modulo length). Find the new tail at position (length - k - 1) and break the circle to get the new head.

**Java Code:**
```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;
        
        // Find length and last node
        ListNode current = head;
        int length = 1;
        while (current.next != null) {
            current = current.next;
            length++;
        }
        
        // Connect tail to head to make it circular
        current.next = head;
        
        // Find the new tail: (length - k % length - 1)th node
        k = k % length;
        int stepsToNewTail = length - k - 1;
        
        ListNode newTail = head;
        for (int i = 0; i < stepsToNewTail; i++) {
            newTail = newTail.next;
        }
        
        // Break the circle
        ListNode newHead = newTail.next;
        newTail.next = null;
        
        return newHead;
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 4. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) ![Medium](https://img.shields.io/badge/Medium-ffa116?style=flat-square)

**Description:**  
A linked list is given where each node contains an additional random pointer which could point to any node in the list or null. Return a deep copy of the list.

**Approach:**  
Use a HashMap to store the mapping between original nodes and copied nodes. First pass creates all new nodes and stores them in the map. Second pass sets the next and random pointers of the copied nodes using the map.

**Java Code:**
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        // HashMap to store original node -> copied node mapping
        Map<Node, Node> map = new HashMap<>();
        
        // First pass: create all nodes
        Node current = head;
        while (current != null) {
            map.put(current, new Node(current.val));
            current = current.next;
        }
        
        // Second pass: assign next and random pointers
        current = head;
        while (current != null) {
            Node copiedNode = map.get(current);
            copiedNode.next = map.get(current.next);
            copiedNode.random = map.get(current.random);
            current = current.next;
        }
        
        return map.get(head);
    }
}
```

**Alternative Approach (O(1) Space):**
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        // Step 1: Create copied nodes and interweave them
        Node current = head;
        while (current != null) {
            Node copy = new Node(current.val);
            copy.next = current.next;
            current.next = copy;
            current = copy.next;
        }
        
        // Step 2: Assign random pointers for copied nodes
        current = head;
        while (current != null) {
            if (current.random != null) {
                current.next.random = current.random.next;
            }
            current = current.next.next;
        }
        
        // Step 3: Separate the two lists
        current = head;
        Node copyHead = head.next;
        Node copyCurrent = copyHead;
        
        while (current != null) {
            current.next = current.next.next;
            if (copyCurrent.next != null) {
                copyCurrent.next = copyCurrent.next.next;
            }
            current = current.next;
            copyCurrent = copyCurrent.next;
        }
        
        return copyHead;
    }
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n) for HashMap approach, O(1) for interweaving approach

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