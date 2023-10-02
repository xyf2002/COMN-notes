In programming we can generally reuse variable names:
```scala
def foo(x: Int) = x + 1
def bar(x: Int) = x * x
```
The occurrences of `x` in `foo` have nothing to do with those in `bar`.

# Definition
The *scope* of a variable name is the collection of program locations in which occurrences of the variable refer to the same thing.

N.b. "refer to the same thing" doesn't necessarily mean that the two variable occurrences evaluate to the same value at run time.

# Binding
TODO

Essentially, renaming all binding and bound occurrences in a scope consistently (avoiding name clashes) should not change the meaning of the programming

# Let-binding
TODO

## Equivalence up to consistent renaming
We consider expressions equivalent (written $e_1\equiv e_2$) if they have the same binding structure.
We can rename bound names to get equivalent expressions:
$$
let x = y + z in x == w \equiv let u = y + z in u == w
$$
TODO fix

Some renamings can change the binding structure:
$$
let x = y + z in x == w \equiv let w = y + z in w == w
$$
Intuition: renaming $x$ to $u$ is alright, as $u$ was not already used, however renaming to $w$ is not alright as $w$ is "in use".

# Free variables
The set of free variables in an expression is defined as:
TODO copy

Examples:
$$
FV(x+y) = \{x,y\}
$$
$$
FV(let x = y in x) = \{y\}
$$
$$
FV(let x = x + y in z) = \{x,y,z\}
$$
TODO fix

# Renaming
We will use the following swapping operation to rename variables:
TODO copy

# Alpha conversion
We can now define "consistent renaming"
Suppose $y\notin FV(e_2)$. Then we can rename a let expression as follows:
$$
let x=e_1 in e_2 \rightsquigarrow let y=e_1 in e_2(x\leftrightarrow y)
$$
TODO fix

This is called *alpha-conversion*

TODO get rest

TODO get examples

