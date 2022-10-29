# Interactive Linear Algebra
>[Source](https://textbooks.math.gatech.edu/ila/index.html)

## Chapter 1 - Systems of Linear Equations: Algebra	

Systems of linear equations can be easily represented using matrices whose elements are the coefficients of the equations.
```math
\left\{ 
\begin{align*}
x + 2y + 3z &=6 \\
2x - 3y + 2z &=14 \\
3x + y - z &= -2 
\end{align*}
\right.
\xrightarrow{\text{becomes}}
\left[
  \begin{matrix}
    1 & 2 & 3 \\
    2 & -3 & 2 \\
    3 & 1 & -1 \\
  \end{matrix}
  \left|
    \,
    \begin{matrix}
      6  \\
      14  \\
      -2  \\
    \end{matrix}
  \right.
\right]
```

This is called an **augmented matrix**

> **Remark**: Replacing one equation in the system of equations with a linear combination of the equation along with the other equations will not change the solution set
> 
> **Intuition**: The new linear surface will still pass through the points in the solution set as it is a linear combination of the other equations. If the solution set works for the components that were combined, it will work for the sum as well.

We could say that any augmented matrix modified through above mentioned meaningful row operations will be **row equivalent** in nature -- i.e., they will yield the same solution set.

### Echelon Forms

Example of matrix in row echelon form:

```math
\left(
\begin{matrix}
\star & \star & \star & \star & \star & \star \\
0 & \star & \star & \star & \star & \star  \\
0 & 0 & 0 & \star & \star & \star  \\
0 & 0 & 0 & 0 & 0 & 0  \\
\end{matrix}
\right)
```

A matrix in row echelon form is generally easily solvable through back substitution.

>**Definition:** A  **_pivot_**  is the first nonzero entry of a row of a matrix in row echelon form.

> **Definition:** A matrix is in **_reduced row echelon form_** if it is in row echelon form, and in addition:
> - Each pivot is equal to 1
> - Each pivot is the only nonzero entry in its column

Example of reduced row echelon form:

$$
\left(
\begin{matrix}
\red{1} & 0 & \star & 0 & \star \\
0 & \red{1} & \star & 0 & \star  \\
0 & 0 & 0 & \red{1} & \star  \\
0 & 0 & 0 & 0 & 0  \\
\end{matrix}
\right)
$$

An RRE matrix is in some sense at the extreme of simplified representation. If an augmented matrix is in reduced row echelon form, the corresponding linear system is viewed as _solved_.

>**Theorem:** Every matrix is row equivalent to one and only one matrix in reduced row echelon form.
>
>**Intuition**: [**Double check this**] Wouldn't the row echelon forms that are possible to generate from a matrix be the same except for the fact that the rows may be scaled differently (In the end they will definitely produce same solution set!). So if you are normalising the pivots to 1, this will make sure that the RRE matrix for a set of equations will be unique.

The RRE matrix for an inconsistent system (no solutions) will have it's last column as a pivot column. 

$$
\left[
  \begin{matrix}
    \red{1} & 0 & \star \\
    0 & \red{1} & \star \\
    0 &0 & 0 \\
  \end{matrix}
  \left|
    \,
    \begin{matrix}
      0  \\
      0  \\
      \red{1}  \\
    \end{matrix}
  \right.
\right]
$$

If you think about it, it's the only way to get nonsense out of an RRE matrix. Anything else would point to some sort of solution set.

### Parametric Form

Let's try solving the system of equations given by

$$
\left\{ 
\begin{align*}
2x + y + 12z &=1 \\
x + 2y + 9z &=-1 
\end{align*}
\right.
$$

The RRE matrix will come out to be

$$
\left[
  \begin{matrix}
    1 & 0 & \red{5} \\
    0 & 1 & \red{2} \\
  \end{matrix}
  \left|
    \,
    \begin{matrix}
      1 \\
      -1 \\
    \end{matrix}
  \right.
\right]
$$

Notice how the column corresponding to $z$ is not a pivot column. It means there is no way to determine $z$ in reference to anything. $z$ is essentially free to assume whatever value. The other variables are bound by some rules but $z$ if a ***free variable***.

The solved system will look like this

$$
\left\{ 
\begin{align*}
x &=1-5z \\
y &=-1-2z  \\
z &= z
\end{align*}
\right.
$$

or more succinctly expressed as $(1-5z, -1-2z, z)$

You could take a look at the parametric representation of the solution set to get an idea about the solution set. For the above example there is a single free variable which implies the solution set will be a line. If there were two free variables it would've been a plane and no free variables would lead to a single point.

#### Number of Solutions

There are three possibilities for the RRE matrix from which we can conclude the nature of the solution set.

1. *The last column is a pivot column.* In this case the RRE matrix is nonsensical and leads to no solutions.
2. *Every column except the last column is a pivot column.* In this case the system has a unique solution.
3. *The last column is not a pivot column, and some other column is not a pivot column either.* In this case the system has infinitely many solutions owing to the free variables that are present.

## Chapter 2 - Systems of Linear Equations: Geometry

$$
\left\{ 
\begin{align*}
x + 2y + 3z &=6 \\
2x - 3y + 2z &=14 \\
3x + y - z &= -2 
\end{align*}
\right.
\xrightarrow{\text{becomes}}
x\left(
\begin{matrix}
1 \\
2 \\
3
\end{matrix}
\right)
+y\left(
\begin{matrix}
2 \\
-3 \\
1
\end{matrix}
\right)
+z\left(
\begin{matrix}
3 \\
2 \\
-1
\end{matrix}
\right)
= \left(
\begin{matrix}
6 \\
14 \\
-2
\end{matrix}
\right)
$$

We just changed the notation a bit. As long as the symbols point to the same action it wouldn't matter how we represent something. But, sometimes writing down an idea into a much more fundamental form makes it easier to see the patterns that were always there from the beginning.
Here when we rewrite the system of linear equations into a vector equation we are removing redundancies in the representation. Thus the question of finding the solution set of a system of linear equations reveals itself to be the same as finding what linear combination of given vectors form another vector.

> **Remark:** Asking whether or not a vector equation has a solution is the same as asking if a given vector is a linear combination of some other given vectors.

#### Three Characterizations of Consistency  
We now have three distinct (yet equivalent!) ways of representing a linear system.
1. As a system of equations:

$$
\left\{
\begin{align*}
2x_1+3x_2-2x_3&=7 \\
x_1-x_2-3x_3&=5
\end{align*}
\right.
$$ 

2. As an augmented matrix:

$$
\left[
  \begin{matrix}
    2 & 3 & -2 \\
    1 & -1 & -3 \\
  \end{matrix}
  \left|
    \,
    \begin{matrix}
      7 \\
      5 \\
    \end{matrix}
  \right.
\right]
$$

3. As a vector equation

$$
x_1\begin{pmatrix}2\\1\end{pmatrix} +
x_2\begin{pmatrix}3\\-1\end{pmatrix} +
x_3\begin{pmatrix}-2\\-3\end{pmatrix} =
\begin{pmatrix}7\\5\end{pmatrix}
$$

The different representations lends to the mathematician their own unique perspectives on analysing the system. Some ideas will be easier to reason out in the geometric representation than the algebraic or vice versa. 

> **Definition**: Let $v_1,v_2,\dots,v_k$ be vectors in $\mathbb{R}^{n}$. The ***span*** of $v_1,v_2,\dots,v_k$ is the set of all points attainable using these vectors as basis vectors.

Now we have three ways of stating that a linear system is consistent.
1. The vector $b$ is in the span of $v_1,v_2,\dots,v_k$.
2. The vector equation $$x_1v_1+x_2v_2+\dots+x_kv_k=b$$ has a solution
3. The augmented matrix is consistent.

$$
\left(
  \begin{matrix}
    | & | & & | \\
    v_1 & v_2 & \dots & v_k \\
    | & | & & | 
  \end{matrix}
  \left|
    \,
    \begin{matrix}
      | \\
      b \\
      |
    \end{matrix}
  \right.
\right)
$$

### 2.3 Matrix Equations

The product of an $m \times n$ matrix $A$ with a vector $x$ in $\mathbb{R}^{n}$ is defined as

$$
Ax =
\left(
  \begin{matrix}
    | & | & & | \\
    v_1 & v_2 & \dots & v_n \\
    | & | & & | 
  \end{matrix}
\right)
\left(
\begin{matrix}
x_1 \\ x_2 \\ \vdots \\ x_n
\end{matrix}
\right)
=x_1v_1 + x_2v_2 + \dots+x_nv_n.
$$

The product is only meaningful when  $A$ is an $m \times n$ matrix ($n$ vectors, each $m$-dimensional) and $x$ has $n$ entries. The product will be an $m$ dimensional vector.

Two straightforward properties can be stated at this moment. Assume $A$ is a matrix, $u$, $v$ are vectors, $c$ is a scalar and the multiplications make sense.
- $A(u+v) = Au+Av$
- $A(cu) = cAu$

> Definition: A ***matrix equation*** is an equation of the form $Ax=b$ where $A$ is an $m\times n$ matrix, $b$ is a vector in $\mathbb{R}^m$, and $x$ is a vector whose coefficients $x_1, x_2,\dots,x_n$ are unknown.

