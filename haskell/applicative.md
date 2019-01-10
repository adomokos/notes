### Applicatives


Applicatives are monoidal functors. The function we are applying is also embedded in some structure.

```haskell
   class Functor f => Applicative f where
    pure :: a -> f a
    (<*>) :: f (a -> b) -> f a -> f b -- named `apply` or `ap` or `tie fighter`
```

Comparing with Functor:

```haskell
   -- fmap
   (<$>) :: Functor f
         =>   (a -> b) -> f a -> f b

   (<$>) :: Applicative f
         => f (a -> b) -> f a -> f b
```

Applicative provides some helper functions:

```haskell
   liftA  :: Applicative f => (a -> b) -> f a -> f b
   liftA2 :: Applicative f => (a -> b -> c) -> f a -> f b -> f c
   liftA3 :: Applicative f => (a -> b -> c -> d) -> f a -> f b -> f c -> f d
```

Notice this:

```haskell
   ($)   ::   (a -> b) ->   a ->   b
   (<$>) ::   (a -> b) -> f a -> f b
   (<*>) :: f (a -> b) -> f a -> f b
```

Tuple monoid and Applicative side by side

```haskell
   instance (Monoid a, Monoid b) => Monoid (a,b) where
     mempty = (mempty, mempty)
     (a,b) `mappend` (a', b') =
       (a `mappend` a', b `mappend` b')

   instance (Monoid a) => Applicative ((,) a) where
     pure x = (mempty, x)
     (u, f) <*> (v, x) =
       (u `mappend` v, f x)
```

#### Maybe Monoid and Applicative

```haskell
    instance Monoid a => Monoid (Maybe a) where
      mempty = Nothing
      mappend m Nothing = m
      mappend Nothing m = m
      mappend (Just a) (Just a') =
        Just (a `mappend` a')

    instance Applicative Maybe where
      pure = Just

      Nothing <*> _ = Nothing
      _ <*> Nothing = Nothing
      Just f <*> Just x = Just (f x)
```

Applicative in Use:

~List Applicative~


```haskell
    -- f - []
    (<*>) :: f (a -> b) -> f a -> f b
    (<*>) :: [] (a -> b) -> [] a -> [] b

    -- more syntactically typical
    (<*>) :: [(a -> b)] ->  [a] ->  [b]

    pure :: a ->  f a
    pure :: a -> [] a
```
