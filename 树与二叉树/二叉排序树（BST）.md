## 二叉排序树（BST）

![二叉排序树](D:\笔记\数据结构\思维导图\二叉排序树.png)

**左子树结点值 < 根结点值 < 右子树结点值**

用中序遍历可以得到一个递增的有序序列



#### 二叉排序树的查找

若树非空，目标值与根结点的值比较：

1. 若相等则查找成功
2. 若小于根结点值则在左子树上查找，反之则在右子树上查找

```c++
//二叉排序树结点的定义
typedef struct BSTNode{
    int key;
    BSTNOde *lchild , *rchild;
}BSTNode , *BSTree;

//查找（非递归实现）
//最坏空间复杂度O(1)
BSTNode *BST_Search(BSTree *T , int target)
{
    while(T != NULL && target != T -> key)	//若树空或找到目标值则结束循环
    {
        if(target < T -> key)
            T = T -> lchild;
        else
            T = T -> rchild;
	}
    return T;
}

//查找（递归实现）
//最坏空间复杂度O(h)
BSTNode *BST_Search(BSTree *T , int target)
{
    if(T == NULL)
        return NULL;
    if(target == T -> key)
        return T;
    else if(target < T -> key)
        return BST_Search(T -> lchild , target);
    else
        return BST_Search(T -> rchild , target);
}
```



#### 二叉排序树的插入

若树为空，则直接插入结点；

若树非空，如果插入值小于根结点值则插入到左子树，否则插入到右子树

**插入位置一定是叶结点**

```c++
//插入（递归实现）
//最坏空间复杂度O(h)
int BST_Insert(BSTree &T , int k)
{
	if(T == NULL)
    {
		T = (BSTree) malloc (sizeof(BSTNode));
        T - key = k;
        T -> lchild = T -> rchild = NULL;
        return 1;								//插入成功返回1
    }
    else if(k == T -> key)						//如果树中已经存在值为k的结点，插入失败
        return 0;
    else if(k < T -> key)
        return BST_Insert(T -> lchild , k);
    else
        return BST_Insert(T -> rchild , k);
}
```



#### 二叉排序树的构造

按照给定序列构造二叉排序树，不同的序列可能会得到不同的二叉排序树

```c++
void Creat_BST(BSTree &T , int str[] , int n)
{
	T = NULL;
    int i = 0;
    while(i < n)
    {
        BST_Insert(T , str[i]);
        i ++;
	}        
}
```



#### 二叉排序树的删除

1. 若被删除结点**z**是叶节点，则直接删除
2. 若被删除的结点**z**只有一棵左子树或右子树，则让**z**的子树成为**z**的父节点的子树
3. 若结点**z**有左右两棵子树，则可以让z的**直接前驱/直接后继**替代**z**，然后删去这个前驱/后继，这样就可以转换成上面两种情况

直接前驱：左子树中最大的值/左子树中最右下的值

直接后继：右子树中最小的值/右子树中最左下的值



#### 查找效率分析

平均查找长度**ASL(Average Search Length)**

![查找效率](D:\笔记\数据结构\思维导图\查找效率.png)



![查找失败ASL](D:\笔记\数据结构\思维导图\查找失败ASL.png)