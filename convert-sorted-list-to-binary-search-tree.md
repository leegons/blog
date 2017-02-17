# Convert Sorted List to Binary Search Tree

> link: https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/
>
> Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

首先想到的方式是分治递归，因为平衡二叉树的左右子树的高度差不超过1，在左右子树子节点数量差不超过一的情况下很容易达到。所以解决的思路大概为：

1. 找到中心点将数对半分，中心点做根
2. 将小于中心点的数生成平衡二叉树作为左子树
3. 将大于中心店的数生成平衡二叉树作为右子树

思路清晰了，接下来就是实现，之前有一个题目是将数组转成平衡二叉树，按照这个思路可以很快生成代码：

```py
class Solution(object):
    def sortedArrayToBST(self, nums):
        l = len(nums)
        if l < 1:
            return None

        p = l / 2
        t = TreeNode(nums[p])
        t.left = self.sortedArrayToBST(nums[0 : p])
        t.right = self.sortedArrayToBST(nums[p + 1 : ])
        return t
```

但是这个题目给出的是有序链表，也就是说找到中心点的成本是O\(n\)，生成整颗树的成本接近O\(n^2\)

方案一，将链表转成数组，然后按照前面的方法生成二叉树。这种方案需要额外O\(n\)的空间来存储中间数组。

其他方案，待定。

