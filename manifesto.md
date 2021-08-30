# MultiLanguage Manifesto

MultiLanguage programming paradigm is a principle of building software using hierarhical structure united by something like hyperlinks instead of writing a flat text.
Each MultiLanguage consists of a main language and different aspect languages.

**Main principles:**

1. segregation of a main language and aspects related to it
2. a main language + an aspect language -> binary code + analytics
3. gluing aspect parts and generated code using a hash
4. make it work, make it right, make it fast
5. implicit matters in a main language but explicit ones in aspects related languages
6. the best is the enemy of the good.
7. released software performance is significant

## Main principles detailed description:


### 1. segregation of a main language and aspects related to it

There are design tradeoffs in modern languages between easiness of learning and working with programming language vs durability and performance. 

So, as a result, we have a lot of easy languages which mostly everybody can quickly learn but which provoke developing buggy and slow software. On another side, there are marginal languages that allow writing high-quality code but their usage in production is too little. Moreover, the amount of developers who understand those complex languages is too low to force companies to use any of those languages. And for sure there are a lot of languages between both corner cases.

It leads in turn to a huge amount of incorrect, unoptimized, full of vulnerabilities code written by humanity. But is there are a way to fix it? Can a language be easy to learn and use but be powerful and durable at the same time?

The principle suggests a way to fix the problem. It is ancient and simple: divide and rule. We, developers, use the principle all the time while writing software. We do it to fight complexity. But this time the principle has been applied to language design itself.

In simple words: a main language should be separated from secondary languages which are related to performance optimization, correctness or other aspects related to connecting components to each other, allocating them in memory or connecting components to the outer world. There are no such concepts as computer memory or CPU in terms of a main language. There are only transformations from input to output (arrows from one object to another one in terms of category theory).

It leads us to the design when instead of one programming language we have a set of them. A main language expresses domain logic and all others are a set of aspects linked to the main language and adding some nuances to it.

It gives us good flexibility because now we can implement business logic postponing hardware-specific optimizations for a later time. Or we can quickly write code and only then run for example Big O complexity analysis to discover either the algorithm is applicable in practice. Or run resources acquiring analysis for an algorithm like "sorting array" and catch for example that it creates some files (Obviously, it is not something a developer can expect from such algorithm). Etc.

There is another important consequence of separating domain logic from aspects: the latter ones may be chosen depending on needs. For example, in the case of quick prototyping correctness and performance optimization-related analyzes are not needed (Or too early in case of making a production-ready product out of prototype). Or when there is no high demand for performance then there is no need for performance optimization analysis. Etc. 

Here and further an aspect is the same as perspective or point of view. It allows us to concentrate our attention on analyzing a single aspect abstracting away all others. For example, when we are analyzing the completability of a main language function we don't care about its performance. When we examining a function from the performance-optimizing point of view we don't care about side effects etc. 

### 2. a main language + an aspect directives language -> binary code + aspect analytics language

Since in point 1 of the manifesto it is defined that a main language should not express anything but an algorithm abstracted from particular hardware then an aspect language is the way to control different aspects of generated binary. 

Combining an aspect language with a main language produces binary code and some analytics related to the aspect. 

Of course, the way of decreasing developing complexity assumes that there should be some IDE support allowing to easily see in one place an item of a main language and a  linked to it aspect and its analytics. Thus a developer will be able to switch between each aspect to analyze it separately from other aspects. 

Modification of an aspect language part of code will always change generated binary code and analytics. Each aspect-related analytic allows understanding behavior of generated code from that aspect's perspective. 

### 3. gluing aspect parts using a hash 

Analyzing a whole codebase each time when only a few lines are changed is not practical at all. Because running some analytics may be quite CPU-consuming. But on another side, since analytics are the results of applying aspects to code then any changing of code will change analytics. So does it mean that static analysis of a main language item is not practical?

The principle suggests a way to run analytics efficiently: to be able to modify a main language item without running a thorough analyzing down function calls hierarchy the main language items should be split into the smallest possible pieces (like functions or even function lines for example) and then each piece should be signed by a hash. It will allow using the memoization technique in a compiler to increase the speed of compiling a main language and aspects.   

When a parent function depends on subroutine functions then a source of hash should be not only the parent function content but also hash codes of subroutine functions. So when down hierarchy any piece of code is changed hash code of related subroutine code will be changed thus analytics should be run for the subroutine hierarchy and then for a parent function. Changing name of a function should not change its hash or hash of using it functions.

Since an analytic file and a binary file are generated from a function by applying a directive then all four pieces should be tied together by a hash code. 

### 4. make it work, make it right, make it fast

A language design should support the most effective developing steps described in the title. Those steps may go in a different order or may be applied altogether depending on a project.

#### 4.1 make it work

"Make it work" means that language should allow making experiments and prototyping quickly. Without applying any aspect to a main language it will generate binary not more durable than generates any prototyping weakly typed language. But it is important to emphasize that even at that stage size of data like integral is considered endless. Usage of a size-limited integer type is just performance optimization caused by limitations of the hardware. Thus it should not be applied without dealing with a corresponding aspect. So even though "make it right" can't be applied at that stage it doesn't mean that a main language will produce binary code containing subtle errors like an integer overflow, memory leaks, usage of released memory or releasing it twice, etc.

#### 4.2 make it right

"Make it right" means that a language should provide a set of correctness-related aspects. Those aspects should allow adding different restrictions like data types, domains of function, acquiring/releasing resources, etc. Also, it means that contracts between components should be defined using those aspects.

#### 4.3 make it fast

There should be a possibility to apply different computer-specific optimizations like using limited size integers instead of unlimited ones, concurrency-related optimizations, etc. Also, there are should be aspects allowing making algorithmic complexity analysis (looks like an aspect language can't be applied to change algorithmic complexity because that requires changing of an algorithm that in its turn requires changing of code written in a main language). But optimization related aspects should not be able to break correctness-related aspects.

### 5. implicit matters in a main language but explicit ones in aspects related languages

A main language should be as concise as possible thus a lot of matters will be implicitly assumed but invisible in code. On the other side aspect related languages should express everything explicitly even it would require more verbose language. 

For example, such implicit things as implicit type definition of each variable, implicit type conversion lead to more compact code easier to understand. 
It less distracts a developer from analyzing an algorithm itself so it leads to fewer errors in software as a result.

Although implicit things make code more expressive and concise it leads to subtle and difficult to catch errors. It happens because a developer has to keep in mind all language's often complicated rules of applying those invisible in a language things.

To fix that contradiction, each implicit behavior in a main language should be expressed explicitly in the corresponding analytics file. 

### 6. the best is the enemy of the good.

Because (in general) it is impossible to prove the completability of code existing static code analyzers don't try to check it at all.
At the same time, a lot of business applications are simple and don't contain complex algorithms. And even complex software may be composed of some simple to check cycles and/or recursions.
So it is better to have a guarantee that (for example) claims that 90% of the code is completable than not at all. 
And even more: in this case, a developer will be able to see which part of code can't be checked automatically for completability thus will analyze code without a guarantee more carefully.
As a result, it will prevent spending time and intellectual efforts by analyzing code that can be analyzed automatically. 

The same reasoning is valid for all other aspects of development like memory management, algorithmic complexity, etc. 

### 7. released software performance is significant

In the modern world of mobile domination, a compiler should optimize the performance of a generated software as much as possible.  Firstly it will make a mobile device working longer per charge. Secondly, it will consume less energy, therefore, making the language greener. 

Spending a lot of time making a heavily optimized build will increase the battery life of millions or even billions of devices (or reduce the power consumption of devices having a permanent power supply). Making an operating system having the best possible optimizations will reduce environmental contamination even more.

To achieve the highest possible performance a language should use the zero-cost abstractions principle. It is the principle that Rust and C++ languages have. 

That principle says that high-level abstractions are needed to decrease the complexity of source code should not exist in a generated binary code.
