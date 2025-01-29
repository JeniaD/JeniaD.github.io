---
title: Linear algebra
date: 2025-01-29 22:09:00 +0200
categories: [University, math]
tags: [math]
math: true
---

### Systems of Linear Equations  

A system of equations can have:  
- **No solution** (inconsistent)  
- **One solution** (consistent)  
- **Infinitely many solutions** (consistent)  

A system is called **consistent** if it has at least one solution and **inconsistent** if it has none.  

#### Augmented Matrix Representation  
The system:  
$$
\begin{aligned}
a_{11}x_{1} + a_{12}x_{2} + \dots + a_{1n}x_{n} &= b_{1} \\
a_{21}x_{1} + a_{22}x_{2} + \dots + a_{2n}x_{n} &= b_{2} \\
\vdots \quad &\vdots \\
a_{m1}x_{1} + a_{m2}x_{2} + \dots + a_{mn}x_{n} &= b_{m}
\end{aligned}
$$

can be represented as:
$$
\begin{bmatrix}
a_{11} & a_{12} & \dots & a_{1n} \\
a_{21} & a_{22} & \dots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \dots & a_{mn}
\end{bmatrix}
\begin{bmatrix}
x_{1} \\
x_{2} \\
\vdots \\
x_{n}
\end{bmatrix}
=
\begin{bmatrix}
b_{1} \\
b_{2} \\
\vdots \\
b_{m}
\end{bmatrix}
$$  

### Elementary Row Operations  
1. Multiply a row by a non-zero constant.  
2. Swap two rows.  
3. Add or subtract a multiple of one row to another row.  

---

### Echelon and Reduced Row Echelon Forms  

1. **Echelon Form:**  
$$
\begin{bmatrix}
p_{11} & p_{12} & \dots & p_{1n} \\
0 & p_{22} & \dots & p_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \dots & p_{mn}
\end{bmatrix}
$$  

2. **Reduced Row Echelon Form (RREF):**  
$$
\begin{bmatrix}
1 & 0 & \dots & 0 & r_{1} \\
0 & 1 & \dots & 0 & r_{2} \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & \dots & 1 & r_{m}
\end{bmatrix}
$$  

---

### Homogeneous Linear Systems  

The system:
$$
\begin{aligned}
a_{11}x_{1} + a_{12}x_{2} + \dots + a_{1n}x_{n} &= 0 \\
a_{21}x_{1} + a_{22}x_{2} + \dots + a_{2n}x_{n} &= 0 \\
\vdots \quad &\vdots \\
a_{m1}x_{1} + a_{m2}x_{2} + \dots &+ a_{mn}x_{n} = 0
\end{aligned}
$$  

Homogeneous systems always have at least the trivial solution:  
$$[0, 0, \dots, 0]$$  

---

### Properties of Matrices  

1. \( A + B = B + A \)  
2. \( A + (B + C) = (A + B) + C \)  
3. \( A(BC) = (AB)C \) *(order matters!)*  
4. \( A(B + C) = AB + AC \)  
5. \( (A + B)C = AC + BC \)  
6. \( a(B + C) = aB + aC \)  
7. \( (a + b)A = aA + bA \)  
8. \( a(bC) = (ab)C \)  
9. \( a(BC) = (aB)C \)  

---

### Invertibility of a Matrix  

For a \( 2 \times 2 \) matrix:  
$$
A = 
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$  

The inverse exists if \(\det(A) \neq 0\):  
$$
A^{-1} = \frac{1}{\det(A)} 
\begin{bmatrix}
d & -b \\
-c & a
\end{bmatrix}
$$  

---

### Determinants  

- **Minor**: A square matrix obtained by deleting one row and one column from the original matrix.  
- **Cofactor**:  
$$C_{ij} = (-1)^{i+j} M_{ij}$$  
where \( M_{ij} \) is the minor corresponding to element \( a_{ij} \).  

---

### LU Decomposition  

A matrix \( A \) can be decomposed as:  
$$ A = LU $$  

where \( L \) is a lower triangular matrix and \( U \) is an upper triangular matrix:  

$$
L =
\begin{bmatrix}
l_{11} & 0 & 0 \\
l_{21} & l_{22} & 0 \\
l_{31} & l_{32} & l_{33}
\end{bmatrix},
\quad
U =
\begin{bmatrix}
u_{11} & u_{12} & u_{13} \\
0 & u_{22} & u_{23} \\
0 & 0 & u_{33}
\end{bmatrix}
$$  

Steps:  
1. Solve \( LY = B \).  
2. Solve \( UX = Y \).
