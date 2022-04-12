---
title: Notes of An Introduction to Difference Equations
toc: true
category: learn
date: 2022-04-08 23:29:48
tags:
---

An Introduction to Differen Equations (Third Edition) &mdash; Saber Elaydi

<!-- more -->

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

## Dynamics of First-Order Difference Equations

### Introduction

1. Starting from \\(x_0\\), we have first iterate \\(f(x_0)\\) whereas second iterate \\(f(f(x_0))=f^2(x_0)\\); more generally \\(n\\)th iterate \\(f^n(x_0)\\). Especially \\(f^0(x_0)=x_0\\).
2. The set of all (positive) iterates \\(\\{f^n(x_0):n\geq 0\\}=O(x_0)\\), called (positive) orbit of \\(x_0\\).
3. \\(x(n+1)\\) is a function of the \\(n\\)th iterate number \\(x(n)\\):
   <div>
   $$
   \tag{1.1.1}
   \text{Autonomous, time-invariant system}\qquad
   x(n+1)=f^{n+1}(x_0)=f(x(n))
   $$
   </div>
4. \\(x(n+1)\\) might also have this form. Here, \\(g:\mathbb{Z}^+\times\mathbb{R}\rarr\mathbb{R}\\):
   <div>
   $$
   \tag{1.1.2}
   \text{Nonautonomous, time-variant system}\qquad
   x(n+1)=g(n,x(n))
   $$
   </div>

   If an initial condition \\(x(n_0)=x_0\\) is given, then for \\(n\geq n_0\\), there is a *unique solution* \\(x(n)\equiv x(n,n_0,x_0)\\) of (1.1.2).
   <details>
     <summary><span class="list-summary">&dagger; WHY &dagger;</span></summary>
     This may be shown easily by iteration:
     $$
     \begin{array}{l}
        x(n_0+1)=g(n_0,x(n_0))=g(n_0,x_0) \\
        x(n_0+2)=g(n_0+1,x(n_0+1))=g(n_0+1,g(n_0,x_0)) \\
        x(n_0+3)=g(n_0+2,x(n_0+2))=g[n_0+2,g(n_0+1,g(n_0,x_0))]
     \end{array}
     $$
     Inductively, we get \(x(n)=g[n-1,x(n-1)]=x(n,n_0,x_0)\) because there is unknown \(x_0\), \(n_0\) in \(x(n-1)\)
   </details>

### Linear First-Order Difference Equations

1. Typical linear *homogeneous* first-order equation:
   <div>
   $$
   \tag{1.2.1}
   x(n+1)=a(n)x(n),\quad x(n_0)=x_0,\quad n\geq n_0\geq 0
   $$
   </div>

   and the associated *nonhomogeneous* equation:
   <div>
   $$
   \tag{1.2.2}
   y(n+1)=a(n)y(n)+g(n),\quad y(n_0)=y_0,\quad n\geq n_0\geq 0
   $$
   </div>

   where in both equations it is assumed that \\(a(n)\neq 0\\), and \\(a(n)\\) and \\(g(n)\\) are real-valued functions defined for \\(n\geq n_0\geq 0\\).
2. The solution of (1.2.1) may be obtained by a simple iteration, then we get:
   <div>
   $$
   \tag{1.2.3}
   x(n)=\left[\prod_{i=n_0}^{n-1}a(i)\right]x_0
   $$
   </div>
   <details>
     <summary><span class="list-summary">&dagger; Details &dagger;</span></summary>
     $$
     \begin{array}{l}
       x(n_0+1)=a(n_0)x(n_0)=a(n_0)x_0 \\
       x(n_0+2)=a(n_0+1)x(n_0+1)=a(n_0+1)a(n_0)x_0 \\
       x(n_0+3)=a(n_0+2)x(n_0+2)=a(n_0+2)a(n_0+1)a(n_0)x_0
     \end{array}
     $$
     And, inductively, it is easy to see that \(x(n)=a(n-1)a(n-2)\cdots a(n_0)x_0\)
   </details>
   The unique solution of (1.2.2):
   <div>
   $$
   \tag{1.2.4}
   y(n)=\left[\prod_{i=n_0}^{n-1}a(i)\right]y_0+\sum_{r=n_0}^{n-1}\left[\prod_{i=r+1}^{n-1}a(i)\right]g(r)
   $$
   </div>
   This may be found by iteration and can be proof by using mathematical induction. Also notice that \(\prod_{i=n}^{n-1}a(i)=1\).
   <details>
     <summary><span class="list-summary">&dagger; Details &dagger;</span></summary>
     $$
     \begin{alignedat}{1}
       y(n_0+1)= & a(n_0)y_0+g(n_0) \\
       y(n_0+2)= & a(n_0+1)y(n_0+1)+g(n_0+1) \\
               = & a(n_0+1)a(n_0)y_0+a(n_0+1)g(n_0)+g(n_0+1) \\
       y(n_0+3)= & a(n_0+2)y(n_0+2)+g(n_0+2) \\
               = & a(n_0+2)a(n_0+1)a(n_0)y_0+a(n_0+2)a(n_0+1)g(n_0) \\
                & +a(n_0+2)g(n_0+1)+g(n_0+2)
     \end{alignedat}
     $$
     Then we see the form of (1.2.4). To establish this, assume that formaula (1.2.4) holds for \(n=k\). Then from (1.2.2), \(y(k+1)=a(k)y(k)+g(k)\), which by formula (1.2.4) yields:
     $$
     \begin{alignedat}{1}
       y(k+1)= & a(k)\left[\prod_{i=n_0}^{k-1}a(i)\right]y_0+\sum_{r=n_0}^{k-1}\left[a(k)\prod_{i=r+1}^{k-1}a(i)\right]g(r)+g(k) \\
             = & \left[\prod_{i=n_0}^k a(i)\right]y_0+\sum_{r=n_0}^{k-1}\left[\prod_{i=r+1}^k a(i)\right]g(r)+\left[\prod_{i=k+1}^ka(i)\right]g(k) \\
             = & \left[\prod_{i=n_0}^k a(i)\right]y_0+\sum_{r=n_0}^{k}\left[\prod_{i=r+1}^k a(i)\right]g(r)
     \end{alignedat}
     $$
     Hence formula (1.2.4) holds for all \(n\in\mathbb{Z}^+\).
   </details>
