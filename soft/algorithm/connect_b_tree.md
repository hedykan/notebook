# 填充每个节点的下一个右侧节点指针

## 题目描述
给定一个 完美二叉树 ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：  
```c
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
初始状态下，所有 next 指针都被设置为 NULL。

## 题目解析

## c语言实现
```c
struct Node* connect(struct Node* root) {
    if(root == NULL) {
        return root;
    }
	connectItem2(root);
    return root;
}

// 方法一，给定任意两个节点进行连接
void connectItem1(struct Node *first, struct Node *second) {
    if(first == NULL || second == NULL) {
        return;
    }
    first->next = second;

    connectItem(first->left, first->right);
    connectItem(second->left, second->right);
    connectItem(first->right, second->left);
    return;
}

// 方法二，通过循环将同父级的左右子树之间相邻的节点连接在一起
void connectItem2(struct Node *root) {
    if(root == NULL) {
        return;
    }
    struct Node *p = root->left;
    struct Node *q = root->right;

    while(p != NULL) {
        p->next = q; // 除了第一次是同父级相连，其他时候都是把当前父级的左右子树中间连在一起
        p = p->right;
        q = q->left;
    }
    connectItem(root->left);
    connectItem(root->right);
    return;
}
```