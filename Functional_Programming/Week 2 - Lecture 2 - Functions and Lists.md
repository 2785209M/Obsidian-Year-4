## Tuples
- a **Tuple** is an ordered sequence of expressions with a known length
```
(1, 2, “hello”) :: (Int, Int, String)
(1.0, 1) :: (Float, Int)
() :: ()
```
- they are very useful for returning values from functions

### Deconstructing Tuples
- you can deconstruct a tuple by using the fst and snd functions
```
fst (1, "hello") -> 1 snd(1, "hello") -> "hello"
```
- We can also deconstruct by **pattern matching**
```
let (x, y) = (1, "hello") in x -> 1
```

# Functions and Equations
## Functions
```
\name -> "Hello, " ++ name
 \n -> n+5
```
- A function maps a value to another value
- Function definitions have parameters which act as placeholders for arguments when the function is applied
- In Haskell, a function is **first class** when: we can let-bind it, return it from another function, and pass it as an argument
- Functions without a name (such as the one above) are called anonymous
- The backslash should be read as **"lambda"**

## Functions with multiple parameters
- A function maps a single argument to a result. so what about Multiple Arguments?
- We could use a tuple...
```
\(n1, n2) -> n1 + n2
```
- A little annoying. We have to deconstruct the tuple whenever we call a function. Can we do better?
## Currying
- In practice, functions with multiple arguments are defined by **nesting** multiple functions
```
\n1 -> (\n2 -> n1 + n2)
=>
Int -> (Int -> Int)
```

## Equations
- An **equation** gives meaning to a name
```
favouriteNumber = 5
add = \x -> \y -> x + y
```

- Equations can be used to **define functions more concisely**
```
add x y = x + y
min x y = if x < y then x else y
```

- the left-hand side cannot contain any computations
```
(x + 1) = 5 ERROR
```

## Function Equations
- A function takes some arguments, performs some computation, and returns a result
- Haskell has many useful functions defined in the **prelude**
```
min :: Int -> Int -> Int
min x y = if x < y then x else y
```

## Function Application
- Expressions can contain function calls
- In Haskell you do not need to parenthesize arguments to functions
- Function application binds tightest:
```
f x * 2 means (f x) * 2
```

## Partial Application
- an advantage of currying is **partial application**
```
add x y = x + y
addFive = add 5
```
- This allows us to specialize a function without needing to rewrite or duplicate the logic

## Function Composition
- we can compose two functions using the function composition operator
```
(.) :: (b -> c) -> (a -> b) -> (a -> c)
f (.) g = \x -> f(g(x))
```
- Function Composition is used to write code in **point-free** style, which tries to avoid introducing variable names where possible
```
show . add10
vs
\x -> show (add10 x)
```

## Dollar Notation
```
negate addOne 5 DOESNT WORK
negate (addOne 5) does work
negate (addOne 5) == negate $ addOne 5
```
- The dollar sign essentially "puts parenthesis around the rest of the expression"

## Aside: Parentheses
```
1 + 2 * 3
=
1 + (2 * 3)
=
(1 + (2 * 3))
```
- parentheses are needed around things like negative numbers to disambiguate from subtraction

## Equations are not assignments
- Don't reassign variables in Haskell. It will get confuse if you say x = x + 1 (x can't be 2 and 3 at the same time), so create a new variable instead

## Parametric Polymorphism
```
\x -> \y -> x :: Int -> Bool -> Int
\x -> \y -> x :: String -> Int -> String
\x -> \y -> x :: Float -> Bool -> Float
```
- You can define a function that can be used with many different types, provided the shape of the data is consistent

## Type Variables
- Polymorphic functions have **type variables** to stand for types
```
\x -> \y -> x :: a -> b -> a
\x -> x :: a -> a
```
- We also have type variables in types (e.g., tuples and lists)
```
reverse :: [a] -> [a]
\(x, y) -> (y, x) :: (a, b) -> (b, a)
```

# Lists and Sequences

## Lists
- A list is an ordered sequence of values of the same type
```
[1,2,3] :: [Int]
[“Hello”, “FP”, “Students”] :: [String]
```
- Haskell supports a concise notation for creating ordered lists
- [1..10]
- ['a'..'z']
- [1..]
- Note that these are constructed **lazily**
	- You can construct and infinite list and only use what you need

## List comprehensions
- List comprehension allows you to use set builder notation
```
doubleEvens = [2 * x | x <- [1..], x 'mod' 2 == 0]
```

## Accessing Lists
```
head :: [a] -> a
tail :: [a] -> [a]
(!!) :: [a] -> Int -> a
```
- NOTE: only use these is you know what you are doing.
	- List indexing, head, and tail are all potentially undefined
	- It is often better to use **pattern matching**

## Zipping Lists
![[Pasted image 20261015192213.png|581]]

![[Pasted image 20261015192237.png]]

## Zipping with a custom function
![[Pasted image 20261015192810.png|640]]

### Aside: Dependent Types (Non-examinable)
- This is an occasion where our type signature is not specific enough to let us know the function’s behavior.
- Dependent type systems allow values within types, so we can be much more specific.
```
zipDep :: Vect n a -> Vect n b -> Vect n (a, b)
Vect n a: List of length n containing values of type a
```
- Haskell has some support for dependent types (but we won’t use it in this course)

## When to use a tuple, and when to use a list?
- Use a **Tuple** when:
	- you know the number of values you are storing
	- The types of values you are storing are different
- Use a **list** when;
	- You don't know the number of elements in advance
	- You have an ordered sequence of values of the same type
	- You want to operate uniformly over your data

## Which is the better type signature?
```
pythagTriples :: Int -> [(Int, Int, Int)]
```
or
```
pythagTriples :: Int -> [[Int]]
```


# Haskell building blocks
- Consider the quadratic equation:
```
Python:
def roots(a,b,c):
det2 = b * b – 4 * a * c
det = sqrt(det2)
rootp = (-b + det)/ a / 2
rootm = (-b - det)/ a / 2
return [rootm,rootp]
```

- Haskell, using **let bindings**
```
roots a b c =
 let
  det2 = b * b – 4 * a * c
  det = sqrt det2
  rootp = (-b + det)/ a / 2
  rootm = (-b - det)/ a / 2
in
 [rootm, rootp]
```
- Similarly...
```
roots a b c =
 let det2 = b * b – 4 * a * c in
 let det = sqrt det2 in
 let rootp = (-b + det)/ a / 2 in
 let rootm = (-b - det)/ a / 2 in
 [rootm, rootp]
```
- Note: each let needs a continuation, because it is not a statement!

- In Haskell, using **where bindings**
```
roots a b c = [rootm, rootp]
 where
  det2 = b * b – 4 * a * c
  det = sqrt det2
  rootp = (-b + det)/ a / 2
  rootm = (-b - det)/ a / 2
```
- where bindings are equivalent to let bindings, but they go after the function body

## Case Expressions
- In C and Java, we have switch/case statements:
```
switch (num) {
 case 1: return “one”
 case 2: return “two”
 case 3: return “three”
 default:
  return “something else”
}
```

## Guards
- Sometimes we want to do different things based on our input value
- we could do this via if-else chains
```
gradeFromGPA :: Int -> String
gradeFromGPA gpa =
 if gpa >= 18
 then “A” else
 if gpa >= 15
 then “B” else
 if gpa >= 12
 then “C” else
 “below C”
```
- In practice, Haskell's **Guard** notation is often cleaner: each is "guarded" by a boolean predicate
- 'Otherwise' is just a synonym for true
```
gradeFromGPA :: Int -> String
gradeFromGPA gpa
| gpa >= 18 = “A”
| gpa >= 15 = “B”
| gpa >= 12 = “C”
| otherwise = “below C”
```