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

<!-- more -->

## Chapter 1

### Vectors and Linear Combinations

1. \\(3\bm{v}+5\bm{w}\\) is a typical **linear combination** \\(c\bm{v}+d\bm{w}\\) of the vectors \\(\bm{v}\\) and \\(\bm{w}\\)
2. For \\(\bm{v}=\begin{bmatrix} 1 \\\ 1 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix} 2 \\\ 3 \end{bmatrix}\\)
3. The vector \\(\begin{bmatrix} 2 \\\ 3 \end{bmatrix}=\begin{bmatrix} 2 \\\ 0 \end{bmatrix}+\begin{bmatrix} 0 \\\ 3 \end{bmatrix}\\) goes across to \\(x=2\\) and up to \\(y=3\\) in the \\(xy\\) plane.
4. The combinations \\(c\begin{bmatrix} 1 \\\ 1 \end{bmatrix}+d\begin{bmatrix} 2 \\\ 3 \end{bmatrix}\\) fill the whole \\(xy\\) place. They produce every \\(\begin{bmatrix} x \\\ y \end{bmatrix}\\)
5. The combinations \\(c\begin{bmatrix} 1 \\\ 1 \\\ 1 \end{bmatrix}+d\begin{bmatrix} 2 \\\ 3 \\\ 4 \end{bmatrix}\\) fill a **plane** in \\(xyz\\) space. Same plane for \\(\begin{bmatrix} 1 \\\ 1 \\\ 1 \end{bmatrix}\\), \\(\begin{bmatrix} 3 \\\ 4 \\\ 5 \end{bmatrix}\\).
6. But \\(\begin{array}{l} c+2d=1 \\\ c+3d=0 \\\ c+4d=0 \end{array}\\) has no solution because its right side \\(\begin{bmatrix} 1 \\\ 0 \\\ 0 \end{bmatrix}\\) is not on that plane.

### Lengths and Dot Products

1. The "dot product" of \\(\bm{v}=\begin{bmatrix} 1 \\\ 2 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix} 4 \\\ 5 \end{bmatrix}\\) is \\(\bm{v}\cdot\bm{w}=(1)(4)+(2)(5)=4+10=\bm{14}\\)
2. \\(\bm{v}=\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) and \\(\bm{w}=\left[\begin{array}{r} 4 \\\ -4 \\\ 4 \end{array}\right]\\) are perpendicular because \\(\bm{v}\cdot\bm{w}\\) is zero: \\((1)(4)+(3)(-4)+(2)(4)=\bm{0}\\)
3. The length squared of \\(\bm{v}=\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) is \\(\bm{v}\cdot\bm{v}=1+9+4=14\\). **The length is** \\(\\|\bm{v}\\|=\sqrt{\bm{14}}\\).
4. Then \\(\bm{u}=\dfrac{\bm{v}}{\\|\bm{v}\\|}=\dfrac{\bm{v}}{\sqrt{14}}=\dfrac{1}{\sqrt{14}}\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) has length \\(\\|\bm{u}\\|=\bm{1}\\). Check \\(\dfrac{1}{\underline{14}}+\dfrac{9}{\underline{14}}+\dfrac{4}{\underline{14}}=1\\).
5. The angle \\(\theta\\) between \\(\bm{v}\\) and \\(\bm{w}\\) has \\(\cos\theta=\dfrac{\bm{v}\cdot\bm{w}}{\\|\bm{v}\\|\\|\bm{w}\\|}\\)
6. The angle between \\(\begin{bmatrix} 1 \\\ 0 \end{bmatrix}\\) and \\(\begin{bmatrix} 1 \\\ 1\end{bmatrix}\\) has \\(\cos\theta=\dfrac{1}{(1)(\sqrt{2})}\\). That angle is \\(\theta=45\degree\\)
7. All angles have \\(|\cos\theta|\leq 1\\). So all vectors have \\(\boxed{|\bm{v}\cdot\bm{w}|\leq\\|\bm{v}\\|\\|\bm{w}\\|}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-num">1.2 A</span>&emsp;For the vectors \(\bm{v}=(3,4)\) and \(\bm{w}=(4,3)\) test the Schwarz inqeuality on \(\bm{v}\cdot\bm{w}\) and the triangle inqeuality on \(\|\bm{v}+\bm{w}\|\). Find \(\cos\theta\) for the angle between \(\bm{v}\) and \(\bm{w}\). Which \(\bm{v}\) and \(\bm{w}\) give <i>equality</i> \(\|\bm{v}\cdot\bm{w}\|=\|\bm{v}\|\|\bm{w}\|\) and \(\|\bm{v}+\bm{w}\|=\|\bm{w}\|+\|\bm{w}\|\)?
      <br><br>
      <span class="list-num">Solution</span>&emsp;The dot product is \(\bm{v}\cdot\bm{w}=(3)(4)+(4)(3)=24\). The length of \(\bm{v}\) is \(\|\bm{v}\|=\sqrt{9+16}=5\) and also \(\|\bm{w}\|=5\). The sum \(\bm{v}+\bm{w}=(7,7)\) has length \(7\sqrt{2}<10\).
      $$
      \begin{array}{ll}
        \textbf{Schwarz inequality} & |\bm{v}\cdot\bm{w}|\leq\|\bm{v}\|\|\bm{w}\|\quad\text{is}\quad 24<25 \\
        \textbf{Triangle inequality} & \|\bm{v}+\bm{w}\|\leq\|\bm{v}\|+\|\bm{w}\|\quad\text{is}\quad 7\sqrt{2}<5+5 \\
        \textbf{Cosine of angle} & \cos\theta=\frac{24}{25}\quad\text{Thin angle from }\bm{v}=(3,4)\text{ to }\bm{w}=(4,3)
      \end{array}
      $$
      <i>Equality</i>: One vector is a multiple of the other as in \(\bm{w}=c\bm{v}\). Then the angle is \(0\degree\) or \(180\degree\). In this case \(|\cos\theta|=1\) and \(|\bm{v}\cdot\bm{w}|\) <i>equals</i> \(\|\bm{v}\|\|\bm{w}\|\). If the angle is \(0\degree\), as in \(\bm{w}=2\bm{v}\), then \(\|\bm{v}+\bm{w}\|=\|\bm{v}\|+\|\bm{w}\|\) (both sides give \(3\|\bm{v}\|\)). This \(\bm{v}\), \(2\bm{v}\), \(3\bm{v}\) triangle is flat!
    </li>
    <li>
      <span class="list-num">1.2 B</span>&emsp;Find a unit vector \(\bm{u}\) in the direction of \(\bm{v}=(3,4)\). Find a unit vector \(\bm{U}\) that is perpendicular to \(\bm{u}\). How many possibilities for \(\bm{U}\)?<br><br>
      <span class="list-num">Solution</span>&emsp;For a unit vector \(\bm{u}\), divide \(\bm{v}\) by its length \(\|\bm{v}\|=5\). For a perpendicular vector \(\bm{V}\) we can choose \((-4,3)\) since the dot product \(\bm{v}\cdot\bm{V}\) is \((3)(-4)+(4)(3)=0\). For a <i>unit</i> vector perpendicular to \(\bm{u}\), divide \(\bm{V}\) by its length \(\|\bm{V}\|\):
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
      \begin{alignedat}{3} % 0.5 per column, columms align are col1=right, col2=left, col3=right...
        \bm{x}\cdot\bm{r}=1\qquad\text{is}\qquad &  & 2c- && d=1\qquad & \textbf{The same equations as}  \\
        \bm{x}\cdot\bm{s}=0\qquad\text{is}\qquad & - & c+ && 2d=0\qquad & \textbf{in Worked Example 1.1 C}
      \end{alignedat}
      $$
      &emsp;&ensp;<i>Comment on n equations for \(x=(x_1,\ldots,x_n)\) in n-dimensional space</i><br>
      Section 1.1 would start with columns \(\bm{v}_j\). The goal is to produce \(x_1\bm{v}_1+\cdots+x_n\bm{v}_n=\bm{b}\).<br>
      This section would start from rows \(\bm{r}_i\). Now the goal is to find \(\bm{x}\) with \(\bm{x}\cdot\bm{r}_i=b_i\).<br>
      &emsp;&ensp;Soon the \(\bm{v}\)'s will be columns of a matrix \(A\), and the \(\bm{r}\)'s will be the rows of \(A\). Then the (one and only) problem will be to solve \(A\bm{x}=\bm{b}\).
    </li>
  </ol>
</details>

