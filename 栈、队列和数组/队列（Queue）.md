## 队列（Queue）

只允许在一端插入，在另一端删除的线性表

```c++
#define maxsize 50
typedef struct{
	ElemType data[maxsize];
    int front , rear;
}SqQueue;
```

**队尾指针指向后一个位置**

初始化

```c++
void InitQueue(SqQueue &Q)
{
	Q.rear = Q.front = 0;
}
```



判断队列是否为空

```c++
bool Empty(SqQueue Q)
{
	if(Q.rear == Q.front)
		return true;
	else
		return false;
}
```



入队

```c++
bool EnQueue(SqQueue &Q , Elemtype x)
{
	if(size == maxsize)
        return false;
    Q.data[Q.rear] = x;
    Q.rear = (Q.rear + 1) % maxsize;
    return true;
}
```



出队

```c++
bool DeQueue(SqQueue &Q , ElemType &x)
{
	if(Q.rear == Q.front)
		return false;
	
	x = Q.data[Q.front];
	Q.front = (Q.front + 1) % maxsize;
	return true;
}
```



---



1.可以在定义时设置一个size，来表示队列的长度，判断队列是否为空或者为满

```c++
typedef struct{
	int data[maxsize];
    int front = 0 , rear = 0;
    int size = 0;
}SqQueue;
```

插入成功执行**size++**；删除成功执行**size--**



2.设置一个tag，表示上一次操作是插入还是删除

```c++
typedef struct{
	int data[maxsize];
    int front = 0 , rear = 0;
    int tag;
}
```

每次插入成功，令**tag = 1；每次删除成功，令**tag = 0**

```c++
//队满条件
front == rear && tag == 1;

//队空条件
front == rear && tag == 0;
```



---



**队尾指针指向队尾元素**

```c++
//初始化
rear = maxsize - 1;
```



```c++
//入队操作
Q.rear = (Q.rear + 1) % maxsize;
Q.data[Q.rear] = x;
```



```c++
//出队
x = Q.data[Q.front];
Q.front = (Q.front + 1) % maxsize;
```



```c++
//判空
(Q.rear + 1) % Maxsize == Q.front.
    
//设置size表示队列长度或者tag表示上一次操作是删除还是插入
```



---



**队列的链式存储**

1.带头结点

```c++
//定义
typedef struct LinkNode{
	Elemtype data;
	struct LinkNode *next;
}LinkNode;

typedef struct{
	LinkNode *front , *rear;
}LinkQueue;

//初始化
void InitQueue(LinkQueue &Q)
{
    Q.front = Q.rear = (LinkNode*) malloc (sizeof(LinkNode)); //申请一个头节点
    Q.front -> next = NULL;
}

//判空
bool IsEmpty(LinkQueue Q)
{
    if(Q.front == Q.rear)
        return true;
   	else
        return false;
}

//入队
void EnQueue(LinkQueue &Q , Elemtype x)
{
    LinkNode *s = (LinkNode*) mallocs(sizeof(LinkNode));
    s -> data = x;
    s -> next = NULL;
    Q.rear -> next = s;
    Q.rear = s;
}

//出队
bool DeQueue(LinkQueue &Q , Elemtype x)
{
    if(Q.front == Q.rear)
        return false;
   	LinkNode *p = Q.front -> next;
    x = p -> data;
    Q.foront -> next = p -> next;
    
    if(Q.rear == p)//判断要删除的是否是队列中的最后一个元素
        Q.rear = Q.front;
    free(p);
    return true;
}
```



2.不带头结点

```c++
//初始化
void InitQueue(LinkQueue &Q)
{
    Q.front = NULL;
    Q.rear = NULL;
}

//判空
bool IsEmpty(LinkQueue Q)
{
    if(Q.front == NULL)
        return true;
   	else
        return false;
}

//入队
void EnQueue(LinkQueue &Q , Elemtype x)
{
	LinkNode *s = (LinkNode*) mallocs(sizeof(LinkNode));
	s -> data = x;
    s -> next = NULL;
    
    if(Q.front == NULL)//第一个元素入队时需要特殊操作，修改front 和 rear 的指针
    {
        Q.fornt = s;
        Q.rear = s;
	}
    else
    {
        Q.rear -> next = s;
        Q.rear = s;
	}
}

//出队
bool DeQueue(LinkQueue &Q , Elemtype x)
{
    if(Q.front == NULL)
        return false;
    LinkNode *p = Q.front;
    Q.front = p -> next;
    x = p -> data;
    
    if(Q.rear == p)
    {
        Q.front = NULL;
        Q.rear = NULL;
	}
    free(p);
    return true;
}
```

**链式存储一般不会队满**



---



**双端队列**：两端都允许出队和入队的队列

重点：判断输出序列是否正确

**栈中合法的序列，双端队列中也一定合法**

![双端队列](D:\笔记\数据结构\思维导图\双端队列.png)



---



## 队列的应用

1.树的层次遍历

2.图的广度优先遍历

3.打印数据缓冲区
