
**Aspect examples:**

1. domain of a function
2. side effects
3. acquiring/releasing resources
4. algorithmic complexity
5. completability
6. concurrency
7. execution order
8. data interchange tracing
9. function execution time (compile time, runtime)
10. identifiers description
11. function pass by (by value, by name)

...etc...


## Aspects detailed description:

### 1. domain of function aspect

It is all about the mathematical domain of a function. So for each function input, the aspect should provide a prediction about function output. Of course, there are a lot of operations for which domain of a function is not defined. So literally every function could return undefined result. 

There are a lot of languages which have a data type to hold an undefined result. But because mostly every operation may fail using special type for an uncertain result will lead to all functions returning an optional result. Another approach to handling such kind of errors is to use exceptions. But again as mostly every operation may fail every function gets verbose with a lot of exceptions being defined in the function signature. 

The aspect helps keep code clean but at the same time, checks function definition. 
For example, if a function contains code that could potentially make output undefined aspect should mark the output as "may be defined". If the function doesn't contain operations which could lead to undefined result then output should not be marked as defined. If function checks the result definition just before returning then result should be defined in that case. If the output is being created from call results of other functions then if any result is "may be defined" then the result of parent function is also "may be defined". 

But most important that from the point of view of a type system the type of any field is going to be T instead of Option[T] or Maybe[T]. So a code will be less verbose. 
At the same time if the aspect is not applied and there are a chain of operations and the first one returns undefined result then the further operation should not quickly fail but should return undefined result. It is like a chain of maybe monads in Haskell but without explicit monadic type definition.

### 2. side effects aspect

The aspect will gather all side effects. For example, if we have a hierarchy of method calls a -> b -> c (
a" calls "b" and "b" calls "c") and the aspect of method "c" says that it writes to a file then method "a" will also have the aspect saying that it writes to a file.

For sure if there are no side effects then the function will be marked as pure. Any not pure function down hierarchy will make parent functions also unsure. It is the same as in Haskell. 

So the main idea of the aspect is the same as behind IO Monad of Haskell Lang. But IO Monad of Haskell splits code into only two categories:
pure functions and having side effects. But unfortunately, it doesn't tell which side effects are hidden inside a piece of code. The aspect will not only gather purity down hierarchy but also will gather all side effects. 

### 3. acquiring-releasing resources aspect

Optimizing heap memory usage, opening-closing files etc should be managed with the aspect.

Again if there is a hierarchy of method calls "a" -> "b" -> "c" and method "c" opens a file and no method of the hierarchy closes the file then both "a" and "b" methods would be marked as methods which are opened the file.

In the case of memory usage, it will allow controlling memory exact time of memory allocation/deallocation. For example method "a" calls method "b" in some long cycle and method "b" allocates some heap variable. Requesting and freeing the variable on each allocation is inefficient. Marking method "a" as the one which must free resource on completion will increase efficiency. 

In general, this aspect should behave like Rust's "ownership system" but without introducing a complex type system to implement it. 

### 4. algorithmic complexity aspect

For each function, algorithmic complexity for each input argument is calculated by this aspect. In case of making a composition of functions with already calculated complexity, there is no need in calculating the complexity of parent function. So memorization could be used here too.
For example function a(x) has complexity O(n) for x argument. And there is fn b(y) = for 0 to z do a(y). Then the complexity of fn b is O(n^2). 

### 5. completability aspect

It is well known that it is not possible to automatically determine completability of a function. But for many functions, it is possible.

For example, there are 3 functions "c","d","e" which are guaranteed to be completed. Then there is parent function "a" which calls "c", "b", "e" and there is no cycle or recursion in a. It is obvious that the function "a" is also completable. Etc. 

Another case is iteration over some collection. Using functional programming mapping or folding over collection construction is completable by definition. For sure it holds true only in case of non-lazy collections.

But even in case of not too complicated cases like "while" or "recursion", there are techniques to check completability. 

So while it is not possible to discover completability of a code it is better to mark some pieces of code as 100% completable and another one as 100% incompletable instead of having an entire black box. 

### 6. concurrency aspect

The aspect allows handling either a code can access concurrently some shared variables or not. Also, it should define a way of the access. By default, it should lock functions accessing the same variables.  Also, it should analyze the possible dead-locks and/or live-locks and report. It is the case similar to completability aspect. So the same as for completability aspect it is better to have some guarantees than not at all. 

### 7. execution order aspect

It is useful to explicitly show a user in what order each instruction is going to be executed instead of the user checking the specification of a language each time. Most probably there should not be any hints but analytics to help to check priority of operators. 

### 8. data interchange tracing aspect

The aspect provides a way to check input to output mapping in case it is a composition of some primitive fields and input is also a composition of fields. 

Example: func f(a,b,c) = side_effect(c), return a+b.
This example shows that a and b influence resulting value while c doesn't. 
More advanced example: 
type A{a,b,c,d}, type B{e,f,g}
fn f(A a) -> B
fn f(A a) {
    return B{
        e = a+d
        f = d
        g = b+c
    }
}
In the case, aspect will show that fields "a" and "d" of type "A" influence "e" of "B", "d" influence "f" field and "b" and "c" influence "g" field. It is simple here but could be very useful in case of complex hierarchies of function calls and complex data structures.

### 9. function execution time aspect 

The aspect allows controlling either function executed at compile time or runtime. 

Compile time execution is the way to make macros in the language. 

### 10. identifiers description aspect

Well, it is just another representation of comments. But that way comment will be tied to code and can't be lost or connected by an accident to another piece of code. Also, it will not add noise to a code. Looks like hints are useless for the aspect.


### 11. function pass by (by value, by name)

The aspect introduces higher-order functions. The aspect introduces higher-order functions. It allows marking input parameters as well as a result of a function to be a function.
