---
title: data_structure
lang: zh-cn
category: learn
date: 2020-09-09 20:54:30
tags:
toc: true
---

从0开始
需要对C语言的指针和数组有一定的了解
全部代码示例皆为C语言
函数中所有用到的变量都声明在开头
函数名使用帕斯卡命名法
变量使用驼峰命名(我平时用匈牙利命名法)

## 数据结构概论

了解一下就好

1. 抽象数据类型(Abstruct Data Type, ADT): 数据对象(Int, String, List等)的逻辑描述方法
   ```
   ADT 线性表(List)
   Data
       创建一个存储 DataType 类型元素的线性表, 线性表的数据对象集合为{a_1, a_2, ..., a_n}, 每个元素的类型均为DataType.
       其中除第一个元素 a_1 外, 每一个元素有且只有一个前驱元素; 除了最后一个元素 a_n 外, 每一个元素有且只有一个直接后继元素.
       数据元素之间的关系是一对一(链式)的关系.
   Operation
       InitList(*L) : 初始化操作, 建立一个空的线性表 L .
       ListEmpty(L) : 判断线性表是否为空表, 若线性表为空, 返回 true , 否则返回 false .
       ClearList(*L) : 将线性表清空.
       GetElem(L, i, *e) : 将线性表 L 中的第 i 个位置元素值返回给 e .
       LocateElem(L, e) : 在线性表 L 中查找与给定值 e 相等的元素, 如果查找成功, 返回该元素在表中序号表示成功; 否则返回 -1 表示失败
       ListInsert(*L, i, e) : 在线性表 L 中第 i 个位置插入新元素 e .
       ListDelete(*L, i, *e) : 删除线性表 L 中第 i 给位置元素, 并用 e 返回其值.
       ListLength(L) : 返回线性表 L 的元素个数.
   endADT
   ```
2. 数据的逻辑结构: 有 集合, 线性, 树形, 图形 四类

## 算法

1. 特性(仅了解):
   * 输入输出: ≥0个输入, ≥1个输出
   * 有穷性: 不会死循环
   * 确定性: 相同的输入, 一样的结果
   * 可行性: 不会消耗过长的时间
2. 要求(仅了解):
   * 正确性
   * 可读性
   * 健壮性: 即使是奇怪的输入也不会导致奇怪的后果
   * 时间效率高
   * 存储量低
### 时间复杂度

表示随着输入规模增大导致耗时增加的程度
记作: \\(T(n) = O(f(n))\\) , 其中 \\(n\\) 代表问题规模, \\(f(n)\\) 是渐进时间复杂度 (高数中的同阶无穷大)
通常我们用 \\(O(f(n))\\) 来表示算法的时间复杂度, 叫作大O阶
推导大O阶:
1. 只保留 \\(T(n)\\) 中的最高阶项
2. 去除最高阶项的常数(除非最高阶是常数 1)
4. 常见的时间复杂度
   | 函数阶           | 非正式用语       | 常见于                                |
   |------------------|------------------|---------------------------------------|
   | \\(O(1)\\)       | 常数阶           | 略                                    |
   | \\(O(n)\\)       | 线性阶           | 循环结构                              |
   | \\(O(n^2)\\)     | 平方阶           | 嵌套双循环                            |
   | \\(O(n^3)\\)     | 立方阶           | 嵌套3循环                             |
   | \\(O(2^n)\\)     | 指数阶           | 嵌套n个循环(嵌套多了, 就变成指数阶了) |
   | \\(O(\log n)\\)  | 对数阶           | 循环子的增长呈指数的循环              |
   | \\(O(n\log n)\\) | \\(n\log n\\) 阶 | 对数阶+套个循环                       |
   耗费的时间从小到大依次是:
   \\(O(1)<O(\log n)<O(n)<O(n\log n)<O(n^2)<O(n^3)<O(2^n)<O(n!)<O(n^n)\\)
### 最坏情况与平均情况

算法的时间复杂度不是固定的, 依据输入的数据而决定
通常算法给出的时间复杂度都是指最坏情况

### 空间复杂度

算法所需占用的内存
记作 \\(S(n)=O(f(n))\\)
空间复杂度为 \\(O(1)\\) 时称为**原地工作**
实际情况下通常不考虑空间复杂度

## 线性表(List)

### 定义和抽象数据类型

1. 定义:
   线性表的结构长这样:
   {% asset_img List.png %}
   记作: \\((a_1, a_2, \dots, a_{i-1}, a_i, a_{i+1}, \dots, a_n)\\)
   若元素个数 n=0 时, 称为空表, i 称作**位序**
   每一个元素有1个**前驱**和**后继**, 第一个和最后一个元素除外.
   如图中 \\(a_i\\) 有前驱 \\(a_{i-1}\\) 和 后继 \\(a_{i+1}\\)
2. ADT 定义:
   ```
   ADT 线性表(List)
   Data    
       线性表的数据对象集合为{a1, a2, ......, an}, 每个元素的类型均为DataType.
       其中, 除第一个元素a1外, 每一个元素有且只有一个直接前驱元素, 除了最后一个元素an外, 每一个元素有且
       只有一个直接后继元素.
       数据元素之间的关系是一对一的关系.
   Operation    
       InitList(*L):          初始化操作, 建立一个空的线性表L.    
       ListEmpty(L):          若线性表为空, 返回true, 否则返回false.    
       ClearList(*L):         将线性表清空.    
       GetElem(L, i, *e):     将线性表L中的第i个位置元素值返回给e.    
       LocateElem(L, e):      在线性表L中查找与给定值e相等的元素, 如果查找成功, 
                              返回该元素在表中序号表示成功；否则, 返回-1表示失败
       ListInsert(*L,i,e):    在线性表L中的第i个位置插入新元素e
       ListDelete(*L,i,*e):   删除线性表L中的第i个位置元素, 并用e返回其值
       ListLength(L):         返回线性表L的元素个数
   EndADT
   ```

### 线性表的顺序存储结构

1. 结构特点:
   用一段地址连续的存储单元依次存储线性表的数据元素
   {% asset_img List-sequence-structure.png %}
2. 顺序存储结构需要三个属性:
   * 存储空间地址 data
   * 线性表的最大存储容量 MaxSize
   * 线性表的当前有效长度 length
3. 线性表的顺序存储结构代码:
   ```C++
   typedef int ElemType;         // ElemType类型根据实际情况而定, 这里假设为int
   
   typedef struct
   {
       ElemType data[MAXSIZE];   // 数组存储数据元素, 最大值为MAXSIZE
       int length;               // 线性表当前长度
   
   } SequenceList;
   ```
4. 地址的计算:
   要取得 \\(a_i\\) 的地址: `ElemType a_i = data + (i-1) * sizeof(ElemType);`
5. 线性表顺序存储结果的优缺点
   <table>
   <thead>
     <tr>
       <th>优点</th>
       <th>缺点</th>
     </tr>
   </thead>
   <tbody>
     <tr>
       <td>* 无须为表示表中元素之间的逻辑关系而增加额外的存储空间<br>* 可以快速地存取表中任一位置的数据</td>
       <td>* 插入和删除操作需要移动大量元素<br>* 当线性表长度变化较大时, 难以确定存储空间的容量<br>* 造成存储空间的"碎片"</td>
     </tr>
   </tbody>
   </table>

#### 顺序存储结构的插入与删除

1. GetElem(L, i, *e) 的实现
   返回 0 代表 OK, -1 代表 ERROR
   ```C++
   #define OK 0
   #define ERROR -1
   
   int GetElem(SequenceList list, int i, ElemType *e)
   {
       if (list.length == 0 || i < 1 || i > L.length)
           return ERROR;
   
       *e = list.data[i - 1];
       return OK;
   }
   ```
2. 插入操作
   {% asset_img List-sequence-insert.png %}
   思路:
   * 为了给插入元素腾出空间, 遍历从 i 开始至最后一个元素, 将它们向后移动一个位置(除非插入到表尾).
   * 将元素插入位置 i 处
   * 表长+1
   * 若插入位置不合理, 抛出异常
   * 若线性表已满, 抛出异常或动态增加容量
   
   实现代码如下:
   ```C++
   #define OK 0
   #define ERROR -1
   
   int ListInsert(SequenceList *list, int i, ElemType e)
   {
   
       if (list->length == MAXSIZE)                            /* 顺序线性表已满 */
           return ERROR;
       if (i < 1 || i > list->length + 1)                       /* 当i不在范围内时 */
           return ERROR;
   
       if (i <= list->length)                                  /* 若插入数据位置不在表尾 */
       {
           for (int k = list->length - 1; k >= i - 1; k--)     /*将要插入位置后数据元素向后移动一位 */
               list->data[k + 1] = list->data[k];
       }
   
       list->data[i - 1] = e;                                 /* 将新元素插入 */
       list->length++;
       return OK;
   }
   ```
3. 删除操作
   {% asset_img List-sequence-delete.png %}
   思路:
   * 为了缩回空出的地方, 遍历从 i 开始至最后一个元素, 将它们向前移动一个位置(除非删除的是表尾的元素).
   * 表长-1
   * 如果删除位置不合理, 抛出异常
   
   实现代码如下:
   ```C++
   #define OK 0
   #define ERROR -1

   int ListDelete(SequenceList *list, int i, ElemType *e)
   {
       if (list->length == 0)                     /* 线性表为空 */
           return ERROR;
   
       if (i < 1 || i > list->length)             /* 删除位置不正确 */
           return ERROR;
   
       *e = list->data[i - 1];
   
       if (i < list->length)                      /* 如果删除不是最后位置 */
       {
          for (int k = i; k < list->length; k++)  /* 将删除位置后继元素前移 */
              list->data[k - 1] = list->data[k];
       }

       list->length--;
       return OK;
   }
   ```
4. 插入与删除的时间复杂度分析
   * 如果元素要插入到最后一个位置, 或者删除最后一个元素, 时间复杂度为 \\(O(1)\\), 通常把具有这一特点的存储结构称为**随机存取结构**.
   * 如果元素要插入到第一个位置或者删除第一个元素, 意味着要移动所有的元素向后或者向前, 时间复杂度为 \\(O(n)\\)
   * 平均情况:
      由于元素插入到第 i 个位置, 或删除第 i 个元素, 需要移动 n-i 个元素. 平均执行次数是 \\(\frac{(n-1)+(n-2)+\dots+(n-n)}{n}=\frac{n^2-\frac{(1+n)n}{2}}{n}=n-\frac{1+n}{2}=\frac{n-1}{2}\\) (运用等差数列的知识进行化简)
      化为大O阶后平均时间复杂度还是 \\(O(n)\\)

### 线性表的链式存储结构

顺序线性表的缺点就是插入和删除时需要移动大量元素, 显然很耗费时间
因此有了存储形式非线性的链式线性表

1. 结构特点:
   1. 用一组任意的存储单元存储线性表的数据, 这组存储单元可以是连续的, 也可以是不连续的. 这些数据可以存在内存未被占用的任意位置
      {% asset_img List-chain-structure.png %}
   2. 存储元素的区域称为**数据域**, 存储后继位置的域称为**指针域**
      这样的一个单元称为**结点**
      因为此链表的每个结点只包含一个指针域, 所以叫作**单链表**
      {% asset_img List-chain-structure-element.png %}
      注意:
         * 为了方便记录链表信息, 可在链表在第一个结点前附设一个**头结点**, 头结点的数据域可以存储如线性表长度等信息.
         * 指向链表起始位置的指针称为**头指针**, 若链表有头结点, 则是指向头结点
         * 链表最后一个结点(称为**终端结点**)的**尾指针**为空(用 "**NULL**" 或 "**^**" 表示)
2. 线性表的单链表存储结构代码
   ```C++
   typedef struct Node
   {
      ElemType data;
      struct Node *next;
   } Node, *LinkList;
   ```

#### 单链表的读取

1. 获得链表第 i 结点上数据的算法思路:
   1. 声明一个指针 p 用于存储遍历的地址, 初始值为第一个结点地址
   2. 初始化循环子 j=1, 当 j < 1 时, 就遍历链表, 让 p 指针向后移动, 不断指向下一结点, j 累加 1
   3. 若循环到 p 为 NULL (还没到 i 结点就到单链表结尾了), 说明第 i 结点不存在, 抛出异常
   4. 若非法输入 i < 1, 抛出异常
2. 实现代码如下:
   ```C++
   int GetElem(LinkList list, int i, ElemType *e)
   {
      LinkList p = list->next;  /* 声明一指针p, 让p指向链表list的第一个结点 */
      int j = 1                 /* j为计数器 */

      for(; j < i && p; j++)    /* p不为空且计数器j还没有等于i时, 循环继续 */
          p = p->next;          /* 让p指向下一个结点 */
      
      if(!p || i < 1)
          return ERROR          /* 第i个结点不存在 */
      
      *e = p->data;             /* 取第i个结点的数据 */
      return OK;
   }
   ```
   最坏情况时间复杂度为 \\(O(n)\\) (对比顺序线性表读取始终为 \\(O(1)\\))

#### 单链表的插入与删除

1. 单链表的插入
   {% asset_img List-chain-insert.png %}
   先把结点 \\(s\\) 的后继指向 \\(p\to next\\) , 再把结点 \\(p\\) 的后继改为指向结点 \\(s\\)
   ```
   s->next = p->next;
   p->next = s;
   ```
   执行后:
   {% asset_img List-chain-insert-2.png %}
   对于单链表的表头和表尾的情况, 因为只要动前一个元素的后继指向, 所以操作是相同的
2. 单链表插入为第 i 个结点
   1. 首先需要获得链表第 i-1 结点的地址(参考上一章, 但不同的是 p 初始没有指向第一个结点)
   2. 若查找成功, 生成空结点 s, 将数据 e 赋给 s->data
   3. 进行上面讲的插入操作
   
   实现代码如下:
   ```C++
   int ListInsert(LinkList *list, int i, ElemType e)
   {
      LinkList p, s;
      int j = 1;
      
      p = *list;

      for(; j < i && p; j++)               /* 遍历寻找第i-1个结点 */
         p = p->next;

      if(!p || i < 1)
          return ERROR                     /* 第i个结点不存在 */
      
      s = (LinkList) malloc(sizeof(Node)); /* 分配内存空间(C标准函数) */
      s->data = e;
      s->next = p->next;                   /* 将p的后继结点赋给s的后继 */

      p->next = s;                         /* 将s赋值给p的后继 */
   }
   ```
3. 单链表第 i 个结点删除
   {% asset_img List-chain-delete.png %}
   思路:
   1. 首先需要获得链表第 i-1 结点的地址(参考上一章)
   2. 将结点 i-1 的后继指向结点 i+1 : `p->next = q->next`
   
   实现代码如下:
   ```C++
   int ListDelete(LinkList *list, int i, ElemType *e)
   {
      LinkList p, q;
      int j = 1;

      p = *list;
      for(; j < i && p->next; j++)    /* 遍历寻找第i-1个结点 */
         p = p->next;
      
      if(!(p->next) || i < 1)
          return ERROR;

      q = p->next;

      p->next = q->next;              /* 将q的后继赋值给p的后继 */
      *e = q->data;                   /* 将q结点中的数据给e */

      free(q);                        /* 释放内存 */
      return OK;

   }
   ```
4. 插入与删除的时间复杂度分析
   单链表的插入和删除算法, 都是由遍历查找第 i 结点和插入和删除结点这两部分构成, 时间复杂度都是 \\(O(n)\\)
   如果不知道第 i 个结点的指针位置, 单链表结构在插入和删除操作上与顺序线性表是没有太大优势的.
   但若希望从第 i 个位置, 插入 10 个结点, 对于顺序存储结构来说每次插入都需要 \\(O(n)\\); 单链表只需要在第一次时找到第 i 个位置的指针, 此时为 \\(O(n)\\) , 接下来只是简单地移动指针, 时间复杂度都是 O(1) .
   因此, **对于插入或删除数据越频繁的操作, 单链表的效率优势就越是明显**

#### 单链表的整表创建

创建单链表的过程是一个动态生成链表的过程. 即从"空表"的初始状态起, 依次建立各元素结点, 并逐个插入链表

* 头插法: 类似于插队, 始终让新结点在第一的位置
  思路:
  1. 创建空表
  2. 循环以下动作:
     1. 创建新结点, 随机生成数字赋给新结点的数据域
     2. 将头指针的值赋给新结点的后继
     3. 将新结点插入到头结点之后

  实现代码如下:
  ```C++
  /* n 为要建立的单链表长度 */
  void CreateListHead(LinkList *list, int n)
  {
     LinkList p;

     // 初始化随机数种子
     srand(time(0));

     // 先建立一个带头结点的单链表
     *list = (LinkList) malloc(sizeof(Node));
     (*list)->next= NULL;

     for(int i = 0; i < n; i++)
     {
        p = (LinkList) malloc(sizeof(Node)); // 生成新结点
        p->data = rand() % 100 + 1; // 随机生成 100 以内的数字
        p->next = (*list)->next; // 设置结点的后继

        (*list)->next = p; // 插入到表头
     }
  }
  ```
* 尾插法
  思路:
  1. 创建空表
  2. 需要一个指针来记录尾部结点(以下称 r)
  3. 循环以下动作:
     1. 创建新结点, 随机生成数字赋给新结点的数据域
     2. 将尾部结点(也就是 r)的后继设为新结点的地址
     3. 将新结点设为尾部结点(r = 新结点)

  实现代码如下:
  ```C++
  /* n 为要建立的单链表长度 */
  void CreateListTail(LinkList *list, int n)
  {
     LinkList p, r;

     // 初始化随机数种子
     srand(time(0));

     // 先建立一个带头结点的单链表
     *list = (LinkList) malloc(sizeof(Node));
     r = *list; // r 记录尾部结点的地址(这里表刚创好只有一个结点所以赋 *list)

     for(int i = 0; i < n; i++)
     {
        p = (LinkList) malloc(sizeof(Node)); // 生成新结点
        p->data = rand() % 100 + 1; // 随机生成 100 以内的数字

        r->next = p; // 将表尾结点的后继指向新结点
        r = p; // 将新结点定义为表尾结点
     }

     r->next = NULL; // 别忘了初始化表尾结点的后继
  }
  ```

  #### 单链表的整表删除

  单链表整表删除:
1. 思路:
   1. 新建一个指针 p 存储第一个结点 
   2. 向后不断遍历结点的后继并删除当前结点, 将 p 不断后移
   3. 最终将头指针置空
2. 实现代码如下:
   ```C++
   int ClearList(LinkList *list)
   {
      LinkList p = (*list)->next; // p 初始化指向第一个结点
 
      while(p) // 循环直到 p 为 NULL
      {
         LinkList q = p->next; // 需要一个临时变量来存储地址
         free(p);
         p = q; // 将下一结点的地址赋给 p 
      }
 
      // 最后将头指针设为空
      (*list)->next = NULL;
      return OK;
   }
   ```

### 顺序存储结构和单链表结构的比较

<table>
<thead>
  <tr>
    <th>存储分配方式</th>
    <th>时间性能</th>
    <th>空间性能</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>* 顺序存储结构用一段连续的存储单元依次存储线性表的数据元素<br>* 单链表采用链式存储结构, 用一组任意的存储单元存放线性表的元素</td>
    <td>* 查找<br>  * 顺序存储结构 O(1)<br>  * 单链表 O(n)<br>* 插入和删除<br>  * 顺序存储结构需要平均移动表长一半的元素, 时间为 O(n)<br>  * 单链表在找出某位置的指针(O(n))后, 插入和删除时间仅为 O(1)</td>
    <td>* 顺序存储结构需要预分配存储空间, 分大了, 浪费, 分小了易发生上溢<br>* 单链表不需要分配存储空间, 只要有就可以分配, 元素个数也不受限制</td>
  </tr>
</tbody>
</table>

* 若线性表需要频繁查找, 很少进行插入和删除操作时, 宜采用顺序存储结构. 若需要频繁插入和删除时, 宜采用单链表结构
* 当线性表中的元素个数变化较大或者根本不知道有多大时, 最好用单链表结构, 这样可以不需要考虑存储空间的大小问题. 而如果事先知道线性表的大致长度, 用顺序存储结构效率会高很多

### 静态链表

用数组储存结点的链表叫做静态链表(游标实现法).
为了方便插入数据, 通常会把数组建得大一些, 以防止空间不够而溢出

```C++
#define MAXSIZE 1000

typedef struct
{
   ElemType data;
   int cur; // 游标(Cursor), 为 0 时表示无指向
} Component, StaticLinkList[MAXSIZE];
```

{% asset_img List-static-chain-list.png %}
如图所示, 静态链表中:
1. 数组中未被使用的空间称为**备用链表**
2. 数组的第一个结点和终端结点作为特殊结点, 不存数据.
   第一个结点(下标为 0)的 cur 存放备用链表的第一个结点的下标;
   终端结点的 cur 存放第一个有数值结点的下标(相当于单链表中的头指针), 若整个链表为空时, 则为 0 .
3. 实现代码如下:
   ```C++
   // 假设 MAXSIZE = 1000
   int InitList(StaticLinkList list)
   {
      // 初始化数组下标 0~998 的游标
      for(int i = 0; i < MAXSIZE -1; i++)
          list[i].cur = i + 1;
      
      // 将终端结点(下标 999)的游标置为 0
      list[MAXSIZE - 1].cur = 0;
      return OK;
   }

   // 通过遍历游标并统计遍历次数的方法得到表长
   int ListLength(StaticLinkList list)
   {
      int length = 0;
      int lastCursor = list[MAXSIZE - 1].cur;
      while(lastCursor)
      {
         lastCursor = list[lastCursor].cur;
         length++;
      }
      return length;
   }
   ```
4. 举个例子:
   假设已经将数据存入静态链表, 比如分别存放着"甲"、"乙"、"丁"、"戊"、"己"、"庚"等数据
   {% asset_img List-static-chain-list-example.png %}

#### 静态链表的插入操作

1. 首先要解决的是: 如何用静态(预分配了内存空间)模拟动态链表的存储空间分配, 即需要时申请, 无用时释放.
   解决方案是将数组上所有未被使用的及已删除的分量用游标链成一个备用链表, 每当进行插入时, 便可以从备用链表取它的第一个分量作为待插入的新结点
   实现代码如下:
   ```C++
   // 若备用链表非空, 则返回备用链表第一个分量的下标, 否则返回 0
   int Malloc_SLL(StaticLinkList list)
   {
      // 根据静态链表定义, 当前数组第一个结点的cur存的值, 即为备用链表第一个分量的下标
      int i = list[0].cur;
   
      if(list[0].cur) // 判断是否还有下一个分量
          list[0].cur = list[i].cur; // 将它的下一个分量赋给第一个结点的cur
      return i;
   }
   ```
2. 插入为第 i 结点:
   思路:
   1. 首先从备用链表获得一个分量的游标, 然后将数据赋给此分量.
   2. 从终端结点的游标开始遍历, 直到第 i-1 号结点的下标;
   3. 然后参考单链表的插入操作
   
   实现代码如下:
   ```C++
   // i 为要插入的下标
   int ListInsert(StaticLinkList list, int i, ElemType e)
   {
      int lastCursor, spaceCursor;
      lastCursor = MAXSIZE - 1; // 首先将这个记录用变量初始化为终端结点的下标

      if (i < 1 || i > ListLength(list) + 1)
          return ERROR;

      spaceCursor = Malloc_SSL(list);
      if (spaceCursor)
      {
         // 将数据 e 赋给此分量
         list[spaceCursor].data = e;

         // 类似于单链表, 找到第 i-1 结点
         for (int j = 1; j <= i - 1; j++)
             lastCurser = list[lastCursor].cur;
         
         // 参考单链表的插入操作
         list[spaceCursor].cur = list[lastCursor].cur;
         list[lastCursor].cur = spaceCursor;
         return OK;
      }
      return ERROR;
   }
   ```

静态链表实现了在数组中, 不移动元素, 却插入了数据的操作. (但失去了随机读取的特性)

#### 静态链表的删除操作

1. 首先需要考虑把删除的空间回收到备用链表
   ```C++
   // 回收第 i 结点
   void Free_SSL(StaticLinkList list, int i)
   {
      // 采用头插法
      list[i].cur = list[0].cur;
      list[0].cur = k;
   }
   ```
2. 删除第 i 结点:
   ```C++
   int ListDelete(StaticLinkList list, int i)
   {
      int j, lastCursor;
      if (i < 1 || i > ListLength(list))
          return ERROR;
      
      lastCursor = MAXSIZE - 1;
   
      // 类似于单链表, 找到第 i-1 结点
      for (j = 1; j <= i - 1; j++)
          lastCursor = list[lastCursor].cur;
      
      j = list[lastCursor].cur; // j 重复利用
      list[lastCursor].cur = list[j].cur; // 将 i-1 结点的后继设为 i+1 (跳过第 i 结点)
      Free_SSL(list, j);
      return OK
   }
   ```

#### 静态链表优缺点

<table>
<thead>
  <tr>
    <th>优点</th>
    <th>缺点</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>* 在插入和删除操作时, 只需要修改游标, 不需要移动元素, 从而改进了在顺序存储结构中的插入和删除操作需要移动大量元素的缺点</td>
    <td>* 没有解决连续存储分配带来的表长难以确定的问题<br>* 失去了顺序存储结构随机读取的特性</td>
  </tr>
</tbody>
</table>

### 循环链表

将单链表中终端结点的后继由空指针改为指向头结点, 就使整个单链表形成一个环, 称为单循环链表, 简称循环链表(Circular Linked List)

1. 示意图:
   1. 空的循环链表(只有一个头结点):
      {% asset_img List-circular-linked-list-empty.png %}
   2. 非空的循环链表
      {% asset_img List-circular-linked-list-non-empty.png %}
2. 循环列表的特点
   循环链表和单链表的主要区别在于循环的判断条件上, 原来是判断 p->next 是否为空, 现在则是 p->next 不等于头结点, 则循环未结束.
   有头结点的单链表可以用 \\(O(1)\\) 的时间访问第一个结点, 访问到终端结点却需要 \\(O(n)\\) 的时间, 因为需要将单链表全部遍历一遍
3. 尾循环列表
   为了解决终端结点访问效率低的问题, 再改造一下这个循环链表, 不用头指针而用指向终端结点的尾指针来表示链表.
   {% asset_img List-circular-linked-list-tail.png %}
4. 尾循环列表的特点
   终端结点用尾指针 `rear` 表示, 查找终端结点时间为 \\(O(1)\\) , 第一个结点是 `rear->next->next` , 时间也为 \\(O(1)\\)
5. 合并多个尾循环列表
   将尾指针分别是 `rearA` 和 `rearB` 的两个循环链表合并成一个表:
   {% asset_img List-circular-linked-list-merge.png %}
   合并后:
   {% asset_img List-circular-linked-list-merge-2.png %}
   如图所示, 具体操作为:
   1. 将 B 表的 `rearB` 指向 A 表的头结点
   2. 将 A 表的 `rearA` 指向 B 表的第一个结点, 同时释放 B 表的头结点
   
   实现代码为:
   ```C++
   CircularLinkList p = rearA->next; // 保存 A 表的头结点
   rearA->next = rearB->next->next; // 将 A 表的头结点替换为 B 表的第一个结点
   free(rearB->next); // 释放 B 表的头结点(不再需要)
   rearB->next = p; // 将 B 表尾指针指向 A 表的头结点
   ```

### 双向链表

双向链表(Double linked list)是在单链表的每个结点中, 再设置一个指向其前驱结点的指针域. 双向链表中的结点都有两个指针域, 一个指向后继, 一个指向后驱

```C++
typedef struct DuLinkNode
{
   ElemType data;
   struct DuLinkNode *prior; // 前驱指针
   struct DuLinkNode *next; // 后继指针
} DuLinkNode, *DuLinkList;
```

这里我们讨论循环+双向列表的情况

1. 示意图:
   1. 空的循环双向链表:
      {% asset_img List-circular-double-linked-list-empty.png %}
   2. 非空的循环链表
      {% asset_img List-circular-double-linked-list-non-empty.png %}
2. 双向链表插入结点
   {% asset_img List-circular-double-linked-list-insert.png %}
   思路: 先搞定 s 的前驱和后继, 再搞定后结点的前驱, 最后解决前结点的后继
   实现代码如下:
   ```C++
   s->prior = p; // 1. s 的前驱是 p
   s->next = p->next; // 2. s 的后继是 p->next
   p->next->prior = s; // 3. 后结点的前驱是 s
   p->next = s; // 3. 前结点的后继是 s
   ```
3. 双向列表删除结点
   思路: 先搞定前结点的后继, 再搞定后结点的前驱, 最后释放 p 的空间
   ```C++
   p->prior->next = p->next;
   p->next->prior = p->prior;
   free(p)
   ```

## 栈与队列

### 栈的定义

栈(Stack)是限定仅在表尾进行插入和删除操作的线性表

**栈顶(top)** 允许数据插入和删除, 另一端叫**栈底(bottom)**
栈又被称为 **后进先出(Last In, First Out)** 的线性表, 简称**LIFO结构**

* 栈的插入操作称作**压栈、进栈**或**入栈**
  {% asset_img Stack-push.png %}
* 栈的删除操作称作**弹栈**或**出栈**
  {% asset_img Stack-pop.png %}

### 栈的抽象数据类型

```ADT
ADT 栈(Stack)
Data
    同线性表. 元素具有相同的类型, 相邻元素具有前驱和后继关系
Operation
    InitStack(*S) :    初始化操作, 建立一个空栈 S
    DestroyStack(*S) : 将栈销毁
    ClearStack(*S) :   将栈清空
    StackEmpty(S) :    若栈为空, 返回 true ; 否则返回 false
    GetTop(S, *e) :    用 e 返回栈 S 的栈顶元素
    Push(*S, e) :      插入新元素 e 到栈 S 中并成为栈顶元素. 又称: 压栈, 进栈, 入栈
    Pop(*S, *e) :      删除栈 S 中栈顶元素, 并用 e 返回其值. 又称: 弹栈, 出栈
    StackLength(S) :   返回栈 S 的元素个数
endADT
```

### 栈的顺序存储结构及实现

1. 栈的顺序存储结构(Stack Sequence)
   {% asset_img Stack-top-example.png %}
   栈顶指针**top**指向当前栈顶部元素的地址. 它在空栈时为 -1 , 存在一个元素时为 0 .
   ```C++
   // SElemType类型根据实际情况而定, 这里假设为 int
   typedef int SElemType
   typedef struct
   {
      SElemType data[MAXSIZE];
      int top; // 栈顶指针(下标)
   } SqStack;
   ```
2. 顺序栈的进栈
   {% asset_img Stack-sequence-push.png %}
   ```C++
   // 插入元素 e 为新的栈顶元素
   int Push(SqStack *S, SElemType e)
   {
      if (S->top == MAXSIZE - 1) // 栈满
          return ERROR;
      
      S->top++; // 栈顶指针+1
      S->data[S->top] = e; // 将新加入的元素赋给栈顶空间
      // 以上两句可以合并为 S->data[++S->top] = e;
      return OK;
   }
   ```
3. 顺序栈的出栈
   ```C++
   // 删除栈顶元素, 用 e 返回其值
   int Pop(SqStack *S, SElemType *e)
   {
      if (S->top == -1) // 若栈为空
          return ERROR;
      
      *e = S->data[S->top]; // 将要删除的栈顶元素赋值给 e
      S->top--; // 栈顶指针-1
      // 以上两句可以合并为 *e = S->data[S->top--];
      return OK;
   }
   ```
   值得一提的是, 出栈并没有清除数据, 因为栈在创建时即分配了固定的内存空间, 没有必要清理数据.
4. 时间复杂度分析
   入栈和出栈的时间复杂度均是 \\(O(1)\\)

#### 两栈共享空间

1. 两栈共享空间
   将一个数组分为两个顶部相连的栈, 一个栈的栈底为数组的始端, 另一个栈的栈顶为数组的末端
   {% asset_img Stack-sequence-double.png %}
   实现代码如下:
   ```C++
   typedef struct
   {
      SElemType data[MAXSIZE]
      int top1; // 栈 1 栈顶指针
      int top2; // 栈 2 栈顶指针
   } SqDoubleStack;
   ```
2. 共享栈的进栈
   对于两栈共享空间的 Push() 方法, 除了要插入元素值参数外, 还需要有一个判断是栈 1 还是栈 2 的栈号参数 **stackNumber**
   ```C++
   int Push(SqDoubleStack *S, SElemType e, int stackNumber)
   {
      if (S->top1 + 1 == S->top2) // 两个栈顶相挨, 说明栈已满
          return ERROR;
      
      if (stackNumber == 1)
          S->data[++S->top1] = e; // 栈 1 同普通栈
      else if (stackNumber == 2)
          S->data[--S->top2] = e; // 栈 2 要先 top2-1 后给数组元素赋值
      
      return OK;
   }
   ```
2. 共享栈的出栈
   对于两栈共享空间的 Pop() 方法, 同样需要增加判断栈 1 还是栈 2 的参数 **stackNumber**
   ```C++
   int Pop(SqDoubleStack *S, SElemType *e, int stackNumber)
   {
      if (stackNumber == 1)
      {
          if (S->top1 == -1) // 若栈 1 是空栈
              return ERROE;
         
          *e = S->data[S->top1--]; // 栈 1 栈顶元素出栈
      }
      else if (stackNumber == 2)
      {
          if (S->top2 == MAXSIZE) // 若栈 2 已经是空栈, 栈 2 的栈底是 MAXSIZE
              return ERROR;
         
          *e = S->data[S->top2++]; // 栈 2 栈顶元素出栈
      }

      return OK;
   }
   ```

### 栈的作用

栈的引入简化了程序设计的问题, 划分了不同的关注层次, 使得思考范围缩小, 更加聚焦于要解决的问题核心. 反之, 像数组等, 因为要分散精力去考虑数组的下标增减等细节问题, 反而掩盖了问题的本质.

### 栈的应用

#### 递归

1. **斐波那契数列**实现
   1. 斐波那契数列介绍
      | 月数     | 1 | 2 | 3 | 4 | 5 | 6 | 7  | 8  | 9  | 10 | 11 | 12  |
      |----------|---|---|---|---|---|---|----|----|----|----|----|-----|
      | 兔子对数 | 1 | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 | 55 | 89 | 144 |
      {% asset_img Stack-Fibonacci-sequence.png %}
      > 如图, 编号 ① 的一对兔子经过六个月变成8对兔子

      数学定义:
      \\(F(n) = \begin{cases} 0 & n=0 \\\ 1 & n=1 \\\ F(n-1)+F(n-2) & n>1 \end{cases}\\)
      发现规律了吗? 斐波那契数列第 i 个月(i > 1, 第零月算0)的值等于前两个月的和
   2. 打印前 40 位的斐波那契数列数, 实现代码如下:
      ```C++
      #include "stdio.h"

      int Fbi(int i) // 斐波那契的递归函数
      {
         if (i < 2)
             return i == 0 ? 0 : 1;
         return Fbi(i-1) + Fbi(i-2); // 递归调用(即调用自己)
      }

      int main()
      {
         int i;
         int a[40];

         // 方法1 使用迭代来实现斐波那契数列
         printf("迭代显示斐波那契数列: \n");
         a[0]=0;
         a[1]=1;
         printf("&d ", a[0]);
         printf("%d ", a[1]);

         for(i = 2; i < 40; i++)
         {
             a[i] = a[i-1] + a[i-2]
             printf("%d ", a[i]);
         }
         printf("\n");

         // 方法2 使用迭代来实现斐波那契数列
         printf("递归显示斐波那契数列: \n");
         for(i = 0; i < 40; i++)
             printf("%d ", Fbi(i));
         
         return 0;
      }
      ```
      Fbi(i) 函数当 i=5 的执行过程(分析递归的方法):
      {% asset_img Stack-fbi-function-5.png %}
2. 递归和迭代的区别
   | 迭代                               | 递归                                                                                                                       |
   |------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
   | 循环结构                           | 选择结构                                                                                                                   |
   | 不需要反复调用函数和占用额外的内存 | 使程序结构更清晰简洁, 更容易让人理解, 从而减少读懂代码的时间. 但是大陆的递归调用会建立函数的副本, 从而耗费大量的时间和内存 |
   
   你可能注意到本章似乎并没有提及栈的内容, 为什么呢?
   在前行阶段, 对于每一层递归, 函数的局部变量、参数值以及返回地址都被压入栈中. 在退回阶段, 位于栈顶的局部变量、参数值和返回地址被弹出, 用于返回调用层次中执行代码的其余部分, 也就是恢复了调用的状态.

#### 后缀表达式

我们平时描述数字表达式用的是中缀表达法, 但计算机解析它需要递归, 从而耗费大量的资源
后缀表达式能够解决这个问题

1. 后缀表示法(逆波兰表示法):
   逆波兰表达式去掉括号也不会有歧义
   9+(3-1)\*3+10÷2 = 9 (3 1-) 3\*+ 10 2/+ = 9 3 1- 3\*+ 10 2/+
2. 后缀表达式的计算方法
   规则: 从左到右遍历表达式的每个数字和符号, 遇到数字就进栈, 遇到符号就将两个数字出栈, 进行运算, 再将运算结果进栈. 重复以上步骤直到最终获得结果.
   以 9+(3-1)\*3+10÷2 = 9 3 1- 3\*+ 10 2/+ 为例
   1. 初始化一个空栈用来对要运算的数字进出使用. 后缀表达式中前三个都是数字, 所以9, 3, 1 进栈, 如图
      {% asset_img Stack-arithmetic-express-1.png %}
   2. 接下来是减号"-", 所以将栈中的 1 出栈作为减数, 3 出栈作为被减数, 并运算 3-2 , 再将得到的结果 2 进栈, 如图
      {% asset_img Stack-arithmetic-express-2.png %}
   3. 接着是数字 3 进栈
   4. ...
   5. 总之最后得到结果 20, 出栈变为空栈
3. 中缀表达式转后缀表达式
   9+(3-1)\*3+10÷2 ----> 9 3 1- 3\*+ 10 2/+
   规则:
   从左到右遍历中缀表达式的每个数字和符号, 若是数字就输出;
   若是符号, 则判断其优先级不高于栈顶符号(遵循乘除优先加减, 左边高于右边)则将栈内元素依次出栈并输出, 并将当前符号进栈, 直到输出完整的后缀表达式.
   若是右括号, 则栈内元素依次出栈直到对应的左括号出栈
   1. 初始化一个空栈, 用于存储读取的符号.
   2. 第一个字符是9, 输出9, 后面是符号"+", 进栈.
      {% asset_img Stack-arithmetic-express-convert-1.png %}
   3. 第三个字符是"(", 进栈. 后面是数字3, 进栈. 接着是"-", 进栈
      {% asset_img Stack-arithmetic-express-convert-2.png %}
   4. 接下来是数字1, 输出. 后面是符号")", 所以栈顶依次出栈, 直到"("出栈为止.
      接着是符号"\*", 因为此时的栈顶符号为"+"号, 优先级低于"\*", 因此不输出, "*"进栈. 接着说数字3, 输出.
      {% asset_img Stack-arithmetic-express-convert-3.png %}
   5. ...
   6. 读到最后, 将栈中元素全部出栈并输出. 得到后缀表达式结果: 9 3 1- 3\*+ 10 2/+

### 队列的定义

队列(queue)是只允许在一端进行插入操作, 而在另一端进行删除操作的线性表

队列是一组先进先出(First In First Out)的线性表, 简称FIFO. 允许插入的一端称为队尾, 允许删除的一端称为队头.
{% asset_img Queue-example.png %}

### 队列的抽象数据类型

```
ADT 队列(Queue)
Data
    同线性表. 元素具有相同的类型, 相邻元素具有前驱和后续关系.
Operation
    InitQueue(*Q) :   初始化操作, 建立一个空队列Q
    DestroyQueue(*Q): 若队列Q存在, 则销毁它
    ClearQueue(*Q) :  将队列Q清空
    QueueEmpty(Q) : 若队列为空, 返回true, 否则返回false
    GetHead(Q, *e) :  用e返回队列Q的队头元素
    EnQueue(*Q, e) :  插入新元素e到队列Q中并成为队尾元素
    DeQueue(*Q, *e) : 删除队列Q中队头元素, 并用e返回其值
    QueueLength(Q) :  返回队列Q的元素个数
endADT
```

### 循环队列

1. 队列顺序存储的不足
   入列操作就是在队尾追加一个元素, 不需要移动任何元素, 时间复杂度为 \\(O(1)\\)
   出列操作, 队列中的所有元素都得向前移动, 以保证队列的队头不为空, 此时时间复杂度为 \\(O(1)\\)
   为了避免当只有一个元素时, 队头和队尾重合使处理变得麻烦, 引入俩个指针: 指向队头元素的 **front** 和指向队尾元素的**下一个位置**的 **rear**. 当 front 等于 rear 时, 队列为空.
   {% asset_img Queue-front-rear.png %}
2. 循环队列定义
   队列的这种头尾相接的顺序存储结构称为循环队列
   (即指针 rear 可以在指针 front 的前面)
   {% asset_img Queue-circular-example.png %}
   1. 队列已满判断
      如果我们再入队两个元素:
      {% asset_img Queue-circular-full.png %}
      如何判断此时的队列已满呢?
      * 方法1: 设置一个标志变量 **flag** , 当 front==rear且 flag=0 时队列空; 当 front==rear 且 flag=1 时队列满.
      * 方法2: 当队列空时, 条件就是 front=rear, 当队列满时, 认为 **(rear+1)%MAXSIZE == front** 即为队列满
        (取模MAXSIZE相当于让 rear>4 时自动-4).
        也就是说, 数组中保留一个空闲单元, **不允许右边的情况出现**.
        {% asset_img Queue-circular-check-full.png %}
      
      **一般用第二种方法**
   1. 队列实际长度的计算
      * 当 rear 在 front 之后(即 front < rear), 队列长度为 rear - front
      * 当 rear 在 front 之前(即 front > rear), 队列长度分为两段, 一段是 0 + rear, 另一段是 MAXSIZE - front. 整合起来就是: rear - front + MAXSIZE
      
      结合两种情况, 得到通用的公式: **(rear - front + MAXSIZE)%MAXSIZE**
3. 循环队列的具体实现
   * 循环队列顺序存储结构
     ```C++
     typedef struct
     {
        QElemType data[MAXSIZE]
        int front;
        int rear;
     } SqQueue;
     ```
   * 循环队列初始化 InitQueue(*Q)
     ```C++
     int InitQueue(SqQueue *Q)
     {
        Q->front = 0;
        Q->rear = 0;
        return OK;
     }
     ```
   * 循环队列求长度 QueueLength(Q)
     ```C++
     int QueueLength(SqQueue Q)
     {
         return (Q.rear - Q.front + MAXSIZE) % MAXSIZE;
     }
     ```
   * 循环队列入队操作 EnQueue(*Q, e)
     ```C++
     int EnQueue(SqQueue *Q, QElemType e)
     {
         if ((Q->rear + 1) % MAXSIZE == Q->front) // 队列满的判断
             return ERROR;
         
         Q->data[Q->rear] = e; // 将e赋给队尾
         Q->rear = (Q->rear + 1) % MAXSIZE; // rear后移一位置, 若在最后则转到头部
         return OK;
     }
     ```
   * 循环队列出列操作 DeQueue(*Q, *e)
     ```C++
     int DeQueue(SqQueue *Q, QElemType *e)
     {
         if (Q->front == Q->rear) // 队列空的判断
             return ERROR;
         
         *e = Q->data[Q->front]; // 将队头元素赋给e
         Q->front = (Q->front + 1) % MAXSIZE; // front指针后移一位置, 若在最后则转到数组头部

         return OK;
     }
     ```

### 队列的链式存储结构及实现

队列的链式存储结构即为**只能尾进头出的单链表**, 简称**链队列**.

{% asset_img Queue-linked-example.png %}

空队列时, front 和 rear 都指向头结点

{% asset_img Queue-linked-empty-example.png %}

1. 链队列的结构
   ```C++
   // QElemType类型根据实际情况而定, 这里假设为int
   typedef int QElemType;
   
   typedef struct QNode // 结点结构
   {
      QElemType data;
      struct QNode *next;
   } QNode, *QueuePtr;
   
   typedef struct // 队列的链表结构
   {
      QueuePtr front, rear; // 队头, 队尾指针
   } LinkQueue;
   ```
2. 入队操作
   入队操作就是在链表尾部插入结点
   {% asset_img Queue-linked-insert.png %}
   实现代码如下:
   ```C++
   int EnQueue(LinkQueue *Q, QElemType e)
   {
      QueuePtr newNode = (QueuePtr) malloc(sizeof(QNode));
      
      if(!newNode) // 存储分配失败
          exit(OVERFLOW);
      
      newNode->data = e;
      newNode->next = NULL;

      Q->rear->next = newNode; // 将新阶段设置为原队尾结点后继
      Q->rear = newNode; // 将新结点设置为队尾结点
      return OK;
   }
   ```
3. 出队操作
   出队操作时就是将第一个结点(头结点的后继)结点出队, 再将头结点的后继改为它后面的结点.
   若链表除头结点外只剩一个元素时, 则需将 rear 指向头结点
   {% asset_img Queue-linked-delete.png %}
   实现代码如下:
   ```C++
   int DeQueue(LinkQueue *Q, QElemType *e)
   {
       QueuePtr p;

       if (Q->front == Q->rear) // 若队列为空
           return ERROR;
       
       p = Q->front->next; // 记下欲删除的结点地址
       *e = p->data; // 将欲删除结点的值赋给e

       Q->front->next = p->next; // 将头结点后继改为欲删除结点的后继

       if (Q->rear == p) // 若队头就是队尾, 则将rear指向头结点
           Q->rear = Q->front;
       
       free(p);
       return OK;
   }
   ```

### 循环队列与链队列比较

* 时间上, 基本操作都为 \\(O(1)\\) , 不过循环队列是事先分配好空间, 使用期间不释放; 而对于链队列, 每次申请和释放结点也会有额外的时间开销, 如果入队出队频繁, 则两者还是有细微差异.
* 空间上, 循环队列必须有一个固定的长度, 所以就有空间浪费的问题. 而链队列不存在这个问题, 尽管它每个结点额外需要一个指针域, 会产生一些空间上的开销, 但也可以接受. 所以在空间上, 链队列更加灵活.
* 在可以确定队列长度最大值的情况下, 建议用循环队列, 如果无法预估队列的长度时, 则用链队列.

## 串

### 串的定义

**串(string)** 是由零个或多个字符组成的有限序列, 又名叫**字符串**

一般记为 \\(s="a_1a_2\dots a_n"(n\geqslant 0)\\) , \\(s\\) 是串的名称, 用双引号(或单引号)括起来的是串的值. \\(a_i(1\leqslant i\leqslant n\\) 可以是字母、数字或其他字符.

**空串(nullstring)**, 它的长度为零, 可以直接用两双引号 "" 表示, 也可以用希腊字母 Φ 来表示.

**空格串**, 只包含空格的串. 可以不止一个空格

串中任意个数的连续字符组成的串叫**子串**. 相应地, 包含子串的串称为**主串**.

### 串的比较

串的比较是通过组成串的字符之间的编码来进行的, 字符的编码指字符在对应字符集(如ASCII)中的序号.

ASCII编码由7位二进制表示一个字符, 共能表示128个字符;
扩展ASCII码由8位二进制数表示一个字符, 共能表示256个字符

Unicode编码由16位二进制数表示一个字符, 共能表示 \\(2^16\\) 个字符(约6.5万), 为了与ASCII码兼容, Unicode的前256个字符与ASCII码完全相同

在C语言中比较两个串相等, 必须是串的长度和对应字符都相等.
两个串不相等时, 若对应字符不同, 比对应字符在ASCII码表的编码号. 若对应字符相同, 比两个串的长度.

### 串的抽象数据类型

线性表关注的是单个元素的操作
串关注的是多个元素的操作, 如查找子串位置, 得到指定位置子串, 替换子串等操作

串的抽象数据类型
```C++
ADT 串(string)
Data
    串中单个元素仅由一个字符组成, 相邻元素具有前驱和后继关系.
Operation
    StrAssign(T, *chars) : 生成一个值为*chars的串T
    StrCopy(T, S) : 若串S存在, 由串S复制得串T
    
    ClearString(S) : 若串S存在, 将串清空
    StringEmpty(S) : 若串S为空, 返回true, 否则返回false
    
    StrLength(S) : 返回串S的长度
    
    StrCompare(S, T) : 若 S>T, 返回值 >0; 若 S=0 返回 0; 若 S<T, 返回值 <0
    
    Concat(T, S1, S2) : 用T返回由S1和S2联接而成的新串
    
    SubString(Sub, S, pos, len) : 若串S存在, 且 1 ≤ pos ≤ StrLength(S) , 0 ≤ len ≤ StrLength(S) - pos + 1 .
                                     返回截取从pos起长度为len的子串Sub
    
    Index(S, T, pos) : 若串S和串T存在, T是非空串, 且 1 ≤ pos ≤ StrLength(S) . 若主串S中存在和串T相同的子串,
                       则返回它在主串S中第pos个字符起第一次出现的位置, 否则返回 -1
    
    Replace(S, T, V) : 若串S、T、V存在, 且T是非空串. 用V替换主串S中出现的所有与T相等的子串.
    
    StrInsert(S, pos, T) : 若串S和T存在, 且 1 ≤ pos ≤ StrLength(S) + 1 . 在串S第pos个字符之前插入串T
    
    StrDelete(S, pos, len) : 若串S存在, 且 1 ≤ pos ≤ StrLength(S) - len + 1 . 从串S中删除以第pos个字符起长度为len的子串
endADT
```

### 串的存储结构

1. 串的顺序存储结构
   串的顺序存储结构是用一组地址连续的存储单元来存储串中的字符序列. 一般用数组来定义.
   
   C语言中, 串的最后用"\0"来表示串值的终结.
   {% asset_img String-sequence-example.png %}
2. 串的链式存储结构
   由于串结构的特殊性, 结构中每个元素数据是一个字符, 如果也简单地一个结点对应一个字符, 就会存在很大的空间浪费.
   因此, 一个结点也可以考虑存放多个字符, 最后一个结点若是未被占满时, 可以用"#"或其他字符填充.
   {% asset_img String-linked-example.png %}
   串的链式存储结构除了在连接串与串操作时有一定方便之外, 总的来说不如顺序存储灵活, 性能也不如顺序存储结构好

### 朴素的模式匹配算法

**子串的定位操作通常称做串的模式匹配(Pattern matching)**

1. 举个例子: 从主串 S="goodgoogle" 中找到 T="google" 这个子串的位置

   (图中, 竖直线表示相等, 闪电状弯折表示不等.)
   1. 主串 S 第一位开始, S 与 T 前三个字母都匹配成功, 但 S 第四个字符是 'd' 而子串 T 的是 'g', 因此第一位匹配失败.
      {% asset_img String-naive-pattern-match-1.png %}
   2. 从主串 S 第二位开始, 首字母是 'o', 与 T 的 'g' 不同, 匹配失败
      {% asset_img String-naive-pattern-match-2.png %}
   3. 从主串 S 第三位开始, 首字母还是 'o', 与 T 的 'g' 不同, 匹配失败
      {% asset_img String-naive-pattern-match-3.png %}
   4. 从主串 S 第四位开始, s首字母是 'd', 与 T 的 'g' 不同, 匹配失败
      {% asset_img String-naive-pattern-match-4.png %}
   5. 从主串 S 第五位开始, 6 个字母全匹配, 匹配成功
      {% asset_img String-naive-pattern-match-5.png %}
2. 操作 `Index(S, T, pos)` 的实现算法
   思路: 主串 S 从 pos 之后, 不断截取长度为 Strlength(T) 的子串与 T 进行比较
   ```C++
   int Index(String S, String T, int pos)
   {
       int n, m, i;
       String sub;
       
       if(pos > 0)
       {
           n = StrLength(S); // 得到主串S长度
           m = Strlength(T); // 得到子串T长度
           for(i = pos; i <= n - m + 1; i++)
           {
               SubString(sub, S, i, m);
               if (StrCompare(sub, T) == 0) // 如果两串相等
                   return i;
           }
       }
       return -1; // 若无子串与T相等, 返回 -1
   }
   ```
   不用StrLength(), SubString(), StrCompare() 的实现
   ```C++
   int Index(String S, String T)
   {
       int i, j; // i, j 分别记录循环在 S, T 比较的下标
       for (i = 1, j = 1; i <= S.[0] && j <= T.[0];)
       {
           if (S[i] == T[j])
           {
               i++;
               j++;
           }
           else // 如果比较失败
           {
               i = i - j + 2; // i 回溯并+1
               j = 1; // j 回溯
           }
           if (j = T.[0])
               return (i - T.[0];
           else
               return 0;
       }
   }
   ```
3. 时间复杂度分析:
   最坏情况下, 每次不成功的匹配都发生在串 T 的最后一个字符
   设 S 和 T 长度分别为 n=32, m=8
   如主串 S="00000000000000000000000000000001", 而要匹配的子串 T="00000001",
   所以最坏情况时间复杂度为 m(n-m+1)=O(m(n-m))

### KMP模式匹配算法

KMP (Knuth, Morris, Pratt 三人发现)
KMP算法的改进之处在于主串的 i 指针不用回溯, 而是利用之前"匹配程度"(以 j 指针来反应)将匹配串T向右滑动尽可能远的距离后继续比较.

如何决定滑动的距离呢?
定义一个数组 next[j] 表示当子串T中第j个字符与主串第i个字符不等时, 下一次比较的位置为 T[next[j]]
{% asset_img String-KMP.png %}
如图中 next[6] = 3, 则将 T[3]与 S[6] 比较

在此贴上 next[i] 的定义:
\\(next[j]=\begin{cases} 0, 当 j=1 时 \\\ Max\\{k|1<k<j, 且 "a_1 a_2 \dots a_{k=1}=a_{j-k+1} a_{j-k+1}\dots a_{j-1}"\\} \\\ 1, 其他情况\end{cases}\\)

具体实现代码:
```C++
void get_next(String T, int *next)
{
   int j, k;
   j = 1;
   k = 0;
   next[1] = 0;
   while(j < T.[0]) // 此处T[0]表示串T的长度
   {
      if (k == 0 || T[j] == T[k]) // 
      {
          j++;
          k++;
          next[j] = k;
      }
      else
          k = next[k]; // k 值回溯, 同样利用了KMP, 利用之前已经算好的next[k]
   }
}

int Index_KMP(String S, String T, int pos)
{
    int i = pos;
    int j = 1;
    int next[255];
    get_next(T, next);
    while (i <= S[0] && j <= T[0])
    {
        if (j == 0 || S[i] == T[i])
        {
            i++;
            j++;
        }
        else
            j = next[i];
    }
    if (j > T[0])
        return i - T[0];
    else
        return 0;
}
```
1. 时间复杂度分析
TODO

## 树

### 树的定义

树(Tree) 是 n (n≥0) 个结点的有限集, n=0 时称为**空树**.
在任意一颗非空树中:
1. 有且仅有一个特定的根(Root)结点
2. 当 n>1 时, 其余结点可分为 m (m>0) 个互不相交的有限集 \\(T_1, T_2, \dots, T_m\\) ,
   其中每一个集合本身又是一棵树, 并且称为根的**子树**
   {% asset_img Tree-example-1.png %}
   如下图, 子树 \\(T_1\\) 和 \\(T_2\\) 是根结点 A 的子树
   (D、G、H、I是以B为根结点的子树, E、J是以C为根结点的子树)
   {% asset_img Tree-example-2.png %}
3. 子数之间一定是互不相交的, 下图的两个结构就不符合树的定义
   {% asset_img Tree-non-tree.png %}

2. 结点的分类
   树的结点包含数据元素和若干指向其子树的分支.
   结点的子树数量称为结点的**度(Degree)**.
   度为0的结点称为**叶结点**或**终端结点**;
   度不为0的结点称为**分支结点**或**非终端结点**, 非根结点的分支结点也叫**内部结点**.
   树的度为树内各结点的度的最大值.
   举个例子:
   {% asset_img Tree-node-example.png %}
   此树的度为3
3. 结点间关系
   孩子(Child), 双亲(Parent), 兄弟(Sibling)
   {% asset_img Tree-node-relationship-example.png %}
4. 树的其他相关概念
   1. 结点的层次(Level)
      一个 **深度(Depth)** 或 **高度** 为4的树
      {% asset_img Tree-depth-example.png %}
   2. 如果将树中结点的各子树看出从左至右是有次序的, 不能互换的, 则称该树为**有序树**, 否则称为无序树.
   3. **森林(Forest)** 是 m (m≥0) 课互不相交的树的集合. 对树中每个结点而言, 其子树的集合即为森林.

### 树的抽象数据类型

```
ADT 树(tree)
Data
    树是由一个根结点和若干课子树构成, 树中结点具有相同数据类型及层次关系
Operation
    InitTree(*T) : 构造空树T
    DestroyTree(*T) : 摧毁树T
    CreateTree(*T, definition) : 按definition中给出的树的定义来构造树
    ClearTree(*T) : 若树T存在, 则将树T清空
    TreeEmpty(T) : 若T为空树, 返回true, 否则返回false
    TreeDepth(T) : 返回树T的深度
    Root(T) : 返回树T的根结点
    Value(T, cur_e) : cur_e是树T中一个结点, 返回此结点的数据
    Assign(T, cur_e, value) : 给树T的结点cur_e赋值为value
    Parent(T, cur_e) : 若cur_e是树T的非根结点, 返回它的双亲, 否则返回空
    LeftChild(T, cur_e) : 若cur_e是树T的非叶结点, 则返回它最左边的孩子, 否则返回空
    RightSibling(T, cur_e) : 若cur_e有右兄弟, 则返回它的右兄弟, 否则返回空
    InsertChild(*T, *p, i, c) : p指向树T的某个结点, i为所指结点p的度加上1, 非空树c与T不相交
                                操作结果为插入树c为树T中p所指结点的第i颗子树
    DeleteChile(*T, *p, i) : 其中p指向树T的某个结点, i为所指结点p的度, 操作结果为删除树T中p所指结点的第i颗子树
endADT
```

### 树的存储结构

1. 双亲表示法
   假设以一组连续空间存储树的结点, 同时在每个结点中, 附设一个治时期指示其双亲结点在数组中的位置. 每个结点除了知道自己是谁以外, 还知道它的双亲在哪里
   ```C++
   #define MAX_TREE_SIZE 100

   typedef int TElemType; // 假设储存的是整型

   typedef struct PTNode
   {
       TElemType data; // 结点数据
       int parent;  // 双亲位置
   } PTNode; // 结点结构

   typedef struct
   {
       PTNode nodes[MAX_TREE_SIZE]; // 结点数组
       int r, n; // 根的位置和结点数
   } PTree;
   ```
   由于根结点没有双亲, 约定根结点的 parent 设为 -1
   {% asset_img Tree-parent-representation.png %}
   改进:
   * 增加一个存储最左边孩子的 **长子域(firstChild)**, 如果结点没有孩子, 就设为 -1
     {% asset_img Tree-parent-representation-firstchild.png %}
   * 增加一个存储右兄弟的域(rightsib), 如果结点没有右兄弟, 就设为 -1
     {% asset_img Tree-parent-representation-rightsib.png %}
2. 孩子表示法
   由于树中每个结点可能有多颗子树, 可以考虑多重链表, 即**每个指针指向一棵子树的根结点**, 这种方法叫**多重链表表示法**.
   * 方案一: 指针域的个数等于树的度
     TODO: 补充不重要内容
   * 方案二: 按需分配空间
     TODO: 补充不重要内容
     孩子表示法: 设置两种结构, 一个是孩子链表的孩子结点, 另一个是表头数组的表头结点. 把每个结点的孩子结点以单链表形式排列起来
     {% asset_img Tree-child-representation.png %}
     ```C++
     #define MAX_TREE_SIZE 100
     typedef int TElemType;
     typedef struct CTNode // 孩子结点
     {
        int child; // 存储该孩子在表头数组中的下标
        struct CTNode *next; // 指向某结点的下一个孩子结点的指针
     } *ChildPtr;
     
     typedef struct // 表头结点
     {
        TElemType data;
        ChildPtr firstchild;
     } CTBox;
     
     typedef struct
     {
        CTBox nodes[MAX_TREE_SIZE]; // 表头数组
        int r, n; // 根的位置和结点数
     } CTree;
     ```
     这样的结构对于要查找某个结点的某个孩子, 或者某结点的兄弟, 只需要查找这个结点的孩子链表即可. 但要知道某结点的双亲是谁比较麻烦, 需要遍历整棵树才行.
     双亲孩子表示法:
     在孩子表示法的表头数组中, 元素增加一个记录双亲的域.
3. 孩子兄弟表示法
   设置两个指针, 分别指向该结点的**第一个孩子**和**右兄弟**
   {% asset_img Tree-child-brother-representation.png %}
   ```C++
   typedef struct CSNode
   {
      TElemType data;
      struct CSNode *firstchild, *rightsib;
   } CSNode, *CSTree;
   ```
   这种表示法, 查找某个结点的某个孩子只需要找到此结点的长子 `firstchild`, 再通过遍历长子结点的兄弟 `rightsib` 找到具体的孩子
   这个表示法的最大好处是把一颗复杂的树变成了一颗二叉树

### 二叉树的定义

1. 二叉树的特点
   1. 每个结点最多有两颗子树(左子树和右子树), 即二叉树中不存在度大于2的结点
   2. 左子树和右子树是有顺序的, 不能任意颠倒. 即使某结点只有一颗子树也要区分左子树和右子树.
2. 二叉树的五种基本形态
   1. 空二叉树
   2. 只有一个根结点
   3. 根结点只有左子树
   4. 根结点只有右子树
   5. 树既有左子树也要右子树
3. 特殊二叉树
   1. 斜树
      所有的结点都只有左子树的二叉树叫作左斜树(从上往下斜);
      所有的结点都只有右子树的二叉树叫作右斜树.
      {% asset_img Tree-binary-tree-skew-tree.png %}
      
      特点:
      * 线性表也可以理解为斜树
      * 斜树的结点个数根该树的深度相同
   2. 满二叉树
      所有的分支结点都存在左子树和右子树, 并且所有叶子都在同一层上, 这样的二叉树称为满二叉树.
      {% asset_img Tree-binary-tree-full-binary-tree.png %}

      特点:
      * 叶子只能出现在最下一层, 出现在其他车就不是满二叉树
      * 非叶子结点的度一定是2
      * 在同样深度的二叉树中, 满二叉树的结点个数最多, 叶子最多
   3. 完全二叉树
      对一颗具有 n 给结点的二叉树按层序编号, 如果编号为 i(1≤i≤n) 的结点与同样深度的满二叉树中编号
      为 i 的结点位置相同, 则这颗二叉树为完全二叉树
      (简单点说就是除了最后一层没有满编其他地方与满二叉树都相同)
      {% asset_img Tree-binary-tree-complete-binary-tree.png %}

      完全二叉树是满二叉树的必要但不充分条件
      {% asset_img Tree-binary-tree-full-complete-venn-diagram.png %}

      完全二叉树的特点:
      * 叶子结点只能出现在最下两层
      * 最下层的叶子一定集中在左部连续位置
      * 倒数两层, 若有叶子结点, 一个都在右部连续位置
      * 同样结点数的二叉树, 完全二叉树的深度最小

### 二叉树的性质

(下一层结点是上一层的2倍, 得到性质1和2)
1. 性质1 二叉树的第i层上至多有 \\(2^{i-1}\\) 个结点
   {% asset_img Tree-binary-tree-property-1.png %}
2. 性质2 深度为k的二叉树至多有 \\(2^{k}-1\\) 个结点
   {% asset_img Tree-binary-tree-property-2.png %}
3. 性质3 对任何一颗二叉树T, 如果其终端结点数(叶子数)为 \\(n_0\\) , 度为 2 的结点数为 \\(n_2\\) , 则 \\(n_0=n_2+1\\)
   (一棵二叉树中, 除了叶子外, 剩下的就是度为 1 或 2 的结点了, 设 \\(n_1\\) 为度为 1 的结点数, 则数的总结点数 \\(n=n_0+n_1+n_2\\))
   {% asset_img Tree-binary-tree-property-3.png %}
4. 性质4
   具有 n 个结点的完全二叉树的深度为 \\(\llcorner\log_2 n\lrcorner +1\\) (\\(\llcorner xxx\lrcorner\\)表示不大于xxx的最大整数)
   深度为 k 的满二叉树的结点数 \\(n=2^k-1\\) , 倒推得到满二叉树的深度为 \\(k=\log_2 (n+1)\\) .
   比如结点数 15 的满二叉树, 深度为 4 .
   完全二叉树的叶子只会出现在最下面两层. 结点数一定少于同样深度的满二叉树 \\(2^k-1\\), 但一定多于少一层的满二叉树 \\(2^{k-1}-1\\)
   即满足 \\(2^{k-1}-1 < n \leqslant 2^k-1\\) .
   \\(n\leqslant 2^k-1 \rArr n < 2^k\\) , \\(n > 2^{k-1}-1 \rArr n \geqslant 2^{k-1}\\)
   因此 \\(2^{k-1}\leqslant n < 2^k\\) , 不等式两边取对数得 \\(k-1 \leqslant \log_2 n < k\\)
   所以 \\(k=\llcorner\log_2 n\lrcorner +1\\)
5. 性质5
   具有 n 个结点的完全二叉树(深度\\(\llcorner\log_2 n\lrcorner +1\\)) 的结点按层序编号, 对任一结点 i (1≤i≤n) 有:
   1. 如果 i=1, 则结点 i 是二叉树的根; 如果 i>1 则双亲是结点 \\(\llcorner\frac{i}{2}\lrcorner\\)
   2. 如果 2i>n, 则结点 i 无左孩子(即结点 i 为叶子); 否则其左孩子是结点 2i
   3. 如果 2i+1>n, 则结点 i 无右孩子; 否则其右孩子是结点 2i+1
   
   {% asset_img Tree-binary-tree-property-5.png %}

### 二叉树的存储结构

1. 完全二叉树的存储
   二叉树顺序结构是用一维数组存储二叉树中的结点, 并且结点的存储位置(即数组下标)要能体现结点之间的逻辑关系, 比如双亲与孩子的关系, 左右兄弟的关系
   {% asset_img Tree-binary-tree-sequence-structure-1.png %}

   将这颗二叉树存入到数组中, 相应的下标对于其同样的位置
   {% asset_img Tree-binary-tree-sequence-structure-2.png %}

   完全二叉树存入到数组中, 相应的下标对应同样的位置, 一般的二叉树层序编号不能反映逻辑关系, 但可以将其补全为完全二叉树来编号, 把不存在的结点设置为"^"
   {% asset_img Tree-binary-tree-sequence-structure-3.png %}

   但使用这种方式编号也有缺点, 一种机端的情况是一颗深度为 k 的右斜树, 它只有 k 个结点, 却需要分配 \\(2^k-1\\) 个存储单元, 会造成对空间的极度浪费, 所以顺序结构一般**只用于完全二叉树**
   {% asset_img Tree-binary-tree-sequence-structure-4.png %}
2. 二叉链表
   二叉树每个结点最多有两个孩子, 设计一个数据域和两个指针域
   {% asset_img Tree-binary-tree-link-structure-1.png %}
   结构定义代码如下:
   ```C++
   typedef struct BiTNode // 结点结构
   {
       TElemType data;
       struct BiTNode *lchild, *rchild; // 左右孩子指针
   } BiTNode, *BiTree;
   ```
   结构示意图:
   {% asset_img Tree-binary-tree-link-structure-2.png %}

### 遍历二叉树

1. 二叉树遍历方法
   1. 前序遍历
      若二叉树为空, 则空操作返回, 否则
      1. 先访问根结点
      2. 前序遍历左子树
      3. 前序遍历右子树
      {% asset_img Tree-binary-tree-traversal-preorder-traversal.png %}

      图中遍历的顺序为:ABDGHCEIF
      实现代码如下:
      ```C++
      /* 二叉树前序遍历递归算法 */
      void PreOrderTraverse(BiTree T)
      {
          if (T == NULL) // 也可用 if (T) 包裹全部
              return;
          
          printf("%c", T->data);
          
          PreOrderTraverse(T->lchild); // 前序遍历左子树
          PreOrderTraverse(T->rchild); // 前序遍历右子树
      }
      ```
   2. 中序遍历
      若二叉树为空, 则空操作返回, 否则
      1. 从根结点开始(注意不是先访问根结点)
      2. 中序遍历根结点的左子树, 然后是访问根结点
      3. 中序遍历右子树
      {% asset_img Tree-binary-tree-traversal-inorder-traversal.png %}

      图中遍历的顺序为: GDHBAEICF
      实现代码如下:
      ```C++
      /* 二叉树中序遍历递归算法 */
      void InOrderTraverse(BiTree T)
      {
          if (T == NULL)
              return;
          
          InOrderTraverse(T->lchild); // 中序遍历左子树
          
          printf("%c", T->data);
          
          InOrderTraverse(T->rchild); // 中序遍历右子树
      }
      ```
   3. 后序遍历
      若二叉树为空, 则空操作返回, 否则
      1. 从左到右先叶子后结点的方式遍历访问左右子树
      2. 最后是访问根结点
      {% asset_img Tree-binary-tree-traversal-postorder-traversal.png %}

      图中遍历的顺序为: GHDBIEFCA
      实现代码如下:
      ```C++
      void PostOrderTraverse(BiTree T)
      {
          if (T == NULL)
              return;
          
          PostOrderTraverse(T->lchild); // 后续遍历左子树
          PostOrderTraverse(T->rchild); // 后续遍历右子树
          printf("%c", T->data);
      }
      ```
   4. 层序遍历
      若树为空, 则空操作返回, 否则
      1. 从树的第一层, 也就是根结点开始访问
      2. 从上而下逐层遍历
      3. 中同一层中, 按从左到右的顺序对结点逐个访问
      {% asset_img Tree-binary-tree-traversal-level-order-traversal.png %}
2. 推导遍历结果
   * 已知前序遍历序列和中序遍历序列, 能够唯一确定一颗二叉树
   * 已知后序遍历序列和中序遍历序列, 能够唯一确定一颗二叉树
   * 已知前序和后续遍历, 不能唯一确定一颗二叉树

### 二叉树的建立

要建立一颗普通的二叉树, 将这颗二叉树中每一个结点的空指针引出一个虚结点, 其值为一特定值, 比如"#".
处理好的二叉树为扩展二叉树, 扩展二叉树能够通过一个"前序"或"后序"遍历序列确定一颗二叉树
(这样就方便用一串字符序列来建立二叉树了)

{% asset_img Tree-binary-tree-create.png %}
图中前序遍历序列为AB#D##C##

实现代码如下:
```C++
// 按前序输入二叉树中各点的值
void CreateBitree(Bitree *T)
{
    TElemType ch;
    scanf("%c", &ch); // 输入结点数据字符
    if(ch == '#')
        *T = NULL;
    else
    {
        *T = (BiTree) malloc(sizeof(BiTNode));
        if(!*T) // 如果分配失败
            exit(OVERFLOW);
         
        (*T)->data = ch; // 给结点数据域赋值
        CreateBiTree(&(*T)->lchild); // 构造左孩子(子树)
        CreateBiTree(&(*T)->rchild); // 构造右孩子(子树)
    }
}
```

### 线索二叉树

1. 线索二叉树原理
   一个有 n 个结点的二叉链表. 每一个结点有指向左右孩子的两个指针域, 一共有 2n 个指针域.
   如图, n 个结点的二叉树一共拥有 n-1 条分支线(根结点无前驱), 存在 2n - (n-1) = n+1 个空指针域

   **指向前驱和后继的指针称为线索**,
   **加上线索的二叉链表则称为线索链表**;
   **加上线索的的二叉树称为线索二叉树(Threaded Binary Tree)**
   树的线索化:
   中序遍历后(图中为HDIBJEAFCG)
   1. 将所有空指针域中的 rchild, 改为指向它的后继结点.
      如图, H 的后继是 D, I 的后继是 B, J 的后继是 E, E的后继是 A, F的后继是C, G的后继因为不存在而指向NULL. 此时共有6个空指针域被利用

   2. 将这颗二叉树的所有空指针域中的 lchild 改为指向当前结点的前驱.
      如图, H 的前驱是 NULL, I 的前驱是 D, J 的前驱是 B, F 的前驱是 A, G 的前驱是 C. 一共5个空指针域被利用.
   
   将上面两种方法整合后, 正好11个空指针域被利用

   线索二叉树, 等于是把一颗二叉树转变成了一个双向链表, 对插入删除、查找某个结点都带来了方便.
   **对二叉树以某种次序遍历使其变为线索二叉树的过程称作是线索化**

   如图, 空心箭头实线为前驱, 虚线黑箭头为后继

   每个结点再增设两个标志域 ltag 和 rtag, ltag 和 rtag 是布尔型变量, 它们的值取决于结点是否存在左右孩子
   * ltag 为 0 是指向该结点的左孩子, 为 1 时指向该结点的前驱
   * rtag 为 0 时指向该结点的右孩子, 为 1 时指向该结点的后继
2. 线索二叉树结构实现
   二叉树的线索存储结构定义代码如下:
   ```C++
   // 创建一个枚举类型用来给下面的 LTag, RTag 赋值
   // Link 的值为 0, 指向左右孩子指针
   // Thread 的值为 1, 指向前驱或后继
   typedef eum { Link, Thread } PointerTag;

   typedef struct BiThrNode
   {
       TElemType data;
       struct BiThrNode *lchild, *rchild;
       PointerTag LTag;
       PointerTag RTag;
   } BiThrNode, *BiThrTree;
   ```
   线索化的实现(在遍历二叉树的过程中修改空指针)
   实现代码如下:
   ```C++
   // 中序线索化
   BiThrTree pre; // 全局变量(因为递归要用), 存储刚刚访问过的结点(当前结点的前驱)
   void InThreading(BitThrTree p)
   {
       if (p)
       {
           InThreading(p->lchild); // 中序遍历 (1/2)

           if (!p->lchild) // 若结点无左孩子
           {
               p->LTag = Thread; // 设置该结点 ltag
               p->lchild = pre; // 左孩子指针指向前驱
           }

           if (!pre->rchild) // 若该结点的前驱没有右孩子
           {
               pre->RTag = Thread; // 设置前驱的 ltag
               pre->rchild = p; // 前驱右孩子指针指向当前结点
           }

           pre = p; // 设置为当前访问的结点
           
           InThreading(p->rchild); // 中序遍历 (2/2)
       }
   }
   ```
   双向线索链表:
   在线索链表上加入一个头结点. 它的 lchild 域指向二叉树根结点; rchild 域指向中序序列最后一个结点.
   中序序列第一个结点的 lchild 和最后一个结点的 rchild 域均指向头结点.
   这样的优点是方便从第一个结点起顺后继进行遍历, 也能从最后一个结点起顺前驱进行遍历
   TODO: 补充图片
   遍历双向线索链表:
   实现代码如下:
   ```C++
   // T 指向头结点
   // 中序遍历, 时间复杂度为 O(n)
   int InOrderTraverse_Thr(BitThrTree T)
   {
      BitThrTree p;
      p = T->lchild; // p 指向根结点
      while (p != T) // 空树或遍历结束时, p == T
      {
          while (p->LTag == Link) // 遍历到当前树的最左边
              p = p->lchild;
          
          printf("%c", p->data); // 输出结点数据
          
          while (p->RTag == Thread && p->rchild != T) // 若当前结点p的后继是线索化的, 且p的后继不是头结点
          {
              p = p->rchild; // 进入后继结点(退到上级)
              printf("%c", p->data);
          }
          p = p->rchild; // p 进入当前结点的右子树
      }
      return OK;
   }
   ```
   如果所用的二叉树需要经常遍历或查找结点时需要某种遍历序列中的前驱和后继, 那么采用线索二叉树的存储结构就是非常不错的选择

### 树、森林与二叉树的转换

二叉树中, 左孩子代表孩子, 右孩子代表兄弟

1. 树转换为二叉树
   1. 加线. 在所有兄弟结点之间加一条线
   2. 去线. 对树中每个结点, 只保留它与第一个孩子的连线.
   3. (对于作图的)层次调整. 一树的根结点为轴心, 将整棵树顺时针旋转一定的角度, 使之结构层次分明.
   
   TODO: 补充图片
2. 森林转换为二叉树
   森林是由若干颗树组成的, 可以理解为森林中的每一颗树都是兄弟, 可以按照兄弟的处理方法来操作:
   1. 把每个树转换为二叉树
   2. 第一款二叉树不懂, 依次把后一颗二叉树的根结点作为前一颗二叉树的根结点的右孩子.
   
   TODO: 补充图片
3. 二叉树转换为树
   二叉树转换为树就是树转换为二叉树的逆过程
   1. 加线. 若某结点的左孩子结点存在, 则将这个左孩子的 n 个右孩子结点都作为此结点的孩子. 将该结点的与这些右孩子结点用线连接
   2. 去线. 删除原二叉树中所有结点与其右孩子结点的连线
   3. (对于作图的)层次调整. 使之结构层次分明
   
   TODO: 补充图片
4. 二叉树转换为森林
   判断一颗二叉树能否转换成森林, 看二叉树的根结点有右孩子
   1. 从根结点开始, 若右孩子存在, 则把与右孩子的连线删除; 再查看分离后的二叉树, 若右孩子存在, 则连线删除..., 直到分离后树的根结点没有右孩子.
   2. 再将每颗分离后的二叉树转换为树即可

   TODO: 补充图片
5. 树与森林的遍历
   树的遍历分为两种方式:
   * 先根遍历. 即先访问树的根结点, 然后依次先根遍历根的每颗子树
   * 后根遍历. 即先依次后根遍历每颗子树, 然后再访问根结点
   
   如图, 先根遍历序列为ABEFCDG, 后根遍历为EFBCGDA
   TODO: 补充图片

   森林的遍历也分为两种方式:
   * 前序遍历: 从左到右依次先根遍历森林中的每棵树
   * 后序遍历: 从左到右依次后根遍历森林中的每棵树

   如图, 前序遍历序列为ABCDEFGHJI, 后序遍历序列为BCDAFEJHIG
   TODO: 补充图片

   将森林转换为二叉树后, 森林的前序遍历和二叉树的前序遍历结果相同, 森林的后序遍历和二叉树的中序遍历结果相同
   TODO: 补充图片

   当以二叉链表作为树的存储结构时, 树的先根遍历和后根遍历完全可以借用二叉树的前序遍历和中序遍历的算法来实现

### 赫夫曼树及其应用

1. 赫夫曼定义与原理
   TODO: 补充图片
   从树中一个结点到另一个结点之间的分支(线段)构成两个结点之间的路径, 路径上的分支(线段)数目称作路径长度
   树的路径长度: 从树根到每一结点的路径长度之和

   如图, 二叉树a中, 根节点到D的路径长度为4. 二叉树a的树路径长度为1+1+2+2+3+3+4+4=20; 二叉树b的树路径为1+1+2+2+2+2+3+3=16

   如果考虑到带权结点, 结点的带权路径长度为: 从该结点到树根之间的路径长度x节点的权.
   假设有 n 个权值 \\(w_1, w_2, \dots, w_n\\) , 构造一颗有 n 个叶子结点的二叉树, 每个叶子结点带权 \\(w_k\\) , 每个叶子的路径长度为 \\(l_k\\) , 则其中带权路径长度(WPL)最小的的二叉树称作赫夫曼树(最优二叉树).
   
   
   二叉树a的 \\(\text{WPL}=5*1+15*2+40*3+30*4+10*4=315\\)
   二叉树b的 \\(\text{WPL}=5*3+15*3+40*2+30*2+10*2\\)
   它们都不是赫夫曼树

   构造赫夫曼树:
   1. 先把有权值的叶子结点按照从小到大的顺序排列成一个有序序列. 即: A5, E10, B15, D30, C40
   2. 取头两个最小权值的结点作为一个新结点 \\(N_1\\) 的两个叶子, 相对较小的是左孩子, 这里就是A为 \\(N_1\\) 的左孩子, E为 \\(N_1\\) 的右孩子, 新结点的权值为两个叶子权值的和(5+10=15)
   3. 用新结点 \\(N_1\\) 替换A与E, 插入有序序列中, 保存从小到大排列. 即: \\(N_1\\)15, B15, D30, C40
   4. 重复步骤2和3, \\(N_2=N_1 15+B15=30\\), 有序序列: \\(N_2\\)30, D30, C40
   5. 重复步骤2和3, \\(N_3=N_2 30+D30=60\\), 有序序列: \\(N_3\\) 60, C40
   6. 重复步骤2. 将\\(N_3\\) 与C作为新结点T的两个叶子, 由于T是根节点,  完成赫夫曼树的构造
   
   WPL=40x1+30x2+15x3+10x4+5x4=205
   构造赫夫曼树的赫夫曼算法描述:
   1. 根据给定的n个权值 \\(w_1, w_2, \dots, w_n\\) 构成n颗二叉树的集合 \\(F=T_1, T_2, \dots, T_n\\) , 其中每颗二叉树 \\(T_i\\) 中只有一个 \\(w_i\\) 的根结点, 其左右子树均为空.
   2. 在F中选取两颗根结点的权值最小的树作为左右子树构造一棵新的二叉树, 新的二叉树根节点的权值为其左右子树根节点的权重之和.
   3. 在F中删除这两棵树, 同时将新的到的二叉树加入F中
   4. 重复2和3步骤, 直到F只含一棵树位置. 这棵树便是赫夫曼树
2. 赫夫曼编码
   传输文字内容为"BADCADFEED", 相应的二进制数据表示如下
   | 字母 | A | B | C | D | E | F |
   | ---- | - | - | - | - | - | - |
   | 二进制字符 | 000 | 001 | 010 | 011 | 100 | 101 |
   真正传输的数据就是编码后的"01000011010000011101100100011"
   
   假设六个字母的频率(权重)为 A27, B8, C15, E30, F5, 合起来正好是 100%. 完全可以重新按照赫夫曼树来规划它们
   TODO: 补充图片
   左图为构造赫夫曼树的权值显示. 右图为将权值左分支改为0, 右分支改为1的赫夫曼树
   
   对这六个字母用从树根到叶子所经过路径的0或1来编码, 可以得到如表所示这样的定义:
   | 字母 | A | B | C | D | E | F |
   | ---- | - | - | - | - | - | - |
   | 二进制字符 | 01 | 1001 | 101 | 00 | 11 | 1000 |
   将文字内容为"BADCADFEED"再次编码, 对比可以看到结果串变小了
   数据被压缩了, 节约了大约17%的存储和传输成本. 随着字符的增加和多字符权重的不同, 这种压缩会更加显出其优势.

   **若要设计长短不等的编码, 则必须是任一字符的编码都不是另一个字符的编码的前缀(否则会造成歧义), 这种编码称作前缀编码**

   在解码时, 还要用到赫夫曼树, 即发送方和接收方必须要约定好同样的赫夫曼编码规则

   设需要编码的字符集为 \\(d_1, d_2, \dots, d_n\\) , 各个字符在电文中出现的次数(或频率)集合为 \\(w_1, w_2, \dots, w_n\\) ,
   以 \\(d_1, d_2, \dots, d_n\\) 作为叶子结点, 以 \\(w_1, w_2, \dots, w_n\\) 作为相应叶子结点的权值来构造一颗赫夫曼树.
   规定赫夫曼树的左分支代表0, 右分支代表1, 则从根节点到叶子所经过的路径分支组成序列便为该结点对应字符的编码, 也就是赫夫曼编码

## 图

### 图的定义

图(Graph)是由顶点构成的有穷非空集合和顶点之间边的集合组成
通常表示为: G(V, E), 其中G表示一个图, V是图G中顶点的集合, E是图G中边的集合

TODO: 补充图片
* 线性表中数据元素叫**元素**, 树中数据元素叫**结点**, 图中数据元素叫**顶点(Vertex)**
* 线性表中可以没有数据元素, 称为**空表**; 树中可以没有结点, 叫作**空树**; 但图结构中不允许没有顶点.
  在定义中, 若V是顶点的集合, 则强调了顶点集合V有穷非空
* 线性表中, 相邻的数据元素之间具有线性关系; 树结构中, 相邻两层的结点具有层次关系; 图中任意两个顶点之间都可能有关系, 顶点之间的逻辑关系用边来表示, 边集可以是空的

1. 各种图的定义
   1. 若顶点 \\(v_i\\) 到 \\(v_j\\) 之间的边没有方向, 则称这条边为无向边(Edge), 用无序偶对 \\((v_i, v_j)\\) 来表示.
      如果图中任意两个顶点之间的边都是无向边, 则称该图为**无向图(Undirected graphs)**
      TODO: 补充图片
      无序对(A,D)也可以写成(D,A)
      对于无向图 \\(G_1\\) :
      * \\(G_1=(V_1, \\{E_1\\})\\)
      * 顶点集合 \\(V_1=\\{A, B, C, D\\}\\)
      * 边集合 \\(E_1=\\{(A,B),(B,C),(C,D),(D,A),(A,C)\\}\\)
   
   2. 若从顶点 \\(v_i\\) 到 \\(v_j\\) 的边有方向, 则称这条边为有向边, 也称为弧(Arc). 用有序偶对<\\(v_i\\), \\(v_j\\)>来表示, \\(v_i\\) 称为弧尾(Tail), \\(v_j\\) 称为弧头(Head).
      如果图中任意两个顶点之间的边都是有向边, 则称该图为**有向图(Directed graphs)**
      TODO: 补充图片
      有序对<A,D>, A是弧尾, D是弧头, 不能写成<D,A>
      对于有向图 \\(G_2\\) :
      * \\(G_2=(V_2, \\{E_2\\})\\)
      * 顶点集合 \\(V_2=\\{A,B,C,D\\}\\)
      * 弧集合 \\(E_2=\\{<A,D>,<B,A>,<C,A>,<B,C>\\}\\)

      无向边用小括号"()"表示, 而有向边用尖括号"<>"表示
   3. 在图中, 若同一条边不重复出现(如左图), 且不存在顶点到其自身的边(如右图), 则称这样的图为简单图
      TODO: 补充图片
      在无向图中, 如果任意两个顶点之间都存在边, 则称该图为无向完全图. 含有n个顶点的无向完全图有 \\(\frac{n(n-1)}{2}\\) 条边
      TODO: 补充图片
      在有向图中, 如果任意两个顶点之间都存在方向互为相反的两条弧, 则称该图为有向完全图. 含有n个顶点的有向完全图有 \\(n(n-1)\\) 条边
      TODO: 补充图片

      
      **有很少条边或弧的图称为稀疏图, 反之称为稠密图**. 稀疏和稠密是模糊的概念, 是相对而言的
      有些图的边或弧具有权(Weight). 这些权可以表示从一个顶点到另一个顶点的距离或耗费. 这种带权的图通常称为网(Network)
      TODO: 补充图片
      假设有两个图 G=(V,{E}) 和 G'=(V',{E'}), 如果 V'∈V 且 E'∈E , 则称 G' 为 G 的子图(Sub-graph)
      TODO: 补充图片
2. 图的顶点与边间关系
   对于无向图 G=(V,{E}) , 如果边 (v, v')∈E, 则称顶点 v 和 v' 互为邻接点(Adjacent), 即 v 和 v' 相邻接. 边 (v,v') 依附(Incident)于顶点 v 和 v', 或者说 (v,v') 与顶点 v 和 v' 相关联. 顶点 v 的度(Degree)是和 v 相关联的边的数目, 记为 TD(v). 边数是各顶点度数和的一半, 多出的一半是因为重复两次记数, \\(e=\frac{1}{2}\displaystyle\sum_{i=1}^n TD\\{V_i\\}\\)

   对于有向图 G=(V,{E}), 如果弧 <v,v'>∈E, 则称顶点 v 和 v' 互为邻接点. 弧 <v,v'> 和顶点 v, v' 相关联. 以顶点 v 为头的弧的数目称为 v 的**入度(InDegree)**, 记为 ID(v); 以 v 结尾的弧的数目称为v的**出度(OutDegree)**, 记为 OD(v)
   顶点 v 的度为 TD(v)=ID(v)+OD(v), \\(e=\displaystyle\sum_{i=1}^n ID\\{V_i\\}=\displaystyle\sum_{i=1}^n OD\\{V_i\\}\\)

   **路径的长度是路径上的边或弧的数目**
   无向图 G=(V,{E}) 中从顶点 v 到顶点 v' 的路径(Path)是一个顶点序列
   \\(v=v_{i,0},v_{i,1},\dots,v'=v_{i,m}\\) , 其中 \\((v_{i,j-1},v_{i,j})\in E, 1\leqslant j\leqslant m\\)

   如图, 顶点B到D有四种不同路径
   如果G是有向图, 则路径也是有向的, 顶点序列应满足 \\(<v_{i,j-1}, v_{i,j}>\in E, 1\leqslant j\leqslant m\\)
   TODO: 补充图片
   
   第一个顶点和最后一个顶点相同的路径称为回路或环(Cycle).
   序列中顶点不重复出现的路径称为简单路径.
   除了第一个顶点和最后一个顶点之外, 其余顶点不重复出现的回路, 称为简单回路或简单环

   如图, 两图的的粗线都构成环, 左侧属于简单环; 右侧的环由于顶点C的重复不是简单环
   TODO: 补充图片
   
3. 连通图相关术语
   1. 在无向图 G 中, 如果从顶点 v 到顶点 v' 有路径, 则称 v 和 v' 是连通的.
      如果对于图中任意两个顶点 \\(v_i, v_j\in V\\) , \\(v_i\\) 和 \\(v_j\\) 都是连通的, 则称 G 是连通图(Connected Graph)
      
      如图, 图1显然顶点 A、B、C、D 与顶点 E 或 F 就无路径, 因此不能算是连通图; 图2顶点 A、B、C、D 都是相互连通的, 所以是连通图
      TODO:补充图片, 补充下面的图片. (共两张)
      
      无向图中的极大连通子图称为**连通分量**. 强调:
      * 要是子图
      * 子图是连续的
      * 连通子图含有极大顶点数
      * 具有极大定点数的连通子图包含依附于这些顶点的所有边
      
      比如刚才的图中, 图1是一个无向非连通图. 它有两个连通分量(即图2和图3).
      而图4尽管是图1的子图, 但是它不满足连通子图极大顶点数. 因此它不是图1的连通分量
   2. 在有向图 G 中, 如果对于每一对 \\(v_i,v_j\in V \quad(v_i\neq v_j)\\) , 从 \\(v_i\\) 到 \\(v_j\\) 和从 \\(v_j\\) 到 \\(v_i\\) 都存在路径, 则称 G 是强连通图.
      有向图中的极大强连通子图称作有向图的强连通分量
      
      如图, 图1不是强连通图, 因为 <D,A> 不存在. 图2是强连通图, 且是图1的强连通分量
      TODO: 补充图片

   3. 连通图的生成树是一个极小连通子图, 它含有图中全部的 n 个顶点, 但只有足以构成一棵树的 n-1 条边

      如图, 图1是普通树, 当去掉两条构成环的边后(图2和图3), 就满足 n 个顶点 n-1 条边且连通的定义, 它们都是生成树.

      逻辑关系: 多于 n-1 条边 是 构成环 的充分必要条件; 小于 n-1 条边 是 非连通图 的充分必要条件; 等于 n-1 条边 是 生成树 的必要但不充分条件(图4)
      TODO: 补充图片

      **如果一个有向图恰好有一个顶点的入读为0, 其余顶点的入度均为1, 则是一个有向树.**
      入度为0相当于树中的根节点, 其余顶点入度为1意味着树中非根节点的双亲只有一个.
      **一个有向图可分解为若干颗不相交的有向树, 它们组成了含有图中全部顶点的生成森林**
      TODO: 补充图片
4. 总结
   按照有无方向: 无向图和有向图
   按照边的多少: 稀疏图和稠密图
   顶点之间有邻接点的概念, 边有依附的概念.
   无向图顶点的边数叫度, 有向图顶点的边分为入度和出度.
   图上的边带权则称为网
   若路径最终回到起始点称为环, 不重复叫简单路径.
   若顶点两两相连， 称为连通图, 有向则称强连通图.
   极大连通子图称为连通分量, 有向则称强连通分量
   无向图中连通且 n 个顶点 n-1 条边叫生成树. 有向图中一顶点入度为0其余顶点入度为1的叫有向树. 有向图可分解为若干颗有向树构成生成森林

### 图的抽象数据类型

```C++
ADT 图(Graph)
Data
    顶点的有穷非空集合和边的集合
Operation
    CreateGraph(*G, V, VR) : 按照顶点集合V和边弧集VR的定义构造图G
    DestroyGraph(*G) : 图G存在则销毁
    LocateVex(G, u) : 若图G中存在顶点 u, 则返回图中的位置
    GetVex(G, v) : 返回图G中顶点 v 的值
    PutVex(G, v, value) : 将图 G 中顶点 v 赋为 value
    FirstAdjVex(G, *v) : 返回顶点 v 的一个邻接顶点, 若顶点在 G 中无邻接顶点返回 NULL
    NextAdjVex(G, v, *w) : 返回顶点 v 相对于顶点 w 的下一个邻接顶点, 若 w 是 v 的最后一个邻接点则返回 NULL
    InsertVex(*G, v) : 在图G中增添新顶点 v
    DeleteVex(*G, v) : 删除图 G 中顶点 v 及其相关的弧
    InsertArc(*G, v, w) : 在图G中增添弧 <v,w>, 若 G 是无向图, 还需要增添对称弧 <w,v>
    DeleteArc(*G, v, w) : 在图G中删除弧 <v,w>, 若 G 是无向图, 还需要删除对称弧 <w,v>
    DESTraverse(G) : 对图 G 中进行深度优先遍历, 以在在遍历过程中对每个顶点调用
    HFSTraverse(G) : 对图 G 中进行广度优先遍历, 以在在遍历过程中对每个顶点调用
endADT
```

### 图的存储结构

从图的逻辑结构定义来看, 任意顶点都可看作第一个顶点, 顶点间的逻辑关系与顶点所在位置无关
TODO: 补充图片

1. 邻接矩阵
   图的邻接矩阵（Adjacency Matrix)存储方式是用两个数组来表示图.
   一个一维数组存储图中顶点信息, 一个二维数组(即邻接矩阵)存储图中的边或弧的信息
   设 G 有 n 个顶点, 则邻接矩阵是一个 n×n 的方阵, 定义为:
   \\(arc[i][j]=\begin{cases} 1, \text{若}(v_i, v_j)\in E \text{或} <v_i, v_j>\in E \\\ 0, 反之 \end{cases}\\)
   TODO: 补充图片
   顶点数组为 \\(\text{vertex}[4]=\\{v_0, v_1, v_2, v_3\\}\\) , 边数组 \\(\text{arc}[4][4]\\) 为右图这样的一个矩阵.
   矩阵的主对角线的值全为 0 是因为不存在顶点到自身的边, 无向图的边数组是一个对称矩阵.
   **对称矩阵就是 n 阶矩阵的元满足 \\(a_{ij}=a_{ji} \quad(0\leqslant i, j\leqslant n)\\) , 即从矩阵左上至右下角的主对角线为轴呈对称关系**

   特点:
   1. 判定任意两顶点是否邻接
   2. 要知道顶点 \\(v_i\\) 的度, 可求 \\(v_i\\) 在邻接矩阵中第i行(或第i列)的元素之和(有向图中横向出度统计, 纵向入度统计)
   
   2. 有向图的邻接矩阵
      顶点数组为 \\(\text{vertex}[4]=\\{v_0, v_1, v_2, v_3\\}\\) , 边数组 \\(\text{arc}[4][4]\\) 为右图这样的一个矩阵.
      对角线上数值依旧为0, 此矩阵并不对称
      
      TODO: 补充图片
      特点:
      1. 有向图讲究入度与出度, 如顶点 \\(v_1\\) 入度为第 i 纵列总和, 出度为第 i 横行总和
      2. 判断从顶点 \\(v_i\\) 到 \\(v_j\\) 是否存在弧, 只需要查找矩阵中 arc[i][j] 是否为 1
      
   3. 网图的邻接矩阵
      设图 G 是网图, 有 n 个顶点, 则邻接矩阵是一个 n×n 的方阵, 定义为:
      \\(\text{arc}[i][j]=\begin{cases} W_{ij} & \text{if } (v_i, v_j)\in E \text{ or } <v_i, v_j>\in E \\\ 0 & \text{if } i=j \\\ \infty & \text{other} \end{cases}\\)
      TODO: 补充图片
      ∞ 表示一个计算机允许的、大于所有边上权值的值, 也就是一个不可能的极限值
      
      网图的邻接矩阵存储结构代码:
      ```C++
      #define MAXVEX 100      // 最大顶点数
      #define INFINITY 65535  // 用 65535 代替网图邻接矩阵的 ∞
      
      typedef char VertexType; // 顶点类型, 假设为 char
      typedef int EdgeType;   // 边上的权值类型, 假设为 int
      
      typedef struct
      {
          VertexType vexs[MAXVEX];      // 顶点表
          EdgeType arc[MAXVEX][MAXVEX]; // 邻接矩阵(边表)
          int numVertexes, numEdges;    // 图的当前顶点数和边数
      } NGraph;
      ```

      **无向**网图的邻接矩阵结构 CreateGraph() :
      ```C++
      void CreateNGraph(NGraph *G)
      {
         int i, j, k, w;
         
         printf("输入顶点数和边数:\n");
         scanf("%d,%d", &G->numVertexes, &G->numEdges);

         // 给顶点表赋值
         for (i = 0; i < G->numVertexes; i++)
             scanf(&G->vexs[i]);

         // 邻接矩阵初始化
         for (i = 0; i < G->numVertexes; i++)
             for (j = 0; j < numVertexes; j++)
                 G->arc[i][j] = INFINITY;
         
         for (k = 0; k < G->numEdges; k++)
         {
            printf("输入边 (v_i, v_j) 的 i、j 和该边的权 w :\n");
            scanf("%d,%d,%d", &i, &j, &w);
            G->arc[i][j] = w;
            G->arc[j][i] = G->arc[i][j]; // 因为是无向图, 矩阵对称 (有向则没有这句)
         }
      }
      ```

      时间复杂度分析:
      n 个顶点和 e 条边的无向网图的创建, 时间复杂度为 \\(O(n+n^2+e)\\)
      
   邻接矩阵存储结构对于边数相对顶点较少的图来说极大浪费存储空间
2. 邻接表
   **数组与链表相结合的存储方法称为邻接表(Adjacency List)**
   邻接表的处理方法:
   1. 顶点用一个一维数组存储.
      顶点数组中, 每个元素还有一个指针域, 指向该顶点的第一个邻接点
   2. 图中每个顶点的所有邻接点构成一个线性表(存储邻接点在顶点数组中的下标), 由于邻接点的个数不定, 所以用单链表存储
      无向图称为顶点 \\(v_i\\) 的边表; 有向图则称为顶点 \\(v_i\\) 的出边表(有向图也可以建立一个逆邻接表, 即为每个顶点建立一个入边表)
   3. 对于带权值的网图, 可以在边表顶点定义在再增加一个数据域存储权值
   
   TODO: 补充图片, 补充下一张图片, 补充下一张图片

   特点:
   1. 要想查某个顶点的度, 就去查这个顶点的边表中顶点的个数.
   2. 若要判断顶点 \\(v_i\\) 到 \\(v_j\\) 是否存在边, 只需测试顶点 \\(v_i\\) 的边表中是否存在顶点 \\(v_j\\) 的下标 j .

   边表顶点定义代码:
   ```C++
   #define MAXVEX 100      // 最大顶点数

   typedef char VertexType; // 顶点类型, 假设为 char
   typedef int EdgeType;   // 边上的权值类型, 假设为 int
   
   // 边表顶点
   typedef struct EdgeNode
   {
       int adjVex;      // 邻接点域, 存储该顶点对应下标
       EdgeType weight; // 存储权值, 非网图可以不需要
       struct EdgeNode *next;
   } EdgeNode;

   // 顶点表元素
   typedef struct VertexNode
   {
       VertexType data;     // 顶点域, 存储顶点信息
       EdgeNode *firstEdge; // 该顶点边表的头指针
   } VertexNode, AdjList[MAXVEX];

   typedef struct
   {
       AdjList adjList;
       int numVertexes, numEdges; // 图的当前顶点数和边数
   } GraphAdjList;
   ```

   无向图的邻接表结构 CreateGraph() :
   ```C++
   void CreateALGraph(GraphAdjList *G)
   {
      int i, j, k;
      EdgeNode *e;

      printf("输入顶点数和边数:\n");
      scanf("%d,%d", &G->numVertexes, &G->numEdges);

      for (i = 0; i < G->numVertexes; i++)
      {
          scanf(&G->adjList[i].data); // 输入顶点信息
          G->adjList[i].firstEdge = NULL; // 初始化边表指针
      }

      for (k = 0; l < G->numEdges; k++)
      {
          printf("输入边 (v_i, v_j) 上的顶点序号:\n");
          scanf("%d,%d", &i, &j);

          // 创建边表新顶点
          e = (EdgeNode*) malloc(sizeof(EdgeNode));
          e->adjVex = j; // 邻接顶点下标为 j
          // 使用头插法
          e->next = G->adjList[i].firstEdge;
          G->adjList[i].firstEdge = e;

          // 因为是无向图, 添加对应邻接顶点的边表顶点 (有向则没有下面的内容)
          e = (EdgeNode*) malloc(sizeof(EdgeNode));
          e->adjVex = i;
          e->next = G->adjList[j].firstEdge;
          G->adjList[j].firstEdge = e;
      }
   }
   ```
   
   时间复杂度分析:
   本算法对于 n 个顶点 e 条边来说是 O(n+e)
   
   邻接表的缺陷:
   * 只关心出度问题, 想了解入度就必须遍历整个图
   * 逆邻接表解决了入度却不了解出度的情况
3. 十字链表
   **把邻接表和逆邻接表结合起来的存储结构叫十字链表(Orthogonal List)**
   TODO: 做成表格
   重新定义顶点表顶点结构: data, firstin(指向逆邻接表第一个顶点), firstout(指向邻接表第一个顶点)
   重新定义边表顶点结构: tailvex, headvex, headlink, taillink
   (headlink逆邻接表的下一个顶点, taillink邻接表下一个顶点)
   TODO: 补充图片(最好把图片改一改, 划分一下区域)

   十字链表的优势:
   * 把邻接表和逆邻接表整合在了一起, 容易找到以 \\(v_i\\) 为尾的弧和以 \\(v_i\\) 为头的弧, 容易求得顶点的出度和入度
   * 除了结构复杂一点外, CreateGraph() 的时间复杂度和邻接表相同
4. 邻接多重表
   重新定义边表顶点结构: ivex, ilink, jvex, jlink
   ivex和jvex是某条边依附的两个顶点的下标. link指向依附顶点ivex的下一条边, jlink指向依附顶点jvex的下一条边
   ilink指向顶点的jvex一定要和它本身的ivex的值相同
   TODO: 补充下下张图片(一张)
   邻接多重表和邻接表的区别: 在邻接多重表中同一条边只有一个顶点. 若要删除左图的 \\((v_0, v_2)\\) 这条边, 只需要将右图的 ⑥⑨ 的链接改为 ∧ 即可
5. 边集数组
   边集数组是由两个一维数组构成. 一个存储顶点的信息(vexs[MAXVEX]); 另一个存储边的信息(edges[MAXEDGE])
   TODO: 做成表格
   边数组中每个元素的结构: begin, end, weight
   TODO: 补充图片
   边集数组关注的是边的集合, 在边集数组中要查找一个顶点的度需要扫描整个边数组, 效率并不高.
   因此他更适合对边依次进行处理的操作, 而不适合对顶点相关的操作

### 图的遍历

从图中某一顶点出发访遍图中其余顶点且每个顶点仅被访问一次, 这一过程称为图的遍历(Traversing Graph)

1. 深度优先遍历(Depth First Search)
   也称深度优先搜索, 简称DFS. 类似于树的前序遍历

   (深度优先指的是: 优先找邻接点, 贯穿整个图)
   从图中某个顶点 \\(v_i\\) 出发, 然后从 \\(v_i\\) 周围的(未被访问的)邻接点 \\(v_j\\) 出发找邻接点, 重复此过程直至图中所有和 \\(v_i\\) 有路径相通的顶点都被访问到.
   以上说的只是连通图, 对于非连通图, 只需要对它的连通分量分别进行深度优先遍历. 即进行一次深度优先遍历后, 若图中尚有顶点未被访问, 则另选图个一个未被访问的顶点作为起始点, 再次进行深度优先遍历. 重复此过程直到所有顶点都被访问过

   访问数组 visited[n], n 是图中顶点的个数, 数组中元素初值为 0, 访问后为 1

   邻接矩阵的深度优先遍历算法:
   ```C++
   typedef enum { false, true } bool; // C++ 中自带 bool 类型, 可以不用这句
   bool visited[MAX]; // MAX 等于图的 numVertexes

   // 邻接矩阵的深度优先递归算法, i 为当前遍历顶点的下标
   void DFS(MGraph G, int i)
   {
       int j;
       visited[i]=true;

       printf("%c", G.vexs[i]) // 打印顶点数据, 也可以是其他操作

       for (j = 0; j < G.numVertexes; j++)
       {
           if (G.arc[i][j] == 1 && !visited[j]) // 找任何与 v_i 邻接的邻接点(未被访问过的)
               DFS(G, j); // 对要访问的邻接点递归调用 DFS()
       }
   }

   // 从这个函数开始
   void DFSTraverse(MGraph G)
   {
       int i;

       // 初始化 visited 数组
       for (i = 0; i < G.numVertexes; i++)
           visited[i] = false;

       for (i = 0; i < G.numVertexes; i++)
           if (!visited[i])
               DFS(G, i); // 对未访问过的顶点调用 DFS() (若是连通图这行代码只会执行一次)
   }
   ```

   对于邻接表结构的图, DFS() 会稍有不同
   邻接表的深度优先遍历算法:
   ```C++
   typedef enum { false, true } bool; // C++ 中自带 bool 类型, 可以不用这句
   bool visited[MAX]; // MAX 等于图的 numVertexes

   // i 为当前遍历顶点的下标
   void DFS(GraphAdjList GL, int i)
   {
       EdgeNode *p;
       visited[i] = true;

       printf("%c", GL->adjList[i].data);

       p = GL->adjList[i].firstEdge;
       while (p)
       {
           if (!visited[p->adjVex])
               DFS(GL, p->adjvex); // 对要访问的邻接点递归调用 DFS()

           p = p->next;
       }
   }

   void DFSTraverse(GraphAdjList GL)
   {
       int i;

       // 初始化 visited 数组
       for (i = 0; i < GL->numVertexes; i++)
           visited[i] = false;

       for (i = 0; i < GL->numVertexes; i++)
           if (!visited[i])
               DFS(GL, i); // 对未访问过的顶点调用 DFS() (若是连通图这行代码只会执行一次)
   }
   ```

   时间复杂度分析:
   如果是邻接矩阵结构表示的图, 每次找邻接点都需要把顶点下标遍历一遍, 时间复杂度 \\(O(n^2)\\)
   而邻接表结构表示的图, 找邻接点所需时间取决于该顶点的出边数量, 时间复杂度 \\(O(n+e)\\)
   对于点多边少的稀疏图来说, 邻接表结构的优势更大
2. 广度优先遍历(Breadth First Search)
   也称广度优先搜索, 简称BFS. 类似于树的层序遍历

   如图, 将第一幅图稍微变形.
   规则是顶点 A 放置在最上第一层, 使与它邻接的顶点 B、F 为第二层,
   再让与 B 和 F 邻接的 C、I、G、E 为第三层,  再将与这四个顶点邻接的 D、H 放在第四层.
   TODO: 补充图片

   邻接矩阵的广度优先遍历算法:
   ```C++
   typedef enum { false, true } bool; // C++ 中自带 bool 类型, 可以不用这句
   bool visited[MAX]; // MAX 等于图的 numVertexes

   void BFSTraverse(MGraph G)
   {
       int i, j, k;
       Queue Q; // 临时存储顶点下标用队列

       // 初始化 visited 数组
       for (i = 0; i < G.numVertexes; i++)
           visited[i] = false;

       InitQueue(&Q); // 初始化队列
       
       for (i = 0; i < G.numVertexes; i++)
       {
           if (!visited[i]) // 对未访问过的顶点进行广度优先遍历 (若是连通图 if 内的代码只会执行一次)
           {
               // 将起始顶点进行打印等操作后加入队列
               visited[i] = true; // 将当前顶点设置为被访问过

               printf("%c", G.vexs[i]); // 打印顶点, 也可以是其他操作

               EnQueue(&Q, i); // 将此顶点加入队列

               while (!QueueEmpty(Q)) // 若当前队列不为空
               {
                   DeQueue(&Q, &k); // 将队列中元素取出赋给变量 k

                   for (j = 0; j < G.numVertexes; j++)
                   {
                       if (G.arc[k][j] == 1 && !visited[j]) // 遍历当前顶点 v_k 的邻接点并加入队列
                       {
                           visited[j] = true; // 将找到的邻接点设置为已访问

                           printf("%c", G.vexs[j]); // 打印顶点, 也可以是其他操作

                           EnQueue(&Q, j);
                       }
                   }
               }
           }
       }
   }
   ```

   邻接矩阵的广度优先遍历算法:
   ```C++
   typedef enum { false, true } bool; // C++ 中自带 bool 类型, 可以不用这句
   bool visited[MAX]; // MAX 等于图的 numVertexes

   void BFSTraverse(GraphAdjList GL)
   {
       int i;
       EdgeNode *p;
       Queue Q; // 临时存储顶点下标用队列

       // 初始化 visited 数组
       for (i = 0; i < GL->numVertexes; i++)
           visited = false;

       InitQueue(&Q);
       for (i = 0; i < GL->numVertexes; i++)
       {
           if (!visited[i])
           {
               visited[i] = true;

               printf("%c", GL->adjList[i].data); // 打印顶点, 也可以是其他操作

               EnQueue(&Q, i); // 起始顶点加入队列

               while (!QueueEmpty(Q))
               {
                   DeQueue(&Q, &i);

                   // 遍历该顶点的边表
                   p = GL->adjList[i].firstEdge;
                   while (p)
                   {
                       if (!visited[p->adjVex]) // 若此顶点未被访问
                       {
                           visited[p->adjVex] = true;

                           printf("%c", GL->adjList[p->adjVex].data); // 打印顶点, 也可以是其他操作

                           EnQueue(&Q, p->adjVex); // 将此顶点加入列
                       }
                       p = p->next;
                   }
               }
           }
       }
   }
   ```

   时间复杂度分析:
   图的深度优先算法的时间复杂度和广度优先算法一样, 不同之处仅在于对顶点的访问次序.
   深度优先算法更适合目标比较明确的, 以找到目标为主要目的的情况;
   而广度优先算法更适合在不断扩大的遍历范围时找到相对最优解的情况

### 最小生成树

最小生成树: 将一个连通加权无向图变为生成树(即只剩 n-1 条边), 其中权值总和最小的生成树就叫最小生成树

1. 普里姆(Prim)算法
   TODO: 补充图片

   思路:
   设 adjVex[j] 存储当前到顶点 j 权值最小的顶点, lowCost[j] 存储当前已知的到顶点 j 最小的权值.
   将 adjVex 和 lowCost 初始化为顶点 v_0 到其他点的权值(读取邻接矩阵第 v_0 行)
   从 v_0 开始, 将 v_0 到附近顶点的权重计入数组 lowCost, 将 lowCost 中权重最小的下标作为最小生成树的下一个顶点(假设为 v_k)
   再将 v_k 到附近的点(假设为 v_j)的权重与当前数组 lowCost[j] 对应的值比较, 如果 v_k 到 v_j 的权重更小则计入 lowCost[j].
   重复, 将 lowCost 中权重最小的下标作为生成树的下一个顶点...直到所有顶点都被纳入最小生成树中(每次循环增加一个顶点, 所以循环 MAXVEX 次).

   实现代码如下:
   ```C++
   #define INFINITY 65535

   // Prim 算法生成最小生成树
   void MiniSpanTree_Prim(MGraph MG)
   {
       int min, i, j, k;
       int adjVex[MAXVEX]; // 存储当前到顶点 j 权值最小的顶点
       int lowCost[MAXVEX]; //  存储当前已知的到顶点 j 最小的权值.
       adjVex[0] = 0; // 初始顶点为 v_0 顶点
       lowCost[0] = 0; // 值为 0 表示该顶点被纳入最小生成树

       for (i = 1; i < MG.numVertexes; i++) // 遍历除下标为0外的全部顶点
       {
           lowCost[i] = MG.arc[0][i]; // 读取矩阵第一行数据赋给lowCost
           adjVex[i] = 0;
       }

       for (i = 1; i < MG.numVertexes; i++)
       {
           min = INFINITY;
           k = 0;
           
           // 从 lowCost 找当前最小的权值, 以作为生成树的下一个顶点(设下一个顶点为 v_k)
           for (j = 1; j < MG.numVertexes; j++)
           {
               if (lowCost[j] != 0 && lowCost[j] < min) // lowCost[j] != 0 排除已纳入最小生成树的顶点
               {
                   min = lowCost[j];
                   k = j;
               }
           }

           printf("(%d, %d)", adjVex[k], k); // 打印最小生成树的边, 可以是其他操作
           lowCost[k] = 0; // 将 v_k 纳入最小生成树中

           // 找 v_k 到邻接点的权值, 如果更小则更新 lowCost 和 adjVex
           for (j = 1; j < MG.numVertexes; j++)
           {
               // 如果顶点 v_k 到附近的邻接点权值比起始点到那个点小
               if (lowCost[j] != 0 && MG.arc[k][j] < lowCost[j]) // 查找邻接矩阵第 k 行的各个权值
               {
                   adjVex[j] = k; // 从 v_k 点到 v_j 点权值更低
                   lowCost[j] = MG.arc[k][j]; // 设置当前到 v_j 的最小权值
               }
           }
       }
   }
   ```

   时间复杂度分析:
   由算法代码中的循环嵌套可得知此算法的时间复杂度为 \\(O(n^2)\\)
2. 克鲁斯卡尔(Kruskal)算法
   将邻接矩阵转化为右图所示的边集数组, 将它们按权值从小到大排序, 且 begin < end
   然后按权值从小到大的顺序开始生成最小生成树, 如果这条边的两个顶点都已经被纳入最小生成树(即这两个顶点已经相通), 跳过这条边
   TODO: 补充图片
   
   如何判断两个顶点已经相通呢? 使用并查集思想, 见下面具体代码
   实现代码如下:
   ```C++
   // 边集数组 Edge 的元素结构
   typedef struct
   {
       int begin;
       int end;
       int weight;
   } Edge;

   // 查找该顶点的最终好友
   int Find(int *buddy, int f)
   {
       while (buddy[f] > 0) // 如果该顶点有好友(好友指可以不邻接但是已经连通)
           f = buddy[f]; // 从它的好友开始继续判断还有无别的好友
       return f;
   }

   // Kruskal 算法生成最小生成树
   void MiniSpanTree_Kruskal(MGraph G)
   {
       int i, n, m;
       Edge edges[MAXEDGE]; // 边集数组, MAXEDGE 为原图的边数
       int buddy[MAXVEX];  // 定义数组 buddy 来帮助判断边与边是否形成环路
       // 这里用到了并查集思想
       // 即 buddy[a]==c, buddy[b]==d, 若 buddy[c]==buddy[d] 则说明 a 已经连通 b 

       // 初始化数组 buddy
       for (i = 0; i < G.numVertexes; i++)
       {
           buddy[i] = 0; // 初始化为 0 是因为边集的 end 不可能是 0
       }

       // 依次遍历 edge 数组
       for (i = 0; i < G.numEdges; i++)
       {
           n = Find(buddy, edges[i].begin);
           m = Find(buddy, edges[i].end);
           if (n != m) // n != m 代表这条边两个顶点的最终好友不同(所以还未连通)
           {
               buddy[n] = m; // 将这两个顶点建立好友关系
               printf("(%d, %d) %d", edges[i].begin, edges[i].end, edges[i].weight) // 打印最小生成树的边, 可以是其他操作
           }
       }
   }
   ```

   实际复杂度分析:
   设 n 为边数, 克鲁斯卡尔算法的时间复杂度为 \\(O(n\log n)\\) (其中 Find() 函数时间花费 \\(O(\log n\\))
   克鲁斯卡尔算法主要是针对边来展开, 边数少时效率会非常高, 对于稀疏图有很大的优势;
   普里姆算法对于稠密图, 即边数非常多的情况会更好一些

### 最短路径

非网图的最短路径, 是指两顶点之间经过的边数最少的路径;
网图的最短路径是指两顶点之间经过的边上权值之和最少的路径.
称路径上的第一个顶点是源点, 最好一个顶点是终点
距离就是两顶点间权值之和, 非网图可理解为所有边权值都为 1 的网图

1. 迪杰斯特拉(Dijkstra)算法
   ```C++
   #define MAXVEX 9
   #define INFINITY 65535

   typedef int Patharc[MAXVEX]         // 用于存储最短路径下标的数组
   typedef int ShortPathTable[MAXVEX]; // 用于存储到各点最短路径的权值和

   // v_0 为起始点, P 为用于存储最短路径下标的数组
   void ShortestPath_Dijkstar(MGraph G, int v_0, Patharc P, ShortPathTable D)
   {
       int v, w, k, min;
       int final[MAXVEX]; // final[w] = 1 表示已经求得顶点v_0到 v_w 的最短路径

       // 初始化数据
       for (v = 0; v < G.numVertexes; v++)
       {
           final[v] = 0;
           D[v] = G.arc[v_0][v];
           P[v] = 0;
       }
       D[v_0] = 0;
       final[v_0] = 1;

       for (v = 1; v < G.numVertexes; v++)
       {
           min = INFINITY;
           for (w = 0; w < G.numVertexes; w++)
           {
               if (!final[w] && D[w] < min)
               {
                   k = w;
                   min = D[w];
               }
           }

           final[k] = 1;
           for (w = 0; w < G.numVertexes; w++)
           {
               if (!final[w] && (min+G.arc[k][w] < D[w]))
               {
                   D[w] = min + G.arc[k][w];
                   P[w] = k;
               }
           }
       }
   }
   ```
2. 弗洛伊德(Floyd)算法

### 拓补排序

### 关键路径

## 查找

### 查找概论

### 顺序表查找

### 有序表查找

### 线性索引查找

### 二叉排序树

### 平衡二叉树(AVL树)

### 多路查找树(B树)

### 散列表查找(哈希表)概述

### 散列函数的构造方法

### 处理散列冲突的方法

### 散列表查找实现

## 排序

### 排序的基本概念与分类

### 冒泡排序

### 简单选择排序

### 直接插入排序

### 希尔排序

### 堆排序

### 归并排序

### 快速排序