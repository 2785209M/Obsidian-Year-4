# Evaluation Strategies
```

```
## Lazy evaluation
short circuit evaluation in C - in an or evaluation, if the first function evaluates to true, the second function won't be called

## Sieve of Eratosthenes
```
primes = sieve [2..]
	where sieve (x:xs) = x:(sieve ........)
```


# Polymorphism
- In Greek, Polymorphism means "Many Forms"
- Allows for the reuse of code
## Parametric Polymorphism
- Functions operate on the shape of the parameters rather than the data
## Polymorphic datatypes
data Tree = 
	Leaf l Node Int Tree Tree

