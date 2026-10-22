# Pattern Matching
## Pattern Matching in Functions
- The goal is to inspect an argument and do two things:
	- Inspect how the argument is constructed
	- Bind any arguments we need to use in the body of the function

- Most basic case: We don't care what numbers x anf y are but we need to give them names so we can compute them on the RHS of the equation
```
max2 :: Int -> Int -> Int
max2 x y =
if x > y then x else y
```

- Matching specific values: this time, we have specific cases for 1, 2, and 3. The final case doesn't place any constraints on the number, but binds it to x so that it can be reused on the RHS
```
numToString :: Int -> String
numToString 1 = “one”
numToString 2 = “two”
numToString 3 = “three”
numToString x = show x
```

- Inspecting structure: in this case we are deconstructing a tuple by writing a pattern that matches the tuple's shape on the left hand side
```
addTuple ::
(Int, (Int, Int)) -> Int
addTuple (x, (y, z)) = x + y + z
```

## Case Expression

- We can use a case expression to do pattern matching inside functions
```
case num of
 1 -> "one"
 2 -> "two"
 3 -> "three"
 _ -> "something else"
```
```
case list of
 [] -> "empty"
 [x] -> "one element"
 _ -> "more elements"
```


# Lists
