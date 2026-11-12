A monad is like an envelope.
The monadic bind function essentially removes, manipulates, and repackages the object inside the envelope.

# Reader Monad
The reader  Monad allows for shared environmental configuration
```
import Control.Monad.Reader
--  ^^^ may need some GHCi hackery ...
--  i.e. :set -package mtl

hi = do
  name <- ask
  return ("hello " ++ name)

bye = do
  name <- ask
  return ("goodbye " ++ name)

conversation = do
  start <- hi
  end <- bye
  return (start ++ " ... " ++ end)

main = do
  putStrLn $ runReader conversation "jeremy"

```

# Writer Monad
The writer Monad builds a growing sequence of state.
The "tell" function tells the writer monad to append something to the log.

```
import Control.Monad.Writer

addOne :: Int -> Writer String Int
addOne x = do
  tell ("incremented " ++ (show x) ++ ",")
  return (x+1)

double :: Int -> Writer String Int
double x = do
  tell ("doubled " ++ (show x) ++ ",")
  return (x*2)

compute =
  tell ("starting with 0,") >> addOne 0 >>= double >>= double >>= addOne

main = do
  print $ runWriter (compute)

```

# State Monad
The state monad combines the previous two, by allowing us to modify the shared state.
You can "get" the current state, or "put" a new value into the state
```
import Control.Monad.State

fibState :: State (Integer, Integer, Integer) Integer
fibState = do
  (x1,x2,n) <- get
  if n == 0
  then return x1
  else do
    put (x2, x1 + x2, n - 1)
    fibState

main = do
  print $ runState fibState (0,1,100)
```