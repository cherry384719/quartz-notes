# 这是我的js笔记

*==显示测试==*：

# 线性表

## 1.顺序表



```cpp
typedef struct
{
	int * data;
    int length;
}List;
```



>   储存元素的data和length，需事先定下capacity，且更改麻烦

#### 主要的算法：

- Init_List
- Add_List
- Insert_List
- Dele_List

C++代码示例：

```cpp
List Init_List()
{
    List L;
    L.elem = NULL;
    L.length = 0;
    return L;
}
```

```cpp
void Add_List(List& L, int e)
{
  	L.elem[length] = e;
  	length++;
}
```

```cpp
void Insert_List(List& L, int n, int e)//在第n个位置插入e
{
  	//检查合法性
  	if(n < 1 || n > L.length + 1)
    {
				cout << "ERROR!" << endl;
      	exit(-1);
    }
  	if(L.length == capacity)
    {
				cout << "can't insert any more" << endl;
      	exit(-1);
    }
  	//移位置
  	for(int i = L.length; i > n - 1; i--)
    {
				L.length[i] = L.length[i - 1];
      	L.length[n - 1] = e;
    }
  	L.length++;
}
```

```cpp
void Delet_List(List& L)
{
    if(L.elem == NULL)
    {
		return;
    }
    else
    {
        free(L.elem);
        L.elem = NULL;
        L.length = 0;
    }
}
```





## 2.链表



### （1）单链表

```cpp
typedef struct Lnode
{
    int data;
    Lnode* next;
}Lnode,*LinkList;
```



>   容易扩容，但不容易查找

#### 主要算法：

-   初始化
-   取值
-   查找
-   插入
-   删除
-   创建单链表
    -   前插法
    -   后插法



C++代码示例：

```cpp
void Init_List(LinkList & L)
{
	L = new Lnode;
    L -> next = NULL;
}
```

```cpp
void get_List(Linklist& L, int n, int e)
{
	Linklist p = L->next;
    int j = 1;
    while(p && j<n)
    {
        p = p->next;
        j++;
    }
    if(!p || j > n)
    {
        return ERROR;
    }
    e = p -> data;
    return OK;
}
```

```cpp
Linklist check_List(Linklist& L, int e)
{
    Linklist p = L -> next;
    while(p && p->data != e)
    {
		p = p->next;
    }
    return p;
}
```

```cpp
void Insert_List(Linklist& L, int n, int e)
{
	Linklist p = L->next;
    int j = 1;
    while(p && (j < n-1)) //找到第n-1个节点的指针
    {
        p = p->next;
        j++;
    }
    if(!p || j > n-1)
    {
        return ERROR;
    }
    //添加节点
    s = new Lnode;
    s -> data = e;
    s -> next = p -> next;
    p -> next = s;
    
    return OK;
}
```

```cpp
void dele_List(Linklist& L, int n)
{
    Linklist p = L -> next;
    int j = 1;
    while(p && j < n-1)
    {
		p = p -> next;
        j++;
    }
    if(!p || j > n-1)
    {
		return ERROR;
    }
    q = p -> next;
    p -> next = q -> next;
    delete q;
}
```

```cpp
//前插法
void add_front_List(Linklist& L, int n)
{
    L = new Lnode;
    L -> next = NULL;
    for(int i = 0; i < n; i++)
    {
        Linklist p = new Lnode;
        cin >> p -> data;
        p -> next = L -> next;   //Q:为什么要在L后加上 ->next ？ //搞清楚储存的结构就明白了
        L -> next = p;
    }
}

//后插法
void add_back_List(Linklist L, int n)
{
    L = new Lnode;
    L -> next = NULL;
    r = L; //尾指针
    for(int i = 0; i < n; i++)
    {
        Linklist p = new Lnode;
        cin >> p -> data;
        p -> next = NULL;
        r -> next = p;
        r = p;
    }
}

```



### （2）循环链表



### （3）双向链表



## 3. 栈

#### (1) 结构：

```cpp
typedef struct
{
    Element * base;
    Element * top;
    int stack_size;
}stack;
```

#### (2) 基本操作：

-   （1）生成空栈
-   （2）销毁栈
-   （3）判断是否为空
-   （4）入栈
-   （5）出栈
-   （6）取栈顶元素
-   （7）置栈空
-   （8）求当前栈的元素个数
-   （9）历遍元素

```cpp
void init_stack(satck& S)
{
    S.base = (ElemType *)malloc (stack_init_size * sizeof(ElemType));
    S.top = S.base;
    S.stack_size = stack_init_size;
}
```

```cpp
void ruin_stack(stack& S)
{
    
}
```

