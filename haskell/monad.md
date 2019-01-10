### Monad

See Function, Functor, Applicative and Monad:

```haskell
    ($)   :: (a -> b)   ->   a ->   b
    (<$>) :: (a -> b)   -> f a -> f b
    (<*>) :: f (a -> b) -> f a -> f b
    (>>=) :: m a -> (a -> m b) - > m b
```

Three minimally complete Monad instance:

```haskell
    (>>=)  :: m a -> (a -> m b) -> m b
    (>>)   :: m a -> m b -> m b
    return :: a -> m a
```

We can force an fmap to work like a monad:

```haskell
    fmap :: Functor f
         => (a -> f b) -> f a -> f (f b)
```

Type of Concat:

```haskell
    concat :: Foldable t => t [a] -> [a]

    -- or simply
    concat :: [[a]] -> [a]

    -- join is similar to concat
    join :: Monad m => m (m a) -> m a

    -- Monad also lifts!
    -- Needed for backward compatibility

    liftA :: Applicative f
          => (a -> b) -> f a -> f b
    liftM :: Monad m
          => (a1 -> r) -> m a1 -> m r
```

Flip, fmap and join converts the monad to be just like functor and applicative:

```haskell
bind :: Monad m => (a -> m b) -> m a -> m b
bind f = join . fmap f
```

When to use monad or applicative?

If your do syntax looks like this:

```haskell
doSomething = do
  a <- f
  b <- g
  c <- h
  pure (a, b, c)
```

You can rewrite it using applicative. See, that `b` is not dependent on `a`.

However, if you have something like this:

```haskell
doSomething' n = do
  a <- f n
  b <- g a
  c <- h b
  pure (a, b, c)
```

You need to use Monad.
