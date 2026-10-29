Deep dive into monads, looking at Haskell syntax and introducing the monad laws

# Defining custom datatypes
- as a tuple:
```
  type student = (String, String, Int)
```
- as a *Product* datatype
```
data Student = Student String String Int
```

However, neither of the approaches above is ideal. It's difficult to know which string is going to be the name and which string is going to be the address.

The solution is to use (syntactic sugar) called "Records"

In Haskell, Records have **named fields**
```
data Student = MkStudent {name :: String, address :: String, level :: Int}
```

Records can be constructed as normal or by explicitly defining fields. All fields need explicit values.

# Monads
A typeclass is a structure that defines a set of intructions or operations that can be shared accross different datatypes.

A monad is a typeclass that defines a systematic way of sequencing operations. (>>=)

Variations of monads: lists, maybes, IO, Reader/Writer.

## Laws
What is a law? A principle that constrains things. Laws constrain behaviours and relationships between entities.

- identity law:
	- map id === id
- composition
	- (map f).(map g) === map (f.g)

## Monad Laws
There are three monad laws. Haskell does not automatically enforce these laws.
### First Monad Law (Left Identity)
A monad must employ a minimum of two functions: return and bind.
```
return :: Monad m => a -> m a
Bind :: Monad m => ma -> (a -> m b) -> m b
```
```
(return x) >>= f === f x
```
### Second Monad Law (Right Identity)
The output of a monad will be re-wrapped into monadic context after being processed.
```
(y >>= return) === y
```

### Third Monad Law (Associativity)
When you have a chain of monadic functions applied with >>=, it doesn't matter how they're nested.
```
(y >>= f) >>= g === y >>= (\x -> (f x >>= g))
```