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
   1. 初始化一个空栈用来对要运算的数字进出使用. 后缀表达式中前三个都是数字, 所以 9, 3, 1 进栈, 如图
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

   如图, 二叉树 a 中, 根结点到 D 的路径长度为 4 . 二叉树 a 的树路径长度为 1+1+2+2+3+3+4+4=20; 二叉树 b 的树路径为 1+1+2+2+2+2+3+3=16

   如果考虑到带权结点, 结点的带权路径长度为: 从该结点到树根之间的路径长度 × 结点的权.
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
   6. 重复步骤2. 将\\(N_3\\) 与C作为新结点T的两个叶子, 由于T是根结点,  完成赫夫曼树的构造
   
   WPL=40x1+30x2+15x3+10x4+5x4=205
   构造赫夫曼树的赫夫曼算法描述:
   1. 根据给定的n个权值 \\(w_1, w_2, \dots, w_n\\) 构成n颗二叉树的集合 \\(F=T_1, T_2, \dots, T_n\\) , 其中每颗二叉树 \\(T_i\\) 中只有一个 \\(w_i\\) 的根结点, 其左右子树均为空.
   2. 在F中选取两颗根结点的权值最小的树作为左右子树构造一棵新的二叉树, 新的二叉树根结点的权值为其左右子树根结点的权重之和.
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
   规定赫夫曼树的左分支代表0, 右分支代表1, 则从根结点到叶子所经过的路径分支组成序列便为该结点对应字符的编码, 也就是赫夫曼编码

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
      入度为 0 相当于树中的根结点, 其余顶点入度为 1 意味着树中非根结点的双亲只有一个.
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
   思路
   设起始点为 v_0, PathWeight 存储 v_0 到各点的最短路径权值和, PathPrior[w] 存储 v_0 到 v_w 最短路径中 v_w 的前驱
   从 v_0 开始, 首先找到 v_0 权值最小的邻接点 v_k, 将其纳入最短路径并接着找 v_k 的邻接点, 继而将 v_k 的邻接点到 v_0 路径的权值纳入 PathWeight (别忘了同时更新 PathPrior)
   这样一番操作之后, v_0 到 v_k 的那些邻接点的权值就不再是 ∞ 了(即被认为和 v_0 连通), 继续按照这种方式遍历剩下的和 v_0 连通的顶点和它们的邻接点, 发现权值和更小的就更新 PathWeight 和 PathPrior. 直到所有顶点都被纳入最短路径
   ```C++
   #define MAXVEX 9
   #define INFINITY 65535

   typedef int PathPrior[MAXVEX]         // 用于存储最短路径对应顶点前驱的数组, 即 PathPrior[w] == k 代表从 v_0 到 v_w 的最短路径中, v_w 的前驱是 v_k (接着持续遍历即可得到完整的最短路径)
   typedef int PathWeight[MAXVEX]; // 用于存储到各点最短路径的权值和

   // v_0 为起始点, PathPrior 为用于存储到各个点最短路径的数组, 
   void ShortestPath_Dijkstar(MGraph G, int v_0, PathPrior pathPrior, PathWeight pathWeight)
   {
       int v, w, k, min;
       int final[MAXVEX]; // 标记已求得最短路径的顶点, final[w] = 1 表示已经求得顶点v_0到 v_w 的最短路径

       // 初始化这三个数组
       for (v = 0; v < G.numVertexes; v++)
       {
           final[v] = 0;
           pathWeight[v] = G.arc[v_0][v]; // 将 v_0 到各点的权值依次赋给 pathWeight
           pathPrior[v] = 0;
       }

       final[v_0] = 1; // v_0 到 v_0 不需要路径

       for (v = 1; v < G.numVertexes; v++)
       {
           min = INFINITY;
           // 遍历 v_0 当前到各个顶点的权值, 记录 v_0 权值最小的(未被记录到路径中的)邻接点
           for (w = 0; w < G.numVertexes; w++)
           {
               if (!final[w] && pathWeight[w] < min)
               {
                   k = w;
                   min = pathWeight[w];
               }
           }

           final[k] = 1; // 将此邻接点纳入最小路径

           // 此时 v_k 为 v_0 附近(刚纳入最小路径)权值最小的邻接点
           for (w = 0; w < G.numVertexes; w++)
           {
               // 遍历 v_k 到各点(设为 v_w)的权值, 加上 v_i 到 v_k 的权值, 然后与 pathWeight 当前存储的到 v_w 的值比较
               // 如果比现有记录的短, 更新到 pathWeight 和 pathPrior 中
               if (!final[w] && (G.arc[k][w] + min < pathWeight[w]))
               {
                   pathWeight[w] = min + G.arc[k][w];
                   pathPrior[w] = k;
               }
           }
       }
   }
   ```

   时间复杂度分析: 从嵌套循环得到此算法的时间复杂度为 \\(O(n^2)\\)
2. 弗洛伊德(Floyd)算法
   该算法可求得图中任意顶点到顶点间的最短路径, 用矩阵(二维数组) PathMatrix 和 PathWeight 存储
   原理也很简单, 具体实现如下:
   ```C++
   typedef int PathMatrix[MAXVEX][MAXVEX];
   typedef int PathWeight[MAXVEX][MAXVEX];

   void ShortestPath_Floyd(MGraph G, PathMatrix pathMatrix, PathWeight pathWeight)
   {
       int v, w, k;
       for (v = 0; v < G.numVertexes; v++)
       {
           for (w = 0; w < G.numVertexes; w++)
           {
               pathWeight[v][w] = G.arc[v][w]; // 初始化与网图的邻接矩阵保持一致
               pathMatrix[v][w] = w; // 初始化为 pathMatrix[v][j] == j
           }
       }

       // 每次试探一个中继顶点都会将表中所有顶点间的当前路径检测一遍, 所有不用担心会有漏的. (因此嵌套顺序不能变)
       for (k = 0; k < G.numVertexes; k++)
       {
           for (v = 0; v < G.numVertexes; v++)
           {
               for (w = 0; w < G.numVertexes; w++)
               {
                   // 如果从 v_v 到 v_w, 经过下标为 k 的顶点路径比当前记录的更短
                   if (pathWeight[v][w] > pathWeight[v][k] + pathWeight[k][w])
                   {
                       pathWeight[v][w] = pathWeight[v][k] + pathWeight[k][w];
                       pathMatrix[v][w] = pathMatrix[v][k]; // 路径设置经过顶点 k
                   }
               }

           }
       }
   }
   ```

   输出弗洛伊德算法得到的最短路径:
   ```C++
   for (v = 0; v < G.numVertexes; v++)
   {
       for (w = v + 1; w < G.numVertexes; w++)
       {
           printf("v%d-v%d weight: %d", v, w, pathWeight[v][w]); // 打印这条最短路径的权
           k = pathMatrix[v][w];
           printf(" path: %d", v); // 打印源点
           while (k != w) // 如果路径顶点不是终点
           {
               printf(" -> %d", k); // 打印路径顶点
               k = pathWeight[k][w]; // 获得下一个路径顶点
           }
           printf(" -> %d\n", w); // 循环结束, 说明抵达终点, 打印终点
       }
       printf("\n");
   }
   ```

   时间复杂度分析: 时间复杂度为 \\(O(n^3)\\)

### 拓补排序



1. 拓扑排序介绍
   无环: 没有回路的图
   AOV 网(Activity On Vertex Network): 用顶点表示活动, 用弧表示活动之间的优先关系. AOV 网中不能存在回路, 属于有向无环图

   如图所示就是一个 AOV 网(图中省略了权值)和它的邻接表形式数据结构
   TODO: 补充图片(在拓扑排序算法那里)

   拓扑序列:
   设 G=(V, E) 是一个具有 n 个顶点的有向图
   满足若从顶点 \\(v_i\\) 到 \\(v_j\\) 有一条路径, 则在顶点序列 V 中 \\(v_i\\) 必在 \\(v_j\\) 之前.
   称 V 为拓扑序列

   拓扑排序: 对一个有向图构造拓扑序列的过程.
   构造时会有两个结果:
   * 如果此网的全部顶点都被输出, 说明这个网图不存在环(回路), 是 AOV 网
   * 如果输出顶点数少了, 说明这个网图存在环(回路), 不是 AOV 网
2. 拓扑排序算法
   拓扑排序的基本思路是:
   从网中选择一个入度为 0 的顶点输出, 然后删去此顶点, 并删除此顶点的出边, 继续重复此步骤, 直到输出全部顶点(是 AOV 网)或者网中不存在入度为 0 的顶点为止(不是 AOV 网)

   以邻接表形式表达该网图. 考虑到算法要查找入度为 0 的顶点, 因此在原来的顶点表结点结构中, 增加一个入度域 in 以记录一个顶点的入度:
   ```C++
   typedef struct EdgeNode // 边表结点
   {
       int adjVex;
       int weight;
       struct EdgeNode *next;
   } EdgeNode;

   typedef struct VertexNode // 顶点表结点
   {
       int in; // 增加了这个入度域
       int data;
       EdgeNode *firstEdge;
   } VertexNode, AdjList[MAXVEX];

   typedef struct
   {
       AdjList adjList;
       int numVertexes, numEdges;
   } GraphAdjList;
   ```

   在算法中还需要栈来存储处理过程中入度为 0 的顶点, 目的是避免每次查找时都要遍历顶点表中有没有入度为 0 的顶点.
   实现代码如下:
   ```C++
   int TopoLogicalSort(GraphAdjList GL)
   {
       EdgeNode *e;
       int i, k, gettop; // gettop 存储当前出栈元素
       
       int top = -1; // 栈的 top 指针
       int count = 0; // 用于统计输出顶点数
       int *stack = (int*) malloc(GL->numVertexes * sizeof(int)); // 用于存储入度为 0 的顶点, 根据顶点数分配内存空间

       // 遍历顶点表, 将入度为 0 的顶点入栈
       for (i = 0; i < GL->numVertexes; i++)
           if (GL->adjList[i].in == 0)
               stack[++top] = i;

       while (top != -1)
       {
           gettop = stack[top--]; // 出栈
           count++; // 统计输出顶点数
           printf("%d -> ", GL->adjList[gettop].data); // 打印此顶点, 可更改为其他操作

           for (e = GL->adjList[gettop].firstEdge; e; e = e->next) // 遍历此顶点的出边(中的邻接点)
           {
               k = e->adjVex;
               if (--GL->adjList[k].in == 0) // 若失去此顶点后邻接点的入度变为 0, 将邻接点也加入到栈中
                   stack[++top] = k;
           }
       }
       
       if (count < GL->numVertexes) // 如果count小于顶点数, 说明存在环, 不是 AOV 网
           return -1;
       else
           return OK;
   }
   ```

   时间复杂度分析: 对于具有 n 条顶点 e 条弧的网图, 时间复杂度为 O(n+e)

### 关键路径

AOE 网(Activity On Edge Network): 用顶点表示时间, 用有向边表示活动, 用边上的权值表示活动的持续时间. AOE 网是有向图

AOE 网中没有入边的顶点称为始点或源点, 没有出边的顶点称为终点或汇点

如图, \\(v_0\\) 是源点, 表示一个工程的开始, \\(v_9\\) 是汇点, 表示整个工程的结束, 顶点 \\(v_0, v_1, \dots, v_9\\) 分别表示事件, 弧 \\(<v_0, v_1>\\), \\(<v_0, v_2>\\), ..., \\(<v_8, v_9>\\) 都表示一个活动, 用 \\(a_0, a_1, \dots, a_12\\) 表示, 它们的值(权值)代表着活动持续的事件
TODO: 补充图片

AOV 与 AOE 的区别
AOV 用顶点表示活动, 它只描述活动之间的制约关系;
AOE 用边表示活动, 要建立在活动之间制约关系没有矛盾的基础之上. 用于分析完成整个工程至少需要多少时间, 或者为了缩短工程所需时间, 应当加快哪些活动等问题.
TODO: 补充图片(2张)

**关键路径: 从源点到汇点具有最大长度的路径叫关键路径**

1. 关键路径算法原理
   定义如下几个参数:
   1. 事件最早发生时间 etv (earliest time of vertex)
   2. 事件最晚发生时间 ltv (latest time of vertex)
   3. 活动最早开工时间 ete (earliest time of edge)
   4. 活动最晚开工时间 lte (latest time of edge)
   
   某条路径上的活动, 最早开工时间和最晚开工时间如果相等意味着该路径上的活动不可延后, 是关键活动, 该路径为关键路径
   由 1 和 2 可以求得 3 和 4, 然后根据 ete[k] 是否与 lte[k] 相等来判断 \\(a_k\\) 是否是关键活动
2. 关键路径算法
   以邻接表结构表达 AOE 网, 弧链表增加了 weight 域, 用来存储弧的权
   TODO: 补充图片

   计算顶点 \\(v_k\\) 的 etv[k] 的数学定义是:
   TODO: 补充图片

   求事件最早发生事件 etv 的过程, 可放在拓扑排序算法中. 因此在求关键路径之前, 需要先调用一次拓扑排序算法来得到 etv 和拓扑序列列表.
   秘诀: 从前面加权是最早时间, 从后面减权是最晚时间
   实现代码如下:
   ```C++
   int *etv, *ltv; // 事件最早发生时间和最晚发生时间数组
   int *stack2; // 用于存储拓扑序列的栈
   int top2; // 用于栈 stack2 的指针

   // 改进过的拓扑排序算法
   int TopoLogicalSort(GraphAdjList GL)
   {
       EdgeNode *e;
       int i, k, gettop;

       int top = -1; // 栈的 top 指针
       int count = 0; // 用于统计输出顶点个数
       int *stack = (int*) malloc(GL->numVertexes * sizeof(int)); // 用于存储入度为 0 的顶点, 根据顶点数分配内存空间

       // 遍历顶点表, 将当前入度为 0 的顶点入栈
       for (i = 0; i < GL->numVertexes; i++)
           if (GL->adjList[i].in == 0)
               stack[++top] = i;

       etv = (int*) malloc(GL->numVertexes * sizeof(int)); // 给 etv 分配空间
       for (i = 0; i < GL->numVertexes; i++)
           etv[i] = 0; // 初始化 etv 数组, 每个顶点的 etv 默认为 0

       stack2 = (int*) malloc(GL->numVertexes * sizeof(int)); // 给存储拓扑序列的栈分配空间
       top2 = -1; // 空栈的 top 指针默认为 -1

       while (top != -1)
       {
           gettop = stack[top--]; // 将入度为 0 的顶点出栈
           count++;
           stack2[++top2] = gettop; // 将弹出的顶点下标压入存储拓扑序列的栈 stack2 中

           // 遍历当前顶点的弧链表, 看是否有失去当前结点后入度为 0 的邻接点
           for (e = GL->adjList[gettop].firstEdge; e; e = e->next)
           {
               k = e->adjVex; // k 为当前顶点的邻接点
               if (--GL->adjList[k].in == 0) // 若 k 的入度 -1 后为 0
                   stack[++top] = k; // 入栈

               // 若当前顶点(gettop)的 etv 加上当前顶点到 k 所需的时间大于已记录的 etv[k]
               if ((etv[gettop] + e->weight) > etv[k])
                   etv[k] = etv[gettop] + e->weight; // 更新 etv[k], 因为 k 的前置活动(顶点 gettop)首先要完成
           }
       }

       if (count < GL->numVertexes) // 如果 count 小于顶点数, 说明存在环
           return -1;
       else
           return OK;
   }

   void CriticalPath(GraphAdjList GL)
   {
       EdgeNode *e;
       int i, gettop, k, j;
       int ete, lte; // 活动最早发生时间和最晚发生时间

       TopoLogicalSort(GL); // 求拓扑序列, 得到 etv 和 stack2

       ltv = (int*) malloc(GL->numVertexes * sizeof(int)); // 事件的最晚发生时间

       for (i = 0; i < GL->numVertexes; i++)
           ltv[i] = etv[GL->numVertexes - 1]; // 初始化 ltv 数组, 每个顶点的 ltv 默认为最后一个事件发生的时间

       while (top2 != -1)
       {
           gettop = stack2[top2--]; // 拓扑序列出栈(序列从后往前遍历)

           // 遍历顶点 gettop 的弧链表
           for (e = GL->adjList[gettop].firstEdge; e; e = e->next)
           {
               k = e->adjVex; // 顶点 k 为顶点 gettop 的下一个事件

               // 若该顶点 gettop 到下一个顶点 k 的最晚时间小于当前记录的最晚发生时间
               if (ltv[k] - e->weight < ltv[gettop])
                   ltv[gettop] = ltv[k] - e->weight; // 更新 ltv[gettop]
           }
       }
       
       for (j = 0; j < GL->numVertexes; j++)
       {
           for (e = GL->adjList[j].firstEdge; e; e = e->next)
           {
               k = e->adjVex;
               ete = etv[j]; // 活动(当前的弧)最早开工时间等于(当前弧尾)最早发生时间
               lte = ltv[k] - e->weight; // 活动(当前的弧)最晚开工时间 为 下个事件(弧头)的最晚发生事件减去当前活动所需时间
               if (ete == lte) // 如果最早开工时间和最晚开工时间相同(开工时间无法延后), 说明当前路径为关键路径
                   printf("<v%d, v%d> length: %d, ", GL->adjList[j].data, GL->adjList[k].data, e->weight);
           }
       }
   }
   ```

   时间复杂度分析:
   拓扑排序 \\(O(n+e)\\) + 初始化 ltv \\(O(n)\\) + 求 ltv \\(O(n+e)\\) + 检测是否关键路径 \\(O(n+e)\\)
   所以最早时间复杂度依然是 \\(O(n+e)\\)

## 查找

### 查找概论

查找(Searching)就是根据给定的值, 在查找表中确定给定值与记录的关键字相同的记录(数据元素)

1. 关键字(Key): 记录中某个数据项的值, 又称键值
   主关键字(Primary Key): 唯一地标识一个记录的关键字(用主关键字查找只会返回唯一的一条记录)
   次关键字(Secondary Key): 多个记录共有的关键字(用次关键字查找会返回所有含有该关键字的记录)
2. 查找表(Search Table): 由同一类型的记录构成的集合
   * 静态查找表(Static Search Table): 只作查找操作的查找表
   * 动态查找表(Dynamic Search Table): 可在查找过程的同时插入查找表中不存在的记录, 或者从查找表中删除已存在的某个记录

### 顺序表查找

顺序查找(Sequential Search)又叫线性查找, 是最基本的查找技术

1. 查找过程是:
   1. 从表中第一个(或最后一个)记录开始, 逐个进行对给定值和记录的关键字比较
   2. 若某个记录的关键字和给定值相等, 则查找成功, 找到所查的记录
   3. 如果直到最后一个(或第一个)记录, 没有与给定值相等的关键字, 则表中没有所查的记录, 查找失败
2. 顺序表查找算法:
   ```C++
   int Sequential_Search(int *searchTable, int length, int key)
   {
       int i;

       for (i = 1; i <= length; i++)
           if (searchTable[i] == key)
               return i; // 若查找成功, 返回查找到的记录在记录表中的下标

       return -1; // 查找不成功
   }
   ```
3. 顺序表查找优化
   设置一个哨兵, 可以免去 for 循环本身的比较
   ```C++
   // 查找表 searchTable 的第一个下标需要预留给哨兵
   int Sequential_Search(int *searchTable, int length, int key)
   {
       int i;
       searchTable[0] = key; // 设置哨兵, 它的关键字为给定值

       // 循环从数组尾部开始
       i = length - 1;
       while (searchTable[i] != key)
           i--;

       return i; // 若返回的是哨兵的未知(下标0), 说明查找失败
   }
   ```
4. 时间复杂度分析:
   时间复杂度为 \\(O(n)\\), 顺序查找算法在 n 很大时, 查找效率极为低下.
   优点是算法非常简单, 对静态查找表的记录没有任何要求.
   在一些小型数据的查找时, 是可以适用的.
   如果对表进行排序, 将查找频率高的记录放在前面, 不常用的放在后面, 效率可以大幅提高.

### 有序表查找

1. 折半查找(Binary Search)技术, 又称为二分查找.
   前提是线性表中的记录必须是关键字有序(通常从小到大有序), 且线性表必须采用顺序存储.

   折半查找的思想:
   在有序表中, 取中间记录作为比较对象,
   若给定值与中间记录的关键字相等, 则查找成功;
   若给定值小于中间记录的关键字, 则在中间记录的左半区继续查找;
   若给定值大于中间记录的关键字, 则在中间记录的右半区继续查找.
   不断重复上述过程, 直到查找成功, 或者没有所查的记录, 查找失败

   实现代码如下:
   ```C++
   int Binary_Search(int *searchTable, int length, int key)
   {
       int low, high, mid;
       low = 0;                      // 定义最低下标为查找表首位
       high = length - 1; // 定义最高下标为查找表末尾

       while (low <= high)
       {
           mid = (low + high) / 2; // 折半

           if (key < searchTable[mid]) // 若给定值比当前中值小
               high = mid - 1;
           else if (key > searchTable[mid]) // 若给定值比当前中值小大
               low = mid + 1;
           else
               return mid; // 若相等返回该下标
       }

       return -1; // 循环结束, 没有找到所查记录, 查找失败
   }
   ```
   折半算法的时间复杂度为 \\(O(\log n)\\)
2. 插值查找(Interpolation Search)
   折半查找可改进为 \\(\text{mid}=\text{low}+\frac{\text{key}-\text{searchTable[low]}}{\text{searchTable[high]-searchTable[low]}}(\text{high}-\text{low})\\)
   即通过计算 key 在当前 (high-low) 这段中的大致位置, 可以更快的跳转到目标位置.
   在折半查找算法的代码中改动如下
   ```diff
   - mid = (low + high) / 2; // 折半
   + mid = low + (key - searchTable[low]) / (searchTable[high] - searchTable[low]) * (high - low)
   ```
   插值查找时间复杂度同样为 \\(O(\log n)\\)
   对于表长较大, 关键字分布又比较均匀的查找表, 插值查找算法的平均性能比折半查找要好得多
3. 斐波那契查找
   根据斐波那契数列的性质来二分
   TODO: 补充图片(需要将图中-1的部分修掉)
   ```C++
   // Fibonacci[k] 为斐波那契数列, | 0 | 1 | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 | 55 | 89 | 144 |
   int Fibonacci_Search(int *searchTable, int length, int key)
   {
       int low, high, mid, i, k;
       low = 0;
       high = length - 1;
       k = 0;

       while (length > Fibonacci[k] - 1) // 取得 n 位于斐波那契数列中的位置
           k++;

       for (i = length, i < Fibonacci[k]; i++) // 数组的大小要与斐波那契查找对应值相等, 将不满的用最后一个记录的值补全
           searchTable[i] = searchTable[length - 1];

       while (low <= high)
       {
           mid = low + Fibonacci[k - 1];
           if (key < searchTable[mid]) // 若查找记录小于当前分隔记录
           {
               high = mid - 1;
               k--; // 斐波那契数列下标减一位
           }
           else if (key > searchTable[mid]) // 若查找记录大于当前分隔记录
           {
               low = mid + 1;
               k -= 2; // 斐波那契数列下标减两位, 因为Fibonacci[k-2]+Fibonacci[k-1]=Fibonacci[k] 
           }
           else
           {
               if (mid <= length)
                   return mid; // 若相等则说明 mid 即为查找到的位置
               else
                   return length - 1; // 若 mid > n 说明是补全数值, 返回 n
           }
       }

       return -1;
   }
   ```
   时间复杂度同样为 \\(O(\log n)\\)
   就平均性能来说, 斐波那契查找要优于折半查找. 如果是最坏情况， 比如要查找的记录在查找表的极左侧, 则斐波那契查找效率要低于折半查找.

### 线性索引查找

索引: 把一个关键字与它对应的记录相关联的过程, 一个索引由若干个索引项构成, 每个索引项至少应包含关键字和其对应的记录在存储器中的位置信息.
线性索引: 将索引项集合组织为线性结构, 也称为索引表

1. 稠密索引
   每个记录对应一个索引项的叫稠密索引
   对于稠密索引表来说, 索引项一定是按照关键字有序的排列.
   TODO: 补充图片
2. 分块索引
   把数据集中的记录分为若干块, 将每块对应一个索引项, 并且这些块需要满足两个条件:
   * 块内无序, 每一块内的记录不要求有序
   * 块间有序, 比如第二块中所有记录的关键字都要大于第一块中所有记录的关键字
   
   分块索引表的索引项结构分三个域:
   * 最大关键字, 存储块中最大关键字(特性: 下一块中的最小关键字也比这一块中最大的关键字要大)
   * 块长, 即块中有多少记录
   * 指针域, 指向关联的块

   TODO: 补充图片

   在分块索引表中查找分两步进行:
   1. 在分块索引表中查找给定值所在的块. 由于分块索引表是块间有序的, 可利用折半、插值等算法确定目标记录所在的块.
   2. 在块中顺序查找关键码. 因为块内无序, 只能顺序查找.

   分块索引的平均查找长度:
   设 n 个记录的数据集被平均分成 m 块, 每个块中有 t 条记录, 显然 \\(m=\frac{n}{t}\\) .
   \\(L_b\\) 为在索引表查找块的平均查找长度, 因最好(1 次)与最差(m 次)的等概率原则, \\(L_b\\) 的平均长度为 \\(\frac{1+m}{2}\\) .
   \\(L_w\\) 为在块中查找记录的平均查找长度, 同理可知平均查找长度为 \\(\frac{1+t}{2}\\) .
   这样分块索引查找的平均查找长度 \\(ASL_w\\) 为:
   \\(ASL_w=L_b+L_w=\frac{1+m}{2}+\frac{1+t}{2}=\frac{1}{2}(m+t)+1=\frac{1}{2}(\frac{n}{t}+t)+1\\)
   
   最佳情况是分的块数与块中记录相等(即 \\(m=t\\) )， 此时 \\(n=m\cdot t=t^2\\)
   \\(ASL_w=\frac{1}{2}(\frac{n}{t}+t)+1=t+1=\sqrt{n}+1\\\)

3. 倒排索引
   多个次关键字指向同一个记录, 一个次关键字指向多个记录
   索引项的通用结构是:
   * 次关键字, 如图中的"英文单词"
   * 记录号表, 存储具有该次关键字的所有记录的记录号(可以是指向记录的指针或者是该记录的主关键字), 如图中的"文章编号"
   
   TODO: 补充图片
   
   倒排索引源于实际应用中需要根据属性(或字段、次关键码)的值来查找记录. 这种索引表中的每一项都包括一个属性值和具有该属性值的各记录的地址. 由于不是由记录来确定属性值, 而是由属性值来确定记录的位置, 因而称为倒排索引

### 二叉排序树

设集合 {62, 88, 58, 47, 35, 73, 51, 99, 37, 93}
将其变为一颗中序遍历序列为 {35, 37, 47, 51, 58, 62, 73, 88, 93, 99} 的**二叉排序树**
TODO: 补充图片, 没懂写的啥

二叉排序树(Binary Sort Tree), 又称为二叉查找树.
二叉排序树具有以下性质:
* 若左子树不空, 则左子树上所有结点的值均小于它的根结点
* 若右子树不空, 则右子树上所有结点的值均大于它的根结点
* 左、右子树也为二叉排序树

1. 二叉排序树查找操作:
   ```C++
   /**
   * 递归查找二叉排序树 T 中是否存在 key
   * 指针 f 指向 T 的前驱(双亲), 初始值为 NULL
   * 若查找成功, 指针 p 指向该数据结点并返回 TRUE
   * 若查找失败, 指针 p 指向查找路径上访问的最后一个结点并返回 FALSE
   */
   int SearchBST(BiTree T, int key, BiTree f, BiTree *p)
   {
       if (!T) // 如果已经没有结点
       {
           *p = f; // 查找失败, 指向上一结点的地址
           return FALSE;
       }
       else if (key == T->data) // 如果当前结点的值等于给定值, 查找成功
       {
           *p = T;
           return TRUE;
       }
       else if (key < T->data) // 否则继续查找下一结点
           return SearchBST(T->lchild, key, T, p); // 在左子树中继续查找
       else
           return SearchBST(T->rchild, key, T, p); // 在右子树中继续查找
   }
   ```
2. 二叉排序树插入操作(附二叉排序树的创建)
   ```C++
   int InsertBST(BiTree *T, int key)
   {
       BiTree p, s;

       if (!SearchBST(*T, key, NULL, &p)) // 调用后指针 p 会指向查找路径上访问的最后一个结点
       {
           // 创建新结点
           s = (BiTree) malloc(sizeof(BiTNode));
           s->data = key;
           s->lchild = s->rchild = NULL;

           if (!p) // 若经过 SearchBST() 后 p 为 null, 说明该树中没有结点 
               *T = s; // 插入s为新的根结点
           else if (key < p->data) // 否则根据与双亲结点的比较决定是左孩子还是右孩子
               p->lchild = s; // 插入 s 为 p 的左孩子
           else
               p->rchild = s; // 插入 s 为 p 的右孩子
           return TRUE;
       }
       else
           return FALSE; // 如果已有相同结点, 返回 False
   }
   ```

   实现二叉排序树的创建:
   ```C++
   int i;
   int a[10] = { 62, 88, 58, 47, 35, 73, 51, 99, 37, 93 };

   // 新建二叉树 T
   BiTree T = (BiTree) malloc(sizeof(BiTNode));
   for (i = 0; i < 10; i++) // 遍历数组并插入为新结点
       InsertBST(&T, a[i])
   ```
3. 二叉排序树删除操作
   * 情况1: 如果待删结点是叶子, 直接将双亲结点的对应后继设为 null 即可
     TODO: 补充图片
   * 情况2: 如果待删结点只有左子树或只有右子树, 将它的左子树或右子树整个移动到被删除结点的位置
     TODO: 补充图片
   * 情况3: 如果待删结点既有左右子树均不空, 根据二叉树的中序序列
     * 用该结点在序列中的前驱代替该结点, 并删除前驱, 接上该前驱的子树;
     * 或者用该结点在序列中的后继代替该结点, 并删除后继, 接上该后继的子树
     
     TODO: 补充图片
   
   具体先根据关键值找到该结点, 然后再根据以上三种情况作处理
   实现代码如下:
   ```C++
   int Delete(BiTree *p)
   {
       BiTree q, s;

       if ((*p)->rchild == NULL) // 右子树空则只需重接它的左子树(待删结点是叶子也走此分支)
       {
           q = *p;
           *p = (*p)->lchild;
           free(q);
       }
       else if ((*p)->lchild == NULL) // 只需重接它的右子树
       {
           q = *p;
           *p = (*p)->rchild;
           free(q);
       }
       else // 左右子树均不空
       {
           q = *p;
           s = (*p)->lchild; // s 初始化为待删结点的左子树

           while (s->rchild) // 找待删结点在中序序列中的前驱(他的左子树中最右孩子或者左子树根结点(如果左子树根结点没有右孩子)), 用变量 s 记录, 用 q 记录 s 的双亲
           {
               q = s;
               s = s->rchild;
           }

           (*p)->data = s->data; // 用前驱代替待删结点

           if (q == *p) // 前驱的双亲等于待删结点, 意味着待删结点的左子树根结点没有右孩子
               q->lchild = s->lchild; // 接上待删结点左子树的左子树
           else
               q->rchild = s->lchild; // 接上最右孩子的左子树(因为最右孩子肯定没有右孩子)

           free(s); // 清理待删结点的前驱
       }
   }

   int DeleteBST(BiTree *T, int key)
   {
       if(!*T) // 如果已经没有结点, 说明查找失败
           return FALSE;
       else
       {
           if (key == (*T)->data) // 如果找到关键字等于 key 的结点
               return Delete(T); // 调用 Delete() 删除该结点
           else if (key < (*T)->data)
               return DeleteBST(&(*T)->lchild, key);
           else
               return DeleteBST(&(*T)->rchild, key);
       }
   }
   ```
4. 二叉排序树总结
   二叉排序树是以链表的方式存储, 保持了链式存储结构在执行插入或删除操作时不用移动元素的优点, 只要找到合适的插入和删除位置后, 仅需修改链接指针即可
   额外特性: 对于二叉排序树的查找, 走的就是从根结点到要查找的结点的路径, 其比较次数等于给定值的结点在二叉排序树的层数. 最少为1次, 即根结点就是要找的结点, 最多不会超过树的深度.

### 平衡二叉树(AVL树)

TODO: 这节未完成

平衡二叉树(Self-Balancing Binary Search Tree 或 Height-Balanced Binary Search Tree), 是一种二叉排序树, 其中**每一个结点的左子树和右子树的高度差至多等于 1**

**二叉树结点的左子树深度减去右子树深度的值称为平衡因子 BF(Balance Factor)**, 平衡二叉树上所有结点的平衡因子只可能是 -1、0、1.

最小不平衡树: 如图, 当新插入结点37时, 距离它最近的平衡因子绝对值超过 1 的结点是 58 (58左子树高度3, 右子树高度 1), 所以从 58 为跟的子树为最小不平衡树
TODO: 补充图片

1. 平衡二叉树实现原理
   构建平衡二叉树的基本思想就是在构建过程中, 每当插入一个结点时, 先检查是否因插入而破坏了树的平衡.
   若是, 则找出最小不平衡树. 在保证二叉排序树特性的前提下, 调整最小不平衡子树中各结点之间的链接关系, 进行相应的旋转, 使之成为新的平衡子树.
   
2. 平衡二叉树实现算法
   平衡二叉树结构定义代码如下:
   ```C++
   typedef struct BiTNode // 结点结构
   {
       int data;
       int bf; // 结点平衡因子
       struct BiTNode *lchild, *rchild;
   } BiTNode, *BiTree;
   ```

   实现代码如下:
   ```C++
   // 右旋操作
   void R_Rotate(BiTree *P)
   {
       BiTree L;
       L = (*P)->lchild; // L 为 P 的左子树根结点
       (*P)->lchild = L->rchild; // 将 P 左子树的右子树挂接为 P 的左子树
       L->rchild = (*P); // 将 L 的右子树根结点设为 P
       *P = L; // P 的新根结点为 L
   }

   // 左旋操作
   void L_Rotate(BiTree *P)
   {
       BiTree R;
       R = (*P)->rchild; // R 为 P 的右子树根结点
       (*P)->rchild = R->lchild; // 将 P 右子树的左子树挂接为 P 的右子树
       R->lchild = (*P); // 将 R 的左子树根结点设为 P
       *P = R; // P 的新根结点为 R
   }

   #define LH 1
   #define EH 0
   #define RH -1

   // 对二叉树 T 作左平衡旋转(即树的左边高于右边的情况, 此时 T->bf == 2)
   void LeftBalance(BiTree *T)
   {
       BiTree L, Lr;
       L = (*T)->lchild; // L 指向 T 的左子树根结点

       switch (L->bf) // 检查 T 的左子树平衡因子
       {
           case LH: // 新插入结点在 T 的左孩子的左子树上, 要作右旋处理
               (*T)->bf = L->bf = EH; // 右旋后 T, L 结点均变为平衡(由作图分析得到)
               R_Rotate(T);
               break;
           case RH: // 新插入结点在 T 的左孩子的右子树上, 要作双旋处理(左子树左旋, 根结点右旋)
               Lr = L->rchild; // Lr 指向 T 的左孩子的右子树根
               switch (Lr->bf) // 根据Lr 的平衡因子设定双旋后 T, L 结点的平衡情况(由作图分析得到)
               {
                   case LH:
                       L->bf = EH;
                       (*T)->bf = RH;
                       break;
                   case EH:
                       (*T)->bf = L->bf = EH;
                       break;
                   case RH:
                       L->bf = LH;
                       (*T)->bf = EH;
                       break;
               }
               Lr->bf = EH;
               L_Rotate(&(*T)->lchild); // 将 T 的左子树左旋, Lr 变为左子树根结点
               R_Rotate(T); // 将 T 右旋, Lr 变为根结点
       }
   }

   int InsertAVL(BiTree *T, int e, bool *taller)
   {
       if (!*T)
       {
           *T = (BiTree) malloc(sizeof(BiTNode));
           (*T)->data = e;
           (*T)->lchild = (*T)->rchild = NULL;
           (*T)->bf = EH;
           *taller = true;
       }
       else
       {
           if (e == (*T)->data)
           {
               *taller = false;
               return false;
           }

           if (e < (*T)->data)
           {
               if (!InvertAVL(&(*T)->lchild, e, taller))
                   return false;

               if (taller)
               {
                   switch((*T)->bf)
                   {
                       case LH:
                           LeftBalance(T);
                           *taller = false;
                           break;
                       case EH:
                           (*T)->bf = LH;
                           *taller = true;
                           break;
                       case RH:
                           (*T)->bf = EH;
                           *taller = false;
                           break;
                   }
               }
           }
           else
           {
               if (!InsertAVL(&(*T)->rchild, e, taller))
                   return false;
            
               if(*taller)
               {
                   switch((*T)->bf)
                   {
                       case LH:
                           (*T)->bf = EH;
                           *taller = false;
                           break;
                       case EH:
                           (*T)->bf = RH;
                           *taller = true;
                           break;
                       case RH:
                           RightBalance(T);
                           *taller = false;
                           break;
                   }
               }
           }
       }
       return true;
   }
   ```

### 多路查找树(B树)

### 散列表查找(哈希表)概述

1. 散列表查找定义
   通过某个函数 f, 使得: 存储位置=f(关键字)
   
   散列技术是在记录的的存储位置和它的关键字之间建立一个确定的对应关系, 使得每个关键字 key 对于一个存储位置 f(key)
   函数 f 称为散列函数, 又称哈希(Hash)函数.
   采用散列技术将记录存储在一块连续的存储空间中, 这块连续存储空间称为散列表或哈希表(Hash table). 关键字对应记录的存储位置称为散列地址
   这样查找关键字不需要比较就可获得需要的记录的存储位置
2. 散列表查找步骤
   1. 在存储时, 所有记录都需要经过散列函数计算出地址再存储
   2. 查找记录时, 通过同样的散列函数计算记录的散列地址从而访问该地址

   因此散列计算既是一种存储方法, 也是一种查找方法.
   散列技术最适合的场景是查找某个指定的记录
   散列技术不适合:
   1. 一个关键字对应多条记录的情况
   2. 范围查找
   
   在理想情况下, 不同的关键字, 通过散列函数计算出来的地址都是不一样的,
   可现实中时常会碰到两个关键字 key1≠key2, 但 f(key1)=f(key2) 的情况,
   这种现象称为冲突(collision), 此时 key1 和 key2 称为这个散列函数的同义词(synonym)

### 散列函数的构造方法

两个原则:
* 计算简单. 散列函数的计算时间不应该超过其他查找技术与关键字比较的时间
* 散列地址分布均匀. 尽量让散列地址均匀地分布在存储空间中, 保证存储空间的有效利用, 减少未处理冲突而耗费的时间

1. 直接定址法
   对 0~100 岁的人口数字统计表, 对年龄这个关键字就可以直接用年龄的数字作为地址. 此时 f(key)=key
   TODO: 补充图片
   统计 1980 后出生年份的人口数, 对出生年份这个关键字可以用年份减去 1980 来作为地址. 此时 f(key)=key-1980
   TODO: 补充图片
   可以取关键字的某个线性函数为散列地址, 即 f(key) = a × key + b
   这样的散列函数优点是简单、均匀、不产生冲突, 但问题是需要事先知道关键字的分布情况,
   适合查找表较小且连续的情况, 由于这样的限制, 此方法在现实中并不常用.
2. 数字分析法
   如果关键字是位数较多的数字,
   比如 11 位手机号前三位是接入号, 一般对应不同运营商公司的子品牌;
   中间四位是 HLR 识别号, 表示归属地;
   后四位才是真正的用户号.
   TODO: 补充图片
   
   如果用手机号作为关键字, 极有可能前 7 位都是相同的. 那么选择后四位作为散列地址就是不错的选择.

   数字分析法是使用关键字的一部分来计算散列存储位置的方法, 通常适合处理关键字位数比较大的情况.
   如果事先知道关键字的分布且关键字的若干位分布较均匀, 可以考虑用此方法
3. 平方取中法
   关键字 1234 的平方是 1522756, 抽取中间的三位 227 用作散列地址.
   平方取中法适合于不知道关键字的分布, 而位数又不是很大的情况.
4. 折叠法
   折叠法是将关键字从左到右分割成位数相等的几部分(最后一部分位数不够时可以短些),
   然后将这几部分叠加求和, 并按散列表表长, 取后几位作为散列地址

   关键字 9876543210, 分为四组 |987|654|321|0, 叠加求和 987+654+321+0=1962, 散列表表长为三位(0~999), 得到散列地址为962
   
   有时还不能保证分布均匀, 不妨从一端向另一端来回折叠后对齐相加. 比如 987 和 321 反转, 在与 654 和 0 相加, 变成 789 + 654 + 123 + 0 = 1566, 此时散列地址为 566

   折叠法事先不需要知道关键字的分布, 适合关键字位数较多的情况
5. 除留余数法
   除留余数法为最常用的散列函数方法. 对于散列表长度为 m 的散列函数公式为: f(key)=key % p (p ≤ m)
   
   此方法不仅可以对关键字直接取模, 也可在折叠、平方取中后再取模. 本方法的关键在于选择合适的 p, p 如果选的不好, 可能会容易产生同义词.

   极端情况: 对于如下数据, 让p为12, 所有的关键字都能整除 12, 都得到了 0 这个散列地址
   TODO: 补充图片

   通常选择 p 为小于等于表长的最小质数或质因子不小于 20 的合数
6. 随机数法
   选择一个随机数, 取关键字的随机函数值为它的散列地址. 即 f(key)=random(key)

   random() 是伪随机函数. 当关键字的长度不等时, 采用这个方法构造散列函数比较合适.

应该视不同的情况采用不同的散列函数. 考虑因素:
1. 计算散列地址所需的时间
2. 关键字的长度
3. 散列表的大小
4. 关键字的分布情况
5. 记录查找的频率

### 处理散列冲突的方法

1. 开放地址法
   也称线性探测法. 指一旦发生冲突, 就去寻找下一个空的散列地址, 只要散列表足够大, 空的散列地址总能找到, 并将记录存入
   公式(m 为散列表表长): \\(f_i(\text{key}=(f(\text{key}+d_i) \\% m (1 \leqslant d_i \leqslant m-1)))\\)
   
   堆积: 本来不是同义词却需要争夺一个地址的情况. 堆积的出现需要不断处理冲突, 无论存入还是查找效率都会大大降低
   二次探测法: 给增加平方运算, 目的是为了不让关键字聚集在某一块区域, 减少堆积的出现.
   \\(f_i(key)=(f(key)+d_i) \\% m (d_i=1^2, -1^2, 2^2, -2^2, \dots, q^2, -q^2)(q\leqslant \frac{m}{2})\\)
   
   还有一种方法是在冲突时, 对于位移量 \\(d_i\\) 采用随机函数计算得到, 称之为随机探测法
   
   开放定址法只要在散列表未填满时, 总能找到不发生冲突的地址, 是常用的解决冲突的办法
2. 再散列函数法
   事先准备多个散列函数, 每当发生散列地址冲突时, 就换一个散列函数计算.
   这种方法能够使得关键字不产生聚集, 但也增加了计算的时间
3. 链地址法
   将所有关键字为同义词的记录存储在一个单链表中, 称这种表为同义词子表, 在散列表中只存储所有同义词子表的头指针
   
   举个例子: 对关键字集合 {12, 67, 56, 16, 25, 37, 22, 29, 15, 47, 48, 34}, 散列函数为除留余数法, 用 12 作为除数
   TODO: 补充图片
   
   链地址法提供了绝不会出现找不到地址的保障. 但也带来了查找时需要遍历单链表的性能损耗
4. 公共溢出区法
   增加一个专门存储同义词的溢出表
   
   举个例子: 关键字 37, 48, 34 与之前的关键字位置有冲突, 将它们存储到溢出表
   TODO: 补充图片
   
   查找时, 对给定值通过散列函数计算出散列地址后, 先与基本表的相应位置进行比对, 如果相等, 则查找成功; 如果不相等, 则到溢出表去进行顺序查找.
   如果相对于基本表, 有冲突的数据很少的情况下, 公共溢出区法的性能还是非常高的

### 散列表查找实现

1. 散列表查找算法实现
   HashTable 是散列表结构, 结构中 *elements 为一个动态数组.
   ```C++

   typedef struct
   {
       int *elements; // 数据元素存储基址, 动态数组
       int count; // 当前数据元素个数
   } HashTable;
   int HashTableLength = 0; // 散列表表长, 全局变量
   ```

   对散列表初始化
   ```C++
   #define HASHSIZE 12 // 假设表长为 12
   #define NULLKEY -32768 // 初始键值, 选择一个关键字取不到的值

   int InitHashTable(HashTable *H)
   {
       int i;
       HashTableLength = HASHSIZE // 设置散列表表长
       H->count = HashTableLength;
       H->elements = (int*) malloc(HashTableLength * sizeof(int));

       for (i = 0; i < HashTableLength; i++) // 初始化 elements 数组
           H->elem[i] = NULLKEY;

       return OK;
   }
   ```

   选择一个散列函数, 这里选择了除留余数法:
   ```C++
   int Hash(int key)
   {
       return key % HashTableLength;
   }
   ```

   插入操作函数:
   ```C++
   void InsertHash(HashTable *H, int key)
   {
       int addr = Hash(key);
       while (H->elements[addr] != NULLKEY) // 如果该地址不等于初始值, 说明已被别的键值占有, 冲突了
           addr = (addr + 1) % HashTableLength; // 这里用开放地址法解决冲突

       H->elements[addr] = key;
   }
   ```

   查找操作函数:
   ```C++
   // 查找成功后 *addr 指向该关键字的散列地址
   int SearchHash(HashTable H, int key, int *addr)
   {
       *addr = Hash(key); // 获取散列地址

       while (H.elements[*addr] != key) // 如果该地址下元素与要找的不同, 说明之前发生过冲突
       {
           *addr = (*addr + 1) % HashTableLength; // 因为之前用开放地址法解决冲突, 所有同样用这个方法找散列地址
           // 前者判断在该记录未存入散列表的情况下为 true, 基于开放地址法的特性. (同时散列表也不能删除已存入元素)
           // 后者判断在散列表全满的情况下为 true
           if (H.elements[*addr] == NULLKEY || *addr == Hash(key))
           {
               return ERROR;
           }
       }
       return OK;
   }
   ```
2. 散列表查找性能分析
   散列查找的平均查找长度取决因素:
   1. 散列函数是否均匀
   2. 处理冲突的方法
   3. 散列表的装填因子
      装填因子形容散列表装满的程度, 符号 \\(\alpha=\frac{\text{当前表中记录个数}}{\text{散列表长度}}\\)
      \\(\alpha\\) 越大, 产生冲突的可能性就越大. 冲突越多, 平均查找长度同样也越长.

      解决方法是选择一个合适的装填因子以便将平均查找长度限定在一个范围之内, 此时散列查找的时间复杂度是 \\(O(n)\\)
      通常都是将散列表的空间设置得比查找集合大, 虽然是浪费了一定的空间, 但换来的是查找效率的大大提升

## 排序

### 排序的基本概念与分类

设有 n 个记录的序列 \\(\\{r_1, r_2, \dots, r_n\\}\\)
记录对应的关键字为 \\(\\{k_1, k_2, \dots, k_n\\}\\)
需确定一种排列 \\(p_1, p_2, \dots, p_n\\) 满足 \\(k_{p_1}, k_{p_2}, \dots, k_{p_n}\\) 按顺序或逆序排列
使序列成为按关键字有序的序列 \\(\\{r_{p_1}, r_{p_2}, \dots, r_{p_n}\\}\\)

1. 排序的稳定性
   假设 \\(k_i=j_j \enspace(1\leqslant i , 1\leqslant j\leqslant n , i \neq j)\\)
   且在排序前的序列中 \\(r_i\\) 领先于 \\(r_j\\) .
   如果排序后 \\(r_i\\) 仍领先于 \\(r_j\\) 则称所用的排序方法是稳定的;
   反之, 若可能使得排序后序列中 \\(r_j\\) 领先于 \\(r_i\\) 则称所用的排序方法是不稳定的.
   即两个关键字一样的记录, 排序后顺序不变的说明排序方法是稳定的
   TODO: 补充图片
2. 内排序与外排序
   根据在排序过程中待排序的记录是否全部被放置在内存中, 将排序分为内排序和外排序.
   内排序是在整个排序过程中, 待排序的记录全部放在内存中.
   外排序是由于排序的记录太多, 在整个排序过程中, 需要在内外存之间多次交换数据.

   对于内排序, 排序算法性能主要受 3 个方面影响
   1. 时间性能
      内排序中, 主要进行两种操作: 比较和移动.
      高效率的内排序算法是尽可能少的关键字比较次数和记录移动次数
   2. 辅助空间
      除了存放排序所占用的存储空间之外, 执行算法所需要的其他存储空间
   3. 算法的复杂性 
      指算法代码的复杂度, 不是算法的时间复杂度. 算法过于复杂也会影响排序的性能
3. 排序用到的结构与函数
   排序用的顺序表结构
   ```C++
   #define MAXSIZE 10
   typedef struct
   {
       int r[MAXSIZE + 1]; // r[0] 用作哨兵或临时变量, 所以要 MAXSIZE + 1
       int length;
   } SqList;
   ```
   数组两元素的交换函数
   ```C++
   void swap(SqList *L, int i, int j)
   {
       int temp = L->r[i];
       L->r[i] = L->r[j];
       L->r[j] = temp;
   }
   ```

### 冒泡排序

1. 最简单排序实现
   ```C++
   // 不算标准的冒泡排序, 从小到大排列
   // 思路是让每一个关键字都和它后面的每一个关键字比较, 如果大则交换
   // 这样这个关键字在一次循环后一定变成它后面数中的最小值
   void NotBubbleSort(SqList *L)
   {
       int i, j;
       // 将每个 r[i] 与 i 之后的每个 r[j] 比一遍
       for (i = 1; i < L->length; i++)
       {
           for (j = i + 1; j <= L->length; j++)
           {
               if (L->r[i] > L->r[j])
                   swap(L, i, j);
           }
       }
   }
   ```
   时间复杂度为: \\(\frac{n(n-1)}{2}=O(n^2)\\) , 该算法的效率非常低
2. 冒泡排序算法
   冒泡排序(Bubble Sort)是一种交换排序
   基本思想是: 两两比较相邻记录的关键字, 如果反序则交换, 直到没有反序的记录为止
   正宗的冒泡算法:
   ```C++
   void BubbleSort(SqList *L)
   {
       int i, j;
       for (i = 1; i < L->length; i++)
       {
           // j 从后往前循环, 比较时 j 是前者, j + 1 是后者
           for (j = L->length - 1; j >= i; j--)
           {
               // 若前者大于后者, 交换它们的位置
               if (L->r[j] > L->r[j + 1])
                   swap(L, j, j + 1);
           }
       }
   }
   ```
   每次 j 的循环结束后, r[i] 位置的关键字总是后面的记录中最小的. 每次 j 的循环, 最小的关键字都会向泡泡一个依次"浮"上来
   时间复杂度同样为 \\(O(n^2)\\) 
3. 冒泡排序优化
   增加一个标记变量flag, 如果某轮 j 循环没有任何数据交换, 说明此序列已经有序,不需要再继续后面的排序工作.
   ```C++
   void BubbleSort2(SqList *L)
   {
       int i, j;
       bool flag = true;
       for (i = 1; i < L->length && flag; i++) // 增加对 flag 的判断
       {
           flag = false; // i 循环每轮初始值为 false
           for (j = L->length - 1; j >= i; j--)
           {
               if (L->r[j] > L->r[j + 1])
               {
                   swap(L, j, j + 1);
                   flag = true; // 如果有数据交换, 则 flag 为 true, 下次循环继续
               }
           }
       }
   }
   ```

   设待排序的序列是 {2, 1, 3, 4, 5, 6, 7, 8, 9}
   TODO: 补充图片
4. 冒泡排序复杂度分析
   最好情况: 要排序的表本身就是有序的, 放在冒泡排序优化版只有 n-1 次的比较, 没有数据交换, 时间复杂度为 \\(O(n)\\)
   最坏情况: 排序表是逆序的情况, 此时需要比较 \\(\frac{n(n-1)}{2}\\) 次并作等量的移动操作, 时间复杂度为 \\(O(n^2)\\)

### 简单选择排序

1. 简单选择排序算法
   简单选择排序法(Simple Selection Sort)是通过 n-i 次关键字间的比较, 从 n-i+1 个记录中选出关键字最小的记录, 并和第 i 个记录交换
   ```C++
   void SelectSort(SqList *L)
   {
       int i, j, min;
       for (i = 1; i < L->length; i++)
       {
           min = i; // min 记录当前找到的最小值, 初始化值为 i
           for (j = i + 1; j <= L->length; j++) // 在 i 之后的记录中找最小值
           {
               if (L->r[min] > L->r[j]) // 如果有比 min 小的
                   min = j; // 将该值赋给 min
           }
           if (i != min) // 若 min 不等于 i, 说明找到最小值, 交换
               swap(L, i, min);
       }
   }
   ```
2. 简单选择排序复杂度分析
   简单排序相比冒泡排序的优点是交换记录位置的次数相当少.
   对于交换次数, 最好情况为顺序表交换 0 次, 最坏情况为逆序表 n-1 次.
   但无论最好最差的情况, 其比较次数都是 \\(\frac{n(n-1)}{2}\\) 次.
   因此, 时间复杂度依然为 \\(O(n^2)\\)

   尽管与冒泡排序同为 \\(O(n^2)\\) , 但实际情况简单选择排序的性能还是略优于冒泡排序

### 直接插入排序

1. 直接插入排序算法
   直接插入排序(Straight Insertion Sort)的基本操作是将一个记录插入到已经排好序的有序表中, 从而得到一个新的、记录数增 1 的有序表
   实现代码如下:
   ```C++
   void InsertSort(SqList *L)
   {
       int i, j;
       for (i = 2; i <= L->length; i++)
       {
           // r[i] 为后者, r[i - 1] 为前者, 如果后者小于前者, 说明后者需要插到前面去
           if (L->r[i] < L->r[i - 1])
           {
               L->r[0] = L->r[i]; // 将后者的值存入哨兵
               
               // 将记录后移, 给插入提供空间
               for (j = i - 1; L->r[j] > L->r[0]; j--) // 用 j 遍历 i 前面的记录. 如果 r[j] 大于哨兵的值
                   L->r[j + 1] = L->r[j] // 将该记录后移
            
               L->r[j + 1] = L->r[0] // 循环结束后, j+1 抵消最后一次循环结束后的 j--, 即为合适的插入位置
           }
       }
   }
   ```
2. 直接插入排序复杂度分析
   * 最好情况: 表本身有序, 共比较了 n-1 次, 没有移动记录的操作, 时间复杂度为 \\(O(n)\\)
   * 最坏情况: 表是逆序, 需要比较 \\(2+3+...+n=\frac{(n+2)(n-1)}{2}\\) 次, 记录移动次数 \\(\displaystype\sum_{i=2}^n (i+1)=\frac{(n+4)(n-1)}{2}\\)
   * 评价情况: 根据等概率原则, 评价比较和移动次数约为 \\(\frac{n^2}{4}\\)
   
   因此时间复杂度同样为 \\(O(n^2)\\)
   但直接插入排序法比冒泡和简单选择排序的性能要好

### 希尔排序

1. 希尔排序原理
   将原本的大量记录进行分组, 分割成若干个子序列.
   此时每个子序列按排序的记录个数比较少, 然后在这些子序列内分别进行直接插入排序, 当整个序列都基本有序时, 再对全体记录进行依次直接插入排序

   基本有序: 小的关键字基本在前面, 大的基本在后面, 不大不小的基本在中间

   {2,1,3,6,4,7,5,8,9} 可以称为基本有序. {1,5,9,3,7,8,2,4,6} 的 9 在第三位, 谈不上基本有序, 其中的 {1, 5, 9} 算局部有序

   跳跃分割策略: 将相距某个"增量"的记录组成一个子序列, 以保证直接插入排序后的子序列是基本有序而不是局部有序
2. 希尔排序算法
   ```C++
   void ShellSort(SqList *L)
   {
       int i, j;
       int increment = L->length; // 增量, 初始化为表的长度

       do {
           // 确定增量大小, 每完成一次循环, 增量都会逐步减小
           increment = increment / 3 + 1; // 后面 +1 是为了确保增量大于等于1, 增量等于零时此算法相当于直接插入排序

           for (i = increment + 1; i <= L->length; i++)
           {
               if (L->r[i] < L->r[i - increment]) // 将 i 和前一个子序列对应的值比较, 小则交换位置
               {
                   L-r[0] = L->r[i]; // 暂存在L->r[0]
                   for (j = i - increment; j > 0 && L->r[j] > L->r[0]; j -= increment) // 从后往前, 将 L-r[0] 与每个子序列中相应位置记录比较
                   {
                       L->r[j + increment] = L->r[j]; // 如果大于 L-r[0], 将该记录移到后一个子序列表相应位置中(第一次位置为 r[i] 的位置)
                   }
                   L->r[j + increment] = L->r[0];  // 循环结束后, 抵消最后一次循环结束后的 j -= increment, 即为合适的插入位置
               }
           }
       }
       while (increment > 1);
   }
   ```
3. 希尔排序复杂度分析
   希尔排序的关键是将相隔某个距离的记录组成一个个子序列, 实现跳跃式的排序, 使得排序的效率提高.
   希尔排序时间复杂度为 \\(O(n^2)\\) .
   由于记录是跳跃式的移动, 希尔排序并不是一种稳定的排序算法

### 堆排序

堆是具有下列性质的完全二叉树:
每个结点的值都大于或等于其左右孩子的值, 称为大顶堆;
或每个结点的值都小于或等于其左右孩子的值, 称为小顶堆

根结点一定是堆中所有结点最大(小)者
较大(小)的结点靠近根结点(不绝对)
TODO: 补充图片

按照层序遍历的方式给 n 个结点从 1 开始编号, 结点之间满足如下关系:
\\(\begin{cases} k_i\geqslant k_{2i} \\ k_i\geqslant k_{2i+1} \end{cases}\\) 或 \\(\begin{cases} k_i\leqslant k_{2i} \\ k_i\leqslant k_{2i+1} \end{cases} (1\leqslant i\leqslant \frac{n}{2})\\)
(可以回顾完全二叉树的存储)

将大顶堆或小顶堆用层序遍历存入数组:
TODO: 补充图片

1. 堆排序算法
   堆排序(Heap Sort)是利用堆(假设利用大顶堆)进行排序的方法.
   基本思想是将待排序的序列构造成一个大顶堆.
   此时序列的最大值就是根节点, 将他移至序列末尾(与末尾记录交换), 然后将剩余的序列重新构造成一个堆,
   这样就会得到次大值. 如此反复执行, 便能得到一个有序序列.

   实现需要解决两个问题:
   1. 如何由无序序列构建成一个堆
   2. 如何在输出堆顶元素后, 调整剩余元素成为一个新堆

   实现代码如下:
   ```C++
   // 堆调整函数(大顶堆), 使堆顶为当前最大的数, 调用时应确认树中子堆应该已经调整过
   void HeapAdjust(SqList *L, int s, int m)
   {
       int temp, j;
       temp = L->r[s]; // temp 存储原本堆顶的值
       for (j = 2 * s; j <= m; j *= 2) // 循环遍历左孩子
       {
           if (j < m && L->r[j] < L->r[j + 1]) // 如果 j < m 说明不是最后一个结点, 左孩子小于右孩子
               j++; // j 指针加 1 以使其指向右孩子
           if (temp >= L->r[j]) // 如果 temp 的值大于 j 指向的较大的孩子
               break; // 结束循环(考虑到下面的子堆已经调整过, j 下面已经不可能会有比堆顶还大的数)

           L->r[s] = L->r[j]; // 否则将此孩子的值赋给当前堆顶
           s = j; // 接下来调整子堆, 将指向堆顶的 s 指针指向当前孩子 j
       }
       L->r[s] = temp; // 将原本堆顶的值存入 s 指向的地方
   }

   void HeapSort(SqList *L)
   {
       int i;
       
       // 构建堆, 将数组调整为一个大顶堆
       for (i = L->length / 2; i > 0; i--) // 从子堆开始调整
           HeapAdjust(L, i, L->length);

       for (i = L->length; i > 1; i--)
       {
           swap(L, 1, i); // 将堆顶记录和 i 交换(i 从后往前)
           HeapAdjust(L, 1, i - 1); // 重新调整堆
       }
   }
   ```
2. 堆排序复杂度分析
   运行时间主要是消耗在初始构建堆和在重建堆时的反复筛选上
   构建堆: 因为是完全二叉树从最下层最右边的非终端结点开始构建, 将它与其孩子进行比较和若有必要的互换, 对于每个非终端结点, 最多进行两次比较和互换操作, 时间复杂度为 \\(O(n)\\)
   
   正式排序: 第 i 次取堆顶记录重建堆需要用 \\(O(\log i)\\) 的时间(完全二叉树的某个结点到根结点的距离为 \\(\log_2 i + 1\\) , 并且需要取 n-1 次堆顶记录, 因此重建堆的时间复杂度为 \\(O(n\log_n)\\)
​	
   总体堆排序的时间复杂度为 \\(O(nlogn)\\)

   由于记录的比较与交换是跳跃式进行, 堆排序也是一种不稳定的排序方法
   由于初始构建堆所需的比较次数较多, 不适合待排序序列个数较少的情况
   空间复杂度上只有一个用来交换的暂存单元

### 归并排序

1. 归并排序算法
2. 归并排序复杂度分析
3. 非递归实现归并排序

### 快速排序

1. 快速排序算法
2. 快速排序复杂度分析
3. 快速排序优化