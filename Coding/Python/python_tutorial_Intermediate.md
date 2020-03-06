













<p align="center"> <font size=7> Python Intermediate Tutorial </font> </p>







<p align="center"> Shuai Wang </p>

<p align="center">USTC, April 11, 2019 </p>















<p align="center"> <font color=blue> Copyright (c) 2019 Shuai Wang. All rights reserved. </font> </p>

<p align="center"> <i>Shuai Wang copyrights this specification. No part of this specification may be reproduced in any form or means, without the prior written consent of Shuai Wang. </i></p>











# 2. Intermediate Tutorial

## 2.1 数据操作 (Numpy)

[Numpy](http://www.numpy.org/) is the core library for scientific computing in Python. It provides a high-performance multidimensional array object, and tools for working with these arrays. If you are already familiar with MATLAB, you might find [this tutorial useful](https://docs.scipy.org/doc/numpy-dev/user/numpy-for-matlab-users.html) to get started with Numpy.

### Arrays

#### 数组创建

A numpy array is a grid of values, all of the **same type**, and is indexed by a tuple of nonnegative integers. The number of **dimensions is the *rank* (axis) of the array**; the *shape* of an array is a tuple of integers giving the size of the array along each dimension.

We can initialize numpy arrays from nested Python lists, and access elements using square brackets:

```python
import numpy as np

a = np.array([1, 2, 3])   # Create a rank 1 array
print(type(a))            # Prints "<class 'numpy.ndarray'>"
print(a.shape)            # Prints "(3,)"
print(a[0], a[1], a[2])   # Prints "1 2 3"
a[0] = 5                  # Change an element of the array
print(a)                  # Prints "[5, 2, 3]"

b = np.array([[1,2,3],[4,5,6]])    # Create a rank 2 array
print(b.shape)                     # Prints "(2, 3)"
print(b[0, 0], b[0, 1], b[1, 0])   # Prints "1 2 4"
```

Numpy also provides many functions to create arrays:

```python
import numpy as np

a = np.zeros((2,2))   # Create an array of all zeros，维度参数是一个元组
print(a)              # Prints "[[ 0.  0.]
                      #          [ 0.  0.]]"

b = np.ones((1,2))    # Create an array of all ones
print(b)              # Prints "[[ 1.  1.]]"

c = np.full((2,2), 7)  # Create a constant array
print(c)               # Prints "[[ 7.  7.]
                       #          [ 7.  7.]]"

d = np.eye(2)         # Create a 2x2 identity matrix
print(d)              # Prints "[[ 1.  0.]
                      #          [ 0.  1.]]"

e = np.random.random((2,2))  # Create an array filled with random values
print(e)                     # Might print "[[ 0.91940167  0.08143941]
                             #               [ 0.68744134  0.87236687]]"
```

- You can read about other methods of array creation [in the documentation](http://docs.scipy.org/doc/numpy/user/basics.creation.html#arrays-creation).
- 关于如何高效创建numpy array 参见https://ask.helplib.com/python/post_544244

#### 数组查询

**ndim：**

返回的是数组的维度，返回的只有一个数，该数即表示数组的维度（轴的个数）。

**shape:**

返回表示各位维度大小的元组。

- 对于一维数组：是（6，）为什么不是（1，6），因为ndim维度为1，元组内只返回一个数。

- 对于二维数组：按照我们对矩阵的理解，第一个数代表行 轴0，第二个数代表列，轴1
- 对于多维数组：元组内维度的顺序按照数组轴的顺序出现。最后两维度便是我们理解的行和列，在做维度变换时要注意，多维数组也可以理解成对二维的堆叠。



**dtype：**

返回的是该数组的数据类型。由于图中的数据都为整形，整形返回的都是int32，浮点型就会返回float64。这是numpy使用的数据类型，注意与其他库使用的数据类型区分并判定是否兼容。比如存储图像时，一般使用Int8。



**astype**

转换数组的数据类型。

### int32 --> float64        

float64 --> int32        会将小数部分截断

string_ --> float64        如果字符串数组表示的全是数字，也可以用astype转化为数值类型




### Array indexing

Numpy offers several ways to index into arrays.

**Slicing:**
Similar to Python lists, numpy arrays can be sliced.
Since arrays may be multidimensional, you must specify a slice for each dimension of the array:

```python
import numpy as np

# Create the following **rank 2 array** with shape (3, 4)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# Use slicing to pull out the subarray consisting of the first 2 rows
# and columns 1 and 2; b is the following array of shape (2, 2):
# [[2 3]
#  [6 7]]
b = a[:2, 1:3]

# A slice of an array is a view into the same data, so modifying it
# will modify the original array.
print(a[0, 1])   # Prints "2"
b[0, 0] = 77     # b[0, 0] is the same piece of data as a[0, 1]
print(a[0, 1])   # Prints "77"
```

- You can also mix integer indexing with slice indexing. However, doing so will yield an array of lower rank than the original array. Note that this is quite different from the way that MATLAB handles array slicing:
- **Noticing the rank of array and the difference of shape caused by two kinds of slice methonds! **

```python
import numpy as np

# Create the following rank 2 array with shape (3, 4)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# Two ways of accessing the data in the middle row of the array.
# Mixing integer indexing with slices yields an array of lower rank,
# while using only slices yields an array of the same rank as the
# original array:
row_r1 = a[1, :]    # Rank 1 view of the second row of a
row_r2 = a[1:2, :]  # Rank 2 view of the second row of a
print(row_r1, row_r1.shape)  # Prints "[5 6 7 8] (4,)"
print(row_r2, row_r2.shape)  # Prints "[[5 6 7 8]] (1, 4)"

# We can make the same distinction when accessing columns of an array:
col_r1 = a[:, 1] # 1 dimention
col_r2 = a[:, 1:2] # two dimention
print(col_r1, col_r1.shape)  # Prints "[ 2  6 10] (3,)"
print(col_r2, col_r2.shape)  # Prints "[[ 2]
                             #          [ 6]
                             #          [10]] (3, 1)"
```

**Integer array indexing:**

- When you index into numpy arrays using slicing, the resulting array view will always be a subarray of the original array. 
- In contrast, integer array indexing allows you to construct arbitrary arrays using the data from another array. Here is an example:

```python
import numpy as np

a = np.array([[1,2], [3, 4], [5, 6]])

# An example of integer array indexing.
# The returned array will have shape (3,) and
print(a[[0, 1, 2], [0, 1, 0]])  # Prints "[1 4 5]",参数看作一个 index array,首先是轴０的index,然后是轴１的index,最后组成三个对应元素的index对

# The above example of integer array indexing is equivalent to this:
print(np.array([a[0, 0], a[1, 1], a[2, 0]]))  # Prints "[1 4 5]"

# When using integer array indexing, you can reuse the same
# element from the source array:
print(a[[0, 0], [1, 1]])  # Prints "[2 2]"

# Equivalent to the previous integer array indexing example
print(np.array([a[0, 1], a[0, 1]]))  # Prints "[2 2]"
```

One useful trick with integer array indexing is selecting or mutating one element from each row of a matrix:

```python
import numpy as np

# Create a new array from which we will select elements
a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])

print(a)  # prints "array([[ 1,  2,  3],
          #                [ 4,  5,  6],
          #                [ 7,  8,  9],
          #                [10, 11, 12]])"

# Create an array of indices
b = np.array([0, 2, 0, 1])

# Select one element from each row of a using the indices in b
print(a[np.arange(4), b])  # Prints "[ 1  6  7 11]" #np.arange(4)=array([0, 1, 2, 3])

# Mutate one element from each row of a using the indices in b
a[np.arange(4), b] += 10 # won't create a new array

print(a)  # prints "array([[11,  2,  3],
          #                [ 4,  5, 16],
          #                [17,  8,  9],
          #                [10, 21, 12]])
```

**Boolean array indexing:**
Boolean array indexing lets you pick out arbitrary elements of an array.
Frequently this type of indexing is used to select the elements of an array
that satisfy some condition. Here is an example:

```python
import numpy as np

a = np.array([[1,2], [3, 4], [5, 6]])

bool_idx = (a > 2)   # Find the elements of a that are bigger than 2;
                     # this returns a numpy array of Booleans of the same
                     # shape as a, where each slot of bool_idx tells
                     # whether that element of a is > 2.

print(bool_idx)      # Prints "[[False False]
                     #          [ True  True]
                     #          [ True  True]]"

# We use boolean array indexing to construct a rank 1 array
# consisting of the elements of a corresponding to the True values
# of bool_idx
print(a[bool_idx])  # Prints "[3 4 5 6]"

# We can do all of the above in a single concise statement:
print(a[a > 2])     # Prints "[3 4 5 6]"
```

For brevity we have left out a lot of details about numpy array indexing; if you want to know more you should [read the documentation](http://docs.scipy.org/doc/numpy/reference/arrays.indexing.html).



### Array Stacking

**row_stak() & column_stack()**

- stack one dimentional list or multiple dimentional array along the row or column. The inputs can be numpy arrays or python lists or both of them. But the outputs will always be a numpy array.
- note that the input data type is tuple!

```python
import numpy as np

# case1: 1-list + 1-list
>>> x = [1,2]
>>> row = [3,4]
>>> x = np.row_stack((x,row))
>>> x
array([[1, 2],
       [3, 4]])

# case2: 1-list + 2-list
>>> x = [1,2]
>>> row2 = [[3,4],[5,6]]
>>> x = np.row_stack((x,row2))
>>> x
array([[1, 2],
       [3, 4],
       [5, 6]])

# case3: 1-list + 2-array
>>> row2 = np.array([[3,4],[5,6]])
>>> x = np.row_stack((x,row2))
>>> x
array([[1, 2],
       [3, 4],
       [5, 6]])


# case4: 2-array + 1-list
>>> x = np.array(([1,2],[3,4]))
>>> col = [5,6]
>>> x = np.column_stack((x,col))
>>> x
array([[1, 2, 5],
       [3, 4, 6]])


# case5: 2-array + 2-list
>>> x = np.array(([1,2],[3,4]))
>>> col = [[5,6],[7,8]]
>>> x = np.column_stack((x,col))
>>> x
array([[1, 2, 5, 6],
       [3, 4, 7, 8]])

# case6: 2-array + 2-array
>>> x = np.array(([1,2],[3,4]))
>>> col = np.array([[5,6],[7,8]])
>>> x = np.column_stack((x,col))
>>> x
array([[1, 2, 5, 6],
       [3, 4, 7, 8]])

# case7: 2-list + 2-list 
>>> x = [[1,2],[3,4]]
>>> col = [[5,6],[7,8]]
>>> x = np.row_stack((x,col))
>>> x
array([[1, 2],
       [3, 4],
       [5, 6],
       [7, 8]])

```



**hstack()、vstack（）、stack（）、dstack（）、vsplit（）、concatenate（）**

- stack（）：沿着新的轴加入一系列数组。
- vstack（）：堆栈数组垂直顺序（行）
- hstack（）：堆栈数组水平顺序（列）。
- dstack（）：堆栈数组按顺序深入（沿第三维）。
- vsplit（）：将数组分解成垂直的多个子数组的列表。
- concatenate（）：连接沿现有轴的数组序列。

- 判断stack之后的维度根据原始各个轴的维度以及堆叠的维度来判断，最外面的括号是轴0，依次往里增加；

1. numpy.stack()函数
   函数原型：numpy.stack(arrays,axis=0)
   示例：

![](https://img-blog.csdn.net/20170925102550583)

2. numpy.hstack()函数
   函数原型：numpy.hstack(tup)，其中tup是arrays序列，阵列必须具有相同的形状，除了对应于轴的维度（默认情况下，第一个）。
   等价于np.concatenate（tup,axis=1）
   示例：

   ![](https://img-blog.csdn.net/20170925103154996)

3. numpy.vstack()函数
   函数原型：numpy.vstack(tup)
   等价于：np.concatenate(tup,axis=0) 
   示例：

   ![](https://img-blog.csdn.net/20170925103643317)

4. numpy.dstack()函数
   函数原型：numpy.dstack(tup)
   等价于：np.concatenate(tup,axis=2)
   示例：

   ![](https://img-blog.csdn.net/20170925103942145)

5. numpy.concatenate()函数
   函数原型：numpy.concatenate((a1,a2,...),axis=0)
   示例：

   ![](https://img-blog.csdn.net/20170925110600199)

6. numpy.vsplit()函数
   函数原型：numpy.vsplit(ary,indices_or_sections)
   示例：

![](https://img-blog.csdn.net/20170925111053737)



### Array deleting

```python
#删除一列

>>> dataset=[[1,2,3],[2,3,4],[4,5,6]]
>>> import numpy as np
>>> dataset = np.delete(dataset, -1, axis=1)
>>> dataset
array([[1, 2],
       [2, 3],
       [4, 5]])

# 删除多列
arr = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
np.delete(arr, [1,2], axis=1)
array([[ 1,  4],
       [ 5,  8],
       [ 9, 12]])

```



#### Array new axis

```python
x = np.random.randint(1, 8, size=5)

x
Out[48]: array([4, 6, 6, 6, 5])

x1 = x[np.newaxis, :]

x1
Out[50]: array([[4, 6, 6, 6, 5]])

x2 = x[:, np.newaxis]

x2
Out[52]: 
array([[4],
       [6],
       [6],
       [6],
       [5]])
```

由以上代码可以看出，当把newaxis放在前面的时候

以前的shape是5，现在变成了1××5，也就是前面的维数发生了变化，后面的维数发生了变化

而把newaxis放后面的时候，输出的新数组的shape就是5××1，也就是后面增加了一个维数

所以，newaxis放在第几个位置，就会在shape里面看到相应的位置增加了一个维数

![](https://img-blog.csdn.net/20170518222227406?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveHRpbmdqaWU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Datatypes

Every numpy array is a grid of elements of the **same type**. Numpy provides a large set of numeric datatypes that you can use to construct arrays. Numpy tries to guess a datatype when you create an array, but functions that construct arrays usually also include an optional argument to explicitly specify the datatype. Here is an example:

```python
import numpy as np

x = np.array([1, 2])   # Let numpy choose the datatype
print(x.dtype)         # Prints "int64"

x = np.array([1.0, 2.0])   # Let numpy choose the datatype
print(x.dtype)             # Prints "float64"

x = np.array([1, 2], dtype=np.int64)   # Force a particular datatype
print(x.dtype)                         # Prints "int64"
```

You can read all about numpy datatypes [in the documentation](http://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html).

<a name='numpy-math'></a>

### Array math

Basic mathematical functions operate **elementwise** on arrays, and are available both as operator overloads and as functions in the numpy module:

```python
import numpy as np

x = np.array([[1,2],[3,4]], dtype=np.float64)
y = np.array([[5,6],[7,8]], dtype=np.float64)

# Elementwise sum; both produce the array
# [[ 6.0  8.0]
#  [10.0 12.0]]
print(x + y)
print(np.add(x, y))

# Elementwise difference; both produce the array
# [[-4.0 -4.0]
#  [-4.0 -4.0]]
print(x - y)
print(np.subtract(x, y))

# Elementwise product; both produce the array
# [[ 5.0 12.0]
#  [21.0 32.0]]
print(x * y)
print(np.multiply(x, y))

# Elementwise division; both produce the array
# [[ 0.2         0.33333333]
#  [ 0.42857143  0.5       ]]
print(x / y)
print(np.divide(x, y))

# Elementwise square root; produces the array
# [[ 1.          1.41421356]
#  [ 1.73205081  2.        ]]
print(np.sqrt(x))
```

Note that unlike MATLAB, `*` is elementwise multiplication, not matrix multiplication. We instead use the `dot` function to compute inner products of vectors, to multiply a vector by a matrix, and to multiply matrices. `dot` is available both as a function in the numpy module and as an instance method of array objects:

```python
import numpy as np

x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])

v = np.array([9,10])
w = np.array([11, 12])

# Inner product of vectors; both produce 219
print(v.dot(w))
print(np.dot(v, w))

# Matrix / vector product; --the result is array (one dimentional array showed in row by default)
#both produce the rank 1 array [29 67]
print(x.dot(v))
print(np.dot(x, v)) 

# Matrix / matrix product; both produce the rank 2 array
# [[19 22]
#  [43 50]]
print(x.dot(y))
print(np.dot(x, y))
```

Numpy provides many useful functions for performing computations on arrays; one of the most useful is `sum`:

```python
import numpy as np

x = np.array([[1,2],[3,4]])

print(np.sum(x))  # Compute sum of all elements; prints "10"
print(np.sum(x, axis=0))  # Compute sum of each column; prints "[4 6]"
print(np.sum(x, axis=1))  # Compute sum of each row; prints "[3 7]"
```

You can find the full list of mathematical functions provided by numpy [in the documentation](http://docs.scipy.org/doc/numpy/reference/routines.math.html).

Apart from computing mathematical functions using arrays, we frequently need to **reshape** or otherwise manipulate data in arrays. The simplest example of this type of operation is transposing a matrix; to transpose a matrix, simply use the `T` attribute of an array object:

```python
import numpy as np

x = np.array([[1,2], [3,4]])
print(x)    # Prints "[[1 2]
            #          [3 4]]"
print(x.T)  # Prints "[[1 3]
            #          [2 4]]"

# Note that taking the transpose of a rank 1 array does nothing:
v = np.array([1,2,3])
print(v)    # Prints "[1 2 3]"
print(v.T)  # Prints "[1 2 3]"
```

Numpy provides many more functions for manipulating arrays; you can see the full list
[in the documentation](http://docs.scipy.org/doc/numpy/reference/routines.array-manipulation.html).

<a name='numpy-broadcasting'></a>



#### 取整计算

**around：**

- np.around 返回四舍五入后的值，可指定精度。

  ```python
  around(a, decimals=0, out=None)
  
  # a 输入数组
  # decimals 要舍入的小数位数。 默认值为0。 如果为负，整数将四舍五入到小数点左侧的位置 
  ```

**floor** (向下取整)

- np.floor 返回不大于输入参数的最大整数。 即对于输入值 x ，将返回最大的整数 i ，使得 i <= x。 注意在Python中，向下取整总是从 0 舍入。

**ceil:** (向上取整)

- np.ceil 函数返回输入值的上限，即对于输入 x ，返回最小的整数 i ，使得 i> = x。

####  np.where

- `numpy.where(condition, x, y])`根据 condition 从 x 和 y 中选择元素，当为 True 时，选 x，否则选 y。

https://docs.scipy.org/doc/numpy/reference/generated/numpy.where.html

```python
import numpy as np

data = np.random.random([2, 3])
print data
'''
[[ 0.93122679  0.82384876  0.28730977]
 [ 0.43006042  0.73168913  0.02775572]]
'''

result = np.where(data > 0.5, data, 0)
print result
'''
[[ 0.93122679  0.82384876  0.        ]
 [ 0.          0.73168913  0.        ]]
 '''

```

#### np.any(), np.all()

> any(iterables)和all(iterables)对于检查两个对象相等时非常实用，但是要注意，any和all是python内置函数，同时numpy也有自己实现的any和all，功能与python内置的一样，只不过把numpy.ndarray类型加进去了。因为python内置的对高于1维的ndarray没法理解，所以numpy基于的计算最好用numpy自己实现的any和all。

本质上讲，any()实现了或(OR)运算，而all()实现了与(AND)运算。

- 对于any(iterables)，如果可迭代对象iterables中任意存在每一个元素为True则返回True。特例：若可迭代对象为空，比如空列表[]，则返回False。 
- 对于all(iterables)，如果可迭代对象iterables中所有元素都为True则返回True。特例：若可迭代对象为空，比如空列表[]，则返回True。 

### Broadcasting

Broadcasting is a powerful mechanism that allows numpy to work with arrays of different
shapes when performing arithmetic operations. Frequently we have a smaller array and a
larger array, and **we want to use the smaller array multiple times to perform some operation**
**on the larger array.**

For example, suppose that we want to add a constant vector to each row of a matrix. We could do it like this:

```python
import numpy as np

# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = np.empty_like(x)   # Create an empty matrix with the same shape as x

# Add the vector v to each row of the matrix x with an explicit loop
for i in range(4):
    y[i, :] = x[i, :] + v

# Now y is the following
# [[ 2  2  4]
#  [ 5  5  7]
#  [ 8  8 10]
#  [11 11 13]]
print(y)
```

This works; however when the matrix `x` is very large, computing an explicit loop
in Python could be slow. Note that adding the vector `v` to each row of the matrix
`x` is equivalent to forming a matrix `vv` by stacking multiple copies of `v` vertically,
then performing elementwise summation of `x` and `vv`. We could implement this
approach like this:

```python
import numpy as np

# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
vv = np.tile(v, (4, 1))   # Stack 4 copies of v on top of each other
print(vv)                 # Prints "[[1 0 1]
                          #          [1 0 1]
                          #          [1 0 1]
                          #          [1 0 1]]"
y = x + vv  # Add x and vv elementwise
print(y)  # Prints "[[ 2  2  4
          #          [ 5  5  7]
          #          [ 8  8 10]
          #          [11 11 13]]"
```

Numpy broadcasting allows us to perform this computation without actually
creating multiple copies of `v`. Consider this version, using broadcasting:

```python
import numpy as np

# We will add the vector v to each row of the matrix x,
# storing the result in the matrix y
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = x + v  # Add v to each row of x using broadcasting
print(y)  # Prints "[[ 2  2  4]
          #          [ 5  5  7]
          #          [ 8  8 10]
          #          [11 11 13]]"
```

The line `y = x + v` works even though `x` has shape `(4, 3)` and `v` has shape
`(3,)` due to broadcasting; this line works as if `v` actually had shape `(4, 3)`,
where each row was a copy of `v`, and the sum was performed elementwise.

Broadcasting two arrays together follows these rules:

1. If the arrays do not have the same rank, prepend the shape of the lower rank array
   with 1s until both shapes have the same length.
2. The two arrays are said to be *compatible* in a dimension if they have the same
   size in the dimension, or if one of the arrays has size 1 in that dimension.
3. The arrays can be broadcast together if they are compatible in all dimensions.
4. After broadcasting, each array behaves as if it had shape equal to the elementwise
   **maximum of shapes** of the two input arrays.
5. In any dimension where one array had size 1 and the other array had size greater than 1,
   the first array behaves as if it were copied along that dimension

If this explanation does not make sense, try reading the explanation [from the documentation](http://docs.scipy.org/doc/numpy/user/basics.broadcasting.html) or [this explanation](http://wiki.scipy.org/EricsBroadcastingDoc).

Functions that support broadcasting are known as *universal functions*. You can find the list of all universal functions [in the documentation](http://docs.scipy.org/doc/numpy/reference/ufuncs.html#available-ufuncs).

Here are some applications of broadcasting:

```python
import numpy as np

# Compute outer product of vectors
v = np.array([1,2,3])  # v has shape (3,)
w = np.array([4,5])    # w has shape (2,)
# To compute an outer product, we first reshape v to be a column
# vector of shape (3, 1); we can then broadcast it against w to yield
# an output of shape (3, 2), which is the outer product of v and w:
# [[ 4  5]
#  [ 8 10]
#  [12 15]]
print(np.reshape(v, (3, 1)) * w)

# Add a vector to each row of a matrix
x = np.array([[1,2,3], [4,5,6]])
# x has shape (2, 3) and v has shape (3,) so they broadcast to (2, 3),
# giving the following matrix:
# [[2 4 6]
#  [5 7 9]]
print(x + v)

# Add a vector to each column of a matrix
# x has shape (2, 3) and w has shape (2,).
# If we transpose x then it has shape (3, 2) and can be broadcast
# against w to yield a result of shape (3, 2); transposing this result
# yields the final result of shape (2, 3) which is the matrix x with
# the vector w added to each column. Gives the following matrix:
# [[ 5  6  7]
#  [ 9 10 11]]
print((x.T + w).T)
# Another solution is to reshape w to be a column vector of shape (2, 1);
# we can then broadcast it directly against x to produce the same
# output.
print(x + np.reshape(w, (2, 1)))

# Multiply a matrix by a constant:
# x has shape (2, 3). Numpy treats scalars as arrays of shape ();
# these can be broadcast together to shape (2, 3), producing the
# following array:
# [[ 2  4  6]
#  [ 8 10 12]]
print(x * 2)
```

Broadcasting typically makes your code more concise and faster, so you should strive to use it where possible.



### Reshape & Resize 

NumPy 中有两个跟形状转换相关的函数（及方法）`reshape` 以及 `resize`，它们都能方便的改变矩阵的形状，但是它们之间又有一个显著的差别，我们会着重的来讲。

**numpy.reshape():**

我们先来看看会在各种数据计算中经常用到的改变数组形状的函数 `reshape()`。

```python
import numpy as np

arrayA = np.arange(8)
# arrayA = array([0, 1, 2, 3, 4, 5, 6, 7])

np.reshape(arrayA, (2, 4))
#array([[0, 1, 2, 3],
#       [4, 5, 6, 7]])
```

这里它把一个有 8 个元素的向量，转换成了形状为 `(4, 2)` 的一个矩阵。因为转换前后的元素数目一样，所以能够成功的进行转换，假如前后数目不一样的话，就会有错误 `ValueError` 报出。

```python
In [1]: np.reshape(arrayA, (3, 4))
    ---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
ValueError: cannot reshape array of size 8 into shape (3,4)
```

我们仔细看转换后的数据，第一行是 `arrayA` 的前四个数据，第二行是 `arrayA` 的后四个数据，也就是它是按行来填充数据的，有些时候我们需要把数据填充的顺序改成按列来填充，那我们需要改变函数中的另外一个输入参数 `order=`

```python
In [1]: np.reshape(arrayA, (2, 4), order='F')
Out[1]: array([[0, 2, 4, 6],
        	  [1, 3, 5, 7]])
```

`order` 默认的参数是 `C`，也就是按行填充，当参数变为 `F` 时，就变成按列填充。

**说明**

这里用按行或者按列来填充，是为了便于理解，其实具体的不同是由于函数是按照类似 `C` 的索引顺序还是按照类似 `Fortan` 的索引顺序来读取输入矩阵的内容，具体可以参照[Numpy reshape 的官方说明文档](https://docs.scipy.org/doc/numpy/reference/generated/numpy.reshape.html)。

**reshape() 函数/方法内存:**

`reshape` 函数或者方法生成的新数组和原始数组是共用一个内存的，有点类似于 Python 里面的 `shallow copy`，当你改变一个数组的元素，另外一个数组的元素也相应的改变了。

```python
In [1]: arrayA = np.arange(8)
    	arrayB = arrayA.reshape((2, 4))
        arrayB
Out[2]:	array([[0, 1, 2, 3],
       		[4, 5, 6, 7]])
In [2]: arrayA[0] = 10
    	arrayA
Out[2]: array([10, 1, 2, 3, 4, 5, 6, 7]) 
In [3]: arrayB    
Out[3]:	array([[10, 1, 2, 3],
       		[4, 5, 6, 7]])
```

**numpy.resize():**

`numpy.resize()` 跟 `reshape` 类似，可以改变矩阵的形状，但它有几点不同，

1. 没有 `order` 参数了，它只有跟 `reshape` 里面 `order='C'`的方式。
2. 假如要转换成的矩阵形状中的元素数量跟原矩阵不同，它会强制进行转换，而不报错。

我们具体来看一下第二点

```python
In [1]: arrayA = np.arange(8)
        arrayB = np.resize(arrayA, (2, 4))
Out[1]: array([[0, 1, 2, 3],
       [4, 5, 6, 7]])
```

这是尺寸大小正常情况， 跟 `reshape` 的结果是一样的。

```python
In [1]: arrayC = np.resize(arrayA, (3, 4))
    	arrayC
Out[1]: array([[0, 1, 2, 3],
       [4, 5, 6, 7],
       [0, 1, 2, 3]])
In [2]: arrayD = np.resize(arrayA, (4, 4))
    	arrayD
Out[2]: array([[0, 1, 2, 3],
       [4, 5, 6, 7],
       [0, 1, 2, 3],
       [4, 5, 6, 7]])
```

当新形状行数超出的话，它会开始重复填充原始矩阵的内容，实现形状大小的自动调整而不报错。

```python
In [1]: arrayE = np.resize(arrayA, (2, 2))
		arrayE
Out[1]: array([[0, 1],
       [2, 3]])    
In [2]: np.resize(arrayA, (1,4))
Out[2]: array([[0, 1, 2, 3]])
```

当新形状比原形状所需要的数据小的时候，它会从原矩阵读读取出所需要个数的数据，然后按照先按行填充的方式来对新矩阵元素赋值。

注意

当新矩阵的形状在原矩阵形状内时，比如 `(2, 2` 和 `(2, 4)` 的情形，新矩阵的元素不是按照子集来获取的。比如上例中，`np.resize(arrayA, (2, 2))` 不是 `array([[0, 1], [4, 5])`。假如你需要这样的截取方法，我们会在后续的索引和切片操作中介绍到的。

```python
In [1]: np.resize(arrayA, (3, 5))
Out[1]: array([[0, 1, 2, 3, 4],
       [5, 6, 7, 0, 1],
       [2, 3, 4, 5, 6]])
```

当新形状比原形状要大时，它会先按行去填充旧矩阵的元素，并且在元素被用光后，再重复的填充这些元素，直到新矩阵的最后一元素。

**resize 函数/方法内存:**

跟 `reshape` 不一样的是，`resize` 函数/方法生成的新数组跟原数组并不共用一个内存，所以彼此元素的改变不会影响到对方。

```python
In [1]: arrayA = np.arange(8)
    	arrayB = arrayA.reshape((2, 4))
        arrayB
Out[2]:	array([[0, 1, 2, 3],
       		[4, 5, 6, 7]])
In [2]: arrayA[0] = 10
    	arrayA
Out[2]: array([10, 1, 2, 3, 4, 5, 6, 7]) 
In [3]: arrayB    
Out[3]:	array([[0, 1, 2, 3],
       		[4, 5, 6, 7]])
```

### Transpose

```python
>>> x = np.arange(4).reshape((2,2))
>>> x
array([[0, 1],
       [2, 3]])
>>>
>>> np.transpose(x)
array([[0, 2],
       [1, 3]])
>>>
>>> x = np.ones((1, 2, 3))
>>> np.transpose(x, (1, 0, 2)).shape
(2, 1, 3)
```



### Repet & Tile

numpy数组扩展函数有repeat和tile，由于数组不能进行动态扩展，故函数调用之后都重新分配新的空间来存储扩展后的数据。

**repeat函数:**

功能：对数组中的 元素 进行连续重复复制

用法：

- numpy.repeat(a, repeats, axis=None)
- a.repeats(repeats, axis=None)

其中a为数组，repeats为重复的次数，axis表示数组维度

```python
>>> import numpy as np
>>> a = np.arange(16).reshape(4,4)
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
>>> b = a.repeat(3)
# 未指定axis 参数时，会默认把高维数组flatten至一维
>>> b
array([ 0,  0,  0,  1,  1,  1,  2,  2,  2,  3,  3,  3,  4,  4,  4,  5,  5,
        5,  6,  6,  6,  7,  7,  7,  8,  8,  8,  9,  9,  9, 10, 10, 10, 11,
       11, 11, 12, 12, 12, 13, 13, 13, 14, 14, 14, 15, 15, 15])
>>> b.reshape(4,12)
array([[ 0,  0,  0,  1,  1,  1,  2,  2,  2,  3,  3,  3],
       [ 4,  4,  4,  5,  5,  5,  6,  6,  6,  7,  7,  7],
       [ 8,  8,  8,  9,  9,  9, 10, 10, 10, 11, 11, 11],
       [12, 12, 12, 13, 13, 13, 14, 14, 14, 15, 15, 15]])

>>> c
array([[ 0,  0,  0,  1,  1,  1,  2,  2,  2,  3,  3,  3],
       [ 4,  4,  4,  5,  5,  5,  6,  6,  6,  7,  7,  7],
       [ 8,  8,  8,  9,  9,  9, 10, 10, 10, 11, 11, 11],
       [12, 12, 12, 13, 13, 13, 14, 14, 14, 15, 15, 15]])
>>> c = c.repeat(3, axis=0)
>>> c
array([[ 0,  0,  0,  1,  1,  1,  2,  2,  2,  3,  3,  3],
       [ 0,  0,  0,  1,  1,  1,  2,  2,  2,  3,  3,  3],
       [ 0,  0,  0,  1,  1,  1,  2,  2,  2,  3,  3,  3],
       [ 4,  4,  4,  5,  5,  5,  6,  6,  6,  7,  7,  7],
       [ 4,  4,  4,  5,  5,  5,  6,  6,  6,  7,  7,  7],
       [ 4,  4,  4,  5,  5,  5,  6,  6,  6,  7,  7,  7],
       [ 8,  8,  8,  9,  9,  9, 10, 10, 10, 11, 11, 11],
       [ 8,  8,  8,  9,  9,  9, 10, 10, 10, 11, 11, 11],
       [ 8,  8,  8,  9,  9,  9, 10, 10, 10, 11, 11, 11],
       [12, 12, 12, 13, 13, 13, 14, 14, 14, 15, 15, 15],
       [12, 12, 12, 13, 13, 13, 14, 14, 14, 15, 15, 15],
       [12, 12, 12, 13, 13, 13, 14, 14, 14, 15, 15, 15]])
```



**tile函数:**

功能：对整个数组进行复制拼接

用法：numpy.tile(a, reps)

其中a为数组，reps为重复的次数

```python
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
>>> b = np.tile(a, 2)
>>> b
array([[ 0,  1,  2,  3,  0,  1,  2,  3],
       [ 4,  5,  6,  7,  4,  5,  6,  7],
       [ 8,  9, 10, 11,  8,  9, 10, 11],
       [12, 13, 14, 15, 12, 13, 14, 15]])
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
>>> c = np.tile(a, (2,2))
>>> c
array([[ 0,  1,  2,  3,  0,  1,  2,  3],
       [ 4,  5,  6,  7,  4,  5,  6,  7],
       [ 8,  9, 10, 11,  8,  9, 10, 11],
       [12, 13, 14, 15, 12, 13, 14, 15],
       [ 0,  1,  2,  3,  0,  1,  2,  3],
       [ 4,  5,  6,  7,  4,  5,  6,  7],
       [ 8,  9, 10, 11,  8,  9, 10, 11],
       [12, 13, 14, 15, 12, 13, 14, 15]])

```

### array to list

```python
>>> a = np.array([1, 2])
>>> a.tolist()
[1, 2]
>>> a = np.array([[1, 2], [3, 4]])
>>> list(a)
[array([1, 2]), array([3, 4])]
>>> a.tolist()
[[1, 2], [3, 4]]

# list to array
a = np.array(a.tolist()).
```



### sort

#### sort

- 

#### argsort

numpy.argsort(*a*, *axis=-1*, *kind='quicksort'*, *order=None*) 

Returns the indices that would sort an array.

Perform an indirect sort along the given axis using the algorithm specified by the *kind* keyword. It returns an array of indices of the same shape as *a* that index data along the given axis in sorted order.

Parameters:

- **a** : array_like. Array to sort.

- **axis** : int or None, optional. Axis along which to sort. The default is -1 (the last axis). If None, the flattened array is used.

- **kind** : {‘quicksort’, ‘mergesort’, ‘heapsort’, ‘stable’}, optionalSorting algorithm.

- **order** : str or list of str, optional When *a* is an array with fields defined, this argument specifies which fields to compare first, second, etc. A single field can be specified as a string, and not all fields need be specified, but unspecified fields will still be used, in the order in which they come up in the dtype, to break ties.

Returns:

- **index_array** : ndarray, int. Array of indices that sort *a* along the specified axis. If *a* is one-dimensional, `a[index_array]` yields a sorted *a*. More generally, `np.take_along_axis(a, index_array, axis=a)` always yields the sorted *a*, irrespective of dimensionality.

Examples

```python
# One dimensional array:
>>> x = np.array([3, 1, 2])
>>> np.argsort(x)
array([1, 2, 0])

# Two-dimensional array:
>>> x = np.array([[0, 3], [2, 2]])
>>> x
array([[0, 3],
       [2, 2]])
>>> np.argsort(x, axis=0)  # sorts along first axis (down)
array([[0, 1],
       [1, 0]])

>>> np.argsort(x, axis=1)  # sorts along last axis (across)
array([[0, 1],
       [0, 1]])

# Indices of the sorted elements of a N-dimensional array:

>>> ind = np.unravel_index(np.argsort(x, axis=None), x.shape)
>>> ind
(array([0, 1, 1, 0]), array([0, 0, 1, 1]))
>>> x[ind]  # same as np.sort(x, axis=None)
array([0, 2, 2, 3])
​```

# Sorting with keys:
>>> x = np.array([(1, 0), (0, 1)], dtype=[('x', '<i4'), ('y', '<i4')])
>>> x
array([(1, 0), (0, 1)],
      dtype=[('x', '<i4'), ('y', '<i4')])
>>> np.argsort(x, order=('x','y'))
array([1, 0])
>>> np.argsort(x, order=('y','x'))
array([0, 1])
```



See also

- [`sort`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.sort.html#numpy.sort)

  Describes sorting algorithms used.

- [`lexsort`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.lexsort.html#numpy.lexsort)

  Indirect stable sort with multiple keys.

- [`ndarray.sort`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.sort.html#numpy.ndarray.sort)

  Inplace sort.

- [`argpartition`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.argpartition.html#numpy.argpartition)

  Indirect partial sort.



### histogram

```python
numpy.histogram(a, bins=10, range=None, normed=None, weights=None, density=None)
```

- **a** : array_like Input data. The histogram is computed over the flattened array.

- **bins** : int or sequence of scalars or str, optional
  - If *bins* is an int, it defines the number of equal-width bins in the given range (10, by default). 
  - If *bins* is a sequence, it defines a monotonically increasing array of bin edges, including the rightmost edge, allowing for non-uniform bin widths.
  - If *bins* is a string, it defines the method used to calculate the optimal bin width, as defined by [`histogram_bin_edges`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram_bin_edges.html#numpy.histogram_bin_edges).

- **range** : (float, float), optional; The lower and upper range of the bins. If not provided, range is simply `(a.min(), a.max())`. Values outside the range are ignored. The first element of the range must be less than or equal to the second. *range* affects the automatic bin computation as well. While bin width is computed to be optimal based on the actual data within *range*, the bin count will fill the entire range including portions containing no data.



### Numpy Documentation

This brief overview has touched on many of the important things that you need to know about numpy, but is far from complete. Check out the [numpy reference](http://docs.scipy.org/doc/numpy/reference/)
to find out much more about numpy.

<a name='scipy'></a>

## 2.2 数据读写

### csv 文件

#### 写入

```python
import csv

#python2要用file替代open
with open("test.csv","w") as csvfile: 
    writer = csv.writer(csvfile)

    #先写入columns_name
    writer.writerow(["index","a_name","b_name"])
    #写入多行用writerows
    writer.writerows([[0,1,3],[1,2,3],[2,3,4]])
```



#### 读取

```python
import csv
with open("test.csv","r") as csvfile:
    reader = csv.reader(csvfile)
    #这里不需要readlines
    for line in reader:
        print line
```

**注意：**　采用以上方法，写入的都是字符串类型的！！！当是单个数值时读取时采用float(line[0]) 强制转化一下即可，但如果写入了列表，就无法转换了！！！

#### Save Numpy array to csv file

- Use “savetxt” method of numpy to save a numpy array to csv file 

- Use “genfromtxt” method to read a csv file into a numpy array 

```python
# Imports 
import numpy as np

# Let's creat a numpy array
nparray = np.array([[1, 2, 4],[1, 3, 9],[1, 4, 16]])

# Saving the array
np.savetxt("firstarray.csv", nparray, delimiter=",")

# Reading the csv into an array
firstarray = np.genfromtxt("firstarray.csv", delimiter=",")

print(firstarray)
```



```
[[  1.   2.   4.]
 [  1.   3.   9.]
 [  1.   4.  16.]]
```



### text 文件

**打开文件：**

```python
with open('/path/to/file', 'r') as f:
    data = f.read()
```

`open` 函数的常用模式主要有：

| ‘r'  |                读模式                |
| :--: | :----------------------------------: |
| ‘w'  |                写模式                |
| ‘a'  |               追加模式               |
| ‘b'  | 二进制模式（可添加到其他模式中使用） |
| ‘+'  | 读/写模式（可添加到其他模式中使用）  |

**读取文件：**

通常而言，读取文件有以下几种方式：

- 一次性读取所有内容，使用 `read()` 或 `readlines()`；
- 按字节读取，使用 `read(size)`；
- 按行读取，使用 `readline()`；

`readlines()` 方法会把文件读入一个字符串列表，文件中的每一行在列表中存为一个字符串。

假设有一个文件 data.txt，它的文件内容如下（数字之间的间隔符是'\t'）：

```python
10  1   9   9
6   3   2   8
20  10  3   23
1   4   1   10
10  8   6   3
10  2   1   6
```

我们使用 `readlines()` 将文件读入一个字符串列表：

```python
with open('data.txt', 'r') as f:
    lines = f.readlines()
    line_num = len(lines)
    print lines
    print line_num
```

执行结果：

```
['10\t1\t9\t9\n', '6\t3\t2\t8\n', '20\t10\t3\t23\n', '1\t4\t1\t10\n', '10\t8\t6\t3\n', '10\t2\t1\t6']
6
```

可以看到，列表中的每个元素都是一个字符串，刚好对应文件中的每一行。

**按字节读取：**

如果文件较小，一次性读取所有内容确实比较方便。但是，如果文件很大，比如有 100G，那就不能一次性读取所有内容了。这时，我们构造一个固定长度的缓冲区，来不断读取文件内容。

看看例子：

```python
with open('path/to/file', 'r') as f:
    while True:
        piece = f.read(1024)        # 每次读取 1024 个字节（即 1 KB）的内容
        if not piece:
            break
        print piece
```

在上面，我们使用 `f.read(1024)` 来每次读取 1024 个字节（1KB） 的文件内容，将其存到 piece，再对 piece 进行处理。

事实上，我们还可以结合 yield 来使用：

```python
def read_in_chunks(file_object, chunk_size=1024):
    """Lazy function (generator) to read a file piece by piece.
    Default chunk size: 1k."""
    while True:
        data = file_object.read(chunk_size)
        if not data:
            break
        yield data

with open('path/to/file', 'r') as f:
    for piece in read_in_chunks(f):
        print piece
```

**逐行读取**

在某些情况下，我们希望逐行读取文件，这时可以使用 `readline()` 方法。

看看例子：

```python
with open('data.txt', 'r') as f:
    while True:
        line = f.readline()     # 逐行读取
        if not line:
            break
        print line,             # 这里加了 ',' 是为了避免 print 自动换行
```

执行结果：

```
10  1   9   9
6   3   2   8
20  10  3   23
1   4   1   10
10  8   6   3
10  2   1   6
```

**文件迭代器**

在 Python 中，**文件对象是可迭代的**，这意味着我们可以直接在 for 循环中使用它们，而且是逐行迭代的，也就是说，效果和 `readline()` 是一样的，而且更简洁。

看看例子：

```python
with open('data.txt', 'r') as f:
    for line in f:
        print line,
```

在上面的代码中，f 就是一个文件迭代器，因此我们可以直接使用 `for line in f`，它是逐行迭代的。

看看执行结果：

```python
10  1   9   9
6   3   2   8
20  10  3   23
1   4   1   10
10  8   6   3
10  2   1   6
```

再看一个例子：

```python
with open(file_path, 'r') as f:
    lines = list(f)
    print lines
```

执行结果：

```python
['10\t1\t9\t9\n', '6\t3\t2\t8\n', '20\t10\t3\t23\n', '1\t4\t1\t10\n', '10\t8\t6\t3\n', '10\t2\t1\t6']
```

可以看到，我们可以对文件迭代器执行和普通迭代器相同的操作，比如上面使用 `list(open(filename))`将 f 转为一个字符串列表，这样所达到的效果和使用 `readlines` 是一样的。

**写文件**

写文件使用 `write` 方法，如下：

```python
with open('/Users/ethan/data2.txt', 'w') as f:
    f.write('one\n')
    f.write('two')
```

- 如果上述文件已存在，则会清空原内容并覆盖掉；
- 如果上述路径是正确的（比如存在 /Users/ethan 的路径），但是文件不存在（data2.txt 不存在），则会新建一个文件，并写入上述内容；
- 如果上述路径是不正确的（比如将路径写成 /Users/eth ），这时会抛出 IOError；

如果我们想往已存在的文件追加内容，可以使用 'a' 模式，如下：

```python
from datetime import datetime
date = datetime.now().strftime('%Y_%m_%d')

output_path = '/home/ubuntu16/catkin_ws/src/sonar_navigation/h5/'
output_file = output_path + 'MLP_training_' + date + '.txt'
with open(output_file, 'a') as f:
    for i in range(3):
        f.write(output_file+ '\n')
```

### Pandas

`Pandas` 是我最喜爱的库之一。通过带有标签的列和索引，`Pandas` 使我们可以以一种所有人都能理解的方式来处理数据。它可以让我们毫不费力地从诸如 `csv` 类型的文件中导入数据。我们可以用它快速地对数据进行复杂的转换和过滤等操作。`Pandas` 真是超级棒。

我觉得它和 `Numpy`、`Matplotlib` 一起构成了一个 Python 数据探索和分析的强大基础。`Scipy` （将会在下一篇推文里介绍）当然也是一大主力并且是一个绝对赞的库，但是我觉得前三者才是 Python 科学计算真正的顶梁柱。

**安装pandas:**

```
pip install pandas
```



**导入 Pandas:**

第一件事当然是请出我们的明星 —— Pandas。

```python
import pandas as pd # This is the standard
```

这是导入 `pandas` 的标准方法。我们不想一直写 `pandas` 的全名，但是保证代码的简洁和避免命名冲突都很重要，所以折中使用 `pd` 。如果你去看别人使用 `pandas` 的代码，就会看到这种导入方式。



**Pandas 中的数据类型:**

Pandas 基于两种数据类型，series 和 dataframe。

series 是一种一维的数据类型，其中的每个元素都有各自的标签。如果你之前看过这个系列关于 [Numpy](http://codingpy.com/) 的推文，你可以把它当作一个由带标签的元素组成的 `numpy` 数组。标签可以是数字或者字符。

dataframe 是一个二维的、表格型的数据结构。Pandas 的 dataframe 可以储存许多不同类型的数据，并且每个轴都有标签。你可以把它当作一个 series 的字典。

**将数据导入 Pandas:**

在对数据进行修改、探索和分析之前，我们得先导入数据。多亏了 Pandas ，这比在 `Numpy` 中还要容易。

这里我鼓励你去找到自己感兴趣的数据并用来练习。你的（或者别的）国家的网站就是不错的数据源。如果要举例的话，首推[英国政府数据](https://data.gov.uk/data/search)和[美国政府数据](http://catalog.data.gov/dataset)。[Kaggle](https://www.kaggle.com/)也是个很好的数据源。

我将使用英国降雨数据，这个数据集可以很容易地从英国政府网站上下载到。此外，我还下载了一些日本降雨量的数据。

> 英国降雨数据：[下载地址](<https://github.com/pcp1976/uk-rain/blob/master/data/uk_rain_2014.csv>) 日本的数据实在是没找到，抱歉。

```python
# Reading a csv into Pandas.
df = pd.read_csv('uk_rain_2014.csv', header=0)
df.shape # 返回数据的维度
```

> 译者注：如果你的数据集中有中文的话，最好在里面加上 `encoding = 'gbk'` ，以避免乱码问题。后面的导出数据的时候也一样。

这里我们从 `csv` 文件里导入了数据，并储存在 dataframe 中。这一步非常简单，你只需要调用 `read_csv` 然后将文件的路径传进去就行了。`header` 关键字告诉 Pandas 哪些是数据的列名。如果没有列名的话就将它设定为 `None` 。Pandas 非常聪明，所以这个经常可以省略。



**探索和分析的数据:**

现在数据已经导入到 Pandas 了，我们也许想看一眼数据来得到一些基本信息，以便在真正开始探索之前找到一些方向。

查看前 x 行的数据：

```python
# Getting first x rows.
df.head(5) # 默认是前5行
```

我们只需要调用 `head()` 函数并且将想要查看的行数传入。

得到的结果如下：

![image](https://i2.wp.com/www.datadependence.com/wp-content/uploads/2016/05/head-1.png?resize=640,124)

你可能还想看看最后几行：

```python
# Getting last x rows.
df.tail(5)
```

跟 `head` 一样，我们只需要调用 `tail` 并且传入想要查看的行数即可。注意，它并不是从最后一行倒着显示的，而是按照数据原来的顺序显示。

得到的结果如下：

![image](https://i1.wp.com/www.datadependence.com/wp-content/uploads/2016/05/tail-1.png?resize=640,126)

你通常使用列的名字来在 Pandas 中查找列。这一点很好而且易于使用，但是有时列名太长，比如调查问卷的一整个问题。不过你把列名缩短之后一切就好说了。

```python
# Changing column labels.
df.columns = ['water_year','rain_octsep', 'outflow_octsep',
              'rain_decfeb', 'outflow_decfeb', 'rain_junaug', 'outflow_junaug']

df.head(5)

```

需要注意的一点是，我故意没有在每列的标签中使用空格和破折号。之后你会看到这样为变量命名可以使我们少打一些字符。

你得到的数据与之前的一样，只是换了列的名字：

![image](https://i2.wp.com/www.datadependence.com/wp-content/uploads/2016/05/col_rename-1.png?resize=640,158)

你通常会想知道数据的另一个特征——它有多少条记录。在 Pandas 中，一条记录对应着一行，所以我们可以对数据集调用 `len` 方法，它将返回数据集的总行数：

```
# Finding out how many rows dataset has.
len(df)

```

上面的代码返回一个表示数据行数的整数，在我的数据集中，这个值是 33 。

你可能还想知道数据集的一些基本的统计数据，在 Pandas 中，这个操作简单到哭：

```python
# Finding out basic statistical information on your dataset.
pd.options.display.float_format = '{:,.3f}'.format # Limit output to 3 decimal places.
df.describe()

```

这将返回一张表，其中有诸如总数、均值、标准差之类的统计数据：

![image](https://i0.wp.com/www.datadependence.com/wp-content/uploads/2016/05/describe-1.png?resize=640,248)



**过滤:**

在探索数据的时候，你可能经常想要抽取数据中特定的样本，比如你有一个关于工作满意度的调查表，你可能就想要提取特定行业或者年龄的人的数据。

在 Pandas 中有多种方法可以实现提取我们想要的信息：

有时你想提取一整列，使用列的标签可以非常简单地做到：

```python
# Getting a column by label
df['rain_octsep']
```

注意，当我们提取列的时候，会得到一个 series ，而不是 dataframe 。记得我们前面提到过，你可以把 dataframe 看作是一个 series 的字典(key是列名)，所以在抽取列的时候，我们就会得到一个 series。

还记得我在命名列标签的时候特意指出的吗？不用空格、破折号之类的符号，这样我们就可以像访问对象属性一样访问数据集的列——只用一个点号。

```python
# Getting a column by label using .
df.rain_octsep
```

这句代码返回的结果与前一个例子完全一样——是我们选择的那列数据。

如果你读过这个系列关于 `Numpy` 的推文，你可能还记得一个叫做 **布尔过滤（boolean masking）**的技术，通过在一个数组上运行条件来得到一个布尔数组。在 Pandas 里也可以做到。

```python
# Creating a series of booleans based on a conditional
df.rain_octsep < 1000 # Or df['rain_octsep] < 1000
```

上面的代码将会返回一个由布尔值构成的 dataframe。`True` 表示在十月-九月降雨量小于 1000 mm，`False` 表示大于等于 1000 mm。

我们可以用这些条件表达式来过滤现有的 dataframe。

```python
# Using a series of booleans to filter
df[df.rain_octsep < 1000]
```

这条代码只返回十月-九月降雨量小于 1000 mm 的记录：

![image](http://liubj2016.github.io/Akuan/images/tupian.png)

也可以通过复合条件表达式来进行过滤：

```python
# Filtering by multiple conditionals
df[(df.rain_octsep < 1000) & (df.outflow_octsep < 4000)] # Can't use the keyword 'and'
```

这条代码只会返回 `rain_octsep` 中小于 1000 的和 `outflow_octsep` 中小于 4000 的记录：

注意重要的一点：这里不能用 `and` 关键字，因为会引发操作顺序的问题。必须用 `&` 和圆括号。

![image](http://liubj2016.github.io/Akuan/images/ppp.png)

如果你的数据中字符串，好消息，你也可以使用字符串方法来进行过滤：

```python
# Filtering by string methods
df[df.water_year.str.startswith('199')]
```

注意，你必须用 `.str.[string method]` ，而不能直接在字符串上调用字符方法。上面的代码返回所有 90 年代的记录：

![image](http://liubj2016.github.io/Akuan/images/199.png)



**索引:**

之前的部分展示了如何通过列操作来得到数据，但是 Pandas 的行也有标签。行标签可以是基于数字的或者是标签，而且获取行数据的方法也根据标签的类型各有不同。

如果你的行标签是数字型的，你可以通过 `iloc` 来引用：

```python
# Getting a row via a numerical index
df.iloc[30] # 这是df中的索引，从0开始，对应csv文件中的第32行

```

`iloc` 只对数字型的标签有用。它会返回给定行的 series，行中的每一列都是返回 series 的一个元素。

也许你的数据集中有年份或者年龄的列，你可能想通过这些年份或者年龄来引用行，这个时候我们就可以设置一个（或者多个）新的索引：

```python
# Setting a new index from an existing column
df = df.set_index(['water_year'])
df.head(5)
```

上面的代码将 `water_year` 列设置为索引。注意，列的名字实际上是一个列表，虽然上面的例子中只有一个元素。如果你想设置多个索引，只需要在列表中加入列的名字即可。

![image](http://liubj2016.github.io/Akuan/images/newindex.png)

上例中我们设置的索引列中都是字符型数据，这意味着我们不能继续使用 `iloc` 来引用，那我们用什么呢？用 `loc` 。

```python
# Getting a row via a label-based index
df.loc['2000/01']
```

和 `iloc` 一样，`loc` 会返回你引用的列，唯一一点不同就是此时你使用的是基于字符串的引用，而不是基于数字的。

还有一个引用列的常用常用方法—— `ix` 。如果 `loc` 是基于标签的，而 `iloc` 是基于数字的，那 `ix` 是基于什么的？事实上，`ix` 是基于标签的查询方法，但它同时也支持数字型索引作为备选。

```python
# Getting a row via a label-based or numerical index
df.ix['1999/00'] # Label based with numerical index fallback *Not recommended
```

与 `iloc`、`loc` 一样，它也会返回你查询的行。

如果 `ix` 可以同时起到 `loc` 和 `iloc` 的作用，那为什么还要用后两个？一大原因就是 `ix` 具有轻微的不可预测性。还记得我说过它所支持的数字型索引只是备选吗？这一特性可能会导致 `ix` 产生一些奇怪的结果，比如讲一个数字解释为一个位置。而使用 `iloc` 和 `loc` 会很安全、可预测并且让人放心。但是我要指出的是，`ix` 比 `iloc` 和 `loc` 要快一些。

将索引排序通常会很有用，在 Pandas 中，我们可以对 dataframe 调用 `sort_index` 方法进行排序。

```python
df.sort_index(ascending=False).head(5) #inplace=True to apple the sorting in place
```

我的索引本来就是有序的，为了演示，我将参数 `ascending` 设置为 `false`，这样我的数据就会呈降序排列。

![image](http://liubj2016.github.io/Akuan/images/sort.png)

当你将一列设置为索引的时候，它就不再是数据的一部分了。如果你想将索引恢复为数据，调用 `set_index` 相反的方法 `reset_index` 即可：

```python
# Returning an index to data
df = df.reset_index('water_year')
df.head(5)
```

这一语句会将索引恢复成数据形式：

![images](http://liubj2016.github.io/Akuan/images/hf.png)



**对数据集应用函数:**

有时你想对数据集中的数据进行改变或者某种操作。比方说，你有一列年份的数据，你需要新的一列来表示这些年份对应的年代。Pandas 中有两个非常有用的函数，`apply` 和 `applymap`。

```python
# Applying a function to a column
def base_year(year):
    base_year = year[:4]
    base_year= pd.to_datetime(base_year).year
    return base_year

df['year'] = df.water_year.apply(base_year)
df.head(5)
```

上面的代码创建了一个叫做 `year` 的列，它只将 `water_year` 列中的年提取了出来。这就是 `apply` 的用法，即对一列数据应用函数。如果你想对整个数据集应用函数，就要使用 `applymap` 。

![image](http://liubj2016.github.io/Akuan/images/apply.png)



**操作数据集的结构:**

另一常见的做法是重新建立数据结构，使得数据集呈现出一种更方便并且（或者）有用的形式。

掌握这些转换最简单的方法就是观察转换的过程。比起这篇文章的其他部分，接下来的操作需要你跟着练习以便能掌握它们。

首先，是 `groupby` ：

```python
#Manipulating structure (groupby, unstack, pivot)
# Grouby
df.groupby(df.year // 10 *10).max()
```

`groupby` 会按照你选择的列对数据集进行分组。上例是按照年代分组。不过仅仅这样做并没有什么用，我们必须对其调用函数，比如 `max` 、 `min` 、`mean` 等等。例中，我们可以得到 90 年代的均值。

![image](http://liubj2016.github.io/Akuan/images/groupby.png)

你也可以按照多列进行分组：

```python
# Grouping by multiple columns
decade_rain = df.groupby([df.year // 10 * 10, df.rain_octsep // 1000 * 1000])[['outflow_octsep',                                                              'outflow_decfeb', 'outflow_junaug']].mean()
decade_rain
```

![image](http://liubj2016.github.io/Akuan/images/groupby2.png)

接下来是 `unstack` ，最开始可能有一些困惑，它可以将一列数据设置为列标签。最好还是看看实际的操作：

```python
# Unstacking
decade_rain.unstack(0)
```

这条语句将上例中的 dataframe 转换为下面的形式。它将第 0 列，也就是 `year` 列设置为列的标签。

![image](http://liubj2016.github.io/Akuan/images/unstack.png)

让我们再操作一次。这次使用第 1 列，也就是 `rain_octsep` 列：

```
# More unstacking
decade_rain.unstack(1)
```

![image](http://liubj2016.github.io/Akuan/images/unstack.png)

在进行下次操作之前，我们先创建一个用于演示的 dataframe :

```
# Create a new dataframe containing entries which 
# has rain_octsep values of greater than 1250
high_rain = df[df.rain_octsep > 1250]
high_rain
```

上面的代码将会产生如下的 dataframe ，我们将会在上面演示**轴向旋转（pivoting）**。

![image](http://liubj2016.github.io/Akuan/images/newdata.png)

轴旋转其实就是我们之前已经看到的那些操作的一个集合。首先，它会设置一个新的索引（`set_index()`），然后对索引排序（`sort_index()`），最后调用 `unstack` 。以上的步骤合在一起就是 `pivot` 。接下来看看你能不能搞清楚下面的代码在干什么：

```
#Pivoting
#does set_index, sort_index and unstack in a row
high_rain.pivot('year', 'rain_octsep')[['outflow_octsep', 'outflow_decfeb', 'outflow_junaug']].fillna('')
```

注意，最后有一个 `.fillna('')` 。`pivot` 产生了很多空的记录，也就是值为 `NaN` 的记录。我个人觉得数据集里面有很多 `NaN` 会很烦，所以使用了 `fillna('')` 。你也可以用别的别的东西，比方说 0 。我们也可以使用 `dropna(how = 'any')` 来删除有 `NaN` 的行，不过这样就把所有的数据都删掉了，所以不这样做。

![image](http://liubj2016.github.io/Akuan/images/pivot.png)

上面的 dataframe 展示了所有降雨超过 1250 的 `outflow` 。诚然，这并不是讲解 `pivot` 实际应用最好的例子，但希望你能明白它的意思。看看你能在你的数据集上得到什么结果。



**合并数据集:**

有时你有两个相关联的数据集，你想将它们放在一起比较或者合并它们。好的，没问题，在 Pandas 里很简单：

```python
# Merging two datasets together
rain_jpn = pd.read_csv('jpn_rain.csv')
rain_jpn.columns = ['year', 'jpn_rainfall']

uk_jpn_rain = df.merge(rain_jpn, on='year')
uk_jpn_rain.head(5)
```

首先你需要通过 `on` 关键字来指定需要合并的列。通常你可以省略这个参数，Pandas 将会自动选择要合并的列。

如下图所示，两个数据集在年份这一类上合并了。`jpn_rain` 数据集只有年份和降雨量两列，通过年份列合并之后，`jpn_rain` 中只有降雨量那一列合并到了 `UK_rain` 数据集中。

![image](https://i0.wp.com/www.datadependence.com/wp-content/uploads/2016/05/merge-1.png?resize=640,132)



**使用 Pandas 快速作图:**

`Matplotlib` 很棒，但是想要绘制出还算不错的图表却要写不少代码，而有时你只是想粗略的做个图来探索下数据，搞清楚数据的含义。Pandas 通过 `plot` 来解决这个问题：

```python
# Using pandas to quickly plot graphs
uk_jpn_rain.plot(x='year', y=['rain_octsep', 'jpn_rainfall'])
```

这会调用 `Matplotlib` 快速轻松地绘出了你的数据图。通过这个图你就可以在视觉上分析数据，而且它能在探索数据的时候给你一些方向。比如，看到我的数据图，你会发现在 1995 年的英国好像有一场干旱。

![image](http://liubj2016.github.io/Akuan/images/tu.png)

你会发现英国的降雨明显少于日本，但人们却说英国总是下雨。



**保存你的数据集:**

在清洗、重塑、探索完数据之后，你最后的数据集可能会发生很大改变，并且比最开始的时候更有用。你应该保存原始的数据集，但是你同样应该保存处理之后的数据。

```python
# Saving your data to a csv
df.to_csv('uk_rain.csv')
```

上面的代码将会保存你的数据到 `csv` 文件以便下次使用。

我们对 Pandas 的介绍就到此为止了。就像我之前所说的， Pandas 非常强大，我们只是领略到了一点皮毛而已，不过你现在知道的应该足够你开始清洗和探索数据了。

像以前一样，我建议你用自己感兴趣的数据集做一下练习，坐下来，一杯啤酒配数据。这确实是你唯一熟悉 `Pandas` 以及这个系列其他库的方式。而且你也许会发现一些有趣的东西。

## 2.3 数据可视化

### PIL

<https://pillow.readthedocs.io/en/stable/>

**图片读取与保存**

```python
#1、显示图片
from PIL import Image
im = Image.open('lena.png')
im.show()

#2、将 PIL Image 图片转换为 numpy 数组
im_array = np.array(im)
# 也可以用 np.asarray(im) 区别是 np.array() 是深拷贝，np.asarray() 是浅拷贝

#3、保存 PIL 图片
from PIL import Image
I = Image.open('lena.png')
I.save('new_lena.png')

#4、将 numpy 数组转换为 PIL 图片
# 这里采用 matplotlib.image 读入图片数组，注意这里读入的数组是 float32 型的，范围是 0-1
# 而 PIL.Image 数据是 uinit8 型的，范围是0-255，所以要进行转换：
import matplotlib.image as mpimg
from PIL import Image
lena = mpimg.imread('lena.png') # 这里读入的数据是 float32 型的，范围是0-1
im = Image.fromarray(np.uinit8(lena*255))
im.show()

#5、RGB 转换为灰度图、二值化图
L = R * 299/1000 + G * 587/1000 + B * 114/1000
from PIL import Image
I = Image.open('lena.png')
I.show()
L = I.convert('L')   #转化为灰度图
L = I.convert('1')   #转化为二值化图
L.show()


from PIL import Image
import numpy as np
import matplotlib.pyplot as plt

img = Image.open('lena.jpg')
img = np.array(img)
if img.ndim == 3:
    img = img[:,:,0]
    plt.subplot(221); plt.imshow(img)
    plt.subplot(222); plt.imshow(img, cmap ='gray')
	plt.subplot(223); plt.imshow(img, cmap = plt.cm.gray)
	plt.subplot(224); plt.imshow(img, cmap = plt.cm.gray_r)
	plt.show()
```

**图片基础操作(Image 类)**：

```python
# 1. 首先需要导入需要的图像库：
     import Image
    
# 2. 读取一张图片：
     im=Image.open('/home/Picture/test.jpg')

# 3. 显示一张图片：
    im.show()

# 4. 保存图片：
    im.save("save.gif","GIF")      #保存图像为gif格式

# 5. 创建新图片：
#    Image.new(mode, size)  
#    Image.new(mode, size, color)  

    newImg = Image.new("RGBA",(640,480),(0,255,0))
    newImg.save("newImg.png","PNG")

# 6. 两张图片相加：
    Image.blend(img1, img2, alpha)    # 这里alpha表示img1和img2的比例参数

#7. 点操作：
#    im.point(function) #,这个function接受一个参数，且对图片中的每一个点执行这个函数
    out = im.point(lambda i : i*1.5)#对每个点进行50%的加强

# 8. 查看图像信息：
    im.format, im.size, im.mode

# 9.  图片裁剪：
#（left, upper, right, lower），坐标系统的原点（0, 0）是左上角
    box = (100,100,500,500)  #设置要裁剪的区域 
    region = im.crop(box)     #此时，region是一个新的图像对象。

# 10. 图像黏贴（合并）
    im.paste(region, box)#粘贴box大小的region到原先的图片对象中。 

# 11. 通道分离：
   r,g,b = im.split()#分割成三个通道  ，此时r,g,b分别为三个图像对象。

# 12. 通道合并：
   im = Image.merge("RGB", (b, g, r))#将b,r两个通道进行翻转。

# 13. 改变图像的大小：
   out = img.resize((128, 128))#resize成128*128像素大小

# 14. 旋转图像：
   out = img.rotate(45) #逆时针旋转45度  
   region = region.transpose(Image.ROTATE_180）

# 15. 图像转换：
   out = im.transpose(Image.FLIP_LEFT_RIGHT)#左右对换。
   out= im.transpose(Image.FLIP_TOP_BOTTOM)#上下对换

# 16. 图像类型转换：
    im=im.convert("RGBA")

# 17. 获取某个像素位置的值：
   im.getpixel((4,4))

# 18.  写某个像素位置的值：
   img.putpixel((4,4),(255,0,0))

#附:PIL可以对图像的颜色进行转换，并支持诸如24位彩色、8位灰度图和二值图等模式，简单的转换可以通过Image.convert(mode)函数完 成，其中mode表示输出的颜色模式，例如''L''表示灰度，''1''表示二值图模式等。但是利用convert函数将灰度图转换为二值图时，是采用 固定的阈 值127来实现的，即灰度高于127的像素值为1，而灰度低于127的像素值为0。
```



### Matplotlib

<https://matplotlib.org/index.html>

***官方例程合集：<https://matplotlib.org/gallery/index.html***>

```python
# 默认参数查看
import matplotlib.pyplot as plt
print(plt.rcParams) # to examine all values
print(plt.rcParams.get('figure.figsize')) # [6.0, 4.0]



```





#### Lines, bars and markers

List of named colors — Matplotlib 3.1.0 documentation
https://matplotlib.org/3.1.0/gallery/color/named_colors.html

matplotlib.markers — Matplotlib 3.2.0 documentation
https://matplotlib.org/api/markers_api.html

matplotlib.pyplot.plot — Matplotlib 2.1.1 documentation
https://matplotlib.org/2.1.1/api/_as_gen/matplotlib.pyplot.plot.html

#### Images, contours and fields

**Image Demo**

```python
import numpy as np
import matplotlib.cm as cm
import matplotlib.pyplot as plt
import matplotlib.cbook as cbook
from matplotlib.path import Path
from matplotlib.patches import PathPatch
```

```python
# generate a simple bivariate normal distribution
delta = 0.025
x = y = np.arange(-3.0, 3.0, delta)
X, Y = np.meshgrid(x, y)
Z1 = np.exp(-X**2 - Y**2)
Z2 = np.exp(-(X - 1)**2 - (Y - 1)**2)
Z = (Z1 - Z2) * 2

fig, ax = plt.subplots()
im = ax.imshow(Z, interpolation='bilinear', cmap=cm.RdYlGn,
               origin='lower', extent=[-3, 3, -3, 3],
               vmax=abs(Z).max(), vmin=-abs(Z).max())

plt.show()
```

<img src=https://matplotlib.org/_images/sphx_glr_image_demo_001.png width=200 >

```python
#  show images of pictures
# A sample image
with cbook.get_sample_data('ada.png') as image_file:
    image = plt.imread(image_file)

fig, ax = plt.subplots()
ax.imshow(image)
ax.axis('off')  # clear x-axis and y-axis


# And another image
w, h = 512, 512
with cbook.get_sample_data('ct.raw.gz') as datafile:
    s = datafile.read()
A = np.frombuffer(s, np.uint16).astype(float).reshape((w, h))
A /= A.max()

fig, ax = plt.subplots()
extent = (0, 25, 0, 25) # 一个25x25的画布？
im = ax.imshow(A, cmap=plt.cm.hot, origin='upper', extent=extent) #

markers = [(15.9, 14.5), (16.8, 15)]
x, y = zip(*markers)
ax.plot(x, y, 'o')

ax.set_title('CT density')

plt.show()
```

<img src=https://matplotlib.org/_images/sphx_glr_image_demo_002.png width=300 ><img src=https://matplotlib.org/_images/sphx_glr_image_demo_003.png width=300 >>

```python
# Interpolations for imshow/matshow
import matplotlib.pyplot as plt
import numpy as np

methods = [None, 'none', 'nearest', 'bilinear', 'bicubic', 'spline16',
           'spline36', 'hanning', 'hamming', 'hermite', 'kaiser', 'quadric',
           'catrom', 'gaussian', 'bessel', 'mitchell', 'sinc', 'lanczos']

# Fixing random state for reproducibility
np.random.seed(19680801)

grid = np.random.rand(4, 4)

fig, axs = plt.subplots(nrows=3, ncols=6, figsize=(9, 6),
                        subplot_kw={'xticks': [], 'yticks': []}) # 不显示坐标轴

for ax, interp_method in zip(axs.flat, methods):
    ax.imshow(grid, interpolation=interp_method, cmap='viridis')
    ax.set_title(str(interp_method))

plt.tight_layout()
plt.show()
```

<img src=https://matplotlib.org/_images/sphx_glr_interpolation_methods_001.png width=500 >

```python
# specify whether images should be plotted with the array origin x[0,0] in the upper left or lower right by using the origin parameter

x = np.arange(120).reshape((10, 12))

interp = 'bilinear'
fig, axs = plt.subplots(nrows=2, sharex=True, figsize=(3, 5))
axs[0].set_title('blue should be up')
axs[0].imshow(x, origin='upper', interpolation=interp)

axs[1].set_title('blue should be down')
axs[1].imshow(x, origin='lower', interpolation=interp)
plt.show()
```



```python
# show an image using a clip path
delta = 0.025
x = y = np.arange(-3.0, 3.0, delta)
X, Y = np.meshgrid(x, y)
Z1 = np.exp(-X**2 - Y**2)
Z2 = np.exp(-(X - 1)**2 - (Y - 1)**2)
Z = (Z1 - Z2) * 2

path = Path([[0, 1], [1, 0], [0, -1], [-1, 0], [0, 1]])
patch = PathPatch(path, facecolor='none')

fig, ax = plt.subplots()
ax.add_patch(patch)

im = ax.imshow(Z, interpolation='bilinear', cmap=cm.gray,
               origin='lower', extent=[-3, 3, -3, 3],
               clip_path=patch, clip_on=True)
im.set_clip_path(patch) # 仅显示path区域内的图像

plt.show()

# Note: 与之前颜色不同是因为更改了 cmap 参数
```

<img src=https://matplotlib.org/_images/sphx_glr_image_demo_006.png width=300 >

**常用参数设置**

- cmap:常用配色示例 <https://chrisalbon.com/python/basics/set_the_color_of_a_matplotlib/>

  

#### Subplots, axes and figures

```python
# 适用与python2
import matplotlib.gridspec as gridspec

fig = plt.figure(figsize=(6,4), dpi=300)
gs1 = gridspec.GridSpec(2, 2)
#gs1.update(left=0.05, right=0.48, wspace=0.05)
ax1 = fig.add_subplot(gs1[0, 0])
ax2 = fig.add_subplot(gs1[0, 1])
ax3 = fig.add_subplot(gs1[1, :])

x = np.arange(1,49,1)
ax1.plot(x, rmse, label="RMSE")
ax1.set_xlim(0,49)
ax1.set_xticks(np.arange(0, 49, 6))
ax1.set_xlabel("Sequence Length")
ax1.set_ylabel("RMSE")
ax1.legend(prop={'size':8})

#ax12.plot(x, baseline_r_square)
ax2.plot(x, r2, label='R2')
ax2.plot(x, ar2, label='R2_adjust')
ax2.set_xlim(0,49)
ax2.set_xticks(np.arange(0, 49, 6))
ax2.set_xlabel("Sequence Length")
ax2.set_ylabel("R2 & R2_adjust")
ax2.legend(loc="lower right", prop={'size':8})

ax3.stem(x, n_hidden)
ax3.set_xlim(0,49)
ax3.set_xticks(np.arange(0, 49, 4))
ax3.set_yticks(np.arange(0, 576, 64))
ax3.set_xlabel("Sequence Length")
ax3.set_ylabel("Hidden Neurons")

fig.tight_layout()

plt.show()



# python3 参考官方文档

```

区域放大图

Zoom region inset axes — Matplotlib 3.1.3 documentation
https://matplotlib.org/3.1.3/gallery/subplots_axes_and_figures/zoom_inset_axes.html

```python
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1.inset_locator import zoomed_inset_axes
from mpl_toolkits.axes_grid1.inset_locator import mark_inset

# data
payload = [0.1, 0.2, 0.3, 0.4]
single_adv = [ 2.707000,   1.031000,   1.000200,  1.000010]
orig = [2.474600,    1.002000,   1.000000,   1.000001]
mean_adv = [ 2.611600,    1.036333,   1.002000,   1.000000]
splice_orig = [ 4.550600,    1.488333,   1.207667,   1.001667]
splice_adv = [5.728200,    1.876000,   1.607667,   1.953333]

# create figure
fig, ax = plt.subplots(figsize=(6,5), dpi=300)

## 绘制主图
# plot(x, y, color='green', marker='o', linestyle='dashed',linewidth=2, markersize=12)
ax.plot(payload, splice_adv, color='cyan', linewidth=2, marker='s', label='ADV-IMS')
ax.plot(payload, splice_orig, color='cyan', linestyle='dashed',marker='s', linewidth=2, label='IMS [11]')
ax.plot(payload, single_adv, color='lime', linewidth=2, marker='o',label='ADV-SIG [31]')
ax.plot(payload, mean_adv, color='red', linewidth=2, marker='^',label='ADV-EVEN')
ax.plot(payload, orig, color='red', linestyle='dashed', linewidth=2, marker='^',label='EVEN [7]')

#设置坐标轴范围
#ax.set_xlim((0, 0.4))
#ax.set_ylim((-2, 2))
#设置坐标轴名称
ax.set_xlabel('Payload (bpp)')
ax.set_ylabel('Average rank in 50 actors')
#设置坐标轴刻度
my_x_ticks = np.arange(0.1, 0.5, 0.1)
my_y_ticks = np.arange(1, 7, 1)
ax.set_xticks(my_x_ticks)
ax.set_yticks(my_y_ticks)
ax.legend() # 放到最后会导致显式子图的label

## 绘制子图
# inset axes....
axins = ax.inset_axes([0.5, 0.31, 0.485, 0.35]) # [x0, y0, width, height] 数值代表的是比例
axins.plot(payload, single_adv, color='lime', linewidth=2, marker='o')
axins.plot(payload, mean_adv, color='red', linewidth=2, marker='^')
axins.plot(payload, orig, color='red', linestyle='dashed', linewidth=2, marker='^')

# sub region of the original image
x1, x2, y1, y2 = 0.19, 0.21, 0.99, 1.15
axins.set_xlim(x1, x2)
axins.set_ylim(y1, y2)

axins.set_xticks([0.2])
axins.set_yticks([1.00, 1.07, 1.14])

ax.indicate_inset_zoom(axins)

#axins.set_xticklabels('')
#axins.set_yticklabels('')
# fix the number of ticks on the inset axes
#axins.yaxis.get_major_locator().set_params(nbins=10)
#axins.xaxis.get_major_locator().set_params(nbins=2)
#plt.xticks(visible=True)
#plt.yticks(visible=True)

fig.tight_layout()
fig.savefig('1.eps')
plt.show()
```



#### Statistics



#### Pie and ploar charts

#### Text, labels and annotations

```python
 # 设置图例位置,loc可以为[upper, lower, left, right, center]
        plt.legend(loc="upper left", prop=myfont, shadow=True)
plt.legend(loc = 0, prop = {'size':8}) # 右上1,左上2,3,4

```



#### Pyplot



#### Color

**Colarbar**

Use [`colorbar`](https://matplotlib.org/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure.colorbar) by specifying the mappable object (here the [`AxesImage`](https://matplotlib.org/api/image_api.html#matplotlib.image.AxesImage) returned by [`imshow`](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.imshow.html#matplotlib.axes.Axes.imshow)) and the axes to attach the colorbar to.

```python
import numpy as np
import matplotlib.pyplot as plt

# setup some generic data
N = 37
x, y = np.mgrid[:N, :N]
Z = (np.cos(x*0.2) + np.sin(y*0.3))

# mask out the negative and positive values, respectively
Zpos = np.ma.masked_less(Z, 0)
Zneg = np.ma.masked_greater(Z, 0)

fig, (ax1, ax2, ax3) = plt.subplots(figsize=(13, 3), ncols=3)

# plot just the positive data and save the
# color "mappable" object returned by ax1.imshow
pos = ax1.imshow(Zpos, cmap='Blues', interpolation='none')

# add the colorbar using the figure's method,
# telling which mappable we're talking about and
# which axes object it should be near
fig.colorbar(pos, ax=ax1)

# repeat everything above for the negative data
neg = ax2.imshow(Zneg, cmap='Reds_r', interpolation='none')
fig.colorbar(neg, ax=ax2)

# Plot both positive and negative values between +/- 1.2
pos_neg_clipped = ax3.imshow(Z, cmap='RdBu', vmin=-1.2, vmax=1.2,
                             interpolation='none')
# Add minorticks on the colorbar to make it easy to read the
# values off the colorbar.
cbar = fig.colorbar(pos_neg_clipped, ax=ax3, extend='both')
cbar.minorticks_on()
plt.show()
```

<img src=https://matplotlib.org/_images/sphx_glr_colorbar_basics_001.png width=400 >

**Save image**

```python
fig1.savefig(fname="1",format="svg")    

savefig(fname, dpi=None, facecolor='w', edgecolor='w',
              orientation='portrait', papertype=None, format=None,
              transparent=False, bbox_inches=None, pad_inches=0.1,
              frameon=None)
Keyword arguments:
      *dpi*: [ *None* | ``scalar > 0`` | 'figure']
        The resolution in dots per inch.  If *None* it will default to
        the value ``savefig.dpi`` in the matplotlibrc file. If 'figure'
        it will set the dpi to be the value of the figure.    
      *facecolor*, *edgecolor*:
        the colors of the figure rectangle
      *orientation*: [ 'landscape' | 'portrait' ]
        not supported on all backends; currently only on postscript output
      *papertype*:
        One of 'letter', 'legal', 'executive', 'ledger', 'a0' through
        'a10', 'b0' through 'b10'. Only supported for postscript
        output.
      *format*:
        One of the file extensions supported by the active
        backend.  Most backends support png, pdf, ps, eps and svg.
```



#### Shapes and collections



#### Style sheets



#### Axes Grid



#### Axis Artist

#### Showcase

#### Animation

#### Event handling

#### Front Page

#### Miscellaneous

#### 3D plotting

```python
# This import registers the 3D projection, but is otherwise unused.
from mpl_toolkits.mplot3d import Axes3D  # noqa: F401 unused import

import matplotlib.pyplot as plt
import numpy as np

# Fixing random state for reproducibility
np.random.seed(19680801)


def randrange(n, vmin, vmax):
    '''
    Helper function to make an array of random numbers having shape (n, )
    with each number distributed Uniform(vmin, vmax).
    '''
    return (vmax - vmin)*np.random.rand(n) + vmin

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

n = 100

# For each set of style and range settings, plot n random points in the box
# defined by x in [23, 32], y in [0, 100], z in [zlow, zhigh].
for m, zlow, zhigh in [('o', -50, -25), ('^', -30, -5)]:
    xs = randrange(n, 23, 32)
    ys = randrange(n, 0, 100)
    zs = randrange(n, zlow, zhigh)
    ax.scatter(xs, ys, zs, marker=m)

ax.set_xlabel('X Label')
ax.set_ylabel('Y Label')
ax.set_zlabel('Z Label')

plt.show()
```



#### Our Favorite Recipes

#### Scales

#### Specialty Plots

#### Ticks and spines

```python
ax.set_xlabel('Short-Term Time Stamp', fontsize=8, fontweight='bold', color='b')
```



#### Units

#### Embedding Matplotlib in graphical user interfaces

#### Userdemo

#### Widgets

### plot

The most important function in matplotlib is `plot`,
which allows you to plot 2D data. Here is a simple example:

```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
# 在jupyter notebook 中画图需要加此句，否则第一次运行不会显示图像

# 创建函数
# 在[-3,3]区间产生50个点
x = np.linspace(-3,3,50)
y1 = 2*x + 1
y2 = x**2

# 绘图-1
plt.figure()
plt.plot(x,y1, label="y1")
plt.plot(x,y2,color='red',linewidth=2,linestyle='--', label='y2')
#添加相关图例
plt.xlabel("x")
plt.ylabel('y')
 # 设置图例位置,loc可以为[upper, lower, left, right, center]
        plt.legend(loc="upper left", prop=myfont, shadow=True)
plt.legend(loc = 0, prop = {'size':8}) # 右上1,左上2,3,4
plt.title('I')


# 调整-2
#gca 'get current axes' 获取图像的四个轴 right、top、bottom、left
ax = plt.gca() 
# 设置两个轴颜色为空隐藏两个轴
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
# 移动其余两个轴到指定位置；设置原点为(0,0)
ax.spines['bottom'].set_position(('data', 0))
ax.spines['left'].set_position(('data',0))
plt.title('II')

# 调整-3
# 利用axes对象设置轴线的显示范围，与plt.xlim(-1,2)和plt.ylim(-2,3)的作用相同
ax.set_xlim(-1,2)
ax.set_ylim(-2,3)
# 利用axes对象设置坐标轴的标签
ax.set_xlabel('x data')
ax.set_ylabel('y data')
# 设置坐标轴上的数字显示的位置，top:显示在顶部  bottom:显示在底部,默认是none
ax.xaxis.set_ticks_position('bottom')
ax.yaxis.set_ticks_position('left')
ax.set_title('III')

plt.show()
# 现在坐标轴会显示默认数据范围
```

Running this code produces the following plot:

 <img src="/media/ubuntu16/F/Self-Driving/Python/imgs/python_tutorial/plt_1.png"> <img src="/media/ubuntu16/F/Self-Driving/Python/imgs/python_tutorial/plt_2.png"> <img src="/media/ubuntu16/F/Self-Driving/Python/imgs/python_tutorial/plt_3.png">

You can read much more about the `plot` function [in the documentation](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.plot).

```python
# -*- coding: utf-8 -*-
'''
save维图与等高线图
更多用法参见：
https://blog.csdn.net/Mr_Cat123/article/details/80677525
'''

from matplotlib import pyplot as plt
import numpy as np
from mpl_toolkits.mplot3d import Axes3D

x1 = np.arange(-1, 1, 0.1)
x2 = np.arange(-1, 1, 0.1)
# 通过复制将x1,x2变成2维数据，便于点对点操作
x1_gride, x2_gride = np.meshgrid(x1, x2)
y = x1_gride**2 + x2_gride**2

fig = plt.figure()
ax = Axes3D(fig)
# 具体函数方法可用 help(function) 查看，如：help(ax.plot_surface)
ax.plot_surface(x1_gride, x2_gride, y, rstride=1, cstride=1, cmap='rainbow')
plt.show()

fig = plt.figure()
#填充颜色，f即filled
plt.contourf(x1,x2,y)
#画等高线
plt.contour(x1,x2,y)
plt.show()
```

<img src=imgs/python_tutorial/plt_4.png > <img src=imgs/python_tutorial/plt_5.png >

```python
'''
绘制三维散点图
'''

import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

data = np.random.randint(0, 255, size=[40, 40, 40])

x, y, z = data[0], data[1], data[2]
# 创建一个三维的绘图工程
ax = plt.subplot(111, projection='3d')  
#  将数据点分成三部分画，在颜色上有区分度
ax.scatter(x[:10], y[:10], z[:10], c='y')  # 绘制数据点
ax.scatter(x[10:20], y[10:20], z[10:20], c='r')
ax.scatter(x[30:40], y[30:40], z[30:40], c='g')

ax.set_zlabel('Z')  # 坐标轴
ax.set_ylabel('Y')
ax.set_xlabel('X')
plt.show()
```

<img src=imgs/python_tutorial/scatter_1.png >



动态图：



<https://zhuanlan.zhihu.com/p/31323002>

在matplotlib中画图有两种显示模式：

（1）阻塞模式，即必须利用plt.show()显示图片，且图片关闭之前代码将阻塞在该行。

（2）交互模式，即plt.plot()后立马显示图片，且不阻塞代码的继续运行。

Matplotlib中默认是使用阻塞模式。看一下这里用到的matplotlib中的几个函数：

```python3
plt.ion()：打开交互模式
plt.ioff()：关闭交互模式
plt.clf()：清除当前的Figure对象
plt.cla()：清除当前的Axes对象
plt.pause()：暂停功能
```

了解了以上几个函数之后，就可以很方便的画出动态图了。原理很简单，就是一个“画图-->清理-->画图”的循环，注意这中间的pause暂停。

```python
# _*_ coding: utf-8 _*_

"""
python_visual_animation.py by xianhu
"""

import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm
from mpl_toolkits.mplot3d import Axes3D

# 解决中文乱码问题
myfont = fm.FontProperties(fname="/Library/Fonts/Songti.ttc", size=14)
matplotlib.rcParams["axes.unicode_minus"] = False


def simple_plot():
    """
    simple plot
    """
    # 生成画布
    plt.figure(figsize=(8, 6), dpi=80)

    # 打开交互模式
    plt.ion()

    # 循环
    for index in range(100):
        # 清除原有图像
        plt.cla()

        # 设定标题等
        plt.title("动态曲线图", fontproperties=myfont)
        plt.grid(True)

        # 生成测试数据
        x = np.linspace(-np.pi + 0.1*index, np.pi+0.1*index, 256, endpoint=True)
        y_cos, y_sin = np.cos(x), np.sin(x)

        # 设置X轴
        plt.xlabel("X轴", fontproperties=myfont)
        plt.xlim(-4 + 0.1*index, 4 + 0.1*index)
        plt.xticks(np.linspace(-4 + 0.1*index, 4+0.1*index, 9, endpoint=True))

        # 设置Y轴
        plt.ylabel("Y轴", fontproperties=myfont)
        plt.ylim(-1.0, 1.0)
        plt.yticks(np.linspace(-1, 1, 9, endpoint=True))

        # 画两条曲线
        plt.plot(x, y_cos, "b--", linewidth=2.0, label="cos示例")
        plt.plot(x, y_sin, "g-", linewidth=2.0, label="sin示例")

        # 设置图例位置,loc可以为[upper, lower, left, right, center]
        plt.legend(loc="upper left", prop=myfont, shadow=True)

        # 暂停
        plt.pause(0.1)

    # 关闭交互模式
    plt.ioff()

    # 图形显示
    plt.show()
    return
# simple_plot()


def scatter_plot():
    """
    scatter plot
    """
    # 打开交互模式
    plt.ion()

    # 循环
    for index in range(50):
        # 清除原有图像
        # plt.cla()

        # 设定标题等
        plt.title("动态散点图", fontproperties=myfont)
        plt.grid(True)

        # 生成测试数据
        point_count = 5
        x_index = np.random.random(point_count)
        y_index = np.random.random(point_count)

        # 设置相关参数
        color_list = np.random.random(point_count)
        scale_list = np.random.random(point_count) * 100

        # 画散点图
        plt.scatter(x_index, y_index, s=scale_list, c=color_list, marker="o")

        # 暂停
        plt.pause(0.2)

    # 关闭交互模式
    plt.ioff()

    # 显示图形
    plt.show()
    return
# scatter_plot()


def three_dimension_scatter():
    """
    3d scatter plot
    """
    # 生成画布
    fig = plt.figure()

    # 打开交互模式
    plt.ion()

    # 循环
    for index in range(50):
        # 清除原有图像
        fig.clf()

        # 设定标题等
        fig.suptitle("三维动态散点图", fontproperties=myfont)

        # 生成测试数据
        point_count = 100
        x = np.random.random(point_count)
        y = np.random.random(point_count)
        z = np.random.random(point_count)
        color = np.random.random(point_count)
        scale = np.random.random(point_count) * 100

        # 生成画布
        ax = fig.add_subplot(111, projection="3d")

        # 画三维散点图
        ax.scatter(x, y, z, s=scale, c=color, marker=".")

        # 设置坐标轴图标
        ax.set_xlabel("X Label")
        ax.set_ylabel("Y Label")
        ax.set_zlabel("Z Label")

        # 设置坐标轴范围
        ax.set_xlim(0, 1)
        ax.set_ylim(0, 1)
        ax.set_zlim(0, 1)

        # 暂停
        plt.pause(0.2)

    # 关闭交互模式
    plt.ioff()

    # 图形显示
    plt.show()
    return
# three_dimension_scatter()
```

<https://stackoverflow.com/questions/34486642/what-is-the-currently-correct-way-to-dynamically-update-plots-in-jupyter-ipython>

```
%matplotlib notebook

import numpy as np
import matplotlib.pyplot as plt
import time

def pltsin(ax, colors=['b']):
    x = np.linspace(0,1,100)
    if ax.lines:
        for line in ax.lines:
            line.set_xdata(x)
            y = np.random.random(size=(100,1))
            line.set_ydata(y)
    else:
        for color in colors:
            y = np.random.random(size=(100,1))
            ax.plot(x, y, color)
    fig.canvas.draw()

fig,ax = plt.subplots(1,1)
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_xlim(0,1)
ax.set_ylim(0,1)
for f in range(5):
    pltsin(ax, ['b', 'r'])
    time.sleep(1)
```



### Subplot

You can plot different things in the same figure using the `subplot` function.
Here is an example:

```python
import numpy as np
import matplotlib.pyplot as plt

# Compute the x and y coordinates for points on sine and cosine curves
x = np.arange(0, 3 * np.pi, 0.1)
y_sin = np.sin(x)
y_cos = np.cos(x)

# Set up a subplot grid that has height 2 and width 1,
# and set the first such subplot as active.
plt.subplot(2, 1, 1)

# Make the first plot
plt.plot(x, y_sin)
plt.title('Sine')

# Set the second subplot as active, and make the second plot.
plt.subplot(2, 1, 2)
plt.plot(x, y_cos)
plt.title('Cosine')

# Show the figure.
plt.show()
```

<div class='fig figcenter fighighlight'>
  <img src='imgs/python_tutorial/sine_cosine_subplot.png'>
</div>

You can read much more about the `subplot` function
[in the documentation](http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.subplot).

<a name='matplotlib-images'></a>



### subplots

```python
matplotlib.pyplot.subplots(nrows=1, ncols=1, sharex=False, sharey=False, squeeze=True, subplot_kw=None, gridspec_kw=None, **fig_kw)
```

**作用：**

创建一个画像(figure)和一组子图(subplots)
**参数：**

- nrows,ncols：整型，可选参数，默认为1，表示子图网格(grid)的行数与列数
- sharex,sharey：控制x(sharex)或y(sharey)轴之间的属性共享，布尔值或者{'none','all','row','col'}，默认：False
  - True或者'all'：x或y轴属性将在所有子图(subplots)中共享.
  - False或'none'：每个子图的x或y轴都是独立的部分
  - 'row'：每个子图在一个x或y轴共享行(row)
  - 'col':每个子图在一个x或y轴共享列(column)
  - 当子图在x轴有一个共享列时('col'),只有底部子图的x tick标记是可视的。
  - 同理，当子图在y轴有一个共享行时('row'),只有第一列子图的y tick标记是可视的。
- squeeze：布尔类型，可选参数，默认：True
  - 如果是True，额外的维度从返回的Axes(轴)对象中挤出
    - 如果只有一个子图被构建(nrows=ncols=1)，结果是单个Axes对象作为标量被返回
    - 对于N*1或1*N个子图，返回一个1维数组
    - 对于N*M，N>1和M>1返回一个2维数组
  - 如果是False，不进行挤压操作：返回一个元素为Axes实例的2维数组，即使它最终是1x1
- subplot_kw:字典类型，可选参数。把字典的关键字传递给add_subplot()来创建每个子图。
- gridspec_kw字典类型，可选参数。把字典的关键字传递给GridSpec构造函数创建子图放在网格里(grid)。
- fig_kw：把所有详细的关键字参数传给figure()函数

**返回：**

- fig：matplotlib.figure.Figure对象
- ax：Axes(轴)对象或Axes(轴)对象数组。



```python
matplotlib.pyplot.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>, clear=False, **kwargs)
```

**作用 ：**创建一个新的画布(figure)。
**参数：**

- num：整型或者字符串，可选参数，默认：None。
              如果不提供该参数，一个新的画布(figure)将被创建而且画布数量将会增加。
              如果提供该参数，带有id的画布是已经存在的，激活该画布并返回该画布的引用。
              如果这个画布不存在，创建并返回画布实例。
              如果num是字符串，窗口标题将被设置为该图的数字。
- figsize：整型元组，可选参数 ，默认：None。
                  每英寸的宽度和高度。如果不提供，默认值是figure.figsize。
- dpi：整型，可选参数，默认：None。每英寸像素点。如果不提供，默认是figure.dpi。
- facecolor：背景色。如果不提供，默认值：figure.facecolor。
- edgecolor：边界颜色。如果不提供，默认值：figure.edgecolor。
- framemon：布尔类型，可选参数，默认值：True。如果是False，禁止绘制画图框。
- FigureClass：源于matplotlib.figure.Figure的类。（可选）使用自定义图实例。
- clear：布尔类型，可选参数，默认值：False。如果为True和figure已经存在时，这是清理掉改图。

**返回：**

- figure：Figure。返回的Figure实例也将被传递给后端的new_figure_manager，这允许将自定义的图类挂接到pylab接口中。 附加的kwarg将被传递给图形init函数

```python
#coding=utf8
import numpy as np
import matplotlib.pyplot as plt
#创建一个数组0-100，数据间隔是0.1
x=np.arange(0,100,0.1)
 
y=x**2
 
#调用subplots函数
#指定图像分辨率、大小和w,h比例
#创建一个800*600像素、100dpi(每英寸100点)分辨率的图形
#返回一个画布对象和一个轴数组
fig,axe=plt.subplots(figsize=(4,3),dpi=100)
 
#在axe上绘制一条抛物线，红色 点
axe.plot(x,y,"r:")

axe.set_xlabel("X")
axe.set_ylabel("Y")
axe.legend(['y'])
axe.set_title("y=x**2")
plt.show()


# 创建两个子图并共用一个y轴
f, (ax1, ax2) = plt.subplots(1, 2, sharey=True)
ax1.plot(x, y)
ax1.set_title('Sharing Y axis')
ax2.scatter(x, y)
plt.show()

# 创建四个子图并通过索引指定
fig, axes = plt.subplots(2, 2, subplot_kw=dict(polar=True))
axes[0, 0].plot(x, y)
axes[1, 1].scatter(x, y)
plt.show()
```

<img src=imgs/python_tutorial/subplots_1.png > <img src=imgs/python_tutorial/subplots_2.png > <img src=imgs/python_tutorial/subplots_3.png >



### 三维图

**一、初始化**

假设已经安装了matplotlib工具包。

利用matplotlib.figure.Figure创建一个图框：

```python
`import` `matplotlib.pyplot as plt``from` `mpl_toolkits.mplot3d ``import` `Axes3D``fig ``=` `plt.figure()``ax ``=` `fig.add_subplot(``111``, projection``=``'3d'``)`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170427232821475-1581674640.png)

**二、直线绘制（Line plots）**

基本用法：

```python
`ax.plot(x,y,z,label``=``' '``)`
```

code:



```python
`import` `matplotlib as mpl
``from` `mpl_toolkits.mplot3d ``import` `Axes3D``
import` `numpy as np`
`import` `matplotlib.pyplot as plt` 
`mpl.rcParams[``'legend.fontsize'``] ``=` `10` 
`fig ``=` `plt.figure()`
`ax ``=` `fig.gca(projection``=``'3d'``)``theta ``=` `np.linspace(``-``4` `*` `np.pi, ``4` `*` `np.pi, ``100``)``z ``=` `np.linspace(``-``2``, ``2``, ``100``)``r ``=` `z``*``*``2` `+` `1``x ``=` `r ``*` `np.sin(theta)``y ``=` `r ``*` `np.cos(theta)``ax.plot(x, y, z, label``=``'parametric curve'``)``ax.legend()` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170427233438506-816080794.png)

**三、散点绘制（Scatter plots）**

基本用法：

```python
`ax.scatter(xs, ys, zs, s``=``20``, c``=``None``, depthshade``=``True``, ``*``args, ``*``kwargs)`
```

> - xs,ys,zs：输入数据；
> - s:scatter点的尺寸
> - c:颜色，如c = 'r'就是红色；
> - depthshase:透明化，True为透明，默认为True，False为不透明
> - *args等为扩展变量，如maker = 'o'，则scatter结果为’o‘的形状

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `Axes3D``import` `matplotlib.pyplot as plt``import` `numpy as np`  `def` `randrange(n, vmin, vmax):``    ``'''``    ``Helper function to make an array of random numbers having shape (n, )``    ``with each number distributed Uniform(vmin, vmax).``    ``'''``    ``return` `(vmax ``-` `vmin)``*``np.random.rand(n) ``+` `vmin` `fig ``=` `plt.figure()``ax ``=` `fig.add_subplot(``111``, projection``=``'3d'``)` `n ``=` `100` `# For each set of style and range settings, plot n random points in the box``# defined by x in [23, 32], y in [0, 100], z in [zlow, zhigh].``for` `c, m, zlow, zhigh ``in` `[(``'r'``, ``'o'``, ``-``50``, ``-``25``), (``'b'``, ``'^'``, ``-``30``, ``-``5``)]:``    ``xs ``=` `randrange(n, ``23``, ``32``)``    ``ys ``=` `randrange(n, ``0``, ``100``)``    ``zs ``=` `randrange(n, zlow, zhigh)``    ``ax.scatter(xs, ys, zs, c``=``c, marker``=``m)` `ax.set_xlabel(``'X Label'``)``ax.set_ylabel(``'Y Label'``)``ax.set_zlabel(``'Z Label'``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170427234600069-1123635540.png)

**四、线框图（Wireframe plots）**

基本用法：

```python
`ax.plot_wireframe(X, Y, Z, ``*``args, ``*``*``kwargs)`
```

> - X,Y,Z：输入数据
> - rstride:行步长
> - cstride:列步长
> - rcount:行数上限
> - ccount:列数上限

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `axes3d``import` `matplotlib.pyplot as plt`  `fig ``=` `plt.figure()``ax ``=` `fig.add_subplot(``111``, projection``=``'3d'``)` `# Grab some test data.``X, Y, Z ``=` `axes3d.get_test_data(``0.05``)` `# Plot a basic wireframe.``ax.plot_wireframe(X, Y, Z, rstride``=``10``, cstride``=``10``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170427235353975-1165295870.png)

**五、表面图（Surface plots）**

基本用法：

```python
`ax.plot_surface(X, Y, Z, ``*``args, ``*``*``kwargs)`
```

> - X,Y,Z：数据
> - rstride、cstride、rcount、ccount:同Wireframe plots定义
> - color:表面颜色
> - cmap:图层

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `Axes3D``import` `matplotlib.pyplot as plt``from` `matplotlib ``import` `cm``from` `matplotlib.ticker ``import` `LinearLocator, FormatStrFormatter``import` `numpy as np`  `fig ``=` `plt.figure()``ax ``=` `fig.gca(projection``=``'3d'``)` `# Make data.``X ``=` `np.arange(``-``5``, ``5``, ``0.25``)``Y ``=` `np.arange(``-``5``, ``5``, ``0.25``)``X, Y ``=` `np.meshgrid(X, Y)``R ``=` `np.sqrt(X``*``*``2` `+` `Y``*``*``2``)``Z ``=` `np.sin(R)` `# Plot the surface.``surf ``=` `ax.plot_surface(X, Y, Z, cmap``=``cm.coolwarm,``                       ``linewidth``=``0``, antialiased``=``False``)` `# Customize the z axis.``ax.set_zlim(``-``1.01``, ``1.01``)``ax.zaxis.set_major_locator(LinearLocator(``10``))``ax.zaxis.set_major_formatter(FormatStrFormatter(``'%.02f'``))` `# Add a color bar which maps values to colors.``fig.colorbar(surf, shrink``=``0.5``, aspect``=``5``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170427235619381-1099728432.png)

**六、三角表面图（Tri-Surface plots）**

基本用法：

```python
`ax.plot_trisurf(``*``args, ``*``*``kwargs)`
```

> - X,Y,Z:数据
> - 其他参数类似surface-plot

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `Axes3D``import` `matplotlib.pyplot as plt``import` `numpy as np`  `n_radii ``=` `8``n_angles ``=` `36` `# Make radii and angles spaces (radius r=0 omitted to eliminate duplication).``radii ``=` `np.linspace(``0.125``, ``1.0``, n_radii)``angles ``=` `np.linspace(``0``, ``2``*``np.pi, n_angles, endpoint``=``False``)` `# Repeat all angles for each radius.``angles ``=` `np.repeat(angles[..., np.newaxis], n_radii, axis``=``1``)` `# Convert polar (radii, angles) coords to cartesian (x, y) coords.``# (0, 0) is manually added at this stage,  so there will be no duplicate``# points in the (x, y) plane.``x ``=` `np.append(``0``, (radii``*``np.cos(angles)).flatten())``y ``=` `np.append(``0``, (radii``*``np.sin(angles)).flatten())` `# Compute z to make the pringle surface.``z ``=` `np.sin(``-``x``*``y)` `fig ``=` `plt.figure()``ax ``=` `fig.gca(projection``=``'3d'``)` `ax.plot_trisurf(x, y, z, linewidth``=``0.2``, antialiased``=``True``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428064410412-2144400745.png)

**七、等高线（Contour plots）**

基本用法：

```python
`ax.contour(X, Y, Z, ``*``args, ``*``*``kwargs)`
```

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `axes3d``import` `matplotlib.pyplot as plt``from` `matplotlib ``import` `cm` `fig ``=` `plt.figure()``ax ``=` `fig.add_subplot(``111``, projection``=``'3d'``)``X, Y, Z ``=` `axes3d.get_test_data(``0.05``)``cset ``=` `ax.contour(X, Y, Z, cmap``=``cm.coolwarm)``ax.clabel(cset, fontsize``=``9``, inline``=``1``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428064857709-331889624.png)

二维的等高线，同样可以配合三维表面图一起绘制：

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `axes3d``from` `mpl_toolkits.mplot3d ``import` `axes3d``import` `matplotlib.pyplot as plt``from` `matplotlib ``import` `cm` `fig ``=` `plt.figure()``ax ``=` `fig.gca(projection``=``'3d'``)``X, Y, Z ``=` `axes3d.get_test_data(``0.05``)``ax.plot_surface(X, Y, Z, rstride``=``8``, cstride``=``8``, alpha``=``0.3``)``cset ``=` `ax.contour(X, Y, Z, zdir``=``'z'``, offset``=``-``100``, cmap``=``cm.coolwarm)``cset ``=` `ax.contour(X, Y, Z, zdir``=``'x'``, offset``=``-``40``, cmap``=``cm.coolwarm)``cset ``=` `ax.contour(X, Y, Z, zdir``=``'y'``, offset``=``40``, cmap``=``cm.coolwarm)` `ax.set_xlabel(``'X'``)``ax.set_xlim(``-``40``, ``40``)``ax.set_ylabel(``'Y'``)``ax.set_ylim(``-``40``, ``40``)``ax.set_zlabel(``'Z'``)``ax.set_zlim(``-``100``, ``100``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428065239631-897876346.png)

也可以是三维等高线在二维平面的投影：

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `axes3d``import` `matplotlib.pyplot as plt``from` `matplotlib ``import` `cm` `fig ``=` `plt.figure()``ax ``=` `fig.gca(projection``=``'3d'``)``X, Y, Z ``=` `axes3d.get_test_data(``0.05``)``ax.plot_surface(X, Y, Z, rstride``=``8``, cstride``=``8``, alpha``=``0.3``)``cset ``=` `ax.contourf(X, Y, Z, zdir``=``'z'``, offset``=``-``100``, cmap``=``cm.coolwarm)``cset ``=` `ax.contourf(X, Y, Z, zdir``=``'x'``, offset``=``-``40``, cmap``=``cm.coolwarm)``cset ``=` `ax.contourf(X, Y, Z, zdir``=``'y'``, offset``=``40``, cmap``=``cm.coolwarm)` `ax.set_xlabel(``'X'``)``ax.set_xlim(``-``40``, ``40``)``ax.set_ylabel(``'Y'``)``ax.set_ylim(``-``40``, ``40``)``ax.set_zlabel(``'Z'``)``ax.set_zlim(``-``100``, ``100``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428065622537-1308648947.png)

 **八、Bar plots（条形图）**

基本用法：

```python
`ax.bar(left, height, zs``=``0``, zdir``=``'z'``, ``*``args, ``*``*``kwargs`
```

> - x，y，zs = z，数据
> - zdir:条形图平面化的方向，具体可以对应代码理解。

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `Axes3D``import` `matplotlib.pyplot as plt``import` `numpy as np` `fig ``=` `plt.figure()``ax ``=` `fig.add_subplot(``111``, projection``=``'3d'``)``for` `c, z ``in` `zip``([``'r'``, ``'g'``, ``'b'``, ``'y'``], [``30``, ``20``, ``10``, ``0``]):``    ``xs ``=` `np.arange(``20``)``    ``ys ``=` `np.random.rand(``20``)` `    ``# You can provide either a single color or an array. To demonstrate this,``    ``# the first bar of each set will be colored cyan.``    ``cs ``=` `[c] ``*` `len``(xs)``    ``cs[``0``] ``=` `'c'``    ``ax.bar(xs, ys, zs``=``z, zdir``=``'y'``, color``=``cs, alpha``=``0.8``)` `ax.set_xlabel(``'X'``)``ax.set_ylabel(``'Y'``)``ax.set_zlabel(``'Z'``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428065810647-854711136.png)

**九、子图绘制（subplot）**

　　**A-不同的2-D图形，分布在3-D空间**，其实就是投影空间不空，对应code:

```python
`from` `mpl_toolkits.mplot3d ``import` `Axes3D``import` `numpy as np``import` `matplotlib.pyplot as plt` `fig ``=` `plt.figure()``ax ``=` `fig.gca(projection``=``'3d'``)` `# Plot a sin curve using the x and y axes.``x ``=` `np.linspace(``0``, ``1``, ``100``)``y ``=` `np.sin(x ``*` `2` `*` `np.pi) ``/` `2` `+` `0.5``ax.plot(x, y, zs``=``0``, zdir``=``'z'``, label``=``'curve in (x,y)'``)` `# Plot scatterplot data (20 2D points per colour) on the x and z axes.``colors ``=` `(``'r'``, ``'g'``, ``'b'``, ``'k'``)``x ``=` `np.random.sample(``20``*``len``(colors))``y ``=` `np.random.sample(``20``*``len``(colors))``c_list ``=` `[]``for` `c ``in` `colors:``    ``c_list.append([c]``*``20``)``# By using zdir='y', the y value of these points is fixed to the zs value 0``# and the (x,y) points are plotted on the x and z axes.``ax.scatter(x, y, zs``=``0``, zdir``=``'y'``, c``=``c_list, label``=``'points in (x,z)'``)` `# Make legend, set axes limits and labels``ax.legend()``ax.set_xlim(``0``, ``1``)``ax.set_ylim(``0``, ``1``)``ax.set_zlim(``0``, ``1``)``ax.set_xlabel(``'X'``)``ax.set_ylabel(``'Y'``)``ax.set_zlabel(``'Z'``)`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428070320694-1392528345.png)

 　　**B-子图Subplot用法**

与MATLAB不同的是，如果一个四子图效果，如：

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428071243834-1968412328.png)

> MATLAB：
>
> ```python
> `subplot(``2``,``2``,``1``)``subplot(``2``,``2``,``2``)``subplot(``2``,``2``,[``3``,``4``])`
> ```
>
> Python:
>
> ```python
> `subplot(``2``,``2``,``1``)``subplot(``2``,``2``,``2``)``subplot(``2``,``1``,``2``)`
> ```

code:

```python
`import` `matplotlib.pyplot as plt``from` `mpl_toolkits.mplot3d.axes3d ``import` `Axes3D, get_test_data``from` `matplotlib ``import` `cm``import` `numpy as np`  `# set up a figure twice as wide as it is tall``fig ``=` `plt.figure(figsize``=``plt.figaspect(``0.5``))` `#===============``#  First subplot``#===============``# set up the axes for the first plot``ax ``=` `fig.add_subplot(``2``, ``2``, ``1``, projection``=``'3d'``)` `# plot a 3D surface like in the example mplot3d/surface3d_demo``X ``=` `np.arange(``-``5``, ``5``, ``0.25``)``Y ``=` `np.arange(``-``5``, ``5``, ``0.25``)``X, Y ``=` `np.meshgrid(X, Y)``R ``=` `np.sqrt(X``*``*``2` `+` `Y``*``*``2``)``Z ``=` `np.sin(R)``surf ``=` `ax.plot_surface(X, Y, Z, rstride``=``1``, cstride``=``1``, cmap``=``cm.coolwarm,``                       ``linewidth``=``0``, antialiased``=``False``)``ax.set_zlim(``-``1.01``, ``1.01``)``fig.colorbar(surf, shrink``=``0.5``, aspect``=``10``)` `#===============``# Second subplot``#===============``# set up the axes for the second plot``ax ``=` `fig.add_subplot(``2``,``1``,``2``, projection``=``'3d'``)` `# plot a 3D wireframe like in the example mplot3d/wire3d_demo``X, Y, Z ``=` `get_test_data(``0.05``)``ax.plot_wireframe(X, Y, Z, rstride``=``10``, cstride``=``10``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428071408428-487524193.png)

 **补充：**

文本注释的基本用法：

code:

```python
`from` `mpl_toolkits.mplot3d ``import` `Axes3D``import` `matplotlib.pyplot as plt`  `fig ``=` `plt.figure()``ax ``=` `fig.gca(projection``=``'3d'``)` `# Demo 1: zdir``zdirs ``=` `(``None``, ``'x'``, ``'y'``, ``'z'``, (``1``, ``1``, ``0``), (``1``, ``1``, ``1``))``xs ``=` `(``1``, ``4``, ``4``, ``9``, ``4``, ``1``)``ys ``=` `(``2``, ``5``, ``8``, ``10``, ``1``, ``2``)``zs ``=` `(``10``, ``3``, ``8``, ``9``, ``1``, ``8``)` `for` `zdir, x, y, z ``in` `zip``(zdirs, xs, ys, zs):``    ``label ``=` `'(%d, %d, %d), dir=%s'` `%` `(x, y, z, zdir)``    ``ax.text(x, y, z, label, zdir)` `# Demo 2: color``ax.text(``9``, ``0``, ``0``, ``"red"``, color``=``'red'``)` `# Demo 3: text2D``# Placement 0, 0 would be the bottom left, 1, 1 would be the top right.``ax.text2D(``0.05``, ``0.95``, ``"2D Text"``, transform``=``ax.transAxes)` `# Tweaking display region and labels``ax.set_xlim(``0``, ``10``)``ax.set_ylim(``0``, ``10``)``ax.set_zlim(``0``, ``10``)``ax.set_xlabel(``'X axis'``)``ax.set_ylabel(``'Y axis'``)``ax.set_zlabel(``'Z axis'``)` `plt.show()`
```

![img](https://images2015.cnblogs.com/blog/1085343/201704/1085343-20170428070440772-788186162.png)





### histogram

```python
matplotlib.pyplot.hist(x, bins=None, range=None, density=None, weights=None, cumulative=False, bottom=None, histtype='bar', align='mid', orientation='vertical', rwidth=None, log=False, color=None, label=None, stacked=False, normed=None, *, data=None, **kwargs)[source]
```

**Parameters:**

- **x** : (n,) array or sequence of (n,) arrays
- **bins** : int or sequence or str, optional
  - If an integer is given, `bins + 1` bin edges are calculated and returned, consistent with [`numpy.histogram`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html#numpy.histogram).
  - If `bins` is a sequence, gives bin edges, including left edge of first bin and right edge of last bin. In this case, `bins` is returned unmodified.(左闭右开，最后闭)
  - With Numpy 1.11 or newer, you can alternatively provide a string describing a binning strategy, such as 'auto', 'sturges', 'fd', 'doane', 'scott', 'rice' or 'sqrt', see [`numpy.histogram`](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html#numpy.histogram).
  - The default is taken from `rcParams["hist.bins"] = 10`.

- **range** : tuple or None, optional; If not provided, *range* is `(x.min(), x.max())`. Range has no effect if *bins* is a sequence.

- **density** : bool, optional; If `True`, probability density, Default is `None` , value.

- **weights** : (n, ) array_like or None, optional

An array of weights, of the same shape as *x*. Each value in *x* only contributes its associated weight towards the bin count (instead of 1). If *normed* or *density* is `True`, the weights are normalized, so that the integral of the density over the range remains 1.

Default is `None`.

This parameter can be used to draw a histogram of data that has already been binned, e.g. using `np.histogram` (by treating each bin as a single point with a weight equal to its count)

```
counts, bins = np.histogram(data)
plt.hist(bins[:-1], bins, weights=counts)
```

(or you may alternatively use [`bar()`](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.bar.html#matplotlib.pyplot.bar)).

- **cumulative** : bool, optional

If `True`, then a histogram is computed where each bin gives the counts in that bin plus all bins for smaller values. The last bin gives the total number of datapoints. If *normed* or *density* is also `True` then the histogram is normalized such that the last bin equals 1. If *cumulative* evaluates to less than 0 (e.g., -1), the direction of accumulation is reversed. In this case, if *normed* and/or *density* is also `True`, then the histogram is normalized such that the first bin equals 1.

Default is `False`

**bottom** : array_like, scalar, or None

Location of the bottom baseline of each bin. If a scalar, the base line for each bin is shifted by the same amount. If an array, each bin is shifted independently and the length of bottom must match the number of bins. If None, defaults to 0.

Default is `None`

- **histtype** : {'bar', 'barstacked', 'step', 'stepfilled'}, optional

  The type of histogram to draw.

  - 'bar' is a traditional bar-type histogram. If multiple data are given the bars are arranged side by side.
  - 'barstacked' is a bar-type histogram where multiple data are stacked on top of each other.
  - 'step' generates a lineplot that is by default unfilled.
  - 'stepfilled' generates a lineplot that is by default filled.



**align** : {'left', 'mid', 'right'}, optional

Controls how the histogram is plotted.

> - 'left': bars are centered on the left bin edges.
> - 'mid': bars are centered between the bin edges.
> - 'right': bars are centered on the right bin edges.

Default is 'mid'

**orientation** : {'horizontal', 'vertical'}, optional

If 'horizontal', [`barh`](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.barh.html#matplotlib.pyplot.barh) will be used for bar-type histograms and the *bottom* kwarg will be the left edges.

- **rwidth** : scalar or None, optional

The relative width of the bars as a fraction of the bin width. If `None`, automatically compute the width.

Ignored if *histtype* is 'step' or 'stepfilled'.

Default is `None`

**log** : bool, optional

If `True`, the histogram axis will be set to a log scale. If *log* is `True` and *x* is a 1D array, empty bins will be filtered out and only the non-empty `(n, bins, patches)` will be returned.

Default is `False`

**color** : color or array_like of colors or None, optional

Color spec or sequence of color specs, one per dataset. Default (`None`) uses the standard line color sequence.

Default is `None`

**label** : str or None, optional

String, or sequence of strings to match multiple datasets. Bar charts yield multiple patches per dataset, but only the first gets the label, so that the legend command will work as expected.

default is `None`

**stacked** : bool, optional

If `True`, multiple data are stacked on top of each other If `False` multiple data are arranged side by side if histtype is 'bar' or on top of each other if histtype is 'step'

Default is `False`

**normed** : bool, optional

Deprecated; use the density keyword argument instead.



return:

**n** : array or list of arrays

The values of the histogram bins. See *density* and *weights* for a description of the possible semantics. If input *x* is an array, then this is an array of length *nbins*. If input is a sequence of arrays `[data1, data2,..]`, then this is a list of arrays with the values of the histograms for each of the arrays in the same order. The dtype of the array *n* (or of its element arrays) will always be float even if no weighting or normalization is used.

**bins** : array

The edges of the bins. Length nbins + 1 (nbins left edges and right edge of last bin). Always a single array even when multiple data sets are passed in.

**patches** : list or list of lists

Silent list of individual patches used to create the histogram or list of such list if multiple input datasets.

<https://matplotlib.org/3.1.0/gallery/statistics/hist.html>



### 保存图片

plt.savefig("test.jpg")

### Images

You can use the `imshow` function to show images. Here is an example:

```python
import numpy as np
from scipy.misc import imread, imresize
import matplotlib.pyplot as plt

img = imread('assets/cat.jpg')
img_tinted = img * [1, 0.95, 0.9]

# Show the original image
plt.subplot(1, 2, 1)
plt.imshow(img)

# Show the tinted image
plt.subplot(1, 2, 2)

# A slight gotcha with imshow is that it might give strange results
# if presented with data that is not uint8. To work around this, we
# explicitly cast the image to uint8 before displaying it.
plt.imshow(np.uint8(img_tinted))
plt.show()
```

<div class='fig figcenter fighighlight'>
  <img src='imgs/python_tutorial/cat_tinted_imshow.png'>
</div>
The following code will load an image from a file `image.png` and will display it as grayscale.

```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image

fname = 'image.png'
image = Image.open(fname).convert("L")
arr = np.asarray(image)
plt.imshow(arr, cmap='gray', vmin=0, vmax=255)
plt.show()
```

If you want to display the inverse grayscale, switch the cmap to `cmap='gray_r'`.



```python
# 1、显示图片
import matplotlib.pyplot as plt #plt 用于显示图片
import matplotlib.image as mpimg #mpimg 用于读取图片
import numpy as np
lena = mpimg.imread('lena.png') #读取和代码处于同一目录下的lena.png
# 此时 lena 就已经是一个 np.array 了，可以对它进行任意处理
lena.shape #(512, 512, 3)
plt.imshow(lena) # 显示图片
plt.axis('off') # 不显示坐标轴
plt.show()

# 2、显示图片的第一个通道
lena_1 = lena[:,:,0]
plt.imshow('lena_1')
plt.show()

# 此时会发现显示的是热量图，不是我们预想的灰度图，可以添加 cmap 参数，有如下几种添加方法：
#方法一
plt.imshow('lena_1', cmap='Greys_r')
plt.show()

#方法二
img = plt.imshow('lena_1')
img.set_cmap('gray') # 'hot' 是热量图
plt.show()


#3、将 RGB 转为灰度图
def rgb2gray(rgb):
    return np.dot(rgb[...,:3], [0.299, 0.587, 0.114])

gray = rgb2gray(lena)    
# 也可以用 plt.imshow(gray, cmap = plt.get_cmap('gray'))
plt.imshow(gray, cmap='Greys_r')
plt.axis('off')
plt.show()

#4、对图像进行放缩
from scipy import misc
lena_new_sz = misc.imresize(lena, 0.5) # 第二个参数如果是整数，则为百分比，如果是tuple，则为输出图像的尺寸
plt.imshow(lena_new_sz)
plt.axis('off')
plt.show()

附上imresize的用法
功能：改变图像的大小。
用法：
B = imresize(A,m)
B = imresize(A,m,method)
B = imresize(A,[mrows ncols],method)
B = imresize(...,method,n)
B = imresize(...,method,h)

imrersize函数使用由参数method指定的插值运算来改变图像的大小。
method的几种可选值：
'nearest'（默认值）最近邻插值
'bilinear'双线性插值
'bicubic'双三次插值
B = imresize(A,m)表示把图像A放大m倍
B = imresize(...,method,h)中的h可以是任意一个FIR滤波器（h通常由函数ftrans2、fwind1、fwind2、或fsamp2等生成的二维FIR滤波器）。


#5、保存 matplotlib 画出的图像
plt.savefig('lena_new_sz.png')

#5、将 array 保存为图像
from scipy import misc
misc.imsave('lena_new_sz.png', lena_new_sz)

#5、直接保存 array
#读取之后还是可以按照前面显示数组的方法对图像进行显示，这种方法完全不会对图像质量造成损失
np.save('lena_new_sz', lena_new_sz) # 会在保存的名字后面自动加上.npy
img = np.load('lena_new_sz.npy') # 读取前面保存的数组

```

更多图像显示参考：https://www.cnblogs.com/denny402/p/5122594.html













### SciPy

Numpy provides a high-performance multidimensional array and basic tools to compute with and manipulate these arrays. [SciPy](http://docs.scipy.org/doc/scipy/reference/) builds on this, and provides a large number of functions that operate on numpy arrays and are useful for different types of scientific and engineering applications.

The best way to get familiar with SciPy is to [browse the documentation](http://docs.scipy.org/doc/scipy/reference/index.html). We will highlight some parts of SciPy that you might find useful for this class.

You can install packages via commands such as:

```bash
python -m pip install --user numpy scipy matplotlib ipython jupyter pandas sympy nose pillow
```

We recommend using an *user* install, using the `--user` flag to pip (note: do not use `sudo pip`, which can cause problems). This installs packages for your local user, and does not write to the system directories.

### Image operations

SciPy provides some basic functions to work with images.
For example, it has functions to read images from disk into numpy arrays, to write numpy arrays to disk as images, and to resize images. Here is a simple example that showcases these functions:

`scipy.misc.imsave`(*name*, *arr*)[[source\]](http://github.com/scipy/scipy/blob/v0.13.0/scipy/misc/pilutil.py#L130)[¶](https://docs.scipy.org/doc/scipy-0.13.0/reference/generated/scipy.misc.imsave.html#scipy.misc.imsave)

Save an array as an image.

```
Parameters :	
name : str

Output filename.

arr : ndarray, MxN or MxNx3 or MxNx4 Array containing image values. If the shape is MxN, the array represents a grey-level image. Shape MxNx3 stores the red, green and blue bands along the last dimension. An alpha layer may be included, specified as the last colour band of an MxNx4 array.
```



Examples

Construct an array of gradient intensity values and save to file:

```python
>>> x = np.zeros((255, 255))
>>> x = np.zeros((255, 255), dtype=np.uint8)
>>> x[:] = np.arange(255) # all the rows of X is [0,1,2,...,254]
>>> imsave('/tmp/gradient.png', x)
```

Construct an array with three colour bands (R, G, B) and store to file:

```python
>>> rgb = np.zeros((255, 255, 3), dtype=np.uint8)
>>> rgb[..., 0] = np.arange(255)
>>> rgb[..., 1] = 55
>>> rgb[..., 2] = 1 - np.arange(255)
>>> imsave('/tmp/rgb_gradient.png', rgb)
```

```python
from scipy.misc import imread, imsave, imresize

# Read an JPEG image into a numpy array
img = imread('assets/cat.jpg')
print(img.dtype, img.shape)  # Prints "uint8 (400, 248, 3)"

# We can tint the image by scaling each of the color channels
# by a different scalar constant. The image has shape (400, 248, 3);
# we multiply it by the array [1, 0.95, 0.9] of shape (3,);
# numpy broadcasting means that this leaves the red channel unchanged,
# and multiplies the green and blue channels by 0.95 and 0.9
# respectively.
img_tinted = img * [1, 0.95, 0.9]

# Resize the tinted image to be 300 by 300 pixels.
img_tinted = imresize(img_tinted, (300, 300))

# Write the tinted image back to disk
imsave('assets/cat_tinted.jpg', img_tinted)
```

<div class='fig figcenter fighighlight'>
  <img src='imgs/python_tutorial/cat.jpg'>
  <img src='imgs/python_tutorial/cat_tinted.jpg'>
  <div class='figcaption'>
    Left: The original image.
    Right: The tinted and resized image.
  </div>
</div>

<a name='scipy-matlab'></a>

### MATLAB files

The functions `scipy.io.loadmat` and `scipy.io.savemat` allow you to **read and write MATLAB files**. You can read about them [in the documentation](http://docs.scipy.org/doc/scipy/reference/io.html).

<a name='scipy-dist'></a>

### Distance between points

SciPy defines some useful functions for computing distances between sets of points.

The function `scipy.spatial.distance.pdist` computes the distance between all pairs
of points in a given set:

```python
import numpy as np
from scipy.spatial.distance import pdist, squareform

# Create the following array where each row is a point in 2D space:
# [[0 1]
#  [1 0]
#  [2 0]]
x = np.array([[0, 1], [1, 0], [2, 0]])
print(x)

# Compute the Euclidean distance between all rows of x.
# d[i, j] is the Euclidean distance between x[i, :] and x[j, :],
# and d is the following array:
# [[ 0.          1.41421356  2.23606798]
#  [ 1.41421356  0.          1.        ]
#  [ 2.23606798  1.          0.        ]]
d = squareform(pdist(x, 'euclidean'))
print(d)
```

You can read all the details about this function
[in the documentation](http://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.pdist.html).

A similar function (`scipy.spatial.distance.cdist`) computes the distance between all pairs
across two sets of points; you can read about it
[in the documentation](http://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.cdist.html).

<a name='matplotlib'></a>

<http://codingpy.com/article/an-introduction-to-numpy/>

<http://codingpy.com/article/a-quick-intro-to-matplotlib/>

<http://codingpy.com/article/a-quick-intro-to-pandas/>


