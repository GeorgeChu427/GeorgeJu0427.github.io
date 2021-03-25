## How to use Kotlin Lambda 

### Overtrue
In this article, we’re going to explore Lambdas in the Kotlin language. Keep in mind that lambdas aren’t unique to Kotlin and have been around for many years in many other languages.

Lambdas Expressions are essentially anonymous functions that we can treat as values – we can, for example, pass them as arguments to methods, return them, or do any other thing we could do with a normal object.

### Defining a Lambda
As we’ll see, Kotlin Lambdas are very similar to Java Lambdas. You can find out more about how to work with Java Lambdas and some best practices here.

To define a lambda, we need to stick to the syntax:
```kotlin
val lambdaName : Type = { argumentList -> codeBody }
The only part of a lambda that isn’t optional is the codeBody.
```
The argument list can be skipped when defining at most one argument and the Type can often be inferred by the Kotlin compiler. We don’t always need a variable as well, the lambda can be passed directly as a method argument.

The type of the last command within a lambda block is the returned type.