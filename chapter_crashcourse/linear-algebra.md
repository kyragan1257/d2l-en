
# Linear algebra

Now that you can store and manipulate data, 
let's briefly review the subset of basic linear algebra 
that you'll need to understand most of the models. 
We'll introduce all the basic concepts, 
the corresponding mathematical notation, 
and their realization in code all in one place. 
If you're already confident in your basic linear algebra, 
feel free to skim or skip this chapter. 


```python
from mxnet import nd
```

## Scalars

If you never studied linear algebra or machine learning, 
you're probably used to working with one number at a time.
And know how to do basic things like add them together or multiply them.
For example, in Palo Alto, the temperature is $52$ degrees Fahrenheit. 
Formally, we call these values $scalars$.
If you wanted to convert this value to Celsius (using metric system's more sensible unit of temperature measurement),
you'd evaluate the expression $c = (f - 32) * 5/9$ setting $f$ to $52$.
In this equation, each of the terms $32$, $5$, and $9$ is a scalar value.
The placeholders $c$ and $f$ that we use are called variables
and they stand in for unknown scalar values.

In mathematical notation, we represent scalars with ordinary lower cased letters ($x$, $y$, $z$).
We also denote the space of all scalars as $\mathcal{R}$.
For expedience, we're going to punt a bit on what precisely a space is,
but for now, remember that if you want to say that $x$ is a scalar, 
you can simply say $x \in \mathcal{R}$.
The symbol $\in$ can be pronounced "in" and just denotes membership in a set.

In MXNet, we work with scalars by creating NDArrays with just one element. 
In this snippet, we instantiate two scalars and perform some familiar arithmetic operations with them.


```python
##########################
# Instantiate two scalars
##########################
x = nd.array([3.0]) 
y = nd.array([2.0])

##########################
# Add them
##########################
print('x + y = ', x + y)

##########################
# Multiply them
##########################
print('x * y = ', x * y)

##########################
# Divide x by y
##########################
print('x / y = ', x / y)

##########################
# Raise x to the power y. 
##########################
print('x ** y = ', nd.power(x,y))
```

    x + y =  
    [ 5.]
    <NDArray 1 @cpu(0)>
    x * y =  
    [ 6.]
    <NDArray 1 @cpu(0)>
    x / y =  
    [ 1.5]
    <NDArray 1 @cpu(0)>
    x ** y =  
    [ 9.]
    <NDArray 1 @cpu(0)>


We can convert any NDArray to a Python float by calling its `asscalar` method


```python
x.asscalar()
```




    3.0



## Vectors 
You can think of a vector as simply a list of numbers, for example ``[1.0,3.0,4.0,2.0]``. 
Each of the numbers in the vector consists of a single scalar value.
We call these values the *entries* or *components* of the vector.
Often, we're interested in vectors whose values hold some real-world significance.
For example, if we're studying the risk that loans default,
we might associate each applicant with a vector 
whose components correspond to their income, 
length of employment, number of previous defaults, etc. 
If we were studying the risk of heart attack in hospital patients, 
we might represent each patient with a vector
whose components capture their most recent vital signs,
cholesterol levels, minutes of exercise per day, etc. 
In math notation, we'll usually denote vectors as bold-faced, 
lower-cased letters ($\mathbf{u}$, $\mathbf{v}$, $\mathbf{w})$. 
In MXNet, we work with vectors via 1D NDArrays with an arbitrary number of components.


```python
u = nd.arange(4)
print('u = ', u)
```

    u =  
    [ 0.  1.  2.  3.]
    <NDArray 4 @cpu(0)>


We can refer to any element of a vector by using a subscript. 
For example, we can refer to the $4$th element of $\mathbf{u}$ by $u_4$. 
Note that the element $u_4$ is a scalar, 
so we don't bold-face the font when referring to it.
In code, we access any element $i$ by indexing into the ``NDArray``.


```python
u[3]
```




    
    [ 3.]
    <NDArray 1 @cpu(0)>



## Length, dimensionality, and, shape

A vector is just an array of numbers. And just as every array has a length, so does every vector. 
In math notation, if we want to say that a vector $x$ consists of $n$ real-valued scalars,
we can express this as $\mathbf{x} \in \mathcal{R}^n$.
The length of a vector is commonly called its $dimension$.
As with an ordinary Python array, we can access the length of an NDArray 
by calling Python's in-built ``len()`` function.


```python
len(u)
```




    4



We can also access a vector's length via its `.shape` attribute. 
The shape is a tuple that lists the dimensionality of the NDArray along each of its axes. 
Because a vector can only be indexed along one axis, its shape has just one element.


```python
u.shape
```




    (4,)



Note that the word dimension is overloaded and this tends to confuse people.
Some use the *dimensionality* of a vector to refer to its length (the number of components). 
However some use the word *dimensionality* to refer to the number of axes that an array has.
In this sense, a scalar *would have* $0$ dimensions and a vector *would have* $1$ dimension.
**To avoid confusion, when we say *2D* array or *3D* array, we mean an array with 2 or 3 axes repespectively. But if we say *$n$-dimensional* vector, we mean a vector of length $n$.**


```python
a = 2
x = nd.array([1,2,3])
y = nd.array([10,20,30])
print(a * x)
print(a * x + y)
```

## Matrices

Just as vectors generalize scalars from order $0$ to order $1$, 
matrices generalize vectors from $1D$ to $2D$. 
Matrices, which we'll denote with capital letters ($A$, $B$, $C$), 
are represented in code as arrays with 2 axes.  
Visually, we can draw a matrix as a table,
where each entry $a_{ij}$ belongs to the $i$-th row and $j$-th column. 


$$A=\begin{pmatrix}
 a_{11} & a_{12} & \cdots & a_{1m} \\
 a_{21} & a_{22} & \cdots & a_{2m} \\
\vdots & \vdots & \ddots & \vdots \\
 a_{n1} & a_{n2} & \cdots & a_{nm} \\
\end{pmatrix}$$

We can create a matrix with $n$ rows and $m$ columns in MXNet
by specifying a shape with two components `(n,m)`
when calling any of our favorite functions for instantiating an `ndarray`
such as `ones`, or `zeros`.


```python
A = nd.zeros((5,4))
A
```




    
    [[ 0.  0.  0.  0.]
     [ 0.  0.  0.  0.]
     [ 0.  0.  0.  0.]
     [ 0.  0.  0.  0.]
     [ 0.  0.  0.  0.]]
    <NDArray 5x4 @cpu(0)>



We can also reshape any 1D array into a 2D ndarray by calling `ndarray`'s reshape method and passing in the desired shape. Note that the product of shape components `n * m` must be equal to the length of the original vector. 


```python
x = nd.arange(20)
A = x.reshape((5, 4))
A
```




    
    [[  0.   1.   2.   3.]
     [  4.   5.   6.   7.]
     [  8.   9.  10.  11.]
     [ 12.  13.  14.  15.]
     [ 16.  17.  18.  19.]]
    <NDArray 5x4 @cpu(0)>



Matrices are useful data structures: they allow us to organize data that has different modalities of variation. For example, returning to the example of medical data, rows in our matrix might correspond to different patients, while columns might correspond to different attributes.

We can access the scalar elements $a_{ij}$ of a matrix $A$ by specifying the indices for the row ($i$) and column ($j$) respectively. Let's grab the element $a_{2,3}$ from the random matrix we initialized above.


```python
print('A[2, 3] = ', A[2, 3])
```

    A[2, 3] =  
    [ 11.]
    <NDArray 1 @cpu(0)>


We can also grab the vectors corresponding to an entire row $\mathbf{a}_{i,:}$ or a column $\mathbf{a}_{:,j}$.


```python
print('row 2', A[2, :])
print('column 3', A[:, 3])
```

    row 2 
    [  8.   9.  10.  11.]
    <NDArray 4 @cpu(0)>
    column 3 
    [  3.   7.  11.  15.  19.]
    <NDArray 5 @cpu(0)>


We can transpose the matrix through `T`. That is, if $B = A^T$, then $b_{ij} = a_{ji}$ for any $i$ and $j$.


```python
A.T
```




    
    [[  0.   4.   8.  12.  16.]
     [  1.   5.   9.  13.  17.]
     [  2.   6.  10.  14.  18.]
     [  3.   7.  11.  15.  19.]]
    <NDArray 4x5 @cpu(0)>



## Tensors 

Just as vectors generalize scalars, and matrices generalize vectors, we can actually build data structures with even more axes. Tensors give us a generic way of discussing arrays with an arbitrary number of axes. Vectors, for example, are first-order tensors, and matrices are second-order tensors.

Using tensors will become more important when we start working with images, which arrive as 3D data structures, with axes corresponding to the height, width, and the three (RGB) color channels. But in this chapter, we're going to skip past and make sure you know the basics.


```python
X = nd.arange(24).reshape((2, 3, 4))
print('X.shape =', X.shape)
print('X =', X)
```

    X.shape = (2, 3, 4)
    X = 
    [[[  0.   1.   2.   3.]
      [  4.   5.   6.   7.]
      [  8.   9.  10.  11.]]
    
     [[ 12.  13.  14.  15.]
      [ 16.  17.  18.  19.]
      [ 20.  21.  22.  23.]]]
    <NDArray 2x3x4 @cpu(0)>


## Element-wise operations

Oftentimes, we want to apply functions to arrays. 
Some of the simplest and most useful functions are the element-wise functions. 
These operate by performing a single scalar operation on the corresponding elements of two arrays.
We can create an element-wise function from any function that maps from the scalars to the scalars.
In math notations we would denote such a function as $f: \mathcal{R} \rightarrow \mathcal{R}$.
Given any two vectors $\mathbf{u}$ and $\mathbf{v}$ *of the same shape*, and the function f,
we can produce a vector $\mathbf{c} = F(\mathbf{u},\mathbf{v})$ 
by setting $c_i \gets f(u_i, v_i)$ for all $i$.
Here, we produced the vector-valued $F: \mathcal{R}^d \rightarrow \mathcal{R}^d$
by *lifting* the scalar function to an element-wise vector operation.
In MXNet, the common standard arithmetic operators (+,-,/,\*,\*\*)
have all been *lifted* to element-wise operations for identically-shaped tensors of arbitrary shape.


```python
u = nd.array([1, 2, 4, 8])
v = nd.ones_like(u) * 2
print('v =', v)
print('u + v', u + v)
print('u - v', u - v)
print('u * v', u * v)
print('u / v', u / v)
```

    v = 
    [ 2.  2.  2.  2.]
    <NDArray 4 @cpu(0)>
    u + v 
    [  3.   4.   6.  10.]
    <NDArray 4 @cpu(0)>
    u - v 
    [-1.  0.  2.  6.]
    <NDArray 4 @cpu(0)>
    u * v 
    [  2.   4.   8.  16.]
    <NDArray 4 @cpu(0)>
    u / v 
    [ 0.5  1.   2.   4. ]
    <NDArray 4 @cpu(0)>


We can call element-wise operations on any two tensors of the same shape, including matrices.


```python
B = nd.ones_like(A) * 3
print('B =', B)
print('A + B =', A + B)
print('A * B =', A * B)
```

    B = 
    [[ 3.  3.  3.  3.]
     [ 3.  3.  3.  3.]
     [ 3.  3.  3.  3.]
     [ 3.  3.  3.  3.]
     [ 3.  3.  3.  3.]]
    <NDArray 5x4 @cpu(0)>
    A + B = 
    [[  3.   4.   5.   6.]
     [  7.   8.   9.  10.]
     [ 11.  12.  13.  14.]
     [ 15.  16.  17.  18.]
     [ 19.  20.  21.  22.]]
    <NDArray 5x4 @cpu(0)>
    A * B = 
    [[  0.   3.   6.   9.]
     [ 12.  15.  18.  21.]
     [ 24.  27.  30.  33.]
     [ 36.  39.  42.  45.]
     [ 48.  51.  54.  57.]]
    <NDArray 5x4 @cpu(0)>


## Basic properties of tensor arithmetic

Scalars, vectors, matrices, and tensors of any order have some nice properties that we'll often rely on.
For example, as you might have noticed from the definition of an element-wise operation, 
given operands with the same shape, 
the result of any element-wise operation is a tensor of that same shape. 
Another convenient property is that for all tensors, multiplication by a scalar 
produces a tensor of the same shape. 
In math, given two tensors $X$ and $Y$ with the same shape,
$\alpha X + Y$ has the same shape. 
(numerical mathematicians call this the AXPY operation). 


```python
a = 2
x = nd.ones(3)
y = nd.zeros(3)
print(x.shape)
print(y.shape)
print((a * x).shape)
print((a * x + y).shape)
```

    (3,)
    (3,)
    (3,)
    (3,)


Shape is not the the only property preserved under addition and multiplication by a scalar. These operations also preserve membership in a vector space. But we'll postpone this discussion for the second half of this chapter because it's not critical to getting your first models up and running. 

## Sums and means 

The next more sophisticated thing we can do with arbitrary tensors 
is to calculate the sum of their elements. 
In mathematical notation, we express sums using the $\sum$ symbol. 
To express the sum of the elements in a vector $\mathbf{u}$ of length $d$, 
we can write $\sum_{i=1}^d u_i$. In code, we can just call ``nd.sum()``.


```python
nd.sum(u)
```

We can similarly express sums over the elements of tensors of arbitrary shape. For example, the sum of the elements of an $m \times n$ matrix $A$ could be written $\sum_{i=1}^{m} \sum_{j=1}^{n} a_{ij}$. 


```python
nd.sum(A)
```

A related quantity is the *mean*, which is also called the *average*. 
We calculate the mean by dividing the sum by the total number of elements. 
With mathematical notation, we could write the average 
over a vector $\mathbf{u}$ as $\frac{1}{d} \sum_{i=1}^{d} u_i$ 
and the average over a matrix $A$ as  $\frac{1}{n \cdot m} \sum_{i=1}^{m} \sum_{j=1}^{n} a_{ij}$. 
In code, we could just call ``nd.mean()`` on tensors of arbitrary shape:


```python
print(nd.mean(A))
print(nd.sum(A) / A.size)
```

## Dot products

<!-- So far, we've only performed element-wise operations, sums and averages. And if this was we could do, linear algebra probably wouldn't deserve it's own chapter. However, -->

One of the most fundamental operations is the dot product. Given two vectors $\mathbf{u}$ and $\mathbf{v}$, the dot product $\mathbf{u}^T \mathbf{v}$ is a sum over the products of the corresponding elements: $\mathbf{u}^T \mathbf{v} = \sum_{i=1}^{d} u_i \cdot v_i$.


```python
nd.dot(u, v)
```

Note that we can express the dot product of two vectors ``nd.dot(u, v)`` equivalently by performing an element-wise multiplication and then a sum:


```python
nd.sum(u * v)
```

Dot products are useful in a wide range of contexts. For example, given a set of weights $\mathbf{w}$, the weighted sum of some values ${u}$ could be expressed as the dot product $\mathbf{u}^T \mathbf{w}$. When the weights are non-negative and sum to one ($\sum_{i=1}^{d} {w_i} = 1$), the dot product expresses a *weighted average*. When two vectors each have length one (we'll discuss what *length* means below in the section on norms), dot products can also capture the cosine of the angle between them.

## Matrix-vector products

Now that we know how to calculate dot products we can begin to understand matrix-vector products. Let's start off by visualizing a matrix $A$ and a column vector $\mathbf{x}$.

$$A=\begin{pmatrix}
 a_{11} & a_{12} & \cdots & a_{1m} \\
 a_{21} & a_{22} & \cdots & a_{2m} \\
\vdots & \vdots & \ddots & \vdots \\
 a_{n1} & a_{n2} & \cdots & a_{nm} \\
\end{pmatrix},\quad\mathbf{x}=\begin{pmatrix}
 x_{1}  \\
 x_{2} \\
\vdots\\
 x_{m}\\
\end{pmatrix} $$

We can visualize the matrix in terms of its row vectors

$$A=
\begin{pmatrix}
\cdots & \mathbf{a}^T_{1} &...  \\
\cdots & \mathbf{a}^T_{2} & \cdots \\
 & \vdots &  \\
 \cdots &\mathbf{a}^T_n & \cdots \\
\end{pmatrix},$$

where each $\mathbf{a}^T_{i} \in \mathbb{R}^{m}$
is a row vector representing the $i$-th row of the matrix $A$.

Then the matrix vector product $\mathbf{y} = A\mathbf{x}$ is simply a column vector $\mathbf{y} \in \mathbb{R}^n$ where each entry $y_i$ is the dot product $\mathbf{a}^T_i \mathbf{x}$.

$$A\mathbf{x}=
\begin{pmatrix}
\cdots & \mathbf{a}^T_{1} &...  \\
\cdots & \mathbf{a}^T_{2} & \cdots \\
 & \vdots &  \\
 \cdots &\mathbf{a}^T_n & \cdots \\
\end{pmatrix}
\begin{pmatrix}
 x_{1}  \\
 x_{2} \\
\vdots\\
 x_{m}\\
\end{pmatrix}
= \begin{pmatrix}
 \mathbf{a}^T_{1} \mathbf{x}  \\
 \mathbf{a}^T_{2} \mathbf{x} \\
\vdots\\
 \mathbf{a}^T_{n} \mathbf{x}\\
\end{pmatrix}
$$

So you can think of multiplication by a matrix $A\in \mathbb{R}^{m \times n}$ as a transformation that projects vectors from $\mathbb{R}^{m}$ to $\mathbb{R}^{n}$.

These transformations turn out to be quite useful. For example, we can represent rotations as multiplications by a square matrix. As we'll see in subsequent chapters, we can also use matrix-vector products to describe the calculations of each layer in a neural network. 

Expressing matrix-vector products in code with ``ndarray``, we use the same ``nd.dot()`` function as for dot products. When we call ``nd.dot(A, x)`` with a matrix ``A`` and a vector ``x``, ``MXNet`` knows to perform a matrix-vector product. Note that the column dimension of ``A`` must be the same as the dimension of ``x``.


```python
nd.dot(A, u)
```

## Matrix-matrix multiplication

If you've gotten the hang of dot products and matrix-vector multiplication, then matrix-matrix multiplications should be pretty straightforward.

Say we have two matrices, $A \in \mathbb{R}^{n \times k}$ and $B \in \mathbb{R}^{k \times m}$:

$$A=\begin{pmatrix}
 a_{11} & a_{12} & \cdots & a_{1k} \\
 a_{21} & a_{22} & \cdots & a_{2k} \\
\vdots & \vdots & \ddots & \vdots \\
 a_{n1} & a_{n2} & \cdots & a_{nk} \\
\end{pmatrix},\quad
B=\begin{pmatrix}
 b_{11} & b_{12} & \cdots & b_{1m} \\
 b_{21} & b_{22} & \cdots & b_{2m} \\
\vdots & \vdots & \ddots & \vdots \\
 b_{k1} & b_{k2} & \cdots & b_{km} \\
\end{pmatrix}$$

To produce the matrix product $C = AB$, it's easiest to think of $A$ in terms of its row vectors and $B$ in terms of its column vectors:

$$A=
\begin{pmatrix}
\cdots & \mathbf{a}^T_{1} &...  \\
\cdots & \mathbf{a}^T_{2} & \cdots \\
 & \vdots &  \\
 \cdots &\mathbf{a}^T_n & \cdots \\
\end{pmatrix},
\quad B=\begin{pmatrix}
\vdots & \vdots &  & \vdots \\
 \mathbf{b}_{1} & \mathbf{b}_{2} & \cdots & \mathbf{b}_{m} \\
 \vdots & \vdots &  &\vdots\\
\end{pmatrix}.
$$

Note here that each row vector $\mathbf{a}^T_{i}$ lies in $\mathbb{R}^k$ and that each column vector $\mathbf{b}_j$ also lies in $\mathbb{R}^k$.

Then to produce the matrix product $C \in \mathbb{R}^{n \times m}$ we simply compute each entry $c_{ij}$ as the dot product $\mathbf{a}^T_i \mathbf{b}_j$.

$$C = AB = \begin{pmatrix}
\cdots & \mathbf{a}^T_{1} &...  \\
\cdots & \mathbf{a}^T_{2} & \cdots \\
 & \vdots &  \\
 \cdots &\mathbf{a}^T_n & \cdots \\
\end{pmatrix}
\begin{pmatrix}
\vdots & \vdots &  & \vdots \\
 \mathbf{b}_{1} & \mathbf{b}_{2} & \cdots & \mathbf{b}_{m} \\
 \vdots & \vdots &  &\vdots\\
\end{pmatrix}
= \begin{pmatrix}
\mathbf{a}^T_{1} \mathbf{b}_1 & \mathbf{a}^T_{1}\mathbf{b}_2& \cdots & \mathbf{a}^T_{1} \mathbf{b}_m \\
 \mathbf{a}^T_{2}\mathbf{b}_1 & \mathbf{a}^T_{2} \mathbf{b}_2 & \cdots & \mathbf{a}^T_{2} \mathbf{b}_m \\
 \vdots & \vdots & \ddots &\vdots\\
\mathbf{a}^T_{n} \mathbf{b}_1 & \mathbf{a}^T_{n}\mathbf{b}_2& \cdots& \mathbf{a}^T_{n} \mathbf{b}_m 
\end{pmatrix}
$$

You can think of the matrix-matrix multiplication $AB$ as simply performing $m$ matrix-vector products and stitching the results together to form an $n \times m$ matrix. Just as with ordinary dot products and matrix-vector products, we can compute matrix-matrix products in ``MXNet`` by using ``nd.dot()``.


```python
A = nd.ones(shape=(3, 4))
B = nd.ones(shape=(4, 5))
nd.dot(A, B)
```




    
    [[ 4.  4.  4.  4.  4.]
     [ 4.  4.  4.  4.  4.]
     [ 4.  4.  4.  4.  4.]]
    <NDArray 3x5 @cpu(0)>



## Norms

Before we can start implementing models, 
there's one last concept we're going to introduce. 
Some of the most useful operators in linear algebra are norms.
Informally, they tell us how big a vector or matrix is. 
We represent norms with the notation $\|\cdot\|$. 
The $\cdot$ in this expression is just a placeholder. 
For example, we would represent the norm of a vector $\mathbf{x}$ 
or matrix $A$ as $\|\mathbf{x}\|$ or $\|A\|$, respectively. 

All norms must satisfy a handful of properties:
1. $\|\alpha A\| = |\alpha| \|A\|$
2. $\|A + B\| \leq \|A\| + \|B\|$
3. $\|A\| \geq 0$
4. If $\forall {i,j}, a_{ij} = 0$, then $\|A\|=0$

To put it in words, the first rule says 
that if we scale all the components of a matrix or vector 
by a constant factor $\alpha$, 
its norm also scales by the *absolute value* 
of the same constant factor. 
The second rule is the familiar triangle inequality.
The third rule simple says that the norm must be non-negative. 
That makes sense, in most contexts the smallest *size* for anything is 0.
The final rule basically says that the smallest norm is achieved by a matrix or vector consisting of all zeros.
It's possible to define a norm that gives zero norm to nonzero matrices,
but you can't give nonzero norm to zero matrices. 
That's a mouthful, but if you digest it then you probably have grepped the important concepts here.

If you remember Euclidean distances (think Pythagoras' theorem) from grade school, 
then non-negativity and the triangle inequality might ring a bell.
You might notice that norms sound a lot like measures of distance.

In fact, the Euclidean distance $\sqrt{x_1^2 + \cdots + x_n^2}$ is a norm. 
Specifically it's the $\ell_2$-norm. 
An analogous computation, 
performed over the entries of a matrix, e.g. $\sqrt{\sum_{i,j} a_{ij}^2}$, 
is called the Frobenius norm. 
More often, in machine learning we work with the squared $\ell_2$ norm (notated $\ell_2^2$).
We also commonly work with the $\ell_1$ norm.
The $\ell_1$ norm is simply the sum of the absolute values. 
It has the convenient property of placing less emphasis on outliers.

To calculate the $\ell_2$ norm, we can just call ``nd.norm()``.  


```python
nd.norm(u)
```

To calculate the L1-norm we can simply perform the absolute value and then sum over the elements.


```python
nd.sum(nd.abs(u))
```

## Norms and objectives

While we don't want to get too far ahead of ourselves, we do want you to anticipate why these concepts are useful.
In machine learning we're often trying to solve optimization problems: *Maximize* the probability assigned to observed data. *Minimize* the distance between predictions and the ground-truth observations. Assign vector representations to items (like words, products, or news articles) such that the distance between similar items is minimized, and the distance between dissimilar items is maximized. Oftentimes, these objectives, perhaps the most important component of a machine learning algorithm (besides the data itself), are expressed as norms.


# Intermediate linear algebra

If you've made it this far, and understand everything that we've covered,
then honestly, you *are* ready to begin modeling. 
If you're feeling antsy, this is a perfectly reasonable place to move on.
You already know nearly all of the linear algebra required 
to implement a number of many practically useful models
and you can always circle back when you want to learn more.

But there's a lot more to linear algebra, even as concerns machine learning. 
At some point, if you plan to make a career of machine learning,
you'll need to know more than we've covered so far. 
In the rest of this chapter, we introduce some useful, more advanced concepts.



## Basic vector properties

Vectors are useful beyond being data structures to carry numbers.
In addition to reading and writing values to the components of a vector,
and performing some useful mathematical operations,
we can analyze vectors in some interesting ways.

One important concept is the notion of a vector space.
Here are the conditions that make a vector space:

* **Additive axioms** (we assume that x,y,z are all vectors): 
  $x+y = y+x$ and $(x+y)+z = x+(y+z)$ and $0+x = x+0 = x$ and $(-x) + x = x + (-x) = 0$.
* **Multiplicative axioms** (we assume that x is a vector and a, b are scalars):
  $0 \cdot x = 0$ and $1 \cdot x = x$ and $(a b) x = a (b x)$.
* **Distributive axioms** (we assume that x and y are vectors and a, b are scalars):
  $a(x+y) = ax + ay$ and $(a+b)x = ax +bx$.

## Special matrices 

There are a number of special matrices that we will use throughout this tutorial. Let's look at them in a bit of detail:

* **Symmetric Matrix** These are matrices where the entries below and above the diagonal are the same. In other words, we have that $M^\top = M$. An example of such matrices are those that describe pairwise distances, i.e. $M_{ij} = \|x_i - x_j\|$. Likewise, the Facebook friendship graph can be written as a symmetric matrix where $M_{ij} = 1$ if $i$ and $j$ are friends and $M_{ij} = 0$ if they are not. Note that the *Twitter* graph is asymmetric - $M_{ij} = 1$, i.e. $i$ following $j$ does not imply that $M_{ji} = 1$, i.e. $j$ following $i$.
* **Antisymmetric Matrix** These matrices satisfy $M^\top = -M$. Note that any arbitrary matrix can always be decomposed into a symmetric and into an antisymmetric matrix by using $M = \frac{1}{2}(M + M^\top) + \frac{1}{2}(M - M^\top)$. 
* **Diagonally Dominant Matrix** These are matrices where the off-diagonal elements are small relative to the main diagonal elements. In particular we have that $M_{ii} \geq \sum_{j \neq i} M_{ij}$ and $M_{ii} \geq \sum_{j \neq i} M_{ji}$. If a matrix has this property, we can often approximate $M$ by its diagonal. This is often expressed as $\mathrm{diag}(M)$. 
* **Positive Definite Matrix** These are matrices that have the nice property where $x^\top M x > 0$ whenever $x \neq 0$. Intuitively, they are a generalization of the squared norm of a vector $\|x\|^2 = x^\top x$. It is easy to check that whenever $M = A^\top A$, this holds since there $x^\top M x = x^\top A^\top A x = \|A x\|^2$. There is a somewhat more profound theorem which states that all positive definite matrices can be written in this form. 

## Conclusions

In just a few pages (or one Jupyter notebook) we've taught you all the linear algebra you'll need to understand a good chunk of neural networks. Of course there's a *lot* more to linear algebra. And a lot of that math *is* useful for machine learning. For example, matrices can be decomposed into factors, and these decompositions can reveal low-dimensional structure in real-world datasets. There are entire subfields of machine learning that focus on using matrix decompositions and their generalizations to high-order tensors to discover structure in datasets and solve prediction problems. But this book focuses on deep learning. And we believe you'll be much more inclined to learn more mathematics once you've gotten your hands dirty deploying useful machine learning models on real datasets. So while we reserve the right to introduce more math much later on, we'll wrap up this chapter here.

If you're eager to learn more about linear algebra, here are some of our favorite resources on the topic
* For a solid primer on basics, check out Gilbert Strang's book [Introduction to Linear Algebra](http://math.mit.edu/~gs/linearalgebra/)
* Zico Kolter's [Linear Algebra Reivew and Reference](http://www.cs.cmu.edu/~zkolter/course/15-884/linalg-review.pdf)

## Next
[Probability and statistics](../chapter01_crashcourse/probability.ipynb)

For whinges or inquiries, [open an issue on  GitHub.](https://github.com/zackchase/mxnet-the-straight-dope)