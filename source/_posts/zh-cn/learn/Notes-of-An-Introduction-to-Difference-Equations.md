---
title: Notes of An Introduction to Difference Equations
toc: true
category: learn
date: 2022-04-08 23:29:48
tags:
---

An Introduction to Differen Equations (Third Edition) &mdash; Saber Elaydi

<!-- more -->

## Dynamics of First-Order Difference Equations

### Introduction

1. Starting from \\(x_0\\), we have first iterate \\(f(x_0)\\) whereas second iterate \\(f(f(x_0))=f^2(x_0)\\); more generally \\(n\\)th iterate \\(f^n(x_0)\\). Especially \\(f^0(x_0)=x_0\\).
2. The set of all (positive) iterates \\(\\{f^n(x_0) : n\geq 0\\}=O(x_0)\\), called (positive) orbit of \\(x_0\\).
3. \\(x(n+1)\\) is a function of the \\(n\\)th iterate number \\(x(n)\\):
   <div>
   $$
   x(n+1)=f^{n+1}(x_0)=f(x(n))
   $$
   </div>
