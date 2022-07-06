## 栈（Stack）

定义：只允许在一端进行插入或者删除的线性表

```c++
#define MaxSize 10
typedef struct{
	ElemType data[MaxSize];
	int top;
}SqStack;

void InitSqStack(SqStack &S)
{
    S.top = - 1;
}

bool StackEmpty(Sqstack S)
{
    if(S.top = - 1)
        return true;
    else
        return false;
}
```



---



进栈：

```c++
bool Push(SqStack &S , ElemType x)
{
    if(S.top == Maxsize - 1)
		return false;
    S.data[++ S.top] = x;
    
    return true;
}
```



出栈

```c++
bool pop(SqStack &S , ElemType &x)
{
    if(S.top == - 1)
        return false;
    x = S.data[top --];
    
    return true;
}
```



读取栈顶元素

```c++
bool GetTop(SqStack S , ElemType &x)
{
    if(S.top == - 1)
        return false;
    x = S.data[top];
    
    return true;
}
```



---



### 栈的应用

#### 1.括号匹配

```c++
#define maxsize 10
typedef struct{
    char data[maxsize];
    int top;
}SqStack;

bool bracketCheck(char str[] , int length)
{
    SqStack S;
    InitStack(s);
    
    for(int i = 0 ; i < length ; i ++)
    {
        if(str[i] == '(' || str[i] == '{' || str[i == '[')
        	Push(S , str[i]);
        else
        {
            if(StackEmpty(s))
                return false;
            
            char topelem;
            Pop(S , topelem);
            if(str[i] == '(' && topelem != ')')
                return false;
            if(str[i] == '{' && topelem != '}')
                return false;
            if(str[i] == '[' && topelem != ']')
                return false;
		}
    }
    return StackEmpty(S);
}
```



---



### 2.**表达式求值**

表达式由三个部分组成：**操作数、运算符、界限符**，其中界限符反应了运算的先后顺序

逆波兰表达式 = 后缀表达式		波兰表达式 = 前缀表达式

**中缀表达式**：运算符在两个表达式中间；

中缀表达式转后缀表达式**左优先原则**：只要左边的运算符能够先计算，就优先算左边的（保证运算顺序唯一）

中缀表达式转前缀表达式**右优先原则**：只要右边的运算符能够先计算，就优先算右边的（保证运算顺序唯一）



**中缀表达式转后缀表达式：**

初始化一个栈，用于存放保存暂时不能确定运算顺序的运算符，然后从左到右处理各个元素，直到末尾，可能遇到三种情况：

1. 遇到**操作数**，直接将其加入后缀表达式

2. 遇到**界限符**，如果遇到用栈实现后缀表达式 **"("** 则直接入栈，遇到 **")"**则一次弹出栈内运算符并加入后缀表达式，直到弹出 **"("** , 并不加入后缀表达式

3. 遇到**运算符**，依次弹出栈中优先级高于或等于当前运算符的所有运算符，并加入后缀表达式，若碰到 **"("** 或栈空则停止，之后再把当前运算符入栈

4. 处理完所有字符后，将栈中剩余运算符依次弹出，并加入到后缀表达式中

   

**用栈实现中缀表达式计算：**

中缀表达式转后缀表达式 + 后缀表达式求值

```c++
#include <iostream>
#include <algorithm>
#include <stack>
#include <unordered_map>

using namespace std;

stack<int> num;
stack<char> op;

void eval()
{
    int x;
    auto right = num.top();   num.pop();
    auto left = num.top();   num.pop();
    auto c = op.top();  op.pop();
    
    if(c == '+')    x = left + right;
    else if(c == '-')    x = left - right;
    else if(c == '*')    x = left * right;
    else    x = left / right;
    
    num.push(x);
}

int main()
{
    string str;
    cin >> str;
    unordered_map<char , int> pr{{'+' , 1} , {'-' , 1} , {'*' , 2} , {'/' , 2}};
    
    for(int i = 0 ; i < str.size() ; i ++)
    {
        auto c = str[i];
        if(isdigit(c))
        {
            int x = 0 , j = i;
            while(j < str.size() && isdigit(str[j]))
                x = x * 10 + str[j ++] - '0';
            i = j - 1;                    //因为后面的字符有可能已经被读取，所以更新i的值
            num.push(x);
        }
        else if(c == '(')  op.push(c);
        else if(c == ')')
        {
            while(op.top() != '(')
                eval();
            op.pop();
        }
        else
        {
            while(op.size() && pr[op.top()] >= pr[c])
                eval();
            op.push(c);
        }
    }
    
    while(op.size())
        eval();
    cout << num.top() << endl;
    
    return 0;
}
```









✨**后缀表达式**：运算符在两个操作数后面；

后缀表达式的运算方法：从左往右扫描，每遇到一个运算符，就让运算符前面最近的两个操作数执行对应运算，合为一个操作数；

用栈实现后缀表达式：

1. 从左往右扫描下一个元素，直到处理完所有元素；
2. 若扫描到操作数则将操作数入栈，并继续进行第1步操作；否则执行第3步
3. 若扫描到运算符，则弹出栈顶元素（**先出栈的是右操作数**），执行与运算符相对应的操作，并将运算结果入栈，然后再次执行第1步；



**前缀表达式**：运算符在两个操作数前面；

用栈实现前缀表达式：

1. 从右往左扫描下一个元素，直到处理完所有元素；
2. 若扫描到操作数则将操作数入栈，并继续进行第1步操作；否则执行第3步
3. 若扫描到运算符，则弹出栈顶元素（**先出栈的式左操作数**），执行与运算符相对应的操作，并将运算结果入栈，然后再次执行第1步；

**注意：前缀表达式和后缀表达式遍历时，先出栈顺序不一样，在进行除法和减法运算时需要注意元素顺序**



----



## 3.递归

递归中函数调用的特点：最后被调用的函数最先执行结束

递归调用时，函数调用栈可称为“函数工作栈”，每进入一层递归，就将递归调用所需信息压入栈顶，每退出一层递归，就从栈顶弹出相应的信息，太多层递归可能会导致栈溢出

缺点：可能包含很多重复计算，效率低
