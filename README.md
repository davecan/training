# Security Concepts Overview

- CIA
- IAAA
- Defense in Depth (overlapping / compensating controls)
- CWE -> CVE
  - this course focuses on preventing CWEs

- Trusted Components
  - i.e. components you rely on the most to work
  - Ex: microservice A using backend of microservice B for remote system access
  - Trust must be earned
  - Identify them, prove they are trustworthy, & protect them

- Trust Boundaries
  - Define and manage them w/ clear APIs
  - Ex:
    - Draw microservice A & B topology
    - Draw trust boundaries around each, and the platform + apps overall
    - Mention we could draw "boundaries" enclosing each user -> app path also, to show isolation from other app users

- Mediated Access
  - Control access & data flow across your trust boundaries
  - Only use trusted communication channels
  - POLP / etc
  - Back to the trust boundary diagram
    - mediate access via API
    - mediate access between apps by requiring 2875

- Fail Secure
  - Exception / error management comes into play here

- Minimize sharing of resources
  - Only share resources when you trust them and mediate access to them
  - Share/reuse of trusted components is OK but protect them accordingly

- KISS
  - Complexity is the enemy of security
  - Use Clear Abstractions (APIs/modules/etc)
    - Use simple well-defined interfaces & functions (see #35)
    - Makes mediated access easier
    - But remember Law of Leaky Abstractions



# Injection & Input Validation

4 - Ensure that security-sensitive methods are called with validated arguments 
  - the example in the book is good to show it doesn't just apply to web code

7 - Prevent code injection

8 - Prevent XPath injection



# Sensitive Data Protection

1 - Limit the lifetime of sensitive data

2 - Do not store unencrypted sensitive information on the client side



# Authentication / Access Control / Mediated Access

17 - Minimize privileged code
   - this is a great concept to reinforce, even though the example given isn't very useful

18 - Do not expose methods that use reduced-security checks to untrusted code
   - first example (class loader) not useful
   - second example (sql conn str) is great & reiterates injection in general
   - third & fourth aren't directly relevant to us but do demonstrate hidden dangers



# Proper Crypto Use

12 - Do not use insecure or weak cryptographic algorithms

13 - Store passwords using a hash function
   - we don't use passwords but this is great for them to know in general

14 - Ensure that SecureRandom is properly seeded

## Less Important

11 - Do not use Object.equals() to compare cryptographic keys


# Output Sanitization (XSS)

6 - Properly encode or escape output


# Secure File & Resource Management

5 - Prevent arbitrary file upload

27 - Identify files using multiple file attributes (TOCTOU issue)

59 - Do not make assumptions about file creation

46 - Do not serialize direct handles to system resources
   - the general concept is more important here

43 - Use a try-with-resources statement to safely handle closeable resources



# Defensive Programming


## General Principles

62 - Use comments consistently and in a readable fashion
   - I like the explicit if(false) guard

63 - Detect and remove superfluous code and values

64 - Strive for logical completeness


## Variables & Constants

22 - Minimize the scope of variables

37 - Do not shadow or obscure identifiers in subscopes

38 - Do not declare more than one variable per declaration

39 - Use meaningful symbolic constants to represent literal values in program logic
   - nice example about BUFSIZE being an out-of-band assumption

40 - Properly encode relationships in constant definitions

71 - Understand how escape characters are interpreted when strings are loaded

50 - Be careful using visually misleading identifiers and literals


## Expressions & Control Flow

58 - Use parentheses for precedence of operation

69 - Do not confuse abstract object equality with reference equality

70 - Understand the differences between bitwise and logical operators

55 - Do not place a semicolon immediately following an if, for, or while condition


### Numeric Expressions

60 - Convert integers to floating-point for floating-point operations

29 - Be aware of numeric promotion behavior

68 - Do not assume that the remainder operator always returns a nonnegative result for integral operands


### Branching

54 - Use braces for the body of an if, for, or while statement

53 - Do not perform assignments in conditional expressions

45 - Use the same type for the second and third operands in conditional expressions

56 - Finish every set of statements associated with a case label with a break statement

### Loops

47 - Prefer using iterators over enumerations
   - makes good point about unintended mutability side effect

57 - Avoid inadvertent wrapping of loop counters


## Methods


### Return Values

26 - Always provide feedback about the resulting value of a method

41 - Return an empty array or collection instead of a null value for methods that return an array or collection

52 - Avoid in-band error indicators (better example preferred though)


### Overloads & Varargs

72 - Do not use overloaded methods to differentiate between runtime types

65 - Avoid ambiguous or confusing uses of overloading

30 - Enable compile-time type checking of variable arity parameter types

51 - Avoid ambiguous overloading of variable arity methods


## Classes, Interfaces, Modules

24 - Minimize the accessibility of classes and their members

35 - Carefully design interfaces before releasing them
   - applies more generally to web APIs as well

73 - Never confuse the immutability of a reference with that of the referenced object

31 - Do not apply public final to constants whose value might change in later releases

32 - Avoid cyclic dependencies between packages


### Less Important

3 - Provide sensitive mutable classes with unmodifiable wrappers
  - can be good to touch on but seems more applicable if devs are exporting libs, not a big use case for us yet



## Exception Handling & Error Management

33 - Prefer user-defined exceptions over more general exception types

34 - Try to gracefully recover from system errors

42 - Use exceptions only for exceptional conditions

44 - Do not use assertions to verify the absence of runtime errors



## Garbage Collection

36 - Write garbage collection–friendly code

49 - Remove short-lived objects from long-lived container objects

75 - Do not attempt to help the garbage collector by setting local reference variables to null



# Less Important (maybe briefly mention these)

(some are edge cases or use threading, in others the attacker needs access for these & they probably want to do more interesting things at that point)

10 - Do not use the clone() method to copy untrusted method parameters
   - attacker creating a new class seems unlikely in our case

15 - Do not rely on methods that can be overridden by untrusted code
   - requires attacker have access to extend the class

16 - Avoid granting excess privileges
   - the concept is important but the example given doesn't seem to fit our model

21 - Do not let untrusted code misuse privileges of callback methods
   - in context of something like Spring Boot async or similar, not client-side

23 - Minimize the scope of the @SuppressWarnings annotation

28 - Do not attach significance to the ordinal associated with an enum

61 - Ensure that the clone() method calls super.clone()

74 - Use the serialization methods writeUnshared()and readUnshared() with care



# Unnecessary

(unless you disagree or someone specifically requests discussion during class, which I doubt)

9 - Prevent LDAP injection
  - we don't manage our own Active Directory or query it

19 - Define custom security permissions for fine-grained security
   - seems to revolve around desktop/client or library access where attacker creates malicious calling code

20 - Create a secure sandbox using a security manager
   - good to know this is possible but doesn't seem applicable to what they do

25 - Document thread-safety and use annotations where applicable

48 - Do not use direct buffers for short-lived, infrequently used objects

66 - Do not assume that declaring a reference volatile guarantees safe publication of the members of the referenced object

67 - Do not assume that the sleep(), yield(), or getState() methods provide synchronization semantics