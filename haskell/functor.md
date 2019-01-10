### Functor

Functor is implemented with this type class:

```haskell
    class Functor f where
      fmap :: (a -> b) -> f a -> f b

    Function signature:
    ($)   :: (a -> b) ->   a ->   b
    (<$>) :: Functor f
          => (a -> b) -> f a -> f b

    ## Functor laws
    1. Idenity:
    fmap id == id

    2. Composition:
    fmap (f . g) = fmap f . fmap g
```

Functor implementation for a data type like Maybe:

```haskell
data FixMePls a =
    FixMe
  | Pls a
  deriving (Eq, Show)

instance Functor FixMePls where
  fmap _ FixMe = FixMe
  fmap f (Pls x) = Pls (f x)
```
