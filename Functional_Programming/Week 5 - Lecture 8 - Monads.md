# Potentially-Failing Computations
```
data Maybe a = Just a | Nothing
```
- Whenever we want to use a value of type Maybe a, we’ll need to case split to
see whether it contains a value or not.

The **maybe** type allows us to return nothing if the computation fails

## Managing Multiple Types
```
divAndAdd :: Int -> Int -> Int -> Int -> Maybe Int
divAndAdd x y d1 d2 =
case (safeDiv x d1, safeDiv y d2) of
(Just x', Just y') -> Just (x' + y')
(_, _) -> Nothing
```
- This is horrible

### The better version of that function:
```
withJust :: Maybe a -> (a -> Maybe b) -> Maybe b
withJust Nothing _ = Nothing
withJust (Just x) f = f x
```

# Nondeterministic Computations
```
coinToss :: [CoinToss]
coinToss = [Heads, Tails]
```
- We want to build a list with possible outcomes of the two tosses. 
```
withEach :: [a] -> (a -> [b]) -> [b]
withEach xs f = concat (map f xs)
```
```
twoCoinTosses :: [(CoinToss, CoinToss)]
twoCoinTosses =
coinToss `withEach` (\x ->
coinToss `withEach` (\y ->
[(x, y)]))
```
- List comprehension is essentially syntactic sugar implemented using an an analogue of withEach

## Writing IO computations without do
```
withIOResult :: IO a -> (a -> IO b) -> IO b

getAndPrintReverse :: IO ()
getAndPrintReverse =
 getLine `withIOResult` (\str ->
 let revStr = reverse str in
 putStrLn revStr)
```

# Monads
Can you see a pattern in these functions?
```
withJust :: Maybe a -> (a -> Maybe b) -> Maybe b
withEach :: [a] -> (a -> [b]) -> [b]
withIOResult :: IO a -> (a -> IO b) -> IO b
```
In each case we have:
- A computation producing a result
- A function that builds a bigger computation from the result of the first computation
- The overall result being the bigger computation

- Maybe, List, and IO are all instances of a more general pattern known as a "Monad"

- To be a Monad, a datatype needs two things
	- A way of constructing trivial datatypes from a value
	- And a way of building a larger computation from the results of a previous one

## The Monad Typeclass
- Every monad must be an applicative functor (What this means isn't important for now)
```
class (Applicative m) => Monad m where
 return :: a -> m a
 (>>=) :: m a -> (a -> m b) -> m b
 (>>) :: m a -> m b -> m b
```

## What is a Monad?
**A monad is a data type that implements the Monad typeclass, and satisfies the Monad Laws**

### Example: The Monad Bond
- The maybe monad it determined - it will stop at nothing!
- Idea: Can sequence many computations, but if any return nothing then the whole computation returns nothing
- We are just implementing >>= the same as our WithJust Function
```
instance Monad Maybe where
return x = Just x
(Just x) >>= f = f x
Nothing >>= f = Nothing
```

## Managing Multiple Maybe Monadically
```
divAndAdd' :: Int -> Int -> Int -> Maybe Int
divAndAdd' x y d1 d2 = safeDiv x d1 >>= (\res1 ->
 safeDiv y d2 >>= (\res2 ->
 return (res1 + res2)))
```
- Here, we are using the **Bind** operator **>>=** and provide a function where we assume each safe division has succeeded
- We can then use return to create a Maybe Int from res1 + res2
- If any sub-computation fails, then the whole thing will return **nothing**


# do-notation
```
greetReverse :: IO ()
greetReverse = do
 name <- getLine
 let reverseName =
  reverse name
 putStrLn “hello”
 putStrLn reverseName
```
=>
```
greetReverse :: IO ()
greetReverse =
 getLine >>= \name ->
 let reverseName =
  reverse name in
 putStrLn “hello” >>
 putStrLn reverseName
```

## Rules of do-notation
**do-notation is syntactic sugar.**
It is widely used and very useful, but it is sometimes more efficient to write monadic expressions directly
![[Pasted image 20261022190540.png|366]]

## do-notation for other monads
the do-notation works for any data type that is a member of the monad typeclass.
We can rewrite our Maybes example from earlier using do notation:
```
divAndAdd' :: Int -> Int -> Int -> Maybe Int
divAndAdd' x y d1 d2 = do res1 <- safeDiv x d1
 res2 <- safeDiv y d2
 return (res1 + res2)
```

## Monad Typeclass Hierarchy
Nowadays monads must also be an instance of two other typeclasses called **Functor** and **Applicative**
For the purposes of this course, we will stick with the definitions given in the lectures