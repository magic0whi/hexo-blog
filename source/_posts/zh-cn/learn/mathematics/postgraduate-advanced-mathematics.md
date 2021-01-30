---
title: postgraduate-advanced-mathematics
toc: true
category: mathematics
lang: zh-cn
date: 2021-01-19 09:08:57
tags:
---

同济高等数学笔记整合(上)
基于 [wmathor/Postgraduate-Advanced-Mathematics](https://github.com/wmathor/Postgraduate-Advanced-Mathematics)
不定积分是为定积分服务的, 它的目的是找原函数

<!-- more -->

## 函数与极限

### 函数

1. 函数:
   有集合 \\(D\\) 和 \\(x\\)、\\(y\\) 两个变量. 若对任意 \\(x\in D\\) ,
   总存在唯一确定的解 \\(y\\) 与 \\(x\\) 对应, 称 \\(y\\) 为 \\(x\\) 的函数,
   记作 \\(y=f(x)\\) , \\(D\\) 为 \\(x\\) 的定义域
2. 反函数:
   \\(y=f(x)\enspace\\) (\\(x\in D\\)) 严格单调 **(单调函数必有单调反函数)**
   若 \\(y=f(x) \implies x=\varphi(y)\\)
   则 \\(\varphi(y)\\) 就是 \\(f(x)\\) 的反函数
3. 基本初等函数:
   1. \\(x^a\\)
   2. \\(a^x\\) (\\(a>0\\) 且 \\(a\ne 1\\))
   3. \\(\log_a{x}\\) (\\(a>0\\) 且 \\(a\ne 1\\))
   4. \\(\sin x,\cos x,\tan x,\cot x,\sec x,\csc x\\)
4. 初等函数: 由常数、基本初等函数经过四则运算、复合运算而成的式子

#### 初等性质

1. 奇偶性
   设 \\(y=f(x)\enspace\\) (\\(x\in D\\) , \\(D\\) 关于原点对称)
   若对于 \\(\forall x\in D\\), 有
   \\(f(-x)=-f(x)\\) , 则为奇函数;
   \\(f(-x)=f(x)\\) , 则为偶函数
2. 单调性
   设 \\(y=f(x)\enspace\\) (\\(x\in D\\))
   若 \\(\exist x_1,x_2\in D\\) 且 \\(x_1<x_2\\) 时 , 有 \\(f(x_1)<f(x_2)\\) , 称 \\(f(x)\\) 在 \\(D\\) 上严格单调递增;
   若 \\(\exist x_1,x_2\in D\\) 且 \\(x_1<x_2\\) 时 , 有 \\(f(x_1)>f(x_2)\\) , 称 \\(f(x)\\) 在 \\(D\\) 上严格单独递减
   {% asset_img 1.png %}

3. 有界性
   设 \\(y=f(x)\enspace\\) (\\(x\in D\\)) , 函数的界 \\(M\\) (即值域范围)
   若 \\(\exist M>0\\) , 对于 \\(\forall x\in D\\) , 有 \\(|f(x)|\leqslant M\\) , 称 \\(f(x)\\) 在 \\(D\\) 上有界
   {% asset_img 2.png %}

   若 \\(\forall x\in D\\) , \\(f(x)\geqslant M_1\\) , 有下界
   若 \\(\forall x\in D\\) , \\(f(x)\leqslant M_2\\) , 有上界
   如: \\(|f(x)|\leqslant 3\hArr\begin{cases} f(x)\geqslant -3 \\\ f(x)\leqslant 3 \end{cases}\\)
4. 周期性
   设 \\(y=f(x)\enspace\\) (\\(x\in D\\)) ,
   若 \\(\exist T>0\\) , 对于 \\(\forall x\in D\enspace\\) (\\(x+T\in D\\)) , 有 \\(f(x+T)=f(x)\\) , 称 \\(f(x)\\) 为周期函数

### 数列极限

1. 数列收敛定义(\\(\epsilon−N\\)):
   设数列 \\(\\{a_n\\}\\) , \\(A\\) 为极限值, 最大误差 \\(\varepsilon\\) , 数列在元素 \\(a_N\\) 后极限有效(即之后数列元素与极限值的误差进入容许范围),
   若 \\(\forall\varepsilon > 0 , \exist N > 0\\) , 当 \\(n > N\\) 时 \\(|a_n - A| < \varepsilon\\)
   称收敛于极限 \\(A\\) , 记作 \\(\lim\limits_{n\to\infty}=A\\) 或 \\(a_n\to A\enspace\\) (\\(n\to\infty\\))
   例:
   设通项公式 \\(a_n=\frac{n+1}{2n}\\), 具体值为 \\(\frac{3}{4},\frac{2}{3},\frac{5}{8},\frac{3}{5},\dots\\) , 观察得极限 \\(a_n\to\frac{1}{2}\\)
   设最大误差 \\(\varepsilon=\frac{1}{10}>0\\), 则误差值 \\(|a_n-\frac{1}{2}|=\frac{1}{2n}<\frac{1}{10}\Harr n>5\\).
   同理:
   若 \\(\varepsilon=\frac{1}{100}>0\\), 则 \\(\frac{1}{2n}<\frac{1}{100}\Harr n>50\\)
   若 \\(\varepsilon=\frac{1}{1000}>0\\), 则 \\(\frac{1}{2n}<\frac{1}{1000}\Harr n>500\\)
   由此发现该数列有极限: \\(\lim\limits_{n\to\infty}a_n=\frac{1}{2}\\)
2. 性质
   1. 唯一性: 数列有极限必唯一
      证明(反证法):
      设数列有两个极限 \\(\lim\limits_{n\to\infty}a_n=A ,\enspace \lim\limits_{n\to\infty}a_n = B \\) , 且 \\(A\neq B\\)
      不妨设 \\(A>B\\) , \\(\varepsilon=-\dfrac{A+B}{2}\\) (误差值的定义由 \\(\varepsilon + A=-\varepsilon - B\\) 解出, 目的是让下面两个\\(a_n\\)的值域没有交集)
      \\(\because\lim\limits_{n\to\infty}a_n=A\\) , \\(\therefore\exist N_1>0\\) , 当 \\(n>N_1\\) 时, \\(|a_n-A|<\varepsilon\Harr\frac{3A+B}{2}<a_n<\frac{A-B}{2}\\) (1)
      \\(\because\lim\limits_{n\to\infty}a_n=B\\) , \\(\therefore\exist N_2>0\\) , 当 \\(n>N_2\\) 时, \\(|a_n-B|<\varepsilon\Harr\frac{A-B}{2}<a_n<\frac{-A+B}{2}\\) (2)
      取 \\(N=\operatorname{max}\\{N_1,N_2\\}\\) , 当 \\(n<N_2\\) 时, (1)、(2) 都成立, 但这两个不等式没有交集, 矛盾, 所以 \\(A>B\\) 不对, 同理 \\(B>A\\) 也不对.
      \\(\therefore A=B\\), 极限值只有一个.
   2. 有界性:
      若 \\(\lim\limits_{n\to\infty}a_n=A\\) , 则 \\(\exist M>0\\) , 使得 \\(|a_n|\leqslant M\\) , **反之不成立**.
      证明: 
      \\(\rArr\\) 设 \\(\varepsilon=1>0\\)
      \\(\because\lim\limits_{n\to\infty}a_n=A\\)
      \\(\therefore\exist N>0\\) , 当 \\(n>N\\) 时 , \\(|a_n-A|<1\\)
      \\(\because||a_n|-|A||\leqslant|a_n-A|\\) (三角不等式)
      \\(\therefore\\) 当 \\(n>N\\) 时, \\(||a_n|-|A_1||<1\rArr|a_n|<1+|A|\\) 在 \\(n>N\\) 时成立
      取 \\(M=\operatorname{max}\\{|a_1|,|a_2|,\dots,|a_n|,1+|A|\\}\\) , 对于 \\(\forall n\\) , 有 \\(|a_n|\leqslant M\\)
      \\(\nLeftarrow\\) 设 \\(a_n=1+(-1)^n\\) , 有 \\(|a_n|\leqslant 2\\) , 但 \\(\lim\limits_{n\to\infty}a_n\\) 不存在
   3. 保号性:
      若 \\(\lim\limits_{n\to\infty}a_n=A>0(<0)\\) , 则 \\(\exist N>0\\) , 当 \\(n>N\\) 时 , \\(a_n>0(<0)\\)
      证明:
      设 \\(A>0\\) , 取 \\(\varepsilon=\frac{A}{2}>0\\)
      \\(\because\lim\limits_{n\to\infty}a_n=A\\) , \\(\therefore\exist N>0\\) , 当 \\(n>N\\) 时 , \\(|a_n-A|<\frac{A}{2}\hArr\frac{A}{2}<a_n<\frac{3A}{2}\hArr a_n>\frac{A}{2}>0\\)
      设 \\(A<0\\) , 取 \\(\varepsilon=-\frac{A}{2}>0\\)
      \\(\because\lim\limits_{n\to\infty}a_n=A\\) , \\(\therefore\exist N>0\\) , 当 \\(n>N\\) 时 , \\(|a_n-A|<-\frac{A}{2}\hArr\frac{3A}{2}<a_n<\frac{A}{2}\hArr a_n<\frac{A}{2}<0\\)

### 函数极限

1. 定义
   \\(\varepsilon-\delta\\) 语言定义法: (\\(\varepsilon\\) 为值域的误差, \\(\delta\\) 为定义域的误差)
   若 \\(\forall\varepsilon>0\\) , \\(\exist\delta>0\\) , 当 \\(0<|x-a|<\delta\\) 时 \\(|f(x)-A|<\varepsilon\\) ,
   称 \\(f(x)\\) 当 \\(x\to 0\\) 时以 \\(A\\) 为极限
   记作 \\(\lim\limits_{x\to a}f(x)=A\\) 或 \\(f(x)\to A\enspace\\) (\\(x\to a\\))

   \\(\varepsilon-X\\) 语言定义法: (\\(X\\) 为值域临界点)
   若 \\(\forall\varepsilon>0\\) , \\(\exist X>0\\) , \\(|f(x)-A|<\varepsilon\\)
   1. 当 \\(x>X\\) 时, \\(\lim\limits_{x\to+\infty}f(x)=A\\)
   2. 当 \\(x<-X\\) 时, \\(\lim\limits_{x\to-\infty}f(x)=A\\)
   3. 当 \\(|x|>X\\) 时, \\(\lim\limits_{x\to\infty}f(x)=A\\)
2. 性质
   1. 唯一性(函数有极限必唯一)
      证明: 设 \\(\lim\limits_{x\to a}f(x)=A\\) , \\(\lim\limits_{x\to a}f(x)=B\\)
      不妨设 \\(A>B\\) , 取 \\(\varepsilon=\frac{A-B}{2}>0\\) (凑出矛盾的结果)
      \\(\because\lim\limits_{x\to a}f(x)=A\\)
      \\(\therefore\exist\delta_1>0\\) , 当 \\(0<|x-a|<\delta_1\\) 时, \\(|f(x)-A|<\frac{A-B}{2}\hArr\frac{A+B}{2}<f(x)<\frac{3A-B}{2}\\) (1)
      又 \\(\because\lim\limits_{x\to a}f(x)=B\\)
      \\(\therefore\exist\delta_2>0\\) , 当 \\(0<|x-a|<\delta_2\\) 时, \\(|f(x)-B|<\frac{A-B}{2} \hArr \frac{3B-A}{2}<f(x)<\frac{A+B}{2}\\) (2)
      取 \\(\delta=\operatorname{min}\\{\delta_1,\delta_2\\}\\) , 当 \\(0<|x-a|<\delta\\) 时 (1)(2) 皆成立, 矛盾.
      \\(\therefore A>B\\) 不对, 同理 \\(A<B\\) 也不对.
      \\(\therefore A=B\\), 极限值只有一个.
   2. 局部有界性: 设 \\(\lim\limits_{x\to a}f(x)=A\\) , \\(\exist\delta>0\\) , \\(M>0\\) , 当 \\(0<|x-a|<\delta\\) 时, \\(|f(x)|\leqslant M\\)
      证明: 证明目标是找出 \\(|f(x)|\leqslant\\) 某个确切的值
      取 \\(\varepsilon=1>0\\)
      \\(\because\lim\limits_{x\to a}f(x)=A\\)
      \\(\therefore\exist\delta>0\\) , 当 \\(0<|x-a|<\delta\\) 时, \\(|f(x)-A|<1\\)
      又 \\(\because||f(x)|-|A||\leqslant|f(x)-A|\\) (三角不等式)
      \\(\therefore\\) 当 \\(0<|x-a|<\delta\\) 时, \\(||f(x)|-|A||<1\rArr|f(x)|<1+|A|(=M)\\)
      \\(\therefore\\) 当 \\(0<|x-a|<\delta\\) 时, \\(|f(x)|\leqslant M\\)
   3. 保号性: 设 \\(\lim\limits_{x\to a}f(x)=A>0(<0)\\) , 则 \\(\exist\delta>0\\) , 当 \\(0<|x-a|<\delta\\) 时, \\(f(x)>0(<0)\\)
      证明:
      1. 若 \\(A>0\\) , 取 \\(\varepsilon=\frac{A}{2}>0\\)
         \\(\because\lim\limits_{x\to a}f(x)=A\\) ,
         \\(\therefore\exist\delta>0\\) , 当 \\(0<|x-a|<\delta\\) 时 \\(|f(x)-A|<\frac{A}{2}\rArr f(x)>\frac{A}{2}>0\\)
      2. 若 \\(A<0\\) , 取 \\(\varepsilon=-\frac{A}{2}>0\\)
         \\(\because\lim\limits_{x\to a}f(x)=A\\) ,
         \\(\therefore\exist\delta>0\\) , 当 \\(0<|x-a|<\delta\\) 时 \\(|f(x)-A|<-\frac{A}{2}\rArr f(x)>\frac{A}{2}>0\\)

Notes:
1. \\(\\{x|0<|x-a|<\delta\\}\\) 可表示为 \\(\mathring{\bigcup}(a\cdot\delta)\\) (\\(a\\) 的去心 \\(\delta\\) 邻域)
   {% asset_img 3.png %}

2. \\(x\to a\\) 时 \\(\begin{cases} x\to a^- \\\ x\to a^+ \end{cases}\\)
3. 左极限与右极限
   若 \\(\forall\varepsilon>0\\) , \\(\exist\delta>0\\)
   1. 当 \\(x\in(a-\delta,a)\\) 时 \\(|f(x)-A| < \varepsilon\\) ,
      此时 \\(A\\) 称 \\(f(x)\\) 在 \\(x=a\\) 处左极限,
      记为 \\(\lim\limits_{x\to a^-}f(x)=A\\) 或 \\(f(a-0)=A\\)
   2. 当 \\(x\in(a,a+\delta)\\) 时 \\(|f(x-B)|<\varepsilon\\) ,
      此时 \\(B\\) 称 \\(f(x)\\) 在 \\(x=a\\) 处右极限,
      记为 \\(\lim\limits_{x\to a^+}f(x)=B\\) 或 \\(f(a+0)=B\\)
4. \\(\lim\limits_{x\to a}f(x)\\) 意味着左右极限存在且相等
5. \\(\lim\limits_{x\to a}f(x)\\) 与 \\(f(a)\\) 无关(除非该点连续)
   如:
   1. 分段函数 \\(f(x)=\begin{cases} x-1 & x<0 \\\ 0 & x=0 \\\ 2x-1 & x>0 \end{cases}\\), 它的极限为 \\(\begin{cases} \lim\limits_{x\to 0^+}f(x)=-1 \\\ \lim\limits_{x\to 0^-}f(x)=-1 \end{cases}\rArr\lim\limits_{x\to 0}f(x)=-1\\) , 但 \\(f(0)=0\\)
   2. \\(\lim\limits_{x\to 2}\frac{x^2-4}{x-2}=\lim\limits_{x\to 2}(x+2)=4\\), 但 \\(f(2)\\) 时分母为零

### 无穷小与无穷大

1. 无穷小
   1. \\(\varepsilon-\delta\\) 定义: 若 \\(\forall\varepsilon>0\\) , \\(\exist\delta>0\\) , 当 \\(0<|x-x_0|<\delta\\) 时 , \\(|\alpha(x)-0|   <\varepsilon\\) , 称 \\(\alpha(x)\\) 在 \\(x\to x_0\\) 时无穷小, 记 \\(\lim\limits_{x\to x_0}\alpha(x)=0\\)
   2. 常规性质:
      1. 若 \\(\alpha\to 0\\) , \\(\beta\to 0\\) , 则 \\(\begin{cases} \alpha\pm\beta\to 0 \\\ k\alpha\to 0 \\\ \alpha\beta\to0 \end{cases}\\)
      2. 无穷小乘有界函数还是无穷小: \\(\alpha\to 0\\) , \\(|\beta|\leqslant M\\) , 则 \\(\alpha\beta\to 0\\)
      3. (重要)任意极限加无穷小还是原极限 \\(\lim\limits_{x\to x_0}f(x)=A \hArr \lim\limits_{x\to x_0}f(x)=A+\alpha\enspace\\) (\\(\alpha\to 0\\))
   
   Notes: 0 是无穷小, 但无穷小不一定为 0
2. 无穷大
   \\(\varepsilon-\delta\\) 定义: 若 \\(\forall M>0\\) , \\(\exist\delta>0\\) , 当 \\(0<|x-x_0|<\delta\\) 时, \\(|f(x)|\geqslant M\rArr\\) 称 \\(f(x)\\) 在 \\(x\to x_0\\) 时无穷大, 记 \\(\lim\limits_{x\to x_0}f(x)=\infty\\)
   \\(\varepsilon-X\\) 定义: 若 \\(\forall M>0\\) , \\(\exist X>0\\) , 当 \\(x>X\\) 时, \\(|f(x)|\geqslant M\rArr\\) 称 \\(f(x)\\) 在 \\(x\to +\infty\\) 时无穷大, 记 \\(\lim\limits_{x\to+\infty}f(x)=\infty\\)
3. 无穷小与无穷大的关系: \\(\lim\limits_{x\to x_0}f(x)=0\hArr\lim\limits_{x\to x_0}\frac{1}{f(x)}=\infty\\)

### 极限的运算法则

1. 四则求导法则
   有 \\(\lim\limits_{x\to x_0}f(x)=A\\) , \\(\lim\limits_{x\to x_0}g(x)=B\\)
   1. 加减: \\(\lim\limits_{x\to x_0}[f(x)\pm g(x)]=\lim\limits_{x\to x_0}f(x)\pm\lim\limits_{x\to x_0}g(x)=A\pm B\\)
   2. 数乘: \\(\lim\limits_{x\to x_0}kf(x)=k\lim\limits_{x\to x_0}f(x)=kA\\)
   3. 乘法: \\(\lim\limits_{x\to x_0}[f(x)g(x)]=\lim\limits_{x\to x_0}f(x)\cdot\lim\limits_{x\to x_0}g(x)=AB\\)
   4. 除法: \\(\lim\limits_{x\to x_0}\frac{f(x)}{g(x)}=\frac{\lim\limits_{x\to x_0}f(x)}{\lim\limits_{x\to x_0}g(x)}=\frac{A}{B}\\)
      极限除法的值以最高阶系数为准:
      设 \\(P(x)=a_nx^n+\dots+a_1x+a_0\\) , \\(Q(x)=b_mx^m+\dots+b_1x+b_0\\)
      则 \\(\lim\limits_{x\to\infty}\frac{P(x)}{Q(x)}=\begin{cases} \frac{a_n}{b_m} & n=m \\\ \infty & n>m \\\ 0 & n<m \end{cases}\\)
2. 复合函数极限法则: 
   设 \\(u=\varphi(x)\neq a\\) , \\(\lim\limits_{u\to a}f(u)=A\\) , \\(\lim\limits_{x\to x_0}\varphi(x)=a\\) , 则 \\(\lim\limits_{x\to x_0}f[g(x)]=A\\)

### 极限存在法则和两个重要极限

1. 极限存在准则
   1. 夹逼定理
      1. 数列型:
         设 \\(\begin{cases} a_n\leqslant b_n\leqslant c_n \\\ \lim\limits_{n\to\infty}a_n=\lim\limits_{n\to\infty}c_n=A \end{cases}\\)
         则 \\(\lim\limits_{n\to\infty}b_n=A\\)
         (证明方法: 利用 \\(\varepsilon−N\\) 定义推出 \\(A-\varepsilon<b_n<A+\varepsilon\rArr|b_n-A|<\varepsilon\\))
      2. 函数型:
         设 \\(\begin{cases} f(x)\leqslant g(x)\leqslant h(x) \\\ \lim f(x)=\lim h(x)=A \end{cases}\\)
         则 \\(\lim g(x)=A\\)
   2. 单调有界数列必有极限
      * \\(\\{a_n\\}\\) 有界 \\(\hArr\\{a_n\\}\\) 有上下界
      * 若 \\(\\{a_n\\}\\) 单调递增: \\(\begin{cases} \text{有上界}\rArr\lim\limits_{n\to\infty}a_n \text{存在} \\\ \text{无上界}\rArr\lim\limits_{n\to\infty}a_n\text{不存在} \end{cases}\\)
      * 若 \\(\\{a_n\\}\\) 单调递减: \\(\begin{cases} \text{有下界}\rArr\lim\limits_{n\to\infty}a_n \text{存在} \\\ \text{无下界}\rArr\lim\limits_{n\to\infty}a_n\text{不存在} \end{cases}\\)
2. 两个重要极限
   1. \\(\lim\limits_{x\to 0}\frac{\sin x}{x}=1\\)
      证明:
      {% asset_img 4.png %}
      
      设 \\(0<x<\frac{\pi}{2}\\)
      \\(S_{\triangle AOB}=\frac{1}{2}\sin x\\)
      \\(S_{扇形AOB}=\frac{1}{2}x\\)
      \\(S_{RT\triangle AOC}=\frac{1}{2}\tan x\\)
      \\(\therefore\\) 结合图片和三者面积公式可得 \\(\sin x<x<\tan x \rArr 1<\frac{x}{\sin x}<\frac{1}{\cos x}\\)
      \\(\because\lim\limits_{x\to0}\cos x=1\\)
      \\(\therefore\lim\limits_{x\to0}\frac{1}{\cos x}=1\\) , \\(\lim\limits_{x\to0}1=1\rArr\lim\limits_{x\to 0}\frac{x}{\sin x}=1\\) (夹逼定理) \\(\rArr\lim\limits_{x\to 0}\frac{\sin x}{x}=1\\)
   2. \\(\lim\limits_{n\to\infty}(1+\frac{1}{n})^n=e\\)

### 无穷小的比较(相除)

1. 无穷小的比较
   设 \\(\alpha\to 0\\) , \\(\beta\to 0\\)
   若 \\(\lim\frac{\beta}{\alpha}=0\\) , 称 \\(\beta\\) 为 \\(\alpha\\) 的高阶无穷小, 记 \\(\beta=\circ(\alpha)\\)
   若 \\(\lim\frac{\beta}{\alpha}=\infty\\) , 称 \\(\beta\\) 为 \\(\alpha\\) 的低阶无穷小
   若 \\(\lim\frac{\beta}{\alpha^k}=k\enspace\\) (\\(k\neq 0,\infty\\)) , 称 \\(\beta\\) 为 \\(\alpha\\) 的 \\(k\\) 阶无穷小
   若 \\(\lim\frac{\beta}{\alpha}=k\enspace\\) (\\(k\neq 0,\infty)\\) , 称 \\(\beta\\) 为 \\(\alpha\\) 的同阶无穷小, 记 \\(\beta=\bigcirc(\alpha)\\)
   若 \\(\lim\frac{\beta}{\alpha}=1\\) , 称 \\(\beta\\) 为 \\(\alpha\\) 的等价无穷小, 记 \\(\beta\sim\alpha\\)
   等价无穷小是同阶无穷小的充分不必要条件
2. 等价无穷小的性质
   设 \\(\alpha\to 0\\) , \\(\beta\to 0\\)
   1. \\(\alpha\sim\beta\hArr\beta=\alpha+\circ(\alpha)\\)
   2. 若 \\(\alpha\sim\alpha_1\\) , \\(\beta\sim\beta_1\\) , \\(\lim\frac{\beta_1}{\alpha_1}=A\\) , 则 \\(\lim\frac{\beta}{\alpha}=A\\)
3. 常用等价无穷小 (\\(x\to 0\\))
   1. \\(x\sim\sin x\\) , \\(x\sim\tan x\\) , \\(x\sim\arcsin x\\) , \\(x\sim\arctan x\\) , \\(x\sim\ln(1+x)\\) , \\(x\sim e^x-1\\)
   2. \\(1-\cos x\sim\frac{x^2}{2}\\)
   3. \\((1+x)^a-1\sim ax\\)

### 函数的连续性与间断点

连续是极限存在的充分不必要条件, 因为间断不代表极限不存在

1. 连续
   1. 函数在一点连续: \\(\lim\limits_{x\to a}f(x)=f(a)\\) 或 \\(f(a-0)=f(a+0)=f(a)\\)
      若 \\(f(a-0)=f(a)\\) , 称 \\(f(a)\\) 在 \\(x=a\\) 左连续
      若 \\(f(a+0)=f(a)\\) , 称 \\(f(a)\\) 在 \\(x=a\\) 右连续
   2. 函数在闭区间连续:
      设 \\(f(x)\\) 在 \\([a,b]\\) 上有定义, 若:
      1. \\(f(x)\\) 在 \\((a,b)\\) 内处处连续
      2. \\(f(a)=f(a+0)\\) , \\(f(b)=f(b-0)\\)
      
      则称 \\(f(x)\\) 在 \\([a,b]\\) 上连续, 记 \\(f(x)\in c[a,b]\\)
2. 间断点及分类
   1. 间断: 若 \\(\lim\limits_{x\to a}f(x)\neq f(a)\\) , 称 \\(f(x)\\) 在 \\(x=a\\) 间断
   2. 分类:
      1. 第一类间断点: \\(f(a-0)\\) , \\(f(a+0)\\) 皆存在
         * 可去间断点: \\(f(a-0)=f(a+0)\neq f(a)\\) {% asset_img 5.png %}
         * 跳跃间断点: \\(f(a-0)\neq f(a+0)\\) {% asset_img 6.png %}
      2. 第二类间断点: \\(f(a-0)\\) , \\(f(a+0)\\) 至少一个不存在
      3. \\(\lim\limits_{x\to 0^-}e^\frac{1}{x}=0\\) , \\(\lim\limits_{x\to 0^+}e^\frac{1}{x}=+\infty\\)

### 连续函数运算及初等函数连续性

1. 连续函数运算
   1. 四则运算
      设 \\(f(x)\\) , \\(g(x)\\) 在 \\(x=x_0\\) 处连续, 则
      1. \\(f(x)\pm g(x)\\) 在 \\(x=x_0\\) 处连续
      2. \\(f(x)g(x)\\) 在 \\(x=x_0\\) 处连续
      3. \\(\frac{f(x)}{g(x)}\\) 在 \\(x=x_0\\) 处连续 (\\(g(x)\neq 0\\))
      (证明思路是证明该点处极限与函数实际取该点值相等, 也就是上节讲到的函数连续定义)
   2. 复合运算
      设 \\(f(u)\\) , \\(u=\varphi(x)\neq a\\)
      若 \\(\lim\limits_{u\to a}f(u)=f(a)\\) , \\(\lim\limits_{x\to x_0}\varphi(x)=a\\) , 则 \\(\lim\limits_{x\to x_0}f[\varphi(x)]=f[\lim\limits_{x\to x_0}\varphi(x)]=f(a)\\)
2. 初等函数连续性: 基本初等函数、初等函数在其定义域内连续

### 闭区间上连续函数的性质

1. 最值定理
   设 \\(f(x)\in c[a, b]\\)
   则 \\(f(x)\\) 在 \\([a,b]\\) 上取到最小值 \\(m\\) 和最大值 \\(M\\)
   即 \\(\exist x_1,x_2\in[a,b]\\) , 使 \\(f(x_1)=m\\) , \\(f(x_2)=M\\)
2. 有界定理
   设 \\(f(x)\in c[a,b]\\)
   则 \\(\exist k>0\\) , 使 \\(\forall x\in[a,b]\\) 有 \\(|f(x)|\leqslant k\\)
3. 零点定理(高中数学中零点存在定理)
   设 \\(f(x)\in c[a,b]\\) , 若 \\(f(a)f(b)<0\\)
   则 \\(\exist c\in[a,b]\\) 使 \\(f\(c\)=0\\)
4. 介值定理
   设 \\(f(x)\in c[a, b]\\)
   则 \\(\forall\eta\in[m,M]\\) , \\(\exist\xi\in[a,b]\\) 使 \\(f(\xi)=\eta\\)
   (即介于 \\(m\\) 和 \\(M\\) 之间的 \\(f(x)\\) 皆可取到)

## 导数与微分

### 导数的概念

1. 导数定义
   设 \\(y=f(x)\enspace\\) (\\(x\in D\\)) , \\(\Delta y=f(x_0+\Delta x)-f(x_0)\enspace\\) (\\(x_0,(x_0+\Delta x)\in D\\))
   若 \\(\lim\limits_{\Delta x\to 0}\frac{\Delta y}{\Delta x}\\) 存在, 称 \\(f(x)\\) 在 \\(x=x_0\\) 处可导, 该极限称为 \\(f(x)\\) 在 \\(x=x_0\\) 处的导数, 记 \\(f^\prime(x_0)\\) 或 \\(\frac{\mathrm{d}y}{\mathrm{d}x}|_{x=x_0}\\)
2. 注意:
   1. 可导一定连续, 连续不一定可导.
      (因为连续函数在点 \\(f^\prime(x)=0\\) 处不可导, 如正弦曲线的 \\(y=1\\) 处左右导数不同)
   2. \\(f^\prime(x_0)=\lim\limits_{\Delta x\to 0}\frac{f(x_0+\Delta x)-f(x_0)}{\Delta x}=\lim\limits_{x\to x_0}\frac{f(x)-f(x_0)}{x-x_0}\\)
      (证明可通过代入\\(\Delta x=x-x_0\\))
   3. 可导时左右导数存在且相等
      左导数: \\(\lim\limits\_{\Delta x\to 0^-}\frac{\Delta y}{\Delta x}(=\lim\limits_{x\to x_0^-}\frac{f(x)-f(x_0)}{x-x_0})=f\_-^\prime(x_0)\\)
      右导数: \\(\lim\limits\_{\Delta x\to 0^+}\frac{\Delta y}{\Delta x}(=\lim\limits_{x\to x_0^+}\frac{f(x)-f(x_0)}{x-x_0})=f\_+^\prime(x_0)\\)

#### 常见初等函数的导函数

1. \\(C^\prime=0\\)
2. \\((x^n)^\prime=nx^{n-1}\\)
3. \\((a^x)^\prime=a^x\ln a\\)
4. \\((\log_a x)^\prime=\frac{1}{x\ln a}\\) , \\((\ln x)^\prime=\frac{1}{x}\\)
5. \\((\sin x)^\prime=\cos x\\)
   \\((\cos x)^\prime=-\sin x\\)
   \\((\tan x)^\prime=\sec^2x\\)
   \\((\cot x)^\prime=-\csc^2x\\)
   \\((\sec x)^\prime=\sec x\tan x\\)
   \\((\csc x)^\prime=-\csc x\cot x\\)
6. \\((\arcsin x)^\prime=\frac{1}{\sqrt{1-x^2}}\\)
   \\((\arccos x)^\prime=-\frac{1}{\sqrt{1-x^2}}\\)
   \\((\arctan x)^\prime=\frac{1}{1+x^2}\\)
   \\((\operatorname{arccot}x)^\prime=-\frac{1}{1+x^2}\\) <!-- katex好像没有\arccot -->
7. \\((\sin x)^{(n)}=\sin(x+\frac{nx}{2})\\)
   \\((\cos x)^{(n)}=\cos(x+\frac{nx}{2})\\)
   \\((\frac{1}{ax+b})^{(n)}=(-1)^n n!\frac{a^n}{(ax+b)^{n+1}}\\)

推导:
直接代入 \\(f^\prime(x)=\lim\limits_{\Delta x\to0}\frac{f(x+\Delta x)-f(x)}{\Delta x}\\)
1. \\((x^n)^\prime=nx^{n-1}\\) 会用到二项式展开式
2. \\((a^x)^\prime=a^x\ln a\\) 会用到 \\(x=e^{\ln x}\\) 和 \\(x\sim(e^x-1)\\)
   \*特别地 \\((e^x)^\prime=e^x\\)
3. \\((\log_a x)^\prime=\frac{1}{x\ln a}\\)
   <div>
   $$
   f^\prime(x)=\dfrac{\mathrm{d}y}{\mathrm{d}x}
   =\overbrace{\dfrac{\log_a(x+\mathrm{d}x)-\log_a x}{\mathrm{d}x}=\dfrac{\log_a(\frac{x+\mathrm{d}x}{x})}{\mathrm{d}x}}^{\log_a \frac{M}{N}
   =\log_a M-\log_a N}=\dfrac{\log_a(1+\frac{\mathrm{d}x}{x})}{\mathrm{d}x}
   =\overbrace{\dfrac{\frac{\ln(1+\frac{\mathrm{d}x}{x})}{\ln a}}{\mathrm{d}x}}^{\log_a N=\frac{\log_b N}{\log_b a}}
   =\overbrace{\dfrac{\frac{\frac{\mathrm{d}x}{x}}{\ln a}}{\mathrm{d}x}}^{x\sim\ln(1+x)}=\dfrac{1}{x\ln a}
   $$
   </div>
4. <div>
   $$
   \lim\limits_{\Delta x\to 0}\frac{\sin(x+\Delta x)-\sin x}{\Delta x}
   =\underbrace{\lim\limits_{\Delta x\to 0}\frac{\sin x\cos\Delta x+\cos x\sin\Delta x-\sin x}{\Delta x}}_{\text{和差角公式}}
   =\lim\limits_{\Delta x\to 0}\frac{\sin x(\cos\Delta x-1)}{\Delta x}+\cos x\lim\limits_{\Delta x\to 0}\underbrace{\frac{\sin\Delta x}{\Delta x}}_{x\sim\sin x}=\cos x 
   $$
   </div>
   
   \\((\cos x)^\prime\\) 的推导也同理
   后面的都是将其化为\\(\sin x\\) 和 \\(\cos x\\)的形式然后利用求导的四则运算法则推导 (如 \\(\sec x=\frac{1}{\cos x}\\)、\\(\csc x=\frac{1}{\sin x}\\))
5. 使用[反函数求导法则](#反函数求导法则):
   这里只证明 \\(\arcsin x\\)
   \\(f(x):y=\arcsin x\\) , \\(\varphi(y):x=\sin y\\)
   \\(f^\prime(x)=\frac{1}{\varphi^\prime(y)}\rArr(\arcsin x)^\prime=\dfrac{1}{\cos y}=\underbrace{\frac{1}{\sqrt{1-\sin^2 y}}}_{平方关系式}=\underbrace{\frac{1}{\sqrt{1-x^2}}}\_{带入 x=\sin y}\\)
6. 使用高阶导数归纳法 <span id="induction_method"></span>
   1. \\(f(x)=\sin x\\)
      \\(f^\prime(x)=\cos x=\sin(x+\frac{\pi}{2})\\)
      \\(f^{\prime\prime}(x)=-\sin x=\sin(x+\frac{2\pi}{2})\\)
      \\(f^{\prime\prime\prime}(x)=-\cos x=\sin\(x+\frac{3\pi}{2})\\)
      \\(f^{(4)}(x)=\sin x=\sin\(x+\frac{4\pi}{2})\\)
      \\(\therefore f^{(n)}(x)=\sin(x+\frac{nx}{2})\\)
      同理 \\((\cos x)^{(n)}=\cos(x+\frac{nx}{2})\\)
   2. \\(f(x)=(2x+1)^{-1}\\)
      \\(f^\prime(x)=(-1)(2x+1)^{-2}\times 2\\)
      \\(f^{\prime\prime}(x)=(-1)(-2)(2x+1)^{-3}\times 2^2\\)
      \\(\therefore f^{(n)}(x)=(-1)^n\times n! \times 2^n \times (2x+1)^{-(n+1)}\\)

### 求导法则

1. 四则法则
   设 \\(u(x) , v(x)\\) 可导, 则
   1. 加减: \\([u(x)\pm v(x)]^\prime=u^\prime(x)\pm v^\prime(x)\\)
   2. 数乘: \\((ku)^\prime=ku^\prime\\)
   3. 乘: \\([u(x)v(x)]^\prime=u^\prime(x)v(x)+u(x)v^\prime(x)\\)
   4. 除: \\([\frac{u(x)}{v(x)}]^\prime=\frac{u^\prime(x)v(x)-u(x)v^\prime(x)}{v^2(x)}\enspace\\) (\\(v(x)\neq 0\\))
      推论: \\((uvw)^\prime=u^\prime vw+uv^\prime w+uvw^\prime\\)
2. 四则法则证明:
   1. 令 \\(\varphi(x)=u(x)+v(x)\\)
      则 \\(\Delta\varphi=\varphi(x+\Delta x)-\varphi(x)=u(x+\Delta x)+v(x+\Delta x)-u(x)-v(x)=\Delta u+\Delta v\\)
      进而 \\(\lim\limits_{\Delta x\to0}\frac{\Delta\varphi}{\Delta x}=\lim\limits_{\Delta x\to0}\frac{\Delta u}{\Delta x}+\lim\limits_{\Delta x\to 0}\frac{\Delta v}{\Delta x}=u^\prime(x)+v^\prime(x)\\)
   2. 令 \\(\varphi(x)=u(x)v(x)\\)
      <div>
      $$
      \begin{aligned}
       \Delta\varphi & =\varphi(x+\Delta x)-\varphi(x) \\
       & =u(x+\Delta x)v(x+\Delta x)-u(x)v(x) \\
       & =u(x+\Delta x)v(x+\Delta x)-u(x)v(x+\Delta x)+u(x)v(x+\Delta x)-u(x)v(x) \\
       & =\Delta uv(x+\Delta x)+u(x)\Delta v
      \end{aligned}
      $$
      </div>
      
      进而 \\(\lim\limits_{\Delta x\to 0}\frac{\Delta\varphi}{\Delta x}=\lim\limits_{\Delta x\to 0}\frac{\Delta u}{\Delta x}\cdot\lim\limits_{\Delta x\to 0}v(x+\Delta x)+u(x)\lim\limits_{\Delta x\to0}\frac{\Delta v}{\Delta x}=u^\prime(x)v(x)+u(x)v^\prime(x)\\)
      提示: \\(\lim\limits_{\Delta x\to0}v(x+\Delta x)=v(x)\\)
   3. 令 \\(\varphi(x)=\dfrac{u(x)}{v(x)}\enspace\\) (\\(v(x)\neq 0\\))
      <div>
      $$
      \begin{aligned}
       \Delta\varphi=\varphi(x+\Delta x)-\varphi(x) & =\frac{u(x+\Delta x)}{v(x+\Delta x)}-\frac{u(x)}{v(x)} \\
       & =\frac{u(x+\Delta x)v(x)-v(x+\Delta x)u(x)}{v(x+\Delta x)v(x)} \\
       & =\frac{[u(x+\Delta x)v(x)-u(x)v(x)]-[u(x)v(x+\Delta x)-u(x)v(x)]}{v(x+\Delta x)v(x)} \\
       & =\frac{\Delta uv(x)-u(x)\Delta v}{v(x+\Delta x)v(x)}
      \end{aligned}
      $$
      </div>
      
      进而 \\(\lim\limits_{\Delta x\to 0}\frac{\Delta\varphi}{\Delta x}=\frac{\lim\limits_{\Delta x\to 0}\frac{\Delta u}{\Delta x}\cdot v(x)-u(x)\cdot\lim\limits_{\Delta x\to0}\frac{\Delta v}{\Delta x}}{v(x)\lim\limits_{\Delta x\to0}v(x+\Delta x)}=\frac{u^\prime(x)v(x)-u(x)v^\prime(x)}{v^2(x)}\\)
      提示: \\(\lim\limits_{\Delta x\to 0}v(x+\Delta x)=v(x)\\)
3. 反函数求导法则
   设 \\(y=f(x)\\) 可导且 \\(f(x)^\prime\neq 0\\) (导数不为零意味着严格单调),
   则反函数 \\(x=\varphi(y)\\) 可导且 \\(\varphi^\prime(y)=\frac{1}{f^\prime(x)}\\)
   证明:
   \\(\varphi^\prime(y)=\lim\limits_{\Delta y\to 0}\frac{\Delta x}{\Delta y}=\lim\limits_{\Delta y\to 0}\frac{1}{\frac{\Delta y}{\Delta x}}=\underbrace{\lim\limits_{\Delta x\to 0}}\_{(1)}\frac{1}{\frac{\Delta y}{\Delta x}}=\frac{1}{f^\prime(x)}\\)
   (1) \\(\lim\limits_{\Delta x\to0}\frac{\Delta y}{\Delta x}\neq 0 \rArr \Delta y=\bigcirc(\Delta x)\\) 同阶无穷小
4. 复合函数求导法则
   (如何判断复合函数: 观察有几个初等函数)
   1. 复合函数求导法则:
      设 \\(y=f(u)\\) 可导, \\(u=\varphi(x)\\) 可导且 \\(\varphi^\prime(x)\neq 0\\) , 则 \\(y=f(\varphi(x))\\) 可导
      有 \\(f^\prime(\varphi(x))=f^\prime[\varphi(x)]\varphi^\prime(x)\\)
   3. 证明:
      1. 用极限:
         <div>
         $$
         f^\prime(\varphi(x))=\lim\limits_{\Delta x\to 0}\frac{\Delta y}{\Delta x}
         =\lim\limits_{\Delta x\to 0}(\frac{\Delta y}{\Delta u}\cdot\frac{\Delta u}{\Delta x})
         =\lim\limits_{\Delta x\to 0}\frac{\Delta y}{\Delta u}\cdot\lim\limits_{\Delta x\to 0}\frac{\Delta u}{\Delta x}
         =\underbrace{\lim\limits_{\Delta u\to 0}}_{(1)}\frac{\Delta y}{\Delta u}\cdot\lim\limits_{\Delta x\to 0}\frac{\Delta u}{\Delta x}
         =f^\prime(u)\cdot\varphi^\prime(x)
         $$
         </div>
         
         (1) \\(\varphi^\prime(x)=\lim\limits_{\Delta x\to 0}\frac{\Delta u}{\Delta x}\neq 0\rArr\Delta u=\bigcirc(\Delta x)\\) 同阶无穷小
      2. 用微分: \\(f^\prime(\varphi(x))=\frac{\\mathrm{d}y}{\mathrm{d}x}=\frac{\mathrm{d}y}{\mathrm{d}u}\cdot\frac{\mathrm{d}u}{\mathrm{d}x}=f^\prime(u)\cdot\varphi^\prime(x)=f^\prime[\varphi(x)]\varphi^\prime(x)\\)

### 高阶导数

二阶及以上的导数称为高阶导数.

1. 二阶导数: 对导数再求导一次就是二阶导数
   \\(f^{\prime\prime}(x)=[f^\prime(x)]^\prime=\frac{\mathrm{d}(\frac{\mathrm{d}y}{\mathrm{d}x})}{\mathrm{d}x}=\frac{\mathrm{d}^2y}{\mathrm{d}x^2}\\)
   同理三阶导数 \\(\frac{\mathrm{d}^3y}{\mathrm{d}x^3}\\) , \\(n\\) 阶导数 \\(f^{(n)}=\frac{\mathrm{d}^ny}{\mathrm{d}x^n}\\)
2. 高阶导数求导方法:
   1. [归纳法](#induction_method)
   2. 公式法:
      \\((uv)^\prime=u^\prime v+uv^\prime\rArr(uv)^{\prime\prime}=(u^\prime v)^\prime+(uv^\prime)^\prime=u^{\prime\prime}v+2u^\prime v^\prime+uv^{\prime\prime}\\)
      莱布尼茨公式(请记住)
      \\((uv)^{(n)}=C_n^0 u^{(n)}v+C_n^1 u^{(n-1)}v^\prime+C_n^2 u^{(n-2)}v^{\prime\prime}+\dots+C_n^n uv^{(n)}\\)

### 隐函数及由参数方程确定的函数求导

1. 隐函数求导
   * 显函数: \\(y=f(x)\\)
   * 隐函数: \\(F(x,y)=0\enspace\underrightarrow{\text{显式化}}\enspace y=f(x)\\)
   * 方法:
     有 \\(F(x,y)=0\\) , 确定 \\(y\\) 为 \\(x\\) 的函数.
     两边对 \\(x\\) 求导. 求导时 \\(y^\prime=f^\prime(x)=\frac{\mathrm{d}y}{\mathrm{d}x}\\) , 最终将一边化为 \\(\frac{\mathrm{d}y}{\mathrm{d}x}\\)
   * 例: \\(e^{x+y}=x^2+y+1\\) 确定 \\(y\\) 为 \\(x\\) 的函数, 求 \\(\frac{\mathrm{d}y}{\mathrm{d}x}\\)
     解: 两边对 \\(x\\) 求导得
     <div>
     $$
     \begin{aligned}
      & e^{x+y}(1+\frac{\mathrm{d}y}{\mathrm{d}x})=2x+\frac{\mathrm{d}y}{\mathrm{d}x} \\
      \hArr & (e^{x+y}-1)\frac{\mathrm{d}y}{\mathrm{d}x}=2x-e^{x+y} \\
      \hArr & \frac{\mathrm{d}y}{\mathrm{d}x}=\frac{2x-e^{x+y}}{e^{x+y}-1}
     \end{aligned}
     $$
     </div>
2. 参数方程确定的函数求导
   设 \\(\begin{cases} x=\varphi(t) \\\ y=\psi(t) \end{cases}\\) , 其中 \\(\varphi(t) , \psi(t)\\) 可导, 则 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=\frac{\psi^\prime(t)}{\varphi^\prime(t)}\enspace\\) (\\(\varphi(t)\neq 0\\))
   证明: \\(\frac{\psi^\prime(t)}{\varphi^\prime(t)}=\frac{\frac{\mathrm{d}y}{\mathrm{d}t}}{\frac{\mathrm{d}x}{\mathrm{d}t}}=\frac{\mathrm{d}y}{\mathrm{d}x}\\)

### 微分

1. 微分定义
   1. 什么是微分
      设 \\(y=f(x)\enspace\\) (\\(x\in D\\)) , \\(\Delta y=f(x_0+\Delta x)-f(x_0)\enspace\\) (\\(x_0,(x_0+\Delta x)\in D\\))
      若 \\(\Delta y\\) 和 \\(\Delta x\\) 能化为线性形式: \\(\Delta y=A\Delta x+\circ(\Delta x)\\)
      则该函数在点 \\(x=x_0\\) 可微, 记作 \\(\mathrm{d}y|_{x=x_0}=A\Delta x=A\mathrm{d}x\\)
   2. 结合导数定义 \\(f^\prime(x)=\frac{\mathrm{d}y}{\mathrm{d}x}\\) 可得:
      * \\(\mathrm{d}y=f^\prime(x)\mathrm{d}x\approx \lim\limits_{\Delta x\to 0}\Delta y\\)
      * \\(\mathrm{d}x=\lim\limits_{\Delta x\to 0}\Delta x\\)
      
      可以发现: 可导 \\(\hArr\\) 可微
      (一般情况下都是先有导数后求微分, 微分的主要应用是在知道导数的情况下求 \\(\Delta y\\) 的近似值 \\(\mathrm{d}y\\))
2. 近似计算:
   若要求 \\(f(x)\\) 的值, 可以找离 \\(x\\) 较近的点 \\(x_0\\) , 它们的距离是 \\(\Delta x\\), 且 \\(f(x_0)\\) 的值已知
   则 \\(f(x)=f(x_0+\Delta x)=f(x_0)+\Delta y\approx f(x_0)+f^\prime(x_0)\Delta x\\)
   * 求近似值还可利用 0 的特殊性, 即 \\(x\to 0\\) 时, \\(f(x)=f(0+x)\approx f(0)+f^\prime(0)x\\)
     1. \\(\sqrt[n]{1+x}\approx 1+\frac{x}{n}\\)
     2. \\(e^x\approx 1+x\\)
     3. \\(\ln(1+x)\approx x\\)
3. 微分公式
   1. \\(\mathrm{d}\(c\)=c^\prime\mathrm{d}x=0\\)
   2. \\(\mathrm{d}(x^a)=ax^{a-1}\mathrm{d}x\\)
   3. \\(\mathrm{d}(a^x)=a^x\ln a\mathrm{d}x\\)
   4. \\(\mathrm{d}(\log_a x)=\frac{1}{x\ln a}\mathrm{d}x\\)
   5. \\(\mathrm{d}(\sin x)=\cos x\mathrm{d}x\\)
      \\(\mathrm{d}(\cos x)=-\sin x\mathrm{d}x\\)
      \\(\mathrm{d}(\tan x)=\sec^2 x\mathrm{d}x\\)
      \\(\mathrm{d}(\cot x)=-csc^2 x\mathrm{d}x\\)
      \\(\mathrm{d}(\sec x)=\sec x\tan x\mathrm{d}x\\)
      \\(\mathrm{d}(\csc x)=-\csc x\cot x\mathrm{d}x\\)
   6. \\(\mathrm{d}(\arcsin x)=\frac{1}{\sqrt{1-x^2}}\mathrm{d}x\\)
      \\(\mathrm{d}(\arccos x)=-\frac{1}{\sqrt{1-x^2}}\mathrm{d}x\\)
      \\(\mathrm{d}(\arctan x)=\frac{1}{1+x^2}\mathrm{d}x\\)
      \\(\mathrm{d}(\operatorname{arccot} x)=-\frac{1}{1+x^2}\mathrm{d}x\\)
4. 微分四则运算
   设函数 \\(u\\)、\\(v\\)
   1. 加减: \\(\mathrm{d}(u\pm v)=\mathrm{d}u\pm\mathrm{d}v\\)
   2. 乘: \\(\mathrm{d}(uv)=\mathrm{d}u\cdot v+u\cdot\mathrm{d}v\\)
   3. 除: \\(\mathrm{d}(\frac{u}{v})=\frac{\mathrm{d}u\cdot v-u\cdot \mathrm{d}v}{v^2}\\)
5. 复合函数求微分
   设 \\(y=f(u)\\) , \\(u=\varphi(x)\\)
   则 \\(\mathrm{d}y=f^\prime[\varphi(x)]\varphi^\prime(x)\mathrm{d}x=f^\prime[\varphi(x)]\mathrm{d}\varphi(x)=f^\prime(u)\mathrm{d}u\\)

## 微分中值定理及导数应用

### 微分中值定理

1. 罗尔中值定理
   {% asset_img 7.png %}
   若:
   1. \\(f(x)\in c[a,b]\\)
   2. \\(f(x)\\) 在 \\((a,b)\\) 内可导
   3. \\(f(a)=f(b)\\)

   则 \\(\exist\xi\in(a,b)\\) , 使 \\(f^\prime(\xi)=0\\)
2. 拉格朗日中值定理
   {% asset_img 8.png %}
   若:
   1. \\(f(x)\in c[a,b]\\)
   2. \\(f(x)\\) 在 \\((a,b)\\) 内可导
   
   则 \\(\exist\xi\in(a,b)\\) , 使 \\(f^\prime(\xi)=\frac{f(b)-f(a)}{b-a}\\)
   (几何意义为 \\(a,b\\) 间存在某个点的斜率等于 \\(a,b\\) 两点所成直线的斜率. 某点导数就是该点切线的斜率)
   
   * 证明:
     作虚线 \\(L_{ab}\\) 连接点 \\(a,b\\) , 带入直线点斜式方程可得:
     \\(L_{ab}:y-f(a)=\frac{f(b)-f(a)}{b-a}(x-a)\rArr y=f(a)+\frac{f(b)-f(a)}{b-a}(x-a)\\\)
     令 \\(\varphi(x)=\text{曲}-\text{直}=f(x)-y=f(x)-f(a)-\frac{f(b)-f(a)}{b-a}(x-a)\\)
     \\(\varphi(x)\in c[a,b]\\) , \\(\varphi(x)\\) 在 \\((a,b)\\) 内可导且 \\(\varphi(a)=\varphi(b)=0\\)
     根据罗尔中值定理, \\(\exist\xi\in(a,b)\\) , 使 \\(\varphi^\prime(\xi)=0\\)
     \\(\therefore\varphi^\prime(\xi)=f^\prime(\xi)-\frac{f(b)-f(a)}{b-a}=0\rArr f^\prime(\xi)=\frac{f(b)-f(a)}{b-a}\\)
3. 柯西中值定理
   若:
   1. \\(f(x),g(x)\in c[a,b]\\)
   2. \\(f(x),g(x)\\) 在 \\((a,b)\\) 内可导
   3. \\(g^\prime(x)\neq 0\enspace\\) (\\(x\in(a,b)\\))
   
   则 \\(\exist\xi\in(a,b)\\) , 使 \\(\frac{f^\prime(\xi)}{g^\prime(\xi)}=\frac{f(b)-f(a)}{g(b)-g(a)}\\)

   证明:
   构造辅助函数 \\(\varphi(x)=f(x)-f(a)-\frac{f(b)-f(a)}{g(b)-g(a)}[g(x)-g(a)]\\)
   \\(\varphi(x)\in c[a,b]\\) , \\(\varphi(x)\\) 在 \\((a,b)\\) 内可导且可知 \\(\varphi(a)=\varphi(b)=0\\)
   \\(\therefore\exist\xi\in(a,b)\\) , 使 \\(\varphi^\prime(\xi)=0\\)
   \\(\therefore\varphi^\prime(\xi)=f^\prime(\xi)-\frac{f(b)-f(a)}{g(b)-g(a)}g^\prime(\xi)=0\rArr\frac{f^\prime(\xi)}{g^\prime(\xi)}=\frac{f(b)-f(a)}{g(b)-g(a)}\\)

### 洛必达法则

1. 问题的起源: 求 \\(\lim\limits_{x\to 0}\frac{\tan x-\sin x}{x^3}\\)
   解:
   法1: 原式 \\(=\lim\limits_{x\to 0}\frac{\tan x}{x}\cdot\frac{1-\cos x}{x^2}=\frac{1}{2}\enspace\\) (提示 \\(\tan x=\frac{\sin x}{\cos x}\\))
   法2: 原式 \\(=\lim\limits_{x\to 0}\frac{x-x}{x^3}=0\\) (不行, 精确度不够)
   同样使用等价无限小, 却得到了不同的结果.
   说明对于存在 无穷小比无穷小(\\(\frac{0}{0}\\)) 的极限 , 用**等价无穷小解极限有局限性**, 分子分母经等价无穷小转化后不同阶, 导数精确度不够

   洛必达法则的目标: 解 \\(\frac{0}{0}\\) , \\(\frac{\infty}{\infty}\\) 类极限新方法
2. 洛必达法则:
   若:
   1. \\(f(x)\\)、\\(g(x)\\) 在点 \\(x=a\\) 的去心领域内可导且 \\(g^\prime(x)\neq 0\\)
   2. \\(\frac{0}{0}\\) 型: \\(\lim\limits_{x\to a}f(x)=0\\) , \\(\lim\limits_{x\to a}g(x)=0\\)
      \\(\frac{\infty}{\infty}\\) 型: \\(\lim\limits_{x\to a}f(x)=\infty\\) , \\(\lim\limits_{x\to a}g(x)=\infty\\)
   3. \\(\lim\limits_{x\to a}\frac{f^\prime(x)}{g^\prime(x)}=A\\)

   则 \\(\lim\limits_{x\to a}\frac{f(x)}{g(x)}=A\\)
3. 洛必达法则证明:
   设 \\(f(a)=0\\) , \\(g(a)=0\\)
   则 \\(\frac{f^\prime(\xi)}{g^\prime(\xi)}=\frac{f(x)-f(a)}{g(x)-g(a)}=\frac{f(x)}{g(x)}\enspace\\) (\\(\xi\in(a,b)\\) , 使用柯西中值定理)
   \\(\therefore\lim\limits_{x\to a}\frac{f(x)}{g(x)}=\lim\limits_{x\to a}\frac{f^\prime(\xi)}{g^\prime(\xi)}=\lim\limits_{\xi\to a}\frac{f^\prime(\xi)}{g^\prime(\xi)}=A\\) 
(\\(x\\)趋向于\\(a\\) , \\(\xi\\) 介于 \\(a\\) 与 \\(x\\) 之间 \\(\rArr\xi\\) 也趋向于 \\(a\\))
4. 注意
   1. 若 \\(\lim\limits_{x\to a}\frac{f^\prime(x)}{g^\prime(x)}\\) 不存在, 只表明洛必达法则不能使用, 不代表极限 \\(\lim\limits_{x\to a}\frac{f(x)}{g(x)}\\) 一定不存在
   2. \\(\lim\limits_{x\to+\infty}\frac{\ln x}{x^a}=0\enspace\\) (\\(a>0\\))
      \\(\lim\limits_{x\to+\infty}\frac{x^a}{b^x}=0\enspace\\) (\\(a>0,b>1\\))

### 泰勒公式

1. 泰勒公式
   设 \\(f(x)\\) 在 \\(x=x_0\\) 邻域内 \\(n+1\\) 阶可导
   则 \\(f(x)=P_n(x)+R_n(x)\\)
   其中:
   \\(P_n(x)=f(x_0)+f^\prime(x_0)(x-x_0)+\frac{f^{\prime\prime}(x_0)}{2!}(x-x_0)^2+\dots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n\\)
   \\(R_n(x)=\circ((x-x_0)^n)=\frac{f^{(n+1)}(\xi)}{(n+1)!}(x-x_0)^{n+1}\enspace\\) (\\(\xi\\) 介于 \\(x_0\\) 与 \\(x\\) 之间)
   如果 \\(x_0=0\\) , 这个式子被称为麦克劳林公式
2. 泰勒公式证明:
   设 \\(f(x)=a_0+a_1(x-x_0)+a_2(x-x_0)^2+a_3(x-x_0)^3+\dots+a_n(x-x_0)^n\\)
   要使这个式子派用场, 得找到 \\(a_0,a_1,\dots\\) 这些系数的值, 怎么做呢?
   要得到 \\(a_0\\) , 可以使 \\(x=x_0\\), 这样其他项就消除了
   那其他的 \\(a_1,a_2,a_3\\) 呢? 我们可以不断对该式求它的一阶, 二阶, 三阶, ... 导数:
   \\(f^\prime(x)=a_1+2a_2(x-x_0)+3a_3(x-x_0)^2+\dots+na_n(x-x_0)^{n-1}\\)
   \\(f^{\prime\prime}(x)=2a_2+3\cdot 2a_3(x-x_0)^2+\dots+n(n-1)(x-x_0)^{n-2}\\)
   \\(\dots\\)
   发现规律了吗, 将 \\(x=x_0\\) 带入, 可以轻松得到对应项的系数值:
   \\(a_1=\frac{f^\prime(x_0)}{1}\\) , \\(a_2=\frac{f^{\prime\prime}(x_0)}{1\cdot 2}\\) , \\(a_3=\frac{f^{\prime\prime\prime}(x_0)}{1\cdot 2\cdot 3}\rArr a_n=\frac{f^{(n)}(x_0)}{n!}\\)
   然后式子就能写成 \\(f(x)=f(x_0)+f^\prime(x_0)(x-x_0)+\frac{f^{\prime\prime}(x_0)}{2!}(x-x_0)^2+\dots+\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n\\)

(常见泰勒展开式见章节: 无穷级数->函数展开成幂级数)

### 导数与函数单调性和曲线凹凸性的关系

1. 导数与函数单调性
   导数可以判别函数的单调性: (导数就是函数变化的速度, 速度的正负代表函数变化方向)
   设 \\(f(x)\enspace\\) (\\(x\in[a,b]\\) 且在 \\((a,b)\\) 内可导)
   1. 若 \\(f^\prime(x)>0\enspace\\) (\\(a<x<b\\)) , 则 \\(f(x)\\) 在 \\([a,b]\\) 上单调递增
   2. 若 \\(f^\prime(x)<0\enspace\\) (\\(a<x<b\\)) , 则 \\(f(x)\\) 在 \\([a,b]\\) 上单调递减
2. 导数与曲线凹凸性
   1. 曲线凹凸性的定义:
      1. 若 \\(\forall x_1,x_2\in D\\) 且 \\(x_1\neq x_2\\) , 有
         \\(f(\frac{x_1+x_2}{2})<\frac{f(x_1)+f(x_2)}{2}\\)
         则 \\(f(x)\\) 在 \\(D\\) 内为凹函数
         
         {% asset_img 9.png %}
      2. 若 \\(\forall x_1,x_2\in D\\) 且 \\(x_1\neq x_2\\) , 有
         \\(f(\frac{x_1+x_2}{2})>\frac{f(x_1)+f(x_2)}{2}\\)
         则 \\(f(x)\\) 在 \\(D\\) 内为凸函数

         {% asset_img 10.png %}
   2. 二阶导数判别法: (可理解为二阶导数就是函数变化的加速度, 加速度的变化代表函数凹凸)
      设 \\(f(x)\enspace\\) (\\(x\in[a,b]\\) 且在 \\((a,b)\\) 内二阶可导)
      1. 若 \\(f^{\prime\prime}(x)>0\enspace\\) (\\(a<x<b\\)) , 则 \\(y=f(x)\\) 图像在 \\([a,b]\\) 上是凹的
      2. 若 \\(f^{\prime\prime}(x)<0\enspace\\) (\\(a<x<b\\)) , 则 \\(y=f(x)\\) 图像在 \\([a,b]\\) 上是凸的

### 极值与最值

一个函数中, 极大值、极小值可以有很多, 但最大值、最小值只能有一个

1. 极大值与极小值
   1. 定义:
      设 \\(y=f(x)\enspace\\) (\\(x,x_0\in D\\))
      若 \\(\exist\delta>0\\) , 当 \\(0<|x-x_0|<\delta\\) 时
      1. 有 \\(f(x)>f(x_0)\\) , 称 \\(x_0\\) 为极小点, \\(f(x_0)\\) 为极小值
      2. 有 \\(f(x)<f(x_0)\\) , 称 \\(x_0\\) 为极大点, \\(f(x_0)\\) 为极大值
   2. 求极值的步骤:
      1. 法一(利用目标点附近导数)
         1. 若 \\(\begin{cases} x<x_0 & f^\prime(x)<0 \\\ x>x_0 & f^\prime(x)>0 \end{cases}\\) , \\(x=x_0\\) 为极小点
         2. 若 \\(\begin{cases} x<x_0 & f^\prime(x)>0 \\\ x>x_0 & f^\prime(x)<0 \end{cases}\\) , \\(x=x_0\\) 为极大点
      2. 法二(利用二阶导数)
         设 \\(f^\prime(x_0)=0\\) , \\(f^{\prime\prime}(x_0)\begin{cases} >0 & x_0 \text{为极小点} \\\ < 0 & x_0 \text{为极大点} \end{cases}\\)
2. 最大值与最小值
   设 \\(f(x)\in c[a,b]\\)
   则极小值和极大值可在 \\(f(a)\\)、\\(f(b)\\) 和其他使 \\(f'(x)=0\\) 或不存在的点之中找到

### 函数图像描绘

1. 渐近线
   设函数 \\(f(x)\\)
   1. 水平渐近线:
      {% asset_img 11.png %}
      
      若 \\(\lim\limits_{x\to\infty}f(x)=A\\)
      称 \\(f(x)\\) 有水平渐近线 \\(y=A\\)
   2. 垂直渐近线:
      {% asset_img 12.png %}

      若 \\(\lim\limits_{x\to a}f(x)=\infty\\)
      称 \\(f(x)\\) 有垂直渐近线 \\(x=a\\)
   3. 斜渐近线:
      若
      1. \\(\lim\limits_{x\to\infty}\frac{f(x)}{x}=k \quad\\) (可以理解为斜率)
      2. \\(\lim\limits_{x\to\infty}[f(x)-kx]=b\\)
      
      称 \\(f(x)\\) 有斜渐近线 \\(y=ax+b\\)
2. 作图
   (根据一阶和二阶导数的值描绘函数曲线)
设函数 \\(f(x) , x\in D\\)
1. 在定义域内:
   找出满足 \\(f^\prime(x)=0\\) 或不存在的所有点
   找出满足 \\(f^{\prime\prime}(x)=0\\) 不存在的所有点
2. 画渐近线
3. 作表
   | \\(x\\) | () | ? | () | ? | \\(\dots\\) |
   |-|-|-|-|-|-|
   | \\(f^\prime(x)\\) | \\(+\\) |  | \\(-\\) |  |  |
   | \\(f^{\prime\prime}(x)\\) | \\(+\\) |  | \\(+\\) |  |  |
   | \\(f(x)\\) | \\(\nearrow\\) | 极大 | \\(\searrow\\) |  |  |
4. 在坐标系上找到关键点, 描图

### 弧微分与曲率

1. 弧微分
   前面的微分能让我们求 \\(\Delta y\\) 的近似值, 也就是从 \\(x\\) 到 \\(x_0\\) 后 \\(y\\) 轴的偏移量近似值
   但如果我们想要求从 \\(x\\) 到 \\(x_0\\) 间这段函数曲线长度的近似值(即弧微分)呢?
   {% asset_img 13.png %}

   两种弧微分形式:
   1. 普通函数
      \\(L:f(x)\\)
      \\(\mathrm{d}s=\sqrt{(\mathrm{d}x)^2+(\frac{\mathrm{d}y}{\mathrm{d}x})^2(\mathrm{d}x)^2}=\sqrt{1+(\frac{\mathrm{d}y}{\mathrm{d}x})^2}\mathrm{d}x=\sqrt{1+[f^\prime(x)]^2}\mathrm{d}x\\)
   2. 参数方程
      \\(L:\begin{cases} x=\varphi(t) \\\ y=\psi(t) \end{cases}\\)
      \\(\mathrm{d}s=\sqrt{(\mathrm{d}x)^2+(\mathrm{d}y)^2}=\sqrt{(\frac{\mathrm{d}x}{\mathrm{d}t})^2+(\frac{\mathrm{d}y}{\mathrm{d}t})^2}\mathrm{d}t=\sqrt{[\varphi^\prime(t)]^2+[\psi^\prime(t)]^2}\mathrm{d}t\\)
2. 曲率与曲率半径
   1. 曲率大小的因素:
      1. 
         {% asset_img 14.png %}
         
         \\(\Delta\alpha\\) 一定, \\(|\stackrel\frown{M_1N_1}|<|\stackrel\frown{M_2N_2}|\\)
         **角度一定, 弯曲度与两点间的弧长成反比**
      2. 
         {% asset_img 15.png %}

         \\(|\stackrel\frown{MN}|\\) 一定, \\(\Delta\alpha_1>\Delta\alpha_2\\)
         **弧长一定, 弯曲度与切线夹角成正比**
3. 曲率定义:
   {% asset_img 16.png %}
   (切线夹角即切线的变化角度)
   设 \\(L:f(x)\\) , \\(|\stackrel\frown{MM^\prime}|=\Delta s\\)
   * 平均曲率 \\(\bar{k}=\dfrac{|\Delta\alpha|}{|\Delta s|}\\)
   * 某点曲率 \\(k=\lim\limits_{\Delta x\to 0}|\frac{\Delta\alpha}{\Delta s}|=|\frac{\mathrm{d}\alpha}{\mathrm{d}s}|=\frac{|f^{\prime\prime}(x)|}{(1+[f^\prime(x)]^2)^\frac{3}{2}}\\)
   * 曲率半径: \\(R=\frac{1}{k}\\)
   * 证明:
     {% asset_img 17.png %}
     
     可知 \\(f^\prime(x)=\lim\limits_{\Delta x\to 0}\tan\alpha\\) , 两边对 \\(x\\) 求导得 \\(f^{\prime\prime}(x)=\sec^2\alpha\cdot\frac{\mathrm{d}\alpha}{\mathrm{d}x}\\)
     \\(\because\sec^2\alpha=1+\tan^2\alpha=1+[f^\prime(x)]^2\\)
     \\(\therefore\frac{\mathrm{d}\alpha}{\mathrm{d}x}=\frac{f^{\prime\prime}(x)}{1+[f^\prime(x)]^2}\rArr\mathrm{d}\alpha=\frac{f^{\prime\prime}(x)}{1+[f^\prime(x)]^2}\mathrm{d}x\\)


## 不定积分

### 不定积分的概念与性质

1. 原函数:
   设 \\(f(x)\\) , \\(F(x)\enspace\\) (\\(x\in I)\\)
   若 \\(\forall x\in I\\) , 有 \\(F^\prime(x)=f(x)\\) , 称 \\(F(x)\\) 为 \\(f(x)\\) 的**其中一个**原函数
   注意:
   1. 一个函数若有原函数, 则一定有无数个原函数
      设 \\(F^\prime(x)=f(x)\\)
      则 \\((F(x)+C)\\) 为 \\(f(x)\\) 的一切原函数
   2. 一个函数的的任意两个原函数之差 \\(\in C\\) 
2. 不定积分
   设 \\(F(x)+C\\) 为 \\(f(x)\\) 的所有原函数
   则 \\(F(x)+C\\) 就是 \\(f(x)\\) 的不定积分, 记作 \\(\int f(x)\mathrm{d}x\\)
3. 不定积分公式
   1. \\(\int k\mathrm{d}x=kx+C\\)
   2. \\(\int x^a\mathrm{d}x=\begin{cases} \frac{1}{a+1}x^{a+1}+C & (a\neq -1) \\\ \ln|x|+C & (a=-1) \end{cases}\\)
   1. \\(\int a^x\mathrm{d}x=\frac{a^x}{\ln a}+C\\)
   2. \\(\int\sin x\mathrm{d}x=-\cos x+C\\)
      \\(\int\cos x\mathrm{d}x=\sin x+C\\)
      \\(\int\tan x\mathrm{d}x=-\ln|\cos x|+C\\)
      \\(\int\cot x\mathrm{d}x=\ln|\sin x|+C\\)
      \\(\int\csc x\mathrm{d}x=\ln|\csc x-\cot x|+C=\ln|\tan\frac{x}{2}|+C\\)
      \\(\int\sec x\mathrm{d}x=\ln|\sec x+\tan x|+C\\)
      \\(\int\sec^2x\mathrm{d}x=\tan x+C\\)
      \\(\int\csc^2x\mathrm{d}x=-\cot x+C\\)
      \\(\int\sec x\tan x\mathrm{d}x=\sec x+C\\)
      \\(\int\csc x\cot x\mathrm{d}x=-\csc x+C\\)
   3. \\(\int\frac{1}{\sqrt{1-x^2}}\mathrm{d}x=\arcsin x+C\\)
      \\(\int\frac{1}{\sqrt{a^2-x^2}}\mathrm{d}x=\arcsin\frac{x}{a}+C\\)
      \\(\int\frac{1}{1+x^2}\mathrm{d}x=\arctan x+C\\)
      \\(\int\frac{1}{a^2+x^2}\mathrm{d}x=\frac{1}{a}\arctan\frac{x}{a}+C\\)
      \\(\int\frac{1}{x^2-a^2}\mathrm{d}x=\frac{1}{2a}\ln|\frac{x-a}{x+a}|+C\\)
      \\(\int\frac{1}{\sqrt{x^2+a^2}}\mathrm{d}x=\ln(x+\sqrt{x^2+a^2})+C\\)
      \\(\int\frac{1}{\sqrt{x^2-a^2}}\mathrm{d}x=\ln|x+\sqrt{x^2-a^2}|+C\\)
      \\(\int\frac{1}{\sqrt{a^2-x^2}}\mathrm{d}x=\frac{a^2}{2}\arcsin\frac{x}{a}+\frac{1}{2}x\sqrt{a^2-x^2}+C\\)

   * 推导:
     1. \\(\int x^a\mathrm{d}x\enspace(a=-1)\rArr\begin{cases} \int\frac{1}{x}\mathrm{d}x=\ln(-x)+C & (x<0) \\\ \int\frac{1}{x}\mathrm{d}x=\ln(x)+C & (x>0) \end{cases}\rArr\ln|x|+C\\)
     2. \\(\int\tan x\mathrm{d}x=\int\frac{\sin x}{\cos x}\mathrm{d}x=-\int\frac{1}{\cos x}\mathrm{d}(\cos x)=-\ln|\cos x|+C\\)
     3. \\(\int\cot x\mathrm{d}x=\int\frac{\cos x}{\sin x}\mathrm{d}x=\int\frac{1}{\sin x}\mathrm{d}(\sin x)=\ln|\sin x|+C\\)
     4. 这里只证明 \\(\int\frac{1}{\sqrt{x^2+a^2}}\mathrm{d}x\\)
        
        {% asset_img 18.png %}
        解: 令 \\(x=a\tan t\\) (第二类换元积分法)
        <div>
        $$
        \begin{aligned}
         \text{原式}=\int\frac{a\sec^2t}{a\sec t}\mathrm{d}t & =\int\sec t\mathrm{d}t \\
         & =\ln|\sec t+\tan t|+C \\
         & =\ln|\frac{\sqrt{x^2+a^2}}{a}+\frac{x}{a}|+C \\
         & =\ln|x+\sqrt{x^2+a^2}|\cdot\frac{1}{a}+C \\
         & =\ln|x+\sqrt{x^2+a^2}|+\ln\frac{1}{a}+C \\
         & =\ln|x+\sqrt{x^2+a^2}|+C\enspace(\ln\frac{1}{a}\text{并入}C) \\
         & =\ln|x+\sqrt{x^2+a^2}|+C \\
         & =\ln(x+\sqrt{x^2+a^2})+C
        \end{aligned}
        $$
        </div>

### 不定积分性质

1. \\(\int[f(x)\pm g(x)]\mathrm{d}x=\int f(x)\mathrm{d}x+\int g(x)\mathrm{d}x\\)
2. \\(\int af(x)\mathrm{d}x=a\int f(x)\mathrm{d}x\\)

### 积分方法

1. 换元积分法
   1. 第一类换元积分法
      \\(f(u)\\) 存在原函数, \\(\varphi(x)\\) 可导, \\(F(u)\\) 为 \\(f(u)\\) 的原函数, 则
      \\(\int f[\varphi(x)]\underbrace{\varphi^\prime(x)\mathrm{d}x}_{f^\prime(x)\mathrm{d}x=\mathrm{d}f(x)}=\int\underbrace{f[\varphi(x)]\mathrm{d}\varphi(x)}\_{\text{设}\varphi(x)=t}=\int f(t)\mathrm{d}t=F(t)+C=F[\varphi(x)]+C\\)
   2. 第二类换元积分法
      \\(x=\psi(t)\\) 可导且 \\(\psi^\prime(t)\neq 0\\)
      \\(\int f(x)\mathrm{d}x=\int f[\psi(t)]\psi^\prime(t)\mathrm{d}t=\int g(t)\mathrm{d}t=G(t)+C=G[\psi^{-1}(x)]+C\enspace\\) (\\(t\\) 为反函数 \\(\psi^{-1}(x)\\))
      例: \\(\int\frac{1}{\sqrt{x}+\sqrt[3]{x}}\mathrm{d}x\\)
      解:
      令 \\(x=t^6\\) , 有
      <div>
      $$
      \begin{aligned}
       & 6\int\frac{t^5}{t^3+t^2}\mathrm{d}t \\
       & =6\int\frac{(t^3+1)-1}{t+1}\mathrm{d}t \\
       & =6\int\frac{t^3+t^2+1-t^2}{t+1}-\frac{1}{t+1}\mathrm{d}t \\
       & =6\int\frac{t^2(t+1)+(1+t)(1-t)}{t+1}-\frac{1}{t+1}\mathrm{d}t \\
       & =6\int(t^2-t+1)-\frac{1}{t+1}\mathrm{d}t \\
       & =6[\frac{1}{3}t^3-\frac{1}{2}t^2+t-\ln|t+1|]+C \\
       & =2t^3-3t^2+6t-6\ln|t+1|+C \\
       & =2\sqrt{x}-3\sqrt[3]{x}+6\sqrt[6]{x}-6\ln|\sqrt[6]{x}+1|+C
      \end{aligned}
      $$
      </div>
2. 分部积分法
   第一步先用换元积分法将积分式子变成 \\(\int u\mathrm{d}v\\) 的形式
   然后用分部积分公式: \\(\int u\mathrm{d}v=uv-\int v\mathrm{d}u\\)
   * 推导:
     结合导数四则运算和不定积分性质可得 \\(\int(uv)^\prime\mathrm{d}x=\int u^\prime v\mathrm{d}x+\int uv^\prime\mathrm{d}x\\)
     结合第一类换元积分法进一步变换得 \\(\underbrace{uv}_{\int(uv)^\prime\mathrm{d}x}=\int v\mathrm{d}u+\int u\mathrm{d}v\\)
   
   例: \\(\int x^2\ln x\mathrm{d}x\\)
   解:
   <div>
   $$
   \begin{aligned}
    \int x^2\ln x\mathrm{d}x & =\int\ln x\mathrm{d}(\frac{1}{3}x^3) \\
    & =\frac{1}{3}x^3\ln x-\int\frac{1}{3}x^3\mathrm{d}(\ln x) \\
    & =\frac{1}{3}x^3\ln x-\frac{1}{3}\int x^2\mathrm{d}x \\
    & =\frac{1}{3}x^3\ln x-\frac{1}{9}x^3+C
   \end{aligned}
   $$
   </div>

## 定积分

### 定积分的概念与性质

1. 定积分定义
   设 \\(f(x)\\) 在 \\([a,b]\\) 上有界(有界不一定可积) , \\(x_1,x_2,\dots,x_n\\) 为 \\([a,b]\\) 间的分段点
   1. \\(a=x_0<x_1<x_2<\dots<x_n=b\\) , \\(\Delta x_i=x_i-x_{i-1}\enspace\\) (\\(1\leqslant i\leqslant n)\\)
   2. \\(\exist s_i\in[x_{i-1},x_i]\\) , 作 \\(\displaystyle\sum_{i=1}^n f(\xi_i)\Delta x_i\\)
   3. \\(\lambda=\operatorname{max}\\{\Delta x_1,\Delta x_2,\dots,\Delta x_n\\}\\)
   
   若 \\(\displaystyle\lim\limits_{\lambda\to 0}\sum_{i=1}^n f(\xi_i)\Delta x_i\\) 存在, 称 \\(f(x)\\) 在 \\([a,b]\\) 上可积
   该极限称为 \\(f(x)\\) 在 \\([a,b]\\) 上的定积分, 记作 \\(\int_a^b f(x)\mathrm{d}x\\)
   即 \\(\lim\limits_{\lambda\to 0}\displaystyle\sum_{i=1}^n\textstyle f(\xi_1)\Delta x_i=\int_a^b f(x)\mathrm{d}x\\)
   
   注意:
   1. \\(L:y=f(x)\geqslant 0\enspace\\) (\\(x\in[a,b]\\))
      则 \\(A=\int_a^b f(x)\mathrm{d}x\\)
      例: 物理中知道速度函数和时间求位移(\\(v\\) 为速度, \\(t\\) 为时间)
      设 \\(v=V(t)\enspace\\) (\\(t\in[a,b]\\))
      则 \\(S=\int_a^b V(t)\mathrm{d}t\\)
   2. \\(\displaystyle\lim\limits_{\lambda\to 0}\sum_{i=1}^n f(\xi_1)\Delta x_i\\) 与 \\([a,b]\\) 分法及 \\(\xi_i\\) 取法无关
   3. \\(f(x)\\) 在 \\([a,b]\\) 上有界不一定可积
      如分段函数 \\(f(x)=\begin{cases} 1 & x\in Q \\\ 0 & x\in R-Q \end{cases}\\)
   4. 若 \\(f(x)\in c[a,b]\\) , 则 \\(f(x)\\) 在 \\([a,b]\\) 上可积
      若 \\(f(x)\\) 在 \\([a,b]\\) 上只有有限个第一类间断点, 则 \\(f(x)\\) 在 \\([a,b]\\) 上可积
2. 定积分的一般性质
   1. \\(\int_a^a f(x)\mathrm{d}x=0\\)
      \\(\int_a^b f(x)\mathrm{d}x=-\int_b^a f(x)\mathrm{d}x\\)
   2. \\(\int_a^b[f(x)\pm g(x)]\mathrm{d}x=\int_a^b f(x)\mathrm{d}x\pm\int_a^b g(x)\mathrm{d}x\\)
   3. \\(\int_a^b kf(x)\mathrm{d}x=k\int_a^b f(x)\mathrm{d}x\\)
   4. \\(\int_a^b f(x)\mathrm{d}x=\int_a^c f(x)\mathrm{d}x+\int_c^b f(x)\mathrm{d}x\enspace\\) (\\(a<b\\))
      (即使 \\(c<a<b\\) 也成立, 因为 \\(\int_a^c\\) 是负的, 正好减去了 \\(\int_c^b\\) 多出来的)
   5. \\(\int_a^b 1\mathrm{d}x=b-a\\)
   6. 定积分间的比较
      1. \\(f(x)\geqslant 0\enspace\\) (\\(a\leqslant x\leqslant b\\)) , 则 \\(\int_a^b f(x)\mathrm{d}x\geqslant 0\\)
      2. \\(f(x)\geqslant g(x)\enspace\\) (\\(a\leqslant x\leqslant b\\)) , 则 \\(\int_a^b f(x)\mathrm{d}x\geqslant\int_a^b g(x)\mathrm{d}x\\)
      3. 若 \\(f(x)\\)、\\(|f(x)|\\) 在 \\([a,b]\\) 上可积, 则 \\(|\int_a^b f(x)\mathrm{d}x|\leqslant\int_a^b|f(x)|\mathrm{d}x\\)
   7. 积分中值定理
      {% asset_img 19.png %}
      
      设 \\(f(x)\in c[a,b]\\) , 则 \\(\exist\xi\in[a,b]\\) , 使 \\(\underbrace{\int_a^b f(x)\mathrm{d}x}\_{\text{曲边梯形面积}}=\underbrace{f(\xi)(b-a)}_{\text{矩形面积}}\\)
      * 证明:
        \\(\because f(x)\in c[a,b]\\)
        \\(\therefore\exist m,M\in[a,b]\\) , 使 \\(m(b-a)\leqslant\int_a^b f(x)\mathrm{d}x\leqslant M(b-a)\\) (介值定理)
        \\(\rArr m\leqslant\frac{1}{b-a}\int_a^b f(x)\mathrm{d}x\leqslant M\\)
        \\(\therefore\exist\xi\in[a,b]\\) , 使 \\(f(\xi)=\frac{1}{b-a}\int_a^b f(x)\mathrm{d}x\rArr\int_a^b f(x)\mathrm{d}x=(b-a)f(\xi)\\)

### 积分基本定理

1. 积分上限函数及其与目标函数的关系:
   设 \\(f(x)\in c[a,b]\\) , 令 \\(\varPhi(x)=\int_a^x f(t)\mathrm{d}t\\) , 则 \\(\varPhi^\prime(x)=\frac{\mathrm{d}}{\mathrm{d}x}\int_a^x f(t)\mathrm{d}t=f(x)\\)
   复合函数推论: \\(\varPhi^\prime(\varphi(x))=\frac{\mathrm{d}}{\mathrm{d}x}\int_a^{\varphi(x)}f(t)\mathrm{d}t=f[\varphi(x)]\cdot\varphi^\prime(x)\\)

* 证明:
  1. 定积分与不定积分的比较:
     \\(\int f(x)\neq\int f(t)\enspace\\) (\\(x\\) 与 \\(t\\) 可能是函数且是不同的函数, 值域不同)
     \\(\int_a^b f(x)\mathrm{d}x=\int_a^b f(t)\mathrm{d}t\enspace\\) (\\(x\\) 与 \\(t\\) 可能不同, 但值域被限制在 \\([a,b]\\) 所以相等)
     因此: 定积分由上下限、函数关系确定, 与积分变量无关
     即存在积分上限函数 \\(\varPhi(x)=\int_a^x f(x)\mathrm{d}x=\int_a^x f(t)\mathrm{d}t\enspace\\) (注意自变量的 \\(x\\) 与积分上限的 \\(x\\) 不同) 
  2. 因为 \\(\Delta\varPhi=\varPhi(x+\Delta x)-\varPhi(x)=\int_a^{x+\Delta x}f(t)\mathrm{d}t-\int_a^x f(t)\mathrm{d}t=\int_x^{x+\Delta x}f(t)\mathrm{d}t\\)
     \\(\because f(x)\in c[a,b]\rArr f(x)\in c[x,x+\Delta x]\\)
     \\(\therefore\exist\xi\in[x,x+\Delta x]\\) , 使: (积分中值定理)
     <div>
     $$
     \begin{aligned}
      f(\xi)\Delta x=\int_x^{x+\Delta x}f(t)\mathrm{d}t=\Delta\varPhi & \rArr\frac{\Delta\varPhi}{\Delta x}=f(\xi) \\
      & \rArr\lim\limits_{\Delta x\to 0}\frac{\Delta\varPhi}{\Delta x}=\lim\limits_{\Delta x\to 0}f(\xi)\enspace(=\lim\limits_{\xi\to x}f(\xi)=f(x)) \\
      & \rArr\varPhi^\prime(x)=f(x)
     \end{aligned}
     $$
     </div>
2. 牛顿莱布尼茨公式
   \\(f(x)\in c[a,b]\\) , \\(F(x)\\) 为 \\(f(x)\\) 的一个原函数, 则 \\(\int_a^b f(x)\mathrm{d}x=F(b)-F(a)=F(x)|_a^b\\)
   * 证明:
     \\(F^\prime(x)=f(x)\\)
     \\(\varPhi(x)=\int_a^x f(t)\mathrm{d}t\\) , \\(\varPhi^\prime(x)=f(x)\\)
     (\\(F(x)\\) 与 \\(\varPhi(x)\\) 皆为 \\(f(x)\\) 的原函数)
     \\(\therefore \begin{cases} F(a)-\varPhi(a)=C_0 \\\ F(b)-\varPhi(b)=C_0 \end{cases}\rArr F(a)-\varPhi(a)=F(b)-\varPhi(b)\\)
     \\(\because\varPhi(a)=\int_a^a f(t)\mathrm{d}t=0\\)
     \\(\therefore F(a)=F(b)-\varPhi(b)\rArr\varPhi(b)=F(b)-F(a)\\)
     即 \\(\int_a^b f(x)\mathrm{d}x=F(b)-F(a)\\)
3. 积分中值定理的推广
   设 \\(f(x)\in c[a,b]\\) , 则 \\(\exist\xi\in(a,b)\\) (闭区间), 使 \\(\int_a^b f(x)\mathrm{d}x=f(\xi)(b-a)\\)
   * 证明: 设 \\(F(x)=\int_a^x f(t)\mathrm{d}t\\) , \\(F^\prime(x)=f(x)\\)
     \\(\exist\xi\in(a,b)\\) 使 \\(F^\prime(\xi)=\frac{F(b)-F(a)}{b-a}\\) (拉格朗日中值定理)
     \\(\therefore\int_a^b f(x)\mathrm{d}x=F(b)-F(a)=F^\prime(\xi)(b-a)=f(\xi)(b-a)\enspace\\) (\\(a<\xi<b\\))

### 定积分的换元积分法与分部积分法

1. 换元积分法
   若 \\(f(x)\in c[a,b]\\) , \\(x=\varphi(t)\\) 满足:
   1. \\(\varphi(t)\\) 为单调函数且 \\(\varphi(\alpha)=a\\) , \\(\varphi(\beta)=b\\)
   2. \\(x=\varphi(t)\\) 连续可导

   则 \\(\int_a^b f(x)\mathrm{d}x=\int_\alpha^\beta f[\varphi(t)]\varphi^\prime(t)\mathrm{d}t\\)

   * 证明:
     设 \\(F(x)\\) 为 \\(f(x)\\) 的原函数
     \\(\because(F[\varphi(t)])^\prime=F^\prime[\varphi(t)]\cdot\varphi^\prime(t)=f[\varphi(t)]\cdot\varphi^\prime(t)\\)
     \\(\therefore\int_\alpha^\beta f[\varphi(t)]\varphi^\prime(t)\mathrm{d}t=F[\varphi(\beta)]-F[\varphi(\alpha)]=F(b)-F(a)=\int\_a^b f(x)\mathrm{d}x\\) (牛顿莱布尼茨公式)
2. 分部积分法
   \\(\int_a^b u\mathrm{d}v=(uv)|\_a^b-\int_a^b v\mathrm{d}u\\)
   Notes:
   \\(I_n=\int_0^{\frac{\pi}{2}}\sin^n x\mathrm{d}x=\int_0^{\frac{\pi}{2}}\cos^n x\mathrm{d}x=\frac{n-1}{n}I_{n-2}\\)
   \\(I_0=\frac{\pi}{2} , I_1=1\\)

### 反常积分

1. 正常积分标准:
   1. 区间有限
   2. \\(f(x)\\) 在区间上连续或第一类间断点(有限个)
2. 积分区间无限
   1. \\(f(x)\in c[a,+\infty)\\)
      有 \\(\lim\limits_{b\to+\infty}[F(b)-F(a)]=\int_a^{+\infty} f(x)\mathrm{d}x\\)
      1. 若 \\(\lim\limits_{b\to+\infty}[F(b)-F(a)]\\) 存在, 称 \\(\int_a^{+\infty} f(x)\mathrm{d}x\\) 收敛
         有 \\(\lim\limits_{b\to+\infty}[F(b)-F(a)]=\int_a^{+\infty} f(x)\mathrm{d}x=A\\)
      2. 若 \\(\lim\limits_{b\to+\infty}[F(b)-F(a)]\\) 不存在, 称 \\(\int_a^{+\infty} f(x)\mathrm{d}x\\) 发散
   2. \\(f(x)\in c(-\infty,a]\\)
      有 \\(\lim\limits_{b\to-\infty}[F(a)-F(b)]=\int_{-\infty}^a f(x)\mathrm{d}x\\)
      1. 若 \\(\lim\limits_{b\to-\infty}[F(a)-F(b)]\\) 存在, 称 \\(\int_{-\infty}^a f(x)\mathrm{d}x\\) 收敛
         有 \\(\lim\limits_{b\to-\infty}[F(a)-F(b)]=\int_{-\infty}^a f(x)\mathrm{d}x=A\\)
      2. 若 \\(\lim\limits_{b\to-\infty}[F(a)-F(b)]\\) 不存在, 称 \\(\int_{-\infty}^a f(x)\mathrm{d}x\\) 发散
   3. \\(f(x)\in c(-\infty,+\infty)\\)
      若 \\(\int_{-\infty}^{+\infty}f(x)\mathrm{d}x\\) 收敛 \\(\hArr\int_{-\infty}^{a}f(x)\mathrm{d}x\\) 与 \\(\int_{a}^{+\infty}f(x)\mathrm{d}x\\) 收敛
      且 \\(\int_{-\infty}^{+\infty}f(x)\mathrm{d}x=\int_{-\infty}^a f(x)\mathrm{d}x+\int_a^{+\infty}f(x)\mathrm{d}x\\)
   4. \\(\Gamma\\) 函数
      1. 定义: \\(\Gamma(\alpha)=\int_0^{+\infty}x^{\alpha-1}\cdot e^{-x}\mathrm{d}x\\)
      2. 特性:
         1. \\(\Gamma(\alpha+1)=\alpha\Gamma(\alpha)\\)
         2. \\(\Gamma(n+1)=n!\enspace\\) (\\(n\in Z\\))
         3. \\(\Gamma(\frac{1}{2})=\sqrt{\pi}\\)
3. 无界函数反常积分
   1. 左无界: \\(f(x)\in c(a,b]\\) 且 \\(f(a+0)=\infty\enspace\\) (\\(a\\) 称作瑕点)
      \\(\forall\varepsilon>0\\) , \\(\lim\limits_{\varepsilon\to 0^+}[F(b)-F(a+\varepsilon)]=\int_{a+\varepsilon}^b f(x)\mathrm{d}x\\)
      1. 若 \\(\lim\limits_{\epsilon\to 0^+}[F(b)-F(a+\varepsilon)]\\) 存在, 称 \\(\int_a^b f(x)\mathrm{d}x\\) 收敛
         若极限是有限实数, 则 \\(\lim\limits_{\varepsilon\to 0^+}[F(b)-F(a+\varepsilon)]=\int_a^b f(x)\mathrm{d}x=A\\)
      2. 若 \\(\lim\limits_{\varepsilon\to 0^+}[F(b)-F(a+\varepsilon)]\\) 不存在, 称 \\(\int_a^b f(x)\mathrm{d}x\\) 发散
   2. 右无界: \\(f(x)\in c[a,b)\\) 且 \\(f(b-0)=\infty\\)
      \\(\forall\varepsilon>0\\) , \\(F(b-\varepsilon)-F(a)=\int_a^{b-\varepsilon}f(x)\mathrm{d}x\\)
      1. 若 \\(\lim\limits_{\epsilon\to 0^+}[F(b-\varepsilon)-F(a)]\\) 存在, 称 \\(\int_a^b f(x)\mathrm{d}x\\) 收敛
         若极限是有限实数, 则 \\(\lim\limits_{\varepsilon\to 0^+}[F(b-\varepsilon)-F(a)]=\int_a^b f(x)\mathrm{d}x=A\\)
      2. 若 \\(\lim\limits_{\epsilon\to 0^+}[F(b-\varepsilon)-F(a)]\\) 不存在, 称 \\(\int_a^b f(x)\mathrm{d}x\\) 发散
   3. 中无界: \\(f(x)\in c[a,c)\cup(c,b]\\) 且 \\(\lim\limits_{x\to c}f(x)=\infty\\)
      若 \\(\int_a^b f(x)\mathrm{d}x\\) 收敛 \\(\hArr\int_a^c f(x)\mathrm{d}x\\) 与 \\(\int_c^b f(x)\mathrm{d}x\\) 收敛
      且 \\(\int_a^b f(x)\mathrm{d}x=\int_a^c f(x)\mathrm{d}x+\int_c^b f(x)\mathrm{d}x\\)

## 定积分应用

1. 元素法
   经典积分思想: 分成无数细小的段然后加起来
   元素法思想:
   {% asset_img 20.png %}
   1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
   2. \\(\mathrm{d}A=f(x)\mathrm{d}x\\)
   3. \\(A=\int_a^b\mathrm{d}A=\int_a^b f(x)\mathrm{d}x\\)

   例: 求 \\(L:x^2+y^2=R^2\\) 围成的面积
   {% asset_img 21.png %}
   解:
   1. 取 \\([x,x+\mathrm{d}x]\subset[0,R]\\)
   2. \\(\mathrm{d}A_1=\sqrt{R^2-x^2}\mathrm{d}x\\)
   3. \\(A_1=\int_0^R\sqrt{R^2-x^2}\mathrm{d}x\\)
   4. 定积分换元积分法设 \\(x=R\sin t\\) , 则有
      \\(\int_0^\frac{\pi}{2}\sqrt{R^2-R^2\sin^2t}(R\cos t)\mathrm{d}t\\)
      \\(=\int_0^\frac{\pi}{2}R^2\sqrt{1-\sin^2t}\cos t\mathrm{d}t\\)
      \\(=\int_0^\frac{\pi}{2}R^2\cos^2t\mathrm{d}t\\)
      \\(=R^2\int_0^\frac{\pi}{2}\cos^2t\mathrm{d}t\\)
      \\(=R^2\times\frac{1}{2}I_0=\frac{\pi}{4}R^2\\)
      \\(\therefore A=4A_1=\pi R^2\\)
2. 几何应用(元素法的拓展)
   1. 面积
      1. 贴 \\(x\\) 轴曲面
         {% asset_img 22.png %}

         1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
         2. \\(\mathrm{d}A=f(x)\mathrm{d}x\\)
         3. \\(A=\int_a^b f(x)\mathrm{d}x\\)
      2. 浮空曲边
         {% asset_img 23.png %}

         1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
         2. \\(\mathrm{d}A=[f(x)-g(x)]\mathrm{d}x\\)
         3. \\(A=\int_a^b[f(x)-g(x)]\mathrm{d}x\\)
      3. 曲边扇形面积(以下内容为弧度制)
         \\(L:R=r(\theta)\enspace\\) (\\(\alpha\leqslant\theta\leqslant\beta\\))
         {% asset_img 24.png %}

         1. 取 \\([\theta,\theta+\mathrm{d}\theta]\subset[\alpha,\beta]\\)
         2. \\(\mathrm{d}A=\frac{1}{2}r^2(\theta)\mathrm{d}\theta\\) (曲边扇形面积公式)
         3. \\(A=\int_\alpha^\beta\mathrm{d}A=\int_\alpha^\beta\frac{1}{2}r^2(\theta)\mathrm{d}\theta\\)
   2. 体积
      1. 旋转体的体积
         1. \\(V_x\\) (绕 \\(x\\) 轴旋转)
            {% asset_img 25.png %}

            1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
            2. \\(\mathrm{d}V_x=\pi f^2(x)\mathrm{d}x\\)
            3. \\(V_x=\pi\int_a^b f^2(x)\mathrm{d}x\\)
         2. \\(V_y\\) (绕 \\(y\\) 轴旋转)
            {% asset_img 26.png %}
            {% asset_img 28.png %}

            1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
            2. \\(\mathrm{d}V_y=2\pi|x|\cdot|f(x)|\cdot\mathrm{d}x\\)
            3. \\(V_y=2\pi\int_a^b |x|\cdot|f(x)|\mathrm{d}x\\)
      2. 截口面积已知求几何体体积
         有关于底面积的函数 \\(A(x)\\)
         {% asset_img 27.png %}
         
         1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
         2. \\(\mathrm{d}V=A(x)\mathrm{d}x\\)
         3. \\(V=\int_a^b A(x)\mathrm{d}x\\)
   3. 弧长
      {% asset_img 29.png %}

      1. \\(L:y=f(x)\enspace\\) (\\(a\leqslant x\leqslant b\\))
         1. 取 \\([x,x+\mathrm{d}x]\subset[a,b]\\)
         2. \\(\mathrm{d}s=\sqrt{(\mathrm{d}x)^2+(\mathrm{d}y)^2}=\sqrt{1+(\frac{\mathrm{d}y}{\mathrm{d}x})^2}\mathrm{d}x=\sqrt{1+[f^\prime(x)]^2}\mathrm{d}x\\)
         3. \\(l=\int_a^b\mathrm{d}s\mathrm{d}x=\int_a^b\sqrt{1+[f^\prime(x)]^2}\mathrm{d}x\\)
      2. \\(L:\begin{cases} x=\varphi(t) \\\ y=\psi(t) \end{cases}\enspace\\) (\\(\alpha\leqslant t\leqslant\beta\\))
         1. 取 \\([t,t+\mathrm{d}t]\subset[\alpha,\beta]\\)
         2. \\(\mathrm{d}s=\sqrt{(\mathrm{d}x)^2+(\mathrm{d}y)^2}=\sqrt{[\varphi^\prime(t)]^2+[\psi^\prime(t)]^2}\mathrm{d}t\\)
         3. \\(l=\int_\alpha^\beta\mathrm{d}s\mathrm{d}t=\int_\alpha^\beta\sqrt{[\varphi^\prime(x)]^2+[\psi^\prime(x)]^2}\mathrm{d}t\\)

## 微分方程

解微分方程即根据已知微分条件求目标函数

### 微分方程的基本概念

1. 微分方程: 含有导数或微分的方程
   常微分方程: 只含一个自变量, 一个函数的微分方程, 如 \\(f^\prime(x)+7f(x)=0\\)
   这里探讨的范围仅限于常微分方程
2. 微分方程的阶: 在微分方程中, 导数或微分的最高阶数
3. 微分方程的解:
   已知 \\(y\\) 关于 \\(x\\) 的微分方程 \\(F(y^{(n)},y^{(n-1)},\dots,y^\prime,y,x)=0\\)
   若代入函数 \\(y=\varphi(x)\\) 能够满足 \\(F(y^{(n)},y^{(n-1)},\dots,y^\prime,y,x)=0\\)
   称 \\(y=\varphi(x)\\) 为该微分方程的解
   1. 通解: 设 \\(F(y^{(n)},y^{(n-1)},\dots,y^\prime,y,x)=0\\) 为 \\(n\\) 阶微分方程, 若该方程的解含 \\(n\\) 个相互独立的任意常数, 称该解为通解
      如 \\(y=C_1e^x+C_2e^{2x}\\) 为 \\(y^{\prime\prime}-3y^\prime+2y=0\\) 的通解
   2. 特解: 不含任意常数的解

### 可分离变量微分方程

1. 定义: 形如 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=\varphi_1(x)\varphi_2(y)\\)
2. 解法
   \\(\frac{\mathrm{d}y}{\mathrm{d}x}=\varphi_1(x)\varphi_2(y)\rArr\frac{\mathrm{d}y}{\varphi_2(y)}=\varphi_1(x)\mathrm{d}x\\)
   两边积分得:
   \\(\int\frac{\mathrm{d}y}{\varphi_2(y)}=\int\varphi_1(x)\mathrm{d}x+C\\) (微分方程中自变量一端注意要加 \\(C\\))

### 齐次微分方程

1. 定义: 形如 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=\varphi(\frac{y}{x})\\)
2. 解法
   换元法, 设 \\(u=\frac{y}{x}\\) , 则 \\(y=ux\rArr y^\prime=u^\prime x+ux^\prime\rArr\frac{\mathrm{d}y}{\mathrm{d}x}=x\frac{\mathrm{d}u}{\mathrm{d}x}+u\\)
   将 \\(u=\frac{y}{x}\\) 和 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=x\frac{\mathrm{d}u}{\mathrm{d}x}+u\\) 代入方程得到 \\(x\frac{\mathrm{d}u}{\mathrm{d}x}+u=\varphi(u)\\)
   然后就可以分离变量, 最后两边积分: 
   \\(\frac{\mathrm{d}x}{x}=\frac{\mathrm{d}u}{\varphi(u)-u}\rArr\int\frac{\mathrm{d}x}{x}=\int\frac{\mathrm{d}u}{\varphi(u)-u}+C\\)

### 一阶线性微分方程

1. 一阶齐次线性方程
   1. 定义: 形如 \\(\frac{\mathrm{d}y}{\mathrm{d}x}+P(x)y=0\\)
   2. 解法及通解公式
      * 情况1 \\(y=0\\) 为方程的解
      * 情况2 \\(y\neq 0\\) 时, 先分离变量, 然后两边积分, 再套用积分公式求解:
        <div>
        $$
        \begin{aligned}
         \frac{1}{y}\mathrm{d}y=-P(x)\mathrm{d}x & \rArr\int\frac{1}{y}\mathrm{d}y=\int-P(x)\mathrm{d}x \\
         & \rArr\ln|y|=-\int P(x)\mathrm{d}x+C_0 \\
         & \rArr|y|=e^{-\int P(x)\mathrm{d}x+C_0} \\
         & \rArr y=\pm e^{C_0}\cdot e^{-\int P(x)\mathrm{d}x}=Ce^{-\int P(x)\mathrm{d}x}
        \end{aligned}
        $$
        </div>

        通解 \\(y=Ce^{-\int P(x)\mathrm{d}x}\\)
2. 一阶非齐线性微分方程
   1. 定义: 形如 \\(\frac{\mathrm{d}y}{\mathrm{d}x}+P(x)y=Q(x)\\)
   2. 解法
      通过常数易变法, 将上面齐次方程通解中的常数项换做关于 \\(x\\) 的函数 \\(C=u(x)\\), 得到 \\(y=u(x)e^{-\int P(x)\mathrm{d}x}\\) (1)
      然后对 \\(y\\) 求导: (这里注意复合函数求导法则, 且对积分求导的结果是原函数)
      \\(y^\prime=\frac{\mathrm{d}y}{\mathrm{d}x}=u^\prime(x)e^{-\int P(x)\mathrm{d}x}-u(x)P(x)e^{-\int P(x)\mathrm{d}x}\\) (2)
      将 (1)、(2) 带入一阶非齐线性微分方程得:
      \\(u^\prime(x)e^{-\int P(x)\mathrm{d}x}-u(x)P(x)e^{-\int P(x)\mathrm{d}x}+P(x)u(x)e^{-\int P(x)\mathrm{d}x}=Q(x)\\)
      \\(\rArr u^\prime(x)e^{-\int P(x)\mathrm{d}x}=Q(x)\\)
      \\(\rArr u^\prime(x)=Q(x)e^{\int P(x)\mathrm{d}x}\\)
      \\(\rArr\int u^\prime(x)\mathrm{d}x=\int Q(x)e^{\int P(x)\mathrm{d}x}\mathrm{d}x+C\\)
      \\(\rArr u(x)=\int Q(x)e^{\int P(x)\mathrm{d}x}\mathrm{d}x+C\\)
      求得的 \\(u(x)\\) 再次带入通解式得 \\(y=[\int Q(x)e^{\int P(x)\mathrm{d}x}\mathrm{d}x+C]e^{-\int P(x)\mathrm{d}x}\\)

### 可降阶的高阶微分方程

1. \\(y^{(n)}=f(x)\enspace(n\geqslant2)\\)
   (解法较简单, 见例题)
   例: \\(y^{\prime\prime}=x^3+e^{2x}\\)
   解:
   \\(y^\prime=\frac{1}{4}x^4+\frac{1}{2}e^{2x}+C_1\\)
   \\(y=\frac{1}{20}x^5+\frac{1}{4}e^{2x}+C_1x+C_2\\)
2. \\(f(x,y^\prime,y^{\prime\prime})=0\\) (缺失 \\(y\\))
   解法:
   令 \\(y^\prime=p\\) , \\(y^{\prime\prime}=\frac{\mathrm{d}p}{\mathrm{d}x}\\) 代入得
   \\(f(x,p,\frac{\mathrm{d}p}{\mathrm{d}x})=0\rArr p=\varphi(x,C_1)\\) , 即 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=\varphi(x,C_1)\\)
   \\(\therefore y=\int\varphi(x, C_1)\mathrm{d}x+C_2\\)
   
   例: 求 \\(xy^{\prime\prime}+2y^\prime=0\\) 通解
   解: 令 \\(y^\prime=p\\) , \\(y^{\prime\prime}=\frac{\mathrm{d}p}{\mathrm{d}x}\\) , 代入得
   \\(x\frac{\mathrm{d}p}{\mathrm{d}x}+2p=0\rArr\frac{\mathrm{d}p}{p}=-\frac{2\mathrm{d}x}{x}\\)
   \\(\rArr\int\frac{1}{p}\mathrm{d}p=-2\int\frac{1}{x}\mathrm{d}x+C_0\\)
   \\(\rArr\ln|p|=-2\ln|x|+C_0\\)
   \\(\rArr|p|=e^{-2\ln|x|+C_0}=\frac{e^{C_0}}{x^2}\\)
   \\(\rArr p=\pm\frac{e^{C_0}}{x^2}=\frac{C_1}{x^2}\\) , 即 \\(y^\prime=\frac{C_1}{x^2}\\)
   \\(\therefore y=-\frac{C_1}{x}+C_2\\)
3. \\(f(y,y^\prime,y^{\prime\prime})=0\\) (缺失 \\(x\\))
   解法:
   令 \\(y^\prime=p\\) , \\(y^{\prime\prime}=\frac{\mathrm{d}p}{\mathrm{d}x}\rArr y^{\prime\prime}=\frac{\mathrm{d}p}{\mathrm{d}y}\cdot\frac{\mathrm{d}y}{\mathrm{d}x}=\frac{\mathrm{d}p}{\mathrm{d}y}\cdot p\\) , 代入得 
   \\(f(y,p,\frac{\mathrm{d}p}{\mathrm{d}y}\cdot p)=0\rArr p=\varphi(y,C_1)\\)
   即 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=\varphi(y,C_1)\rArr\frac{\mathrm{d}y}{\varphi(y,C_1)}=\mathrm{d}x\rArr\int\frac{\mathrm{d}y}{\varphi(y,C_1)}=\int\mathrm{d}x+C_2\\)

   例: 求 \\(yy^{\prime\prime}-y^{\prime 2}=0\\) 满足初始条件 \\(y(0)=1\\) , \\(y^\prime(0)=1\\) 的特解
   解: 令 \\(y^\prime=p\\) , \\(y^{\prime\prime}=p\frac{\mathrm{d}p}{\mathrm{d}y}\\) 代入得
   \\(yp\frac{\mathrm{d}p}{\mathrm{d}y}-p^2=0\\)
   \\(\because p\neq 0\\)
   \\(\therefore y\frac{\mathrm{d}p}{\mathrm{d}y}-p=0\rArr\dots\rArr p=C_1y\\)
   即 \\(\frac{\mathrm{d}y}{\mathrm{d}x}=C_1y\rArr\frac{\mathrm{d}y}{C_1y}=\mathrm{d}x\rArr\int\frac{\mathrm{d}y}{C_1y}=\int\mathrm{d}x+C_2\rArr\ln|C_1y|=x+C_2\rArr y=\frac{C_3e^x}{C_1}\\)
   \\(\because y(0)=1,y^\prime(0)=1\\)
   \\(\therefore C_1=1,C_3=1\\)
   特解为 \\(y=e^x\\)

### 高阶线性微分方程

1. 基本概念
   1. 二阶齐次线性微分方程: \\(y^{\prime\prime}+a(x)y^\prime+b(x)y=0\\)
      二阶非齐线性微分方程: \\(y^{\prime\prime}+a(x)y^\prime+b(x)y=c(x)\\)
   2. \\(n\\) 阶齐次线性微分方程: \\(y^{(n)}+a_1(x)y^{(n-1)}+\dots+a_{n-1}y^\prime+a_n(x)y=0\\)
      \\(n\\) 阶非齐线性微分方程: \\(y^{(n)}+a_1(x)y^{(n-1)}+\dots+a_{n-1}y^\prime+a_n(x)y=f(x)\\)
   3. 设 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 为两个函数
      若 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 不成比例, 称 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 线性无关
      若 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 成比例, 称 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 线性相关
      
      如 \\(x^2\\) 和 \\(\sin x\\) 线性无关, \\(x^2\\) 和 \\(3x^2\\) 线性相关
2. 性质
   设:
   (1) \\(y^{\prime\prime}+a(x)y^\prime+b(x)y=0\\)
   (2) \\(y^{\prime\prime}+a(x)y^\prime+b(x)y=c(x)\\)
   (联立方程式代入可证)
   1. 若 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 为 (1) 的解, 则 \\(y=C_1\varphi_1(x)+C_2\varphi_2(x)\\) 仍为 (1) 的解(线性组合)
   2. 若 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 分别为 (1), (2) 的解, 则 \\(y=\varphi_1(x)+\varphi_2(x)\\) 为 (2) 的解
   3. 若 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 为 (2) 的解, 则 \\(y=\varphi_2(x)-\varphi_1(x)\\) 为 (1) 的解
   4. 1. 设 \\(\varphi_1(x) , \varphi_2(x)\\) 为 (1) 的两个线性无关解
         则 (1) 通解为 \\(y=C_1\varphi_1(x)+C_2\varphi_2(x)\\)
      1. 设 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 为 (1) 的两个线性无关解, \\(\varphi_0(x)\\) 为 (2) 的一个特解
         则 (2) 的通解为 \\(y=C_1\varphi_1(x)+C_2\varphi_2(x)+\varphi_0(x)\\)
   5. (3) \\(y^{\prime\prime}+a(x)y^\prime+b(x)y=f(x)\\)
      若 \\(f(x)=f_1(x)+f_2(x)\\) 则
      (3)' \\(\enspace y^{\prime\prime}+a(x)y^\prime+b(x)y=f_1(x)\\)
      (3)'' \\(\enspace y^{\prime\prime}+a(x)y^\prime+b(x)y=f_2(x)\\)
      若 \\(\varphi_1(x)\\) , \\(\varphi_2(x)\\) 为 (3)', (3)'' 的解, 则 \\(y=\varphi_1(x)+\varphi_2(x)\\) 为 (3) 的解

   * 证明:
     1. 第三条的证明:
        \\(\begin{cases} \varphi_1^{\prime\prime}+a(x)\varphi_1^\prime+b(x)\varphi_1=c(x) \\\ \varphi_2^{\prime\prime}+a(x)\varphi_2^\prime+b(x)\varphi_2=c(x) \end{cases}\\)
        将 \\(y=\varphi_2(x)-\varphi_1(x)\\) 代入 (2) 得
        \\((\varphi_2-\varphi_1)^{\prime\prime}+a(x)(\varphi_2-\varphi_1)^\prime+b(x)(\varphi_2-\varphi_1)\\)
        \\(=(\varphi_2^{\prime\prime}+a(x)\varphi_2^\prime+b(x)\varphi_2)-(\varphi_1^{\prime\prime}+a(x)\varphi_1^\prime+b(x)\varphi_1)\\)
        \\(=c(x)-c(x)=0\\)
        \\(\therefore y=\varphi_2(x)-\varphi_1(x)\\) 为 (1) 的解
     2. 第五条的证明:
        \\(\begin{cases} \varphi_1^{\prime\prime}+a(x)\varphi_1^\prime+b(x)\varphi_1=f_1(x) \\\ \varphi_2^{\prime\prime}+a(x)\varphi_2^\prime+b(x)\varphi_2=f_2(x) \end{cases}\\)
        将 \\(y=\varphi_1(x)+\varphi_2(x)\\) 代入 (3) 得
        \\((\varphi_1+\varphi_2)^{\prime\prime}+a(x)(\varphi_1+\varphi_2)^\prime+b(x)(\varphi_1+\varphi_2)=f(x)\\)
        \\(\hArr(\varphi_1^{\prime\prime}+a(x)\varphi_1^\prime+b(x)\varphi_1)+(\varphi_1^{\prime\prime}+a(x)\varphi_2^\prime+b(x)\varphi_2)=f(x)\\)
        \\(\hArr f_1(x)+f_2(x)=f(x)\\)
        \\(\therefore y=\varphi_1(x)+\varphi_2(x)\\) 为 (3) 的解

### 常系数齐次线性微分方程

1. 二阶常系数齐次线性微分方程: \\(y^{\prime\prime}+py^\prime+qy=0\\) , 特征方程: \\(\lambda^2+p\lambda+q=0\\)
   1. \\(\Delta>0\\) , \\(y=C_1e^{\lambda_1x}+C_2e^{\lambda_2x}\\)
   2. \\(\Delta=0\\) , \\(y=(C_1+C_2x)e^{\lambda_1x}\\)
   3. \\(\Delta<0\\) , \\(y=e^{\alpha x}(C_1\cos\beta x+C_2\sin\beta x)\\)
2. 解释:
   设二阶常系数齐次线性微分方程 \\(y^{\prime}+py^\prime+qy=0\enspace\\) (\\(p\\)、\\(q\\) 为常数) (\*)
   猜测: (\*) 解的形式? \\(\begin{cases} e^{\lambda x} \\\ \sin(\beta x)\cdot\cos(\beta x) \end{cases}\\)
   令 \\(y=e^{\lambda x}\\) 为 (\*) 的解, 代入得
   \\(\lambda^2e^{\lambda x}+p\lambda e^{\lambda x}+qe^{\lambda x}=0\rArr\lambda^2+p\lambda+q=0\\) , 称其为 (\*) 的特征方程
   * 情况1 \\(\Delta=p^2-4q>0\\)
     则 \\(\lambda^2+p\lambda+q=0\\) 有两个不同实根 \\(\lambda_1\\)、\\(\lambda_2\\)
     因此 \\(y_1=e^{\lambda_1x}\\) , \\(y_2=e^{\lambda_2x}\\) 为 (\*) 的解
     \\(\because\lambda_1\neq\lambda_2\\) , \\(\therefore y_1\\) 与 \\(y_2\\) 线性无关
     \\(\therefore\\) (\*) 的通解为 \\(y=C_1e^{\lambda_1x}+C_2e^{\lambda_2x}\\) (高阶线性线性微分方程性质1)
   * 情况2 \\(\Delta=p^2-4q=0\\)
     则 \\(\lambda^2+p\lambda+q=0\\) 有两个相等的实根 \\(\lambda_1=\lambda_2\\)
     \\(y_1=e^{\lambda_1x}\\) 为 (\*) 的解
     令 \\(\frac{y_2}{y_1}=u(x)(\neq C)\\) 且 \\(y_2\\) 为 (\*) 的解, 得到
     \\(y_2=ue^{\lambda_1x}\\)
     \\(y_2^\prime=u^\prime e^{\lambda_1x}+\lambda_1ue^{\lambda_1x}\\)
     \\(y_2^{\prime\prime}=u^{\prime\prime}e^{\lambda_1x}+2\lambda_1u^\prime e^{\lambda_1x}+\lambda_1^2ue^{\lambda_1x}\\)
     将以上三式代入 \\(y^{\prime\prime}+py^\prime+qy=0\\) 得
     \\(u^{\prime\prime}e^{\lambda_1x}+2\lambda_1u^\prime e^{\lambda_1x}+\lambda_1^2ue^{\lambda_1x}+pu^\prime e^{\lambda_1x}+p\lambda_1ue^{\lambda_1x}+que^{\lambda_1x}=0\\)
     \\(\rArr u^{\prime\prime}+2\lambda_1u^\prime+\lambda_1^2u+pu^\prime+p\lambda_1u+qu=0\\)
     \\(\rArr u^{\prime\prime}+(2\lambda_1+p)u^\prime+(\lambda_1^2+p\lambda_1+q)u=0\\)
     \\(\because \begin{cases} \lambda_1^2+p\lambda_1+q=0 \\\ \lambda_1+\lambda_2=-p \rArr 2\lambda_1+p=0\enspace(\text{韦达定理}) \end{cases}\\)
      \\(\therefore u^{\prime\prime}=0\\) , 取 \\(u(x)=C_1x+C_2\\)
      \\(\therefore y_2=(C_1x+C_2)e^{\lambda_1x}=C_1xe^{\lambda_1x}+C_2e^{\lambda_1x}\\)
      则通解 \\(y=C_0e^{\lambda_1x}+C_1xe^{\lambda_1x}+C_2e^{\lambda_1x}=(C_0+C_2)e^{\lambda_1x}+C_1xe^{\lambda_1x}=(C_3+C_1x)e^{\lambda_1x}\\)
   * 情况3 \\(\Delta=p^2-4q<0\\)
     则 \\(\lambda^2+p\lambda+q=0\\) 有两个虚数解 \\(\lambda_{1,2}=\alpha\pm i\beta\\)
     \\(y_1=e^{(\alpha+\beta i)x}\\) 与 \\(y_2=e^{(\alpha-\beta i)x}\\) 为 (\*) 的解
     \\(y_1=e^{\alpha x+\beta xi}=e^{\alpha x}\cdot e^{\beta xi}=e^{\alpha x}\cdot[\cos(\beta x)+i\sin(\beta x)]\enspace\\) (欧拉公式 \\(e^{i\theta}=\cos\theta+i\sin\theta\\))
     \\(y_2=e^{\alpha x-\beta xi}=e^{\alpha x}\cdot e^{-\beta xi}=e^{\alpha x}\cdot[\cos(\beta x)-i\sin(\beta x)]\\)
     \\(Y_1=\frac{1}{2}y_1+\frac{1}{2}y_2=e^{\alpha x}\cos(\beta x)\\) (高阶线性线性微分方程性质1)
     \\(Y_2=\frac{1}{2i}y_1+(-\frac{1}{2i})y_2=\frac{1}{2i}(y_1-y_2)=e^{\alpha x}\sin(\beta x)\\)
     \\(\therefore\\) (\*) 的通解为 \\(y=C_1e^{\alpha x}\cos(\beta x)+C_2e^{\alpha x}\sin(\beta x)=e^{\alpha x}[C_1\cos(\beta x)+C_2\sin(\beta x)]\\)
3. 高阶常系数齐次线性微分方程
例 \\(y^{\prime\prime\prime}+py^{\prime\prime}+qy^\prime+\Gamma y=0\\) , 它的特征方程 \\(\lambda^3+p\lambda^2+q\lambda+\Gamma=0\\)
   1. \\(\lambda_1\\)、\\(\lambda_2\\)、\\(\lambda_3\\) 为实数且各不相等
      \\(y=C_1e^{\lambda_1x}+C_2e^{\lambda_2x}+C_3e^{\lambda_3x}\\)
   2. \\(\lambda_1\\)、\\(\lambda_2\\)、\\(\lambda_3\\) 为实数且 \\(\lambda_1=\lambda_2\neq\lambda_3\\)
      \\(y=(C_1+C_2x)e^{\lambda_1x}+C_3e^{\lambda_3x}\\)
   3. \\(\lambda_1\\)、\\(\lambda_2\\)、\\(\lambda_3\\) 为实数且相等
      \\(y=(C_1+C_2x+C_3x^2)e^{\lambda_1x}\\)
   4. \\(\lambda_1\in R\\) , \\(\lambda_{2,3}=\alpha\pm i\beta\\)
      \\(y=C_1e^{\lambda_1x}+e^{\alpha x}[C_2\cos(\beta x)+C_3\sin(\beta x)]\\)

### 常系数非齐次线性微分方程


1. 通解思路:
   \\(y^{\prime\prime}+py^\prime+qy=f(x)\\)
   * 先求 \\(y^{\prime\prime}+py^\prime+qy=0\\) 通解
   * 找 \\(y^{\prime\prime}+py^\prime+qy=f(x)\\) 的一个特解 \\(y_0(x)\\) (很困难, 接下来只谈两种情况)
   
   则有通解 \\(y=C_1e^{-x}+C_2e^{2x}+y_0(x)\\) (高阶线性微分方程性质5)
2. \\(f(x)=P_n(x)e^{kx}\\)
   若 \\(n=|\\{\lambda|k=\lambda_1=\dots\lambda_n\\}|\\)
   令特解为 \\(y_0=x^n(ax+b)e^{kx}\\)
   * 例1: 求 \\(y^{\prime\prime}-y^\prime-2y=(2x+1)e^x\\) 通解
     解:
     1. 特征方程: \\(\lambda^2-\lambda-2=0\rArr\lambda_1=-1\\) , \\(\lambda_2=2\\)
        \\(y^{\prime\prime}-y^\prime-2y=0\\) 通解为 \\(y=C_1e^{-x}+C_2e^{2x}\\)
     2. 令特解 \\(y_0(x)=(ax+b)e^xx\\) 代入得 \\(a=-1\\) , \\(b=-1\\)
        即 \\(y_0(x)=-(x-1)e^x\\)
      
     \\(\therefore\\) 原方程通解为 \\(y=C_1e^{-x}+C_2e^{2x}-(x-1)e^x\\)
   * 例2: 求 \\(y^{\prime\prime}-3y^\prime+2y=(2x+3)e^x\\) 通解
     解:
     1. 特征方程: \\(\lambda^2-3\lambda+2=0\rArr\lambda_1=1\\) , \\(\lambda_2=2\\)
        \\(y^{\prime\prime}-3y^\prime+2y=0\\) 通解为 \\(y=C_1e^{x}+C_2e^{2x}\\)
     2. 令特解 \\(y_0(x)=x(ax+b)e^x=(ax^2+bx)e^x\\) 代入得 \\(a=-1\\) , \\(b=-5\\)
        即 \\(y_0(x)=-(x^2+5x)e^x\\)

     \\(\therefore\\) 原方程通解为 \\(y=C_1e^x+C_2e^{2x}-(x^2+5x)e^x\\)
3. \\(f(x)=e^{\alpha x}\\) [\\(\text{多项式}\cdot\cos(\beta x)+\text{多项式}\cdot\sin(\beta x)\\)]
   若 \\(n=|\\{\lambda|(\alpha+i\beta)=\lambda_1=\dots\lambda_n\\}|\\)
   特解为 \\(y_0(x)=x^ne^{\alpha x}[a\cdot\cos(\beta x)+b\cdot\sin(\beta x)]\enspace\\) (\\(a\\)、\\(b\\) 根据多项式内容决定)
   * 例3: \\(y^{\prime\prime}-2y^\prime+2y=(x+1)e^x\cos x\\)
     解:
     1. 特征方程: \\(\lambda^2-2\lambda+2=0\rArr\lambda_{1,2}=1\pm i\\)
        \\(y^{\prime\prime}-2y^\prime+2y=0\rArr y=e^x(\cos x+\sin x)\\)
     2. \\(\alpha=1\\) , \\(\beta=1\rArr\alpha+i\beta=1+i\\) 有一个 \\(\lambda\\) 值与其相等
        因此令特解 \\(y_0(x)=xe^x[(ax+b)\cos x+(cx+d)\sin x]\\)
        \\(\dots\\) (接下来解出 \\(a,b,c,d\\) , 最后得出通解)