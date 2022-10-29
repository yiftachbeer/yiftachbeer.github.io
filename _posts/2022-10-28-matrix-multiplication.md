---
layout: post
title: (Work in progress) How to Think About Matrix Multiplication
---

Matrix multiplication is one of the concepts where reading the formula is easy, but developing intuition takes more time. Since it is such a common concept in linear algebra, in this post we try to develop a better intuition for it.

Matrix multiplication is formally defined as follows - if $A$ is a matrix of size $m \times n$ and $B$ is a matrix of size $n \times p$, then $C = A \cdot B$ is a matrix of size $m \times p$, where its cells are given by

$$ C_{ij}=\sum_{k=1}^{n} A_{ik} \cdot B_{kj} $$

In other words, the cell at row $i$ and column $j$ in $C$ is calculated by summing the products of each item in the $i$-th row of $A$ and the $j$-th column of $B$.

![Basic intuition]({{ site.baseurl }}/images/matrix_multiplication/basic_intuition.png "Basic intuition")

Stopping the explanation at this point, however, is doing matrix multiplication an injustice - this technical definition does not show the situations in which matrix multiplications might arise, or help understand how its output is affected by its input. To make things worse, the same concept has multiple possible interpretations each suitable to a different situation, and having in mind an interpretation unsuitable for a given situation might only make things more confusing.

The goal of this post, therefore, is to outline some common interpretations of matrix multiplication, propose how to best mentally visualize them, and help identify when each of them might be useful.

A key to having a good intuition in mind is to be very clear and explicit about what the input and output represent. We therefore start with describing the various options to think about them.

## The Data

### Vectors

The most basic building block of linear algebra is a vector, which is just an array of numbers:

![Array of numbers]({{ site.baseurl }}/images/matrix_multiplication/array_of_numbers.png "Array of numbers")

This bare-bones representation is mainly useful when each cell describes a completely different property in a different scale, which just happen to be grouped in an arbitrary ordering. For example, if the vector represents the attributes of a house, the first cell might contain its size and the second cell its number of rooms.

Sometimes, however, we prefer to think of these numbers as coordinates of a space. While this is always possible, it is mainly helpful when all cells contain numbers with similar meaning and scale (otherwise distances and angles don't tell us much). It is then helpful to imagine the vector as a point in space, or an arrow from the origin to that point:

![Vectors in euclidean space]({{ site.baseurl }}/images/matrix_multiplication/arrows_in_space.png "Vectors in euclidean space")

Lastly, sometimes our vectors represent objects in some domain such as an image or an audiowave. While also made of numbers, they may have domain-specific interpretation, e.g. making all numbers composing an image smaller makes the image darker.

![Images]({{ site.baseurl }}/images/matrix_multiplication/image_vectors.png "Images")

The appropriate visualization for a vector generally depends on the context in which it is going to be used. For that, let us introduce a way to group vectors together and to apply transformations to them.

### Matrices

Another important building block is the matrix, which is a two-dimensional array of numbers, also known as a table:

![Table of numbers]({{ site.baseurl }}/images/matrix_multiplication/table_of_numbers.png "Table of numbers")

While all matrices are made up of such numbers, sometimes the numbers come with an additional meaning. The most simple example is a matrix that is formed by grouping vectors, either as rows or as columns:

![Matrix split into rows or columns]({{ site.baseurl }}/images/matrix_multiplication/matrix_as_cols_and_rows.png "Matrix split into rows and columns>")

Every matrix can also be used as a transformation which takes a vector as an input and returns another vector as its output. Instead of saying we **apply** the matrix **to** a vector, we say we **multiply** the matrix **with** the vector. 

While we will get to the way such a transformation works later in this post, a key idea is that when a matrix is interpreted as a transformation, what we visualize instead of the **content** of the matrix is usually its **effect** on vectors.

![Before and after]({{ site.baseurl }}/images/matrix_multiplication/matrix_as_transformation.png "Before and after")

Now that we understand how to think about the data, let's see how to think about transformations we can apply to it. 

## Multiplications

To build up to multiplying a matrix by a matrix, we start with multiplying a vector by a vector.

### Vector-vector multiplication

Multiplying a vector $v$ with a vector $u$ gives a single number, a scalar, that is calculated with the formula $\sum_{i=1}^{n} v_{i} \cdot u_{i}$, which we have seen appearing for each cell in the output matrix in the introduction. Based on what the vectors represent, we can give this operation different meanings: 

1. When one of the vectors is a set of coefficients, this sum is a weighted combination of the other. In the house prices setting, if the $i$-th cell of $v$ contains some attribute of the house and the corresponding cell of $u$ contains the money we can expect for one unit of this attribute, then $v \cdot u$ gives us the money we can expect for this house.

![Weighted combination]({{ site.baseurl }}/images/matrix_multiplication/dot_as_combination_house.png "Weighted combination")

2. When both vectors are interpreted as points in space, the product also has a geometric meaning. 
An additional formula for the product of $v$ and $u$ is then

$$ v \cdot u = \|v\| \cdot \|u\| \cdot \cos(\theta) $$

Where 
$\|v\|$ is the length of $v$, $\|u\|$ is the length of $u$ and $ \theta $ is the angle between them. Notice that the right-hand side does not contain vectors, only scalars. 

This is mainly interesting when the lengths of both vectors are constant (e.g. they are 1), and then a higher result corresponds to the angle between them being more acute. In other words, the product measures "to what degree are $u$ and $v$ pointing in the same direction" which we think of as "how similar are $u$ and $v$".

![Projection]({{ site.baseurl }}/images/matrix_multiplication/dot_as_projection.png "Projection")

3. When both vectors represent an object of the same type (for example, an image) we still interpret their product as similarity or correlation, without thinking about angles.

![Pattern matching]({{ site.baseurl }}/images/matrix_multiplication/dot_as_pattern_matching.png "Pattern matching")

### Matrix-vector multiplication

The interpretation for a product of the form $u = A \cdot v$ highly depends on what $A$ and $v$ represent.

1. When $A$ is a list of row vectors, this is equivalent to a many simultaneous vector-vector multiplications (in the house prices example - calculate the price of many houses)

2. When $A$ is a list of column vectors, and the vector is a list of coefficients - we think of it as mixing the columns of $A$ into a single column using $v$  (example - tissue mixing?)

![Matrix weighted combination]({{ site.baseurl }}/images/matrix_multiplication/matrix_weighted_combination.png "Matrix weighted combination")

3. When $A$ is a transformation and $v$ is a point or object to be transformed by $A$, $u$ is the $v$ after being transformed.  

If $v$ is a point and $A$ is a geometric operation such as a rotation or scaling, then $u$ is the transformed point:

(image)

If $v$ is a higher-level object such as an image, and $A$ is a transformation such as blurring, then $u$ is the transformed object:

(image)

### Matrix-matrix multiplication

1. The simplest case is when B is a list of column vectors. In this case the multiplication can be broken down to a list of independent multiplications from the previous section, which we interpret using the guidelines from before, based on what $A$ represents.

For example, when A is a transformation - C is a list of transformed vectors.

If A is a list of row vectors, the result is a list of lists, i.e. a table, where the cell at index i,j is the result of multiplication between the vector at row i to vector at column j, as in the first visualization in this page. This visualization is helpful, for example, in the movie recommendation collaborative filtering scenario. 

When A is a list of column vectors - result is a list of columns, where column j is a weighted combination of the columns of A with the coefficients in column j of B (e.g. gene expression)

2. When both A and B are transformation, C too is a transformation that is achieved by applying B and then A, since:

$$C \cdot v = (A \cdot B) \cdot v = A \cdot (B \cdot v)$$

3. Lastly, a rare one that is more math-heavy is when B is a list of row vectors - result is a transformation that is the sum of n rank-1 outer products (i.e. 1d projections) between column i of A and row i of B. This gives us a way to break down the overall effect of the transformation into components (e.g. SVD).

## Conclusion

Hopefully, by now it is clear that the same concept can be visualized in multiple ways, and when these might be useful. While this list of visualizations might seem long, it is important to emphasize that some of these situations are much more common than others. The key takeaway is that being clear and explicit about what the matrices represent enables choosing the most helpful mental visualization.
