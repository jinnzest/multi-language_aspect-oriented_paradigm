# Aspectation Manifesto


**Main principles:**

1. segregation of source code and aspects
2. aspect = hints + source code -> binary code + analytics
3. gluing aspect parts and generated code using a hash
4. make it work, make it right, make it fast
5. implicit one in a code but explicit one in a hint
6. perfect is the enemy of the good
7. released software performance is significant

**Aspects:**

1. domain of a function
2. side effects
3. acquiring/releasing resources
4. algorithmic complexity
5. completability
6. concurrency
7. order execution
8. data interchange tracing
9. function execution time (compile time, runtime eager, runtime lazy, inlined or outlined)
10. identifiers description
...etc...


## Main principles detailed description:


### 1. segregation of source code and aspects

There are design tradeoff in modern languages between easiness of learning and working with programming language vs durability and performance potentialities. 

So we get a lot of languages as a result which mostly everybody can learn but which provoke to write buggy and slow code. On another side, there are some marginal languages which allow writing high-quality code but the amount of production-ready code is written using them is too little. Moreover, amount of developers who understand those complex languages is too low to force enterprises using any of those languages. And for sure there are a lot of languages between both corner cases.

It leads in turn to a huge amount of incorrect unoptimized and full of vulnerabilities code is written by humanity. But is there are a way to fix it? Can language be easy to learn and use but be powerful and durable at the same time?

The principle suggests a way to fix the problem. It is ancient and simple: divide and rule. We developers use the principle all the time while writing software. We do it in order to fight complexity. But this time the principle has been applied to language design itself.

In simple words: defined by category theory arrows over data should be separated from language constructions which are related to performance optimization, correctness or other aspects related to connecting components to each other, allocating them in memory or connecting the components to the outer world. 

It gives us good flexibility because now we can implement business logic postponing hardware specific optimizations for a later time. Or we can quickly write code and only then run Big O complexity analyze to discover either the algorithm is applicable in practice. Or run resources acquiring analyze for a generic algorithm like sorting and discover that it creates some files (Obviously, it is not something a developer can expect from such algorithm). Etc.

But most important that aspects may be chosen depending on needs. For example in case of quick prototyping correctness and performance optimization related analyzes are not needed (Or too early in case of making a production-ready product out of prototype). Or when there is no high demand for performance there is no need in performance optimization analyze. Etc. 

### 2. aspect = hints + source code -> binary code + analytics

Separating of a code and an aspect means that controls over different aspects should be moved to another place. Literally to different files. Let us call them hints set (it means hints for a compiler). 

So because in point 1 of the manifesto it is defined that code should not contain anything but an algorithm itself a hint is the way to control different aspects of generated binary. 
As result of applying hints of different aspects to a code analytic file for each aspect is generated. Changing a hint developer can then check the correctness of applying it in an analytic file related to the hint.  

Of course, the way of decreasing developing complexity assumes that there should be some IDE support allowing to easily see in one place a code and connected to it hints and analytics. So a developer will be able to switch between each aspect to analyze it separately from other aspects. 

### 3. gluing aspect parts using a hash 

Running each time all analytics for the whole amount of code in the case when only a few lines are changed is not practical in general case. Because running some analytics may be quite CPU consuming. But on another side, as analytics are results of applying hints to code then any changing of code will change analytics. So does it mean that static analyzing of a code is not practical?

The principle suggests a way to run analytics efficiently: to be able to modify a code without running full analyzing down hierarchy the code should be split to smallest possible pieces and then each piece should be signed by a hash. It will allow using memoization technique in a compiler to increase the speed of compiling an code.   

When main code depends on subroutines code then a source of hash should be not only the main code but also hash codes of subroutines code. So when down hierarchy any piece of code is changed hash code of related subroutine code will be changed so analytics should be run for the subroutine hierarchy and main code. 

As an analytic file and a binary file are generated from a code by applying a hint then all four pieces should be tied together by a hash code. 

### 4. make it work, make it right, make it fast

Language design should be defined the way that supports most effective developing flow described in the principal title.

#### 4.1 make it work

It means that language should allow quickly make experiments and prototyping. Without applying any aspects code itself it is going be not more durable than any prototyping weakly typed language. But it is important to emphasize that even on that stage size of data like integral is considered as endless. It is so because limited size of the integer type is just hardware optimization of speed thus should not be applied without dealing with a corresponding aspect. So even though "make it right" is not being applied on that stage it doesn't mean that code may lead to subtle errors. It means only that full correctness analyzing should not be run on the stage.

#### 4.2 make it right

It means that all possible aspects regarding correctness must be run.  Those aspects should allow adding different restrictions like data types, domains of function, acquiring/releasing resources etc. Also, it means that contracts between components should be defined using those aspects.

#### 4.3 make it fast

There should be a possibility to apply a different computer specific optimizations like using limited size integers instead of unlimited ones, concurrency related optimizations etc. Also, there is should be aspects allowing to make complexity analyze( looks like hints can't be applied to change complexity as it requires to change an algorithm, therefore, change a code). But optimization related aspected should not be able to break correctness related aspects.

Even though statement assumes the particular order of its application those principles may be applied in any other order or all at once depending on particular requirements for a software being developed. 

### 5. implicit one in a code but explicit one in a hint

Implicit thing like implicit type definition of each variable, implicit type conversion leads to more compact code easier to understand. 
It less distracts developer from analyzing algorithm itself so it leads to less amount of errors in software as result.

Although implicit one makes an code more expressive and concise it leads to subtle and difficult to catch errors. 
As consequences are not obvious about applying an implicit one to code a developer has to keep in mind all language's often complicated rules.
In order to fix the matter, each implicit thing in a code should be expressed explicitly in the corresponding analytics file. 

### 6. perfect is the enemy of the good

As a generic algorithm of checking completability of an algorithm is impossible it is considered that no software can be checked for completability at all.
At the same time, a lot of business applications are quite simple and doesn't contain complex algorithms. And an even complex application does not completely consist of complicated cycles and/or recursions.
So it is better to have some guarantee that says for example that 90% of the code is completable than not at all. 
And even more: in the case, a developer will be able to see which part of code can't be checked automatically for completability so the one will analyze code without a guarantee more carefully.
At the same time, it is going to prevent spending time and intellectual efforts by analyzing code which can be analyzed automatically. 

The same reasoning is valid for all other aspects of development like memory management etc. 

### 7. released software performance is significant

In the modern world of mobile devices domination, it is very important that software is optimized as much as possible. It will make a mobile device work longer per one charge on one side. On another side, it is going to consume less energy thus making the language greener. 

So even though making super optimized software build could take a day it is worth it because the optimization can save some lifetime of millions or even billions of devices (or reduce power consumption of devices having a permanent power supply). Especially in the case when an operating system is re-written from the scratch using the language. 

Of course, this kind of optimization implies that there should be mostly only zero-cost abstractions like in Rust or C++. 
Zero-cost runtime abstractions is a principle both C++ and Rust languages are based upon. 
It means that all high-level abstractions are used only to decrease the complexity of source code but at runtime, they are eliminated. 



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

### 7. identifiers description aspect

Well, it is just another representation of comments. But that way comment will be tied to code and can't be lost or connected by an accident to another piece of code. Also, it will not add noise to a code. Looks like hints are useless for the aspect.

### 8. order execution aspect

It is useful to explicitly show a user in what order each instruction is going to be executed instead of the user checking the specification of a language each time. Most probably there should not be any hints but analytics to help to check priority of operators. 

### 9. data interchange tracing aspect

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

### 10. function execution time aspect 

The aspect allows controlling either function executed at compile time or runtime. 

Compile time execution is the way to make macros in the language. 
Also, it controls either function should be passed by name or by value (eager or lazy evaluation). 
