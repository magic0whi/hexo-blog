---
title: Introduction to Linear Algebra
toc: true
categories:
- learn
- mathematics
lang: zh-cn
date: 2021-02-24 11:53:14
tags: mathematics
---

<style>
.content {
	font-family: KaTeX_Main, 'FZYaSongS-R-GB';
}
article.article .content {
	font-size: 1.3em;
}
.katex .base {
    font-size: 0.8em;
}
/* .katex-display {
    overflow-x: hidden;
} */
.content ol.worked-examples {
    list-style-type: none;
    margin-left: 0em;
}
.content ol.worked-examples > li {
    margin-bottom: 1.5em;
}
.content ol.worked-examples > li > span.list-num {
    font-family: Ubuntu, Roboto, 'Open Sans';
    font-weight: bold;
}
.content details summary span.list-summary {
    font-family: Ubuntu, Roboto, 'Open Sans';
    font-weight: bold;
    color: RoyalBlue;
}
</style>


This is a copy of both brief summary and worked examples of each section in the Introduction to Linear Algebra (Fifth Edition) by Gilbert Strang

https://github.com/mitmath/1806/

TODO: draw svg format figure by using tikz

<!-- more -->

## Chapter 1 Introduction to Vectors

### Vectors and Linear Combinations

1. \\(3\bm{v}+5\bm{w}\\) is a typical **linear combination** \\(c\bm{v}+d\bm{w}\\) of the vectors \\(\bm{v}\\) and \\(\bm{w}\\)
2. For \\(\bm{v}=\begin{bmatrix} 1 \\\ 1 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix} 2 \\\ 3 \end{bmatrix}\\)
3. The vector \\(\begin{bmatrix} 2 \\\ 3 \end{bmatrix}=\begin{bmatrix} 2 \\\ 0 \end{bmatrix}+\begin{bmatrix} 0 \\\ 3 \end{bmatrix}\\) goes across to \\(x=2\\) and up to \\(y=3\\) in the \\(xy\\) plane.
4. The combinations \\(c\begin{bmatrix} 1 \\\ 1 \end{bmatrix}+d\begin{bmatrix} 2 \\\ 3 \end{bmatrix}\\) fill the whole \\(xy\\) place. They produce every \\(\begin{bmatrix} x \\\ y \end{bmatrix}\\)
5. The combinations \\(c\begin{bmatrix} 1 \\\ 1 \\\ 1 \end{bmatrix}+d\begin{bmatrix} 2 \\\ 3 \\\ 4 \end{bmatrix}\\) fill a **plane** in \\(xyz\\) space. Same plane for \\(\begin{bmatrix} 1 \\\ 1 \\\ 1 \end{bmatrix}\\), \\(\begin{bmatrix} 3 \\\ 4 \\\ 5 \end{bmatrix}\\).
6. But \\(\begin{array}{l} c+2d=1 \\\ c+3d=0 \\\ c+4d=0 \end{array}\\) has no solution because its right side \\(\begin{bmatrix} 1 \\\ 0 \\\ 0 \end{bmatrix}\\) is not on that plane.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">1.1 A</span>&emsp;The linear combinations of \(\bm{v}=(1,1,0)\) and \(\bm{w}=(0,1,1)\) fill a plane in \(\mathbf{R}^3\). <em>Describe that plane</em>. Find a vector that is <em>not</em> a combination of \(\bm{v}\) and \(\bm{w}\)&mdash;not on the plane.
      <br><br>
      <span class="list-num">Solution</span>&emsp;The plane of \(\bm{v}\) and \(\bm{w}\) contains all combinations \(c\bm{v}+d\bm{w}\). The vectors in that plane allow any \(c\) and \(d\). The plane of Figure 1.3 fills in between the two lines.
      <details>
        <summary>&dagger; Figure 1.3 &dagger;</summary>
        {% asset_img Figure1.3.png %}
      </details>
      <br>
      $$
      \text{Combinations}\qquad c\bm{v}+d\bm{w}=c\begin{bmatrix} 1 \\ 1 \\ 0 \end{bmatrix}+d\begin{bmatrix} 0 \\ 1 \\ 1 \end{bmatrix}=\begin{bmatrix} c \\ c+d \\ d \end{bmatrix}\text{fill a plane.}
      $$
      Four vectors in that plane are \((0,0,0)\) and \((2,3,1)\) and \((5,7,2)\) and \((\pi,2\pi,\pi)\). The second component \(c+d\) is always the sum of the first and third components. Like most vectors, \((1,2,3)\) <em>is <strong>not</strong> in the plane, because</em> \(2\neq 1+3\).
      <br>
      &emsp;&ensp;Another description of this plane through \((0,0,0)\) is to know that \(\bm{n}=(1,-1,1)\) is <strong>perpendicular</strong> to the plane. Section 1.2 will confirm that \(90\degree\) angle by testing dot products: \(\bm{v}\cdot\bm{m}=0\) and \(\bm{w}\cdot\bm{n}=0\). Perpendicular vectors have zero dot products.
    </li>
    <li>
      <span class="list-num">1.1 B</span>&emsp;For \(\bm{v}=(1,0)\) and \(\bm{w}=(0,1)\), describe all points \(c\bm{v}\) with (<b>1</b>) <em>whole numbers \(c\)</em> (<b>2</b>) <em>nonnegative numbers \(c\geq 0\)</em>. Then add all vectors \(d\bm{w}\) and describe all \(c\bm{v}+d\bm{w}\)
      <br><br>
      <span class="list-num">Solution</span>
      <br>
      &ensp;(<b>1</b>)&ensp;The vectors \(c\bm{v}=(c,0)\) with whole numbers \(c\) are <strong>equally spaced points</strong> along the \(x\) axis (the direction of \(\bm{v}\)). They include \((-2,0)\), \((-1,0)\), \((0,0)\), \((1,0)\), \((2,0)\).
      <br>
      &ensp;(<b>2</b>)&ensp;The vectors \(c\bm{v}\) with \(c\geq 0\) fill a <strong>half-line</strong>. It is the positive \(x\) axis. This half-line starts at \((0,0)\) where \(c=0\). It includes \((100,0)\) and \((\pi,0)\) but not \((-100,0)\).
      <br>
      &ensp;(<b>1&#x2B9;</b>)&ensp;Adding all vectors \(d\bm{w}=(0,d)\) puts a vertical line through those equally spaced \(c\bm{v}\). We have infinitely many <em><strong>parallel lines</strong></em> from (<em>whole number \(c\), any number \(d\)</em>).
      <br>
      &ensp;(<b>2&#x2B9;</b>)&ensp;Adding all vectors \(d\bm{w}\) puts a vertical line through every \(c\bm{v}\) on the half-line. Now we have <em><strong>half-plane</strong></em>. The right half of the \(xy\) plane has any \(x\geq 0\) and any \(y\).
    </li>
    <li>
      <span class="list-num">1.1 C</span>&emsp;Find two equations for \(c\) and \(d\) so that <strong>the linear combination \(c\bm{v}+d\bm{w}\) equals \(\bm{b}\)</strong>:
      $$
      \bm{v}=\begin{bmatrix} 2 \\ -1 \end{bmatrix}\qquad\bm{w}=\begin{bmatrix} -1 \\ 2 \end{bmatrix}\qquad\bm{b}=\begin{bmatrix} 1 \\ 0 \end{bmatrix}
      $$
      <span class="list-num">Solution</span>&emsp;In applying mathematics, many problems have two parts:
      <br>
      &emsp;<b>1</b>&ensp;<em>Modeling part</em>&emsp;Express the problem by a set of equations.
      <br>
      &emsp;<b>2</b>&ensp;<em>Computational part</em>&emsp;Solve those equations by a fast and accurate algorithm.
      <br>
      Here we are only asked for the first part (the equations). Chapter 2 is devoted to the second part (the solution). Our example fits into fundamental model for linear algebra:
      $$
      \text{Find $n$ numbers }c_1,\ldots,c_n\text{ so that }c_1\bm{v}_1+\cdots+c_n\bm{v}_n=\bm{b}
      $$
      For \(n=2\) we will find a formula for the \(c\)'s. The &ldquo;elimination method&rdquo; in Chapter 2 succeeds far beyond \(n=1000\). For \(n\) greater than 1 billion, see Chapter 11. Here \(n=2\):
      $$
      \colorbox{e8f1fe}{$\begin{array}{l} \textbf{Vector equation} \\ c\bm{v}+d\bm{w}=\bm{b} \end{array}\qquad c\begin{bmatrix*}[r] 2 \\ -1 \end{bmatrix*}+d\begin{bmatrix*}[r] -1 \\ 2 \end{bmatrix*}=\begin{bmatrix} 1 \\ 0 \end{bmatrix}$}
      $$
      The required equations for \(c\) and \(d\) just come from the two components separately:
      $$
      \textcolor{RoyalBlue}{\textbf{Two ordinary equations}}
      \qquad
      \begin{alignedat}{2.5} % Using alignedat for no space between columns. For parameter there is +0.5 per column (pair of rl columns), columms align are col1=right, col2=left, col3=right...
        && 2c- && d=1 \\
        - && c+ && 2d=0
      \end{alignedat}
      $$
      Each equation produces a line. The two lines cross at the solution \(c=\dfrac{2}{3}\), \(d=\dfrac{1}{3}\). Why not see this also as a <strong>matrix equation</strong>, since that is where we are going:
      $$
      \textbf{2 by 2 matrix}
      \qquad
      \begin{bmatrix*}[r] \bm{2} & \bm{-1} \\ \bm{-1} & \bm{2} \end{bmatrix*}
      \begin{bmatrix} c \\ d \end{bmatrix}=\begin{bmatrix} 1 \\ 0 \end{bmatrix}
      $$
    </li>
  </ol>
</details>

### Lengths and Dot Products

1. The "dot product" of \\(\bm{v}=\begin{bmatrix} 1 \\\ 2 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix} 4 \\\ 5 \end{bmatrix}\\) is \\(\bm{v}\cdot\bm{w}=(1)(4)+(2)(5)=4+10=\bm{14}\\)
2. \\(\bm{v}=\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix*}[r] 4 \\\ -4 \\\ 4 \end{bmatrix*}\\) are perpendicular because \\(\bm{v}\cdot\bm{w}\\) is zero: \\((1)(4)+(3)(-4)+(2)(4)=\bm{0}\\)
3. The length squared of \\(\bm{v}=\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) is \\(\bm{v}\cdot\bm{v}=1+9+4=14\\). **The length is** \\(\\|\bm{v}\\|=\sqrt{\bm{14}}\\).
4. Then \\(\bm{u}=\dfrac{\bm{v}}{\\|\bm{v}\\|}=\dfrac{\bm{v}}{\sqrt{14}}=\dfrac{1}{\sqrt{14}}\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) has length \\(\\|\bm{u}\\|=\bm{1}\\). Check \\(\dfrac{1}{\underline{14}}+\dfrac{9}{\underline{14}}+\dfrac{4}{\underline{14}}=1\\).
5. The angle \\(\theta\\) between \\(\bm{v}\\) and \\(\bm{w}\\) has \\(\cos\theta=\dfrac{\bm{v}\cdot\bm{w}}{\\|\bm{v}\\|\\|\bm{w}\\|}\\)
6. The angle between \\(\begin{bmatrix} 1 \\\ 0 \end{bmatrix}\\) and \\(\begin{bmatrix} 1 \\\ 1\end{bmatrix}\\) has \\(\cos\theta=\dfrac{1}{(1)(\sqrt{2})}\\). That angle is \\(\theta=45\degree\\)
7. All angles have \\(|\cos\theta|\leq 1\\). So all vectors have \\(\boxed{|\bm{v}\cdot\bm{w}|\leq\\|\bm{v}\\|\\|\bm{w}\\|}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">1.2 A</span>&emsp;For the vectors \(\bm{v}=(3,4)\) and \(\bm{w}=(4,3)\) test the Schwarz inqeuality on \(\bm{v}\cdot\bm{w}\) and the triangle inqeuality on \(\|\bm{v}+\bm{w}\|\). Find \(\cos\theta\) for the angle between \(\bm{v}\) and \(\bm{w}\). Which \(\bm{v}\) and \(\bm{w}\) give <em>equality</em> \(\|\bm{v}\cdot\bm{w}\|=\|\bm{v}\|\|\bm{w}\|\) and \(\|\bm{v}+\bm{w}\|=\|\bm{w}\|+\|\bm{w}\|\)?
      <br><br>
      <span class="list-num">Solution</span>&emsp;The dot product is \(\bm{v}\cdot\bm{w}=(3)(4)+(4)(3)=24\). The length of \(\bm{v}\) is \(\|\bm{v}\|=\sqrt{9+16}=5\) and also \(\|\bm{w}\|=5\). The sum \(\bm{v}+\bm{w}=(7,7)\) has length \(7\sqrt{2}<10\).
      $$
      \begin{array}{ll}
        \textbf{Schwarz inequality} & |\bm{v}\cdot\bm{w}|\leq\|\bm{v}\|\|\bm{w}\|\quad\text{is}\quad 24<25 \\
        \textbf{Triangle inequality} & \|\bm{v}+\bm{w}\|\leq\|\bm{v}\|+\|\bm{w}\|\quad\text{is}\quad 7\sqrt{2}<5+5 \\
        \textbf{Cosine of angle} & \cos\theta=\frac{24}{25}\quad\text{Thin angle from }\bm{v}=(3,4)\text{ to }\bm{w}=(4,3)
      \end{array}
      $$
      <em>Equality</em>: One vector is a multiple of the other as in \(\bm{w}=c\bm{v}\). Then the angle is \(0\degree\) or \(180\degree\). In this case \(|\cos\theta|=1\) and \(|\bm{v}\cdot\bm{w}|\) <em>equals</em> \(\|\bm{v}\|\|\bm{w}\|\). If the angle is \(0\degree\), as in \(\bm{w}=2\bm{v}\), then \(\|\bm{v}+\bm{w}\|=\|\bm{v}\|+\|\bm{w}\|\) (both sides give \(3\|\bm{v}\|\)). This \(\bm{v}\), \(2\bm{v}\), \(3\bm{v}\) triangle is flat!
    </li>
    <li>
      <span class="list-num">1.2 B</span>&emsp;Find a unit vector \(\bm{u}\) in the direction of \(\bm{v}=(3,4)\). Find a unit vector \(\bm{U}\) that is perpendicular to \(\bm{u}\). How many possibilities for \(\bm{U}\)?
      <br><br>
      <span class="list-num">Solution</span>&emsp;For a unit vector \(\bm{u}\), divide \(\bm{v}\) by its length \(\|\bm{v}\|=5\). For a perpendicular vector \(\bm{V}\) we can choose \((-4,3)\) since the dot product \(\bm{v}\cdot\bm{V}\) is \((3)(-4)+(4)(3)=0\). For a <em>unit</em> vector perpendicular to \(\bm{u}\), divide \(\bm{V}\) by its length \(\|\bm{V}\|\):
      $$
        \bm{u}=\frac{\bm{v}}{\|\bm{v}\|}=\left(\frac{3}{5},\frac{4}{5}\right)
        \qquad
        \bm{U}=\frac{\bm{V}}{\|\bm{V}\|}=\left(-\frac{4}{5},\frac{3}{5}\right)
        \qquad
        \bm{u}\cdot\bm{U}=0
      $$
      The only other perpendicular unit vector would be \(-\bm{U}=(\frac{4}{5},-\frac{3}{5})\)
    </li>
    <li>
      <span class="list-num">1.2 C</span>&emsp;Find a vector \(\bm{x}=(c,d)\) that has dot products \(\bm{x}\cdot\bm{r}=1\) and \(\bm{x}\cdot\bm{s}=0\) with two given vectors \(\bm{r}=(2,-1)\) and \(\bm{s}=(-1,2)\).
      <br><br>
      <span class="list-num">Solution</span>&emsp;Those two dot products give linear equations for \(c\) and \(d\). Then \(\bm{x}=(c,d)\).
      $$
      \begin{alignedat}{3}
        \bm{x}\cdot\bm{r}=1\qquad\text{is}\qquad &  & 2c- && d=1\qquad & \textbf{The same equations as}  \\
        \bm{x}\cdot\bm{s}=0\qquad\text{is}\qquad & - & c+ && 2d=0\qquad & \textbf{in Worked Example 1.1 C}
      \end{alignedat}
      $$
      &emsp;&ensp;<em>Comment on n equations for \(x=(x_1,\ldots,x_n)\) in n-dimensional space</em>
      <br>
      Section 1.1 would start with columns \(\bm{v}_j\). The goal is to produce \(x_1\bm{v}_1+\cdots+x_n\bm{v}_n=\bm{b}\).
      <br>
      This section would start from rows \(\bm{r}_i\). Now the goal is to find \(\bm{x}\) with \(\bm{x}\cdot\bm{r}_i=b_i\).
      <br>
      &emsp;&ensp;Soon the \(\bm{v}\)'s will be columns of a matrix \(A\), and the \(\bm{r}\)'s will be the rows of \(A\). Then the (one and only) problem will be to solve \(A\bm{x}=\bm{b}\).
    </li>
  </ol>
</details>

### Matrices

1. \\(A=\begin{bmatrix} 1 & 2 \\\ 3 & 4 \\\ 5 & 6 \end{bmatrix}\\) is a 3 by 2 matrix : \\(m=3\\) rows and \\(n=2\\) columns.
2. \\(A\bm{x}=\begin{bmatrix} 1 & 2 \\\ 3 & 4 \\\ 5 & 6 \end{bmatrix}\begin{bmatrix} x_1 \\\ x_2 \end{bmatrix}\\) is **a combination of the columns**&emsp;\\(A\bm{x}=x_1\begin{bmatrix} 1 \\\ 3 \\\ 5 \end{bmatrix}+x_2\begin{bmatrix} 2 \\\ 4 \\\ 6 \end{bmatrix}\\).
3. The 3 components of \\(A\bm{x}\\) are dot products of the 3 rows of \\(A\\) with the vector \\(\bm{x}\\) :
   <div>
   $$
   \textbf{Row at a time}
   \qquad
   \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}\begin{bmatrix} 7 \\ 8 \end{bmatrix}=\begin{bmatrix} 1\cdot 7+2\cdot 8 \\ 3\cdot 7+4\cdot 8 \\ 5\cdot 7+6\cdot 8 \end{bmatrix}=\begin{bmatrix} 23 \\ 53 \\ 83 \end{bmatrix}\text{.}
   $$
   </div>
4. Equations in matrix form \\(A\bm{x}=\bm{b}\\) : \\(\begin{bmatrix} 2 & 5 \\\ 3 & 7 \end{bmatrix}\begin{bmatrix} x_1 \\\ x_2 \end{bmatrix}=\begin{bmatrix} b_1 \\\ b_2 \end{bmatrix}\\) replaces \\(\begin{array}{l} 2x_1+5x_2=b_1 \\\ 3x_1+7x_2=b_2 \end{array}\\).
5. The solution to \\(A\bm{x}=\bm{b}\\) can be written as \\(\bm{x}=A^{-1}\bm{b}\\). But some matrices don't allow \\(A^{-1}\\)

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">1.3 A</span>&emsp;Change the southwest entry \(a_{31}\) of \(A\) (row 3, column 1) to \(a_{31}=1\):
      $$
      A\bm{x}=\bm{b}\qquad\begin{bmatrix*}[r] 1 & 0 & 0 \\ -1 & 1 & 0 \\ \bm{1} & -1 & 1 \end{bmatrix*}\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}=\left[\begin{alignedat}{1} & x_1 \\ - & x_1+x_2 \\  & \bm{x_1}-x_2+x_3 \end{alignedat}\right]=\begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}\text{.}
      $$
      <strong>Find the solution \(\bm{x}\) for any \(\bm{b}\). From \(\bm{x}=A^{-1}\bm{b}\) read off the inverse matrix \(A^{-1}\).</strong>
      <br><br>
      <span class="list-num">Solution</span>&emsp;Solve the (Linear triangular) system \(A\bm{x}=\bm{b}\) from top to bottom:
      $$
      \begin{alignedat}{3}
      & \text{first}\quad && x_1=b_1 && \\
      & \text{then}\quad && x_2=b_1+ && b_2 \\
      & \text{then}\quad && x_3= && b_2+b_3
      \end{alignedat}
      \text{This says that }\bm{x}=A^{-1}\bm{b}=\begin{bmatrix} 1 & 0 & 0 \\ 1 & 1 & 0 \\ 0 & 1 & 1 \end{bmatrix}\begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}
      $$
      This is good practice to see the columns of the inverse matrix multiplying \(b_1\), \(b_2\), and \(b_3\). The first column of \(A^{-1}\) is the solution for \(\bm{b}=(1,0,0)\). The second column is the solution for \(\bm{b}=(0,1,0)\). The third column \(\bm{x}\) of \(A^{-1}\) is the solution for \(A\bm{x}=\bm{x}=(0,0,1)\).
      <br>
      &emsp;&ensp;The three columns of \(A\) are still independent. They don't lie in a plane. The combinations of those three columns, using the right weights \(x_1\), \(x_2\), \(x_3\), can produce any three-dimensional vector \(\bm{b}=(b_1,b_2,b_3)\). Those weights come from \(\bm{x}=A^{-1}\bm{b}\).
    </li>
    <li>
      <span class="list-num">1.3 B</span>&emsp;This \(E\) is an <strong>elimination matrix</strong>. \(E\) has a subtraction and \(E^{-1}\) has an addition.
      $$ % Sorry for the type, will reformat as soon as KaTex support multicolumn.
      \bm{b}=E\bm{x}\qquad\begin{bmatrix} b_1 \\ b_2 \end{bmatrix}=\begin{bmatrix} x_1 \\ x_2-\ell x_1 \end{bmatrix}=\begin{bmatrix*}[r] \bm{1} & 0 \\ -\bm{\ell} & \bm{1} \end{bmatrix*}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}\qquad E=\begin{bmatrix*}[r] \bm{1} & 0 \\ -\bm{\ell} & \bm{1} \end{bmatrix*}
      $$
      The first equation is \(x_1=b_1\). The second equation is \(x_2-\ell x_1=b_2\). The inverse will <en>add</em> \(\ell b_1\) to \(b_1\), because the elimination matrix <em>subtracted</em> :
      $$
      \bm{x}=E^{-1}\bm{b}\qquad\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}=\left[\begin{alignedat}{1} & b_1 \\ \ell & b_1+b_2 \end{alignedat}\right]=\begin{bmatrix} \bm{1} & 0 \\ \bm{\ell}  & \bm{1} \end{bmatrix}\begin{bmatrix} b_1 \\ b_2 \end{bmatrix}\qquad E^{-1}=\begin{bmatrix} \bm{1} & 0 \\ \bm{\ell}  & \bm{1} \end{bmatrix}
      $$
    </li>
    <li>
      <span class="list-num">1.3 C</span>&emsp;Change \(C\) from a cyclic difference to a <strong>centered difference</strong> producing \(x_3-x_1\) :
      $$
      \begin{equation}
        C\bm{x}=\bm{b}\qquad\begin{bmatrix*}[r] 0 & 1 & 0 \\ -1 & 0 & 1 \\ 0 & -1 & 0 \end{bmatrix*}\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}=\left[\begin{alignedat}{1} x_2 & -0 \\ x_3 & -x_1 \\ 0 & -x_2 \end{alignedat}\right]=\begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}\text{.}
      \end{equation}
      $$
      \(C\bm{x}=\bm{b}\) can only be solved when \(b_1+b_3=x_2-x_2=0\). That is a plane of vectors \(\bm{b}\) in three -dimensional space. Each column of \(C\) is in the plane, the matrix has no inverse. So this plane conatins all combinations of those columns (which are all the vectors \(C\bm{x}\))
      <br>
      &emsp;&ensp;I included the zeros so you could see that this \(C\) produces &ldquo;centered differences&rdquo;. Row \(i\) of \(C\bm{x}\) is \(x_{i+1}\) (<em>right of center</em>) minus \(x_{i-1}\) (<em>left of center</em>). Here is 4 by 4 :
      $$
      \begin{equation}
        \begin{array}{l} C\bm{x}=\bm{b} \\ \textbf{Centered} \\ \textbf{differences} \end{array}
        \begin{bmatrix*}[r]
         0 & 1 & 0 & 0 \\
         -1 & 0 & 1 & 0 \\
         0 & -1 & 0 & 1 \\
         0 & 0 & -1 & 0
       \end{bmatrix*}
       \begin{bmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{bmatrix}=\left[\begin{alignedat}{1} x_2 & -0 \\ x_3 & -x_1 \\ x_4 & -x_2 \\ 0 & -x_3 \end{alignedat}\right]=\begin{bmatrix} b_1 \\ b_2 \\ b_3 \\ b_4 \end{bmatrix}
      \end{equation}
      $$
      Surprisingly this matrix is now invertible! The first and last rows tell you \(x_2\) and \(x_3\). Then the middle rows give \(x_1\) and \(x_4\). It is possible to write down the inverse matrix \(C^{-1}\). But 5 by 5 will be singular (<em>not invertible</em>) again...
    </li>
  </ol>
</details>

## Chapter 2 Solving Linear Equations

### Vectors and Linear Equations

1. **The column picture of** \\(A\bm{x}=\bm{b}\\) : a combination of \\(n\\) columns of \\(A\\) produces the vector \\(\bm{b}\\).
2. This is a vector equation \\(A\bm{x}=x_1\bm{a}_1+\cdots+x_n\bm{a}_n=\bm{b}\\) : the columns of \\(A\\) are \\(\bm{a}_1,\bm{a}_2,\ldots,\bm{a}_n\\).
3. When \\(\bm{b}=\bm{0}\\), a combination \\(A\bm{x}\\) of the columns is *zero* : one possibility is \\(\bm{x}=(0,\ldots,0)\\).
4. The **row picture of** \\(A\bm{x}=\bm{b}\\) : \\(m\\) equations from \\(m\\) rows give \\(m\\) planes meeting at \\(\bm{x}\\).
5. A dot product gives the equation of each plane : \\((\textbf{row 1})\cdot\bm{x}=b_1,\ldots,(\textbf{row }\bm{m})\cdot\bm{x}=b_m\\)
6. When \\(\bm{b}=\bm{0}\\), all the planes \\((\textbf{row }\bm{i})\cdot\bm{x}=0\\) go through the center point \\(\bm{x}=(0,0,\ldots,0)\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">2.1 A</span>&emsp;Describe the column picture of these three equations \(A\bm{x}=\bm{b}\). Solve by careful inspection of the columns (instead of elimination):
      $$
      \begin{array}{r} x+3y+2z=-3 \\ 2x+2y+2z=-2 \\ 3x+5y+6z=-5 \end{array}\qquad\text{which is}\qquad\begin{bmatrix} 1 & \bm{3} & 2 \\ 2 & \bm{2} & 2 \\ 3 & \bm{5} & 6 \end{bmatrix}\begin{bmatrix} x \\ y \\ z \end{bmatrix}=\begin{bmatrix*}[r] -3 \\ -2 \\ -5 \end{bmatrix*}
      $$
      <span class="list-num">Solution</span>&emsp;The column picture asks for a linear combination that produces \(\bm{b}\) from the three columns of \(A\). In this example \(\bm{b}\) is <em>minus the second column</em>. So the solution is \(x=0\), \(y=-1\), \(z=0\). To show that \((0,-1,0)\) is the <em>only</em> solution we have to know that &ldquo;\(A\) is invertible&rdquo; and &ldquo;the columns are independent&rdquo; and &ldquo;the determinant isn't zero.&rdquo;
      <br>
      &emsp;&ensp;Those words are not yet defined but the test comes from elimination: We need (and for this matrix we find) a full set of three nonzero pivots.
      <br>
      &emsp;&ensp;Suppose the right side changes to \(\bm{b}=(4,4,8)=\) sum of the first two columns. Then the good combination has \(x=1\), \(y=1\), \(z=0\). The solution becomes \(\bm{x}=(1,1,0)\).
    </li>
    <li>
      <span class="list-num">2.1 B</span>&emsp;This system has <em>no solution</em>. The planes in the row picture don't meet at a point.
      $$
      \textcolor{RoyalBlue}{\textit{\textbf{No combination of the three columns produces b. How to show this?}}} \\
      \begin{array}{r} x+3y+5z=4 \\ x+2y-3z=5 \\ 2x+5y+2z=8 \end{array}\qquad\begin{bmatrix*}[r] 1 & 3 & 5 \\ 1 & 2 & -3 \\ 2 & 5 & 2 \end{bmatrix*}\begin{bmatrix} x \\ y \\ z \end{bmatrix}=\begin{bmatrix} 4 \\ 5 \\ 8 \end{bmatrix}=\bm{b}
      $$
      <em>Idea</em>&emsp;Add \((\text{equation 1})+(\text{equation 2})-(\text{equation 3})\). The result is \(\bm{0}=\bm{1}\). This system cannot have a solution. We could say: The vector \((1,1,-1)\) is orthogonal to all three columns of \(A\) but <em>not</em> orthogonal to \(\bm{b}\).
      <br>
      &ensp;(<b>1</b>)&ensp;Are any two of the three planes parallel? What are the equations of planes parallel to \(x+3y+5z=4\)?
      <br>
      &ensp;(<b>2</b>)&ensp;Take the dot product of each column of \(A\) (and also \(\bm{b}\)) with \(\bm{y}=(1,1,-1)\). How do those dot products show that no combination of columns equals \(\bm{b}\)?
      &ensp;(<b>3</b>)&ensp;Find three different right side vectors \(\bm{b}^{*}\) and \(\bm{b}^{**}\) and \(\bm{b}^{***}\) that *do* allow solutions.
      <br>
      <span class="list-num">Solution</span>&emsp;
      <br>
      &ensp;(<b>1</b>)&ensp;The planes don't meet at a point, even though no two planes are parallel. For a plane parallel to \(x+3y+5z=4\). Change the &ldquo;4&rdquo;. The parallel plane \(x+3y+5z=0\) goes through the origin \((0,0,0)\). And the equation multiplied by any nonzero constant still gives the same plane, as in \(2x+6y+10z=8\).
      <br>
      &ensp;(<b>2</b>)&ensp;The dot product of each column of \(A\) with \(\bm{y}=(1,1,-1)\) is <em>zero</em>. On the right side, \(\bm{y}\cdot\bm{b}=(1,1,-1)\cdot(4,5,8)=\bm{1}\) is <em>not zero</em>. \(A\bm{x}=\bm{b}\) led to \(0=1\): <strong>no solution</strong>.
      <br>
      &ensp;(<b>3</b>)&ensp;There is a solution when \(\bm{b}\) is a combination of the columns. These three choices of \(\bm{b}\) have solutions including \(\bm{x}^{*}=(1,0,0)\) and \(\bm{x}^{**}=(1,1,1)\) and \(\bm{x}^{***}=(0,0,0)\):
      $$
      \bm{b}^{*}=\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}=\text{first column}\qquad\bm{b}^{**}=\begin{bmatrix} 9 \\ 0 \\ 9 \end{bmatrix}=\text{sum of columns}\qquad\bm{b}^{***}=\begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}
      $$
    </li>
  </ol>
</details>

### The Idea of Elimination

1. For \\(\bm{m}=\bm{n}=3\\), there are three equations \\(A\bm{x}=\bm{b}\\) and three unknowns \\(x_1\\), \\(x_2\\), \\(x_3\\).
2. The first two equations are \\(a_{11}x_1+\cdots=b_1\\) and \\(a_{21}x_1+\cdots=b_2\\)
3. Multiply the first equation by \\(a_{21}/a_{11}\\) and subtract from the second : then \\(x_1\\) **is eliminated**.
4. The corner entry \\(a_{11}\\) is the first "pivot" and the ratio \\(a_{21}/a_{11}\\) is the first "multiplier."
5. Eliminate \\(x_1\\) from every remaining equation \\(i\\) by subtracting \\(a_{i1}/a_{11}\\) times the first equation.
6. Now the last \\(n-1\\) equations contain \\(n-1\\) unknowns \\(x_2,\ldots,x_n\\). *Repeat to eliminate* \\(x_2\\).
7. Elimination breaks down if zero appears in the pivot. Exchanging two equations may save it.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">2.2 A</span>&emsp;When elimination is applied to this matrix \(A\), what are the first and second pivots? What is the multiplier \(\ell_{21}\) in the first step (\(\ell_{21}\) times row 1 is <em>subtracted</em> from row 2)?
      $$
      A=\begin{bmatrix} 1 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 2 \end{bmatrix}\longrightarrow\begin{bmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 \end{bmatrix}\longrightarrow\begin{bmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix}=U
      $$
      What entry in the 2,2 position (instead of 2) would force an exchange of rows 2 and 3? Why is the lower left multiplier \(\ell_{31}=0\), subtracting zero times row 1 from row 3?
      <br>
      <em><strong>If you change the corner entry from \(a_{33}=2\) to \(a_{33}=1\), why does elimination fail?</strong></em>
      <br><br>
      <span class="list-num">Solution</span>&emsp;The first pivot is 1. The multiplier \(\ell_{21}\) is 1,1. When 1 times row 1 is subtracted from row 2, the second pivot is revealed as another 1. If the original middle entry had been 1 instead of 2, that would have forced a row exchange.
      &emsp;&ensp;The multiplier \(\ell_{31}\) is zero because \(a_{31}=0\). A zero at the start of a row needs no elimination. This \(A\) is a "<em>band matrix</em>". Everything stays zero outside the band.
      <br>
      &emsp;&ensp;The last pivot is also 1. So if the original corner \(a_{33}=2\) reduced by 1, elimination would produce 0. <strong>No third pivot, elimination fails.</strong>
    </li>
    <li>
      <span class="list-num">2.2 B</span>&emsp;Suppose \(A\) is already a <em><strong>triangular matrix</strong></em> (upper triangular or lower triangular). <em>Where do you see its pivots?</em> When does \(A\bm{x}=\bm{b}\) have exactly one solution for every \(\bm{b}\)?
      <br><br>
      <span class="list-num">Solution</span>&emsp;The pivots of a triangular matrix are already set along the main diagonal. <em>Elimination succeeds when all those numbers are nonzero.</em> Use <em><strong>back</strong></em> substitution when \(A\) is upper triangular, go <em><strong>forward</strong></em> when \(A\) is lower triangular.
    </li>
    <li>
      <span class="list-num">2.2 C</span>&emsp;Use elimination to reach upper triangular matrices \(U\). Solve by back substitution or explain why this is impossible. What are the pivots (never zero)? Exchange equations when necessary. The only difference is the \(-x\) in the last equation.
      $$
      \begin{array}{rrrr}
        \textbf{Success}\quad & x+y+z=7 & \qquad\textbf{Failure}\quad & x+y+z=7 \\
        & x+y-z=5 & & x+y-z=5 \\
        & x-y+z=3 & & -x-y+z=3
      \end{array}
      $$
      <span class="list-num">Solution</span>&emsp;For the first system, subtract equation 1 from equations 2 and 3 (the multipliers are \(\ell_{21}=1\) and \(\ell_{31}=1\)). The 2,2 entry becomes zero, so exchange equations 2 and 3 :
      $$
      \textbf{Success}\qquad
      \begin{alignedat}{3.5}
        x+ && y+ && z= && 7 \\
        && \bm{0}y- && 2z= && -2 \\
        -&& 2y+ && 0z= && -4
      \end{alignedat}
      \qquad\text{exchanges into}\qquad
      \begin{alignedat}{3.5}
        x+ && y+ && z= && 7 \\
        \bm{-} && \bm{2}y+ && 0z= && -4 \\
        && - && 2z= && -2
      \end{alignedat}
      $$
      Then back substitution gives \(z=1\) and \(y=2\) and \(x=4\). The pivots are 1, &minus;2, &minus;2.
      <br>
      &emsp;&ensp;For the second system, subtract equation 1 from equation 2 as before. Add equation 1 to equation 3. This leaves zero in the 2,2 entry <em>and also below</em> :
      $$
      \textbf{Failure}\qquad
      \begin{alignedat}{4}
        x+ && y+ && z= && 7 & \qquad\text{There is \textbf{no pivot in column 2} (it was \textemdash\space column 1)} \\
        && \bm{0}y- && 2z= && -2 & \qquad\text{A further elimination step gives $\bm{0z=8}$} \\
        && \bm{0}y+ && 2z= && 10 & \qquad\text{The three planes \textbf{don't meet}}
      \end{alignedat}
      $$
      Plane 1 meets plane 2 in a line. Plane 1 meets plane 3 in a parallel line. <em>No solution.</em>
      <br>
      &emsp;&ensp;If we change the &ldquo;3&rdquo; in the original third equation to &ldquo;&minus;5&rdquo; then elimination would lead to \(0=0\). There are infinitely many solutions! <em>The three planes now meet along a whole line.</em>
      <br>
      &emsp;&ensp;Changing 3 to &minus;5 moved the third plane to meet the other two. The second equation gives \(z=1\). Then the first equation leaves \(x+y=6\). <strong>No pivot in column 2 makes \(\bm{y}\) free</strong> (free variables can have any value). Then \(x=6-y\).
    </li>
  </ol>
</details>

### Elimination Using Matrices

1. The first step multiplies the equations \\(A\bm{x}=\bm{b}\\) by a matrix \\(E_{21}\\) to produce \\(E_{21}A\bm{x}=E_{21}\bm{b}\\).
2. That matrix \\(E_{21}A\\) has a zero in row 2, column 1 because \\(x_1\\) is eliminated from equation 2.
3. \\(E_{21}\\) is the **identity matrix** (diagonal of 1's) minus the multiplier \\(a_{21}/a_{11}\\) in row 2, column.
4. Matrix-matrix multiplication is \\(n\\) matrix-vector multiplications: \\(\bm{EA}=\begin{bmatrix} \bm{Ea_1} & \ldots & \bm{Ea_n} \end{bmatrix}\\)
5. We must also multiply \\(E\bm{b}\\)! So \\(E\\) is multiplying the **augmented matrix** \\(\begin{bmatrix} A & \bm{b} \end{bmatrix}=\begin{bmatrix} \bm{a}_1 & \ldots & \bm{a}_n & \bm{b} \end{bmatrix}\\).
6. Elimination multiplies \\(A\bm{x}=\bm{b}\\) by \\(E_{21}\\), \\(E_{31}\\), \\(\ldots\\), \\(E_{n1}\\), then \\(E_{32}\\), \\(E_{42}\\), \\(\ldots\\), \\(E_{n2}\\), and onward.
7. The **row exchange matrix** is not \\(E_{ij}\\) but \\(P_{ij}\\). To find \\(P_{ij}\\), exchange rows \\(i\\) and \\(j\\) of \\(I\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">2.3 A</span>&emsp;What 3 by 3 matrix \(E_{21}\) subtracts 4 times row 1 from row 2? What matrix \(P_{32}\) exchanges row 2 and row 3? If you multiply \(A\) on the <em>right</em> instead of the left, describe the results \(AE_{21}\) and \(AP_{32}\).
      <br><br>
      <span class="list-num">Solution</span>&emsp;By doing those operations on the identity matrix \(I\), we find
      $$
      E_{21}=\begin{bmatrix*}[r] 1 & 0 & 0 \\ -4 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix*}
      \qquad\text{and}\qquad
      P_{32}=\begin{bmatrix} 1 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 1 & 0 \end{bmatrix}
      $$
      Multiplying by \(E_{21}\) on the right side will subtract 4 times <strong>column 2</strong> from <strong>column 1</strong>. Multiplying by \(P_{32}\) on the right will exchange <strong>columns 2</strong> and <strong>3</strong>.
    </li>
    <li>
      <span class="list-num">2.3 B</span>&emsp;Write down the augmented matrix \(\begin{bmatrix} A & \bm{b} \end{bmatrix}\) with an extra column:
      $$
      \begin{array}{r}
        x+2y+2z=1 \\
        4x+8y+9z=3 \\
        3y+2z=1
      \end{array}
      $$
      Apply \(E_{21}\) and then \(P_{32}\) to reach a triangular system. Solve by back substitution. What combined matrix \(P_{32}E_{21}\) will do both steps at once?
      <br><br>
      <span class="list-num">Solution</span>&emsp;\(E_{21}\) removes the 4 in column 1. But zero also appears in column 2:
      $$
      \begin{bmatrix} A & \bm{b} \end{bmatrix}=
      \begin{bmatrix}
        1 & 2 & 2 & 1 \\
        \bm{4} & 8 & 9 & 3 \\
        0 & 3 & 2 & 1
      \end{bmatrix}
      \qquad\text{and}\qquad
      E_{21}\begin{bmatrix} A & \bm{b} \end{bmatrix}=
      \begin{bmatrix*}[r]
        1 & 2 & 2 & 1 \\
        \bm{0} & \bm{0} & 1 & -1 \\
        0 & 3 & 2 & 1
      \end{bmatrix*}
      $$
      Now \(P_{32}\) exchanges rows 2 and 3. Back substitution produces \(z\) then \(y\) and \(x\).
      $$
      P_{32}E_{21}\begin{bmatrix} A & \bm{b} \end{bmatrix}=
      \begin{bmatrix*}[r]
        1 & 2 & 2 & 1 \\
        0 & 3 & 2 & 1 \\
        0 & 0 & 1 & -1
      \end{bmatrix*}
      \qquad\text{and}\qquad
      \begin{bmatrix} x \\ y \\ z \end{bmatrix}=\begin{bmatrix*}[r] 1 \\ 1 \\ -1 \end{bmatrix*}
      $$
      For the matrix \(P_{32}E_{21}\) that does both steps at once, <em>apply \(P_{32}\) to \(E_{21}\)</em>.
      $$
      \begin{array}{l}
       \textbf{One matrix} \\ 
       \textbf{Both steps}
      \end{array}
      \qquad
      P_{32}E_{21}=\text{exchange the rows of }E_{21}=\begin{bmatrix*} 1 & 0 & 0 \\ 0 & 0 & 1 \\ -4 & 1 & 0 \end{bmatrix*}\text{.}
      $$
    </li>
    <li>
      <span class="list-num">2.3 C</span>&emsp;Multiply these matrices in two ways. First, rows of \(A\) times columns of \(B\). Second, <em><strong>columns of \(\bm{A}\) times rows of \(\bm{B}\)</strong></em>. That unusual way produces two matrices that add to \(AB\). How many separate ordinary multiplications are needed?
      $$
      \textbf{Both ways}\qquad
      AB=\begin{bmatrix} 3 & 4 \\ 1 & 5 \\ 2 & 0 \end{bmatrix}\begin{bmatrix} 2 & 4 \\ 1 & 1 \end{bmatrix}=\begin{bmatrix*}[r] \bm{10} & \bm{16} \\ \bm{7} & \bm{9} \\ \bm{4} & \bm{8} \end{bmatrix*}
      $$
      <span class="list-num">Solution</span>&emsp;Rows of \(A\) times columns of \(B\) are dot products of vectors:
      $$
      \begin{alignedat}{1.5}
        (\text{row 1})\cdot(\text{column 1})=
        \begin{matrix} \begin{bmatrix} 3 & 4 \end{bmatrix} \\ \\ \text{} \end{matrix}
        \begin{bmatrix} 2 \\ 1 \end{bmatrix}
        =
      &&
        \bm{10}\qquad\text{is the (1,1) entry of $AB$}
      \\
      \\
        (\text{row 2})\cdot(\text{column 1})=
        \begin{bmatrix} 1 & 5 \end{bmatrix}\begin{bmatrix} 2 \\ 1 \end{bmatrix}
        =
      &&
        \bm{7}\qquad\text{is the (2,1) entry of $AB$}
      \end{alignedat}
      $$
      We need 6 dot products, 2 multiplications each, 12 in all (3 &sdot; 2 &sdot; 2). The same \(AB\) comes from <em>columns of \(A\) times rows of \(B\)</em>. A column times a row is a matrix.
      $$
      AB=
      \begin{bmatrix} 3 \\ 1 \\ 2 \end{bmatrix}
      \begin{matrix} \begin{bmatrix} 2 & 4 \end{bmatrix} \\ \text{} \end{matrix}
      +
      \begin{bmatrix} 4 \\ 5 \\ 0 \end{bmatrix}
      \begin{matrix} \\ \begin{bmatrix} 1 & 1 \end{bmatrix} \end{matrix}
      =
      \begin{bmatrix*}[r] \bm{6} & \bm{12} \\ \bm{2} & \bm{4} \\ \bm{4} & \bm{8} \end{bmatrix*}
      +
      \begin{bmatrix} \bm{4} & \bm{4} \\ \bm{5} & \bm{5} \\ \bm{0} & \bm{0} \end{bmatrix}
      $$
    </li>
  </ol>
</details>

### Rules for Matrix Operations

1. Matrices \\(A\\) with \\(n\\) columns multiply matrices \\(B\\) with \\(n\\) rows : \\(\boxed{A_{\bm{m}\times\bm{n}}B_{\bm{n}\times\bm{p}}=C_{\bm{m}\times\bm{p}}}\\).
2. Each entry in \\(AB=C\\) is a dot product : \\(C_{ij}=(\text{row $i$ of $A$})\cdot(\text{column $j$ of $B$})\\).
3. This rule is chosen so that <strong>\\(\bm{AB}\\) times \\(C\\) equals \\(A\\) times \\(BC\\)</strong>. And \\((AB)\bm{x}=A(B\bm{x})\\)
4. More ways to compute \\(AB\\) : \\((\text{$A$ times columns of $B$})\\), \\((\text{rows of $A$ times $B$})\\), \\((\textit{columns times rows})\\).
5. It is not usually true that \\(AB=BA\\). In most cases \\(A\\) <em>doesn't commute with</em> \\(B\\).
6. Matrices can be multiplied by <em>blocks</em> : \\(A=\begin{bmatrix} A_1 & A_2 \end{bmatrix}\\) times \\(B=\begin{bmatrix} B_1 \\\ B_2 \end{bmatrix}\\) is \\(A_1B_1+A2B_2\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">2.4 A</span>&emsp;A graph or a network has \(n\) nodes. Its <strong>adjacency matrix</strong> \(S\) is \(n\) by \(n\). This is a 0&ndash;1 matrix with \(s_{ij}=1\) when nodes \(i\) and \(j\) are connected by an edge.
      {% asset_img Screenshot_20220315_150242.png %}
      The matrix \(S^2\) has a useful interpretation. <strong>\(\bm{(S^2)_{ij}}\) counts the walks of length 2</strong> between node \(i\) and node \(j\). Between nodes 2 and 3 the graph has two walks: go via 1 or go via 4. From node 1 to node 1, there are also two walks: 1&ndash;2&ndash;1 and 1&ndash;3&ndash;1.
      $$
      S^2=
      \begin{bmatrix}
        \bm{2} & 1 & 1 & 2 \\
        1 & 3 & \bm{2} & 1 \\
        1 & 2 & 3 & 1 \\
        2 & 1 & 1 & 2
      \end{bmatrix}
      \qquad
      S^3=
      \begin{bmatrix}
        2 & \bm{5} & 5 & 2 \\
        5 & 4 & 5 & 5 \\
        5 & 5 & 4 & 5 \\
        2 & 5 & 5 & 2
      \end{bmatrix}
      $$
      Can you find 5 walks of length 3 between nodes 1 and 2?
      <br>
      &emsp;&ensp;The real question is why \(S^N\) counts all the \(N\)-step paths between pairs of nodes. Start with \(S^2\) and look at matrix multiplication by dot products:
      $$
      \begin{equation}
        (S^2)_{ij}=(\text{row $i$ of $S$})\cdot(\text{column $j$ of $S$})=s_{i1}s_{1j}+s_{i2}s_{2j}+s_{i3}s_{3j}+s_{i4}s_{4j}
      \end{equation}
      $$
      If there is a 2-step path \(i\rarr 1\rarr j\), the first multiplication gives \(s_{i1}s_{1j}=(1)(1)=1\). If \(i\rarr 1\rarr j\) is <em>not</em> a path, then either \(i\rarr 1\) is missing or \(1\rarr j\) is missing. So the multiplication gives \(s_{i1}s_{1j}=0\) in that case.
      <br>
      &emsp;&ensp;\((S^2)_{ij}\) is adding up 1's for all the 2-step paths \(i\rarr k\rarr j\). So it counts those paths. In the same way \(S^{N-1}S\) will count \(N\)-step paths, because those are \((N-1)\)-step paths from \(i\) to \(k\) followed by one step from \(k\) to \(j\). Matrix multiplication is exactly suited to counting paths on a graph&mdash;channels of communcation between employees in a company.
    </li>
    <li>
      <span class="list-num">2.4 B</span>&emsp;For these matrices, when does \(AB=BA\)? When does \(BC=CB\)? When does \(A\) times \(BC\) equal \(AB\) times \(C\)? Give the conditions on their entries \(p\), \(q\), \(r\), \(z\) :
      $$
      A=\begin{bmatrix} p & 0 \\ q & r \end{bmatrix}
      \qquad
      B=\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}
      \qquad
      C=\begin{bmatrix} 0 & z \\ 0 & 0 \end{bmatrix}
      $$
      If \(p\), \(q\), \(r\), \(1\), \(z\) are 4 by 4 blocks instead of numbers, do the answers change?
      <br><br>
      <span class="list-num">Solution</span>&emsp;First of all, \(A\) times \(BC\) <em>always</em> equals \(AB\) times \(C\). Parentheses are not needed in \(A(BC)=(AB)C=ABC\). But we must keep the matrices in this order :
      $$
      \begin{array}{l}
        \textbf{Usually }\bm{AB\neq BA}\qquad AB=\begin{bmatrix} p & p \\ q & q+r \end{bmatrix}\qquad BA=\begin{bmatrix} p+q & r \\ q & r \end{bmatrix}\text{.}
      \\
      \\
        \textbf{By chance }\bm{BC=CB}\qquad BC=\begin{bmatrix} 0 & z \\ 0 & 0 \end{bmatrix}\qquad CB=\begin{bmatrix} 0 & z \\ 0 & 0 \end{bmatrix}\text{.}
      \end{array}
      $$
      \(B\) and \(C\) happen to commute. Part of the explanation is that the diagonal of \(B\) is \(I\), which commutes with all 2 by 2 matrices. When \(p\), \(q\), \(r\), \(z\) are 4 by 4 blocks and 1 changes to \(I\), all these products remain correct. So the answers are the same.
    </li>
  </ol>
</details>

### Inverse Matrices

1. If the square matrix \\(A\\) has an inverse, then both \\(A^{-1}A=I\\) and \\(AA^{-1}=I\\)
2. The <em>algorithm</em> to test invertibility is elimination : \\(A\\) must have \\(n\\) (nonzero) pivots.
3. The <em>algebra</em> test for invertibility is the determinant of \\(A\\) : \\(\det A\\) must not zero.
4. The <em>equation</em> that tests for invertibility is \\(A\bm{x}=\bm{0}\\) : <strong>\\(\bm{x=0}\\) must be the only solution.</strong>
5. If \\(A\\) and \\(B\\) (same size) are invertible then so is \\(AB\\) : \\(\boxed{(AB)^{-1}=B^{-1}A^{-1}}\\) .
6. \\(AA^{-1}=I\\) is \\(n\\) equations for \\(n\\) columns of \\(A^{-1}\\). Gauss-Jordan eliminates \\(\begin{bmatrix} A & I \end{bmatrix}\\) to \\(\begin{bmatrix} I & A^{-1} \end{bmatrix}\\).
7. [The last section of this page](#LINEAR-ALGEBRA-IN-A-NUTSHELL) gives 14 equivalent conditions for a square \\(A\\) to be invertible.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">2.5 A</span>&emsp;The inverse of a triangular <strong>difference matrix</strong> \(A\) is a triangular <strong>sum matrix</strong> \(S\) :
      $$
      \begin{alignedat}{1}
      \begin{bmatrix} A & I \end{bmatrix}
      & =
        \left[\begin{array}{rrr|rrr}
         1 & 0 & 0 & 1 & 0 & 0 \\
         -1 & 1 & 0 & 0 & 1 & 0 \\
         0 & -1 & 1 & 0 & 0 & 1 
        \end{array}\right]
        \rarr
        \left[\begin{array}{rrr|rrr}
          1 & 0 & 0 & 1 & 0 & 0 \\
          0 & 1 & 0 & 1 & 1 & 0 \\
          0 & -1 & 1 & 0 & 0 & 1
        \end{array}\right]
      \\
      & \rarr
        \left[\begin{array}{rrr|rrr}
          1 & 0 & 0 & 1 & 0 & 0 \\
          0 & 1 & 0 & 1 & 1 & 0 \\
          0 & 0 & 1 & 1 & 1 & 1
        \end{array}\right]
        =\begin{bmatrix} I & A^{-1} \end{bmatrix}
        =\begin{bmatrix} I & \textit{sum matrix} \end{bmatrix}
      \end{alignedat}
      $$
      It I change \(a_{13}\) to \(-1\), then all rows of \(A\) add to zero. The equation \(A\bm{x}=0\) will now have the nonzero solution \(\bm{x}=(1,1,1)\). A clear signal : <em><strong>This new \(A\) can't be inverted.</strong></em>
    </li>
    <li>
      <span class="list-num">2.5 B</span>&emsp;Three of these matrices are invertible, and three are singular. Find the inverse when it exists. Give reasons for noninvertibility (zero determinant, too few pivots, nonzero solution to \(A\bm{x}=\bm{0}\)) for the other three. The matrices are in the order \(A\), \(B\), \(C\), \(D\), \(S\), \(E\) :
      $$
      \begin{bmatrix} 4 & 3 \\ 8 & 6 \end{bmatrix}
      \qquad
      \begin{bmatrix} 4 & 3 \\ 8 & 7 \end{bmatrix}
      \qquad
      \begin{bmatrix} 6 & 6 \\ 6 & 0 \end{bmatrix}
      \qquad
      \begin{bmatrix} 6 & 6 \\ 6 & 6 \end{bmatrix}
      \qquad
      \begin{bmatrix} 1 & 0 & 0 \\ 1 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix}
      \qquad
      \begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix}
      $$
      <span class="list-num">Solution</span>
      $$
      B^{-1}=\frac{1}{4}\begin{bmatrix*}[r] 7 & -3 \\ -8 & 4 \end{bmatrix*}
      \qquad
      C^{-1}=\frac{1}{36}\begin{bmatrix*}[r] 0 & 6 \\ 6 & -6 \end{bmatrix*}
      \qquad
      S^{-1}=\begin{bmatrix*} 1 & 0 & 0 \\ -1 & 1 & 0 \\ 0 & -1 & 1 \end{bmatrix*}
      $$
      \(A\) is not invertible because its determinant is \(4\cdot 6-3\cdot 8=24-24=0\). \(D\) is not invertible because there is only one pivot; the second row becomes zero when the first row is subtracted. \(E\) has two equal rows (and the second column minus the first column is zero). In other words \(E\bm{x}=\bm{0}\) has the solution \(\bm{x}=(-1,1,0)\).
      <br>
      &emsp;&ensp;Of course all three reasons for noninvertibility would apply to each of \(A\), \(D\), \(E\).
    </li>
    <li>
      <span class="list-num">2.5 C</span>&emsp;Apply the Gauss-Jordan method to invert this triangular &ldquo;Pascal matrix&rdquo; \(L\). You see <strong>Pascal's triangle</strong>&mdash;adding each entry to the entry on its left gives the entry below. The entries of \(L\) are &ldquo;binomial coefficients&rdquo;. The next row would be \(1, 4, 6, 4, 1\).
      $$
      \textbf{Triangular Pascal matrix}\qquad
      \bm{L}=
      \begin{bmatrix}
        \bm{1} & 0 & 0 & 0 \\
        \bm{1} & \bm{1} & 0 & 0 \\
        \bm{1} & \bm{2} & \bm{1} & 0 \\
        \bm{1} & \bm{3} & \bm{3} & \bm{1}
      \end{bmatrix}
      =\texttt{abs}(\texttt{pascal}(4,1))
      $$
      <span class="list-num">Solution</span>&emsp;Gausss-Jordan starts with \(\begin{bmatrix} L & I \end{bmatrix}\) and produces zeros by subtracting row 1:
      $$
      \begin{bmatrix} \bm{L} & \bm{I} \end{bmatrix}
      =
      \left[\begin{array}{cccc|cccc}
        \bm{1} & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
        \bm{1} & \bm{1} & 0 & 0 & 0 & 1 & 0 & 0 \\
        \bm{1} & \bm{2} & \bm{1} & 0 & 0 & 0 & 1 & 0 \\
        \bm{1} & \bm{3} & \bm{3} & \bm{1} & 0 & 0 & 0 & 1
      \end{array}\right]
      \rarr
      \left[\begin{array}{rrrr|rrrr}
        1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
        \bm{0} & 1 & 0 & 0 & \bm{-1} & 1 & 0 & 0 \\
        \bm{0} & 2 & 1 & 0 & \bm{-1} & 0 & 1 & 0 \\
        \bm{0} & 3 & 3 & 1 & \bm{-1} & 0 & 0 & 1
      \end{array}\right]
      \text{.}
      $$
      The next stage creates zeros below the second pivot, using multipliers 2 and 3. Then the last stage subtracts 3 times the new row 3 from the new row 4:
      $$
      \rarr
      \left[\begin{array}{rrrr|rrrr}
        1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 & -1 & 1 & 0 & 0 \\
        0 & \bm{0} & 1 & 0 & \bm{1} & \bm{-2} & 1 & 0 \\
        0 & \bm{0} & 3 & 1 & \bm{2} & \bm{-3} & 0 & 1
      \end{array}\right]
      \rarr
      \left[\begin{array}{rrrr|rrrr}
        1 & 0 & 0 & 0 & \bm{1} & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 & \bm{-1} & \bm{1} & 0 & 0 \\
        0 & 0 & 1 & 0 & \bm{1} & \bm{-2} & \bm{1} & 0 \\
        0 & 0 & 0 & 1 & \bm{-1} & \bm{3} & \bm{-3} & \bm{1}
      \end{array}\right]
      =
      \begin{bmatrix} \bm{I} & \bm{L^{-1}} \end{bmatrix}
      $$
      All the pivots were 1! So we didn't need to divide rows by pivots to get \(I\). The inverse matrix \(L^{-1}\) look like \(L\) itself, except odd-numbered diagonals have minus signs.
      <br>
      &emsp;&ensp;The same pattern continues to \(n\) by \(n\) Pascal matrices. \(L^{-1}\) has &ldquo;alternating diagonals&rdquo;
    </li>
</details>

### Elimination <math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mo>=</mo></mrow></math> Factorization: <math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>A</mi><mo>=</mo><mi>L</mi><mi>U</mi></mrow></math>

1. Each elimination step \\(E_{ij}\\) is inverted by \\(L_{ij}\\). Off the main diagonal change \\(-\ell_{ij}\\) to \\(+\ell_{ij}\\).
2. The whole forward elimination process (with no row exchanges) is inverted by \\(\bm{L}\\) :
   <div>
   $$
   \bm{L}=(L_{21}L_{31}\ldots L_{n1})(L_{32}\ldots L_{n2})(L_{43}\ldots L_{n3})\ldots(L_{n\space n-1})\text{.}
   $$
   <div>
3. That product matrix \\(\bm{L}\\) is still lower triangular. **Every multiplier \\(\bm{\ell_{ij}}\\) is in row \\(\bm{i}\\), column \\(\bm{j}\\).**
4. The original \\(A\\) is recovered from \\(U\\) by \\(\bm{A}=\bm{LU}=(\text{lower triangular})(\text{upper triangular})\\).
5. Elimination on \\(A\bm{x}=\bm{b}\\) reaches \\(U\bm{x}=\bm{c}\\). Then back-substitution solves \\(U\bm{x}=\bm{c}\\)
6. Solving a triangular system takes \\(n^2/2\\) multiply-subtracts. Elimination to find \\(U\\) takes \\(n^3/3\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">2.6 A</span>&emsp;The lower triangular Pascal matrix \(L\) contains the famous <em>&ldquo;Pascal triangle&rdquo;</em>. Gauss-Jordan inverted \(L\) in the worked example <span class="list-num">2.5 C</span>. Here we factor Pascal.
      <br>
      &emsp;&ensp;<strong>The symmetric Pascal matrix \(\bm{P}\) is a product of triangular Pascal matrices \(L\) and \(U\).</strong> The symmetric \(P\) has Pascal's triangle tilted, so each entry is the sum of entry above and the entry to the left. The \(n\) by \(n\) symmetric \(P\) is \(\texttt{pascal}(n)\) in \(\textsf{MATLAB}\).
      <br>
      <strong>Problem:</strong> <em>Establish the amazing lower-upper factorization \(P=LU\).</em>
      $$
      \texttt{pascal}(4)=
      \begin{bmatrix}
        \bm{1} & \bm{1} & \bm{1} & \bm{1} \\
        \bm{1} & \bm{2} & \bm{3} & 4 \\
        \bm{1} & \bm{3} & 6 & 10 \\
        \bm{1} & 4 & 10 & 20
      \end{bmatrix}
      =
      \begin{bmatrix}
        \bm{1} & 0 & 0 & 0 \\
        \bm{1} & \bm{1} & 0 & 0 \\
        \bm{1} & \bm{2} & \bm{1} & 0 \\
        \bm{1} & \bm{3} & \bm{3} & \bm{1}
      \end{bmatrix}
      \begin{bmatrix}
        \bm{1} & \bm{1} & \bm{1} & \bm{1} \\
        0 & \bm{1} & \bm{2} & \bm{3} \\
        0 & 0 & \bm{1} & \bm{3} \\
        0 & 0 & 0 & \bm{1}
      \end{bmatrix}
      =LU\text{.}
      $$
      Then predict and check the next row and column for 5 by 5 Pascal matrices.
      <br>
      <span class="list-num">Solution</span>&emsp;You could multiply \(LU\) to get \(P\). Better to start with the symmetric \(P\) and reach the upper triangular \(U\) by elimination :
      $$
      P=
      \begin{bmatrix}
        1 & 1 & 1 & 1 \\
        1 & 2 & 3 & 4 \\
        1 & 3 & 6 & 10 \\
        1 & 4 & 10 & 20
      \end{bmatrix}
      \rarr
      \begin{bmatrix}
        1 & 1 & 1 & 1 \\
        0 & 1 & 2 & 3 \\
        0 & 2 & 5 & 9 \\
        0 & 3 & 9 & 19
      \end{bmatrix}
      \rarr
      \begin{bmatrix}
        1 & 1 & 1 & 1 \\
        0 & 1 & 2 & 3 \\
        0 & 0 & 1 & 3 \\
        0 & 0 & 3 & 10 \\
      \end{bmatrix}
      \rarr
      \begin{bmatrix}
        1 & 1 & 1 & 1 \\
        0 & 1 & 2 & 3 \\
        0 & 0 & 1 & 3 \\
        0 & 0 & 0 & 1
      \end{bmatrix}
      =U
      $$
      The multipliers \(\ell_{ij}\) that entered these steps go perfectly into \(L\). Then \(P=LU\) is a paticularly neat example. <em>Notice that every pivot is \(1\) on the diagonal of \(U\).</em>
      <br>
      &emsp;&ensp;The next section will show how symmetry produces a special relationship between the triangular \(L\) and \(U\). For Pascal, \(U\) is the <strong>&ldquo;transpose&rdquo;</strong> of \(\bm{L}\).
      <br>
      &emsp;&ensp;You might expect the \(\textsf{MATLAB}\) command \(\texttt{lu}(\texttt{pascal}(4))\) to produce these \(L\) and \(U\). That doesn't happen because \(\textbf{\textsf{lu}}\) subroutine chooses the largest available pivot in each column. The second pivot will change from 1 to 3. But a &ldquo;Cholesky factorization&rdquo; does no row exchanges: \(U=\texttt{chol}(\texttt{pascal}(4))\)
      <br>
      &emsp;&ensp;The full proof of \(P=LU\) for all Pascal sizes is quite fascinating. The paper &ldquo;Pascal Matrices&rdquo; is on the course web page <strong>web.mit.edu/18.06</strong> which is also available through MIT's <em>OpenCourseWare</em> at <strong>ocw.mit.edu</strong>. These Pascal matrices have so many remarkable properties&mdash;we will see them again.
    </li>
    <li>
      <span class="list-num">2.6 B</span>&emsp;The problem is: <em>Solve \(P\bm{x}=\bm{b}=(1,0,0,0)\).</em> This right side \(=\) column of \(I\) means that \(\bm{x}\) will be the first column of \(P^{-1}\). That is Gauss-Jordan, matching the columns of \(PP^{-1}=I\). We already know the Pascal matrices \(L\) and \(U\) as factors of \(P\):
      $$
      \textbf{Two triangular systems}\qquad L\bm{c}=\bm{b}\text{ (forward)}\qquad U\bm{x}=\bm{c}\text{ (back).}
      $$
      <span class="list-num">Solution</span>&emsp;The lower triangular system \(L\bm{c}=\bm{b}\) is solved <em>top to bottum</em>:
      $$
      \begin{alignedat}{3.5}
        c_1 &   &      &   &      && =1 \\
        c_1 & + & c_2  &   &      && =0 \\
        c_1 & + & 2c_2 & + & c_3  && =0 \\
        c_1 & + & 3c_2 & + & 3c_3 && +c_4=0
      \end{alignedat}
      \qquad
      \text{gives}
      \qquad
      \begin{alignedat}{0.5}
        c_1=+1 \\
        c_2=-1 \\
        c_3=+1 \\
        c_4=-1
      \end{alignedat}
      $$
      Forward elimination is multiplication by \(L^{-1}\). It produces the upper triangular system \(U\bm{x}=\bm{c}\). The solution \(\bm{x}\) comes as always by back substitution, <em>bottom to top:</em>
      $$
      \begin{alignedat}{3.5}
        x_1+x_2+ &&  x_3+ &&  x_4= &&  1 \\
            x_2+ && 2x_3+ && 3x_4= && -1 \\
                 &&  x_3+ && 3x_4= &&  1 \\
                 &&       &&  x_4= && -1
      \end{alignedat}
      \qquad
      \text{gives}
      \qquad
      \begin{alignedat}{0.5}
        x_1=\bm{+4} \\
        x_2=\bm{-6} \\
        x_3=\bm{+4} \\
        x_4=\bm{-1}
      \end{alignedat}
      $$
      I see a pattern in that \(\bm{x}\), but I don't know where it comes from. Try \(\texttt{inv}(\texttt{pascal}(4))\).
    </li>
</details>

## LINEAR ALGEBRA IN A NUTSHELL

<div style="text-align:center"><strong>((<em>The matrix \(A\) is \(n\) by \(n\)</em>))</strong></div>
<div>
$$
\begin{array}{ll}
\textcolor{RoyalBlue}{\textbf{Nonsingular}} & \textcolor{RoyalBlue}{\textbf{Singular}} \\
A\text{ is invertible} & A\text{ is not invertible} \\
\text{The columns are independent} & \text{The columns are dependent} \\
\text{The rows are independent} & \text{The rows are dependent} \\
\text{The determinant is not zero} & \text{The determinant is zero} \\
A\bm{x}=\bm{0}\text{ has one solution }\bm{x}=\bm{0} & A\bm{x}=\bm{0}\text{ has infinitely many solutions} \\
A\bm{x}=\bm{b}\text{ has one solution }\bm{x}=A^{-1}\bm{b} & A\bm{x}=\bm{b}\text{ has no solution or infinitely many} \\
A\text{ has $n$ (nonzero) pivots} & A\text{ has $r\lt n$ pivots} \\
A\text{ has full rank }r=n & A\text{ has rank }r\lt n \\
\text{The reduced row echelon form is }R=I\qquad & R\text{ has at least one zero row} \\
\text{The column space is all of }\mathbf{R}^n & \text{The column space has dimension }r\lt n \\
\text{The row space is all of }\mathbf{R}^n & \text{The row space has dimension }r\lt n \\
\text{All eigenvalues are nonzero} & \text{Zero is an eigenvalue of }A \\
A^TA\text{ is symmetric positive definite} & A^TA\text{ is only semidefinite} \\
A\text{ has $n$ (positive) singular values} & A\text{ has $r\lt n$ singular values}
\end{array}
$$
</div>