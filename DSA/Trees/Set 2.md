# DSA Trees - Set 2

📋 Problems Included: 3

1. Maximum Depth of Binary Tree (Easy)
2. Balanced Binary Tree (Easy)
3. Binary Tree Level Order Traversal (Medium)

---

# 1. Maximum Depth of Binary Tree Easy

### LeetCode Problem

🔗 https://leetcode.com/problems/maximum-depth-of-binary-tree/

### Description

Given the root of a binary tree, return its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Approach

Use recursion to find the depth of the left and right subtrees. The answer is the maximum of both depths plus one.

### C++ Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == NULL) return 0;

        int left = maxDepth(root->left);
        int right = maxDepth(root->right);

        int ans = max(left, right)+1;
        return ans;
        
    }
};

```

### Time Complexity

O(n)

### Space Complexity

O(h)

---

# 2. Balanced Binary Tree Easy

### LeetCode Problem

🔗 https://leetcode.com/problems/balanced-binary-tree/

### Description

Given a binary tree, determine if it is height-balanced.

A binary tree is balanced if the heights of the two subtrees of every node never differ by more than one.

### Approach

Calculate the height of each subtree. If the difference between left and right heights exceeds one at any node, the tree is not balanced.

### C++ Code

```cpp
class Solution {
public:
    int height(TreeNode* root) {
        if (!root)
            return 0;

        int left = height(root->left);
        if (left == -1)
            return -1;

        int right = height(root->right);
        if (right == -1)
            return -1;

        if (abs(left - right) > 1)
            return -1;

        return 1 + max(left, right);
    }

    bool isBalanced(TreeNode* root) {
        return height(root) != -1;
    }
};
```

### Time Complexity

O(n)

### Space Complexity

O(h)

---

# 3. Diameter of Binary Tree Easy

### LeetCode Problem

🔗 https://leetcode.com/problems/diameter-of-binary-tree/

### Description

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

### Approach 1

For every node, calculate:

* Height of the left subtree
* Height of the right subtree

The diameter passing through that node is:

```text
leftHeight + rightHeight
```

Keep track of the maximum diameter encountered while computing heights recursively.

### C++ Code

```cpp
class Solution {
public:
    int diameter = 0;

    int height(TreeNode* root) {
        if (!root)
            return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        diameter = max(diameter, leftHeight + rightHeight);

        return 1 + max(leftHeight, rightHeight);
    }

    int diameterOfBinaryTree(TreeNode* root) {
        height(root);
        return diameter;
    }
};
```

### Time Complexity

O(n)

### Space Complexity

O(h)


