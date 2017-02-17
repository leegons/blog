# Validate Binary Search Tree

> link: [https://leetcode.com/problems/validate-binary-search-tree/](https://leetcode.com/problems/validate-binary-search-tree/)
>
> Given a binary tree, determine if it is a valid binary search tree \(BST\).
>
> Assume a BST is defined as follows:
>
> * The left subtree of a node contains only nodes with keys
>   **less than**
>   the node's key.
> * The right subtree of a node contains only nodes with keys
>   **greater than**
>   the node's key.
> * Both the left and right subtrees must also be binary search trees.
>
> **Example 1:**
>
> ```
>     2
>    / \
>   1   3
> ```
>
> Binary tree
>
> `[2,1,3]`
>
> , return true.
>
> **Example 2:**
>
> ```
>     1
>    / \
>   2   3
> ```
>
> Binary tree
>
> `[1,2,3]`
>
> , return false.

题意：校验一棵二叉树是不是二叉搜索树。

二叉搜索树符合以下条件：

1. 左子树下所有节点的值都要小于根节点。
2. 右子树下所有节点的值都要大于根节点。
3. 左子树和右子树也是二叉搜索树。

这个题目比较简单，因为有递归这种逆天的东西存在，每次都将大小限制往下传，遇到不符合的节点透传返回false。

因为存在无上限或者无下限，所以使用指针的方式传参，无限制时传NULL。

代码：

```c
bool validNode(struct TreeNode* root, int* min, int* max) {
    return (!min || *min < root->val) &&
           (!max || *max > root->val) &&
           (!root->left  || validNode(root->left, min, &(root->val))) &&
           (!root->right || validNode(root->right, &(root->val), max));
}

bool isValidBST(struct TreeNode* root) {
    return !root || validNode(root, NULL, NULL);
}
```



