## Algebraic Datatypes

There are two kinds of constructors in Haskell:
* type constructor (used at the type level)
* data constructor

```haskell
data Trivial = Trivial'

data UnaryTypeCon a = UnaryValueCon a
```

`Trivial` is a constant at the type level. It takes no argument, hence, it's `nullary`.
`Trivial'` is a data constructor, it's also constant, but exists in value.
`UnaryTypeCon` is a type constructor with one argument.
`UnaryValueCon` is a data constructor of one argument.

### Type Constructors and Kinds

Kinds are types of types, types one level up. Represented in Haskell with `*`. We know something is a fully applied, concrete type when it is represented as `*`. When it is `* -> *`, like a function, it is still waiting to be applied.

```shell
λ> :k Bool
Bool :: *
λ> :k [Int]
[Int] :: *
λ> :k []
[] :: * -> *
```

### Sum types

```haskell
data Bool = False | True
```
The "|" represents logical disjunction - that is, "or". This is the "sum" in algebraic data types.

### Product types

A product type's cardinality is the product of the cardinaltities of its inhabitants. Arithmetically, products are the result of __multiplication__. Where a sum type was expressing or, a product type expresses __and__.

For those that have programmed in C-like languages before, a product is like a struct. For those that have not, a product is a way to carry multiple values around in a single data constructor.

### newtype

One key contrast between a __newtype__ and a type alias is taht you can define typeclass instances for newtypes that differ from the instances for their underlying type.

