# Multi-Language Manifesto

Multi-Language programming paradigm is a principle of building software using hierarhical structure united by something like hyperlinks instead of writing a flat text.

**Main principles:**

1. segregation of a morphism and aspects related to it
2. aspect = hints + a morphism -> binary code + analytics
3. gluing aspect parts and generated code using a hash
4. make it work, make it right, make it fast
5. implicit one in a morphism but explicit one in a hint
6. the best is the enemy of the good.
7. released software performance is significant

## Main principles detailed description:


### 1. segregation of a morphism and aspects related to it

There are design tradeoffs in modern languages between easiness of learning and working with programming language vs durability and performance. 

So, as a result, we have a lot of languages which mostly everybody can learn but which provoke developing buggy and slow software. On another side, there are marginal languages that allow writing high-quality code but their usage in production is too little. Moreover, the amount of developers who understand those complex languages is too low to force companies to use any of those languages. And for sure there are a lot of languages between both corner cases.

It leads in turn to a huge amount of incorrect, unoptimized, full of vulnerabilities code written by humanity. But is there are a way to fix it? Can a language be easy to learn and use but be powerful and durable at the same time?

The principle suggests a way to fix the problem. It is ancient and simple: divide and rule. We, developers, use the principle all the time while writing software. We do it to fight complexity. But this time the principle has been applied to language design itself.

In simple words: a morphism (category theory morphism, something that just returns output for a given input) should be separated from language constructions which are related to performance optimization, correctness or other aspects related to connecting components to each other, allocating them in memory or connecting components to the outer world. There are no such concepts as computer memory or CPU in terms of morphisms. There are only arrows from one object to another one (in terms of category theory).

It leads us to the design when instead of one programming language we have set of them. One language is a set of morphisms and all other are set of aspects linked to arrows and objects (of category theory) and adding some nuances to them. Let's call the set of languages Software Development System or SDS shortly. 

It gives us good flexibility because now we can implement business logic postponing hardware-specific optimizations for a later time. Or we can quickly write code and only then run Big O complexity analysis to discover either the algorithm is applicable in practice. Or run resources acquiring analyze for an algorithm like "sorting array" and catch that it creates some files (Obviously, it is not something a developer can expect from such algorithm). Etc.

There is another important consequence of separating morphisms from aspects: the latter ones may be chosen depending on needs. For example in case of quick prototyping correctness and performance optimization related analyzes are not needed (Or too early in case of making a production-ready product out of prototype). Or when there is no high demand for performance then there is no need for performance optimization analysis. Etc. 

Here and further an aspect is the same as perspective or point of view. It allows us to concentrate on analyzing one aspect of morphism abstracting away all others. For example, when we are analyzing the completability of a morphism we don't care about its performance. When we examining a morphism from the performance-optimizing point of view we don't care about side effects etc. 

In terms of Category Theory, an aspect is natural transformation and can be defined like this: 
alpha 'a' :: A1 'a' -> A2 'a' ->  ... -> An 'a'
Where "alpha 'a'" is the component of alpha at 'a', A is an aspect, n is the order of analyzing an aspect among all other aspects. Here the particular order of analyzing aspects is irrelevant. Most important is that all aspects are connected to the same morphism. So for an aspect A to generate analytics, we apply the morphism 
A f :: A ('h','c') -> A 'a'
Where 'h' is a hint, 'c' is morphism and 'a' is resulting analytics. Generated binary could be expressed as: 
(A1 * A2 * ... An) 'c' -> A b
Where 'c' is a morphism, b is generated binary code, A with number is an aspect and right A is a result of applying all aspects.

### 2. aspect = hints + morphism -> binary code + analytics

Separating a morphism and an aspect means that controls over different aspects should be moved to another place. Literally to different files. Let us call them hints (it means hints for a compiler). 

Since in point 1 of the manifesto it is defined that a morphism should not contain anything but an algorithm then a hint is the way to control different aspects of generated binary. 
As a result of applying hints of different aspects to a code analytic file for each aspect is generated. Changing a hint developer can then check the correctness of applying it in an analytic file related to the hint.  

Of course, the way of decreasing developing complexity assumes that there should be some IDE support allowing to easily see in one place a code and linked to it hints and analytics. Thus a developer will be able to switch between each aspect to analyze it separately from other aspects. 

Modification of a hint will always change generated binary code and analytics. Analytics is like a detailed explanation of the generated code. Each aspect-related analytic allows understanding the behavior of generated code from an aspect's perspective. 

### 3. gluing aspect parts using a hash 

Running each time all analytics for the whole amount of code in the case when only a few lines are changed is not practical at all. Because running some analytics may be quite CPU consuming. But on another side, as analytics are results of applying hints to code then any changing of code will change analytics. So does it mean that static analysis of a morphism is not practical?

The principle suggests a way to run analytics efficiently: to be able to modify a morphism without running thorough analyzing down function calls hierarchy the morphism should be split to smallest possible pieces (like functions for example) and then each piece should be signed by a hash. It will allow using the memoization technique in a compiler to increase the speed of compiling morphisms and aspects.   

When a parent function depends on subroutine functions then a source of hash should be not only the parent function content but also hash codes of subroutine functions. So when down hierarchy any piece of code is changed hash code of related subroutine code will be changed so analytics should be run for the subroutine hierarchy and then for a parent morphism. 

Since an analytic file and a binary file are generated from a morphism by applying a hint then all four pieces should be tied together by a hash code. 

### 4. make it work, make it right, make it fast

Language design should be defined as the way that supports the most effective developing flow described in the principal title.

#### 4.1 make it work

It means that language should allow making experiments and prototyping quickly. Without applying any aspects to morphisms it will be durable not more than any prototyping weakly typed language. But it is important to emphasize that even on that stage size of data like integral is considered endless. Because the limited size of the integer type is just hardware optimization of speed thus should not be applied without dealing with a corresponding aspect. So even though "make it right" can't be applied on that stage it doesn't mean that a morphism may lead to subtle errors.

#### 4.2 make it right

It means that all possible aspects regarding correctness must be run.  Those aspects should allow adding different restrictions like data types, domains of function, acquiring/releasing resources, etc. Also, it means that contracts between components should be defined using those aspects.

#### 4.3 make it fast

There should be a possibility to apply different computer-specific optimizations like using limited size integers instead of unlimited ones, concurrency related optimizations, etc. Also, there are should be aspects allowing making algorithmic complexity analyze (looks like a hint can't be applied to change algorithmic complexity because that requires changing of an algorithm that in its turn requires changing of a morphism). But optimization related aspects should not be able to break correctness related aspects.

### 5. implicit one in a morphism but explicit one in a hint

Implicit things like implicit type definition of each variable, implicit type conversion lead to more compact code easier to understand. 
It less distracts a developer from analyzing an algorithm itself so it leads to fewer errors in software as a result.

Although implicit one makes code more expressive and concise it leads to subtle and difficult to catch errors. 
Because a developer has to keep in mind all language's often complicated rules of applying those invisible in morphisms things.

To fix the matter, each implicit thing in a morphism should be expressed explicitly in the corresponding analytics file. 

### 6. the best is the enemy of the good.

Because a generic algorithm of checking completability of an algorithm is impossible it is considered that no software can be checked for completability at all.
At the same time, a lot of business applications are quite simple and doesn't contain complex algorithms. And an even complex application does not completely consist of complicated cycles and/or recursions.
So it is better to have some guarantee that says for example that 90% of the code is completable than not at all. 
And even more: in the case, a developer will be able to see which part of code can't be checked automatically for completability so the one will analyze code without a guarantee more carefully.
As a result, it will prevent spending time and intellectual efforts by analyzing code which can be analyzed automatically. 

The same reasoning is valid for all other aspects of development like memory management, algorithmic complexity, etc. 

### 7. released software performance is significant

In the modern world of mobile domination, the software must be optimized as much as possible.  On one side it will make a mobile device working longer per charge. On another side, it will consume less energy thus making the language greener. 

So even though making super optimized software build could take a day it is worth it because the optimization can increase the battery life of millions or even billions of devices (or reduce the power consumption of devices having a permanent power supply). Especially in the case when an operating system is re-written from scratch using the language. 

Of course, this kind of optimization implies that there should be mostly only zero-cost abstractions like in Rust or C++. 
Zero-cost runtime abstractions is a principle both C++ and Rust languages are based upon. 

It means that all high-level abstractions should be used to decrease the complexity of source code but at runtime, they should be eliminated. 
