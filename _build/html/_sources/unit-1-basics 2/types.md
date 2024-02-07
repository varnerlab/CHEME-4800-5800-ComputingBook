---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Julia
  language: julia
  name: julia-1.10
---

# Expressions, Variables, and Types

```{topic} Overview
We'll begin our discussion of technical computing by introducing expressions, variables, and types. Understanding expressions, variables, and types is fundamental to writing programs in any programming language.

* [Expressions](content:references:expressions) are combinations of values, variables, and operators that produce a new value when evaluated.
Expressions are a fundamental building block of most programming languages, and they are used to perform calculations, compare values, and assign values to variables.

* [Numerical and logical data types](content:references:variables-numerical-types) are the starting points for most technical computing projects.
Variables are named storage locations that can hold values and are used to store values that may change during a program's execution. Types are categories of values, and every value belongs to a specific type. Programming languages have many types, including primitive types (such as numbers and booleans) and composite types (such as arrays and user-defined objects).

* [Character and String data types](content:references:variables-character-string-types) are used to represent text on the computer. 
Traditionally, strings, e.g., `Julia rocks` were represented as arrays of characters, e.g., `['J','u','l','i','a',' ','r','o','c','k','s'].` Modern languages, such as [Julia](https://docs.julialang.org) or [Python](https://www.python.org), have sophisticated built-in `String` types constructed using the [Unicode](https://en.wikipedia.org/wiki/Unicode) character set.

* [User-defined composite types](content:references:variables-composite-types) are custom data types made up of one or more other types that users can define. 
These types can be Immutable or Mutable. Immutable types are types whose fields cannot be changed after the type is created. Mutable types are types whose fields can be modified after they are created. For example, a `Student` type might have a `sid` and a `netid` data fields, where `sid` is an integer type and `netid` is a string type. User-defined composite types are a powerful tool for organizing and manipulating data.
```

<!-- Expressions are combinations of variables and values that can be evaluated to a single value. Variables are symbols that represent values, which can be changed or assigned to different values. Types refer to the kind of value that a variable can hold, such as integers, floating-point numbers, or strings. -->

---

(content:references:expressions)=
## Expressions and Variables
Expressions are a combination of values, variables, and operators that _evaluate_ to a single value. A _variable_ is like a box that holds a value, and that value has a _type_. For example, types can be numbers such as `3`, Boolean values like `true` or `false`, or text like:

>"Your computer is the only thing in the universe that unconditionally loves you, perhaps excluding your mother. Your computer will do anything you ask it to do with no pushback and no attitude. You just need to know how to talk to it!"

However, computers and Humans don't speak the same language. Humans understand that `3` is an integer, i.e., `3`$\in\mathbb{Z}$, and `Julia rocks` is text, but the computer doesn't see these values the same way we do. Expressions are a fundamental building block of most programming languages, and they are used to perform calculations, compare values, and assign values to variables. Here are some common examples of expressions in the [Julia programming language](https://docs.julialang.org):

* `2 + 3`: This expression evaluates to the value `5`, an `integer`. 
* `x * y`: This expression evaluates to the product of the variables `x` and `y`, which are assumed to be numbers. However, `x` and `y` can be other types, e.g., text, and the `*` operator will perform a different operation, e.g., string concatenation.
* `x == y`: This expression evaluates to `true` if `x` and `y` are equal and `false` if they are not; `true` and `false` are Boolean types, i.e., `true`$\in\mathbb{B}$ and `false`$\in\mathbb{B}$ where $\mathbb{B} = \left\{\text{true}, \text{false}\right\}$.
* `x > y`: This expression evaluates to `true` if `x` is greater than `y` and `false` if it is not; `true` and `false` are Boolean types. This statement assumes `x` and `y` are numbers. However, `x` and `y` can be other types. For example, if they are text, the `>` operator will perform a different operation, e.g., lexicographic comparison.
* `x = 3`: This expression assigns the value `3` to the variable `x`.

Expressions evaluate to a value that has a type. For example, the expression `2 + 3` evaluates to the value `5`, an integer. The expression `x = 3` assigns the value `3` to an integer variable `x.` The expression `x == y` evaluates to `true` or `false,` which are Boolean values. The expression `x * y` evaluates to the product of the variables `x` and `y`, which are assumed to be numbers. However, `x` and `y` can be other types, e.g., text, and the `*` operator will perform a different operation, e.g., string concatenation.

(content:references:variables-numerical-types)=
## Numerical and Logical Data Types
For a computer, everything is a [binary number](https://en.wikipedia.org/wiki/Binary_number), i.e., numbers written to the `base 2`.  From this perspective, integers are binary numbers, text is a set of binary numbers, Boolean values are binary numbers, etc.  In many traditional programming languages, it is required to declare the variable type before using it so that the computer, i.e., the [compiler](https://en.wikipedia.org/wiki/Compiler) or [interpreter](https://en.wikipedia.org/wiki/Interpreter_(computing)) that is processing your code, can check the correctness of the program and allocate the appropriate amount of memory to store the variable. While most modern languages can guess (or infer) the type, declaring types is still good practice because it helps with the readability of the compute code. 

````{prf:remark} Why do we need Types?
Types are an essential concept in programming, and they are used to ensure the correctness and efficiency of your code. Modern languages such as [Julia](https://docs.julialang.org) and [Python](https://www.python.org) have sophisticated type systems that include basic numerical, logical, and text types, and they allow you to create your own types and define how they interact with other types. However, some languages, such as [Python](https://www.python.org), hide the type system from the user, while other languages, such as [Julia](https://docs.julialang.org), expose the type system to the user. 
````

But how are types represented on the computer? At the smallest scale, information is stored as bits and bytes in the computer ({numref}`fig-64-bit-byte-label-example`)

```{figure} ./figs/Fig-64-bit-byte-label-pattern.png
---
height: 260px
name: fig-64-bit-byte-label-example
---
A schematic of bytes and bits used in computer storage. Each box contains a digit in the numbering system. In a binary system, each box contains a `0` or `1`. 
```

A `bit` is the smallest unit of storage on a computer; a `bit` is a `0` or a `1`. However, a `bit` is too tiny for practical computing tasks. Instead, `bits` are grouped into `bytes`; a group of 8 bits equals  1 $\times$ `byte.` Different types of things, e.g., integers or text, are then represented as different numbers of `bytes.`

Integers, floating-points, and logical values are the basic building blocks of arithmetic and computation. Built-in representations of these values, i.e., the structure that the computer understands, are called `numeric primitives.` On the other hand, the models of numbers that humans understand, e.g., integers, floating-point numbers, etc., are called numeric literals, e.g., `1` is an integer literal, and `1.0` is a floating-point literal. Modern programming languages such as [Julia](https://docs.julialang.org) and [Python](https://www.python.org) provide a broad range of primitive numeric types. Further, many standard mathematical operations are defined over them, e.g., addition, subtraction, and multiplication.

Numeric primitives, which the computer understands, are binary numbers (numbers written to the `base 2`). However, there are other number systems that you may encounter, e.g., numbers written in the `base 8` (octal) or `base 16` (hexadecimal) system ({prf:ref}`defn-number-system`):

````{prf:definition} Base $\texttt{b}$ numbers
:label: defn-number-system

Let $k$ denote the $\texttt{word-size}$ of the computer, i.e., the number of bits in a \texttt{word}.
The base $b$ representation of a number uses the digit set:

$$
\begin{equation}
\mathcal{D}_{b} = \left\{0, 1, \dots, (b - 1)\right\}
\end{equation}
$$

For any $n\geq{0}$ and $b\geq{2}$, there is a string of $k$-digits $\left(a_{k-1}\,a_{k-2},\dots,a_{2}\,a_{1}a_{0}\right)_{b}$ 
where $a_{i}\in\mathcal{D}_{b}\,\forall{i}$ such that the \texttt{base-10} representation of the number $n$ is given by:

$$
\begin{equation}
n = \sum_{j=0}^{k-1}a_{j}\cdot{b^{j}}
\end{equation}
$$

where $a_{j}$ denotes the digit in position $j$, the quantity $b$ denotes the base, 
and $k$ denotes the number of bits in a $\texttt{word}$.
````

Let's look at example of a `base 8` number ({prf:ref}`example-base-8-number`):

````{prf:example} Base 8 representation
:class: dropdown
:label: example-base-8-number

Write the octal number $\left(112\right)_{8}$ in `base 10`. 

__Solution__: For a `base 8` (octal) number, $b=8$. The octal number $\left(112\right)_{8}$ can be expanded using {prf:ref}`defn-number-system`:

```{math}
:label: example-eqn-base-8-expansion
n = 2\times{8}^{0}+1\times{8}^{1}+1\times{8}^2
```

or $n = 74$.
````

### Integers
Signed integers, represented by the set $\mathbb{Z}$, are the positive and negative natural numbers along with zero:

```{math}
\mathbb{Z} = \left\{\dots, -3,-2, -1, 0, 1, 2, 3, \dots\right\}
```

The in-memory representation of signed integers, i.e., their `numeric primitive` representation, is typically a 4 $\times$ byte (32-bit) or 8 $\times$ byte (64-bit) binary number; on newer hardware and operating systems, the default value for a signed integer is an 8 $\times$ byte (64-bit) binary number ({prf:ref}`example-binary-1800`).

````{prf:example} 64-bit integer in binary format
:class: dropdown
:label: example-binary-1800

Show that the 64-bit binary representation of the integer value `1800` is given by:

```{math}
:label: eqn-bitstring-1800-64bit
1800 \stackrel{?}{=} (0000000000000000000000000000000000000000000000000000011100001000)_{2}
```

__Solution__:
Equation {eq}`eqn-bitstring-1800-64bit` is a binary number, i.e., a number written with respect to the `base 2`; thus, $b=2$ and we have the digit set $\mathcal{D}=\left\{0,1\right\}$. Further, we know that k = 64-bits; thus, the summation in {prf:ref}`defn-number-system` runs from $0\rightarrow{63}$:

```{math}
:label: eqn-binary-1800-sum
1800 \stackrel{?}{=} \sum_{j=0}^{63}a_{j}\cdot{2^j}
```

where $a_{j}$ denotes the value in the jth position of the binary number, i.e., $a_{j}=0$ or $a_{j}=1$ in position $j$. Most of the the $a_{j}$ values in Eq {eq}`eqn-bitstring-1800-64bit` are zero _except_ for a few positions; thus, the summation in {prf:ref}`defn-number-system` reduces to:

```{math}
1800 \stackrel{?}{=} 2^{3}+2^{8}+2^{9}+2^{10}
```

__Tip__: The [bitstring](https://docs.julialang.org/en/v1/base/numbers/#Base.bitstring) function in [Julia](https://docs.julialang.org/en/v1/) displays the binary representation of different types of data, e.g., numerical data types as well as strings and characters.

````

{prf:ref}`defn-number-system` shows the representation of numbers in different bases, e.g., integers written in `base 2` (binary numbers). However, the set $\mathbb{Z}$ also contains negative numbers; how can we represent negative integers in a `base 2` system? 

Negative integers are created using [Two complement](https://en.wikipedia.org/wiki/Two%27s_complement), a method to represent negative integers in binary form. It allows for efficient arithmetic operations, such as addition and subtraction, to be performed on signed numbers. [Two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) is executed by first inverting all bits, i.e., flipping `0` $\rightarrow$ `1` and vice-versa, and then adding (using binary addition) a `1` to the least significant digit (far right bit) of the result
({prf:ref}`example-twos-complement`):

````{prf:example} Two's complement
:class: dropdown
:label: example-twos-complement

Develop the 64-bit pattern for $x=-8$ using [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement). 

__Solution__: To perform [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement), we flip all the bits and add `1` to the least significant bit (the bit at index 0). This procedure requires binary addition: $1+0 = 1$, $0+1 = 1$, $0+0 = 0$ and $1+1 = 0\text{carry}{1}$. The 64-bit pattern for $x=8$ is given by:

```{math}
:label: eqn-64-bit-value-positive-8
8 = \left(0\dots00001000\right)_{2}
```

__Step 1__: Flip all the bits from `0` $\rightarrow$ `1` and vice-versa:

```{math}
\left(1\dots11110111\right)_{2}
```

__Step 2__: Add a `1` to the least-significant bit (index 0), which gives:

```{math}
-8 = \left(1\dots11111000\right)_{2}
```

````

### Boolean values
Boolean values, e.g., values of `true` and `false` are represented in modern languages using a `Bool` type. For example, both [Julia](https://docs.julialang.org) and [Python](https://www.python.org) have built-in Boolean types. However, foundational languages such as the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) do not have a dedicated Boolean type; instead, Boolean values in [C](https://en.wikipedia.org/wiki/C_(programming_language)) were represented by integers, i.e., `true = 1` and `false = 0`. Thus, it should not be surprising that in languages such as [Julia](https://docs.julialang.org), which is a distant relative of [C](https://en.wikipedia.org/wiki/C_(programming_language)), that `Bool` is implemented as a subtype of integer (a special `8-bit` integer).

In [Julia](https://docs.julialang.org) values of the `Bool` type are a kind of number: `false` is numerically equal to `0` while true is equivalent to `1`. However, unlike an `Int64`, only 1$\times$byte (8-bits) is required to store a `Bool` value in [Julia](https://docs.julialang.org); because a `Bool` can only assume one of two possible values the computer doesn't need to use extra storage:

```{code-cell} julia
# These are examples a expressions, that set a Bool value
value_true = true
value_false = false

# What is the bitstring that encodes this value?
println("False: $(bitstring(value_false)) and True: $(bitstring(value_true))")
```

Thus, while a `Bool` value will evaluate to either `1` or `0`:

```{code-cell} julia
# This expression sets a Bool value
value_true = true

# Does true evaluate to 1 (the == is a test for equality)?
value_true == 1
```

it requires less storage than an equivalent `Int` value:

```{code-cell} julia
# This expression sets a value of 1 (interesting: how does Julia know this is an Int?)
int_value = 1

# What is the bitstring that encodes this value?
bitstring(int_value)
```

### Floating point numbers

```{figure} ./figs/Fig-Float32-bit-pattern.png
---
height: 140px
name: fig-32bit-floating-point-schematic
---
Schematic of the bit-pattern for a 4$\times$byte (32-bit) floating point number
```

Floating point numbers $x\in\mathbb{R}$ are stored using 4$\times$bytes (single-precision) or 8$\times$bytes (double-precision)
following the [IEEE-754 standard](ttps://en.wikipedia.org/wiki/IEEE_754), where different components of the floating number are encoded in different segments of the 32- or 64-bits ({numref}`fig-32bit-floating-point-schematic`). For a $\texttt{64-bit}$ float $x\in\mathbb{R}$, the number is stored as:

$$
\begin{equation}
x = -1^{S}\times{M}\times{2}^{(E-1023)}
\end{equation}
$$

where $S$ denotes the sign bit, $M$ denotes the mantissa (fraction) and $E$ denotes the exponent. 
* For a $\texttt{32-bit}$ floating point number, $S$ is $b_{31}$, $M$ is encoded in bits $b_0\rightarrow{b_{22}}$ and $E$ is encoded by bits $b_{23}\rightarrow{b_{30}}$.
* For a $\texttt{64-bit}$ floating point number, the sign bit $S$ is $b_{63}$, $M$ is encoded by bits $b_0\rightarrow{b_{51}}$, and $E$ is encoded by bits $b_{52}\rightarrow{b_{62}}$.
where $b_{i}$ denotes the $i$-th bit in the word, and $M$ is defined as (for a $\texttt{64-bit}$ float):

$$
\begin{equation}
M = \left(1+\sum_{i=1}^{52}b_{52-i}2^{-i}\right)
\end{equation}
$$
Let's consider the representation of a `Float64` value for $\pi$ ({prf:ref}`example-float64-representation`):

````{prf:example} Float64 representation in Julia
:class: dropdown
:label: example-float64-representation

Compute the $S, M$ and $E$ components of the `64-bit` floating point number $x=3.14159$ in [Julia](https://docs.julialang.org).

__Solution__: The bitstring representation of $x$ is given by:

```{math}
:label: eqn-64bit-bitstring
3.14159 = \left(0100000000001001001000011111100111110000000110111000011001101110\right)_{2}
```

Note, in [Julia](https://docs.julialang.org), the default floating-point number (depending upon your hardware) is `Float64`. Thus, the bitstring in Eqn. {eq}`eqn-64bit-bitstring` is a `64-bit` floating point number. The sign bit in Eqn. {eq}`eqn-64bit-bitstring` is $S=0$, while the fraction $M = 1.570795$ and the exponent $E = 128$ which gives:

```{math}
3.1459 = 1\times{1.570795}\times{2}
```
````

(content:references:variables-character-string-types)=
## Character and String Types
Textual data on a computer is represented as the `String` data type. `Strings` in languages such as [C](https://en.wikipedia.org/wiki/C_(programming_language)) were modeled as a sequence of characters, where each character was type `Char`. Further, characters were represented via the [American Standard Code for Information Interchange (ASCII) system](https://en.wikipedia.org/wiki/ASCII), which was a set of `7-bit` teleprinter codes for the [AT&T](https://www.att.com) Teletypewriter exchange (TWX) network. For example, the character `A` in the ASCII system has index 65. Later, `8-bit` character mappings were developed, i.e., the so-called [extended ASCII systems](https://en.wikipedia.org/wiki/Extended_ASCII), which had $0,\dots,255$ possible character values. 

However, modern languages use sophisticated built-in `String` types constructed using the [Unicode](https://en.wikipedia.org/wiki/Unicode) character set. 
* The [Unicode](https://en.wikipedia.org/wiki/Unicode) standard, which encodes approximately 1.1 million possible characters, where the first 128 of these are the same as the original ASCII set. [Unicode](https://en.wikipedia.org/wiki/Unicode) characters, which use up to 4$\times$bytes (32-bits) of storage per character, are indexed using the `base 16` (hexadecimal) number systems.

Let's consider the representation of the character `J` in the ASCII and [Unicode](https://en.wikipedia.org/wiki/Unicode) systems ({prf:ref}`example-unicode-J`):

````{prf:example} Unicode and Hexadecimal 
:class: dropdown
:label: example-unicode-J

Compute the [Unicode](https://en.wikipedia.org/wiki/Unicode) index for the character `J`. 

__Solution__: The character `J` has index 74 in the ASCII table (which is included in the [Unicode](https://en.wikipedia.org/wiki/Unicode) system). Thus, first, we must convert 74 to a `base 16` (hexadecimal) number and then convert that to the [Unicode](https://en.wikipedia.org/wiki/Unicode) index format; [Unicode](https://en.wikipedia.org/wiki/Unicode) indexes left-pad the hexadecimal character value with zeros until a 4-digit code is generated, and then `U+` is appended to the four-digit code. 

Hexadecimal numbers use decimal digits and six extra symbols; the decimal values $(0,1,\dots,9)$, and the letters A, B, C, D, E, and F where hexadecimal A = decimal 10, thru hexadecimal F = decimal 15 are used in the hexadecimal numbering system.

__Approach__:
* Step 1: Divide the given decimal number by 16 and write down the quotient and remainder
* Step 2: Divide the previous quotient by 16 and write the down the quotient and remainder
* Step 3: Repeat steps 1 and 2 until the quotient equals zero.
* Step 4: Map all the remainder values to their corresponding hexadecimal equivalents 
* Step 5: Starting with the last value and moving to the first, write each of the hexadecimal values

Let's compute the hexadecimal equivalent of 74:

| value | quotient | remainder | hex code |
| ----- | -------- | --------- | ------- |
74/16 | 4 | 10 | A |
4/16 | 0 | 4 | 4 |

Thus, the hexadecimal equivalent of 74 is 4A, and the [Unicode](https://en.wikipedia.org/wiki/Unicode) index for `J` is `U+004A`.

````

Modern languages, such as [Julia](https://docs.julialang.org) or [Python](https://www.python.org), have sophisticated built-in `String` types constructed using the [Unicode](https://en.wikipedia.org/wiki/Unicode) character set.  `Strings` can be created using double quotes in [Julia](https://docs.julialang.org) or single quotes in [Python](https://www.python.org):


`````{tab-set}
````{tab-item} julia
```julia
# This is an expression to create a string in Julia
string = "Julia strings use double quotes"
```
````

````{tab-item} python
```python
# This is an expression to create a string in Python
string = 'Python strings use single quotes. Why python, why?'
```
````
`````

However, while the types of characters that can be incorporated into a [Julia](https://docs.julialang.org) `String` is more diverse, in many ways, modern strings share features with the older array representation of text. For example, a [Julia](https://docs.julialang.org) (or [Python](https://www.python.org)) `String` can be indexed like an array:

```{code-cell} julia
# This is a Julia expression to create a string
string = "Julia strings use double quotes"

# grab a range of characters (from 1 to 5)
println(string[1:5])
```

The fragment generated by indexing, e.g., the sequence of characters in the range $1\rightarrow{5}$ shown above, is also of type `String`:

```{code-cell} julia
# This is an expression to create a string in Julia
string = "Julia strings use double quotes"

# grab a range of characters
fragment = string[1:5]

# what type is the stuff that I just grabbed?
println("The fragment is type -> $(typeof(fragment))")
```

If you want (or need) to work with the invidual characters in text, you can convert a `String` type into a `Array{Char,1}` type using the `collect` function in [Julia](https://docs.julialang.org):

```{code-cell} julia
# This is an expression to create a string in Julia
string = "Julia rocks the house"

# Make an array of characters 
array = collect(string)
```

By default each character in a [Julia](https://docs.julialang.org) string requires 4$\times$bytes (32-bits) of storage. 

(content:references:variables-composite-types)=
## Composite types
Composite types are custom data types made up of one or more other types. Let's start with two simple composite types: [arrays](https://docs.julialang.org/en/v1/base/arrays/#lib-arrays) and [structs](https://docs.julialang.org/en/v1/manual/types/#Composite-Types).  Later, we'll consider some additional composite types, e.g., [tuples](https://docs.julialang.org/en/v1/manual/types/#Tuple-Types-1) and [dictionaries](https://docs.julialang.org/en/v1/manual/collections/#Dictionaries-1) which have special properties that make them useful for specific tasks. An `array` is a composite data type that allows theoretically any type of data to be stored in a sequence of elements. Arrays can be `read-only`, in the sense that they have a fixed size and cannot be modified after they are created, or `read-write`, in the sense that they can be modified after they are created. On the other hand, A `struct` is a composite data type that allows data to be stored in named fields. `Structs` can be `read-only`, i.e., Immutable, or `read-write`, i.e., Mutable. 

You define a struct by using the `struct` keyword followed by a name for the struct and a list of field names and types. For example, let's define an immutable `Student` struct that has a `sid` and a `netid` field:

```julia
struct Student
    
    # data fields 
    sid::Int64
    netid::String
end

# build an instance -
student = Student(1,"xyz123"); # we pass the required data into the struct as args 
```

A mutable struct is a struct whose fields can be modified after it is created. You can create a mutable struct by using the `mutable struct` keyword instead of struct and by adding a _constructor_ method:

```julia
mutable struct Student
    
    # data fields 
    sid::Int64
    netid::String

    # constructor: builds a new empty Student
    Student() = new()
end

# build an empty instance -
student = Student(); # contains no data
student.sid = 1
student.netid = "xyz123" # we add data using the "dot" notation
```

The struct composite data type contains only data; in the examples above the `Student` datatype holds two values, `sid` is an integer type and `netid` is a string type. Except for the special case of the constructur on the mutable `Student` struct, composite types in [Julia](https://docs.julialang.org) do not have functions attached to them. 

In other mainstream programming languages, e.g., [Python](https://www.python.org), [Java](https://www.oracle.com/java/), [C++](https://en.wikipedia.org/wiki/C%2B%2B), [Ruby](https://www.ruby-lang.org/en/), etc composite types also have named functions associated with them, and the combination is called an `object`. In purer object-oriented languages, such as [Ruby](https://www.ruby-lang.org/en/) or [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk), all values are objects whether they are composites or not. In less refined object-oriented languages, including [C++](https://en.wikipedia.org/wiki/C%2B%2B) and [Java](https://www.oracle.com/java/), some values, such as integers and floating-point values, are not objects, while instances of user-defined composite types are true objects with associated methods. In [Julia](https://docs.julialang.org), all values are objects, but functions are not bundled with the objects they operate on. 

---

# Summary
In this lecture, we introduced expressions, variables, and types. Understanding expressions, variables, and types are fundamental to writing programs in any programming language.

* Expressions are combinations of values, variables, and operators that produce a new value when evaluated. 
* Variables are named storage locations that can hold values and are used to store values that may change during the execution of a program. 
* Types are categories of values, and every value belongs to a specific type. Programming languages have many different types, including primitive types (such as numbers and booleans) and composite types (such as arrays and objects). 

Next, we build upon our introduction of expressions, variables, and types and consider some other technical computing build blocks and strategies, namely [Functions, Control Statements, and Recursion](./functions.md).