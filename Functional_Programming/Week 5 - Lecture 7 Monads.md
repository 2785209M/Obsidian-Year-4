# IO Tpes
Each side-effecting operation is marked with a type constructor, IO
```
putStrLn :: String -> IO()
```

You can't mix pure functions and IO functions.
A pure function is data, but an IO function is a computation that produces data.

In Haskel, side-effecting computations are always marked in their types

# Mixing Pure functions and IO computations
```
getAndPrintReverse :: IO()
getAndPrintReverse = do
 str <- getLine
 let revStr = reverse str
 putStrlLn revStr
```

```
getAndPrintReverse :: IO String
getAndPrintReverse = do
 str <- getLine
 let revStr = reverse str
 return revStr
```

# The Bigger Picture
Every Haskell function starts at the entry point main
```
main :: IO ()
main = do
line <- getLine
putStrLn (makeUpper line)
```

# Trace Debugging
```
import Debug.Trace
trace :: String -> a -> a 
```

Example: tracing factorial function
```
fact :: Int -> Int
fact 0 = trace (show 1) 1
fact n = trace ((show n) ++ " * ") (n * fact (n-1))
```

# Reference Calls
```
newIORef :: a -> IO (IORef a) - creates new IORef
readIORef :: IORef a -> IO a - Reads an IORef
writeIORef :: IORef a -> a -> IO () - Writes to an IORef
```