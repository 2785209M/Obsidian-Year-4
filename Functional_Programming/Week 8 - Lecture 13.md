# Semigroups
Examples of Semigroups:
- Set of positive integers with addition
- Set of all finite strings over a fixed alphabet with string concatenation
- 2x2 square non-negative matrices with matrix multiplication

# Monoids
a *monoid* is a set S with associative binary operation ⨁ and identity element e.
A monoid is essentially a semigroup with an identity element. The identity element is unique within the monoid.

The Haskell Typeclass definition is as follows:
```
class Semigroup m => Monoid m where
	mempty :: m
	-- defining mappend is unnecessary, it copies from Semigroup
	mappend :: m -> m -> m
	mappend = (<>)
	-- defining mconcat is optional, since it has the following default:
	mconcat :: [m] -> m
	mconcat = foldr mappend mempty
```

All instances of Monoid must satisfy the following laws:
```
-- left and right identity
x <> mempty == x
mempty <> x == x

-- Associativity
(x <> y) <> z == x <> (y <> z)
```
