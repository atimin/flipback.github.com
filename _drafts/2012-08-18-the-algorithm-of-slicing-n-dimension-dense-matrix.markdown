---
layout: post
title: "The Algorithm of slicing N-dimension dense matrix"
date: 2012-08-18 18:27
comments: true
published: false
categories: 
---

## Introduction

In last [post]({{ site.url }}) I promised to write several articles about algorithms slicing for N-dimension matrix which I exploited in the [NMatrix]() project. The article is my attempt to describe the first algorithm. It's slicing N-dimension **dense** matrix.

<!-- more -->

## Data structures

Before we begin to discover algorithm we must look structures of data which will be using. And so, what is dense matrix? It's matrix whose elements store in linear array. For example, 3 dimension matrix _M\[i,j,k\]_:

$$
\text {Shape: } \mathbf{S} = \begin{bmatrix} 3 & 3 & 3 \end{bmatrix}
$$

$$
\mathbf{M}_1  =  \begin{vmatrix}
0 & 1 & 2 \\
3 & 4 & 5 \\
6 & 7 & 8
\end{vmatrix} \text{, i=1}
$$

$$
\mathbf{M}_2 = \begin{vmatrix}
9 & 10 & 11 \\
12 & 13 & 14 \\
15 & 16 & 17 \\
\end{vmatrix} \text{, i=2}
$$

$$
\mathbf{M}_3 = \begin{vmatrix}
18 & 19 & 20 \\
21 & 22 & \underline{\bf{23}} \\
24 & 25 & 26 \\
\end{vmatrix} \text{, i=3}
$$


In memory it storages:

$$
\mathbf{A} =  \begin{bmatrix} 
0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & 16 & 17 & 18 & 19 & 20 & 21 & 22 & \underline{\bf{23}} & 24 & 25 & 26 
\end{bmatrix}
$$

Alway then we access to elements of matrix _M\[i,j,k\]_ we must compute index(_n_) of array elements:

$$
n = \sum_{i=0}^{rank-1} S_i G_i \it{\text{   (1)}}
$$

, when rank - dimension of matrix; S -shape, G - stride. In the last formula we use cryptic _G_ named as stride. It is array which stores size of step for each dimension which we must do when move to next column(row or N-dimension). For example, in matrix _M_ element _M\[0,0,0\]_ corresponds element _A[0]_. If I increase _i_ by one element _M\[1,0,0\]_ will correspond element _A[9]_ because step for this dimension and shape equals 9. We can compute strides for each dimension by:

$$
G_j = \prod_{i=j+1}^{rank-1} S_i \it{\text{   (2)}}
$$

If we come to an agreement that the shape and rank will not change after creation matrix we will compute the strides at moment initialization structure but we alway must calculate element of array each times then access to matrix. It's the lager shortcoming of dense matrix. So, we can write structure of dense matrix in C:

{% codeblock lang:c %}
struct dense_matrix {
  size_t rank;            /* For matrix M rank = 3 */
  size_t *shape;          /* shape = [3, 3, 3] */
  size_t *strides;        /* stride = [9, 3, 1] */
  void* elements;         /* elements = [0,1,2,3, .. 26] */
};
{% endcodeblock %}

## Algorithm

When data structures is defined we can begin to develop the algorithm. For better understanding we can imagine _N_ matrix as slice _M\[1..2, 1..2, 1..2\]_:

$$
\mathbf{M}_1  =  \begin{vmatrix}
0 & 1 & 2 \\
3 & \bf{4} & \bf{5} \\
6 & \bf{7} & \bf{8}
\end{vmatrix}
$$

$$
\mathbf{M}_2 = \begin{vmatrix}
9 & 10 & 11 \\
12 & \bf{13} & \bf{14} \\
15 & \bf{16} & \bf{17}
\end{vmatrix}
$$

$$
\mathbf{M}_3 = \begin{vmatrix}
18 & 19 & 20 \\
21 & \bf{22} & \bf{23} \\
24 & \bf{25} & \bf{26} 
\end{vmatrix}
$$

And _N_ (bold numbers) is represented by Array _B_:

$$
\mathbf{A} =  \begin{bmatrix} 
0 & 1 & 2 & 3 & \bf{4} & \bf{5} & 6 & \bf{7} & \bf{8} & 9 & 10 & 11 & 12 & \bf{13} & \bf{14} & 15 & \bf{16} & \bf{17} & 18 & 19 & 20 & 21 & \bf{22} & \bf{23} & 24 & \bf{25} & \bf{26}
\end{bmatrix}
$$


