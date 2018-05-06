[Functional Programming & Haskell - Computerphile](https://www.youtube.com/watch?v=LnX3B9oaKzw)

Functional Programming: where functions don't have side-effects, only produces output from input, doesn't modify either.

Functional programming is used for server stuff nowadays.
Haskell is used for all the spam filtering on facebook.
Whatsapp, Twitter also use functional programming languages.
Scala is inspired by Haskell but integrates well with Java.

A functional program is very much like a mathematical equation, it satisfies laws. You can replace one program with another (for efficiency) and you know it won't change the overall behaviour of your program.  (freely rewrite, won't introduce bugs)

Functional programming languages take care of a lot of details you usually have to manage manually (memory management), there is some performance penalty for this. But parallelization is easier with functional programming language as there are no side-effects to it's function.

Hack-proof code. Privacy constraints (library in Haskell)

Quick check - instead of writing tests by hand, generate tests.
Tell Haskell what your code should do, it generates tests.
