### leetcode [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
> 例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
>

```cpp
//方法一：利用二叉搜索树的性质，判断其值，决定位置
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode *minNode=p->val>q->val?q:p;
        TreeNode *maxNode=p->val>q->val?p:q;
        while(true)
        {
            if(root->val>maxNode->val)
            {
                root=root->left;
            }
            else if(root->val<minNode->val)
            {
                root=root->right;
            }
            else{
                break;
            }
        }
        return root;
    }
//方法二：通用做法，跟二叉树的最近公共祖先一个做法
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root==nullptr || root->val==p->val || root->val==q->val)
        return root;
    TreeNode *left=lowestCommonAncestor(root->left,p,q);
    TreeNode *right=lowestCommonAncestor(root->right,p,q);
    if(left==nullptr)   return right;
    if(right==nullptr)  return left;
    return root;
}
```

