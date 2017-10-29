
# Python Numpy Tutorial From Stanford
This tutorial was contributed by Justin [Johnson](http://cs.stanford.edu/people/jcjohns/).
Python is a great general-purpose programming language on its own, but with the help of a few popular libraries (numpy, scipy, matplotlib) it becomes a powerful environment for scientific computing.

- [Python](#python)

  - [Basic data types](#basic)
  - [Containers](#containers)
    - [Lists](#lists)
    - [Dictionaries](#dictionaries)
    - [Sets](#sets)
    - [Tuples](#tuples)
  - [Functions](#functions)
  - [Classes](#classes)
  
  - [Numpy](#numpy)
    - [Arrays](#arrays)
    - [Array indexing](#indexing)
    - [Datatypes](#datatypes)
    - [Array math](#math)
    - [Broadcasting](#broadcasting)

<font color="black" size="6"><span id="python">Python</span></font>

Python is a high-level, dynamically typed multiparadigm programming language. Python code is often said to be almost like pseudocode, since it allows you to express very powerful ideas in very few lines of code while being very readable. As an example, here is an implementation of the classic quicksort algorithm in Python:


```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr)//2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x==pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)

print(quicksort([3,6,8,10,1,2,1]))
```

    [1, 1, 2, 3, 6, 8, 10]
    

There are currently two different supported versions of Python, 2.7 and 3.6. Somewhat confusingly, Python 3.0 introduced many backwards-incompatible changes to the language, so code written for 2.7 may not work under 3.6 and vice versa. For this class all code will use Python 3.6.

You can check your Python version at the command line by running python --version.

<font color="#dd0000" size="5"><span id="basic" color="#dd0000">Basic data types</span></font>


Like most languages, Python has a number of basic types including integers, floats, booleans, and strings. These data types behave in ways that are familiar from other programming languages.

<font color="blue" size="4">Numbers</font>: Integers and floats work as you would expect from other languages:


```python
x = 3
print(type(x))
print(x)
print(x+1)
print(x**2)
x += 1
print(x)
x *= 2
print(x)
y = 2.5
print(type(y))
print(y,y+1,y*2,y**2)
```

    <class 'int'>
    3
    4
    9
    4
    8
    <class 'float'>
    2.5 3.5 5.0 6.25
    

Note that unlike many languages, Python does not have unary increment (`x++`) or decrement (`x--`) operators.

Python also has built-in types for complex numbers; you can find all of the details in the documentation.

<font color="green" size="4">Booleans</font>: Python implements all of the usual operators for Boolean logic, but uses English words rather than symbols (&&, ||, etc.):


```python
t = True
f = False
print(type(t))
print(t and f)
print(t != f)
```

    <class 'bool'>
    False
    True
    

<font color="blue" size="4">Strings</font>: Python has great support for strings:


```python
hello = 'hello'
world = 'world'
print(hello)
print(len(hello))
hw = hello + ' ' + world
print(hw)
hw12 = '%s %d %s'%(hello,12,world)
print(hw12)
```

    hello
    5
    hello world
    hello 12 world
    

String objects have a bunch of useful methods; for example:


```python
s = 'hello'
print(s.capitalize())
print(s.upper())
print(s.rjust(7)) # Right-justify a string, padding with spaces; prints "  hello"
print(s.center(7))  # Center a string, padding with spaces; prints " hello "
print(s.replace('l','(ell)'))
print('  \nworld'.strip())
```

    Hello
    HELLO
      hello
     hello 
    he(ell)(ell)o
    world
    

<font color="#dd0000" size="5"><span id="containers" color="#dd0000">Containers</span></font>

Python includes several built-in container types: lists, dictionaries, sets, and tuples.

<font color="blue" size="4"><span id="lists" color="#dd0000">Lists</span></font>

A list is the Python equivalent of an array, but is resizeable and can contain elements of different types:


```python
xs = [3,1,2]
print(xs,xs[2])
print(xs[-1])
xs[2] = 'foo'
print(xs)
xs.append('bar')
print(xs)
x = xs.pop()
print(x,xs)
```

    [3, 1, 2] 2
    2
    [3, 1, 'foo']
    [3, 1, 'foo', 'bar']
    bar [3, 1, 'foo']
    

Slicing: In addition to accessing list elements one at a time, Python provides concise syntax to access sublists; this is known as slicing:


```python
nums = list(range(5))
print(nums)
print(nums[2:4])
print(nums[2:])
print(nums[:2])
print(nums[:])
print(nums[:-1])
nums[2:4] = [8,9]
print(nums)
```

    [0, 1, 2, 3, 4]
    [2, 3]
    [2, 3, 4]
    [0, 1]
    [0, 1, 2, 3, 4]
    [0, 1, 2, 3]
    [0, 1, 8, 9, 4]
    

If you want access to the index of each element within the body of a loop, use the built-in enumerate function:


```python
animals = ['cat','dog','monkey']
for id,animal in enumerate(animals):
    print('%d: %s'%(id+1,animal))
```

    1: cat
    2: dog
    3: monkey
    

<font color="blue" size="4"><span id="dictionaries" color="#dd0000">Dictionaries</span></font>

A dictionary stores (key, value) pairs, similar to a Map in Java or an object in Javascript. You can use it like this:


```python
d = {'cat': 'cute', 'dog': 'furry'}
print('cat' in d)
print(d.get('monkey','N/A'))
del d['cat']
print(d)
```

    True
    N/A
    {'dog': 'furry'}
    

Loops: It is easy to iterate over the keys in a dictionary:


```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal in d:
    legs = d[animal]
    print('a %s has %d legs'%(animal,legs))
```

    a person has 2 legs
    a cat has 4 legs
    a spider has 8 legs
    

If you want access to keys and their corresponding values, use the items method:


```python
for animal,legs in d.items():
    print('a %s has %d legs'%(animal,legs))
```

    a person has 2 legs
    a cat has 4 legs
    a spider has 8 legs
    

Dictionary comprehensions: These are similar to list comprehensions, but allow you to easily construct dictionaries. For example:


```python
nums = [0, 1, 2, 3, 4]
dd = {x:x**2 for x in nums if x%2 == 0}
print(dd)
```

    {0: 0, 2: 4, 4: 16}
    

<font color="blue" size="4"><span id="sets" color="#dd0000">Sets</span></font>

A set is an unordered collection of distinct elements. As a simple example, consider the following:



```python
animals = {'cat', 'dog'}
print('cat' in animals)   # Check if an element is in a set; prints "True"
print('fish' in animals)  # prints "False"
animals.add('fish')       # Add an element to a set
print('fish' in animals)  # Prints "True"
print(len(animals))       # Number of elements in a set; prints "3"
animals.add('cat')        # Adding an element that is already in the set does nothing
print(len(animals))       # Prints "3"
animals.remove('cat')     # Remove an element from a set
print(len(animals)) 
```

    True
    False
    True
    3
    3
    2
    

Loops: Iterating over a set has the same syntax as iterating over a list; however since sets are unordered, you cannot make assumptions about the order in which you visit the elements of the set:

<font color="blue" size="4"><span id="tuples" color="#dd0000">Tuples</span></font>

A tuple is an (immutable) ordered list of values. A tuple is in many ways similar to a list; one of the most important differences is that tuples can be used as keys in dictionaries and as elements of sets, while lists cannot. Here is a trivial example:


```python
d = {(x,x+1):x for x in range(10)}
print(d)
t = (5,6)
print(type(t))
print(d[t])
print(d[(1,2)])
```

    {(0, 1): 0, (1, 2): 1, (2, 3): 2, (3, 4): 3, (4, 5): 4, (5, 6): 5, (6, 7): 6, (7, 8): 7, (8, 9): 8, (9, 10): 9}
    <class 'tuple'>
    5
    1
    

<font color="#dd0000" size="5"><span id="functions" color="#dd0000">Functions</span></font>

Python functions are defined using the def keyword. For example:




```python
def sign(x):
    if x > 0:
        return 'positive'
    elif x < 0:
        return 'negative'
    else:
        return 'zero'

for x in [-1, 0, 1]:
    print(sign(x))
# Prints "negative", "zero", "positive"
```

    negative
    zero
    positive
    

<font color="#dd0000" size="5"><span id="classes" color="#dd0000">Classes</span></font>

The syntax for defining classes in Python is straightforward:




```python
class Greeter(object):

    # Constructor
    def __init__(self, name):
        self.name = name  # Create an instance variable

    # Instance method
    def greet(self, loud=False):
        if loud:
            print('HELLO, %s!' % self.name.upper())
        else:
            print('Hello, %s' % self.name)

g = Greeter('Fred')  # Construct an instance of the Greeter class
g.greet()            # Call an instance method; prints "Hello, Fred"
g.greet(loud=True)   # Call an instance method; prints "HELLO, FRED!"
```

    Hello, Fred
    HELLO, FRED!
    

<font color="#dd0000" size="5"><span id="numpy" color="#dd0000">Numpy</span></font>

Numpy is the core library for scientific computing in Python. It provides a high-performance multidimensional array object, and tools for working with these arrays. If you are already familiar with MATLAB, you might find this tutorial useful to get started with Numpy.


<font color="blue" size="4"><span id="arrays" color="#dd0000">Arrays</span></font>

A numpy array is a grid of values, all of the same type, and is indexed by a tuple of nonnegative integers. The number of dimensions is the rank of the array; the shape of an array is a tuple of integers giving the size of the array along each dimension.

We can initialize numpy arrays from nested Python lists, and access elements using square brackets:


```python
import numpy as np
a = np.array([1,2,3])
print(type(a))
print(a.shape)
print(a[0],a[1])
a[0] = 5
print(a)

b = np.array([[1,2,3],[4,5,6]])
print(b.shape)
print(b[0,0])
```

    <class 'numpy.ndarray'>
    (3,)
    1 2
    [5 2 3]
    (2, 3)
    1
    

Numpy also provides many functions to create arrays:


```python
a = np.zeros((2,2))
print(a)
b = np.ones((1,2))
print(b)
c = np.full((2,2),7)
print(c)
d = np.eye(2)  # Create a 2x2 identity matrix
print(d)
e = np.random.random((2,2))
print(e)
```

    [[ 0.  0.]
     [ 0.  0.]]
    [[ 1.  1.]]
    [[7 7]
     [7 7]]
    [[ 1.  0.]
     [ 0.  1.]]
    [[ 0.91141318  0.78561508]
     [ 0.37697922  0.9396488 ]]
    

<font color="blue" size="4"><span id="indexing" color="#dd0000">Array indexing</span></font>

Numpy offers several ways to index into arrays.

Slicing: Similar to Python lists, numpy arrays can be sliced. Since arrays may be multidimensional, you must specify a slice for each dimension of the array:


```python
a = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
b = a[:2,1:3]
print(b)
print(a[0,1])
b[0,0] = 77
print(b)
print(a[0,1])
```

    [[2 3]
     [6 7]]
    2
    [[77  3]
     [ 6  7]]
    77
    

You can also mix integer indexing with slice indexing. However, doing so will yield an array of lower rank than the original array. Note that this is quite different from the way that MATLAB handles array slicing:


```python
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
row_r1 = a[1,:]
row_r2 = a[1:2,:]
print(row_r1,row_r1.shape)
print(row_r2,row_r2.shape)

col_c1 = a[:,1]
col_c2 = a[:,2]
print(col_c1, col_c1.shape)  # Prints "[ 2  6 10] (3,)"
print(col_c2, col_c2.shape) 
```

    [5 6 7 8] (4,)
    [[5 6 7 8]] (1, 4)
    [ 2  6 10] (3,)
    [ 3  7 11] (3,)
    

## Updating...


```python

```
