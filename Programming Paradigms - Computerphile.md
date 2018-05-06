# [Programming Paradigms - Computerphile](https://www.youtube.com/watch?v=sqV3pL5x8PI)

Contrasting imperative programming and functional programming.

Example: Adding the numbers 1 to 10

## Imperative programming - Java
*assign value to variables*
```java
int total = 0;
for(int i = 0; i < 10; i++){
  total = total + i; //total is assigned the value of (total + i)
}
```
total = 55

**revolves around updating a global variable**

## Functional programming - Haskell
*assign arguments to functionc*
```haskell
sum[1...10]
```
There is a library function in the background.
Sum is the function that takes a list of integers and returns a single integer.
Sum defined in terms of pattern matching:
A function can have multiple definition associated with the value that is given to it, pattern-matching is deciding which value of the function to apply in the given case depending on the structure of the data given to it.

for example:
```
not::Bool -> Bool
not True = False
not False = True
```

sum::\[int\] -> int
sum\[n\] = n
sum(n:ns) = n + sum(ns)

so:
sum\[4, 5, 2\]
= 4 + sum\[5,2\]
= 4 + (5 + sum\[2\])
= 4 + (5 + 2)
= 4 + 7 = 11

**There is no state outside of the function body, they don't modify outside of the function as functions don't have side effects.**
