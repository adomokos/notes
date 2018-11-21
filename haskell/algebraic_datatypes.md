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
