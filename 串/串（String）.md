## 串（String）

### 1.串的基本操作

通常以字串为操作对象

![](D:\笔记\数据结构\思维导图\串的基本操作.png)

```c++
#define maxlen 255
typedef struct{
	char ch[maxlen];
	int length;
}SString;


//取出一个子串
bool SubString(SString &Sub , SString S , int pos , int len)
{
    if(pos + len > S.length)
        return false;			//防止字串越界
    for(int i = pos ; i < pos + len ; i ++)
        Sub.ch[i - pos + 1] = S.ch[i];
    Sub.length = len;
    return true;
}


//比较字符串
int StrCompare(SString S , SString T)
{
    for(int i = 0 ; i < S.length && i < T.length ; i ++)
        if(S.ch[i] != T.ch[i])
            return S.ch[i] - T.ch[i];
    
    return S.length - T.length;
}


//定位操作
int Index(SString S , SString T)
{
    int i = 1, n = StrLength(S) , m = StrLength(T);
    SString sub;
    while(i <= n - m + 1)
    {
        SubString(sub , S , i , m);
        if(StrCompare(sub , T) != 0)
            ++i;
        else
            return i;
	}
    return 0;
}
```



字符集：
英文字符 --- ASCII

中英文 --- Unicode



---



### 2.串的存储结构

1.顺序存储

```c++
//定长分配存储
#define maxlen 255
typedef struct{
	char ch[maxlen];
	int length;
}SString;


//堆分配存储
typedef struct{
	char *ch;
    int length;
}HString;
	HString S;
	S.ch = (char*) malloc (maxlen * sizeof(char));
	S.length = 0;
```



2.链式存储

```c++
typedef struct{
	char ch;
	struct StringNode *next;
}StringNode , *String;
```

![串的链式存储1](D:\笔记\数据结构\思维导图\串的链式存储1.png)

上述方式存储密度低，优化如下:

```c++
typedef struct{
	char ch[4];
	struct StringNode *next;
}StringNode , *String;
```

![串的链式存储2](D:\笔记\数据结构\思维导图\串的链式存储2.png)



