Benefits of functional programming: efficient and concise

**Haskell** is a general-purpose purely functional programming language with lazy evaluation

**Statement**: a statement is an instruction/computational step that does not return a value

**Expression**: an expression is an instruction/computational step that will eventually reduce to a value. Expressions can be contained within a statement.

in **functional programming** EVERYTHING is an EXPRESSION

## conditional statements as expressions
```
if 2 < 3 then
 "two is less than three"
else
 "three is less than two"
```

## Statements as expressions
```
let msg =
 if 2 < 3
  then 
   "two is less than three"
 else
  "three is less than two"
in
 "result: " ++ msg
```
(++ is the string concatenation operator)

## Types
- Every expression has a type, which classifies values
```
5 :: Int
True :: Bool
False :: Bool
(1, “Hello”) :: (Int, String)
```
- Types can rule out nonsensical expressions
- Haskell is statically typed; this means type errors are caught before the program is run
- Haskell can **infer** types from expressions, but it is good practice to add a type signature for top-level functions

## Reduction

### Evaluation in Imperative vs functional languages
```
# imperative
def addOne(x):
 return x + 1

xs = []
for x in [1, 2, 3]:
 xs.append(addOne(x))
return xs
```

```
-- functional
addOne :: Int -> Int
addOne x = x + 1

map addOne [1,2,3]
```

### Reduction
- Functional reduction: reducing complex expressions to a value
- Church-Rosser property: reductions can be performed in any order
- **Key point**: Reduction takes an expression and eventually produces a value
- **Church-Rosser**: evaluation can be in **any order**
	- in the context of term rewriting systems reduction is confluent
	- useful for equational reasoning and functional parallelism
- Treated in much more depth in the theory and computation of programming languages

