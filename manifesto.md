# The MultiLanguage Manifesto

The MultiLanguage programming paradigm is a method of building software using a hierarchical structure connected by links instead of relying on flat text. Each MultiLanguage program comprises a primary language and several other languages that define different aspects.

**Main Principles:**

1. **Primary and Aspect Languages**
2. **The Primary Language is Minimalistic and Concise**
3. **Transparency Through Comprehensive Analytics**
4. **Enhancing Application Correctness with Comprehensive Analytics**
5. **Enhancing Application Efficiency with Comprehensive Analytics**
6. **Enhancing Analytics Generation Performance Through Incremental Generation**

## Detailed Descriptions of Main Principles:

### 1. Primary and Aspect Languages

- There is a primary deterministic language that abstracts everything unrelated to abstract data types (machine-independent data types) or data transformation instructions (how to produce output from input).
- Various aspect languages describe different aspects of a program, such as data item size, function domain, algorithmic complexity, etc. Each aspect consists of directives and/or analytics.
- A directive language is used to configure a construct of the primary language concerning an aspect. Developers provide directives to fine-tune aspects. The compiler applies directives to the primary language to generate analytics and binary code.
- Developers read analytics to gain insights about each construct of the primary language. The compiler utilizes analytics to generate binary code.
- Each construct in the primary language is linked to constructs in different aspect languages (through directives and/or analytics).

### 2. The Primary Language is Minimalistic and Concise

- The primary language is kept minimalistic, concise, and as simple as possible.
- If a few constructs can be composed from simpler constructs, they are defined in the standard library rather than being defined as primary language constructs.
- When a construct needs tuning, a related aspect language is available for configuration and analysis.

### 3. Transparency Through Comprehensive Analytics

- The compiler generates default directives for each construct when developers do not provide any.
- The amount of generated analytics is sufficient for local reasoning about any construct in the primary language.
- There is no "behind-the-scenes" code generation strategy that could lead to subtle errors when developers forget certain rules. Everything that could behave differently is expressed explicitly in the appropriate aspect.

### 4. Enhancing Application Correctness with Comprehensive Analytics

- All default presets in various aspects aim for the highest level of correctness, even if it impacts performance.
- For example, machine-specific integers cannot be used until a numeric variable has a defined range. In such cases, the system assumes that any integer is infinite in size, limited only by the computer's memory capacity.
- Memory management analytics ensure the same level of correctness in languages with runtime memory management.

### 5. Enhancing Application Efficiency with Comprehensive Analytics

- The compiler generates as few runtime abstractions as possible.
- An aspect allows for adding real-time introspection when necessary.
- Various aspects are designed to reduce runtime overhead. For example, the system uses function domains to enhance integer performance by limiting integer size, enabling the use of computer-native types. Default presets are employed to achieve efficient memory management compared to manual memory management.
- Furthermore, various aspects are available for fine-tuning performance, such as algorithmic complexity analyzers and more.

### 6. Enhancing Analytics Generation Performance Through Incremental Generation

- With each change to the source code, the compiler re-generates analytics only for the modified constructs and those dependent on them.
- Each construct in the primary language possesses a unique digital signature. Changing its name does not alter the signature, but modifying any construct it contains does. Moreover, each item in an aspect links to a construct in the primary language using this signature.
- Subsequently, the compiler employs the primary language source code, directives, and analytics to incrementally generate machine-specific instructions.
