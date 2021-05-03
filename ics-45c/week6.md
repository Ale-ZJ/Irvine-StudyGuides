# week6

## Well Behaved classes

Objects of a well-behaved class have these properties:

1. Statically-allocated portion is small and fits on the run-time stack 
2. Clean up after themselves when they die 
3. Can be passed by value and preserve the original stuff 
4. Can be assigned to objects of the same type, with respective copying and clean up.
5. Can be **const** while preserving the ability to perform any const operation
6. Are efficient: no unnecessary work.

