**RELEASE DATE**: 21-March-2012

**AUTHOR**: Kevin Van Workum, PhD

**EMAIL**: vanw at sabalcore dot com

## Abstract ##

Tensor is a generic C++ template class designed to be highly
efficient, use Einstein's index notation for performing tensor
mathematics, and be independent of the underlying storage container
class.

## Introduction ##

Although there are a few other tensor libraries available, this class
is unique in that it is generic with respect to the underlying data
storage container. Additionally, this class correctly handles
expressions with which other tensor libraries fail. Namely, the
"LTensor" library fails with expressions where the data on left hand
side of operator= also appears within the expression on the right hand
side. For example:
```
x(i) = A(i,j)*x(j) + x(i);
```
The well-known Boost Library also has some tensor functionality, but
does not strictly follow index notation. Both, LTensor and Boost, also
do not allow complete freedom with respect to the data storage class.

The goal of the project is provide a highly efficient tensor class
which uses Einstein's index notation and allows for complete freedom
with respect to data storage.

## Index Notation ##

Index notation is an simple and intuitive way to write (and now
program) tensor, vector, or matrix mathematics. In index notation, a
summation is assumed over indices which appear on both sides of an
`*` operator. For example:
```
y(i) = A(i,j)*x(j) + b(i)
```
is equivalent to in 3 dimension:
```
y(0) = Sum_over_j( A(0,j) * x(j) ) + b(0)
y(1) = Sum_over_j( A(1,j) * x(j) ) + b(1)
y(2) = Sum_over_j( A(2,j) * x(j) ) + b(2)
```
Non-repeated indices on the left or right of a `*` operator simply
indicate a tensor component. See:

http://en.wikipedia.org/wiki/Einstein_notation

for a more detailed explanation.

## Efficiency ##

The Tensor class uses Expression Templating to achieve highly
efficient instructions. This completely eliminates the creation, copy,
and destruction of temporaries. For example, in the following statement:
```
A(i,j) = B(i,j) + C(j,i) + D(i,j); // Example 1
```
The compiler will not generate any temporary instantiations as it
might in traditional OOP which would could create up to 4 temporaries
(one for the transpose, one for each +, and =).

It also reduces the evaluation of an expression to a single (possibly
nested) for-loop. Using Example 1, the compiler will generate
instructions in exactly the same way as:
```
for(int i=0; i<DIM; i++)
  for(int j=0; j<DIM; j++)
    A(i,j) = B(i,j) + C(j,i) + D(i,j);
```
With traditional OOP, one would have to write a specialized function
to perform the equivalent efficiency.

## Generic Storage ##

The Tensor class is a template whose only template parameter is the
data storage type. So you could use Boost's multi\_array or create your
own based on std::vector for example. A very simple MultiArray
template class is provided as an example with the source code. The
only requirements the storage type has are: 1) operator() be defined
for accessing elements in the array, and 2) the rank of the tensor be
fixed. Boost::multi\_array meet these requirements.

## Usage ##

See the demo.C program for example usage. Also, in the "tests"
directory, test5.C provides some realistic use-cases. Tests 1-4 simply
exercise some of the functionality of the class.

Just edit the Makefile.inc and the run "make" to build the demo and
tests.

### Cavet ###

In order to keep the class as efficient as possible, and the code
reasonably simple, there is no special converting of the possible
permuations of indices. Each permuation is explicitly coded. This
requires a rather large amount of code. Therefore, 3 TCL scripts are
provided in the "tools" directory which are used to generate the code
for each pair of possible permuations based on a template of code. I
guess you could call it template-template code.

### Author ###

Kevin Van Workum, PhD is a scientist, engineer, and High Performance Computing specialist at [Sabalcore Computing Inc.](http://www.sabalcore.com) You may contact the author via email: vanw at sabalcore dot com. If you find this code useful let us know and please cite the author in any derived work or publications:

K. Van Workum. Generic Tensor Class. _http://code.google.com/p/generic-tensor/_, 2012