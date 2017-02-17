# Binary Search Tree Iterator

> link: https://leetcode.com/problems/binary-search-tree-iterator/
>
> Implement an iterator over a binary search tree \(BST\). Your iterator will be initialized with the root node of a BST.
>
> Calling`next()`will return the next smallest number in the BST.
>
> **Note:**`next()`and`hasNext()`should run in average O\(1\) time and uses O\(h\) memory, wherehis the height of the tree.

题意：实现一个查找树的迭代器，要求有O\(1\)的时间复杂度，只允许使用O\(h\)的空间复杂度，其中h是树的高度。

题目比较简单，仅需要将之前递归的栈用链表保存起来就行。难点在于怎么保存状态，以及开始节点以及最后一个节点的状态保存。

直接给代码：

```cpp
struct TreeNodeLink {
    struct TreeNode* node;
    struct TreeNodeLink* next;
    int viewed;
};
inline struct TreeNodeLink* addLinkNode(struct TreeNode* node, struct TreeNodeLink* head) {
    struct TreeNodeLink* tmp = (struct TreeNodeLink*)malloc(sizeof(struct TreeNodeLink));
    tmp->node = node;
    tmp->next = head;
    tmp->viewed = 0;
    return tmp;
}
inline struct TreeNodeLink* freeAndNext(struct TreeNodeLink* head) {
    if (!head) {
        return NULL;
    }
    struct TreeNodeLink* next = head->next;
    free(head);
    return next;
}

struct BSTIterator {
    struct TreeNode* root;
    struct TreeNodeLink* head;
};

struct BSTIterator *bstIteratorCreate(struct TreeNode *root) {
    if (!root) {
        return NULL;
    }
    struct BSTIterator* it = (struct BSTIterator*)malloc(sizeof(struct BSTIterator));
    it->root = root;
    struct TreeNodeLink* nlink = addLinkNode(root, NULL);
    while (nlink->node->left) {
        nlink = addLinkNode(nlink->node->left, nlink);
    }
    it->head = nlink;
    return it;
}

/** @return whether we have a next smallest number */
bool bstIteratorHasNext(struct BSTIterator *iter) {
    if (!iter) {
        return false;
    }
    struct TreeNodeLink* nlink = iter->head;
    for (; nlink != NULL; nlink = nlink->next) {
        if (nlink->viewed < 1 ||
                (nlink->viewed < 2 && nlink->node->right)) {
            return true;
        }
    }
    return false;
}

/** @return the next smallest number */
int bstIteratorNext(struct BSTIterator *iter) {
    if (!iter) {
        assert(false);
    }
    struct TreeNodeLink* nlink = iter->head;
    for (; nlink != NULL; nlink = freeAndNext(nlink)) {
        iter->head = nlink;
        if (nlink->viewed < 1) {
            nlink->viewed = 1;
            return nlink->node->val;
        }
        if (nlink->viewed < 2 && nlink->node->right) {
            nlink->viewed = 2;
            nlink = addLinkNode(nlink->node->right, nlink);
            while (nlink->node->left) {
                nlink = addLinkNode(nlink->node->left, nlink);
            }
            iter->head = nlink;
            nlink->viewed = 1;
            return nlink->node->val;
        }
    }
    assert(false);
}

/** Deallocates memory previously allocated for the iterator */
void bstIteratorFree(struct BSTIterator *iter) {
    while (iter && iter->head) {
        iter->head = freeAndNext(iter->head);
    }
}
```



