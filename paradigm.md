# Multi-View Languages Integration Paradigm

The Multi-View Languages Integration Paradigm (MLP) is a method of building software using a hierarchical structure connected by links instead of relying on flat text. Each MLP program comprises a primary language and several other languages that define different aspects.

**Main Principles:**

1. **Primary and Aspect Languages**
2. **The Primary Language is Minimalistic and Concise**
3. **Transparency Through Comprehensive Analytics**

## Detailed Descriptions of Main Principles:

### 1. Primary and Aspect Languages

- There is a primary deterministic language that abstracts everything unrelated to abstract data types (machine-independent data types) or data transformation instructions (i.e., how to produce output from input).
- Various aspect languages describe different aspects of a program, such as data item size, function domain, algorithmic complexity, and so on. Each aspect is represented by directives and/or analytics.
- A directive language is used to configure a construct of the primary language concerning an aspect. Developers provide directives to fine-tune these aspects. The compiler applies directives to the primary language, generating analytics and binary code.
- Developers read analytics to gain insights into each construct of the primary language. The compiler uses directives and possibly analytics to generate binary code.
- Each construct in the primary language is linked to constructs in different directive and/or analytics languages of related aspects.

### 2. The Primary Language is Minimalistic and Concise

- The primary language is kept minimalistic, concise, and as simple as possible.
- If a few constructs can be composed from simpler constructs, they are defined in the standard library rather than being defined as primary language constructs.
- When a construct needs tuning, a related aspect language is available for configuration and analysis.

### 3. Transparency Through Comprehensive Analytics

- The compiler generates default directives for each construct when developers do not provide any.
- The amount of generated analytics is sufficient for local reasoning about any construct in the primary language.
- There is no ‘behind-the-scenes’ code generation strategy that could lead to subtle errors when developers forget certain rules. Everything that could behave differently is expressed explicitly in the appropriate aspect.
