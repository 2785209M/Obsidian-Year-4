# Previous Error Handling Examples
- Partial Functions
- Maybe type
- Runtime Errors

# Pure Approaches
## Maybe Type
```
data Maybe a = Nothing | Just a
```
- The Maybe type has many advantages
	- Signifies failure without stopping execution
	- Can be used in Pure Code
	- Can be used Monadically to avoid nested case matching

- **However**, it only tells us **that** something went wrong, not **why** it went wrong

## Either Type
```
data Either a b = Left a | Right b
```
### Type Isomorphisms
Maybe can be expressed using Either

### Either for error handling
```
data Either err res = Left err | Right res
```
- This can be used as a primitive building block for error handling

### Either in Practice: Parsec
```
parse :: Stream s Identity t =>
	Parsec s () a ->
	SourceName ->
	s ->
	Either ParseError a
```

## The Either Monad
- Maybe was a Monad, which means we use do-notation to compose multiple Maybes and write the code as though everything succeeds
- Either can be used similarly, provided we fix the error type
- Idea: if any sub-computation fails with an error, the whole computation fails with an error
```
instance Monad (Either a) where
	return x = Right x
	(Left x) >>= f = Left x
	(Right x) >>= f = f x
```

## "Pure" Exceptions in Practice
- Pure approaches in practice use Control.Monad.Except, which is a wrapper aroud Either
```
return :: a -> Except e a
throwE :: e -> Except e a
catchE :: Except e a -> (e -> Except e’ a) -> Except e’ a
runExcept :: Except e a -> Either e a
```

# Impure Approaches
## Runtime Exceptions
- This is a unique kind of error that tells the runtime to stop with an error rather than just expressing failure as a datatype

## Throwing Runtime Exceptions
- Runtime Exceptions can be created by making an algebraic datatype and then making it an exception
```
data ResponseException = NotExcitedException | BadParadigmException
instance Exception ResponseException
```
- We can then throw exceptions using the throw function
```
throw :: (HasCallStack, Exception e) => e -> a
```
- But it is considered best practice to use throw.io instead
```
throwIO :: (HasCallStack, Exception e) => e -> IO a
```

## Catching Runtime Exceptions
- Runtime exceptions in Haskell are transparent to the type system: they don't show up in any types
- Exceptions can also be caught using the IO Monad
```
catch :: Exception e =>
	IO a ->
	(e -> IO a) ->
	IO a
```

## Exceptions in practice
- Pure code generally uses Maybe or Either to indicate failure
- Impure code that interacts with the real world will generally raise an exception

# Towards monad transformers
- For using two Monads together, it can be useful to layer the monads; This pattern is known as a **Monad Transformer**
```
type ExceptIO e a = ExceptT e IO a
```