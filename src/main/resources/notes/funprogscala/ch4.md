Section 4.1:
* Big idea - represent errors (or, more generally, side effects) as values, then introduce higher-order patterns for dealing with maybe-esque situations
* Normal thrown exceptions - two big problems:
    * Not referentially transparent -- the outcome of throwing an exception depends on the context it's embedded into (here: "will it be caught?")
    * Not type-safe -- the type of a function that handles exceptions tells you nothing about what kind of errors it throws
        ** this is why a lot of Java code simply handles the most generic RuntimeException
* Instead - harken back to C-style return codes, except these are higher-order "possibly defined" values with higher-order constructs to deal with them!

Section 4.2:
* How else can we handle this? Maybe we explicitly code for "bad" cases (partial function)? Or we provide a null value for bad cases?
* Functional rejects this for the following reasons:
    * Leads to silent failures (think sudden NaNs or nulls in your data)
    * Boiler-plate: `if (xs.length > 0) { ... do the thing ... }` -- how does this aggregate and compose?
    * No polymorphism - generics can't invent a null value for a generic type `foo`
    * More knowledge needed by invokers!
        * Options / Trys let you be more intentional with your handling via the type system and higher-order values, while releasing your obligation to understand all functions you invoke.
