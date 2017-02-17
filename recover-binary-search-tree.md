# Recover Binary Search Tree

> link: [https://leetcode.com/problems/recover-binary-search-tree/](https://leetcode.com/problems/recover-binary-search-tree/)
>
> Two elements of a binary search tree \(BST\) are swapped by mistake.
>
> Recover the tree without changing its structure.

题意： 一个二叉查找树的两个节点被调换的位置，找到并将位置修复，算法不允许改变树的结构。

这个题目乍一看很复杂，因为发现两个节点位置不对时很难确定出问题的节点是哪个，这样算下来可能的情况太多。

不过题目的条件是不允许改变二叉树的结构，因此可以转换下思路，先按照先序便利二叉树转化成数组，然后题目就成了修复一个有序数组的两个问题元素。思路：

1. 遍历二叉树，转化成一个数组。
2. 分别从左向右以及从右向左找到两个出问题的节点。
3. 调换这两个节点。

分析下这里空间复杂度是O\(n\)，时间复杂度是O\(2n\)，其中n为节点个数。

另外关于上边第二条找到问题节点的方法，大家可以证明下有效性。

上代码：

```cpp
int countnode(struct TreeNode* head) {
    if (!head) {
        return 0;
    }
    return countnode(head->left) + countnode(head->right) + 1;
}

void mapnode(struct TreeNode* head, struct TreeNode** links, int* pos) {
    if (!head) {
        return;
    }
    mapnode(head->left, links, pos);
    links[(*pos)++] = head;
    mapnode(head->right, links, pos);
}

void recoverTree(struct TreeNode* root) {
    if (!root) {
        return;
    }

    int cnt = countnode(root);
    struct TreeNode** links = (struct TreeNode**)malloc(sizeof(struct TreeNode*) * cnt);

    int pos = 0;
    mapnode(root, links, &pos);
    assert(cnt == pos);

    struct TreeNode** left = links;
    struct TreeNode** right = links + (cnt - 1);

    for (; left < right; ++left) {
        if ((*left)->val > (*(left + 1))->val) {
            break;
        }
    }
    for (; right > left; --right) {
        if ((*right)->val < (*(right - 1))->val) {
            break;
        }
    }
    if (left != right) {
        int tmp = (*left)->val;
        (*left)->val = (*right)->val;
        (*right)->val = tmp;
    }
}
```

另外想了想是不是可以将转化成数组的这一步省掉呢，只要先序遍历和后序遍历找到两个出错的节点就行，这样可以节省O\(n\)的空间消耗。

不过实现之后发现时间消耗比上面这种方式还大，可能是数据量过小。

上代码：

```cpp
struct TreeNode* g_pre;

bool checkLeft(struct TreeNode* now) {
    if (g_pre && now->val < g_pre->val) {
        return true;
    }
    g_pre = now;
    return false;
}
bool checkRight(struct TreeNode* now) {
    if (g_pre && now->val > g_pre->val) {
        return true;
    }
    g_pre = now;
    return false;
}

bool findLeft(struct TreeNode* head) {
    return head->left && findLeft(head->left) || 
           checkLeft(head) ||
           head->right && findLeft(head->right);
}
bool findRight(struct TreeNode* head) {
    return head->right && findRight(head->right) ||
           checkRight(head) ||
           head->left && findRight(head->left);
}

void recoverTree(struct TreeNode* root) {
    if (!root) {
        return;
    }
    
    struct TreeNode* left = NULL;
    struct TreeNode* right = NULL;
    
    g_pre = NULL;
    if (findLeft(root)) {
        left = g_pre;
    }
    g_pre = NULL;
    if (findRight(root)) {
        right = g_pre;
    }
    
    if (left && right && left->val > right->val) {
        int tmp = left->val;
        left->val = right->val;
        right->val = tmp;
    }
}
```



