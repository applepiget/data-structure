## B树

又称多路平衡查找树，B树中所有结点的孩子个数的最大值称为B树的阶，通常用*m*表示。

![B树](D:\笔记\数据结构\思维导图\B树.png)



**5叉查找树**

![五叉排序树](D:\笔记\数据结构\思维导图\五叉排序树.png)

**B树的高度**

![B树的高度](D:\笔记\数据结构\思维导图\B树的高度.png)

![B树的高度范围](D:\笔记\数据结构\思维导图\B树的高度范围.png)



---



## B树的插入和删除

#### 插入：

1. 新元素插入的位置一定是最底层的终端节点中，用B树的查找来确定插入的位置
2. 若插入关键字后，导致原结点关键字个数超过上限，则从中间为值(m / 2 上取整)将其中的关键字分为两部分，左部分包含的关键字放在原结点中，右部分包含的关键字放在新结点中，中间位置的结点插入原结点的父节点



#### 删除：

1. 若被删除的关键字在终端结点，则直接删除该关键字（需要注意关键字个数是否小于B树中最小的关键字个数）
2. 若被删除的关键字在非终端姐点，则用直接前驱或直接后继来代替被删除的关键字，将对非终端结点的删除转化为对终端结点的删除

![删除元素](D:\笔记\数据结构\思维导图\删除元素.png)

3. 若删除关键字后该结点的关键字个数低于下限，并且此节点的右（左）兄弟结点的关键字个数大于关键字个数的下限，则可以需要调整该结点

   ![调整过程1](D:\笔记\数据结构\思维导图\调整过程1.png)

![调整过程2](D:\笔记\数据结构\思维导图\调整过程2.png)



4. 若左右兄弟结点都不能进行以上调整，则需要进行合并

   ![结点合并1](D:\笔记\数据结构\思维导图\结点合并1.png)



![结点合并2](D:\笔记\数据结构\思维导图\结点合并2.png)

由于合并之后，他们的父节点又出现相同的情况，所以需要进行相同的操作



![结点合并3](D:\笔记\数据结构\思维导图\结点合并3.png)