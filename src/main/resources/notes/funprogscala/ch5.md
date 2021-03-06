Chapter 5:

How can we use ideas of strict and lazy evaluation to make our programs run better? Think of operations on a list, and all the immediately-discarded intermediate results - how can we do better? Answer: non-strictness can improve modularity by separating the description of an expression from the how-and-when of its evaluation.

Section 5.1:
* An unevaluated term is called a _thunk_ - you have to force evaluation of it to get a result.
* `||` and `&&` are both short-circuiting ie lazy operators
* Create a function that takes in arguments as () => A (which desugars to Function0[A]) - this won't be evaluated until explicitly referenced in the function body.
* This call-by-name scenario is common enough that the syntax is: `def fn[A](arg1: => A)`
* For repeated use of a thunk, cache it (much like Spark does) with a lazy val.
* The result will still only be evaluated when explicitly referenced, but after the first reference it will persist.
* Formal definition of strictness: an evaluated expression terminates to the bottom for all possible values!

Section 5.2
* Create a lazy / memoized version of Lists using thunks, and call it a Stream!
* Call-by-name prevails, and explicit evaluation of an expression must be done to retain value (`h()` to get head instead of just `h`)
* A pattern of memoization prevails on data structures with a lowercase name for a "smart constructor" (`cons` instead of `Cons`)
* Caching by-name expressions in lazy vals allows us to only compute once, rather than repeatedly.

Section 5.3
* Think on separation of concerns:
    * first-class functions essentially delay evaluation until some parameters are passed in
    * Option types defers immediate context switch on missingness / errors to allow higher-order computation
    * Streams allow you to manipulate collections without having to materialize the entire thing in memory right away
    * Sidenote: @transients in Scala / distributed Spark contexts allow you to essentially pass an unevaluated expression between nodes, to be evaluated locally only!
* Take the original problem of materializing intermediate lists during composed functionality - streams allow you to compose functionality over partially-evaluated lists!
    * This means if you have `Stream(1,2,3,4).map(_ + 10).filter(_ % 2 == 0).map(_ * 2)`, filter DOES NOT HAVE TO WAIT for Stream(11,12,13,14) before it starts filtering!
    * This seems honestly like a clever implementation detail - I'd have to see how the pointers and low-level allocation shuffles to be convinced this is truly great.

Section 5.4:
* Corecursion / guarded recursion - while recursive iterates on smaller / converging cases, corecursion need not end as long as it's productive! Corecursive produces!
* We can make Streams corecursive by referencing the original val / def, which will help with memoization. Just be careful with vals v. defs v. lazy vals!
* The main point of it all - separate description from evaluation!
    * Functions are a description with delayed evaluation - streams are that for collections, and Options are that for errors!
