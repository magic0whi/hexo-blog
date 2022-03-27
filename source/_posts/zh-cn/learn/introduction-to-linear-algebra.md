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
.content ol.worked-examples > li > span.list-head, span.list-head {
    font-family: Ubuntu, Roboto, 'Open Sans';
    font-weight: bold;
}
.content details summary span.list-summary {
    font-family: Ubuntu, Roboto, 'Open Sans';
    font-weight: bold;
    color: RoyalBlue;
}
.list-table {
    border-collapse: collapse;
    width:100%;
}
.list-table tr > td:nth-child(1) {
    white-space: nowrap;
    text-align: right;
    vertical-align: top;
    border: none;
    padding: 0;
    width: 1%;
}
.list-table tr > td:nth-child(2) {
    text-align: left;
    vertical-align: top;
    border: none;
    padding: 0;
}
</style>

This is an excerpt of both brief summary and worked examples of each section in the Introduction to Linear Algebra (Fifth Edition) by Gilbert Strang

[MIT Course 18.06, Spring 2022](https://github.com/mitmath/1806/)

TODO: draw svg format figure by using tikz; Using `gather*` or `align*` or `array` while there is a line break in display mode LaTex.

<!-- more -->

<!-- <div>
$$
\newcommand\d[1]{\operatorname{d}\!{\\#1}}
$$
</div> -->

## Chapter 1 Introduction to Vectors

### Vectors and Linear Combinations

1. \\(3\bm{v}+5\bm{w}\\) is a typical **linear combination** \\(c\bm{v}+d\bm{w}\\) of the vectors \\(\bm{v}\\) and \\(\bm{w}\\).
2. For \\(\bm{v}=\begin{bmatrix} 1 \\\ 1 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix} 2 \\\ 3 \end{bmatrix}\\) that combination is \\(3\begin{bmatrix} 1 \\\ 1 \end{bmatrix}+5\begin{bmatrix} 2 \\\ 3 \end{bmatrix}=\begin{bmatrix} 3+10 \\\ 3+15 \end{bmatrix}=\begin{bmatrix} 13 \\ 18 \end{bmatrix}\\).
3. The vector \\(\begin{bmatrix} 2 \\\ 3 \end{bmatrix}=\begin{bmatrix} 2 \\\ 0 \end{bmatrix}+\begin{bmatrix} 0 \\\ 3 \end{bmatrix}\\) goes across to \\(x=2\\) and up to \\(y=3\\) in the \\(xy\\) plane.
4. The combinations \\(c\begin{bmatrix} 1 \\\ 1 \end{bmatrix}+d\begin{bmatrix} 2 \\\ 3 \end{bmatrix}\\) fill the whole \\(xy\\) place. They produce every \\(\begin{bmatrix} x \\\ y \end{bmatrix}\\).
5. The combinations \\(c\begin{bmatrix} 1 \\\ 1 \\\ 1 \end{bmatrix}+d\begin{bmatrix} 2 \\\ 3 \\\ 4 \end{bmatrix}\\) fill a **plane** in \\(xyz\\) space. Same plane for \\(\begin{bmatrix} 1 \\\ 1 \\\ 1 \end{bmatrix}\\), \\(\begin{bmatrix} 3 \\\ 4 \\\ 5 \end{bmatrix}\\).
6. But \\(\begin{array}{l} c+2d=1 \\\ c+3d=0 \\\ c+4d=0 \end{array}\\) has no solution because its right side \\(\begin{bmatrix} 1 \\\ 0 \\\ 0 \end{bmatrix}\\) is not on that plane.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">1.1 A</span>&emsp;The linear combinations of \(\bm{v}=(1,1,0)\) and \(\bm{w}=(0,1,1)\) fill a plane in \(\mathbf{R}^3\). <em>Describe that plane</em>. Find a vector that is <em>not</em> a combination of \(\bm{v}\) and \(\bm{w}\)&mdash;not on the plane.
      <br><br>
      <span class="list-head">Solution</span>&emsp;The plane of \(\bm{v}\) and \(\bm{w}\) contains all combinations \(c\bm{v}+d\bm{w}\). The vectors in that plane allow any \(c\) and \(d\). The plane of Figure 1.3 fills in between the two lines.
      <details>
        <summary>&dagger; Figure 1.3 &dagger;</summary>
        {% asset_img Figure1.3.png %}
      </details>
      <br>
      $$
      \text{Combinations}\quad
      c\bm{v}+d\bm{w}=c\begin{bmatrix} 1 \\ 1 \\ 0 \end{bmatrix}+d\begin{bmatrix} 0 \\ 1 \\ 1 \end{bmatrix}=\begin{bmatrix} c \\ c+d \\ d \end{bmatrix}
      \text{fill a plane.}
      $$
      Four vectors in that plane are \((0,0,0)\) and \((2,3,1)\) and \((5,7,2)\) and \((\pi,2\pi,\pi)\). The second component \(c+d\) is always the sum of the first and third components. Like most vectors, <em>\((1,2,3)\) is <strong>not</strong> in the plane, because \(2\neq 1+3\).</em>
      <br>
      &emsp;&ensp;Another description of this plane through \((0,0,0)\) is to know that \(\bm{n}=(1,-1,1)\) is <strong>perpendicular</strong> to the plane. Section 1.2 will confirm that 90&deg; angle by testing dot products: \(\bm{v}\cdot\bm{n}=0\) and \(\bm{w}\cdot\bm{n}=0\). Perpendicular vectors have zero dot products.
    </li>
    <li>
      <span class="list-head">1.1 B</span>&emsp;For \(\bm{v}=(1,0)\) and \(\bm{w}=(0,1)\), describe all points \(c\bm{v}\) with (<b>1</b>) <em>whole numbers</em> \(c\) (<b>2</b>) <em>nonnegative numbers</em> \(c\geq 0\). Then add all vectors \(d\bm{w}\) and describe all \(c\bm{v}+d\bm{w}\).
      <br><br>
      <span class="list-head">Solution</span>
      <br>
      <table class="list-table">
        <tr>
          <td>(<b>1</b>)&ensp;</td>
          <td>
            The vectors \(c\bm{v}=(c,0)\) with whole numbers \(c\) are <strong>equally spaced points</strong> along the \(x\) axis (the direction of \(\bm{v}\)). They include \((-2,0)\), \((-1,0)\), \((0,0)\), \((1,0)\), \((2,0)\).
          </td>
        </tr>
        <tr>
          <td>(<b>2</b>)&ensp;</td>
          <td>
            The vectors \(c\bm{v}\) with \(c\geq 0\) fill a <em><strong>half-line</strong></em>. It is the positive \(x\) axis. This half-line starts at \((0,0)\) where \(c=0\). It includes \((100,0)\) and \((\pi,0)\) but not \((-100,0)\).
          </td>
        </tr>
        <tr>
          <td>(<b>1&#x2B9;</b>)&ensp;</td>
          <td>
            Adding all vectors \(d\bm{w}=(0,d)\) puts a vertical line through those equally spaced \(c\bm{v}\). We have infinitely many <em><strong>parallel lines</strong></em> from (<em>whole number \(c\), any number \(d\)</em>).
          </td>
        </tr>
        <tr>
          <td>(<b>2&#x2B9;</b>)&ensp;</td>
          <td>
            Adding all vectors \(d\bm{w}\) puts a vertical line through every \(c\bm{v}\) on the half-line. Now we have a <em><strong>half-plane</strong></em>. The right half of the \(xy\) plane has any \(x\geq 0\) and any \(y\).
          </td>
        </tr>
      </table>
    </li>
    <li>
      <span class="list-head">1.1 C</span>&emsp;Find two equations for \(c\) and \(d\) so that <strong>the linear combination \(c\bm{v}+d\bm{w}\) equals \(\bm{b}\)</strong>:
      $$
      \bm{v}=\begin{bmatrix*}[r] 2 \\ -1 \end{bmatrix*}\qquad
      \bm{w}=\begin{bmatrix*}[r] -1 \\ 2 \end{bmatrix*}\qquad
      \bm{b}=\begin{bmatrix} 1 \\ 0 \end{bmatrix}
      \text{.}
      $$
      <span class="list-head">Solution</span>&emsp;In applying mathematics, many problems have two parts:
      <br>
      &emsp;<b>1</b>&ensp;<em>Modeling part</em>&emsp;Express the problem by a set of equations.
      <br>
      &emsp;<b>2</b>&ensp;<em>Computational part</em>&emsp;Solve those equations by a fast and accurate algorithm.
      <br>
      Here we are only asked for the first part (the equations). Chapter 2 is devoted to the second part (the solution). Our example fits into fundamental model for linear algebra:
      $$
      \text{Find $n$ numbers $c_1,\ldots,c_n$ so that $c_1\bm{v}_1+\cdots+c_n\bm{v}_n=\bm{b}$.}
      $$
      For \(n=2\) we will find a formula for the \(\bm{c}\)'s. The &ldquo;elimination method&rdquo; in Chapter 2 succeeds far beyond \(n=1000\). For \(n\) greater than 1 billion, see Chapter 11. Here \(n=2\):
      $$
      \colorbox{e8f1fe}{$
        \begin{array}{l}
          \textbf{Vector equation} \\
          c\bm{v}+d\bm{w}=\bm{b}
        \end{array}
        \qquad
        c\begin{bmatrix*}[r] 2 \\ -1 \end{bmatrix*}+d\begin{bmatrix*}[r] -1 \\ 2 \end{bmatrix*}=\begin{bmatrix} 1 \\ 0 \end{bmatrix}
      $}
      $$
      The required equations for \(c\) and \(d\) just come from the two components separately:
      $$
      \textcolor{RoyalBlue}{\textbf{Two ordinary equations}}\qquad
      \begin{alignedat}{2.5} % Using alignedat for no space between columns. For parameter there is +0.5 per column (pair of rl columns), columms align are col1=right, col2=left, col3=right...
        && 2c- && d=1 \\
        - && c+ && 2d=0
      \end{alignedat}
      $$
      Each equation produces a line. The two lines cross at the solution \(c=\dfrac{2}{3}\), \(d=\dfrac{1}{3}\). Why not see this also as a <strong>matrix equation</strong>, since that is where we are going:
      $$
      \textbf{2 by 2 matrix}\qquad
      \begin{bmatrix*}[r] \bm{2} & \bm{-1} \\ \bm{-1} & \bm{2} \end{bmatrix*}
      \begin{bmatrix} c \\ d \end{bmatrix}=\begin{bmatrix} 1 \\ 0 \end{bmatrix}
      \text{.}
      $$
    </li>
  </ol>
</details>

### Lengths and Dot Products

1. The "dot product" of \\(\bm{v}=\begin{bmatrix} 1 \\\ 2 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix} 4 \\\ 5 \end{bmatrix}\\) is \\(\bm{v}\cdot\bm{w}=(1)(4)+(2)(5)=4+10=\bm{14}\\).
2. \\(\bm{v}=\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) and \\(\bm{w}=\begin{bmatrix*}[r] 4 \\\ -4 \\\ 4 \end{bmatrix*}\\) are perpendicular because \\(\bm{v}\cdot\bm{w}\\) is zero: \\((1)(4)+(3)(-4)+(2)(4)=\bm{0}\\).
3. The length squared of \\(\bm{v}=\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) is \\(\bm{v}\cdot\bm{v}=1+9+4=14\\). **The length is** \\(\\|\bm{v}\\|=\sqrt{\bm{14}}\\).
4. Then \\(\bm{u}=\dfrac{\bm{v}}{\\|\bm{v}\\|}=\dfrac{\bm{v}}{\sqrt{14}}=\dfrac{1}{\sqrt{14}}\begin{bmatrix} 1 \\\ 3 \\\ 2 \end{bmatrix}\\) has length \\(\\|\bm{u}\\|=\bm{1}\\). Check \\(\dfrac{1}{14}+\dfrac{9}{14}+\dfrac{4}{14}=1\\).
5. The angle \\(\theta\\) between \\(\bm{v}\\) and \\(\bm{w}\\) has \\(\cos\theta=\dfrac{\bm{v}\cdot\bm{w}}{\\|\bm{v}\\|\\|\bm{w}\\|}\\).
6. The angle between \\(\begin{bmatrix} 1 \\\ 0 \end{bmatrix}\\) and \\(\begin{bmatrix} 1 \\\ 1\end{bmatrix}\\) has \\(\cos\theta=\dfrac{1}{(1)(\sqrt{2})}\\). That angle is \\(\theta=45\degree\\).
7. All angles have \\(|\cos\theta|\leq 1\\). So all vectors have \\(\boxed{|\bm{v}\cdot\bm{w}|\leq\\|\bm{v}\\|\\|\bm{w}\\|}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">1.2 A</span>&emsp;For the vectors \(\bm{v}=(3,4)\) and \(\bm{w}=(4,3)\) test the Schwarz inqeuality on \(\bm{v}\cdot\bm{w}\) and the triangle inqeuality on \(\|\bm{v}+\bm{w}\|\). Find \(\cos\theta\) for the angle between \(\bm{v}\) and \(\bm{w}\). Which \(\bm{v}\) and \(\bm{w}\) give <em>equality</em> \(\|\bm{v}\cdot\bm{w}\|=\|\bm{v}\|\|\bm{w}\|\) and \(\|\bm{v}+\bm{w}\|=\|\bm{w}\|+\|\bm{w}\|\)?
      <br><br>
      <span class="list-head">Solution</span>&emsp;The dot product is \(\bm{v}\cdot\bm{w}=(3)(4)+(4)(3)=24\). The length of \(\bm{v}\) is \(\|\bm{v}\|=\sqrt{9+16}=5\) and also \(\|\bm{w}\|=5\). The sum \(\bm{v}+\bm{w}=(7,7)\) has length \(7\sqrt{2}\lt 10\).
      $$
      \begin{array}{ll}
        \textbf{Schwarz inequality} & \text{$|\bm{v}\cdot\bm{w}|\leq\|\bm{v}\|\|\bm{w}\|$\quad is\quad$24\lt 25$.} \\
        \textbf{Triangle inequality} & \text{$\|\bm{v}+\bm{w}\|\leq\|\bm{v}\|+\|\bm{w}\|$\quad is\quad$7\sqrt{2}\lt 5+5$.} \\
        \textbf{Cosine of angle} & \text{$\cos\theta=\frac{24}{25}$\quad Thin angle from $\bm{v}=(3,4)$ to $\bm{w}=(4,3)$.}
      \end{array}
      $$
      <em>Equality:</em> One vector is a multiple of the other as in \(\bm{w}=c\bm{v}\). Then the angle is 0&deg; or 180&deg;. In this case \(|\cos\theta|=1\) and \(|\bm{v}\cdot\bm{w}|\) <em>equals</em> \(\|\bm{v}\|\|\bm{w}\|\). If the angle is 0&deg;, as in \(\bm{w}=2\bm{v}\), then \(\|\bm{v}+\bm{w}\|=\|\bm{v}\|+\|\bm{w}\|\) (both sides give \(3\|\bm{v}\|\)). This \(\bm{v}\), \(2\bm{v}\), \(3\bm{v}\) triangle is flat!
    </li>
    <li>
      <span class="list-head">1.2 B</span>&emsp;Find a unit vector \(\bm{u}\) in the direction of \(\bm{v}=(3,4)\). Find a unit vector \(\bm{U}\) that is perpendicular to \(\bm{u}\). How many possibilities for \(\bm{U}\)?
      <br><br>
      <span class="list-head">Solution</span>&emsp;For a unit vector \(\bm{u}\), divide \(\bm{v}\) by its length \(\|\bm{v}\|=5\). For a perpendicular vector \(\bm{V}\) we can choose \((-4,3)\) since the dot product \(\bm{v}\cdot\bm{V}\) is \((3)(-4)+(4)(3)=0\). For a <em>unit</em> vector perpendicular to \(\bm{u}\), divide \(\bm{V}\) by its length \(\|\bm{V}\|\):
      $$
        \bm{u}=\frac{\bm{v}}{\|\bm{v}\|}=\left(\frac{3}{5},\frac{4}{5}\right)\qquad
        \bm{U}=\frac{\bm{V}}{\|\bm{V}\|}=\left(-\frac{4}{5},\frac{3}{5}\right)\qquad
        \bm{u}\cdot\bm{U}=0
      $$
      The only other perpendicular unit vector would be \(-\bm{U}=(\frac{4}{5},-\frac{3}{5})\).
    </li>
    <li>
      <span class="list-head">1.2 C</span>&emsp;Find a vector \(\bm{x}=(c,d)\) that has dot products \(\bm{x}\cdot\bm{r}=1\) and \(\bm{x}\cdot\bm{s}=0\) with two given vectors \(\bm{r}=(2,-1)\) and \(\bm{s}=(-1,2)\).
      <br><br>
      <span class="list-head">Solution</span>&emsp;Those two dot products give linear equations for \(c\) and \(d\). Then \(\bm{x}=(c,d)\).
      $$
      \begin{alignedat}{3}
        \bm{x}\cdot\bm{r}=1\qquad\text{is}\qquad &  & 2c- && d=1\qquad & \textbf{The same equations as}  \\
        \bm{x}\cdot\bm{s}=0\qquad\text{is}\qquad & - & c+ && 2d=0\qquad & \textbf{in Worked Example 1.1 C}
      \end{alignedat}
      $$
      &emsp;&ensp;<em>Comment on n equations for \(x=(x_1,\ldots,x_n)\) in n-dimensional space</em>
      <br>
      Section 1.1 would start with columns \(\bm{v}_j\). The goal is to produce \(x_1\bm{v}_1+\cdots+x_n\bm{v}_n=\bm{b}\). This section would start from rows \(\bm{r}_i\). Now the goal is to find \(\bm{x}\) with \(\bm{x}\cdot\bm{r}_i=b_i\).
      <br>
      &emsp;&ensp;Soon the \(\bm{v}\)'s will be columns of a matrix \(A\), and the \(\bm{r}\)'s will be the rows of \(A\). Then the (one and only) problem will be to solve \(A\bm{x}=\bm{b}\).
    </li>
  </ol>
</details>

### Matrices

1. \\(A=\begin{bmatrix} 1 & 2 \\\ 3 & 4 \\\ 5 & 6 \end{bmatrix}\\) is a 3 by 2 matrix : \\(m=3\\) rows and \\(n=2\\) columns.
2. \\(A\bm{x}=\begin{bmatrix} 1 & 2 \\\ 3 & 4 \\\ 5 & 6 \end{bmatrix}\begin{bmatrix} x_1 \\\ x_2 \end{bmatrix}\\) is **a combination of the columns**&emsp;\\(A\bm{x}=x_1\begin{bmatrix} 1 \\\ 3 \\\ 5 \end{bmatrix}+x_2\begin{bmatrix} 2 \\\ 4 \\\ 6 \end{bmatrix}\\).
3. The 3 components of \\(A\bm{x}\\) are dot products of the 3 rows of \\(A\\) with the vector \\(\bm{x}\\):
   <div>
   $$
   \textbf{Row at a time}\qquad
   \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}\begin{bmatrix} 7 \\ 8 \end{bmatrix}
   =
   \begin{bmatrix} 1\cdot 7+2\cdot 8 \\ 3\cdot 7+4\cdot 8 \\ 5\cdot 7+6\cdot 8 \end{bmatrix}
   =
   \begin{bmatrix} 23 \\ 53 \\ 83 \end{bmatrix}
   \text{.}
   $$
   </div>
4. Equations in matrix form \\(A\bm{x}=\bm{b}\\): \\(\begin{bmatrix} 2 & 5 \\\ 3 & 7 \end{bmatrix}\begin{bmatrix} x_1 \\\ x_2 \end{bmatrix}=\begin{bmatrix} b_1 \\\ b_2 \end{bmatrix}\\) replaces \\(\begin{array}{l} 2x_1+5x_2=b_1 \\\ 3x_1+7x_2=b_2 \end{array}\\).
5. The solution to \\(A\bm{x}=\bm{b}\\) can be written as \\(\bm{x}=A^{-1}\bm{b}\\). But some matrices don't allow \\(A^{-1}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">1.3 A</span>&emsp;Change the southwest entry \(a_{31}\) of \(A\) (row 3, column 1) to \(a_{31}=\bm{1}\):
      $$
      A\bm{x}=\bm{b}\qquad
      \begin{bmatrix*}[r] 1 & 0 & 0 \\ -1 & 1 & 0 \\ \bm{1} & -1 & 1 \end{bmatrix*}\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}
      =
      \left[\begin{alignedat}{1}
          & x_1 \\
        - & x_1+x_2 \\
          & \bm{x_1}-x_2+x_3
      \end{alignedat}\right]
      =
      \begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}
      \text{.}
      $$
      <strong>Find the solution \(\bm{x}\) for any \(\bm{b}\). From \(\bm{x}=A^{-1}\bm{b}\) read off the inverse matrix \(A^{-1}\).</strong>
      <br><br>
      <span class="list-head">Solution</span>&emsp;Solve the (linear triangular) system \(A\bm{x}=\bm{b}\) from top to bottom:
      $$
      \begin{alignedat}{3}
        & \text{first}\quad && x_1=b_1  && \\
        & \text{then}\quad  && x_2=b_1+ && b_2 \\
        & \text{then}\quad  && x_3=     && b_2+b_3
      \end{alignedat}
      \text{
        This says that $\bm{x}=A^{-1}\bm{b}=
        \begin{bmatrix} 1 & 0 & 0 \\ 1 & 1 & 0 \\ 0 & 1 & 1 \end{bmatrix}\begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}$
        .
      }
      $$
      This is good practice to see the columns of the inverse matrix multiplying \(b_1\), \(b_2\), and \(b_3\). The first column of \(A^{-1}\) is the solution for \(\bm{b}=(1,0,0)\). The second column is the solution for \(\bm{b}=(0,1,0)\). The third column \(\bm{x}\) of \(A^{-1}\) is the solution for \(A\bm{x}=\bm{b}=(0,0,1)\).
      <br>
      &emsp;&ensp;The three columns of \(A\) are still independent. They don't lie in a plane. The combinations of those three columns, using the right weights \(x_1\), \(x_2\), \(x_3\), can produce any three-dimensional vector \(\bm{b}=(b_1,b_2,b_3)\). Those weights come from \(\bm{x}=A^{-1}\bm{b}\).
    </li>
    <li>
      <span class="list-head">1.3 B</span>&emsp;This \(E\) is an <strong>elimination matrix</strong>. \(E\) has a subtraction and \(E^{-1}\) has an addition.
      $$ % Sorry for the type, will reformat as soon as KaTex support multicolumn.
      \bm{b}=E\bm{x}\qquad
      \begin{bmatrix} b_1 \\ b_2 \end{bmatrix}
      =
      \begin{bmatrix} x_1 \\ x_2-\ell x_1 \end{bmatrix}
      =
      \begin{bmatrix*}[r] \bm{1} & 0 \\ -\bm{\ell} & \bm{1} \end{bmatrix*}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}\qquad
      E=\begin{bmatrix*}[r] \bm{1} & 0 \\ -\bm{\ell} & \bm{1} \end{bmatrix*}
      $$
      The first equation is \(x_1=b_1\). The second equation is \(x_2-\ell x_1=b_2\). The inverse will <em>add</em> \(\ell b_1\) to \(b_2\), because the elimination matrix <em>subtracted</em> :
      $$
      \bm{x}=E^{-1}\bm{b}\qquad
      \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}
      =
      \left[\begin{alignedat}{1}
             & b_1 \\
        \ell & b_1+b_2
      \end{alignedat}\right]
      =
      \begin{bmatrix} \bm{1} & 0 \\ \bm{\ell}  & \bm{1} \end{bmatrix}\begin{bmatrix} b_1 \\ b_2 \end{bmatrix}\qquad
      E^{-1}=\begin{bmatrix} \bm{1} & 0 \\ \bm{\ell}  & \bm{1} \end{bmatrix}
      $$
    </li>
    <li>
      <span class="list-head">1.3 C</span>&emsp;Change \(C\) from a cyclic difference to a <strong>centered difference</strong> producing \(x_3-x_1\):
      $$
      \begin{equation}
        C\bm{x}=\bm{b}\qquad
        \begin{bmatrix*}[r] 0 & 1 & 0 \\ -1 & 0 & 1 \\ 0 & -1 & 0 \end{bmatrix*}\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}
        =
        \left[\begin{alignedat}{1}
          x_2 & -0 \\
          x_3 & -x_1 \\
          0   & -x_2
        \end{alignedat}\right]
        =
        \begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}
        \text{.}
      \end{equation}
      $$
      \(C\bm{x}=\bm{b}\) can only be solved when \(b_1+b_3=x_2-x_2=0\). That is a plane of vectors \(\bm{b}\) in three-dimensional space. Each column of \(C\) is in the plane, the matrix has no inverse. So this plane conatins all combinations of those columns (which are all the vectors \(C\bm{x}\)).
      <br>
      &emsp;&ensp;I included the zeros so you could see that this \(C\) produces &ldquo;centered differences&rdquo;. Row \(i\) of \(C\bm{x}\) is \(x_{i+1}\) (<em>right of center</em>) minus \(x_{i-1}\) (<em>left of center</em>). Here is 4 by 4:
      $$
      \begin{equation}
        \begin{array}{l}
          C\bm{x}=\bm{b} \\
          \textbf{Centered} \\
          \textbf{differences}
        \end{array}\qquad
        \begin{bmatrix*}[r]
         0 & 1 & 0 & 0 \\
         -1 & 0 & 1 & 0 \\
         0 & -1 & 0 & 1 \\
         0 & 0 & -1 & 0
       \end{bmatrix*}
       \begin{bmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{bmatrix}
       =
       \left[\begin{alignedat}{1}
         x_2 & -0 \\
         x_3 & -x_1 \\
         x_4 & -x_2 \\
         0   & -x_3
       \end{alignedat}\right]
       =
       \begin{bmatrix} b_1 \\ b_2 \\ b_3 \\ b_4 \end{bmatrix}
      \end{equation}
      $$
      Surprisingly this matrix is now invertible! The first and last rows tell you \(x_2\) and \(x_3\). Then the middle rows give \(x_1\) and \(x_4\). It is possible to write down the inverse matrix \(C^{-1}\). But 5 by 5 will be singular (<em>not invertible</em>) again&mldr;
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
      <span class="list-head">2.1 A</span>&emsp;Describe the column picture of these three equations \(A\bm{x}=\bm{b}\). Solve by careful inspection of the columns (instead of elimination):
      $$
      \begin{array}{r} x+3y+2z=-3 \\ 2x+2y+2z=-2 \\ 3x+5y+6z=-5 \end{array}\qquad\text{which is}\qquad\begin{bmatrix} 1 & \bm{3} & 2 \\ 2 & \bm{2} & 2 \\ 3 & \bm{5} & 6 \end{bmatrix}\begin{bmatrix} x \\ y \\ z \end{bmatrix}=\begin{bmatrix*}[r] -3 \\ -2 \\ -5 \end{bmatrix*}
      $$
      <span class="list-head">Solution</span>&emsp;The column picture asks for a linear combination that produces \(\bm{b}\) from the three columns of \(A\). In this example \(\bm{b}\) is <em>minus the second column</em>. So the solution is \(x=0\), \(y=-1\), \(z=0\). To show that \((0,-1,0)\) is the <em>only</em> solution we have to know that &ldquo;\(A\) is invertible&rdquo; and &ldquo;the columns are independent&rdquo; and &ldquo;the determinant isn't zero.&rdquo;
      <br>
      &emsp;&ensp;Those words are not yet defined but the test comes from elimination: We need (and for this matrix we find) a full set of three nonzero pivots.
      <br>
      &emsp;&ensp;Suppose the right side changes to \(\bm{b}=(4,4,8)=\) sum of the first two columns. Then the good combination has \(x=1\), \(y=1\), \(z=0\). The solution becomes \(\bm{x}=(1,1,0)\).
    </li>
    <li>
      <span class="list-head">2.1 B</span>&emsp;This system has <em>no solution</em>. The planes in the row picture don't meet at a point.
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
      <span class="list-head">Solution</span>&emsp;
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
      <span class="list-head">2.2 A</span>&emsp;When elimination is applied to this matrix \(A\), what are the first and second pivots? What is the multiplier \(\ell_{21}\) in the first step (\(\ell_{21}\) times row 1 is <em>subtracted</em> from row 2)?
      $$
      A=\begin{bmatrix} 1 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & 2 \end{bmatrix}\longrightarrow\begin{bmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 1 & 2 \end{bmatrix}\longrightarrow\begin{bmatrix} 1 & 1 & 0 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix}=U
      $$
      What entry in the 2,2 position (instead of 2) would force an exchange of rows 2 and 3? Why is the lower left multiplier \(\ell_{31}=0\), subtracting zero times row 1 from row 3?
      <br>
      <em><strong>If you change the corner entry from \(a_{33}=2\) to \(a_{33}=1\), why does elimination fail?</strong></em>
      <br><br>
      <span class="list-head">Solution</span>&emsp;The first pivot is 1. The multiplier \(\ell_{21}\) is 1,1. When 1 times row 1 is subtracted from row 2, the second pivot is revealed as another 1. If the original middle entry had been 1 instead of 2, that would have forced a row exchange.
      &emsp;&ensp;The multiplier \(\ell_{31}\) is zero because \(a_{31}=0\). A zero at the start of a row needs no elimination. This \(A\) is a "<em>band matrix</em>". Everything stays zero outside the band.
      <br>
      &emsp;&ensp;The last pivot is also 1. So if the original corner \(a_{33}=2\) reduced by 1, elimination would produce 0. <strong>No third pivot, elimination fails.</strong>
    </li>
    <li>
      <span class="list-head">2.2 B</span>&emsp;Suppose \(A\) is already a <em><strong>triangular matrix</strong></em> (upper triangular or lower triangular). <em>Where do you see its pivots?</em> When does \(A\bm{x}=\bm{b}\) have exactly one solution for every \(\bm{b}\)?
      <br><br>
      <span class="list-head">Solution</span>&emsp;The pivots of a triangular matrix are already set along the main diagonal. <em>Elimination succeeds when all those numbers are nonzero.</em> Use <em><strong>back</strong></em> substitution when \(A\) is upper triangular, go <em><strong>forward</strong></em> when \(A\) is lower triangular.
    </li>
    <li>
      <span class="list-head">2.2 C</span>&emsp;Use elimination to reach upper triangular matrices \(U\). Solve by back substitution or explain why this is impossible. What are the pivots (never zero)? Exchange equations when necessary. The only difference is the \(-x\) in the last equation.
      $$
      \begin{array}{rrrr}
        \textbf{Success}\quad & x+y+z=7 & \qquad\textbf{Failure}\quad & x+y+z=7 \\
        & x+y-z=5 & & x+y-z=5 \\
        & x-y+z=3 & & -x-y+z=3
      \end{array}
      $$
      <span class="list-head">Solution</span>&emsp;For the first system, subtract equation 1 from equations 2 and 3 (the multipliers are \(\ell_{21}=1\) and \(\ell_{31}=1\)). The 2,2 entry becomes zero, so exchange equations 2 and 3 :
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
      <span class="list-head">2.3 A</span>&emsp;What 3 by 3 matrix \(E_{21}\) subtracts 4 times row 1 from row 2? What matrix \(P_{32}\) exchanges row 2 and row 3? If you multiply \(A\) on the <em>right</em> instead of the left, describe the results \(AE_{21}\) and \(AP_{32}\).
      <br><br>
      <span class="list-head">Solution</span>&emsp;By doing those operations on the identity matrix \(I\), we find
      $$
      E_{21}=\begin{bmatrix*}[r] 1 & 0 & 0 \\ -4 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix*}
      \qquad\text{and}\qquad
      P_{32}=\begin{bmatrix} 1 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 1 & 0 \end{bmatrix}
      $$
      Multiplying by \(E_{21}\) on the right side will subtract 4 times <strong>column 2</strong> from <strong>column 1</strong>. Multiplying by \(P_{32}\) on the right will exchange <strong>columns 2</strong> and <strong>3</strong>.
    </li>
    <li>
      <span class="list-head">2.3 B</span>&emsp;Write down the augmented matrix \(\begin{bmatrix} A & \bm{b} \end{bmatrix}\) with an extra column:
      $$
      \begin{array}{r}
        x+2y+2z=1 \\
        4x+8y+9z=3 \\
        3y+2z=1
      \end{array}
      $$
      Apply \(E_{21}\) and then \(P_{32}\) to reach a triangular system. Solve by back substitution. What combined matrix \(P_{32}E_{21}\) will do both steps at once?
      <br><br>
      <span class="list-head">Solution</span>&emsp;\(E_{21}\) removes the 4 in column 1. But zero also appears in column 2:
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
      <span class="list-head">2.3 C</span>&emsp;Multiply these matrices in two ways. First, rows of \(A\) times columns of \(B\). Second, <em><strong>columns of \(\bm{A}\) times rows of \(\bm{B}\)</strong></em>. That unusual way produces two matrices that add to \(AB\). How many separate ordinary multiplications are needed?
      $$
      \textbf{Both ways}\qquad
      AB=\begin{bmatrix} 3 & 4 \\ 1 & 5 \\ 2 & 0 \end{bmatrix}\begin{bmatrix} 2 & 4 \\ 1 & 1 \end{bmatrix}=\begin{bmatrix*}[r] \bm{10} & \bm{16} \\ \bm{7} & \bm{9} \\ \bm{4} & \bm{8} \end{bmatrix*}
      $$
      <span class="list-head">Solution</span>&emsp;Rows of \(A\) times columns of \(B\) are dot products of vectors:
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
      <span class="list-head">2.4 A</span>&emsp;A graph or a network has \(n\) nodes. Its <strong>adjacency matrix</strong> \(S\) is \(n\) by \(n\). This is a 0&ndash;1 matrix with \(s_{ij}=1\) when nodes \(i\) and \(j\) are connected by an edge.
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
      <span class="list-head">2.4 B</span>&emsp;For these matrices, when does \(AB=BA\)? When does \(BC=CB\)? When does \(A\) times \(BC\) equal \(AB\) times \(C\)? Give the conditions on their entries \(p\), \(q\), \(r\), \(z\) :
      $$
      A=\begin{bmatrix} p & 0 \\ q & r \end{bmatrix}
      \qquad
      B=\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}
      \qquad
      C=\begin{bmatrix} 0 & z \\ 0 & 0 \end{bmatrix}
      $$
      If \(p\), \(q\), \(r\), \(1\), \(z\) are 4 by 4 blocks instead of numbers, do the answers change?
      <br><br>
      <span class="list-head">Solution</span>&emsp;First of all, \(A\) times \(BC\) <em>always</em> equals \(AB\) times \(C\). Parentheses are not needed in \(A(BC)=(AB)C=ABC\). But we must keep the matrices in this order :
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
      <span class="list-head">2.5 A</span>&emsp;The inverse of a triangular <strong>difference matrix</strong> \(A\) is a triangular <strong>sum matrix</strong> \(S\) :
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
      <span class="list-head">2.5 B</span>&emsp;Three of these matrices are invertible, and three are singular. Find the inverse when it exists. Give reasons for noninvertibility (zero determinant, too few pivots, nonzero solution to \(A\bm{x}=\bm{0}\)) for the other three. The matrices are in the order \(A\), \(B\), \(C\), \(D\), \(S\), \(E\) :
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
      <span class="list-head">Solution</span>
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
      <span class="list-head">2.5 C</span>&emsp;Apply the Gauss-Jordan method to invert this triangular &ldquo;Pascal matrix&rdquo; \(L\). You see <strong>Pascal's triangle</strong>&mdash;adding each entry to the entry on its left gives the entry below. The entries of \(L\) are &ldquo;binomial coefficients&rdquo;. The next row would be \(1,4,6,4,1\).
      $$
      \textbf{Triangular Pascal matrix}\quad
      \bm{L}=
      \begin{bmatrix}
        \bm{1} & 0 & 0 & 0 \\
        \bm{1} & \bm{1} & 0 & 0 \\
        \bm{1} & \bm{2} & \bm{1} & 0 \\
        \bm{1} & \bm{3} & \bm{3} & \bm{1}
      \end{bmatrix}
      =\textsf{abs}(\textsf{pascal}(4,1))
      $$
      <span class="list-head">Solution</span>&emsp;Gausss-Jordan starts with \(\begin{bmatrix} L & I \end{bmatrix}\) and produces zeros by subtracting row 1:
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
      \text{.}
      $$
      All the pivots were 1! So we didn't need to divide rows by pivots to get \(I\). The inverse matrix \(L^{-1}\) look like \(L\) itself, except odd-numbered diagonals have minus signs.
      <br>
      &emsp;&ensp;The same pattern continues to \(n\) by \(n\) Pascal matrices. \(L^{-1}\) has &ldquo;alternating diagonals&rdquo;.
    </li>
  </ol>
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
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
       In elimination, to find new entries below the first pivot row requires \(n^2-n\) multiplications and subtractions. Here we using \(n^2\) and for simplicity ignore that the row 1 is actually does not change (In CS this is the usual case&mdash;focus on most affected factors in time complexicity). The next stage will has \((n-1)^n\) multiplications and subtractions. The matrices are getting smaller as elimination goes forward. The rough count to reach \(U\) is the sum of squares:
       $$
       n^2+(n-1)^2+\cdots+2^2+1^2=\frac{1}{3}n(n+\frac{1}{2})(n+1)\approx\frac{1}{3}n^3
       $$
       (When \(n\) is large, the \(\frac{1}{2}\) and the \(1\) are not important).
       <br>
       &emsp;&ensp;For solving a triangular system, in back substitution, the routh count of the multiplications:
       $$
         1+2+\cdots+n=\frac{(1+n)n}{2}\approx\frac{n^2}{2}
       $$
       and the routh count of the subtractions:
       $$
         0+1+\cdots+(n-1)=\frac{(n-1)n}{2}\approx\frac{n^2}{2}
       $$
   </details>

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">2.6 A</span>&emsp;The lower triangular Pascal matrix \(L\) contains the famous <em>&ldquo;Pascal triangle&rdquo;</em>. Gauss-Jordan inverted \(L\) in the worked example <span class="list-head">2.5 C</span>. Here we factor Pascal.
      <br>
      &emsp;&ensp;<strong>The symmetric Pascal matrix \(\bm{P}\) is a product of triangular Pascal matrices \(L\) and \(U\).</strong> The symmetric \(P\) has Pascal's triangle tilted, so each entry is the sum of entry above and the entry to the left. The \(n\) by \(n\) symmetric \(P\) is \(\textsf{pascal}(n)\) in \(\textsf{MATLAB}\).
      <br>
      <strong>Problem:</strong> <em>Establish the amazing lower-upper factorization \(P=LU\).</em>
      $$
      \textsf{pascal}(4)=
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
      <span class="list-head">Solution</span>&emsp;You could multiply \(LU\) to get \(P\). Better to start with the symmetric \(P\) and reach the upper triangular \(U\) by elimination :
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
      &emsp;&ensp;You might expect the \(\textsf{MATLAB}\) command \(\textsf{lu}(\textsf{pascal}(4))\) to produce these \(L\) and \(U\). That doesn't happen because \(\textbf{\textsf{lu}}\) subroutine chooses the largest available pivot in each column. The second pivot will change from 1 to 3. But a &ldquo;Cholesky factorization&rdquo; does no row exchanges: \(U=\textsf{chol}(\textsf{pascal}(4))\)
      <br>
      &emsp;&ensp;The full proof of \(P=LU\) for all Pascal sizes is quite fascinating. The paper &ldquo;Pascal Matrices&rdquo; is on the course web page <strong>web.mit.edu/18.06</strong> which is also available through MIT's <em>OpenCourseWare</em> at <strong>ocw.mit.edu</strong>. These Pascal matrices have so many remarkable properties&mdash;we will see them again.
    </li>
    <li>
      <span class="list-head">2.6 B</span>&emsp;The problem is: <em>Solve \(P\bm{x}=\bm{b}=(1,0,0,0)\).</em> This right side \(=\) column of \(I\) means that \(\bm{x}\) will be the first column of \(P^{-1}\). That is Gauss-Jordan, matching the columns of \(PP^{-1}=I\). We already know the Pascal matrices \(L\) and \(U\) as factors of \(P\):
      $$
      \textbf{Two triangular systems}\qquad L\bm{c}=\bm{b}\text{ (forward)}\qquad U\bm{x}=\bm{c}\text{ (back).}
      $$
      <span class="list-head">Solution</span>&emsp;The lower triangular system \(L\bm{c}=\bm{b}\) is solved <em>top to bottum</em>:
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
      I see a pattern in that \(\bm{x}\), but I don't know where it comes from. Try \(\textsf{inv}(\textsf{pascal}(4))\).
    </li>
  </ol>
</details>

### Transposes and Permutations

1. The transposes of \\(A\bm{x}\\) and \\(AB\\) and \\(A^{-1}\\) are \\(\bm{x}^\mathrm{T}A^\mathrm{T}\\) and \\(B^\mathrm{T}A^\mathrm{T}\\) and \\((A^\mathrm{T})^{-1}\\).
   <details>
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
       To understand \((AB)^\mathrm{T}=B^\mathrm{T}A^\mathrm{T}\), start with \((A\bm{x})^\mathrm{T})=\bm{x}^\mathrm{T}A^\mathrm{T}\) when \(B\) is just a vector:
       $$
       \textcolor{RoyalBlue}{
         \textit{\textbf{$A\bm{x}$ combines the columns of $A$ while $\bm{x}^\mathrm{T}A^\mathrm{T}$ combines the rows of $A^\mathrm{T}$}}
       }
       $$
       In algebra language, this is:
       $$
       \begin{alignedat}{1}
         (A\bm{x})^\mathrm{T}
       & =
         \left(
           \begin{bmatrix} \bm{a}_1 & \bm{a}_2 & \cdots & \bm{a}_n \end{bmatrix}
           \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_n \end{bmatrix}
         \right)^\mathrm{T}
       \\
       & =(x_1\bm{a}_1+x_2\bm{a}_2+\cdots+x_n\bm{a}_n)^\mathrm{T}
       \\
       & =x_1\bm{a}_1^\mathrm{T}+x_2\bm{a}_2^\mathrm{T}+\cdots+x_n\bm{a}_n^\mathrm{T}
       \\
         \bm{x}^\mathrm{T}A^\mathrm{T}
       & =
         \begin{bmatrix} x_1 & x_2 & \cdots & x_n \end{bmatrix}
         \begin{bmatrix} \bm{a}_1^\mathrm{T} \\ \bm{a}_2^\mathrm{T} \\ \vdots \\ \bm{a}_n^\mathrm{T} \end{bmatrix}
       \\
       & =x_1\bm{a}_1^\mathrm{T}+x_2\bm{a}_2^\mathrm{T}+\cdots+x_n\bm{a}_n^\mathrm{T}
       \\
       & \therefore (A\bm{x})^\mathrm{T}=\bm{x}^\mathrm{T}A^\mathrm{T}
       \end{alignedat}
       $$
       Now we can prove the formula \((AB)^\mathrm{T}=B^\mathrm{T}A^\mathrm{T}\), let \(B=\begin{bmatrix} \bm{b}_1 & \bm{b}_2 & \bm{b}_n \end{bmatrix}\):
       $$
       (AB)^\mathrm{T}=
       \left(\begin{bmatrix} A\bm{b}_1 & A\bm{b}_2 & \cdots & A\bm{b}_n \end{bmatrix}\right)^\mathrm{T}
       =
       \begin{bmatrix} \bm{b}_1^\mathrm{T}A^\mathrm{T} \\ \bm{b}_2^\mathrm{T}A^\mathrm{T} \\ \vdots \\ \bm{b}_n^\mathrm{T}A^\mathrm{T} \end{bmatrix}
       =
       \begin{bmatrix} \bm{b}_1^\mathrm{T} \\ \bm{b}_2^\mathrm{T} \\ \vdots \\ \bm{b}_n^\mathrm{T} \end{bmatrix}A^\mathrm{T}
       =B^\mathrm{T}A^\mathrm{T}
       $$
       Apply this rule by transposing both sides of \(A^\mathrm{-1}A=I\):
       $$
       (A^{-1}A)^\mathrm{T}=I^\mathrm{T}
       \harr
       A^\mathrm{T}(A^{-1})^\mathrm{T}=I
       \harr
       (A^{-1})^\mathrm{T}=(A^\mathrm{T})^{-1}
       $$
   </details>
2. The dot product (inner product) is \\(\bm{x}\cdot\bm{y}=\bm{x}^\mathrm{T}\bm{y}\\). This is \\((1\times n)(n\times 1)=(1\times 1)\\).
   The outer product is \\(\bm{xy^\mathrm{T}}=\text{column times row}=(n\times 1)(1\times n)=n\times n\text{ matrix}\\).
3. The idea behind \\(A^\mathrm{T}\\) is that \\(A\bm{x}\cdot\bm{y}\\) equals \\(\bm{x}\cdot A^\mathrm{T}\bm{y}\\) because \\((A\bm{x})^\mathrm{T}\bm{y}=\bm{x}^\mathrm{T}A^\mathrm{T}\bm{y}=\bm{x}^\mathrm{T}(A^\mathrm{T}\bm{y})\\).
4. A <strong>symmetric matrix</strong> has \\(\bm{S^\mathrm{T}=S}\\) (and the product \\(A^\mathrm{T}A\\) is always symmetric).
5. An <strong>orthogonal matrix</strong> has \\(\bm{Q^\mathrm{T}=Q^{-1}}\\). The columns of \\(Q\\) are orthogonal unit vectors.
6. A <strong>permutation matrix \\(\bm{P}\\)</strong> has the same rows as \\(I\\) (in any order). There are \\(\bm{n!}\\) different orders.
7. Then \\(P\bm{x}\\) puts the components \\(x_1\\), \\(x_2\\), \\(\ldots\\), \\(x_n\\) in that new order. And \\(P^\mathrm{T}\\) equals \\(P^{-1}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">2.7 A</span>&emsp;Applying the permutation \(P\) to the rows of \(S\) destorys its symmetry:
      $$
      P=\begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ 1 & 0 & 0 \end{bmatrix}
      \qquad
      S=\begin{bmatrix} 1 & 4 & 5 \\ 4 & \bm{2} & 6 \\ 5 & 6 & \bm{3} \end{bmatrix}
      \qquad
      PS=\begin{bmatrix} 4 & \bm{2} & 6 \\ 5 & 6 & \bm{3} \\ 1 & 4 & 5 \end{bmatrix}
      $$
      What permutation \(Q\) applied to the <em>columns</em> of \(PS\) will recover symmetry in \(PSQ\)? The numbers \(1\), \(2\), \(3\) must come back to the main diagonal (not necessarily in order). Show that \(Q\) is \(P^\mathrm{T}\), so that <strong>symmetry is saved by</strong> \(PSP^\mathrm{T}\).
      <br><br>
      <span class="list-head">Solution</span>&emsp;To recover symmetry and put &ldquo;2&rdquo; back on the diagonal, column 2 of \(PS\) must move to column 1. Column 3 of \(PS\) (containing &ldquo;3&rdquo;) must move to column 2. Then the &ldquo;1&rdquo; moves to the 3,3 position. The matrix that permutes columns is \(Q\):
      $$
      PS=\begin{bmatrix} 4 & \bm{2} & 6 \\ 5 & 6 & \bm{3} \\ 1 & 4 & 5 \end{bmatrix}
      \qquad
      Q=\begin{bmatrix} 0 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{bmatrix}
      \qquad
      PSQ=\begin{bmatrix} \bm{2} & 6 & 4 \\ 6 & \bm{3} & 5 \\ 4 & 5 & 1 \end{bmatrix}
      \text{is symmetric.}
      $$
      <em><strong>The matrix \(\bm{Q}\) is \(\bm{P^\textbf{\textrm{T}}}\).</strong></em> This choice always recovers symmetry, because \(PSP^\mathrm{T}\) is guaranteed to be symmetric. (Its transpose is again \(PSP^\mathrm{T}\).) <em>The matrix \(Q\) is also \(P^{-1}\), because the inverse of every permutation matrix is its transpose.</em>
      <br>
      If \(D\) is a diagonal matrix, we are finding that \(PDP^\mathrm{T}\) is also diagonal. When \(P\) moves row 1 down to row 3, \(P^\mathrm{T}\) on the right will move column 1 to column 3. The (1,1) entry moves down to (3,1) and over to (3,3).
    </li>
    <li>
      <span class="list-head">2.7 B</span>&emsp;Find the symmetric factorization \(S=LDL^\mathrm{T}\) for the matrix \(S\) above.
      <span class="list-head">Solution</span>&emsp;To factor \(S\) into \(LDL^{-1}\) we eliminate as usual to reach \(U\):
      $$
      S=
      \begin{bmatrix} 1 & 4 & 5 \\ 4 & 2 & 6 \\ 5 & 6 & 3 \end{bmatrix}
      \longrightarrow
      \begin{bmatrix} 1 & 4 & 5 \\ 0 & -14 & -14 \\ 0 & -14 & -22 \end{bmatrix}
      \longrightarrow
      \begin{bmatrix} 1 & 4 & 5 \\ 0 & -14 & -14 \\ 0 & 0 & -8 \end{bmatrix}
      =U
      $$
      The multipliers were \(\ell_{21}=4\) and \(\ell_{31}=5\) and \(\ell_{32}=1\). <strong>The pivots 1, &minus;14, &minus;8 go into \(\bm{D}\).</strong> When we divide the rows of \(U\) by those pivots, \(L^\mathrm{T}\) should appear:
      $$
      \begin{array}{l}
        \textbf{Symmetric} \\
        \textbf{factorization} \\
        \textbf{when $\bm{S=S^\textrm{T}}$}
      \end{array}
      \qquad
      \bm{S}=\bm{LDL^\mathrm{T}}=
      \begin{bmatrix} 1 & 0 & 0 \\ 4 & 1 & 0 \\ 5 & 1 & 1 \end{bmatrix}
      \begin{bmatrix} 1 & & \\ & -14 & \\ & & -8 \end{bmatrix}
      \begin{bmatrix} 1 & 4 & 5 \\ 0 & 1 & 1 \\ 0 & 0 & 1 \end{bmatrix}
      \text{.}
      $$
      This matrix \(S\) is invertible because <em>it has three pivots</em>. Its inverse is \((L^\mathrm{T})^{-1}D^{-1}L^{-1}\) and \(S^{-1}\) is also symmetric. The numbers 14 and 8 will turn up in the denominators of \(S^{-1}\). The &ldquo;determinant&rdquo; of \(S\) is the product of the pivots \((1)(-14)(-8)=122\).
    </li>
    <li>
      <span class="list-head">2.7 C</span>&emsp;For a rectangular \(A\), this <em><strong>saddle-point matrix \(\bm{S}\)</strong></em> is symmetric and important:
      $$
      \begin{array}{l}
        \textbf{Block matrix} \\
        \textbf{from least squares}
      \end{array}
      \qquad
      \bm{S}=\begin{bmatrix} \bm{I} & \bm{A} \\ \bm{A^\textbf{\textrm{T}}} & \bm{0} \end{bmatrix}=\bm{S^\textbf{\textrm{T}}}
      \text{ has size $m+n$.}
      $$
      Applying block elimination to find a <strong>block factorization</strong> \(S=LDL^\mathrm{T}\). Then test invertibility:
      $$
      \textcolor{RoyalBlue}{
        \bm{S}\textit{\textbf{ is invertible}}\quad\Longleftrightarrow\quad\bm{A^\textbf{\textrm{T}}A}\textit{\textbf{ is invertible $\quad\Longleftrightarrow\quad\bm{Ax\neq 0}$ whenever $\bm{x\neq 0}$}}
      }
      $$
      <span class="list-head">Solution</span>&emsp;The first block pivot is \(I\). Subtract \(A^\mathrm{T}\) times row 1 from row 2:
      $$
      \textbf{Block elimination}\quad
      S=\begin{bmatrix} I & A \\ A^\mathrm{T} & 0 \end{bmatrix}
      \quad\text{goes to}\quad
      \begin{bmatrix} I & A \\ 0 & \bm{-A^\textbf{\textrm{T}}A} \end{bmatrix}
      \text{.\quad This is $U$.}
      $$
      The block pivot matrix \(D\) contains \(I\) and \(-A^\mathrm{T}A\). Then \(L\) and \(L^\mathrm{T}\) contain \(A^\mathrm{T}\) and \(A\):
      $$
      \boxed{
        \qquad
        \textbf{Block factorization}\quad
        S=LDL^\mathrm{T}=
        \begin{bmatrix} I & 0 \\ A^\mathrm{T} & I \end{bmatrix}
        \begin{bmatrix} I & 0 \\ 0 & -A^\mathrm{T}A \end{bmatrix}
        \begin{bmatrix} I & A \\ 0 & I \end{bmatrix}
        \text{.}
        \qquad
      }
      $$
      \(L\) is certainly invertible, with diagonal 1's. The inverse of the middle matrix involves \((A^\mathrm{T}A)^{-1}\). Section 4.2 answers a key question about the matrix \(A^\mathrm{T}A\).
      $$
      \textcolor{RoyalBlue}{
        \begin{array}{l}
          \textbf{When is $\bm{A^\textrm{T}A}$ invertible?}\textit{ Answer: $A$ must have independent columns.} \\
          \textbf{Then $A\bm{x}=\bm{0}$ only if $\bm{x}=\bm{0}$. Otherwise $A\bm{x}=\bm{0}$ will lead to $A^\mathrm{T}A\bm{x}=0$.}
        \end{array}
      }
      $$
    </li>
  </ol>
</details>

## Chapter 3 Vector Spaces and Subspaces

### Spaces of Vectors

1. The standard \\(n\\)-dimensional space \\(\mathbf{R}^n\\) contains all real column vectors with \\(n\\) components.
2. If \\(\bm{v}\\) and \\(\bm{w}\\) are in a **vector space** \\(\bm{S}\\), every combination \\(c\bm{v}+d\bm{w}\\) must be in \\(\bm{S}\\).
3. The "vectors" in \\(\bm{S}\\) can be matrices or functions of \\(x\\). The 1-point space \\(\bm{Z}\\) consists of \\(\bm{x}=\bm{0}\\).
4. A **subspace** of \\(\mathbf{R}^n\\) is a vector space inside \\(\mathbf{R}^n\\). *Example*: The line \\(y=3x\\) inside \\(\mathbf{R}^2\\).
5. The **column space** of \\(A\\) contains all combinations of the columns of \\(A\\) : a subspace of \\(\mathbf{R}^m\\).
6. The column space contains all the vectors \\(A\bm{x}\\). So \\(A\bm{x}=\bm{b}\\) is solvable when \\(\bm{b}\\) is in \\(\bm{C}(A)\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">3.1 A</span>&emsp;We are given three different vectors \(\bm{b}_1\), \(\bm{b}_2\), \(\bm{b}_3\). Construct a matrix so that the equations \(A\bm{x}=\bm{b}_1\) and \(A\bm{x}=\bm{b}_2\) are solvable, but \(A\bm{x}=\bm{b}_3\) is not solvable. How can you decide if this is possible? How could you construct \(A\)?
      <span class="list-head">Solution</span>&emsp; We want to have \(b_1\) and \(b_2\) in the column space of \(A\). Then \(A\bm{x}=\bm{b}_1\) and \(A\bm{x}=\bm{b}_2\) will be solvable. <em>The quickest way is to make \(\bm{b}_1\) and \(\bm{b}_2\) the two columns of \(A\).</em> Then the solutions are \(\bm{x}=(1,0)\) and \(\bm{x}=(0,1)\).
      <br>
      &emsp;&ensp;Also, we don't want \(A\bm{x}=\bm{b}_3\) to be solvable. So don't make the column space any larger! Keeping only the columns \(\bm{b}_1\) and \(\bm{b}_2\), the question is:
      $$
      \text{Is $A\bm{x}=\begin{bmatrix} & \\ \bm{b}_1 & \bm{b}_2 \\ & \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}=\bm{b}_3$ solvable?}
      \qquad
      \text{Is $\bm{b}_3$ a combination of $\bm{b}_1$ and $\bm{b}_2$?}
      $$
      If the answer is <em>no</em>, we have the desired matrix \(A\). If the answer is <em>yes</em>, then it is <em>not possible</em> to construct \(A\). When the column space contains \(\bm{b}_1\) and \(\bm{b}_2\), it will have to contain all their linear combinations. So \(\bm{b}_3\) would necessarily be in that column space and \(A\bm{x}=\bm{b}_3\) would necessarily be solvable.
    </li>
    <li>
      <span class="list-head">3.1 B</span>&emsp;Describe a subspace \(\mathbf{S}\) of each vector space \(\mathbf{V}\), and then a subspace \(\mathbf{SS}\) of \(\mathbf{S}\).
      $$
      \begin{array}{l}
        \mathbf{V}_1=\text{all combinations of $(1,1,0,0)$ and $(1,1,1,0)$ and $(1,1,1,1)$} \\
        \mathbf{V}_2=\text{all vectors perpendicular to $\bm{u}=(1,2,1)$, so $\bm{u}\cdot\bm{v}=0$} \\
        \mathbf{V}_3=\text{all symmetric 2 by 2 matrices (a subspace of $\mathbf{M}$)} \\
        \mathbf{V}_4=\text{all solutions to the equation $\mathrm{d}^4y/\mathrm{d}x^4=0$ (a subspace of $\mathbf{F}$)}
      \end{array}
      $$
      Describe each \(\mathbf{V}\) two ways: &ldquo;<em>All combinations of</em>&mldr;&rdquo; &ldquo;<em>All solutions of the equations</em>&mldr;&rdquo;
      <br>
      <span class="list-head">Solution</span>&emsp;\(\mathbf{V}_1\) starts with three vectors. A subspace \(\mathbf{S}\) comes from all combinations of the first two vectors \((1,1,0,0)\) and \((1,1,1,0)\). A subspace \(\mathbf{SS}\) of \(\mathbf{S}\) comes from all multiples \((c,c,0,0)\) of the first vector. So many possibilities.
      <br>
      &emsp;&ensp;A subspace \(\mathbf{S}\) of \(\mathbf{V}_2\) is the line through \((1,-1,1)\). This line is perpendicular to \(\bm{u}\). The vector \(\bm{x}=(0,0,0)\) is in \(\bm{S}\) and all its multiples \(c\bm{x}\) give the smallest subspace \(\mathbf{SS}=\mathbf{Z}\).
      <br>
      &emsp;&ensp;The diagonal matrices are a subspace \(\mathbf{S}\) of the symmetric matrices. The multiples \(cI\) are a subspace \(\mathbf{SS}\) of the diagonal matrices.
      <br>
      &emsp;&ensp;\(\mathbf{V}_4\) contains all cubic polynomials \(y=a+bx+cx^2+dx^3\), with \(\mathrm{d}^4y/\mathrm{d}x^4=0\). The quadratic polynomials give a subspace \(\mathbf{S}\). The linear polynomials are one choice of \(\mathbf{SS}\). The constants could be \(\mathbf{SSS}\).
      <br>
      &emsp;&ensp;In all four parts we could take \(\mathbf{S}=\mathbf{V}\) itself, and \(\mathbf{SS}=\) the zero subspace \(Z\).
      <br>
      &emsp;&ensp;Each \(\mathbf{V}\) can be described as <em>all combinations of</em>&mldr; and as <em>all solutions of</em>&mldr;:
      $$
      \begin{array}{ll}
        \mathbf{V}_1=\text{all combinations of the 3 vectors} & \mathbf{V}_1=\text{all solutions of }v_1-v_2=0 \\
        \mathbf{V}_2=\text{all combinations of $(1,0,-1)$ and $(1,-1,1)$} & \mathbf{V}_2=\text{all solutions of }\bm{u}\cdot\bm{v}=0 \\
        \mathbf{V}_3=\text{all combinations of }\left[\begin{smallmatrix} 1 & 0 \\ 0 & 0 \end{smallmatrix}\right],\left[\begin{smallmatrix} 0 & 1 \\ 1 & 0 \end{smallmatrix}\right],\left[\begin{smallmatrix} 0 & 0 \\ 0 & 1 \end{smallmatrix}\right] & \mathbf{V}_3=\text{all solutions $\left[\begin{smallmatrix} a & b \\ c & d \end{smallmatrix}\right]$ of }b=c \\
        \mathbf{V}_4=\text{all combinations of }1,x,x^2,x^3 & \mathbf{V}_4=\text{all solutions to }\mathrm{d}^4y/\mathrm{d}x^4=0
      \end{array}
      $$
    </li>
  </ol>
</details>

### The Nullspace of <math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>A</mi></mrow></math>: Solving <math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>A</mi><mi mathvariant="bold-italic">x</mi><mo>=</mo><mi mathvariant="bold">0</mi></mrow></math> and <math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>R</mi><mi mathvariant="bold-italic">x</mi><mo>=</mo><mi mathvariant="bold">0</mi></mrow></math>

1. The **nullspace** \\(\bm{N}(A)\\) in \\(\mathbf{R}^n\\) contains all solutions \\(\bm{x}\\) to \\(A\bm{x}=\bm{0}\\). This includes \\(\bm{x}=\bm{0}\\)
2. Elimination (from \\(A\\) to \\(U\\) to \\(R\\)) does not change the nullspace: \\(\bm{N}(A)=\bm{N}(U)=\bm{N}\(R\)\\).
3. The **reduced row echelon form** \\(\bm{R=\textbf{\textsf{rref}}(A)}\\) has all pivots \(=1\), with zeros above and below.
4. If column \\(j\\) of \\(R\\) is free (no pivot), there is a "*special solution*" to \\(A\bm{x}=\bm{0}\\) with \\(x_j=1\\).
5. Number of pivots = number of nonzero rows in \\(R=\textbf{rank }\bm{r}\\). There are \\(n-r\\) free columns.
6. Every matrix with \\(m\lt n\\) has nonzero solution to \\(A\bm{x}=\bm{0}\\) in its nullspace.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">3.2 A</span>&emsp;Why do \(A\) and \(R\) have the same nullspace if \(EA=R\) and \(E\) is invertible?
      <br><br>
      <table class="list-table">
        <tr>
          <td><span class="list-head">Solution</span>&emsp;</td>
          <td>
            If \(A\bm{x}=\bm{0}\) then \(R\bm{x}=EA\bm{x}=E\bm{0}=\bm{0}\)
            <br>
            If \(R\bm{x}=\bm{0}\) then \(A\bm{x}=E^{-1}R\bm{x}=E^{-1}\bm{0}=\bm{0}\)
            <br>
            \(A\) and \(R\) also have the same row space and the same rank.
          </td>
        </tr>
      </table>
    </li>
    <li>
      <span class="list-head">3.2 B</span>&emsp;Create a 3 by 4 matrix \(R\) whose special solutions to \(R\bm{x}=\bm{0}\) are \(\bm{s}_1\) and \(\bm{s}_2\):
      $$
      \bm{s_1}=\begin{bmatrix*}[r] -3 \\ 1 \\ 0 \\ 0 \end{bmatrix*}
      \quad\text{and}\quad
      \bm{s_2}=\begin{bmatrix*}[r] -2 \\ 0 \\ -6 \\ 1 \end{bmatrix*}
      \qquad
      \begin{array}{l}
        \text{pivot columns 1 and 3} \\
        \text{free varibales $x_2$ and $x_4$}
      \end{array}
      $$
      Describe all possible matrices \(A\) with this nullspace \(\bm{N}(A)=\) all combinations of \(\bm{s}_1\) and \(\bm{s}_2\).
      <br>
      <span class="list-head">Solution</span>&emsp; The reduced matrix \(R\) have pivots \(=1\) in columns 1 and 3. There is no third pivot, so row 3 of \(R\) is all zeros. The free columns 2 and 4 will be combinations of the pivot columns: 3, 0, 2, 6 in \(R\) come from -3, -0, -2, -6 in \(s_1\) and \(s_2\). <strong>Every \(\bm{A=ER}\).</strong>
      <br>
      &emsp;&ensp;Every 3 by 4 matrix has at least one special solution. <em>These matrices have two.</em>
      $$
      R=
      \begin{bmatrix}
        \bm{1} & \bm{3} & 0 & \bm{2} \\
        0 & \bm{0} & \bm{1} & \bm{6} \\
        0 & 0 & 0 & 0
      \end{bmatrix}
      \quad\text{has}\quad
      R\bm{s}_1=\bm{0}
      \quad\text{and}\quad
      R\bm{s}_2=\bm{0}
      \text{.}
      $$
    </li>
    <li>
      <span class="list-head">3.2 C</span>&emsp;Find the row reduced form \(R\) and the rank \(r\) of \(A\) and \(B\) (<em>those depend on \(c\)</em>). Which are the pivot coumns of \(A\)? What are the special solutions?
      $$
      \textbf{Find special solutions}\qquad
      A=
      \begin{bmatrix}
        1 & 2 & 1 \\
        3 & 6 & 3 \\
        4 & 8 & c
      \end{bmatrix}
      \quad\text{and}\quad
      B=
      \begin{bmatrix}
       c & c \\
       c & c
      \end{bmatrix}
      \text{.}
      $$
      <span class="list-head">Solution</span>&emsp;The matrix \(A\) has \(\text{row 2}=3(\text{row 1})\). The rank of \(A\) is \(r=2\) <em><strong>except if</strong></em> \(\bm{c=4}\). \(\text{Row 4}-4(\text{row 1})\) ends in \(c-4\). The pivots are in columns 1 and 3. The second variable \(x_2\) is free. Notice the form of \(R\): Row 3 has moved up into row 2.
      $$
      \bm{c\neq 4}\quad R=
      \begin{bmatrix}
        \bm{1} & 2 & 0 \\
        0 & 0 & \bm{1} \\
        0 & 0 & 0
      \end{bmatrix}
      \qquad
      \bm{c=4}\qquad R=
      \begin{bmatrix}
        \bm{1} & 2 & 1 \\
        0 & 0 & 0 \\
        0 & 0 & 0
      \end{bmatrix}
      \text{.}
      $$
      Two pivots leave one free variable \(x_2\). But when \(c=4\), the only pivot is in column 1 (rank one). The second and third variables are free, producing two special solutions:
      $$
      c\neq 4\quad\text{Special solution $(-2,1,0)$}
      \qquad
      c=4\quad\text{Another special solution $(-1, 0, 1)$.}
      $$
      &emsp;&ensp;The 2 by 2 matrix \(B=\left[\begin{smallmatrix} c & c \\ c & c \end{smallmatrix}\right]\) has rank \(r=1\) <em><strong>except if</strong></em> \(\bm{c=0}\), when the rank is zero!
      $$
      \bm{c\neq 0}\quad R=\begin{bmatrix} 1 & 1 \\ 0 & 0 \end{bmatrix}
      \qquad
      \bm{c=0}\quad R=\begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}\quad\text{all nullspace$=\mathbf{R}^2$.}
      $$
    </li>
  </ol>
</details>

### The Complete Solution to <math xmlns="http://www.w3.org/1998/Math/MathML"><mrow><mi>A</mi><mi mathvariant="bold-italic">x</mi><mo>=</mo><mi mathvariant="bold-italic">b</mi></mrow></math>

1. **Complete solution** to \\(A\bm{x}=\bm{b}\\): \\(\bm{x}=(\text{one particular solution $\bm{x}_p$})+(\text{any $\bm{x}_n$ in the nullspace})\\).
2. Elimination on \\(\begin{bmatrix} A & \bm{b} \end{bmatrix}\\) leads to \\(\begin{bmatrix} R & \bm{d} \end{bmatrix}\\). Then \\(A\bm{x}=\bm{b}\\) is equivalent to \\(R\bm{x}=\bm{d}\\).
3. \\(A\bm{x}=\bm{b}\\) and \\(R\bm{x}=\bm{d}\\) are solvable only when all zero rows of \\(R\\) have zeros in \\(\bm{d}\\).
4. When \\(R\bm{x}=\bm{d}\\) is solvable, one very particular solution \\(\bm{x}_p\\) has all free variables equal to zero.
5. \\(A\\) has **full column rank** \\(\bm{r=n}\\) when its nullspace \\(\bm{N}(A)=\text{zero vector}\\): *no free variables*.
6. \\(A\\) has **full row rank** \\(\bm{r=m}\\) when its column space \\(\bm{C}(A)\\) is \\(\mathbf{R}^m\\): \\(A\bm{x}=\bm{b}\\) is always solvable.
7. The four cases are \\(r=m=n\\) (\\(A\\) is invertible) and \\(r=m\lt n\\) (every \\(A\bm{x}=\bm{b}\\) is solvable) and \\(r=n\lt m\\) (\\(A\bm{x}=\bm{b}\\) has 1 or 0 solutions) and \\(r\lt m\\), \\(r\lt n\\) (0 or &infin; solutions).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">3.3 A</span>&emsp;This question connects elimination (<strong>pivot columns and back substitution</strong>) to <strong>column space-nullspace-rank-solvability</strong>. \(A\) has rank 2:
      $$
      A\bm{x}=\bm{b}
      \quad\text{is}\quad
      \begin{alignedat}{1.5}
        x_1+2x_2+3x_3+ && 5x_4=b_1 \\
        2x_1+4x_2+8x_3+ && 12x_4=b_2 \\
        3x_1+6x_2+7x_3+ && 13x_4=b_3
      \end{alignedat}
      $$
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;<b>1.</b>&ensp;</td>
          <td>
            Reduce \(\begin{bmatrix} A & \bm{b} \end{bmatrix}\) to \(\begin{bmatrix} U & \bm{c} \end{bmatrix}\), so that \(A\bm{x}=\bm{b}\) becomes a triangular system \(U\bm{x}=\bm{c}\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>2.</b>&ensp;</td>
          <td>
            Find the condition on \(b_1\), \(b_2\), \(b_3\) for \(A\bm{x}=\bm{b}\) to have a solution.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>3.</b>&ensp;</td>
          <td>
            Describe the column space of \(A\). Which plane in \(\mathbf{R}^3\)?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>4.</b>&ensp;</td>
          <td>
            Describe the nullspace of \(A\). Which special solutions in \(\mathbf{R}^4\)?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>5.</b>&ensp;</td>
          <td>
            Reduce \(\begin{bmatrix} U & \bm{c} \end{bmatrix}\) to \(\begin{bmatrix} R & \bm{d} \end{bmatrix}\): Special solutions from \(R\), particular solution from \(\bm{d}\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>6.</b>&ensp;</td>
          <td>
            Find a particular solution to \(A\bm{x}=(0,6,-6)\) and then the complete solution.
          </td>
        </tr>
      </table>
      <span class="list-head">Solution</span>
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;<b>1.</b>&ensp;</td>
          <td>
            The multipliers in elimination are 2 and 3 and -1. They take \(\begin{bmatrix} A & \bm{b} \end{bmatrix}\) to \(\begin{bmatrix} U & \bm{c} \end{bmatrix}\).
          </td>
        </tr>
        <tr>
          <td colspan="2">
            $$
            \begin{bmatrix*}[r]
              1 & 2 & 3 & 5 & \mathbf{b_1} \\
              2 & 4 & 8 & 12 & \mathbf{b_2} \\
              3 & 6 & 7 & 13 & \mathbf{b_3}
            \end{bmatrix*}
            \rarr
            \left[\begin{array}{rrrr|l}
              1 & 2 & 3 & 5 & \mathbf{b_1} \\
              0 & 0 & 2 & 2 & \mathbf{b_2-2b_1} \\
              0 & 0 & -2 & -2 & \mathbf{b_3-3b_1}
            \end{array}\right]
            \rarr
            \left[\begin{array}{cccc|l}
              1 & 2 & 3 & 5 & \mathbf{b_1} \\
              0 & 0 & 2 & 2 & \mathbf{b_2-2b_1} \\
              0 & 0 & 0 & 0 & \mathbf{b_3+b_2-5b_1}
            \end{array}\right]
            $$
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>2.</b>&ensp;</td>
          <td>
            The last equation shows the solvability condition \(b_3+b_2-5b_1=0\). Then \(0=0\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>3.</b>&ensp;</td>
          <td>
            <strong>First description:</strong> The column space is the plane containing all combinations of the pivot columns \((1,2,3)\) and \((3,8,7)\). The pivots are in columns 1 and 3. <strong>Second description:</strong> The column space contains all vectors with \(b_3+b_2-5b_1=0\). That makes \(A\bm{x}=\bm{b}\) solvable, so \(\bm{b}\) is in the column space. <em>All columns of \(A\) pass this test \(b_3+b_2-5b_1=0\). This is the equation for the plane in the first description!</em>
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>4.</b>&ensp;</td>
          <td>
            The special solutions have free variables \(x_2=1\), \(x_4=0\) and then \(x_2=0\), \(x_4=1\):
            $$
            \begin{array}{l}
              \textbf{Special solutions to }A\bm{x}=\bm{0} \\
              \textbf{Back substitution in }U\bm{x}=\bm{0} \\
              \textbf{or change signs of 2, 2, 1 in }\bm{R}
            \end{array}
            \qquad
            s_1=\begin{bmatrix*}[r] -2 \\ 1 \\ 0 \\ 0 \end{bmatrix*}
            \qquad
            s_2=\begin{bmatrix*}[r] -2 \\ 0 \\ -1 \\ 1 \end{bmatrix*}
            $$
            The nullspace \(\bm{N}(A)\) in \(\mathbf{R}^4\) contains all \(\bm{x}_n=c_1\bm{s}_1+c_2\bm{s}_2\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>5.</b>&ensp;</td>
          <td>
            In the reduced form \(R\), the third column changes from \((3,2,0)\) in \(U\) to \((0,1,0)\).
            The right side \(\bm{c}=(0,6,0)\) becomes \(\bm{d}=(-9,3,0)\) showing &minus;9 and 3 in \(\bm{x}_p\):
            $$
            \begin{bmatrix} U & \bm{c} \end{bmatrix}=
            \begin{bmatrix}
              1 & 2 & 3 & 5 & \bm{0} \\
              0 & 0 & 2 & 2 & \bm{6} \\
              0 & 0 & 0 & 0 & \bm{0}
            \end{bmatrix}
            \rarr
            \begin{bmatrix} R & \bm{d} \end{bmatrix}=
            \begin{bmatrix*}[r]
              1 & 2 & 0 & 2 & \bm{-9} \\
              0 & 0 & 1 & 1 & \bm{3} \\
              0 & 0 & 0 & 0 & \bm{0}
            \end{bmatrix*}
            $$
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>6.</b>&ensp;</td>
          <td>
            One particular solution \(\bm{x}_p\) has free variables = zero. Back substitute in \(U\bm{x}=\bm{c}\):
            $$
            \begin{array}{l}
              \textbf{Particular solution to }A\bm{x}_p=\bm{b} \\
              \textbf{Bring $\mathord{-}9$ and $3$ from the vector }\bm{d} \\
              \textbf{Free variables $x_2$ and $x_4$ are zero}
            \end{array}
            \quad
            \bm{x}_p=\begin{bmatrix*}[r] -9 \\ 0 \\ 3 \\ 0 \end{bmatrix*}
            $$
            The complete solution to \(A\bm{x}=(0,6,-6)\) is \(\bm{x}=\bm{x}_p+\bm{x}_n=x_p+c_1\bm{s}_1+c_2\bm{s}_2\).
          </td>
        </tr>
      </table>
    </li>
    <li>
      <span class="list-head">3.3 B</span>&emsp;Suppose you have this information about the solutions to \(A\bm{x}=\bm{b}\) for a specific \(\bm{b}\). What does that tell you about \(m\) and \(n\) and \(r\) (and \(A\) itself)? And possibly about \(\bm{b}\).
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;<b>1.</b>&ensp;</td>
          <td>
            There is exactly one solution.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>2.</b>&ensp;</td>
          <td>
            All solutions to \(A\bm{x}=\bm{b}\) have the form \(\bm{x}=\left[\begin{smallmatrix} 2 \\ 1 \end{smallmatrix}\right]+c\left[\begin{smallmatrix} 1 \\ 1 \end{smallmatrix}\right]\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>3.</b>&ensp;</td>
          <td>
            There are no solutions.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>4.</b>&ensp;</td>
          <td>
            All solutions to \(A\bm{x}=\bm{b}\) have the form \(\bm{x}=\left[\begin{smallmatrix} 1 \\ 1 \\ 0 \end{smallmatrix}\right]+c\left[\begin{smallmatrix} 1 \\ 0 \\ 1 \end{smallmatrix}\right]\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>5.</b>&ensp;</td>
          <td>
            There are infinitely many solutions.
          </td>
        </tr>
      </table>
      <span class="list-head">Solution</span>&emsp;In case <b>1</b>, with exactly one solution, \(A\) must have full column rank \(r=n\). The nullspace of \(A\) contains only the zero vector. Necessarily \(m\geq n\).
      <br>
      &emsp;&ensp;In case <b>2</b>, \(A\) must have \(n=2\) columns (and \(m\) is arbitrary). With \(\left[\begin{smallmatrix} 1 \\ 1 \end{smallmatrix}\right]\) in the nullspace of \(A\), column 2 is the <em>negative</em> of column 1. Also \(A\neq 0\): the rank is 1. With \(\bm{x}=\left[\begin{smallmatrix} 2 \\ 1 \end{smallmatrix}\right]\) as a solution, \(\bm{b}=2(\text{column 1})+(\text{column 2})\). My choice for \(\bm{x}_p\) would be \((1,0)\).
      <br>
      &emsp;&ensp;In case <b>3</b> we only know that \(\bm{b}\) is not in the column space of \(A\). The rank of \(A\) must be less than \(m\). I guess we know \(\bm{b}\neq\bm{0}\), otherwise \(\bm{x}=\bm{0}\) would be a solution.
      <br>
      &emsp;&ensp;In case <b>4</b>, \(A\) must have \(n=3\) columns. With \((1,0,1)\) in the nullspace of \(A\), column 3 is the negative of column 1. Column 2 must <em>not</em> be a multiple of column 1, or the nullspace would contain another special solution. So the rank of \(A\) is \(3-1=2\). Necessarily \(A\) has \(m\geq 2\) rows. The right side \(\bm{b}\) is \(\text{column 1}+\text{column 2}\).
      <br>
      &emsp;&ensp;In case <b>5</b> with infinitely many solutions, the nullspace must contain nonzero vectors. The rank \(r\) must be less than \(n\) (no full column rank), and \(\bm{b}\) must be in the column space of \(A\). We don't know if <em>every</em> \(\bm{b}\) is in the column space, so we don't know if \(r=m\).
    </li>
    <li>
      <span class="list-head">3.3 C</span>&emsp;Find the complete solution \(\bm{x}=\bm{x}_p+\bm{x}_n\) by forward elimination on \(\begin{bmatrix} A & \bm{b} \end{bmatrix}\):
      $$
      \begin{bmatrix}
        1 & 2 & 1 & 0 \\
        2 & 4 & 4 & 8 \\
        4 & 8 & 6 & 8
      \end{bmatrix}
      \begin{bmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{bmatrix}
      =
      \begin{bmatrix*}[r] 4 \\ 2 \\ 10 \end{bmatrix*}
      \text{.}
      $$
      Find numbers \(y_1\), \(y_2\), \(y_3\) so that \(y_1(\text{row 1})+y_2(\text{row 2})+y_3(\text{row 3})=\textit{\textbf{zero row}}\). Check that \(\bm{b}=(4,2,10)\) satisfies the condition \(y_1b_1+y_2b_2+y_3b_3=0\). Why is this the condition for the equations to be solvable and \(\bm{b}\) to be in the column space?
      <br>
      <span class="list-head">Solution</span>&emsp;Forward elimination on \(\begin{bmatrix} A & \bm{b} \end{bmatrix}\) produces a zero row in \(\begin{bmatrix} U & \bm{c} \end{bmatrix}\). The third equation becomes \(0=0\) and the equations are consistent (and solvable):
      $$
      \begin{bmatrix*}[r]
        1 & 2 & 1 & 0 & \bm{4} \\
        2 & 4 & 4 & 8 & \bm{2} \\
        4 & 8 & 6 & 8 & \bm{10}
      \end{bmatrix*}
      \rarr
      \begin{bmatrix*}[r]
        1 & 2 & 1 & 0 & \bm{4} \\
        0 & 0 & 2 & 8 & \bm{-6} \\
        0 & 0 & 2 & 8 & \bm{-6}
      \end{bmatrix*}
      \rarr
      \begin{bmatrix*}[r]
        1 & 2 & 1 & 0 & \bm{4} \\
        0 & 0 & 2 & 8 & \bm{-6} \\
        0 & 0 & 0 & 0 & \bm{0}
      \end{bmatrix*}
      \text{.}
      $$
      Columns 1 and 3 contain pivots. The variables \(x_2\) and \(x_4\) are free. If we set those to zero we can solve (back substitution) for the particular solution or we continue to \(R\).
      <br>
      &emsp;&ensp;\(R\bm{x}=\bm{d}\) shows that the particular solution with \(\text{free variables}=0\) is \(\bm{x}_p=(7,0,-3,0\).
      $$
      \begin{bmatrix*}[r]
        1 & 2 & 1 & 0 & \bm{4} \\
        0 & 0 & 2 & 8 & \bm{-6} \\
        0 & 0 & 0 & 0 & \bm{0}
      \end{bmatrix*}
      \rarr
      \begin{bmatrix*}[r]
        1 & 2 & 1 & 0 & \bm{4} \\
        0 & 0 & 1 & 4 & \bm{-3} \\
        0 & 0 & 0 & 0 & \bm{0}
      \end{bmatrix*}
      \rarr
      \begin{bmatrix*}[r]
        1 & 2 & 0 & -4 & \bm{7} \\
        0 & 0 & 1 & 4 & \bm{-3} \\
        0 & 0 & 0 & 0 & \bm{0}
      \end{bmatrix*}
      .
      $$
      For the nullspace part \(\bm{x}_n\) with \(\bm{b}=\bm{0}\), set the free variables \(x_2\), \(x_4\), to \(1,0\) and also \(0,1\):
      $$
      \textbf{Special solutions}\qquad
      \bm{s}_1=(-2,1,0,0)
      \quad\text{and}\quad
      \bm{s}_2=(4,0,-4,1)
      $$
      Then the complete solution to \(A\bm{x}=\bm{b}\) (and \(R\bm{x}=\bm{d}\)) is \(\bm{x}_\text{complete}=\bm{x}_p+c_1\bm{s}_1+c_2\bm{s}_2\).
      <br>
      &emsp;&ensp;The rows of \(A\) produced the zero row from \(2(\text{row 1})+(\text{row 2})-(\text{row 3})=(0,0,0,0)\). Thus \(\bm{y}=(2,1,-1)\). The same combination for \(\bm{b}=(4,2,10)\) gives \(2(4)+(2)-1(10)=0\).
      <br>
      &emsp;&ensp;If a combination of the rows (on the left side) gives the zero row, then the same combination must give zero on the right side. Of course! <em>Otherwise no solution.</em>
      <br>
      &emsp;&ensp;Later we will say this again in different words: If every column of \(A\) is perpendicular to \(\bm{y}=(2,1,-1)\), then any combination \(\bm{b}\) of those columns must also be perpendicular to \(\bm{y}\). Otherwise \(\bm{b}\) is not in the column space and \(A\bm{x}=\bm{b}\) is not solvable.
      <br>
      &emsp;&ensp;And again: If \(\bm{y}\) is in the nullspace of \(A^\mathrm{T}\) then \(\bm{y}\) must be perpendicular to every \(\bm{b}\) in the column space of \(A\). Just looking ahead&mldr;
    </li>
  </ol>
</details>

### Independence, Basis and Dimension

1. **Independent columns** of \\(A\\): The only solution to \\(A\bm{x}=\bm{0}\\) is \\(\bm{x}=\bm{0}\\). The nullspace is \\(\bm{Z}\\).
2. Independent vectors: The only zero combination \\(c_1\bm{v}_1+\cdots+c_k\bm{v}_k=\bm{0}\\) has all \\(c\text{'s}=0\\).
3. A matrix with \\(m\lt n\\) has **dependent columns**: At least \\(n-m\\) free variables/special solutions.
4. The vectors \\(\bm{v}_1,\ldots,\bm{v}_k\\) **span the space** \\(\bm{S}\\) if \\(\bm{S}=\text{all combinations of the $\bm{v}$'s}\\).
5. The vectors \\(\bm{v}_1,\ldots,\bm{v}_k\\) are a **basis for** \\(S\\) if they are independent and they span \\(\bm{S}\\).
6. The **dimension of a space** \\(\bm{S}\\) is the number of vectors in every basis for \\(\bm{S}\\).
7. If \\(A\\) is 4 by 4 and invertible, its columns are a basis for \\(\mathbf{R}^4\\). The dimension of \\(\mathbf{R}^4\\) is 4.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">3.4 A</span>&emsp;Start with the vectors \(\bm{v}_1=(1,2,0)\) and \(\bm{v}_2=(2,3,0)\).&emsp;<b>(a)</b> Are they linearly independent?&emsp;<b>(b)</b> Are they a basis for any space?&emsp;<b>(c)</b> What space \(\mathbf{V}\) do they span?&emsp;<b>(d)</b> What is the dimension of \(\mathbf{V}\)?&emsp;<b>(e)</b> Which matrices \(A\) have \(\mathbf{V}\) as their column space?&emsp;(f) Which matrices have \(\mathbf{V}\) as their nullspace?&emsp;<b>(g)</b> Describe all vectors \(\bm{v}_3\) that complete a basis \(\bm{v}_1\), \(\bm{v}_2\), \(\bm{v}_3\) for \(\mathbf{R}^3\).
      <br><br>
      <span class="list-head">Solution</span>
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;<b>(a)</b>&ensp;</td>
          <td>
            \(\bm{v}_1\) and \(\bm{v}_2\) are independent&mdash;the only combination to give \(\bm{0}\) is \(0\bm{v}_1+0\bm{v}_2\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>(b)</b>&ensp;</td>
          <td>
            Yes, they are a basis for the space they span.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>(c)</b>&ensp;</td>
          <td>
            That space \(\mathbf{V}\) contains all vectors \((x,y,0)\). It is the \(xy\) plane in \(\mathbf{R}^3\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>(d)</b>&ensp;</td>
          <td>
            The dimension of \(\mathbf{V}\) is 2 since the basis contains two vectors.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>(e)</b>&ensp;</td>
          <td>
            This \(\mathbf{V}\) is the column space of any 3 by \(n\) matrix \(A\) of rank 2, if every column is a combination of \(\bm{v}_1\) and \(\bm{v}_2\). In particular \(A\) could just have columns \(\bm{v}_1\) and \(\bm{v}_2\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>(f)</b>&ensp;</td>
          <td>
            This \(\mathbf{V}\) is the nullspace of any \(m\) by 3 matrix \(B\) of rank 1, if every row is a multiple of \((0,0,1)\). In particular take \(B=\begin{bmatrix} 0 & 0 & 1\end{bmatrix}\). Then \(B\bm{v}_1=\bm{0}\) and \(B\bm{v}_2=\bm{0}\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>(g)</b>&ensp;</td>
          <td>
            Any third vector \(\bm{v}_3=(a,b,c)\) will complete a basis for \(\mathbf{R}^3\) provided \(c\neq 0\).
          </td>
        </tr>
      </table>
    </li>
    <li>
      <span class="list-head">3.4 B</span>&emsp;Start with three independent vectors \(\bm{w}_1\), \(\bm{w}_2\), \(\bm{w}_3\). Take combinations of those vectors to produce \(\bm{v}_1\), \(\bm{v}_2\), \(\bm{v}_3\). Write the combinations in matrix form as \(V=WB\):
      $$
      \begin{alignedat}{2.5}
        & \bm{v}_1=\bm{w}_1+ &  \bm{w}_2 &   &\\
        & \bm{v}_2=\bm{w}_1+ & 2\bm{w}_2 & + & \bm{w}_3 \\
        & \bm{v}_3=          &  \bm{w}_2 & + & c\bm{w}_3
      \end{alignedat}
      \quad\text{which is}\quad
      \begin{bmatrix} & &  \\ \bm{v}_1 & \bm{v}_2 & \bm{v}_3 \\ & & \end{bmatrix}
      =
      \begin{bmatrix} & & \\ \bm{w}_1 & \bm{w}_2 & \bm{w}_3 \\ & & \end{bmatrix}
      \begin{bmatrix} 1 & 1 & 0 \\ 1 & 2 & 1 \\ 0 & 1 & c \end{bmatrix}
      $$
      What is the test on \(B\) to see if \(V=WB\) has independent columns? If \(c\neq 1\) show that \(\bm{v}_1\), \(\bm{v}_2\), \(\bm{v}_3\) are linearly independent. If \(c=1\) show that the \(\bm{v}\)'s are linearly <em>dependent</em>.
      <br><br>
      <span class="list-head">Solution</span>&emsp;The test on \(V\) for independence of its columns was in our first definition: <em>The nullspace of \(V\) must contain only the zero vector.</em> Then \(\bm{x}=(0,0,0)\) is the only combination of the columns that gives \(V\bm{x}=\text{zero vector}\).
      <br>
      &emsp;&ensp;If \(c=1\) in our problem, we can see <em>dependence</em> in two ways. First, \(\bm{v}_1+\bm{v}_3\) will be the same as \(\bm{v}_2\). (If you add \(\bm{w}_1+\bm{w}_2\) to \(\bm{w}_2+\bm{w}_3\) you get \(\bm{w}_1+2\bm{w}_2+\bm{w}_3\) which is \(\bm{v}_2\).) In other words \(\bm{v}_1-\bm{v}_2+\bm{v}_3=\bm{0}\)&mdash;which says that the \(\bm{v}\)'s are not independent.
      <br>
      &emsp;&ensp;The other way is to look at the nullspace of \(B\). If \(c=1\), the vector \(\bm{x}=(1,-1,1)\) is in that nullspace, and \(B\bm{x}=\bm{0}\). Then certainly \(WB\bm{x}=\bm{0}\) which is the same as \(V\bm{x}=\bm{0}\). So the \(\bm{v}\)'s are dependent. This specific \(\bm{x}=(1,-1,1)\) from the nullspace tells us again that \(\bm{v}_1-\bm{v}_2+\bm{v}_3=\bm{0}\).
      <br>
      &emsp;&ensp;Now suppose \(c\neq 1\). Then the matrix \(B\) is invertible. So if \(\bm{x}\) is <em>any nonzero vector</em> we know that \(B\bm{x}\) is nonzero. Since the \(\bm{w}\)'s are given as independent, we further know that \(WB\bm{x}\) is nonzero. Since \(V=WB\), this says that \(\bm{x}\) is <em>not</em> in the nullspace of \(V\). In other words \(\bm{v}_1\), \(\bm{v}_2\), \(\bm{v}_3\) are independent.
      <br>
      &emsp;&ensp;The general rule is &ldquo;independent \(\bm{v}\)'s from independent \(\bm{w}\)'s when \(B\) is invertible&rdquo;. And if these vectors are in \(\mathbf{R}^3\), they are not only independent&mdash; they are a basis for \(\mathbf{R}^3\). <em><strong>&ldquo;Basis of \(\bm{v}\)'s from basis of \(\bm{w}\)'s when the change of basis matrix \(B\) is invertible.&rdquo;</strong></em>
    </li>
    <li>
      <span class="list-head">3.4 C</span>&emsp;(<strong>Important example</strong>) Suppose \(\bm{v}_1,\ldots,\bm{v}_n\) is a basis for \(\mathbf{R}^n\) and the \(n\) by \(n\) matrix \(A\) is invertible. Show that \(A\bm{v}_1,\ldots,A\bm{v}_n\) is also a basis for \(\mathbf{R}^n\).
      <br><br>
      <span class="list-head">Solution</span>&emsp;In <em>matrix language</em>: Put the basis vectors \(\bm{v}_1,\ldots,\bm{v}_n\) in the columns of an invertible(!) matrix \(V\). Then \(A\bm{v}_1,\ldots,A\bm{v}_n\) are the columns of \(AV\). Since \(A\) is invertible, so is \(AV\) and its columns give a basis.
      <br>
      &emsp;&ensp;In <em>vector language</em>: Suppose \(c_1A\bm{v}_1+\cdots+c_nA\bm{v}_n=\bm{0}\). This is \(A\bm{v}=\bm{0}\) with \(\bm{v}=c_1\bm{v}_1+\cdots+c_n\bm{v}_n\). Multiply by \(A^{-1}\) to reach \(\bm{v}=\bm{0}\). By linear independence of the \(\bm{v}\)'s, all \(c_i=0\). This shows that the \(A\bm{v}\)'s are independent.
      <br>
      &emsp;&ensp;To show that the \(A\bm{v}\)'s span \(\mathbf{R}^n\), solve \(c_1A\bm{v}_1+\cdots+c_nA\bm{v}_n=\bm{b}\) which is the same as \(c_1\bm{v}_1+\cdots+c_n\bm{v}_n=A^{-1}\bm{b}\). Since the \(\bm{v}\)'s are a basis, this must be solvable.
    </li>
  </ol>
</details>

### Dimensions of the Four Subspaces

1. The column space \\(\bm{C}(A)\\) and the row space \\(\bm{C}(A^\mathrm{T})\\) both have *dimension* \\(r\\) (the rank of \\(A\\)).
2. The nullspace \\(\bm{N}(A)\\) has *dimension* \\(n-r\\). The left nullspace \\(\bm{N}(A^\mathrm{T})\\) has *dimension* \\(m-r\\).
3. Elimination produces bases for the row space and nullspace of \\(A\\): They are the same as for \\(R\\).
4. Elimination often changes the column space and left nullspace (but dimensions don't change).
5. **Rank one matrices**: \\(A=\bm{uv}^\mathrm{T}=\text{column times row}\\): \\(\bm{C}(A)\\) has basis \\(\bm{u}\\), \\(\bm{C}(A^\mathrm{T})\\) has basis \\(\bm{v}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">3.5 A</span>&emsp;Put four 1's into a 5 by 6 matrix of zeros, keeping the dimension of its <em>row space</em> as small as possible. Describe all the ways to make the dimension of its <em>column space</em> as small as possible. How to make the <em>sum of the dimensions of all four subspaces small</em> ?
      <br><br>
      <span class="list-head">Solution</span>&emsp;The rank is 1 if the four 1's go into the same row, or into the same column. They can also go into <em>two rows and two columns</em> (so \(a_{ii}=a_{ij}=a_{ji}=a_{jj}=1\)). Since the column space and row space always have the same dimensions, this answers the first two questions: Dimension 1.
      <br>
      &emsp;&ensp;The nullspace has its smallest possible dimension \(6-4=1\) when the rank is \(r=4\). To achieve rank 4, the 1's must go into four different rows and four different columns.
      <br>
      &emsp;&ensp;<strong>You can't do anything about the sum \(r+(n-r)+r+(m-r)=\bm{n+m}\).</strong> It will be \(6+5=11\) no matter how the 1's are placed. The sum is 11 even if there aren't any 1's&mldr;
      <br>
      &emsp;&ensp;If all the orhter entries of \(A\) are 2's instead of 0's, how do these answers change?
    </li>
    <li>
      <span class="list-head">3.5 B</span>&emsp;Fact: All the rows of \(AB\) are combinations of the rows of \(B\). So the row space of \(AB\) is contained in (possibly equal to) the row space of \(B\). \(\textbf{Rank($\bm{AB}$)$\bm{\leq}$rank($\bm{B}$)}\).
      <br>
      &emsp;&ensp;All columns of \(AB\) are combinations of the columns of \(A\). So the column space of \(AB\) is contained in (possibly equal to) the column space of \(A\). \(\textbf{Rank($\bm{AB}$)$\bm{\leq}$rank($\bm{A}$)}\).
      <br>
      &emsp;&ensp;If we multiply by an <em>invertible</em> matrix, the rank will not change. The rank can't drop, because when we multiply by the inverse matrix the rank can't jump back.
    </li>
  </ol>
</details>

## Chapter 4 Orthogonality

### Orthogonality of the Four Subspaces

1. Orthogonal vectors have \\(\bm{v}^\mathrm{T}\bm{w}=0\\). Then \\(\\|\bm{v}\\|^2+\\|\bm{w}\\|^2=\\|\bm{v}+\bm{w}\\|^2=\\|\bm{v}-\bm{w}\\|^2\\).
2. Subspaces \\(\bm{V}\\) and \\(\bm{W}\\) are orthogonal when \\(\bm{v}^\mathrm{T}\bm{w}=0\\) for every \\(\bm{v}\\) in \\(\bm{V}\\) and every \\(\bm{w}\\) in \\(\bm{W}\\).
3. The row space of \\(A\\) is orthogonal to the nullspace. The column space is orthogonal to \\(\bm{N}(A^\mathrm{T}\\).
4. One pair of dimensions adds to \\(r+(n-r)=n\\). The other pair has \\(r+(m-r)-m\\).
5. Row space and nullspace are orthogonal *complements*: Every \\(\bm{x}\\) in \\(\mathbf{R}^n\\) splits into \\(\bm{x}\_\textbf{row}+\bm{x}_\textbf{null}\\).
6. Suppose a space \\(\bm{S}\\) has dimension \\(d\\). Then every basis for \\(\bm{S}\\) consists of \\(d\\) vectors.
7. If \\(d\\) vectors in \\(\bm{S}\\) are independent, they span \\(\bm{S}\\). If \\(d\\) vectors span \\(\bm{S}\\), they are independent.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">4.1 A</span>&emsp;Suppose \(\bm{S}\) is a six-dimensional subspace of nine-dimensional space \(\mathbf{R}^n\)
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;(a)&ensp;</td>
          <td>
            What are the possible dimensions of subspaces orthogonal to \(\bm{S}\)?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(b)&ensp;</td>
          <td>
            What are the possible dimensions of the orthogonal complement \(\bm{S}^\perp\) of \(\bm{S}\)?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(c)&ensp;</td>
          <td>
            What is the smallest possible size of a matrix \(A\) that has row space \(\bm{S}\)?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(d)&ensp;</td>
          <td>
            What is the smallest possible size of a matrix \(B\) that has nullspace \(\bm{S}^\perp\)?
          </td>
        </tr>
      </table>
      <span class="list-head">Solution</span>
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;(a)&ensp;</td>
          <td>
           If \(\bm{S}\) is six-dimensional in \(\mathbf{R}^9\), subspaces orthogonal to \(\bm{S}\) can have dimensions 0, 1, 2, 3.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(b)&ensp;</td>
          <td>
            The complement \(\bm{S}^\perp\) is the largest orthogonal subspace, with dimension 3.
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(c)&ensp;</td>
          <td>
            The smallest matrix \(A\) is 6 by 9 (its six rows will be a basis for \(\bm{S}\))
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(d)&ensp;</td>
          <td>
            This is the same as question (c)!
          </td>
        </tr>
      </table>
      &emsp;&ensp;If a new row 7 of \(B\) is a combination of the six rows of \(A\), then \(B\) has the same row space as \(A\). It also has the same nullspace. The special solutions \(\bm{s}_1\), \(\bm{s}_2\), \(\bm{s}_3\) to \(A\bm{x}=\bm{0}\), will be the same for \(B\bm{x}=0\). Elimination will change row 7 of \(B\) to all zeros.
    </li>
    <li>
      <span class="list-head">4.1 B</span>&emsp;The equation \(x-3y-4z=0\) describes a plane \(\bm{P}\) in \(\mathbf{R}^3\) (actually a subspace).
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;(a)&ensp;</td>
          <td>
            The plane \(\bm{P}\) is the nullspace \(\bm{N}(A)\) of what 1 by 3 matrix \(A\)? <em>Ans:</em> \(A=\begin{bmatrix} 1 & -3 & -4 \end{bmatrix}\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(b)&ensp;</td>
          <td>
            Find a basis \(\bm{s}_1\), \(\bm{s}_2\) of special solutions of \(x-3y-4z=0\) (these would be the columns of the nullspace matrix \(N\)). <em>Answer:</em> \(\bm{s}_1=(3,1,0)\) and \(\bm{s}_2=(4,0,1)\).
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;(c)&ensp;</td>
          <td>
            Find a basis for the line \(\bm{P}^\perp\) that is perpendicular to \(\bm{P}\). <em>Answer:</em> \((1,-3,-4)\)!
          </td>
        </tr>
      </table>
    </li>
  </ol>
</details>

### Projections

1. The projection of a vector \\(\bm{b}\\) onto the line through \\(\bm{a}\\) is the closest point \\(\bm{p}=\bm{a}(\bm{a^\mathrm{T}b}/\bm{a^\mathrm{T}a})\\).
   <details>
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
     {% asset_img Figure4.6.png %}
     When projection onto a line, the projection \(\bm{p}\) will be some multiple of \(\bm{a}\). Call it \(\bm{p}=\widehat{x}\bm{a}=\text{\textquotedblleft$x$ hat\textquotedblright\space times $\bm{a}$}\).
     <br>
     &emsp;&ensp;The dotted line from \(\bm{b}\) to \(\bm{p}\) is perpendicular to \(\bm{a}\), which is called the "error": \(\bm{e}=\bm{b}-\bm{p}=\bm{b}-\widehat{x}\bm{a}\). Use the fact that \(\bm{a}\cdot\bm{e}=0\):
     $$
     \bm{a}^\mathrm{T}\bm{e}=0
     \harr
     \bm{a}^\mathrm{T}(\bm{b}-\widehat{x}\bm{a})=0
     \harr
     \bm{a}^\mathrm{T}\bm{b}-\widehat{x}\bm{a}^\mathrm{T}\bm{a}=0
     \harr
     \widehat{x}=\frac{\bm{a}^\mathrm{T}\bm{b}}{\bm{a}^\mathrm{T}\bm{a}}
     $$
     Then \(\bm{p}=\widehat{x}\bm{a}=\frac{\bm{a}^\mathrm{T}\bm{b}}{\bm{a}^\mathrm{T}\bm{a}}\bm{a}\).
   </details>
2. The error \\(\bm{e}=\bm{b}-\bm{p}\\) is perpendicular to \\(\bm{a}\\): Right triangle \\(\bm{b\space p\space e}\\) has \\(\\|\bm{p}\\|^2+\\|\bm{e}\\|^2=\\|\bm{b}\\|^2\\)
3. The **projection** of \\(\bm{b}\\) onto a subspace \\(\bm{S}\\) is the closest vector \\(\bm{p}\\) in \\(\bm{S}\\); \\(\bm{b}-\bm{p}\\) is orthogonal to \\(\bm{S}\\).
4. \\(A^\mathrm{T}A\\) is invertible (and symmetric) only if \\(A\\) has independent columns: \\(\bm{N}(A^\mathrm{T}A)=\bm{N}(A)\\).
5. Then the projection of \\(\bm{b}\\) onto the column space of \\(A\\) is the vector \\(\bm{p}=A(A^\mathrm{T}A)^{-1}A^\mathrm{T}\bm{b}\\).
   <details>
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
     {% asset_img Figure4.6.png %}
     As we know a subspace can be described as the column space of a matrix \(A\), when projection onto a subspace, we are actually looking for the combination of the columns of \(A\) that is closest to a given vector \(\bm{b}\):
     $$
     \bm{p}=\widehat{x}_1\bm{a}_1+\cdots+\widehat{x}_n\bm{a}_n=A\widehat{\bm{x}}
     \qquad
     \text{Here $\widehat{\bm{x}}$ comes a vector}
     $$
     The error vector \(\bm{b}-A\widehat{\bm{x}}\) is perpendicular to the subspace, which means every columns of \(A\) times \(\bm{e}\) is zero:
     $$
     \begin{bmatrix}
       \bm{a}_1^\mathrm{T} \\
       \vdots \\
       \bm{a}_n^\mathrm{T}
     \end{bmatrix}
     \begin{bmatrix}
       \bm{b}-A\widehat{\bm{x}}
     \end{bmatrix}
     =
     \bm{0}
     \harr
     A^\mathrm{T}(\bm{b}-A\widehat{\bm{x}})=\bm{0}
     \harr
     A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}
     $$
     Here \(A^\mathrm{T}A\) is a \(n\) by \(n\) symmetric matrix. It is invertible if the \(\bm{a}\)'s are independent. (Be aware \(A\) might not invertible as it may not a square matrix.) Solve this system we find \(\widehat{\bm{x}}\) and then \(p=A\widehat{\bm{x}}=A(A^\mathrm{T}A)^{-1}A^\mathrm{T}\bm{b}\).
     <br>
     &emsp;&ensp;From <a href="https://math.stackexchange.com/questions/1840801/why-is-ata-invertible-if-a-has-independent-columns">Math StackExchange</a>. To prove \(A^\mathrm{T}A\) is invertible we can work on the statement &ldquo;\(A^\mathrm{T}A\bm{x}=\bm{0}\) has only one solution \(\bm{x}=\bm{0}\)&rdquo;. Because \(A^\mathrm{T}\bm{y}=\bm{0}\) may have solutions \(\bm{y}\neq\bm{0}\), we must prove that \(\bm{y}=\bm{0}\) while \(\bm{y}=A\bm{x}\):
     <br>
     &emsp;&ensp;\(A\bm{x}\) is a combination in \(\bm{C}(A)\) which is also in \(\bm{N}(A^\mathrm{T})\). However \(\bm{N}(A^\mathrm{T})\) and \(\bm{C}(A)\) are orthogonal complements, the only vector that can be in both a subspace and the orthogonal complement of its subspace is the zero vector, so \(A\bm{x}\) must produce zero vector.
     <br>
     &emsp;&ensp;For \(A\bm{x}=\bm{0}\) there is obviously only one solution \(\bm{x}=\bm{0}\). Then \(A^\mathrm{T}A\bm{x}=\bm{0}\harr\bm{x}=\bm{0}\) if \(A\) has independent columns.
   </details>
6. The projection matrix onto \\(\bm{C}(A)\\) is \\(\boxed{P=A(A^\mathrm{T}A)^{-1}A^\mathrm{T}}\\). It has \\(\bm{p}=P\bm{b}\\) and \\(P^2=P=P^\mathrm{T}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">4.2 A</span>&emsp;Project the vector \(\bm{b}=(3,4,4)\) onto the line through \(\bm{a}=(2,2,1)\) and then onto the plane that also contains \(\bm{a}^*=(1,0,0)\). Check that the first error vector \(\bm{b}-\bm{p}\) is perpendicular to \(\bm{a}\), and the second error vector \(\bm{e}^*=\bm{b}-\bm{p}^*\) is also perpendicular to \(\bm{a}^*\).
      <br>
      &emsp;&ensp;Find the 3 by 3 projection matrix \(P\) onto that plane of \(\bm{a}\) and \(\bm{a}^*\). Find a vector whose <em>projection onto the plane is the zero vector</em>. Why is it exactly the error \(\bm{e}^*\)?
      <br><br>
      <span class="list-head">Solution</span>&emsp;The projection of \(\bm{b}=(3,4,4)\) onto the line through \(\bm{a}=(2,2,1)\) is \(\bm{p}=2\bm{a}\).
      $$
      \textbf{Onto a line}\qquad
      \bm{p}=\frac{\bm{a^\mathrm{T}b}}{\bm{a^\mathrm{T}a}}\bm{a}=\frac{18}{9}(2,2,1)=(4,4,2)=2\bm{a}.
      $$
      The error vector \(\bm{e}=\bm{b}-\bm{p}=(-1,0,2)\) is perpendicular to \(\bm{a}=(2,2,1)\). So \(\bm{p}\) is correct.
      <br>
      &emsp;&ensp;The plane of \(\bm{a}=(2,2,1)\) and \(\bm{a}^*=(1,0,0)\) is the column space of \(A=\begin{bmatrix} \bm{a} & \bm{a}^* \end{bmatrix}\):
      $$
      A=\begin{bmatrix} 2 & 1 \\ 2 & 0 \\ 1 & 0 \end{bmatrix}\quad
      A^\mathrm{T}A=\begin{bmatrix} 9 & 2 \\ 2 & 1 \end{bmatrix}\quad
      (A^\mathrm{T}A)^{-1}=\frac{1}{5}\begin{bmatrix*}[r] 1 & -2 \\ -2 & 9 \end{bmatrix*}\quad
      P=\begin{bmatrix} 1 & 0 & 0 \\ 0 & .8 & .4 \\ 0 & .4 & .2 \end{bmatrix}
      $$
      Now \(\bm{p}^*=P\bm{b}=(3,4.8,2.4\). The error \(\bm{e}^*=\bm{b}-\bm{p}^*=(0,-.8,1.6\) is perpendicular to \(\bm{a}\) and \(\bm{a}^*\). This \(\bm{e}^*\) is in the nullspace of \(P\) and <em>its projection is zero</em>! Note \(P^2=P=P^\mathrm{T}\).
    </li>
    <li>
      <span class="list-head">4.2 B</span>&emsp;Suppose your pulse is measured at \(x=70\) beats per minute, then at \(x=80\), then at \(x=120\). Those three equations \(A\bm{x}=\bm{b}\) in one unknown have \(A^\mathrm{T}=\begin{bmatrix} 1 & 1 & 1 \end{bmatrix}\) and \(\bm{b}=(70,80,120)\). <em><strong>The best \(\widehat{\bm{x}}\) is the ____ of \(\bm{70,80,120}\).</strong></em> Use calculus and projection:
      <ol>
        <li>
          Minimize \(E=(x-70)^2+(x-80)^2+(x-120)^2\) by solving \(\mathrm{d}E/\mathrm{d}x=0\).
        </li>
        <li>
          Project \(\bm{b}=(70,80,120)\) onto \(\bm{a}=(1,1,1)\) to find \(\widehat{x}=\bm{a}^\mathrm{T}\bm{b}/\bm{a}^\mathrm{T}\bm{a}\).
        </li>
      </ol>
      <span class="list-head">Solution</span>&emsp;The colsest horizontal line to the heights 70, 80 120 is the <em>average</em> \(\widehat{x}=90\):
      $$
      \begin{array}{l}
        \dfrac{\mathrm{d}E}{\mathrm{d}x}=2(x-70)+2(x-80)+2(x-120)=0
        \quad\text{gives}\quad
        \widehat{x}=\dfrac{70+80+120}{3}=90
        .
      \\
        \textbf{Also by projection:}\qquad
        \widehat{x}=\dfrac{\bm{a^\mathrm{T}b}}{\bm{a^\mathrm{T}a}}=\dfrac{(1,1,1)^\mathrm{T}(70,80,120)}{(1,1,1)^\mathrm{T}(1,1,1)}=\dfrac{70+80+120}{3}=90
        .
      \end{array}
      $$
      In <em>recursive</em> least squares, a fourth measurement 130 changes the average \(\widehat{x}_\text{old}=90\) to \(\widehat{x}_\text{new}=100\). Verify the <em>updage formula</em> \(\widehat{x}_\text{new}=\widehat{x}_\text{old}+\frac{1}{4}(130-\widehat{x}_\text{old})\). When a new measurement arrives, we don't have to average all the old measurements again!
    </li>
  </ol>
</details>

### Least Squares Approximations

1. Solving \\(\boxed{A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}}\\) gives the projection \\(\bm{p}=A\widehat{\bm{x}}\\) of \\(\bm{b}\\) onto the column space of \\(A\\).
2. When \\(A\bm{x}=\bm{b}\\) has no solution, \\(\widehat{\bm{x}}\\) is the "leqst-squares solution": \\(\\|\bm{b}-A\widehat{\bm{x}}\\|^2=\text{minimum}\\).
3. Setting partial derivatives of \\(E=\\|A\bm{x}-\bm{b}\\|^2\\) to zero \\(\left(\dfrac{\partial E}{\partial x_i}=0\right)\\) also produces \\(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\\).
   <details>
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
       Treat \(E=\|A\bm{x}-\bm{b}\|^2\) as a error function. Most functions are minimized by calculus! The graph bottoms out and the derivative in every direction is zero.
       <br>
       &emsp;&ensp;For example:
       $$
       \begin{array}{c}
         A=\begin{bmatrix} 1 & 0 \\ 1 & 1 \\ 1 & 2 \end{bmatrix}
         \qquad
         \bm{x}=\begin{bmatrix} C \\ D \end{bmatrix}
         \qquad
         \bm{b}=\begin{bmatrix} 6 \\ 0 \\ 0 \end{bmatrix}
       \\\\
         E=\|A\bm{x}-\bm{b}\|^2=(C+D\cdot 0-6)^2+(C+D\cdot 1)^2+(C+D\cdot 2)^2
       \end{array}
       $$
       Here the function \(E\) to be minimized is a <em>sum of squares</em> \(e_1^2+e_2^2+e_3^2\) (the square of the error in each equation).
       <br>
       &emsp;&ensp;The unknowns are \(C\) and \(D\). With two unknowns there are <em>two derivatives</em>&mdash;both zero at the minimum. They are &ldquo;partial derivatives&rdquo; because \(\partial E/\partial D\) treats \(D\) as constant and \(\partial E/\partial D\) treats \(C\) as constant:
       $$
       \begin{alignedat}{3.5}
         & \partial E/\partial C=2(C+D\cdot 0-6)         && +2(C+D\cdot 1)         && +2(C+D\cdot 2)         & =0 \\
         & \partial E/\partial D=2(C+D\cdot 0-6)(\bm{0}) && +2(C+D\cdot 1)(\bm{1}) && +2(C+D\cdot 2)(\bm{2}) & =0
       \end{alignedat}
       $$
       \(\partial E/\partial D\) contains the extra factors \(\bm{0}\), \(\bm{1}\), \(\bm{2}\) from the chain rule. Those factors are just \(\bm{1}\), \(\bm{1}\), \(\bm{1}\) in \(\partial E/\partial C\)
       <br>
       &emsp;&ensp;It is no accident that those factors \(\bm{1}\), \(\bm{1}\), \(\bm{1}\) and \(\bm{0}\), \(\bm{1}\), \(\bm{2}\) in the derivatives of \(\|A\bm{x}-\bm{b}\|^2\) are the columns of \(A\). Now cancel 2 from every term and collect all \(C\)'s and \(D\)'s:
       $$
       \begin{array}{l}
         \text{The $C$ derivative is zero:}\quad 3C+3D=6 \\
         \text{The $D$ derivative is zero:}\quad 3C+5D=0
       \end{array}
       \quad
       \textbf{This matrix $\begin{bmatrix} 3 & 3 \\ 5 & 5 \end{bmatrix}$ is $A^\mathrm{T}A$}
       $$
       <em><strong>These equations are identical with</strong></em> \(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\). The best \(C\) and \(D\) are the components of \(\widehat{\bm{x}}\). The equations from calculus are the same as the &ldquo;normal equations&rdquo; from linear algebra.
   </details>
4. To fit points \\((t_1,b_1),\ldots,(t_m,b_m)\\) by a straight line, \\(A\\) has columns \\(1,\ldots,1\\) and \\(t_1,\ldots,t_m\\).
5. In that case \\(A^\mathrm{T}A\\) is the 2 by 2 matrix \\(\begin{bmatrix} m & \bm{\Sigma}t_i \\\ \bm{\Sigma}t_i & \bm{\Sigma}t_i^2 \end{bmatrix}\\) and \\(A^\mathrm{T}\bm{b}\\) is the vector \\(\begin{bmatrix*}[l] \bm{\Sigma}b_i \\\ \bm{\Sigma}t_ib_i \end{bmatrix*}\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">4.3 A</span>&emsp;Start with nine measurements \(b_1\) to \(b_9\), <em>all zero,</em> at time \(t=1,\ldots,9\). The tenth measurement \(b_{10}=40\) is an outlier. Find the best horizontal line \(\bm{y=C}\) to fit the ten points \((1,0)\), \((2,0)\), &mldr;, \((9,0)\), \((10,40)\) using three options for the error \(E\):
      <br>
      <b>(1)</b>&ensp;Least <em>squares</em> \(E_2=e_1^2+\cdots+e_{10}^2\) (then the normal equation for \(C\) is linear).
      <br>
      <b>(2)</b>&ensp;Least <em>maximum</em> error \(E_\infty=|e_\text{max}|\).
      <br>
      <b>(3)</b>&ensp;Least <em>sum</em> of errors \(E_1=|e_1|+\cdots+|e_{10}|\).
      <br>
      <span class="list-head">Solution</span>&emsp;<b>(1)</b>&ensp;The least squares fit to \(0,0,\ldots,0,40\) by a horizontal line is \(\bm{C=4}\):
      $$
      A=\text{column of 1's}\quad
      A^\mathrm{T}A=10\quad
      A^\mathrm{T}\bm{b}=\text{sum of $b_i=40$.\quad So $10C=40$.}
      $$
      <b>(2)</b>&ensp;The least maximum error requires \(\bm{C=20}\), halfway between 0 amd 40.
      <br>
      <b>(3)</b>&ensp;The least sum requires \(\bm{C=0}\) (!!). The sum of errors \(9|C|+|40-C|\) would increase if \(C\) moves up from zero.
      <br>
      &emsp;&ensp;The least sum comes from the <em>median</em> measurement (the median of \(0,\ldots,0,40\) is zero). Many statisticians feel that the least squares solution is too heavily influenced by outliers like \(b_{10}=40\), and they prefer least sum. But the equations become <em>nonlinear</em>.
      <br>
      &emsp;&ensp;Now find the least squares line \(C+D\bm{t}\) through those ten points \((1,0)\) to \((10,40)\):
      $$
      A^\mathrm{T}A=
      \begin{bmatrix} 10 & \sum t_i  \\ \sum t_i & \sum t_i^2  \end{bmatrix}
      =
      \begin{bmatrix} 10 & 55 \\ 55 & 385 \end{bmatrix}
      \qquad
      A^\mathrm{T}\bm{b}=
      \begin{bmatrix} \sum b_i \\ \sum t_ib_i \end{bmatrix}
      =
      \begin{bmatrix} 40 \\ 400 \end{bmatrix}
      $$
      Those come from equation (8). Then \(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\) gives \(C=-8\) and \(D=24/11\).
      <details>
        <summary><span class="list-summary">&dagger; Equation (8) &dagger;</span></summary>
        The line \(C+D\bm{t}\) minimizes \(e_1^2+\cdots+e_m^2=\|A\bm{x}+\bm{b}\|^2\) when \(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\):
        $$
        \begin{equation}\tag{8}
          A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\qquad
          \begin{bmatrix} m & \sum t_i \\ \sum t_i & \sum t_i^2 \end{bmatrix}
          \begin{bmatrix} C \\ D \end{bmatrix}
          =
          \begin{bmatrix} \sum b_i \\ \sum t_ib_i \end{bmatrix}
          .
        \end{equation}
        $$
      </details>
      &emsp;&ensp;What happens to \(C\) and \(D\) if you multiply \(\bm{b}=(0,0,\ldots,40)\) by 3 and then add 30 to get \(\bm{b}_\text{new}=(30,30,\ldots,150)\)? Linearity allows us to rescale \(\bm{b}\). Multiplying \(\bm{b}\) by 3 will multiply \(C\) and \(D\) by 3. Adding 30 to all \(b_i\) will add 30 to \(C\).
    </li>
    <li>
      <span class="list-head">4.3 B</span>&emsp;Find the parabola \(C+Dt+Et^2\) that comes closest (least squares error) to the values \(\bm{b}=(0,0,1,0,0)\) at the times \(t=-2,-1,0,1,2\). First write down the five equations \(A\bm{x}=\bm{b}\) in three unknowns \(\bm{x}=(C,D,E)\) for a parabola to go through the five points. No solution because no such parabola exists. Solve \(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\).
      <br>
      &emsp;&ensp;I would predict \(D=0\). Why should the best parabola be symmetric around \(t=0\)? In \(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\), equation 2 for \(D\) should uncouple from equations 1 and 3.
      <br>
      <span class="list-head">Solution</span>&emsp;The five equations \(A\bm{x}=\bm{b}\) have a rectangular <em>Vandermonde matrix</em> \(A\):
      $$
      \begin{alignedat}{3.5}
        C+D && (-2)+E && (-2)^2=0 \\
        C+D && (-1)+E && (-1)^2=0 \\
        C+D &&  (0)+E && (0)^2=1 \\
        C+D &&  (1)+E && (1)^2=0 \\
        C+D &&  (2)+E && (2)^2=0 \\
      \end{alignedat}
      \quad
      A=
      \begin{bmatrix*}[r]
        1 & -2 & 4 \\
        1 & -1 & 1 \\
        1 & 0 & 0 \\
        1 & 1 & 1 \\
        1 & 2 & 4
      \end{bmatrix*}
      \quad
      A^\mathrm{T}A=\begin{bmatrix} 5 & \bm{0} & 10 \\ \bm{0} & 10 & \bm{0} \\ 10 & \bm{0} & 34 \end{bmatrix}
      $$
      Those zeros in \(A^\mathrm{T}A\) mean that column 2 of \(A\) is orthogonal to columns 1 and 3. We see this directly in \(A\) (the times \(-2,-1,0,1,2\) are symmetric). The best \(C\), \(D\), \(E\) in the parabola \(C+Dt+Et^2\) come from \(A^\mathrm{T}A\widehat{\bm{x}}=A^\mathrm{T}\bm{b}\), and \(D\) is uncoupled from \(C\) and \(E\):
      $$
      \begin{bmatrix} 5 & 0 & 10 \\ 0 & 10 & 0 \\ 10 & 0 & 34 \end{bmatrix}
      \begin{bmatrix} C \\ D \\ E \end{bmatrix}
      =
      \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}
      \quad\text{leads to}\quad
      \begin{array}{l}
        C=34/70 \\
        D=0\text{ as predicted} \\
        E=-10/70
      \end{array}
      $$
    </li>
  </ol>
</details>

### Orthonormal Bases and Gram-Schmidt

1. The columns \\(\bm{q}_1,\ldots,\bm{q}_n\\) are orthonormal if \\(\bm{q}_i^\mathrm{T}\bm{q}_j=\left\\{\begin{array}{l} \text{0 for $i\neq j$} \\\ \text{1 for $i=j$} \end{array}\right\\}\\). Then \\(\boxed{Q^\mathrm{T}Q=I}\\).
2. If \\(Q\\) is also square, then \\(QQ^\mathrm{T}=I\\) and \\(\boxed{Q^\mathrm{T}=Q^{-1}}\\). \\(Q\\) is an "orthogonal matrix".
3. The least squares solution to \\(Q\bm{x}=\bm{b}\\) is \\(\widehat{\bm{x}}=Q^\mathrm{T}\bm{b}\\). Projection of \\(\bm{b}\\): \\(\bm{p}=QQ^\mathrm{T}\bm{b}=P\bm{b}\\).
4. The **Gram-Schmidt** process takes independent \\(\bm{a}_i\\) to orthonormal \\(\bm{q}_i\\). Start with \\(\bm{q}_1=\bm{a}_1/\\|\bm{a}_1\\|\\).
5. \\(\bm{q}_i\\) is \\((\bm{a}_i-\text{projection }\bm{p}_i)/\\|\bm{a}_i-\bm{p}_i\\|\\); projection \\(\bm{p}\_i=(\bm{a}\_i^\mathrm{T}\bm{q}\_1)\bm{q}\_1+\cdots+(\bm{a}\_i^\mathrm{T}\bm{q}\_{i-1})\bm{q}\_{i-1}\\).
   <details>
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
     The one and only idea of the Gram-Schmidt process is that <em><strong>subtract from every new vector its projections in the directions already set</strong></em>.
     <br>
     &emsp;&ensp;Begin by choosing \(\bm{\alpha}=\bm{a}\), the first direction is accepted as it comes. Then (at the end may be easiest) divide its length to get \(\bm{q}_i=\bm{\alpha}_i/\|\bm{\alpha}_i\|\). Start with \(\bm{a}_2\) subtract its projection along \(\bm{a}_1\). This leaves the perpendicular part, which is the orthogonal vector \(\bm{\alpha}_2\):
     $$
     \textbf{First Gram-Schmidt step}\quad
     \begin{alignedat}{1}
       \bm{\alpha}_2
     & =
       \bm{a}_2-\bm{\alpha}_1(\cfrac{\bm{\alpha}_1^\mathrm{T}\bm{a}_2}{\bm{\alpha}_1^\mathrm{T}\bm{\alpha}_1})
     \\
     & =
       \bm{a}_2-\cfrac{\bm{\alpha}_1(\bm{\alpha}_1^\mathrm{T}\bm{a}_2)}{\|\bm{\alpha}_1\|^2}
     \\
     & =
       \bm{a}_2-\bm{q}_1(\bm{q}_1^\mathrm{T}\bm{a}_2)
     \end{alignedat}
     $$
     The third direction starts with \(\bm{a}_3\). Subtract off its components in those two directions to get a perpendicular direction \(\bm{\alpha}_3\):
     $$
     \textbf{Next Gram-Schmidt step}\quad
     \begin{alignedat}{1}
       \bm{\alpha}_3
     & =
       \bm{a}_3-\bm{\alpha}_1(\cfrac{\bm{\alpha}_1^\mathrm{T}\bm{a}_3}{\bm{\alpha}_1^\mathrm{T}\bm{\alpha}_1})-\bm{\alpha}_2(\cfrac{\bm{\alpha}_2^\mathrm{T}\bm{a}_3}{\bm{\alpha}_2^\mathrm{T}\bm{\alpha}_2})
     \\
     & =
       \bm{a}_3-\bm{q}_1(\bm{q}_1^\mathrm{T}\bm{a}_3)-\bm{q}_2(\bm{q}_2^\mathrm{T}\bm{a}_3)
     \end{alignedat}
     $$
   </details>
6. Each \\(\bm{a}_i\\) will be a combination of \\(\bm{q}_1\\) and \\(\bm{q}_i\\). Then \\(\bm{A=QR}\\): orthogonal \\(Q\\) and triangular \\(R\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">4.4 A</span>&emsp;Add two more columns with all entries 1 or &minus;1, so the columns of this 4 by 4 &ldquo;Hadamard matrix&rdquo; are orthogonal. How do you turn \(H_4\) into an <em>orthogonal matrix</em> \(Q\)?
      $$
      H_2=\begin{bmatrix*}[r] 1 & 1 \\ 1 & -1 \end{bmatrix*}
      \quad
      H_4=
      \begin{bmatrix*}[r]
        1 & 1 & x & x \\
        1 & -1 & x & x \\
        1 & 1 & x & x \\
        1 & -1 & x & x
      \end{bmatrix*}
      \quad\text{and}\quad
      Q_4=
      \begin{bmatrix}
        & & & \\
        & & & \\
        & & & \\
        & & &
      \end{bmatrix}
      $$
      The block matrix \(H_8=\begin{bmatrix*}[r] H_4 & H_4 \\ H_4 & -H_4 \end{bmatrix*}\quad\begin{array}{l} \text{is the next Hadamard matrix with 1's and $\mathord{-}1$'s.} \\ \text{What is the product $H_8^\mathrm{T}H_8$?} \end{array}\)
      <br>
      The projection of \(\bm{b}=(6,0,0,2)\) onto the first column of \(H_4\) is \(\bm{p}_1=(2,2,2,2)\). The projection onto the second column is \(\bm{p}_2=(1,-1,1,-1)\). What is the projection \(\bm{p}_{1,2}\) of \(\bm{b}\) onto the 2-dimensional space spanned by the first two columns?
      <span class="list-head">Solution</span>&emsp;\(H_4\) can be built from \(H_2\) just as \(H_8\) is built from \(H_4\):
      $$
      H_4=
      \begin{bmatrix*}[r] H_2 & H_2 \\ H_2 & -H_2 \end{bmatrix*}
      =
      \begin{bmatrix*}[r]
        1 & 1 & 1 & 1 \\
        1 & -1 & 1 & -1 \\
        1 & 1 & -1 & -1 \\
        1 & -1 & -1 & 1
      \end{bmatrix*}
      \quad
      \text{has orthogonal columns.}
      $$
      Then \(Q=H/2\) has orthonormal columns. Dividing by 2 gives unit vectors in \(Q\). A 5 by 5 Hadamard matrix is impossible because the dot product of columns would have five 1's and/or &minus;1's and could not add to zero. \(H_8\) has orthogonal columns of length \(\sqrt{8}\).
      $$
      H_8^\mathrm{T}H_8=
      \begin{bmatrix*}[r] H^\mathrm{T} & H^\mathrm{T} \\ H^\mathrm{T} & -H^\mathrm{T} \end{bmatrix*}
      \begin{bmatrix*}[r] H & H \\ H & -H \end{bmatrix*}
      =
      \begin{bmatrix} 2H^\mathrm{T}H & 0 \\ 0 & 2H^\mathrm{T}H \end{bmatrix}
      =
      \begin{bmatrix} 8I & 0 \\ 0 & 8I \end{bmatrix}
      .\space
      Q_8=\frac{H_8}{\sqrt{8}}
      $$
    </li>
    <li>
      <span class="list-head">4.4 B</span>&emsp;<strong>What is the key point of orthogonal columns?</strong> Answer: \(A^\mathrm{T}A\) is diagonal and easy to invert. <strong>We can project onto lines and just add.</strong> The axes are orthogonal.
      $$
      \textbf{Add $\bm{p}$'s}\quad\text{Projection $\bm{p}_{1,2}$ onto a plane equals $\bm{p}_1+\bm{p}_2$ onto orthogonal lines.}
      $$
    </li>
  </ol>
</details>

## Determinants

### The properties of Determinants

1. The **determinant** of \\(A=\begin{bmatrix} a & b \\\ c & d \end{bmatrix}\\) is \\(\bm{ad-bc}\\). Singular matrix \\(A=\begin{bmatrix} a & xa \\\ c & xc \end{bmatrix}\\) has \\(\bm{\det=0}\\)
2. \\(\begin{array}{l} \textbf{Row exchange} \\\ \textbf{reverses signs} \end{array} PA=\begin{bmatrix} 0 & 1 \\\ 1 & 0 \end{bmatrix}\begin{bmatrix} a & b \\\ c & d \end{bmatrix}=\begin{bmatrix} c & d \\\ a & b \end{bmatrix}\quad\text{has }\det PA=bc-ad=\bm{-\det A}\\).
3. The determinant of \\(\begin{bmatrix} xa+yA & xb+yB \\\ c & d \end{bmatrix}\\) is \\(x(ad-bc)+y(Ad-Bc)\\). \\(\begin{array}{l} \textbf{Det is linear in} \\ \textbf{row 1 by itself.} \end{array}\\)
   <figure class="highlight diff">
     <table>
       <tr>
         <td class="gutter">
           <span class="line">1</span>
         </td>
         <td class="code">
           <span class="line">
             <span class="deletion">
               &minus; The determinant of \(\begin{bmatrix} xa+yA & xb+yB \\ c & d \end{bmatrix}\) is \(x(ad-bc)+y(Ad-Bc)\). \(\begin{array}{l} \textbf{Det is linear in} \\ \textbf{row 1 by itself.} \end{array}\)
             </span>
           </span>
         </td>
       </tr>
       <tr>
         <td class="gutter">
           <span class="line">2</span>
         </td>
         <td class="code">
           <span class="line">
             <span class="addition">
               + <em>The determinant is a linear function of each row separately</em> (all other rows stay fixed), see in example 5.1 A
             </span>
           </span>
         </td>
       <tr>
     </table>
   </figure>
4. **Elimination** \\(EA=\begin{bmatrix} a & b \\\ 0 & d-\dfrac{c}{a}b \end{bmatrix}\quad\bm{\det EA}=a(d-\dfrac{c}{a}b)=\textbf{product of pivots}=\bm{\det A}\\).
5. If \\(A\\) is \\(n\\) by \\(n\\) then the four statements above remain true: \\(\bm{\det=0}\\) when \\(A\\) is singular, **det reverses sign** when row are exchanged, **det is linear in row 1 by itself,** \\(\det=\textbf{product of the pivots}\\). Always \\(\bm{\det BA=(\det B)(\det A)}\\) and \\(\bm{\det A^\textbf{\textrm{T}}=\det A}\\). This is an amazing number.

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">5.1 A</span>&emsp;Apply these operations to \(A\) and find the determinants of \(M_1\), \(M_2\), \(M_3\), \(M_4\):
      <br>
      &emsp;In \(M_1\), multiplying each \(a_{ij}\) by \((-1)^{i+j}\) gives a checker board sign pattern.
      <br>
      &emsp;In \(M_2\), rows 1, 2, 3 of \(A\) are <em>subtracted</em> from rows 2, 3, 1.
      <br>
      &emsp;In \(M_3\), rows 1, 2, 3 of \(A\) are <em>added</em> to rows 2, 3, 1.
      <br>
      <em>How are the determinants of \(M_1\), \(M_2\), \(M_3\) related to the determinant of \(A\)?</em>
      $$
      \begin{bmatrix*}[r]
        a_{11} & -a_{12} & a_{13} \\
        -a_{21} & a_{22} & -a_{23} \\
        a_{31} & -a_{32} & a_{33}
      \end{bmatrix*}
      \quad
      \begin{bmatrix}
        \text{row 1 $-$ row 3} \\
        \text{row 2 $-$ row 1} \\
        \text{row 3 $-$ row 2}
      \end{bmatrix}
      \quad
      \begin{bmatrix}
        \text{row 1 $+$ row 3} \\
        \text{row 2 $+$ row 1} \\
        \text{row 3 $+$ row 2}
      \end{bmatrix}
      $$
      <span class="list-head">Solution</span>&emsp;The three determinanats are \(\det A\), \(0\), and \(2\det A\). Here are reasons:
      \(M_1=\begin{bmatrix} 1 & & \\ & -1 & \\ & & 1 \end{bmatrix}\begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{bmatrix}\begin{bmatrix} 1 & & \\ & -1 & \\ & & 1 \end{bmatrix}\quad\text{so $\det M_1=(-1)(\det A)(-1)$.}\)
      <br>
      \(M_2\) is singular because its rows add to the zero row. Its determinant is zero.
      <br>
      \(M_3\) can be split into <em>eight matrices</em> by Rule 3 (linearity in each row separately):
      $$
      \begin{vmatrix}
        \text{row 1 $+$ row 3} \\
        \text{row 2 $+$ row 1} \\
        \text{row 3 $+$ row 2}
      \end{vmatrix}
      =
      \begin{vmatrix}
        \text{row 1} \\
        \text{row 2} \\
        \text{row 3}
      \end{vmatrix}
      +
      \begin{vmatrix}
        \text{row 3} \\
        \text{row 2} \\
        \text{row 3}
      \end{vmatrix}
      +
      \begin{vmatrix}
        \text{row 1} \\
        \text{row 1} \\
        \text{row 3}
      \end{vmatrix}
      +\cdots+
      \begin{vmatrix}
        \text{row 3} \\
        \text{row 1} \\
        \text{row 2}
      \end{vmatrix}
      .
      $$
      All but the first and last have repeated rows and zero determinant. The first is \(A\) and the last has <em>two</em> row exchanges. So \(\det M_3=\det A+\det A\). (Try \(A=I\).)
    </li>
    <li>
      <span class="list-head">5.1 B</span>&emsp;Explain how to reach this determinant by row operations:
      $$
      \det\begin{bmatrix} 1-a & 1 & 1 \\ 1 & 1-a & 1 \\ 1 & 1 & 1-a \end{bmatrix}=a^2(3-a)
      .
      $$
      <span class="list-head">Solution</span>&emsp;Subtract row 3 from row 1 and then from row 2. This leaves
      $$
      \det\begin{bmatrix} -a & 0 & a \\ 0 & -a & a \\ 1 & 1 & 1-a \end{bmatrix}
      .
      $$
      Now add column 1 to column 3, and also column 2 to column 3. This leaves a lower triangular matrix with \(-a\), \(-a\), \(3-a\) on the diagonal: \(\det=(-a)(-a)(3-a)\).
      <br>
      &emsp;&ensp;The determinant is zero if \(a=0\) or \(a=3\). For \(a=0\) we have the <em>all-ones matrix</em>&mdash;certainly singular. For \(a=3\), each row adds to zero&mdash;again singular. Those numbers 0 and 3 are the <strong>eigenvalues</strong> of the all-ones matrix. This example is revealing and important, leading toward Chapter 6.
    </li>
  </ol>
</details>

### Permutations and Cofactors

1. **2 by 2**: \\(ad-bc\\) has \\(2!\\) terms with \\(\pm\\) signs. **\\(\bm{n}\\) by \\(\bm{n}\\): \\(\bm{\det A}\\) adds \\(\bm{n!}\\) with \\(\bm{\pm}\\) signs.**
2. For \\(n=3\\), \\(\det A\\) adds \\(3!=6\\) terms. Two terms are \\(+a_{12}a_{23}a_{31}\\) and \\(-a_{13}a_{22}a_{31}\\).
3. That minus sign came because the column order 3, 2, 1 needs one exchange to recover 1, 2, 3.
4. The six terms include \\(+a_{11}a_{22}a_{33}-a_{11}a_{23}a_{32}=a_{11}(\bm{a_{22}a_{33}-a_{23}a_{32}})=a_{11}(\textbf{cofactor }\bm{C_{11}})\\).
5. Always \\(\det A=a_{11}C_{11}+a_{12}C_{12}+\cdots+a_{1n}C_{1n}\\). Cofactors are determinants of size \\(n-1\\).

<details>
  <summary><span class="list-summary">&dagger; WORKED EXAMPLES &dagger;</span></summary>
  <ol class="worked-examples">
    <li>
      <span class="list-head">5.2 A</span>&emsp;A <em>Hessenberg matrix</em> is a triangular matrix with one extra diagonal. Use cofactors of row 1 to show that the 4 by 4 determinant satisfies Fibonacci's rule \(|H_4|=|H_3|+|H_2|\). The same rule will continue for all sizes, \(|H_n|=|H_{n-1}|+|H_{n-2}|\). Which Fibonacci number is \(|H_n|\)?
      $$
      H_2=\begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}
      \qquad
      H_3=\begin{bmatrix} 2 & 1 & \\ 1 & 2 & 1 \\ 1 & 1 & 2 \end{bmatrix}
      \qquad
      H_4=
      \begin{bmatrix}
        2 & 1 & & \\
        \bm{1} & 2 & \bm{1} & \\
        \bm{1} & 1 & \bm{2} & \bm{1} \\
        \bm{1} & 1 & \bm{1} & \bm{2}
      \end{bmatrix}
      $$
      <span class="list-head">Solution</span>&emsp;The cofactor \(C_{11}\) for \(H_4\) is the determinant \(|H_3|\). We also need \(C_{12}\) (in boldface):
      $$
      C_{12}=-
      \begin{vmatrix}
        \bm{1} & \bm{1} & \bm{0} \\
        \bm{1} & \bm{2} & \bm{1} \\
        \bm{1} & \bm{1} & \bm{2}
      \end{vmatrix}
      =-
      \begin{vmatrix}
        2 & 1 & 0 \\
        1 & 2 & 1 \\
        1 & 1 & 2
      \end{vmatrix}
      +
      \begin{vmatrix}
        1 & 0 & 0 \\
        1 & 2 & 1 \\
        1 & 1 & 2
      \end{vmatrix}
      $$
      Rows 2 and 3 stayed the same and we used linearity in row 1. The two determinants on the right are \(-|H_3|\) and \(+|H_2|\). Then the 4 by 4 determinant is
      $$
      |H_4|=2C_{11}+1C_{12}=2|H_3|-|H_3|+|H_2|=|H_3|+|H_2|.
      $$
      The actual numbers are \(|H_2|=3\) and \(|H_3|=5\) (and of course \(|H_1|=2\)). Since \(|H_n|=2,3,5,8,\ldots\) follows Fibonacci's rule \(|H_{n-1}|+|H_{n-2}|\), it must be \(|H_n|=F_{n+2}\).
    </li>
    <li>
      <span class="list-head">5.2 B</span>&emsp;These questions use the \(\pm\) signs (even and odd \(P\)'s) in the big formula for \(\det A\):
      <table class="list-table">
        <tr>
          <td>&emsp;&ensp;<b>1.</b>&ensp;</td>
          <td>
            If \(A\) is 10 by 10 all-ones matrix, how does the big formula give \(\det A=0\)?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>2.</b>&ensp;</td>
          <td>
            If you multiply all \(n!\) permutations together into a single \(P\), is \(P\) odd or even?
          </td>
        </tr>
        <tr>
          <td>&emsp;&ensp;<b>3.</b>&ensp;</td>
          <td>
            If you multiply each \(a_{ij}\) by the fraction \(i/j\), why is \(\det A\) unchanged?
          </td>
        </tr>
      </table>
      <span class="list-head">Solution</span>&emsp;In Question <b>1</b>, with all \(a_{ij}=1\), all the products in the big formula (8) will be 1. Half of them come with a plus sign, and half with minus. So they cancel to leave \(\det A=0\). (Of course the all-ones matrix is singular. I am assuming \(n\gt 1\).)
      <details>
        <summary><span class="list-summary">&dagger; BIG FORMULA &dagger;</span></summary>
        $$
        \tag{8}
        \begin{alignedat}{1}
          \det A & =\text{sum over all $\bm{n}!$ column permutations $P=(\alpha,\beta,\ldots,\omega)$} \\
                 & = \sum(\det P)a_{1\alpha}a_{2\beta}\cdots a_{n\omega}=\textbf{BIG FORMULA}.
        \end{alignedat}
        $$
        e.g. 3 by 3 matrix \(A=\):
        $$
        \begin{alignedat}{1}
          \det A=
        &
          a_{11}a_{22}a_{33}\begin{bmatrix} 1 & & \\ & 1 & \\ & & 1 \end{bmatrix}
          +a_{12}a_{23}a_{31}\begin{bmatrix} & 1 & \\ & & 1 \\ 1 & & \end{bmatrix}
          +a_{13}a_{21}a_{32}\begin{bmatrix} & & 1 \\ 1 & & \\ & 1 & \end{bmatrix}
        \\
        &
        +a_{11}a_{23}a_{32}\begin{bmatrix} 1 & & \\ & & 1 \\ & 1 & \end{bmatrix}
        +a_{12}a_{21}a_{33}\begin{bmatrix} & 1 & \\ 1 & & \\ & & 1 \end{bmatrix}
        +a_{13}a_{22}a_{31}\begin{bmatrix} & & 1 \\ & 1 & \\ 1 & & \end{bmatrix}
        .
        \end{alignedat}
        $$
      </details>
      &emsp;&ensp;In Question <b>2</b>, multiplying \(\left[\begin{smallmatrix} 1 & 0 \\ 0 & 1 \end{smallmatrix}\right]\left[\begin{smallmatrix} 0 & 1 \\ 1 & 0 \end{smallmatrix}\right]\) gives an odd permutation. Also for 3 by 3, the three odd permutations multiply (in any order) to give <em>odd</em>. But for \(n\gt 3\) the product of all permutations will be <em>even</em>. There are \(n!/2\) odd permutations and that is an even number as soon as \(n!\) includes the factor 4.
      <br>
      &emsp;&ensp;In Question <b>3</b>, each \(a_{ij}\) is multiplied by \(i/j\). So each product \(a_{1\alpha}a_{2\beta}\cdots a_{n\omega}\) in the big formula is multiplied by all the row numbers \(i=1,2,\ldots,n\) and divided by all the column numbers \(j=1,2,\ldots,n\). (The columns come in some permuted order!) Then each product is unchanged and \(\det A\) stays the same.
      <br>
      &emsp;&ensp;Another approach to Question <b>3</b>: We are multiplying the matrix \(A\) by the diagonal matrix \(D=\textbf{\textsf{diag}}(1 : n)\) when row \(i\) is multiplied by \(i\). And we are postmultiplying by \(D^{-1}\) when column \(j\) is divided by \(j\). The determinant of \(DAD^{-1}\) is the same as \(\det A\) by the product rule.
    </li>
  </ol>
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
A^\mathrm{T}A\text{ is symmetric positive definite} & A^\mathrm{T}A\text{ is only semidefinite} \\
A\text{ has $n$ (positive) singular values} & A\text{ has $r\lt n$ singular values}
\end{array}
$$
</div>
