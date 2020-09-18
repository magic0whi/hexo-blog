---
title: data_structure
lang: zh-cn
category: learn
date: 2020-09-09 20:54:30
tags:
toc: true
---

需要对C语言的指针和数组有一定的了解
全部代码示例皆为C语言
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
2. 顺序存储结构需要三个属性：
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
   假设已经将数据存入静态链表，比如分别存放着"甲"、"乙"、"丁"、"戊"、"己"、"庚"等数据
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
   为了解决终端节点访问效率低的问题, 再改造一下这个循环链表, 不用头指针而用指向终端结点的尾指针来表示链表.
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

双向链表(Double linked list)是在单链表的每个结点中, 再设置一个指向其前驱节点的指针域. 双向链表中的结点都有两个指针域, 一个指向后继, 一个指向后驱

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

**栈顶(top)**允许数据插入和删除, 另一端叫**栈底(bottom)**
栈又被称为**后进先出(Last In, First Out)**的线性表, 简称**LIFO结构**

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
    InitStack(*S) : 初始化操作, 建立一个空栈 S
    DestoryStack(*S) : 将栈销毁
    ClearStack(*S) : 将栈清空
    StackEmpty(S) 若栈为空, 返回 true ; 否则返回 false
    GetTop(S, *e) : 用 e 返回栈 S 的栈顶元素
    Push(*S, e) ： 插入新元素 e 到栈 S 中并成为栈顶元素. 又称: 压栈, 进栈, 入栈
    Pop(*S, *e) : 删除栈 S 中栈顶元素, 并用 e 返回其值. 又称: 弹栈, 出栈
    StackLength(S) : 返回栈 S 的元素个数
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

#### 四则运算表达式求值

### 队列的定义

### 队列的抽象数据类型

### 循环队列

### 队列的链式存储结构及实现

