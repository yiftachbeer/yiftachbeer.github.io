---
layout: post
title: (Work in progress) How to Think About Matrix Multiplication
---

## Intro

One of the most common concepts in linear algebra is matrix multiplication, which can be summarized by a single formula. If $A$ is a matrix of size $m \times n$ and $B$ is a matrix of size $n \times p$, then $C = A \cdot B$ is a matrix of size $m \times p$, where its cells are given by the following formula:

$$ C_{ij}=\sum_{k=1}^{n} A_{ik} \cdot B_{kj} $$

In other words, the cell at row $i$ and column $j$ in $C$ is calculated by summing the products of each item in the $i$-th row of $A$ and the $j$-th column of $B$.

(animation for basic intuition, e.g. Wikipedia)

Stopping the explanation at this point, however, is doing matrix multiplication an injustice - this technical definition does not give any helpful intuition on in what situations matrix multiplications might arise, or how its output is affected by its input. To make things worse, the same concept has multiple possible interpretations each suitable to a different situation, and having only a single interpretation in mind might only make it confusing when another interpretation is more suitable.

The goal of this post, therefore, is to outline some common interpretations of matrix multiplication, propose how to best mentally visualize them, and help identify when each of them might be useful.

A key to having a good intuition in mind is to be very clear and explicit about what the input and output represent. We therefore start with describing the various options to think about them.

## The Data

### Vectors

The most basic building block of linear algebra is a vector, which is just an array of numbers:

(image of array of numbers)

This bare bones representation is mainly useful when each cell describes a completely different property in a different scale, which just happen to be grouped in an arbitrary ordering. For example, if the vector represents the attributes of a house, the first cell might contain its size and the second cell its number of rooms.

Sometimes, however, we prefer to think of these numbers as coordinates of a space. While this is always possible, it is mainly helpful when all cells contain numbers with similar meaning and scale (otherwise distances and angles don't tell us much). It is then helpful to imagine the vector as a point in space, or an arrow from the origin to that point:

(images of vectors in euclidean space)

Lastly, sometimes our vectors represent objects in some domain such as an image or an audiowave. While also made of numbers, they may have domain-specific interpretation, e.g. making all numbers composing an image smaller makes the image darker.

(image of images with corresponding numbers)

The appropriate visualization for a vector generally depends on the context in which it is going to be used. For that, let us introduce a way to group vectors together and to apply transformations to them.

### Matrices

Another important building block is the matrix, which is a two-dimensional array of numbers, also known as a table:

(image of 2d array of numbers)

While all matrices are made up of such numbers, sometimes the numbers have an additional meaning. The most simple example is a matrix that is formed by grouping vectors, as rows or as columns:

(image of 2d array of numbers split into rows or columns)
?(image of groups of the objects from previous paragraph?)

Every matrix can also be used as a transformation which takes a vector as an input and outputs another vector. Instead of saying we apply the matrix to a vector, we say we multiply the matrix with the vector. While we will get to the way such a transformation works later in this post, a key idea is that when a matrix is interpreted as a transformation, what we visualize instead of the *content* of the matrix is usually its *effect* on vectors.

This splits into two cases - if the transformation outputs vectors with the same meaning as its input, we might visualize them inside the same space, possibly using animation to interpolate between them. But if the transformation outputs vectors with another meaning, we might visualize inputs and outputs side by side. 

(image of before and after)

Now that we understand how to think about the data, let's see how to think about transformations we can apply to it. 

## Multiplications

To build up to multiplying a matrix by a matrix, we start with multiplying a vector by a vector.

### Vector-vector multiplication

Multiplying a vector $v$ with a vector $u$ gives a single number, a scalar, that is calculated with the formula $\sum_{i=1}^{n} v_{i} \cdot u_{i}$, which we have seen appearing for each cell in the output matrix in the introduction. 

When one of the vectors is a set of coefficients, this sum is a weighted combination of the other. In the house prices setting, if the $i$-th cell of $v$ contains some attribute of the house and the corresponding cell of $u$ contains the money we can expect for one unit of this attribute, then $v \cdot u$ gives us the money we can expect for this house.

(image of weighted mix)

When both vectors are interpreted as points in space, concepts such as angles and lengths become meaningful, giving the product a geometrical intuition. 

Specifically, the product of $v$ and $u$ is: 
$$ v \cdot u = |v| \cdot |u| \cdot \cos \theta $$
Where $|v|$ is the length of $v$, $|u|$ is the length of $u$ and $\theta$ is the angle between them. Notice that the left-hand side is a product of three numbers (scalars) and not vectors. This is mainly interesting when the lengths of both vectors are constant (e.g. they are 1), and then a higher result corresponds to the angle between them being more acute. In other words, the product measures "to what degree are $u$ and $v$ pointing in the same direction" which we think of as "how similar are $u$ and $v$".

TODO describe projection in direction

(image of projection)

When both vectors represent an object of the same type (for example, an image) we still interpret their product as similarity or correlation, without thinking about angles. (image pattern matching)

(image of pattern matching)

### Matrix-vector multiplication

The interpertation for a product of the form $u = A \cdot v$ highly depends on what $A$ and $v$ represent.

When $A$ is a list of row vectors, this is equivalent to a many simultaneous vector-vector multiplications (in the house prices example - calculate the price of many houses)

When $A$ is a list of column vectors, and the vector is a list of coefficients - we think of it as mixing the columns of $A$ into a single column using $v$  (example - tissue mixing?)

$A$ is a geometric operation, $v$ is a point - $u$ is the point $v$ after being transformed by A. e.g. rotation, scaling

$A$ is a transformation, $v$ is an object - $u$ is the object $v$ after being transformed by $A$. e.g. image blurring

### Matrix-matrix multiplication

The simplest case is when B is a list of column vectors. In this case the multiplication can be broken down to a list of independent multiplications from the previous section, which we interpret using the guidelines from before, based on what $A$ represents.

For example, when A is a transformation - C is a list of transformed vectors.

If A is a list of row vectors, the result is a list of lists, i.e. a table, where the cell at index i,j is the result of multiplication between the vector at row i to vector at column j, as in the first visualization in this page. This visualization is helpful, for example, in the movie recommendation collaborative filtering scenario. 

When A is a list of column vectors - result is a list of columns, where column j is a weighted combination of the columns of A with the coefficients in column j of B (e.g. gene expression)

When both A and B are transformation, C too is a transformation that is achieved by applying B and then A, since:

$$C \cdot v = (A \cdot B) \cdot v = A \cdot (B \cdot v)$$

Lastly, a rare one that is more math-heavy is when B is a list of row vectors - result is a transformation that is the sum of n rank-1 outer products (i.e. 1d projections) between column i of A and row i of B. This gives us a way to break down the overall effect of the transformation into components (e.g. SVD).

## Conclusion

Hopefully, by now it is clear that the same concept can be visualized in multiple ways, and when these might be useful. While this list of visualizations might seem long, it is important to emphasize that some of these situations are much more common than others. The key takeaway is that being clear and explicit about what the matrices represent enables choosing the most helpful mental visualization.
